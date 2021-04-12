---
title: 'Démarrage rapide : Configurer et activer le micro-agent Defender-IoT pour Azure RTOS'
description: Découvrez comment intégrer et activer le micro-agent Defender-IoT pour le service Azure RTOS dans votre hub Azure IoT.
services: defender-for-iot
ms.topic: quickstart
ms.date: 01/24/2021
ms.openlocfilehash: db0fc0be3cd10bc57a46cb5a056ca5e057ae255a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104781158"
---
# <a name="quickstart-defender-iot-micro-agent-for-azure-rtos-preview"></a>Démarrage rapide : Micro-agent Defender IoT pour Azure RTOS (préversion)

Cet article décrit les prérequis avant de commencer et explique comment activer le micro-agent Defender IoT pour le service Azure RTOS sur un hub IoT. Si vous n’avez pas de hub IoT, pour commencer, voir [Créer un hub IoT à l’aide du portail Azure](../iot-hub/iot-hub-create-through-portal.md).

## <a name="prerequisites"></a>Prérequis 

### <a name="supported-devices"></a>Appareils pris en charge

- Kit de découverte ST STM32F746G
- NXP i.MX RT1060 EVK
- Puce SAM E54 Xplained Pro EVK

Téléchargez, compilez et exécutez un des fichiers .zip pour le tableau et l’outil spécifiques (IAR, IDE semi ou PC) de votre choix à partir du [micro-agent Defender IoT pour la ressource GitHub Azure RTOS](https://github.com/azure-rtos/azure-iot-preview/releases).

### <a name="azure-resources"></a>Ressources Azure

La phase suivante de la mise en route consiste à préparer vos ressources Azure. Vous avez besoin d’un hub IoT et nous suggérons un espace de travail Log Analytics. Pour IoT Hub, vous aurez besoin d’une chaîne de connexion IoT Hub pour vous connecter à votre appareil. 
  
### <a name="iot-hub-connection"></a>Connexion au hub IoT

Une connexion au hub IoT est nécessaire pour commencer. 

1. Ouvrez votre **hub IoT** dans le portail Azure.

1. Accédez à **Appareils IoT**.

1. Sélectionnez **Create** (Créer).

1. Copiez la chaîne de connexion IoT dans le [fichier de configuration](how-to-azure-rtos-security-module.md).

Les informations d’identification de connexion sont extraites des paramètres **HOST_NAME**, **DEVICE_ID** et **DEVICE_SYMMETRIC_KEY** de la configuration de l’application utilisateur.

Le micro-agent Defender-IoT pour Azure RTOS utilise des connexions middleware Azure IoT basées sur le protocole **MQTT**.

## <a name="next-steps"></a>Étapes suivantes

Passez à l’article suivant pour terminer la configuration et la personnalisation de votre solution.

> [!div class="nextstepaction"]
> [Configurer et personnaliser le micro-agent Defender pour IoT pour Azure RTOS (préversion)](how-to-azure-rtos-security-module.md)
