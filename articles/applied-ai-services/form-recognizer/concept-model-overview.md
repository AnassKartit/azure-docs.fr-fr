---
title: Modèles Form Recognizer
titleSuffix: Azure Applied AI Services
description: Concepts englobant l’extraction et l’analyse de données à l’aide de modèles prédéfinis
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 10/07/2021
ms.author: lajanuar
recommendations: false
ms.openlocfilehash: f2e2ef59d4c3608065edab4ffd1d1ec55122f2ad
ms.sourcegitcommit: af303268d0396c0887a21ec34c9f49106bb0c9c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/11/2021
ms.locfileid: "129754612"
---
<!-- markdownlint-disable MD033 -->

# <a name="form-recognizer-models"></a>Modèles Form Recognizer

 Les modèles prédéfinis d’Azure Form Recognizer vous permettent d’ajouter un traitement intelligent des formulaires à vos applications et à vos flux sans avoir à effectuer l’apprentissage et la construction de vos propres modèles. Les modèles prédéfinis utilisent la reconnaissance optique de caractères (OCR) combinée à des modèles Deep Learning pour identifier et extraire des champs de texte et de données prédéfinis communs à des types de formulaires et de documents spécifiques. Form Recognizer extrait les données des formulaires et des documents, puis renvoie une réponse JSON organisée et structurée. Form Recognizer v2.1 prend en charge les modèles de facture, de reçu, de document d’identité et de carte de visite.

## <a name="model-overview"></a>Vue d’ensemble des modèles

