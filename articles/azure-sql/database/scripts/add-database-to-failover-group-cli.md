---
title: 'L’interface de ligne de commande Azure CLI : Ajouter une base de données à un groupe de basculement'
description: Utilisez l’exemple de script Azure CLI pour créer une base de données dans Azure SQL Database, l’ajouter à un groupe de basculement automatique et tester le basculement.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: sqldbrb=1, devx-track-azurecli
ms.devlang: azurecli
ms.topic: sample
author: BustosMSFT
ms.author: robustos
ms.reviewer: mathoma
ms.date: 07/16/2019
ms.openlocfilehash: fe14d8845b743fae801b8a6d6e0cb2e4a44be7d6
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2021
ms.locfileid: "110694051"
---
# <a name="use-the-azure-cli-to-add-a-database-to-a-failover-group"></a>Utiliser Azure CLI pour ajouter une base de données à un groupe de basculement

[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Cet exemple de script Azure CLI crée une base de données dans Azure SQL Database, crée un groupe de basculement, y ajoute la base de données et teste le basculement.

Si vous choisissez d’installer et d’utiliser l’interface de ligne de commande localement, vous devez exécuter Azure CLI version 2.0 ou une version ultérieure pour poursuivre la procédure décrite dans cet article. Exécutez `az --version` pour trouver la version. Si vous devez effectuer une installation ou une mise à niveau, consultez [Installer Azure CLI]( /cli/azure/install-azure-cli).

## <a name="sample-script"></a>Exemple de script

### <a name="sign-in-to-azure"></a>Connexion à Azure

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

```azurecli-interactive
$subscription = "<subscriptionId>" # add subscription here

az account set -s $subscription # ...or use 'az login'
```

### <a name="run-the-script"></a>Exécuter le script

[!code-azurecli-interactive[main](../../../../cli_scripts/sql-database/failover-groups/add-single-db-to-failover-group-az-cli.sh "Add Azure SQL Database to failover group")]

### <a name="clean-up-deployment"></a>Nettoyer le déploiement

Utilisez la commande suivante pour supprimer le groupe de ressources et toutes les ressources associées.

```azurecli-interactive
az group delete --name $resource
```

## <a name="sample-reference"></a>Informations de référence sur l’exemple

Ce script utilise les commandes suivantes. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Description |
|---|---|
| [az sql db](/cli/azure/sql/db) | Commandes de base de données. |
| [az sql failover-group](/cli/azure/sql/failover-group) | Commandes de groupe de basculement. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](/cli/azure).

Vous trouverez des exemples supplémentaires de scripts CLI SQL Database dans [Documentation Azure SQL Database](../az-cli-script-samples-content-guide.md).
