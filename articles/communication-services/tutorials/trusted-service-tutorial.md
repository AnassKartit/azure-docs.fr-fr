---
title: Créer un service d’accès des utilisateurs approuvés à l’aide d’Azure Functions dans Azure Communication Services
titleSuffix: An Azure Communication Services tutorial
description: Découvrez comment créer un service d’accès des utilisateurs approuvés pour Communication Services avec Azure Functions.
author: ddematheu2
manager: chpalm
services: azure-communication-services
ms.author: dademath
ms.date: 06/30/2021
ms.topic: tutorial
ms.service: azure-communication-services
ms.openlocfilehash: a2598d56d74c159c2db00aaa26b9bde04a7d2bcb
ms.sourcegitcommit: 2eac9bd319fb8b3a1080518c73ee337123286fa2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2021
ms.locfileid: "123258957"
---
# <a name="build-a-trusted-user-access-service-using-azure-functions"></a>Créer un service d’accès des utilisateurs approuvés à l’aide d’Azure Functions

Cet article explique comment utiliser Azure Functions pour créer un service d’accès des utilisateurs approuvés.

> [!IMPORTANT]
> Le point de terminaison créé à la fin de ce tutoriel n’est pas sécurisé. Veillez à consulter les détails de sécurité disponibles dans l’article [Sécurité d’Azure Functions](../../azure-functions/security-concepts.md). Vous devez ajouter un élément de sécurité au point de terminaison pour vous empêcher des acteurs malveillants d’approvisionner des jetons.

[!INCLUDE [Trusted Service JavaScript](./includes/trusted-service-js.md)]

## <a name="securing-azure-function"></a>Sécurisation d’Azure Functions

Dans le cadre de la configuration d’un service approuvé permettant d’approvisionner des jetons d’accès pour les utilisateurs, nous devons prendre en compte la sécurité de ce point de terminaison afin de veiller à ce qu’aucun acteur malveillant ne puisse créer des jetons de manière aléatoire pour votre service. Azure Functions fournit des fonctionnalités de sécurité intégrées que vous pouvez utiliser pour sécuriser le point de terminaison à l’aide de différents types de stratégies d’authentification. En savoir plus sur la [Sécurité d’Azure Functions](../../azure-functions/security-concepts.md)

## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous voulez nettoyer et supprimer un abonnement Communication Services, vous pouvez supprimer la ressource ou le groupe de ressources. La suppression du groupe de ressources efface également les autres ressources qui y sont associées. Apprenez-en davantage sur le [nettoyage des ressources Azure Communication Service](../quickstarts/create-communication-resource.md#clean-up-resources) et le [nettoyage des ressources Azure Functions](../../azure-functions/create-first-function-vs-code-csharp.md#clean-up-resources).

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [En savoir plus sur la sécurité d’Azure Functions](../../azure-functions/security-concepts.md)

> [!div class="nextstepaction"]
> [Ajouter l’appel vocal à votre application](../quickstarts/voice-video-calling/getting-started-with-calling.md)

Vous souhaiterez peut-être également :

- [Ajouter la conversation à votre application](../quickstarts/chat/get-started.md)
- [Créer des jetons d’accès utilisateur](../quickstarts/access-tokens.md)
- [En savoir plus sur l’architecture client et serveur](../concepts/client-and-server-architecture.md)
- [En savoir plus sur l’authentification](../concepts/authentication.md)