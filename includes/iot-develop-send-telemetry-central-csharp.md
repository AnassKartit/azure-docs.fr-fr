---
title: Fichier include
description: Fichier include
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 04/28/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: b4ff1f14ac628f28e67f4b619983760d06f60d8b
ms.sourcegitcommit: 557ed4e74f0629b6d2a543e1228f65a3e01bf3ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2021
ms.locfileid: "129516633"
---
[![Parcourir le code](../articles/iot-develop/media/common/browse-code.svg)](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/main/iot-hub/Samples/device/PnpDeviceSamples)

Dans ce guide de démarrage rapide, vous allez découvrir un workflow simple de développement d’application Azure IoT. Tout d’abord, vous créez une application Azure IoT Central pour héberger des appareils. Ensuite, vous utilisez un exemple Azure IoT device SDK pour exécuter un contrôleur de température simulé, le connecter en toute sécurité à IoT Central et envoyer la télémétrie.

## <a name="prerequisites"></a>Prérequis
- [Visual Studio (Community, Professional ou Enterprise) 2019](https://visualstudio.microsoft.com/downloads/).
- Une copie locale du dépôt GitHub [Microsoft Azure IoT Samples for C# (.NET)](https://github.com/Azure-Samples/azure-iot-samples-csharp). Téléchargez une copie du dépôt et extrayez-la : [Télécharger le fichier .zip](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/main.zip).

[!INCLUDE [iot-develop-create-central-app-with-device](iot-develop-create-central-app-with-device.md)]

## <a name="run-a-simulated-device"></a>Exécuter un appareil simulé
Dans cette section, vous allez configurer votre environnement local et exécuter un exemple qui crée un contrôleur de température simulé.

Pour exécuter l’exemple d’application dans Visual Studio :

1. Dans le dossier où vous avez décompressé les exemples Azure IoT pour C#, ouvrez le fichier de solution *azure-iot-samples-csharp-main\iot-hub\Samples\device\IoTHubDeviceSamples.sln"* dans Visual Studio. 

1. Dans **Explorateur de solutions**, sélectionnez le fichier de projet **PnpDeviceSamples > TemperatureController**, cliquez dessus avec le bouton droit et sélectionnez **Définir comme projet de démarrage**.

1. Cliquez avec le bouton droit sur le projet **TemperatureController**, sélectionnez **Propriétés**, sélectionnez l’onglet **Déboguer**, puis ajoutez les variables d’environnement suivantes au projet :

    | Nom | Valeur |
    | ---- | ----- |
    | IOTHUB_DEVICE_SECURITY_TYPE | DPS |
    | IOTHUB_DEVICE_DPS_ENDPOINT | global.azure-devices-provisioning.net |
    | IOTHUB_DEVICE_DPS_ID_SCOPE | Valeur d’étendue d’ID que vous avez notée précédemment. |
    | IOTHUB_DEVICE_DPS_DEVICE_ID | sample-device-01 |
    | IOTHUB_DEVICE_DPS_DEVICE_KEY | Valeur de clé d’appareil générée que vous avez notée précédemment. |

1. Enregistrez le fichier de projet **TemperatureController** mis à jour.

1. Appuyez sur CTRL+F5 pour exécuter l'exemple.

    Une fois que votre appareil simulé se connecte à votre application IoT Central, il commence à envoyer des données de télémétrie. Les détails de connexion et la sortie des données de télémétrie apparaissent dans la console : 
    
    ```output
        [05/04/2021 11:53:50]info: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Press Control+C to quit the sample.
        [05/04/2021 11:53:50]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Set up the device client.
        [05/04/2021 11:53:50]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Initializing via DPS
        [05/04/2021 11:53:56]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Set handler for 'reboot' command.
        [05/04/2021 11:53:57]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Connection status change registered - status=Connected, reason=Connection_Ok.
        [05/04/2021 11:53:57]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Set handler for "getMaxMinReport" command.
        [05/04/2021 11:53:57]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Set handler to receive 'targetTemperature' updates.
        [05/04/2021 11:53:57]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Property: Update - component = 'deviceInformation', properties update is complete.
        [05/04/2021 11:53:58]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Property: Update - { "serialNumber": "SR-123456" } is complete.
        [05/04/2021 11:53:58]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Telemetry: Sent - component="thermostat1", { "temperature": 44.9 } in °C.
        [05/04/2021 11:53:58]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Property: Update - component="thermostat1", { "maxTempSinceLastReboot": 44.9 } in °C is complete.
        [05/04/2021 11:53:58]dbug: Microsoft.Azure.Devices.Client.Samples.TemperatureControllerSample[0]
              Telemetry: Sent - component="thermostat2", { "temperature": 40.8 } in °C.
    ```