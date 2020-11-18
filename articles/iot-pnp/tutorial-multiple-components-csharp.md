---
title: Tutoriel - Connecter l’exemple de code d’appareil de composant C# IoT Plug-and-Play à Azure IoT Hub | Microsoft Docs
description: Tutoriel - Créer et exécuter un exemple de code d’appareil C# IoT Plug-and-Play qui utilise plusieurs composants et se connecte à un hub IoT. Utilisez l’outil Azure IoT Explorer pour afficher les informations envoyées par l’appareil au hub.
author: ericmitt
ms.author: ericmitt
ms.date: 07/14/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: f7a1aa85c4456bdcf7ed50e42382fc440618ec7d
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93421445"
---
# <a name="tutorial-connect-an-iot-plug-and-play-multiple-component-device-application-running-on-windows-to-iot-hub-c"></a>Tutoriel : Connecter une application d’appareil multicomposant IoT Plug-and-Play s’exécutant sur Windows à IoT Hub (C#)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

Ce tutoriel vous montre comment créer un exemple d'application d'appareil IoT Plug-and-Play avec des composants, comment le connecter à votre hub IoT, et comment utiliser l'outil Explorateur Azure IoT pour voir les informations qu'il envoie au hub. L’exemple d’application est écrit en C# et est inclus dans le SDK Azure IoT Device pour C#. Un créateur de solutions peut utiliser l’outil Explorateur Azure IoT pour comprendre les fonctionnalités d’un appareil IoT Plug-and-Play sans avoir besoin d’examiner le code d’appareil.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [iot-pnp-prerequisites](../../includes/iot-pnp-prerequisites.md)]

Pour effectuer ce tutoriel sur Windows, vous devez installer les logiciels suivants sur votre environnement Windows local :

