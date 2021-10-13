---
title: PowerShell pour les rôles Azure AD dans PIM – Azure AD | Microsoft Docs
description: Gérez les rôles Azure AD à l’aide des applets de commande PowerShell dans Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: KarenH444
editor: ''
ms.service: active-directory
ms.subservice: pim
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/07/2021
ms.author: curtand
ms.reviewer: shaunliu
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b25ae76e2af971c32b027245a852c29f088a145
ms.sourcegitcommit: bee590555f671df96179665ecf9380c624c3a072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/07/2021
ms.locfileid: "129669223"
---
# <a name="powershell-for-azure-ad-roles-in-privileged-identity-management"></a>PowerShell pour les rôles Azure AD dans Privileged Identity Management

Cet article contient des instructions concernant l’utilisation des applets de commande PowerShell Azure Active Directory (Azure AD) pour gérer les rôles Azure AD dans Privileged Identity Management (PIM). Il fournit également des informations sur la configuration à l’aide du module Azure AD PowerShell.

## <a name="installation-and-setup"></a>Installation et configuration

1. Installer le module Azure AD en préversion

    ```powershell
    Install-module AzureADPreview
    ```

1. Veillez à disposer des autorisations de rôle requises avant de commencer. Si vous essayez d’effectuer des tâches de gestion telles qu’une attribution de rôle ou la mise à jour d’un paramètre de rôle, veillez à disposer du rôle Administrateur général ou Administrateur de rôle privilégié. Si vous essayez simplement d’activer votre propre attribution, aucune autorisation n’est requise au-delà des autorisations utilisateur par défaut.

1. Connectez-vous à Azure AD.

    ```powershell
    $AzureAdCred = Get-Credential  
    Connect-AzureAD -Credential $AzureAdCred
    ```

1. Recherchez l’ID de locataire de votre organisation Azure AD en accédant à **Azure Active Directory** > **Propriétés** > **ID de répertoire**. Dans la section des applets de commande, utilisez cet ID chaque fois que vous devez fournir l’ID de la ressource.

    ![Rechercher l’ID d’organisation dans les propriétés de l’organisation Azure AD](./media/powershell-for-azure-ad-roles/tenant-id-for-Azure-ad-org.png)

> [!Note]
> Les sections suivantes sont des exemples simples qui peuvent vous aider à démarrer. Vous pouvez trouver une documentation plus détaillée concernant les cmdlets suivants à l'adresse [/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true#privileged_role_management](/powershell/module/azuread/?view=azureadps-2.0-preview&preserve-view=true#privileged_role_management). Toutefois, vous devez remplacer « azureResources » dans le paramètre providerID par « aadRoles ». Vous devez également vous souvenir d’utiliser l’ID de locataire pour votre organisation Azure AD en tant que paramètre resourceId.

## <a name="retrieving-role-definitions"></a>Récupération des définitions de rôles

Utilisez la cmdlet suivante pour obtenir tous les rôles Azure AD intégrés et personnalisés dans votre organisation Azure AD. Cette étape importante vous donne le mappage entre le nom de rôle et le paramètre roleDefinitionId. Le paramètre roleDefinitionId est utilisé dans ces applets de commande pour faire référence à un rôle spécifique.

Le paramètre roleDefinitionId est spécifique à votre organisation Azure AD et diffère du paramètre roleDefinitionId retourné par l’API de gestion des rôles.

```powershell
Get-AzureADMSPrivilegedRoleDefinition -ProviderId aadRoles -ResourceId 926d99e7-117c-4a6a-8031-0cc481e9da26
```

Résultat :

![Obtenir tous les rôles pour l’organisation Azure AD](./media/powershell-for-azure-ad-roles/get-all-roles-result.png)

## <a name="retrieving-role-assignments"></a>Récupération des attributions de rôles

Utilisez l’applet de commande suivante pour récupérer toutes les attributions de rôles dans votre organisation Azure AD.

```powershell
Get-AzureADMSPrivilegedRoleAssignment -ProviderId "aadRoles" -ResourceId "926d99e7-117c-4a6a-8031-0cc481e9da26"
```

