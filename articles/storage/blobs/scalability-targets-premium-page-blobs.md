---
title: Objectifs d’extensibilité pour les comptes de stockage d’objets blob de pages Premium
titleSuffix: Azure Storage
description: Un compte de stockage d’objets blob de pages Premium est optimisé pour les opérations de lecture/écriture. Ce type de compte de stockage stocke un disque non managé pour une machine virtuelle Azure.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 09/24/2021
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: d9d669d583563e81fa55d3626e6505ebe108340c
ms.sourcegitcommit: df2a8281cfdec8e042959339ebe314a0714cdd5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2021
ms.locfileid: "129153460"
---
# <a name="scalability-and-performance-targets-for-premium-page-blob-storage-accounts"></a>Objectifs d’extensibilité et de performances pour les comptes de stockage d’objets blob de pages Premium

[!INCLUDE [storage-scalability-intro-include](../../../includes/storage-scalability-intro-include.md)]

## <a name="scale-targets-for-premium-page-blob-accounts"></a>Objectifs de mise à l’échelle pour les comptes d’objets blob de pages Premium

Un compte de stockage d’objets blob de pages Premium est optimisé pour les opérations de lecture/écriture. Ce type de compte de stockage stocke un disque non managé pour une machine virtuelle Azure.

> [!NOTE]
> Microsoft recommande d’utiliser des disques managés avec des machines virtuelles Azure (VM) si possible. Pour plus d'informations sur les disques managés, consultez [Vue d'ensemble de Stockage sur disque Azure pour les machines virtuelles](../../virtual-machines/managed-disks-overview.md).

Les objectifs d’extensibilité des comptes de stockage d’objets blob de pages Premium sont les suivants :

| Capacité totale des comptes                            | Bande passante totale pour un compte de stockage localement redondant                     |
| ------------------------------------------------- | --------------------------------------------------------------------------- |
| Capacité du disque : 4 To (disque individuel)/35 To (total cumulé de tous les disques) <br>Capacité d’instantané : 10 To<sup>3</sup> | Jusqu’à 50 Go par seconde pour les données entrantes <sup>1</sup> et sortantes <sup>2</sup> |

<sup>1</sup> Toutes les données (demandes) envoyées à un compte de stockage

<sup>2</sup> Toutes les données (réponses) reçues d’un compte de stockage

<sup>3</sup> Le nombre total d’instantanés d’un objet blob de pages individuel peut avoir la valeur 100.

Un compte d’objets blob de pages Premium est un compte à usage général configuré pour des performances Premium. Les comptes de stockage v2 à usage général sont recommandés.

Si vous utilisez des comptes de stockage d’objets blob de page Premium pour des disques non managés et que votre application dépasse les objectifs d’extensibilité d’un compte de stockage unique, Microsoft vous recommande de migrer vers des disques managés. Pour plus d'informations sur les disques managés, consultez [Vue d'ensemble de Stockage sur disque Azure pour les machines virtuelles](../../virtual-machines/managed-disks-overview.md).

Si vous ne pouvez pas migrer vers des disques managés, générez votre application pour utiliser plusieurs comptes de stockage et partitionnez vos données sur ces différents comptes. Par exemple, si vous souhaitez attacher des disques de 51 To sur plusieurs machines virtuelles, répartissez-les entre deux comptes de stockage. La limite pour un compte de stockage Premium unique est de 35 To. Vérifiez qu’un compte de stockage Premium n’a jamais plus de 35 To de disques configurés.

## <a name="see-also"></a>Voir aussi

- [Objectifs d’extensibilité et de performances pour les comptes de stockage standard](../common/scalability-targets-standard-account.md)
- [Objectifs d’extensibilité pour les comptes de stockage d’objets blob de blocs Premium](../blobs/scalability-targets-premium-block-blobs.md)
- [Quotas et limites d’abonnement Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md)
