---
title: Prise en charge des langues – Service Speech
titleSuffix: Azure Cognitive Services
description: Le service Speech prend en charge de nombreuses langues, que ce soit pour la reconnaissance vocale, la synthèse vocale ou la traduction vocale. Cet article fournit une liste complète des langues prise en charge, par fonctionnalité de service.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/07/2021
ms.author: pafarley
ms.custom: references_regions
ms.openlocfilehash: 8965a05f60ca8fd67d02f4095b8c82dee82e1e0d
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128605123"
---
# <a name="language-and-voice-support-for-the-speech-service"></a>Prise en charge des langues et de la voix pour le service Speech

La prise en charge des langues varie selon les fonctionnalités du service Speech. Les tableaux suivants résument la prise en charge des langues pour les offres de service [Reconnaissance vocale](#speech-to-text), [Synthèse vocale](#text-to-speech) et [Traduction vocale](#speech-translation).

## <a name="speech-to-text"></a>Reconnaissance vocale

Le Kit de développement logiciel (SDK) et l’API REST Speech de Microsoft prennent en charge les langages suivants (paramètres régionaux). 

Afin d'améliorer la précision, la personnalisation est proposée pour une partie des langues en chargeant des **transcriptions audio + étiquetées à la main** ou des **textes associés : Phrases.** La prise en charge de la personnalisation du modèle acoustique incluant des **données audio + transcriptions étiquetées à la main** se limite aux modèles de base spécifiques listés ci-dessous. Les autres langues et modèles de base utilisent uniquement le texte des transcriptions pour l’entraînement des modèles personnalisés, comme avec **Texte associé : Phrases.** Pour en savoir plus sur la personnalisation, consultez [Bien démarrer avec Custom Speech](./custom-speech-overview.md).

<!--
To get the AM and ML bits:
https://westus.dev.cognitive.microsoft.com/docs/services/speech-to-text-api-v3-0/operations/GetSupportedLocalesForModels

To get pronunciation bits:
https://cris.ai -> Click on Adaptation Data -> scroll down to section "Pronunciation Datasets" -> Click on Import -> Locale: the list of locales there correspond to the supported locales
-->

| Langage                 | Paramètres régionaux (BCP-47) | Personnalisations  | [Identification de la langue](how-to-automatic-language-detection.md) | [Évaluation de la prononciation](how-to-pronunciation-assessment.md) |
|------------------------------------|--------|---------------------------------------------------|-------------------------------|--------------------------|
| Arabe (Algérie)                   | `ar-DZ` | Text                                   |                           |                          |
| Arabe (Bahreïn), standard moderne  | `ar-BH` | Texte                                   |                           |                          |
| Arabe (Égypte)                     | `ar-EG` | Texte                                   | Oui                          |                          |
| Arabe (Irak)                      | `ar-IQ` | Texte                                   |                           |                          |
| Arabe (Israël)                    | `ar-IL` | Texte                                   |                           |                          |
| Arabe (Jordanie)                    | `ar-JO` | Texte                                   |                           |                          |
| Arabe (Koweït)                    | `ar-KW` | Texte                                   |                           |                          |
| Arabe (Liban)                   | `ar-LB` | Texte                                   |                           |                          |
| Arabe (Libye)                     | `ar-LY` | Text                                   |                           |                          |
| Arabe (Maroc)                   | `ar-MA` | Texte                                   |                           |                          |
| Arabe (Oman)                      | `ar-OM` | Texte                                   |                           |                          |
| Arabe (Qatar)                     | `ar-QA` | Texte                                   |                           |                          |
| Arabe (Arabie saoudite)              | `ar-SA` | Texte                                   |                           |                          |
| Arabe (Autorité palestinienne)     | `ar-PS` | Texte                                   |                           |                          |
| Arabe (Syrie)                     | `ar-SY` | Texte                                   |                           |                          |
| Arabe (Tunisie)                   | `ar-TN` | Texte                                   |                           |                          |
| Arabe (Émirats arabes unis)      | `ar-AE` | Texte                                   |                           |                          |
| Arabe (Yémen)                     | `ar-YE` | Texte                                   |                           |                          |
| Bulgare (Bulgarie)               | `bg-BG` | Texte                                   |                           |                          |
| Catalan (Espagne)                    | `ca-ES` | Texte<br>Prononcer                  | Oui                          |                          |
| Chinois (cantonais, traditionnel)   | `zh-HK` | Audio (20201015)<br>Texte                 |        Oui                   |                          |
| Chinois (mandarin, simplifié)     | `zh-CN` | Audio (20200910)<br>Texte                 |     Oui                      | Oui                         |
| Chinois (mandarin, taïwanais)       | `zh-TW` | Audio (20190701, 20201015)<br>Texte                 |           Oui                |                          |
| Croate (Croatie)                 | `hr-HR` | Texte<br>Prononcer                  |                           |                          |
| Tchèque (République tchèque)             | `cs-CZ` | Texte<br>Prononcer                  |                           |                          |
| Danois (Danemark)                   | `da-DK` | Texte<br>Prononcer                  | Oui                          |                          |
| Néerlandais (Pays-Bas)                | `nl-NL` | Audio (20201015)<br>Texte<br>Prononcer|    Oui                       |                          |
| Anglais (Australie)                | `en-AU` | Audio (20201019)<br>Texte<br>Prononcer| Oui                          |                          |
| Anglais (Canada)                   | `en-CA` | Audio (20201019)<br>Texte<br>Prononcer| Oui                          |                          |
| Anglais (Ghana)                    | `en-GH` | Texte<br>Prononcer                  |                           |                          |
| Anglais (Hong Kong)                | `en-HK` | Texte<br>Prononcer                  |                           |                          |
| Anglais (Inde)                    | `en-IN` | Audio (20200923)<br>Texte<br>Prononcer |                          |                          |
| Anglais (Irlande)                  | `en-IE` | Texte<br>Prononcer                  |                           |                          |
| Anglais (Kenya)                    | `en-KE` | Texte<br>Prononcer                  |                           |                          |
| Anglais (Nouvelle-Zélande)              | `en-NZ` | Audio (20201019)<br>Texte<br>Prononcer |                          |                          |
| Anglais (Nigeria)                  | `en-NG` | Texte<br>Prononcer                  |                           |                          |
| Anglais (Philippines)              | `en-PH` | Texte<br>Prononcer                  |                           |                          |
| Anglais (Singapour)                | `en-SG` | Texte<br>Prononcer                  |                           |                          |
| Anglais (Afrique du Sud)             | `en-ZA` | Texte<br>Prononcer                  |                           |                          |
| Anglais (Tanzanie)                 | `en-TZ` | Texte<br>Prononcer                  |                           |                          |
| Anglais (Royaume-Uni)           | `en-GB` | Audio (20201019)<br>Texte<br>Prononcer| Oui                          | Oui                         |
| Anglais (États-Unis)            | `en-US` | Audio (20201019, 20210223)<br>Texte<br>Prononcer| Oui                          | Oui                         |
| Estonien (Estonie)                  | `et-EE` | Texte<br>Prononcer                  |                           |                          |
| Filipino (Philippines)             | `fil-PH`| Texte<br>Prononcer                  |                           |                          |
| Finnois (Finlande)                  | `fi-FI` | Texte<br>Prononcer                  |     Oui                      |                          |
| Français (Canada)                    | `fr-CA` | Audio (20201015)<br>Texte<br>Prononcer|     Oui                      |                          |
| Français (France)                    | `fr-FR` | Audio (20201015)<br>Texte<br>Prononcer|      Oui                     |                          |
| Français (Suisse)               | `fr-CH` | Texte<br>Prononcer                  |                           |                          |
| Allemand (Autriche)                   | `de-AT` | Texte<br>Prononcer                  |                           |                          |
| Allemand (Allemagne)                   | `de-DE` | Audio (20190701, 20200619, 20201127)<br>Texte<br>Prononcer|  Oui                         |                          |
| Grec (Grèce)                     | `el-GR` | Texte                                   |  Oui                         |                          |
| Goudjrati (Inde)                  | `gu-IN` | Texte                                   |                           |                          |
| Hébreu (Israël)                    | `he-IL` | Texte                                   |                           |                          |
| Hindi (Inde)                      | `hi-IN` | Audio (20200701)<br>Texte                 |     Oui                      |                          |
| Hongrois (Hongrie)                | `hu-HU` | Texte<br>Prononcer                  |                           |                          |
| Indonésien (Indonésie)             | `id-ID` | Texte<br>Prononcer                  |                           |                          |
| Irlandais (Irlande)                    | `ga-IE` | Texte<br>Prononcer                  |                           |                          |
| Italien (Italie)                    | `it-IT` | Audio (20201016)<br>Texte<br>Prononcer|      Oui                     |                          |
| Japonais (Japon)                   | `ja-JP` | Texte                                   |      Oui                     |                          |
| Coréen (Corée)                     | `ko-KR` | Audio (20201015)<br>Texte                 |      Oui                     |                          |
| Letton (Lettonie)                   | `lv-LV` | Texte<br>Prononcer                  |                           |                          |
| Lituanien (Lituanie)             | `lt-LT` | Texte<br>Prononcer                  |                           |                          |
| Malais (Malaisie)                   | `ms-MY` | Texte                                   |                           |                          |
| Maltais (Malte)                    | `mt-MT` | Texte                                   |                           |                          |
| Marathi (Inde)                    | `mr-IN` | Texte                                   |                           |                          |
| Norvégien (bokmål, Norvège)         | `nb-NO` | Texte                                   |     Oui                      |                          |
| Persan (Iran)                     | `fa-IR` | Texte                                   |                           |                          |
| Polonais (Pologne)                    | `pl-PL` | Texte<br>Prononcer                  |       Oui                    |                          |
| Portugais (Brésil)                | `pt-BR` | Audio (20190620, 20201015)<br>Texte<br>Prononcer|          Oui                 |                          |
| Portugais (Portugal)              | `pt-PT` | Texte<br>Prononcer                  |             Oui              |                          |
| Roumain (Roumanie)                 | `ro-RO` | Texte<br>Prononcer                  |  Oui                         |                          |
| Russe (Russie)                   | `ru-RU` | Audio (20200907)<br>Texte                 |                Oui           |                          |
| Slovaque (Slovaquie)                  | `sk-SK` | Texte<br>Prononcer                  |                           |                          |
| Slovène (Slovénie)               | `sl-SI` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Argentine)                | `es-AR` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Bolivie)                  | `es-BO` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Chili)                    | `es-CL` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Colombie)                 | `es-CO` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Costa Rica)               | `es-CR` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Cuba)                     | `es-CU` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (République dominicaine)       | `es-DO` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Équateur)                  | `es-EC` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Salvador)              | `es-SV` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Guinée équatoriale)        | `es-GQ` | Texte                                   |                           |                          |
| Espagnol (Guatemala)                | `es-GT` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Honduras)                 | `es-HN` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Mexique)                   | `es-MX` | Audio (20200907)<br>Texte<br>Prononcer|    Oui                       |                          |
| Espagnol (Nicaragua)                | `es-NI` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Panama)                   | `es-PA` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Paraguay)                 | `es-PY` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Pérou)                     | `es-PE` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Porto Rico)              | `es-PR` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Espagne)                    | `es-ES` | Audio (20201015)<br>Texte<br>Prononcer|  Oui                         |                          |
| Espagnol (Uruguay)                  | `es-UY` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (États-Unis)                      | `es-US` | Texte<br>Prononcer                  |                           |                          |
| Espagnol (Venezuela)                | `es-VE` | Texte<br>Prononcer                  |                           |                          |
| Swahili (Kenya)                    | `sw-KE` | Texte                                   |                           |                          |
| Suédois (Suède)                   | `sv-SE` | Texte<br>Prononcer                  |   Oui                        |                          |
| Tamoul (Inde)                      | `ta-IN` | Texte                                   |                           |                          |
| Télougou (Inde)                     | `te-IN` | Texte                                   |                           |                          |
| Thaï (Thaïlande)                    | `th-TH` | Texte                                   |      Oui                     |                          |
| Turc (Turquie)                   | `tr-TR` | Texte                                   |                           |                          |
| Vietnamien (Vietnam)               | `vi-VN` | Texte                                   |                           |                          |

