---
title: 'Démarrage rapide : Bibliothèque Stockage Blob Azure v12 - C++'
description: Dans ce guide de démarrage rapide, vous découvrez comment utiliser la bibliothèque de client Stockage Blob Azure version 12 pour C++ afin de créer un conteneur et un objet blob dans le stockage (d’objets) blob. Vous apprenez ensuite à télécharger l’objet blob sur votre ordinateur local et à lister tous les objets blob dans un conteneur.
author: kyle-patterson
ms.author: kylepa
ms.date: 06/21/2021
ms.service: storage
ms.subservice: blobs
ms.topic: quickstart
ms.openlocfilehash: 34a232990f037e72cee97c6f185b567056cc394e
ms.sourcegitcommit: e8b229b3ef22068c5e7cd294785532e144b7a45a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2021
ms.locfileid: "123470595"
---
# <a name="quickstart-azure-blob-storage-client-library-v12-for-c"></a>Démarrage rapide : Bibliothèque de client Stockage Blob Azure v12 pour C++

Bien démarrer avec la bibliothèque de client Stockage Blob Azure v12 pour C++. Le Stockage Blob Azure est la solution de stockage d’objets de Microsoft pour le cloud. Suivez les étapes pour installer le package et essayer l’exemple de code pour les tâches de base. Stockage Blob est optimisé pour stocker des quantités massives de données non structurées.

Utilisez la bibliothèque de client Stockage Blob Azure v12 pour C++ pour :

- Créez un conteneur.
- Charger un blob dans le stockage Azure
- Lister tous les objets blob d’un conteneur
- Télécharger l’objet blob sur votre ordinateur local
- Supprimer un conteneur

Ressources :

