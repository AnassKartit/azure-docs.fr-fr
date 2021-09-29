---
title: Rechercher une API pour une application personnalisée | Azure
description: Comment configurer les autorisations dont vous avez besoin pour accéder à une API particulière dans l’application Azure AD que vous avez développée
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 09/27/2021
ms.author: ryanwi
ROBOTS: NOINDEX
ms.openlocfilehash: 2d5c7f6e603b79b87297b0bbadb3d210ceaf0484
ms.sourcegitcommit: df2a8281cfdec8e042959339ebe314a0714cdd5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2021
ms.locfileid: "129154657"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Comment rechercher une API spécifique requise pour une application personnalisée

L’accès aux API suppose de configurer les étendues d’accès et les rôles. Si vous souhaitez exposer aux applications clientes les API web de vos applications de ressources, configurez les étendues d’accès et les rôles de l’API. Si vous voulez qu’une application cliente puisse accéder à une API web, configurez les autorisations d’accès à l’API lors de l’inscription de l’application.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Configuration d’une application de ressource pour exposer les API web

Lorsque vous exposez vos API web, l’API s’affiche dans la liste **Sélectionner une API** lorsque vous ajoutez des autorisations à l’inscription d’une application. Pour ajouter des étendues d’accès, suivez les étapes décrites dans [Configurer une application pour exposer les API web](quickstart-configure-app-expose-web-apis.md).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Configuration d’une application cliente pour accéder aux API web

Lorsque vous ajoutez des autorisations à l’inscription de votre application, vous pouvez **ajouter un accès à l’API** à des API web exposées. Pour ajouter des étendues d’accès, suivez les étapes décrites dans [Configurer une application cliente pour accéder aux API web](quickstart-configure-app-access-web-apis.md).

## <a name="next-steps"></a>Étapes suivantes

- [Connaître le manifeste d’application Azure Active Directory](./reference-app-manifest.md)
