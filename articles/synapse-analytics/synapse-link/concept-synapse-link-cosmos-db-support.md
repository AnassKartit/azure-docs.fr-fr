---
title: Fonctionnalités prises en charge par Azure Synapse Link pour Azure Cosmos DB
description: Présentation de la liste des actions prises en charge par Azure Synapse Link pour Azure Cosmos DB
services: synapse-analytics
author: Rodrigossz
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: synapse-link
ms.date: 06/02/2021
ms.author: rosouz
ms.reviewer: jrasnick
ms.custom: cosmos-db
ms.openlocfilehash: 8400a479b45770570c43ec906a192bf4f05a71a0
ms.sourcegitcommit: dcf1defb393104f8afc6b707fc748e0ff4c81830
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2021
ms.locfileid: "123110392"
---
# <a name="azure-synapse-link-for-azure-cosmos-db-supported-features"></a>Fonctionnalités prises en charge par Azure Synapse Link pour Azure Cosmos DB

Cet article décrit les fonctionnalités actuellement prises en charge par Azure Synapse Link pour Azure Cosmos DB.

## <a name="azure-synapse-support"></a>Prise en charge d’Azure Synapse

Il existe deux types de conteneurs dans Azure Cosmos DB :
* Le conteneur HTAP : il s’agit d’un conteneur prenant en charge Synapse Link. Ce conteneur a à la fois un magasin transactionnel et un magasin analytique. 
* Conteneur OLTP : conteneur ne prenant pas en charge Synapse Link. Ce conteneur dispose uniquement d’un magasin transactionnel et n’a aucun magasin analytique.

Vous pouvez vous connecter à un conteneur Azure Cosmos DB sans activer Synapse Link. Dans ce scénario, vous pouvez uniquement lire et écrire des données dans le magasin transactionnel. La liste ci-dessous répertorie les fonctionnalités qui sont actuellement prises en charge dans Synapse Link pour Azure Cosmos DB. 

| Category              | Description |[Pool Apache Spark](../sql/on-demand-workspace-overview.md) | [Pool SQL serverless](../sql/on-demand-workspace-overview.md) |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- | ----------------------------------------------------------- |
| **Prise en charge à l’exécution** |Runtime Azure Synapse pris en charge pour accéder à Azure Cosmos DB| ✓ | ✓ |
| **Prise en charge de l’API Azure Cosmos DB** : | Type d’API Azure Cosmos DB pris en charge | SQL / MongoDB | SQL / MongoDB |
| **Object**  |Possibilité de créer des objets (tels qu’une table), qui pointent directement vers le conteneur Azure Cosmos DB| DataFrame, Affichage, Table | Affichage |
| **Lire**    | Type de conteneur Azure Cosmos DB qui peut être lu | OLTP / HTAP | HTAP  |
| **Écrire**   | Le runtime Azure Synapse peut-il être utilisé pour écrire des données dans un conteneur Azure Cosmos DB | Oui | Non |

* Si vous écrivez des données dans un conteneur Azure Cosmos DB à partir de Spark, ce processus s’effectuera via le magasin transactionnel d’Azure Cosmos DB. Ce processus aura un impact sur les performances transactionnelles d’Azure Cosmos DB, puisqu’il consommera des unités de requête.
* L’intégration d’un pool SQL dédié par le biais de tables externes n’est pas prise en charge actuellement.
 
## <a name="supported-code-generated-actions-for-spark"></a>Actions générées par le code prises en charge pour Spark

| Mouvement              | Description |OLTP |HTAP  |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- |----------------------------------------------------------- |
| **Charger dans un DataFrame** |Charger et lire des données dans un DataFrame Spark |✓| ✓ |
| **Créer une table Spark** |Créer une table pointant vers un conteneur Azure Cosmos DB|✓| ✓ |
| **Écrire un DataFrame dans un conteneur** |Écrire des données dans un conteneur|✓| ✓ |
| **Charger un DataFrame de streaming à partir d’un conteneur** |Streamer des données à l’aide du flux de modification Azure Cosmos DB|✓| ✓ |
| **Écrire un DataFrame de streaming dans le conteneur** |Streamer des données à l’aide du flux de modification Azure Cosmos DB|✓| ✓ |

## <a name="supported-code-generated-actions-for-serverless-sql-pool"></a>Actions générées par le code prises en charge pour un pool SQL serverless

| Mouvement              | Description |OLTP |HTAP |
| -------------------- | ----------------------------------------------------------- |----------------------------------------------------------- |----------------------------------------------------------- |
| **Explorer des données** |Explorer des données d’un conteneur avec une syntaxe T-SQL familière et une inférence de schéma automatique|X| ✓ |
| **Créer des vues et générer des rapports BI** |Créer une vue SQL pour avoir un accès direct à un conteneur pour BI par le biais d’un pool SQL serverless |X| ✓ |
| **Joindre des sources de données disparates et des données Cosmos DB** | Stocker les résultats de la requête de lecture des données à partir de conteneurs Cosmos DB avec les données dans le stockage Blob Azure ou Azure Data Lake Storage à l’aide de CETAS |X| ✓ |

## <a name="next-steps"></a>Étapes suivantes

* [Découvrir comment se connecter à Synapse Link pour Azure Cosmos DB](../quickstart-connect-synapse-link-cosmos-db.md)
* [Découvrir comment interroger le magasin analytique Cosmos DB avec Spark 3](how-to-query-analytical-store-spark-3.md)
* [Découvrir comment interroger le magasin analytique Cosmos DB avec Spark 2](how-to-query-analytical-store-spark.md)
