---
title: 'Démarrage rapide : Connexion et jeu avec l’instance Azure Web PubSub'
description: Démarrage rapide montrant comment jouer avec l’instance à partir d’Azure CLI.
author: vicancy
ms.author: lianwei
ms.service: azure-web-pubsub
ms.topic: quickstart
ms.date: 08/06/2021
ms.openlocfilehash: ac81c3422036abafc5e2d034e06e81e1f35d2e02
ms.sourcegitcommit: 8000045c09d3b091314b4a73db20e99ddc825d91
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/19/2021
ms.locfileid: "122445383"
---
# <a name="quickstart-connect-to-the-azure-web-pubsub-instance-from-cli"></a>Démarrage rapide : Connexion à l’instance Azure Web PubSub à partir de l’interface CLI

Ce guide de démarrage rapide montre comment se connecter à l’instance Azure Web PubSub et publier des messages sur les clients connectés avec [l’interface de ligne de commande Azure (Azure CLI)](/cli/azure).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

- La version 2.22.0 (ou une version ultérieure) de l’interface Azure CLI est requise pour ce démarrage rapide. Si vous utilisez Azure Cloud Shell, la version la plus récente est déjà installée.

## <a name="create-a-resource-group"></a>Créer un groupe de ressources

[!INCLUDE [Create a resource group](includes/cli-rg-creation.md)]

## <a name="create-a-web-pubsub-instance"></a>Création d’une instance Web PubSub

[!INCLUDE [Create a Web PubSub instance](includes/cli-awps-creation.md)]

## <a name="play-with-the-instance"></a>Jeu avec l’instance

[!INCLUDE [Try the Web PubSub instance](includes/cli-awps-client.md)]

## <a name="next-steps"></a>Étapes suivantes

Ce tutoriel donne une idée de base de la procédure à suivre pour se connecter au service Web PubSub et publier des messages sur les clients connectés.

Dans les applications réelles, vous pouvez utiliser des kits de développement logiciel (SDK, Software Development Kit) dans différents langages pour créer votre propre application. Nous fournissons également des extensions de fonction qui permettent de créer facilement des applications serverless.

[!INCLUDE [next step](includes/include-next-step.md)]
