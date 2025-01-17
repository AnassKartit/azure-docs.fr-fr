---
title: Azure SignalR comme source Event Grid
description: Décrit les propriétés qui sont fournies pour les événements Azure SignalR avec Azure Event Grid
ms.topic: conceptual
ms.date: 09/15/2021
ms.openlocfilehash: a7caf42d40308ea2f95d7bf71091fd8e0ee63088
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128651063"
---
# <a name="azure-event-grid-event-schema-for-signalr-service"></a>Schéma des événements Azure Event Grid pour SignalR Service

Cet article décrit les propriétés et le schéma des événements SignalR Service.  Pour une présentation des schémas d’événements, consultez [Schéma d’événements Azure Event Grid](event-schema.md). Cet article fournit également une liste de démarrages rapides et de tutoriels permettant d’utiliser Azure SignalR comme source d’événement.


## <a name="available-event-types"></a>Types d’événement disponibles

SignalR Service émet les types d’événements suivants :

| Type d'événement | Description |
| ---------- | ----------- |
| Microsoft.SignalRService.ClientConnectionConnected | Déclenché lorsqu’une connexion client est établie. |
| Microsoft.SignalRService.ClientConnectionDisconnected | Déclenché lorsqu’une connexion client est interrompue. |

## <a name="example-event"></a>Exemple d’événement

# <a name="event-grid-event-schema"></a>[Schéma d’événement Event Grid](#tab/event-grid-event-schema)
L’exemple suivant montre le schéma d’un événement de connexion client : 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionConnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Le schéma pour un événement de déconnexion de client est similaire : 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionDisconnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23",
    "errorMessage": "Internal server error."
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

# <a name="cloud-event-schema"></a>[Schéma d’événement cloud](#tab/cloud-event-schema)

L’exemple suivant montre le schéma d’un événement de connexion client : 

```json
[{
  "source": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "type": "Microsoft.SignalRService.ClientConnectionConnected",
  "time": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23"
  },
  "specversion": "1.0"
}]
```

Le schéma pour un événement de déconnexion de client est similaire : 

```json
[{
  "source": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "type": "Microsoft.SignalRService.ClientConnectionDisconnected",
  "time": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23",
    "errorMessage": "Internal server error."
  },
  "specversion": "1.0"
}]
```

---


### <a name="event-properties"></a>Propriétés d’événement


# <a name="event-grid-event-schema"></a>[Schéma d’événement Event Grid](#tab/event-grid-event-schema)
Un événement contient les données générales suivantes :

| Propriété | Type | Description |
| -------- | ---- | ----------- |
| `topic` | string | Chemin d’accès complet à la source de l’événement. Ce champ n’est pas modifiable. Event Grid fournit cette valeur. |
| `subject` | string | Chemin de l’objet de l’événement, défini par le serveur de publication. |
| `eventType` | string | Un des types d’événements inscrits pour cette source d’événement. |
| `eventTime` | string | L’heure à quelle l’événement est généré selon l’heure UTC du fournisseur. |
| `id` | string | Identificateur unique de l’événement. |
| `data` | object | Données d’événement SignalR Service. |
| `dataVersion` | string | Version du schéma de l’objet de données. Le serveur de publication définit la version du schéma. |
| `metadataVersion` | string | Version du schéma des métadonnées d’événement. Event Grid définit le schéma des propriétés de niveau supérieur. Event Grid fournit cette valeur. |

# <a name="cloud-event-schema"></a>[Schéma d’événement cloud](#tab/cloud-event-schema)

Un événement contient les données générales suivantes :

| Propriété | Type | Description |
| -------- | ---- | ----------- |
| `source` | string | Chemin d’accès complet à la source de l’événement. Ce champ n’est pas modifiable. Event Grid fournit cette valeur. |
| `subject` | string | Chemin de l’objet de l’événement, défini par le serveur de publication. |
| `type` | string | Un des types d’événements inscrits pour cette source d’événement. |
| `time` | string | L’heure à quelle l’événement est généré selon l’heure UTC du fournisseur. |
| `id` | string | Identificateur unique de l’événement. |
| `data` | object | Données d’événement SignalR Service. |
| `specversion` | string | Version de la spécification de schéma CloudEvents. |

---

L’objet de données comporte les propriétés suivantes :

| Propriété | Type | Description |
| -------- | ---- | ----------- |
| `timestamp` | string | L’heure à quelle l’événement est généré selon l’heure UTC du fournisseur. |
| `hubName` | string | Hub auquel appartient la connexion client. |
| `connectionId` | string | Identificateur unique de la connexion client. |
| `userId` | string | L’identificateur d’utilisateur défini dans la revendication. |
| `errorMessage` | string | L’erreur qui provoque la déconnexion. |

## <a name="tutorials-and-how-tos"></a>Tutoriels et guides pratiques
|Intitulé | Description |
|---------|---------|
| [Réagir aux événements Azure SignalR Service à l’aide d’Event Grid](../azure-signalr/signalr-concept-event-grid-integration.md) | Vue d’ensemble de l’intégration d’Azure SignalR Service à Event Grid. |
| [Comment envoyer des événements Azure SignalR Service à Event Grid](../azure-signalr/signalr-howto-event-grid-integration.md) | Montre comment envoyer des événements Azure SignalR Service à une application via Event Grid. |

## <a name="next-steps"></a>Étapes suivantes

* Pour une présentation d’Azure Event Grid, consultez [Présentation d’Event Grid](overview.md).
* Pour plus d’informations sur la création d’un abonnement Azure Event Grid, consultez [Schéma d’abonnement à Event Grid](subscription-creation-schema.md).
