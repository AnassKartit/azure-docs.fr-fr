---
title: Script CLI - Configurer la haute disponibilité dans la même zone dans une instance d’Azure Database pour MySQL - Serveur flexible (préversion)
description: Cet exemple de script Azure CLI montre comment configurer la haute disponibilité dans la même zone sur une instance d’Azure Database pour MySQL - Serveur flexible.
author: shreyaaithal
ms.author: shaithal
ms.service: mysql
ms.devlang: azurecli
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.date: 09/15/2021
ms.openlocfilehash: f254cc5b8835bb0430d4c2d3ecd0bd2f7d9fa013
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128701303"
---
# <a name="configure-same-zone-high-availability-in-an-azure-database-for-mysql---flexible-server-preview-using-azure-cli"></a>Configurer la haute disponibilité dans la même zone dans une instance d’Azure Database pour MySQL - Serveur flexible (préversion) avec Azure CLI

Cet exemple de script CLI montre comment configurer et gérer la [haute disponibilité dans la même zone](../concepts-high-availability.md) sur une instance d’Azure Database pour MySQL - Serveur flexible. La haute disponibilité dans la même zone peut uniquement être activée lors de la création d’un serveur flexible. Toutefois, elle peut être désactivée à tout moment. La haute disponibilité dans la même zone est prise en charge uniquement pour les niveaux tarifaires Usage général et Mémoire optimisée.


[!INCLUDE [flexible-server-free-trial-note](../../includes/flexible-server-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment](../../../../includes/azure-cli-prepare-your-environment.md)]

- Cet article nécessite la version 2.0 ou ultérieure d’Azure CLI. Si vous utilisez Azure Cloud Shell, la version la plus récente est déjà installée. 

## <a name="sample-script"></a>Exemple de script

Remplacez les lignes mises en surbrillance dans le script par vos valeurs de variables.

[!code-azurecli-interactive[main](../../../../cli_scripts/mysql/flexible-server/high-availability/same-zone-ha.sh?highlight=7,10-11 "Configure Same-Zone High Availability.")]

## <a name="clean-up-deployment"></a>Nettoyer le déploiement

Une fois que l’exemple de script a été exécuté, vous pouvez utiliser l’extrait de code suivant pour nettoyer les ressources.

[!code-azurecli-interactive[main](../../../../cli_scripts/mysql/flexible-server/high-availability/clean-up-resources.sh?highlight=4 "Clean up resources.")]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes. Chaque commande du tableau renvoie à une documentation spécifique.

| **Commande** | **Remarques** |
|---|---|
|[az group create](/cli/azure/group#az_group_create)|Crée un groupe de ressources dans lequel toutes les ressources sont stockées|
|[az mysql flexible-server create](/cli/azure/mysql/flexible-server#az_mysql_flexible_server_create)|Crée un serveur flexible qui héberge les bases de données.|
|[az mysql flexible-server update](/cli/azure/mysql/flexible-server#az_mysql_flexible_server_update)|Met à jour un serveur flexible.|
|[az mysql flexible-server delete](/cli/azure/mysql/flexible-server#az_mysql_flexible_server_delete)|Supprime un serveur flexible.|
|[az group delete](/cli/azure/group#az_group_delete) | Supprime un groupe de ressources, y compris toutes les ressources imbriquées.|

## <a name="next-steps"></a>Étapes suivantes

- Pour essayer d’autres scripts : [Exemples Azure CLI - Azure Database pour MySQL - Serveur flexible (préversion)](../sample-scripts-azure-cli.md)
- Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](/cli/azure).