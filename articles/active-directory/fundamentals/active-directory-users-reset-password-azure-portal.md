---
title: Réinitialiser le mot de passe d’un utilisateur - Azure Active Directory | Microsoft Docs
description: Instructions de réinitialisation du mot de passe d’un utilisateur à l’aide d’Azure Active Directory.
services: active-directory
author: ajburnle
manager: daveba
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
ms.date: 09/05/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7efa458ebcae7e837be4bb574f4a2707e9cb9e30
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124732614"
---
# <a name="reset-a-users-password-using-azure-active-directory"></a>Réinitialiser le mot de passe d’un utilisateur à l’aide d’Azure Active Directory

En tant qu’administrateur, vous pouvez réinitialiser le mot de passe d’un utilisateur en cas d’oubli, si l’utilisateur ne peut plus accéder à un appareil verrouillé, ou bien s’il n’a jamais reçu son mot de passe.

>[!Note]
>Vous ne serez pas en mesure de réinitialiser le mot de passe d’un utilisateur si votre locataire Azure AD n’est pas son répertoire de base. Cela signifie que si votre utilisateur se connecte à votre organisation à l’aide d’un compte venant d’une autre organisation, un compte Microsoft ou un compte Google, vous ne pourrez réinitialiser son mot de passe.<br><br>Si votre utilisateur a une source d’autorité comme Windows Server Active Directory, vous ne serez en mesure de réinitialiser le mot de passe que si vous avez activé la réécriture du mot de passe.<br><br>Si votre utilisateur a une source d’autorité comme Azure AD externe, il se peut que vous ne puissiez pas réinitialiser le mot de passe. Seul l’utilisateur, ou un administrateur dans Azure AD externe, peut réinitialiser le mot de passe.

>[!Note]
>Si vous n’êtes pas administrateur et si vous recherchez des instructions de réinitialisation pour votre propre mot de passe professionnel ou scolaire, consultez [Réinitialiser votre mot de passe professionnel ou scolaire](https://support.microsoft.com/account-billing/reset-your-work-or-school-password-using-security-info-23dde81f-08bb-4776-ba72-e6b72b9dda9e).

## <a name="to-reset-a-password"></a>Pour réinitialiser un mot de passe

1. Connectez-vous au [portail Azure](https://portal.azure.com/) en tant qu’administrateur d’utilisateurs ou administrateur de mots de passe. Pour plus d’informations sur les rôles disponibles, consultez [Rôles intégrés Azure](../roles/permissions-reference.md).

2. Sélectionnez **Azure Active Directory**, sélectionnez **Utilisateurs**, recherchez et sélectionnez l’utilisateur qui a besoin de la réinitialisation, puis sélectionnez **Réinitialiser le mot de passe**.

    La page **Alain Charon - profil** s’affiche avec l’option **Réinitialiser le mot de passe**.

    ![Page du profil de l’utilisateur, avec l’option Réinitialiser le mot de passe mis en surbrillance](media/active-directory-users-reset-password-azure-portal/user-profile-reset-password-link.png)

3. Sur la page **Réinitialiser le mot de passe**, sélectionnez **Réinitialiser le mot de passe**.

    > [!Note]
    > Lorsque Azure Active Directory est utilisé, un mot de passe temporaire est automatiquement généré pour l’utilisateur. Lorsque vous utilisez Active Directory en local, vous créez le mot de passe pour l’utilisateur.

4. Copiez le mot de passe et donnez-le à l’utilisateur. L’utilisateur devra le modifier lors de sa prochaine connexion.

    >[!Note]
    >Le mot de passe temporaire n’expire jamais. La prochaine fois que l’utilisateur se connectera, le mot de passe fonctionnera toujours, quel que soit le temps écoulé dans depuis sa création.

> [!IMPORTANT]
> Si un administrateur ne peut pas réinitialiser le mot de passe de l’utilisateur, et si, dans les journaux des événements de l’application sur le serveur de Azure AD Connect, le code d’erreur hr=80231367 s’affiche, révisez les attributs de l’utilisateur dans Active Directory.  Si l’attribut **AdminCount** a la valeur 1, cela empêchera un administrateur de réinitialiser le mot de passe de l’utilisateur.  L’attribut **AdminCount** doit avoir la valeur 0 pour permettre aux administrateurs de réinitialiser le mot de passe de l’utilisateur.


## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez réinitialisé le mot de passe de votre utilisateur, vous pouvez exécuter les procédures de base suivantes :

- [Ajouter ou supprimer des utilisateurs](add-users-azure-active-directory.md)

- [Attribuer des rôles aux utilisateurs](active-directory-users-assign-role-azure-portal.md)

- [Ajouter ou modifier les informations de profil](active-directory-users-profile-azure-portal.md)

- [Créer un groupe de base et ajouter des membres](active-directory-groups-create-azure-portal.md)

Vous pouvez également suivre des scénarios utilisateur plus complexes, comme l’affectation de délégués, l’utilisation de stratégies et le partage de comptes d’utilisateur. Pour en savoir plus sur les autres actions disponibles, consultez la [documentation Gestion des utilisateurs Azure Active Directory](../enterprise-users/index.yml).