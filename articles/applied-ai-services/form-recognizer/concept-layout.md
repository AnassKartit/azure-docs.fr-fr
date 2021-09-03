---
title: Dispositions - Form Recognizer
titleSuffix: Azure Applied AI Services
description: Découvrez les concepts liés à l’analyse des dispositions avec l’API Form Recognizer - utilisation et limites.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 08/09/2021
ms.author: lajanuar
ms.openlocfilehash: af45cf3d3f9ae534be52ae617b13c76eb0d2eae8
ms.sourcegitcommit: 5f659d2a9abb92f178103146b38257c864bc8c31
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2021
ms.locfileid: "122563884"
---
# <a name="form-recognizer-layout-service"></a>Service Layout de Form Recognizer

L’API de disposition d’Azure Form Recognizer extrait du texte, des tableaux, des marques de sélection et des informations de structure à partir de documents (PDF, TIFF) et d’images (JPG, PNG, BMP). Elle permet aux clients de prendre des documents dans une multitude de formats et de retourner des représentations de données structurées des documents. Elle combine une version améliorée de nos puissantes capacités de [reconnaissance optique de caractères (OCR)](../../cognitive-services/computer-vision/overview-ocr.md) avec des modèles de deep learning (apprentissage approfondi) pour extraire du texte, des tableaux, des marques de sélection et la structure du document.

## <a name="what-does-the-layout-service-do"></a>Comment fonctionne le service Layout ?

L’API de disposition extrait du texte, des tableaux avec leurs en-têtes inclus, des marques de sélection et les informations de structurelle des documents avec une précision exceptionnelle, et retourne une réponse JSON organisée et structurée. Les documents peuvent être dans divers formats et de diverses qualités. Il peut s’agir notamment d’images capturées par téléphone, de documents numérisés et de fichiers PDF numériques. L’API de disposition extrait avec précision la sortie structurée de tous ces documents.

![Exemple de disposition](./media/layout-demo.gif)

## <a name="try-it&quot;></a>Essayer

Pour tester le service Layout de Form Recognizer, accédez à l’outil d’exemple d’interface utilisateur en ligne :

