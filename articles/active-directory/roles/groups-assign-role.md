---
title: Attribuer des rôles Azure AD aux groupes – Azure Active Directory
description: Attribuez des rôles Azure AD à des groupes assignables à un rôle dans le portail Azure, PowerShell ou l’API Graph.
services: active-directory
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: article
ms.date: 07/30/2021
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 35982d62a0e92b5f4647243cde5920529d6c2989
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122524040"
---
# <a name="assign-azure-ad-roles-to-groups"></a>Attribuer des rôles Azure AD aux groupes

Cette section décrit comment un administrateur informatique peut attribuer le rôle Azure Active Directory (Azure AD) à un groupe de Azure AD.

## <a name="prerequisites"></a>Prérequis

- Licence Azure AD Premium P1 ou P2
- Administrateur de rôle privilégié ou Administrateur général
- Module AzureAD (avec PowerShell)
- Consentement administrateur (avec l'Afficheur Graph pour l'API Microsoft Graph)

Pour plus d’informations, consultez [Prérequis pour utiliser PowerShell ou de l’Afficheur Graph](prerequisites.md).

## <a name="azure-portal"></a>Portail Azure

L’attribution d’un groupe à un rôle Azure AD est similaire à l’affectation d’utilisateurs et de principaux de service, à ceci près que seuls les groupes qui sont assignables par rôle peuvent être utilisés. Dans le Portail Azure, seuls les groupes qui sont assignables par rôle sont affichés.

1. Connectez-vous au [portail Azure](https://portal.azure.com) ou au [centre d’administration Azure AD](https://aad.portal.azure.com).

1. Sélectionnez **Azure Active Directory** > **Rôles et administrateurs**, puis sélectionnez le rôle que vous souhaitez attribuer.

1. Sur la page ***nom du rôle** _, sélectionnez > _*Ajouter une affectation**.

   ![Ajouter la nouvelle affectation de rôle](./media/groups-assign-role/add-assignment.png)

1. Sélectionnez le groupe. Seuls les groupes qui peuvent être affectés à des rôles Azure AD sont affichés.

    [![Seuls les groupes qui peuvent être assignés sont affichés pour une nouvelle affectation de rôle.](./media/groups-assign-role/eligible-groups.png "Seuls les groupes qui peuvent être assignés sont affichés pour une nouvelle affectation de rôle.")](./media/groups-assign-role/eligible-groups.png#lightbox)

1. Sélectionnez **Ajouter**.

Pour plus d’informations sur l’affectation d’autorisations de rôles, consultez [Attribuer des rôles administrateur et non administrateur aux utilisateurs](../fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="powershell"></a>PowerShell

### <a name="create-a-group-that-can-be-assigned-to-role"></a>Créer un groupe assignable à un rôle

```powershell
$group = New-AzureADMSGroup -DisplayName "Contoso_Helpdesk_Administrators" -Description "This group is assigned to Helpdesk Administrator built-in role in Azure AD." -MailEnabled $true -SecurityEnabled $true -MailNickName "contosohelpdeskadministrators" -IsAssignableToRole $true 
```

### <a name="get-the-role-definition-for-the-role-you-want-to-assign"></a>Obtenir la définition de rôle que vous souhaitez attribuer

```powershell
$roleDefinition = Get-AzureADMSRoleDefinition -Filter "displayName eq 'Helpdesk Administrator'" 
```

### <a name="create-a-role-assignment"></a>Création d'une affectation de rôle

```powershell
$roleAssignment = New-AzureADMSRoleAssignment -DirectoryScopeId '/' -RoleDefinitionId $roleDefinition.Id -PrincipalId $group.Id 
```

## <a name="microsoft-graph-api"></a>API Microsoft Graph

### <a name="create-a-group-that-can-be-assigned-azure-ad-role"></a>Créer un groupe assignable à un rôle Azure AD

```
POST https://graph.microsoft.com/beta/groups
{
"description": "This group is assigned to Helpdesk Administrator built-in role of Azure AD.",
"displayName": "Contoso_Helpdesk_Administrators",
"groupTypes": [],
"mailEnabled": false,
"securityEnabled": true,
"mailNickname": "contosohelpdeskadministrators",
"isAssignableToRole": true
}
```

### <a name="get-the-role-definition"></a>Obtenir la définition de rôle

```
GET https://graph.microsoft.com/beta/roleManagement/directory/roleDefinitions?$filter = displayName eq 'Helpdesk Administrator'
```

### <a name="create-the-role-assignment"></a>Créer l’attribution de rôle

```
POST https://graph.microsoft.com/beta/roleManagement/directory/roleAssignments
{
"principalId":"<Object Id of Group>",
"roleDefinitionId":"<ID of role definition>",
"directoryScopeId":"/"
}
```
## <a name="next-steps"></a>Étapes suivantes

- [Utiliser des groupes Azure AD pour gérer les attributions de rôles](groups-concept.md)
- [Résoudre les problèmes de rôles Azure AD attribués aux groupes](groups-faq-troubleshooting.yml)
