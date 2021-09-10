---
title: Appeler l’API de détection – Visage
titleSuffix: Azure Cognitive Services
description: Ce guide explique comment utiliser la détection des visages pour extraire des attributs tels que l’âge, les émotions ou la position de la tête à partir d’une image donnée.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 08/04/2021
ms.author: pafarley
ms.custom: devx-track-csharp
ms.openlocfilehash: 1fe1cddcbd194ec4b8ca25edfc4907631cb18df1
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122562988"
---
# <a name="call-the-detect-api"></a>Appeler l’API de détection

Ce guide explique comment utiliser l’API de détection des visages pour extraire des attributs tels que l’âge, les émotions ou la position de la tête à partir d’une image donnée. Vous découvrirez les différentes façons de configurer le comportement de cette API pour répondre à vos besoins.

Les extraits de code de ce guide sont écrits en C# en utilisant la bibliothèque cliente Visage d’Azure Cognitive Services. La même fonctionnalité est disponible via l’[API REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).


## <a name="setup"></a>Programme d’installation

Ce guide suppose que vous avez déjà construit un objet [FaceClient](/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient), nommé `faceClient`, avec une URL de point de terminaison et une clé d’abonnement Visage. Pour obtenir des instructions sur la configuration de cette fonctionnalité, suivez un des guides de démarrage rapide.

## <a name="submit-data-to-the-service"></a>Envoyer des données au service

Pour rechercher des visages et savoir où ils se trouvent sur une image, appelez la méthode [DetectWithUrlAsync](/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync) ou [DetectWithStreamAsync](/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync). **DetectWithUrlAsync** prend une chaîne URL en entrée, et **DetectWithStreamAsync** prend le flux d’octets brut d’une image en entrée.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="basic1":::

Vous pouvez interroger les objets [DetectedFace](/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface) retournés pour obtenir leurs ID uniques et un rectangle qui fournit les coordonnées en pixels du visage. De cette façon, vous pouvez savoir quel ID de visage qui correspond à quel visage dans l’image d’origine.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="basic2":::

Pour plus d’informations sur la façon d’analyser l’emplacement et les dimensions du visage, consultez [FaceRectangle](/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle). En règle générale, ce rectangle contient les yeux, les sourcils, le nez et la bouche. Le sommet du crâne, les oreilles et le menton ne sont pas nécessairement inclus. Pour utiliser le rectangle du visage pour rogner une tête complète ou obtenir un portrait en plan moyen, vous devez étendre le rectangle dans chaque direction.

## <a name="determine-how-to-process-the-data"></a>Déterminer le mode de traitement des données

Ce guide se concentre sur les spécificités de l’appel de détection, telles que les arguments que vous pouvez transmettre et ce que vous pouvez faire avec les données retournées. Nous vous recommandons d’interroger uniquement les fonctionnalités dont vous avez besoin. Chaque opération prend plus de temps.

### <a name="get-face-landmarks"></a>Obtenir les points de repère du visage

Les [points de repère de visage](../concepts/face-detection.md#face-landmarks) sont un ensemble de points faciles à trouver sur un visage, tels que les pupilles ou la pointe du nez. Pour obtenir les données des points de repère de visage, définissez le paramètre _detectionModel_ sur `DetectionModel.Detection01` et le paramètre _returnFaceLandmarks_ sur `true`.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="landmarks1":::

### <a name="get-face-attributes"></a>Obtenir les attributs du visage

Outre les points de repère et les rectangles des visages, l’API de détection des visages peut analyser plusieurs attributs conceptuels d’un visage. Pour obtenir la liste complète, consultez la section conceptuelle [Attributs du visage](../concepts/face-detection.md#attributes).

Pour analyser les attributs d’un visage, définissez le paramètre _detectionModel_ sur `DetectionModel.Detection01` et le paramètre _returnFaceAttributes_ sur une liste de valeurs [enum FaceAttributeType](/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype).

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="attributes1":::


## <a name="get-results-from-the-service"></a>Obtenir les résultats du service

### <a name="face-landmark-results"></a>Résultats des points de repère de visage

Le code suivant montre comment récupérer les emplacements du nez et des pupilles :

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="landmarks2":::

Vous pouvez également utiliser les données des points de repère de visage pour calculer avec précision l’orientation du visage. Par exemple, vous pouvez définir la rotation du visage en tant que vecteur allant du centre de la bouche jusqu’au centre des yeux. Le code suivant calcule ce vecteur :

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="direction":::

Lorsque vous connaissez l’orientation du visage, vous pouvez faire pivoter le cadre rectangulaire du visage pour mieux l’aligner. Pour rogner les visages dans une image, vous pouvez faire pivoter l’image par programmation afin que les visages apparaissent toujours verticaux.


### <a name="face-attribute-results"></a>Résultats des attributs du visage

Le code suivant montre comment vous pouvez récupérer les données d’attributs du visage que vous avez demandées dans l’appel d’origine.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="attributes2":::

Pour en savoir plus sur les différents attributs, consultez le guide conceptuel [Détection et attributs de visage](../concepts/face-detection.md).

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide, vous avez appris à utiliser les différentes fonctionnalités de la détection et de l’analyse des visages. Ensuite, intégrez ces fonctionnalités à une application pour ajouter les données de visage associées aux utilisateurs.

- [Tutoriel : Ajouter des utilisateurs à un service Visage](../enrollment-overview.md)

## <a name="related-articles"></a>Articles connexes

- [Documentation de référence (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Documentation de référence (kit SDK .NET)](/dotnet/api/overview/azure/cognitiveservices/face-readme)
