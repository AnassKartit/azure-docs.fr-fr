---
title: Créer une signature d’accès partagé (SAS) de service pour un conteneur ou un blob
titleSuffix: Azure Storage
description: Découvrez comment créer une signature d’accès partagé (SAP) de service pour un conteneur ou un blob à l’aide de la bibliothèque de client de Stockage Blob Azure.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 03/23/2021
ms.author: tamram
ms.reviewer: dineshm
ms.subservice: blobs
ms.custom: devx-track-csharp
ms.openlocfilehash: 5e2674a3e9227f5cf5cf2a6ebff0a317f4377f3a
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128606166"
---
# <a name="create-a-service-sas-for-a-container-or-blob"></a>Créer une signature d’accès partagé (SAS) de service pour un conteneur ou un blob

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

Cet article explique comment utiliser la clé du compte de stockage pour créer une SAS de service pour un conteneur ou un blob avec la bibliothèque cliente Stockage Azure pour Stockage Blob.

## <a name="create-a-service-sas-for-a-blob-container"></a>Créer une SAP de service pour un conteneur d’objets blob

L’exemple de code suivant crée une SAP pour un conteneur. Si le nom d’une stratégie d’accès stockée existante est fourni, cette stratégie est associée à la SAP. Dans le cas contraire, le code crée une SAP ad hoc sur le conteneur.

### <a name="net-v12-sdk"></a>[Kit de développement logiciel (SDK) .NET v12](#tab/dotnet)

Une SAP de service est signée avec la clé d’accès au compte. Utilisez la classe [StorageSharedKeyCredential](/dotnet/api/azure.storage.storagesharedkeycredential) afin de créer les informations d’identification utilisées pour signer le jeton SAS. Créez ensuite un objet [BlobSasBuilder](/dotnet/api/azure.storage.sas.blobsasbuilder) et appelez [ToSasQueryParameters](/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) pour obtenir la chaîne de jeton SAS.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Sas.cs" id="Snippet_GetServiceSasUriForContainer":::

### <a name="net-v11-sdk"></a>[Kit de développement logiciel (SDK) .NET v11](#tab/dotnetv11)

Pour créer une SAP de service pour un conteneur, appelez la méthode [CloudBlobContainer.GetSharedAccessSignature](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getsharedaccesssignature).

