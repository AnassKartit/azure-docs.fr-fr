---
title: Vue d’ensemble de l’enregistrement d’appel Azure Communication Services
titleSuffix: An Azure Communication Services concept document
description: Fournit une vue d’ensemble de la fonctionnalité d’enregistrement d’appel et des API associées.
author: GrantMeStrength
manager: anvalent
services: azure-communication-services
ms.author: jken
ms.date: 06/30/2021
ms.topic: conceptual
ms.custom: references_regions
ms.service: azure-communication-services
ms.openlocfilehash: fef4972271046f7435140fd2d9ba3d18c7c3b11c
ms.sourcegitcommit: 2eac9bd319fb8b3a1080518c73ee337123286fa2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2021
ms.locfileid: "123254758"
---
# <a name="calling-recording-overview"></a>Vue d’ensemble de l’enregistrement d’appel

[!INCLUDE [Public Preview](../../includes/public-preview-include-document.md)]

> [!NOTE]
> L’enregistrement d’appel est disponible pour les ressources Communication Services créées aux États-Unis, au Royaume-Uni, en Europe, en Asie et en Australie.

L’enregistrement d’appel fournit un ensemble d’API permettant de démarrer, d’arrêter, de suspendre et de reprendre l’enregistrement. Ces API sont accessibles à partir de la logique métier côté serveur, ou via des événements déclenchés par des actions utilisateur. La sortie multimédia enregistrée est au format MP4 Audio+Video, qui est le même format que celui utilisé par Teams pour enregistrer des données multimédias. Les notifications liées au contenu multimédia et aux métadonnées sont émises via Event Grid. Les enregistrements sont stockés pendant 48 heures sur un stockage temporaire intégré, en prévision de leur récupération et de leur déplacement vers une solution de stockage à long terme de votre choix. 

![Diagramme du concept d’enregistrement d’appel](../media/call-recording-concept.png)

## <a name="media-output-types"></a>Types de sorties multimédias
L’enregistrement d’appel prend actuellement en charge le format de sortie MP4 audio mixte+vidéo. Les données multimédias de sortie correspondent aux enregistrements de réunion produits par le biais de l’enregistrement Microsoft Teams.

| Type de canal | Format du contenu | Vidéo | Audio |
| :----------- | :------------- | :---- | :--------------------------- |
| audioVideo | mp4 | vidéo 1920x1080 8 FPS de tous les participants dans la disposition des vignettes par défaut | audio mixte mp4a 16kHz de tous les participants |


