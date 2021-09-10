---
title: Attribuer un rôle Azure pour l’accès aux données de file d’attente
titleSuffix: Azure Storage
description: Découvrez comment attribuer des autorisations pour des données de file d’attente à un principal de sécurité Azure Active Directory avec le contrôle d’accès en fonction du rôle Azure (RBAC Azure). Le service Stockage Azure prend en charge les rôles personnalisés Azure et ceux intégrés pour l’authentification et l’autorisation via Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/13/2021
ms.author: tamram
ms.reviewer: dineshm
ms.subservice: common
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 28185918667b35012bd5fd0cc6bb6785d92b29a5
ms.sourcegitcommit: 9339c4d47a4c7eb3621b5a31384bb0f504951712
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/14/2021
ms.locfileid: "113761633"
---
# <a name="assign-an-azure-role-for-access-to-queue-data"></a>Attribuer un rôle Azure pour l’accès aux données de file d’attente

Azure Active Directory (Azure AD) autorise les droits d’accès aux ressources sécurisées par le biais du [contrôle d’accès en fonction du rôle Azure (Azure RBAC)](../../role-based-access-control/overview.md). Le Stockage Azure définit un ensemble de rôles intégrés Azure qui englobent les ensembles communs d’autorisations permettant d’accéder aux données de file d’attente.

Lorsqu’un rôle Azure est attribué à un principal de sécurité Azure AD, Azure octroie l’accès à ces ressources pour ce principal de sécurité. Un principal de sécurité Azure AD peut correspondre à un utilisateur, à un groupe, à un principal de service d’application ou à une [identité managée pour les ressources Azure](../../active-directory/managed-identities-azure-resources/overview.md).

Pour en savoir plus sur l’utilisation d’Azure AD pour autoriser l’accès aux données de file d’attente, consultez [Autoriser l’accès aux files d’attente à l’aide d’Azure Active Directory](authorize-access-azure-active-directory.md).

> [!NOTE]
> Cet article explique comment attribuer un rôle Azure pour accéder aux données de file d’attente dans un compte de stockage. Pour en savoir plus sur l’attribution de rôles pour les opérations de gestion dans Stockage Azure, consultez [Utiliser le fournisseur de ressources Stockage Azure pour accéder aux ressources de gestion](../common/authorization-resource-provider.md).

## <a name="assign-an-azure-role"></a>Affecter un rôle Azure

Vous pouvez utiliser le portail Azure, PowerShell, Azure CLI ou un modèle Azure Resource Manager pour attribuer un rôle d’accès aux données.

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

Pour accéder aux données de file d’attente dans le portail Azure avec des informations d’identification Azure AD, un utilisateur doit disposer des attributions de rôle suivantes :

- Un rôle d’accès aux données, par exemple **Contributeur aux données en file d’attente du stockage**
- Le rôle **Lecteur** d’Azure Resource Manager

Pour savoir comment attribuer ces rôles à un utilisateur, suivez les instructions fournies dans [Attribuer des rôles Azure à l’aide du portail Azure](../../role-based-access-control/role-assignments-portal.md).

