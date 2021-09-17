---
title: Requêtes de diagnostic utiles - Hyperscale (Citus) - Azure Database pour PostgreSQL
description: Des requêtes pour en savoir plus sur les données distribuées et plus encore
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 8/23/2021
ms.openlocfilehash: efd251e48beb4e2f2b3db16f694da14888e876dd
ms.sourcegitcommit: d11ff5114d1ff43cc3e763b8f8e189eb0bb411f1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2021
ms.locfileid: "122822534"
---
# <a name="useful-diagnostic-queries"></a>Requêtes de diagnostic utiles

## <a name="finding-which-node-contains-data-for-a-specific-tenant"></a>Recherche du nœud contenant les données d’un locataire spécifique

Dans un cas d’utilisation multi-locataire, nous pouvons déterminer le nœud Worker qui contient les lignes d’un locataire spécifique.  Hyperscale (Citus) regroupe les lignes des tables distribuées dans des partitions, puis place chaque partition sur un nœud Worker dans le groupe de serveurs. 

Supposons que les locataires de notre application sont des magasins et que nous souhaitons identifier le nœud Worker contenant les données pour ID de magasin=4.  En d’autres termes, nous souhaitons déterminer l’emplacement de la partition contenant les lignes dont la colonne de distribution a la valeur 4 :

``` postgresql
SELECT shardid, shardstate, shardlength, nodename, nodeport, placementid
  FROM pg_dist_placement AS placement,
       pg_dist_node AS node
 WHERE placement.groupid = node.groupid
   AND node.noderole = 'primary'
   AND shardid = (
     SELECT get_shard_id_for_distribution_column('stores', 4)
   );
```

La sortie contient l’hôte et le port de la base de données Worker.

```
┌─────────┬────────────┬─────────────┬───────────┬──────────┬─────────────┐
│ shardid │ shardstate │ shardlength │ nodename  │ nodeport │ placementid │
├─────────┼────────────┼─────────────┼───────────┼──────────┼─────────────┤
│  102009 │          1 │           0 │ 10.0.0.16 │     5432 │           2 │
└─────────┴────────────┴─────────────┴───────────┴──────────┴─────────────┘
```

## <a name="finding-the-distribution-column-for-a-table"></a>Recherche de la colonne de distribution d’une table

Chaque table distribuée dans Hyperscale (Citus) comporte une « colonne de distribution ». (Pour plus d’informations, consultez [Modélisation des données distribuées](concepts-hyperscale-choose-distribution-column.md).) Il peut être important de savoir de quelle colonne il s’agit. Par exemple, lors de la jointure ou du filtrage de tables, vous pouvez voir des messages d’erreur avec des indicateurs comme « ajouter un filtre à la colonne de distribution ».

Les tables `pg_dist_*` sur le nœud coordinateur contiennent diverses métadonnées sur la base de données distribuée. En particulier, `pg_dist_partition` contient des informations sur la colonne de distribution pour chaque table. Vous pouvez utiliser une fonction utilitaire pratique pour rechercher le nom de la colonne de distribution à partir des détails de bas niveau dans les métadonnées. Voici un exemple et sa sortie :

``` postgresql
-- create example table

CREATE TABLE products (
  store_id bigint,
  product_id bigint,
  name text,
  price money,

  CONSTRAINT products_pkey PRIMARY KEY (store_id, product_id)
);

-- pick store_id as distribution column

SELECT create_distributed_table('products', 'store_id');

-- get distribution column name for products table

SELECT column_to_column_name(logicalrelid, partkey) AS dist_col_name
  FROM pg_dist_partition
 WHERE logicalrelid='products'::regclass;
```

Exemple de sortie :

```
┌───────────────┐
│ dist_col_name │
├───────────────┤
│ store_id      │
└───────────────┘
```

## <a name="detecting-locks"></a>Détection des verrous

Cette requête s’exécutera sur tous les nœuds Worker et identifiera les verrous, la durée pendant laquelle ils ont été ouverts, et les requêtes incriminées :

``` postgresql
SELECT run_command_on_workers($cmd$
  SELECT array_agg(
    blocked_statement || ' $ ' || cur_stmt_blocking_proc
    || ' $ ' || cnt::text || ' $ ' || age
  )
  FROM (
    SELECT blocked_activity.query    AS blocked_statement,
           blocking_activity.query   AS cur_stmt_blocking_proc,
           count(*)                  AS cnt,
           age(now(), min(blocked_activity.query_start)) AS "age"
    FROM pg_catalog.pg_locks         blocked_locks
    JOIN pg_catalog.pg_stat_activity blocked_activity
      ON blocked_activity.pid = blocked_locks.pid
    JOIN pg_catalog.pg_locks         blocking_locks
      ON blocking_locks.locktype = blocked_locks.locktype
     AND blocking_locks.DATABASE IS NOT DISTINCT FROM blocked_locks.DATABASE
     AND blocking_locks.relation IS NOT DISTINCT FROM blocked_locks.relation
     AND blocking_locks.page IS NOT DISTINCT FROM blocked_locks.page
     AND blocking_locks.tuple IS NOT DISTINCT FROM blocked_locks.tuple
     AND blocking_locks.virtualxid IS NOT DISTINCT FROM blocked_locks.virtualxid
     AND blocking_locks.transactionid IS NOT DISTINCT FROM blocked_locks.transactionid
     AND blocking_locks.classid IS NOT DISTINCT FROM blocked_locks.classid
     AND blocking_locks.objid IS NOT DISTINCT FROM blocked_locks.objid
     AND blocking_locks.objsubid IS NOT DISTINCT FROM blocked_locks.objsubid
     AND blocking_locks.pid != blocked_locks.pid
    JOIN pg_catalog.pg_stat_activity blocking_activity ON blocking_activity.pid = blocking_locks.pid
    WHERE NOT blocked_locks.GRANTED
     AND blocking_locks.GRANTED
    GROUP BY blocked_activity.query,
             blocking_activity.query
    ORDER BY 4
  ) a
$cmd$);
```

