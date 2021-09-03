---
title: Cartes d’identité – Form Recognizer
titleSuffix: Azure Applied AI Services
description: Découvrez les concepts liés à l’extraction de données à partir de documents d’identité à l’aide de l’API Pre-built IDs de Form Recognizer.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 08/09/2021
ms.author: lajanuar
ms.openlocfilehash: bfa6cb13533a92c65369b8016971e0c1e37cdb61
ms.sourcegitcommit: 5f659d2a9abb92f178103146b38257c864bc8c31
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2021
ms.locfileid: "122534986"
---
# <a name="form-recognizer-prebuilt-identification-id-document-model"></a>Modèle de document d’identification (ID) prédéfini de Form Recognizer

Azure Form Recognizer peut analyser et extraire des informations des documents d’identification gouvernementaux à l’aide de son modèle d’ID prédéfini. Il combine nos puissantes fonctionnalités de [reconnaissance optique de caractères (OCR)](../../cognitive-services/computer-vision/overview-ocr.md) avec des fonctionnalités de reconnaissance d’identités pour extraire des informations clés de passeports internationaux et de permis de conduire émis aux États-Unis (50 États + Washington D.C.). L’API des cartes d’identité extrait les informations clés de ces documents d’identité, telles que le prénom, le nom, la date de naissance, le numéro du document, etc. Cette API est disponible dans Form Recognizer v2.1 sous forme de service cloud. 

## <a name="customer-scenarios"></a>Scénarios clients

Les données extraites avec l’API Cartes d’identité peuvent être utilisées pour effectuer diverses tâches dans des scénarios visant à identifier la clientèle dans des secteurs comme ceux de la finance, de la santé, de l’assurance, de la fonction publique, etc. Voici quelques exemples :

* Intégration numérique : L’utilisateur final peut utiliser une application mobile pour scanner ses cartes d’identité et s’inscrire à divers services. La vérification du client distant est facilitée par l’extraction des données des cartes d’identité. 

* Validation et mise en correspondance des cartes d’identité : L’utilisateur final peut remplir une demande et attacher des images de cartes d’identité. Les cartes d’identité prédéfinies permettent à une banque de vérifier que les informations correspondent aux données disponibles.

* Préremplissage de formulaires : Dans le cadre d’une déclaration de sinistre à son assureur, l’utilisateur final soumet ses cartes d’identité et les champs sont préremplis dans les documents en ligne, ce qui lui permet de gagner du temps.

L’API Cartes d’identité alimente également la [fonctionnalité de lecture de cartes d’identité AI Builder](/ai-builder/prebuilt-id-reader).

## <a name="try-it"></a>Essayer

Pour tester le service Form Recognizer relatif aux pièces d’identité, accédez à l’outil d’exemple d’interface utilisateur en ligne :

