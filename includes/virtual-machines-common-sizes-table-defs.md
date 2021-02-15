---
title: Fichier Include
description: Fichier Include
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: d610193783d44b86d48cd5ceaa91377b32b7061b
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99579938"
---
<!-- Not used for Ls-series -->

## <a name="size-table-definitions"></a>Définitions des tailles de tables

- La capacité de stockage est indiquée en unités de Gio ou 1 024^3 octets. Lorsque vous comparez des disques mesurés en Go (1 000^3 octets) à des disques mesurés en Gio (1 024^3), n’oubliez pas que les nombres de capacité donnés en Gio peuvent paraitre inférieurs. Par exemple, 1 023 Gio = 1 098,4 Go.
- Le débit de disque est mesuré en opérations d’entrée/sortie par seconde (IOPS) et Mbit/s  où Mbit/s  = 10^6 octets par seconde.
- Les disques de données peuvent fonctionner en mode avec ou sans mise en cache. En cas de fonctionnement du disque de données avec mise en cache, le mode de mise en cache hôte est défini sur **ReadOnly** ou **ReadWrite**.  En cas de fonctionnement du disque de données sans mise en cache, le mode de mise en cache hôte est défini sur **Aucun**.
- Pour découvrir comment obtenir les meilleures performances de stockage pour vos machines virtuelles, consultez [Performances des disques et des machines virtuelles](../articles/virtual-machines/disks-performance.md).
- La **bande passante réseau attendue** est la bande passante agrégée maximale qui est allouée par type de machine virtuelle entre toutes les cartes réseau, pour toutes les destinations. Pour plus d’informations, consultez [Bande passante réseau des machines virtuelles](../articles/virtual-network/virtual-machine-network-throughput.md).

  Les limites supérieures ne sont pas garanties. Les limites permettent de sélectionner le type de machine virtuelle approprié pour l’application prévue. Les performances réseau réelles dépendent de nombreux facteurs, notamment la congestion du réseau, les charges de l’application, ainsi que les paramètres réseau. Pour plus d’informations sur l’optimisation du débit du réseau, consultez [Optimiser le débit du réseau pour les machines virtuelles Azure](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Pour atteindre la performance réseau attendue sous Linux ou Windows, il peut être nécessaire de sélectionner une version spécifique ou d’optimiser votre machine virtuelle. Pour plus d’informations, consultez [Test de bande passante/débit (NTTTCP)](../articles/virtual-network/virtual-network-bandwidth-testing.md).



