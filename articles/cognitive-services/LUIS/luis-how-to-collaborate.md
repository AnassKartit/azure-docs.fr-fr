---
title: Collaborer avec d’autres personnes - LUIS
titleSuffix: Azure Cognitive Services
description: Un propriétaire d’application peut ajouter des contributeurs à la ressource de création. Ces contributeurs peuvent modifier le modèle, l’entraîner et publier l’application.
services: cognitive-services
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 11/19/2019
ms.openlocfilehash: 60b100e1100b8a4925a967f8b7265f1612e5beb7
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95018903"
---
# <a name="add-contributors-to-your-app"></a>Ajouter des contributeurs à votre application

Un propriétaire d’application peut ajouter des contributeurs à des applications. Ces collaborateurs peuvent modifier le modèle, entraîner et publier l’application.

[!INCLUDE [Manage collaborators and contributors based on migrated or not-migrated apps](./includes/manage-contributor-collaborator-migration.md)]

## <a name="add-contributor-to-azure-authoring-resource"></a>Ajouter un contributeur à une ressource de création Azure

La procédure suivante est destinée à tous les utilisateurs qui ont **migré** pour utiliser la ressource de création Azure.

Vous avez migré si votre expérience de création LUIS est liée à une ressource de création dans la page **Gérer -> Ressources Azure** du portail LUIS.

1. Dans le portail Azure, recherchez la ressource de création Language Understanding (LUIS). Son type est `LUIS.Authoring`.
1. Dans la page **Contrôle d’accès (IAM)** de cette ressource, sélectionnez **+Ajouter**, puis **Ajouter une attribution de rôle**.

    ![Dans le portail Azure, ajoutez une attribution de rôle à la ressource de création.](./media/luis-how-to-collaborate/authoring-resource-access-control-add-role.png)

