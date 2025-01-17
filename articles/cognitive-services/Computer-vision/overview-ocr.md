---
title: Présentation de la Reconnaissance optique de caractères
titleSuffix: Azure Cognitive Services
description: Le service Reconnaissance optique de caractères extrait le texte visible d'une image et le renvoie sous forme de chaînes structurées.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: overview
ms.date: 06/21/2021
ms.author: pafarley
ms.custom: seodec18, devx-track-csharp
ms.openlocfilehash: 02654c3f196b6c3bc199e636b3601777ed372a99
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2021
ms.locfileid: "129992531"
---
# <a name="what-is-optical-character-recognition"></a>Présentation de la Reconnaissance optique de caractères

La reconnaissance optique de caractères (OCR) vous permet d’extraire du texte imprimé ou manuscrit à partir d’images, comme des photos de plaques de rue ou de produits, ainsi qu’à partir de documents (factures, rapports financiers, articles, etc.). Les technologies OCR de Microsoft prennent en charge l’extraction de texte imprimé en [plusieurs langues](./language-support.md). Pour bien démarrer, suivez un [guide de démarrage rapide](./quickstarts-sdk/client-library.md).

![Versions de démonstration OCR](./Images/ocr-demo.gif)

Cette documentation contient les types d’articles suivants :
* Les [guides de démarrage rapide](./quickstarts-sdk/client-library.md) sont des instructions pas à pas qui vous permettent d’effectuer des appels au service et d’obtenir des résultats en peu de temps. 
* Les [guides patiques](./Vision-API-How-to-Topics/call-read-api.md) contiennent des instructions sur l’utilisation du service de manière plus spécifique ou personnalisée.
<!--* The [conceptual articles](Vision-API-How-to-Topics/call-read-api.md) provide in-depth explanations of the service's functionality and features.
* The [tutorials](./tutorials/storage-lab-tutorial.md) are longer guides that show you how to use this service as a component in broader business solutions. -->

## <a name="read-api"></a>API Lire 

L’[API Read](https://centraluseuap.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/5d986960601faab4bf452005) du service Vision par ordinateur est la dernière technologie OCR d’Azure ([découvrir les nouveautés](./whats-new.md)). Elle extrait le texte imprimé (en plusieurs langues), le texte manuscrit (en plusieurs langues), les chiffres et les symboles monétaires à partir d’images et de documents PDF multipages. Elle est optimisée pour extraire le texte d’images à forte composante textuelle et de documents PDF multipages en langue mixte. Elle prend en charge la détection de texte imprimé et manuscrit dans la même image ou le même document.

![Comment la reconnaissance optique de caractères convertit les images et les documents en une sortie structurée avec du texte extrait](./Images/how-ocr-works.svg)

## <a name="input-requirements"></a>Critères des entrées

L’appel **Lire** utilise des images et des documents comme entrée. Les conditions requises sont les suivantes :

* Formats de fichiers pris en charge : JPEG, PNG, BMP, PDF et TIFF.
* Pour les fichiers PDF et TIFF, jusqu’à 2000 pages (seules les deux premières pages pour le niveau gratuit) sont traitées.
* La taille de fichier doit être inférieure à 50 Mo (6 Mo pour le niveau gratuit), et les dimensions comprises entre 50 × 50 pixels et 10000 × 10000 pixels. 

## <a name="supported-languages"></a>Langues prises en charge
L’API Read prend en charge 122 langues pour le texte imprimé et 7 langues pour le texte manuscrit, y compris les langues et les fonctionnalités en préversion.

L’OCR pour le texte à imprimé prend en charge l’anglais, le français, l’allemand, l’italien, le portugais, l’espagnol, le chinois, le japonais, le coréen et le russe (en préversion), ainsi que les langues latines et cyrilliques dans la dernière mise à jour de la préversion.

L’OCR pour le texte manuscrit comprend la prise en charge de l’anglais, le français (en préversion), l’allemand, l’italien, le portugais, l’espagnol et le chinois.

Consultez [Comment spécifier la version du modèle](./Vision-API-How-to-Topics/call-read-api.md#determine-how-to-process-the-data-optional) pour utiliser les langages et fonctionnalités en préversion. Consultez la liste complète des [langues prises en charge par OCR](./language-support.md#optical-character-recognition-ocr). Le modèle en préversion comprend toutes les améliorations apportées aux langues et fonctionnalités actuellement en disponibilité générale.

## <a name="key-features"></a>Fonctionnalités clés

L’API Read comprend les fonctionnalités suivantes.

* Extraction de texte imprimé en 122 langues
* Extraction de texte manuscrit en 7 langues
* Lignes de texte et mots avec scores de localisation et de confiance
* Aucune identification de langue requise
* Prise en charge des langues mixtes et du mode mixte (impression et écriture manuscrite)
* Sélection de pages et de plages de pages à partir de grands documents multipages
* Option d’ordre de lecture naturel pour la sortie des lignes de texte (Latin uniquement)
* Classification de l’écriture manuscrite pour les lignes de texte (Latin uniquement)
* Disponible en tant que conteneur Docker Distroless pour un déploiement local

Découvrez [comment utiliser les fonctionnalités OCR](./vision-api-how-to-topics/call-read-api.md).

## <a name="use-the-cloud-api-or-deploy-on-premise"></a>Utiliser l’API cloud ou déployer localement
Les API cloud Read 3.x sont l’option préférée pour la plupart des clients en raison de la facilité d’intégration et de la productivité rapide prête à l’emploi. Azure et le service de Vision par ordinateur gèrent l’évolutivité, les performances, la sécurité des données et les besoins en matière de conformité tout en répondant aux besoins de vos clients.

Pour un déploiement local, le [conteneur Docker Read (préversion)](./computer-vision-how-to-install-containers.md) vous permet de déployer les nouvelles capacités OCR dans votre propre environnement local. Les conteneurs conviennent particulièrement bien à certaines exigences de sécurité et de gouvernance des données.

> [!WARNING]
> Les opérations RecognizeText de Vision par ordinateur 2.0 seront prochainement remplacées par la nouvelle [API Read](#read-api) abordée dans cet article. Les clients existants devraient [effectuer la transition vers les opérations de lecture](upgrade-api-versions.md).

## <a name="data-privacy-and-security"></a>Sécurité et confidentialité des données

Comme avec tous les services Cognitive Services, les développeurs utilisant le service Vision par ordinateur doivent connaître les politiques de Microsoft relatives aux données client. Pour en savoir plus, consultez la [page Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) dans le Centre de gestion de la confidentialité Microsoft.

## <a name="next-steps"></a>Étapes suivantes

- Démarrez avec les [guides de démarrage rapide consacrés à la bibliothèque de client ou à l’API REST (Lire) OCR](./quickstarts-sdk/client-library.md).
- Découvrez l’[API REST Lire 3.2](https://westus.dev.cognitive.microsoft.com/docs/services/computer-vision-v3-2/operations/5d986960601faab4bf452005).
