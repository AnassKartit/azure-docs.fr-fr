---
title: Exemples Azure PowerShell pour l’API Core (SQL) Azure Cosmos DB
description: Obtenir les exemples Azure PowerShell pour effectuer des tâches courantes dans Azure Cosmos DB pour l’API Core (SQL)
author: markjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 01/20/2021
ms.author: mjbrown
ms.openlocfilehash: f3e4f38af571f0356cfa92776801784a97225cde
ms.sourcegitcommit: dcf1defb393104f8afc6b707fc748e0ff4c81830
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2021
ms.locfileid: "123117363"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db-core-sql-api"></a>Exemples Azure PowerShell pour l’API Core (SQL) Azure Cosmos DB
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

Le tableau suivant comprend des liens vers des scripts Azure PowerShell couramment utilisés pour Azure Cosmos DB. Utilisez les liens sur la droite pour accéder aux exemples spécifiques aux API. Les exemples communs sont les mêmes pour toutes les API. Des pages d’informations de référence pour toutes les applets de commande PowerShell Azure Cosmos DB sont disponibles dans les [Informations de référence sur Azure PowerShell](/powershell/module/az.cosmosdb). Le module `Az.CosmosDB` fait maintenant partie du module `Az`. [Téléchargez et installez](/powershell/azure/install-az-ps) la dernière version du module Az pour accéder aux applets de commande Azure Cosmos DB. Vous pouvez également récupérer la version la plus récente à partir de [PowerShell Gallery](https://www.powershellgallery.com/packages/Az/5.4.0). Vous pouvez également dupliquer (fork) ces exemples PowerShell pour Cosmos DB à partir de notre dépôt GitHub : [Exemples PowerShell pour Cosmos DB sur GitHub](https://github.com/Azure/azure-docs-powershell-samples/tree/master/cosmosdb).

Pour obtenir des applets de commande PowerShell destinés à d’autres API, consultez [Exemples PowerShell pour Cassandra](../cassandra/powershell-samples.md), [Exemples PowerShell pour l’API MongoDB](../mongodb/powershell-samples.md), [Exemples PowerShell pour Gremlin](../graph/powershell-samples.md), [Exemples PowerShell pour Table](../table/powershell-samples.md)

## <a name="common-samples"></a>Exemples communs

|Tâche | Description |
|---|---|
|[Mettre à jour un compte](../scripts/powershell/common/account-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Mettez à jour le niveau de cohérence par défaut d’un compte Cosmos DB. |
|[Mettre à jour les régions d’un compte](../scripts/powershell/common/update-region.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Mettez à jour les régions d’un compte Cosmos DB. |
|[Modifier la priorité de basculement ou déclencher un basculement](../scripts/powershell/common/failover-priority-update.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Modifiez la priorité de basculement régional d’un compte Azure Cosmos ou déclenchez un basculement manuel. |
|[Clés de compte ou chaînes de connexion](../scripts/powershell/common/keys-connection-strings.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Obtenez des clés primaires et secondaires, des chaînes de connexion, ou regénérez une clé de compte d’un compte Azure Cosmos DB. |
|[Créer un compte Cosmos avec un pare-feu IP](../scripts/powershell/common/firewall-create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Créez un compte Azure Cosmos DB avec le pare-feu IP activé. |
|||

## <a name="core-sql-api-samples"></a>Exemples d’API Core (SQL)

|Tâche | Description |
|---|---|
|[Créer un compte, une base de données et un conteneur](../scripts/powershell/sql/create.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Créez un compte, une base de données et un conteneur Azure Cosmos DB. |
|[Créer un compte, une base de données et un conteneur avec mise à l’échelle automatique](../scripts/powershell/sql/autoscale.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Crée un compte, une base de données et un conteneur Azure Cosmos DB avec mise à l’échelle automatique. |
|[Créer un conteneur avec une grande clé de partition](../scripts/powershell/sql/create-large-partition-key.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Créez un conteneur avec une grande clé de partition. |
|[Créer un conteneur sans stratégie d’index](../scripts/powershell/sql/create-index-none.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Créer un conteneur Azure Cosmos dont la stratégie d’index est désactivée.|
|[Lister ou obtenir des bases de données et des conteneurs](../scripts/powershell/sql/list-get.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Listez ou obtenez des bases de données et des conteneurs. |
|[Opérations de débit](../scripts/powershell/sql/throughput.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Opérations de débit sur une base de données ou un conteneur, comme l’obtention du débit, la mise à jour du débit et la migration entre le débit standard et le débit avec mise à l’échelle automatique. |
|[Verrouiller des ressources contre la suppression](../scripts/powershell/sql/lock.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Empêchez la suppression de ressources à l’aide de verrous de ressources. |
|||
