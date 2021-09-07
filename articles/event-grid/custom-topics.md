---
title: Rubriques personnalisées dans Azure Event Grid
description: Décrit les rubriques personnalisées dans Azure Event Grid.
ms.topic: conceptual
ms.date: 07/27/2021
ms.openlocfilehash: 54d331d2080d8adf96bf39c9250e7d6888723c1d
ms.sourcegitcommit: f2eb1bc583962ea0b616577f47b325d548fd0efa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2021
ms.locfileid: "114727749"
---
# <a name="custom-topics-in-azure-event-grid"></a>Rubriques personnalisées dans Azure Event Grid
Une rubrique Event Grid fournit un point de terminaison où la source envoie des événements. L’éditeur crée la rubrique Event Grid et décide si une source d’événements a besoin d’une ou plusieurs rubriques. Une rubrique est utilisée pour une collection d’événements connexes. Pour répondre à certains types d’événements, les abonnés décident des rubriques auxquelles ils souhaitent s’abonner.

Les **rubriques personnalisées** sont des rubriques tierces et applicatives. Quand vous créez une rubrique personnalisée ou que l’accès à une rubrique personnalisée vous est accordé, celle-ci est visible dans votre abonnement. 

Lorsque vous concevez votre application, vous pouvez choisir le nombre de rubriques à créer. Pour les solutions volumineuses, créez une **rubrique personnalisée** pour **chaque catégorie d’événements associés**. Par exemple, envisagez une application qui envoie des événements liés à la modification de comptes d’utilisateur et au traitement de commandes. Il est peu probable qu’un gestionnaire d’événements accepte les deux catégories d’événements. Créez deux rubriques personnalisées et laissez les gestionnaires d’événements s’abonner à celle qui les intéresse. Pour les solutions de petite taille, vous pouvez à la place envoyer tous les événements à une seule rubrique. Les abonnés à des événements peuvent filtrer les types d’événements qu’ils souhaitent.

## <a name="event-schema"></a>Schéma d’événement
Pour obtenir une vue d’ensemble détaillée du schéma d’événement, consultez [Schéma d’événement Azure Event Grid](event-schema.md). Pour les rubriques personnalisées, l’éditeur d’événements détermine l’objet de **données**. Les données de niveau supérieur doivent avoir les mêmes champs que des événements standard définis par les ressources.

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

Les sections suivantes contiennent des liens vers des tutoriels permettant de créer des rubriques personnalisées à l’aide du portail Azure, de l’interface CLI, de PowerShell et de modèles Azure Resource Manager (ARM). 


## <a name="azure-portal-tutorials"></a>Tutoriels pour le portail Azure
|Intitulé  |Description  |
|---------|---------|
| [Démarrage rapide : Créer et acheminer des événements personnalisés avec le portail Azure](custom-event-quickstart-portal.md) | Montre comment utiliser le portail pour envoyer des événements personnalisés. |
| [Démarrage rapide : Acheminer des événements personnalisés vers le stockage File d’attente Azure](custom-event-to-queue-storage.md) | Décrit comment envoyer des événements personnalisés à un stockage File d’attente. |
| [Guide pratique pour publier dans une rubrique personnalisée](post-to-custom-topic.md) | Montre comment publier un événement dans une rubrique personnalisée. |


## <a name="azure-cli-tutorials"></a>Tutoriels pour Azure CLI
|Intitulé  |Description  |
|---------|---------|
| [Démarrage rapide : Créer et acheminer des événements personnalisés avec Azure CLI](custom-event-quickstart.md) | Montre comment utiliser Azure CLI pour envoyer des événements personnalisés. |
| [Azure CLI : Créer une rubrique personnalisée Event Grid](./scripts/event-grid-cli-create-custom-topic.md)|Exemple de script qui crée une rubrique personnalisée. Le script récupère le point de terminaison et une clé.|
| [Azure CLI : S’abonner aux événements d’une rubrique personnalisée](./scripts/event-grid-cli-subscribe-custom-topic.md)|Exemple de script qui crée un abonnement pour une rubrique personnalisée. Il envoie les événements vers un webhook.|

## <a name="azure-powershell-tutorials"></a>Tutoriels pour Azure PowerShell
|Intitulé  |Description  |
|---------|---------|
| [Démarrage rapide : Créer et acheminer des événements personnalisés avec PowerShell](custom-event-quickstart-powershell.md) | Montre comment utiliser Azure PowerShell pour envoyer des événements personnalisés. |
| [PowerShell : Créer une rubrique personnalisée Event Grid](./scripts/event-grid-powershell-create-custom-topic.md)|Exemple de script qui crée une rubrique personnalisée. Le script récupère le point de terminaison et une clé.|
| [PowerShell : S’abonner aux événements d’une rubrique personnalisée](./scripts/event-grid-powershell-subscribe-custom-topic.md)|Exemple de script qui crée un abonnement pour une rubrique personnalisée. Il envoie les événements vers un webhook.|

## <a name="arm-template-tutorials"></a>Tutoriels pour les modèles ARM
|Intitulé  |Description  |
|---------|---------|
| [Modèle Resource Manager : rubrique personnalisée et point de terminaison webhook](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.eventgrid/event-grid) | Modèle Resource Manager qui crée une rubrique personnalisée et un abonnement pour celle-ci. Il envoie les événements vers un webhook. |
| [Modèle Resource Manager : rubrique personnalisée et point de terminaison Event Hubs](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.eventgrid/event-grid-event-hubs-handler)| Modèle Resource Manager qui crée un abonnement pour une rubrique personnalisée. Il envoie les événements vers Azure Event Hubs. |

> [!NOTE]
> Azure Digital Twins peut acheminer les notifications d’événements vers des rubriques personnalisées que vous créez avec Event Grid. Pour plus d’informations, consultez [Gérer les points de terminaison et les routes dans Azure Digital Twins](../digital-twins/how-to-manage-routes.md).

## <a name="next-steps"></a>Étapes suivantes
Voir les articles suivants : 

- [Rubriques sur le système](system-topics.md)
- [Domaines](event-domains.md)