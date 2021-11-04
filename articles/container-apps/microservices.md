---
title: Microservices avec la préversion d’Azure Container Apps
description: Créez un microservice dans Azure Container Apps.
services: app-service
author: craigshoemaker
ms.service: app-service
ms.topic: conceptual
ms.date: 11/02/2021
ms.author: cshoe
ms.custom: ignite-fall-2021
ms.openlocfilehash: 99903809a7809db0164025df1de80f13262c8c2c
ms.sourcegitcommit: 96deccc7988fca3218378a92b3ab685a5123fb73
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2021
ms.locfileid: "131577039"
---
# <a name="microservices-with-azure-containers-apps-preview"></a>Microservices avec la préversion d’Azure Container Apps

Les [architectures de microservices](https://azure.microsoft.com/solutions/microservice-applications/#overview) vous permettent de développer, de mettre à niveau, de versionner et de mettre à l’échelle de manière indépendante des zones principales de fonctionnalités dans un système global. Azure Container Apps fournit la base pour le déploiement de microservices avec notamment :

- [Mise à l’échelle](scale-app.md), [versioning](application-lifecycle-management.md) et [mises à niveau](application-lifecycle-management.md) indépendants.
- [Détection du service](connect-apps.md)
- [Intégration Dapr](microservices-dapr.md) native

:::image type="content" source="media/microservices/azure-container-services-microservices.png" alt-text="Les applications de conteneur sont déployées en tant que microservices.":::

Un [environnement](environment.md) Container Apps fournit une limite de sécurité autour d’un groupe d’applications de conteneur. Une application de conteneur unique représente généralement un microservice, qui est composé de pods constitués d’un ou plusieurs [conteneurs](containers.md).

## <a name="dapr-integration"></a>Intégration de Dapr

Lors de l’implémentation d’un système composé de microservices, les appels de fonction sont répartis sur le réseau. Pour prendre en charge la nature distribuée des microservices, vous devez prendre en compte les échecs, les nouvelles tentatives et les délais d’attente. Bien que Container Apps offre les modules pour l’exécution de microservices, l’utilisation de [Dapr](https://docs.dapr.io/concepts/overview/) fournit un modèle de programmation de microservices encore plus riche. Dapr comprend des fonctionnalités telles que l’observabilité, Pub/Sub et l’appel de service à service avec TLS mutuel, nouvelles tentatives, et bien plus encore.

Pour plus d’informations sur l’utilisation de Dapr, consultez [Générer des microservices avec Dapr](microservices-dapr.md).

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Mise à l'échelle](scale-app.md)
