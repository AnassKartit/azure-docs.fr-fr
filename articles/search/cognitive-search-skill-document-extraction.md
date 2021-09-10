---
title: Compétence cognitive Extraction de document
titleSuffix: Azure Cognitive Search
description: Extrait le contenu d’un fichier dans le pipeline d’enrichissement.
manager: nitinme
author: careyjmac
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 08/12/2021
ms.author: chalton
ms.openlocfilehash: 02d3431ecc7a5c460be75885fd786b3b4c4d276f
ms.sourcegitcommit: 6c6b8ba688a7cc699b68615c92adb550fbd0610f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122525384"
---
# <a name="document-extraction-cognitive-skill"></a>Compétence cognitive Extraction de document

La compétence **Extraction de document** extrait le contenu d’un fichier dans le pipeline d’enrichissement. Cela vous permet de tirer parti de l’étape d’extraction de document qui se produit normalement avant l’exécution des compétences avec des fichiers qui peuvent être générés par d’autres compétences.

> [!NOTE]
> Cette compétence n’est pas liée à Cognitive Services et n’a pas d’exigence de clé Cognitive Services.
> Cette compétence permet d’extraire du texte et des images. L’extraction de texte est gratuite. L’extraction d’images est [mesurée par Recherche cognitive Azure](https://azure.microsoft.com/pricing/details/search/). Sur un service de recherche gratuit, le coût de 20 transactions par indexeur par jour est absorbé, ce qui vous permet de suivre des démarrages rapides, des tutoriels, etc., gratuitement. Pour les niveaux tarifaires De base, Standard et supérieurs, l’extraction d’images est facturée.
>

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.DocumentExtractionSkill

## <a name="supported-document-formats"></a>Formats de document pris en charge

DocumentExtractionSkill peut extraire du texte à partir des formats de document suivants :

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

## <a name="skill-parameters"></a>Paramètres de la compétence

Les paramètres respectent la casse.

| Entrées | Valeurs autorisées | Description |
|-----------------|----------------|-------------|
| `parsingMode`   | `default` <br/> `text` <br/> `json`  | Définissez ce paramètre sur `default` pour l’extraction de documents à partir de fichiers qui ne sont pas en mode texte pur ou JSON. Définissez ce paramètre sur `text` pour améliorer les performances des fichiers en texte brut. Définissez ce paramètre sur `json` pour extraire le contenu structuré des fichiers JSON. Si `parsingMode` n’est pas défini explicitement, il sera défini sur `default`. |
| `dataToExtract` | `contentAndMetadata` <br/> `allMetadata` | Définissez ce paramètre sur `contentAndMetadata` pour extraire toutes les métadonnées et le contenu textuel de chaque fichier. Définissez ce paramètre sur `allMetadata` pour extraire uniquement les [propriétés de métadonnées pour le type de contenu](search-blob-metadata-properties.md) (par exemple, seules les métadonnées propres aux fichiers .png). Si `dataToExtract` n’est pas défini explicitement, il sera défini sur `contentAndMetadata`. |
| `configuration` | Voir ci-dessous. | Dictionnaire de paramètres facultatifs qui ajustent le mode d’exécution de l’extraction de document. Reportez-vous au tableau ci-dessous pour consulter une description des propriétés de configuration prises en charge. |

| Paramètre de configuration   | Valeurs autorisées | Description |
|-------------------------|----------------|-------------|
| `imageAction`           | `none`<br/> `generateNormalizedImages`<br/> `generateNormalizedImagePerPage` | Définissez ce paramètre sur `none` pour ignorer les images incorporées ou les fichiers image du jeu de données. Il s’agit de la valeur par défaut. <br/>Pour l’[analyse d’image à l’aide de compétences cognitives](cognitive-search-concept-image-scenarios.md), définissez ce paramètre sur `generateNormalizedImages` pour que la compétence crée un tableau d’images normalisées dans le cadre du [craquage de documents](search-indexer-overview.md#document-cracking). Cette action nécessite de définir le paramètre `parsingMode` sur `default` et le paramètre `dataToExtract` sur `contentAndMetadata`. Une image normalisée fait référence à un traitement supplémentaire qui entraîne une sortie d’image uniforme, dimensionnée et pivotée pour promouvoir un rendu cohérent lorsque vous incluez des images dans les résultats d’une recherche visuelle (par exemple, des photographies de même taille dans un contrôle de graphique comme illustré dans la [démonstration JFK](https://github.com/Microsoft/AzureSearch_JFK_Files)). Ces informations sont générées pour chaque image lorsque vous utilisez cette option.  <br/>Si vous définissez le paramètre sur `generateNormalizedImagePerPage`, les fichiers PDF sont traités différemment. Au lieu d’extraire les images incorporées, chaque page est rendue sous la forme d’une image et normalisée en conséquence.  Les autres types de fichiers sont traités de la même façon que si `generateNormalizedImages` était défini.
| `normalizedImageMaxWidth` | Entier compris entre 50 et 10000 | Largeur maximale (en pixels) des images normalisées générées. La valeur par défaut est 2000. | 
| `normalizedImageMaxHeight` | Entier compris entre 50 et 10000 | Hauteur maximale (en pixels) des images normalisées générées. La valeur par défaut est 2000. |

> [!NOTE]
> La valeur par défaut de 2000 pixels pour la hauteur et la largeur maximales des images normalisées est basée sur les tailles maximales prises en charge par la [compétence de reconnaissance optique de caractères](cognitive-search-skill-ocr.md) et la [compétence d’analyse d’image](cognitive-search-skill-image-analysis.md). La [compétence de reconnaissance optique de caractères](cognitive-search-skill-ocr.md) (OCR) prend en charge une largeur et une hauteur maximales de 4200 pour les langues autres que l'anglais et de 10000 pour l'anglais.  Si vous augmentez les limites maximales, le traitement des images plus volumineuses peut échouer en fonction de la définition de vos compétences et de la langue des documents. 
## <a name="skill-inputs"></a>Entrées de la compétence

| Nom d’entrée     | Description |
|--------------------|-------------|
| `file_data` | Fichier à partir duquel le contenu doit être extrait. |

L’entrée « file_data » doit être un objet défini comme suit :

```json
{
  "$type": "file",
  "data": "BASE64 encoded string of the file"
}
```

or

```json
{
  "$type": "file",
  "url": "URL to download file",
  "sasToken": "OPTIONAL: SAS token for authentication if the URL provided is for a file in blob storage"
}
```

Cet objet de référence de fichier peut être généré de 3 manières :

 - En définissant le paramètre `allowSkillsetToReadFileData` de votre définition d’indexeur sur la valeur « true ».  Ceci permet de créer un chemin `/document/file_data` qui est un objet représentant les données du fichier d’origine téléchargées à partir de votre source de données d’objets blob. Ce paramètre s’applique uniquement aux données dans le stockage d’objets BLOB.

 - En définissant le paramètre `imageAction` de votre définition d’indexeur sur autre valeur que `none`.  Ceci crée un tableau d’images qui suivent la convention nécessaire pour les entrées de cette compétence, si elles sont passées individuellement (c’est-à-dire `/document/normalized_images/*`).

 - Avoir une compétence personnalisée renvoie un objet JSON défini exactement comme indiqué ci-dessus.  Le paramètre `$type` doit être défini exactement sur `file` et le paramètre `data` doit correspondre aux données de tableau d’octets encodé en base 64 du fichier de contenu, ou le paramètre `url` doit être une URL au format correct avec un accès pour télécharger le fichier à cet emplacement.

## <a name="skill-outputs"></a>Sorties de la compétence

| Nom de sortie    | Description |
|--------------|-------------|
| `content` | Contenu textuel du document. |
| `normalized_images`   | Si la propriété `imageAction` est définie sur une autre valeur que `none`, le nouveau champ *normalized_images* contiendra un tableau d’images. Si vous souhaitez en savoir plus sur le format de sortie de chaque image, veuillez consulter la [documentation relative à l’extraction d’images](cognitive-search-concept-image-scenarios.md). |

##  <a name="sample-definition"></a>Exemple de définition

```json
 {
    "@odata.type": "#Microsoft.Skills.Util.DocumentExtractionSkill",
    "parsingMode": "default",
    "dataToExtract": "contentAndMetadata",
    "configuration": {
        "imageAction": "generateNormalizedImages",
        "normalizedImageMaxWidth": 2000,
        "normalizedImageMaxHeight": 2000
    },
    "context": "/document",
    "inputs": [
      {
        "name": "file_data",
        "source": "/document/file_data"
      }
    ],
    "outputs": [
      {
        "name": "content",
        "targetName": "extracted_content"
      },
      {
        "name": "normalized_images",
        "targetName": "extracted_normalized_images"
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
        "file_data": {
          "$type": "file",
          "data": "aGVsbG8="
        }
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
        "content": "hello",
        "normalized_images": []
      }
    }
  ]
}
```

## <a name="see-also"></a>Voir aussi

+ [Compétences prédéfinies](cognitive-search-predefined-skills.md)
+ [Guide pratique pour définir un ensemble de compétences](cognitive-search-defining-skillset.md)
+ [Comment traiter et extraire des informations d’images dans des scénarios de recherche cognitive](cognitive-search-concept-image-scenarios.md)