---
title: Qu’est-ce qu’Azure Content Moderator ?
titleSuffix: Azure Cognitive Services
description: Découvrez comment utiliser Content Moderator pour suivre, marquer, évaluer et filtrer le matériau inapproprié dans le contenu généré par l’utilisateur.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: overview
ms.date: 04/16/2021
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: modérateur de contenu, modérateur de contenu Azure, modérateur en ligne, logiciel de filtrage de contenu, service de modération de contenu, modération du contenu
ms.openlocfilehash: 931b57b35cfca94b35a2b5c267d4469d5ca5ec07
ms.sourcegitcommit: c05e595b9f2dbe78e657fed2eb75c8fe511610e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/11/2021
ms.locfileid: "112031706"
---
# <a name="what-is-azure-content-moderator"></a>Qu’est-ce qu’Azure Content Moderator ?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

[!INCLUDE [deprecation notice](includes/tool-deprecation.md)]

Azure Content Moderator est un service d’IA qui vous permet de gérer le contenu potentiellement offensant, risqué ou indésirable. Il comprend le service de modération de contenu avec intelligence artificielle qui analyse le texte, les images et les vidéos et applique automatiquement des indicateurs de contenu, ainsi que l’outil de vérification, un environnement de modération en ligne pour une équipe de réviseurs humains.

Vous pouvez être amené à intégrer un logiciel de filtrage de contenu dans votre application afin de vous conformer aux réglementations ou de maintenir l’environnement souhaité pour vos utilisateurs.

Cette documentation contient les types d’articles suivants :  

* Les [**Démarrages rapides**](client-libraries.md) sont des instructions de prise en main qui vous guident dans la formulation de vos requêtes au service.  
* Les [**Guides pratiques**](try-text-api.md) contiennent des instructions sur l’utilisation du service de manière plus spécifique ou personnalisée.  
* Les [**Concepts**](text-moderation-api.md) fournissent des explications approfondies sur les fonctions et fonctionnalités du service.  
* Les [**Tutoriels**](ecommerce-retail-catalog-moderation.md) sont des guides plus longs qui montrent comment utiliser le service en tant que composant dans des solutions métier élargies.  

## <a name="where-its-used"></a>Utilisation

Voici quelques scénarios dans lesquels un développeur de logiciels ou une équipe requiert un service de modération de contenu :

- Modération des catalogues de produits et du contenu généré par l’utilisateur sur les marketplaces en ligne
- Modération des artefacts créés par les utilisateurs et des messageries instantanées par les sociétés de jeu
- Modération des images, du texte et des vidéos ajoutés par les utilisateurs sur les plateformes de messagerie sociale
- Mise en place par les sociétés de contenu d’entreprise d’un système de modération centralisé du contenu
- Filtrage du contenu inapproprié pour les élèves et les enseignants par les fournisseurs de solutions éducatives (maternelle-terminale)

> [!IMPORTANT]
> Vous ne pouvez pas utiliser Content Moderator pour détecter des images illégales d’exploitation d’enfants. Cependant, les organisations qualifiées peuvent utiliser le [service cloud PhotoDNA](https://www.microsoft.com/photodna "Service cloud PhotoDNA de Microsoft") pour détecter ce type de contenu.

## <a name="what-it-includes"></a>Contenu

Le service Content Moderator se compose de plusieurs API de service web disponibles via les appels REST et un SDK .NET. Il inclut également l’outil de révision, qui permet aux réviseurs humains d’aider le service et d’améliorer ou d’ajuster sa fonction de modération.

## <a name="moderation-apis"></a>API de modération

Le service Content Moderator inclut des API de modération qui vérifient le contenu afin d'y détecter des éléments potentiellement inappropriés ou répréhensibles.

![Schéma fonctionnel des API de modération Content Moderator](images/content-moderator-mod-api.png)

Le tableau suivant décrit les différents types d’API de modération.

| Groupe d’API | Description |
| ------ | ----------- |
|[**Modération de texte**](text-moderation-api.md)| Analyse du texte à la recherche de contenu offensant, de contenu sexuellement explicite ou suggestif, de blasphèmes et de données personnelles.|
|[**Listes de termes personnalisées**](try-terms-list-api.md)| Analyse du texte par rapport à une liste personnalisée de termes, en plus des termes prédéfinis. Utilisez les listes personnalisées pour bloquer ou autoriser du contenu en fonction de vos propres stratégies de contenu.|  
|[**Modération d’images**](image-moderation-api.md)| Analyse des images à la recherche de contenu pour adultes et osé, détection de texte dans les images grâce à la reconnaissance optique des caractères (OCR) et détection des visages.|
|[**Listes d’images personnalisées**](try-image-list-api.md)| Analyse des images par rapport à une liste d’images personnalisée. Utilisez les listes d’images personnalisées pour filtrer les instances de contenu récurrent que vous ne souhaitez pas classifier à nouveau.|
|[**Modération de vidéos**](video-moderation-api.md)| Analyse des vidéos à la recherche de contenu pour adultes ou osé, en renvoyant des marqueurs de temps pour ce contenu.|

## <a name="review-apis"></a>Consultation des API

Les API de révision vous permettent d’intégrer votre pipeline de modération avec les réviseurs humains. Utilisez les opérations [Travaux](review-api.md#jobs), [Révisions](review-api.md#reviews) et [Flux de travail](review-api.md#workflows) pour créer et automatiser les workflows impliquant une intervention humaine avec l'[outil de révision](#review-tool) (ci-dessous).

> [!NOTE]
> L’API de workflow n’est pas encore disponible dans le kit de développement logiciel (SDK) .NET, mais peut être utilisée avec le point de terminaison REST.

![Schéma fonctionnel des API de révision Content Moderator](images/content-moderator-rev-api.png)

## <a name="review-tool"></a>Outil de révision

Le service Content Moderator inclut également l’[Outil de révision](Review-Tool-User-Guide/human-in-the-loop.md) basé sur le web, qui héberge les révisions de contenu en vue de leur traitement par des modérateurs humains. Les entrées humaines n’entraînent pas le service, mais le travail combiné du service et des équipes de vérification humaine permet aux développeurs de trouver le bon équilibre entre efficacité et précision. L’outil de révision fournit également un front-end convivial pour plusieurs ressources Content Moderator.

![Page d’accueil de l’outil de révision Content Moderator](images/homepage.PNG)

## <a name="data-privacy-and-security"></a>Sécurité et confidentialité des données

Comme avec tous les services Cognitive Services, les développeurs utilisant le service Content Moderator doivent connaître les politiques de Microsoft relatives aux données client. Pour en savoir plus, consultez la [page Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) dans le Centre de gestion de la confidentialité Microsoft.

## <a name="next-steps"></a>Étapes suivantes

Pour commencer à utiliser Content Moderator sur le portail web, suivez [Essayer Content Moderator sur le web](quick-start.md). Vous pouvez également effectuer un [démarrage rapide de la bibliothèque cliente ou de l’API REST](client-libraries.md) pour implémenter les scénarios de base dans le code.