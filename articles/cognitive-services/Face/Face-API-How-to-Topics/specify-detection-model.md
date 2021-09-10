---
title: Guide pratique pour spécifier un modèle de détection – Visage
titleSuffix: Azure Cognitive Services
description: Cet article vous montre comment choisir le modèle de détection de visages à utiliser avec votre application Azure Visage.
services: cognitive-services
author: yluiu
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 03/05/2021
ms.author: yluiu
ms.custom: devx-track-csharp
ms.openlocfilehash: b933829ec9cfdb322cf0498c10966b9c244ac98e
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122531623"
---
# <a name="specify-a-face-detection-model"></a>Spécifier un modèle de détection des visages

Ce guide vous montre comment spécifier un modèle de détection de visages pour le service Visage d’Azure.

Le service Visage utilise des modèles Machine Learning pour effectuer des opérations sur les visages humains présents dans les images. Nous continuons d’améliorer la précision de nos modèles en fonction des commentaires de nos clients et des progrès de la recherche, et nous intégrons ces améliorations sous forme de mises à jour de modèles. Les développeurs peuvent spécifier la version du modèle de détection des visages qu'ils souhaitent utiliser ; ils peuvent choisir le modèle qui correspond le mieux à leur cas d'utilisation.

Poursuivez votre lecture pour savoir comment spécifier le modèle de détection de visage dans certaines opérations de visage. Le service Visage utilise la détection des visages chaque fois qu’elle convertit l’image d’un visage dans une autre forme de données.

