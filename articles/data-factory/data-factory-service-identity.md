---
title: Identité managée pour Data Factory
description: En savoir plus sur l’identité managée pour Azure Data Factory.
author: nabhishek
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/25/2021
ms.author: abnarain
ms.openlocfilehash: 632c5930fdae7a4a7504d57b9cb8f285efad8d0a
ms.sourcegitcommit: 1fbd591a67e6422edb6de8fc901ac7063172f49e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2021
ms.locfileid: "109481658"
---
# <a name="managed-identity-for-data-factory"></a>Identité managée pour Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Cet article vous aide à mieux comprendre ce qu’est une identité managée pour Data Factory (anciennement Managed Service Identity) et comment elle fonctionne.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Vue d’ensemble

Lors de la création d’une fabrique de données, une identité managée est créée en même temps que la fabrique. L’identité managée est une application managée qui est inscrite auprès d’Azure Active Directory et qui représente cette fabrique de données.

L’identité managée pour Data Factory bénéficie des fonctionnalités suivantes :

- [Stocker des informations d’identification dans Azure Key Vault](store-credentials-in-key-vault.md), pour laquelle l’identité managée de fabrique de données est utilisée pour l’authentification auprès d’Azure Key Vault.
- Accédez aux magasins de données ou aux calculs à l’aide de l’authentification d’identité managée, y compris le stockage Blob Azure, Azure Data Explorer, Azure Data Lake Storage Gen1, Azure Data Lake Storage Gen2, Azure SQL Database, Azure SQL Managed Instance, Azure Synapse Analytics, REST, une activité Databricks, une activité web, etc. Pour plus d’informations, consultez les articles relatifs au connecteur et à l’activité.

## <a name="generate-managed-identity"></a>Générer une identité managée

L’identité managée pour Data Factory est générée de la façon suivante :

