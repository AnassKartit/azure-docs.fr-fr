---
title: Compétence cognitive Sentiment (v3)
titleSuffix: Azure Cognitive Search
description: Fournit des étiquettes de sentiment pour le texte d’un pipeline d’enrichissement de l’IA dans la Recherche cognitive Azure.
manager: jennmar
author: careyjmac
ms.author: chalton
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 08/12/2021
ms.openlocfilehash: 4baadea0d78db64d97d1ae94317d7d506a141f41
ms.sourcegitcommit: 6c6b8ba688a7cc699b68615c92adb550fbd0610f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122532944"
---
# <a name="sentiment-cognitive-skill-v3"></a>Compétence cognitive Sentiment (v3)

La compétence **Sentiment** v3 évalue du texte non structuré. Pour chaque enregistrement, elle fournit des étiquettes de sentiment (par exemple « négatif », « neutre » et « positif ») en fonction du score de confiance le plus élevé trouvé par le service au niveau de la phrase et du document. Elle utilise les modèles Machine Learning fournis par la version 3 du service [Analyse de texte](../cognitive-services/text-analytics/overview.md) dans Cognitive Services. Elle expose également [les capacités d’exploration des opinions de l’API Analyse de texte](../cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis.md#opinion-mining), qui fournit des informations plus granulaires sur les opinions relatives aux attributs des produits ou des services dans le texte.

> [!NOTE]
> Cette compétence est liée à Cognitive Services et nécessite [une ressource facturable](cognitive-search-attach-cognitive-services.md) pour les transactions dépassant 20 documents par indexeur et par jour. L'exécution des compétences intégrées est facturée au prix actuel du [paiement à l'utilisation de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/).
>

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.V3.SentimentSkill

## <a name="data-limits"></a>Limites de données
La taille maximale d’un enregistrement est de 5 000 caractères selon [`String.Length`](/dotnet/api/system.string.length). Si vous avez besoin de découper vos données avant de les envoyer à la compétence Sentiment, utilisez la [compétence Fractionnement du texte](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Paramètres de la compétence

Les paramètres respectent la casse.

| Nom du paramètre | Description |
|----------------|----------------------|
| `defaultLanguageCode` | (Facultatif) Code de langue à appliquer aux documents qui ne spécifient pas explicitement la langue. <br/> Voir la [Liste complète des langues prises en charge](../cognitive-services/text-analytics/language-support.md). |
| `modelVersion`   | (Facultatif) Version du modèle à utiliser pour appeler le service Analyse de texte. Si rien n’est spécifié, c’est la dernière version qui est utilisée par défaut. Nous vous recommandons de ne pas spécifier cette valeur, sauf nécessité absolue. Pour plus d’informations, consultez [Contrôle de version de modèle dans l’API Analyse de texte](../cognitive-services/text-analytics/concepts/model-versioning.md). |
| `includeOpinionMining` | Si `true`, active la [fonctionnalité d’exploration des opinions du service Analyse de texte](../cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis.md#opinion-mining), qui permet d’inclure l’analyse des sentiments basés sur l’aspect dans les résultats de sortie. La valeur par défaut est `false`. |

## <a name="skill-inputs"></a>Entrées de la compétence 

| Nom d’entrée | Description |
|--------------------|-------------|
| `text` | Texte à analyser.|
| `languageCode`    |  (Facultatif) Chaîne indiquant la langue des enregistrements. Si ce paramètre n’est pas spécifié, la valeur par défaut est « en ». <br/>Voir la [Liste complète des langues prises en charge](../cognitive-services/text-analytics/language-support.md).|

## <a name="skill-outputs"></a>Sorties de la compétence

| Nom de sortie | Description |
|--------------------|-------------|
| `sentiment` | Valeur de chaîne représentant l’étiquette de sentiment de l’ensemble du texte analysé (positif, neutre ou négatif). |
| `confidenceScores` | Type complexe avec trois valeurs doubles, une pour l’évaluation positive, une autre pour l’évaluation neutre et la dernière pour l’évaluation négative. Les valeurs sont comprises entre 0 et 1,00, où 1,00 représente la confiance la plus élevée possible dans une attribution d’étiquette donnée. |
| `sentences` | Collection de types complexes qui décompose le sentiment du texte phrase par phrase. C’est également là que les résultats de l’exploration des opinions sont retournés sous la forme de cibles et d’évaluations si `includeOpinionMining` a la valeur `true`.|


##  <a name="sample-definition"></a>Exemple de définition

```json
{
    "@odata.type": "#Microsoft.Skills.Text.V3.SentimentSkill",
    "includeOpinionMining": true,
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/languageCode"
        }
    ],
    "outputs": [
        {
            "name": "sentiment",
            "targetName": "sentiment"
        },
        {
            "name": "confidenceScores",
            "targetName": "confidenceScores"
        },
        {
            "name": "sentences",
            "targetName": "sentences"
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
                "sentiment": "negative",
                "confidenceScores": {
                    "positive": 0.0,
                    "neutral": 0.0,
                    "negative": 1.0
                },
                "sentences": [
                    {
                        "text": "I had a terrible time at the hotel.",
                        "sentiment": "negative",
                        "confidenceScores": {
                            "positive": 0.0,
                            "neutral": 0.0,
                            "negative": 1.0
                        },
                        "offset": 0,
                        "length": 35,
                        "targets": [],
                        "assessments": [],
                    },
                    {
                        "text": "The staff was rude and the food was awful.",
                        "sentiment": "negative",
                        "confidenceScores": {
                            "positive": 0.0,
                            "neutral": 0.0,
                            "negative": 1.0
                        },
                        "offset":36,
                        "length": 42,
                        "targets": [
                            {
                                "text": "staff",
                                "sentiment": "negative",
                                "confidenceScores": {
                                    "positive": 0.0,
                                    "neutral": 0.0,
                                    "negative": 1.0
                                },
                                "offset": 40,
                                "length": 5,
                                "relations": [
                                    {
                                        "relationType": "assessment",
                                        "ref": "#/documents/0/sentences/1/assessments/0",
                                    }
                                ]
                            },
                            {
                                "text": "food",
                                "sentiment": "negative",
                                "confidenceScores": {
                                    "positive": 0.0,
                                    "neutral": 0.0,
                                    "negative": 1.0
                                },
                                "offset": 63,
                                "length": 4,
                                "relations": [
                                    {
                                        "relationType": "assessment",
                                        "ref": "#/documents/0/sentences/1/assessments/1",
                                    }
                                ]
                            }
                        ],
                        "assessments": [
                            {
                                "text": "rude",
                                "sentiment": "negative",
                                "confidenceScores": {
                                    "positive": 0.0,
                                    "neutral": 0.0,
                                    "negative": 1.0
                                },
                                "offset": 50,
                                "length": 4,
                                "isNegated": false
                            },
                            {
                                "text": "awful",
                                "sentiment": "negative",
                                "confidenceScores": {
                                    "positive": 0.0,
                                    "neutral": 0.0,
                                    "negative": 1.0
                                },
                                "offset": 72,
                                "length": 5,
                                "isNegated": false
                            }
                        ],
                    }
                ]
            }
        }
    ]
}
```

## <a name="warning-cases"></a>Cas d’avertissement
Si le texte est vide ou
que la langue n’est pas prise en charge, un avertissement est généré. Aucun résultat de sentiment n’est retourné.

## <a name="see-also"></a>Voir aussi

+ [Compétences prédéfinies](cognitive-search-predefined-skills.md)
+ [Guide pratique pour définir un ensemble de compétences](cognitive-search-defining-skillset.md)