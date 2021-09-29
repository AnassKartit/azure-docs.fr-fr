---
title: 'Démarrage rapide : Bibliothèque de client Form Recognizer ou API REST'
titleSuffix: Azure Applied AI Services
description: Utilisez la bibliothèque de client Form Recognizer ou l’API REST afin de créer une application de traitement des formulaires qui extrait des paires clé/valeur et des données de table de vos documents personnalisés.
author: laujan
manager: nitinme
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 04/14/2021
ms.author: lajanuar
zone_pivot_groups: programming-languages-set-formre
ms.custom: devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
keywords: traitement des formulaires, traitement de données automatisé
ms.openlocfilehash: a1b355bdd67d9bf84a2c9ae24fb557a9e50fa1d9
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128652527"
---
# <a name="quickstart-get-started-with-the-client-library-sdks-or-rest-api"></a>Démarrage rapide : Bien démarrer avec les SDK de bibliothèque de client ou l’API REST

Commencez avec Azure Form Recognizer dans le langage de programmation de votre choix. Azure Form Recognizer est un [service IA appliqué](../../../applied-ai-services/index.yml) qui vous permet de générer des logiciels de traitement de données automatisé à l’aide de la technologie Machine Learning. Identifiez et extrayez du texte, des paires clé/valeur, des marques de sélection, des données de table, et bien plus encore dans vos formulaires : le service génère des données structurées qui incluent les relations dans le fichier d’origine. Vous pouvez utiliser Form Recognizer via l’API REST ou le kit SDK. Nous vous recommandons d’utiliser le service gratuit pendant que vous apprenez la technologie. N’oubliez pas que le nombre de pages gratuites est limité à 500 par mois.

Vous allez utiliser les API suivantes pour extraire des données structurées de formulaires et de documents :

* [Authentifier le client](#authenticate-the-client)
* [Analyser la disposition](#analyze-layout)
* [Analyser les reçus](#analyze-receipts)
* [Analyser les cartes de visite](#analyze-business-cards)
* [Analyser les factures](#analyze-invoices)
* [Analyser les documents d’identité](#analyze-identity-documents)
* [Entraîner un modèle personnalisé](#train-a-custom-model)
* [Analyser les formulaires avec un modèle personnalisé](#analyze-forms-with-a-custom-model)
* [Gérer des modèles personnalisés](#manage-custom-models)

::: zone pivot="programming-language-csharp"

[!INCLUDE [C# SDK quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Java SDK quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [NodeJS SDK quickstart](../includes/quickstarts/javascript-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Python SDK quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="programming-language-rest-api"

[!INCLUDE [REST API quickstart](../includes/quickstarts/rest-api.md)]

::: zone-end