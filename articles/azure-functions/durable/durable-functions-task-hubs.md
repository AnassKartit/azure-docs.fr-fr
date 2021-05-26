---
title: Hubs de tâches dans Fonctions durables - Azure
description: Découvrez ce que représente un hub de tâches dans l’extension Fonctions durables pour Azure Functions. Découvrez comment configurer des hubs de tâches.
author: cgillum
ms.topic: conceptual
ms.date: 05/12/2021
ms.author: azfuncdf
ms.openlocfilehash: 9172075ca22937a85fd7fd5827ebb40a4b58bcfa
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2021
ms.locfileid: "110375755"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Hubs de tâches dans Fonctions durables (Azure Functions)

Un *hub de tâches* dans [Durable Functions](durable-functions-overview.md) est un conteneur logique réservé aux ressources de stockage durables qui sont utilisées pour les orchestrations et entités. Les fonctions d’orchestrateur, d’activité et d’entité ne peuvent interagir directement que si elles appartiennent au même hub de tâches.

> [!NOTE]
> Ce document décrit les détails des hubs de tâches d’une façon qui est spécifique au [fournisseur Stockage Azure par défaut pour Durable Functions](durable-functions-storage-providers.md#azure-storage). Si vous utilisez un fournisseur de stockage autre que celui par défaut pour votre application Durable Functions, vous trouverez une documentation détaillée sur le hub de tâches dans la documentation spécifique au fournisseur :
> 
> * [Informations sur le hub de tâches pour le fournisseur de stockage Netherite](https://microsoft.github.io/durabletask-netherite/#/storage)
> * [Informations sur le hub de tâches pour le fournisseur de stockage Microsoft SQL (MSSQL)](https://microsoft.github.io/durabletask-mssql/#/taskhubs)
> 
> Pour plus d’informations sur les différentes options du fournisseur de stockage et leur comparaison, consultez la documentation sur les [fournisseurs de stockage Durable Functions](durable-functions-storage-providers.md).

Si plusieurs applications de fonction partagent un compte de stockage, chaque application de fonction *doit* être configurée avec un nom de hub de tâches distinct. Un compte de stockage peut comporter de multiples hub de tâches. Cette restriction s’applique généralement également aux autres fournisseurs de stockage.

Le diagramme suivant illustre un hub de tâches par application de fonction dans les comptes Stockage Azure partagés et dédiés.

![Schéma représentant les comptes de stockage partagé et dédié.](./media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Ressources Azure Storage

Un hub de tâches dans Stockage Azure se compose des ressources suivantes :

* Une ou plusieurs files d’attente de contrôle.
* Une file d’attente des éléments de travail.
* Une table d’historique.
* Une table d’instances.
* Un conteneur de stockage comprenant un ou plusieurs objets blob de bail.
* Un conteneur de stockage contenant des charges utiles volumineuses de messages, le cas échéant.

Toutes ces ressources sont automatiquement créées dans le compte Stockage Azure configuré lorsque des fonctions d’orchestrateur, d’entité ou d’activité sont exécutées ou planifiées pour l’être. L’article [Performances et mise à l’échelle](durable-functions-perf-and-scale.md) explique comment ces ressources sont utilisées.

## <a name="task-hub-names"></a>Noms de hub de tâches

Les hubs de tâches dans Stockage Azure sont identifiés par un nom conforme aux règles suivantes :

* Nom uniquement composé de caractères alphanumériques
* Nom commençant par une lettre
* Nom compris entre 3 et 45 caractères

Le nom du hub de tâches est déclaré dans le fichier *host.json*, comme illustré dans l'exemple suivant :

### <a name="hostjson-functions-20"></a>host.json (Functions 2.0)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "MyTaskHub"
    }
  }
}
```

### <a name="hostjson-functions-1x"></a>host.json (Functions 1.x)

```json
{
  "durableTask": {
    "hubName": "MyTaskHub"
  }
}
```

Les hubs de tâches peuvent également être configurés à l’aide de paramètres d’application, comme indiqué dans l’exemple de fichier `host.json` suivant :

### <a name="hostjson-functions-10"></a>host.json (Functions 1.0)

```json
{
  "durableTask": {
    "hubName": "%MyTaskHub%"
  }
}
```

### <a name="hostjson-functions-20"></a>host.json (Functions 2.0)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "%MyTaskHub%"
    }
  }
}
```

Le nom du hub de tâches est défini sur la valeur du paramètre d’application `MyTaskHub`. Le fichier `local.settings.json` suivant montre comment définir le paramètre `MyTaskHub` sur `samplehubname` :

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" : "samplehubname"
  }
}
```

Outre **host.json**, les noms de hub de tâches peuvent également être configurés dans les métadonnées de [liaison du client d’orchestration](durable-functions-bindings.md#orchestration-client). Cela est utile si vous avez besoin d’accéder aux orchestrations ou aux entités qui résident dans une application de fonction distincte. Le code suivant montre comment écrire une fonction recourant à la [liaison du client d’orchestration](durable-functions-bindings.md#orchestration-client) pour utiliser un hub de tâches configuré en tant que paramètre d’application :

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
    [DurableClient(TaskHub = "%MyTaskHub%")] IDurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // Function input comes from the request content.
    object eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

> [!NOTE]
> L’exemple C# précédent porte sur Durable Functions 2.x. Pour Durable Functions 1.x, vous devez utiliser `DurableOrchestrationContext` au lieu de `IDurableOrchestrationContext`. Pour en savoir plus sur les différences entre les versions, consultez l’article [Versions de Durable Functions](durable-functions-versions.md).

# <a name="javascript"></a>[JavaScript](#tab/javascript)

La propriété de hub de tâches dans le fichier `function.json` est définie par le biais du paramètre d’application :

```json
{
    "name": "input",
    "taskHub": "%MyTaskHub%",
    "type": "orchestrationClient",
    "direction": "in"
}
```

# <a name="python"></a>[Python](#tab/python)

La propriété de hub de tâches dans le fichier `function.json` est définie par le biais du paramètre d’application :

```json
{
    "name": "input",
    "taskHub": "%MyTaskHub%",
    "type": "orchestrationClient",
    "direction": "in"
}
```

---

> [!NOTE]
> La configuration de noms de hub de tâches dans les métadonnées de liaison du client est uniquement nécessaire lorsque vous utilisez une application de fonction pour accéder aux orchestrations et aux entités d’une autre application de fonction. Si les fonctions clientes sont définies dans la même application de fonction que les orchestrations et les entités, vous devez éviter de spécifier des noms de hub de tâches dans les métadonnées de liaison. Par défaut, toutes les liaisons clientes obtiennent leurs métadonnées de hub de tâches à partir des paramètres de **host.json**.

Les noms de hubs de tâches dans Stockage Azure doivent commencer par une lettre et contenir uniquement des lettres et des chiffres. S’il n’est pas spécifié, un nom de hub de tâches par défaut sera utilisé comme indiqué dans le tableau suivant :

| Version d’extension durable | Nom du hub de tâches par défaut |
| - | - |
| 2.x | Lorsqu’il est déployé dans Azure, le nom du hub de tâches est dérivé du nom de l’_application de fonction_. En cas d’exécution en dehors d’Azure, le nom du hub de tâches par défaut est `TestHubName`. |
| 1.x | Le nom du hub de tâches par défaut pour tous les environnements est `DurableFunctionsHub`. |

Pour en savoir plus sur les différences entre les versions d’extension, consultez l’article [Versions de Durable Functions](durable-functions-versions.md).

> [!NOTE]
> Le nom permet de différencier les hubs de tâches les uns des autres lorsqu’ils sont plusieurs dans un compte de stockage partagé. Si plusieurs applications de fonction partagent un compte de stockage partagé, vous devez configurer explicitement des noms différents pour chaque hub de tâches dans les fichiers *host.json*. Dans le cas contraire, les applications de fonction seront en concurrence les unes avec les autres pour les messages, ce qui pourrait entraîner un comportement inhabituel, y compris des orchestrations qui se retrouveraient inopinément « bloquées » dans l’état `Pending` ou `Running`.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Découvrez comment gérer le contrôle de version des orchestrations](durable-functions-versioning.md)