Exemple de sortie :

```
┌───────────────────────────────────────────────────────────────────────────────────┐
│                               run_command_on_workers                              │
├───────────────────────────────────────────────────────────────────────────────────┤
│ (10.0.0.16,5432,t,"")                                                             │
│ (10.0.0.20,5432,t,"{""update ads_102277 set name = 'new name' where id = 1; $ sel…│
│…ect * from ads_102277 where id = 1 for update; $ 1 $ 00:00:03.729519""}")         │
└───────────────────────────────────────────────────────────────────────────────────┘
```

## <a name="querying-the-size-of-your-shards"></a>Interrogation de la taille de vos partitions

Cette requête vous indiquera la taille de chaque partition d’une table distribuée donnée, appelée `my_distributed_table` :

``` postgresql
SELECT *
FROM run_command_on_shards('my_distributed_table', $cmd$
  SELECT json_build_object(
    'shard_name', '%1$s',
    'size',       pg_size_pretty(pg_table_size('%1$s'))
  );
$cmd$);
```

Exemple de sortie :

```
┌─────────┬─────────┬───────────────────────────────────────────────────────────────────────┐
│ shardid │ success │                                result                                 │
├─────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│  102008 │ t       │ {"shard_name" : "my_distributed_table_102008", "size" : "2416 kB"}    │
│  102009 │ t       │ {"shard_name" : "my_distributed_table_102009", "size" : "3960 kB"}    │
│  102010 │ t       │ {"shard_name" : "my_distributed_table_102010", "size" : "1624 kB"}    │
│  102011 │ t       │ {"shard_name" : "my_distributed_table_102011", "size" : "4792 kB"}    │
└─────────┴─────────┴───────────────────────────────────────────────────────────────────────┘
```

## <a name="querying-the-size-of-all-distributed-tables"></a>Interrogation de la taille de toutes les tables distribuées

Cette requête obtient une liste des tailles de chaque table distribuée, ainsi que la taille de leurs index.

``` postgresql
SELECT
  tablename,
  pg_size_pretty(
    citus_total_relation_size(tablename::text)
  ) AS total_size
FROM pg_tables pt
JOIN pg_dist_partition pp
  ON pt.tablename = pp.logicalrelid::text
WHERE schemaname = 'public';
```

Exemple de sortie :

```
┌───────────────┬────────────┐
│   tablename   │ total_size │
├───────────────┼────────────┤
│ github_users  │ 39 MB      │
│ github_events │ 98 MB      │
└───────────────┴────────────┘
```

Notez qu’il existe d’autres fonctions Hyperscale (Citus) pour l’interrogation de la taille des tables distribuées.Consultez [Détermination de la taille d’une table](howto-hyperscale-table-size.md).

## <a name="identifying-unused-indices"></a>Identification des index inutilisés

La requête suivante identifiera les index inutilisés sur les nœuds Worker pour une table distribuée donnée (`my_distributed_table`)

``` postgresql
SELECT *
FROM run_command_on_shards('my_distributed_table', $cmd$
  SELECT array_agg(a) as infos
  FROM (
    SELECT (
      schemaname || '.' || relname || '##' || indexrelname || '##'
                 || pg_size_pretty(pg_relation_size(i.indexrelid))::text
                 || '##' || idx_scan::text
    ) AS a
    FROM  pg_stat_user_indexes ui
    JOIN  pg_index i
    ON    ui.indexrelid = i.indexrelid
    WHERE NOT indisunique
    AND   idx_scan < 50
    AND   pg_relation_size(relid) > 5 * 8192
    AND   (schemaname || '.' || relname)::regclass = '%s'::regclass
    ORDER BY
      pg_relation_size(i.indexrelid) / NULLIF(idx_scan, 0) DESC nulls first,
      pg_relation_size(i.indexrelid) DESC
  ) sub
$cmd$);
```

Exemple de sortie :

```
┌─────────┬─────────┬───────────────────────────────────────────────────────────────────────┐
│ shardid │ success │                            result                                     │
├─────────┼─────────┼───────────────────────────────────────────────────────────────────────┤
│  102008 │ t       │                                                                       │
│  102009 │ t       │ {"public.my_distributed_table_102009##some_index_102009##28 MB##0"}   │
│  102010 │ t       │                                                                       │
│  102011 │ t       │                                                                       │
└─────────┴─────────┴───────────────────────────────────────────────────────────────────────┘
```

