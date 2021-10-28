---
title: 'Démarrage rapide : API REST Form Recognizer | Préversion'
titleSuffix: Azure Applied AI Services
description: Traitement, extraction des données et analyse des formulaires et des documents à l’aide de l’API REST Form Recognizer v3.0 (préversion)
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 10/07/2021
ms.author: lajanuar
ms.openlocfilehash: 6ee2aca6eb48b87a1d773d8d713b954eeb08beca
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2021
ms.locfileid: "130240430"
---
# <a name="get-started-form-recognizer-rest-api---preview"></a>Bien démarrer : API REST Form Recognizer | Préversion

>[!NOTE]
> Form Recognizer v3.0 est actuellement en préversion publique. Certaines fonctionnalités risquent de ne pas être prises en charge ou d’avoir des capacités limitées. 

| [API REST Form Recognizer](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument) | [Informations de référence de l’API REST Azure](/rest/api/azure/) |

Azure Cognitive Services Form Recognizer est un service cloud qui utilise le machine learning pour extraire et analyser les champs de formulaire, le texte et les tableaux de vos documents. Vous pouvez facilement appeler des modèles Form Recognizer en intégrant les kits SDK de notre bibliothèque de client dans vos workflows et applications.

### <a name="form-recognizer-models"></a>Modèles Form Recognizer

 L’API REST prend en charge les modèles et les fonctionnalités suivants :

* 🆕Document général – Analysez et extrayez le texte, les tableaux, la structure, les paires clé-valeur et les entités nommées.|
* Disposition – Analysez et extrayez les tableaux, les lignes, les mots et les marques de sélection telles que les cases d’option et les cases à cocher dans des formulaires, sans avoir besoin d’entraîner un modèle.
* Personnalisé – Analysez et extrayez les champs de formulaire et d’autres contenus de vos formulaires personnalisés en utilisant des modèles que vous avez entraînés avec vos propres types de formulaires.
* Factures – Analysez et extrayez les champs courants des factures en utilisant un modèle de facture préentraîné.
* Reçus – Analysez et extrayez les champs courants des reçus en utilisant un modèle de reçu préentraîné.
* Documents d’identité – Analysez et extrayez les champs courants des documents d’identité, tels que les passeports ou les permis de conduire, en utilisant un modèle de pièce d’identité préentraîné.
* Cartes de visite – Analysez et extrayez les champs courants des cartes de visite en utilisant un modèle de carte de visite préentraîné.

Dans ce guide de démarrage rapide, vous allez utiliser les fonctionnalités suivantes pour analyser et extraire les données et les valeurs de formulaires et de documents :

