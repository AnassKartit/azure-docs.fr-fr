---
title: Configurer des encodeurs locaux quand Azure Media Services est utilisé pour créer des flux multidébits | Microsoft Docs
description: Cette rubrique répertorie les encodeurs en direct locaux que vous pouvez utiliser pour capturer vos événements en direct et envoyer un flux en direct à débit unique vers des canaux AMS (avec activation de l’encodage en direct) en vue d’un traitement ultérieur. La rubrique contient des liens vers des didacticiels expliquant comment configurer les encodeurs répertoriés.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: 0ec6f046-0841-4673-9057-883bdbc30d5c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.openlocfilehash: d83c81e1a8a7d8d689e18cc8ba46fbb137959481
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "103014857"
---
# <a name="how-to-configure-on-premises-encoders-when-using-azure-media-services-to-create-multi-bitrate-streams"></a>Comment configurer des encodeurs locaux lorsque Azure Media Services est utilisé pour créer des flux multidébits

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

Cette rubrique répertorie les encodeurs en direct locaux que vous pouvez utiliser pour capturer vos événements en direct et envoyer un flux en direct à débit unique vers des canaux AMS (avec activation de l’encodage en direct) en vue d’un traitement ultérieur. La rubrique contient également des liens vers des didacticiels expliquant comment configurer les encodeurs répertoriés.

> [!NOTE]
> Lors de la diffusion en continu via RTMP, vérifiez les paramètres de pare-feu et/ou de proxy pour confirmer que les ports TCP sortants 1935 et 1936 sont ouverts.

## <a name="haivision-kb-encoder"></a>Encodeur Haivision KB
Pour plus d’informations sur la façon de configurer [l’encodeur Haivision KB](https://www.haivision.com/products/kb-series/) afin d’envoyer un streaming en direct à débit unique à un canal AMS, consultez [Configuration de l’encodeur Haivision KB](media-services-configure-kb-live-encoder.md).

## <a name="telestream-wirecast"></a>Telestream Wirecast
Pour plus d’informations sur la configuration de l’encodeur [Telestream Wirecast](https://www.telestream.net/wirecast/overview.htm) afin d’envoyer un streaming en direct à débit unique à un canal AMS, consultez [Configuration de Wirecast](media-services-configure-wirecast-live-encoder.md).

## <a name="elemental-live"></a>Elemental Live
Pour plus d’informations, consultez [Elemental Live](https://www.elemental.com/products/aws-elemental-appliances-software/#elemental-live).

## <a name="media-services-learning-paths"></a>Parcours d’apprentissage de Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Fournir des commentaires
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Étapes suivantes

[Streaming en direct avec Azure Media Services pour créer des flux multidébits](media-services-manage-live-encoder-enabled-channels.md).

