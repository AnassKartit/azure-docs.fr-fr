---
title: Gérer les journaux de flux NSG – API REST Azure
titleSuffix: Azure Network Watcher
description: Cette page explique comment gérer les journaux d’activité des flux de groupe de sécurité réseau dans Azure Network Watcher avec l’API REST
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
ms.openlocfilehash: cede3018f8922c6771f81470a714eed430cd5cdf
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2021
ms.locfileid: "114291874"
---
# <a name="configuring-network-security-group-flow-logs-using-rest-api"></a>Configuration des journaux d’activité des flux de groupe de sécurité réseau avec l’API REST

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [Azure CLI](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Les journaux de flux de groupe de sécurité réseau désignent une fonctionnalité de Network Watcher qui vous permet d’afficher des informations sur le trafic IP entrant et sortant d’un groupe de sécurité réseau. Ces journaux de flux sont écrits au format json et affichent les flux entrants et sortants en fonction de règles, de la carte réseau à laquelle le flux s’applique, des informations à 5 tuples sur le flux (adresse IP source/de destination, port source/de destination, protocole), et de l’autorisation ou du refus du trafic.

## <a name="before-you-begin"></a>Avant de commencer

ARMclient permet d’appeler l’API REST à l’aide de PowerShell. ARMClient est accessible sur le site chocolatey à partir de la page [ARMClient sur Chocolatey](https://chocolatey.org/packages/ARMClient). Vous trouverez [ici](/rest/api/network-watcher/flowlogs) les spécifications détaillées de l’API REST des journaux de flux NSG. 

Ce scénario suppose que vous ayez déjà suivi la procédure décrite dans [Create a Network Watcher (Créer une instance Network Watcher)](network-watcher-create.md) pour créer une instance Network Watcher.

> [!Important]
> Pour les appels de l’API REST de Network Watcher, le nom du groupe de ressources dans l’URI de requête correspond au groupe de ressources qui contient Network Watcher, et non les ressources sur lesquelles vous exécutez les actions de diagnostic.

## <a name="scenario"></a>Scénario

Le scénario décrit dans cet article vous indique comment activer, désactiver et interroger les journaux d’activité des flux à l’aide de l’API REST. Pour plus d’informations sur la journalisation des flux de groupe de sécurité réseau, consultez l’article [Network Security Group flow logging - Overview](network-watcher-nsg-flow-logging-overview.md) (Présentation de la journalisation des flux de groupe de sécurité réseau).

Dans ce scénario, vous allez :

* Activer les journaux d’activité des flux (version 2)
* Désactiver les journaux d’activité des flux
* Interroger l’état des journaux d’activité des flux

## <a name="log-in-with-armclient"></a>Se connecter à ARMClient

Vous connecter à armclient avec vos informations d’identification Azure.

```powershell
armclient login
```

## <a name="register-insights-provider"></a>Inscription du fournisseur Insights

Pour que les journaux de flux puissent correctement fonctionner, le fournisseur **Microsoft.Insights** doit être inscrit. Si vous ne savez pas si le fournisseur **Microsoft.Insights** est inscrit, exécutez le script suivant.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
armclient post "https://management.azure.com//subscriptions/${subscriptionId}/providers/Microsoft.Insights/register?api-version=2016-09-01"
```

## <a name="enable-network-security-group-flow-logs"></a>Activer les journaux d’activité des flux de groupe de sécurité réseau

La commande d’activation des journaux de flux version 2 est illustrée dans l’exemple suivant. Pour la version 1, remplacez le champ « version » par « 1 » :

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'true',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        },
    'format': {
        'type': 'JSON',
        'version': 2
    }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

La réponse renvoyée par l’exemple précédent est la suivante :

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    },
    "format": {
    "type": "JSON",
    "version": 2
    }
  }
}
```
> [!NOTE]
> - L’API [Network Watchers - Définir la configuration des journaux de flux](/rest/api/network-watcher/network-watchers/set-flow-log-configuration) utilisée ci-dessus est ancienne et peut être dépréciée prochainement.
> - Il est recommandé d’utiliser à la place la nouvelle API REST [Journaux de flux - Créer ou mettre à jour](/rest/api/network-watcher/flow-logs/create-or-update).

## <a name="disable-network-security-group-flow-logs"></a>Désactiver les journaux d’activité des flux de groupe de sécurité réseau

Utilisez l’exemple ci-après pour désactiver les journaux d’activité des flux. L’appel est identique à l’activation des journaux d’activité des flux, à l’exception de la définition de la propriété enabled sur la valeur **false**.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$storageId = "/subscriptions/00000000-0000-0000-0000-000000000000/{resourceGroupName/providers/Microsoft.Storage/storageAccounts/{saName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
    'properties': {
    'storageId': '${storageId}',
    'enabled': 'false',
    'retentionPolicy' : {
            days: 5,
            enabled: true
        },
    'format': {
        'type': 'JSON',
        'version': 2
    }
    }
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/configureFlowLog?api-version=2016-12-01" $requestBody
```

La réponse renvoyée par l’exemple précédent est la suivante :

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": false,
    "retentionPolicy": {
      "days": 5,
      "enabled": true
    },
    "format": {
    "type": "JSON",
    "version": 2
    }
  }
}
```

> [!NOTE]
> - L’API [Network Watchers - Définir la configuration des journaux de flux](/rest/api/network-watcher/network-watchers/set-flow-log-configuration) utilisée ci-dessus est ancienne et peut être dépréciée prochainement.
> - Il est recommandé d’utiliser les nouvelles API REST [Journaux de flux - Créer ou mettre à jour](/rest/api/network-watcher/flow-logs/create-or-update) pour désactiver les journaux de flux [Journaux de flux - Supprimer](/rest/api/network-watcher/flow-logs/delete) pour supprimer une ressource de journaux de flux.

## <a name="query-flow-logs"></a>Interroger les journaux d’activité des flux

L’appel REST ci-après interroge l’état des journaux d’activité des flux sur un groupe de sécurité réseau.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000"
$targetUri = "" # example /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName/providers/Microsoft.Network/networkSecurityGroups/{nsgName}"
$resourceGroupName = "NetworkWatcherRG"
$networkWatcherName = "NetworkWatcher_westcentralus"
$requestBody = @"
{
    'targetResourceId': '${targetUri}',
}
"@

armclient post "https://management.azure.com/subscriptions/${subscriptionId}/ResourceGroups/${resourceGroupName}/providers/Microsoft.Network/networkWatchers/${networkWatcherName}/queryFlowLogStatus?api-version=2016-12-01" $requestBody
```

Voici un exemple de la réponse renvoyée :

```json
{
  "targetResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/networkSecurityGroups/{nsgName}",
  "properties": {
    "storageId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{saName}",
    "enabled": true,
   "retentionPolicy": {
      "days": 5,
      "enabled": true
    },
    "format": {
    "type": "JSON",
    "version": 2
    }
  }
}
```

> [!NOTE]
> - L’API [Network Watchers - Obtenir l’état des journaux de flux](/rest/api/network-watcher/network-watchers/get-flow-log-status) utilisée ci-dessus nécessite une autorisation « lecteur » supplémentaire dans le groupe de ressources de Network Watcher. En outre, cette API est ancienne et peut être dépréciée prochainement.
> - Il est recommandé d’utiliser à la place la nouvelle API REST [Journaux de flux - Obtenir](/rest/api/network-watcher/flow-logs/get).

## <a name="download-a-flow-log"></a>Télécharger un journal de flux

L’emplacement de stockage d’un journal de flux est défini au moment de la création. L’Explorateur Stockage Microsoft Azure est un outil très pratique pour accéder à ces journaux de flux enregistrés dans un compte de stockage. Vous pouvez le télécharger ici : https://storageexplorer.com/

Si un compte de stockage est spécifié, les fichiers de capture de paquets sont enregistrés dans un compte de stockage à l’emplacement suivant :

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId=/SUBSCRIPTIONS/{subscriptionID}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/{nsgName}/y={year}/m={month}/d={day}/h={hour}/m=00/macAddress={macAddress}/PT1H.json
```

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment [visualiser vos journaux d’activité des flux de groupe de sécurité réseau avec Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)

Découvrez comment [visualiser vos journaux de flux de groupe de sécurité réseau avec des outils open source](network-watcher-visualize-nsg-flow-logs-open-source-tools.md).