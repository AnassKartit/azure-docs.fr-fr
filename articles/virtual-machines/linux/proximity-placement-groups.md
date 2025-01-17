---
title: Créer un groupe de placements de proximité à l'aide d'Azure CLI
description: En savoir plus sur la création et l’utilisation de groupes de placement de proximité pour les machines virtuelles dans Azure.
author: cynthn
ms.service: virtual-machines
ms.subservice: proximity-placement-groups
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 3/8/2021
ms.author: cynthn
ms.openlocfilehash: d37301d0c1bdde17f148c3aedb875d3a1c52d123
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122698138"
---
# <a name="deploy-vms-to-proximity-placement-groups-using-azure-cli"></a>Déployer des machines virtuelles dans des groupes de placements avec Azure CLI

**S’applique à :** :heavy_check_mark: Machines virtuelles Linux :heavy_check_mark: Groupes identiques flexibles 

Pour que les machines virtuelles soient aussi proches que possible, avec la latence la plus faible possible, déployez-les dans un [groupe de placements de proximité](../co-location.md#proximity-placement-groups).

Le groupe de placements de proximité est un regroupement logique utilisé pour s’assurer que les ressources de calcul Azure se trouvent proches les unes des autres. Les groupes de placements de proximité sont utiles pour les charges de travail où une latence faible est requise.


## <a name="create-the-proximity-placement-group"></a>Créer le groupe de placements de proximité
Créez un groupe de placements de proximité à l’aide de [`az ppg create`](/cli/azure/ppg#az_ppg_create). 

```azurecli-interactive
az group create --name myPPGGroup --location westus
az ppg create \
   -n myPPG \
   -g myPPGGroup \
   -l westus \
   -t standard 
```

## <a name="list-proximity-placement-groups"></a>Liste de groupes de placements de proximité

Vous pouvez répertorier tous vos groupes de placements de proximité à l’aide de la commande [az ppg list](/cli/azure/ppg#az_ppg_list).

```azurecli-interactive
az ppg list -o table
```

## <a name="create-a-vm"></a>Créer une machine virtuelle

Créez une machine virtuelle dans le groupe placements de proximité avec la commande [new az vm](/cli/azure/vm#az_vm_create).

```azurecli-interactive
az vm create \
   -n myVM \
   -g myPPGGroup \
   --image UbuntuLTS \
   --ppg myPPG  \
   --generate-ssh-keys \
   --size Standard_D1_v2  \
   -l westus
```

Vous pouvez voir la machine virtuelle dans le groupe de placements de proximité avec [az ppg show](/cli/azure/ppg#az_ppg_show).

```azurecli-interactive
az ppg show --name myppg --resource-group myppggroup --query "virtualMachines"
```

## <a name="availability-sets"></a>Groupes à haute disponibilité
Vous pouvez également créer un groupe à haute disponibilité dans votre groupe de placements de proximité. Utilisez le même paramètre `--ppg` avec la cmdlet [az vm availability-set create](/cli/azure/vm/availability-set#az_vm_availability_set_create) pour créer un groupe à haute disponibilité et toutes les machines virtuelles dans le groupe à haute disponibilité seront également créées dans le même groupe de placements de proximité.

## <a name="scale-sets"></a>Groupes identiques

Vous pouvez également créer un groupe identique dans votre groupe de placements de proximité. Utilisez le même paramètre `--ppg` avec [az vmss create](/cli/azure/vmss#az_vmss_create) pour créer un jeu de mise à l’échelle et toutes les instances seront créées dans le même groupe de placements de proximité.

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur les commandes [Azure CLI](/cli/azure/ppg) pour les groupes de placements de proximité.