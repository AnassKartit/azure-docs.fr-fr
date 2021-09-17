---
title: Série HBv2 – Machines virtuelles Microsoft Azure
description: Spécifications pour les machines virtuelles de la série HBv2.
author: vermagit
ms.service: virtual-machines
ms.subservice: vm-sizes-hpc
ms.topic: conceptual
ms.date: 03/08/2021
ms.author: amverma
ms.reviewer: jushiman
ms.openlocfilehash: bceda42909a2d6a2940da00d09cd46767e3c2e2d
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122689989"
---
# <a name="hbv2-series"></a>Série HBv2

**S’applique à :** :heavy_check_mark: Machines virtuelles Linux :heavy_check_mark: Machines virtuelles Windows :heavy_check_mark: Groupes identiques flexibles :heavy_check_mark: Groupes identiques uniformes

Les machines virtuelles de la série HBv2 sont optimisées pour des applications tributaires de la bande passante mémoire, par exemple la dynamique des fluides, l’analyse par éléments finis et la simulation de réservoir. Les machines virtuelles HBv2 disposent de 120 cœurs de processeur AMD EPYC 7742, de 4 Go de RAM par cœur de processeur, et d’aucun multithreading simultané. Chaque machine virtuelle HBv2 fournit jusqu’à 340 Go/s de bande passante de mémoire et jusqu’à 4 téraflops de calcul FP64.

Les machines virtuelles d’origine de la série HBv2 sont dotées de la technologie Mellanox HDR InfiniBand à 200 Gb/s. Ces machines virtuelles sont connectées dans une arborescence FAT non bloquante pour des performances RDMA optimisées et cohérentes. Ces machines virtuelles prennent en charge le routage adaptatif et le transport connecté dynamique (DCT, en plus des transports RC et UD standard). Ces fonctionnalités améliorent les performances, la scalabilité et la cohérence des applications, et leur utilisation est recommandée.

[Stockage Premium](premium-storage-performance.md) : Pris en charge<br>
[Mise en cache du Stockage Premium](premium-storage-performance.md) : Pris(e) en charge<br>
[Disques Ultra](disks-types.md#ultra-disk) : pris en charge ([En savoir plus](https://techcommunity.microsoft.com/t5/azure-compute/ultra-disk-storage-for-hpc-and-gpu-vms/ba-p/2189312) sur la disponibilité, l’utilisation et les performances) <br>
[Migration dynamique](maintenance-and-updates.md) : Non pris en charge<br>
[Mises à jour avec préservation de la mémoire](maintenance-and-updates.md) : Non pris en charge<br>
[Génération de machine virtuelle prise en charge](generation-2.md) : Génération 1 et 2<br>
[Performances réseau accélérées](../virtual-network/create-vm-accelerated-networking-cli.md) : prises en charge ([En savoir plus](https://techcommunity.microsoft.com/t5/azure-compute/accelerated-networking-on-hb-hc-hbv2-and-ndv2/ba-p/2067965) sur les performances et les problèmes potentiels) <br>
[Disques de système d’exploitation éphémères](ephemeral-os-disks.md) : Pris en charge ([en préversion](ephemeral-os-disks.md#preview---ephemeral-os-disks-can-now-be-stored-on-temp-disks))<br>
<br>

| Taille | Processeurs virtuels | Processeur | Mémoire (Gio) | Bande passante mémoire (Go/s) | Fréquence du processeur de base (GHz) | Fréquence de tous les cœurs (GHz, pic) | Fréquence d’un cœur (GHz, pic) | Performances RDMA (Gbit/s) | Prise en charge MPI | Stockage temporaire (Gio) | Disques de données max. | Cartes réseau virtuelles Ethernet max. |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_HB120rs_v2 | 120 | AMD EPYC 7V12 | 456 | 350 | 2.45 | 3.1 | 3.3 | 200 | Tous | 480 + 960 | 8 | 8 |

En savoir plus sur :
- [Architecture et topologie de machine virtuelle](./workloads/hpc/hbv2-series-overview.md)
- [Pile logicielle](./workloads/hpc/hbv2-series-overview.md#software-specifications) prise en charge incluant le système d’exploitation
- [Performances](./workloads/hpc/hbv2-performance.md) attendues de la machine virtuelle série HBv2.

[!INCLUDE [hpc-include](./workloads/hpc/includes/hpc-include.md)]

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes"></a>Autres tailles

- [Usage général](sizes-general.md)
- [Mémoire optimisée](sizes-memory.md)
- [Optimisé pour le stockage](sizes-storage.md)
- [Optimisé pour le GPU](sizes-gpu.md)
- [Calcul haute performance](sizes-hpc.md)
- [Générations précédentes](sizes-previous-gen.md)

## <a name="next-steps"></a>Étapes suivantes

- Consultez les dernières annonces, des exemples de charge de travail HPC et les résultats des performances sur les [blogs de la communauté Azure Compute Tech](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute).
- Pour une vision plus globale de l’architecture d’exécution des charges de travail HPC, consultez [Calcul haute performance (HPC) sur Azure](/azure/architecture/topics/high-performance-computing/).
- Lisez-en davantage sur les [Unités de calcul Azure (ACU)](acu.md) pour découvrir comment comparer les performances de calcul entre les références Azure.
