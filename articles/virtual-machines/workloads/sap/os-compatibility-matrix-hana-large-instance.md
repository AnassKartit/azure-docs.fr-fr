---
title: Matrice de compatibilité du système d'exploitation pour SAP HANA (Grandes instances) | Microsoft Docs
description: La matrice de compatibilité présente la compatibilité des différentes versions du système d'exploitation avec différents types de matériel (Grandes instances).
services: virtual-machines-linux
documentationcenter: ''
author: Ajayan1008
manager: hrushib
editor: ''
ms.service: virtual-machines-sap
ms.subservice: baremetal-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/18/2021
ms.author: madhukan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ad1eb1428493d8488e803560e0dc421765aab465
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2021
ms.locfileid: "110579443"
---
# <a name="compatible-operating-systems-for-hana-large-instances"></a>Systèmes d'exploitation compatibles pour HANA - Grandes instances

## <a name="hana-large-instance-type-i"></a>HANA - Grandes instances Type I     
  | Système d’exploitation | Disponibilité        | Références (SKU)                                                          |
  |------------------|---------------------|---------------------------------------------------------------|
  | SLES 12 SP2      | Plus proposé | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP3      | Disponible           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP4      | Disponible           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |
  | SLES 12 SP5      | Disponible           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |
  | SLES 15 SP1      | Disponible           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |
  | RHEL 7.6         | Disponible           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |

  
### <a name="persistent-memory-skus"></a>Références SKU PMEM

  | Système d’exploitation | Disponibilité | Références (SKU)                             |
  |------------------|--------------|----------------------------------|
  | SLES 12 SP4      | Disponible    | S224oo, S224om, S224ooo, S224oom |
  
## <a name="hana-large-instance-type-ii"></a>HANA - Grandes instances Type II     
  |  Système d’exploitation       | Disponibilité        | Références (SKU)                                                                     |
  |-------------------------|---------------------|--------------------------------------------------------------------------|
  | SLES 12 SP2             | Plus proposé | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m        |
  | SLES 12 SP3             | Disponible           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m        |
  | SLES 12 SP4             | Disponible           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m        |
  | SLES 12 SP5             | Disponible           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S896m, S960m |
  | SLES 15 SP1             | Disponible           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S896m, S960m |
  | RHEL 7.6                | Disponible           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S896m, S960m |

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus :

- [Références SKU disponibles](hana-available-skus.md)
- [Mise à niveau du système d'exploitation](os-upgrade-hana-large-instance.md)
- [Scénario pris en charge pour des grandes instances HANA](hana-supported-scenario.md)
  