> [!div class="nextstepaction"]
> [Essayer le modèle de document ID](https://aka.ms/fott-2.1-ga "Commencez avec un modèle prédéfini pour extraire des données de documents d’identité.")

## <a name="what-does-the-id-service-do"></a>Comment le service des pièces d’identité fonctionne-t-il ?

Le service des identités extrait les valeurs clés des passeports internationaux et des permis de conduire émis aux États-Unis, et les renvoie dans une réponse JSON structurée.

### <a name="drivers-license-example"></a>**Exemple de permis de conduire**

![Exemple de permis de conduire](./media/id-example-drivers-license.JPG)

### <a name="passport-example"></a>**Exemple de passeport**

![Exemple de passeport](./media/id-example-passport-result.JPG)

### <a name="fields-extracted"></a>Champs extraits

|Nom| Type | Description | Valeur (sortie standardisée) |
|:-----|:----|:----|:----|
|  CountryRegion | countryRegion | Code de pays ou de région conforme à la norme ISO 3166 | « USA » |
|  DateOfBirth | Date | Date de naissance au format AAAA-MM-JJ | « 1980-01-01 » |
|  DateOfExpiration | Date | Date d’expiration au format AAAA-MM-JJ | « 2019-05-05 » |
|  DocumentNumber | string | Numéro de passeport, numéro de permis de conduire, etc. | « 340020013 » |
|  FirstName | string | Prénom et initiale du second prénom extraits, le cas échéant | « JENNIFER » |
|  LastName | string | Nom de famille extrait | « BROOKS » |
|  Nationalité | countryRegion | Code de pays ou de région conforme à la norme ISO 3166 | « USA » |
|  Sex | string | Les valeurs extraites possibles sont « M », « F » et « X ». | "F" |
|  MachineReadableZone | object | MRZ du passeport extraite, y compris 2 lignes de 44 caractères chacun | « P<USABROOKS<<JENNIFER<<<<<<<<<<<<<<<<<<<<<<< 3400200135USA8001014F1905054710000307<715816 » |
|  DocumentType ; | string | Type de document, par exemple, passeport ou permis de conduire | « passeport » |
|  Adresse | string | Adresse extraite (permis de conduire uniquement) | « 123 STREET ADDRESS YOUR CITY WA 99999-1234 »|
|  Région | string | Région, État, province et autres informations extraites (permis de conduire uniquement) | « Washington » |

### <a name="additional-features"></a>Fonctionnalités supplémentaires

L’API des cartes d’identité renvoie également les informations suivantes :

* Niveau de confiance de champ (chaque champ retourne une valeur de confiance associée)
* Texte brut OCR (sortie texte extraite par OCR pour l’intégralité du document d’identité)
* Cadre englobant de chaque champ extrait des permis de conduire émis aux États-Unis
* Cadre englobant pour la zone lisible par ordinateur (MRZ) sur les passeports

  > [!NOTE]
  > Pre-built IDs ne détecte pas l’authenticité de la pièce d’identité.
  >
  > Pre-built IDs de Form Recognizer extrait les données de clé des données d’identification. Toutefois, l’API ne détecte pas la validité ni l’authenticité de la pièce d’identité originale.

## <a name="input-requirements"></a>Critères des entrées

[!INCLUDE [input requirements](./includes/input-requirements-receipts.md)]

## <a name="supported-identity-document-types"></a>Types de documents d’identité pris en charge

* **Pre-built IDs v2.1** extrait les valeurs clés de passeports internationaux et de permis de conduire émis aux États-Unis.

  > [!NOTE]
  > Types de pièces d’identité pris en charge
  >
  > Les types d’identités actuellement pris en charge sont les passeports internationaux et les permis de conduire émis aux États-Unis. Nous cherchons activement à étendre notre prise en charge à d’autres pièces d’identité dans le monde.

## <a name="analyze-id-document"></a>Analyze ID Document

L’opération [Analyze ID](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1/operations/5f74a7daad1f2612c46f5822) prend une image ou un fichier PDF d’une pièce d’identité en entrée, puis en extrait les valeurs intéressantes. L’appel retourne un champ d’en-tête de réponse appelé `Operation-Location`. La valeur `Operation-Location` est une URL qui contient l’ID de résultat à utiliser à l’étape suivante.

|En-tête de réponse| URL de résultat |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/formrecognizer/v2.1/prebuilt/idDocument/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

## <a name="get-analyze-id-document-result"></a>Obtenir le résultat du document Analyser l’ID

<!---
Need to update this with updated APIM links when available
-->

La deuxième étape consiste à appeler l’opération [**Get Analyze ID Document Result**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1/operations/5f74a7738978e467c5fb8707) (Obtenir le résultat de l’analyse du document d’identification) Cette opération prend en entrée la pièce d’identité de résultat créée par l’opération Analyze ID. Elle retourne une réponse JSON qui contient un champ **État** avec les possibles valeurs suivantes. Vous appelez cette opération de façon itérative jusqu’à ce qu’elle retourne avec la valeur **succeeded**. Utilisez un intervalle de 3 à 5 secondes pour éviter de dépasser le taux de demandes par seconde (RPS).

|Champ| Type | Valeurs possibles |
|:-----|:----:|:----|
|status | string | notStarted : L’opération d’analyse n’a pas commencé. |
| |  | running : L’opération d’analyse est en cours. |
| |  | failed : L’opération d’analyse a échoué. |
| |  | succeeded : L’opération d’analyse a réussi. |

Quand le champ d’**état** a la valeur de **réussite**, la réponse JSON inclut les résultats de compréhension de ticket de caisse et de reconnaissance de texte. Les résultats des pièces d’identité sont organisés sous la forme d’un dictionnaire de valeurs de champ nommé, où chaque valeur contient le texte extrait, la valeur normalisée, le cadre englobant, la confiance et les éléments de mot correspondants. Le résultat de reconnaissance de texte est organisé sous la forme d’une hiérarchie de lignes et de mots, avec du texte, un cadre englobant et des informations de confiance.

![exemples de résultats de ticket de caisse](./media/id-example-passport-result.JPG)

### <a name="sample-json-output"></a>Exemple de sortir JSON

Voici un exemple de réponse JSON correcte (la sortie a été raccourcie à des fins de simplicité) : le nœud `readResults` contient tout le texte reconnu. Le texte est organisé par page, puis par ligne, puis par mots individuels. Le nœud `documentResults` contient les valeurs spécifiques de la pièce d’identité découvertes par le modèle. C’est également dans ce nœud que vous trouverez des paires clé-valeur utiles comme le prénom, le nom, le numéro de document, etc.

```json
{
  "status": "succeeded",
  "createdDateTime": "2021-03-04T22:29:33Z",
  "lastUpdatedDateTime": "2021-03-04T22:29:36Z",
  "analyzeResult": {
    "version": "2.1.0",
    "readResults": [
     {
        "page": 1,
        "angle": 0.3183,
        "width": 549,
        "height": 387,
        "unit": "pixel",
        "lines": [
          {
            "text": "PASSPORT",
            "boundingBox": [
              57,
              10,
              120,
              11,
              119,
              22,
              57,
              22
            ],
            "words": [
              {
                "text": "PASSPORT",
                "boundingBox": [
                  57,
                  11,
                  119,
                  11,
                  118,
                  23,
                  57,
                  22
                ],
                "confidence": 0.994
              }
            ],
          ...
          }
        ]
      }
    ],

     "documentResults": [
      {
        "docType": "prebuilt:idDocument:passport",
        "docTypeConfidence": 0.995,
        "pageRange": [
          1,
          1
        ],
        "fields": {
          "CountryRegion": {
            "type": "countryRegion",
            "valueCountryRegion": "USA",
            "text": "USA"
          },
          "DateOfBirth": {
            "type": "date",
            "valueDate": "1980-01-01",
            "text": "800101"
          },
          "DateOfExpiration": {
            "type": "date",
            "valueDate": "2019-05-05",
            "text": "190505"
          },
          "DocumentNumber": {
            "type": "string",
            "valueString": "340020013",
            "text": "340020013"
          },
          "FirstName": {
            "type": "string",
            "valueString": "JENNIFER",
            "text": "JENNIFER"
          },
          "LastName": {
            "type": "string",
            "valueString": "BROOKS",
            "text": "BROOKS"
          },
          "Nationality": {
            "type": "countryRegion",
            "valueCountryRegion": "USA",
            "text": "USA"
          },
          "Sex": {
            "type": "string",
            "valueGender": "F",
            "text": "F"
          },
          "MachineReadableZone": {
            "type": "object",
            "text": "P<USABROOKS<<JENNIFER<<<<<<<<<<<<<<<<<<<<<<< 3400200135USA8001014F1905054710000307<715816",
            "boundingBox": [
              16,
              314.1,
              504.2,
              317,
              503.9,
              363,
              15.7,
              360.1
            ],
            "page": 1,
            "confidence": 0.384,
            "elements": [
              "#/readResults/0/lines/33/words/0",
              "#/readResults/0/lines/33/words/1",
              "#/readResults/0/lines/33/words/2",
              "#/readResults/0/lines/33/words/3",
              "#/readResults/0/lines/33/words/4",
              "#/readResults/0/lines/34/words/0"
            ]
          },
          "DocumentType": {
            "type": "string",
            "text": "passport",
            "confidence": 0.995
          }
        }
      }
    ]
  }
}
```

## <a name="next-steps"></a>Étapes suivantes

* Essayez vos propres pièces d’identité et exemples dans l’[exemple d’interface utilisateur de Form Recognizer](https://aka.ms/fott-2.1-ga).
* Suivez un [démarrage rapide de Form Recognizer](quickstarts/client-library.md) pour commencer à écrire une application de traitement des pièces d’identité avec Form Recognizer dans le langage de développement de votre choix.


