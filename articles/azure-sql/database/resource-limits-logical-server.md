---
title: Gestion des ressources dans Azure SQL Database
description: Cet article fournit une vue d’ensemble de la gestion des ressources pour Azure SQL Database. Il fournit également des informations concernant ce qui se passe lorsque ces limites de ressources sont atteintes ou dépassées.
services: sql-database
ms.service: sql-database
ms.subservice: service-overview
ms.custom: ''
ms.devlang: ''
ms.topic: reference
author: dimitri-furman
ms.author: dfurman
ms.reviewer: mathoma
ms.date: 10/01/2021
ms.openlocfilehash: 67983b60457d652a1e4de71c6655f7eedae40542
ms.sourcegitcommit: 7bd48cdf50509174714ecb69848a222314e06ef6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/02/2021
ms.locfileid: "129389757"
---
# <a name="resource-management-in-azure-sql-database"></a>Gestion des ressources dans Azure SQL Database
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Cet article fournit une vue d’ensemble de la gestion des ressources pour Azure SQL Database. Il fournit des informations sur ce qui se passe quand les limites de ressources sont atteintes et décrit les mécanismes de gouvernance des ressources utilisés pour appliquer ces limites.

Pour connaître les limites de ressource spécifiques par niveau tarifaire (également appelé objectif de service) pour les bases de données uniques, consultez [Limites de ressources de base de données unique par DTU](resource-limits-dtu-single-databases.md) ou [Limites de ressources de base de données unique par vCore](resource-limits-vcore-single-databases.md). Pour les limites de ressource de pool élastique, consultez [Limites de ressource de pool élastique par DTU](resource-limits-dtu-elastic-pools.md) ou [Limites de ressource de pool élastique par vCore](resource-limits-vcore-elastic-pools.md).

> [!TIP]
> Pour les limites d’Azure SQL Managed Instance, consultez [Limites de ressources pour les instances managées](../managed-instance/resource-limits.md).
>
> Pour des limites de pool SQL dédiées Azure Synapse Analytics, consultez [Limites de capacité](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md) et [Limites de mémoire et de concurrence](../../synapse-analytics/sql-data-warehouse/memory-concurrency-limits.md).

## <a name="logical-server-limits"></a>Limites du serveur logique

| Ressource | Limite |
| :--- | :--- |
| Bases de données par [serveur logique](logical-servers.md) | 5 000 |
| Nombre par défaut de serveurs logiques par abonnement dans une région | 20 |
| Nombre maximal de serveurs logiques par abonnement dans une région | 250 |
| Quota DTU / eDTU par serveur logique | 54 000 |
| Quota vCore par serveur logique | 540 |
| Nombre maximal de pools élastiques par serveur logique | Limité par le nombre de DTU ou de vCores. Par exemple, si chaque pool contient 1 000 unités DTU, alors un serveur peut prendre en charge 54 pools.|
|||

> [!IMPORTANT]
> Le nombre de bases de données approchant la limite par serveur logique, ce qui suit peut se produire :
>
> - Augmentation de latence dans l’exécution de requêtes sur la base de données master. Cela inclut les vues de statistiques d’utilisation des ressources telles que `sys.resource_stats`.
> - Augmentation de latence des opérations de gestion et le rendu des points de vue de portails qui impliquent des bases de données sur le serveur.

> [!NOTE]
> Pour augmenter le quota DTU/eDTU ou vCore, ou pour obtenir une quantité de serveurs logiques supérieure au nombre par défaut, envoyez une nouvelle demande de support dans le portail Azure. Pour plus d’informations, voir [Demander des augmentations de quota pour Azure SQL Database](quota-increase-request.md).

## <a name="what-happens-when-resource-limits-are-reached"></a>Que se passe-t-il lorsque les limites de ressources sont atteintes ?

### <a name="compute-cpu"></a>Processeur de calcul

Quand l’utilisation du processeur de calcul de la base de données est élevée, la latence des requêtes augmente et celles-ci peuvent même arriver à expiration. Dans ces conditions, les requêtes peuvent être mises en file d’attente par le service et reçoivent des ressources pour leur exécution lorsque des ressources se libèrent.
En cas d’utilisation élevée des calculs, voici certaines des options d’atténuation à votre disposition :

