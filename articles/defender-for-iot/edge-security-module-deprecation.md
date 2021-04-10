---
title: Prise en charge et mise hors service des fonctionnalités
description: Defender pour IoT continuera à prendre en charge C, C# et Edge jusqu’au 1er mars 2022.
ms.date: 1/21/2021
ms.topic: how-to
ms.openlocfilehash: 782e2e8ab0c54e21da643ca73f647a7ea21e4223
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "104784524"
---
# <a name="feature-support-and-retirement"></a>Prise en charge et mise hors service des fonctionnalités

Cet article décrit les fonctionnalités Azure Defender pour IoT et la prise en charge de différentes fonctionnalités dans Defender pour IoT.

## <a name="defender-for-iot-c-c-and-edge-defender-iot-micro-agent-deprecation"></a>Dépréciation de Defender for IoT C, C# et du micro-agent Defender-IoT Edge

Le nouveau micro-agent remplacera les micro-agents C, C# et Defender-IoT Edge actuels.  

Le nouveau micro-agent est basé sur les connaissances et l’expérience collectées à partir des commentaires sur le développement du micro-agent Defender-IoT, ainsi que les remarques de nos clients et partenaires, avec quatre améliorations importantes : 

- **Valeur de sécurité de profondeur** : Le nouvel agent s’exécutera au niveau de l’hôte, ce qui offrira une meilleure visibilité des opérations sous-jacentes de l’appareil et une meilleure couverture de sécurité.

- **Amélioration des performances de l’appareil et réduction de l’encombrement** : Grâce à une taille de mémoire RAM et ROM réduite ainsi qu’à une faible consommation du processeur.  

- **Plug-and-play** : Le nouveau micro-agent n’a plus de dépendances au niveau du noyau, et toutes ses dépendances logicielles sont fournies dans le cadre de son package. Le micro-agent prend en charge une architecture de processeur standard.

- **Facile à déployer** : Le micro-agent prend en charge différents modèles de distribution, via le code source et en tant que package binaire. 

### <a name="timeline"></a>Durée 

Defender pour IoT continuera à prendre en charge C, C# et Edge jusqu’au 1er mars 2022. 

## <a name="micro-agent-preview-support"></a>Prise en charge de la préversion du micro-agent

Pendant la phase de préversion, le micro-agent peut faire l’objet de modifications importantes sans préavis.

## <a name="next-steps"></a>Étapes suivantes

Consultez [API de capteur et de console de gestion Defender pour IoT](references-work-with-defender-for-iot-apis.md).