---
author: dominicbetts
ms.author: dobett
ms.service: iot-develop
ms.topic: include
ms.date: 11/20/2020
ms.openlocfilehash: 15aeb7ca8c98b5e32ab662e4083cc842d19d5235
ms.sourcegitcommit: 557ed4e74f0629b6d2a543e1228f65a3e01bf3ac
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2021
ms.locfileid: "129516741"
---
Ce guide de démarrage rapide vous montre comment créer un exemple d’application d’appareil IoT Plug-and-Play, comment le connecter à votre hub IoT et comment utiliser l’outil Explorateur Azure IoT pour afficher les données de télémétrie qu’il envoie. L’exemple d’application est écrit en Java et est inclus dans l’Azure IoT device SDK pour Java. Un créateur de solutions peut utiliser l’outil Explorateur Azure IoT pour comprendre les fonctionnalités d’un appareil IoT Plug-and-Play sans avoir besoin d’examiner du code d’appareil.

[![Parcourir le code](../articles/iot-central/core/media/common/browse-code.svg)](https://github.com/Azure/azure-iot-sdk-java/tree/main/device/iot-device-samples/pnp-device-sample)

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [iot-pnp-prerequisites](iot-pnp-prerequisites.md)]

Pour suivre ce démarrage rapide sur Windows, installez les logiciels suivants sur votre environnement Windows local :

* Java SE Development Kit 8. Dans [Prise en charge à long terme de Java pour Azure et Azure Stack](/java/azure/jdk/), sous **Prise en charge à long terme**, sélectionnez **Java 8**.
* [Apache Maven 3](https://maven.apache.org/download.cgi).

## <a name="download-the-code"></a>Téléchargement du code

Dans ce démarrage rapide, vous préparez un environnement de développement pour cloner et générer le Kit de développement logiciel (SDK) Java d’appareil Azure IoT Hub.

Ouvrez une invite de commandes dans le répertoire de votre choix. Exécutez la commande suivante pour cloner le dépôt GitHub de [kits de développement logiciel (SDK) et de bibliothèques Java Azure IoT](https://github.com/Azure/azure-iot-sdk-java) dans cet emplacement :

```cmd
git clone https://github.com/Azure/azure-iot-sdk-java.git
```

## <a name="build-the-code"></a>Générer le code

Sous Windows, accédez au dossier racine du dépôt du Kit de développement logiciel (SDK) Java cloné.

Exécutez la commande suivante pour générer l’exemple d’application :

```cmd
mvn install -T 2C -DskipTests
```

## <a name="run-the-device-sample"></a>Exécuter l’exemple d’appareil

[!INCLUDE [iot-pnp-environment](iot-pnp-environment.md)]

Pour en savoir plus sur l’exemple de configuration, consultez l’[exemple de fichier Lisez-moi](https://github.com/Azure/azure-iot-sdk-java/blob/main/device/iot-device-samples/pnp-device-sample/readme.md).

Accédez au dossier *\device\iot-device-samples\pnp-device-sample\thermostat-device-sample*.

Pour exécuter l’exemple d’application, utilisez la commande suivante :

```cmd
mvn exec:java -Dexec.mainClass="samples.com.microsoft.azure.sdk.iot.device.Thermostat"
```

L’appareil est désormais prêt à recevoir des commandes et des mises à jour de propriétés, et a commencé à envoyer des données de télémétrie au hub. Laissez l’exemple s’exécuter pendant que vous effectuez les étapes suivantes.

## <a name="use-azure-iot-explorer-to-validate-the-code"></a>Utiliser l’Explorateur Azure IoT pour valider le code

Une fois l’exemple de client d’appareil démarré, utilisez l’outil Explorateur Azure IoT pour vérifier qu’il fonctionne.

[!INCLUDE [iot-pnp-iot-explorer.md](iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>Vérifier le code

Cet exemple implémente un appareil à thermostat IoT Plug-and-Play simple. Le modèle implémenté par cet exemple n’utilise pas de [composants](../articles/iot-develop/concepts-modeling-guide.md) IoT Plug-and-Play. Le [fichier de modèle DTDL pour le thermostat](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/Thermostat.json) définit la télémétrie, les propriétés et les commandes que l’appareil implémente.

Le code d’appareil utilise la classe `DeviceClient` standard pour se connecter à votre hub IoT. L’appareil envoie l’ID de modèle du modèle DTDL qu’il implémente dans la demande de connexion. Un appareil qui envoie un ID de modèle est un appareil IoT Plug-and-Play :

```java
private static void initializeDeviceClient() throws URISyntaxException, IOException {
    ClientOptions options = new ClientOptions();
    options.setModelId(MODEL_ID);
    deviceClient = new DeviceClient(deviceConnectionString, protocol, options);

    deviceClient.registerConnectionStatusChangeCallback((status, statusChangeReason, throwable, callbackContext) -> {
        log.debug("Connection status change registered: status={}, reason={}", status, statusChangeReason);

        if (throwable != null) {
            log.debug("The connection status change was caused by the following Throwable: {}", throwable.getMessage());
            throwable.printStackTrace();
        }
    }, deviceClient);

    deviceClient.open();
}
```

L’ID de modèle est stocké dans le code, comme indiqué dans l’extrait de code suivant :

```java
private static final String MODEL_ID = "dtmi:com:example:Thermostat;1";
```

Le code qui met à jour les propriétés, gère les commandes et envoie les données de télémétrie est identique au code d’un appareil qui n’utilise pas les conventions IoT Plug-and-Play.

L’exemple utilise une bibliothèque JSON pour analyser les objets JSON dans les charges utiles envoyées à partir de votre hub IoT :

```java
import com.google.gson.Gson;

...

Date since = new Gson().fromJson(jsonRequest, Date.class);
```
