---
title: Entrée et sortie de données
titleSuffix: Azure Digital Twins
description: Découvrez les exigences d’entrée et de sortie pour l’intégration d’Azure Digital Twins avec d’autres services.
author: baanders
ms.author: baanders
ms.date: 6/1/2021
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 0855663fb60f6e8ae420762cdd23ce237de56c04
ms.sourcegitcommit: 2da83b54b4adce2f9aeeed9f485bb3dbec6b8023
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2021
ms.locfileid: "122770580"
---
# <a name="data-ingress-and-egress-for-azure-digital-twins"></a>Entrée et sortie de données pour Azure Digital Twins

Azure Digital Twins est généralement utilisé avec d’autres services pour créer des solutions flexibles et connectées qui utilisent vos données de diverses manières.

À l’aide d’[itinéraires d’événements](concepts-route-events.md), Azure Digital Twins peut recevoir des données de services en amont, comme [IoT Hub](../iot-hub/about-iot-hub.md) ou [Logic Apps](../logic-apps/logic-apps-overview.md), qui sont utilisés pour fournir de la télémétrie et des notifications. 

Azure Digital Twins peut également acheminer des données vers des services en aval, comme [Azure Maps](../azure-maps/about-azure-maps.md) et [Time Series Insights](../time-series-insights/overview-what-is-tsi.md), à des fins de stockage, d’intégration de flux de travail, d’analytique et autres. 

## <a name="data-ingress"></a>Entrée de données

Azure Digital Twins peut être piloté par des données et des événements issus d’un service quel qu’il soit ([IoT Hub](../iot-hub/about-iot-hub.md), [Logic Apps](../logic-apps/logic-apps-overview.md)), votre propre service personnalisé, etc. Ce type de flux de données vous permet de collecter des données de télémétrie à partir d’appareils physiques dans votre environnement, puis de les traiter à l’aide du graphique Azure Digital Twins dans le cloud.

Au lieu d’un IoT Hub intégré en arrière-plan, Azure Digital Twins vous permet d’utiliser vos propres IoT Hub pour les utiliser avec le service. Vous pouvez utiliser un IoT Hub existant actuellement en production ou en déployer un nouveau à cet effet. Avec cette fonctionnalité, vous bénéficiez d’un accès complet à toutes les fonctionnalités de gestion d’appareils d’IoT Hub.

Pour ingérer des données issues d’une source dans Azure Digital Twins, utilisez une [fonction Azure](../azure-functions/functions-overview.md). Pour plus d’informations sur ce modèle, consultez [Ingérer la télémétrie depuis IoT Hub](how-to-ingest-iot-hub-data.md), ou essayez-le vous-même dans Azure Digital Twins [Connecter une solution de bout en bout](tutorial-end-to-end.md). 

Vous pouvez également découvrir comment connecter Azure Digital Twins à un déclencheur Logic Apps dans [Intégrer à Logic Apps](how-to-integrate-logic-apps.md).

## <a name="data-egress-services"></a>Services de sortie des données

Azure Digital Twins peut envoyer des données à des **points de terminaison** connectés. Les points de terminaison pris en charge peuvent être :
* [Concentrateur d’événements](../event-hubs/event-hubs-about.md)
* [Event Grid](../event-grid/overview.md)
* [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md)

Les points de terminaison sont attachés au service Azure Digital Twins à l’aide d’API de gestion ou du portail Azure. Apprenez-en davantage sur la façon d’attacher un point de terminaison à Azure Digital Twins dans [Gérer les points de terminaison et les itinéraires](how-to-manage-routes.md).

Il existe de nombreux autres services vers lesquels vous pouvez finalement diriger vos données, tels que [Stockage Azure](../storage/common/storage-introduction.md), [Azure Maps](../azure-maps/about-azure-maps.md), [Azure Data Explorer](/azure/data-explorer/data-explorer-overview) ou [Time Series Insights](../time-series-insights/overview-what-is-tsi.md). Pour envoyer vos données à de tels services, attachez le service de destination à un point de terminaison.

Par exemple, si vous utilisez également Azure Maps et souhaitez mettre en corrélation l’emplacement avec votre [graphique de jumeau](concepts-twins-graph.md) Azure Digital Twins, vous pouvez utiliser Azure Functions avec Event Grid pour établir la communication entre tous les services au sein de votre déploiement. Pour plus d’informations sur l’intégration d’Azure Maps, consultez [Utiliser Azure Digital Twins pour mettre à jour une carte d’intérieur Azure Maps](how-to-integrate-maps.md)

Vous pouvez également apprendre à acheminer des données de la même façon vers Time Series Insights dans [Intégrer à Time Series Insights](how-to-integrate-time-series-insights.md).

## <a name="next-steps"></a>Étapes suivantes

Apprenez-en davantage sur les points de terminaison et le routage des événements vers des services externes :
* [Routage des événements Azure Digital Twins](concepts-route-events.md)

Découvrez comment configurer Azure Digital Twins pour ingérer des données à partir d’IoT Hub :
* [Ingérer la télémétrie depuis IoT Hub](how-to-ingest-iot-hub-data.md)