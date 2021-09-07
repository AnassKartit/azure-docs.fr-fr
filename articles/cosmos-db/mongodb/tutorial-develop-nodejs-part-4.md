---
title: Créer une application Angular avec l’API d’Azure Cosmos DB pour MongoDB (Partie 1)
description: Il s’agit de la partie 4 de cette série de didacticiels sur la création d’une application MongoDB avec Angular et Node sur Azure Cosmos DB à l’aide des mêmes API que celles utilisées pour MongoDB
author: johnpapa
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 08/26/2021
ms.author: jopapa
ms.custom: seodec18, devx-track-js, devx-track-azurecli
ms.reviewer: sngun
ms.openlocfilehash: 41a025607979fcca68b0bfc61c8dd62b6ba4a0f7
ms.sourcegitcommit: 03f0db2e8d91219cf88852c1e500ae86552d8249
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2021
ms.locfileid: "123035328"
---
# <a name="create-an-angular-app-with-azure-cosmos-dbs-api-for-mongodb---create-a-cosmos-account"></a>Créer une application Angular avec l’API Azure Cosmos DB pour MongoDB - Créer un compte Cosmos
[!INCLUDE[appliesto-mongodb-api](../includes/appliesto-mongodb-api.md)]

Ce tutoriel en plusieurs parties montre comment créer une application écrite en Node.js avec Express et Angular, puis comment la connecter à votre [compte Cosmos configuré avec l’API de Cosmos DB pour MongoDB](mongodb-introduction.md).

La partie 4 de ce didacticiel est basée sur la [partie 3](tutorial-develop-nodejs-part-3.md) et aborde les tâches suivantes :

> [!div class="checklist"]
> * Créer un groupe de ressources Azure à l’aide d’Azure CLI
> * Créer un compte Cosmos à l’aide d’Azure CLI

## <a name="video-walkthrough"></a>Vidéo de procédure pas à pas

> [!VIDEO https://www.youtube.com/embed/hfUM-AbOh94]

## <a name="prerequisites"></a>Prérequis

Avant de commencer cette partie du didacticiel, assurez-vous d’avoir effectué les étapes de la [partie 3](tutorial-develop-nodejs-part-3.md) du didacticiel. 

Dans cette section du tutoriel, vous pouvez utiliser Azure Cloud Shell (dans votre navigateur Internet) ou [Azure CLI](/cli/azure/install-azure-cli) installé localement.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Log in to Azure](../../../includes/login-to-azure.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group.md)]

> [!TIP]
> Ce didacticiel vous guide à travers les étapes pour générer l’application pas à pas. Si vous souhaitez télécharger le projet terminé, vous pouvez obtenir l’application complète à partir du [référentiel cosmosdb-angular](https://github.com/Azure-Samples/angular-cosmosdb) sur GitHub.

## <a name="create-an-azure-cosmos-db-account"></a>Création d’un compte Azure Cosmos DB

Créez un compte Azure Cosmos DB en exécutant la commande [`az cosmosdb create`](/cli/azure/cosmosdb#az_cosmosdb_create).

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

* Dans `<cosmosdb-name>`, utilisez le nom de votre compte Azure Cosmos DB. Il doit être unique parmi tous les noms de compte Azure Cosmos DB présents dans Azure.
* Le paramètre `--kind MongoDB` active les connexions MongoDB pour Azure Cosmos DB.

L’exécution de la commande peut durer quelques minutes. Une fois la commande terminée, la fenêtre du terminal affiche les informations de la nouvelle base de données. 

Après avoir créé le compte Azure Cosmos DB :
1. Ouvrez une nouvelle fenêtre dans votre navigateur et accédez à [https://portal.azure.com](https://portal.azure.com)
1. Cliquez sur le logo Azure Cosmos DB :::image type="icon" source="./media/tutorial-develop-nodejs-part-4/azure-cosmos-db-icon.png"::: dans la barre de gauche pour voir toutes les bases de données Azure Cosmos DB que vous avez.
1. Cliquez sur le compte Azure Cosmos DB que vous venez de créer, sélectionnez l’onglet **Vue d’ensemble** et faites défiler vers le bas pour voir où se trouve la base de données. 

    :::image type="content" source="./media/tutorial-develop-nodejs-part-4/azure-cosmos-db-angular-portal.png" alt-text="La capture d’écran montre la vue d’ensemble d’un compte Azure Cosmos DB.":::

4. Faites défiler vers le bas dans la barre de navigation située à gauche, puis cliquez sur l’onglet **Répliquer les données mondialement** pour afficher un mappage indiquant les différentes zones dans lesquelles vous pouvez répliquer des données. Par exemple, vous pouvez cliquer sur Australie Sud-Est ou Australie Est pour répliquer vos données en Australie. Pour en savoir plus sur la réplication mondiale, consultez [Distribution mondiale des données avec Azure Cosmos DB](../distribute-data-globally.md). Pour le moment, conservons cette instance et lorsque nous voudrons répliquer, nous saurons comment procéder.

    :::image type="content" source="./media/tutorial-develop-nodejs-part-4/azure-cosmos-db-replicate-portal.png" alt-text="La capture d’écran montre un compte Azure Cosmos DB avec l’élément Répliquer les données globalement sélectionné.":::

## <a name="next-steps"></a>Étapes suivantes

Dans cette partie du didacticiel, vous avez :

> [!div class="checklist"]
> * Créé un groupe de ressources Azure à l’aide d’Azure CLI
> * Créé un compte Azure Cosmos DB à l’aide d’Azure CLI

Vous pouvez maintenant passer à la partie suivante du didacticiel afin de connecter Azure Cosmos DB et votre application à l’aide de Mongoose.

> [!div class="nextstepaction"]
> [Utiliser Mongoose pour se connecter à Azure Cosmos DB](tutorial-develop-nodejs-part-5.md)

Vous tentez d’effectuer une planification de la capacité pour une migration vers Azure Cosmos DB ? Vous pouvez utiliser les informations sur votre cluster de bases de données existant pour la planification de la capacité.
* Si vous ne connaissez que le nombre de vCores et de serveurs présents dans votre cluster de bases de données existantes, lisez l’article sur l’[estimation des unités de requête à l’aide de vCores ou de processeurs virtuels](../convert-vcore-to-request-unit.md) 
* Si vous connaissez les taux de requêtes typiques de votre charge de travail de base de données actuelle, lisez la section concernant l’[estimation des unités de requête à l’aide du planificateur de capacité Azure Cosmos DB](estimate-ru-capacity-planner.md)