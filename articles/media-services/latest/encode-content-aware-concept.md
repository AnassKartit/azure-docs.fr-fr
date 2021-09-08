---
title: Présélection d’encodage sensible au contenu
description: Cet article décrit l’encodage sensible au contenu dans Microsoft Azure Media Services v3.
services: media-services
documentationcenter: ''
author: jiayali-ms
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 08/17/2021
ms.author: inhenkel
ms.custom: devx-track-csharp
ms.openlocfilehash: 5f333b4ca86e24c845a8a91c621a2b3f7c8c984e
ms.sourcegitcommit: 1deb51bc3de58afdd9871bc7d2558ee5916a3e89
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/19/2021
ms.locfileid: "122527896"
---
# <a name="content-aware-encoding-preset"></a>Présélection d’encodage sensible au contenu

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Afin de préparer le contenu pour une diffusion en [streaming à débit adaptatif](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), la vidéo doit être encodée à plusieurs débits (du plus élevé au plus faible). Cela garantit une dégradation appropriée de la qualité, la vitesse de transmission et la résolution de la vidéo sont réduites en parallèle. Un tel codage à vitesses de transmission multiples utilise ce qu’on appelle une échelle d’encodage, c’est-à-dire un tableau de résolutions et de vitesses de transmission. Consultez les [présélections d’encodage intégrées](/rest/api/media/transforms/createorupdate#encodernamedpreset) de Media Services.

Vous devez être conscient du contenu que vous traitez et personnaliser ou ajuster l’échelle d’encodage à la complexité de la vidéo. À chaque résolution, il existe un débit au-delà duquel toute augmentation de la qualité n’est pas perceptive : l’encodeur fonctionne à la valeur de débit optimale. Le niveau suivant d’optimisation consiste à sélectionner les résolutions en fonction du contenu, par exemple, une vidéo de présentation PowerPoint ne bénéficie d’aucune amélioration en passant sous 720p. Pour aller plus loin, l’encodeur peut être chargé afin d’optimiser les paramètres pour chaque scène de la vidéo. 

La présélection [Adaptive Streaming](encode-autogen-bitrate-ladder.md) (diffusion en continu adaptative) de Microsoft résout le problème de variabilité de la qualité et de la résolution des vidéos sources. Nos clients bénéficient d’une grande variété de contenus, certains en 1080p, d’autres en 720p, et quelques-uns en SD voire à des résolutions inférieures. En outre, tout le contenu source n’affiche pas les mezzanines haute qualité proposées par les studios de cinéma ou de télévision. La présélection Adaptive Streaming résout ces problèmes en s'assurant que l'échelle de débit ne dépasse jamais la résolution ou le débit moyen de la mezzanine en entrée. Toutefois, cette présélection n’examine pas les propriétés sources autres que la résolution et la vitesse de transmission.

## <a name="the-content-aware-encoding-preset"></a>La présélection d’encodage sensible au contenu

La présélection d’encodage sensible au contenu étend le mécanisme de diffusion en continu à vitesse de transmission adaptative, en incorporant une logique personnalisée permettant à l’encodeur de rechercher la valeur optimale de vitesse de transmission pour une résolution donnée, mais sans nécessiter une analyse informatique complète. Cette présélection produit un ensemble de fichiers MP4 alignés sur le groupe d’images. Étant donné un contenu d’entrée, le service effectue une analyse initiale légère du contenu d’entrée et utilise les résultats pour déterminer le nombre optimal de couches, le débit approprié et les paramètres de résolution pour la livraison par diffusion en continu adaptative. Cette présélection est particulièrement efficace pour les vidéos de complexité faible et moyenne, où les fichiers de sortie sont à des vitesses de transmission inférieures à la présélection Diffusion en continu adaptative, mais à une qualité qui offre toujours une bonne expérience aux viewers. La sortie contiendra des fichiers MP4 avec vidéo et audio entrelacées.

Les exemples de graphiques suivants illustrent la comparaison à l’aide de mesures de qualité comme [PSNR](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio) et [VMAF](https://en.wikipedia.org/wiki/Video_Multimethod_Assessment_Fusion). La source avait été créée en concaténant de petits clips très complexes extraits de films et de séries TV, destinés à soumettre l’encodeur à un test de contrainte. Par définition, cette présélection produit des résultats qui varient selon le contenu : cela signifie également que pour certains contenus, la réduction du débit ou l’amélioration de la qualité reste minime.

![Courbe de distorsion du débit (RD) avec PSNR](media/encode-content-aware-concept/msrv1.png)

**Figure 1 : Courbe de distorsion du débit (RD) avec la métrique PSNR pour une source très complexe**

![Courbe de distorsion du débit (RD) avec VMAF](media/encode-content-aware-concept/msrv2.png)

**Figure 2 : Courbe de distorsion du débit (RD) avec la métrique VMAF pour une source très complexe**

Voici les résultats pour une autre catégorie de contenu source, où l’encodeur a pu déterminer que l’entrée était de mauvaise qualité (de nombreux artefacts de compression en raison d’une faible vitesse de transmission). Notez qu’avec la présélection sensible au contenu, l’encodeur a décidé de ne produire qu’une seule couche de sortie, à une vitesse de transmission suffisamment faible pour que la plupart des clients puissent lire le flux sans saccade.

![Courbe de distorsion du débit (RD) avec PSNR](media/encode-content-aware-concept/msrv3.png)

**Figure 3 : Courbe de distorsion du débit (RD) avec PSNR pour une entrée de faible qualité (à 1080p)**

![Courbe de distorsion du débit (RD) avec VMAF](media/encode-content-aware-concept/msrv4.png)

**Figure 4 : Courbe de distorsion du débit (RD) avec VMAF pour une entrée de faible qualité (à 1080p)**

## <a name="8-bit-hevc-h265-support"></a>Prise en charge de HEVC (H. 265) sur 8 bits

L’encodeur standard des Services Media Azure prend désormais en charge l’encodage 8 bits HEVC (H.265). Le contenu HEVC peut être fourni et empaqueté via l’empaqueteur dynamique à l’aide du format 'hev1'.

Un nouvel encodage .NET personnalisé avec l’exemple HEVC est disponible dans le [dépôt GitHub media-services-v3-dotnet](https://github.com/Azure-Samples/media-services-v3-dotnet/tree/main/VideoEncoding/Encoding_HEVC). En plus de l’encodage personnalisé, AMS prend également en charge d’autres nouvelles présélections d’encodage HEVC intégrées que vous pouvez consulter dans nos [notes de publication de février 2021](https://docs.microsoft.com/azure/media-services/latest/release-notes#february-2021).
  
## <a name="next-steps"></a>Étapes suivantes

* [Tutoriel : Télécharger, encoder et diffuser des vidéos avec Media Services v3](stream-files-tutorial-with-api.md)
* [Tutoriel : Encoder un fichier distant basé sur une URL et diffuser la vidéo en continu - REST](stream-files-tutorial-with-rest.md)
* [Tutoriel : Encoder un fichier distant basé sur une URL et diffuser la vidéo en continu – CLI](stream-files-cli-quickstart.md)
* [Tutoriel : Encoder un fichier distant basé sur une URL et diffuser la vidéo en continu – .NET](stream-files-dotnet-quickstart.md)
* [Tutoriel : Encoder un fichier distant basé sur une URL et diffuser la vidéo en continu – Node.js](stream-files-nodejs-quickstart.md)
