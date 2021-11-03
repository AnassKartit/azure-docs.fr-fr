---
title: Configuration requise pour le kit SDK Azure Kinect Sensor
description: Découvrez la configuration requise pour le kit SDK Azure Kinect Sensor sur Windows et Linux.
author: qm13
ms.author: quentinm
ms.custom: CI 115266, CSSTroubleshooting
manager: dcscontentpm
ms.prod: kinect-dk
ms.date: 03/05/2021
ms.topic: article
keywords: Azure, Kinect, configuration requise, CPU, GPU, USB, processeur, configuration, config, minimum, exigences
ms.openlocfilehash: ba6782ad0a0278f1f66266c8abbe91d33cb5bf68
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131085845"
---
# <a name="azure-kinect-sensor-sdk-system-requirements"></a>Configuration requise pour le kit SDK Azure Kinect Sensor

Ce document fournit des informations sur la configuration requise pour installer le kit SDK du capteur et déployer correctement le kit Azure Kinect DK.

## <a name="supported-operating-systems-and-architectures"></a>Systèmes d’exploitation et architectures pris en charge

- Windows 10 avril 2018 (version 1803, build d’OS 17134) x64 ou version ultérieure
- Linux Ubuntu 18.04 x64 avec pilote de GPU utilisant OpenGLv4.4 ou version ultérieure

Le kit SDK Sensor est disponible pour l’API Windows (Win32) des applications Windows C/C++ natives. Le kit SDK n’est pas disponible pour les applications UWP. Azure Kinect DK n’est pas pris en charge pour Windows 10 en mode S.

## <a name="development-environment-requirements"></a>Exigences relatives à l’environnement de développement

Pour contribuer au développement du kit SDK du capteur, accédez à [GitHub](https://github.com/Microsoft/Azure-Kinect-Sensor-SDK).

## <a name="minimum-host-pc-hardware-requirements"></a>Configuration matérielle minimale du PC hôte

La configuration matérielle requise sur le PC hôte dépend de l’application, de l’algorithme, de la fréquence d’images du capteur et de la résolution disponibles sur le PC hôte. Configuration minimale recommandée pour le kit SDK du capteur sur Windows :

- Processeur Intel&reg; CoreTM i3 de septième génération (double cœur 2,4 GHz avec GPU HD620 ou supérieur)
- 4 Go de mémoire
- Port USB3 dédié
- Prise en charge du pilote graphique pour OpenGL 4.4 ou DirectX 11.0

Les processeurs d’entrée de gamme ou plus anciens peuvent également fonctionner selon votre cas d’usage.

Les performances diffèrent également entre les systèmes d’exploitation Windows/Linux et les pilotes graphiques utilisés.

## <a name="body-tracking-host-pc-hardware-requirements"></a>Configuration matérielle du PC hôte de suivi des corps

La configuration d’un PC hôte de suivi des corps est plus stricte que la configuration d’un PC hôte général. Configuration minimale recommandée pour le kit SDK de suivi des corps sur Windows :

- Processeur Intel&reg;​​ CoreTM i5 de septième génération (quadricœur 2,4 GHz ou plus rapide)
- 4 Go de mémoire
- NVIDIA GEFORCE GTX 1050 ou équivalent
- Port USB3 dédié

La configuration minimale recommandée suppose un mode de profondeur K4A_DEPTH_MODE_NFOV_UNBINNED à 30 images par seconde pour le suivi de 5 personnes. Les processeurs ainsi que les GPU NVIDIA d’entrée de gamme ou plus anciens peuvent également fonctionner selon votre cas d’usage.

## <a name="usb3"></a>USB3

Il existe des problèmes de compatibilité connus avec les contrôleurs hôtes USB. Pour plus d’informations, consultez la [page de résolution des problèmes](troubleshooting.md#usb3-host-controller-compatibility)

## <a name="next-steps"></a>Étapes suivantes

- [Vue d’ensemble du kit Azure Kinect DK](about-azure-kinect-dk.md)

- [Configurer Azure Kinect DK](set-up-azure-kinect-dk.md)

- [Configurer le suivi des corps Azure Kinect](body-sdk-setup.md)