- [Documentation de référence de l’API](https://azure.github.io/azure-sdk-for-cpp/storage.html)
- [Code source de la bibliothèque](https://github.com/Azure/azure-sdk-for-cpp/tree/master/sdk/storage)
- [Exemples](../common/storage-samples-c-plus-plus.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="prerequisites"></a>Prérequis

- [Abonnement Azure](https://azure.microsoft.com/free/)
- [Compte Azure Storage](../common/storage-account-create.md)
- [Compilateur C++](https://azure.github.io/azure-sdk/cpp_implementation.html#supported-platforms)
- [CMake](https://cmake.org/)
- [Vcpkg - Gestionnaire de package C et C++](https://github.com/microsoft/vcpkg/blob/master/docs/README.md)

## <a name="setting-up"></a>Configuration

Cette section vous guide tout au long de la préparation d’un projet pour utiliser la bibliothèque de client Stockage Blob Azure v12 pour C++.

### <a name="install-the-packages"></a>Installer les packages

La commande `vcpkg install` installe le kit de développement logiciel (SDK) d’Azure Storage Blob pour C++ et les dépendances nécessaires :

```console
vcpkg.exe install azure-storage-blobs-cpp:x64-windows
```

Pour plus d’informations, visitez GitHub pour acquérir et créer le [SDK Azure pour C++](https://github.com/Azure/azure-sdk-for-cpp/).

### <a name="create-the-project"></a>Créer le projet

Dans Visual Studio, créez une nouvelle application console C++ pour Windows appelée *BlobQuickstartV12*.

:::image type="content" source="./media/quickstart-blobs-c-plus-plus/vs-create-project.jpg" alt-text="Boîte de dialogue Visual Studio pour la configuration d’une nouvelle application console Windows C++":::

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

## <a name="object-model"></a>Modèle objet

Stockage Blob Azure est optimisé pour stocker des quantités massives de données non structurées. Les données non structurées sont des données qui n’obéissent pas à un modèle ou une définition de données en particulier, comme des données texte ou binaires. Stockage Blob offre trois types de ressources :

- Le compte de stockage
- Un conteneur dans le compte de stockage.
- Un blob dans le conteneur

Le diagramme suivant montre la relation entre ces ressources.

![Diagramme de l’architecture de Stockage Blob](./media/storage-blobs-introduction/blob1.png)

Utilisez ces classes C++ pour interagir avec ces ressources :

- [BlobServiceClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_blob_service_client.html): La classe `BlobServiceClient` vous permet de manipuler les ressources de stockage Azure et les conteneurs blob.
- [BlobContainerClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_blob_container_client.html) : La classe `BlobContainerClient` vous permet de manipuler des conteneurs de stockage Azure et leurs blobs.
- [BlobClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_blob_client.html) : La classe `BlobClient` vous permet de manipuler des blobs de stockage Azure. Il s’agit de la classe de base pour toutes les classes d’objets blob spécialisées.
- [BlockBlobClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_block_blob_client.html): La classe `BlockBlobClient` vous permet de manipuler des objets blob de blocs de Stockage Azure.

## <a name="code-examples"></a>Exemples de code

Ces exemples d’extraits de code vous montrent comment effectuer les tâches suivantes avec la bibliothèque de client Stockage Blob Azure pour C++ :

- [Ajouter des fichiers include](#add-include-files)
- [Obtenir la chaîne de connexion](#get-the-connection-string)
- [Créer un conteneur](#create-a-container)
- [Charger des objets blob sur un conteneur](#upload-blobs-to-a-container)
- [Lister les objets blob d’un conteneur](#list-the-blobs-in-a-container)
- [Télécharger des objets blob](#download-blobs)
- [Supprimer un conteneur](#delete-a-container)

### <a name="add-include-files"></a>Ajouter des fichiers include

À partir du répertoire de projet :

1. Ouvrez le fichier solution *BlobQuickstartV12.sln* dans Visual Studio.
1. Dans Visual Studio, ouvrez le fichier source *BlobQuickstartV12.cpp*.
1. Supprimez le code dans `main` qui a été généré automatiquement
1. Ajoutez les instructions `#include`

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_Includes":::

### <a name="get-the-connection-string"></a>Obtenir la chaîne de connexion

Le code ci-dessous récupère la chaîne de connexion pour votre compte de stockage dans la variable d’environnement créée dans [Configurer votre chaîne de connexion de stockage](#configure-your-storage-connection-string).

Ajoutez ce code dans `main()` :

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_ConnectionString":::

### <a name="create-a-container"></a>Créez un conteneur.

Créez une instance de la classe [BlobContainerClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_blob_container_client.html) en appelant la fonction [CreateFromConnectionString](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_blob_container_client.html#a5d253aacb6e20578b7f5f233547be3e2). Ensuite, appelez [CreateIfNotExists](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_blob_container_client.html#ab3ef187d2e30e1a19ebadf45d0fdf9c4) pour créer le conteneur réel dans votre compte de stockage.

> [!IMPORTANT]
> Les noms de conteneurs doivent être en minuscules. Pour plus d’informations sur l’affectation de noms aux conteneurs et objets blob, consultez [Affectation de noms et références aux conteneurs, objets blob et métadonnées](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

Ajoutez ce code à la fin de `main()` :

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_CreateContainer":::

### <a name="upload-blobs-to-a-container"></a>Charger des objets blob sur un conteneur

L’extrait de code suivant :

1. Déclare une chaîne contenant « Hello Azure! ».
1. Obtient une référence à un objet [BlockBlobClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/1.0.0-beta.2/class_azure_1_1_storage_1_1_blobs_1_1_block_blob_client.html) en appelant la méthode [GetBlockBlobClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/1.0.0-beta.2/class_azure_1_1_storage_1_1_blobs_1_1_blob_container_client.html#acd8c68e3f37268fde0010dd478ff048f) sur le conteneur provenant de la section [Créer un conteneur](#create-a-container).
1. Charge la chaîne dans l’objet blob en appelant la fonction [UploadFrom](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/1.0.0-beta.2/class_azure_1_1_storage_1_1_blobs_1_1_block_blob_client.html#af93af7e37f8806e39481596ef253f93d). Cette fonction crée l’objet blob s’il n’existe pas déjà ou le remplace s’il existe.

Ajoutez ce code à la fin de `main()` :

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_UploadBlob":::

### <a name="list-the-blobs-in-a-container"></a>Lister les objets blob dans un conteneur

Répertoriez les objets Blob dans le conteneur en appelant la fonction [ListBlobs](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_blob_container_client.html#a8f7e6653ef69788cb4fb23ec85d4fd96). Un seul objet blob a été ajouté au conteneur : l’opération retourne donc seulement cet objet blob.

Ajoutez ce code à la fin de `main()` :

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_ListBlobs":::

### <a name="download-blobs"></a>Télécharger des objets blob

Obtenez les propriétés de l’objet blob chargé. Ensuite, déclarez et redimensionnez un nouvel objet `std::vector<uint8_t>` en utilisant les propriétés de l’objet blob chargé. Téléchargez l’objet blob créé précédemment dans le nouvel objet `std::vector<uint8_t>` en appelant la fonction [DownloadTo](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_blob_client.html#aa0bcbdbf6ebd17e4888cced4736420f7) dans la classe de base [BlobClient](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_blob_client.html). Enfin, affichez les données de l’objet blob téléchargé.

Ajoutez ce code à la fin de `main()` :

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_DownloadBlob":::

### <a name="delete-a-blob"></a>Suppression d'un objet blob

Le code suivant supprime l’objet blob du conteneur Stockage Blob Azure en appelant la fonction [BlobClient.Delete](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_blob_client.html#ac121df9f5dfcdeb80f47b50543df0984).

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_DeleteBlob":::

### <a name="delete-a-container"></a>Supprimer un conteneur

Le code suivant nettoie les ressources créées par l’application en supprimant l’ensemble du conteneur avec [BlobContainerClient.Delete](https://azuresdkdocs.blob.core.windows.net/$web/cpp/azure-storage-blobs/12.0.0/class_azure_1_1_storage_1_1_blobs_1_1_blob_container_client.html#ae7661b6d8f37213b409504c0e9a68d69).

Ajoutez ce code à la fin de `main()` :

:::code language="cpp" source="~/azure-storage-snippets/blobs/quickstarts/C++/V12/BlobQuickstartV12/BlobQuickstartV12/BlobQuickstartV12.cpp" ID="Snippet_DeleteContainer":::

## <a name="run-the-code"></a>Exécuter le code

Cette application crée un conteneur et charge un fichier texte dans Stockage Blob Azure. L’exemple liste ensuite le ou les objets blob du conteneur, télécharge le fichier et affiche le contenu du fichier. Enfin, l’application supprime l’objet blob et le conteneur.

La sortie de l’application ressemble à l’exemple suivant :

```output
Azure Blob Storage v12 - C++ quickstart sample
Creating container: myblobcontainer
Uploading blob: blob.txt
Listing blobs...
Blob name: blob.txt
Downloaded blob contents: Hello Azure!
Deleting blob: blob.txt
Deleting container: myblobcontainer
```

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez découvert comment charger, télécharger et lister des objets blob en utilisant C++. Vous avez également découvert comment créer et supprimer un conteneur Stockage Blob Azure.

Pour voir un exemple d’application Stockage Blob C++, passez à :

> [!div class="nextstepaction"]
> [Exemple du SDK Stockage Blob Azure v12 pour C++](https://github.com/Azure/azure-sdk-for-cpp/tree/master/sdk/storage/azure-storage-blobs/sample)
