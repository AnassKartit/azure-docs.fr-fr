---
title: Tailles de machine virtuelle
description: Liste les différentes tailles disponibles pour les machines virtuelles dans Azure.
author: ju-shim
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 07/21/2020
ms.author: jushiman
ms.openlocfilehash: 1da0acf71d8f3f806ec42dcb61c4d74127892c27
ms.sourcegitcommit: 05dd6452632e00645ec0716a5943c7ac6c9bec7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2021
ms.locfileid: "122563773"
---
# <a name="sizes-for-virtual-machines-in-azure"></a>Tailles des machines virtuelles dans Azure

> [!TIP]
> Essayez l’ **[outil de sélection des machines virtuelles](https://aka.ms/vm-selector)** pour trouver d’autres tailles mieux adaptées à votre charge de travail.

Cet article décrit les tailles et options disponibles pour les machines virtuelles Azure que vous pouvez utiliser pour exécuter vos applications et charges de travail. Il expose également les points à prendre en considération pour le déploiement quand vous planifiez l’utilisation de ces ressources. 

:::image type="content" source="media/sizes/azurevmsthumb.jpg" alt-text="Vidéo YouTube sur le choix de la bonne taille pour votre machine virtuelle." link="https://youtu.be/zOSvnJFd3ZM":::

| Type | Tailles | Description |
|------|-------|-------------|
| [Usage général](sizes-general.md)   | B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, Av2, DC, DCv2, Dv4, Dsv4, Ddv4, Ddsv4  | Ratio processeur/mémoire équilibré. Idéal pour le test et le développement, les bases de données petites à moyennes et les serveurs web au trafic faible à moyen. |
| [Optimisé pour le calcul](sizes-compute.md) | F, Fs, Fsv2, FX | Ratio processeur/mémoire élevé. Convient pour les serveurs web au trafic moyen, les appareils réseau, les processus de traitement par lots et les serveurs d’application. |
| [Mémoire optimisée](sizes-memory.md) | Esv3, Ev3, Easv4, Eav4, Ev4, Esv4, Edv4, Edsv4, Mv2, M, DSv2, Dv2 | Ratio mémoire/processeur élevé. Idéal pour les serveurs de base de données relationnelle, les caches moyens à grands et l’analytique en mémoire.                 |
| [Optimisé pour le stockage](sizes-storage.md) | Lsv2 | Débit et nombre d’E/S de disque élevés, idéal pour les Big Data, SQL, les bases de données NoSQL, l’entreposage de données et les grandes bases de données transactionnelles.  |
| [GPU](sizes-gpu.md) | NC, NCv2, NCv3, NCasT4_v3, ND, NDv2, NV, NVv3, NVv4 | Machines virtuelles spécialisées, ciblées pour l’affichage de graphiques complexes et le montage vidéo, ainsi que pour la formation et l’inférence de modèles avec apprentissage approfondi. Disponible avec un ou plusieurs GPU. |
| [Calcul haute performance](sizes-hpc.md) | HB, HBv2, HBv3, HC, H | Nos machines virtuelles les plus rapides et dotées des processeurs les plus puissants avec interfaces réseau haut débit en option (RDMA). |

- Pour plus d’informations sur la tarification des différentes tailles, consultez les pages de tarification pour [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux) ou [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/Windows/#Windows).
- Pour la disponibilité des tailles de machine virtuelle dans les régions Azure, consultez [Disponibilité des produits par région](https://azure.microsoft.com/regions/services/).
- Pour connaître les limites générales des machines virtuelles Azure, consultez [Abonnement Azure et limites, quotas et contraintes du service](../azure-resource-manager/management/azure-subscription-service-limits.md).
- Pour plus d’informations sur la façon dont Azure nomme ses machines virtuelles, consultez [Conventions de nommage des tailles de machines virtuelles Azure](./vm-naming-conventions.md).

## <a name="rest-api"></a>API REST

Pour plus d’informations sur l’utilisation de l’API REST pour demander la taille des machines virtuelles, consultez les rubriques suivantes :

- [Répertorier les tailles disponibles des machines virtuelles pour le redimensionnement](/rest/api/compute/virtualmachines/listavailablesizes)
- [Répertorier les tailles disponibles des machines virtuelles pour un abonnement](/rest/api/compute/resourceskus/list)
- [Répertorier les tailles de machine virtuelle disponibles dans un groupe à haute disponibilité](/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Lisez-en davantage sur les [Unités de calcul Azure (ACU)](acu.md) pour découvrir comment comparer les performances de calcul entre les références Azure.

## <a name="benchmark-scores"></a>Scores de test d’évaluation

En savoir plus sur les performances de calcul pour les machines virtuelles Linux à l’aide des [scores de test d’évaluation CoreMark](./linux/compute-benchmark-scores.md).

En savoir plus sur les performances de calcul pour les machines virtuelles Windows à l’aide des [scores de test d’évaluation SPECInt](./windows/compute-benchmark-scores.md).

## <a name="manage-costs"></a>Gérer les coûts

[!INCLUDE [cost-management-horizontal](../../includes/cost-management-horizontal.md)]

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur les différentes tailles de machines virtuelles qui sont disponibles :

- [Usage général](sizes-general.md)
- [Optimisé pour le calcul](sizes-compute.md)
- [Mémoire optimisée](sizes-memory.md)
- [Optimisé pour le stockage](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Calcul haute performance](sizes-hpc.md)
- Consultez la page [Génération précédente](sizes-previous-gen.md) des séries A Standard, Dv1 (D1-4 et D11-14 v1) et A8-A11
