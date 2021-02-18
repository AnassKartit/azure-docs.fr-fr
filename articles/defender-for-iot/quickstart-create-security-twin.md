---
title: Créer un jumeau de module de sécurité
description: Dans ce guide de démarrage rapide, vous allez découvrir comment créer un jumeau de module Defender pour IoT en vue d’une utilisation avec Azure Defender pour IoT.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/21/2021
ms.author: shhazam
ms.openlocfilehash: 68d137caeed91e9dea2dbf3883929d85c25382aa
ms.sourcegitcommit: 27d616319a4f57eb8188d1b9d9d793a14baadbc3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2021
ms.locfileid: "100523258"
---
# <a name="create-an-azureiotsecurity-module-twin"></a>Créer un jumeau de module azureiotsecurity

Ce guide de démarrage rapide explique comment créer des jumeaux de module _azureiotsecurity_ individuels destinés à de nouveaux appareils, ou créer par lot des jumeaux de module pour tous les appareils d’un hub IoT.

## <a name="understanding-azureiotsecurity-module-twins"></a>Présentation des jumeaux de module azureiotsecurity

Pour les solutions IoT intégrées à Azure, les jumeaux d’appareil jouent un rôle essentiel dans la gestion d’appareils et l’automatisation des processus.

Defender pour IoT offre une intégration complète à votre plateforme de gestion d’appareils IoT existante, ce qui vous permet de gérer l’état de sécurité de vos appareils et de tirer parti des fonctionnalités de contrôle d’appareils existantes.
L’intégration de Defender pour IoT est obtenue par l’utilisation du mécanisme de jumelage IoT Hub.

Consultez [Jumeaux de module IoT Hub](../iot-hub/iot-hub-devguide-module-twins.md) pour en savoir plus sur le concept général des jumeaux de module dans Azure IoT Hub.

Defender pour IoT utilise le mécanisme de jumelage de module et gère un jumeau de module de sécurité nommé _azureiotsecurity_ pour chacun de vos appareils.

Le jumeau de module de sécurité contient toutes les informations relatives à la sécurité de l’appareil pour chacun de vos appareils.

Afin de tirer pleinement parti des fonctionnalités de Defender pour IoT, vous devez créer, configurer et utiliser ce jumeau de module de sécurité pour chaque appareil dans le service.

## <a name="create-azureiotsecurity-module-twin"></a>Créer un jumeau de module azureiotsecurity

Vous pouvez créer des jumeaux de module _azureiotsecurity_ de deux manières :

1. [Script de commandes par lot pour la création de module](https://aka.ms/iot-security-github-create-module) : crée automatiquement le jumeau de module pour les nouveaux appareils ou pour les appareils sans jumeau de module en utilisant la configuration par défaut.
1. Modifiez manuellement chaque jumeau de module individuellement avec des configurations spécifiques pour chaque appareil.

>[!NOTE]
> L’utilisation de la méthode de traitement par lot ne remplace pas les jumeaux de module azureiotsecurity existants. L’utilisation de la méthode de traitement par lot crée UNIQUEMENT des jumeaux de module pour les appareils qui ne disposent pas encore d’un jumeau de module de sécurité.

Consultez [Configuration des agents](how-to-agent-configuration.md) pour savoir comment modifier ou changer la configuration d’un jumeau de module existant.

Pour créer manuellement un jumeau de module _azureiotsecurity_ destiné à un appareil :

1. Dans votre IoT Hub, recherchez et sélectionnez l’appareil pour lequel vous souhaitez créer un jumeau de module de sécurité.

1. Sélectionnez votre appareil, puis **Ajouter l’identité de module**.

1. Dans le champ **Nom de l’identité de module**, entrez **azureiotsecurity**.

1. Sélectionnez **Enregistrer**.

## <a name="verify-creation-of-a-module-twin"></a>Vérifier la création d’un jumeau de module

Pour vérifier l’existence d’un jumeau de module de sécurité pour un appareil particulier :

1. Dans votre hub IoT, sélectionnez **Appareils IoT** dans le menu **Explorateurs**.

1. Entrez l’ID de l’appareil, ou sélectionnez une option dans le **champ d’interrogation des appareils**, puis sélectionnez **Interroger les appareils**.

    :::image type="content" source="./media/quickstart/verify-security-module-twin.png" alt-text="Interroger les appareils":::

1. Sélectionnez l’appareil ou double-cliquez dessus pour ouvrir la page Détails de l’appareil.

1. Sélectionnez le menu **Identités de module** et vérifiez l’existence du module **azureiotsecurity** dans la liste des identités de module associées à l’appareil.

    :::image type="content" source="./media/quickstart/verify-security-module-twin-3.png" alt-text="Modules associés à un appareil":::

Pour en savoir plus sur la personnalisation des propriétés des jumeaux de module Defender pour IoT, consultez [Configuration de l’agent](how-to-agent-configuration.md).

## <a name="next-steps"></a>Étapes suivantes

Passez à l’article suivant pour savoir comment examiner des recommandations de sécurité...

> [!div class="nextstepaction"]
> [Investiguer les recommandations de sécurité](quickstart-investigate-security-recommendations.md)
