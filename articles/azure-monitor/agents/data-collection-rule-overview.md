---
title: Règles de collecte de données dans Azure Monitor (version préliminaire)
description: Vue d’ensemble des règles de collecte de données (DCR) dans Azure Monitor, notamment leur contenu et leur structure, et comment vous pouvez les créer et les utiliser.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/19/2021
ms.custom: references_region
ms.openlocfilehash: 7dca96e05860bb399435ac95ed81107c268ebc5c
ms.sourcegitcommit: c2f0d789f971e11205df9b4b4647816da6856f5b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122662226"
---
# <a name="data-collection-rules-in-azure-monitor"></a>Règles de collecte de données dans Azure Monitor
Les règles de collecte de données (DCR) définissent les données entrantes dans Azure Monitor et spécifient l’emplacement où ces données doivent être envoyées ou stockées. Cet article fournit une vue d’ensemble des règles de collecte de données, notamment leur contenu et leur structure, et comment vous pouvez les créer et les utiliser.

## <a name="input-sources"></a>Sources d’entrée
Les règles de collecte de données prennent actuellement en charge les sources d’entrée suivantes :

- L’agent Azure Monitor s’exécutant sur des machines virtuelles, des groupes de machines virtuelles identiques et Azure Arc pour serveurs. Consultez [Configurer la collecte de données pour l’agent Azure Monitor (version préliminaire)](../agents/data-collection-rule-azure-monitor-agent.md).



## <a name="components-of-a-data-collection-rule"></a>Composants d’une règle de collecte de données
Une règle de collecte de données inclut les composants suivants.

| Composant | Description |
|:---|:---|
| Sources de données | Source unique de données de surveillance avec son propre format et sa propre méthode d’exposition des données. Le journal des événements Windows, les compteurs de performances et Syslog sont des exemples d’une source de données. Chaque source de données correspond à un type de source de données particulier, comme décrit ci-dessous. |
| Flux | Descripteur unique qui décrit un ensemble de sources de données qui seront transformées et schématisées en un seul type. Chaque source de données nécessite un ou plusieurs flux, et un flux peut être utilisé par plusieurs sources de données. Toutes les sources de données d’un flux partagent un schéma commun. Utilisez plusieurs flux de données, par exemple, lorsque vous souhaitez envoyer une source de données particulière à plusieurs tables dans le même espace de travail Log Analytics. |
| Destinations | Ensemble de destinations où les données doivent être envoyées. L’espace de travail Log Analytics et les métriques Azure Monitor sont des exemples. | 
| Flux de données | Définition des flux à envoyer à des destinations. | 

Les règles de collecte de données sont stockées à l’échelle régionale et sont disponibles dans toutes les régions publiques où Log Analytics est pris en charge. Actuellement, les régions et les clouds pour le secteur public ne sont pas pris en charge.

Le diagramme suivant montre les composants d’une règle de collecte de données et leurs relations