* [**Document général**](#try-it-general-document-model)

* [**Layout**](#try-it-layout-model)

* [**Facture prédéfinie**]#try-it-prebuilt-invoice-model)

## <a name="prerequisites"></a>Prérequis

* Abonnement Azure - [En créer un gratuitement](https://azure.microsoft.com/free/cognitive-services)

* [cURL](https://curl.haxx.se/windows/) installé.

* [PowerShell version 6.0 +](/powershell/scripting/install/installing-powershell-core-on-windows) ou une application de ligne de commande similaire.

* Une ressource Cognitive Services ou Form Recognizer. Une fois que vous avez votre abonnement Azure, créez une ressource Form Recognizer [monoservice](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer) ou [multiservice](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne) dans le portail Azure pour obtenir votre clé et votre point de terminaison. Vous pouvez utiliser le niveau tarifaire Gratuit (`F0`) pour tester le service, puis passer par la suite à un niveau payant pour la production.

> [!TIP]
> Créez une ressource Cognitive Services si vous envisagez d’accéder à plusieurs services Cognitive Services sous un seul point de terminaison/clé. Pour l’accès à Form Recognizer uniquement, créez une ressource Form Recognizer. Notez que vous avez besoin d’une ressource monoservice si vous avez l’intention d’utiliser l’[authentification Azure Active Directory](../../../active-directory/authentication/overview-authentication.md).

* Une fois que votre ressource est déployée, cliquez sur **Accéder à la ressource**. Vous avez besoin de la clé et du point de terminaison de la ressource que vous créez pour connecter votre application à l’API Form Recognizer. Vous collerez votre clé et votre point de terminaison dans le code ci-dessous plus loin dans ce guide de démarrage rapide :

  :::image type="content" source="../media/containers/keys-and-endpoint.png" alt-text="Capture d’écran : clés et emplacement du point de terminaison dans le portail Azure":::

## <a name="try-it-general-document-model"></a>**Essayez** : modèle de document général

> [!div class="checklist"]
>
> * Pour cet exemple, vous aurez besoin d’un **fichier de formulaire au niveau d’un URI**. Vous pouvez utiliser notre [exemple de formulaire](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/sample-layout.pdf) pour ce guide de démarrage rapide. Avant d’exécuter la commande, apportez les modifications suivantes :

1. Remplacez `{endpoint}` par le point de terminaison que vous avez obtenu avec votre abonnement Form Recognizer.
1. Remplacez `{subscription key}` par la clé d’abonnement que vous avez copiée à l’étape précédente.
1. Remplacez `{your-document-url}` par un exemple d’URL de formulaire.

#### <a name="request"></a>Requête

```bash
 curl -v -i POST "https://{endpoint}/formrecognizer/documentModels/prebuilt-document:analyze?api-version=2021-09-30-preview&api-version=2021-09-30-preview HTTP/1.1" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: {subscription key}" --data-ascii "{'source': '{your-document-url}'}"
```

#### <a name="operation-location"></a>Operation-Location

Vous recevrez une réponse `202 (Success)` incluant un en-tête **Operation-Location**. La valeur de cet en-tête contient un ID de résultats qui vous permet d’interroger l’état de l’opération asynchrone et d’obtenir les résultats :

https://{host}/formrecognizer/documentModels/{modelId}/analyzeResults/ **{resultId}** ?api-version=2021-07-30-preview

### <a name="get-general-document-results"></a>Obtenir les résultats de document général

Après avoir appelé l’API **[Analyser un document](https://westus.api.cognitive.microsoft.com/formrecognizer/documentModels/prebuilt-document:analyze?api-version=2021-09-30-preview&stringIndexType=textElements)** , appelez l’API **[Obtenir un résultat d’analyse](https://westus.api.cognitive.microsoft.com/formrecognizer/documentModels/prebuilt-document/analyzeResults/{resultId}?api-version=2021-09-30-preview)** pour obtenir l’état de l’opération et les données extraites. Avant d’exécuter la commande, apportez les modifications suivantes :

1. Remplacez `{endpoint}` par le point de terminaison que vous avez obtenu avec votre abonnement Form Recognizer.
1. Remplacez `{subscription key}` par la clé d’abonnement que vous avez copiée à l’étape précédente.
1. Remplacez `{resultId}` par l’ID de résultats de l’étape précédente.
<!-- markdownlint-disable MD024 -->

#### <a name="request"></a>Requête

```bash
curl -v -X GET "https://{endpoint}/formrecognizer/documentModels/prebuilt-document/analyzeResults/{resultId}?api-version=2021-09-30-preview&api-version=2021-09-30-preview"
```

### <a name="examine-the-response"></a>Examiner la réponse

Vous recevez une réponse `200 (Success)` avec une sortie JSON. Le premier champ, `"status"`, indique l’état de l’opération. Si l’opération n’est pas terminée, la valeur de `"status"` sera `"running"` ou `"notStarted"`, et vous devez rappeler l’API, manuellement ou via un script. Nous vous recommandons d’attendre une seconde ou plus entre chaque appel.

Le nœud `"analyzeResults"` contient tout le texte reconnu. Le texte est organisé par page, lignes, tableaux, paires clé-valeur et entités.

#### <a name="sample-response"></a>Exemple de réponse

```json
{
    "status": "succeeded",
    "createdDateTime": "2021-09-28T16:52:51Z",
    "lastUpdatedDateTime": "2021-09-28T16:53:08Z",
    "analyzeResult": {
        "apiVersion": "2021-09-30-preview",
        "modelId": "prebuilt-document",
        "stringIndexType": "textElements",
        "content": "content extracted",
        "pages": [
            {
                "pageNumber": 1,
                "angle": 0,
                "width": 8.4722,
                "height": 11,
                "unit": "inch",
                "words": [
                    {
                        "content": "Case",
                        "boundingBox": [
                            1.3578,
                            0.2244,
                            1.7328,
                            0.2244,
                            1.7328,
                            0.3502,
                            1.3578,
                            0.3502
                        ],
                        "confidence": 1,
                        "span": {
                            "offset": 0,
                            "length": 4
                        }
                    }

                ],
                "lines": [
                    {
                        "content": "Case",
                        "boundingBox": [
                            1.3578,
                            0.2244,
                            3.2879,
                            0.2244,
                            3.2879,
                            0.3502,
                            1.3578,
                            0.3502
                        ],
                        "spans": [
                            {
                                "offset": 0,
                                "length": 22
                            }
                        ]
                    }
                ]
            }
        ],
        "tables": [
            {
                "rowCount": 8,
                "columnCount": 3,
                "cells": [
                    {
                        "kind": "columnHeader",
                        "rowIndex": 0,
                        "columnIndex": 0,
                        "rowSpan": 1,
                        "columnSpan": 1,
                        "content": "Applicant's Name:",
                        "boundingRegions": [
                            {
                                "pageNumber": 1,
                                "boundingBox": [
                                    1.9198,
                                    4.277,
                                    3.3621,
                                    4.2715,
                                    3.3621,
                                    4.5034,
                                    1.9198,
                                    4.5089
                                ]
                            }
                        ],
                        "spans": [
                            {
                                "offset": 578,
                                "length": 17
                            }
                        ]
                    }
                ],
                "spans": [
                    {
                        "offset": 578,
                        "length": 300
                    },
                    {
                        "offset": 1358,
                        "length": 10
                    }
                ]
            }
        ],
        "keyValuePairs": [
            {
                "key": {
                    "content": "Case",
                    "boundingRegions": [
                        {
                            "pageNumber": 1,
                            "boundingBox": [
                                1.3578,
                                0.2244,
                                1.7328,
                                0.2244,
                                1.7328,
                                0.3502,
                                1.3578,
                                0.3502
                            ]
                        }
                    ],
                    "spans": [
                        {
                            "offset": 0,
                            "length": 4
                        }
                    ]
                },
                "value": {
                    "content": "A Case",
                    "boundingRegions": [
                        {
                            "pageNumber": 1,
                            "boundingBox": [
                                1.8026,
                                0.2276,
                                3.2879,
                                0.2276,
                                3.2879,
                                0.3502,
                                1.8026,
                                0.3502
                            ]
                        }
                    ],
                    "spans": [
                        {
                            "offset": 5,
                            "length": 17
                        }
                    ]
                },
                "confidence": 0.867
            }
        ],
        "entities": [
            {
                "category": "Person",
                "content": "Jim Smith",
                "boundingRegions": [
                    {
                        "pageNumber": 1,
                        "boundingBox": [
                            3.4672,
                            4.3255,
                            5.7118,
                            4.3255,
                            5.7118,
                            4.4783,
                            3.4672,
                            4.4783
                        ]
                    }
                ],
                "confidence": 0.93,
                "spans": [
                    {
                        "offset": 596,
                        "length": 21
                    }
                ]
            }
        ],
        "styles": [
            {
                "isHandwritten": true,
                "confidence": 0.95,
                "spans": [
                    {
                        "offset": 565,
                        "length": 12
                    },
                    {
                        "offset": 3493,
                        "length": 1
                    }
                ]
            }
        ]
    }
}

```

## <a name="try-it-layout-model"></a>**Essayez** : modèle de disposition

> [!div class="checklist"]
>
> * Pour cet exemple, vous aurez besoin d’un **fichier de formulaire au niveau d’un URI**. Vous pouvez utiliser notre [exemple de formulaire](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/sample-layout.pdf) pour ce guide de démarrage rapide.

 Avant d’exécuter la commande, apportez les modifications suivantes :

1. Remplacez `{endpoint}` par le point de terminaison que vous avez obtenu avec votre abonnement Form Recognizer.
1. Remplacez `{subscription key}` par la clé d’abonnement que vous avez copiée à l’étape précédente.
1. Remplacez `"{your-document-url}` par l’un des exemples d’URL.

#### <a name="request"></a>Requête

```bash
bash
 curl -v -i POST "https://{endpoint}/formrecognizer/documentModels/prebuilt-layout:analyze?api-version=2021-09-30-preview&api-version=2021-09-30-preview HTTP/1.1" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: {subscription key}" --data-ascii "{'source': '{your-document-url}'}"
```

#### <a name="operation-location"></a>Operation-Location

Vous recevrez une réponse `202 (Success)` incluant un en-tête **Operation-Location**. La valeur de cet en-tête contient un ID de résultats qui vous permet d’interroger l’état de l’opération asynchrone et d’obtenir les résultats :

`https://{host}/formrecognizer/documentModels/{modelId}/analyzeResults/**{resultId}**?api-version=2021-07-30-preview`

### <a name="get-layout-results"></a>Obtenir les résultats de la disposition

Après avoir appelé l’API **[Analyser un document](https://westus.api.cognitive.microsoft.com/formrecognizer/documentModels/prebuilt-layout:analyze?api-version=2021-09-30-preview&stringIndexType=textElements)** , appelez l’API **[Obtenir un résultat d’analyse](https://westus.api.cognitive.microsoft.com/formrecognizer/documentModels/prebuilt-layout/analyzeResults/{resultId}?api-version=2021-09-30-preview)** pour obtenir l’état de l’opération et les données extraites. Avant d’exécuter la commande, apportez les modifications suivantes :

1. Remplacez `{endpoint}` par le point de terminaison que vous avez obtenu avec votre abonnement Form Recognizer.
1. Remplacez `{subscription key}` par la clé d’abonnement que vous avez copiée à l’étape précédente.
1. Remplacez `{resultId}` par l’ID de résultats de l’étape précédente.
<!-- markdownlint-disable MD024 -->

#### <a name="request"></a>Requête

```bash
curl -v -X GET "https://{endpoint}/formrecognizer/documentModels/prebuilt-layout/analyzeResults/{resultId}?api-version=2021-09-30-preview&api-version=2021-09-30-preview"
```

### <a name="examine-the-response"></a>Examiner la réponse

Vous recevez une réponse `200 (Success)` avec une sortie JSON. Le premier champ, `"status"`, indique l’état de l’opération. Si l’opération n’est pas terminée, la valeur de `"status"` sera `"running"` ou `"notStarted"`, et vous devez rappeler l’API, manuellement ou via un script. Nous vous recommandons d’attendre une seconde ou plus entre chaque appel.

## <a name="try-it-prebuilt-invoice-model"></a>**Essayez** : modèle de facture prédéfinie

> [!div class="checklist"]
>
> * Pour cet exemple, vous aurez besoin d’un **fichier de facture au niveau d’un URI**. Vous pouvez utiliser notre [exemple de facture](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-REST-api-samples/master/curl/form-recognizer/sample-invoice.pdf) pour ce guide de démarrage rapide.

### <a name="choose-the-invoice-prebuilt-model-id"></a>Choisir l’ID de modèle de facture prédéfinie

Vous n’êtes pas limité aux factures – Vous avez le choix entre plusieurs modèles prédéfinis, chacun ayant son propre ensemble de champs pris en charge. Le modèle à utiliser pour l’opération d’analyse dépend du type de document à analyser. Voici les ID de modèle pour les modèles prédéfinis actuellement pris en charge par le service Form Recognizer :

* **prebuilt-invoice** : extrait le texte, les marques de sélection, les tableaux, les paires clé-valeur et les informations clés des factures.
* **prebuilt-businessCard** : extrait le texte et les informations clés des cartes de visite.
* **prebuilt-idDocument** : extrait le texte et les informations clés des permis de conduire et des passeports internationaux.
* **prebuilt-receipt** : extrait le texte et les informations clés des reçus.

Avant d’exécuter la commande, apportez les modifications suivantes :

1. Remplacez `{endpoint}` par le point de terminaison que vous avez obtenu avec votre abonnement Form Recognizer.
1. Remplacez `{subscription key}` par la clé d’abonnement que vous avez copiée à l’étape précédente.
1. Remplacez `\"{your-document-url}` par l’un des exemples d’URL.

#### <a name="request"></a>Requête

```bash
bash
 curl -v -i POST "https://{endpoint}/formrecognizer/documentModels/prebuilt-invoice:analyze?api-version=2021-09-30-preview&api-version=2021-09-30-preview HTTP/1.1" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: {subscription key}" --data-ascii "{'source': '{your-document-url}'}"
```

#### <a name="operation-location"></a>Operation-Location

Vous recevrez une réponse `202 (Success)` incluant un en-tête **Operation-Location**. La valeur de cet en-tête contient un ID de résultats qui vous permet d’interroger l’état de l’opération asynchrone et d’obtenir les résultats :

https:\//{host}/formrecognizer/documentModels/{modelId}/analyzeResults/ **{resultId}** ?api-version=2021-07-30-preview

### <a name="get-invoice-results"></a>Obtenir les résultats de la facture

Après avoir appelé l’API **[Analyser un document](https://westus.api.cognitive.microsoft.com/formrecognizer/documentModels/prebuilt-invoice:analyze?api-version=2021-09-30-preview&stringIndexType=textElements)** , appelez l’API **[Obtenir un résultat d’analyse](https://westus.api.cognitive.microsoft.com/formrecognizer/documentModels/prebuilt-invoice/analyzeResults/{resultId}?api-version=2021-09-30-preview)** pour obtenir l’état de l’opération et les données extraites. Avant d’exécuter la commande, apportez les modifications suivantes :

1. Remplacez `{endpoint}` par le point de terminaison que vous avez obtenu avec votre abonnement Form Recognizer.
1. Remplacez `{subscription key}` par la clé d’abonnement que vous avez copiée à l’étape précédente.
1. Remplacez `{resultId}` par l’ID de résultats de l’étape précédente.
<!-- markdownlint-disable MD024 -->

#### <a name="request"></a>Requête

```bash
curl -v -X GET "https://{endpoint}/formrecognizer/documentModels/prebuilt-invoice/analyzeResults/{resultId}?api-version=2021-09-30-preview&api-version=2021-09-30-preview"
```

### <a name="examine-the-response"></a>Examiner la réponse

Vous recevez une réponse `200 (Success)` avec une sortie JSON. Le premier champ, `"status"`, indique l’état de l’opération. Si l’opération n’est pas terminée, la valeur de `"status"` sera `"running"` ou `"notStarted"`, et vous devez rappeler l’API, manuellement ou via un script. Nous vous recommandons d’attendre une seconde ou plus entre chaque appel.

### <a name="improve-results"></a>Améliorer les résultats

[!INCLUDE [improve results](../includes/improve-results-unlabeled.md)]

## <a name="manage-custom-models"></a>Gérer des modèles personnalisés

### <a name="get-a-list-of-models"></a>Obtenir la liste des modèles

La demande   [Lister les modèles](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/GetModels) de la préversion v3.0 retourne une liste paginée de modèles prédéfinis, en plus des modèles personnalisés. Seuls les modèles avec l’état réussi sont inclus. Les modèles en cours ou ayant échoué peuvent être énumérés via la demande [Lister les opérations](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/GetOperations). Utilisez la propriété nextLink pour accéder à la page suivante de modèles, le cas échéant. Pour obtenir plus d’informations sur chaque modèle retourné, y compris la liste des documents pris en charge et de leurs champs, transmettez modelId à la demande  [Obtenir un modèle](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/GetOperations). 

```bash
curl -v -X GET "https://{endpoint}/formrecognizer/documentModels?api-version=2021-07-30-preview"
```

### <a name="get-a-specific-model"></a>Obtenir un modèle spécifique

La demande [Obtenir un modèle](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/GetModel) de la préversion v3.0 récupère les informations relatives à un modèle spécifique avec un état réussi. Pour les modèles en cours ou ayant échoué, utilisez la demande [Obtenir une opération](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/GetOperation) pour suivre l’état des opérations de création de modèle et toutes les erreurs résultantes.

```bash
curl -v -X GET "https://{endpoint}/formrecognizer/documentModels/{modelId}?api-version=2021-07-30-preview" 
```

### <a name="delete-a-model"></a>Supprimer un modèle

La demande [Supprimer le modèle](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/DeleteModel) de la préversion v3.0 supprime le modèle personnalisé et modelId n’est plus accessible par les opérations ultérieures.  De nouveaux modèles peuvent être créés à l’aide du même modelId sans conflit.

```bash
curl -v -X DELETE "https://{endpoint}/formrecognizer/documentModels/{modelId}?api-version=2021-07-30-preview"
```

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez utilisé l’API REST Form Recognizer en préversion (v3.0) pour analyser des formulaires de différentes manières. Explorez à présent la documentation de référence pour découvrir plus en détail l’API Form Recognizer.

> [!div class="nextstepaction"]
> [Documentation de référence de l’API REST en préversion (v3.0)](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument)