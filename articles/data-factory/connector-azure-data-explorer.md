---
title: Copier des données depuis ou vers Azure Data Explorer
titleSuffix: Azure Data Factory & Azure Synapse
description: Découvrez comment copier des données vers ou depuis Azure Data Explorer à l’aide d’une activité de copie dans un pipeline Azure Data Factory.
ms.author: susabat
author: ssabat
ms.service: data-factory
ms.subservice: data-movement
ms.topic: conceptual
ms.custom: synapse
ms.date: 08/30/2021
ms.openlocfilehash: 4f3718699e7438b3b45c84eebebbbbf75126d793
ms.sourcegitcommit: 851b75d0936bc7c2f8ada72834cb2d15779aeb69
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2021
ms.locfileid: "123304531"
---
# <a name="copy-data-to-or-from-azure-data-explorer-by-using-azure-data-factory"></a>Copier des données depuis/vers Azure Data Explorer à l’aide d’Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Cet article décrit comment utiliser l’activité de copie dans Azure Data Factory pour copier des données vers ou depuis [Azure Data Explorer](/azure/data-explorer/data-explorer-overview). Il s’appuie sur l’article [Vue d’ensemble de l’activité de copie](copy-activity-overview.md).

>[!TIP]
>Pour l’intégration de Azure Data Factory et Azure Data Explorer en général, consultez [intégrer Azure Data Factory à Azure Data Factory](/azure/data-explorer/data-factory-integration) pour en savoir plus.

## <a name="supported-capabilities"></a>Fonctionnalités prises en charge

Ce connecteur Azure Data Explorer est pris en charge pour les activités suivantes :

- [Activité Copy](copy-activity-overview.md) avec [prise en charge de la matrice source/du récepteur](copy-activity-overview.md)
- [Activité de recherche](control-flow-lookup-activity.md)

Vous pouvez copier des données à partir de tout magasin de données source pris en charge vers Azure Data Explorer. Vous pouvez également copier des données à partir d’Azure Data Explorer vers tout magasin de données récepteur pris en charge. Pour obtenir la liste des magasins de données prises en charge par l'activité de copie en tant que sources ou récepteurs, consultez la table [Magasins de données pris en charge](copy-activity-overview.md#supported-data-stores-and-formats).

>[!NOTE]
>La copie de données vers ou depuis Azure Data Explorer via un magasin de données local à l’aide d’un runtime d’intégration auto-hébergé est prise en charge depuis la version 3.14.

Le connecteur Azure Data Explorer vous permet d’effectuer les opérations suivantes :

* Copier des données en utilisant une authentification de jeton d’application Azure Active Directory (Azure AD) avec un **principal du service**.
* En tant que source, récupérer des données à l’aide d’une requête KQL (Kusto).
* En tant que récepteur, ajouter des données à une table de destination.

## <a name="getting-started"></a>Prise en main

>[!TIP]
>Pour obtenir une description de l’utilisation du connecteur Azure Data Explorer, consultez [Copier des données vers ou depuis Azure Data Explorer à l’aide d’Azure Data Factory](/azure/data-explorer/data-factory-load-data) et [Copie en bloc depuis une base de données vers Azure Data Explorer](/azure/data-explorer/data-factory-template).

[!INCLUDE [data-factory-v2-connector-get-started](includes/data-factory-v2-connector-get-started.md)]

## <a name="create-a-linked-service-to-azure-data-explorer-using-ui"></a>Créer un service lié à Azure Data Explorateur à l’aide de l’interface utilisateur

Utilisez les étapes suivantes pour créer un service lié à Azure Data Explorer dans l’interface utilisateur du portail Azure.

1. Accédez à l’onglet Gérer dans votre espace de travail Azure Data Factory ou Synapse, sélectionnez Services liés, puis cliquez sur Nouveau :

    # <a name="azure-data-factory"></a>[Azure Data Factory](#tab/data-factory).

    :::image type="content" source="media/doc-common-process/new-linked-service.png" alt-text="Capture d’écran de la création d’un service lié avec l’interface utilisateur Azure Data Factory.":::

    # <a name="azure-synapse"></a>[Azure Synapse](#tab/synapse-analytics)

    :::image type="content" source="media/doc-common-process/new-linked-service-synapse.png" alt-text="Capture d’écran de la création d’un service lié avec l’interface utilisateur Azure Synapse.":::

2. Recherchez Explorer et sélectionnez le connecteur Azure Data Explorer (Kusto).

    :::image type="content" source="media/connector-azure-data-explorer/azure-data-explorer-connector.png" alt-text="Capture d’écran du connecteur Azure Data Explorer (Kusto).":::    

1. Configurez les informations du service, testez la connexion et créez le nouveau service lié.

    :::image type="content" source="media/connector-azure-data-explorer/configure-azure-data-explorer-linked-service.png" alt-text="Capture d’écran de la configuration du service lié pour Azure Data Explorer.":::

## <a name="connector-configuration-details"></a>Informations de configuration du connecteur

Les sections suivantes fournissent des informations détaillées sur les propriétés utilisées pour définir des entités spécifiques au connecteur Azure Data Explorer.

## <a name="linked-service-properties"></a>Propriétés du service lié

Le connecteur Azure Data Explorer prend en charge les types d’authentification suivants. Consultez les sections correspondantes pour plus d’informations :

- [Authentification d’un principal du service](#service-principal-authentication)
- [Authentification d’identité managée affectée par le système](#managed-identity)
- [Authentification d’identité managée affectée par l’utilisateur](#user-assigned-managed-identity-authentication)

### <a name="service-principal-authentication"></a>Authentification d’un principal du service

Pour utiliser l’authentification du principal du service, procédez comme suit pour obtenir un principal du service et accorder des autorisations :

1. Inscrivez une entité d’application dans Azure Active Directory en suivant les étapes décrites dans [Inscrire votre application à un locataire Azure AD](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Prenez note des valeurs suivantes, qui vous permettent de définir le service lié :

    - ID de l'application
    - Clé de l'application
    - ID client

2. Accordez les autorisations qui conviennent au principal de service dans Azure Data Explorer. Consultez [Gérer les autorisations de base de données d’Azure Data Explorer](/azure/data-explorer/manage-database-permissions) pour obtenir des informations détaillées sur les rôles et les autorisations, ainsi que la gestion des autorisations. En règle générale, vous devez :

    - **En tant que source**, accorder au moins le rôle **Observateur de base de données** à votre base de données
    - **En tant que récepteur**, accorder au moins le rôle **Ingéreur de base de données** à votre base de données

>[!NOTE]
>Lorsque vous utilisez l’interface utilisateur Data Factory pour créer, votre compte d’utilisateur de connexion est utilisé par défaut pour répertorier les clusters, bases de données et tables Azure Data Explorer. Vous pouvez choisir de répertorier les objets à l’aide du principal de service en cliquant sur la liste déroulante à côté du bouton actualiser, ou en entrant manuellement le nom si vous n’êtes pas autorisé à effectuer ces opérations.

Les propriétés suivantes sont prises en charge pour le service lié Azure Data Explorer :

| Propriété | Description | Obligatoire |
|:--- |:--- |:--- |
| type | La propriété **type** doit être définie sur **AzureDataExplorer**. | Oui |
| endpoint | URL de point de terminaison du cluster Azure Data Explorer, avec le format `https://<clusterName>.<regionName>.kusto.windows.net`. | Oui |
| database | Nom de base de données. | Oui |
| tenant | Spécifiez les informations de locataire (nom de domaine ou ID de locataire) dans lesquels se trouve votre application. C’est ce que vous connaissez sous le nom « ID d’autorité » dans la [chaîne de connexion Kusto](/azure/kusto/api/connection-strings/kusto#application-authentication-properties). Récupérez-les en pointant la souris dans le coin supérieur droit du Portail Azure. | Oui |
| servicePrincipalId | Spécifiez l’ID client de l’application. C’est ce que vous connaissez sous le nom « ID d'application cliente AAD » dans la [chaîne de connexion Kusto](/azure/kusto/api/connection-strings/kusto#application-authentication-properties). | Oui |
| servicePrincipalKey | Spécifiez la clé de l’application. C’est ce que vous connaissez sous le nom « Clé d'application cliente AAD » dans la [chaîne de connexion Kusto](/azure/kusto/api/connection-strings/kusto#application-authentication-properties). Marquez ce champ en tant que **SecureString** afin de le stocker en toute sécurité dans Data Factory, ou [référencez les données sécurisées stockées dans Azure Key Vault](store-credentials-in-key-vault.md). | Oui |
| connectVia | Le [runtime d’intégration](concepts-integration-runtime.md) à utiliser pour se connecter à la banque de données. Vous pouvez utiliser le runtime d'intégration Azure ou un runtime d’intégration auto-hébergé si votre banque de données se trouve sur un réseau privé. À défaut de spécification, l’Azure Integration Runtime par défaut est utilisé. |Non |

**Exemple : utilisation de l’authentification de la clé du principal de service**

```json
{
    "name": "AzureDataExplorerLinkedService",
    "properties": {
        "type": "AzureDataExplorer",
        "typeProperties": {
            "endpoint": "https://<clusterName>.<regionName>.kusto.windows.net ",
            "database": "<database name>",
            "tenant": "<tenant name/id e.g. microsoft.onmicrosoft.com>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            }
        }
    }
}
```

### <a name="system-assigned-managed-identity-authentication"></a><a name="managed-identity"></a> Authentification d’identité managée affectée par le système

Pour en savoir plus sur les identités managées pour les ressources Azure, consultez [Identités managées pour les ressources Azure](../active-directory/managed-identities-azure-resources/overview.md).

Pour utiliser l’authentification via une identité managée affectée par le système, procédez comme suit pour accorder les autorisations :

1. [Récupérez les informations d’identité managée de Data Factory](data-factory-service-identity.md#retrieve-managed-identity) en copiant la valeur de l’**ID d’objet d’identité managée** générée en même temps que votre fabrique.

2. Accordez les autorisations qui conviennent à l’identité managée dans Azure Data Explorer. Consultez [Gérer les autorisations de base de données d’Azure Data Explorer](/azure/data-explorer/manage-database-permissions) pour obtenir des informations détaillées sur les rôles et les autorisations, ainsi que la gestion des autorisations. En règle générale, vous devez :

    - **En tant que source**, accorder au moins le rôle **Observateur de base de données** à votre base de données
    - **En tant que récepteur**, accorder au moins le rôle **Ingéreur de base de données** à votre base de données

>[!NOTE]
>Lorsque vous utilisez l’interface utilisateur Data Factory pour créer, votre compte d’utilisateur de connexion est utilisé pour répertorier les clusters, bases de données et tables Azure Data Explorer. Entrez manuellement le nom si vous n’avez pas d’autorisation pour ces opérations.

Les propriétés suivantes sont prises en charge pour le service lié Azure Data Explorer :

| Propriété | Description | Obligatoire |
|:--- |:--- |:--- |
| type | La propriété **type** doit être définie sur **AzureDataExplorer**. | Oui |
| endpoint | URL de point de terminaison du cluster Azure Data Explorer, avec le format `https://<clusterName>.<regionName>.kusto.windows.net`. | Oui |
| database | Nom de base de données. | Oui |
| connectVia | Le [runtime d’intégration](concepts-integration-runtime.md) à utiliser pour se connecter à la banque de données. Vous pouvez utiliser le runtime d'intégration Azure ou un runtime d’intégration auto-hébergé si votre banque de données se trouve sur un réseau privé. À défaut de spécification, l’Azure Integration Runtime par défaut est utilisé. |Non |

**Exemple : utiliser l’authentification via une identité managée affectée par le système**

```json
{
    "name": "AzureDataExplorerLinkedService",
    "properties": {
        "type": "AzureDataExplorer",
        "typeProperties": {
            "endpoint": "https://<clusterName>.<regionName>.kusto.windows.net ",
            "database": "<database name>",
        }
    }
}
```

### <a name="user-assigned-managed-identity-authentication"></a>Authentification d’identité managée affectée par l’utilisateur
Pour en savoir plus sur les identités managées pour les ressources Azure, consultez [Identités managées pour les ressources Azure](../active-directory/managed-identities-azure-resources/overview.md)

Pour utiliser l’authentification par identité managée affectée par l’utilisateur, effectuez les étapes suivantes :

1. [Créez une ou plusieurs identités managées affectées par l’utilisateur](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) et accordez une autorisation dans Azure Data Explorer. Consultez [Gérer les autorisations de base de données d’Azure Data Explorer](/azure/data-explorer/manage-database-permissions) pour obtenir des informations détaillées sur les rôles et les autorisations, ainsi que la gestion des autorisations. En règle générale, vous devez :

    - **En tant que source**, accorder au moins le rôle **Observateur de base de données** à votre base de données
    - **En tant que récepteur**, accorder au moins le rôle **Ingéreur de base de données** à votre base de données
     
2. Attribuez une ou plusieurs identités managées affectées par l’utilisateur à votre fabrique de données et [créez des informations d’identification](data-factory-service-identity.md#credentials) pour chaque identité managée affectée par l’utilisateur.

Les propriétés suivantes sont prises en charge pour le service lié Azure Data Explorer :

| Propriété | Description | Obligatoire |
|:--- |:--- |:--- |
| type | La propriété **type** doit être définie sur **AzureDataExplorer**. | Oui |
| endpoint | URL de point de terminaison du cluster Azure Data Explorer, avec le format `https://<clusterName>.<regionName>.kusto.windows.net`. | Oui |
| database | Nom de base de données. | Oui |
| credentials | Spécifiez l’identité managée affectée par l’utilisateur en tant qu’objet d’informations d’identification. | Oui |
| connectVia | Le [runtime d’intégration](concepts-integration-runtime.md) à utiliser pour se connecter à la banque de données. Vous pouvez utiliser le runtime d'intégration Azure ou un runtime d’intégration auto-hébergé si votre banque de données se trouve sur un réseau privé. À défaut de spécification, l’Azure Integration Runtime par défaut est utilisé. |Non |

**Exemple : utiliser l’authentification via une identité managée affectée par l’utilisateur**
```json
{
    "name": "AzureDataExplorerLinkedService",
    "properties": {
        "type": "AzureDataExplorer",
        "typeProperties": {
            "endpoint": "https://<clusterName>.<regionName>.kusto.windows.net ",
            "database": "<database name>",
            "credential": {
                "referenceName": "credential1",
                "type": "CredentialReference"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propriétés du jeu de données

Pour obtenir la liste complète des sections et propriétés disponibles pour la définition de jeux de données, consultez [Jeux de données dans Azure Data Factory](concepts-datasets-linked-services.md). Cette section répertorie les propriétés prises en charge par le jeu de données Azure Data Explorer.

Pour copier des données vers Azure Data Explorer, définissez la propriété type du jeu de données sur **AzureDataExplorerTable**.

Les propriétés prises en charge sont les suivantes :

| Propriété | Description | Obligatoire |
|:--- |:--- |:--- |
| type | La propriété **type** doit être définie sur **AzureDataExplorerTable**. | Oui |
| table | Nom de la table dans la base à laquelle le service lié fait référence. | Oui pour le récepteur, Non pour la source |

**Exemple de propriétés du jeu de données :**

```json
{
   "name": "AzureDataExplorerDataset",
    "properties": {
        "type": "AzureDataExplorerTable",
        "typeProperties": {
            "table": "<table name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Azure Data Explorer linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriétés de l’activité de copie

Pour obtenir la liste complète des sections et des propriétés disponibles pour la définition des activités, consultez [Pipelines et activités dans Azure Data Factory](concepts-pipelines-activities.md). Cette section fournit la liste des propriétés prises en charge par les sources et récepteurs Azure Data Explorer.

### <a name="azure-data-explorer-as-source"></a>Azure Data Explorer en tant que source

Pour copier des données à partir d’Azure Data Explorer, définissez la propriété **type** dans la source d’activité de copie sur **AzureDataExplorerSource**. Les propriétés prises en charge dans la section **source** de l’activité de copie sont les suivantes :

| Propriété | Description | Obligatoire |
|:--- |:--- |:--- |
| type | La propriété **type** de la source d’activité de copie doit être définie sur : **AzureDataExplorerSource** | Oui |
| query | Requête en lecture seule au [format KQL](/azure/kusto/query/). Utilisez la requête KQL personnalisée en tant que référence. | Oui |
| queryTimeout | Temps d’attente avant l’expiration de la demande de requête. La valeur par défaut est 10 minutes (00:10:00) et la valeur maximale autorisée 1 heure (01:00:00). | Non |
| noTruncation | Indique s’il faut tronquer le jeu de résultats retourné. Par défaut, le résultat est tronqué après 500 000 enregistrements ou au-delà de 64 mégaoctets (Mo). La troncation est vivement recommandée pour garantir le bon comportement de l’activité. |Non |

>[!NOTE]
>Par défaut, la source d’Azure Data Explorer présente une taille limite de 500 000 enregistrements ou 64 Mo. Pour récupérer tous les enregistrements sans troncation, vous pouvez spécifier `set notruncation;` au début de votre requête. Pour plus d'informations, consultez [Limites de requête](/azure/kusto/concepts/querylimits).

**Exemple :**

```json
"activities":[
    {
        "name": "CopyFromAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataExplorerSource",
                "query": "TestTable1 | take 10",
                "queryTimeout": "00:10:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Explorer input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

### <a name="azure-data-explorer-as-sink"></a>Azure Data Explorer en tant que récepteur

Pour copier des données vers Azure Data Explorer, définissez la propriété type dans le récepteur d’activité de copie sur **AzureDataExplorerSink**. Les propriétés prises en charge dans la section **sink** (récepteur) de l’activité de copie sont les suivantes :

| Propriété | Description | Obligatoire |
|:--- |:--- |:--- |
| type | La propriété **type** du récepteur d’activité de copie doit être définie sur : **AzureDataExplorerSink**. | Oui |
| ingestionMappingName | Nom d’un [mappage](/azure/kusto/management/mappings#csv-mapping) créé au préalable sur une table Kusto. Pour mapper les colonnes de la source et Azure Data Explorer, ce qui s’applique à [tous les magasins et formats de source pris en charge](copy-activity-overview.md#supported-data-stores-and-formats), notamment les formats CSV/JSON/Avro, vous pouvez utiliser l’activité de copie [mappage des colonnes](copy-activity-schema-and-type-mapping.md) (implicitement par nom ou explicitement configurée) et/ou les mappages d’Azure Data Explorer. | Non |
| additionalProperties | Conteneur des propriétés qui peut être utilisé pour spécifier l’une des propriétés d’ingestion qui ne sont pas déjà définies par le récepteur Azure Data Explorer. Plus précisément, il peut être utile pour spécifier des balises d’ingestion. Pour en savoir plus, consultez le [document relatif à l’ingestion des données Azure Data Explorer](/azure/data-explorer/ingestion-properties). | Non |

**Exemple :**

```json
"activities":[
    {
        "name": "CopyToAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataExplorerSink",
                "ingestionMappingName": "<optional Azure Data Explorer mapping name>",
                "additionalProperties": {<additional settings for data ingestion>}
            }
        },
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Explorer output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="lookup-activity-properties"></a>Propriétés de l’activité Lookup

Pour plus d’informations sur les propriétés, consultez [Activité de recherche](control-flow-lookup-activity.md).

## <a name="next-steps"></a>Étapes suivantes

* Pour obtenir la liste des magasins de données pris en charge par la copie d'activité dans Azure Data Factory en tant que sources et récepteurs, consultez [Magasins de données pris en charge](copy-activity-overview.md#supported-data-stores-and-formats).

* Apprenez-en davantage sur la [copie de données d’Azure Data Factory vers Azure Data Explorer](/azure/data-explorer/data-factory-load-data).