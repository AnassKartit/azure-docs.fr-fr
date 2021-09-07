---
title: Accorder et demander des autorisations à l’échelle du locataire dans Azure Security Center
description: Découvrez comment gérer les autorisations à l’échelle du locataire dans Azure Security Center
author: memildin
ms.author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 03/11/2021
ms.openlocfilehash: 99e7e09ac1b73dfb24cea14fe1b407f491e4ade5
ms.sourcegitcommit: f3b930eeacdaebe5a5f25471bc10014a36e52e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/16/2021
ms.locfileid: "112236892"
---
# <a name="grant-and-request-tenant-wide-visibility"></a>Accorder et demander une visibilité à l’échelle du locataire

Si un utilisateur ayant le rôle Azure Active Directory (AD) d’**administrateur général** peut avoir des responsabilités à l’échelle du locataire, il peut ne pas disposer des autorisations Azure lui permettant de consulter les informations à l’échelle de l’organisation dans Azure Security Center. L’élévation d’autorisations est requise, car les attributions de rôles Azure AD n’accordent pas l’accès aux ressources Azure. 

## <a name="grant-tenant-wide-permissions-to-yourself"></a>Accorder à vous-même des autorisations à l’échelle du locataire

Pour vous attribuer des autorisations au niveau du locataire :

1. Si votre organisation gère l’accès aux ressources avec [Azure AD Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-configure.md) ou tout autre outil PIM, le rôle d’administrateur général doit être actif pour l’utilisateur qui effectue la procédure ci-dessous.

1. En tant qu’utilisateur Administrateur général sans affectation au groupe d’administration racine du locataire, ouvrez la page **Vue d’ensemble** de Security Center, puis sélectionnez le lien **Visibilité à l’échelle du locataire** dans la bannière. 

    :::image type="content" source="media/security-center-management-groups/enable-tenant-level-permissions-banner.png" alt-text="Activer des autorisations au niveau du locataire dans Azure Security Center.":::

1. Sélectionnez le nouveau rôle Azure à attribuer. 

    :::image type="content" source="media/security-center-management-groups/enable-tenant-level-permissions-form.png" alt-text="Formulaire de définition des autorisations au niveau du locataire à attribuer à votre utilisateur.":::

    > [!TIP]
    > En règle générale, le rôle Administrateur de sécurité est nécessaire pour appliquer des stratégies au niveau racine, tandis que Lecteur de Sécurité est suffisant pour fournir une visibilité au niveau locataire. Pour plus d’informations sur les autorisations accordées par ces rôles, consultez la [description du rôle intégré Administrateur de sécurité](../role-based-access-control/built-in-roles.md#security-admin) ou la [description du rôle intégré de Lecteur Sécurité](../role-based-access-control/built-in-roles.md#security-reader).
    >
    > Pour connaître les différences entre ces rôles spécifiques à Security Center, consultez le tableau présenté dans [Rôles et actions autorisées](security-center-permissions.md#roles-and-allowed-actions).

    La vue à l’échelle de l’organisation est obtenue en accordant des rôles au niveau du groupe d’administration racine du locataire.  

1. Déconnectez-vous du portail Azure, puis reconnectez-vous.

1. Une fois que vous disposez d’un accès élevé, ouvrez ou actualisez Azure Security Center pour vérifier que vous voyez tous les abonnements sous votre locataire Azure AD. 

Le processus simple ci-dessus effectue automatiquement un certain nombre d’opérations :

1. Les autorisations de l’utilisateur sont temporairement élevées.
1. Grâce aux nouvelles autorisations, l’utilisateur est affecté au rôle Azure RBAC souhaité sur le groupe d’administration racine.
1. Les autorisations élevées sont supprimées.

Pour plus d’informations sur le processus d’élévation d’Azure AD, consultez [Élever l’accès pour gérer tous les abonnements et groupes d’administration Azure](../role-based-access-control/elevate-access-global-admin.md).


## <a name="request-tenant-wide-permissions-when-yours-are-insufficient"></a>Demander des autorisations à l’échelle du locataire quand les vôtres sont insuffisantes

Si vous vous connectez à Security Center et que vous voyez une bannière indiquant que votre affichage est limité, vous pouvez cliquer dessus pour envoyer une demande à l’administrateur général de votre organisation. Dans la demande, vous pouvez inclure le rôle que vous souhaitez obtenir, et l’administrateur général prendra une décision sur le rôle à accorder. 

Il appartient à l’administrateur général d’accepter ou de rejeter ces demandes. 

> [!IMPORTANT]
> Vous ne pouvez envoyer qu’une demande tous les sept jours.

Pour demander des autorisations à privilège élevé à votre administrateur général :

1. Dans le portail Azure, ouvrez Azure Security Center.

1. Si la bannière « Vous voyez des informations limitées. » s’affiche, Sélectionnez-le.

    :::image type="content" source="media/security-center-management-groups/request-tenant-permissions.png" alt-text="Bannière informant l’utilisateur qu’il peut demander des autorisations à l’échelle du locataire.":::

1. Dans le formulaire détaillé de la demande, sélectionnez le rôle souhaité et la raison pour laquelle vous avez besoin de ces autorisations.

    :::image type="content" source="media/security-center-management-groups/request-tenant-permissions-details.png" alt-text="Page de détails pour la demande d’autorisations à l’échelle du locataire auprès de votre administrateur général Azure.":::

1. Sélectionnez **Demander l’accès**.

    Un e-mail est envoyé à l’administrateur général. L’e-mail contient un lien vers Security Center où il peut approuver ou rejeter la demande.

    :::image type="content" source="media/security-center-management-groups/request-tenant-permissions-email.png" alt-text="E-mail envoyé à l’administrateur général pour de nouvelles autorisations.":::

    Une fois que l’administrateur général a sélectionné **Examiner la demande** et a terminé le processus, la décision est envoyée par e-mail à l’utilisateur qui effectue la demande. 

## <a name="next-steps"></a>Étapes suivantes

Apprenez-en davantage sur les autorisations de Security Center dans la page associée suivante :

- [Autorisations dans Azure Security Center](security-center-permissions.md)