---
title: Compétence cognitive Reconnaissance d’entités nommées
titleSuffix: Azure Cognitive Search
description: Extrait les entités nommées de personne, de lieu et d’organisation du texte dans un pipeline d’enrichissement par IA dans Recherche cognitive Azure.
author: LiamCavanagh
ms.author: liamca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 08/12/2021
ms.openlocfilehash: 0b9de4c1abc492a24dee26dd05e7a164239c8f9b
ms.sourcegitcommit: 6c6b8ba688a7cc699b68615c92adb550fbd0610f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122525372"
---
#   <a name="named-entity-recognition-cognitive-skill"></a>Compétence cognitive Reconnaissance d’entités nommées

La compétence **Reconnaissance d’entités nommées** extrait les entités nommées du texte. Sont notamment disponibles les types d’entités suivants : `person`, `location` et `organization`.

> [!IMPORTANT]
> La compétence de reconnaissance des entités nommées est désormais remplacée par [Microsoft.Skills.Text.V3.EntityRecognitionSkill](cognitive-search-skill-entity-recognition-v3.md). Suivez les recommandations de la page [Compétences de recherche cognitive déconseillées](cognitive-search-skill-deprecated.md) pour migrer vers une compétence prise en charge.

 > [!NOTE]
> Si vous élargissez le champ en augmentant la fréquence des traitements, en ajoutant des documents supplémentaires ou en ajoutant plusieurs algorithmes d’IA, vous devez [attacher une ressource Cognitive Services facturable](cognitive-search-attach-cognitive-services.md). Des frais s’appliquent durant l’appel des API dans Cognitive Services ainsi que pour l’extraction d’images dans le cadre de la phase de craquage de document de la Recherche cognitive Azure. L’extraction de texte à partir des documents est gratuite. L'exécution des compétences intégrées est facturée au prix actuel du [paiement à l'utilisation de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/).
> 
> L’extraction d’images est un supplément facturé par Recherche cognitive Azure, comme décrit sur la [page de tarification](https://azure.microsoft.com/pricing/details/search/). L’extraction de texte est gratuite.
>


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="data-limits"></a>Limites de données
La taille maximale d’un enregistrement doit être de 50 000 caractères telle que mesurée par [`String.Length`](/dotnet/api/system.string.length). Si vous devez subdiviser vos données avant de les envoyer à l’extracteur de phrases clés, envisagez d’utiliser la [compétence Fractionnement de texte](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Paramètres de la compétence

Les paramètres respectent la casse.

| Nom du paramètre     | Description |
|--------------------|-------------|
| categories    | Tableau des catégories à extraire.  Types de catégories possibles : `"Person"`, `"Location"`, `"Organization"`. Si aucune catégorie n’est précisée, tous les types sont retournés.|
|defaultLanguageCode |  Code de langue du texte d’entrée. Langues prises en charge : `de, en, es, fr, it`.|
| minimumPrecision  | Nombre compris entre 0 et 1. Si la précision est inférieure à cette valeur, l’entité n’est pas retournée. La valeur par défaut est 0.|

## <a name="skill-inputs"></a>Entrées de la compétence

| Nom d’entrée      | Description                   |
|---------------|-------------------------------|
| languageCode  | facultatif. La valeur par défaut est `"en"`.  |
| text          | Texte à analyser.          |

## <a name="skill-outputs"></a>Sorties de la compétence

| Nom de sortie     | Description                   |
|---------------|-------------------------------|
| persons      | Tableau de chaînes représentant chacune le nom d’une personne. |
| locations  | Tableau de chaînes représentant chacune un lieu. |
| organizations  | Tableau de chaînes représentant chacune une organisation. |
| entities | Tableau de types complexes. Chaque type complexe contient les champs suivants : <ul><li>la catégorie (`"person"`, `"organization"` ou `"location"`) ;</li> <li>la valeur (le nom réel de l’entité) ;</li><li>le décalage (l’emplacement où elle a été trouvée dans le texte) ;</li><li>la confiance (une valeur comprise entre 0 et 1 représentant la confiance accordée à la valeur en tant qu’entité réelle).</li></ul> |

##  <a name="sample-definition"></a>Exemple de définition

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person", "Location", "Organization"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
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
        "data":
           {
             "text": "This is the loan application for Joe Romero, a Microsoft employee who was born in Chile and who then moved to Australia… Ana Smith is provided as a reference.",
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
      "data" : 
      {
        "persons": [ "Joe Romero", "Ana Smith"],
        "locations": ["Chile", "Australia"],
        "organizations":["Microsoft"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 33,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 124,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Chile",
            "offset": 88,
            "confidence": 0.99
          },
          {
            "category":"location",
            "value": "Australia",
            "offset": 112,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft",
            "offset": 54,
            "confidence": 0.99
          }
        ]
      }
    }
  ]
}
```


## <a name="warning-cases"></a>Cas d’avertissement
Si le code de langue du document n’est pas pris en charge, un avertissement est retourné et aucune entité n’est extraite.

## <a name="see-also"></a>Voir aussi

+ [Compétences prédéfinies](cognitive-search-predefined-skills.md)
+ [Guide pratique pour définir un ensemble de compétences](cognitive-search-defining-skillset.md)
+ [Compétence Reconnaissance d’entités (V3)](cognitive-search-skill-entity-recognition-v3.md)