Utilisez l’applet de commande suivante pour récupérer toutes les attributions de rôles pour un utilisateur particulier. Cette liste est également appelée « Mes rôles » dans le portail Azure AD. La seule différence ici tient au fait que vous avez ajouté un filtre pour l’ID de sujet. L’ID de sujet dans ce contexte est l’ID d’utilisateur ou l’ID de groupe.

```powershell
Get-AzureADMSPrivilegedRoleAssignment -ProviderId "aadRoles" -ResourceId "926d99e7-117c-4a6a-8031-0cc481e9da26" -Filter "subjectId eq 'f7d1887c-7777-4ba3-ba3d-974488524a9d'" 
```

Utilisez l’applet de commande suivante pour récupérer toutes les attributions de rôles pour un rôle particulier. Le paramètre roleDefinitionId ici est l’ID retourné par l’applet de commande précédente.

```powershell
Get-AzureADMSPrivilegedRoleAssignment -ProviderId "aadRoles" -ResourceId "926d99e7-117c-4a6a-8031-0cc481e9da26" -Filter "roleDefinitionId eq '0bb54a22-a3df-4592-9dc7-9e1418f0f61c'"
```

Les applets de commande génèrent la liste d’objets d’attribution de rôle illustrée ci-dessous. L’ID de sujet est l’ID d’utilisateur de l’utilisateur auquel le rôle est attribué. L’état d’attribution peut être actif ou éligible. Si l’utilisateur est actif et qu’il existe un ID dans le champ LinkedEligibleRoleAssignmentId, cela signifie que le rôle est actuellement activé.

Résultat :

![Récupérer toutes les attributions de rôles pour l’organisation Azure AD](./media/powershell-for-azure-ad-roles/get-all-role-assignments-result.png)

## <a name="assign-a-role"></a>Attribuer un rôle

Utilisez l’applet de commande suivante pour créer une attribution éligible.

```powershell
Open-AzureADMSPrivilegedRoleAssignmentRequest -ProviderId 'aadRoles' -ResourceId '926d99e7-117c-4a6a-8031-0cc481e9da26' -RoleDefinitionId 'ff690580-d1c6-42b1-8272-c029ded94dec' -SubjectId 'f7d1887c-7777-4ba3-ba3d-974488524a9d' -Type 'adminAdd' -AssignmentState 'Eligible' -schedule $schedule -reason "dsasdsas" 
```

La planification, qui définit l’heure de début et l’heure de fin de l’attribution, est un objet qui peut être créé comme dans l’exemple suivant :

```powershell
$schedule = New-Object Microsoft.Open.MSGraph.Model.AzureADMSPrivilegedSchedule
$schedule.Type = "Once"
$schedule.StartDateTime = (Get-Date).ToUniversalTime().ToString("yyyy-MM-ddTHH:mm:ss.fffZ")
$schedule.endDateTime = "2020-07-25T20:49:11.770Z"
```
> [!Note]
> Une valeur endDateTime définie sur null indique une attribution permanente.

## <a name="activate-a-role-assignment"></a>Activer une attribution de rôle

Utilisez le cmdlet suivant pour activer une affectation admissible dans le contexte d'un utilisateur régulier :

```powershell
Open-AzureADMSPrivilegedRoleAssignmentRequest -ProviderId 'aadRoles' -ResourceId '926d99e7-117c-4a6a-8031-0cc481e9da26' -RoleDefinitionId 'f55a9a68-f424-41b7-8bee-cee6a442d418' -SubjectId 'f7d1887c-7777-4ba3-ba3d-974488524a9d' -Type 'UserAdd' -AssignmentState 'Active' -Schedule $schedule -Reason "Business Justification for the role assignment"
``` 

Si vous devez activer une affectation éligible en tant qu'administrateur, pour le `Type`paramètre, spécifiez`adminAdd` :

```powershell
Open-AzureADMSPrivilegedRoleAssignmentRequest -ProviderId 'aadRoles' -ResourceId '926d99e7-117c-4a6a-8031-0cc481e9da26' -RoleDefinitionId 'f55a9a68-f424-41b7-8bee-cee6a442d418' -SubjectId 'f7d1887c-7777-4ba3-ba3d-974488524a9d' -Type 'adminAdd' -AssignmentState 'Active' -Schedule $schedule -Reason "Business Justification for the role assignment"
``` 

