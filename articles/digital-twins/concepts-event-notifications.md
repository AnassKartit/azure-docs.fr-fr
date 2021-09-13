---
title: Notifications d’événement
titleSuffix: Azure Digital Twins
description: Découvrez comment interpréter différents types d’événements et leurs messages de notification.
author: baanders
ms.author: baanders
ms.date: 4/8/2021
ms.topic: conceptual
ms.service: digital-twins
ms.custom: contperf-fy21q4
ms.openlocfilehash: d008888968f05641786cdfcb73afac1d540b7596
ms.sourcegitcommit: 2da83b54b4adce2f9aeeed9f485bb3dbec6b8023
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2021
ms.locfileid: "122769866"
---
# <a name="event-notifications"></a>Notifications d’événement

Dans Azure Digital Twins, plusieurs événements peuvent produire des **notifications**. Celles-ci permettent au back-end de la solution de savoir quand des actions se produisent. Ces notifications sont ensuite [routées](concepts-route-events.md) vers différents emplacements, à l’intérieur et à l’extérieur d’Azure Digital Twins, qui peuvent utiliser ces informations pour prendre des mesures.

Plusieurs types de notifications peuvent être générés. En outre, les messages de notification peuvent être différents selon le type d’événement qui les a générés. Cet article donne des détails sur les différents types de messages, ainsi que sur leur aspect.

Ce graphique montre les différents types de notifications :

[!INCLUDE [digital-twins-notifications.md](../../includes/digital-twins-notifications.md)]

## <a name="notification-structure"></a>Structure de notification

En général, les notifications sont constituées de deux parties : l’en-tête et le corps. 

### <a name="event-notification-headers"></a>En-têtes des notifications d’événements

Les en-têtes de message de notification sont représentés par des paires clé-valeur. Les en-têtes de message sont sérialisés différemment, selon le protocole utilisé (MQTT, AMQP ou HTTP). Cette section donne des informations générales sur les en-têtes des messages de notification, sans prendre en compte le protocole et la sérialisation choisis.