| **Modèle**   | **Description**   |
| --- | --- |
| 🆕[Document général (préversion)](#general-document-preview) | Extrait le texte, les tableaux, la structure, les paires clé-valeur et les entités nommées.  |
| [Disposition](#layout)  | Extrait des informations sur le texte et la disposition à partir de documents.  |
| [Facture](#invoice)  | Extrait des informations clés de factures en anglais.  |
| [Réception](#receipt)  | Extrait des informations clés de reçus en anglais.  |
| [Document d’identité](#id-document)  | Extrait des informations clés de permis de conduire américains et de passeports internationaux.  |
| [Carte de visite](#business-card)  | Extrait des informations clés de cartes de visite en anglais.  |
| [Personnalisée](#custom) |  Extrait des données de formulaires et de documents spécifiques à votre entreprise. Les modèles personnalisés sont entraînés pour vos données et cas d’usage spécifiques. |

### <a name="general-document-preview"></a>Document général (préversion)

* L’API de document général prend en charge la plupart des types de formulaires. Elle analyse vos documents et associe des valeurs aux clés et des entrées aux tableaux qu’elle découvre. Elle est idéale pour extraire les paires clé-valeur courantes des documents. Vous pouvez utiliser le modèle de document général comme alternative à la [formation d’un modèle personnalisé sans étiquettes](compose-custom-models.md#train-without-labels).

* Le document général est un modèle préformé qui peut être appelé directement par le biais de l’API REST.

* Le modèle de document général prend en charge la reconnaissance d’entité nommée (NER) pour plusieurs catégories d’entités. La reconnaissance d’entité nommée est la capacité d’identifier différentes entités dans du texte et de les catégoriser en classes ou types prédéfinis tels que : personne, lieu, événement, produit et organisation. L’extraction d’entités peut être utile dans les scénarios où vous souhaitez valider des valeurs extraites. Les entités sont extraites de l’ensemble du contenu.

##### <a name="sample-document-processed-in-the-form-recognizer-studio"></a>Exemple de document traité dans [Form Recognizer Studio](https://formrecognizer.appliedai.azure.com/studio/prebuilt?formType=document) :

:::image type="content" source="media/general-document-analyze.png" alt-text="Capture d’écran : analyse d’un document général dans Form Recognizer Studio.":::

> [!div class="nextstepaction"]
> [En savoir plus : Modèle de document général](concept-general-document.md)

### <a name="layout"></a>Layout

L’API de disposition analyse et extrait le texte, les tableaux et les en-têtes, les marques de sélection et les informations de structure des documents et renvoie une représentation structurée des données JSON.

##### <a name="sample-form-processed-with-form-recognizer-sample-labeling-tool--layout-feature"></a>Exemple de formulaire traité avec la fonctionnalité de disposition [Outil d’étiquetage des exemples Form Recognizer](https://fott-2-1.azurewebsites.net/) :

:::image type="content" source="media/overview-layout.png" alt-text="{alt-text}":::

> [!div class="nextstepaction"]
> [En savoir plus : Modèle de disposition](concept-layout.md)

### <a name="invoice"></a>Facture

Le modèle de facture analyse et extrait les informations clés des factures. L’API analyse les factures dans différents formats, extrait les informations clés, telles que le nom du client, l’adresse de facturation, la date d’échéance et le montant dû, et renvoie une représentation structurée des données JSON.

##### <a name="sample-invoice-processed-with-form-recognizer-sample-labeling-tool"></a>Exemple de facture traitée avec l’[outil d’étiquetage des exemples Form Recognizer](https://fott-2-1.azurewebsites.net/) :

:::image type="content" source="./media/overview-invoices.jpg" alt-text="Exemple de facture" lightbox="./media/overview-invoices.jpg":::

> [!div class="nextstepaction"]
> [En savoir plus : Modèle de facture](concept-invoice.md)

### <a name="receipt"></a>Réception

Le modèle de reçu analyse et extrait les informations clés des reçus. L’API analyse les reçus imprimés et manuscrits, extrait les informations clés telles que le nom du commerçant, le numéro de téléphone du commerçant, la date de la transaction, la taxe et le total de la transaction et renvoie une représentation structurée des données JSON.

##### <a name="sample-receipt-processed-with-form-recognizer-sample-labeling-tool"></a>Exemple de reçu traité avec l’[outil d’étiquetage des exemples Form Recognizer](https://fott-2-1.azurewebsites.net/) :

:::image type="content" source="./media/overview-receipt.jpg" alt-text="exemple de ticket de caisse" lightbox="./media/overview-receipt.jpg":::

> [!div class="nextstepaction"]
> [En savoir plus : Modèle de reçu](concept-receipt.md)

### <a name="id-document"></a>Document d’identité

Le modèle de document d’identité analyse et extrait les informations clés des permis de conduire américains (des 50 États et du district de Columbia) et des pages biographiques des passeports internationaux (à l’exclusion des visas et autres documents de voyage). L’API analyse les documents d’identité, extrait les informations clés, telles que le prénom, le nom, l’adresse et la date de naissance, et renvoie une représentation structurée des données JSON.

##### <a name="sample-us-drivers-license-processed-with-form-recognizer-sample-labeling-tool"></a>Exemple de permis de conduire américain traité avec l’[outil d’étiquetage des exemples Form Recognizer](https://fott-2-1.azurewebsites.net/) :

:::image type="content" source="./media/id-example-drivers-license.jpg" alt-text="exemple de carte d’identité" lightbox="./media/overview-id.jpg":::

> [!div class="nextstepaction"]
> [En savoir plus : Modèle de document d’identité](concept-id-document.md)

### <a name="business-card"></a>Carte de visite

Le modèle de carte de visite analyse et extrait des informations clés à partir d’images de carte de visite. L’API analyse les cartes de visite imprimées, extrait les informations clés, telles que le prénom, le nom, le nom de la société, l’adresse e-mail et le numéro de téléphone, et renvoie une représentation structurée des données JSON.

##### <a name="sample-business-card-processed-with-form-recognizer-sample-labeling-tool"></a>Exemple de carte de visite traitée avec l’[outil d’étiquetage des exemples Form Recognizer](https://fott-2-1.azurewebsites.net/) :

:::image type="content" source="./media/overview-business-card.jpg" alt-text="exemple de carte de visite" lightbox="./media/overview-business-card.jpg":::

> [!div class="nextstepaction"]
> [En savoir plus : Modèle de carte de visite](concept-business-card.md)

### <a name="custom"></a>Custom

Le modèle personnalisé analyse et extrait les données de formulaires et de documents spécifiques à votre entreprise. L’API est un programme de Machine Learning dont l’apprentissage a pour but de reconnaître les champs de formulaire dans vos contenus spécifiques et d’extraire des paires clé-valeur et des données de table. Vous avez seulement besoin de cinq exemples du même type de formulaire pour commencer. L’apprentissage de votre modèle personnalisé peut s’effectuer avec ou sans jeux de données étiquetés.

##### <a name="sample-custom-form-processed-with-form-recognizer-sample-labeling-tool"></a>Exemple de formulaire personnalisé traité avec l’[outil d’étiquetage des exemples Form Recognizer](https://fott-2-1.azurewebsites.net/) :

:::image type="content" source="media/analyze.png" alt-text="Capture d’écran : outil Form Recognizer, fenêtre de l’analyse de formulaire personnalisé.":::

> [!div class="nextstepaction"]
> [En savoir plus : Modèle personnalisé](concept-custom.md)

## <a name="data-extraction"></a>Extraction de données

 | **Modèle**   | **Extraction de texte** |**Paires clé-valeur** |**Marques de sélection**   | **Tables**   |**Entités** |
  | --- | :---: |:---:| :---: | :---: |:---: |
  |🆕Document général  | ✓  |  ✓ | ✓  | ✓  | ✓  |
  | Layout  | ✓  |   | ✓  | ✓  |   |
  | Facture  | ✓ | ✓  | ✓  | ✓ ||
  |Réception  | ✓  |   ✓ |   |  ||
  | Document d’identité | ✓  |   ✓  |   |   ||
  | Carte de visite    | ✓  |   ✓ |   |   ||
  | Custom             |✓  |  ✓ | ✓  | ✓  | ✓  |

## <a name="input-requirements"></a>Critères des entrées

* Pour de meilleurs résultats, fournissez une photo nette ou une copie de qualité par document.
* Formats de fichier pris en charge : JPEG, PNG, BMP, TIFF et PDF (texte incorporé ou numérisé). Les PDF avec du texte incorporé sont préférables pour éviter tout risque d’erreur au niveau de l’extraction et de l’emplacement des caractères.
* Pour PDF et TIFF, il est possible de traiter jusqu’à 2 000 pages (avec un abonnement gratuit, seules les deux premières pages sont traitées).
* La taille du fichier doit être inférieure à 50 Mo.
* Les dimensions des images doivent être comprises entre 50 x 50 et 10 000 x 10 000 pixels.
* Les dimensions des PDF vont jusqu’à 17x17 pouces, ce qui correspond au format papier Legal, A3 ou plus petit.
* La taille totale des données d’entraînement doit être de 500 pages maximum.
* Si vos fichiers PDF sont verrouillés par mot de passe, vous devez supprimer le verrou avant leur envoi.
* Pour l’apprentissage non supervisé (sans données étiquetées) :
  * Les données doivent contenir des clés et des valeurs.
  * Les clés doivent apparaître au-dessus ou à gauche des valeurs, pas en dessous ni à droite.

> [!NOTE]
> L’[outil d’étiquetage des exemples](https://fott-2-1.azurewebsites.net/) ne prend pas en charge le format de fichier BMP. Il s’agit d’une limitation de l’outil et non du service Form Recognizer.

## <a name="form-recognizer-preview-v30"></a>Form Recognizer préversion v3.0

  Form Recognizer v3.0 (préversion) introduit plusieurs nouvelles fonctionnalités et capacités :

* Le modèle [**Document général (préversion)** ](concept-general-document.md) est une nouvelle API qui utilise un modèle préformé pour extraire du texte, des tableaux, une structure, des paires clé-valeur et des entités nommées à partir de formulaires et de documents.
* Le modèle [**Reçu (préversion)** ](concept-receipt.md) prend en charge le traitement des reçus d’hôtel d’une seule page.
* Le modèle [**Document d’identité (préversion)** ](concept-id-document.md) prend en charge l’extraction des approbations, des restrictions et des classifications de véhicules à partir de permis de conduire américains.
* L’[**API Modèle personnalisé (préversion)** ](concept-custom.md) prend en charge la détection de signatures pour les formulaires personnalisés.

### <a name="version-migration"></a>Migration de version

Apprenez à utiliser Form Recognizer v3.0 dans vos applications en suivant notre [**guide de migration Form Recognizer v3.0**](v3-migration-guide.md).

## <a name="next-steps"></a>Étapes suivantes

* [Découvrez comment traiter vos propres formulaires et documents](quickstarts/try-sample-label-tool.md) à l’aide de notre [exemple d’outil Form Recognizer](https://fott-2-1.azurewebsites.net/).

* Suivez un [démarrage rapide Form Recognizer](quickstarts/try-sdk-rest-api.md) et commencez à créer une application de traitement de formulaires dans le langage de développement de votre choix.
