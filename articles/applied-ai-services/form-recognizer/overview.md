---
title: Qu’est-ce qu’Azure Form Recognizer? (mise à jour)
titleSuffix: Azure Applied AI Services
description: Le service Azure Form Recognizer vous permet d’identifier et d’extraire des paires clé/valeur et des données de table de vos formulaires, ainsi que d’extraire des informations importantes des tickets de caisse et des cartes de visite.
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: overview
ms.date: 10/07/2021
ms.author: lajanuar
recommendations: false
keywords: traitement de données automatisé, traitement de documents, entrée de données automatisée, traitement des formulaires
ms.openlocfilehash: 272104444ca3389f69c639fba0984552b93c5893
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2021
ms.locfileid: "129712102"
---
<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD024 -->
# <a name="what-is-azure-form-recognizer"></a>Qu’est-ce qu’Azure Form Recognizer?

Azure Form Recognizer est un service [Azure Applied AI Services](../../applied-ai-services/index.yml) qui vous permet de générer des logiciels de traitement de données automatisés à l’aide de la technologie du machine learning. Form Recognizer analyse vos formulaires et documents, extrait le texte et les données, mappe les relations entre les champs en tant que paires clé-valeur et retourne une sortie JSON structurée. Vous pouvez rapidement obtenir des résultats justes et adaptés à votre contenu en particulier, sans avoir besoin d’une intervention manuelle excessive ou de compétences approfondies en science des données. Utilisez Form Recognizer pour automatiser le traitement des données dans les applications et les workflows, pour améliorer les stratégies pilotées par les données et pour enrichir les fonctionnalités de recherche de documents.

Form Recognizer identifie, extrait et analyse facilement les données de document suivantes :

* Structure et contenu des tableaux.
* Éléments et valeurs de champ des formulaires.
* Texte alphanumérique tapé à la machine et manuscrit.
* Relations entre les éléments.
* Paires clé-valeur.
* Emplacement des éléments avec les coordonnées de cadre englobant.

Cette documentation contient les types d’articles suivants :

* Les [**Concepts**](concept-layout.md) fournissent des explications approfondies sur les fonctions et fonctionnalités du service.
* Les [**Démarrages rapides**](quickstarts/try-sdk-rest-api.md) sont des instructions de prise en main qui vous guident dans la formulation de vos requêtes au service.
* Les [**Guides pratiques**](build-training-data-set.md) contiennent des instructions sur l’utilisation du service de manière plus spécifique ou personnalisée.
* Les [**Tutoriels**](tutorial-ai-builder.md) sont des guides plus longs qui montrent comment utiliser le service en tant que composant dans des solutions métier élargies.

## <a name="form-recognizer-features-and-development-options"></a>Fonctionnalités Form Recognizer et options de développement

### <a name="form-recognizer-ga-v21"></a>[Form Recognizer en disponibilité générale (v2.1)](#tab/v2-1)

Les fonctionnalités suivantes sont prises en charge par le service Form Recognizer v 2.1. Suivez les liens figurant dans ce tableau pour en savoir plus sur chaque fonctionnalité et parcourez les documents de référence sur les API.

