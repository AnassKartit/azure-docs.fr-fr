---
title: Activer Azure Disk Encryption pour les groupes de machines virtuelles identiques
description: Cet article fournit des instructions sur l’activation de Microsoft Azure Disk Encryption pour les groupes de machines virtuelles identiques
author: ju-shim
ms.author: jushiman
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: mimckitt
ms.openlocfilehash: 9e9a0a8b07c300282f26ce12b989a8510ccd1e98
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2021
ms.locfileid: "108738348"
---
# <a name="azure-disk-encryption-for-virtual-machine-scale-sets"></a>Azure Disk Encryption pour les groupes de machines virtuelles identiques

Azure Disk Encryption fournit un chiffrement de volume pour le système d’exploitation et les disques de données de vos machines virtuelles. Cela vous aide à protéger et préserver vos données pour répondre aux engagements de votre entreprise en matière de sécurité et de conformité. Pour en savoir plus, consultez [Azure Disk Encryption : machines virtuelles Linux](../virtual-machines/linux/disk-encryption-overview.md) et [Azure Disk Encryption : machines virtuelles Windows](../virtual-machines/windows/disk-encryption-overview.md)  

Azure Disk Encryption peut également être appliqué aux groupes de machines virtuelles identiques Windows et Linux, dans les cas suivants :
- Groupes identiques créés avec des disques managés. Azure Disk Encryption n’est pas pris en charge pour les groupes identiques de disques natifs (ou non managés).
- Système d’exploitation et volumes de données dans des groupes identiques Windows.
- Volumes de données dans des groupes identiques Linux. Actuellement, le chiffrement de disque de systèmes d’exploitation n’est PAS pris en charge pour les groupes identiques Linux.

Pour découvrir les notions de base d’Azure Disk Encryption pour les groupes de machines virtuelles identiques en quelques minutes seulement, suivez le tutoriel [Chiffrer un groupe de machines virtuelles identiques avec Azure CLI](disk-encryption-cli.md) ou le tutoriel [Chiffrer un groupe de machines virtuelles identiques avec Azure PowerShell](disk-encryption-powershell.md).

## <a name="next-steps"></a>Étapes suivantes

- [Chiffrer un groupe de machines virtuelles identiques à l’aide d’Azure Resource Manager](disk-encryption-azure-resource-manager.md)
- [Créer et configurer un coffre de clés pour Azure Disk Encryption](disk-encryption-key-vault.md)
- [Utiliser Azure Disk Encryption avec le séquencement d’extensions de groupes de machines virtuelles identiques](disk-encryption-extension-sequencing.md)
