---
title: Déployer un conteneur LUIS sur des instances Azure Container
titleSuffix: Azure Cognitive Services
description: Déployez le conteneur LUIS sur une instance Azure Container, et testez-le dans un navigateur web.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: aahi
ms.openlocfilehash: edc2ad0f895b8a1bb6448ce1cdf79b1b2ce83951
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95997191"
---
# <a name="deploy-the-language-understanding-luis-container-to-azure-container-instances"></a>Déployer le conteneur Language Understanding (LUIS) sur des instances Azure Container

Découvrez comment déployer le conteneur [LUIS](luis-container-howto.md) de Cognitive Services sur des [instances Azure Container](../../container-instances/index.yml). Cette procédure illustre la création d’une ressource Détecteur d’anomalies. Puis nous aborderons l’extraction de l’image de conteneur associée. Enfin, nous mettrons en avant la possibilité d’exercer l’orchestration des deux à partir d’un navigateur. L’utilisation de conteneurs peut détourner l’attention des développeurs de la gestion de l’infrastructure, au lieu de se concentrer sur le développement d’applications.

[!INCLUDE [Prerequisites](../containers/includes/container-prerequisites.md)]

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="create-an-azure-file-share"></a>Crée un partage de fichiers Azure

Le conteneur LUIS requiert un fichier de modèle `.gz` extrait au moment du runtime. Le conteneur doit être en mesure d’accéder à ce fichier de modèle via un montage de volume à partir de l’instance de conteneur. Pour plus d’informations sur la création d’un partage de fichiers Azure, voir [Créer un partage de fichiers](../../storage/files/storage-how-to-create-file-share.md). Notez le nom du compte de stockage Azure, la clé et le nom du partage de fichiers, car vous en aurez besoin plus tard.

### <a name="export-and-upload-packaged-luis-app"></a>Exporter et télécharger l’application LUIS empaquetée

Pour charger le modèle LUIS (application empaquetée) dans le partage de fichiers Azure, vous devez <a href="luis-container-howto.md#export-packaged-app-from-luis" target="_blank" rel="noopener">commencer par l’exporter à partir du portail LUIS <span class="docon docon-navigate-external x-hidden-focus"></span></a>. À partir du portail Azure, accédez à la page **Vue d’ensemble** de la ressource du compte de stockage, puis sélectionnez **Partages de fichiers**. Sélectionnez le nom du partage de fichiers que vous avez créé récemment, puis sélectionnez le bouton **Charger**.

> [!div class="mx-imgBorder"]
> ![Charger dans le partage de fichiers](media/luis-how-to-deploy-to-aci/upload-file-share.png)

Chargez le fichier de modèle LUIS.

[!INCLUDE [Create LUIS Container instance resource](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Next steps](../containers/includes/containers-next-steps.md)]