## <a name="run-time-control-apis"></a>API de contrôle à l’exécution
Les API de contrôle à l’exécution peuvent être utilisées pour gérer l’enregistrement via des déclencheurs de logique métier internes, tels qu’une application créant un appel de groupe et enregistrant la conversation, ou à partir d’une action déclenchée par l’utilisateur qui indique à l’application serveur de démarrer l’enregistrement. Les API d’enregistrement d’appel sont des [API Out-of-Call](./call-automation-apis.md#out-of-call-apis), utilisant `serverCallId` pour initier l’enregistrement. Lors de la création d’un appel, un élément `serverCallId` est renvoyé via l'événement `Microsoft.Communication.CallLegStateChanged` après que l’appel a été établi. `serverCallId` figure dans le champ `data.serverCallId`. Consultez notre [Exemple de démarrage rapide d’enregistrement d’appel](../../quickstarts/voice-video-calling/call-recording-sample.md) pour en savoir plus sur la récupération de `serverCallId` à parti du kit de développement logiciel (SDK) Calling. Un élément `recordingOperationId` est renvoyé lorsque l’enregistrement est démarré, puis utilisé pour les opérations de suivi telle que les suspensions et reprises.   

| Opération                            | Fonctionne sur            | Commentaires                       |
| :-------------------- | :--------------------- | :----------------------------- |
| Démarrer l’enregistrement       | `serverCallId`         | Retourne `recordingOperationId`. | 
| Obtenir l’état de l’enregistrement   | `recordingOperationId` | Retourne `recordingState`.       | 
| Suspendre l'enregistrement       | `recordingOperationId` | La suspension et la reprise de l’enregistrement d’appel vous permettent d’ignorer l’enregistrement d’une partie d’un appel ou d’une réunion, et de reprendre l’enregistrement dans un fichier unique. | 
| Reprendre l’enregistrement      | `recordingOperationId` | Reprend une opération d’enregistrement suspendue. Le contenu est inclus dans le même fichier que le contenu avant suspension. | 
| Arrêter l’enregistrement        | `recordingOperationId` | Arrête l’enregistrement et lance le traitement du média final pour le téléchargement de fichiers. | 


## <a name="event-grid-notifications"></a>Notifications Event Grid

> Azure Communication Services fournit un stockage multimédia à court terme pour les enregistrements. **Exportez le contenu enregistré que vous souhaitez conserver sous 48 heures.** Après 48 heures, les enregistrements ne seront plus disponibles.

Une notification Event Grid `Microsoft.Communication.RecordingFileStatusUpdated` est publiée lorsqu’un enregistrement est prêt pour la récupération, en général quelques minutes après la fin du processus d’enregistrement (par exemple, réunion terminée, enregistrement arrêté). Les notifications d’événements d’enregistrement incluent des éléments `contentLocation` et `metadataLocation` utilisés pour récupérer les informations multimédias enregistrées et un fichier de métadonnées d’enregistrement.

### <a name="notification-schema-reference"></a>Référence du schéma de notification
```
{
    "id": string, // Unique guid for event
    "topic": string, // Azure Communication Services resource id
    "subject": string, // /recording/call/{call-id}
    "data": {
        "recordingStorageInfo": {
            "recordingChunks": [
                {
                    "documentId": string, // Document id for retrieving from storage
                    "index": int, // Index providing ordering for this chunk in the entire recording
                    "endReason": string, // Reason for chunk ending: "SessionEnded", "ChunkMaximumSizeExceeded”, etc.
                    "metadataLocation": <string>, // url of the metadata for this chunk
                    "contentLocation": <string>   // url of the mp4, mp3, or wav for this chunk
                }
            ]
        },
        "recordingStartTime": string, // ISO 8601 date time for the start of the recording
        "recordingDurationMs": int, // Duration of recording in milliseconds
        "sessionEndReason": string // Reason for call ending: "CallEnded", "InitiatorLeft", etc.
    },
    "eventType": string, // "Microsoft.Communication.RecordingFileStatusUpdated"
    "dataVersion": string, // "1.0"
    "metadataVersion": string, // "1"
    "eventTime": string // ISO 8601 date time for when the event was created
}
```
## <a name="regulatory-and-privacy-concerns"></a>Questions liées à la réglementation et à la confidentialité

De nombreux pays et états ont des lois et des réglementations qui s’appliquent à l’enregistrement des appels RTC, vocaux et vidéo ; elles exigent souvent le consentement des utilisateurs pour procéder à l’enregistrement de leurs communications. Il vous incombe d’utiliser les fonctionnalités d’enregistrement d’appel en conformité avec la loi. Vous devez obtenir le consentement des parties impliquées dans les communications enregistrées, d’une manière qui soit conforme aux lois applicables à chaque participant.

Les réglementations relatives à la maintenance des données personnelles exigent la possibilité d’exporter des données utilisateur. Afin de prendre en charge ces exigences, l’enregistrement des fichiers de métadonnées inclut l’ID participant de chaque participant à un appel dans le tableau `participants`. Vous pouvez croiser les MRI dans le tableau `participants` avec vos identités d’utilisateur internes pour identifier les participants dans un appel. Un exemple de fichier de métadonnées d’enregistrement est fourni ci-dessous pour référence.

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations, consultez [Exemple de démarrage rapide d’enregistrement d’appel](../../quickstarts/voice-video-calling/call-recording-sample.md).

En savoir plus sur les [API d’automatisation des appels](./call-automation-apis.md).
