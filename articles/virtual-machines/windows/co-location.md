---
title: Colocaliser des machines virtuelles pour améliorer la latence
description: En savoir plus sur la façon dont la colocalisation des ressources de machine virtuelle Azure peut améliorer la latence.
author: cynthn
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 10/30/2019
ms.author: zivr
ms.openlocfilehash: eb7b5d3b477a511432dbd3ee8a6b382002aaab44
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96488462"
---
# <a name="co-locate-resource-for-improved-latency"></a>Colocalisation des ressources pour une latence améliorée

Lors du déploiement de votre application dans Azure, la répartition des instances sur différentes régions ou zones de disponibilité crée une latence réseau, ce qui peut avoir un impact sur les performances globales de votre application. 


## <a name="proximity-placement-groups"></a>Groupes de placements de proximité 

[!INCLUDE [virtual-machines-common-ppg-overview](../../../includes/virtual-machines-common-ppg-overview.md)]

## <a name="next-steps"></a>Étapes suivantes

Déployez une machine virtuelle [sur un groupe de placements de proximité](proximity-placement-groups.md) en utilisant Azure PowerShell.

Découvrez comment [tester le temps de réponse du réseau](../../virtual-network/virtual-network-test-latency.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Découvrez comment [optimiser le débit du réseau](../../virtual-network/virtual-network-optimize-network-bandwidth.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).  

Découvrez comment [utiliser des groupes de placement de proximité avec des applications SAP](../workloads/sap/sap-proximity-placement-scenarios.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).