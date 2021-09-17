---
title: Déplacer des ressources associées à une configuration de maintenance vers une autre région
description: Découvrez comment déplacer des ressources associées à une configuration de maintenance de machine virtuelle vers une autre région Azure.
author: shants123
ms.service: virtual-machines
ms.topic: how-to
ms.date: 03/04/2020
ms.author: shants
ms.openlocfilehash: 59707cfb6e54329017e36914bccf9b565ec9643c
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122690251"
---
# <a name="move-resources-in-a-maintenance-control-configuration-to-another-region"></a>Déplacer des ressources d’une configurations de contrôle de maintenance vers une autre région

**S’applique à :** :heavy_check_mark: Machines virtuelles Linux :heavy_check_mark: Machines virtuelles Windows :heavy_check_mark: Groupes identiques flexibles :heavy_check_mark: Groupes identiques uniformes

Suivez cet article pour déplacer des ressources associées à une configuration de contrôle de maintenance vers une autre région Azure. Vous pouvez envisager de déplacer une configuration pour plusieurs raisons. Par exemple, pour tirer parti d’une nouvelle région, pour déployer des fonctionnalités ou des services disponibles dans une région spécifique, pour répondre à des exigences de stratégie et de gouvernance internes ou pour respecter une planification de capacité.

Le [contrôle de maintenance](maintenance-control.md), avec des configurations de maintenance personnalisées, vous permet de contrôler la façon dont les mises à jour de plateforme sont appliquées aux machines virtuelles et aux hôtes dédiés Azure. Différents scénarios peuvent impliquer le déplacement du contrôle de maintenance entre régions :

- Pour déplacer les ressources associées à une configuration de maintenance, mais pas la configuration elle-même, suivez cet article.
- Pour déplacer votre configuration de contrôle de maintenance, mais pas les ressources associées à la configuration, suivez [ces instructions](move-region-maintenance-configuration.md).
- Pour déplacer la configuration de maintenance et les ressources associées, suivez d’abord [ces instructions](move-region-maintenance-configuration.md). Puis, suivez les instructions de cet article.

## <a name="prerequisites"></a>Prérequis

Avant de commencer à déplacer les ressources associées à une configuration de contrôle de maintenance :

- Assurez-vous d’abord que les ressources que vous déplacez existent dans la nouvelle région.
- Vérifiez les configurations de contrôle de maintenance associées aux machines virtuelles Azure et aux hôtes dédiés Azure que vous souhaitez déplacer. Vérifiez chaque ressource individuellement. Il n’existe actuellement aucun moyen de récupérer des configurations pour plusieurs ressources.
- Quand vous récupérez des configurations pour une ressource :
    - Veillez à utiliser l’ID d’abonnement du compte et non l’ID Azure Dedicated Host.
    - Interface CLI : le paramètre --output table est utilisé uniquement à des fins de lisibilité et peut être supprimé ou modifié.
    - PowerShell : le paramètre Format-Table Name est utilisé uniquement à des fins de lisibilité et peut être supprimé ou modifié.
    - Quand vous utilisez PowerShell, une erreur est générée si vous essayez de lister les configurations pour une ressource à laquelle aucune configuration n’est associée. Cette erreur se présente sous la forme suivante : « L’opération a échoué avec le statut : « Introuvable ». Détails : Erreur du client 404 : Introuvable pour l’URL ».

    
## <a name="prepare-to-move"></a>Préparer le déplacement

1. Avant de commencer, définissez ces variables. Nous avons fourni un exemple pour chacune d’entre elles.

    **Variable** | **Détails** | **Exemple**
    --- | ---
    $subId | ID de l’abonnement contenant les configurations de maintenance | « our-subscription-ID »
    $rsrcGroupName | Nom du groupe de ressources (machine virtuelle Azure) | « VMResourceGroup »
    $vmName | Nom de la ressource de machine virtuelle |  « myVM »
    $adhRsrcGroupName |  Groupe de ressources (hôtes dédiés) | « HostResourceGroup »
    $adh | Nom d’hôte dédié | « myHost »
    $adhParentName | Nom de la ressource parente | « HostGroup »
    
2. Pour récupérer les configurations de maintenance à l’aide de la commande PowerShell [Get-AZConfigurationAssignment](/powershell/module/az.maintenance/get-azconfigurationassignment) :

    - Pour les hôtes dédiés Azure, exécutez :
        ```
        Get-AzConfigurationAssignment -ResourceGroupName $adhRsrcGroupName -ResourceName $adh -ResourceType hosts -ProviderName Microsoft.Compute -ResourceParentName $adhParentName -ResourceParentType hostGroups | Format-Table Name
        ```

    - Pour les machines virtuelles Azure, exécutez :

        ```
        Get-AzConfigurationAssignment -ResourceGroupName $rgName -ResourceName $vmName -ProviderName Microsoft.Compute -ResourceType virtualMachines | Format-Table Name
        ```
3. Pour récupérer les configurations de maintenance à l’aide de la commande CLI [az maintenance assignment](/cli/azure/maintenance/assignment) :

    - Pour les hôtes dédiés Azure :

        ```
        az maintenance assignment list --subscription $subId --resource-group $adhRsrcGroupName --resource-name $adh --resource-type hosts --provider-name Microsoft.Compute --resource-parent-name $adhParentName --resource-parent-type hostGroups --query "[].{HostResourceGroup:resourceGroup,ConfigName:name}" --output table
        ```

    - Pour les machines virtuelles Azure :

        ```
        az maintenance assignment list --subscription $subId --provider-name Microsoft.Compute --resource-group $rsrcGroupName --resource-name $vmName --resource-type virtualMachines --query "[].{HostResourceGroup:resourceGroup, ConfigName:name}" --output table
        ```


## <a name="move"></a>Déplacer 

1. [Suivez ces instructions](../site-recovery/azure-to-azure-tutorial-migrate.md?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json) pour déplacer les machines virtuelles Azure vers la nouvelle région.
2. Quand les ressources ont été déplacées, réappliquez les configurations de maintenance aux ressources dans la nouvelle région en fonction de vos besoins et selon que vous avez déplacé ou non les configurations de maintenance. Vous pouvez appliquer une configuration de maintenance à une ressource à l’aide de [PowerShell](../virtual-machines/maintenance-control-powershell.md) ou de l’[interface CLI](../virtual-machines/maintenance-control-cli.md).


## <a name="verify-the-move"></a>Vérifier le déplacement

Vérifiez les ressources dans la nouvelle région, puis vérifiez les configurations associées aux ressources dans la nouvelle région. 

## <a name="clean-up-source-resources"></a>Nettoyer les ressources sources

Après le déplacement, pensez à supprimer les ressources déplacées de la région source.


## <a name="next-steps"></a>Étapes suivantes

Suivez [ces instructions](move-region-maintenance-configuration.md) si vous devez déplacer des configurations de maintenance. 
