---
title: 'Démarrage rapide : Détecter des visages dans une image à l’aide de l’API REST Azure et de Java'
titleSuffix: Azure Cognitive Services
description: Dans ce guide de démarrage rapide, vous allez utiliser l’API REST Visage Azure avec Java pour détecter des visages dans une image.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/23/2020
ms.custom: devx-track-java
ms.author: pafarley
ms.openlocfilehash: b8ea2908711497db5e86a7dd665548e33b9d9f25
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/24/2020
ms.locfileid: "96020855"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-java"></a>Démarrage rapide : Détecter des visages sur une image avec l’API REST et Java

Dans ce guide de démarrage rapide, vous allez utiliser l’API REST Visage Azure avec Java pour détecter des visages humains dans une image.

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/cognitive-services/) avant de commencer. 

## <a name="prerequisites"></a>Prérequis

* Abonnement Azure - [En créer un gratuitement](https://azure.microsoft.com/free/cognitive-services/)
* Une fois que vous avez votre abonnement Azure, <a href="https://portal.azure.com/#create/Microsoft.CognitiveServicesFace"  title="créez une ressource Visage"  target="_blank">créer une ressource Visage <span class="docon docon-navigate-external x-hidden-focus"></span></a> dans le Portail Azure pour obtenir votre clé et votre point de terminaison. Une fois le déploiement effectué, cliquez sur **Accéder à la ressource**.
    * Vous aurez besoin de la clé et du point de terminaison de la ressource que vous créez pour connecter votre application à l’API Visage. Vous collerez votre clé et votre point de terminaison dans le code ci-dessous plus loin dans le guide de démarrage rapide.
    * Vous pouvez utiliser le niveau tarifaire Gratuit (`F0`) pour tester le service, puis passer par la suite à un niveau payant pour la production.
* Un IDE Java de votre choix.

## <a name="create-the-java-project"></a>Créer le projet Java

1. Créez une application Java en ligne de commande dans votre IDE, puis ajoutez une classe **Main** avec une méthode **main**.
1. Importez les bibliothèques suivantes dans votre projet Java. Si vous utilisez Maven, les coordonnées Maven sont fournies pour chaque bibliothèque.
   - [Client HTTP Apache](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpclient:4.5.6)
   - [Noyau HTTP Apache](https://hc.apache.org/downloads.cgi) (org.apache.httpcomponents:httpcore:4.4.10)
   - [Bibliothèque JSON](https://github.com/stleary/JSON-java) (org.json:json:20180130)
   - [Journalisation Apache Commons](https://commons.apache.org/proper/commons-logging/download_logging.cgi) (commons-logging:commons-logging:1.1.2)

## <a name="add-face-detection-code"></a>Ajoutez le code de détection de visage

Ouvrez la classe main de votre projet. Ici, vous allez ajouter le code nécessaire pour charger des images et détecter des visages.

### <a name="import-packages"></a>Importer des packages

Ajoutez les instructions `import` suivantes en haut du fichier.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="dependencies":::

### <a name="add-essential-fields"></a>Ajouter des champs essentiels

Remplacez la classe **Main** par le code suivant. Ces données spécifient comment se connecter au service Visage et où obtenir les données d’entrée. Vous devez mettre à jour le champ `subscriptionKey` avec la valeur de votre clé d’abonnement, et changer la chaîne `uriBase` pour qu’elle contienne la chaîne de point de terminaison correcte. Vous pouvez également définir la valeur `imageWithFaces` avec un chemin qui pointe vers un fichier image différent.

[!INCLUDE [subdomains-note](../../../../includes/cognitive-services-custom-subdomains-note.md)]

Le champ `faceAttributes` est simplement un tableau de certains types d’attributs. Il spécifie les informations à récupérer sur les visages détectés.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="environment":::

### <a name="call-the-face-detection-rest-api"></a>Appeler l’API REST de détection des visages

Ajoutez la méthode **main** avec le code suivant. Elle construit un appel REST à l’API Visage pour détecter les informations de visage dans l’image distante (la chaîne `faceAttributes` spécifie les attributs de visage à récupérer). Elle écrit ensuite les données de sortie dans une chaîne JSON.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="main":::

### <a name="parse-the-json-response"></a>Analyser la réponse JSON

Juste en dessous du code précédent, ajoutez le bloc suivant. Celui-ci convertit les données JSON retournées dans un format plus facile à lire avant de les imprimer dans la console. Enfin, fermez le bloc try-catch, la méthode **main** et la classe **Main**.

:::code language="java" source="~/cognitive-services-quickstart-code/java/Face/rest/detect.java" id="print":::

## <a name="run-the-app"></a>Exécuter l’application

Compilez le code et exécutez-le. Une réponse correcte affiche les données Visage dans un format JSON facile à lire dans la fenêtre de console. Par exemple :

```json
[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  }
}]
```

## <a name="extract-face-attributes"></a>Extraire des attributs de visage
 
Pour extraire des attributs de visage, utilisez le modèle de détection 1 et ajoutez le paramètre de requête `returnFaceAttributes`.

```java
builder.setParameter("detectionModel", "detection_01");
builder.setParameter("returnFaceAttributes", "age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise");
```

La réponse comprend désormais des attributs de visage. Par exemple :

```json
[{
  "faceRectangle": {
    "top": 131,
    "left": 177,
    "width": 162,
    "height": 162
  },
  "faceAttributes": {
    "makeup": {
      "eyeMakeup": true,
      "lipMakeup": true
    },
    "facialHair": {
      "sideburns": 0,
      "beard": 0,
      "moustache": 0
    },
    "gender": "female",
    "accessories": [],
    "blur": {
      "blurLevel": "low",
      "value": 0.06
    },
    "headPose": {
      "roll": 0.1,
      "pitch": 0,
      "yaw": -32.9
    },
    "smile": 0,
    "glasses": "NoGlasses",
    "hair": {
      "bald": 0,
      "invisible": false,
      "hairColor": [
        {
          "color": "brown",
          "confidence": 1
        },
        {
          "color": "black",
          "confidence": 0.87
        },
        {
          "color": "other",
          "confidence": 0.51
        },
        {
          "color": "blond",
          "confidence": 0.08
        },
        {
          "color": "red",
          "confidence": 0.08
        },
        {
          "color": "gray",
          "confidence": 0.02
        }
      ]
    },
    "emotion": {
      "contempt": 0,
      "surprise": 0.005,
      "happiness": 0,
      "neutral": 0.986,
      "sadness": 0.009,
      "disgust": 0,
      "anger": 0,
      "fear": 0
    },
    "exposure": {
      "value": 0.67,
      "exposureLevel": "goodExposure"
    },
    "occlusion": {
      "eyeOccluded": false,
      "mouthOccluded": false,
      "foreheadOccluded": false
    },
    "noise": {
      "noiseLevel": "low",
      "value": 0
    },
    "age": 22.9
  },
  "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f"
}]
```

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez créé une application console Java simple qui utilise des appels REST à l’API Visage Azure pour détecter des visages dans une image et retourner leurs attributs. Explorez à présent la documentation de référence sur l’API Visage pour en savoir plus sur les scénarios pris en charge.

> [!div class="nextstepaction"]
> [API Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