## <a name="monitoring-client-connection-count"></a>Surveillance du nombre de connexions client

La requête suivante compte les connexions ouvertes sur le coordinateur et les regroupe par type.

``` sql
SELECT state, count(*)
FROM pg_stat_activity
GROUP BY state;
```

Exemple de sortie :

```
┌────────┬───────┐
│ state  │ count │
├────────┼───────┤
│ active │     3 │
│ idle   │     3 │
│ ∅      │     6 │
└────────┴───────┘
```

## <a name="viewing-system-queries"></a>Affichage des requêtes système

### <a name="active-queries"></a>Requêtes actives

La vue `pg_stat_activity` indique les requêtes en cours d’exécution. Vous pouvez filtrer pour rechercher celles activement exécutées, ainsi que l’ID de processus de leur serveur principal :

```sql
SELECT pid, query, state
  FROM pg_stat_activity
 WHERE state != 'idle';
```

### <a name="why-are-queries-waiting"></a>Pourquoi les requêtes sont-elles en attente ?

Nous pouvons également interroger pour voir les raisons les plus courantes pour lesquelles des requêtes non inactives sont en attente. Pour une explication des raisons, consultez la [documentation PostgreSQL](https://www.postgresql.org/docs/current/monitoring-stats.html#WAIT-EVENT-TABLE).

```sql
SELECT wait_event || ':' || wait_event_type AS type, count(*) AS number_of_occurences
  FROM pg_stat_activity
 WHERE state != 'idle'
GROUP BY wait_event, wait_event_type
ORDER BY number_of_occurences DESC;
```

Exemple de sortie lors de l’exécution simultanée de `pg_sleep` dans une requête distincte :

```
┌─────────────────┬──────────────────────┐
│      type       │ number_of_occurences │
├─────────────────┼──────────────────────┤
│ ∅               │                    1 │
│ PgSleep:Timeout │                    1 │
└─────────────────┴──────────────────────┘
```

## <a name="index-hit-rate"></a>Taux d’accès à l’index

Cette requête vous fournira le taux d’accès à l’index sur tous les nœuds. Le taux d’accès à l’index est utile pour déterminer la fréquence à laquelle les index sont utilisés lors de l’interrogation.
Une valeur de 95% ou supérieure est idéale.

``` postgresql
-- on coordinator
SELECT 100 * (sum(idx_blks_hit) - sum(idx_blks_read)) / sum(idx_blks_hit) AS index_hit_rate
  FROM pg_statio_user_indexes;

-- on workers
SELECT nodename, result as index_hit_rate
FROM run_command_on_workers($cmd$
  SELECT 100 * (sum(idx_blks_hit) - sum(idx_blks_read)) / sum(idx_blks_hit) AS index_hit_rate
    FROM pg_statio_user_indexes;
$cmd$);
```

Exemple de sortie :

```
┌───────────┬────────────────┐
│ nodename  │ index_hit_rate │
├───────────┼────────────────┤
│ 10.0.0.16 │ 96.0           │
│ 10.0.0.20 │ 98.0           │
└───────────┴────────────────┘
```

## <a name="cache-hit-rate"></a>Taux d'accès au cache

La plupart des applications accèdent généralement à une petite fraction de leurs données totales à la fois. PostgreSQL conserve les données fréquemment utilisées en mémoire pour éviter les lectures lentes à partir du disque. Vous pouvez consulter les statistiques à ce sujet dans l’affichage [pg_statio_user_tables](https://www.postgresql.org/docs/current/monitoring-stats.html#MONITORING-PG-STATIO-ALL-TABLES-VIEW).

Une mesure importante est le pourcentage de données provenant du cache en mémoire par rapport au disque dans votre charge de travail :

``` postgresql
-- on coordinator
SELECT
  sum(heap_blks_read) AS heap_read,
  sum(heap_blks_hit)  AS heap_hit,
  100 * sum(heap_blks_hit) / (sum(heap_blks_hit) + sum(heap_blks_read)) AS cache_hit_rate
FROM
  pg_statio_user_tables;

-- on workers
SELECT nodename, result as cache_hit_rate
FROM run_command_on_workers($cmd$
  SELECT
    100 * sum(heap_blks_hit) / (sum(heap_blks_hit) + sum(heap_blks_read)) AS cache_hit_rate
  FROM
    pg_statio_user_tables;
$cmd$);
```

Exemple de sortie :

```
┌───────────┬──────────┬─────────────────────┐
│ heap_read │ heap_hit │   cache_hit_rate    │
├───────────┼──────────┼─────────────────────┤
│         1 │      132 │ 99.2481203007518796 │
└───────────┴──────────┴─────────────────────┘
```

Si vous vous retrouvez avec un ratio nettement inférieur à 99 %, vous souhaiterez probablement envisager d’augmenter le cache disponible pour votre base de données.

## <a name="next-steps"></a>Étapes suivantes

* En savoir plus sur les autres [tables système](reference-hyperscale-metadata.md) utiles pour les diagnostics
