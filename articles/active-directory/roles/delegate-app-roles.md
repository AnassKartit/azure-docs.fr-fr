---
title: Déléguer des autorisations d’administrateur de gestion des applications – Azure AD | Microsoft Docs
description: Accorder des autorisations pour la gestion de l’accès aux applications dans Azure Active Directory
services: active-directory
documentationcenter: ''
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: how-to
ms.date: 11/04/2020
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2cd749289b9a389b495481517a56b2652fb2026f
ms.sourcegitcommit: 8000045c09d3b091314b4a73db20e99ddc825d91
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/19/2021
ms.locfileid: "122527915"
---
# <a name="delegate-app-registration-permissions-in-azure-active-directory"></a>Déléguer des autorisations d’inscription d’application dans Azure Active Directory

Cet article explique comment utiliser des autorisations accordées par des rôles personnalisés dans Azure Active Directory (Azure AD) pour répondre à vos besoins en matière de gestion des applications. Dans Azure AD, vous pouvez déléguer les autorisations de création et de gestion d’applications en procédant ainsi :

- [Restriction des utilisateurs autorisés à créer des applications](#restrict-who-can-create-applications) et à gérer les applications qu’ils créent. Par défaut, dans Azure AD, tous les utilisateurs peuvent inscrire des applications et gérer tous les aspects des applications qu’ils créent. Cela peut être limité pour autoriser uniquement les personnes qui sont autorisées.
- [Attribution d’un ou plusieurs propriétaires à une application](#assign-application-owners). Il s’agit d’un moyen simple d’accorder à une personne la possibilité de gérer tous les aspects de la configuration Azure AD pour une application spécifique.
- [Attribution d’un rôle d’administrateur intégré](#assign-built-in-application-admin-roles) qui autorise l’accès à la gestion de la configuration dans Azure AD pour toutes les applications. Il s’agit de la méthode recommandée pour permettre aux experts informatiques de gérer des autorisations de configuration d’application étendues sans accorder l’accès pour gérer d’autres parties de Azure AD qui ne sont pas liées à la configuration de l’application.
- [Création d’un rôle personnalisé](#create-and-assign-a-custom-role-preview) qui définit des autorisations très spécifiques et l’affecte à une personne soit au niveau d’une application unique en tant que propriétaire limité ou au niveau de l’étendue du répertoire (toutes les applications) en tant qu’administrateur limité.

Il est important de considérer l’octroi de l’accès à l’aide de l’une des méthodes ci-dessus pour deux raisons. Tout d’abord, la délégation de la possibilité d’effectuer des tâches d’administration réduit la charge de l’administrateur général. Deuxièmement, l’utilisation d’autorisations limitées améliore votre position de sécurité et réduit le risque d’accès non autorisé. Pour obtenir des instructions sur la planification de la sécurité des rôles, consultez [Sécurisation de l’accès privilégié pour les déploiements hybrides et cloud dans Azure AD](security-planning.md).

## <a name="restrict-who-can-create-applications"></a>Restreindre qui peut créer des applications

Par défaut, dans Azure AD, tous les utilisateurs peuvent inscrire des applications et gérer tous les aspects des applications qu’ils créent. Tout le monde peut également donner son consentement aux applications qui accèdent aux données de l’entreprise en leur nom. Vous pouvez choisir d’accorder ces autorisations de manière sélective en définissant les commutateurs globaux sur « non » et en ajoutant les utilisateurs sélectionnés au rôle développeur d’applications.

### <a name="to-disable-the-default-ability-to-create-application-registrations-or-consent-to-applications"></a>Pour désactiver la possibilité par défaut de créer des inscriptions d’applications ou de donner leur consentement aux applications

1. Connectez-vous à votre organisation Azure AD avec un compte éligible au rôle d’administrateur d’entreprise dans votre organisation Azure AD.
1. Définissez un ou plusieurs des paramètres suivants :

    - Sur la page [Paramètres utilisateur de votre organisation](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings), définissez le paramètre **Les utilisateurs peuvent inscrire des applications** sur Non. Cela désactive la capacité par défaut pour les utilisateurs de créer des inscriptions d’applications.
    - Sur les [paramètres utilisateur pour les applications d’entreprise](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/), définissez **Les utilisateurs peuvent donner leur consentement aux applications qui accèdent à des données d’entreprise en leur nom** sur Non. Cela désactive la possibilité par défaut pour les utilisateurs de donner leur consentement aux applications qui accèdent aux données de l’entreprise en leur nom.

### <a name="grant-individual-permissions-to-create-and-consent-to-applications-when-the-default-ability-is-disabled"></a>Accorder des autorisations individuelles pour créer des applications et donner leur consentement lorsque la capacité par défaut est désactivée

Attribuez le rôle Développeur d’applications pour accorder la possibilité de créer des inscriptions d'applications lorsque le paramètre **Les utilisateurs peuvent inscrire des applications** est défini sur Non. Ce rôle octroie aussi l’autorisation de donner son consentement en son propre nom lorsque le paramètre **Les utilisateurs peuvent autoriser les applications à accéder aux données de l’entreprise en leur nom** est défini sur Non.

## <a name="assign-application-owners"></a>Assigner les propriétaires de l’application

L'assignation de propriétaires est un moyen simple d'octroyer la possibilité de gérer tous les aspects de la configuration Azure AD pour un enregistrement d'application spécifique ou une application d'entreprise. Pour plus d’informations, consultez [Affecter des propriétaires d’applications d’entreprise](../manage-apps/assign-app-owners.md).

## <a name="assign-built-in-application-admin-roles"></a>Affecter des rôles d’administrateur d’application intégrés

Azure AD a un ensemble de rôles d’administrateur intégrés pour accorder l’accès à la gestion de la configuration dans Azure AD pour toutes les applications. Ces rôles sont la méthode recommandée pour accorder aux experts informatiques l’accès pour gérer des autorisations de configuration d’application étendues sans accorder l’accès pour gérer d’autres parties de Azure AD qui ne sont pas liées à la configuration de l’application.

- Administrateur d’application : Les utilisateurs dans ce rôle peuvent créer et gérer tous les aspects des applications d’entreprise, des inscriptions d’application et des paramètres de proxy d’application. Ce rôle permet également de donner son consentement à des autorisations déléguées et des autorisations d’application, à l’exception de Microsoft Graph. Les utilisateurs affectés à ce rôle ne sont pas ajoutés en tant que propriétaires lorsque des inscriptions d’applications ou des applications d’entreprise sont créées.
- Administrateur d’application cloud Les utilisateurs dans ce rôle ont les mêmes autorisations que celles du rôle Administrateur d’application, sans la possibilité de gérer le proxy d’application. Les utilisateurs affectés à ce rôle ne sont pas ajoutés en tant que propriétaires lorsque des inscriptions d’applications ou des applications d’entreprise sont créées.

Pour plus d’informations et pour afficher la description de ces rôles, consultez [Rôles intégrés Azure AD](permissions-reference.md).

Suivez les instructions fournies dans le guide pratique [Attribuer des rôles aux utilisateurs avec Azure Active Directory](../fundamentals/active-directory-users-assign-role-azure-portal.md) pour attribuer des rôles administrateur d’application ou administrateur d’application Cloud.

> [!IMPORTANT]
> Les administrateurs d’applications et les administrateurs d’applications Cloud peuvent ajouter des informations d’identification à une application et utiliser ces informations d’identification pour emprunter l’identité de l’application. L'application peut disposer d'autorisations correspondant à une élévation de privilèges par rapport aux autorisations du rôle admin. Un administrateur de ce rôle peut potentiellement créer ou mettre à jour des utilisateurs ou d’autres objets tout en empruntant l’identité de l’application, en fonction des autorisations de l’application.
> Aucun rôle n’accorde la possibilité de gérer les paramètres d’accès conditionnel.

## <a name="create-and-assign-a-custom-role-preview"></a>Créer et affecter un rôle personnalisé (préversion)

La création de rôles personnalisés et l’attribution de rôles personnalisés sont des étapes distinctes :

- [Créez une *définition de rôle*](custom-create.md) personnalisée et [ajoutez-lui des autorisations à partir d’une liste prédéfinie](custom-available-permissions.md). Il s’agit des mêmes autorisations que celles utilisées dans les rôles intégrés.
- [Créez une *attribution de rôle*](custom-assign-powershell.md) pour attribuer le rôle personnalisé.

Cette séparation vous permet de créer une définition de rôle unique, puis de l'attribuer plusieurs fois à différentes *portées*. Un rôle personnalisé peut être attribué au niveau de l’organisation, ou à l'étendue s'il s'agit d'un seul objet Azure AD. Un exemple d’étendue d’objet est une inscription d’application unique. À l'aide d'étendues différentes, la même définition de rôle peut être attribuée à Sally pour tous les enregistrements d'application de l'organisation, puis à Naveen uniquement pour l'enregistrement de l'application Contoso Expense Reports.

Conseils lors de la création et de l’utilisation de rôles personnalisés pour la délégation de la gestion d’applications :
- Les rôles personnalisés accordent uniquement l’accès dans les panneaux d’inscription d’application les plus récents du portail Azure AD. Ils n’accordent pas d’accès dans les panneaux Inscriptions d’applications héritées.
- Les rôles personnalisés n’accordent pas l’accès au portail Azure lorsque le paramètre utilisateur « [Restreindre l’accès au portail d’administration Azure AD](../fundamentals/users-default-permissions.md) » est défini sur Oui.
- Les inscriptions d’applications auxquels l’utilisateur a accès en utilisant les attributions de rôles s’affichent uniquement dans l’onglet « Toutes les applications » de la page Inscription d’application. Elles ne s’affichent pas dans l’onglet « Applications détenues ».

Pour plus d’informations sur les principes de base des rôles personnalisés, consultez [Vue d’ensemble des rôles personnalisés](custom-overview.md), ainsi que la façon de [créer un rôle personnalisé](custom-create.md) et d’[attribuer un rôle](custom-assign-powershell.md).

## <a name="next-steps"></a>Étapes suivantes

- [Autorisations et sous-types d’inscription d’application](custom-available-permissions.md)
- [Rôles intégrés Azure AD](permissions-reference.md)