- Augmenter la taille de calcul de la base de données ou du pool élastique pour fournir plus de ressources de calcul à la base de données. Consultez [Mise à l’échelle des ressources d’une base de données unique](single-database-scale.md) et [Mise à l'échelle des ressources d’un pool élastique](elastic-pool-scale.md).
- Optimiser les requêtes pour réduire l’utilisation des ressources du processeur de chaque requête. Pour plus d’informations, consultez la page [Paramétrage/Compréhension de requêtes](performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Stockage

Lorsque l'espace de base de données utilisé atteint la limite en termes de taille maximale des données, les insertions et mises à jour de base de données qui augmentent la taille des données échouent et les clients reçoivent un [message d'erreur](troubleshoot-common-errors-issues.md). Les instructions SELECT et DELETE ne sont pas affectées.

Aux niveaux de service Premium et Critique pour l'entreprise, les clients reçoivent également un message d'erreur si la consommation de stockage combinée des données, du journal des transactions et de tempdb dépasse la taille maximale prévue pour le stockage local. Pour plus d'informations, consultez [Gouvernance de l'espace de stockage](#storage-space-governance).

En cas d’utilisation élevée de l’espace, voici certaines des options d’atténuation à votre disposition :

- Augmentez la taille maximale des données de la base de données ou du pool élastique, ou effectuez un scale-up vers un objectif de service offrant une limite plus élevée en termes de taille maximale des données. Consultez [Mise à l’échelle des ressources d’une base de données unique](single-database-scale.md) et [Mise à l'échelle des ressources d’un pool élastique](elastic-pool-scale.md).
- Si la base de données est dans un pool élastique, vous pouvez également déplacer la base de données en dehors du pool afin que son espace de stockage ne soit pas partagé avec d’autres bases de données.
- Réduire une base de données afin de récupérer l’espace inutilisé. Dans les pools élastiques, la réduction d'une base de données offre plus de stockage aux autres bases de données du pool. Pour plus d’informations, consultez [Gérer l’espace des fichiers dans Azure SQL Database](file-space-manage.md).
- Vérifiez si l'utilisation intensive de l'espace est due à un pic de la taille du magasin PVS (Persistent Version Store). Le magasin PVS fait partie de chaque base de données et est utilisé pour implémenter la [récupération de base de données accélérée](../accelerated-database-recovery.md). Pour déterminer la taille actuelle du magasin PVS, consultez [Résolution des problèmes de PVS](/sql/relational-databases/accelerated-database-recovery-management#troubleshooting). Une grande taille de PVS est souvent due à une transaction qui reste ouverte pendant une longue période (plusieurs heures), ce qui empêche le nettoyage des anciennes versions dans le magasin PVS.
- Pour les bases de données volumineuses des niveaux de service Premium et Critique pour l'entreprise, vous pouvez recevoir une erreur de type Espace insuffisant, même si l'espace utilisé dans la base de données est inférieur à la limite définie en termes de taille de données maximale. Cela peut se produire si tempdb ou le journal des transactions consomme une grande quantité de stockage proche de la limite de stockage local. [Basculez](high-availability-sla.md#testing-application-fault-resiliency) la base de données ou le pool élastique pour réinitialiser tempdb sur sa taille inférieure initiale, ou [réduisez](file-space-manage.md#shrinking-transaction-log-file) le journal des transactions pour abaisser la consommation du stockage local.

### <a name="sessions-and-workers-requests"></a>Sessions et workers (requêtes)

Le nombre maximal de sessions et de rôles de travail est déterminé par le niveau de service et la taille de calcul. Lorsque les limites de sessions ou de workers sont atteintes, les nouvelles requêtes sont refusées et les clients reçoivent un message d’erreur. Si le nombre de connexions disponibles peut être contrôlé par l’application, le nombre de workers simultanés est souvent plus difficile à estimer et à contrôler. Cela est particulièrement vrai pendant les pics de charge lorsque les limites de ressources de base de données sont atteintes et que les rôles de travail s’accumulent en raison de requêtes durables, de chaînes de blocage volumineuses ou d’un parallélisme excessif de requêtes.

En cas d’utilisation élevée de workers ou de sessions, voici certaines des options d’atténuation à votre disposition :

- Augmenter le niveau de service ou la taille de calcul du pool élastique ou de la base de données. Consultez [Mise à l’échelle des ressources d’une base de données unique](single-database-scale.md) et [Mise à l'échelle des ressources d’un pool élastique](elastic-pool-scale.md).
- Optimiser les requêtes afin de réduire l’utilisation des ressources de chaque requête si la cause de l’utilisation du rôle de travail accrue est un problème de contention des ressources de calcul. Pour plus d’informations, consultez la page [Paramétrage/Compréhension de requêtes](performance-guidance.md#query-tuning-and-hinting).
- Réduction du paramètre [MAXDOP](configure-max-degree-of-parallelism.md) (degré maximal de parallélisme).
- Optimisation de la charge de travail des requêtes afin de réduire le nombre d’occurrences et la durée du blocage des requêtes. Pour plus d’informations, consultez [Compréhension et résolution des problèmes bloquants Azure SQL](understand-resolve-blocking.md).

### <a name="memory"></a>Mémoire

Contrairement à d’autres ressources (processeur, threads, stockage), l’atteinte de la limite de mémoire n’a pas d’impact négatif sur les performances des requêtes et ne provoque pas d’erreurs ni d’échecs. Comme décrit dans [Guide d’architecture de gestion de la mémoire](/sql/relational-databases/memory-management-architecture-guide), le moteur de base de données utilise souvent toute la mémoire disponible par défaut. La mémoire est principalement utilisée pour la mise en cache des données, afin d’éviter un accès au stockage plus lent. Ainsi, une utilisation plus importante de la mémoire améliore généralement les performances des requêtes, les lectures étant plus rapides à partir de la mémoire qu’à partir du stockage.

Après le démarrage du moteur de base de données, quand la charge de travail commence à lire les données à partir du stockage, le moteur de base de données met en cache les données en mémoire de façon intensive. Après cette période de démarrage initiale, il est courant et normal que les colonnes `avg_memory_usage_percent` et `avg_instance_memory_percent` dans [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) soient proches de 100 %, ou égales à ce niveau, en particulier pour les bases de données qui ne sont pas inactives et qui ne tiennent pas entièrement dans la mémoire.

Outre le cache de données, la mémoire est utilisée dans d’autres composants du moteur de base de données. En cas de demande de mémoire et si toute la mémoire disponible a été utilisée par le cache de données, le moteur de base de données réduit de manière dynamique la taille du cache de données afin de libérer de la mémoire pour d’autres composants et augmente dynamiquement le cache de données quand d’autres composants libèrent de la mémoire.

Dans de rares cas, une charge de travail suffisamment exigeante peut entraîner une insuffisance de mémoire, entraînant des erreurs de mémoire insuffisante. Cela peut se produire à n’importe quel niveau d’utilisation de la mémoire compris entre 0 % et 100 %. Cela est plus susceptible de se produire sur de plus petites tailles de calcul qui ont des limites de mémoire proportionnellement plus petites et/ou avec des charges de travail qui utilisent davantage de mémoire pour le traitement des requêtes, par exemple dans les [pools élastiques denses](elastic-pool-resource-management.md).

Quand vous rencontrez des erreurs de mémoire insuffisante, les options d’atténuation sont les suivantes :
- Augmenter le niveau de service ou la taille de calcul du pool élastique ou de la base de données. Consultez [Mise à l’échelle des ressources d’une base de données unique](single-database-scale.md) et [Mise à l'échelle des ressources d’un pool élastique](elastic-pool-scale.md).
- Optimiser les requêtes et la configuration pour réduire l’utilisation de la mémoire. Les solutions courantes sont décrites dans le tableau suivant.

|Solution|Description|
| :----- | :----- |
|Réduire la taille des allocations de mémoire|Pour plus d’informations sur les allocations de mémoire, consultez le billet de blog [Understanding SQL Server memory grant](https://techcommunity.microsoft.com/t5/sql-server/understanding-sql-server-memory-grant/ba-p/383595). Une solution courante pour éviter les allocations de mémoire excessivement grandes consiste à maintenir des [statistiques](/sql/relational-databases/statistics/statistics) à jour. Cela aboutit à des estimations plus précises de la consommation de mémoire par le moteur de requête, ce qui évite les allocations de mémoire inutilement grandes.</br></br>Dans les bases de données utilisant les niveaux de compatibilité 140 et supérieurs, le moteur de base de données peut ajuster automatiquement la taille d’allocation de mémoire à l’aide du [retour d’allocation de mémoire en mode batch](/sql/relational-databases/performance/intelligent-query-processing#batch-mode-memory-grant-feedback). Dans les bases de données utilisant les niveaux de compatibilité 150 et supérieurs, le moteur de base de données utilise de la même manière la [Retour d’allocation de mémoire en mode ligne](/sql/relational-databases/performance/intelligent-query-processing#row-mode-memory-grant-feedback), pour les requêtes en mode ligne plus courantes. Cette fonctionnalité intégrée permet d’éviter les erreurs de mémoire insuffisante en raison d’allocations de mémoire inutilement grandes.|
|Réduire la taille du cache du plan de requête|Le moteur de base de données met en cache les plans de requête en mémoire, afin d’éviter de compiler un plan de requête pour chaque exécution de requête. Pour éviter que la mise en cache de plans qui ne sont utilisés qu’une seule fois entraîne une augmentation du cache du plan de requête, activez la configuration de portée base de données [OPTIMIZE_FOR_AD_HOC_WORKLOADS](/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql).|
|Réduire la taille de la mémoire des verrous|Le moteur de base de données utilise de la mémoire pour les [verrous](/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide#Lock_Engine). Dans la mesure du possible, évitez les grandes transactions qui peuvent acquérir un grand nombre de verrous et provoquer une consommation de mémoire de verrous élevée.|

## <a name="resource-consumption-by-user-workloads-and-internal-processes"></a>Consommation de ressources par les charges de travail utilisateur et les processus internes

Azure SQL Database nécessite des ressources de calcul pour mettre en œuvre des fonctionnalités de service essentielles telles que la haute disponibilité et la récupération d’urgence, la sauvegarde et la restauration de base de données, la surveillance, le magasin de requêtes, le réglage automatique, etc. Le système met de côté une certaine partie des ressources globales pour ces processus internes à l’aide de mécanismes de [gouvernance des ressources](#resource-governance), et les autres ressources restent donc disponibles pour les charges de travail utilisateur. Parfois, quand des processus internes n’utilisent pas les ressources de calcul, le système les met à disposition des charges de travail utilisateur.

La consommation d’UC et de mémoire totale par les charges de travail utilisateur et les processus internes est signalée dans les vues [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) et [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) des colonnes `avg_instance_cpu_percent` et `avg_instance_memory_percent`. Ces données sont également signalées via les métriques Azure Monitor `sqlserver_process_core_percent` et `sqlserver_process_memory_percent`, pour les [bases de données uniques](../../azure-monitor/essentials/metrics-supported.md#microsoftsqlserversdatabases) et les [pools élastiques](../../azure-monitor/essentials/metrics-supported.md#microsoftsqlserverselasticpools) au niveau du pool.

La consommation d’UC et de mémoire par les charges de travail utilisateur dans chaque base de données est signalée dans les vues [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database) et [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) des colonnes `avg_cpu_percent` et `avg_memory_usage_percent`. Pour les pools élastiques, la consommation de ressources au niveau du pool est signalée dans la vue [sys.elastic_pool_resource_stats](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database). La consommation de processeur de la charge de travail utilisateur est également signalée via la métrique Azure Monitor `cpu_percent`, pour les [bases de données uniques](../../azure-monitor/essentials/metrics-supported.md#microsoftsqlserversdatabases) et les [pools élastiques](../../azure-monitor/essentials/metrics-supported.md#microsoftsqlserverselasticpools) au niveau du pool.

Une répartition plus détaillée de la consommation récente des ressources par les charges de travail utilisateur et les processus internes est indiquée dans les vues [sys.dm_resource_governor_resource_pools_history_ex](/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-history-ex-azure-sql-database) et [sys.dm_resource_governor_workload_groups_history_ex](/sql/relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-history-ex-azure-sql-database). Pour plus d’informations sur les pools de ressources et les groupes de charges de travail référencés dans ces vues, consultez [Gouvernance des ressources](#resource-governance). Ces vues signalent l’utilisation des ressources par les charges de travail utilisateur et par des processus internes spécifiques dans les pools de ressources et les groupes de charges de travail associés.

Dans le contexte de la supervision et du dépannage des performances, il est important de prendre en compte la **consommation de processeur de l’utilisateur** (`avg_cpu_percent`, `cpu_percent`) et la **consommation de processeur totale** par les charges de travail des utilisateurs et les processus internes (`avg_instance_cpu_percent`, `sqlserver_process_core_percent`).

**La consommation d’UC de l’utilisateur** est calculée sous la forme d’un pourcentage des limites de charge de travail utilisateur dans chaque objectif de service. Une **utilisation de processeur de l’utilisateur** à 100 % indique que la charge de travail de l’utilisateur a atteint la limite de l’objectif de service. Toutefois, quand la **consommation de processeur totale** atteint la plage de 70-100 %, il est possible de voir le débit de charge de travail de l’utilisateur s’aplanir et la latence des requêtes augmenter, même si la **consommation de processeur de l’utilisateur** signalée reste inférieure à 100 %. La probabilité que cela se produise est supérieure lors de l’utilisation de plus petits objectifs de service avec une allocation modérée des ressources de calcul, mais avec des charges de travail utilisateur relativement intenses, par exemple dans des [pools élastiques denses](elastic-pool-resource-management.md). Cela peut également se produire avec des objectifs de service inférieurs, lorsque des processus internes requièrent temporairement des ressources supplémentaires, par exemple lors de la création d’un réplica de base de données, ou de la sauvegarde de la base de données.

Quand la **consommation de processeur totale** est élevée, les options d’atténuation sont les mêmes que celles indiquées précédemment dans la section [Processeur de calcul](#compute-cpu), et incluent l’augmentation de l’objectif de service et/ou l’optimisation de la charge de travail utilisateur.

## <a name="resource-governance"></a>Gouvernance des ressources

Pour appliquer des limites de ressources, Azure SQL Database utilise une implémentation de la gouvernance des ressources basée sur [Resource Governor](/sql/relational-databases/resource-governor/resource-governor) SQL Server, modifié et étendu pour s’exécuter dans le cloud. Dans SQL Database, il existe plusieurs [listes de ressources partagées](/sql/relational-databases/resource-governor/resource-governor-resource-pool) et [groupes de charges de travail](/sql/relational-databases/resource-governor/resource-governor-workload-group), avec des limites de ressources définies au niveau des pools et des groupes pour fournir une [base de données en tant que service équilibrée](https://azure.microsoft.com/blog/resource-governance-in-azure-sql-database/). La charge de travail utilisateur et les charges de travail internes sont classées dans des listes de ressources partagées et des groupes de charges de travail distincts. La charge de travail utilisateur sur les réplicas primaires et secondaires accessibles en lecture, y compris les géoréplications, est classée dans la liste de ressources partagées `SloSharedPool1` et dans le groupe de charges de travail `UserPrimaryGroup.DBId[N]`, où `[N]` correspond à la valeur de l’ID de base de données. En outre, il existe plusieurs listes de ressources partagées et groupes de charges de travail pour différentes charges de travail internes.

Outre l’utilisation de Resource Governor pour régir les ressources au sein du moteur de base de données, Azure SQL Database utilise également les [objets de traitement](/windows/win32/procthread/job-objects) Windows pour la gouvernance des ressources au niveau du processus et les [Outils de gestion de ressources pour serveur de fichiers (FSRM)](/windows-server/storage/fsrm/fsrm-overview) Windows pour la gestion des quotas de stockage.

La gouvernance des ressources Azure SQL Database est hiérarchique par nature. De haut en bas, les limites sont appliquées au niveau du système d’exploitation et au niveau du volume de stockage à l’aide des mécanismes de gouvernance des ressources du système d’exploitation et de Resource Governor, puis au niveau de la liste de ressources partagées à l’aide de Resource Governor, et enfin au niveau du groupe de charge de travail à l’aide de Resource Governor. Les limites de gouvernance des ressources en vigueur pour la base de données ou le pool élastique actuels sont rapportées dans l’affichage [sys.dm_user_db_resource_governance](/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database).

### <a name="data-io-governance"></a>Gouvernance des E/S de données

La gouvernance des E/S de données est un processus d’Azure SQL Database utilisé pour limiter les E/S physiques en lecture et en écriture sur les fichiers de données d’une base de données. Les limites IOPS sont définies pour chaque niveau de service afin de réduire l’effet de « voisinage bruyant », d’assurer une allocation des ressources équitable dans un service multilocataire et de rester dans les limites des capacités du matériel et du stockage sous-jacents.

Pour les bases de données uniques, les limites du groupe de charge de travail sont appliquées à toutes les entrées/sorties de stockage de la base de données. Pour les pools élastiques, les limites de groupe de charges de travail s’appliquent à chaque base de données dans le pool. En outre, la limite du pool de ressources s’applique également aux e/s cumulatives du pool élastique. Les e/s de tempdb sont soumises aux limites des groupes de charges de travail, à l’exception des niveaux de service de base, standard et usage général, où les limites d’e/s tempdb plus élevées s’appliquent. En général, les limites des listes de ressources partagées peuvent ne pas être réalisables par la charge de travail sur une base de données (unique ou en pool), car les limites des groupes de charge de travail sont inférieures à celles des listes de ressources partagées et restreignent l’IOPS ou le débit plus tôt. Toutefois, les limites du pool peuvent être atteintes par la charge de travail combinée sur plusieurs bases de données pour le même pool.

Par exemple, si une requête génère 1000 IOPS sans gouvernance des ressources d’E/S, mais que la limite IOPS maximale du groupe de charge de travail est définie sur 900 IOPS, la requête ne peut pas générer plus de 900 IOPS. Toutefois, si la limite IOPS maximale de la liste de ressources partagées est définie sur 1 500 IOPS et que le nombre total d’E/S de tous les groupes de charge de travail associés à la liste de ressources partagées dépasse 1 500 IOPS, les E/S de la même requête peuvent être réduites sous la limite du groupe de travail de 900 IOPS.

Les valeurs maximales d’IOPS et de débit renvoyées par l’affichage [sys.dm_user_db_resource_governance](/sql/relational-databases/system-dynamic-management-views/sys-dm-user-db-resource-governor-azure-sql-database) agissent comme limites/seuils, et non comme garanties. En outre, la gouvernance des ressources ne garantit pas une latence de stockage spécifique. Les meilleurs latence, IOPS et débit possible pour une charge de travail utilisateur donnée dépendent non seulement des limites de gouvernance des ressources d’E/S, mais également de la combinaison des tailles d’E/S utilisées et des fonctionnalités du stockage sous-jacent. SQL Database utilise des E/S dont la taille varie entre 512 Ko et 4 Mo. Dans le cadre de l’application des limites d’IOPS, chaque E/S est comptabilisée, quelle que soit sa taille, à l’exception des bases de données ayant des fichiers de données dans Stockage Azure. Dans ce cas, les E/S de plus de 256 Ko sont comptabilisées comme plusieurs E/S de 256 Ko, afin de les aligner sur la comptabilisation des E/S de Stockage Azure.

Pour les bases de données De base, Standard et Usage général, qui utilisent des fichiers de données dans Stockage Azure, la valeur de `primary_group_max_io` peut ne pas être réalisable si une base de données ne dispose pas de suffisamment de fichiers de données pour fournir de manière cumulative ce nombre d’IOPS, si les données ne sont pas distribuées de manière égale entre les fichiers ou si le niveau de performance des blobs sous-jacents restreint l’IOPS ou le débit au-dessous des limites de gouvernance des ressources. De même, avec de petites E/S de journaux générées par les validations fréquentes de transactions, la valeur de `primary_max_log_rate` peut ne pas être réalisable par une charge de travail en raison de la limite d’IOPS sur le blob Stockage Azure sous-jacent. Pour les bases de données utilisant Stockage Premium Azure, Azure SQL Database utilise des blobs de stockage suffisamment volumineux pour obtenir l’IOPS ou le débit nécessaires, quelle que soit la taille de la base de données. Pour les bases de données plus volumineuses, plusieurs fichiers de données sont créés pour augmenter la capacité totale d’IOPS ou de débit.

Les valeurs d’utilisation des ressources telles que `avg_data_io_percent` et `avg_log_write_percent`, signalées dans les affichages [sys.dm_db_resource_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database), [sys.resource_stats](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database) et [sys.elastic_pool_resource_stats](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database), sont calculées sous la forme de pourcentages des limites de gouvernance des ressources maximales. Ainsi, quand des facteurs autres que la gouvernance des ressources limitent l’IOPS/le débit, il est possible de voir les courbes d’IOPS/de débit s’aplatir et les latences qui augmentent à mesure que la charge de travail augmente, même si l’utilisation des ressources signalée reste inférieure à 100 %.

Pour déterminer l’IOPS, le débit et la latence en lecture et écriture par fichier de base de données, utilisez la fonction [sys.dm_io_virtual_file_stats()](/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql). Cette fonction couvre toutes les E/S de la base de données, y compris les E/S en arrière-plan qui ne sont pas prises en compte pour `avg_data_io_percent`, mais utilise l’IOPS et le débit du stockage sous-jacent et peut avoir un impact sur la latence de stockage observée. La fonction indique une latence supplémentaire qui peut être introduite par la gouvernance des ressources d’E/S pour les lectures et écritures, respectivement dans les colonnes `io_stall_queued_read_ms` et `io_stall_queued_write_ms`.

### <a name="transaction-log-rate-governance"></a>Gouvernance relative au taux de journalisation des transactions

Ce type de gouvernance est un processus utilisé dans Azure SQL Database pour réduire les taux d’ingestions élevés des charges de travail de type bulk Insert, opérations SELECT INTO et création d’index. Ces limites font l’objet d’un suivi et sont appliquées à une vitesse inférieure à la seconde, selon le rythme de génération des enregistrements de journal, ce qui limite le débit quel que soit le nombre d’E/S générées sur les fichiers de données. Les taux de génération du journal des transactions sont actuellement mis à l’échelle de façon linéaire jusqu’à un point dépendant du matériel et du niveau de service.

Les taux de journalisation sont configurés de sorte qu’il soit possible de les obtenir et de les maintenir dans un large éventail de scénarios, tandis que l’ensemble du système peut gérer ses fonctionnalités avec un impact réduit sur la charge utilisateur. La gouvernance des taux de journalisation garantit que les sauvegardes de fichier journal des transactions respectent les SLA publiés en matière de récupérabilité. Cette gouvernance empêche également un backlog excessif sur les réplicas secondaires, ce qui pourrait autrement entraîner un temps d’arrêt plus long que prévu pendant les basculements.

Les E/S physiques réelles pour les fichiers journaux des transactions ne sont pas régies ou limitées. Lorsque des enregistrements de journaux sont générés, chaque opération est évaluée, dans le but de déterminer si elle doit être différée afin que le taux de journalisation maximum souhaité soit maintenu (en Mo/s par seconde). Les retards ne sont pas ajoutés quand les enregistrements de journaux sont vidés dans le stockage : la gouvernance du taux de journalisation est appliquée lors de la génération du taux de journalisation lui-même.

Les taux de génération de journaux réels imposés lors de l’exécution peuvent également être influencés par des mécanismes de commentaires, ce qui réduit temporairement les taux de journalisation disponibles afin de permettre au système de se stabiliser. La gestion de l’espace des fichiers journaux, l’absence d’exécution en cas de saturation de l’espace de journalisation disponible et les mécanismes de réplication des données peuvent réduire temporairement les limites totales du système.

La mise en forme du trafic de l’administrateur des taux de journalisation est présentée par le biais des types d’attente suivants (exposés dans les vues [sys.dm_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) et [sys.dm_os_wait_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql)) :

| Type d’attente | Notes |
| :--- | :--- |
| LOG_RATE_GOVERNOR | Limitation appliquée aux bases de données |
| POOL_LOG_RATE_GOVERNOR | Limitation appliquée aux pools |
| INSTANCE_LOG_RATE_GOVERNOR | Limitation appliquée aux niveaux d’instances |  
| HADR_THROTTLE_LOG_RATE_SEND_RECV_QUEUE_SIZE | Contrôle des commentaires, réplication physique des groupes de disponibilité dans la section Critique pour l’entreprise/Premium ne suivant pas |  
| HADR_THROTTLE_LOG_RATE_LOG_SIZE | Contrôle des commentaires, limitation de taux pour éviter une condition de saturation de l’espace de journalisation |
| HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO | Contrôle des commentaires relatifs à la géoréplication, limitation du taux de journalisation pour éviter une latence élevée des données et une indisponibilité des zones géographiques secondaires|
|||

Lorsque vous rencontrez une limite de taux de journalisation qui entrave l’évolutivité du système, envisagez les options suivantes :

- Montez en puissance vers un niveau de service supérieur afin d’obtenir le taux de consignation maximal d’un niveau de service, ou basculez vers un autre niveau de service. Le niveau de service [Hyperscale](service-tier-hyperscale.md) fournit un taux de journalisation de 100 Mo/s, quel que soit le niveau de service choisi.
- Si les données en cours de chargement sont temporaires, telles que des données de processus de site dans un processus ETL, vous pouvez les charger dans la base de données tempdb (qui présente une journalisation minime).
- Pour des scénarios d’analyse, chargez dans une table [columnstore](/sql/relational-databases/indexes/columnstore-indexes-overview) en cluster, ou une table avec des index qui utilisent la [compression de données](/sql/relational-databases/data-compression/data-compression). Cela réduit le taux de consignation requis. Cette technique augmente l’utilisation de l’UC et s’applique uniquement aux jeux de données qui bénéficient d’index columnstore en cluster ou de la compression de données.

### <a name="storage-space-governance"></a>Gouvernance de l’espace de stockage

Aux niveaux de service Premium et Critique pour l'entreprise, les données client, notamment les *fichiers de données*, les *fichiers journaux de transactions* et les *fichiers tempdb*, sont stockées sur le stockage SSD local de l'ordinateur qui héberge la base de données ou le pool élastique. Le stockage SSD local offre des IOPS et un débit élevés, ainsi qu'une faible latence des entrées/sorties. Outre les données clients, le stockage local est utilisé pour le système d'exploitation, le logiciel de gestion, les données de surveillance et les journaux, ainsi que d'autres fichiers nécessaires au fonctionnement du système.

La taille du stockage local est limitée et dépend des capacités matérielles, qui déterminent la **valeur maximale de stockage local**, ou le stockage local réservé aux données client. Cette limite est définie pour optimiser le stockage des données client, tout en garantissant un fonctionnement sûr et fiable du système. Pour connaître la valeur **maximale de stockage local** de chaque objectif de service, consultez la documentation consacrée aux limites de ressources des [bases de données uniques](resource-limits-vcore-single-databases.md) et des [pools élastiques](resource-limits-vcore-elastic-pools.md).

Vous pouvez également accéder à cette valeur, ainsi qu'à la quantité de stockage local actuellement utilisée par une base de données ou un pool élastique, en utilisant la requête suivante :

```sql
SELECT server_name, database_name, slo_name, user_data_directory_space_quota_mb, user_data_directory_space_usage_mb
FROM sys.dm_user_db_resource_governance
WHERE database_id = DB_ID();
```

|Colonne|Description|
| :----- | :----- |
|`server_name`|Nom du serveur logique|
|`database_name`|Nom de la base de données|
|`slo_name`|Nom de l'objectif de service, génération matérielle comprise|
|`user_data_directory_space_quota_mb`|**Valeur maximale de stockage local**, en Mo|
|`user_data_directory_space_usage_mb`|Consommation actuelle de stockage local par les fichiers de données, les fichiers journaux de transactions et les fichiers tempdb, en Mo. Mise à jour toutes les cinq minutes.|
|||

Cette requête doit être exécutée dans la base de données utilisateur, et non dans la base de données MASTER. Pour les pools élastiques, la requête peut être exécutée dans n'importe quelle base de données du pool. Les valeurs signalées s'appliquent à l'ensemble du pool.

> [!IMPORTANT]
> Aux niveaux de service Premium et Critique pour l'entreprise, si la charge de travail tente d'augmenter la consommation de stockage local combinée des fichiers de données, des fichiers journaux de transactions et des fichiers tempdb au-delà de la limite **maximale de stockage local**, une erreur de type Espace insuffisant se produit.

Au fur et à mesure que les bases de données sont créées, supprimées et que leur taille augmente ou diminue, la consommation du stockage local d'un ordinateur fluctue. Si le système détecte que le stockage local disponible sur un ordinateur est faible et qu'une base de données ou un pool élastique risque de manquer d'espace, il déplace la base de données ou le pool élastique vers un autre ordinateur disposant de suffisamment de stockage local.

Ce déplacement s’effectue en ligne, de la même manière qu’une opération de mise à l’échelle d’une base de données, et a un [impact similaire](single-database-scale.md#impact), notamment un bref basculement (quelques secondes) à la fin de l’opération. Ce basculement met fin aux connexions ouvertes et restaure les transactions, ce qui peut avoir un impact sur les applications qui utilisent la base de données à ce moment-là.

Comme toutes les données sont copiées sur un volume de stockage local situé sur un autre ordinateur, le déplacement de bases de données volumineuses peut prendre beaucoup de temps. Pendant ce temps, si la consommation d'espace local par la base de données ou le pool élastique, ou par la base de données tempdb, augmente rapidement, le risque de manquer d'espace augmente. Le système lance le déplacement de la base de données de façon équilibrée afin de réduire les risques d'erreurs dues au manque d'espace tout en évitant les basculements inutiles.

> [!NOTE]
> Les déplacements de base de données dus à un stockage local insuffisant ne se produisent qu'aux niveaux de service Premium ou Critique pour l'entreprise. Ils ne se produisent pas aux niveaux de service Hyperscale, Usage général, Standard et De base, car ces niveaux ne permettent pas de stocker les fichiers de données localement.

## <a name="next-steps"></a>Étapes suivantes

- Pour plus d’informations sur les limites générales d’Azure, consultez [Abonnement Azure et limites, quotas et contraintes du service](../../azure-resource-manager/management/azure-subscription-service-limits.md).
- Pour plus d’informations sur les DTU et eDTU, consultez [DTU et eDTU](purchasing-models.md#dtu-based-purchasing-model).
- Pour plus d’informations sur les limites de taille de tempdb, consultez [Base de données tempdb dans SQL Database](/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database).
