---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 08/17/2020
ms.author: inhenkel
ms.custom: CLI
ms.openlocfilehash: affd55d65ed8454ebfcdf14f697849badf8e5d3c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "88657081"
---
<!-- ### Upload files with the CLI -->

La commande suivante permet de charger un seul fichier.  

Changez les valeurs suivantes :

1. Remplacez `/path/to/file` par votre chemin local du fichier.  
1. Remplacez `MyContainer` par un élément multimédia que vous avez créé précédemment en utilisant l’ID de l’élément multimédia (pas le nom).
1. Remplacez `MyBlob` par le nom que vous souhaitez attribuer au fichier chargé.
1. Remplacez `MyStorageAccountName` par le nom du compte de stockage que vous utilisez.
1. Remplacez `MyStorageAccountKey` par la clé d’accès de votre compte de stockage.

    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob --acount-name MyStorageAccountName --account-key MyStorageAccountKey
    ```
