---
title: Attribuer une stratégie d’accès Azure Key Vault (CLI)
description: Comment utiliser le Azure CLI pour attribuer une stratégie d’accès Key Vault à un principal de service ou à une identité d’application.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 08/27/2020
ms.author: mbaldwin
ms.openlocfilehash: 0c7910ac149c8de43eeac92913a0d314fcc1854e
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97934575"
---
# <a name="assign-a-key-vault-access-policy"></a>Attribuer une stratégie d’accès Key Vault

Une stratégie d’accès Key Vault détermine si un principal de service donné, à savoir une application ou un groupe d’utilisateurs, peut effectuer différentes opérations sur des [secrets](../secrets/index.yml), [clés](../keys/index.yml) et [certificats](../certificates/index.yml) de Key Vault. Vous pouvez attribuer des stratégies d’accès à l’aide du [portail Azure](assign-access-policy-portal.md), d’Azure CLI (cet article) ou d’[Azure PowerShell](assign-access-policy-powershell.md).

[!INCLUDE [key-vault-access-policy-limits.md](../../../includes/key-vault-access-policy-limits.md)]

Pour plus d’informations sur la création de groupes dans Azure Active Directory à l’aide d’Azure CLI, consultez [az ad group create](/cli/azure/ad/group#az-ad-group-create) et [az ad group member add](/cli/azure/ad/group/member#az-ad-group-member-add).

## <a name="configure-the-azure-cli-and-sign-in"></a>Configurer Azure CLI et se connecter

1. Pour exécuter des commandes Azure CLI localement, installez [Azure CLI](/cli/azure/install-azure-cli).
 
    Pour exécuter des commandes directement dans le cloud, utilisez le service [Azure Cloud Shell](../../cloud-shell/overview.md).

1. CLI locale uniquement : connectez-vous à Azure à l’aide de la commande `az login` :

    ```bash
    az login
    ```

    La commande `az login` ouvre une fenêtre de navigateur pour recueillir les informations d’identification si nécessaire.

## <a name="acquire-the-object-id"></a>Acquérir l’ID d’objet

Déterminez l’ID d’objet de l’application, du groupe ou de l’utilisateur auquel vous souhaitez attribuer la stratégie d’accès :

- Applications et autres principaux de service : utilisez la commande [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) pour récupérer vos principaux de service. Examinez la sortie de la commande pour déterminer l’ID d’objet du principal de sécurité auquel vous souhaitez attribuer la stratégie d’accès.

    ```azurecli-interactive
    az ad sp list --show-mine
    ```

- Groupes : utilisez la commande [az ad group list](/cli/azure/ad/group#az-ad-group-list), en filtrant les résultats avec le paramètre `--display-name` :

     ```azurecli-interactive
    az ad group list --display-name <search-string>
    ```

- Utilisateurs : utilisez la commande [az ad user show](/cli/azure/ad/user#az-ad-user-show), en passant l’adresse e-mail de l’utilisateur dans le paramètre `--id` :

    ```azurecli-interactive
    az ad user show --id <email-address-of-user>
    ```

## <a name="assign-the-access-policy"></a>Attribuer la stratégie d’accès
    
Utilisez la commande [az key vault set-policy](/cli/azure/keyvault#az-keyvault-set-policy) pour attribuer les autorisations souhaitées :

```azurecli-interactive
az keyvault set-policy --name myKeyVault --object-id <object-id> --secret-permissions <secret-permissions> --key-permissions <key-permissions> --certificate-permissions <certificate-permissions>
```

Remplacez `<object-id>` par l’ID objet de votre principal de service.

Vous avez uniquement besoin d’inclure `--secret-permissions`, `--key-permissions`et `--certificate-permissions` lors de l’attribution d’autorisations à ces types particuliers. Les valeurs autorisées pour `<secret-permissions>`, `<key-permissions>` et `<certificate-permissions>` sont indiquées dans la documentation de la commande [az keyvault set-policy](/cli/azure/keyvault#az-keyvault-set-policy).

## <a name="next-steps"></a>Étapes suivantes

- [Sécurité d’Azure Key Vault : Gestion des identités et des accès](security-overview.md#identity-management)
- [Sécuriser votre coffre de clés](secure-your-key-vault.md)
- [Guide du développeur Azure Key Vault](developers-guide.md)