[![Diagramme de DCR](media/data-collection-rule-overview/data-collection-rule-components.png)](media/data-collection-rule-overview/data-collection-rule-components.png#lightbox)

### <a name="data-source-types"></a>Types de sources de données
Chaque source de données a un type de source de données. Chaque type définit un ensemble unique de propriétés qui doivent être spécifiées pour chaque source de données. Les types de sources de données actuellement disponibles sont présentés dans le tableau suivant.

| Type de source de données | Description | 
|:---|:---|
| extension | Sources de données basée sur une extension de machine virtuelle |
| performanceCounters | Compteurs de performances pour Windows et Linux |
| syslog | Événements Syslog sur Linux |
| windowsEventLogs | Journaux d'événements Windows |


## <a name="limits"></a>Limites
Pour connaître les limites qui s’appliquent à chaque règle de collecte des données, consultez [Limites du service Azure Monitor](../service-limits.md#data-collection-rules).

## <a name="data-resiliency-and-high-availability"></a>Résilience et haute disponibilité des données
« Règles de collecte de données » en tant que service est déployé de manière régionale. Une règle est créée et stockée dans la région que vous spécifiez, et est sauvegardée dans la [région jumelée](../../best-practices-availability-paired-regions.md#azure-regional-pairs) au sein de la même zone géographique.  
En outre, le service est déployé dans les 3 [zones de disponibilité](../../availability-zones/az-overview.md#availability-zones) de la région, ce qui en fait un **service redondant dans une zone** qui augmente davantage la haute disponibilité.


**Résidence des données sur une région unique** : La fonctionnalité de préversion permettant le stockage de données client dans une seule région n’est actuellement disponible que dans la région Asie Sud-Est (Singapour) de la zone géographique Asie-Pacifique, et la région Brésil Sud (État de Sao Paulo) de la zone géographique Brésil. La résidence sur une région unique est activée par défaut dans ces régions.

## <a name="create-a-dcr"></a>Créer une DCR
Vous pouvez actuellement utiliser l’une des méthodes suivantes pour créer une DCR :

- [Utilisez le Portail Azure](../agents/data-collection-rule-azure-monitor-agent.md) pour créer une règle de collecte de données et l’associer à une ou plusieurs machines virtuelles.
- Modifiez directement la règle de collecte de données au format JSON et [envoyez-la à l’aide de l’API REST](/rest/api/monitor/datacollectionrules).
- Créez une DCR et des associations avec [Azure CLI](https://github.com/Azure/azure-cli-extensions/blob/master/src/monitor-control-service/README.md).
- Créez une DCR et des associations avec Azure PowerShell.
  - [Get-AzDataCollectionRule](https://github.com/Azure/azure-powershell/blob/master/src/Monitor/Monitor/help/Get-AzDataCollectionRule.md)
  - [New-AzDataCollectionRule](https://github.com/Azure/azure-powershell/blob/master/src/Monitor/Monitor/help/New-AzDataCollectionRule.md)
  - [Set-AzDataCollectionRule](https://github.com/Azure/azure-powershell/blob/master/src/Monitor/Monitor/help/Set-AzDataCollectionRule.md)
  - [Update-AzDataCollectionRule](https://github.com/Azure/azure-powershell/blob/master/src/Monitor/Monitor/help/Update-AzDataCollectionRule.md)
  - [Remove-AzDataCollectionRule](https://github.com/Azure/azure-powershell/blob/master/src/Monitor/Monitor/help/Remove-AzDataCollectionRule.md)
  - [Get-AzDataCollectionRuleAssociation](https://github.com/Azure/azure-powershell/blob/master/src/Monitor/Monitor/help/Get-AzDataCollectionRuleAssociation.md)
  - [New-AzDataCollectionRuleAssociation](https://github.com/Azure/azure-powershell/blob/master/src/Monitor/Monitor/help/New-AzDataCollectionRuleAssociation.md)
  - [Remove-AzDataCollectionRuleAssociation](https://github.com/Azure/azure-powershell/blob/master/src/Monitor/Monitor/help/Remove-AzDataCollectionRuleAssociation.md)

## <a name="sample-data-collection-rule"></a>Exemple de règle de collecte de données
L’exemple de règle de collecte de données ci-dessous concerne les machines virtuelles avec l’agent Azure Monitor et présente les détails suivants :

- Données de performances
  - Collecte des compteurs de processeur, de mémoire, de disque logique et de disque physique spécifiques toutes les 15 secondes et les charge toutes les minutes.
  - Collecte des compteurs de processus spécifiques toutes les 30 secondes et les charge toutes les 5 minutes.
- Événements Windows
  - Collecte des événements de sécurité Windows et les charge toutes les minutes.
  - Collecte des événements d’application et de système Windows et les charge toutes les 5 minutes.
- syslog
  - Collecte des événements de débogage, critiques et d’urgence de l’installation cron.
  - Collecte des événements d’alerte, critiques et d’urgence de l’installation Syslog.
- Destinations
  - Envoie toutes les données à un espace de travail Log Analytics nommé centralWorkspace.

> [!NOTE]
> Pour obtenir une explication des XPaths utilisés pour spécifier la collecte d’événements dans les règles de collecte des données, consultez [Limiter la collecte de données avec des requêtes XPath personnalisées](data-collection-rule-azure-monitor-agent.md#limit-data-collection-with-custom-xpath-queries)


```json
{
    "location": "eastus",
    "properties": {
      "dataSources": {
        "performanceCounters": [
          {
            "name": "cloudTeamCoreCounters",
            "streams": [
              "Microsoft-Perf"
            ],
            "scheduledTransferPeriod": "PT1M",
            "samplingFrequencyInSeconds": 15,
            "counterSpecifiers": [
              "\\Processor(_Total)\\% Processor Time",
              "\\Memory\\Committed Bytes",
              "\\LogicalDisk(_Total)\\Free Megabytes",
              "\\PhysicalDisk(_Total)\\Avg. Disk Queue Length"
            ]
          },
          {
            "name": "appTeamExtraCounters",
            "streams": [
              "Microsoft-Perf"
            ],
            "scheduledTransferPeriod": "PT5M",
            "samplingFrequencyInSeconds": 30,
            "counterSpecifiers": [
              "\\Process(_Total)\\Thread Count"
            ]
          }
        ],
        "windowsEventLogs": [
          {
            "name": "cloudSecurityTeamEvents",
            "streams": [
              "Microsoft-Event"
            ],
            "scheduledTransferPeriod": "PT1M",
            "xPathQueries": [
              "Security!*"
            ]
          },
          {
            "name": "appTeam1AppEvents",
            "streams": [
              "Microsoft-Event"
            ],
            "scheduledTransferPeriod": "PT5M",
            "xPathQueries": [
              "System!*[System[(Level = 1 or Level = 2 or Level = 3)]]",
              "Application!*[System[(Level = 1 or Level = 2 or Level = 3)]]"
            ]
          }
        ],
        "syslog": [
          {
            "name": "cronSyslog",
            "streams": [
              "Microsoft-Syslog"
            ],
            "facilityNames": [
              "cron"
            ],
            "logLevels": [
              "Debug",
              "Critical",
              "Emergency"
            ]
          },
          {
            "name": "syslogBase",
            "streams": [
              "Microsoft-Syslog"
            ],
            "facilityNames": [
              "syslog"
            ],
            "logLevels": [
              "Alert",
              "Critical",
              "Emergency"
            ]
          }
        ]
      },
      "destinations": {
        "logAnalytics": [
          {
            "workspaceResourceId": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.OperationalInsights/workspaces/my-workspace",
            "name": "centralWorkspace"
          }
        ]
      },
      "dataFlows": [
        {
          "streams": [
            "Microsoft-Perf",
            "Microsoft-Syslog",
            "Microsoft-Event"
          ],
          "destinations": [
            "centralWorkspace"
          ]
        }
      ]
    }
  }
```


## <a name="next-steps"></a>Étapes suivantes

- [Créez une règle de collecte de données](data-collection-rule-azure-monitor-agent.md) et une association à celle-ci à partir d’une machine virtuelle à l’aide de l’agent Azure Monitor.
