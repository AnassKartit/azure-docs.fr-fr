---
title: Compétence cognitive Sentiment
titleSuffix: Azure Cognitive Search
description: Extrayez le score de sentiment positif/négatif d’un texte dans un pipeline d’enrichissement de l’IA dans la Recherche cognitive Azure.
author: LiamCavanagh
ms.author: liamca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/17/2021
ms.openlocfilehash: b750e99d30c0b540a381ce9e9b32b228b4dceaf8
ms.sourcegitcommit: e8c34354266d00e85364cf07e1e39600f7eb71cd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/29/2021
ms.locfileid: "129210945"
---
# <a name="sentiment-cognitive-skill"></a>Compétence cognitive Sentiment

La compétence **Sentiment** évalue du texte non structuré sur un continuum positif-négatif et, pour chaque enregistrement, retourne un score numérique compris entre 0 et 1. Un score proche de 1 indique un sentiment positif, et un score proche de 0 un sentiment négatif. Cette compétence utilise les modèles Machine Learning fournis par [Analyse de texte](../cognitive-services/text-analytics/overview.md) dans Cognitive Services.

> [!IMPORTANT]
> La compétence Sentiment n’est plus disponible et est remplacée par [Microsoft.Skills.Text.V3.SentimentSkill](cognitive-search-skill-sentiment-v3.md). Suivez les recommandations de la page [Compétences de recherche cognitive déconseillées](cognitive-search-skill-deprecated.md) pour migrer vers une compétence prise en charge.

> [!NOTE]
> Si vous élargissez le champ en augmentant la fréquence des traitements, en ajoutant des documents supplémentaires ou en ajoutant plusieurs algorithmes d’IA, vous devez [attacher une ressource Cognitive Services facturable](cognitive-search-attach-cognitive-services.md). Des frais s’appliquent durant l’appel des API dans Cognitive Services ainsi que pour l’extraction d’images dans le cadre de la phase de craquage de document de la Recherche cognitive Azure. L’extraction de texte à partir des documents est gratuite.
>
> L'exécution des compétences intégrées est facturée au prix actuel du [paiement à l'utilisation de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/). Les prix appliqués pour l’extraction d’images sont présentés sur la [page de tarification du service Recherche cognitive Azure](https://azure.microsoft.com/pricing/details/search/).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Limites de données
La taille maximale d’un enregistrement est de 5 000 caractères selon [`String.Length`](/dotnet/api/system.string.length). Si vous avez besoin de découper vos données avant de les envoyer à l’Analyseur des sentiments, utilisez la [compétence Fractionnement du texte](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Paramètres de la compétence

Les paramètres respectent la casse.

| Nom du paramètre | Description |
|----------------|----------------------|
| `defaultLanguageCode` | (Facultatif) Code de langue à appliquer aux documents qui ne spécifient pas explicitement la langue. <br/> Voir la [Liste complète des langues prises en charge](../cognitive-services/text-analytics/language-support.md). |

## <a name="skill-inputs"></a>Entrées de la compétence 

| Nom d’entrée | Description |
|--------------------|-------------|
| `text` | Texte à analyser.|
| `languageCode`    |  (Facultatif) Chaîne indiquant la langue des enregistrements. Si ce paramètre n’est pas spécifié, la valeur par défaut est « en ». <br/>Voir la [Liste complète des langues prises en charge](../cognitive-services/text-analytics/language-support.md).|

## <a name="skill-outputs"></a>Sorties de la compétence

| Nom de sortie | Description |
|--------------------|-------------|
| `score` | Valeur comprise entre 0 et 1 qui représente le sentiment du texte analysé. Les valeurs proches de 0 indiquent un sentiment négatif, les valeurs proches de 0,5 un sentiment neutre et les valeurs proches de 1 un sentiment positif.|


##  <a name="sample-definition"></a>Exemple de définition

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/languagecode"
        }
    ],
    "outputs": [
        {
            "name": "score",
            "targetName": "mySentiment"
        }
    ]
}
```

##  <a name="sample-input"></a>Exemple d’entrée

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "I had a terrible time at the hotel. The staff was rude and the food was awful.",
                "languageCode": "en"
            }
        }
    ]
}
```


##  <a name="sample-output"></a>Exemple de sortie

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "score": 0.01
            }
        }
    ]
}
```

## <a name="warning-cases"></a>Cas d’avertissement
Si votre texte est vide, un avertissement est généré et aucun score de sentiment n’est renvoyé.
Si la langue n’est pas prise en charge, un avertissement est généré et aucun score de sentiment n’est renvoyé.

## <a name="see-also"></a>Voir aussi

+ [Compétences prédéfinies](cognitive-search-predefined-skills.md)
+ [Guide pratique pour définir un ensemble de compétences](cognitive-search-defining-skillset.md)
+ [Compétence Sentiment (V3)](cognitive-search-skill-sentiment-v3.md)