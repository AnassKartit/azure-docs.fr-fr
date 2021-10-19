---
title: Utiliser .NET pour créer une SAP de délégation d’utilisateur pour un conteneur, un répertoire ou un blob
titleSuffix: Azure Storage
description: Découvrez comment créer une SAS de délégation d’utilisateur avec des informations d’identification Azure Active Directory à l’aide de la bibliothèque de client .NET pour Stockage Azure.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 02/03/2021
ms.author: tamram
ms.reviewer: dineshm
ms.subservice: blobs
ms.openlocfilehash: ebd9ba6beabe1a0bd3e6f158b90ca90510ea1b66
ms.sourcegitcommit: d2875bdbcf1bbd7c06834f0e71d9b98cea7c6652
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2021
ms.locfileid: "129857819"
---
# <a name="create-a-user-delegation-sas-for-a-container-directory-or-blob-with-net"></a>Créer une SAP de délégation d’utilisateur pour un conteneur, un répertoire ou un blob en utilisant .NET

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

Cet article explique comment utiliser les informations d’identification Azure Active Directory (Azure AD) pour créer une SAP de délégation d’utilisateur pour un conteneur, un répertoire ou un blob avec la bibliothèque de client de Stockage Azure pour .NET version 12.

[!INCLUDE [storage-auth-user-delegation-include](../../../includes/storage-auth-user-delegation-include.md)]

## <a name="assign-azure-roles-for-access-to-data"></a>Attribuer des rôles Azure pour l’accès aux données

Lorsqu’un principal de sécurité Azure AD tente d’accéder aux données blob, ce principal de sécurité doit avoir des autorisations sur la ressource. Que le principal de sécurité soit une identité managée dans Azure ou un compte d’utilisateur Azure AD exécutant du code dans l’environnement de développement, un rôle Azure qui accorde l’accès aux données blob dans le stockage Azure doit être affecté au principal de sécurité. Pour obtenir des informations sur l’attribution d’autorisations via le contrôle d’accès en fonction du rôle Azure, consultez [Attribuer un rôle Azure pour l’accès aux données d’objet blob](assign-azure-role-data-access.md).

Pour en savoir plus sur la façon d’obtenir un jeton avec la bibliothèque de client d’identités Azure à partir de Stockage Azure, consultez [Utiliser la bibliothèque d’identités Azure pour obtenir un jeton d’accès pour l’autorisation](../common/identity-library-acquire-token.md).

## <a name="get-an-authenticated-token-credential"></a>Obtenir des informations d’identification d’un jeton authentifié

Pour obtenir les informations d’identification d’un jeton que votre code peut utiliser pour autoriser les requêtes au stockage Azure, créez une instance de la classe [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential). Pour plus d’informations sur l’utilisation de la classe DefaultAzureCredential pour autoriser une identité managée à accéder à Stockage Azure, consultez [Bibliothèque de client d’identité Azure pour .NET](/dotnet/api/overview/azure/identity-readme).

L’extrait de code suivant montre comment obtenir les informations d’identification d’un jeton authentifié et comment les utiliser pour créer un client de service pour le stockage blob :

```csharp
// Construct the blob endpoint from the account name.
string blobEndpoint = string.Format("https://{0}.blob.core.windows.net", accountName);

// Create a new Blob service client with Azure AD credentials.
BlobServiceClient blobClient = new BlobServiceClient(new Uri(blobEndpoint),
                                                     new DefaultAzureCredential());
```

## <a name="get-the-user-delegation-key"></a>Obtenir la clé de délégation d’utilisateur

Chaque SAP est signée avec une clé. Pour créer une SAP de délégation d’utilisateur, vous devez d’abord demander une clé de délégation d’utilisateur, qui est ensuite utilisée pour signer la SAP. La clé de délégation d’utilisateur est analogue à la clé de compte utilisée pour signer une SAP de service ou une SAP de compte, à ceci près qu’elle s’appuie sur vos informations d’identification Azure AD. Lorsqu’un client demande une clé de délégation d’utilisateur à l’aide d’un jeton OAuth 2.0, le Stockage Azure retourne la clé de délégation utilisateur pour le compte de l’utilisateur.