| Fonctionnalité | Description | Options de développement |
|----------|--------------|-------------------------|
|[**API de disposition**](concept-layout.md) | Extraction et analyse de texte, de marques de sélection et de structures de tableaux, ainsi que de leurs coordonnées de cadre englobant, à partir de formulaires et de documents. | <ul><li>[**Outil d’étiquetage Form Recognizer**](https://fott-2-1.azurewebsites.net/layout-analyze)</li><li>[**API REST**](quickstarts/try-sdk-rest-api.md?pivots=programming-language-rest-api#analyze-layout)</li><li>[**Kit SDK Bibliothèque de client**](quickstarts/try-sdk-rest-api.md)</li><li>[**Conteneur Docker de Form Recognizer**](containers/form-recognizer-container-install-run.md?branch=main&tabs=layout#run-the-container-with-the-docker-compose-up-command)</li></ul>|
|[**Modèle de carte de visite**](concept-business-card.md) | Traitement automatisé des données et extraction d’informations clés à partir de cartes de visite.| <ul><li>[**Outil d’étiquetage Form Recognizer**](https://fott-2-1.azurewebsites.net/prebuilts-analyze)</li><li>[**API REST**](quickstarts/try-sdk-rest-api.md?pivots=programming-language-rest-api#analyze-business-cards)</li><li>[**Kit SDK Bibliothèque de client**](quickstarts/try-sdk-rest-api.md)</li><li>[**Conteneur Docker de Form Recognizer**](containers/form-recognizer-container-install-run.md?tabs=business-card#run-the-container-with-the-docker-compose-up-command)</li></ul>|
|[**Modèle de pièce d’identité**](concept-id-document.md) | Traitement automatisé des données et extraction des informations clés des passeports internationaux et des permis de conduire émis aux États-Unis.| <ul><li>[**Outil d’étiquetage Form Recognizer**](https://fott-2-1.azurewebsites.net/prebuilts-analyze)</li><li>[**API REST**](quickstarts/try-sdk-rest-api.md?pivots=programming-language-rest-api#analyze-identity-id-documents)</li><li>[**Kit SDK Bibliothèque de client**](quickstarts/try-sdk-rest-api.md)</li><li>[**Conteneur Docker de Form Recognizer**](containers/form-recognizer-container-install-run.md?tabs=id-document#run-the-container-with-the-docker-compose-up-command)</li></ul>|
|[**Modèle de facture**](concept-invoice.md) | Traitement automatisé des données et extraction des informations clés des factures de vente. | <ul><li>[**Outil d’étiquetage Form Recognizer**](https://fott-2-1.azurewebsites.net/prebuilts-analyze)</li><li>[**API REST**](quickstarts/try-sdk-rest-api.md?pivots=programming-language-rest-api#analyze-invoices)</li><li>[**Kit SDK Bibliothèque de client**](quickstarts/try-sdk-rest-api.md)</li><li>[**Conteneur Docker de Form Recognizer**](containers/form-recognizer-container-install-run.md?tabs=invoice#run-the-container-with-the-docker-compose-up-command)</li></ul>|
|[**Modèle de reçu**](concept-receipt.md) | Traitement automatisé des données et extraction des informations clés des reçus.| <ul><li>[**Outil d’étiquetage Form Recognizer**](https://fott-2-1.azurewebsites.net/prebuilts-analyze)</li><li>[**API REST**](quickstarts/try-sdk-rest-api.md?pivots=programming-language-rest-api#analyze-receipts)</li><li>[**Kit SDK Bibliothèque de client**](quickstarts/try-sdk-rest-api.md)</li><li>[**Conteneur Docker de Form Recognizer**](containers/form-recognizer-container-install-run.md?tabs=receipt#run-the-container-with-the-docker-compose-up-command)</li></ul>|
|[**Modèle personnalisé**](concept-business-card.md) | Extraction et analyse des données des formulaires et documents spécifiques à des données métier et à des cas d’usage distincts.| <ul><li>[**Outil d’étiquetage Form Recognizer**](https://fott-2-1.azurewebsites.net)</li><li>[**API REST**](quickstarts/try-sdk-rest-api.md?pivots=programming-language-rest-api#analyze-forms-with-a-custom-model)</li><li>[**Kit SDK Bibliothèque de client**](quickstarts/try-sdk-rest-api.md)</li><li>[**Conteneur Docker de Form Recognizer**](containers/form-recognizer-container-install-run.md?tabs=custom#run-the-container-with-the-docker-compose-up-command)</li></ul>|

### <a name="form-recognizer-preview-v30"></a>[Form Recognizer en préversion (v3.0)](#tab/v3-0)

>[!NOTE]
> Form Recognizer Studio est actuellement en préversion publique. Certaines fonctionnalités risquent de ne pas être prises en charge ou d’avoir des capacités limitées.

Les fonctionnalités et options de développement suivantes sont prises en charge par le service Form Recognizer v3.0. Suivez les liens figurant dans ce tableau pour en savoir plus sur chaque fonctionnalité et parcourez les documents de référence sur les API.

| Fonctionnalité | Description | Options de développement |
|----------|--------------|-------------------------|
|[🆕 **Modèle de document général**](concept-general-document.md)|Extrayez le texte, les tableaux, la structure, les paires clé-valeur et les entités nommées.|<ul ><li>[**Form Recognizer Studio**](https://formrecognizer.appliedai.azure.com)</li><li>[**API REST**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument)</li><li>[**Kit de développement logiciel (SDK) C#**](quickstarts/try-v3-csharp-sdk.md)</li><li>[**Kit de développement logiciel (SDK) Python**](quickstarts/try-v3-python-sdk.md)</li></ul> |
|[**Modèle de disposition**](concept-layout.md) | Extrayez le texte, les marques de sélection et les structures de tableaux ainsi que les coordonnées de leurs rectangles englobants, à partir de formulaires et de documents.</br></br> L’API Disposition a été mise à jour vers un modèle prédéfini. | <ul><li>[**Form Recognizer Studio**](https://formrecognizer.appliedai.azure.com)</li><li>[**API REST**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument)</li><li>[**Kit de développement logiciel (SDK) C#**](quickstarts/try-v3-csharp-sdk.md)</li><li>[**Kit de développement logiciel (SDK) Python**](quickstarts/try-v3-python-sdk.md)</li></ul>|
|[**Modèle personnalisé (mise à jour)** ](concept-custom.md) | Extraction et analyse des données des formulaires et documents spécifiques à des données métier et à des cas d’usage distincts.</br></br>L’API Modèle personnalisé v 3.0 prend en charge la **détection de signatures pour les formulaires personnalisés**.</li></ul>| <ul><li>[**Form Recognizer Studio**](https://fott-2-1.azurewebsites.net)</li><li>[**API REST**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument)</li><li>[**Kit de développement logiciel (SDK) C#**](quickstarts/try-v3-csharp-sdk.md)</li><li>[**Kit de développement logiciel (SDK) Python**](quickstarts/try-v3-python-sdk.md)</li></ul>|
|[**Modèle de reçu (mise à jour)** ](concept-receipt.md) | Traitement automatisé des données et extraction des informations clés des reçus.</br></br>Le modèle de reçu v3.0 prend en charge le traitement des **reçus d’hôtel d’une seule page**.| <ul><li>[**Form Recognizer Studio**](https://formrecognizer.appliedai.azure.com)</li><li>[**API REST**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument)</li><li>[**Kit de développement logiciel (SDK) C#**](quickstarts/try-v3-csharp-sdk.md)</li><li>[**Kit de développement logiciel (SDK) Python**](quickstarts/try-v3-python-sdk.md)</li></ul>|
|[**Modèle de document d’identité (mise à jour)** ](concept-id-document.md) |Traitement automatisé des données et extraction des informations clés des passeports internationaux et des permis de conduire émis aux États-Unis.</br></br>L’API de document d’identité prédéfini prend en charge l’**extraction des approbations, des restrictions et des classifications de véhicules des permis de conduire émis aux États-Unis**. |<ul><li> [**Form Recognizer Studio**](https://formrecognizer.appliedai.azure.com)</li><li>[**API REST**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument)</li><li>[**Kit de développement logiciel (SDK) C#**](quickstarts/try-v3-csharp-sdk.md)</li><li>[**Kit de développement logiciel (SDK) Python**](quickstarts/try-v3-python-sdk.md)</li></ul>|
|[**Modèle de facture**](concept-invoice.md) | Traitement automatisé des données et extraction des informations clés des factures de vente. | <ul><li>[**Form Recognizer Studio**](https://formrecognizer.appliedai.azure.com)</li><li>[**API REST**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument)</li><li>[**Kit de développement logiciel (SDK) C#**](quickstarts/try-v3-csharp-sdk.md)</li><li>[**Kit de développement logiciel (SDK) Python**](quickstarts/try-v3-python-sdk.md)</li></ul>|
|[**Modèle de carte de visite**](concept-business-card.md) |Traitement automatisé des données et extraction d’informations clés à partir de cartes de visite.| <ul><li>[**Form Recognizer Studio**](https://formrecognizer.appliedai.azure.com)</li><li>[**API REST**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument)</li><li>[**Kit de développement logiciel (SDK) C#**](quickstarts/try-v3-csharp-sdk.md)</li><li>[**Kit de développement logiciel (SDK) Python**](quickstarts/try-v3-python-sdk.md)</li></ul>|

---

## <a name="prerequisites"></a>Prérequis

* Un abonnement Azure – [**Créez-en un gratuitement**](https://azure.microsoft.com/free/cognitive-services).

* Une fois que vous avez votre abonnement, créez une [**ressource Form Recognizer**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer) dans le portail Azure. Vous pouvez utiliser le niveau tarifaire Gratuit (F0) pour tester le service, puis passer par la suite à un niveau payant pour la production.

* Enfin, vous devez récupérer l’**URL du point de terminaison** de votre ressource et la **clé d’API** à partir du portail Azure pour tester le service Form Recognizer :

  :::image type="content" source="media/containers/keys-and-endpoint.png" alt-text="Capture d’écran :fenêtre des clés et du point de terminaison dans le portail Azure":::

 ## <a name="form-recognizer-workflows"></a>Workflows de Form Recognizer

| Type du document | Considérations | Solution |
| -----------------|-------------------| ----------|
|<ul><li>**Facture**</li><li>**Réception**</li><li>**Carte de visite**</li></ul>| Votre facture, votre reçu ou votre carte de visite sont-ils composés de texte en anglais ? | <ul><li>Oui → modèle [**Facture**](concept-invoice.md), [**Reçu**](concept-receipt.md) ou [**Carte de visite**](concept-business-card.md)</li><li>Non → modèle [**Disposition**](concept-layout.md) ou [**Document général (préversion)** ](concept-general-document.md)</li></ul>|
|<ul><li>**Document d’identité**</li></ul>| Votre document d’identité est-il un passeport international ou un permis de conduire émis aux États-Unis ?| <ul><li>Oui → modèle [**Document d’identité**](concept-id-document.md)</li><li>Non → modèle [**Disposition**](concept-layout.md) ou [**Document général (préversion)** ](concept-general-document.md)</li></ul>|
|<ul><li>**Formulaire** ou **Document**</li></ul>| Votre formulaire ou document correspond-il à un format standard couramment utilisé dans votre entreprise ou votre secteur d’activité ?| <ul><li>Oui → modèle [**Disposition**](concept-id-document.md) ou [**Document général (préversion)** ](concept-general-document.md)</li><li>Non → [**Créer et entraîner un modèle personnalisé**](concept-layout.md) 

## <a name="data-privacy-and-security"></a>Sécurité et confidentialité des données

 Comme c’est le cas pour tous les services Cognitive Services, les développeurs utilisant le service Form Recognizer doivent connaître les politiques de Microsoft relatives aux données client. Consultez notre page [Données, confidentialité et sécurité pour Form Recognizer](/legal/cognitive-services/form-recognizer/fr-data-privacy-security). 

## <a name="next-steps"></a>Étapes suivantes

### <a name="form-recognizer-v21"></a>[Form Recognizer v2.1](#tab/v2-1)

> [!div class="checklist"]
>
> * Essayez notre [**outil en ligne d’étiquetage des exemples**](https://aka.ms/fott-2.1-ga/)
> * Suivez notre [**guide de démarrage rapide de la bibliothèque de client/API REST**](./quickstarts/try-sdk-rest-api.md) pour commencer à extraire des données de vos documents. Nous vous recommandons d’utiliser le service gratuit pendant que vous apprenez la technologie. N’oubliez pas que le nombre de pages gratuites est limité à 500 par mois.
> * Explorez la [**documentation de référence sur l’API REST**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1/operations/AnalyzeWithCustomForm) pour en savoir plus. 
> * Si vous êtes familiarisé avec une version précédente de l’API, consultez l’article [**Nouveautés**](./whats-new.md) pour en savoir plus sur les modifications récentes.

### <a name="form-recognizer-v30"></a>[Form Recognizer v3.0](#tab/v3-0)

> [!div class="checklist"]
>
> * Essayez [**Form Recognizer Studio**](https://formrecognizer.appliedai.azure.com)
> * Explorez la [**documentation de référence sur l’API REST**](https://westus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v3-0-preview-1/operations/AnalyzeDocument) pour en savoir plus. 
> * Si vous êtes familiarisé avec une version précédente de l’API, consultez l’article [**Nouveautés**](./whats-new.md) pour en savoir plus sur les modifications récentes.

---
