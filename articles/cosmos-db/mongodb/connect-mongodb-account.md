---
title: Connecter une application MongoDB à Azure Cosmos DB
description: Découvrir comment connecter une application MongoDB à Azure Cosmos DB à l’aide d’une chaîne de connexion à partir du Portail Azure
author: gahl-levy
ms.author: gahllevy
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 08/26/2021
ms.reviewer: sngun
adobe-target: true
adobe-target-activity: DocsExp-A/B-384740-MongoDB-2.8.2021
adobe-target-experience: Experience B
adobe-target-content: ./connect-mongodb-account-experimental
ms.openlocfilehash: 3c148d2bedd8e9b01874dce379e3463509499c84
ms.sourcegitcommit: 03f0db2e8d91219cf88852c1e500ae86552d8249
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2021
ms.locfileid: "123029214"
---
# <a name="connect-a-mongodb-application-to-azure-cosmos-db"></a>Connecter une application MongoDB à Azure Cosmos DB
[!INCLUDE[appliesto-mongodb-api](../includes/appliesto-mongodb-api.md)]

Découvrez comment connecter votre application MongoDB à une instance Azure Cosmos DB à l’aide d’une chaîne de connexion MongoDB. Vous pouvez ensuite utiliser une base de données Azure Cosmos en tant que magasin de données pour votre application MongoDB.

Ce didacticiel fournit deux façons de récupérer les informations de la chaîne de connexion :

- [Méthode du guide de démarrage rapide](#get-the-mongodb-connection-string-by-using-the-quick-start), à utiliser avec les pilotes .NET, Node.js, MongoDB Shell, Java et Python
- [Méthode de la chaîne de connexion personnalisée](#get-the-mongodb-connection-string-to-customize), à utiliser avec d’autres pilotes.

## <a name="prerequisites"></a>Prérequis

- Un compte Azure. Si vous ne possédez pas de compte Azure, vous pouvez créer un [compte Azure gratuit](https://azure.microsoft.com/free/) dès maintenant.
- Un compte Cosmos. Pour obtenir des instructions, consultez [Générer une application web en utilisant l’API pour MongoDB d’Azure Cosmos DB et le kit SDK .NET](create-mongodb-dotnet.md).

## <a name="get-the-mongodb-connection-string-by-using-the-quick-start"></a>Obtenir la chaîne de connexion MongoDB à l’aide du démarrage rapide

1. Dans un navigateur Internet, connectez-vous au [portail Azure](https://portal.azure.com).
2. Dans le panneau **Azure Cosmos DB**, sélectionnez l’API.
3. Dans le volet gauche du panneau Compte, cliquez sur **Démarrage rapide**.
4. Choisissez votre plateforme ( **.NET**, **Node.js**, **MongoDB Shell**, **Java** ou **Python**). Si votre pilote ou outil n’est pas répertorié, ne vous inquiétez pas, nous développons en permanence de nouveaux extraits de code de connexion. Entrez ci-dessous un commentaire sur ce que vous voudriez voir. Pour savoir comment créer votre propre connexion, lisez [Obtenir les informations de chaîne de connexion du compte](#get-the-mongodb-connection-string-to-customize).
5. Copiez et collez l’extrait de code dans votre application MongoDB.

    :::image type="content" source="./media/connect-mongodb-account/quickstart-blade.png" alt-text="Panneau Démarrage rapide":::

## <a name="get-the-mongodb-connection-string-to-customize"></a>Obtenir la chaîne de connexion MongoDB à personnaliser

1. Dans un navigateur Internet, connectez-vous au [portail Azure](https://portal.azure.com).
2. Dans le panneau **Azure Cosmos DB**, sélectionnez l’API.
3. Dans le volet gauche du panneau Compte, cliquez sur **Chaîne de connexion**.
4. Le panneau **Chaîne de connexion** s’ouvre. Il affiche toutes les informations nécessaires pour la connexion au compte à l’aide d’un pilote pour MongoDB, dont une chaîne de connexion prédéfinie.

   :::image type="content" source="./media/connect-mongodb-account/connection-string-blade.png" alt-text="Panneau Chaîne de connexion" lightbox= "./media/connect-mongodb-account/connection-string-blade.png" :::

## <a name="connection-string-requirements"></a>Exigences relatives à la chaîne de connexion

> [!Important]
> Azure Cosmos DB obéit à des normes et à des exigences strictes en matière de sécurité. Les comptes Azure Cosmos DB nécessitent une authentification et une communication sécurisée via *TLS*.

Azure Cosmos DB prend en charge le format d’URI de la chaîne de connexion MongoDB standard sous certaines conditions spécifiques : Les comptes Azure Cosmos DB nécessitent une authentification et une communication sécurisée via TLS. Le format de chaîne de connexion est donc :

`mongodb://username:password@host:port/[database]?ssl=true`

Les valeurs de cette chaîne sont disponibles dans le panneau **Chaîne de connexion** illustré précédemment :

* Nom d’utilisateur (obligatoire) : Nom du compte Cosmos.
* Mot de passe (obligatoire) : Mot de passe du compte Cosmos.
* Hôte (obligatoire) : Nom de domaine complet du compte Cosmos.
* Port (obligatoire) : 10255.
* Base de données (facultatif) : Base de données utilisée par la connexion. Si aucune base de données n’est fournie, la base de données par défaut est « test ».
* ssl = true (obligatoire)

Par exemple, observez le compte figurant dans le panneau **Chaîne de connexion**. Exemple de chaîne de connexion valide :

`mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@contoso123.documents.azure.com:10255/mydatabase?ssl=true`

## <a name="driver-requirements"></a>Configuration requise des pilotes

Tous les pilotes prenant en charge le protocole filaire version 3.4 ou ultérieure prennent en charge l’API Azure Cosmos DB pour MongoDB.

Plus précisément, les pilotes clients doivent prendre en charge l’extension TLS SNI (identification du nom de service) et/ou l’option de chaîne de connexion appName. Si le paramètre `appName` est fourni, il doit être inclus exactement comme il a été trouvé dans la valeur de la chaîne de connexion dans le portail Azure.

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment [utiliser Studio 3T](connect-using-mongochef.md) avec l’API Azure Cosmos DB pour MongoDB.
- Découvrez comment [utiliser Robo 3T](connect-using-robomongo.md) avec l’API Azure Cosmos DB pour MongoDB.
- Explorez les [exemples](nodejs-console-app.md) MongoDB avec l’API Azure Cosmos DB pour MongoDB.
- Vous tentez d’effectuer une planification de la capacité pour une migration vers Azure Cosmos DB ? Vous pouvez utiliser les informations sur votre cluster de bases de données existantes pour la planification de la capacité.
    - Si vous ne connaissez que le nombre de vCore et de serveurs présents dans votre cluster de bases de données existant, lisez l’article sur l’[estimation des unités de requête à l’aide de vCore ou de processeurs virtuels](../convert-vcore-to-request-unit.md) 
    - Si vous connaissez les taux de requêtes types de votre charge de travail de base de données actuelle, lisez la section concernant l’[estimation des unités de requête à l’aide du planificateur de capacité Azure Cosmos DB](estimate-ru-capacity-planner.md)
