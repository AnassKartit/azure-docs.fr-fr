---
title: Transparent Data Encryption (portail) pour pool SQL dédié (anciennement SQL DW)
description: Transparent Data Encryption (TDE) pour pool SQL dédié (anciennement SQL DW) dans Azure Synapse Analytics
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 06/23/2021
ms.author: jrasnick
ms.reviewer: rortloff
ms.custom: seo-lt-2019
ms.openlocfilehash: ddaa7656b5d926b3bf7fdb34548eafe99e9bcf89
ms.sourcegitcommit: 8b38eff08c8743a095635a1765c9c44358340aa8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/30/2021
ms.locfileid: "113092056"
---
# <a name="get-started-with-transparent-data-encryption-tde-for-dedicated-sql-pool-formerly-sql-dw-in-azure-synapse-analytics"></a>Bien démarrer avec Transparent Data Encryption (TDE) pour pool SQL dédié (anciennement SQL DW) dans Azure Synapse Analytics

> [!div class="op_single_selector"]
>
> * [Présentation de la sécurité](sql-data-warehouse-overview-manage-security.md)
> * [Authentification](sql-data-warehouse-authentication.md)
> * [Chiffrement (portail)](sql-data-warehouse-encryption-tde.md)
> * [Chiffrement (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)

> [!NOTE]
> Cet article s’applique à Azure SQL Database, Azure SQL Managed Instance et Azure Synapse Analytics (pools SQL dédiés (anciennement SQL DW)). Pour la documentation sur le chiffrement transparent des données pour les pools SQL dédiés à l’intérieur d’espaces de travail Synapse, consultez [Chiffrement Azure Synapse Analytics](../security/workspaces-encryption.md).

## <a name="required-permissions"></a>Autorisations requises

Pour activer le chiffrement transparent des données (TDE), vous devez être un administrateur ou un membre du rôle dbmanager.

## <a name="enabling-encryption"></a>Activation du chiffrement

Pour activer TDE, procédez comme suit :

1. Ouvrez la base de données dans le [portail Azure](https://portal.azure.com)
2. Dans le panneau de la base de données, cliquez sur le bouton **Paramètres**
3. Sélectionnez l’option **Chiffrement transparent des données** ![paramètres du portail](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png)
4. Sélectionnez le paramètre **Activé** ![paramètres du portail, activé](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png)
5. Sélectionnez **Enregistrer**
   ![paramètres du portail, enregistrer](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png)  

## <a name="disabling-encryption"></a>Désactivation du chiffrement

Pour désactiver TDE, procédez comme suit :

1. Ouvrez la base de données dans le [portail Azure](https://portal.azure.com)
2. Dans le panneau de la base de données, cliquez sur le bouton **Paramètres**
3. Sélectionnez l’option **Chiffrement transparent des données** ![paramètres du portail](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png)
4. Sélectionnez le paramètre **Désactivé** ![paramètres du portail, désactivé](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png)
5. Sélectionnez **Enregistrer**
   ![paramètres du portail, enregistrer 2](./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png)  

## <a name="encryption-dmvs"></a>DMV de chiffrement

Le chiffrement peut être vérifié avec les vues DMV suivantes :

* [sys.databases](/sql/relational-databases/system-catalog-views/sys-databases-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)
* [sys.dm_pdw_nodes_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true)