- Lors de la création d’une fabrique de données via le **Portail Azure ou PowerShell**, l’identité managée est toujours créée automatiquement.
- Lors de la création d’une fabrique de données grâce au **kit de développement logiciel (SDK)** , l’identité managée n’est créée que si vous spécifiez « Identity = new FactoryIdentity() » durant la création de l’objet usine. Consultez l’exemple dans [Démarrage rapide .NET - Créer une fabrique de données](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Lors de la création d’une fabrique de données grâce à l’**API REST**, l’identité managée n’est créée que si vous le spécifiez la section « identity » dans le corps de la requête. Consultez l’exemple dans [Démarrage rapide REST - Créer une fabrique de données](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Si vous constatez que votre fabrique de données n’est pas associée à une identité managée après l’instruction [retrieve managed identity](#retrieve-managed-identity), vous pouvez en générer une explicitement par programmation, en mettant à jour la fabrique de données avec l’initiateur d’identité :

- [Générer l’identité managée à l’aide de PowerShell](#generate-managed-identity-using-powershell)
- [Générer l’identité managée à l’aide de REST API](#generate-managed-identity-using-rest-api)
- [Générer l’identité managée avec un modèle Azure Resource Manager](#generate-managed-identity-using-an-azure-resource-manager-template)
- [Générer l’identité managée à l’aide du SDK](#generate-managed-identity-using-sdk)

>[!NOTE]
>- L’identité managée ne peut pas être modifiée. La mise à jour d’une fabrique de données pour laquelle vous disposez déjà d’une identité managée n’a aucun effet ; l’identité managée reste inchangée.
>- Si vous mettez à jour une fabrique de données qui dispose déjà d’une identité managée, sans spécifier le paramètre « identity » dans l’objet fabrique, ou sans spécifier la section « identity » dans le corps de la requête REST, vous recevez un message d’erreur.
>- Lorsque vous supprimez une fabrique de données, l’identité managée associée est également supprimée.

### <a name="generate-managed-identity-using-powershell"></a>Générer l’identité managée à l’aide de PowerShell

Appelez la commande **Set-AzDataFactoryV2**, vous verrez alors les champs « identity » qui viennent d’être générés :

```powershell
PS C:\WINDOWS\system32> Set-AzDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName> -Location <region>

DataFactoryName   : ADFV2DemoFactory
DataFactoryId     : /subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory
ResourceGroupName : <resourceGroupName>
Location          : East US
Tags              : {}
Identity          : Microsoft.Azure.Management.DataFactory.Models.FactoryIdentity
ProvisioningState : Succeeded
```

### <a name="generate-managed-identity-using-rest-api"></a>Générer l’identité managée à l’aide de REST API

Appelez ensuite l’API avec la section« identity » dans le corps de la requête :

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2018-06-01
```

**Corps de la requête** : add "identity": { "type": "SystemAssigned" }.

```json
{
    "name": "<dataFactoryName>",
    "location": "<region>",
    "properties": {},
    "identity": {
        "type": "SystemAssigned"
    }
}
```

**Réponse** : l’identité managée est créée automatiquement, et la section « identity » est remplie en conséquence.

```json
{
    "name": "<dataFactoryName>",
    "tags": {},
    "properties": {
        "provisioningState": "Succeeded",
        "loggingStorageAccountKey": "**********",
        "createTime": "2017-09-26T04:10:01.1135678Z",
        "version": "2018-06-01"
    },
    "identity": {
        "type": "SystemAssigned",
        "principalId": "765ad4ab-XXXX-XXXX-XXXX-51ed985819dc",
        "tenantId": "72f988bf-XXXX-XXXX-XXXX-2d7cd011db47"
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory",
    "type": "Microsoft.DataFactory/factories",
    "location": "<region>"
}
```

### <a name="generate-managed-identity-using-an-azure-resource-manager-template"></a>Générer l’identité managée avec un modèle Azure Resource Manager

**Modèle** : add "identity": { "type": "SystemAssigned" }.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "resources": [{
        "name": "<dataFactoryName>",
        "apiVersion": "2018-06-01",
        "type": "Microsoft.DataFactory/factories",
        "location": "<region>",
        "identity": {
            "type": "SystemAssigned"
        }
    }]
}
```

### <a name="generate-managed-identity-using-sdk"></a>Générer l’identité managée à l’aide du SDK

Appelez la fabrique de données pour créer ou mettre à jour la fonction avec Identity=new FactoryIdentity(). Exemple de code utilisant NET :

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="retrieve-managed-identity"></a>Récupérer une identité managée

Vous pouvez récupérer l’identité managée à partir du portail Azure ou par programmation. Les sections suivantes vous montrent quelques exemples.

>[!TIP]
> Si vous ne voyez pas l’identité managée, [générez l’identité managée](#generate-managed-identity) en mettant à jour votre fabrique.

### <a name="retrieve-managed-identity-using-azure-portal"></a>Récupérer l’identité managée à l’aide du portail Azure

Les informations relatives à l'identité managée sont disponibles sur le portail Azure sous : votre fabrique de données -> Propriétés.

- ID d’objet de l’identité managée
- Locataire de l’identité managée

Les informations relatives à l'identité managée apparaissent également lorsque vous créez un service lié qui prend en charge l'authentification de l'identité managée, comme Azure Blob, Azure Data Lake Storage, Azure Key Vault, etc.

Lors de l’octroi de l’autorisation, sous l’onglet Access Control (IAM) de la ressource Azure -> Ajouter une attribution de rôle -> Affecter l’accès à -> sélectionnez Data Factory sous Identité managée par le système -> sélectionnez par nom de fabrique. En général, vous pouvez utiliser l’ID d’objet ou le nom de la fabrique de données (comme nom d’identité managée) pour trouver cette identité. Si vous avez besoin d’obtenir l’ID d’application de l’identité managée, vous pouvez utiliser PowerShell.

### <a name="retrieve-managed-identity-using-powershell"></a>Récupérer l’identité managée à l’aide de PowerShell

L'ID de principal et l'ID de locataire de l'identité managée seront renvoyés lorsque vous aurez obtenu une fabrique de données spécifique, comme illustré ci-dessous. Utilisez le paramètre **PrincipalId** pour octroyer l'accès :

```powershell
PS C:\WINDOWS\system32> (Get-AzDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Pour obtenir l'ID de l'application, copiez l'ID du principal, puis exécutez la commande Azure Active Directory ci-dessous avec l'ID du principal comme paramètre.

```powershell
PS C:\WINDOWS\system32> Get-AzADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

### <a name="retrieve-managed-identity-using-rest-api"></a>Récupérer l’identité managée à l’aide d’API REST

L'ID de principal et l'ID de locataire de l'identité managée seront renvoyés lorsque vous aurez obtenu une fabrique de données spécifique, comme illustré ci-dessous.

Appeler l’API ci-dessous dans la requête :

```
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DataFactory/factories/{factoryName}?api-version=2018-06-01
```

**Réponse**: Vous obtiendrez une réponse comme indiqué dans l’exemple ci-dessous. La section « identité » est remplie en conséquence.

```json
{
    "name":"<dataFactoryName>",
    "identity":{
        "type":"SystemAssigned",
        "principalId":"554cff9e-XXXX-XXXX-XXXX-90c7d9ff2ead",
        "tenantId":"72f988bf-XXXX-XXXX-XXXX-2d7cd011db47"
    },
    "id":"/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<dataFactoryName>",
    "type":"Microsoft.DataFactory/factories",
    "properties":{
        "provisioningState":"Succeeded",
        "createTime":"2020-02-12T02:22:50.2384387Z",
        "version":"2018-06-01",
        "factoryStatistics":{
            "totalResourceCount":0,
            "maxAllowedResourceCount":0,
            "factorySizeInGbUnits":0,
            "maxAllowedFactorySizeInGbUnits":0
        }
    },
    "eTag":"\"03006b40-XXXX-XXXX-XXXX-5e43617a0000\"",
    "location":"<region>",
    "tags":{

    }
}
```

> [!TIP] 
> Pour récupérer l’identité managée à partir d’un modèle ARM, ajoutez une section **sorties** dans le JSON ARM :

```json
{
    "outputs":{
        "managedIdentityObjectId":{
            "type":"string",
            "value":"[reference(resourceId('Microsoft.DataFactory/factories', parameters('<dataFactoryName>')), '2018-06-01', 'Full').identity.principalId]"
        }
    }
}
```

## <a name="next-steps"></a>Étapes suivantes
Consultez les rubriques suivantes qui expliquent quand et comment utiliser l’identité managée de fabrique de données :

- [Stocker des informations d’identification dans Azure Key Vault](store-credentials-in-key-vault.md)
- [Copier des données vers ou depuis Azure Data Lake Storage Gen1 à l’aide d’Azure Data Factory](connector-azure-data-lake-store.md)

Pour plus d’informations sur les identités managées des ressources Azure, sur lesquelles l’identité managée de fabrique de données est basée, consultez [Que sont les identités managées pour les ressources Azure ?](../active-directory/managed-identities-azure-resources/overview.md).