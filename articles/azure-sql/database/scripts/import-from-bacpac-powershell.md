---
title: 'PowerShell : Importer un fichier BACPAC vers une nouvelle base de données dans Azure SQL Database'
description: Exemple de script Azure PowerShell pour importer un fichier BACPAC dans une base de données dans SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: load & move data, sqldbrb=1, devx-track-azurepowershell
ms.devlang: PowerShell
ms.topic: sample
author: rothja
ms.author: jroth
ms.reviewer: mathoma
ms.date: 05/24/2019
ms.openlocfilehash: cbcaa35441d375a6ba12282ee56428d54bd54217
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/22/2021
ms.locfileid: "114462433"
---
# <a name="use-powershell-to-import-a-bacpac-file-into-a-database-in-sql-database"></a>Utiliser PowerShell pour importer un fichier BACPAC dans une base de données dans SQL Database
[!INCLUDE[appliesto-sqldb](../../includes/appliesto-sqldb.md)]

Cet exemple de script Azure PowerShell importe une base de données d’un fichier BACPAC vers une nouvelle base de données dans SQL Database.  

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]
[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Si vous choisissez d’installer et d’utiliser PowerShell localement, ce tutoriel nécessite Az PowerShell 1.4.0 ou ultérieur. Si vous devez effectuer une mise à niveau, consultez [Installer le module Azure PowerShell](/powershell/azure/install-az-ps). Si vous exécutez PowerShell en local, vous devez également lancer `Connect-AzAccount` pour créer une connexion avec Azure.

## <a name="sample-script"></a>Exemple de script

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-database/import-from-bacpac/import-from-bacpac.ps1?highlight=20-21 "Create SQL Database")]

## <a name="clean-up-deployment"></a>Nettoyer le déploiement

Utilisez la commande suivante pour supprimer le groupe de ressources et toutes les ressources associées.

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | Crée un serveur qui héberge des bases de données et des pools élastiques. |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | Crée une règle de pare-feu de niveau serveur pour un serveur. |
| [New-AzSqlDatabaseImport](/powershell/module/az.sql/new-azsqldatabaseimport) | Importe un fichier .bacpac et crée une base de données sur le serveur. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Supprime un groupe de ressources, y compris toutes les ressources imbriquées. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur Azure PowerShell, consultez la [documentation Azure PowerShell](/powershell/azure/).

Vous trouverez des exemples supplémentaires de scripts SQL Database PowerShell dans [Scripts PowerShell Azure SQL Database](../powershell-script-content-guide.md).
