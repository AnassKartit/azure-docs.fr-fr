---
title: API Microsoft Graph
description: L’API Microsoft Graph est une API web RESTful qui vous permet d’accéder aux ressources des services cloud Microsoft.
author: davidmu1
services: active-directory
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 02/13/2020
ms.author: davidmu
ms.custom: aaddev
ms.openlocfilehash: 2e689e620a5aeb7c5028f1a1b30dd6def8e447ab
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2021
ms.locfileid: "107529997"
---
# <a name="microsoft-graph-api"></a>API Microsoft Graph

L’API Microsoft Graph est une API web RESTful qui vous permet d’accéder aux ressources des services cloud Microsoft. Une fois que vous avez inscrit votre application et obtenu les jetons d’authentification d’un utilisateur ou d’un service, vous pouvez adresser des demandes à l’API Microsoft Graph. Pour plus d’informations, consultez [Présentation de Microsoft Graph](/graph/overview).

Microsoft Graph expose les API REST et les bibliothèques clientes pour accéder aux données des services Microsoft 365 suivants :
- Services Microsoft 365 : Delve, Excel, Microsoft Bookings, Microsoft Teams, OneDrive, OneNote, Outlook/Exchange, Planner et SharePoint
- Services Enterprise Mobility + Security : Advanced Threat Analytics, Advanced Threat Protection, Azure Active Directory, Identity Manager et Intune
- Services Windows 10 : activités, appareils, notifications
- Dynamics 365 Business Central

## <a name="versions"></a>Versions

Microsoft Graph prend actuellement en charge deux versions : v1.0 et bêta. La version v1.0 comprend les API généralement disponibles. Utilisez la version v1.0 pour toutes les applications de production. La version bêta comprend les API actuellement en préversion. Comme nos API bêta sont susceptibles d’introduire des changements cassants, nous vous recommandons d’utiliser la version bêta uniquement pour tester des applications en phase de développement. N’utilisez pas les API bêta dans vos applications de production. Pour plus d’informations, consultez [Gestion de versions, support et stratégies de changements cassants pour Microsoft Graph](/graph/versioning-and-support).

Pour commencer à utiliser les API bêta, consultez [Référence de point de terminaison bêta Microsoft Graph](/graph/api/overview?view=graph-rest-beta&preserve-view=true)

Pour commencer à utiliser les API v1.0, consultez [Référence version 1.0 de l’API REST Microsoft Graph](/graph/api/overview?view=graph-rest-1.0&preserve-view=true)

## <a name="get-started"></a>Bien démarrer

Pour lire ou écrire dans une ressource telle qu’un utilisateur ou un e-mail, vous devez créer une requête qui ressemble à ceci :

`{HTTP method} https://graph.microsoft.com/{version}/{resource}?{query-parameters}`

Pour plus d’informations sur les éléments de la requête à créer, consultez [Utiliser l’API Microsoft Graph](/graph/use-the-api)

Des exemples de démarrage rapide sont disponibles pour vous montrer comment accéder à la puissance de l’API Microsoft Graph. Les exemples disponibles accèdent à deux services via une seule authentification : compte Microsoft et Outlook. Chaque démarrage rapide accède aux informations des profils des utilisateurs du compte Microsoft et affiche les événements de leur calendrier.
Les démarrages rapides impliquent quatre étapes :
- Sélectionner votre plateforme
- Obtenir votre ID d’application (ID de client)
- Générer l’exemple
- Se connecter et afficher les événements sur votre calendrier

Une fois le démarrage rapide terminé, vous disposez d’une application prête à être exécutée. Pour plus d’informations, consultez le [FAQ du démarrage rapide de Microsoft Graph](/graph/quick-start-faq). Pour commencer à utiliser les exemples, consultez [Démarrage rapide de Microsoft Graph](https://developer.microsoft.com/graph/quick-start).

## <a name="tools"></a>Outils

L’afficheur Microsoft Graph est un outil web que vous pouvez utiliser pour générer et tester des demandes à l’aide des API Microsoft Graph. Vous pouvez accéder à l’afficheur Microsoft Graph sur : `https://developer.microsoft.com/graph/graph-explorer`.

Postman est un autre outil que vous pouvez utiliser pour générer et tester des demandes à l’aide des API Microsoft Graph. Vous pouvez télécharger Postman à l’adresse : `https://www.getpostman.com/`. Pour interagir avec Microsoft Graph dans Postman, utilisez la collection Microsoft Graph dans Postman. Pour plus d’informations, consultez [Utiliser Postman avec l’API Microsoft Graph](/graph/use-postman).
