---
author: eric-urban
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/22/2020
ms.author: eur
ms.openlocfilehash: 9481d0db9f21b90b38dd89cba028b5fd1059e03f
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/03/2021
ms.locfileid: "131508159"
---
**Interactive**
- Pour les scénarios de commande et de contrôle.
- A une valeur de délai d’expiration de segmentation de X.
- À la fin d’un énoncé reconnu, le service cesse de traiter l’audio de cet ID de demande et termine le tour. La connexion n'est pas fermée.
- La limite maximale de la reconnaissance est de 20 s.
- L’appel Carbon standard à invoquer est `RecognizeOnceAsync`.

**Conversation**
- Pour des reconnaissances plus longues.
- A une valeur de délai d’expiration de segmentation de Y. (Y != X)
- Traite plusieurs énoncés complets sans terminer le tour.
- Termine le tour en raison d’un silence trop long.
- Carbon continuera avec un nouvel ID de demande et une relecture de l’audio si nécessaire.
- Le service se déconnectera de force après 10 minutes de reconnaissance vocale.
- Carbon se reconnectera et relira l’audio non confirmé.
- Appelée dans Carbon avec `StartContinuousRecognition`.

**Dictée**
- Permet aux utilisateurs de spécifier la ponctuation oralement.
- Appelée dans Carbon en spécifiant `EnableDictation` sur l’objet `SpeechConfig` indépendamment de l’appel d’API qui démarre la reconnaissance.
- Le cluster interne <sup></sup>renvoie des messages `speech.fragment` pour les résultats intermédiaires, tandis que le cluster tiers <sup></sup>renvoie des messages `speech.hypothesis`.
