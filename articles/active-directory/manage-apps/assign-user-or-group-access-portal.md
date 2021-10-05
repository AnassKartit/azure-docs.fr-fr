---
title: Affecter des utilisateurs et des groupes
titleSuffix: Azure AD
description: Découvrez comment attribuer et désattribuer des utilisateurs et des groupes pour une application en utilisant Azure Active Directory pour la gestion des identités.
services: active-directory
author: davidmu1
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: how-to
ms.date: 09/23/2021
ms.author: davidmu
ms.reviewer: alamaral
ms.openlocfilehash: 1bb1d7c65451be88864440d2e5b1327b3688f337
ms.sourcegitcommit: 48500a6a9002b48ed94c65e9598f049f3d6db60c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/26/2021
ms.locfileid: "129061349"
---
# <a name="assign-users-and-groups-to-an-application-in-azure-active-directory"></a>Assigner des utilisateurs et des groupes à une application dans Azure Active Directory

Cet article vous montre comment attribuer des utilisateurs et des groupes à une application d’entreprise dans Azure Active Directory (Azure AD) à l’aide de PowerShell. Lorsque vous attribuez un utilisateur à une application, celle-ci apparaît dans le portail Mes applications de l’utilisateur pour en faciliter l’accès. Si l’application expose des rôles, vous pouvez également attribuer un rôle spécifique à l’utilisateur.

## <a name="prerequisites"></a>Prérequis

Pour attribuer des utilisateurs à une application à l’aide de PowerShell, vous avez besoin des éléments suivants :

- Compte Azure avec un abonnement actif. [Créez un compte gratuitement](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Un des rôles suivants : Administrateur général, Administrateur d’application cloud, Administrateur d’application ou propriétaire du principal de service.
- Si vous n’avez pas encore installé le module AzureAD (utilisez la commande `Install-Module -Name AzureAD`). Si vous y êtes invité, installez un module NuGet ou le nouveau module Azure Active Directory V2 PowerShell, tapez O et appuyez sur Entrée.
- Azure Active Directory Premium P1 ou P2 pour l’attribution basée sur le groupe. Pour d’autres conditions de gestion des licences relatives aux composants traités dans le présent article, consultez la [page sur la tarification d’Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory).
- Facultatif : effectuer [Configurer une application](add-application-portal-configure.md).

## <a name="assign-users-and-groups-to-an-app-using-powershell"></a>Attribuer des utilisateurs et des groupes à une application à l’aide de PowerShell

1. Ouvrez une invite de commandes Windows PowerShell avec des privilèges élevés.
1. Exécutez `Connect-AzureAD` et connectez-vous avec un compte d’utilisateur Administrateur général.
1. Pour affecter un utilisateur et un rôle à une application, utilisez le script suivant :

    ```powershell
    # Assign the values to the variables
    $username = "<Your user's UPN>"
    $app_name = "<Your App's display name>"
    $app_role_name = "<App role display name>"

    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }

    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id

To assign a group to an enterprise app, you must replace `Get-AzureADUser` with `Get-AzureADGroup` and replace `New-AzureADUserAppRoleAssignment` with `New-AzureADGroupAppRoleAssignment`.

For more information about how to assign a group to an application role, see the documentation for [New-AzureADGroupAppRoleAssignment](/powershell/module/azuread/new-azureadgroupapproleassignment).

### Example

This example assigns the user Britta Simon to the Microsoft Workplace Analytics application using PowerShell.

1. In PowerShell, assign the corresponding values to the variables $username, $app_name and $app_role_name.

    ```powershell
    # Assign the values to the variables
    $username = "britta.simon@contoso.com"
    $app_name = "Workplace Analytics"

1. In this example, we don't know what is the exact name of the application role we want to assign to Britta Simon. Run the following commands to get the user ($user) and the service principal ($sp) using the user UPN and the service principal display names.

    ```powershell
    # Get the user to assign, and the service principal for the app to assign to
    $user = Get-AzureADUser -ObjectId "$username"
    $sp = Get-AzureADServicePrincipal -Filter "displayName eq '$app_name'"

1. Run the command `$sp.AppRoles` to display the roles available for the Workplace Analytics application. In this example, we want to assign Britta Simon the Analyst (Limited access) Role.
   ![Shows the roles available to a user using Workplace Analytics Role](./media/assign-user-or-group-access-portal/workplace-analytics-role.png)
1. Assign the role name to the `$app_role_name` variable.

    ```powershell
    # Assign the values to the variables
    $app_role_name = "Analyst (Limited access)"
    $appRole = $sp.AppRoles | Where-Object { $_.DisplayName -eq $app_role_name }

1. Run the following command to assign the user to the app role:

    ```powershell
    # Assign the user to the app role
    New-AzureADUserAppRoleAssignment -ObjectId $user.ObjectId -PrincipalId $user.ObjectId -ResourceId $sp.ObjectId -Id $appRole.Id
    ```

## <a name="unassign-users-and-groups-from-an-app-using-powershell"></a>Désattribuer des utilisateurs et des groupes d’une application à l’aide de PowerShell

1. Ouvrez une invite de commandes Windows PowerShell avec des privilèges élevés.
1. Exécutez `Connect-AzureAD` et connectez-vous avec un compte d’utilisateur Administrateur général. Pour supprimer un utilisateur et un rôle d’une application, utilisez le script suivant :

    ```powershell
    # Store the proper parameters
    $user = get-azureaduser -ObjectId <objectId>
    $spo = Get-AzureADServicePrincipal -ObjectId <objectId>

    #Get the ID of role assignment 
    $assignments = Get-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId | Where {$_.PrincipalDisplayName -eq $user.DisplayName}

    #if you run the following, it will show you what is assigned what
    $assignments | Select *

    #To remove the App role assignment run the following command.
    Remove-AzureADServiceAppRoleAssignment -ObjectId $spo.ObjectId -AppRoleAssignmentId $assignments[assignment #].ObjectId
    ```

## <a name="related-articles"></a>Articles connexes

- [Attribuer, ou supprimer l’attribution, d’un utilisateur ou d’un groupe à l’aide du portail Azure](add-application-portal-assign-users.md)
- [Attribuer ou désattribuer des utilisateurs et des groupes pour une application à l’aide de l’API Graph](/graph/api/resources/approleassignment)
- [Gérer l’accès aux applications](what-is-access-management.md)

## <a name="next-steps"></a>Étapes suivantes

- [Masquer une application pour un utilisateur](hide-application-from-user-portal.md)
