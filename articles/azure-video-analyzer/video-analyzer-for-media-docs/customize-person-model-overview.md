---
title: Personnaliser un modèle de personne dans Azure Video Analyzer for Media (anciennement Video Indexer) - Azure
titleSuffix: Azure Media Services
description: Cet article décrit ce qu’est un modèle de personne dans Azure Video Analyzer for Media (anciennement Video Indexer) et explique comment le personnaliser.
services: media-services
author: anikaz
manager: johndeu
ms.topic: article
ms.date: 05/15/2019
ms.author: kumud
ms.openlocfilehash: e9f5e8595f190dfb1d7097514c05ce8237efab39
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2021
ms.locfileid: "110385703"
---
# <a name="customize-a-person-model-in-video-analyzer-for-media"></a>Personnaliser un modèle de personne dans Video Analyzer for Media

Azure Video Analyzer for Media (anciennement Video Indexer) prend en charge la reconnaissance des célébrités dans vos vidéos. La fonctionnalité de reconnaissance de célébrités couvre environ un million de visages en fonction de la source de données couramment demandée comme IMDB, Wikipedia et les principaux influenceurs LinkedIn. Les visages qui ne sont pas reconnus par Video Analyzer for Media sont tout de même détectés, mais sont laissés sans nom. Les clients peuvent créer des modèles de personne personnalisés et activer Video Analyzer for Media pour identifier des visages qui ne sont pas reconnus par défaut. Les clients peuvent créer ces modèles de personne en associant le nom d’une personne avec les fichiers d’image du visage de la personne.  

Si votre compte est adapté à différents scénarios d’utilisation, vous pouvez peut-être tirer parti de la création de plusieurs modèles de personne par compte. Par exemple, si le contenu de votre compte est destiné à être trié dans différents canaux, vous souhaiterez peut-être créer un modèle de personne distinct pour chaque canal. 

> [!NOTE]
> Chaque modèle de personne prend en charge jusqu’à 1 million de personnes, et chaque compte a une limite de 50 modèles de personne. 

Une fois un modèle créé, vous pouvez l’utiliser en fournissant l’ID d’un modèle de personne spécifique lors du chargement /de l’indexation ou de la réindexation d’une vidéo. La formation du modèle sur un nouveau visage dans une vidéo met à jour le modèle personnalisé spécifique auquel la vidéo était associée. 

Si vous n’avez pas besoin de la prise en charge de plusieurs modèles de personne, n’affectez aucun ID de modèle de personne à votre vidéo lors de son chargement/de son indexation ou de sa réindexation. Dans ce cas, Video Analyzer for Media utilise le modèle de personne par défaut dans votre compte. 

Vous pouvez utiliser le site web Video Analyzer for Media pour modifier les visages qui ont été détectés dans une vidéo et pour gérer plusieurs modèles de personne personnalisés dans votre compte, comme indiqué dans la section [Personnaliser un modèle de personne avec le site web Video Indexer](customize-person-model-with-website.md). Vous pouvez également utiliser l’API, comme décrit dans la section  [Personnaliser un modèle de personne avec l’API Video Indexer](customize-person-model-with-api.md).
