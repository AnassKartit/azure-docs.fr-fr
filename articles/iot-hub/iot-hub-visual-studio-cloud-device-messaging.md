---
title: Utiliser VS Cloud Explorer pour gérer la messagerie des appareils Azure IoT Hub
description: Découvrez comment utiliser Cloud Explorer pour Visual Studio afin de superviser les messages appareil-à-cloud et d’envoyer des messages cloud-à-appareil dans Azure IoT Hub.
author: shizn
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: xshi
ms.openlocfilehash: 8461a77d06a63c2ac319323a91b5577ca4dce1cf
ms.sourcegitcommit: 590f14d35e831a2dbb803fc12ebbd3ed2046abff
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2021
ms.locfileid: "107567028"
---
# <a name="use-cloud-explorer-for-visual-studio-to-send-and-receive-messages-between-your-device-and-iot-hub"></a>Utiliser Cloud Explorer pour Visual Studio afin d’envoyer et de recevoir des messages entre votre appareil et un hub IoT

![Diagramme de bout en bout](./media/iot-hub-visual-studio-cloud-device-messaging/e-to-e-diagram.png)

Dans cet article, vous allez apprendre à utiliser Cloud Explorer pour Visual Studio afin de superviser les messages appareil-à-cloud et d’envoyer des messages cloud-à-appareil. Les messages appareil-à-cloud peuvent être des données de capteur collectées par votre appareil, puis envoyées vers votre hub IoT. Les messages cloud-à-appareil peuvent être des commandes envoyées à votre appareil par votre hub IoT. Par exemple, faire clignoter une LED qui est connectée à votre appareil.

[Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS) est une extension Visual Studio utile qui vous permet de visualiser vos ressources Azure, d’inspecter leurs propriétés et d’exécuter des actions de développeur essentielles à partir de Visual Studio. Cet article explique comment utiliser Cloud Explorer pour envoyer et recevoir des messages entre votre appareil et votre hub.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

## <a name="prerequisites"></a>Prérequis

- Un abonnement Azure actif.

- Un hub IoT Azure associé à votre abonnement

- Microsoft Visual Studio 2017 Update 9 ou version ultérieure. Cet article utilise [Visual Studio 2019](https://www.visualstudio.com/vs/).

- Le composant Cloud Explorer de Visual Studio Installer, qui est sélectionné par défaut avec la charge de travail Azure.

## <a name="update-cloud-explorer-to-latest-version"></a>Mettre à jour Cloud Explorer vers la version la plus récente

Le composant Cloud Explorer de Visual Studio Installer pour Visual Studio 2017 prend uniquement en charge la supervision des messages appareil-à-cloud et cloud-à-appareil. Pour utiliser Visual Studio 2017, téléchargez et installez la dernière version de [Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS).

## <a name="sign-in-to-access-your-hub"></a>Se connecter pour accéder à votre hub

Pour accéder à votre hub, effectuez les étapes suivantes :

1. Dans Visual Studio, sélectionnez **Afficher** > **Cloud Explorer** pour ouvrir Cloud Explorer.

1. Sélectionnez l’icône Gestion de compte pour afficher vos abonnements.

    ![Icône Gestion de compte](media/iot-hub-visual-studio-cloud-device-messaging/account-management-icon.png)

1. Si vous êtes connecté à Azure, vos comptes apparaissent. Pour vous connecter à Azure pour la première fois, choisissez **Ajouter un compte**.

1. Sélectionnez les abonnements Azure que vous souhaitez utiliser, puis choisissez **Appliquer**.

1. Développez votre abonnement, puis **IoT Hubs**.  Sous chaque hub, vous pouvez voir vos appareils correspondants.

    ![Liste des appareils](media/iot-hub-visual-studio-cloud-device-messaging/hub-device-list.png)

## <a name="monitor-device-to-cloud-messages"></a>Analyse de messages appareil vers cloud

Pour superviser les messages envoyés de votre appareil à votre hub IoT, effectuez les étapes suivantes :

1. Cliquez avec le bouton droit sur votre hub IoT ou votre appareil et sélectionnez **Démarrer la supervision des messages appareil-à-cloud**.

    ![Démarrer la supervision des messages appareil-à-cloud](media/iot-hub-visual-studio-cloud-device-messaging/start-monitoring-d2c-message-vs2019.png)

1. Les messages supervisés s’affichent sous **Sortie**.

    ![Résultat de supervision des messages appareil-à-cloud](media/iot-hub-visual-studio-cloud-device-messaging/monitor-d2c-message-result-vs2019.png)

1. Pour arrêter la supervision, cliquez avec le bouton droit sur n’importe quel appareil ou hub IoT et sélectionnez **Arrêter la supervision des messages appareil-à-cloud**.

## <a name="send-cloud-to-device-messages"></a>Envoi de messages cloud vers appareil

Pour envoyer un message de votre hub IoT vers votre appareil, effectuez les étapes suivantes :

1. Cliquez avec le bouton droit sur votre appareil et sélectionnez **Envoyer un message cloud-à-appareil**.

1. Entrez le message dans la zone de saisie.

    ![Envoyer un message cloud-à-appareil](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message-test.png)

    Les résultats s’affichent sous **Sortie**.

    ![Résultat d’envoi de message cloud-à-appareil](media/iot-hub-visual-studio-cloud-device-messaging/send-c2d-message-result-vs2019.png)

## <a name="next-steps"></a>Étapes suivantes

Vous avez appris à analyser des messages appareil vers cloud et à envoyer des messages cloud vers appareil entre votre appareil IoT et l’instance IoT Hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]