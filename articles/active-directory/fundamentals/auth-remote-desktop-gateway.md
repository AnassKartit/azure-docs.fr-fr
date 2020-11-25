---
title: Services de passerelle Bureau à distance avec Azure Active Directory
description: Guide architectural sur l’obtention de services de passerelle Bureau à distance avec Azure Active Directory.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 57466cccb71444d5711a9221c324a107757c5e82
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94576793"
---
# <a name="remote-desktop-gateway-services"></a>Services de passerelle Bureau à distance

Un déploiement de services Bureau à distance (RDS) standard inclut divers [services du rôle Bureau à distance](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) s’exécutant sur Windows Server. Le déploiement RDS utilisant la fonctionnalité Proxy d’application d’Azure Active Directory (Azure AD) dispose d’une connexion sortante permanente depuis le serveur exécutant le service de connexion. Les autres déploiements comportent des connexions entrantes ouvertes via un équilibreur de charge. Ce modèle d’authentification vous permet de proposer davantage de types d’applications en publiant des applications locales via les services Bureau à distance. Cela réduit également la surface d’attaque de leur déploiement en utilisant Proxy d’application d’Azure AD.

## <a name="use-when"></a>Cas d'utilisation

Vous devez fournir un accès à distance et protéger votre déploiement RDS par une préauthentification.

![Diagramme architectural](./media/authentication-patterns/rdp-auth.png)

## <a name="components-of-system"></a>Composants du système

* **Utilisateur** : Accède à RDS desservi par Proxy d’application.

* **Navigateur web**  : Composant avec lequel l’utilisateur interagit pour accéder à l’URL externe de l’application.

* **Azure AD** : Authentifie l’utilisateur. 

* **Service Proxy d’application** : Agit en tant que proxy inverse pour transférer la requête de l’utilisateur à RDS. Proxy d’application peut également appliquer toutes les stratégies d’accès conditionnel. 

* **Services Bureau à distance** : Agit en tant que plateforme pour les applications virtualisées individuelles, offrant un accès sécurisé aux applications mobiles et de Bureau à distance et offrant aux utilisateurs finaux la possibilité d’exécuter leurs applications et leurs Bureaux à partir du cloud. 

## <a name="implement-remote-desktop-gateway-services-with-azure-ad"></a>Implémenter des services de passerelle Bureau à distance avec Azure AD

* [Publier le Bureau à distance avec Proxy d’application d’Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-remote-desktop-services) 

* [Ajouter une application locale pour un accès à distance via le service Proxy d’application d’Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)

 
