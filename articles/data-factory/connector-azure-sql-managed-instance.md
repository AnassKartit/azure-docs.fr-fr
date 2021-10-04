---
title: Copier et transformer des données dans Azure SQL Managed Instance
titleSuffix: Azure Data Factory & Azure Synapse
description: Apprenez à copier et à transformer des données dans Azure SQL Managed Instance en utilisant des pipelines Azure Data Factory ou Synapse Analytics.
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.author: jianleishen
author: jianleishen
ms.custom: synapse
ms.date: 09/09/2021
ms.openlocfilehash: 1e56f1ecf0345ccb13e7e5899fb1602dcad666b2
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124744039"
---
# <a name="copy-and-transform-data-in-azure-sql-managed-instance-using-azure-data-factory-or-synapse-analytics"></a>Copier et transformer des données dans Azure SQL Managed Instance en utilisant Azure Data Factory ou Synapse Analytics

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Cet article indique comment utiliser l’activité de copie pour copier des données depuis et vers Azure SQL Managed Instance et utiliser Data Flow pour transformer les données dans Azure SQL Managed Instance. Pour plus d’informations, consultez les articles de présentation pour [Azure Data Factory](introduction.md) et [Synapse Analytics](../synapse-analytics/overview-what-is.md).

## <a name="supported-capabilities"></a>Fonctionnalités prises en charge

Ce connecteur SQL Managed Instance est pris en charge pour les activités suivantes :

- [Activité Copy](copy-activity-overview.md) avec [prise en charge de la matrice source/du récepteur](copy-activity-overview.md)
- [Mappage de flux de données](concepts-data-flow-overview.md)
- [Activité de recherche](control-flow-lookup-activity.md)
- [Activité GetMetadata](control-flow-get-metadata-activity.md)

Pour l’activité de copie, ce connecteur Azure SQL Database prend en charge les fonctions suivantes :

- Copie de données à l’aide de l’authentification SQL et de l’authentification du jeton de l’application Azure Active Directory (Azure AD) avec un principal de service ou l’identité managée pour les ressources Azure.
- En tant que source, la récupération de données à l’aide d’une requête SQL ou d’une procédure stockée. Vous pouvez également choisir de copier en parallèle à partir de la source SQL MI. Pour plus d’informations, consultez la section [Copier en parallèle à partir de SQL MI](#parallel-copy-from-sql-mi).
- En tant que récepteur, la création automatique de la table de destination si elle n’existe pas, en fonction du schéma source, l’ajout de données à une table ou l’appel d’une procédure stockée avec une logique personnalisée pendant la copie.

## <a name="prerequisites"></a>Prérequis

Pour accéder au [point de terminaison public](../azure-sql/managed-instance/public-endpoint-overview.md) SQL Managed Instance, vous pouvez utiliser un runtime d’intégration Azure managé. Assurez-vous que vous activez le terminal public et que vous autorisez également le trafic du terminal public sur le groupe de sécurité réseau afin que le service puisse se connecter à votre base de données. Pour plus d’informations, [consultez cet article](../azure-sql/managed-instance/public-endpoint-configure.md).

Pour accéder au point de terminaison privé SQL Managed Instance, configurez un [runtime d’intégration auto-hébergé](create-self-hosted-integration-runtime.md) pouvant accéder à la base de données. Si vous configurez le runtime d'intégration auto-hébergé dans le même réseau virtuel que votre instance managée, assurez-vous que la machine qui exécute le runtime d'intégration ne se trouve pas dans le même sous-réseau que votre instance managée. Si vous configurez votre runtime d'intégration auto-hébergé dans un réseau virtuel différent de celui de votre instance managée, vous pouvez utiliser un peering de réseau virtuel ou une connexion de réseau virtuel à réseau virtuel. Pour plus d’informations, consultez [Connecter votre application à SQL Managed Instance](../azure-sql/managed-instance/connect-application-instance.md).

## <a name="get-started"></a>Bien démarrer

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-an-azure-sql-managed-instance-using-ui"></a>Créer un service lié à une instance Azure SQL Managed en utilisant l'interface utilisateur.

Suivez les étapes suivantes pour créer un service lié à une instance de SQL Managed dans l'interface utilisateur du portail Azure.

1. Accédez à l’onglet Gérer dans votre espace de travail Azure Data Factory ou Synapse et sélectionnez Services liés, puis cliquez sur Nouveau :

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory).

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Capture d’écran montrant la création d’un service lié avec l’interface utilisateur Azure Data Factory.":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Capture d’écran de la création d’un nouveau service lié avec l’interface utilisateur Azure Synapse.":::

2. Recherchez SQL et sélectionnez le connecteur Managed Instance Azure SQL Server.

    :::image type="content" source="media/connector-azure-sql-managed-instance/azure-sql-managed-instance-connector.png" alt-text="Capture d’écran du connecteur Azure SQL Server Managed Instance.":::    

1. Configurez les informations du service, testez la connexion et créez le nouveau service lié.

    :::image type="content" source="media/connector-azure-sql-managed-instance/configure-azure-sql-managed-instance-linked-service.png" alt-text="Capture d'écran de la configuration du service lié pour une instance de SQL Managed.":::

## <a name="connector-configuration-details"></a>Informations de configuration des connecteurs

Les sections suivantes fournissent des informations détaillées sur les propriétés utilisées pour définir des entités Azure Data Factory spécifiques au connecteur SQL Managed Instance.

## <a name="linked-service-properties"></a>Propriétés du service lié

Les propriétés prises en charge pour le service lié SQL Managed Instance sont les suivantes :

