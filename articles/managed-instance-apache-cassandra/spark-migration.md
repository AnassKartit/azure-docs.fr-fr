---
title: Migration vers Azure Managed Instance pour Apache Cassandra avec Apache Spark
description: Découvrez comment migrer vers Azure Managed Instance pour Apache Cassandra avec Apache Spark.
author: TheovanKraay
ms.author: thvankra
ms.service: managed-instance-apache-cassandra
ms.topic: tutorial
ms.date: 11/02/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: 7c061f808dfbe6b93d9fd6a9266c152fa5aecf57
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131005249"
---
# <a name="migrate-to-azure-managed-instance-for-apache-cassandra-using-apache-spark"></a>Migration vers Azure Managed Instance pour Apache Cassandra avec Apache Spark

Dans la mesure du possible, nous vous recommandons d’utiliser la réplication native Apache Cassandra pour migrer des données de votre cluster actuel vers Azure Managed Instance pour Apache Cassandra en configurant un [cluster hybride](configure-hybrid-cluster.md). Cette approche utilise le protocole de bavardage d’Apache Cassandra pour répliquer des données de votre centre de données source vers votre nouveau centre de données d’instance gérée. Toutefois, il peut y avoir des scénarios dans lesquels la version de la base de données source n’est pas compatible, ou une configuration de cluster hybride n’est pas possible. 

Ce tutoriel explique comment migrer des données vers Azure Managed Instance pour Apache Cassandra en mode hors connexion avec le connecteur Spark Cassandra et Azure Databricks pour Apache Spark.

## <a name="prerequisites"></a>Prérequis

* Approvisionnez un cluster Azure Managed Instance pour Apache Cassandra à l’aide du [portail Azure](create-cluster-portal.md) ou d’[Azure CLI](create-cluster-cli.md), et assurez-vous que vous pouvez [vous connecter à votre cluster avec CQLSH](./create-cluster-portal.md#connecting-to-your-cluster).

* [Approvisionnez un compte Azure Databricks à l’intérieur de votre réseau virtuel Cassandra géré](deploy-cluster-databricks.md). Veillez à ce qu’il dispose également d’un accès réseau à votre cluster Cassandra source.

* Assurez-vous que vous avez déjà migré l’espace de clés/le schéma de table de votre base de données Cassandra source vers votre base de données Cassandra Managed Instance cible.

## <a name="provision-an-azure-databricks-cluster"></a>Provisionner un cluster Azure Databricks

Nous vous recommandons de sélectionner le runtime Databricks version 7.5, qui prend en charge Spark 3.0.

:::image type="content" source="../cosmos-db/cassandra/media/migrate-data-databricks/databricks-runtime.png" alt-text="Capture d’écran montrant la détection de la version du runtime Databricks.":::

## <a name="add-dependencies"></a>Ajout de dépendances

Ajoutez la bibliothèque du connecteur Apache Spark Cassandra à votre cluster pour vous connecter aux points de terminaison Cassandra natifs et Azure Cosmos DB. Dans votre cluster, sélectionnez **Bibliothèques** > **Installer nouveau** > **Maven**, puis ajoutez `com.datastax.spark:spark-cassandra-connector-assembly_2.12:3.0.0` dans les coordonnées Maven.

:::image type="content" source="../cosmos-db/cassandra/media/migrate-data-databricks/databricks-search-packages.png" alt-text="Capture d’écran montrant la recherche de packages Maven dans Databricks.":::

Sélectionnez **Installer**, puis redémarrez le cluster une fois l’installation terminée.

> [!NOTE]
> Veillez à redémarrer le cluster Databricks après l’installation de la bibliothèque du connecteur Cassandra.

## <a name="create-scala-notebook-for-migration"></a>Créer un notebook Scala pour la migration

Créez un notebook Scala dans Databricks. Remplacez vos configurations Cassandra source et cible par les informations d’identification correspondantes, ainsi que les espaces de clés et les tables sources et cibles. Exécutez ensuite le code suivant :

```scala
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql._
import org.apache.spark.SparkContext

// source cassandra configs
val sourceCassandra = Map( 
    "spark.cassandra.connection.host" -> "<Source Cassandra Host>",
    "spark.cassandra.connection.port" -> "9042",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "false",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>"
)

//target cassandra configs
val targetCassandra = Map( 
    "spark.cassandra.connection.host" -> "<Source Cassandra Host>",
    "spark.cassandra.connection.port" -> "9042",
    "spark.cassandra.auth.username" -> "<USERNAME>",
    "spark.cassandra.auth.password" -> "<PASSWORD>",
    "spark.cassandra.connection.ssl.enabled" -> "true",
    "keyspace" -> "<KEYSPACE>",
    "table" -> "<TABLE>",
    //throughput related settings below - tweak these depending on data volumes. 
    "spark.cassandra.output.batch.size.rows"-> "1",
    "spark.cassandra.output.concurrent.writes" -> "1000",
    "spark.cassandra.connection.remoteConnectionsPerExecutor" -> "10",
    "spark.cassandra.concurrent.reads" -> "512",
    "spark.cassandra.output.batch.grouping.buffer.size" -> "1000",
    "spark.cassandra.connection.keep_alive_ms" -> "600000000"
)

//Read from source Cassandra
val DFfromSourceCassandra = sqlContext
  .read
  .format("org.apache.spark.sql.cassandra")
  .options(sourceCassandra)
  .load
  
//Write to target Cassandra
DFfromSourceCassandra
  .write
  .format("org.apache.spark.sql.cassandra")
  .options(targetCassandra)
  .mode(SaveMode.Append) // only required for Spark 3.x
  .save
```

> [!NOTE]
> Si vous avez besoin de conserver ou d’antidater le `writetime` de chaque ligne, consultez l’article [Migration dynamique](dual-write-proxy-migration.md). 

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Gérer les ressources Azure Managed Instance pour Apache Cassandra à l’aide d’Azure CLI](manage-resources-cli.md)