* [Visual Studio (Community, Professional ou Enterprise)](https://visualstudio.microsoft.com/downloads/).
* [Git](https://git-scm.com/download/).

### <a name="clone-the-sdk-repository-with-the-sample-code"></a>Clonez le référentiel SDK avec l’exemple de code

Si vous avez terminé [Démarrage rapide : Connecter un exemple d'application d'appareil IoT Plug-and-Play s'exécutant sous Windows à IoT Hub (C#)](quickstart-connect-device-csharp.md), vous avez déjà cloné le référentiel.

Clonez les exemples à partir du dépôt GitHub des exemples Azure IoT pour C#. Ouvrez une invite de commandes dans un dossier de votre choix. Exécutez la commande suivante pour cloner le référentiel GitHub [Exemples Microsoft Azure IoT pour .NET](https://github.com/Azure-Samples/azure-iot-samples-csharp) :

```cmd
git clone https://github.com/Azure-Samples/azure-iot-samples-csharp.git
```

## <a name="run-the-sample-device"></a>Exécuter l’exemple d’appareil

Dans ce guide de démarrage rapide, vous utilisez un exemple de régulateur thermique écrit en C# comme appareil IoT Plug-and-Play. Pour exécuter l’exemple d’appareil :

1. Ouvrez le fichier de projet *azure-iot-samples-csharp\iot-hub\Samples\device\PnpDeviceSamples\TemperatureController\TemperatureController.csproj* dans Visual Studio 2019.

1. Dans Visual Studio, accédez à **Projet > Propriétés du régulateur thermique > Déboguer**. Puis ajoutez les variables d'environnement suivantes au projet :

    | Nom | Valeur |
    | ---- | ----- |
    | IOTHUB_DEVICE_SECURITY_TYPE | DPS |
    | IOTHUB_DEVICE_DPS_ENDPOINT | global.azure-devices-provisioning.net |
    | IOTHUB_DEVICE_DPS_ID_SCOPE | Valeur que vous avez notée à la fin de la section [Configurer de votre environnement](set-up-environment.md) |
    | IOTHUB_DEVICE_DPS_DEVICE_ID | my-pnp-device |
    | IOTHUB_DEVICE_DPS_DEVICE_KEY | Valeur que vous avez notée à la fin de la section [Configurer de votre environnement](set-up-environment.md) |


1. Vous pouvez maintenant générer l’exemple dans Visual Studio et l’exécuter en mode débogage.

1. Des messages s’affichent pour indiquer que l’appareil a envoyé des informations et qu’il s’est signalé comme étant en ligne. Ces messages indiquent que l’appareil a commencé à envoyer des données de télémétrie au hub et est maintenant prêt à recevoir des commandes et des mises à jour de propriétés. Ne fermez pas cette instance de Visual Studio. Vous en aurez besoin pour confirmer que l'exemple de service fonctionne.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Utiliser l’Explorateur Azure IoT pour valider le code

Une fois l’exemple de client d’appareil démarré, utilisez l’outil Explorateur Azure IoT pour vérifier qu’il fonctionne.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>Vérifier le code

Cet exemple implémente un appareil contrôleur de température IoT Plug-and-Play. Le modèle implémenté par cet exemple utilise [plusieurs composants](concepts-components.md). Le [fichier de modèle DTDL (Digital Twins Definition Language) pour l’appareil de contrôle de température](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) définit la télémétrie, les propriétés et les commandes implémentées par l’appareil.

Le code de l’appareil se connecte à votre hub IoT en utilisant la méthode `CreateFromConnectionString` standard. L’appareil envoie l’ID de modèle du modèle DTDL qu’il implémente dans la demande de connexion. Un appareil qui envoie un ID de modèle est un appareil IoT Plug-and-Play :

```csharp
private static DeviceClient InitializeDeviceClient(string hostname, IAuthenticationMethod authenticationMethod)
{
    var options = new ClientOptions
    {
        ModelId = ModelId,
    };

    var deviceClient = DeviceClient.Create(hostname, authenticationMethod, TransportType.Mqtt, options);
    deviceClient.SetConnectionStatusChangesHandler((status, reason) =>
    {
        s_logger.LogDebug($"Connection status change registered - status={status}, reason={reason}.");
    });

    return deviceClient;
}
```

L’ID de modèle est stocké dans le code, comme indiqué dans l’extrait de code suivant :

```csharp
private const string ModelId = "dtmi:com:example:TemperatureController;1";
```

Une fois que l’appareil se connecte à votre hub IoT, le code inscrit les gestionnaires de commandes. La commande `reboot` est définie dans le composant par défaut. La commande `getMaxMinReport` est définie dans chacun des deux composants de thermostat :

```csharp
await _deviceClient.SetMethodHandlerAsync("reboot", HandleRebootCommandAsync, _deviceClient, cancellationToken);
await _deviceClient.SetMethodHandlerAsync("thermostat1*getMaxMinReport", HandleMaxMinReportCommandAsync, Thermostat1, cancellationToken);
await _deviceClient.SetMethodHandlerAsync("thermostat2*getMaxMinReport", HandleMaxMinReportCommandAsync, Thermostat2, cancellationToken);

```

Il existe des gestionnaires distincts pour les mises à jour des propriétés souhaitées sur les deux composants de thermostat :

```csharp
_desiredPropertyUpdateCallbacks.Add(Thermostat1, TargetTemperatureUpdateCallbackAsync);
_desiredPropertyUpdateCallbacks.Add(Thermostat2, TargetTemperatureUpdateCallbackAsync);

```

L’exemple de code envoie des données de télémétrie depuis chaque composant de thermostat :

```csharp
await SendTemperatureAsync(Thermostat1, cancellationToken);
await SendTemperatureAsync(Thermostat2, cancellationToken);
```

La méthode `SendTemperatureTelemetryAsync` utilise la classe `PnpHhelper` pour créer des messages pour chaque composant :

```csharp
using Message msg = PnpHelper.CreateIothubMessageUtf8(telemetryName, JsonConvert.SerializeObject(currentTemperature), componentName);
```

La classe `PnpHelper` contient d’autres exemples de méthodes que vous pouvez utiliser avec un modèle à plusieurs composants.

Utilisez l’outil Explorateur Azure IoT pour visualiser les données de télémétrie et les propriétés des deux composants de thermostat :

:::image type="content" source="media/tutorial-multiple-components-csharp/multiple-component.png" alt-text="Appareil à plusieurs composants dans l’Explorateur Azure IoT":::

Vous pouvez également utiliser l'outil Explorateur Azure IoT pour appeler des commandes dans l'un ou l'autre des deux composants de thermostat, ou dans le composant par défaut.

## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous avez découvert comment connecter un appareil IoT Plug-and-Play avec des composants à un hub IoT. Pour découvrir plus d’informations sur les modèles d’appareils IoT Plug-and-Play, consultez :

> [!div class="nextstepaction"]
> [Guide du développeur pour la modélisation d'IoT Plug-and-Play](concepts-developer-guide-device-csharp.md)
