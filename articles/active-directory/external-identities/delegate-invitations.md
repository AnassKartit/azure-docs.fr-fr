---
title: Activer les paramètres de collaboration B2B externe - Azure AD
description: Découvrez comment activer la collaboration B2B externe dans Active Directory et gérer les utilisateurs autorisés à en inviter d’autres. Utilisez le rôle Inviteur d’invités pour déléguer des invitations.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 08/24/2021
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c6dbac365b10b7abaeea6c4ae600cef24fcd1d2
ms.sourcegitcommit: d11ff5114d1ff43cc3e763b8f8e189eb0bb411f1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2021
ms.locfileid: "122821793"
---
# <a name="enable-b2b-external-collaboration-and-manage-who-can-invite-guests"></a>Permettre une collaboration B2B externe et gérer les utilisateurs autorisés à en inviter d’autres

Cet article explique comment activer la collaboration B2B dans Azure Active Directory (Azure AD), désigner qui peut inviter des invités et déterminer les autorisations des utilisateurs invités dans votre Azure AD. 

Par défaut, tous les utilisateurs et invités de votre annuaire peuvent inviter des invités, même s’ils ne sont pas associés à un rôle d’administrateur. Les paramètres de collaboration externe vous permettent d’activer ou de désactiver les invitations d’invités pour différents types d’utilisateurs dans votre organisation. Vous pouvez également déléguer des invitations aux utilisateurs individuels, en leur attribuant des rôles qui leur permettent d’inviter des invités.

Azure AD offre la possibilité de limiter ce que peuvent voir les utilisateurs invités externes dans votre annuaire Azure AD. Par défaut, les utilisateurs invités sont définis sur un niveau d’autorisation limité qui les empêche d’énumérer des utilisateurs, des groupes ou d’autres ressources de l’annuaire, mais leur permet de voir l’appartenance à des groupes non masqués. Un nouveau paramètre d’aperçu vous permet de limiter encore davantage l’accès des invités, afin ces derniers puissent uniquement afficher leurs propres informations de profil. Pour plus d’informations, consultez [Restriction des autorisations d’accès invité (préversion)](../enterprise-users/users-restrict-guest-permissions.md).

## <a name="configure-b2b-external-collaboration-settings"></a>Configurer les paramètres de collaboration B2B externe

Avec Azure AD B2B Collaboration, un administrateur de clients peut définir les stratégies d’invitation suivantes :

- Désactiver les invitations
- Seuls les administrateurs et les utilisateurs membres du rôle Inviteur d’invités peuvent envoyer des invitations
- Les administrateurs, le rôle Inviteur d’invités et les membres peuvent envoyer des invitations
- Tous les utilisateurs, notamment les invités, peuvent inviter

Par défaut, tous les utilisateurs, notamment les invités, peuvent inviter des invités.

### <a name="to-configure-external-collaboration-settings"></a>Pour configurer les paramètres de la collaboration externe, procédez comme suit :

