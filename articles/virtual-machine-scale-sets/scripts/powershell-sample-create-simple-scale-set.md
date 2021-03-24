---
title: Exemples Azure PowerShell - Créer un groupe de machines virtuelles identiques de base
description: Ce script crée un groupe de machines virtuelles identiques Azure exécutant Windows Server 2016, auquel vous pouvez accéder via une connexion RDP.
author: mimckitt
ms.author: mimckitt
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: powershell
ms.date: 03/27/2018
ms.reviewer: jushiman
ms.custom: mimckitt, devx-track-azurepowershell
ms.openlocfilehash: 021719751d1f1cc93d1ad841429c2e025a902c8a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "89078892"
---
# <a name="create-a-basic-virtual-machine-scale-set-with-powershell"></a>Créer un groupe de machines virtuelles identiques de base à l’aide de PowerShell
Ce script crée un groupe de machines virtuelles identiques exécutant Windows Server 2016. Une fois que vous avez exécuté le script, vous pouvez accéder aux instances de la machine virtuelle via RDP.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Exemple de script


[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/simple-scale-set/simple-scale-set.ps1 "Create a simple virtual machine scale set")]

## <a name="clean-up-deployment"></a>Nettoyer le déploiement
Exécutez la commande suivante pour supprimer le groupe de ressources, le groupe identique et toutes les ressources associées.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explication du script
Ce script a recours aux commandes suivantes pour créer le déploiement. Chaque élément du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [New-AzVmss](/powershell/module/az.compute/new-azvmss) | Crée le groupe de machines virtuelles identiques et toutes les ressources de support, y compris le réseau virtuel, l’équilibreur de charge et les règles NAT. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Supprime un groupe de ressources et toutes les ressources contenues. |

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur le module Azure PowerShell, consultez [Documentation Azure PowerShell](/powershell/azure/).
