---
title: Gérer des journaux de flux NSG - Azure PowerShell
titleSuffix: Azure Network Watcher
description: Cette page explique comment gérer les journaux d’activité des flux de groupe de sécurité réseau dans Azure Network Watcher avec PowerShell
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: damendo
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 8ee289e069aed9f405463bba6392bbffc2dd2b24
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2021
ms.locfileid: "114291907"
---
# <a name="configuring-network-security-group-flow-logs-with-powershell"></a>Configuration des journaux d’activité des flux de groupe de sécurité réseau avec PowerShell

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [Azure CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Les journaux de flux de groupe de sécurité réseau désignent une fonctionnalité de Network Watcher qui vous permet d’afficher des informations sur le trafic IP entrant et sortant d’un groupe de sécurité réseau. Ces journaux de flux sont écrits au format json et affichent les flux entrants et sortants en fonction de règles, de la carte réseau à laquelle le flux s’applique, des informations à 5 tuples sur le flux (adresse IP source/de destination, port source/de destination, protocole), et de l’autorisation ou du refus du trafic.

Vous trouverez [ici](/powershell/module/az.network/#network-watcher) la spécification détaillée de toutes les commandes de journaux de flux NSG pour différentes versions d’AzPowerShell.

> [!NOTE]
> - Les commandes [Get-AzNetworkWatcherFlowLogStatus](/powershell/module/az.network/get-aznetworkwatcherflowlogstatus) et [Set-AzNetworkWatcherConfigFlowLog](/powershell/module/az.network/set-aznetworkwatcherconfigflowlog) utilisées dans ce document, nécessitent une autorisation de « lecteur » supplémentaire dans le groupe de ressources de l’observateur réseau. En outre, ces commandes sont obsolètes et peuvent bientôt être dépréciées.
> - Il est recommandé d’utiliser à la place les nouvelles commandes [Get-AzNetworkWatcherFlowLog](/powershell/module/az.network/get-aznetworkwatcherflowlog) et [Set-AzNetworkWatcherFlowLog](/powershell/module/az.network/set-aznetworkwatcherflowlog).
> - La nouvelle commande [Get-AzNetworkWatcherFlowLog](/powershell/module/az.network/get-aznetworkwatcherflowlog) propose quatre variantes de flexibilité. Si vous utilisez la variante « Emplacement <String> » de cette commande, il se peut qu’une autorisation de « lecteur » supplémentaire dans le groupe de ressources de l’observateur réseau soit requise. Pour les autres variantes, aucune autorisation supplémentaire n’est requise. 

## <a name="register-insights-provider"></a>Inscription du fournisseur Insights

Pour que les journaux de flux puissent correctement fonctionner, le fournisseur **Microsoft.Insights** doit être inscrit. Si vous ne savez pas si le fournisseur **Microsoft.Insights** est inscrit, exécutez le script suivant.

```powershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Insights
```

## <a name="enable-network-security-group-flow-logs-and-traffic-analytics"></a>Activer les journaux de flux de groupe de sécurité réseau et Traffic Analytics

La commande d’activation des journaux de flux est illustrée dans l’exemple suivant :

```powershell
$NW = Get-AzNetworkWatcher -ResourceGroupName NetworkWatcherRg -Name NetworkWatcher_westcentralus
$nsg = Get-AzNetworkSecurityGroup -ResourceGroupName nsgRG -Name nsgName
$storageAccount = Get-AzStorageAccount -ResourceGroupName StorageRG -Name contosostorage123
Get-AzNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id

#Traffic Analytics Parameters
$workspaceResourceId = "/subscriptions/bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb/resourcegroups/trafficanalyticsrg/providers/microsoft.operationalinsights/workspaces/taworkspace"
$workspaceGUID = "cccccccc-cccc-cccc-cccc-cccccccccccc"
$workspaceLocation = "westeurope"

#Configure Version 1 Flow Logs
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 1

#Configure Version 2 Flow Logs, and configure Traffic Analytics
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2

#Configure Version 2 FLow Logs with Traffic Analytics Configured
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2 -EnableTrafficAnalytics -WorkspaceResourceId $workspaceResourceId -WorkspaceGUID $workspaceGUID -WorkspaceLocation $workspaceLocation

#Query Flow Log Status
Get-AzNetworkWatcherFlowLogStatus -NetworkWatcher $NW -TargetResourceId $nsg.Id
```

Le compte de stockage que vous spécifiez ne peut pas avoir de règles réseau configurées qui limitent l’accès réseau pour les services Microsoft ou des réseaux virtuels spécifiques uniquement. Le compte de stockage peut être dans le même abonnement Azure que le groupe de sécurité réseau pour lequel vous activez le journal de flux, ou dans un autre abonnement. Si vous utilisez des abonnements différents, ils doivent tous deux être associés au même locataire Azure Active Directory. Le compte que vous utilisez pour chaque abonnement doit avoir les [autorisations nécessaires](required-rbac-permissions.md).

## <a name="disable-traffic-analytics-and-network-security-group-flow-logs"></a>Désactiver Traffic Analytics et les journaux de flux de groupe de sécurité réseau

L’exemple suivant permet de désactiver l’analytique du trafic et les journaux de flux :

```powershell
#Disable Traffic Analaytics by removing -EnableTrafficAnalytics property
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $true -FormatType Json -FormatVersion 2 -WorkspaceResourceId $workspaceResourceId -WorkspaceGUID $workspaceGUID -WorkspaceLocation $workspaceLocation

#Disable Flow Logging
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $NW -TargetResourceId $nsg.Id -StorageAccountId $storageAccount.Id -EnableFlowLog $false
```

## <a name="download-a-flow-log"></a>Télécharger un journal de flux

L’emplacement de stockage d’un journal de flux est défini au moment de la création. L’Explorateur Stockage Microsoft Azure est un outil très pratique pour accéder à ces journaux de flux enregistrés dans un compte de stockage. Vous pouvez le télécharger ici : https://storageexplorer.com/

Si un compte de stockage est spécifié, les fichiers journaux de flux sont enregistrés dans un compte de stockage à l’emplacement suivant :

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

Pour plus d’informations sur la structure du journal, consultez [Network Security Group Flow log Overview (Présentation de la journalisation des flux de groupe de sécurité réseau)](network-watcher-nsg-flow-logging-overview.md).

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment [visualiser vos journaux d’activité des flux de groupe de sécurité réseau avec Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Découvrez comment [visualiser vos journaux de flux de groupe de sécurité réseau avec des outils open source](network-watcher-visualize-nsg-flow-logs-open-source-tools.md).