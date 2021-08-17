---
title: Migrer vos données vers un compte d’API Cassandra dans Azure Cosmos DB - Tutoriel
description: Ce tutoriel explique comment copier des données d'Apache Cassandra vers un compte d'API Cassandra dans Azure Cosmos DB.
author: TheovanKraay
ms.author: thvankra
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: 1f38c0f37daebae0a0320882c7232444819dae39
ms.sourcegitcommit: b5508e1b38758472cecdd876a2118aedf8089fec
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/09/2021
ms.locfileid: "113587820"
---
# <a name="tutorial-migrate-your-data-to-a-cassandra-api-account"></a>Tutoriel : Migrer vos données vers un compte d'API Cassandra
[!INCLUDE[appliesto-cassandra-api](includes/appliesto-cassandra-api.md)]

Si vous êtes développeur, vous avez peut-être des charges de travail Cassandra exécutées localement ou dans le cloud que vous aimeriez faire migrer vers Azure. Vous pouvez migrer de telles charges de travail vers un compte d’API Cassandra dans Azure Cosmos DB. Ce tutoriel fournit des instructions sur les différentes options permettant de migrer des données Apache Cassandra vers le compte d’API Cassandra dans Azure Cosmos DB.

Ce tutoriel décrit les tâches suivantes :

> [!div class="checklist"]
> * Planifier la migration
> * Prérequis pour la migration
> * Migrer des données à l'aide de la commande `cqlsh` `COPY`
> * Migrer des données à l’aide de Spark

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

## <a name="prerequisites-for-migration"></a>Prérequis pour la migration

* **Estimer vos besoins en débit :** avant de migrer des données vers le compte d'API Cassandra dans Azure Cosmos DB, vous devez estimer les besoins en débit de votre charge de travail. D'une façon générale, commencez par le débit moyen nécessaire pour les opérations CRUD, puis incluez le débit supplémentaire nécessaire pour les opérations ETL (Extract Transform Load) ou entraînant une charge ponctuelle importante. Vous avez besoin des informations suivantes pour planifier la migration : 

  * **Taille des données existantes ou estimation de la taille des données :** Définit la taille minimale de la base de données et le débit requis. Si vous estimez la taille des données pour une nouvelle application, vous pouvez supposer que les données sont uniformément réparties sur les lignes, et estimer la valeur en multipliant par la taille des données. 

  * **Débit nécessaire :** débit approximatif des opérations de lecture (interroger/obtenir) et d'écriture (mettre à jour/supprimer/insérer). Cette valeur est nécessaire pour calculer les unités de requête nécessaires, ainsi que la taille des données à l'état stable.  

  * **Le schéma :** connectez-vous à votre cluster Cassandra existant via `cqlsh` et exportez le schéma depuis Cassandra : 

    ```bash
    cqlsh [IP] "-e DESC SCHEMA" > orig_schema.cql
    ```

    Après avoir identifié les exigences de votre charge de travail existante, créez un compte, une base de données et des conteneurs Azure Cosmos DB conformément aux spécifications établies en matière de débit.  

  * **Déterminer le coût en unités de requête pour une opération :** vous pouvez déterminer les unités de requête avec n'importe quel kit de développement logiciel (SDK) pris en charge par l'API Cassandra. Cet exemple montre la version .NET pour obtenir les frais de RU.

    ```csharp
    var tableInsertStatement = table.Insert(sampleEntity);
    var insertResult = await tableInsertStatement.ExecuteAsync();

    foreach (string key in insertResult.Info.IncomingPayload)
      {
         byte[] valueInBytes = customPayload[key];
         double value = Encoding.UTF8.GetString(valueInBytes);
         Console.WriteLine($"CustomPayload:  {key}: {value}");
      }
    ```