1. Connectez-vous au [portail Microsoft Azure](https://portal.azure.com) en tant qu’administrateur de locataires.
2. Sélectionnez **Azure Active Directory**.
3. Sélectionnez **Identités externes** > **Paramètres de collaboration externe**.

4. Sous **Restrictions d’accès de l’utilisateur invité (préversion)** , choisissez le niveau d’accès que les utilisateurs invités doivent avoir :
  
   - **Les utilisateurs invités ont le même accès que les membres (le plus inclusif)**  : Cette option donne aux invités le même accès aux ressources Azure AD et aux données d’annuaire que les utilisateurs membres.

   - **Les utilisateurs invités ont un accès limité aux propriétés et aux appartenances des objets d’annuaire** : Ce paramètre empêche les invités d’effectuer certaines tâches d’annuaire, telles que l’énumération d’utilisateurs, de groupes ou d’autres ressources de répertoire. Les invités peuvent voir l’appartenance de tous les groupes non masqués.

   - **L’accès utilisateur invité est limité aux propriétés et aux appartenances de ses propres objets d’annuaire (le plus restrictif)**  : Avec ce paramètre, les invités ne peuvent accéder qu’à leurs propres profils. Les invités ne sont pas autorisés à voir les profils, groupes ou appartenances à des groupes d’autres utilisateurs.

5. Sous **Paramètres d’invitation d’invités**, choisissez les paramètres appropriés :

    ![Paramètres d’invitation d’invités](./media/delegate-invitations/guest-invite-settings.png)

   - **Toute personne de l’organisation peut inviter des utilisateurs invités, y compris des invités et des non administrateurs (option la plus inclusive)**  : Pour permettre aux invités de l’organisation d’inviter d’autres invités, y compris ceux qui ne sont pas membres de l’organisation, sélectionnez cette case d’option.
   - **Les utilisateurs membres et les utilisateurs affectés à des rôles Administrateur spécifiques peuvent inviter des utilisateurs invités, y compris des invités disposant d’autorisations de membre** : Pour permettre aux utilisateurs membres et aux utilisateurs ayant des rôles Administrateur spécifiques d’inviter des invités, sélectionnez cette case d’option.
   - **Seuls les utilisateurs affectés à des rôles Administrateur spécifiques peuvent inviter des utilisateurs invités** : Pour autoriser uniquement les utilisateurs ayant des rôles Administrateur à inviter des invités, sélectionnez cette case d’option. Les rôles Administrateur sont les suivants : [Administrateur général](../roles/permissions-reference.md#global-administrator), [Administrateur d’utilisateurs](../roles/permissions-reference.md#user-administrator) et [Inviteur](../roles/permissions-reference.md#guest-inviter).
   - **Aucune personne dans l’organisation ne peut inviter des utilisateurs invités, y compris les administrateurs (option la plus restrictive)**  : Pour empêcher toute personne dans l’organisation d’inviter des invités, sélectionnez cette case d’option.
     > [!NOTE]
     > Si **Les membres peuvent inviter** est défini sur **Non** et **Les administrateurs et utilisateurs ayant le rôle d’Inviteur d’invités peuvent inviter** est défini sur **Oui**, les utilisateurs du rôle **Inviteur d’invité** pourront toujours inviter des utilisateurs.

6. Sous **Activer l’inscription en libre-service d’invité via des flux utilisateur**, sélectionnez **Oui** si vous souhaitez pouvoir créer des flux d’utilisateurs pour permettre aux utilisateurs de s’inscrire auprès des applications. Pour plus d’informations sur ce paramètre, consultez [Ajouter un flux d’utilisateurs d’inscription en libre-service à une application](self-service-sign-up-user-flow.md).

    ![Paramètre d’inscription en libre-service via des flux d’utilisateurs](./media/delegate-invitations/self-service-sign-up-setting.png)

7. Sous **Restrictions de collaboration**, vous pouvez choisir d’autoriser ou de refuser les invitations aux domaines que vous spécifiez. Vous pouvez notamment entrer des noms de domaine spécifiques dans les zones de texte. Pour plusieurs domaines, entrez chaque domaine sur une nouvelle ligne. Pour plus d’informations, consultez [Autoriser ou bloquer des invitations aux utilisateurs B2B à partir d’organisations spécifiques](allow-deny-list.md).

    ![Paramètres des restrictions de collaboration](./media/delegate-invitations/collaboration-restrictions.png)
## <a name="assign-the-guest-inviter-role-to-a-user"></a>Affecter le rôle Inviteur d’invités à un utilisateur

Avec le rôle Inviteur d’invités, vous pouvez donner à des utilisateurs la possibilité d’inviter des invités sans leur attribuer un rôle d’administrateur d’entreprise, ou tout autre rôle d’administration. Affectez le rôle Inviteur d’invités à des utilisateurs. Ensuite, vérifiez que le paramètre **Les administrateurs et utilisateurs ayant le rôle d’inviteur invité peuvent inviter** a la valeur **Oui**.

Voici un exemple qui montre comment utiliser PowerShell pour ajouter un utilisateur au rôle Inviteur d’invités :

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="next-steps"></a>Étapes suivantes

Consultez les articles suivants sur Azure AD B2B Collaboration :

- [Qu'est-ce que la collaboration B2B d'Azure AD ?](what-is-b2b.md)
- [Ajouter des utilisateurs invités B2B Collaboration sans invitation](add-user-without-invite.md)
- [Ajout d’un utilisateur B2B Collaboration à un rôle](./add-users-administrator.md)