## <a name="text-to-speech"></a>Synthèse vocale

Le Kit de développement logiciel (SDK) Speech de Microsoft et les API REST prennent en charge ces voix qui, à leur tour, prennent chacune en charge une langue et un dialecte spécifiques, identifiés par les paramètres régionaux. Vous pouvez également obtenir la liste complète des langues et des voix prises en charge pour chaque région/point de terminaison spécifique via l’API [voices/list](rest-text-to-speech.md#get-a-list-of-voices). 

> [!IMPORTANT]
> Les prix varient pour les voix standard, personnalisées et neurales. Pour plus d’informations, visitez la page [Tarification](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="neural-voices"></a>Voix neurales

La synthèse vocale neuronale est un nouveau type de synthèse vocale alimentée par les réseaux neuronaux profonds. Lorsque vous utilisez une voix neuronale, il est presque impossible de distinguer la voix synthétisée des enregistrements humains.

Les voix neurales peuvent être utilisées pour rendre les interactions avec les chatbots et des assistants vocaux plus naturelles et agréables, convertir des textes numériques comme les livres électroniques en livres audio et améliorer les systèmes de navigation embarqués. Grâce à la prosodie naturelle quasi humaine et à la bonne articulation des mots, les voix neuronales réduisent considérablement la fatigue d’écoute des utilisateurs qui interagissent avec les systèmes d’intelligence artificielle.

> [!NOTE]
> Les voix neurales sont créées à partir d’exemples qui utilisent un taux d’échantillonnage de 24 kHz.
> Toutes les voix peuvent s’échantillonner ou sous-échantillonner à d’autres taux d’échantillonnage lors de la synthèse.

| Langage | Paramètres régionaux | Sexe | Nom de la voix | Prise en charge du style |
|---|---|---|---|---|
| Arabe (Égypte) | `ar-EG` | Female | `ar-EG-SalmaNeural` | Général |
| Arabe (Égypte) | `ar-EG` | Male | `ar-EG-ShakirNeural` | Général |
| Arabe (Arabie saoudite) | `ar-SA` | Female | `ar-SA-ZariyahNeural` | Général |
| Arabe (Arabie saoudite) | `ar-SA` | Male | `ar-SA-HamedNeural` | Général |
| Bulgare (Bulgarie) | `bg-BG` | Female | `bg-BG-KalinaNeural` | Général |
| Bulgare (Bulgarie) | `bg-BG` | Male | `bg-BG-BorislavNeural` | Général |
| Catalan (Espagne) | `ca-ES` | Female | `ca-ES-AlbaNeural` | Général |
| Catalan (Espagne) | `ca-ES` | Female | `ca-ES-JoanaNeural` | Général |
| Catalan (Espagne) | `ca-ES` | Male | `ca-ES-EnricNeural` | Général |
| Chinois (cantonais, traditionnel) | `zh-HK` | Female | `zh-HK-HiuGaaiNeural` | Général |
| Chinois (cantonais, traditionnel) | `zh-HK` | Female | `zh-HK-HiuMaanNeural` | Général |
| Chinois (cantonais, traditionnel) | `zh-HK` | Male | `zh-HK-WanLungNeural` | Général |
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-XiaoxiaoNeural` | Général, plusieurs styles de voix disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-XiaoyouNeural` | Voix d’enfant, optimisée pour la narration |
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-XiaomoNeural` | Général, plusieurs jeux de rôle et styles disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-XiaoxuanNeural` | Général, plusieurs jeux de rôle et styles disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-XiaohanNeural` | Général, plusieurs styles disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-XiaoruiNeural` | Voix de senior, plusieurs styles disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chinois (mandarin, simplifié) | `zh-CN` | Male | `zh-CN-YunyangNeural` | Optimisée pour la lecture de bulletins d’informations,<br /> plusieurs styles de voix disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chinois (mandarin, simplifié) | `zh-CN` | Male | `zh-CN-YunyeNeural` | Optimisée pour raconter des histoires |
| Chinois (mandarin, simplifié) | `zh-CN` | Male   | `zh-CN-YunxiNeural` | Général, plusieurs styles disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Chinois (mandarin, taïwanais) | `zh-TW` | Female | `zh-TW-HsiaoChenNeural` | Général |
| Chinois (mandarin, taïwanais) | `zh-TW` | Female | `zh-TW-HsiaoYuNeural` | Général |
| Chinois (mandarin, taïwanais) | `zh-TW` | Male | `zh-TW-YunJheNeural` | Général |
| Croate (Croatie) | `hr-HR` | Female | `hr-HR-GabrijelaNeural` | Général |
| Croate (Croatie) | `hr-HR` | Male | `hr-HR-SreckoNeural` | Général |
| Tchèque (Tchéquie) | `cs-CZ` | Female | `cs-CZ-VlastaNeural` | Général |
| Tchèque (Tchéquie) | `cs-CZ` | Male | `cs-CZ-AntoninNeural` | Général |
| Danois (Danemark) | `da-DK` | Female | `da-DK-ChristelNeural` | Général |
| Danois (Danemark) | `da-DK` | Male | `da-DK-JeppeNeural` | Général |
| Néerlandais (Belgique) | `nl-BE` | Female | `nl-BE-DenaNeural` | Général | 
| Néerlandais (Belgique) | `nl-BE` | Male | `nl-BE-ArnaudNeural` | Général | 
| Néerlandais (Pays-Bas) | `nl-NL` | Female | `nl-NL-ColetteNeural` | Général |
| Néerlandais (Pays-Bas) | `nl-NL` | Female | `nl-NL-FennaNeural` | Général |
| Néerlandais (Pays-Bas) | `nl-NL` | Male | `nl-NL-MaartenNeural` | Général |
| Anglais (Australie) | `en-AU` | Female | `en-AU-NatashaNeural` | Général |
| Anglais (Australie) | `en-AU` | Male | `en-AU-WilliamNeural` | Général |
| Anglais (Canada) | `en-CA` | Female | `en-CA-ClaraNeural` | Général |
| Anglais (Canada) | `en-CA` | Male | `en-CA-LiamNeural` | Général |
| Anglais (Hong Kong) | `en-HK` | Female | `en-HK-YanNeural` <sup>Nouveau</sup> | Général |
| Anglais (Hong Kong) | `en-HK` | Male | `en-HK-SamNeural` <sup>Nouveau</sup> | Général |
| Anglais (Inde) | `en-IN` | Female | `en-IN-NeerjaNeural` | Général |
| Anglais (Inde) | `en-IN` | Male | `en-IN-PrabhatNeural` | Général |
| Anglais (Irlande) | `en-IE` | Female | `en-IE-EmilyNeural` | Général |
| Anglais (Irlande) | `en-IE` | Male | `en-IE-ConnorNeural` | Général |
| Anglais (Nouvelle-Zélande) | `en-NZ` | Female | `en-NZ-MollyNeural` <sup>Nouveau</sup> | Général |
| Anglais (Nouvelle-Zélande) | `en-NZ` | Male | `en-NZ-MitchellNeural` <sup>Nouveau</sup> | Général |
| Anglais (Philippines) | `en-PH` | Female | `en-PH-RosaNeural` | Général | 
| Anglais (Philippines) | `en-PH` | Male | `en-PH-JamesNeural` | Général | 
| Anglais (Singapour) | `en-SG` | Female | `en-SG-LunaNeural` <sup>Nouveau</sup> | Général |
| Anglais (Singapour) | `en-SG` | Male | `en-SG-WayneNeural` <sup>Nouveau</sup> | Général |
| Anglais (Afrique du Sud) | `en-ZA` | Female | `en-ZA-LeahNeural` <sup>Nouveau</sup> | Général |
| Anglais (Afrique du Sud) | `en-ZA` | Male | `en-ZA-LukeNeural` <sup>Nouveau</sup> | Général |
| Anglais (Royaume-Uni) | `en-GB` | Female | `en-GB-LibbyNeural` | Général |
| Anglais (Royaume-Uni) | `en-GB` | Female | `en-GB-SoniaNeural` <sup>Nouveau</sup> | Général |
| Anglais (Royaume-Uni) | `en-GB` | Female | `en-GB-MiaNeural` <sup>Mise hors service le 15 octobre, voir ci-dessous</sup> | Général |
| Anglais (Royaume-Uni) | `en-GB` | Male | `en-GB-RyanNeural` | Général |
| Anglais (États-Unis) | `en-US` | Female | `en-US-AriaNeural` | Général, plusieurs styles de voix disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Anglais (États-Unis) | `en-US` | Female | `en-US-JennyNeural` | Général, plusieurs styles de voix disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Anglais (États-Unis) | `en-US` | Male | `en-US-GuyNeural` | Général, plusieurs styles de voix disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Anglais (États-Unis) | `en-US` | Female | `en-US-SaraNeural` <sup>Nouveau</sup> | Général, plusieurs styles de voix disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Anglais (États-Unis) | `en-US` | Female | `en-US-AmberNeural` | Général |
| Anglais (États-Unis) | `en-US` | Female | `en-US-AshleyNeural` | Général |
| Anglais (États-Unis) | `en-US` | Female | `en-US-CoraNeural` | Général |
| Anglais (États-Unis) | `en-US` | Female | `en-US-ElizabethNeural` | Général |
| Anglais (États-Unis) | `en-US` | Female | `en-US-MichelleNeural`| Général |
| Anglais (États-Unis) | `en-US` | Female | `en-US-MonicaNeural` | Général |
| Anglais (États-Unis) | `en-US` | Kid | `en-US-AnaNeural`| Général |
| Anglais (États-Unis) | `en-US` | Male | `en-US-BrandonNeural` | Général |
| Anglais (États-Unis) | `en-US` | Male | `en-US-ChristopherNeural`  | Général |
| Anglais (États-Unis) | `en-US` | Male | `en-US-JacobNeural` | Général |
| Anglais (États-Unis) | `en-US` | Male | `en-US-EricNeural` | Général |
| Estonien (Estonie) | `et-EE` | Female | `et-EE-AnuNeural` | Général |
| Estonien (Estonie) | `et-EE` | Male | `et-EE-KertNeural` | Général |
| Finnois (Finlande) | `fi-FI` | Female | `fi-FI-NooraNeural` | Général |
| Finnois (Finlande) | `fi-FI` | Female | `fi-FI-SelmaNeural` | Général |
| Finnois (Finlande) | `fi-FI` | Male | `fi-FI-HarriNeural` | Général |
| Français (Belgique) | `fr-BE` | Female | `fr-BE-CharlineNeural` | Général | 
| Français (Belgique) | `fr-BE` | Male | `fr-BE-GerardNeural` | Général | 
| Français (Canada) | `fr-CA` | Female | `fr-CA-SylvieNeural` | Général |
| Français (Canada) | `fr-CA` | Male | `fr-CA-AntoineNeural` | Général |
| Français (Canada) | `fr-CA` | Male | `fr-CA-JeanNeural` | Général |
| Français (France) | `fr-FR` | Female | `fr-FR-DeniseNeural` | Général |
| Français (France) | `fr-FR` | Male | `fr-FR-HenriNeural` | Général |
| Français (Suisse) | `fr-CH` | Female | `fr-CH-ArianeNeural` | Général |
| Français (Suisse) | `fr-CH` | Male | `fr-CH-FabriceNeural` | Général |
| Allemand (Autriche) | `de-AT` | Female | `de-AT-IngridNeural` | Général |
| Allemand (Autriche) | `de-AT` | Male | `de-AT-JonasNeural` | Général |
| Allemand (Allemagne) | `de-DE` | Female | `de-DE-KatjaNeural` | Général |
| Allemand (Allemagne) | `de-DE` | Male | `de-DE-ConradNeural` | Général |
| Allemand (Suisse) | `de-CH` | Female | `de-CH-LeniNeural` | Général |
| Allemand (Suisse) | `de-CH` | Male | `de-CH-JanNeural` | Général |
| Grec (Grèce) | `el-GR` | Female | `el-GR-AthinaNeural` | Général |
| Grec (Grèce) | `el-GR` | Male | `el-GR-NestorasNeural` | Général |
| Goudjrati (Inde) | `gu-IN` | Female | `gu-IN-DhwaniNeural` <sup>Nouveau</sup> | Général |
| Goudjrati (Inde) | `gu-IN` | Male | `gu-IN-NiranjanNeural` <sup>Nouveau</sup> | Général |
| Hébreu (Israël) | `he-IL` | Female | `he-IL-HilaNeural` | Général |
| Hébreu (Israël) | `he-IL` | Male | `he-IL-AvriNeural` | Général |
| Hindi (Inde) | `hi-IN` | Female | `hi-IN-SwaraNeural` | Général |
| Hindi (Inde) | `hi-IN` | Male | `hi-IN-MadhurNeural` | Général |
| Hongrois (Hongrie) | `hu-HU` | Female | `hu-HU-NoemiNeural` | Général |
| Hongrois (Hongrie) | `hu-HU` | Male | `hu-HU-TamasNeural` | Général |
| Indonésien (Indonésie) | `id-ID` | Female | `id-ID-GadisNeural` | Général |
| Indonésien (Indonésie) | `id-ID` | Male | `id-ID-ArdiNeural` | Général |
| Irlandais (Irlande) | `ga-IE` | Female | `ga-IE-OrlaNeural` | Général |
| Irlandais (Irlande) | `ga-IE` | Male | `ga-IE-ColmNeural` | Général |
| Italien (Italie) | `it-IT` | Female | `it-IT-ElsaNeural` | Général |
| Italien (Italie) | `it-IT` | Female | `it-IT-IsabellaNeural` | Général |
| Italien (Italie) | `it-IT` | Male | `it-IT-DiegoNeural` | Général |
| Japonais (Japon) | `ja-JP` | Female | `ja-JP-NanamiNeural` | Général |
| Japonais (Japon) | `ja-JP` | Male | `ja-JP-KeitaNeural` | Général |
| Coréen (Corée) | `ko-KR` | Female | `ko-KR-SunHiNeural` | Général |
| Coréen (Corée) | `ko-KR` | Male | `ko-KR-InJoonNeural` | Général |
| Letton (Lettonie) | `lv-LV` | Female | `lv-LV-EveritaNeural` | Général |
| Letton (Lettonie) | `lv-LV` | Male | `lv-LV-NilsNeural` | Général |
| Lituanien (Lituanie) | `lt-LT` | Female | `lt-LT-OnaNeural` | Général |
| Lituanien (Lituanie) | `lt-LT` | Male | `lt-LT-LeonasNeural` | Général |
| Malais (Malaisie) | `ms-MY` | Female | `ms-MY-YasminNeural` | Général |
| Malais (Malaisie) | `ms-MY` | Male | `ms-MY-OsmanNeural` | Général |
| Maltais (Malte) | `mt-MT` | Female | `mt-MT-GraceNeural` | Général |
| Maltais (Malte) | `mt-MT` | Male | `mt-MT-JosephNeural` | Général |
| Marathi (Inde) | `mr-IN` | Female | `mr-IN-AarohiNeural` <sup>Nouveau</sup> | Général |
| Marathi (Inde) | `mr-IN` | Male | `mr-IN-ManoharNeural` <sup>Nouveau</sup> | Général |
| Norvégien (bokmål, Norvège) | `nb-NO` | Female | `nb-NO-IselinNeural` | Général |
| Norvégien (bokmål, Norvège) | `nb-NO` | Female | `nb-NO-PernilleNeural` | Général |
| Norvégien (bokmål, Norvège) | `nb-NO` | Male | `nb-NO-FinnNeural` | Général |
| Polonais (Pologne) | `pl-PL` | Female | `pl-PL-AgnieszkaNeural` | Général |
| Polonais (Pologne) | `pl-PL` | Female | `pl-PL-ZofiaNeural` | Général |
| Polonais (Pologne) | `pl-PL` | Male | `pl-PL-MarekNeural` | Général |
| Portugais (Brésil) | `pt-BR` | Female | `pt-BR-FranciscaNeural` | Général, plusieurs styles de voix disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles) |
| Portugais (Brésil) | `pt-BR` | Male | `pt-BR-AntonioNeural` | Général |
| Portugais (Portugal) | `pt-PT` | Female | `pt-PT-FernandaNeural` | Général |
| Portugais (Portugal) | `pt-PT` | Female | `pt-PT-RaquelNeural` | Général |
| Portugais (Portugal) | `pt-PT` | Male | `pt-PT-DuarteNeural` | Général |
| Roumain (Roumanie) | `ro-RO` | Female | `ro-RO-AlinaNeural` | Général |
| Roumain (Roumanie) | `ro-RO` | Male | `ro-RO-EmilNeural` | Général |
| Russe (Russie) | `ru-RU` | Female | `ru-RU-DariyaNeural` | Général |
| Russe (Russie) | `ru-RU` | Female | `ru-RU-SvetlanaNeural` | Général |
| Russe (Russie) | `ru-RU` | Male | `ru-RU-DmitryNeural` | Général |
| Slovaque (Slovaquie) | `sk-SK` | Female | `sk-SK-ViktoriaNeural` | Général |
| Slovaque (Slovaquie) | `sk-SK` | Male | `sk-SK-LukasNeural` | Général |
| Slovène (Slovénie) | `sl-SI` | Female | `sl-SI-PetraNeural` | Général |
| Slovène (Slovénie) | `sl-SI` | Male | `sl-SI-RokNeural` | Général |
| Espagnol (Argentine) | `es-AR` | Female | `es-AR-ElenaNeural` <sup>Nouveau</sup> | Général |
| Espagnol (Argentine) | `es-AR` | Male | `es-AR-TomasNeural` <sup>Nouveau</sup> | Général |
| Espagnol (Colombie) | `es-CO` | Female | `es-CO-SalomeNeural` <sup>Nouveau</sup> | Général |
| Espagnol (Colombie) | `es-CO` | Male | `es-CO-GonzaloNeural` <sup>Nouveau</sup> | Général |
| Espagnol (Mexique) | `es-MX` | Female | `es-MX-DaliaNeural` | Général |
| Espagnol (Mexique) | `es-MX` | Male | `es-MX-JorgeNeural` | Général |
| Espagnol (Espagne) | `es-ES` | Female | `es-ES-ElviraNeural` | Général |
| Espagnol (Espagne) | `es-ES` | Male | `es-ES-AlvaroNeural` | Général |
| Espagnol (États-Unis) | `es-US` | Female | `es-US-PalomaNeural` <sup>Nouveau</sup> | Général |
| Espagnol (États-Unis) | `es-US` | Male | `es-US-AlonsoNeural` <sup>Nouveau</sup> | Général |
| Swahili (Kenya) | `sw-KE` | Female | `sw-KE-ZuriNeural` <sup>Nouveau</sup> | Général |
| Swahili (Kenya) | `sw-KE` | Male | `sw-KE-RafikiNeural` <sup>Nouveau</sup> | Général |
| Suédois (Suède) | `sv-SE` | Female | `sv-SE-HilleviNeural` | Général |
| Suédois (Suède) | `sv-SE` | Female | `sv-SE-SofieNeural` | Général |
| Suédois (Suède) | `sv-SE` | Male | `sv-SE-MattiasNeural` | Général |
| Tamoul (Inde) | `ta-IN` | Female | `ta-IN-PallaviNeural` | Général |
| Tamoul (Inde) | `ta-IN` | Male | `ta-IN-ValluvarNeural` | Général |
| Télougou (Inde) | `te-IN` | Female | `te-IN-ShrutiNeural` | Général |
| Télougou (Inde) | `te-IN` | Male | `te-IN-MohanNeural` | Général |
| Thaï (Thaïlande) | `th-TH` | Female | `th-TH-AcharaNeural` | Général |
| Thaï (Thaïlande) | `th-TH` | Female | `th-TH-PremwadeeNeural` | Général |
| Thaï (Thaïlande) | `th-TH` | Male | `th-TH-NiwatNeural` | Général |
| Turc (Turquie) | `tr-TR` | Female | `tr-TR-EmelNeural` | Général |
| Turc (Turquie) | `tr-TR` | Male | `tr-TR-AhmetNeural` | Général |
| Ukrainien (Ukraine) | `uk-UA` | Female | `uk-UA-PolinaNeural` | Général | 
| Ukrainien (Ukraine) | `uk-UA` | Male | `uk-UA-OstapNeural` | Général | 
| Ourdou (Pakistan) | `ur-PK` | Female | `ur-PK-UzmaNeural`  | Général | 
| Ourdou (Pakistan) | `ur-PK` | Male | `ur-PK-AsadNeural` | Général | 
| Vietnamien (Vietnam) | `vi-VN` | Female | `vi-VN-HoaiMyNeural` | Général |
| Vietnamien (Vietnam) | `vi-VN` | Male | `vi-VN-NamMinhNeural` | Général |
| Gallois (Royaume-Uni) | `cy-GB` | Female | `cy-GB-NiaNeural` | Général | 
| Gallois (Royaume-Uni) | `cy-GB` | Male | `cy-GB-AledNeural` | Général | 

> [!IMPORTANT]
> La voix en anglais (Royaume-Uni) `en-GB-MiaNeural` sera mise hors service le **30 octobre 2021**. Toutes les demandes de service adressées à `en-GB-MiaNeural` seront automatiquement redirigées vers `en-GB-SoniaNeural` après le **30 octobre 2021**.

#### <a name="neural-voices-in-preview"></a>Voix neuronales en préversion

Les voix neuronales ci-dessous sont en préversion publique. 

| Langage                         | Paramètres régionaux  | Sexe | Nom de la voix                             | Prise en charge du style |
|----------------------------------|---------|--------|----------------------------------------|---------------|
| Anglais (États-Unis) | `en-US` | Female | `en-US-JennyMultilingualNeural` <sup>Nouveau</sup> | Fonctionnalités multilingues générales disponibles [avec SSML](speech-synthesis-markup.md#create-an-ssml-document) |
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-XiaochenNeural` <sup>Nouveau</sup> | Optimisée pour les conversations spontanées |
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-XiaoyanNeural` <sup>Nouveau</sup> | Optimisée pour le service clientèle |
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-XiaoshuangNeural` <sup>Nouveau</sup> | Voix d’enfant, optimisée pour les récits et les conversations pour enfants ; plusieurs styles vocaux disponibles [à l’aide de SSML](speech-synthesis-markup.md#adjust-speaking-styles)|
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-XiaoqiuNeural` <sup>Nouveau</sup> | Optimisée pour la narration |

> [!IMPORTANT]
> Les voix en préversion publique sont uniquement disponibles dans 3 régions de service : USA Est, Europe Ouest et Asie Sud-Est.

> [!TIP]
> `en-US-JennyNeuralMultilingual` prend en charge plusieurs langues. Vérifiez la liste des langues prises en charge par l’[API voices/list](rest-text-to-speech.md#get-a-list-of-voices).

Pour plus d’informations sur la disponibilité régionale, consultez [régions](regions.md#neural-and-standard-voices).

Pour savoir comment configurer et ajuster les voix neuronales, comme les styles de diction, consultez [Langage de balisage de synthèse vocale](speech-synthesis-markup.md#adjust-speaking-styles).

> [!IMPORTANT]
> La voix `en-US-JessaNeural` a basculé sur `en-US-AriaNeural`. Si vous utilisiez « Jessa », procédez à une conversion vers « Aria ».

> [!TIP]
> Vous pouvez continuer à utiliser le mappage de nom de service complet comme « Voix Microsoft Server Speech Text to Speech (en-US, ChristopherNeural) » dans vos requêtes de synthèse vocale.

### <a name="standard-voices"></a>Voix standard

Plus de 75 voix standard sont disponibles dans plus de 45 langues et paramètres régionaux, ce qui vous permet de convertir le texte en parole synthétisée. Pour plus d’informations sur la disponibilité régionale, consultez [régions](regions.md#neural-and-standard-voices).

> [!IMPORTANT]
> Nous retirons les voix standard le **31 août 2024** et elles ne seront plus prises en charge après cette date.Nous avons annoncé cette mise hors service dans les e-mails envoyés à tous les abonnements Speech existants créés avant le **31 août 2021**. Pendant la période de mise hors service (**31 août 2021** - **31 août 2024**), les utilisateurs actuels de voix standard peuvent continuer à utiliser des voix standard, mais tous les nouveaux utilisateurs et toutes les nouvelles ressources de reconnaissance vocale doivent choisir des voix neurales.

> [!NOTE]
> À deux exceptions près, les voix standard sont créées à partir d’exemples qui utilisent un taux d’échantillonnage de 16 kHz.
> **Les voix en-US-AriaRUS** et **en-US-GuyRUS** sont également créées à partir d’exemples qui utilisent un taux d’échantillonnage de 24 kHz.
> Toutes les voix peuvent s’échantillonner ou sous-échantillonner à d’autres taux d’échantillonnage lors de la synthèse.

| Langage | Paramètres régionaux (BCP-47) | Sexe | Nom de la voix |
|--|--|--|--|
| Arabe (arabe) | `ar-EG` | Female | `ar-EG-Hoda`|
| Arabe (Arabie saoudite) | `ar-SA` | Male | `ar-SA-Naayf`|
| Bulgare (Bulgarie) | `bg-BG` | Male | `bg-BG-Ivan`|
| Catalan (Espagne) | `ca-ES` | Female | `ca-ES-HerenaRUS`|
| Chinois (cantonais, traditionnel) | `zh-HK` | Male | `zh-HK-Danny`|
| Chinois (cantonais, traditionnel) | `zh-HK` | Female | `zh-HK-TracyRUS`|
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-HuihuiRUS`|
| Chinois (mandarin, simplifié) | `zh-CN` | Male | `zh-CN-Kangkang`|
| Chinois (mandarin, simplifié) | `zh-CN` | Female | `zh-CN-Yaoyao`|
| Chinois (mandarin, taïwanais) |  `zh-TW` | Female | `zh-TW-HanHanRUS`|
| Chinois (mandarin, taïwanais) |  `zh-TW` | Female | `zh-TW-Yating`|
| Chinois (mandarin, taïwanais) |  `zh-TW` | Male | `zh-TW-Zhiwei`|
| Croate (Croatie) | `hr-HR` | Male | `hr-HR-Matej`|
| Tchèque (République tchèque) | `cs-CZ` | Male | `cs-CZ-Jakub`|
| Danois (Danemark) | `da-DK` | Female | `da-DK-HelleRUS`|
| Néerlandais (Pays-Bas) | `nl-NL` | Female | `nl-NL-HannaRUS`|
| Anglais (Australie) | `en-AU` | Female | `en-AU-Catherine`|
| Anglais (Australie) | `en-AU` | Female | `en-AU-HayleyRUS`|
| Anglais (Canada) | `en-CA` | Female | `en-CA-HeatherRUS`|
| Anglais (Canada) | `en-CA` | Female | `en-CA-Linda`|
| Anglais (Inde) | `en-IN` | Female | `en-IN-Heera`|
| Anglais (Inde) | `en-IN` | Female | `en-IN-PriyaRUS`|
| Anglais (Inde) | `en-IN` | Male | `en-IN-Ravi`|
| Anglais (Irlande) | `en-IE` | Male | `en-IE-Sean`|
| Anglais (Royaume-Uni) | `en-GB` | Male | `en-GB-George`|
| Anglais (Royaume-Uni) | `en-GB` | Female | `en-GB-HazelRUS`|
| Anglais (Royaume-Uni) | `en-GB` | Female | `en-GB-Susan`|
| Anglais (États-Unis) | `en-US` | Male | `en-US-BenjaminRUS`|
| Anglais (États-Unis) | `en-US` | Male | `en-US-GuyRUS`|
| Anglais (États-Unis) | `en-US` | Female | `en-US-AriaRUS`|
| Anglais (États-Unis) | `en-US` | Female | `en-US-ZiraRUS`|
| Finnois (Finlande) | `fi-FI` | Female | `fi-FI-HeidiRUS`|
| Français (Canada) | `fr-CA` | Female | `fr-CA-Caroline`|
| Français (Canada) | `fr-CA` | Female | `fr-CA-HarmonieRUS`|
| Français (France) | `fr-FR` | Female | `fr-FR-HortenseRUS`|
| Français (France) | `fr-FR` | Female | `fr-FR-Julie`|
| Français (France) | `fr-FR` | Male | `fr-FR-Paul`|
| Français (Suisse) | `fr-CH` | Male | `fr-CH-Guillaume`|
| Allemand (Autriche) | `de-AT` | Male | `de-AT-Michael`|
| Allemand (Allemagne) | `de-DE` | Female | `de-DE-HeddaRUS`|
| Allemand (Allemagne) | `de-DE` | Male | `de-DE-Stefan`|
| Allemand (Suisse) | `de-CH` | Male | `de-CH-Karsten`|
| Grec (Grèce) | `el-GR` | Male | `el-GR-Stefanos`|
| Hébreu (Israël) | `he-IL` | Male | `he-IL-Asaf`|
| Hindi (Inde) | `hi-IN` | Male | `hi-IN-Hemant`|
| Hindi (Inde) | `hi-IN` | Female | `hi-IN-Kalpana`|
| Hongrois (Hongrie) | `hu-HU` | Male | `hu-HU-Szabolcs`|
| Indonésien (Indonésie) | `id-ID` | Male | `id-ID-Andika`|
| Italien (Italie) | `it-IT` | Male | `it-IT-Cosimo`|
| Italien (Italie) | `it-IT` | Female | `it-IT-LuciaRUS`|
| Japonais (Japon) | `ja-JP` | Female | `ja-JP-Ayumi`|
| Japonais (Japon) | `ja-JP` | Female | `ja-JP-HarukaRUS`|
| Japonais (Japon) | `ja-JP` | Male | `ja-JP-Ichiro`|
| Coréen (Corée) | `ko-KR` | Female | `ko-KR-HeamiRUS`|
| Malais (Malaisie) | `ms-MY` | Male | `ms-MY-Rizwan`|
| Norvégien (bokmål, Norvège) | `nb-NO` | Female | `nb-NO-HuldaRUS`|
| Polonais (Pologne) | `pl-PL` | Female | `pl-PL-PaulinaRUS`|
| Portugais (Brésil) | `pt-BR` | Male | `pt-BR-Daniel`|
| Portugais (Brésil) | `pt-BR` | Female | `pt-BR-HeloisaRUS`|
| Portugais (Portugal) | `pt-PT` | Female | `pt-PT-HeliaRUS`|
| Roumain (Roumanie) | `ro-RO` | Male | `ro-RO-Andrei`|
| Russe (Russie) | `ru-RU` | Female | `ru-RU-EkaterinaRUS`|
| Russe (Russie) | `ru-RU` | Female | `ru-RU-Irina`|
| Russe (Russie) | `ru-RU` | Male | `ru-RU-Pavel`|
| Slovaque (Slovaquie) | `sk-SK` | Male | `sk-SK-Filip`|
| Slovène (Slovénie) | `sl-SI` | Male | `sl-SI-Lado`|
| Espagnol (Mexique) | `es-MX` | Female | `es-MX-HildaRUS`|
| Espagnol (Mexique) | `es-MX` | Male | `es-MX-Raul`|
| Espagnol (Espagne) | `es-ES` | Female | `es-ES-HelenaRUS`|
| Espagnol (Espagne) | `es-ES` | Female | `es-ES-Laura`|
| Espagnol (Espagne) | `es-ES` | Male | `es-ES-Pablo`|
| Suédois (Suède) | `sv-SE` | Female | `sv-SE-HedvigRUS`|
| Tamoul (Inde) | `ta-IN` | Male | `ta-IN-Valluvar`|
| Télougou (Inde) | `te-IN` | Female | `te-IN-Chitra`|
| Thaï (Thaïlande) | `th-TH` | Male | `th-TH-Pattara`|
| Turc (Turquie) | `tr-TR` | Female | `tr-TR-SedaRUS`|
| Vietnamien (Vietnam) | `vi-VN` | Male | `vi-VN-An` |

> [!IMPORTANT]
> La voix `en-US-Jessa` a basculé sur `en-US-Aria`. Si vous utilisiez « Jessa », procédez à une conversion vers « Aria ».

> [!TIP]
> Vous pouvez continuer à utiliser le mappage de nom de service complet comme « Voix Microsoft Server Speech Text to Speech (en-US, AriaRUS) » dans vos requêtes de synthèse vocale.

### <a name="customization"></a>Personnalisation

Custom Voice est disponible au niveau neuronal (Voix neuronale personnalisée). S’appuyant sur la technologie TTS neuronale et le modèle universel multi-voix multilingue, la voix neuronale personnalisée vous permet de créer des voix de synthèse riches en termes de styles d’élocution ou de plurilinguisme. Vérifiez ci-dessous les langues prises en charge.  

> [!IMPORTANT]
> Le niveau standard, y compris les méthodes de formation paramétrique statistique et par concaténation de Custom Voice, est déprécié et sera retiré le 2/29/2024. Si vous utilisez une instance Custom Voice non neurale/standard, migrez immédiatement vers la Voix neuronale personnalisée pour profiter de la meilleure qualité et déployer les voix de manière responsable. 

| Langage | Paramètres régionaux | Neuronale | Multilingue |
|--|--|--|--|
| Bulgare (Bulgarie)| `bg-BG` | Oui | Non |
| Chinois (mandarin, simplifié) | `zh-CN` | Oui | Oui |
| Chinois (mandarin, simplifié), anglais bilingue | `zh-CN` bilingue | Oui | Oui |
| Néerlandais (Pays-Bas)   | `nl-NL` | Oui | Non |
| Anglais (Australie) | `en-AU` | Oui | Oui |
| Anglais (Inde) | `en-IN` | Oui | Non |
| Anglais (Royaume-Uni) | `en-GB` | Oui | Oui |
| Anglais (États-Unis) | `en-US` | Oui | Oui |
| Français (Canada) | `fr-CA` | Oui | Oui |
| Français (France) | `fr-FR` | Oui | Oui |
| Allemand (Allemagne) | `de-DE` | Oui | Oui |
| Italien (Italie) | `it-IT` | Oui | Oui |
| Japonais (Japon) | `ja-JP` | Oui | Oui |
| Coréen (Corée) | `ko-KR` | Oui | Oui |
| Norvégien (bokmål, Norvège) | `nb-NO` | Oui | Non |
| Portugais (Brésil) | `pt-BR` | Oui | Oui |
| Russe (Russie) | `ru-RU` | Oui | Oui |
| Espagnol (Mexique) | `es-MX` | Oui | Oui |
| Espagnol (Espagne) | `es-ES` | Oui | Oui |

Sélectionnez les paramètres régionaux qui correspondent aux données d’apprentissage servant à entraîner un modèle vocal personnalisé. Par exemple, si vos données d’enregistrement sont en anglais avec un accent britannique, sélectionnez `en-GB`.

> [!NOTE]
> À l’exception du chinois-anglais, nous ne gérons pas l’apprentissage de modèles bilingues dans Custom Voice. Sélectionnez « Bilingue chinois-anglais » si vous souhaitez effectuer l’apprentissage d’une voix en chinois qui parle aussi anglais. L’entraînement du modèle bilingue chinois-anglais à l’aide de la méthode standard est disponible uniquement pour la région Europe Nord et la région USA Centre Nord. L’entraînement de la voix neuronale personnalisée est disponible dans les régions Royaume-Uni Sud et USA Est.

## <a name="speech-translation"></a>Traduction vocale

L’API **Traduction vocale** prend en charge différentes langues pour la traduction de parole en parole et de parole en texte. La langue source doit toujours provenir du tableau des langues de reconnaissance vocale. Les langues cibles disponibles dépendent selon que cible de la traduction est de la parole ou du texte. Vous pouvez traduire les conversations entrantes dans plus de [60 langues](https://www.microsoft.com/translator/business/languages/). Un sous-ensemble de langues est disponible pour la [synthèse vocale](language-support.md#text-languages).

### <a name="text-languages"></a>Langues de texte

| Langue de texte           | Code langue |
|:------------------------|:-------------:|
| Afrikaans               | `af`          |
| Arabe                  | `ar`          |
| Bangla                  | `bn`          |
| Bosniaque (latin)         | `bs`          |
| Bulgare               | `bg`          |
| Cantonais (traditionnel) | `yue`         |
| Catalan                 | `ca`          |
| Chinois (simplifié)      | `zh-Hans`     |
| Chinois traditionnel     | `zh-Hant`     |
| Croate                | `hr`          |
| Tchèque                   | `cs`          |
| Danois                  | `da`          |
| Néerlandais                   | `nl`          |
| Anglais                 | `en`          |
| Estonien                | `et`          |
| Fidjien                  | `fj`          |
| Filipino                | `fil`         |
| Finnois                 | `fi`          |
| Français                  | `fr`          |
| Allemand                  | `de`          |
| Grec                   | `el`          |
| Goudjrati                | `gu`          |
| Créole haïtien          | `ht`          |
| Hébreu                  | `he`          |
| Hindi                   | `hi`          |
| Hmong blanc               | `mww`         |
| Hongrois               | `hu`          |
| Indonésien              | `id`          |
| Irlandais                   | `ga`          |
| Italien                 | `it`          |
| Japonais                | `ja`          |
| Kannada                 | `kn`          |
| Kiswahili               | `sw`          |
| Klingon                 | `tlh-Latn`    |
| Klingon (plqaD)         | `tlh-Piqd`    |
| Coréen                  | `ko`          |
| Letton                 | `lv`          |
| Lituanien              | `lt`          |
| Malgache                | `mg`          |
| Malais                   | `ms`          |
| Malayalam               | `ml`          |
| Maltais                 | `mt`          |
| Maori                   | `mi`          |
| Marathi                 | `mr`          |
| Norvégien               | `nb`          |
| Persan                 | `fa`          |
| Polonais                  | `pl`          |
| Portugais (Brésil)     | `pt-br`       |
| Portugais (Portugal)   | `pt-pt`       |
| Pendjabi                 | `pa`          |
| Queretaro Otomi         | `otq`         |
| Roumain                | `ro`          |
| Russe                 | `ru`          |
| Samoan                  | `sm`          |
| Serbe (cyrillique)      | `sr-Cyrl`     |
| Serbe (latin)         | `sr-Latn`     |
| Slovaque                  | `sk`          |
| Slovène               | `sl`          |
| Espagnol                 | `es`          |
| Suédois                 | `sv`          |
| Tahitien                | `ty`          |
| Tamoul                   | `ta`          |
| Télougou                  | `te`          |
| Thaï                    | `th`          |
| Tonga                  | `to`          |
| Turc                 | `tr`          |
| Ukrainien               | `uk`          |
| Ourdou                    | `ur`          |
| Vietnamien              | `vi`          |
| Gallois                   | `cy`          |
| Yucatec Maya            | `yua`         |

## <a name="speaker-recognition"></a>Reconnaissance de l’orateur

Consultez le tableau suivant qui répertorie les langues prises en charge pour les différentes API Reconnaissance de l’orateur. Consultez la [vue d’ensemble](speaker-recognition-overview.md) pour plus d’informations sur Reconnaissance de l’orateur.

| Langage | Paramètres régionaux (BCP-47) | Vérification dépendante du texte | Vérification indépendante du texte | Identification indépendante du texte |
|----|----|----|----|----|
|Anglais (US)  |  en-US  |  Oui  |  oui  |  Oui |
|Chinois (mandarin, simplifié) | zh-CN     |     n/a |     oui |     Oui|
|Anglais (Australie)     | en-AU     | n/a     | oui     | Oui|
|Anglais (Canada)     | en-CA     | n/a |     oui |     Oui|
|Anglais (Royaume-Uni)     | en-GB     | n/a     | oui     | Oui|
|Français (Canada)     | fr-CA     | n/a     | oui |     Oui|
|Français (France)     | fr-FR     | n/a     | oui     | Oui|
|Allemand (Allemagne)     | de-DE     | n/a     | oui     | Oui|
|Italien | it-IT     |     n/a     | oui |     Oui|
|Japonais     | ja-JP | n/a     | oui     | Oui|
|Portugais (Brésil) | pt-br |     n/a |     oui |     Oui|
|Espagnol (Mexique)     | es-MX     | n/a |     oui |     Oui|
|Espagnol (Espagne)     | es-ES | n/a     | oui |     oui|

## <a name="custom-keyword-and-keyword-verification"></a>Vérification par mot clé et mot clé personnalisé

Le tableau suivant décrit les langues prises en charge pour la vérification par mot clé et mot clé personnalisé.

| Langage | Paramètres régionaux (BCP-47) | Mot clé personnalisé | Vérification du mot clé |
| -------- | --------------- | -------------- | -------------------- |
| Chinois (mandarin, simplifié) | zh-CN | Oui | Oui |
| Anglais (États-Unis) | fr-FR | Oui | Oui |
| Japonais (Japon) | ja-JP | Non | Oui |
| Portugais (Brésil) | pt-br | Non | Oui |

## <a name="next-steps"></a>Étapes suivantes

* [Créez un compte Azure gratuit](https://azure.microsoft.com/free/cognitive-services/)
* [Découvrir comment utiliser la reconnaissance vocale en C#](./get-started-speech-to-text.md?pivots=programming-language-chsarp)
