---
title: Se connecter à Azure Cosmos DB avec Compass
description: Découvrez comment utiliser MongoDB Compass pour stocker et gérer des données dans Azure Cosmos DB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 08/26/2021
author: gahl-levy
ms.author: gahllevy
ms.openlocfilehash: 8d345dad82c692c76ff2701c2b9a1b15ba5390c1
ms.sourcegitcommit: 03f0db2e8d91219cf88852c1e500ae86552d8249
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2021
ms.locfileid: "123032621"
---
# <a name="use-mongodb-compass-to-connect-to-azure-cosmos-dbs-api-for-mongodb"></a>Utiliser MongoDB Compass pour se connecter à l’API Azure Cosmos DB pour MongoDB
[!INCLUDE[appliesto-mongodb-api](../includes/appliesto-mongodb-api.md)]

Ce didacticiel montre comment utiliser [MongoDB Compass](https://www.mongodb.com/products/compass) lorsque vous stockez et/ou gérez des données dans Cosmos DB. Dans cette procédure pas à pas, nous utilisons l’API Azure Cosmos DB pour MongoDB. Pour ceux qui ne connaissent pas Compass, il s’agit d’une interface graphique pour MongoDB. Elle est couramment utilisée pour visualiser les données, exécuter des requêtes ad hoc et gérer les données.

Cosmos DB est le service de base de données multi-modèle de Microsoft, distribué à l’échelle mondiale. Vous avez la possibilité de créer et d’interroger rapidement des documents, des paires clé/valeur et des bases de données de graphe, profitant tous de la distribution à l’échelle mondiale et des capacités de mise à l’échelle horizontale au cœur de Cosmos DB.

## <a name="pre-requisites"></a>Conditions préalables

Pour vous connecter à votre compte Cosmos DB avec MongoDB Compass, vous devez effectuer les opérations suivantes :

* Télécharger et installer [Compass](https://www.mongodb.com/download-center/compass?jmp=hero)
* Obtenir vos informations de [chaîne de connexion](connect-mongodb-account.md) Cosmos DB

## <a name="connect-to-cosmos-dbs-api-for-mongodb"></a>Connexion à l’API Cosmos DB pour MongoDB

Pour connecter votre compte Cosmos DB à Compass, vous pouvez suivre les étapes ci-dessous :

1. Récupérez les informations de connexion pour votre compte Cosmos configuré avec l’API Azure Cosmos DB pour MongoDB en suivant les instructions [ici](connect-mongodb-account.md).

    :::image type="content" source="./media/connect-using-compass/mongodb-compass-connection.png" alt-text="Capture d’écran du panneau Chaîne de connexion":::

2. Cliquez sur le bouton intitulé **Copier dans le presse-papiers** à côté de votre **chaîne de connexion principale/secondaire** dans Cosmos DB. Cliquer sur ce bouton copiera votre chaîne de connexion complète dans votre presse-papiers.

    :::image type="content" source="./media/connect-using-compass/mongodb-connection-copy.png" alt-text="Capture d’écran du bouton Copier dans le presse-papiers":::

3. Ouvrez Compass sur votre ordinateur de bureau/machine, puis cliquez sur **Connexion**, puis **Se connecter à...** .

4. Compass détecte automatiquement la chaîne de connexion dans le presse-papiers et vous demande si vous souhaitez l’utiliser pour vous connecter. Cliquez sur **Oui** comme le montre la capture d'écran ci-dessous.

    :::image type="content" source="./media/connect-using-compass/mongodb-compass-detect.png" alt-text="Capture d’écran d’une boîte de dialogue expliquant que le Presse-papiers contient une chaîne de connexion":::

5. Lorsque vous cliquez sur **Oui** à l’étape précédente, les informations de la chaîne de connexion sont renseignées automatiquement. Supprimez la valeur remplie automatiquement dans **Définir un nom de réplica** pour garantir qu’elle reste vide.

    :::image type="content" source="./media/connect-using-compass/mongodb-compass-replica.png" alt-text="Capture d’écran de la zone de texte Nom du jeu de réplicas":::

6. En bas de la page, cliquez sur **Se connecter** . Votre compte Cosmos DB et ses bases de données doivent maintenant être visibles dans MongoDB Compass.

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment [utiliser Studio 3T](connect-using-mongochef.md) avec l’API Azure Cosmos DB pour MongoDB.
- Explorez les [exemples](nodejs-console-app.md) MongoDB avec l’API Azure Cosmos DB pour MongoDB.
- Vous tentez d’effectuer une planification de la capacité pour une migration vers Azure Cosmos DB ? Vous pouvez utiliser les informations sur votre cluster de bases de données existant pour la planification de la capacité.
    - Si vous ne connaissez que le nombre de vCores et de serveurs présents dans votre cluster de bases de données existant, lisez [Estimation des unités de requête à l’aide de vCores ou de processeurs virtuels](../convert-vcore-to-request-unit.md) 
    - Si vous connaissez les taux de requêtes typiques de votre charge de travail de base de données actuelle, lisez [Estimation des unités de requête à l’aide du planificateur de capacité Azure Cosmos DB](estimate-ru-capacity-planner.md)
