---
title: Flexibilité des tailles de machines virtuelles – Azure Reserved VM Instances
description: Découvrez les gammes de tailles auxquelles s’applique une remise de réservation lorsque vous achetez une instance de machine virtuelle réservée.
ms.service: virtual-machines
ms.subservice: reserved-instances
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 04/06/2021
ms.openlocfilehash: 7b556f485feaa1d6073802aac5aa02ad71e86856
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128599047"
---
# <a name="virtual-machine-size-flexibility-with-reserved-vm-instances"></a>Flexibilité en termes de taille de machine virtuelle avec des instances de machines virtuelles réservées

**S’applique à :** :heavy_check_mark: Machines virtuelles Linux :heavy_check_mark: Machines virtuelles Windows :heavy_check_mark: Groupes identiques flexibles :heavy_check_mark: Groupes identiques uniformes

Quand vous achetez une instance de machine virtuelle réservée, vous pouvez choisir d’optimiser la flexibilité de la taille ou la priorité de capacité de l’instance. Pour plus d’informations sur la définition ou la modification du paramètre d’optimisation pour des instances de machine virtuelle réservées, consultez [Modifier le paramètre d’optimisation pour des instances de machine virtuelle réservées](../cost-management-billing/reservations/manage-reserved-vm-instance.md#change-optimize-setting-for-reserved-vm-instances).

Avec une instance de machine virtuelle réservée optimisée pour la flexibilité de la taille de l’instance, la réservation que vous achetez peut s’appliquer aux tailles de machine virtuelle d’un même groupe de flexibilité de la taille d'instance. Par exemple, si vous achetez une réservation pour une taille de machine virtuelle qui est répertoriée dans la série DSv2, comme Standard_DS3_v2, la remise sur réservation peut s’appliquer aux autres tailles répertoriées dans ce même groupe de flexibilité de la taille d’instance :

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Mais cette remise sur réservation ne s’applique pas aux tailles de machine virtuelle qui figurent dans des groupes de flexibilité de la taille d'instance différents, comme des références SKU dans la table de mémoire haute de la série DSv2 : Standard_DS11_v2, Standard_DS12_v2, etc.

Dans le groupe de flexibilité de la taille d'instance, le nombre de machines virtuelles auxquelles la remise sur réservation s’applique dépend de la taille de machine virtuelle que vous choisissez lorsque vous achetez une réservation. Ce nombre dépend également des tailles des machines virtuelles que vous utilisez. La colonne de ratio compare l’empreinte relative de chaque taille de machine virtuelle de ce groupe de flexibilité de la taille d'instance. Utilisez la valeur de ratio pour calculer la façon dont la remise sur réservation s’applique aux machines virtuelles vous utilisez.

## <a name="examples"></a>Exemples

Les exemples suivants utilisent les tailles et ratios de la table de la série DSv2.

Vous achetez une instance de machine virtuelle réservée avec la taille Standard_DS4_v2, où le ratio ou l’empreinte relative par rapport aux autres tailles de cette série correspond à la valeur 8.

- Scénario 1 : utiliser huit machines virtuelles de taille Standard_DS1_v2 avec un ratio de valeur 1. Votre remise sur réservation s’applique à ces huit machines virtuelles.
- Scénario 2 : utiliser deux machines virtuelles de taille Standard_DS2_v2 avec chacune un ratio de valeur 2. Utiliser également une machine virtuelle de taille Standard_DS3_v2 avec un ratio de valeur 4. Empreinte totale : 2 + 2 + 4 = 8. Votre remise sur réservation s’applique à ces trois machines virtuelles.
- Scénario 3 : utiliser une machine virtuelle de taille Standard_DS5_v2 avec un ratio de valeur 16. Votre remise sur réservation s’applique à la moitié du coût du calcul de cette machine virtuelle.
- Scénario 4 : exécutez un Standard_DS5_v2 avec un ratio de 16 et achetez une réservation Standard_DS4_v2 supplémentaire avec un ratio de 8. Les deux réservations combinent et appliquent la remise à la machine virtuelle entière.

Les sections suivantes présentent les tailles qui figurent dans le même groupe de séries de taille lorsque vous achetez une instance de machine virtuelle réservée optimisée pour la flexibilité de taille d’instance.

## <a name="instance-size-flexibility-ratio-for-vms"></a>Ratio de flexibilité de la taille d’instance pour les machines virtuelles 

Le fichier CSV ci-dessous contient les groupes de flexibilité de la taille d'instance, ArmSkuName et les ratios.  

[Ratios de flexibilité de la taille d'instance](https://isfratio.blob.core.windows.net/isfratio/ISFRatio.csv)

Azure tient à jour le lien et le schémas pour vous permettre d’utiliser le fichier par programme.

## <a name="view-vm-size-recommendations"></a>Afficher les recommandations de taille de machine virtuelle

Azure affiche les recommandations de taille de machine virtuelle dans l’expérience d’achat. Pour afficher les recommandations de taille minimale, sélectionnez **Regrouper par taille minimale**.

:::image type="content" source="./media/reserved-vm-instance-size-flexibility/select-product-recommended-quantity.png" alt-text="Capture d’écran montrant les quantités recommandées." lightbox="./media/reserved-vm-instance-size-flexibility/select-product-recommended-quantity.png" :::

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations, consultez [Qu’est-ce qu’une réservation Azure ?](../cost-management-billing/reservations/save-compute-costs-reservations.md).
