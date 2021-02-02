---
title: Qu’est-ce qu’Azure IoT Edge pour Linux sur Windows | Microsoft Docs
description: Vue d’ensemble de la manière d’exécuter des modules IoT Edge Linux sur des appareils Windows 10
author: kgremban
manager: philmea
ms.reviewer: twarwick
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 01/20/2021
ms.author: kgremban
monikerRange: =iotedge-2018-06
ms.openlocfilehash: ebe0ac1151f3a1f43072f2832e2f433182ccc82d
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98633565"
---
# <a name="what-is-azure-iot-edge-for-linux-on-windows-preview"></a>Qu’est-ce qu’Azure IoT Edge pour Linux sur Windows (préversion)

Azure IoT Edge pour Linux sur Windows vous permet d’exécuter des charges de travail Linux en conteneur avec des applications Windows dans des déploiements IoT Windows. Les entreprises qui s’appuient sur l’IoT Windows pour alimenter leurs appareils périphériques peuvent désormais tirer parti des solutions d’analytique natives Cloud créées dans Linux.

IoT Edge pour Linux sur Windows fonctionne en exécutant une machine virtuelle Linux sur un appareil Windows. La machine virtuelle Linux est préinstallée avec le runtime IoT Edge. Tout module IoT Edge déployé sur l’appareil s’exécute à l’intérieur de la machine virtuelle. Pendant ce temps, les applications Windows s’exécutant sur l’appareil hôte Windows peuvent communiquer avec les modules s’exécutant sur la machine virtuelle Linux.

>[!NOTE]
>Nous vous invitons à participer à notre [enquête produit](https://aka.ms/AzEFLOW-Registration) pour nous aider à améliorer Azure IoT Edge pour Linux sur Windows en fonction de votre expérience et de vos objectifs en lien avec l’IoT Edge. Vous pouvez également utiliser cette enquête pour vous inscrire à de futures annonces concernant Azure IoT Edge pour Linux sur Windows.

## <a name="components"></a>Components

IoT Edge pour Linux sur Windows utilise les composants suivants pour permettre que des charges de travail Linux et Windows s’exécutent les unes à côté des autres et communiquent en toute transparence :

* **Machine virtuelle Linux exécutant Azure IoT Edge** : une machine virtuelle Linux basée sur le système d’exploitation [Mariner](https://github.com/microsoft/CBL-Mariner) de Microsoft est créée avec le runtime IoT Edge et validée en tant qu’environnement compatible de niveau 1 pour des charges de travail IoT Edge.

* **Windows Admin Center** : une extension IoT Edge pour Windows Admin Center facilite l’installation, la configuration et les diagnostics de IoT Edge sur la machine virtuelle Linux. Windows Admin Center peut déployer IoT Edge pour Linux sur Windows sur l’appareil local, ou peut se connecter à des appareils cibles et les gérer à distance.

* **Microsoft Update** : l’intégration avec Microsoft Update permet de tenir à jour les composants Windows Runtime, la machine virtuelle Linux Mariner et le service IoT Edge.

![Windows et la machine virtuelle Linux s’exécutent en parallèle, tandis que Windows Admin Center contrôle les deux composants](./media/iot-edge-for-linux-on-windows/architecture-and-communication.png)

Une communication bidirectionnelle entre le processus Windows et la machine virtuelle Linux signifie que les processus Windows peuvent fournir des interfaces utilisateur ou des proxys matériels pour des charges de travail exécutées dans les conteneurs Linux.

## <a name="samples"></a>Exemples

IoT Edge pour Linux sur Windows met l’accent sur l’interopérabilité entre les composants Linux et Windows.

Pour obtenir des exemples qui illustrent la communication entre les applications Windows et les modules IoT Edge, consultez [Exemples d’IoT Windows 10](https://github.com/microsoft/Windows-IoT-Samples).

## <a name="public-preview"></a>Préversion publique

IoT Edge pour Linux sur Windows est actuellement en [préversion publique](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Les processus d’installation et de gestion peuvent différer de ceux des fonctionnalités généralement disponibles.

Actuellement, IoT Edge pour Linux sur Windows utilise la préversion de Windows Insider de Windows Admin Center. Pour plus d’informations sur le programme Windows Insider et pour vous inscrire, consultez [Qu’est-ce que le programme Windows Insider ?](https://insider.windows.com/about-windows-insider-program).

## <a name="support"></a>Support

Utilisez les canaux de support et de commentaires d’IoT Edge pour obtenir de l’aide concernant IoT Edge pour Linux sur Windows.

**Signalement des bogues** : vous pouvez signaler des bogues sur la [page des problèmes ](https://github.com/azure/iotedge/issues) du projet open source IoT Edge. 

**Équipe de support technique Microsoft** : les utilisateurs qui ont un [plan de support](https://azure.microsoft.com/support/plans/) peuvent solliciter l’équipe de support technique Microsoft en créant un ticket de support directement à partir du [portail Azure](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Demandes de fonctionnalités** : le produit Azure IoT Edge effectue le suivi des demandes de fonctionnalités par le biais de sa [page User Voice](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations et un exemple en action, regardez [IoT Edge pour Linux sur Windows 10 IoT Enterprise](https://aka.ms/EFLOWPPC9).

Pour configurer un appareil avec IoT Edge pour Linux sur Windows, Suivez les étapes décrites dans [installer et approvisionner Azure IoT Edge pour Linux sur un appareil Windows](how-to-install-iot-edge-on-windows.md).
