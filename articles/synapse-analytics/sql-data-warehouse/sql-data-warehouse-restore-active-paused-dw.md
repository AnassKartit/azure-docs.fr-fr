---
title: Restaurer un pool SQL dédié existant dans Azure Synapse Analytics
description: Guide pratique de restauration d’un pool SQL dédié existant dans Azure Synapse Analytics.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 08/29/2018
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 0c3fd0aee0a70743db721f469d91f269b9764e5e
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94577547"
---
# <a name="restore-an-existing-dedicated-sql-pool-in-azure-synapse-analytics"></a>Restaurer un pool SQL dédié existant dans Azure Synapse Analytics

Dans cet article, vous allez apprendre à restaurer un pool SQL dédié existant dans Azure Synapse Analytics à l’aide du portail Azure et de PowerShell.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

**Vérifiez votre capacité de DTU.** Chaque pool est hébergé par un [serveur SQL logique](../../azure-sql/database/logical-servers.md) (par exemple, myserver.database.windows.net) qui dispose d'un quota de DTU par défaut. Vérifiez que le quota DTU restant sur le serveur est suffisant pour la base de données en cours de restauration. Pour savoir comment calculer la capacité DTU nécessaire ou pour demander davantage de capacité DTU, consultez [Request a DTU quota change](sql-data-warehouse-get-started-create-support-ticket.md)(Demander une modification du quota DTU).

## <a name="before-you-begin"></a>Avant de commencer

1. Assurez-vous d’[installer Azure PowerShell](/powershell/azure/?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).
2. Utilisez un point de restauration existant à partir duquel vous souhaitez effectuer la restauration. Si vous souhaitez créer une nouvelle restauration, consultez [le didacticiel pour créer un nouveau point de restauration défini par l’utilisateur](sql-data-warehouse-restore-points.md).

## <a name="restore-an-existing-dedicated-sql-pool-through-powershell"></a>Restaurer un pool SQL dédié existant via PowerShell

Pour restaurer un pool SQL dédié existant à partir d’un point de restauration, utilisez l’applet de commande PowerShell [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

1. Ouvrez PowerShell.

2. Connectez-vous à votre compte Azure et répertoriez tous les abonnements associés à votre compte.

3. Sélectionnez l’abonnement contenant la base de données à restaurer.

4. Listez les points de restauration pour le pool SQL dédié.

5. Sélectionnez le point de restauration souhaité à l’aide de l’élément RestorePointCreationDate.

6. Restaurez le pool SQL dédié sur le point de restauration souhaité à l’aide de l’applet de commande PowerShell [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).

    1. Pour restaurer le pool SQL dédié sur un autre serveur, veillez à spécifier le nom de ce serveur.  Ce serveur peut également se trouver dans un groupe de ressources et une région différents.
    2. Pour effectuer une restauration sur un autre abonnement, utilisez le bouton « Déplacer » afin de déplacer le serveur vers un autre abonnement.

7. Vérifiez que le pool SQL dédié restauré est en ligne.

8. Une fois la restauration terminée, vous pouvez configurer votre pool SQL dédié récupéré en suivant les instructions de la section [Configurer votre base de données après récupération](../../azure-sql/database/disaster-recovery-guidance.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#configure-your-database-after-recovery).

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
#$TargetResourceGroupName="<YourTargetResourceGroupName>" # uncomment to restore to a different server.
#$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>"  
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Or list all restore points
Get-AzSqlDatabaseRestorePoint -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate "xx/xx/xxxx xx:xx:xx xx"
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Use the following command to restore to a different server
#$TargetResourceGroupName = $Database.ResourceGroupName # for restoring to different server in same resourcegroup 
#$RestoredDatabase = Restore-AzSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $TargetResourceGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

## <a name="restore-an-existing-dedicated-sql-pool-through-the-azure-portal"></a>Restaurer un pool SQL dédié existant via le portail Azure

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
2. Accédez au pool SQL dédié à partir duquel effectuer la restauration.
3. En haut du panneau Vue d’ensemble, sélectionnez **Restaurer**.

    ![ Présentation de la restauration](./media/sql-data-warehouse-restore-active-paused-dw/restoring-01.png)

4. Sélectionnez **Points de restauration automatiques** ou **Points de restauration définis par l’utilisateur**. Si le pool SQL dédié n’a pas de points de restauration automatiques, patientez quelques heures ou créez un point de restauration défini par l’utilisateur avant la restauration. Pour les points de restauration définis par l’utilisateur, sélectionnez un point existant ou créez-en un nouveau. Pour **Serveur**, vous pouvez choisir un serveur dans un groupe de ressources et une région différents ou en créer un nouveau. Après avoir fourni tous les paramètres, cliquez sur **Vérifier + Restaurer**.

    ![Points de restauration automatiques](./media/sql-data-warehouse-restore-active-paused-dw/restoring-11.png)

## <a name="next-steps"></a>Étapes suivantes

- [Restaurer un pool SQL dédié supprimé](sql-data-warehouse-restore-deleted-dw.md)
- [Effectuer une restauration à partir d’un pool SQL dédié de géosauvegarde](sql-data-warehouse-restore-from-geo-backup.md)