* **Allouer le débit requis :** Azure Cosmos DB peut procéder à la mise à l'échelle automatique du stockage et du débit au fil de l'accroissement de vos besoins. Vous pouvez estimer vos besoins en débit en utilisant le [calculateur d’unités de requête d’Azure Cosmos DB](https://www.documentdb.com/capacityplanner). 

* **Créer des tables sur le compte d'API Cassandra :** avant d'entamer la migration des données, créez toutes vos tables à partir du portail Azure ou de `cqlsh`. Si vous migrez vers un compte Azure Cosmos DB qui dispose d'un débit de niveau base de données, veillez à fournir une clé de partition au moment de la création des conteneurs.

* **Augmenter le débit :** la durée de la migration de vos données dépend de la quantité de débit que vous avez provisionnée pour les tables dans Azure Cosmos DB. Augmentez le débit pendant la durée de la migration. Si le débit est plus élevé, vous pouvez éviter la limitation de débit et procéder à une migration plus rapide. Une fois que vous avez effectué la migration, diminuez le débit pour réduire les coûts. Nous vous recommandons également de disposer d'un compte Azure Cosmos DB dans la même région que votre base de données source. 

* **Activer TLS** : Azure Cosmos DB obéit à des normes et à des exigences strictes en matière de sécurité. Veillez à activer TLS lorsque vous interagissez avec votre compte. Quand vous utilisez CQL avec SSH, vous pouvez fournir des informations sur TLS.

## <a name="options-to-migrate-data"></a>Options pour migrer des données

Vous pouvez déplacer des données depuis des charges de travail Cassandra existantes vers Azure Cosmos DB à l'aide de la commande `cqlsh` `COPY` ou en utilisant Spark. 

### <a name="migrate-data-by-using-the-cqlsh-copy-command"></a>Migrer des données à l'aide de la commande cqlsh COPY

Utilisez la [commande CQL COPY](https://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) pour copier des données locales vers le compte d'API Cassandra dans Azure Cosmos DB.

1. Obtenez les informations de la chaîne de connexion de votre compte d’API Cassandra :

   * Connectez-vous au [portail Azure](https://portal.azure.com) et accédez à votre compte Azure Cosmos DB.

   * Ouvrez le volet **Chaîne de connexion**. Vous y trouverez toutes les informations dont vous avez besoin pour vous connecter à votre compte d'API Cassandra depuis `cqlsh`.

1. Connectez-vous à `cqlsh` en utilisant les informations de connexion du portail.

1. Utilisez la commande `CQL` `COPY` pour copier des données locales vers le compte d'API Cassandra.

   ```bash
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

### <a name="migrate-data-by-using-spark"></a>Migrer des données à l’aide de Spark 

Effectuez les étapes suivantes pour migrer des données vers le compte d’API Cassandra avec Spark :

1. Approvisionnez un [cluster Azure Databricks](cassandra-spark-databricks.md) ou un [cluster HDInsight](cassandra-spark-hdinsight.md). 

1. Déplacez les données vers le point de terminaison de l'API Cassandra de destination. Reportez-vous à ce [Guide de procédures](cassandra-migrate-cosmos-db-databricks.md) pour la migration avec Azure Databricks.

La migration de données par le biais de tâches Spark est une solution recommandée si vos données se trouvent dans un cluster existant dans des machines virtuelles Azure ou dans un autre cloud. Pour ce faire, vous devez configurer Spark en tant qu'intermédiaire pour une ingestion ponctuelle ou régulière. Vous pouvez accélérer cette migration avec une connectivité Azure ExpressRoute entre votre environnement local et Azure. 

## <a name="clean-up-resources"></a>Nettoyer les ressources

Dès que vous n'en avez plus besoin, vous pouvez supprimer le groupe de ressources, le compte Azure Cosmos DB et toutes les ressources associées. Pour cela, sélectionnez le groupe de ressources de la machine virtuelle, sélectionnez **Supprimer**, puis confirmez le nom du groupe de ressources à supprimer.

## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous avez appris à migrer vos données vers un compte d'API Cassandra dans Azure Cosmos DB. Vous pouvez maintenant découvrir d'autres concepts d'Azure Cosmos DB :

> [!div class="nextstepaction"]
> [Niveaux de cohérence des données paramétrables dans Azure Cosmos DB](../cosmos-db/consistency-levels.md)