Le rôle [Lecteur](../../role-based-access-control/built-in-roles.md#reader) est un rôle d’Azure Resource Manager qui permet aux utilisateurs d’afficher les ressources de compte de stockage, mais pas de les modifier. Il ne fournit pas d’autorisations en lecture pour les données dans Stockage Azure, mais uniquement pour les ressources de gestion de compte. Le rôle **Lecteur** est nécessaire pour que les utilisateurs puissent accéder aux files d’attente et aux messages dans le portail Azure.

Par exemple, si vous attribuez le rôle **Contributeur aux données en file d’attente du stockage** à l’utilisatrice Mary au niveau d’une file d’attente nommée **sample-queue**, Mary se voit attribuer un accès en lecture, écriture et suppression à cette file d’attente. Toutefois, si Mary souhaite afficher une file d’attente dans le portail Azure, le rôle **Contributeur aux données en file d’attente du stockage** ne fournit pas les autorisations suffisantes pour naviguer dans le portail et atteindre la file d’attente dans le but de la consulter. Les autres autorisations sont requises pour naviguer dans le portail et afficher les autres ressources qui y sont visibles.

Un utilisateur doit disposer du rôle **Lecteur** pour utiliser le portail Azure avec les informations d’identification Azure AD. Cela étant, si un utilisateur dispose d’un rôle doté des autorisations **Microsoft.Storage/storageAccounts/listKeys/action**, il peut utiliser le portail avec les clés de compte de stockage, par le biais de l’autorisation de clé partagée. Pour utiliser les clés de compte de stockage, l’accès à la clé partagée doit être autorisé pour le compte de stockage. Pour plus d’informations sur l’autorisation ou l’interdiction de l’accès à la clé partagée, consultez [Empêcher l’autorisation avec clé partagée pour un compte Stockage Azure](../common/shared-key-authorization-prevent.md).

Vous pouvez également attribuer un rôle Azure Resource Manager offrant des autorisations supplémentaires au-delà du rôle **Lecteur**. L’attribution d’autorisations minimales est recommandée en guise de meilleure pratique de sécurité. Pour plus d’informations, consultez [Meilleures pratiques pour Azure RBAC](../../role-based-access-control/best-practices.md).

> [!NOTE]
> Avant de vous attribuer un rôle pour l’accès aux données, vous pouvez accéder aux données de votre compte de stockage via le portail Azure, car ce dernier peut également utiliser la clé de compte pour l’accès aux données. Pour plus d’informations, consultez [Choisir comment autoriser l’accès à des données de files d’attente dans le portail Azure](../queues/authorize-data-operations-portal.md).
>
> La préversion d’Explorateur Stockage dans le portail Azure ne prend pas en charge l’utilisation d’informations d’identification Azure AD pour visualiser et modifier des données de file d’attente. Explorateur Stockage dans le portail Azure utilise toujours les clés de compte pour accéder aux données. Pour utiliser Explorateur Stockage dans le portail Azure, un rôle qui comprend **Microsoft.Storage/storageAccounts/listkeys/action** doit vous être attribué.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Pour attribuer un rôle Azure à un principal de sécurité, appelez la commande [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment). Le format de la commande peut varier selon l’étendue de l’affectation. Pour exécuter la commande, vous devez disposer d’un rôle qui comprend des autorisations **Microsoft.Authorization/roleAssignments/write** qui vous sont assignées au niveau de l’étendue correspondante ou ci-dessus.

Pour affecter un rôle limité à une file d’attente, spécifiez une chaîne contenant l’étendue de la file d’attente pour le paramètre `--scope`. L’étendue d’une file d’attente a la forme :

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

L’exemple suivant affecte le rôle **Contributeur aux données en file d’attente du stockage** à un utilisateur, limité à une file d’attente nommée *sample-queue*. Veillez à remplacer les valeurs de l’exemple et les valeurs d’espace réservé entre les crochets par vos propres valeurs : 

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Storage Queue Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/sample-resource-group/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/sample-queue"
```

Pour plus d’informations sur l’attribution de rôles avec PowerShell au niveau de l’abonnement, du groupe de ressources ou de l’étendue du compte de stockage, consultez [Affecter des rôles Azure à l’aide d’Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Pour attribuer un rôle Azure à un principal de sécurité, utilisez la commande [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create). Le format de la commande peut varier selon l’étendue de l’affectation. Le format de la commande peut varier selon l’étendue de l’affectation. Pour exécuter la commande, vous devez disposer d’un rôle qui comprend des autorisations **Microsoft.Authorization/roleAssignments/write** qui vous sont assignées au niveau de l’étendue correspondante ou ci-dessus.

Pour affecter un rôle limité à une file d’attente, spécifiez une chaîne contenant l’étendue de la file d’attente pour le paramètre `--scope`. L’étendue d’une file d’attente a la forme :

```
/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue-name>
```

L’exemple suivant affecte le rôle **Contributeur aux données de file d'attente du stockage** à un utilisateur, limité au niveau de la file d'attente. Veillez à remplacer les valeurs de l’exemple et les valeurs d’espace réservé entre les crochets par vos propres valeurs :

```azurecli-interactive
az role assignment create \
    --role "Storage Queue Data Contributor" \
    --assignee <email> \
    --scope "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/queueServices/default/queues/<queue>"
```

Pour plus d’informations sur l’attribution de rôles avec PowerShell au niveau de l’abonnement, du groupe de ressources ou de l’étendue du compte de stockage, consultez [Affecter des rôles Azure à l’aide d’Azure CLI](../../role-based-access-control/role-assignments-cli.md).

# <a name="template"></a>[Modèle](#tab/template)

Pour savoir comment utiliser un modèle d’Azure Resource Manager pour affecter un rôle Azure, consultez [Affecter des rôles Azure à l’aide de modèles Azure Resource Manager](../../role-based-access-control/role-assignments-template.md).

---

Gardez à l’esprit les points suivants concernant les attributions de rôles Azure dans Stockage Azure :

- Lorsque vous créez un compte de stockage Azure, aucune autorisation d’accès aux données ne vous est automatiquement attribuée via Azure AD. Vous devez vous attribuer explicitement un rôle Azure pour le Stockage Azure. Vous pouvez l’attribuer au niveau de votre abonnement, groupe de ressources, compte de stockage, conteneur ou file d’attente.
- Si le compte de stockage est verrouillé à l’aide d’un verrou en lecture seule Azure Resource Manager, le verrou empêche l’attribution de rôles Azure étendus au compte de stockage ou à une file d’attente.

## <a name="next-steps"></a>Étapes suivantes

- [Qu’est-ce que le contrôle d’accès en fonction du rôle Azure (RBAC Azure) ?](../../role-based-access-control/overview.md)
- [Meilleures pratiques pour Azure RBAC](../../role-based-access-control/best-practices.md)