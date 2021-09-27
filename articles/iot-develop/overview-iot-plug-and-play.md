---
title: Présentation d’IoT Plug-and-Play | Microsoft Docs
description: Découvrez IoT Plug-and-Play. IoT Plug-and-Play est basé sur un langage de modélisation ouvert qui permet aux appareils IoT intelligents de déclarer leurs fonctionnalités. Les appareils IoT présentent cette déclaration, appelée modèle d’appareil, lorsqu’ils se connectent aux solutions cloud. La solution cloud peut ensuite comprendre automatiquement l’appareil et commencer à interagir avec lui, tout cela sans écrire de code.
author: rido-min
ms.author: rmpablos
ms.date: 08/20/2021
ms.topic: conceptual
ms.service: iot-develop
services: iot-develop
manager: eliotgra
ms.custom:
- references_regions
- contperf-fy22q1
ms.openlocfilehash: ae5c2db75b868732bc5a6790e37111b8b1a493ef
ms.sourcegitcommit: f2d0e1e91a6c345858d3c21b387b15e3b1fa8b4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2021
ms.locfileid: "123542419"
---
# <a name="what-is-iot-plug-and-play"></a>Qu’est ce qu’IoT Plug-and-Play ?

IoT Plug-and-Play permet aux créateurs de solutions d’intégrer des appareils IoT dans leurs solutions sans configuration manuelle. Au cœur de l’IoT Plug-and-Play est un _modèle d’appareil_ qu’un appareil utilise pour publier ses fonctionnalités dans une application IoT Plug-and-Play. Ce modèle est structuré sous la forme d’un ensemble d’éléments qui définissent ce qui suit :

- _Propriétés_ qui représentent l’état en lecture seule ou en écriture d’un appareil ou d’une autre entité. Par exemple, un numéro de série d’appareil peut être une propriété en lecture seule et une température cible sur un thermostat peut être une propriété en écriture.
- _Télémétrie_ qui correspond aux données émises par un appareil, qu’il s’agisse d’un flux régulier de relevés de capteurs, d’une erreur occasionnelle ou d’un message d’information.
- _Commandes_ qui décrivent une fonction ou une opération qui peut être effectuée sur un appareil. Par exemple, une commande peut redémarrer une passerelle ou prendre une photo à l’aide d’une caméra distante.

Vous pouvez regrouper ces éléments dans des interfaces afin de les réutiliser dans différents modèles pour faciliter la collaboration et accélérer le développement.