Certaines notifications sont conformes à la norme [CloudEvents](https://cloudevents.io/). La conformité CloudEvents est la suivante.
* Les notifications émises par les appareils continuent de suivre les spécifications existantes concernant les notifications.
* Les notifications traitées et émises par IoT Hub continuent de suivre les spécifications existantes concernant les notifications, sauf lorsque IoT Hub choisit de prendre en charge CloudEvents, par exemple, via Event Grid.
* Les notifications émises par des [jumeaux numériques](concepts-twins-graph.md) avec un [modèle](concepts-models.md) se conforment à CloudEvents.
* Les notifications traitées et émises par Azure Digital Twins sont conformes à CloudEvents.

Les services doivent ajouter un numéro séquentiel à toutes les notifications pour indiquer l’ordre dans lequel elles doivent s’afficher, ou sinon, les organiser d’une autre façon. 

Les notifications émises par Azure Digital Twins pour Event Grid sont automatiquement mises en forme selon le schéma CloudEvents ou le schéma EventGridEvent, en fonction du type de schéma défini dans la rubrique Event Grid. 

Les attributs d’extension des en-têtes sont ajoutés en tant que propriétés dans le schéma Event Grid, dans la charge utile. 

### <a name="event-notification-bodies"></a>Corps des notifications d’événements

Le corps des messages de notification est présenté ici en JSON. Le corps du message peut être sérialisé différemment selon le type de sérialisation souhaité (par exemple, avec JSON, CBOR, Protobuf, etc.).

L’ensemble de champs que le corps contient varie en fonction du type de notification.

Les sections suivantes décrivent en détail les différents types de notifications émises par IoT Hub et Azure Digital Twins (ou d’autres services Azure IoT). Vous verrez ce qui déclenche chaque type de notification, et vous verrez l’ensemble de champs qui est inclus avec chaque type de corps de notification.

## <a name="digital-twin-change-notifications"></a>Notification de changement du jumeau numérique

Les **notifications de changement de jumeau numérique** sont déclenchées lors de la mise à jour d’un jumeau numérique, par exemple :
* Lorsque les valeurs de propriété ou les métadonnées sont modifiées.
* En cas de modification des métadonnées d’un jumeau numérique ou d’un composant. C’est le cas, par exemple, lorsque vous changez le modèle d’un jumeau numérique.

### <a name="properties"></a>Propriétés

Voici les champs compris dans le corps d’une notification de modification d’un jumeau numérique.

| Nom    | Valeur |
| --- | --- |
| `id` | Identificateur de la notification, tel qu’un UUID ou un compteur géré par le service. `source` + `id` est unique pour chaque événement. |
| `source` | Nom de l’instance IoT Hub ou Azure Digital Twins, par exemple *myhub.azure-devices.net* ou *mydigitaltwins.westus2.azuredigitaltwins.net* |
| `data` | Document de correctif JSON décrivant la mise à jour apportée au jumeau. Pour plus d’informations, consultez les [Détails du corps](#body-details) ci-dessous. |
| `specversion` | *1.0*<br>Le message est conforme à cette version de la [spécification CloudEvents](https://github.com/cloudevents/spec). |
| `type` | `Microsoft.DigitalTwins.Twin.Update` |
| `datacontenttype` | `application/json` |
| `subject` | ID du jumeau numérique |
| `time` | Horodatage du moment où l’opération s’est produite au niveau du jumeau numérique |
| `traceparent` | Contexte de trace W3C pour l’événement |

### <a name="body-details"></a>Détails du corps

Dans le message, le champ `data` contient un document de correctif JSON contenant la mise à jour du jumeau numérique.

Par exemple, imaginons qu’un jumeau numérique a été mis à jour à l’aide du correctif suivant.

:::code language="json" source="~/digital-twins-docs-samples/models/patch-component-2.json":::

Les données de la notification correspondante (si elle est exécutée de façon synchrone par le service, comme lorsqu’Azure Digital Twins met à jour un jumeau numérique) aurait un corps comme le suivant :

```json
{
    "modelId": "dtmi:example:com:floor4;2",
    "patch": [
      {
        "value": 40,
        "path": "/Temperature",
        "op": "replace"
      },
      {
        "value": 30,
        "path": "/comp1/prop1",
        "op": "add"
      }
    ]
  }
```

Il s’agit des informations qui seront placées dans le champ `data` du message de notification du cycle de vie.

## <a name="digital-twin-lifecycle-notifications"></a>Notifications de cycle de vie de jumeaux numériques

Tous les [jumeaux numériques](concepts-twins-graph.md) émettent des notifications, qu’ils représentent ou non des [appareils IoT Hub dans Azure Digital Twins](how-to-ingest-iot-hub-data.md). Ceci est dû aux **notifications de cycle de vie**, qui concernent uniquement le jumeau numérique.

Des notifications de cycle de vie sont déclenchées dans les cas suivants :
* La création d’un jumeau numérique
* La suppression d’un jumeau numérique

### <a name="properties"></a>Propriétés

Voici les champs compris dans le corps d’une notification de cycle de vie.

| Nom | Valeur |
| --- | --- |
| `id` | Identificateur de la notification, tel qu’un UUID ou un compteur géré par le service. `source` + `id` est unique pour chaque événement. |
| `source` | Nom de l’instance IoT Hub ou Azure Digital Twins, par exemple *myhub.azure-devices.net* ou *mydigitaltwins.westus2.azuredigitaltwins.net* |
| `data` | Les données du jumeau numérique qui rencontre l’événement du cycle de vie. Pour plus d’informations, consultez les [Détails du corps](#body-details-1) ci-dessous. |
| `specversion` | *1.0*<br>Le message est conforme à cette version de la [spécification CloudEvents](https://github.com/cloudevents/spec). |
| `type` | `Microsoft.DigitalTwins.Twin.Create`<br>`Microsoft.DigitalTwins.Twin.Delete` |
| `datacontenttype` | `application/json` |
| `subject` | ID du jumeau numérique |
| `time` | Horodatage du moment où l’opération s’est produite au niveau du jumeau |
| `traceparent` | Contexte de trace W3C pour l’événement |

### <a name="body-details"></a>Détails du corps

Voici un exemple de message de notification de cycle de vie : 

```json
{
  "specversion": "1.0",
  "id": "d047e992-dddc-4a5a-b0af-fa79832235f8",
  "type": "Microsoft.DigitalTwins.Twin.Create",
  "source": "contoso-adt.api.wus2.digitaltwins.azure.net",
  "data": {
    "$dtId": "floor1",
    "$etag": "W/\"e398dbf4-8214-4483-9d52-880b61e491ec\"",
    "$metadata": {
      "$model": "dtmi:example:Floor;1"
    }
  },
  "subject": "floor1",
  "time": "2020-06-23T19:03:48.9700792Z",
  "datacontenttype": "application/json",
  "traceparent": "00-18f4e34b3e4a784aadf5913917537e7d-691a71e0a220d642-01"
}
```

Dans le message, le `data` champ contient les données du jumeau numérique affecté, représenté au format JSON. Le schéma *Digital Twins Resource 7.1* est utilisé pour ce champ `data`.

Pour les événements de création, la charge utile `data` reflète l’état du jumeau après la création de la ressource. Par conséquent, elle doit inclure tous les éléments générés par le système, tout comme un appel `GET`.

Voici un exemple de données pour un appareil [IoT Plug-and-Play](../iot-develop/overview-iot-plug-and-play.md), avec des composants et aucune propriété de niveau supérieur. Les propriétés qui n’ont pas d’utilité pour les appareils (telles que les propriétés signalées) doivent être omises. L’objet JSON suivant correspond aux informations qui seront placées dans le champ `data` du message de notification du cycle de vie :

```json
{
  "$dtId": "device-digitaltwin-01",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "thermostat": {
    "temperature": 80,
    "humidity": 45,
    "$metadata": {
      "$model": "dtmi:com:contoso:Thermostat;1",
      "temperature": {
        "desiredValue": 85,
        "desiredVersion": 3,
        "ackVersion": 2,
        "ackCode": 200,
        "ackDescription": "OK"
      },
      "humidity": {
        "desiredValue": 40,
        "desiredVersion": 1,
        "ackVersion": 1,
        "ackCode": 200,
        "ackDescription": "OK"
      }
    }
  },
  "$metadata": {
    "$model": "dtmi:com:contoso:Thermostat_X500;1",
  }
}
```

Voici un autre exemple de données de jumeau numérique. Celui-ci est basé sur un [modèle](concepts-models.md) et ne prend pas en charge les composants :

```json
{
  "$dtId": "logical-digitaltwin-01",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "avgTemperature": 70,
  "comfortIndex": 85,
  "$metadata": {
    "$model": "dtmi:com:contoso:Building;1",
    "avgTemperature": {
      "desiredValue": 72,
      "desiredVersion": 5,
      "ackVersion": 4,
      "ackCode": 200,
      "ackDescription": "OK"
    },
    "comfortIndex": {
      "desiredValue": 90,
      "desiredVersion": 1,
      "ackVersion": 3,
      "ackCode": 200,
      "ackDescription": "OK"
    }
  }
}
```

## <a name="digital-twin-relationship-change-notifications"></a>Notifications de modification d’une relation de jumeau numérique

Les **notifications de modification de relation** sont déclenchées lorsqu’une relation d’un jumeau numérique est créée, mise à jour ou supprimée. 

### <a name="properties"></a>Propriétés

Voici les champs du corps d’une notification de modification d’une relation.

| Nom    | Valeur |
| --- | --- |
| `id` | Identificateur de la notification, tel qu’un UUID ou un compteur géré par le service. `source` + `id` est unique pour chaque événement. |
| `source` | Nom de l’instance Azure Digital Twins, comme *mydigitaltwins.westus2.azuredigitaltwins.net* |
| `data` | Charge utile de la relation qui a été modifiée. Pour plus d’informations, consultez les [Détails du corps](#body-details-2) ci-dessous. |
| `specversion` | *1.0*<br>Le message est conforme à cette version de la [spécification CloudEvents](https://github.com/cloudevents/spec). |
| `type` | `Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br>`Microsoft.DigitalTwins.Relationship.Delete` |
| `datacontenttype` | `application/json` |
| `subject` | ID de la relation, comme `<twin-ID>/relationships/<relationshipID>` |
| `time` | Horodatage du moment où l’opération s’est produite au niveau de la relation |
| `traceparent` | Contexte de trace W3C pour l’événement |

### <a name="body-details"></a>Détails du corps

Dans le message, le champ `data` contient la charge utile d’une relation, au format JSON. Il utilise le même format qu’une requête `GET` pour une relation via l’[API DigitalTwins](/rest/api/digital-twins/dataplane/twins). 

Voici un exemple de données pour une notification de relation de mise à jour. « La mise à jour d’une relation » signifie que les propriétés de la relation ont changé, de sorte que les données affichent la propriété mise à jour et sa nouvelle valeur. L’objet JSON suivant correspond aux informations qui seront placées dans le champ `data` du message de notification de la relation du jumeau numérique :

```json
{
    "modelId": "dtmi:example:Floor;1",
    "patch": [
      {
        "value": "user3",
        "path": "/ownershipUser",
        "op": "replace"
      }
    ]
  }
```

Voici un exemple de données pour une notification de création ou de suppression de relation. Pour `Relationship.Delete`, le corps est le même que celui de la requête `GET`, et il obtient l’état le plus récent avant la suppression.

```json
{
    "$relationshipId": "device_to_device",
    "$etag": "W/\"72479873-0083-41a8-83e2-caedb932d881\"",
    "$relationshipName": "Connected",
    "$targetId": "device2",
    "connectionType": "WIFI"
}
```

## <a name="digital-twin-telemetry-messages"></a>Messages de télémétrie du jumeau numérique

Les **messages de télémétrie** sont reçus dans Azure Digital Twins à partir des appareils connectés qui collectent et envoient des mesures.

### <a name="properties"></a>Propriétés

Voici les champs compris dans le corps d’un message de télémétrie.

| Nom    | Valeur |
| --- | --- |
| `id` | Identificateur de la notification, fourni par le client lors de l’appel de l’API de télémétrie. |
| `source` | Nom complet du jumeau numérique auquel l’événement de télémétrie a été envoyé. Utilisez le format suivant : `<your-Digital-Twin-instance>.api.<your-region>.digitaltwins.azure.net/<twin-ID>`. |
| `specversion` | *1.0*<br>Le message est conforme à cette version de la [spécification CloudEvents](https://github.com/cloudevents/spec). |
| `type` | `microsoft.iot.telemetry` |
| `data` | Message de télémétrie envoyé aux jumeaux numériques. La charge utile n’est pas modifiée et peut ne pas s’aligner avec le schéma du jumeau numérique qui a reçu la télémétrie. |
| `dataschema` | le schéma de données est l'ID de modèle du jumeau ou du composant qui émet les données de télémétrie. Par exemple : `dtmi:example:com:floor4;2`. |
| `datacontenttype` | `application/json` |
| `traceparent` | Contexte de trace W3C pour l’événement. |

### <a name="body-details"></a>Détails du corps

Le corps contient la mesure de télémétrie, ainsi que des informations contextuelles sur l’appareil.

Voici un exemple de corps de message de télémétrie : 

```json
{
  "specversion": "1.0",
  "id": "df5a5992-817b-4e8a-b12c-e0b18d4bf8fb",
  "type": "microsoft.iot.telemetry",
  "source": "contoso-adt.api.wus2.digitaltwins.azure.net/digitaltwins/room1",
  "data": {
    "Temperature": 10
  },
  "dataschema": "dtmi:example:com:floor4;2",
  "datacontenttype": "application/json",
  "traceparent": "00-7e3081c6d3edfb4eaf7d3244b2036baa-23d762f4d9f81741-01"
}
```

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur la transmission d’événements à différentes destinations, à l’aide des points de terminaison et des itinéraires :
* [Routes d’événements](concepts-route-events.md)