Une fois que vous disposez de la clé de délégation d’utilisateur, vous pouvez utiliser cette clé pour créer un nombre quelconque de signatures d’accès partagé de délégation d’utilisateur tout au long de la durée de vie de la clé. La clé de délégation d’utilisateur est indépendante du jeton OAuth 2.0 utilisé pour l’acquérir. Il n’est donc pas nécessaire de renouveler le jeton tant que la clé est toujours valide. Vous pouvez spécifier que la clé est valide pendant une période allant jusqu’à 7 jours.

Utilisez l’une des méthodes suivantes pour demander la clé de délégation d’utilisateur :

- [GetUserDelegationKey](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkey)
- [GetUserDelegationKeyAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobclient.getuserdelegationkeyasync)

L’extrait de code suivant permet d’obtenir la clé de délégation d’utilisateur et d’écrire ses propriétés :

```csharp
// Get a user delegation key for the Blob service that's valid for seven days.
// You can use the key to generate any number of shared access signatures over the lifetime of the key.
UserDelegationKey key = await blobClient.GetUserDelegationKeyAsync(DateTimeOffset.UtcNow,
                                                                   DateTimeOffset.UtcNow.AddDays(7));

// Read the key's properties.
Console.WriteLine("User delegation key properties:");
Console.WriteLine("Key signed start: {0}", key.SignedStartsOn);
Console.WriteLine("Key signed expiry: {0}", key.SignedExpiresOn);
Console.WriteLine("Key signed object ID: {0}", key.SignedObjectId);
Console.WriteLine("Key signed tenant ID: {0}", key.SignedTenantId);
Console.WriteLine("Key signed service: {0}", key.SignedService);
Console.WriteLine("Key signed version: {0}", key.SignedVersion);
```

## <a name="get-a-user-delegation-sas-for-a-blob"></a>Obtenir une SAP de délégation d’utilisateur pour un blob

L’exemple de code suivant montre le code complet pour l’authentification du principal de sécurité et la création de la SAP de délégation d’utilisateur pour un blob :

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Sas.cs" id="Snippet_GetUserDelegationSasBlob":::

L’exemple suivant teste la SAP de délégation d’utilisateur créée dans l’exemple précédent à partir d’une application cliente simulée. Si la SAP est valide, l’application cliente est en mesure de lire le contenu de l’objet blob. Si la SAP n’est pas valide, par exemple si elle a expiré, le Stockage Azure retourne le code d’erreur 403 (interdit).

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Sas.cs" id="Snippet_ReadBlobWithSasAsync":::

## <a name="get-a-user-delegation-sas-for-a-container"></a>Obtenir une SAP de délégation d’utilisateur pour un conteneur

L’exemple de code suivant montre comment générer une SAP de délégation d’utilisateur pour un conteneur :

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Sas.cs" id="Snippet_GetUserDelegationSasContainer":::

L’exemple suivant teste la SAP de délégation d’utilisateur créée dans l’exemple précédent à partir d’une application cliente simulée. Si la SAP est valide, l’application cliente est en mesure de lire le contenu de l’objet blob. Si la SAP n’est pas valide, par exemple si elle a expiré, le Stockage Azure retourne le code d’erreur 403 (interdit).

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Sas.cs" id="Snippet_ListBlobsWithSasAsync":::

## <a name="get-a-user-delegation-sas-for-a-directory"></a>Obtenir une SAP de délégation d’utilisateur pour un répertoire

L’exemple de code suivant montre comment générer une SAP de délégation d’utilisateur pour un répertoire lorsqu’un espace de noms hiérarchique est activé pour le compte de stockage :

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Sas.cs" id="Snippet_GetUserDelegationSasDirectory":::

L’exemple suivant teste la SAP de délégation d’utilisateur créée dans l’exemple précédent à partir d’une application cliente simulée. Si la SAP est valide, l’application cliente est en mesure de répertorier les chemins d’accès aux fichiers de ce répertoire. Si la SAP n’est pas valide, par exemple si elle a expiré, le Stockage Azure retourne le code d’erreur 403 (interdit).

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Sas.cs" id="Snippet_ListFilePathsWithDirectorySasAsync":::

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Voir aussi

- [Accorder un accès limité aux ressources du Stockage Azure à l’aide des signatures d’accès partagé (SAP)](../common/storage-sas-overview.md)
- [Obtenir une opération de clé de délégation d’utilisateur](/rest/api/storageservices/get-user-delegation-key)
- [Créer une SAP de délégation d’utilisateur (API REST)](/rest/api/storageservices/create-user-delegation-sas)
