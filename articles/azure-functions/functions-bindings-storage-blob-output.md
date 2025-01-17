---
title: Liaison de sortie Stockage Blob Azure pour Azure Functions
description: Découvrez comment fournir des données de liaison de sortie de Stockage Blob Azure à une fonction Azure.
author: craigshoemaker
ms.topic: reference
ms.date: 02/13/2020
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: ede3789b8e0470436612ded9a9712b7aedb8c518
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2021
ms.locfileid: "129996429"
---
# <a name="azure-blob-storage-output-binding-for-azure-functions"></a>Liaison de sortie Stockage Blob Azure pour Azure Functions

La liaison de sortie vous permet de modifier et de supprimer des données de stockage d’objets Blob dans une fonction Azure.

Pour plus d’informations sur les détails d’installation et de configuration, consultez la [vue d’ensemble](./functions-bindings-storage-blob.md).

## <a name="example"></a>Exemple

# <a name="c"></a>[C#](#tab/csharp)

L’exemple suivant est une [fonction C#](functions-dotnet-class-library.md) qui utilise un déclencheur d’objet blob et deux liaisons de sortie d’objet blob. La fonction est déclenchée par la création d’un objet blob d’image dans le conteneur *sample-images*. Il crée des copies de petite et moyenne taille de l’objet blob d’image.

```csharp
using System.Collections.Generic;
using System.IO;
using Microsoft.Azure.WebJobs;
using SixLabors.ImageSharp;
using SixLabors.ImageSharp.Formats;
using SixLabors.ImageSharp.PixelFormats;
using SixLabors.ImageSharp.Processing;

public class ResizeImages
{
    [FunctionName("ResizeImage")]
    public static void Run([BlobTrigger("sample-images/{name}")] Stream image,
        [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall,
        [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
    {
        IImageFormat format;

        using (Image<Rgba32> input = Image.Load<Rgba32>(image, out format))
        {
            ResizeImage(input, imageSmall, ImageSize.Small, format);
        }

        image.Position = 0;
        using (Image<Rgba32> input = Image.Load<Rgba32>(image, out format))
        {
            ResizeImage(input, imageMedium, ImageSize.Medium, format);
        }
    }

    public static void ResizeImage(Image<Rgba32> input, Stream output, ImageSize size, IImageFormat format)
    {
        var dimensions = imageDimensionsTable[size];

        input.Mutate(x => x.Resize(dimensions.Item1, dimensions.Item2));
        input.Save(output, format);
    }

    public enum ImageSize { ExtraSmall, Small, Medium }

    private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
        { ImageSize.ExtraSmall, (320, 200) },
        { ImageSize.Small,      (640, 400) },
        { ImageSize.Medium,     (800, 600) }
    };

}
```

# <a name="c-script"></a>[Script C#](#tab/csharp-script)

<!--Same example for input and output. -->

L’exemple suivant montre des liaisons d’entrée et de sortie d’objet blob dans un fichier *function.json* et le code de [script C# (.csx)](functions-reference-csharp.md) qui utilise ces liaisons. La fonction effectue une copie d’un objet blob de texte. La fonction est déclenchée par un message de file d’attente qui contient le nom de l’objet blob à copier. Le nouvel objet blob est nommé *{originalblobname}-Copy*.

Dans le fichier *function.json*, la propriété de métadonnées `queueTrigger` est utilisée pour spécifier le nom de l’objet blob dans les propriétés `path` :

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

La section [configuration](#configuration) décrit ces propriétés.

Voici le code Script C# :

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

# <a name="java"></a>[Java](#tab/java)

Cette section contient les exemples suivants :

* [Déclencheur HTTP, utilisation d’OutputBinding](#http-trigger-using-outputbinding-java)
* [Déclencheur de file d’attente, utilisation de la valeur de retour de la fonction](#queue-trigger-using-function-return-value-java)

#### <a name="http-trigger-using-outputbinding-java"></a>Déclencheur HTTP, utilisation d’OutputBinding (Java)

 L’exemple suivant montre une fonction Java qui utilise l’annotation `HttpTrigger` pour recevoir un paramètre qui contient le nom d’un fichier dans un conteneur de stockage d’objets blob. L’annotation `BlobInput` lit ensuite le fichier et transmet son contenu à la fonction en tant que `byte[]`. L’annotation `BlobOutput` lie à `OutputBinding outputItem`, qui est ensuite utilisé par la fonction pour écrire le contenu de l’objet blob d’entrée dans le conteneur de stockage configuré.

```java
  @FunctionName("copyBlobHttp")
  @StorageAccount("Storage_Account_Connection_String")
  public HttpResponseMessage copyBlobHttp(
    @HttpTrigger(name = "req", 
      methods = {HttpMethod.GET}, 
      authLevel = AuthorizationLevel.ANONYMOUS) 
    HttpRequestMessage<Optional<String>> request,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{Query.file}") 
    byte[] content,
    @BlobOutput(
      name = "target", 
      path = "myblob/{Query.file}-CopyViaHttp")
    OutputBinding<String> outputItem,
    final ExecutionContext context) {
      // Save blob to outputItem
      outputItem.setValue(new String(content, StandardCharsets.UTF_8));

      // build HTTP response with size of requested blob
      return request.createResponseBuilder(HttpStatus.OK)
        .body("The size of \"" + request.getQueryParameters().get("file") + "\" is: " + content.length + " bytes")
        .build();
  }
```

#### <a name="queue-trigger-using-function-return-value-java"></a>Déclencheur de file d’attente, utilisation de la valeur de retour de la fonction (Java)

 L’exemple suivant montre une fonction Java qui utilise l’annotation `QueueTrigger` pour recevoir un message qui contient le nom d’un fichier dans un conteneur de stockage d’objets blob. L’annotation `BlobInput` lit ensuite le fichier et transmet son contenu à la fonction en tant que `byte[]`. L’annotation `BlobOutput` lie à la valeur de retour de la fonction, qui est ensuite utilisée par le runtime pour écrire le contenu de l’objet blob d’entrée dans le conteneur de stockage configuré.

```java
  @FunctionName("copyBlobQueueTrigger")
  @StorageAccount("Storage_Account_Connection_String")
  @BlobOutput(
    name = "target", 
    path = "myblob/{queueTrigger}-Copy")
  public String copyBlobQueue(
    @QueueTrigger(
      name = "filename", 
      dataType = "string",
      queueName = "myqueue-items") 
    String filename,
    @BlobInput(
      name = "file", 
      path = "samples-workitems/{queueTrigger}") 
    String content,
    final ExecutionContext context) {
      context.getLogger().info("The content of \"" + filename + "\" is: " + content);
      return content;
  }
```

 Dans la [bibliothèque du runtime des fonctions Java](/java/api/overview/azure/functions/runtime), utilisez l’annotation `@BlobOutput` sur les paramètres de fonction dont la valeur serait écrite dans un objet du stockage Blob.  Le type de paramètre doit être `OutputBinding<T>`, où T désigne n’importe quel type Java natif ou un POJO.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

<!--Same example for input and output. -->

L’exemple suivant montre des liaisons d’entrée et de sortie d’objets blob dans un fichier *function.json* et du [code JavaScript](functions-reference-node.md) qui utilise les liaisons. La fonction effectue une copie d’un objet blob. La fonction est déclenchée par un message de file d’attente qui contient le nom de l’objet blob à copier. Le nouvel objet blob est nommé *{originalblobname}-Copy*.

Dans le fichier *function.json*, la propriété de métadonnées `queueTrigger` est utilisée pour spécifier le nom de l’objet blob dans les propriétés `path` :

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

La section [configuration](#configuration) décrit ces propriétés.

Voici le code JavaScript :

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

L’exemple suivant montre comment créer une copie d’un BLOB entrant comme sortie d’une [fonction PowerShell](functions-reference-powershell.md).

Dans le fichier config de la fonction (*function.json*), la propriété de métadonnées `trigger` est utilisée pour spécifier le nom du BLOB de sortie dans les propriétés `path`.

> [!NOTE]
> Pour éviter les boucles infinies, vérifiez que vos chemins d’entrée et de sortie sont différents.

```json
{
  "bindings": [
    {
      "name": "myInputBlob",
      "path": "data/{trigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in",
      "type": "blobTrigger"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "data/copy/{trigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Voici le code PowerShell :

```powershell
# Input bindings are passed in via param block.
param([byte[]] $myInputBlob, $TriggerMetadata)
Write-Host "PowerShell Blob trigger function Processed blob Name: $($TriggerMetadata.Name)"
Push-OutputBinding -Name myOutputBlob -Value $myInputBlob
```

# <a name="python"></a>[Python](#tab/python)

<!--Same example for input and output. -->

L’exemple suivant montre des liaisons d’entrée et de sortie d’objets blob dans un fichier *function.json* et dans du [code Python](functions-reference-python.md) qui utilise les liaisons. La fonction effectue une copie d’un objet blob. La fonction est déclenchée par un message de file d’attente qui contient le nom de l’objet blob à copier. Le nouvel objet blob est nommé *{originalblobname}-Copy*.

Dans le fichier *function.json*, la propriété de métadonnées `queueTrigger` est utilisée pour spécifier le nom de l’objet blob dans les propriétés `path` :

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "queuemsg",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "inputblob",
      "type": "blob",
      "dataType": "binary",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "outputblob",
      "type": "blob",
      "dataType": "binary",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

La section [configuration](#configuration) décrit ces propriétés.

Voici le code Python :

```python
import logging
import azure.functions as func


def main(queuemsg: func.QueueMessage, inputblob: bytes, outputblob: func.Out[bytes]):
    logging.info(f'Python Queue trigger function processed {len(inputblob)} bytes')
    outputblob.set(inputblob)
```

---

## <a name="attributes-and-annotations"></a>Attributs et annotations

# <a name="c"></a>[C#](#tab/csharp)

Dans les [bibliothèques de classes C#](functions-dotnet-class-library.md), utilisez l’attribut [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs).

Le constructeur de l’attribut prend le chemin de l’objet blob et un paramètre `FileAccess` indiquant la lecture ou l’écriture, comme indiqué dans l’exemple suivant :

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
{
    ...
}
```

Vous pouvez définir la propriété `Connection` pour spécifier le compte de stockage à utiliser, comme indiqué dans l’exemple suivant :

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write, Connection = "StorageConnectionAppSetting")] Stream imageSmall)
{
    ...
}
```

# <a name="c-script"></a>[Script C#](#tab/csharp-script)

Les attributs ne sont pas pris en charge par le script C#.

# <a name="java"></a>[Java](#tab/java)

L'attribut `@BlobOutput` vous permet d’accéder à l’objet blob qui a déclenché la fonction. Si vous utilisez un tableau d’octets avec l’attribut, définissez `dataType` sur `binary`. Reportez-vous à l’[exemple de sortie](#example) pour plus d'informations.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Les attributs ne sont pas pris en charge par JavaScript.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Les attributs ne sont pas pris en charge par PowerShell.

# <a name="python"></a>[Python](#tab/python)

Les attributs ne sont pas pris en charge par Python.

---

Pour obtenir un exemple complet, consultez [Exemple de sortie](#example).

Vous pouvez utiliser l’attribut `StorageAccount` pour spécifier le compte de stockage au niveau de la classe, de la méthode ou du paramètre. Pour plus d’informations, consultez [Déclencheur - attributs](./functions-bindings-storage-blob-trigger.md#attributes-and-annotations).

## <a name="configuration"></a>Configuration

Le tableau suivant décrit les propriétés de configuration de liaison que vous définissez dans le fichier *function.json* et l’attribut `Blob`.

|Propriété function.json | Propriété d’attribut |Description|
|---------|---------|----------------------|
|**type** | n/a | Cette propriété doit être définie sur `blob`. |
|**direction** | n/a | Cette propriété doit être définie sur `out` pour une liaison de type sortie. Les exceptions sont notées à la section [utilisation](#usage). |
|**name** | n/a | Nom de la variable qui représente l’objet blob dans le code de la fonction.  La valeur doit être `$return` pour faire référence à la valeur de retour de la fonction.|
|**path** |**BlobPath** | Chemin du conteneur d’objet blob. |
|**connection** |**Connection**| Nom d’un paramètre d’application ou d’une collection de paramètres d’application qui spécifie la façon de se connecter à des objets blob Azure. Consultez [Connexions](#connections).|
|n/a | **y accéder** | Indique si vous lirez ou écrirez. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

[!INCLUDE [functions-storage-blob-connections](../../includes/functions-storage-blob-connections.md)]

## <a name="usage"></a>Usage

# <a name="c"></a>[C#](#tab/csharp)

[!INCLUDE [functions-bindings-blob-storage-output-usage.md](../../includes/functions-bindings-blob-storage-output-usage.md)]

# <a name="c-script"></a>[Script C#](#tab/csharp-script)

[!INCLUDE [functions-bindings-blob-storage-output-usage.md](../../includes/functions-bindings-blob-storage-output-usage.md)]

# <a name="java"></a>[Java](#tab/java)

L'attribut `@BlobOutput` vous permet d’accéder à l’objet blob qui a déclenché la fonction. Si vous utilisez un tableau d’octets avec l’attribut, définissez `dataType` sur `binary`. Reportez-vous à l’[exemple de sortie](#example) pour plus d'informations.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Accédez aux données BLOB en utilisant `context.bindings.<BINDING_NAME>`, où le nom de la liaison est défini dans le fichier _function.json_.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Accédez aux données BLOB via un paramètre qui correspond au nom désigné par le paramètre Nom de la liaison dans le fichier _function.json_.

# <a name="python"></a>[Python](#tab/python)

Vous pouvez déclarer des paramètres de fonction comme types suivants pour écrire dans le stockage blob :

* Chaînes comme `func.Out[str]`
* Flux comme `func.Out[func.InputStream]`

Reportez-vous à l’[exemple de sortie](#example) pour plus d'informations.

---

## <a name="exceptions-and-return-codes"></a>Exceptions et codes de retour

| Liaison |  Informations de référence |
|---|---|
| Objet blob | [Codes d’erreur du service BLOB](/rest/api/storageservices/fileservices/blob-service-error-codes) |
| Objet blob, Table, File d’attente |  [Codes d’erreur de stockage](/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| Objet blob, Table, File d’attente |  [Dépannage](/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Étapes suivantes

- [Exécuter une fonction quand les données de stockage Blob changent](./functions-bindings-storage-blob-trigger.md)
- [Lire des données de stockage de blobs lors de l’exécution d’une fonction](./functions-bindings-storage-blob-input.md)
