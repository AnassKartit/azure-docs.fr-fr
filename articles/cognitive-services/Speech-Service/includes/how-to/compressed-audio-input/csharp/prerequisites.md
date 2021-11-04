---
author: eric-urban
ms.service: cognitive-services
ms.topic: include
ms.date: 03/09/2020
ms.author: eur
ms.openlocfilehash: 3df7dc7e5eeb71dc046ea860927e98bb72b45231
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/03/2021
ms.locfileid: "131520493"
---
La gestion d'un contenu audio compressé est implémentée à l’aide de [GStreamer](https://gstreamer.freedesktop.org). Pour une raison liée à la gestion des licences, les fichiers binaires GStreamer ne sont pas compilés et liés avec le Kit de développement logiciel (SDK) Speech. Les développeurs doivent installer plusieurs dépendances et plug-ins. Voir [Installation sur Windows](https://gstreamer.freedesktop.org/documentation/installing/on-windows.html?gi-language=c) ou [Installation sur Linux](https://gstreamer.freedesktop.org/documentation/installing/on-linux.html?gi-language=c). Les fichiers binaires GStreamer doivent se trouver sous le chemin d’accès système afin que le Kit de développement logiciel (SDK) Speech puisse les charger lors de l’exécution du runtime. Par exemple, sur Windows, si le Kit de développement logiciel (SDK) Speech est en mesure de trouver `libgstreamer-1.0-0.dll` ou `gstreamer-1.0-0.dll` (pour la version la plus récente de GStreamer) lors de l’exécution, cela signifie que les fichiers binaires GStreamer se trouvent sous le chemin d’accès système.