1. Dans la fenêtre **Ajouter une attribution de rôle**, sélectionnez le **Rôle** du contributeur. Pour l’option **Attribuer l’accès à**, sélectionnez un **utilisateur, un groupe ou un principal de service Azure AD**. Pour l’option **Sélectionner**, entrez l’adresse e-mail de l’utilisateur. Si l’utilisateur a plus de 1 adresse e-mail pour le même domaine, veillez à entrer le compte de courrier _principal_.

    ![Ajouter l’e-mail de l’utilisateur au rôle contributeur pour Azure AD](./media/luis-how-to-collaborate/add-role-assignment-for-contributor.png)

    Lorsque l’e-mail de l’utilisateur est trouvé, sélectionnez le compte, puis **Enregistrer**.

    Si vous rencontrez des problèmes avec cette attribution de rôle, voir [Attribution de rôle Azure](../../role-based-access-control/role-assignments-portal.md) et [Résolution des problèmes liés au contrôle d’accès Azure](../../role-based-access-control/troubleshooting.md#problems-with-azure-role-assignments).

## <a name="add-collaborator-to-luis-app"></a>Ajouter un collaborateur à une application LUIS

La procédure suivante est destinée à tous les utilisateurs qui **n’ont pas migré** pour utiliser la ressource de création Azure.

Vous n’avez pas opéré de migration si votre expérience de création LUIS n’est pas liée à une ressource de création sur la page **Gérer -> Ressources Azure** du portail LUIS.

Une application possède un seul créateur, son propriétaire, mais peut avoir de nombreux collaborateurs. Pour autoriser des collaborateurs à modifier votre application LUIS, vous devez ajouter l’e-mail qu’ils utilisent pour accéder au portail LUIS à la liste des collaborateurs. Une fois qu’ils sont ajoutés, l’application apparaît dans leur portail LUIS.

1. Sélectionnez **Gérer** dans le menu supérieur droit, puis **Collaborateurs** dans le menu gauche.

1. Sélectionnez **Ajouter un collaborateur** dans la barre d’outils.

1. Entrez l’adresse e-mail utilisée par le collaborateur pour se connecter au portail LUIS.

    ![Ajouter l’adresse e-mail du collaborateur](./media/luis-how-to-collaborate/add-collaborator-pop-up.png)


### <a name="users-with-multiple-emails"></a>Utilisateurs avec plusieurs adresses e-mail

Si vous ajoutez un contributeur/collaborateur à une application LUIS, vous spécifiez son adresse e-mail exacte. Si Azure Active Directory (Azure AD) permet à un utilisateur d’avoir plusieurs comptes de courrier interchangeables, LUIS exige que l’utilisateur se connecte avec l’adresse e-mail spécifiée lors de son ajout en tant que contributeur/collaborateur.

<a name="owner-and-collaborators"></a>

### <a name="azure-active-directory-resources"></a>Ressources Azure Active Directory

Si vous utilisez [Azure Active Directory](../../active-directory/index.yml) (Azure AD) dans votre organisation, Language Understanding (LUIS) a besoin d’une autorisation pour accéder aux informations sur l'accès de vos utilisateurs quand ils souhaitent utiliser LUIS. Les ressources dont LUIS a besoin sont minimes.

La description détaillée s’affiche quand vous tentez de vous inscrire avec un compte qui a un consentement de l’administrateur ou qui n’en a pas besoin, de sorte que le consentement de l’administrateur :

* Vous autorise à vous connecter à l’application avec votre compte professionnel et laisse l’application lire votre profil. Autorise également l’application à lire les informations de base sur l’entreprise. Cela accorde à LUIS l’autorisation de lire les données de profil de base, telles que l’ID d’utilisateur, l’adresse e-mail et le nom.
* Autorise l’application à afficher et à mettre à jour vos données, même quand vous n’utilisez pas l’application. L'autorisation est nécessaire pour actualiser le jeton d’accès de l’utilisateur.


### <a name="azure-active-directory-tenant-user"></a>Utilisateur du locataire Azure Active Directory

LUIS utilise le flux de consentement standard Azure Active Directory (Azure AD).

L’administrateur de locataire doit travailler directement avec l’utilisateur qui a besoin d’accéder à LUIS dans Azure AD.

* D’abord, l’utilisateur se connecte à LUIS, et une boîte de dialogue contextuelle nécessitant l’approbation de l’administrateur s’affiche. L’utilisateur doit contacter l’administrateur de locataire avant de poursuivre.
* Ensuite, l’administrateur de locataire se connecte à LUIS, et une boîte de dialogue contextuelle de flux de consentement s’affiche. C’est avec cette boîte de dialogue que l’administrateur accorde l’autorisation à l’utilisateur. Une fois que l’administrateur a donné son autorisation, l’utilisateur peut poursuivre avec LUIS. Si l’administrateur de locataire ne se connecte pas à LUIS, l’administrateur peut [consentir](https://account.activedirectory.windowsazure.com/r#/applications) à l’accès à LUIS, comme illustré sur la capture d'écran suivante. Notez que la liste est filtrée sur les éléments incluant le nom `LUIS`.

![Autorisation Azure Active Directory par site web d’application](./media/luis-how-to-collaborate/tenant-permissions.png)

Si l’administrateur de locataire veut uniquement que certaines personnes utilisent LUIS, plusieurs solutions sont possibles :
* Donnez le « consentement administrateur » (à tous les utilisateurs d’Azure AD), mais affectez ensuite la valeur « Oui » à « Affectation de l’utilisateur obligatoire » sous les propriétés de l’application d’entreprise, et enfin attribuer/ajouter uniquement les utilisateurs souhaités à l’application. Avec cette méthode, l’administrateur offre quand même le « consentement administrateur » à l’application, mais il peut contrôler les utilisateurs en mesure d’y accéder.
* Une deuxième solution consiste à utiliser l’[API de gestion des identités et des accès Azure AD dans Microsoft Graph](/graph/azuread-identity-access-management-concept-overview) pour fournir un consentement à chaque utilisateur spécifique.

Découvrez-en plus sur les utilisateurs Azure Active Directory et le consentement :
* [Limiter votre application](../../active-directory/develop/howto-restrict-your-app-to-a-set-of-users.md) à un ensemble d’utilisateurs

## <a name="next-steps"></a>Étapes suivantes

* Découvrez [comment utiliser des versions](luis-how-to-manage-versions.md) pour contrôler le cycle de vie de votre application.
* Découvrez les concepts de [ressource de création](luis-how-to-azure-subscription.md#authoring-key) et de [contributeurs](luis-how-to-azure-subscription.md#contributions-from-other-authors).
* Découvrez [comment créer](luis-how-to-azure-subscription.md) des ressources de création et de runtime.
* Migrez vers la nouvelle [ressource de création](luis-migration-authoring.md).