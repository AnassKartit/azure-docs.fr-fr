---
title: Gérer les domaines d’erreur dans les groupes de machines virtuelles identiques Azure
description: Découvrez comment choisir le bon nombre de domaines d’erreur pendant la création d’un groupe de machines virtuelles identiques.
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: availability
ms.date: 12/18/2018
ms.reviewer: jushiman
ms.custom: mimckitt, devx-track-azurecli
ms.openlocfilehash: 8c114d6260cf81bcc4fb256fc8a09947ab9ce1d8
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102502482"
---
# <a name="choosing-the-right-number-of-fault-domains-for-virtual-machine-scale-set"></a>Choisir le bon nombre de domaines d’erreur pour un groupe de machines virtuelles identiques
Des groupes de machines virtuelles identiques sont créés avec cinq domaines d’erreur par défaut dans les régions Azure sans zones. Pour les régions qui prennent en charge le déploiement zonal de groupes de machines virtuelles identiques et si cette option est sélectionnée, la valeur par défaut du nombre de domaines d’erreur est de 1 pour chacune des zones. FD = 1 implique dans ce cas que les instances de machine virtuelle appartenant au groupe identique sont réparties entre plusieurs racks dans la mesure du possible.

Vous pouvez également envisager d’aligner le nombre de domaines d’erreur du groupe identique avec le nombre de domaines d’erreur de la fonctionnalité Disques managés. Cet alignement peut aider à éviter la perte de quorum si tout un domaine d’erreur de la fonctionnalité Disques managés tombe en panne. Le nombre de domaines d'erreur peut être défini sur une valeur inférieure ou égale au nombre de domaines d'erreur de la fonctionnalité Disques managés disponibles dans chacune des régions. Reportez-vous à ce [document](../virtual-machines/availability.md) pour en savoir plus sur le nombre de domaines d’erreur de la fonctionnalité Disques managés par région.

## <a name="rest-api"></a>API REST
Vous pouvez définir la propriété `properties.platformFaultDomainCount` sur 1, 2 ou 3 (la valeur par défaut est 3 si aucune valeur n’est spécifiée). Reportez-vous à la documentation de l’API REST [ici](/rest/api/compute/virtualmachinescalesets/createorupdate).

## <a name="azure-cli"></a>Azure CLI
Vous pouvez définir le paramètre `--platform-fault-domain-count` sur 1, 2 ou 3 (la valeur par défaut est 3 si aucune valeur n’est spécifiée). Reportez-vous à la documentation d’Azure CLI [ici](/cli/azure/vmss#az-vmss-create).

```azurecli-interactive
az vmss create \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --admin-username azureuser \
  --platform-fault-domain-count 3\
  --generate-ssh-keys
```

La création et la configuration des l’ensemble des ressources et des machines virtuelles du groupe identique prennent quelques minutes.

## <a name="next-steps"></a>Étapes suivantes
- En savoir plus sur les [fonctionnalités de disponibilité et de redondance](../virtual-machines/availability.md) pour les environnements Azure.