```csharp
private static string GetContainerSasUri(CloudBlobContainer container,
                                         string storedPolicyName = null)
{
    string sasContainerToken;

    // If no stored policy is specified, create a new access policy and define its constraints.
    if (storedPolicyName == null)
    {
        // Note that the SharedAccessBlobPolicy class is used both to define
        // the parameters of an ad hoc SAS, and to construct a shared access policy
        // that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocPolicy = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed
            // to be the time when the storage service receives the request. Omitting
            // the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List
        };

        // Generate the shared access signature on the container,
        // setting the constraints directly on the signature.
        sasContainerToken = container.GetSharedAccessSignature(adHocPolicy, null);

        Console.WriteLine("SAS for blob container (ad hoc): {0}", sasContainerToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the container. In this case,
        // all of the constraints for the shared access signature are specified
        // on the stored access policy, which is provided by name. It is also possible
        // to specify some constraints on an ad hoc SAS and others on the stored access policy.
        sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

        Console.WriteLine("SAS for container (stored access policy): {0}", sasContainerToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

# <a name="javascript-v12-sdk"></a>[Kit SDK JavaScript v12](#tab/javascript)

Une SAP de service est signée avec la clé d’accès au compte. Utilisez la classe [StorageSharedKeyCredential](/javascript/api/@azure/storage-blob/storagesharedkeycredential) afin de créer les informations d’identification utilisées pour signer le jeton SAS. Appelez ensuite la fonction [generateBlobSASQueryParameters](/javascript/api/@azure/storage-blob/#generateBlobSASQueryParameters_BlobSASSignatureValues__StorageSharedKeyCredential_) en fournissant les paramètres requis pour obtenir la chaîne de jeton SAP.

:::code language="javascript" source="~/azure-storage-snippets/blobs/howto/JavaScript/NodeJS-v12/SAS.js" id="Snippet_ContainerSAS":::

---

## <a name="create-a-service-sas-for-a-blob"></a>Créer une SAP de service pour un objet blob

L’exemple de code suivant crée une SAP sur un objet blob. Si le nom d’une stratégie d’accès stockée existante est fourni, cette stratégie est associée à la SAP. Dans le cas contraire, le code crée une SAP ad hoc sur l’objet blob.

# <a name="net-v12-sdk"></a>[Kit de développement logiciel (SDK) .NET v12](#tab/dotnet)

Une SAP de service est signée avec la clé d’accès au compte. Utilisez la classe [StorageSharedKeyCredential](/dotnet/api/azure.storage.storagesharedkeycredential) afin de créer les informations d’identification utilisées pour signer le jeton SAS. Créez ensuite un objet [BlobSasBuilder](/dotnet/api/azure.storage.sas.blobsasbuilder) et appelez [ToSasQueryParameters](/dotnet/api/azure.storage.sas.blobsasbuilder.tosasqueryparameters) pour obtenir la chaîne de jeton SAS.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Sas.cs" id="Snippet_GetServiceSasUriForBlob":::

# <a name="net-v11-sdk"></a>[Kit de développement logiciel (SDK) .NET v11](#tab/dotnetv11)

Pour créer une SAP de service pour un objet blob, appelez la méthode [CloudBlob.GetSharedAccessSignature](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getsharedaccesssignature).

```csharp
private static string GetBlobSasUri(CloudBlobContainer container,
                                    string blobName,
                                    string policyName = null)
{
    string sasBlobToken;

    // Get a reference to a blob within the container.
    // Note that the blob may not exist yet, but a SAS can still be created for it.
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    if (policyName == null)
    {
        // Create a new access policy and define its constraints.
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters
        // of an ad hoc SAS, and to construct a shared access policy that is saved to
        // the container's shared access policies.
        SharedAccessBlobPolicy adHocSAS = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be
            // the time when the storage service receives the request. Omitting the start time
            // for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Read |
                          SharedAccessBlobPermissions.Write |
                          SharedAccessBlobPermissions.Create
        };

        // Generate the shared access signature on the blob,
        // setting the constraints directly on the signature.
        sasBlobToken = blob.GetSharedAccessSignature(adHocSAS);

        Console.WriteLine("SAS for blob (ad hoc): {0}", sasBlobToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the blob. In this case, all of the constraints
        // for the SAS are specified on the container's stored access policy.
        sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        Console.WriteLine("SAS for blob (stored access policy): {0}", sasBlobToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

# <a name="javascript-v12-sdk"></a>[Kit SDK JavaScript v12](#tab/javascript)

Pour créer une SAP de service pour un objet blob, appelez la méthode [CloudBlob.GetSharedAccessSignature](/dotnet/api/microsoft.azure.storage.blob.cloudblob.getsharedaccesssignature).

Pour créer une SAP de service pour un blob, appelez la fonction [generateBlobSASQueryParameters](/javascript/api/@azure/storage-blob/#generateBlobSASQueryParameters_BlobSASSignatureValues__StorageSharedKeyCredential_) en fournissant les paramètres requis.

:::code language="javascript" source="~/azure-storage-snippets/blobs/howto/JavaScript/NodeJS-v12/SAS.js" id="Snippet_BlobSAS":::

---

## <a name="create-a-service-sas-for-a-directory"></a>Créer une SAS de service pour un répertoire

Dans un compte de stockage doté d’un espace de noms hiérarchique activé, vous pouvez créer une SAS de service pour un répertoire. Pour créer la SAS de service, assurez-vous d’avoir installé la version 12.5.0 ou ultérieure du package [Azure.Storage.Files.DataLake](https://www.nuget.org/packages/Azure.Storage.Files.DataLake/).

L’exemple suivant montre comment créer une SAS de service destinée à un répertoire avec la bibliothèque de client v12 pour .NET :

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Sas.cs" id="Snippet_GetServiceSasUriForDirectory":::

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

[!INCLUDE [storage-blob-javascript-resources-include](../../../includes/storage-blob-javascript-resources-include.md)]

## <a name="next-steps"></a>Étapes suivantes

- [Accorder un accès limité aux ressources du Stockage Azure à l’aide des signatures d’accès partagé (SAP)](../common/storage-sas-overview.md)
- [Créer une SAP de service](/rest/api/storageservices/create-service-sas)
