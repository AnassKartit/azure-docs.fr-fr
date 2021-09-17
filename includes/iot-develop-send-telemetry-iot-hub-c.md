---
title: Fichier include
description: Fichier include
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 08/03/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: cf398acc35f309218d123a0c8fc60e7d6f4a3a81
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "121744353"
---
[![Parcourir le code](../articles/iot-develop/media/common/browse-code.svg)](https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/pnp)

Dans ce guide de démarrage rapide, vous allez découvrir un workflow simple de développement d’application Azure IoT. Vous utilisez Azure CLI pour créer un hub Azure IoT et un appareil. Ensuite, vous utilisez un exemple Azure IoT device SDK pour exécuter un contrôleur de température simulé, le connecter en toute sécurité au hub et envoyer des données de télémétrie.

## <a name="prerequisites"></a>Prérequis
- Si vous n’avez pas d’abonnement Azure, [créez-en un gratuitement](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.
- [Git](https://git-scm.com/downloads).
- [Azure IoT Explorer](https://github.com/Azure/azure-iot-explorer/releases) : utilitaire multiplateforme pour surveiller et gérer Azure IoT 
- Azure CLI. Vous avez le choix entre deux options pour exécuter les commandes Azure CLI dans ce guide de démarrage rapide :
    - Utilisez Azure Cloud Shell, un interpréteur de commandes interactif qui exécute des commandes CLI dans votre navigateur. Cette option est recommandée, car vous n’avez pas besoin d’installer quoi que ce soit. Si vous utilisez Cloud Shell pour la première fois, connectez-vous au [portail Azure](https://portal.azure.com). Suivez les étapes décrites dans [Démarrage rapide de Cloud Shell](../articles/cloud-shell/quickstart.md) pour **démarrer Cloud Shell** et **sélectionner l’environnement Bash**.
    - Si vous le souhaitez, exécutez Azure CLI sur votre ordinateur local. Si l’interface de ligne de commande Azure est déjà installée, exécutez `az upgrade` pour la mettre à niveau, ainsi que les extensions, vers la version actuelle. Pour installer l’interface de ligne de commande Azure, consultez [Installer l’interface de ligne de commande Azure]( /cli/azure/install-azure-cli).

Installez les composants requis restants pour votre système d’exploitation.

### <a name="linux"></a>Linux
Les étapes de ce tutoriel ont été testées à l’aide d’Ubuntu Linux 18.04.

Pour suivre ce guide de démarrage rapide sur Linux, vous devez installer les logiciels suivants sur votre environnement Linux :

Installez **GCC**, **Git**, **cmake** et les dépendances nécessaires à l’aide de la commande `apt-get` :

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

Vérifiez que la version de `cmake` est supérieure à **2.8.12** et que la version de **GCC** est supérieure à **4.4.7**.

```sh
cmake --version
gcc --version
```

### <a name="windows"></a>Windows
Pour suivre ce guide de démarrage rapide sur Windows, installez Visual Studio 2019 et ajoutez les composants requis pour le développement C et C++.

1. Pour les nouveaux utilisateurs, installez [Visual Studio (Community, Professional ou Enterprise) 2019](https://visualstudio.microsoft.com/downloads/). Téléchargez l’édition que vous souhaitez installer et démarrez le programme d’installation.
    > [!NOTE]
    > Pour les utilisateurs de Visual Studio 2019 existants, sélectionnez **Démarrer** dans Windows, tapez *Visual Studio Installer*, puis démarrez le programme d’installation.
1. Sous l’onglet **Charges de travail** du programme d’installation, sélectionnez la charge de travail **Développement Desktop en C++** .
1. Exécutez l’installation.

[!INCLUDE [iot-hub-include-create-hub-iot-explorer](iot-hub-include-create-hub-iot-explorer.md)]

## <a name="run-a-simulated-device"></a>Exécuter un appareil simulé
Dans cette section, vous allez utiliser le kit SDK C pour envoyer des messages d’un appareil simulé à votre hub IoT. Vous allez exécuter un exemple qui implémente un contrôleur de température avec deux capteurs de thermostat.

### <a name="build-the-sample"></a>Générer l’exemple
1. Ouvrez une console pour installer le kit SDK C d’appareil Azure IoT et exécuter l’exemple de code. Pour Windows, sélectionnez **Démarrer**, tapez *Invite de commandes développeur pour VS 2019*, puis ouvrez la console. Pour Linux, ouvrez Bash.

    > [!NOTE]
    > Si vous utilisez une installation locale d’Azure CLI, vous pourriez maintenant avoir deux fenêtres de console ouvertes. Veillez à entrer les commandes de cette section dans la console que vous venez d’ouvrir, et non dans celle que vous avez utilisée pour l’interface CLI.

1. Dans votre console C, clonez le kit SDK C d’appareil Azure IoT sur votre ordinateur local :
    ```console
    git clone https://github.com/Azure/azure-iot-sdk-c.git
    ```
1. Accédez au dossier racine du kit SDK et exécutez la commande suivante pour mettre à jour les dépendances :
    ```console
    cd azure-iot-sdk-c
    git submodule update --init
    ```
    Cette opération prend quelques minutes.
1. Exécutez les commandes suivantes pour générer le kit SDK et les exemples :

    ```console
    cmake -Bcmake -Duse_prov_client=ON -Dhsm_type_symm_key=ON -Drun_e2e_tests=OFF
    cmake --build cmake
    ```
1. Définissez les variables d’environnement suivantes pour permettre à votre appareil simulé de se connecter à Azure IoT.
    * Définissez une variable d’environnement appelée `IOTHUB_DEVICE_CONNECTION_STRING`. Pour la valeur de la variable, utilisez la chaîne de connexion de l’appareil que vous avez enregistrée dans la section précédente.
    * Définissez une variable d’environnement appelée `IOTHUB_DEVICE_SECURITY_TYPE`. Pour la variable, utilisez la valeur de chaîne littérale `connectionString`.

    **CMD**

    ```console
    set IOTHUB_DEVICE_CONNECTION_STRING=<your connection string here>
    set IOTHUB_DEVICE_SECURITY_TYPE=connectionString
    ```
    > [!NOTE]
    > Pour Windows CMD, il n’y a pas de guillemets avant et après les valeurs de chaîne pour chaque variable.

    **Bash**

    ```bash
    export IOTHUB_DEVICE_CONNECTION_STRING="<your connection string here>"
    export IOTHUB_DEVICE_SECURITY_TYPE="connectionString"
    ```

### <a name="run-the-code"></a>Exécuter le code
1. Exécutez l’exemple de code à l’aide de la commande appropriée pour votre console.

    **CMD**
    ```console
    cmake\iothub_client\samples\pnp\pnp_temperature_controller\Debug\pnp_temperature_controller.exe
    ```

    **Bash**
    ```bash
    cmake/iothub_client/samples/pnp/pnp_temperature_controller/Debug/pnp_temperature_controller
    ```
    > [!NOTE]
    > Cet exemple de code utilise Azure IoT Plug-and-Play, qui vous permet d’intégrer des appareils intelligents dans vos solutions sans aucune configuration manuelle.  Par défaut, la plupart des exemples de cette documentation utilisent IoT Plug-and-Play. Pour en savoir plus sur les avantages et les cas d’utilisation d’IoT PnP, consultez [Qu’est ce qu’IoT Plug-and-Play ?](../articles/iot-develop/overview-iot-plug-and-play.md).

L’exemple se connecte en toute sécurité à votre IoT Hub en tant qu’appareil que vous avez inscrit, et commence à envoyer des messages de télémétrie. L’exemple de sortie s’affiche dans votre console. 

## <a name="view-telemetry"></a>Afficher les données de télémétrie

Vous pouvez afficher la télémétrie des appareils avec l’Explorateur IoT. Si vous le souhaitez, vous pouvez afficher les données de télémétrie à l’aide d’Azure CLI.

Pour afficher la télémétrie dans Azure IoT Explorer :

1. À partir de votre hub IoT dans IoT Explorer, sélectionnez **Voir les appareils dans ce hub**, puis sélectionnez votre appareil dans la liste. 
1. Dans le menu de gauche pour votre appareil, sélectionnez **Télémétrie**.
1. Confirmez qu’**Utiliser un Event Hub intégré** est défini sur *Oui*, puis sélectionnez **Démarrer**.
1. Affichez la télémétrie à mesure que l’appareil envoie des messages au cloud.

    :::image type="content" source="media/iot-develop-send-telemetry-iot-hub-c/iot-explorer-device-telemetry.png" alt-text="Capture d’écran de télémétrie d’appareil dans IoT Explorer":::

1. Sélectionnez **Arrêter** pour cesser de recevoir des événements.

Pour lire les données de télémétrie envoyées par les composants d’appareils individuels, vous pouvez utiliser les fonctionnalités Plug-and-Play dans l’Explorateur IoT. Par exemple, le contrôleur de température dans ce démarrage rapide a deux thermostats : thermostat1 et thermostat2. Pour afficher la température signalée par thermostat1 : 

1. Sur votre appareil dans l’Explorateur IoT, sélectionnez **Composants IoT Plug-and-Play** dans le menu de gauche. Ensuite, sélectionnez **thermostat1** dans la liste des composants.

1. Sur le volet du composant **thermostat1**, sélectionnez **Télémétrie** dans le menu supérieur.

1. Sur le volet **Télémétrie**, suivez les mêmes étapes que précédemment. Vérifiez qu’**Utiliser un Event Hub intégré** est défini sur *Oui*, puis sélectionnez **Démarrer**.

Pour afficher la télémétrie de l’appareil avec Azure CLI :

1. Dans votre application CLI, exécutez la commande [az iot hub monitor-events](/cli/azure/iot/hub#az_iot_hub_monitor_events) pour surveiller les événements envoyés par l’appareil simulé à votre hub IoT. Utilisez les noms que vous avez créés précédemment dans Azure IoT pour votre appareil et le hub IoT.

    ```azurecli
    az iot hub monitor-events --output table --device-id mydevice --hub-name {YourIoTHubName}
    ```

1. Visualisez les détails de la connexion et la sortie de la télémétrie dans la console.

    ```output
    Starting event monitor, filtering on device: mydevice, use ctrl-c to stop...
    event:
      component: ''
      interface: dtmi:com:example:TemperatureController;1
      module: ''
      origin: mydevice
      payload: '{"workingSet":1251}'
    
    event:
      component: thermostat1
      interface: dtmi:com:example:TemperatureController;1
      module: ''
      origin: mydevice
      payload: '{"temperature":22.00}'
    ```