> [!div class=&quot;nextstepaction&quot;]
> [Essayer le modèle de disposition](https://aka.ms/fott-2.1-ga &quot;Commencez avec le modèle de disposition prédéfini pour extraire des données de vos formulaires.")

Vous aurez besoin d’un abonnement Azure ([créez-en un gratuitement](https://azure.microsoft.com/free/cognitive-services)), ainsi que d’un point de terminaison et d’une clé de [ressource Form Recognizer](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer) pour tester l’API Layout de Form Recognizer.

![Capture d’écran de l’exemple d’interface utilisateur. Le texte, les tables et les marques de sélection d’un document sont analysés.](./media/analyze-layout.png)

## <a name="input-requirements"></a>Critères des entrées

[!INCLUDE [input requirements](./includes/input-requirements-receipts.md)]

## <a name="analyze-layout"></a>Analyser la disposition

Tout d’abord, appelez l’opération [Analyze Layout](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1/operations/AnalyzeLayoutAsync) (Analyser la disposition). Analyze Layout prend un document (fichier image, TIFF ou PDF) en entrée et en extrait le texte, les tableaux, les marques de sélection et la structure. L’appel retourne un champ d’en-tête de réponse appelé `Operation-Location`. La valeur `Operation-Location` est une URL qui contient l’ID de résultat à utiliser à l’étape suivante.

|En-tête de réponse| URL de résultat |
|:-----|:----|
|Operation-Location | `https://cognitiveservice/formrecognizer/v2.1/layout/analyzeResults/{resultId}' |

## <a name="get-analyze-layout-result"></a>Obtenir le résultat de la disposition de l’analyse

La seconde étape consiste à appeler l’opération d’[obtention du résultat de l’analyse de la disposition](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1/operations/GetAnalyzeLayoutResult). Cette opération prend en entrée l’ID de résultat créé par l’opération d’analyse de la disposition. Elle retourne une réponse JSON qui contient un champ **État** avec les possibles valeurs suivantes.

|Champ| Type | Valeurs possibles |
|:-----|:----:|:----|
|status | string | `notStarted` : L’opération d’analyse n’a pas commencé.<br /><br />`running` : L’opération d’analyse est en cours.<br /><br />`failed` : L’opération d’analyse a échoué.<br /><br />`succeeded` : L’opération d’analyse a réussi.|

Appelez cette opération de façon itérative jusqu’à ce qu’elle renvoie la valeur `succeeded`. Utilisez un intervalle de 3 à 5 secondes pour éviter de dépasser le taux de demandes par seconde (RPS).

Quand le champ **status** a la valeur `succeeded`, la réponse JSON inclut la mise en page, le texte, les tableaux et les marques de sélection extraits. Les données extraites comprennent les lignes de texte et les mots extraits, les cadres englobants, l’aspect du texte avec indication manuscrite, les tableaux et les marques de sélection avec indication de sélection/non sélection.

## <a name="sample-json-output"></a>Exemple de sortir JSON

La réponse à l’opération *Get Analyze Layout Result* est une représentation structurée du document avec toutes les informations extraites.
Reportez-vous à cet [exemple de fichier de document](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-layout.pdf) et cet [exemple de sortie de disposition](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/curl/form-recognizer/sample-layout-output.json) structurée.

La sortie JSON comporte deux parties :

* Le nœud `readResults` contient tout le texte reconnu et toutes les marques de sélection. Le texte est organisé par page, puis par ligne, puis par mots individuels.
* Le nœud `pageResults` contient les tables et les cellules extraites avec leurs cadres englobants, la confiance et une référence aux lignes et aux mots dans « readResults ».

## <a name="features"></a>Fonctionnalités

### <a name="tables-and-table-headers"></a>Tableaux et en-têtes de tableau

L’API de disposition extrait des tableaux dans la section `pageResults` de la sortie JSON. Les documents peuvent être scannés, photographiés ou numériques. Les tableaux peuvent être complexes avec des cellules ou des colonnes fusionnées, avec ou sans bordures et avec des angles irréguliers. Les informations extraites d’un tableau comprennent le nombre de colonnes et de lignes, la portée des lignes et la portée des colonnes. Chaque cellule avec son cadre englobant est une sortie avec des informations indiquant si elle soit reconnue comme faisant partie, ou non, d’un en-tête. Les cellules d’en-tête prédites du modèle peuvent s’étendre sur plusieurs lignes et ne sont pas nécessairement les premières lignes d’un tableau. Elles fonctionnent également avec les tableaux pivotés. Chaque cellule de tableau comprend également le texte complet avec des références aux mots individuels de la section `readResults`.

:::image type="content" source="./media/layout-table-headers-example.png" alt-text="Sortie des en-têtes de tableau de disposition":::

### <a name="selection-marks"></a>Marques de sélection

L’API de disposition extrait aussi les marques de sélection des documents. Les marques de sélection extraites incluent le cadre englobant, la confiance et l’état (sélectionné/non sélectionné). Les informations sur les marques de sélection sont extraites dans la section `readResults` de la sortie JSON.

:::image type="content" source="./media/layout-selection-marks.png" alt-text="Sortie des marques de sélection de disposition":::

### <a name="text-lines-and-words"></a>Lignes de texte et mots

L’API de disposition extrait du texte des documents et des images avec plusieurs angles et couleurs de texte. Elle accepte les photos de documents, les télécopies, les textes imprimés et/ou manuscrits (en anglais uniquement) et les modes mixtes. Le texte est extrait avec des informations fournies sur les lignes, les mots, les cadres englobants, les scores de confiance et le style (manuscrit ou autre). Toutes les informations textuelles sont incluses dans la section `readResults` de la sortie JSON.

:::image type="content" source="./media/layout-text-extraction.png" alt-text="Sortie d’extraction de texte de disposition":::

### <a name="natural-reading-order-for-text-lines-latin-only"></a>Ordre de lecture naturel pour les lignes de texte (langues latines uniquement)

Vous pouvez spécifier l’ordre dans lequel les lignes de texte sont générées avec le paramètre de requête `readingOrder`. Utilisez `natural` pour une sortie d’ordre de lecture plus conviviale, comme illustré dans l’exemple suivant. Cette fonctionnalité est prise en charge uniquement pour les langues latines.

:::image type="content" source="./media/layout-reading-order-example.png" alt-text="Disposition – Exemple d’ordre de lecture" lightbox="../../cognitive-services/Computer-vision/Images/ocr-reading-order-example.png":::

### <a name="handwritten-classification-for-text-lines-latin-only"></a>Classification manuscrite pour les lignes de texte (Latin uniquement)

La réponse inclut le classement de chaque ligne de texte selon qu’elle est de style manuscrit ou non, avec un score de confiance. Cette fonctionnalité est prise en charge uniquement pour les langues latines. L’exemple suivant illustre la classification manuscrite pour le texte de l’image.

:::image type="content" source="./media/layout-handwriting-classification.png" alt-text="Exemple de classification d’écriture manuscrite":::

### <a name="select-page-numbers-or-ranges-for-text-extraction"></a>Sélectionner des numéros de page ou des plages de pages pour l’extraction de texte

Pour les documents volumineux comportant plusieurs pages, utilisez le paramètre de requête `pages` pour indiquer des numéros de page ou des plages de pages spécifiques pour l’extraction de texte. L’exemple suivant montre un document de 10 pages, avec le texte extrait pour les deux cas : toutes les pages (1 à 10) et certaines pages (3 à 6).

:::image type="content" source="./media/layout-select-pages-for-text.png" alt-text="Disposition – Sortie des pages sélectionnées":::

## <a name="next-steps"></a>Étapes suivantes

* Testez l’extraction d’une mise en page à l’aide de l’[outil d’exemple d’interface utilisateur Form Recognizer](https://aka.ms/fott-2.1-ga).
* Suivez un [démarrage rapide Form Recognizer](quickstarts/client-library.md#analyze-layout) pour commencer à extraire des layouts dans le langage de développement de votre choix.

## <a name="see-also"></a>Voir aussi

* [Qu’est-ce que Form Recognizer ?](./overview.md)
* [Documentation de référence sur l’API REST](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1/operations/AnalyzeLayoutAsync)
