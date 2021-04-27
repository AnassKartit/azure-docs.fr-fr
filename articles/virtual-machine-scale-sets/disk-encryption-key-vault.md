---
title: Création et configuration d’un coffre de clés pour Azure Disk Encryption
description: Cet article décrit les étapes de création et de configuration d’un coffre de clés à utiliser avec Azure Disk Encryption.
author: ju-shim
ms.author: jushiman
ms.topic: tutorial
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: mimckitt
ms.custom: mimckitt, devx-track-azurecli
ms.openlocfilehash: 77a2fc900e3888575fb6411a8c2b76b2c639d719
ms.sourcegitcommit: 6686a3d8d8b7c8a582d6c40b60232a33798067be
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/20/2021
ms.locfileid: "107748585"
---
# <a name="create-and-configure-a-key-vault-for-azure-disk-encryption"></a>Créer et configurer un coffre de clés pour Azure Disk Encryption

Azure Disk Encryption utilise Azure Key Vault pour contrôler et gérer les clés et les secrets de chiffrement de disque.  Pour plus d’informations sur les coffres de clés, consultez [Bien démarrer avec Azure Key Vault](../key-vault/general/overview.md) et [Fonctionnalités de sécurité d’Azure Key Vault](../key-vault/general/security-overview.md).

La création et la configuration d’un coffre de clés à utiliser avec Azure Disk Encryption impliquent trois étapes :

1. Création d’un groupe de ressources, si nécessaire
2. Création d’un coffre de clés 
3. Définition de stratégies d’accès avancé au coffre de clés

Ces étapes sont illustrées dans les guides de démarrage rapide suivants :

Si vous le souhaitez, vous pouvez également générer ou importer une clé de chiffrement principale (KEK, key encryption key).

## <a name="install-tools-and-connect-to-azure"></a>Installer les outils et se connecter à Azure

Les étapes décrites dans cet article peuvent être effectuées avec [Azure CLI](/cli/azure/), le [module Az Azure PowerShell](/powershell/azure/) ou le [portail Azure](https://portal.azure.com).

### <a name="connect-to-your-azure-account"></a>Se connecter au compte Azure

Avant d’utiliser Azure CLI ou Azure PowerShell, vous devez vous connecter à votre abonnement Azure. Pour cela, vous devez [vous connecter avec Azure CLI](/cli/azure/authenticate-azure-cli), [vous connecter avec Azure PowerShell](/powershell/azure/authenticate-azureps) ou fournir vos informations d’identification dans le portail Azure quand vous y êtes invité.

```azurecli-interactive
az login
```

```azurepowershell-interactive
Connect-AzAccount
```

[!INCLUDE [disk-encryption-key-vault](../../includes/disk-encryption-key-vault.md)]
 
## <a name="next-steps"></a>Étapes suivantes

- [Vue d’ensemble d’Azure Disk Encryption](disk-encryption-overview.md)
- [Chiffrer un groupe de machines virtuelles identiques à l’aide d’Azure CLI](disk-encryption-cli.md)
- [Chiffrer un groupe de machines virtuelles identiques à l’aide d’Azure PowerShell](disk-encryption-powershell.md)