Cette applet de commande est presque identique à l’applet de commande permettant de créer une attribution de rôle. La principale différence entre les cmdlets réside dans le fait que pour le paramètre –Type, l’activation est « userAdd » au lieu de « adminAdd ». L’autre différence tient au fait que le paramètre –AssignmentState a pour valeur « Active » au lieu de « Eligible ».

> [!Note]
> Il existe deux scénarios de limitation de l’activation de rôle via PowerShell.
> 1. Si vous avez besoin d’un système de ticket/numéro de ticket dans votre paramètre de rôle, il n’est pas possible de les fournir en tant que paramètre. Il n’est donc pas possible d’activer le rôle au-delà du portail Azure. Cette fonctionnalité est en cours de déploiement dans PowerShell et sera disponible au cours des prochains mois.
> 1. Si vous avez besoin d’une authentification multifacteur pour l’activation de rôle, PowerShell n’a actuellement aucun moyen de tester l’utilisateur lorsqu’il active son rôle. Au lieu de cela, les utilisateurs doivent déclencher la demande MFA quand ils se connectent à Azure AD en suivant [ce billet de blog](http://www.anujchaudhary.com/2020/02/connect-to-azure-ad-powershell-with-mfa.html) d’un de nos ingénieurs. Si vous développez une application pour PIM, une implémentation possible consiste à solliciter les utilisateurs et à les reconnecter au module une fois qu’ils ont reçu une erreur « MfaRule ».

## <a name="retrieving-and-updating-role-settings"></a>Récupération et mise à jour des paramètres de rôle

Utilisez l’applet de commande suivante pour obtenir tous les paramètres de rôles dans votre organisation Azure AD.

```powershell
Get-AzureADMSPrivilegedRoleSetting -ProviderId 'aadRoles' -Filter "ResourceId eq '926d99e7-117c-4a6a-8031-0cc481e9da26'"
```

Il existe quatre objets principaux dans le paramètre. Seuls trois de ces objets sont actuellement utilisés par PIM. Les paramètres UserMemberSettings sont des paramètres d’activation, les paramètres AdminEligibleSettings sont des paramètres d’attribution pour les attributions éligibles, et les paramètres AdminmemberSettings sont des paramètres d’attribution pour les attributions actives.

[![Get and update role settings.](media/powershell-for-azure-ad-roles/get-update-role-settings-result.png)](media/powershell-for-azure-ad-roles/get-update-role-settings-result.png#lightbox)

Pour mettre à jour le paramètre de rôle, vous devez obtenir l’objet de paramètre existant pour un rôle particulier et y apporter des modifications :

```powershell
Get-AzureADMSPrivilegedRoleSetting -ProviderId 'aadRoles' -Filter "ResourceId eq 'tenant id' and RoleDefinitionId eq 'role id'"
$settinga = New-Object Microsoft.Open.MSGraph.Model.AzureADMSPrivilegedRuleSetting
$settinga.RuleIdentifier = "JustificationRule"
$settinga.Setting = '{"required":false}'
```

Vous pouvez ensuite continuer et appliquer le paramètre à l’un des objets d’un rôle particulier, comme indiqué ci-dessous. Ici, l’ID est l’ID de paramètre de rôle qui peut être récupéré à partir du résultat de l’applet de commande qui liste les paramètres de rôle.

```powershell
Set-AzureADMSPrivilegedRoleSetting -ProviderId 'aadRoles' -Id 'ff518d09-47f5-45a9-bb32-71916d9aeadf' -ResourceId '3f5887ed-dd6e-4821-8bde-c813ec508cf9' -RoleDefinitionId '2387ced3-4e95-4c36-a915-73d803f93702' -UserMemberSettings $settinga 
```

## <a name="next-steps"></a>Étapes suivantes

- [Définitions de rôles dans Azure AD](../roles/permissions-reference.md)
