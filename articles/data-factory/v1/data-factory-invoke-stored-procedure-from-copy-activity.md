---
title: Appeler une procédure stockée à partir d’une activité de copie dans Azure Data Factory
description: Découvrez comment appeler une procédure stockée dans Azure SQL Database ou SQL Server à partir d’une activité de copie Azure Data Factory.
author: linda33wj
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 6f06b84ac0807a37c7adc603a557894be85a4cea
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100374964"
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>Appeler une procédure stockée à partir d’une activité de copie dans Azure Data Factory
> [!NOTE]
> Cet article s’applique à la version 1 de Data Factory. Si vous utilisez la version actuelle du service Data Factory, consultez [Transformer des données à l’aide d’une activité de procédure stockée dans Data Factory](../transform-data-using-stored-procedure.md).


Lorsque vous copiez des données dans [SQL Server](data-factory-sqlserver-connector.md) ou [Azure SQL Database](data-factory-azure-sql-connector.md), vous pouvez configurer l’élément **SqlSink** dans l’activité de copie pour appeler une procédure stockée. Vous pourriez vouloir utiliser la procédure stockée pour effectuer les traitements supplémentaires (fusion de colonnes, recherche de valeurs, insertion dans plusieurs tables, etc.) requis avant d’insérer les données dans la table de destination. Cette fonction tire parti des [paramètres table](/dotnet/framework/data/adonet/sql/table-valued-parameters). 

L’exemple suivant montre comment invoquer une procédure stockée dans une base de données SQL Server à partir d’un pipeline Data Factory (activité de copie) :  

## <a name="output-dataset-json"></a>JSON du jeu de données de sortie
Dans le JSON du jeu de données de sortie, définissez le **type** sur : **SqlServerTable**. Affectez-lui la valeur **AzureSqlTable** pour l’utiliser avec Azure SQL Database. La valeur de la propriété **tableName** doit correspondre au nom du premier paramètre de la procédure stockée.  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a>Section SqlSink dans le JSON de l’activité de copie
Définissez la section **SqlSink** dans le JSON de l’activité de copie comme suit. Pour appeler une procédure stockée lors de l’insertion de données dans la base de données réceptrice ou de destination, spécifiez des valeurs pour les propriétés **SqlWriterStoredProcedureName** et **SqlWriterTableType**. Pour obtenir une description de ces propriétés, consultez la [section SqlSink de l’article sur le connecteur SQL Server](data-factory-sqlserver-connector.md#sqlsink).

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a>Définition de la procédure stockée 
Dans votre base de données, définissez la procédure stockée avec le même nom que **SqlWriterStoredProcedureName**. La procédure stockée gère les données d’entrée provenant du magasin de données source et insère les données dans une table dans la base de données de destination. Le nom du premier paramètre de la procédure stockée doit correspondre au nom de table défini dans le JSON du jeu de données (Marketing).

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>Définition du type de table
Dans votre base de données, définissez le type de table avec le même nom que **SqlWriterTableType**. Le schéma du type de table doit correspondre au schéma du jeu de données d’entrée.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Étapes suivantes
Pour accéder à des exemples JSON complets, consultez les articles suivants sur les connecteurs : 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)