---
title: Groupes de placement de proximité pour les groupes de machines virtuelles identiques
description: En savoir plus sur la création et l’utilisation de groupes de placements de proximité pour les groupes de machines virtuelles identiques Windows dans Azure.
author: cynthn
ms.author: cynthn
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: proximity-placement-groups
ms.date: 07/01/2019
ms.reviewer: zivr
ms.custom: mimckitt, devx-track-azurepowershell
ms.openlocfilehash: b5533f14a239e3e2d01c597d57ab2b2726e930b9
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122693263"
---
# <a name="creating-and-using-proximity-placement-groups-using-powershell"></a>Création et utilisation de groupes de placements de proximité à l’aide de PowerShell

**S’applique à :** :heavy_check_mark: Machines virtuelles Linux :heavy_check_mark: Machines virtuelles Windows :heavy_check_mark: Groupes identiques uniformes

Pour que les machines virtuelles soient aussi proches que possible, avec la latence la plus faible possible, déployez votre groupe identique dans un [groupe de placements de proximité](../virtual-machines/co-location.md#proximity-placement-groups).

Le groupe de placements de proximité est un regroupement logique utilisé pour s’assurer que les ressources de calcul Azure se trouvent proches les unes des autres. Les groupes de placements de proximité sont utiles pour les charges de travail où une latence faible est requise.


## <a name="create-a-proximity-placement-group"></a>Créer un groupe de placements de proximité
Créez un groupe de placements de proximité à l’aide de la cmdlet [New-AzProximityPlacementGroup](/powershell/module/az.compute/new-azproximityplacementgroup). 

```azurepowershell-interactive
$resourceGroup = "myPPGResourceGroup"
$location = "East US"
$ppgName = "myPPG"
New-AzResourceGroup -Name $resourceGroup -Location $location
$ppg = New-AzProximityPlacementGroup `
   -Location $location `
   -Name $ppgName `
   -ResourceGroupName $resourceGroup `
   -ProximityPlacementGroupType Standard
```

## <a name="list-proximity-placement-groups"></a>Liste de groupes de placements de proximité

Vous pouvez créer une liste de tous les groupes de placements de proximité à l’aide de la cmdlet [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup).

```azurepowershell-interactive
Get-AzProximityPlacementGroup
```


## <a name="create-a-scale-set"></a>Créer un groupe identique

Créez un groupe identique dans le groupe de placements de proximité avec `-ProximityPlacementGroup $ppg.Id` pour faire référence à l’ID de groupe de placements de proximité quand vous utilisez [New-AzVMSS](/powershell/module/az.compute/new-azvmss) pour créer le groupe identique.

```azurepowershell-interactive
$scalesetName = "myVM"

New-AzVmss `
  -ResourceGroupName $resourceGroup `
  -Location $location `
  -VMScaleSetName $scalesetName `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -ProximityPlacementGroup $ppg.Id
```

Vous pouvez voir l’instance dans le groupe de placements avec la commande [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup).

```azurepowershell-interactive
  Get-AzProximityPlacementGroup `
   -ResourceId $ppg.Id | Format-Table `
   -Wrap `
   -Property VirtualMachineScaleSets
```

## <a name="next-steps"></a>Étapes suivantes

Vous pouvez également utiliser [Azure CLI](../virtual-machines/linux/proximity-placement-groups.md) pour créer des groupes de placements de proximité.
