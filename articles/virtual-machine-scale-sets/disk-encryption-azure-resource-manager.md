---
title: Créer et chiffrer un groupe de machines virtuelles identiques avec des modèles Azure Resource Manager
description: Dans ce guide de démarrage rapide, vous allez apprendre à utiliser les modèles Azure Resource Manager pour créer et chiffrer un groupe de machines virtuelles identiques
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 058ba9a3b2593ec6f635485771d5bbe140beb510
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122698987"
---
# <a name="encrypt-virtual-machine-scale-sets-with-azure-resource-manager"></a>Chiffrer des groupes de machines virtuelles identiques avec Azure Resource Manager

**S’applique à :** :heavy_check_mark: Machines virtuelles Linux :heavy_check_mark: Machines virtuelles Windows :heavy_check_mark: Groupes identiques uniformes

Vous pouvez chiffrer ou déchiffrer des groupes de machines virtuelles identiques Linux à l’aide de modèles Azure Resource Manager.

## <a name="deploying-templates"></a>Déploiement de modèles

Tout d’abord, sélectionnez le modèle qui convient à votre scénario.

- [Activer le chiffrement de disque sur un groupe de machines virtuelles identiques Linux en cours d’exécution](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/encrypt-running-vmss-linux)

- [Activer le chiffrement de disque sur un groupe de machines virtuelles identiques Windows en cours d’exécution](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/encrypt-running-vmss-windows)

  - [Déployer un groupe de machines virtuelles identiques Linux avec une machine jumpbox et activer le chiffrement sur les groupes de machines virtuelles identiques Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/encrypt-vmss-linux-jumpbox)

  - [Déployer un groupe de machines virtuelles identiques Windows avec une machine jumpbox et activer le chiffrement sur les groupes de machines virtuelles identiques Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/encrypt-vmss-windows-jumpbox)

- [Désactiver le chiffrement de disque sur un groupe de machines virtuelles identiques Linux en cours d’exécution](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/decrypt-vmss-linux)

- [Désactiver le chiffrement de disque sur un groupe de machines virtuelles identiques Windows en cours d’exécution](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.compute/decrypt-vmss-windows)

Exécutez ensuite les opérations qui suivent :

1. Cliquez sur **Déployer dans Azure**.
2. Renseignez les champs obligatoires, puis acceptez les conditions générales.
3. Sélectionnez **Acheter** pour déployer le modèle.

## <a name="next-steps"></a>Étapes suivantes

- [Azure Disk Encryption pour les groupes de machines virtuelles identiques](disk-encryption-overview.md)
- [Chiffrer un groupe de machines virtuelles identiques à l’aide d’Azure CLI](disk-encryption-cli.md)
- [Chiffrer un groupe de machines virtuelles identiques à l’aide d’Azure PowerShell](disk-encryption-powershell.md)
- [Créer et configurer un coffre de clés pour Azure Disk Encryption](disk-encryption-key-vault.md)
- [Utiliser Azure Disk Encryption avec le séquencement d’extensions de groupes de machines virtuelles identiques](disk-encryption-extension-sequencing.md)