Pour qu’IoT Plug-and-Play fonctionne avec [Azure Digital Twins](../digital-twins/overview.md), vous définissez des modèles et des interfaces à l’aide du langage [DTDL (Digital Twins Definition Language)](https://github.com/Azure/opendigitaltwins-dtdl). IoT Plug-and-Play et DTDL sont ouverts à la communauté, et Microsoft accepte la collaboration avec les clients, les partenaires et le secteur. Les deux sont basés sur des standards W3C ouverts, tels que JSON-LD et RDF, qui permettent une adoption plus facile des services et outils.

Il n’existe aucun coût supplémentaire pour l’utilisation d’IoT Plug-and-Play et de DTDL. Les tarifs standard pour [Azure IoT Hub](../iot-hub/about-iot-hub.md) et d’autres services Azure restent les mêmes.

Cet article décrit :

- Les rôles typiques associés à un projet qui utilise IoT Plug-and-Play.
- Comment utiliser les appareils IoT Plug-and-Play dans votre application.
- Comment développer une application d’appareil IoT qui prend en charge IoT Plug-and-Play.

## <a name="user-roles"></a>Rôles d’utilisateur

IoT Plug-and-Play est utile pour deux types de développeurs :

- Un _développeur de solution_ est responsable du développement d’une solution IoT à l’aide d’Azure IoT Hub et d’autres ressources Azure, et de l’identification des appareils IoT à intégrer. Pour plus d’informations, consultez [Guide du développeur de service IoT Plug-and-Play](concepts-developer-guide-service.md).
- Un _développeur d’appareil_ crée le code qui s’exécute sur un appareil connecté à votre solution. Pour plus d’informations, consultez [Guide du développeur d’appareil IoT Plug-and-Play](concepts-developer-guide-device.md).

## <a name="use-iot-plug-and-play-devices"></a>Utiliser des appareils IoT Plug-and-Play

En tant que développeur de solution, vous pouvez utiliser [IoT Central](../iot-central/core/overview-iot-central.md) ou [IoT Hub](../iot-hub/about-iot-hub.md) pour développer une solution IoT hébergée dans le cloud qui utilise des appareils IoT Plug-and-Play.

Dans IoT Central, l’interface utilisateur web vous permet de superviser les conditions des appareils, de créer des règles et de gérer des millions d’appareils et leurs données tout au long de leur cycle de vie. Les appareils IoT Plug-and-Play se connectent directement à une application IoT Central dans laquelle vous pouvez utiliser des tableaux de bord personnalisables pour superviser et contrôler vos appareils. Vous pouvez également utiliser des modèles d’appareil dans l’interface utilisateur web d’IoT Central pour créer et modifier des modèles DTDL.

IoT Hub : service cloud managé ; agit comme hub de messages pour la communication bidirectionnelle et sécurisée entre votre application IoT et vos appareils. Quand vous connectez un appareil IoT Plug-and-Play à un hub IoT, vous pouvez utiliser l’outil [Explorateur Azure IoT](../iot-fundamentals/howto-use-iot-explorer.md) pour afficher les données de télémétrie, les propriétés et les commandes définies dans le modèle DTDL.

Si des capteurs existants sont attachés à une passerelle Windows ou Linux, vous pouvez utiliser le [pont IoT Plug-and-Play](./concepts-iot-pnp-bridge.md) pour connecter ces capteurs et créer des appareils IoT Plug-and-Play sans avoir à écrire de logiciel/microprogramme d’appareil (pour les [protocoles pris en charge](./concepts-iot-pnp-bridge.md#supported-protocols-and-sensors)).

Pour plus d’informations, consultez [Architecture IoT Plug-and-Play](concepts-architecture.md).

## <a name="develop-an-iot-device-application"></a>Développer une application d’appareil IoT

En tant que développeur d’appareil, vous pouvez développer un produit matériel IoT qui prend en charge IoT Plug-and-Play. Le processus comprend trois étapes clés :

1. Définissez le modèle d’appareil. Vous créez un ensemble de fichiers JSON qui définissent les capacités de votre appareil à l’aide de [DTDL](https://github.com/Azure/opendigitaltwins-dtdl). Un modèle décrit une entité complète, telle qu’un produit physique, et définit l’ensemble des interfaces implémentées par cette entité. Les interfaces sont des contrats partagés qui identifient de façon unique les données de télémétrie, les propriétés et les commandes prises en charge par un appareil. Vous pouvez réutiliser les interfaces dans différents modèles.

1. Créez le logiciel ou le microprogramme d’appareil de manière à ce que leurs données de télémétrie, leurs propriétés et leurs commandes suivent les [conventions IoT Plug-and-Play](concepts-convention.md). Si vous connectez des capteurs existants attachés à une passerelle Windows ou Linux, le [pont IoT Plug-and-Play](./concepts-iot-pnp-bridge.md) peut simplifier cette étape.

1. L’appareil annonce l’ID de modèle dans le cadre de la connexion MQTT. Le SDK Azure IoT inclut de nouvelles constructions permettant de fournir l’ID de modèle au moment de la connexion.

> [!Important]
> Les appareils IoT Plug-and-Play doivent utiliser MQTT ou MQTT sur WebSocket. D’autres protocoles tels que AMQP ou HTTP ne sont pas valides pour implémenter les appareils IoT Plug-and-Play.

## <a name="device-certification"></a>Certification d’appareil

Le [programme de certification d’appareil IoT Plug-and-Play](../certification/program-requirements-pnp.md) vérifie qu’un appareil répond aux exigences de certification IoT Plug-and-Play. Vous pouvez ajouter un appareil certifié au [catalogue des appareils certifiés pour Azure IoT](https://aka.ms/devicecatalog).

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous disposez d’une vue d’ensemble d’IoT Plug-and-Play, l’étape suivante suggérée consiste à essayer l’un des démarrages rapides suivants :

- [Connecter un appareil à IoT Hub](./tutorial-connect-device.md)
- [Interagir avec un appareil à partir de votre solution](./tutorial-service.md)