| Propriété | Description | Obligatoire |
|:--- |:--- |:--- |
| type | La propriété type doit être définie sur **AzureSqlMI**. | Oui |
| connectionString |Cette propriété spécifie les informations **connectionString** nécessaires pour se connecter à l’instance managée SQL avec l’authentification SQL. Pour plus d'informations, consultez les exemples suivants. <br/>Le port par défaut est 1433. Si vous utilisez SQL Managed Instance avec un point de terminaison public, spécifiez explicitement le port 3342.<br> Vous pouvez également placer un mot de passe dans Azure Key Vault. En cas d’authentification SQL, extrayez la configuration `password` de la chaîne de connexion. Pour plus d’informations, consultez l’exemple JSON figurant après le tableau et l’article [Stocker des informations d’identification dans Azure Key Vault](store-credentials-in-key-vault.md). |Oui |
| servicePrincipalId | Spécifiez l’ID client de l’application. | Oui, quand vous utilisez l’authentification Azure AD avec le principal de service. |
| servicePrincipalKey | Spécifiez la clé de l’application. Marquez ce champ en tant que **SecureString** afin de le stocker en toute sécurité ou [référencez un secret stocké dans Azure Key Vault](store-credentials-in-key-vault.md). | Oui, quand vous utilisez l’authentification Azure AD avec le principal de service. |
| tenant | Spécifiez les informations de locataire, comme le nom de domaine ou l’ID de locataire, dans lequel votre application se trouve. Récupérez-les en pointant la souris dans le coin supérieur droit du Portail Azure. | Oui, quand vous utilisez l’authentification Azure AD avec le principal de service. |
| azureCloudType | Pour l'authentification du principal de service, spécifiez le type d'environnement cloud Azure auquel votre application Azure AD est inscrite. <br/> Les valeurs autorisées sont **AzurePublic**, **AzureChina**, **AzureUsGovernment** et **AzureGermany**. Par défaut, l’environnement cloud du service est utilisé. | Non |
| alwaysEncryptedSettings | Spécifiez les informations **alwaysencryptedsettings** nécessaires pour permettre à Always Encrypted de protéger les données sensibles stockées dans SQL Server à l’aide d’une identité managée ou d’un principal de service. Pour plus d’informations, consultez l’exemple JSON figurant après le tableau et la section [Utilisation d’Always Encrypted](#using-always-encrypted). S’il n’est pas spécifié, le paramètre par défaut Always Encrypted est désactivé. |Non |
| connectVia | Ce [runtime d'intégration](concepts-integration-runtime.md) permet de se connecter au magasin de données. Vous pouvez utiliser un runtime d'intégration auto-hébergé ou un runtime d'intégration Azure si votre instance gérée possède un terminal public et autorise le service à y accéder. À défaut de spécification, l’Azure Integration Runtime par défaut est utilisé. |Oui |

> [!NOTE]
> SQL Managed Instance [**Always Encrypted**](/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=sql-server-ver15&preserve-view=true) n’est actuellement pas pris en charge dans le flux de données. 

Pour en savoir plus sur les autres types d’authentification, consultez les sections suivantes sur les prérequis et les exemples JSON, respectivement :

- [Authentification SQL](#sql-authentication)
- [Authentification par jeton d’application Azure AD : Principal du service](#service-principal-authentication)
- [Authentification par jeton d’application Azure AD : Identités managées pour les ressources Azure](#managed-identity)

### <a name="sql-authentication"></a>Authentification SQL

**Exemple 1 : utilisez l’authentification SQL**

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemple 2 : utilisez l’authentification SQL avec un mot de passe dans Azure Key Vault**

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;",
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemple 3 : utilisez l’authentification SQL avec Always Encrypted**

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
        },
        "alwaysEncryptedSettings": {
            "alwaysEncryptedAkvAuthType": "ServicePrincipal",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Authentification d’un principal du service

Pour utiliser l’authentification du jeton d’application Azure AD basée sur le principal de service, effectuez les étapes suivantes :

1. Suivez les étapes décrites dans [Approvisionner un administrateur d’Azure Active Directory pour votre instance managée](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-managed-instance).

2. [Créez une application Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal) dans le Portail Azure. Prenez note du nom de l’application et des valeurs suivantes qui définissent le service lié :

    - ID de l'application
    - Clé de l'application
    - ID client

3. [Créez des connexions](/sql/t-sql/statements/create-login-transact-sql) pour le l’identité managée. Dans SSMS (SQL Server Management Studio), connectez-vous à votre instance managée à l’aide d’un compte SQL Server **administrateur système**. Dans la base de données de **référence**, exécutez le code T-SQL suivant :

    ```sql
    CREATE LOGIN [your application name] FROM EXTERNAL PROVIDER
    ```

4. [Créez des utilisateurs de base de données autonome](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) pour l’identité managée. Connectez-vous à la base de données à partir de laquelle ou vers laquelle vous souhaitez copier des données, puis exécutez la commande T-SQL suivante : 
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER
    ```

5. Accordez les autorisations requises par l’identité managée comme vous le feriez d’habitude pour des utilisateurs SQL et autres. Exécutez le code suivant. Pour plus d’options, consultez [ce document](/sql/t-sql/statements/alter-role-transact-sql).

    ```sql
    ALTER ROLE [role name e.g. db_owner] ADD MEMBER [your application name]
    ```

6. Configurez un service lié SQL Managed Instance.

**Exemple : utilisez l’authentification du principal de service**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a> Identités managées pour authentifier les ressources Azure

Un espace de travail Data Factory ou Synapse peut être associé à une [identité managée pour les ressources Azure](data-factory-service-identity.md), laquelle représente le service lors de l’authentification auprès d’autres services Azure. Vous pouvez utiliser cette identité managée pour l’authentification SQL Managed Instance. Le service en question peut accéder à votre base de données et copier des données depuis ou vers celle-ci à l’aide de cette identité.

Pour utiliser l’authentification par identité managée, effectuez les étapes suivantes.

1. Suivez les étapes décrites dans [Approvisionner un administrateur d’Azure Active Directory pour votre instance managée](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-managed-instance).

2. [Créez des connexions](/sql/t-sql/statements/create-login-transact-sql) pour le l’identité managée. Dans SSMS (SQL Server Management Studio), connectez-vous à votre instance managée à l’aide d’un compte SQL Server **administrateur système**. Dans la base de données de **référence**, exécutez le code T-SQL suivant :

    ```sql
    CREATE LOGIN [your_factory_or_workspace_ name] FROM EXTERNAL PROVIDER
    ```

3. [Créez des utilisateurs de base de données autonome](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) pour l’identité managée. Connectez-vous à la base de données à partir de laquelle ou vers laquelle vous souhaitez copier des données, puis exécutez la commande T-SQL suivante : 
  
    ```sql
    CREATE USER [your_factory_or_workspace_name] FROM EXTERNAL PROVIDER
    ```

4. Accordez les autorisations requises par l’identité managée comme vous le feriez d’habitude pour des utilisateurs SQL et autres. Exécutez le code suivant. Pour plus d’options, consultez [ce document](/sql/t-sql/statements/alter-role-transact-sql).

    ```sql
    ALTER ROLE [role name e.g. db_owner] ADD MEMBER [your_factory_or_workspace_name]
    ```

5. Configurez un service lié SQL Managed Instance.

**Exemple : utilise l’authentification d’identité managée**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlMI",
        "typeProperties": {
            "connectionString": "Data Source=<hostname,port>;Initial Catalog=<databasename>;"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propriétés du jeu de données

Pour obtenir la liste complète des sections et propriétés disponibles pour la définition de jeux de données, consultez l'article consacré aux jeux de données. Cette section fournit la liste des propriétés prises en charge par le jeu de données SQL Managed Instance.

Les propriétés suivantes sont prises en charge pour la copie de données vers et à partir d’une instance managée SQL Azure :

| Propriété | Description | Obligatoire |
|:--- |:--- |:--- |
| type | La propriété type du jeu de données doit être définie sur **AzureSqlMITable**. | Oui |
| schéma | Nom du schéma. |Non pour Source, Oui pour Récepteur  |
| table | Nom de la table/vue. |Non pour Source, Oui pour Récepteur  |
| tableName | Nom de la table/vue avec schéma. Cette propriété est prise en charge pour la compatibilité descendante. Pour les nouvelles charges de travail, utilisez `schema` et `table`. | Non pour Source, Oui pour Récepteur |

**Exemple**

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "AzureSqlMITable",
        "linkedServiceName": {
            "referenceName": "<SQL Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "schema": "<schema_name>",
            "table": "<table_name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriétés de l’activité de copie

Pour obtenir la liste complète des sections et des propriétés disponibles pour la définition des activités, consultez l'article [Pipelines](concepts-pipelines-activities.md). Cette section fournit la liste des propriétés prises en charge par la source et le récepteur d’instance managée SQL.

### <a name="sql-managed-instance-as-a-source"></a>Instance managée SQL comme source

>[!TIP]
>Pour savoir comment charger efficacement des données à partir de SQL MI à l’aide du partitionnement, voir [Copier en parallèle à partir de SQL MI](#parallel-copy-from-sql-mi).

Pour la copie de données à partir de l’instance managée SQL, les propriétés suivantes sont prises en charge dans la section source de l’activité de copie :

| Propriété | Description | Obligatoire |
|:--- |:--- |:--- |
| type | La propriété type de la source d’activité de copie doit être définie sur **SqlMISource**. | Oui |
| sqlReaderQuery |Cette propriété utilise la requête SQL personnalisée pour lire les données. par exemple `select * from MyTable`. |Non |
| sqlReaderStoredProcedureName |Cette propriété est le nom de la procédure stockée qui lit les données dans la table source. La dernière instruction SQL doit être une instruction SELECT dans la procédure stockée. |Non |
| storedProcedureParameters |Ces paramètres concernent la procédure stockée.<br/>Les valeurs autorisées sont des paires de noms ou de valeurs. Les noms et la casse des paramètres doivent correspondre aux noms et à la casse des paramètres de la procédure stockée. |Non |
| isolationLevel | Spécifie le comportement de verrouillage des transactions pour la source SQL. Les valeurs autorisées sont les suivantes : **ReadCommitted**, **ReadUncommitted**, **RepeatableRead**, **Serializable**, **Snapshot**. S’il n’est pas spécifié, le niveau d’isolation par défaut de la base de données est utilisé. Pour plus d’informations, consultez [ce document](/dotnet/api/system.data.isolationlevel). | Non |
| partitionOptions | Spécifie les options de partitionnement des données utilisées pour charger des données à partir de SQL MI. <br>Les valeurs autorisées sont les suivantes : **None** (valeur par défaut), **PhysicalPartitionsOfTable** et **DynamicRange**.<br>Lorsqu’une option de partition est activée (donc, autre que `None`), le degré de parallélisme pour charger simultanément des données à partir de SQL MI est contrôlé par le paramètre [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) de l’activité de copie. | Non |
| partitionSettings | Spécifiez le groupe de paramètres pour le partitionnement des données. <br>S’applique lorsque l’option de partitionnement n’est pas `None`. | Non |
| ***Sous `partitionSettings`:*** | | |
| partitionColumnName | Spécifiez le nom de la colonne source **en type entier ou date/DateHeure** (`int`, `smallint`, `bigint`, `date`, `smalldatetime`, `datetime`, `datetime2` ou `datetimeoffset`) qu’utilisera le partitionnement par plages de valeurs pour la copie en parallèle. S’il n’est pas spécifié, l’index ou la clé primaire de la table seront automatiquement détectés et utilisés en tant que colonne de partition.<br>S’applique lorsque l’option de partitionnement est `DynamicRange`. Si vous utilisez une requête pour récupérer des données sources, utilisez `?AdfDynamicRangePartitionCondition ` dans la clause WHERE. Pour obtenir un exemple, consultez la section [Copier en parallèle à partir de la base de données SQL](#parallel-copy-from-sql-mi). | Non |
| partitionUpperBound | Valeur maximale de la colonne de partition pour le fractionnement de la plage de partition. Cette valeur est utilisée pour décider du stride de la partition, et non pour filtrer les lignes de la table. Toutes les lignes de la table ou du résultat de la requête seront partitionnées et copiées. Si la valeur n’est pas spécifiée, l’activité de copie la détecte automatiquement.  <br>S’applique lorsque l’option de partitionnement est `DynamicRange`. Pour obtenir un exemple, consultez la section [Copier en parallèle à partir de la base de données SQL](#parallel-copy-from-sql-mi). | Non |
| partitionLowerBound | Valeur minimale de la colonne de partition pour le fractionnement de la plage de partition. Cette valeur est utilisée pour décider du stride de la partition, et non pour filtrer les lignes de la table. Toutes les lignes de la table ou du résultat de la requête seront partitionnées et copiées. Si la valeur n’est pas spécifiée, l’activité de copie la détecte automatiquement.<br>S’applique lorsque l’option de partitionnement est `DynamicRange`. Pour obtenir un exemple, consultez la section [Copier en parallèle à partir de la base de données SQL](#parallel-copy-from-sql-mi). | Non |

**Notez les points suivants :**

- Si **sqlReaderQuery** est spécifié comme **SqlMISource**, l’activité de copie exécute cette requête sur la source de l’instance managée SQL pour obtenir les données. Vous pouvez également spécifier une procédure stockée en spécifiant **sqlReaderStoredProcedureName** et **storedProcedureParameters** si la procédure stockée accepte des paramètres.
- Lorsque vous utilisez une procédure stockée dans la source pour récupérer des données, sachez que si votre procédure stockée est conçue pour renvoyer un schéma différent quand une valeur de paramètre différente est entrée, vous risquez d’échouer ou d’obtenir un résultat inattendu lors de l’importation d’un schéma à partir de l’interface utilisateur ou lors de la copie de données dans la base de données SQL avec création de table automatique.

**Exemple : Utilisation d'une requête SQL**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlMISource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Exemple : Utilisation d'une procédure stockée**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SQL Managed Instance input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlMISource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

**Définition de la procédure stockée**

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
    select *
    from dbo.UnitTestSrcTable
    where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sql-managed-instance-as-a-sink"></a>Instance managée SQL comme récepteur

> [!TIP]
> Pour en savoir plus sur les bonnes pratiques, les configurations et les comportements d’écriture pris en charge, consultez l’article [Bonnes pratiques de chargement de données dans SQL Managed Instance](#best-practice-for-loading-data-into-sql-managed-instance).

Pour la copie de données vers une instance managée SQL, les propriétés suivantes sont prises en charge dans la section du récepteur d’activité de copie :

| Propriété | Description | Obligatoire |
|:--- |:--- |:--- |
| type | La propriété type du récepteur de l'activité de copie doit être définie sur **SqlMISink**. | Oui |
| preCopyScript |Cette propriété spécifie une requête SQL que l’activité de copie doit exécuter avant l’écriture des données dans l’instance managée SQL. Elle n'est appelée qu'une seule fois par copie. Vous pouvez utiliser cette propriété pour nettoyer des données préchargées. |Non |
| tableOption | Spécifie si [la table du récepteur doit être créée automatiquement](copy-activity-overview.md#auto-create-sink-tables) si elle n’existe pas en fonction du schéma source. La création automatique de la table n’est pas prise en charge quand le récepteur spécifie une procédure stockée. Les valeurs autorisées sont `none` (par défaut) et `autoCreate`. |Non |
| sqlWriterStoredProcedureName | Nom de la procédure stockée qui définit comment appliquer des données sources dans une table cible. <br/>Cette procédure stockée est *appelée par lot*. Pour les opérations qui ne s’exécutent qu’une seule fois et qui n’ont rien à voir avec les données sources (par exemple, supprimer ou tronquer), utilisez la propriété `preCopyScript`.<br>Voir l’exemple dans la section [Appel d’une procédure stockée à partir d’un récepteur SQL](#invoke-a-stored-procedure-from-a-sql-sink). | Non |
| storedProcedureTableTypeParameterName |Nom du paramètre du type de table spécifié dans la procédure stockée.  |Non |
| sqlWriterTableType |Nom du type de table à utiliser dans la procédure stockée. L'activité de copie rend les données déplacées disponibles dans une table temporaire avec ce type de table. Le code de procédure stockée peut ensuite fusionner les données copiées avec les données existantes. |Non |
| storedProcedureParameters |Paramètres de la procédure stockée.<br/>Les valeurs autorisées sont des paires de noms et de valeurs. Les noms et la casse des paramètres doivent correspondre aux noms et à la casse des paramètres de la procédure stockée. | Non |
| writeBatchSize |Nombre de lignes à insérer dans la table SQL *par lot*.<br/>Les valeurs autorisées sont des entiers pour le nombre de lignes. Par défaut, le service détermine de façon dynamique la taille de lot appropriée selon la taille de ligne.  |Non |
| writeBatchTimeout |Cette propriété spécifie le délai d'attente avant expiration de l'opération d'insertion de lot.<br/>Les valeurs autorisées sont celles qui expriment un intervalle de temps. Par exemple, « 00:30:00 » (30 minutes). |Non |
| maxConcurrentConnections |La limite supérieure de connexions simultanées établies au magasin de données pendant l’exécution de l’activité. Spécifiez une valeur uniquement lorsque vous souhaitez limiter les connexions simultanées.| Non |

**Exemple 1 : Ajout de données**

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlMISink",
                "tableOption": "autoCreate",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Exemple 2 : Appeler une procédure stockée pendant la copie**

Pour en savoir plus, consultez [Appel d'une procédure stockée à partir d'un récepteur SQL MI](#invoke-a-stored-procedure-from-a-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<SQL Managed Instance output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlMISink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "storedProcedureTableTypeParameterName": "MyTable",
                "sqlWriterTableType": "MyTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="parallel-copy-from-sql-mi"></a>Copier en parallèle à partir de SQL MI

Le connecteur Azure SQL Managed Instance dans l’activité de copie propose un partitionnement de données intégré pour copier des données en parallèle. Vous trouverez des options de partitionnement de données dans l’onglet **Source** de l’activité de copie.

:::image type="content" source="./media/connector-sql-server/connector-sql-partition-options.png" alt-text="Capture d’écran représentant les options de partition":::

Lorsque vous activez la copie partitionnée, l’activité de copie exécute des requêtes en parallèle sur votre source SQL MI pour charger des données par partitions. Le degré de parallélisme est contrôlé via le paramètre [`parallelCopies`](copy-activity-performance-features.md#parallel-copy) sur l’activité de copie. Par exemple, si vous définissez `parallelCopies` sur la valeur quatre, le service génère et exécute simultanément quatre requêtes selon l’option de partition et les paramètres que vous avez spécifiés, chacune récupérant une partie des données de votre MI SQL.

Il vous est recommandé d’activer la copie en parallèle avec partitionnement des données, notamment lorsque vous chargez une grande quantité de données à partir de SQL MI. Voici quelques suggestions de configurations pour différents scénarios. Lors de la copie de données dans un magasin de données basé sur un fichier, il est recommandé d’écrire les données dans un dossier sous la forme de plusieurs fichiers (spécifiez uniquement le nom du dossier). Les performances seront meilleures qu’avec l’écriture de données dans un seul fichier.

| Scénario                                                     | Paramètres suggérés                                           |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Chargement complet à partir d’une table volumineuse, avec des partitions physiques.        | **Option de partition** : Partitions physiques de la table. <br><br/>Pendant l’exécution, le service détecte automatiquement les partitions physiques et copie les données par partition. <br><br/>Pour vérifier si votre table possède, ou non, une partition physique, vous pouvez vous reporter à [cette requête](#sample-query-to-check-physical-partition). |
| Chargement complet d’une table volumineuse, sans partitions physiques, avec une colonne d’entiers ou DateHeure pour le partitionnement des données. | **Options de partition** : Partition dynamique par spécification de plages de valeurs.<br>**Colonne de partition** (facultatif) : Spécifiez la colonne utilisée pour partitionner les données. Si la valeur n’est pas spécifiée, la colonne de l’index ou de la clé primaire est utilisée.<br/>**Limite supérieure de partition** et **limite inférieure de partition** (facultatif) : Spécifiez si vous souhaitez déterminer le stride de la partition. Cela ne permet pas de filtrer les lignes de la table ; toutes les lignes de la table sont partitionnées et copiées. Si les valeurs ne sont pas spécifiées, l’activité de copie les détecte automatiquement.<br><br>Par exemple, si les valeurs de la colonne de partition « ID » sont comprises entre 1 et 100, et que vous définissez la limite inférieure à 20 et la limite supérieure à 80, avec la copie parallèle sur 4, le service récupère des données par 4 partitions, les ID dans la plage <=20, [21, 50], [51, 80] et >=81, respectivement. |
| Chargement d’une grande quantité de données à l’aide d’une requête personnalisée, sans partitions physiques, et avec une colonne d’entiers ou de date/DateHeure pour le partitionnement des données. | **Options de partition** : Partition dynamique par spécification de plages de valeurs.<br>**Requête**: `SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`.<br>**Colonne de partition** : Spécifiez la colonne utilisée pour partitionner les données.<br>**Limite supérieure de partition** et **limite inférieure de partition** (facultatif) : Spécifiez si vous souhaitez déterminer le stride de la partition. Cela ne permet pas de filtrer les lignes de la table ; toutes les lignes du résultat de la requête sont partitionnées et copiées. Si la valeur n’est pas spécifiée, l’activité de copie la détecte automatiquement.<br><br>Lors de l’exécution, le service remplace `?AdfRangePartitionColumnName` par le nom réel de la colonne et les plages de valeurs de chaque partition et les envoie à votre MI SQL. <br>Par exemple, si les valeurs de la colonne de partition « ID » sont comprises entre 1 et 100, et que vous définissez la limite inférieure à 20 et la limite supérieure à 80, avec la copie parallèle sur 4, le service récupère des données par 4 partitions, les ID dans la plage <=20, [21, 50], [51, 80] et >=81, respectivement. <br><br>Voici d’autres exemples de requêtes pour différents scénarios :<br> 1. Interroger l’ensemble de la table : <br>`SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition`<br> 2. Interroger une table avec une sélection de colonnes et des filtres de la clause WHERE supplémentaires : <br>`SELECT <column_list> FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`<br> 3. Effectuer une requête avec des sous-requêtes : <br>`SELECT <column_list> FROM (<your_sub_query>) AS T WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>`<br> 4. Effectuer une requête avec une partition dans une sous-requête : <br>`SELECT <column_list> FROM (SELECT <your_sub_query_column_list> FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition) AS T`
|

Meilleures pratiques pour charger des données avec l’option de partition :

1. Choisissez une colonne distinctive comme colonne de partition (p. ex. : clé primaire ou clé unique) pour éviter l’asymétrie des données. 
2. Si la table possède une partition intégrée, utilisez l’option de partition « Partitions physiques de la table » pour obtenir de meilleures performances.    
3. Si vous utilisez Azure Integration Runtime pour copier des données, vous pouvez définir des « [unités d’intégration de données (DIU)](copy-activity-performance-features.md#data-integration-units) » plus grandes (>4) pour utiliser davantage de ressources de calcul. Vérifiez les scénarios applicables ici.
4. Le « [degré de parallélisme de copie](copy-activity-performance-features.md#parallel-copy) » contrôle le nombre de partitions : un nombre trop élevé nuit parfois aux performances. Il est recommandé de définir ce nombre selon (DIU ou nombre de nœuds d'IR auto-hébergé) * (2 à 4).

**Exemple : chargement complet à partir d’une table volumineuse, avec des partitions physiques**

```json
"source": {
    "type": "SqlMISource",
    "partitionOption": "PhysicalPartitionsOfTable"
}
```

**Exemple : requête avec partition dynamique par spécification de plages de valeurs**

```json
"source": {
    "type": "SqlMISource",
    "query": "SELECT * FROM <TableName> WHERE ?AdfDynamicRangePartitionCondition AND <your_additional_where_clause>",
    "partitionOption": "DynamicRange",
    "partitionSettings": {
        "partitionColumnName": "<partition_column_name>",
        "partitionUpperBound": "<upper_value_of_partition_column (optional) to decide the partition stride, not as data filter>",
        "partitionLowerBound": "<lower_value_of_partition_column (optional) to decide the partition stride, not as data filter>"
    }
}
```

### <a name="sample-query-to-check-physical-partition"></a>Exemple de requête pour vérifier une partition physique

```sql
SELECT DISTINCT s.name AS SchemaName, t.name AS TableName, pf.name AS PartitionFunctionName, c.name AS ColumnName, iif(pf.name is null, 'no', 'yes') AS HasPartition
FROM sys.tables AS t
LEFT JOIN sys.objects AS o ON t.object_id = o.object_id
LEFT JOIN sys.schemas AS s ON o.schema_id = s.schema_id
LEFT JOIN sys.indexes AS i ON t.object_id = i.object_id 
LEFT JOIN sys.index_columns AS ic ON ic.partition_ordinal > 0 AND ic.index_id = i.index_id AND ic.object_id = t.object_id 
LEFT JOIN sys.columns AS c ON c.object_id = ic.object_id AND c.column_id = ic.column_id 
LEFT JOIN sys.partition_schemes ps ON i.data_space_id = ps.data_space_id 
LEFT JOIN sys.partition_functions pf ON pf.function_id = ps.function_id 
WHERE s.name='[your schema]' AND t.name = '[your table name]'
```

Si la table a une partition physique, vous voyez « HasPartition » avec la valeur « yes » (oui) comme suit.

:::image type="content" source="./media/connector-azure-sql-database/sql-query-result.png" alt-text="Résultat d’une requête SQL":::

## <a name="best-practice-for-loading-data-into-sql-managed-instance"></a>Bonnes pratiques de chargement de données dans une instance managée SQL

Quand vous copiez des données vers une instance managée SQL, vous pouvez avoir besoin d’un comportement d’écriture différent :

- [Append](#append-data): Mes données sources contiennent uniquement de nouveaux enregistrements.
- [Upsert](#upsert-data) : Mes données sources contiennent à la fois des insertions et des mises à jour.
- [Remplacer](#overwrite-the-entire-table) : Je veux recharger la totalité de la table de dimension à chaque fois.
- [Écrire avec une logique personnalisée](#write-data-with-custom-logic) : J’ai besoin d’un traitement supplémentaire avant l’insertion finale dans la table de destination. 

Consultez les sections correspondantes pour savoir comment effectuer des configurations et connaître les bonnes pratiques associées.

### <a name="append-data"></a>Ajout de données

L’ajout de données est le comportement par défaut de ce connecteur de récepteur SQL Managed Instance. Le service effectue une insertion en bloc pour écrire efficacement dans votre table. Vous pouvez configurer la source et le récepteur en conséquence dans l’activité de copie.

### <a name="upsert-data"></a>Effectuer un upsert de données

**Option 1 :** Lorsque vous avez une grande quantité de données à copier, vous pouvez charger en masse tous les enregistrements dans une table de mise en lots à l’aide de l’activité de copie, puis exécuter une activité de procédure stockée pour appliquer une instruction [MERGE](/sql/t-sql/statements/merge-transact-sql) ou INSERT/UPDATE en une seule fois. 

Actuellement, l’activité de copie ne prend pas en charge en mode natif le chargement des données dans une table temporaire de base de données. Il existe une méthode avancée pour le mettre en place en combinant plusieurs activités. Pour en savoir plus à ce sujet, consultez [Optimize Azure SQL Database Bulk Upsert scenarios](https://github.com/scoriani/azuresqlbulkupsert) (Optimisation des scénarios d’upsert en masse Azure SQL Database). Vous trouverez ci-dessous un exemple d’utilisation d’une table permanente comme mise en lots.

Par exemple, vous pouvez créer un pipeline avec une **activité Copy** chaînée avec une **activité de procédure stockée**. La première activité copie des données de votre magasin source vers une table de mise en lots Azure SQL Managed Instance, par exemple **UpsertStagingTable**, comme nom de table dans le jeu de données. La seconde activité appelle ensuite une procédure stockée pour fusionner les données sources de la table de mise en lots dans la table cible et nettoyer la table de mise en lots.

:::image type="content" source="./media/connector-azure-sql-database/azure-sql-database-upsert.png" alt-text="Upsert":::

Dans votre base de données, définissez une procédure stockée avec la logique MERGE, comme dans l’exemple suivant, qui est pointée à partir de l’activité de procédure stockée précédente. Supposons que la cible est la table **Marketing** comportant trois colonnes : **ProfileID**, **State** et **Category**. Effectuez l’opération d’upsert sur la colonne **ProfileID**.

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
    MERGE TargetTable AS target
    USING UpsertStagingTable AS source
    ON (target.[ProfileID] = source.[ProfileID])
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT matched THEN
        INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    
    TRUNCATE TABLE UpsertStagingTable
END
```

**Option 2 :** Vous pouvez choisir d’[appeler une procédure stockée dans l’activité de copie](#invoke-a-stored-procedure-from-a-sql-sink). Cette approche exécute chaque lot (tel que gouverné par la propriété `writeBatchSize`) de la table source au lieu d’utiliser l’insertion en bloc comme approche par défaut dans l’activité de copie.

### <a name="overwrite-the-entire-table"></a>Remplacer l’intégralité de la table

Vous pouvez configurer la propriété **preCopyScript** dans un récepteur d’activité de copie. Dans le cas présent, pour chaque activité Copy qui s’exécute, le service exécute d’abord le script. Il exécute ensuite la copie pour insérer les données. Par exemple, pour remplacer l’intégralité de la table par les données les plus récentes, spécifiez un script pour d’abord supprimer tous les enregistrements avant de charger en bloc les nouvelles données à partir de la source.

### <a name="write-data-with-custom-logic"></a>Écrire des données avec une logique personnalisée

Les étapes permettant d’écrire des données à l’aide d’une logique personnalisée sont semblables à celles décrites dans la section [Effectuer un upsert de données](#upsert-data). Quand vous devez appliquer un traitement supplémentaire avant l’insertion finale des données sources dans la table de destination, vous pouvez effectuer le chargement vers une table de mise en lots, puis appeler une activité de procédure stockée ou appeler une procédure stockée dans le récepteur de l’activité de copie pour appliquer les données.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a><a name="invoke-a-stored-procedure-from-a-sql-sink"></a> Appel d'une procédure stockée à partir d'un récepteur SQL

Quand vous copiez des données dans une instance managée SQL, vous pouvez également configurer et appeler une procédure stockée spécifiée par l’utilisateur avec des paramètres supplémentaires sur chaque lot de la table source. La fonction de procédure stockée tire parti des [paramètres table](/dotnet/framework/data/adonet/sql/table-valued-parameters).

Vous pouvez utiliser une procédure stockée à la place des mécanismes de copie intégrée. Par exemple, quand vous souhaitez appliquer un traitement supplémentaire avant l’insertion finale de données sources dans la table de destination. Fusionner des colonnes, rechercher des valeurs supplémentaires et effectuer des insertions dans plusieurs tables sont des exemples de traitement supplémentaire.

L’exemple suivant montre comment utiliser une procédure stockée pour effectuer une opération upsert simple dans une table de la base de données SQL Server. Supposons que les données d’entrée et la table réceptrice **Marketing** ont trois colonnes : **ProfileID**, **State** et **Category**. Effectuez l’opération upsert basée sur la colonne **ProfileID** et appliquez-la uniquement à une catégorie spécifique appelée « ProductA ».

1. Dans votre base de données, définissez le type de table avec le même nom que **sqlWriterTableType**. Le schéma du type de table doit être identique au schéma retourné par vos données d'entrée.

    ```sql
    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL,
        [Category] [varchar](256) NOT NULL
    )
    ```

2. Dans votre base de données, définissez la procédure stockée portant le même nom que **sqlWriterStoredProcedureName**. Elle gère les données d’entrée à partir de la source que vous avez spécifiée et les fusionne dans la table de sortie. Le nom de paramètre du type de table de la procédure stockée doit être identique au **tableName** défini dans le jeu de données.

    ```sql
    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
    AS
    BEGIN
    MERGE [dbo].[Marketing] AS target
    USING @Marketing AS source
    ON (target.ProfileID = source.ProfileID and target.Category = @category)
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT MATCHED THEN
        INSERT (ProfileID, State, Category)
        VALUES (source.ProfileID, source.State, source.Category);
    END
    ```

3. Dans votre pipeline, définissez la section **Récepteur SQL MI** de l’activité de copie comme suit :

    ```json
    "sink": {
        "type": "SqlMISink",
        "sqlWriterStoredProcedureName": "spOverwriteMarketing",
        "storedProcedureTableTypeParameterName": "Marketing",
        "sqlWriterTableType": "MarketingType",
        "storedProcedureParameters": {
            "category": {
                "value": "ProductA"
            }
        }
    }
    ```

## <a name="mapping-data-flow-properties"></a>Propriétés du mappage de flux de données

Lors de la transformation de données dans le flux de données de mappage, vous pouvez lire et écrire dans des tables à partir d’Azure SQL Managed Instance. Pour plus d’informations, consultez la [transformation de la source](data-flow-source.md) et la [transformation du récepteur](data-flow-sink.md) dans le flux de données de mappage.


### <a name="source-transformation"></a>Transformation de la source

Le tableau ci-dessous répertorie les propriétés prises en charge par une source Azure SQL Managed Instance. Vous pouvez modifier ces propriétés sous l’onglet **Options de la source**.

| Nom | Description | Obligatoire | Valeurs autorisées | Propriété du script de flux de données |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Table de charge de travail | Si vous sélectionnez Table comme entrée, le flux de données extrait toutes les données de la table spécifiée dans le jeu de données. | Non | - |- |
| Requête | Si vous sélectionnez Requête comme entrée, spécifiez une requête SQL pour extraire des données de la source, qui remplace toute table que vous spécifiez dans le jeu de données. L’utilisation de requêtes est un excellent moyen de réduire le nombre de lignes pour les tests ou les recherches.<br><br>La clause **Order By** n’est pas prise en charge, mais vous pouvez définir une instruction SELECT FROM complète. Vous pouvez également utiliser des fonctions de table définies par l’utilisateur. **select * from udfGetData()** est une fonction UDF dans SQL qui retourne une table que vous pouvez utiliser dans le flux de données.<br>Exemple de requête : `Select * from MyTable where customerId > 1000 and customerId < 2000`.| Non | String | query |
| Taille du lot | Spécifiez la taille de lot que doivent avoir les lectures créées à partir d’un large volume de données. | Non | Integer | batchSize |
| Niveau d’isolation | Choisissez l’un des niveaux d’isolation suivants :<br>– Lecture validée.<br>– Lecture non validée (par défaut).<br>– Lecture renouvelable.<br>– Sérialisable.<br>– Aucun (ignorer le niveau d’isolation). | Non | <small>READ_COMMITTED<br/>READ_UNCOMMITTED<br/>REPEATABLE_READ<br/>SERIALIZABLE<br/>NONE</small> |isolationLevel |

#### <a name="azure-sql-managed-instance-source-script-example"></a>Exemple de script de source Azure SQL Managed Instance

Quand vous utilisez Azure SQL Managed Instance comme type de source, le script de flux de données associé est le suivant :

```
source(allowSchemaDrift: true,
    validateSchema: false,
    isolationLevel: 'READ_UNCOMMITTED',
    query: 'select * from MYTABLE',
    format: 'query') ~> SQLMISource
```

### <a name="sink-transformation"></a>Transformation du récepteur

Le tableau ci-dessous répertorie les propriétés prises en charge par un récepteur Azure SQL Managed Instance. Vous pouvez modifier ces propriétés sous l’onglet **Options du récepteur**.

| Name | Description | Obligatoire | Valeurs autorisées | Propriété du script de flux de données |
| ---- | ----------- | -------- | -------------- | ---------------- |
| Mettre à jour la méthode | Spécifiez les opérations autorisées sur la destination de votre base de données. Par défaut, seules les insertions sont autorisées.<br>Pour mettre à jour, effectuer un upsert ou supprimer des lignes, une [transformation de modification de ligne](data-flow-alter-row.md) est requise afin de baliser les lignes relatives à ces actions. | Oui | `true` ou `false` | deletable <br/>insertable <br/>updateable <br/>upsertable |
| Colonnes clés | Pour les mises à jour, les opérations upsert et les suppressions, une ou plusieurs colonnes clés doivent être définies afin de déterminer la ligne à modifier.<br>Le nom de colonne que vous choisissez comme clé sera utilisé dans le cadre des opérations suivantes de mise à jour, d’upsert et de suppression. Vous devez donc choisir une colonne qui existe dans le mappage du récepteur. | Non | Array | clés |
| Ignorer l’écriture des colonnes clés | Si vous ne souhaitez pas écrire la valeur dans la colonne clé, sélectionnez « Ignorer l’écriture des colonnes clés ». | Non | `true` ou `false` | skipKeyWrites |
| Action table |Détermine si toutes les lignes de la table de destination doivent être recréées ou supprimées avant l’écriture.<br>- **Aucun** : Aucune action ne sera effectuée sur la table.<br>- **Recréer** : La table sera supprimée et recréée. Obligatoire en cas de création dynamique d’une nouvelle table.<br>- **Tronquer** : Toutes les lignes de la table cible seront supprimées. | Non | `true` ou `false` | recreate<br/>truncate |
| Taille du lot | Spécifiez le nombre de lignes écrites dans chaque lot. Les plus grandes tailles de lot améliorent la compression et l’optimisation de la mémoire, mais risquent de lever des exceptions de type mémoire insuffisante lors de la mise en cache des données. | Non | Integer | batchSize |
| Pré et post-scripts SQL | Spécifiez des scripts SQL multilignes qui s’exécutent avant (prétraitement) et après (post-traitement) l’écriture de données dans votre base de données de réception. | Non | String | preSQLs<br>postSQLs |

#### <a name="azure-sql-managed-instance-sink-script-example"></a>Exemple de script de récepteur Azure SQL Managed Instance

Quand vous utilisez Azure SQL Managed Instance comme type de récepteur, le script de flux de données associé est le suivant :

```
IncomingStream sink(allowSchemaDrift: true,
    validateSchema: false,
    deletable:false,
    insertable:true,
    updateable:true,
    upsertable:true,
    keys:['keyColumn'],
    format: 'table',
    skipDuplicateMapInputs: true,
    skipDuplicateMapOutputs: true) ~> SQLMISink
```

## <a name="lookup-activity-properties"></a>Propriétés de l’activité Lookup

Pour en savoir plus sur les propriétés, consultez [Activité Lookup](control-flow-lookup-activity.md).

## <a name="getmetadata-activity-properties"></a>Propriétés de l’activité GetMetadata

Pour en savoir plus sur les propriétés, consultez [Activité GetMetadata](control-flow-get-metadata-activity.md). 

## <a name="data-type-mapping-for-sql-managed-instance"></a>Mappage de type de données pour une instance managée SQL

Quand des données sont copiées vers et depuis SQL Managed Instance à l’aide de l’activité de copie, les mappages suivants sont utilisés entre les types de données SQL Managed Instance et les types de données intermédiaires utilisés en interne dans le service. Pour savoir comment l'activité de copie mappe le schéma et le type de données entre la source et le récepteur, consultez [Mappages de schémas et de types de données](copy-activity-schema-and-type-mapping.md).

| Type de données d’une instance managée SQL | Type de données de service intermédiaire |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| Date |DateTime |
| Datetime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| NCHAR |String, Char[] |
| ntext |String, Char[] |
| numeric |Decimal |
| NVARCHAR |String, Char[] |
| real |Unique |
| rowversion |Byte[] |
| smalldatetime |DateTime |
| SMALLINT |Int16 |
| SMALLMONEY |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| TINYINT |Int16 |
| UNIQUEIDENTIFIER |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| Xml |String |

>[!NOTE]
> Pour les types de données mappés avec le type intermédiaire Decimal,l’activité de copie prend actuellement en charge une précision maximale de 28. Si vous disposez de données qui nécessitent une précision supérieure à 28, pensez à les convertir en chaîne dans une requête SQL.

## <a name="using-always-encrypted"></a>Utilisation d’Always Encrypted

Quand vous copiez des données depuis/vers SQL Server avec [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine), effectuez les étapes suivantes : 

1. Stockez la valeur [Clé principale de colonne (CMK)](/sql/relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted?view=sql-server-ver15&preserve-view=true) dans un coffre [Azure Key Vault](../key-vault/general/overview.md). En savoir plus sur la [configuration d’Always Encrypted à l’aide d’Azure Key Vault](../azure-sql/database/always-encrypted-azure-key-vault-configure.md?tabs=azure-powershell)

2. Vérifiez que le coffre de clés où la valeur [Clé principale de colonne (CMK)](/sql/relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted?view=sql-server-ver15&preserve-view=true) est stockée est totalement accessible. Consultez cet [article](/sql/relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted?view=sql-server-ver15&preserve-view=true#key-vaults) pour connaître les autorisations requises.

3. Créez un service lié pour vous connecter à votre base de données SQL et activez la fonction « Always Encrypted » en utilisant une identité managée ou un principal de service. 

>[!NOTE]
>SQL Server [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) prend en charge les scénarios suivants : 
>1. Les magasins de données sources ou récepteurs utilisent l’identité managée ou le principal de service comme type d’authentification du fournisseur de clés.
>2. Les magasins de données sources et récepteurs utilisent l’identité managée comme type d’authentification du fournisseur de clés.
>3. Les magasins de données sources et récepteurs utilisent le même principal de service que le type d’authentification du fournisseur de clés.

## <a name="next-steps"></a>Étapes suivantes
Consultez les [magasins de données pris en charge](copy-activity-overview.md#supported-data-stores-and-formats) pour obtenir la liste des sources et magasins de données pris en charge en tant que récepteurs par l’activité de copie.
