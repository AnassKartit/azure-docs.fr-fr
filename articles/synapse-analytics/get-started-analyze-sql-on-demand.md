---
title: 'Tutoriel : Démarrage de l’analyse des données avec un pool SQL serverless'
description: Dans ce tutoriel, vous allez voir comment analyser des données avec un pool SQL serverless, en utilisant des données situées dans des bases de données Spark.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: tutorial
ms.date: 04/15/2021
ms.openlocfilehash: 492f8d42a072418586a6646a9beed2e750849b31
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2021
ms.locfileid: "129996983"
---
# <a name="analyze-data-with-a-serverless-sql-pool"></a>Analyser des données avec un pool SQL serverless

Dans ce tutoriel, vous allez apprendre à analyser des données avec un pool SQL serverless. 

## <a name="the-built-in-serverless-sql-pool"></a>Pool SQL serverless intégré

Les pools SQL serverless vous permettent d’utiliser SQL sans avoir de capacité de réserve. La facturation du pool SQL serverless est basée sur la quantité de données traitées pour exécuter la requête, et non sur le nombre de nœuds utilisés pour exécuter la requête.

Chaque espace de travail est fourni avec un pool SQL serverless préconfiguré, dit **Intégré**. 

## <a name="analyze-nyc-taxi-data-with-a-serverless-sql-pool"></a>Analyser les données de NYC Taxi avec un pool SQL serverless
 
> [!NOTE]
> Vérifiez que vous avez [placé les exemples de données dans le compte de stockage principal](get-started-create-workspace.md#place-sample-data-into-the-primary-storage-account).

1. Dans Synapse Studio, accédez au hub **Développer**
1. Un nouveau script SQL est alors créé.
1. Collez le code suivant dans le script.

    ```sql
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://contosolake.dfs.core.windows.net/users/NYCTripSmall.parquet',
            FORMAT='PARQUET'
        ) AS [result]
    ```
1. Cliquez sur **Exécuter**. 

L’exploration des données n’est qu’un scénario simplifié qui vous permet de comprendre les caractéristiques de base de vos données. Découvrez-en plus sur l’exploration et l’analyse des données dans ce [tutoriel](sql/tutorial-data-analyst.md).

## <a name="create-data-exploration-database"></a>Créer une base de données d’exploration des données

Vous pouvez parcourir le contenu des fichiers directement par le biais de la base de données `master`. Pour certains scénarios simples d’exploration des données, vous n’avez pas besoin de créer une base de données distincte.
En revanche, si vous approfondissez l’exploration des données, vous risquez d’avoir besoin de créer des objets utilitaires, comme les suivants :
- Sources de données externes qui représentent les références nommées pour les comptes de stockage.
- Informations d’identification étendues à la base de données qui vous permettent de spécifier le mode d’authentification auprès d’une source de données externe.
- Utilisateurs de base de données disposant des autorisations nécessaires pour accéder à des sources de données ou des objets de base de données.
- Vues, procédures et fonctions utilitaires que vous pouvez utiliser dans les requêtes.

1. Utilisez la base de données `master` pour créer une base de données distincte pour les objets de base de données personnalisés. Les objets de base de données personnalisés ne peuvent pas être créés dans la base de données `master`.

    ```sql
    CREATE DATABASE DataExplorationDB 
                    COLLATE Latin1_General_100_BIN2_UTF8
    ```

   > [!IMPORTANT]
   > Utilisez un classement avec un suffixe `_UTF8` pour vérifier que le texte UTF-8 est correctement converti en colonnes `VARCHAR`. `Latin1_General_100_BIN2_UTF8` offre les meilleures performances dans les requêtes qui lisent des données issues de fichiers Parquet et de conteneurs Cosmos DB.

2. Passez à `DataExplorationDB` où vous pouvez créer des objets utilitaires comme des informations d’identification et des sources de données.

    ```sql
    CREATE EXTERNAL DATA SOURCE ContosoLake
    WITH ( LOCATION = 'https://contosolake.dfs.core.windows.net')
    ```

   > [!NOTE]
   > Une source de données externe peut être créée sans informations d’identification. Si aucune information d’identification n’existe, l’identité de l’appelant est utilisée pour accéder à la source des données externes.

3. Utilisez éventuellement la base de données MASTER pour créer une connexion pour un utilisateur dans `DataExplorationDB` qui va accéder aux données externes :

    ```sql
    CREATE LOGIN data_explorer WITH PASSWORD = 'My Very Strong Password 1234!';
    ```

    Créez ensuite un utilisateur de base de données dans `DataExplorationDB` pour la connexion et octroyez-lui l’autorisation `ADMINISTER DATABASE BULK OPERATIONS`.
    ```sql
    CREATE USER data_explorer FOR LOGIN data_explorer;
    GO
    GRANT ADMINISTER DATABASE BULK OPERATIONS TO data_explorer;
    GO
    ```

4. Explorez le contenu du fichier à l’aide du chemin relatif et de la source de données :

    ```sql
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
                BULK '/users/NYCTripSmall.parquet',
                DATA_SOURCE = 'ContosoLake',
                FORMAT='PARQUET'
        ) AS [result]
    ```

La base de données d’exploration des données est tout simplement un espace réservé dans lequel vous pouvez stocker vos objets utilitaires. Un pool Synapse SQL vous permet d’effectuer bien d’autres tâches et de créer un entrepôt de données logique : une couche relationnelle créée par-dessus des sources de données Azure. Découvrez-en plus sur la création d’un entrepôt de données logique dans ce [tutoriel](sql/tutorial-data-analyst.md).

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Analyser des données avec un pool Spark serverless](get-started-analyze-spark.md)
