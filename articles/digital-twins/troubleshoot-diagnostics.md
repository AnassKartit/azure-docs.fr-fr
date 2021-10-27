---
title: 'Résolution des problèmes : journaux des diagnostics'
titleSuffix: Azure Digital Twins
description: Dans cet article, découvrez comment activer la journalisation avec les paramètres de diagnostic et comment interroger les journaux pour un affichage immédiat. Découvrez également les catégories de journaux et leurs schémas.
author: baanders
ms.author: baanders
ms.date: 9/24/2021
ms.topic: how-to
ms.service: digital-twins
ms.custom: contperf-fy22q1
ms.openlocfilehash: 89b7c741ce75a629de99e3337428027429bce5b7
ms.sourcegitcommit: 147910fb817d93e0e53a36bb8d476207a2dd9e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2021
ms.locfileid: "130131768"
---
# <a name="troubleshooting-azure-digital-twins-diagnostics-logs"></a>Dépannage d’Azure Digital Twins : Journaux des diagnostics

Cet article vous montre comment configurer les paramètres de diagnostic dans le [portail Azure](https://portal.azure.com), y compris les types de journaux à collecter et où les stocker (comme Log Analytics ou un compte de stockage de votre choix). Ensuite, vous pouvez interroger les journaux pour obtenir rapidement des informations personnalisées.

Azure Digital Twins peut collecter les **journaux** de votre instance de service pour superviser ses performances, son accès et d’autres données. Vous pouvez utiliser ces journaux pour obtenir un aperçu de ce qui se passe dans votre instance Azure Digital Twins et analyser des causes racines sur des problèmes sans avoir besoin de contacter le support Azure.

Cet article contient également des informations sur toutes les **catégories de journaux** que les jumeaux numériques Azure peuvent collecter, ainsi que leurs **schémas**.

## <a name="turn-on-diagnostic-settings"></a>Activer les paramètres de diagnostic 

Activez les paramètres de diagnostic pour démarrer la collecte des journaux sur votre instance Azure Digital Twins. Vous pouvez aussi choisir la destination à laquelle les journaux exportés doivent être stockés. Voici comment activer les paramètres de diagnostic pour votre instance Azure Digital Twins.

1. Connectez-vous au [portail Azure](https://portal.azure.com) et accédez à votre instance Azure Digital Twins. Vous pouvez la trouver en tapant son nom dans la barre de recherche du portail. 

2. Sélectionnez **Paramètres de diagnostic** dans le menu, puis **Ajouter un paramètre de diagnostic**.

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Capture d’écran montrant la page des paramètres de diagnostic dans le portail Azure et le bouton à ajouter" lightbox="media/troubleshoot-diagnostics/diagnostic-settings.png":::

3. Dans la page qui suit, renseignez les valeurs suivantes :
     * **Nom du paramètre de diagnostic** : Nommez les paramètres de diagnostic.
     * **Détails de la catégorie** : choisissez les opérations à surveiller, puis cochez les cases pour activer les diagnostics pour ces opérations. Les opérations sur lesquelles les paramètres de diagnostic peuvent établir des rapports sont :
        - DigitalTwinsOperation
        - EventRoutesOperation
        - ModelsOperation
        - QueryOperation
        - AllMetrics
        
        Pour plus d’informations sur ces catégories et les informations qu’elles contiennent, consultez la section [Catégories de journaux](#log-categories) ci-dessous.
     * **Détails de la destination** : Indiquez où vous voulez envoyer les journaux d’activité. Vous pouvez sélectionner n’importe quelle combinaison des trois options suivantes :
        - Envoyer à Log Analytics
        - Archiver dans un compte de stockage
        - Diffuser vers un hub d’événements

        Il se peut que vous soyez invité à fournir des détails supplémentaires s’ils sont nécessaires pour la sélection de votre destination.  
    
4. Enregistrez les nouveaux paramètres. 

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings-details.png" alt-text="Capture d’écran montrant la page de paramètres de diagnostic dans le portail Azure où l’utilisateur a rempli des informations de paramètres de diagnostic" lightbox="media/troubleshoot-diagnostics/diagnostic-settings-details.png":::

Les nouveaux paramètres prennent effet au bout de 10 minutes environ. Après cela, les journaux réapparaissent dans la cible configurée sur la page **Paramètres de diagnostic** de votre instance. 

Pour plus d’informations sur les paramètres de diagnostic et leurs options de configuration, vous pouvez consulter [Créer des paramètres de diagnostic pour envoyer des journaux et des métriques de plateforme à différentes destinations](../azure-monitor/essentials/diagnostic-settings.md).

## <a name="view-and-query-logs"></a>Afficher et interroger les journaux

Après avoir configuré les détails de stockage de vos journaux Azure Digital Twins, vous pouvez écrire des **requêtes personnalisées** pour eux afin de générer des informations et de résoudre les problèmes. Le service fournit également quelques exemples de requêtes qui peuvent vous aider à démarrer, en répondant aux questions courantes que les clients peuvent se poser sur leurs instances.

Voici comment interroger les journaux de votre instance.

1. Connectez-vous au [portail Azure](https://portal.azure.com) et accédez à votre instance Azure Digital Twins. Vous pouvez la trouver en tapant son nom dans la barre de recherche du portail. 

2. Sélectionnez **Journaux** dans le menu pour ouvrir la page d’interrogation de journal. Cette page s’ouvre dans une fenêtre nommée *Requêtes*.

    :::image type="content" source="media/troubleshoot-diagnostics/logs.png" alt-text="Capture d’écran montrant la page Journaux pour une instance Azure Digital Twins dans le portail Azure avec la fenêtre Requêtes superposée, montrant les requêtes prédéfinies" lightbox="media/troubleshoot-diagnostics/logs.png":::

    Il s’agit d’exemples de requêtes prédéfinies écrites pour divers journaux. Vous pouvez sélectionner l’une des requêtes pour la charger dans l’éditeur de requête, puis l’exécuter afin de voir ces journaux pour votre instance.

    Vous pouvez aussi fermer la fenêtre *Requêtes* sans rien exécuter pour accéder directement à la page de l’éditeur de requête, où vous pouvez écrire ou modifier du code de requête personnalisé.

3. Après avoir quitté la fenêtre *Requêtes*, la page principale de l’éditeur de requête s’affiche. Ici, vous pouvez voir et modifier le texte des exemples de requêtes, ou écrire vos propres requêtes ex nihilo.
    :::image type="content" source="media/troubleshoot-diagnostics/logs-query.png" alt-text="Capture d’écran montrant la page Journaux pour une instance Azure Digital Twins dans le portail Azure. Elle comprend la liste des journaux, le code de requête et l’historique des requêtes." lightbox="media/troubleshoot-diagnostics/logs-query.png":::

    Dans le volet gauche : 
    - L’onglet *Tables* présente les différentes [catégories de journaux](#log-categories) Azure Digital Twins utilisables dans vos requêtes. 
    - L’onglet *Requêtes* contient les exemples de requêtes que vous pouvez charger dans l’éditeur.
    - L’onglet *Filtre* vous permet de personnaliser une vue filtrée des données retournées par la requête.

Pour plus d’informations sur les requêtes de journal et sur la façon de les écrire, vous pouvez consulter [Vue d’ensemble des requêtes de journal dans Azure Monitor](../azure-monitor/logs/log-query-overview.md).

## <a name="log-categories"></a>Catégories de journal

Voici plus d’informations sur les catégories de journaux collectées par Azure Digital Twins.

| Catégorie de journal | Description |
| --- | --- |
| ADTModelsOperation | Consigner tous les appels d’API en lien avec des modèles |
| ADTQueryOperation | Consigner tous les appels d’API en lien avec des requêtes |
| ADTEventRoutesOperation | Consigner tous les appels d’API se rapportant aux itinéraires d’événements, ainsi que la sortie d’événements provenant d’Azure Digital Twins vers un service de point de terminaison comme Event Grid, Event Hubs et Service Bus |
| ADTDigitalTwinsOperation | Consigner tous les appels d’API en lien avec des jumeaux individuels |

Chaque catégorie de journal se compose d'opérations d'écriture, de lecture, de suppression et d'action. Ces opérations sont mappées à des appels d'API REST, comme suit :

| Type d'événement | Opérations de l'API REST |
| --- | --- |
| Write | PUT et PATCH |
| Lire | GET |
| DELETE | Suppression |
| Action | POST |

Voici une liste complète des opérations et des [appels d’API REST Azure Digital Twins](/rest/api/azure-digitaltwins/) correspondants consignés dans chaque catégorie. 

>[!NOTE]
> Chaque catégorie de journal contient plusieurs opérations/appels d’API REST. Dans le tableau ci-dessous, chaque catégorie de journal est mappée à l’ensemble des opérations/appels d’API REST en dessous, jusqu’à ce que la catégorie suivante du journal soit affichée. 

| Catégorie de journal | Opération | Appels d’API REST et autres événements |
| --- | --- | --- |
| ADTModelsOperation | Microsoft.DigitalTwins/models/write | API de mise à jour des modèles de jumeaux numériques |
|  | Microsoft.DigitalTwins/models/read | API de modèles de jumeaux numériques Get by ID et Liste |
|  | Microsoft.DigitalTwins/models/delete | API de suppression des modèles de jumeaux numériques |
|  | Microsoft.DigitalTwins/models/action | API d’ajout des modèles de jumeaux numériques |
| ADTQueryOperation | Microsoft.DigitalTwins/query/action | API de jumeaux numériques de requête |
| ADTEventRoutesOperation | Microsoft.DigitalTwins/eventroutes/write | API d’ajout de routages d’événements |
|  | Microsoft.DigitalTwins/eventroutes/read | API de routages d'événements Get by ID et Liste |
|  | Microsoft.DigitalTwins/eventroutes/delete | API de suppression de routages d’événements |
|  | Microsoft.DigitalTwins/eventroutes/action | Échec lors de la tentative de publication des événements sur un service de point de terminaison (pas un appel d’API) |
| ADTDigitalTwinsOperation | Microsoft.DigitalTwins/digitaltwins/write | Ajouter des jumeaux numériques, Ajouter une relation, Mettre à jour, Mettre à jour un composant |
|  | Microsoft.DigitalTwins/digitaltwins/read | Jumeaux numériques Get by ID, Obtenir un composant, Obtenir une relation par ID, Relations entrantes, Lister les relations |
|  | Microsoft.DigitalTwins/digitaltwins/delete | Supprimer des jumeaux numériques, Supprimer une relation |
|  | Microsoft.DigitalTwins/digitaltwins/action | Envoyer des données de télémétrie de composant de jumeaux numériques, Envoyer des données de télémétrie |

## <a name="log-schemas"></a>Schémas des journaux 

Chaque catégorie de journal dispose d'un schéma qui définit la façon dont les événements de cette catégorie sont signalés. Chaque entrée de journal est stockée sous forme de texte et formatée en tant qu'objet blob JSON. Les champs du journal et des exemples de corps JSON sont fournis pour chaque type de journal ci-dessous. 

`ADTDigitalTwinsOperation`, `ADTModelsOperation` et `ADTQueryOperation` utilisent un schéma de journal d’API cohérent. `ADTEventRoutesOperation` étend le schéma pour qu’il contienne un champ `endpointName` dans les propriétés.

### <a name="api-log-schemas"></a>Schéma des journaux d'API

Ce schéma de journal est cohérent pour `ADTDigitalTwinsOperation`, `ADTModelsOperation` et `ADTQueryOperation`. Le même schéma est également utilisé pour `ADTEventRoutesOperation`, à l’**exception** du nom de l’opération `Microsoft.DigitalTwins/eventroutes/action` (pour plus d’informations sur ce schéma, consultez la section suivante, [Schémas de journaux de sortie](#egress-log-schemas)).

Le schéma contient des informations relatives aux appels d’API à une instance d’Azure Digital Twins.

Vous trouverez ci-dessous les descriptions des champs et des propriétés des journaux d'API.

| Nom du champ | Type de données | Description |
|-----|------|-------------|
| `Time` | DateTime | Date et heure (UTC) auxquelles l'événement s'est produit |
| `ResourceId` | String | ID Azure Resource Manager de la ressource sur laquelle l'événement s'est produit |
| `OperationName` | String  | Type d'action réalisée pendant l'événement |
| `OperationVersion` | String | Version de l'API utilisée pendant l'événement |
| `Category` | String | Type de ressource émise |
| `ResultType` | String | Résultat de l'événement |
| `ResultSignature` | String | Code d'état HTTP de l'événement |
| `ResultDescription` | String | Détails supplémentaires sur l'événement |
| `DurationMs` | String | Temps nécessaire pour exécuter l'événement, en millisecondes |
| `CallerIpAddress` | String | Adresse IP source masquée de l'événement |
| `CorrelationId` | Guid | Le client a fourni un identificateur unique pour l'événement |
| `ApplicationId` | Guid | ID d’application utilisé dans l’autorisation du porteur |
| `Level` | Int | Gravité de la journalisation de l'événement |
| `Location` | String | Région où s'est produit l'événement |
| `RequestUri` | Uri | Point de terminaison utilisé pendant l'événement |
| `TraceId` | String | `TraceId`, dans le cadre du [contexte de suivi du W3C](https://www.w3.org/TR/trace-context/). ID de la trace entière utilisé pour identifier de façon unique une trace distribuée sur plusieurs systèmes. |
| `SpanId` | String | `SpanId` dans le cadre du [contexte de suivi du W3C](https://www.w3.org/TR/trace-context/). ID de cette requête dans la trace. |
| `ParentId` | String | `ParentId` dans le cadre du [contexte de suivi du W3C](https://www.w3.org/TR/trace-context/). Une requête sans ID parent est la racine de la trace. |
| `TraceFlags` | String | `TraceFlags` dans le cadre du [contexte de suivi du W3C](https://www.w3.org/TR/trace-context/). Contrôle des indicateurs de suivi tels que l’échantillonnage, le niveau de trace, etc. |
| `TraceState` | String | `TraceState` dans le cadre du [contexte de suivi du W3C](https://www.w3.org/TR/trace-context/). Informations d’identification de trace supplémentaires spécifiques au fournisseur pour s’étendre sur différents systèmes de suivi distribués. |

Vous trouverez ci-dessous des exemples de corps JSON correspondant à ces types de journaux.

#### <a name="adtdigitaltwinsoperation"></a>ADTDigitalTwinsOperation

```json
{
  "time": "2020-03-14T21:11:14.9918922Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/digitaltwins/write",
  "operationVersion": "2020-10-31",
  "category": "DigitalTwinOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": 8,
  "callerIpAddress": "13.68.244.*",
  "correlationId": "2f6a8e64-94aa-492a-bc31-16b9f0b16ab3",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/digitaltwins/factory-58d81613-2e54-4faa-a930-d980e6e2a884?api-version=2020-10-31",
  "properties": {},
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
}
```

#### <a name="adtmodelsoperation"></a>ADTModelsOperation

```json
{
  "time": "2020-10-29T21:12:24.2337302Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/models/write",
  "operationVersion": "2020-10-31",
  "category": "ModelsOperation",
  "resultType": "Success",
  "resultSignature": "201",
  "resultDescription": "",
  "durationMs": "80",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "9dcb71ea-bb6f-46f2-ab70-78b80db76882",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/Models?api-version=2020-10-31",
  "properties": {},
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
}
```

#### <a name="adtqueryoperation"></a>ADTQueryOperation

```json
{
  "time": "2020-12-04T21:11:44.1690031Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/query/action",
  "operationVersion": "2020-10-31",
  "category": "QueryOperation",
  "resultType": "Success",
  "resultSignature": "200",
  "resultDescription": "",
  "durationMs": "314",
  "callerIpAddress": "13.68.244.*",
  "correlationId": "1ee2b6e9-3af4-4873-8c7c-1a698b9ac334",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/query?api-version=2020-10-31",
  "properties": {},
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
}
```

#### <a name="adteventroutesoperation"></a>ADTEventRoutesOperation

Voici un exemple de corps JSON pour une `ADTEventRoutesOperation` qui n’est **pas** de type `Microsoft.DigitalTwins/eventroutes/action` (pour plus d’informations sur ce schéma, consultez la section suivante, [Schémas de journaux de sortie](#egress-log-schemas)).

```json
  {
    "time": "2020-10-30T22:18:38.0708705Z",
    "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
    "operationName": "Microsoft.DigitalTwins/eventroutes/write",
    "operationVersion": "2020-10-31",
    "category": "EventRoutesOperation",
    "resultType": "Success",
    "resultSignature": "204",
    "resultDescription": "",
    "durationMs": 42,
    "callerIpAddress": "212.100.32.*",
    "correlationId": "7f73ab45-14c0-491f-a834-0827dbbf7f8e",
    "identity": {
      "claims": {
        "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
      }
    },
    "level": "4",
    "location": "southcentralus",
    "uri": "https://myinstancename.api.scus.digitaltwins.azure.net/EventRoutes/egressRouteForEventHub?api-version=2020-10-31",
    "properties": {},
    "traceContext": {
      "traceId": "95ff77cfb300b04f80d83e64d13831e7",
      "spanId": "b630da57026dd046",
      "parentId": "9f0de6dadae85945",
      "traceFlags": "01",
      "tracestate": "k1=v1,k2=v2"
    }
  },
```

### <a name="egress-log-schemas"></a>Schéma des journaux de sortie

L’exemple suivant est un schéma pour les journaux `ADTEventRoutesOperation` spécifiques au nom d’opération `Microsoft.DigitalTwins/eventroutes/action`. Ceux-ci contiennent des détails sur les exceptions et les opérations d'API relatives aux points de terminaison de sortie connectés à une instance d'Azure Digital Twins.

|Nom du champ | Type de données | Description |
|-----|------|-------------|
| `Time` | DateTime | Date et heure (UTC) auxquelles l'événement s'est produit |
| `ResourceId` | String | ID Azure Resource Manager de la ressource sur laquelle l'événement s'est produit |
| `OperationName` | String  | Type d'action réalisée pendant l'événement |
| `Category` | String | Type de ressource émise |
| `ResultDescription` | String | Détails supplémentaires sur l'événement |
| `CorrelationId` | Guid | Le client a fourni un identificateur unique pour l'événement |
| `ApplicationId` | Guid | ID d’application utilisé dans l’autorisation du porteur |
| `Level` | Int | Gravité de la journalisation de l'événement |
| `Location` | String | Région où s'est produit l'événement |
| `TraceId` | String | `TraceId`, dans le cadre du [contexte de suivi du W3C](https://www.w3.org/TR/trace-context/). ID de la trace entière utilisé pour identifier de façon unique une trace distribuée sur plusieurs systèmes. |
| `SpanId` | String | `SpanId` dans le cadre du [contexte de suivi du W3C](https://www.w3.org/TR/trace-context/). ID de cette requête dans la trace. |
| `ParentId` | String | `ParentId` dans le cadre du [contexte de suivi du W3C](https://www.w3.org/TR/trace-context/). Une requête sans ID parent est la racine de la trace. |
| `TraceFlags` | String | `TraceFlags` dans le cadre du [contexte de suivi du W3C](https://www.w3.org/TR/trace-context/). Contrôle des indicateurs de suivi tels que l’échantillonnage, le niveau de trace, etc. |
| `TraceState` | String | `TraceState` dans le cadre du [contexte de suivi du W3C](https://www.w3.org/TR/trace-context/). Informations d’identification de trace supplémentaires spécifiques au fournisseur pour s’étendre sur différents systèmes de suivi distribués. |
| `EndpointName` | String | Nom du point de terminaison de sortie créé dans Azure Digital Twins |

Vous trouverez ci-dessous des exemples de corps JSON correspondant à ces types de journaux.

#### <a name="adteventroutesoperation-for-microsoftdigitaltwinseventroutesaction"></a>ADTEventRoutesOperation pour Microsoft.DigitalTwins/eventroutes/action

Voici un exemple de corps JSON pour une `ADTEventRoutesOperation` qui est de type `Microsoft.DigitalTwins/eventroutes/action`.

```json
{
  "time": "2020-11-05T22:18:38.0708705Z",
  "resourceId": "/SUBSCRIPTIONS/BBED119E-28B8-454D-B25E-C990C9430C8F/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.DIGITALTWINS/DIGITALTWINSINSTANCES/MYINSTANCENAME",
  "operationName": "Microsoft.DigitalTwins/eventroutes/action",
  "operationVersion": "",
  "category": "EventRoutesOperation",
  "resultType": "",
  "resultSignature": "",
  "resultDescription": "Unable to send EventHub message to [myPath] for event Id [f6f45831-55d0-408b-8366-058e81ca6089].",
  "durationMs": -1,
  "callerIpAddress": "",
  "correlationId": "7f73ab45-14c0-491f-a834-0827dbbf7f8e",
  "identity": {
    "claims": {
      "appId": "872cd9fa-d31f-45e0-9eab-6e460a02d1f1"
    }
  },
  "level": "4",
  "location": "southcentralus",
  "uri": "",
  "properties": {
    "endpointName": "myEventHub"
  },
  "traceContext": {
    "traceId": "95ff77cfb300b04f80d83e64d13831e7",
    "spanId": "b630da57026dd046",
    "parentId": "9f0de6dadae85945",
    "traceFlags": "01",
    "tracestate": "k1=v1,k2=v2"
  }
},
```

## <a name="next-steps"></a>Étapes suivantes

* Pour en savoir plus sur la configuration des diagnostics, consultez [Collecter et utiliser des données de journaux à partir de vos ressources Azure](../azure-monitor/essentials/platform-logs-overview.md).
* Pour plus d’informations sur les métriques d’Azure Digital Twins, consultez [Dépannage : métriques](troubleshoot-metrics.md).
* Pour savoir comment activer les alertes pour vos métriques, consultez [Résolution des problèmes : alertes](troubleshoot-alerts.md).