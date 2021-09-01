---
author: DCtheGeek
ms.service: resource-graph
ms.topic: include
ms.date: 08/31/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 74111377405182aa74abf6e537c3b0433f8ed237
ms.sourcegitcommit: 851b75d0936bc7c2f8ada72834cb2d15779aeb69
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2021
ms.locfileid: "123308775"
---
### <a name="get-cost-savings-summary-from-azure-advisor"></a>Obtenir le récapitulatif des économies d’Azure Advisor

Cette requête résume les économies de chaque suggestion d’[Azure Advisor](../../../../articles/advisor/advisor-overview.md).

```kusto
AdvisorResources
| where type == 'microsoft.advisor/recommendations'
| where properties.category == 'Cost'
| extend
    resources = tostring(properties.resourceMetadata.resourceId),
    savings = todouble(properties.extendedProperties.savingsAmount),
    solution = tostring(properties.shortDescription.solution),
    currency = tostring(properties.extendedProperties.savingsCurrency)
| summarize
    dcount(resources),
    bin(sum(savings), 0.01)
    by solution, currency
| project solution, dcount_resources, sum_savings, currency
| order by sum_savings desc
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az graph query -q "AdvisorResources | where type == 'microsoft.advisor/recommendations' | where properties.category == 'Cost' | extend resources = tostring(properties.resourceMetadata.resourceId), savings = todouble(properties.extendedProperties.savingsAmount), solution = tostring(properties.shortDescription.solution), currency = tostring(properties.extendedProperties.savingsCurrency) | summarize dcount(resources), bin(sum(savings), 0.01) by solution, currency | project solution, dcount_resources, sum_savings, currency | order by sum_savings desc"
```

# <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Search-AzGraph -Query "AdvisorResources | where type == 'microsoft.advisor/recommendations' | where properties.category == 'Cost' | extend resources = tostring(properties.resourceMetadata.resourceId), savings = todouble(properties.extendedProperties.savingsAmount), solution = tostring(properties.shortDescription.solution), currency = tostring(properties.extendedProperties.savingsCurrency) | summarize dcount(resources), bin(sum(savings), 0.01) by solution, currency | project solution, dcount_resources, sum_savings, currency | order by sum_savings desc"
```

# <a name="portal"></a>[Portail](#tab/azure-portal)

:::image type="icon" source="../../../../articles/governance/resource-graph/media/resource-graph-small.png"::: Essayez cette requête dans l’Explorateur Azure Resource Graph :

- Portail Azure : <a href="https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/AdvisorResources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.advisor%2frecommendations%27%0a%7c%20where%20properties.category%20%3d%3d%20%27Cost%27%0a%7c%20extend%0a%09resources%20%3d%20tostring(properties.resourceMetadata.resourceId)%2c%0a%09savings%20%3d%20todouble(properties.extendedProperties.savingsAmount)%2c%0a%09solution%20%3d%20tostring(properties.shortDescription.solution)%2c%0a%09currency%20%3d%20tostring(properties.extendedProperties.savingsCurrency)%0a%7c%20summarize%0a%09dcount(resources)%2c%0a%09bin(sum(savings)%2c%200.01)%0a%09by%20solution%2c%20currency%0a%7c%20project%20solution%2c%20dcount_resources%2c%20sum_savings%2c%20currency%0a%7c%20order%20by%20sum_savings%20desc" target="_blank">portal.azure.com</a>
- Portail Azure Government : <a href="https://portal.azure.us/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/AdvisorResources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.advisor%2frecommendations%27%0a%7c%20where%20properties.category%20%3d%3d%20%27Cost%27%0a%7c%20extend%0a%09resources%20%3d%20tostring(properties.resourceMetadata.resourceId)%2c%0a%09savings%20%3d%20todouble(properties.extendedProperties.savingsAmount)%2c%0a%09solution%20%3d%20tostring(properties.shortDescription.solution)%2c%0a%09currency%20%3d%20tostring(properties.extendedProperties.savingsCurrency)%0a%7c%20summarize%0a%09dcount(resources)%2c%0a%09bin(sum(savings)%2c%200.01)%0a%09by%20solution%2c%20currency%0a%7c%20project%20solution%2c%20dcount_resources%2c%20sum_savings%2c%20currency%0a%7c%20order%20by%20sum_savings%20desc" target="_blank">portal.azure.us</a>
- Portail Azure China 21Vianet : <a href="https://portal.azure.cn/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/AdvisorResources%0a%7c%20where%20type%20%3d%3d%20%27microsoft.advisor%2frecommendations%27%0a%7c%20where%20properties.category%20%3d%3d%20%27Cost%27%0a%7c%20extend%0a%09resources%20%3d%20tostring(properties.resourceMetadata.resourceId)%2c%0a%09savings%20%3d%20todouble(properties.extendedProperties.savingsAmount)%2c%0a%09solution%20%3d%20tostring(properties.shortDescription.solution)%2c%0a%09currency%20%3d%20tostring(properties.extendedProperties.savingsCurrency)%0a%7c%20summarize%0a%09dcount(resources)%2c%0a%09bin(sum(savings)%2c%200.01)%0a%09by%20solution%2c%20currency%0a%7c%20project%20solution%2c%20dcount_resources%2c%20sum_savings%2c%20currency%0a%7c%20order%20by%20sum_savings%20desc" target="_blank">portal.azure.cn</a>

---