Si vous hésitez à utiliser le modèle le plus récent, passez à la section [Évaluer des modèles différents](#evaluate-different-models) pour évaluer le nouveau modèle et comparer les résultats en utilisant votre ensemble de données actuel.

## <a name="prerequisites"></a>Prérequis

Vous devez maîtriser les concepts de la détection de visages par intelligence artificielle (AI). Si ce n’est pas le cas, consultez le guide conceptuel ou le guide pratique pour la détection de visage :

* [Concepts de détection de visage](../concepts/face-detection.md)
* [Appeler l’API de détection](HowtoDetectFacesinImage.md)

## <a name="detect-faces-with-specified-model"></a>Détecter des visages avec le modèle spécifié

La détection des visages identifie l’emplacement des visages humains dans les cadres englobants et trouve leurs repères visuels. Cela permet d’extraire les traits du visage et de les stocker pour une utilisation ultérieure dans les opérations de [reconnaissance](../concepts/face-recognition.md).

Lorsque vous utilisez l’API [Face - Detect], vous pouvez attribuer la version du modèle avec le paramètre `detectionModel`. Les valeurs disponibles sont :

* `detection_01`
* `detection_02`
* `detection_03`

Une URL de requête pour l’API REST [Face - Detect] se présentera comme suit :

`https://westus.api.cognitive.microsoft.com/face/v1.0/detect[?returnFaceId][&returnFaceLandmarks][&returnFaceAttributes][&recognitionModel][&returnRecognitionModel][&detectionModel]&subscription-key=<Subscription key>`

Si vous utilisez la bibliothèque cliente, vous pouvez attribuer la valeur de `detectionModel` en passant une chaîne appropriée. Si vous n’affectez pas de valeur, l’API utilise la version de modèle par défaut (`detection_01`). Consultez l'exemple de code suivant pour la bibliothèque cliente .NET.

```csharp
string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
var faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, false, false, recognitionModel: "recognition_04", detectionModel: "detection_03");
```

## <a name="add-face-to-person-with-specified-model"></a>Ajouter un visage à une personne avec le modèle spécifié

Le service Visage peut extraire les données de visage d’une image et les associer à un objet **Person** via l’API [PersonGroup Person - Add Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b). Dans cet appel d’API, vous pouvez spécifier le modèle de détection de la même façon que dans [Visage - Détecter].

Consultez l'exemple de code suivant pour la bibliothèque cliente .NET.

```csharp
// Create a PersonGroup and add a person with face detected by "detection_03" model
string personGroupId = "mypersongroupid";
await faceClient.PersonGroup.CreateAsync(personGroupId, "My Person Group Name", recognitionModel: "recognition_04");

string personId = (await faceClient.PersonGroupPerson.CreateAsync(personGroupId, "My Person Name")).PersonId;

string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
await client.PersonGroupPerson.AddFaceFromUrlAsync(personGroupId, personId, imageUrl, detectionModel: "detection_03");
```

Ce code crée un **PersonGroup** avec l’ID `mypersongroupid` et lui ajoute un objet **Person**. Puis il ajoute un visage à cet objet **Person** à l’aide du modèle `detection_03`. Si vous ne spécifiez pas le paramètre *detectionModel*, l’API utilise le modèle par défaut, `detection_01`.

> [!NOTE]
> Vous n’avez pas besoin d’utiliser le même modèle de détection pour tous les visages dans un objet **Person**, et vous n’avez pas besoin d’utiliser le même modèle de détection lors de la détection de nouveaux visage à comparer avec un objet **Person** (dans l’API [Visage - Identifier], par exemple).

## <a name="add-face-to-facelist-with-specified-model"></a>Ajouter un visage à FaceList avec le modèle spécifié

Vous pouvez également spécifier un modèle de détection lorsque vous ajoutez un visage à un objet **FaceList** existant. Consultez l'exemple de code suivant pour la bibliothèque cliente .NET.

```csharp
await faceClient.FaceList.CreateAsync(faceListId, "My face collection", recognitionModel: "recognition_04");

string imageUrl = "https://news.microsoft.com/ceo/assets/photos/06_web.jpg";
await client.FaceList.AddFaceFromUrlAsync(faceListId, imageUrl, detectionModel: "detection_03");
```

Ce code crée un objet **FaceList** appelé `My face collection` et lui ajoute un visage avec le modèle `detection_03`. Si vous ne spécifiez pas le paramètre *detectionModel*, l’API utilise le modèle par défaut, `detection_01`.

> [!NOTE]
> Vous n’avez pas besoin d’utiliser le même modèle de détection pour tous les visages dans un objet **FaceList**, et vous n’avez pas besoin d’utiliser le même modèle de détection lors de la détection de nouveaux visage à comparer avec un objet **FaceList**.

## <a name="evaluate-different-models"></a>Évaluer des modèles différents

Les différents modèles de détection sont optimisés pour différentes tâches. Consultez le tableau suivant pour une vue d’ensemble des différences.

|**detection_01**  |**detection_02**  |**detection_03** 
|---------|---------|---|
|Choix par défaut pour toutes les opérations de détection de visage. | Publié en mai 2019 et disponible si vous le souhaitez dans toutes les opérations de détection de visage. |  Publié en février 2021 et disponible si vous le souhaitez dans toutes les opérations de détection de visage.
|Non optimisé pour les visages petits, pris de côté ou flous.  | Amélioration de la précision pour les visages petits, pris de côté ou flous. | Amélioration de la précision, notamment sur les visages plus petits (64x64 pixels) et les orientations de visages tournés.
|Retourne les principaux attributs du visage (posture de la tête, âge, émotion, etc.) s’ils sont spécifiés dans l’appel de détection. |  Ne retourne pas les attributs du visage.     | Retourne les attributs de masque et de posture de tête s’ils sont spécifiés dans l’appel de détection.
|Retourne les points de repère du visage s’ils sont spécifiés dans l’appel de détection.   | Ne retourne pas les points de repère du visage.  | Retourne les points de repère du visage s’ils sont spécifiés dans l’appel de détection.

La meilleure façon de comparer les performances des modèles de détection consiste à les utiliser sur un exemple de jeu de données. Nous vous recommandons d’appeler l’API [Visage - Détecter] sur un grand nombre d’images, en particulier les images avec de nombreux visages ou des visages qui sont difficiles à voir, avec chaque modèle de détection. Prêtez attention au nombre de visages que chaque modèle retourne.

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous avez appris comment spécifier le modèle de détection des visages à utiliser avec différentes API Visage. Voici maintenant un démarrage rapide pour prendre en main la détection et l’analyse des visages.

* [SDK .NET Visage](../quickstarts/client-libraries.md?pivots=programming-language-csharp%253fpivots%253dprogramming-language-csharp)
* [Kit de développement logiciel (SDK) Face Python](../quickstarts/client-libraries.md?pivots=programming-language-python%253fpivots%253dprogramming-language-python)
* [Kit de développement logiciel (SDK) Go face Go](../quickstarts/client-libraries.md?pivots=programming-language-go%253fpivots%253dprogramming-language-go)

[Face - Detect]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d
[Face - Find Similar]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237
[Visage - Identifier]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239
[Face - Verify]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a
[PersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244
[PersonGroup - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246
[PersonGroup Person - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b
[PersonGroup - Train]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249
[LargePersonGroup - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d
[FaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b
[FaceList - Get]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c
[FaceList - Add Face]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250
[LargeFaceList - Create]: https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc