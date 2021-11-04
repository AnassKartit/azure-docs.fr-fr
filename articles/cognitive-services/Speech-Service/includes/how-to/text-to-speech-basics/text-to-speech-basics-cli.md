---
author: eric-urban
ms.service: cognitive-services
ms.topic: include
ms.date: 08/11/2020
ms.author: eur
ms.openlocfilehash: 0bc81935e597198ed1c65a732ae1feaee7aa6226
ms.sourcegitcommit: 2cc9695ae394adae60161bc0e6e0e166440a0730
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/03/2021
ms.locfileid: "131510927"
---
Dans ce guide de démarrage rapide, vous découvrez les modèles de conception courants qui permettent d’utiliser la synthèse vocale au moyen du kit SDK Speech. Vous commencez par créer une configuration et une synthèse de base, puis passez à des exemples plus poussés en matière de développement d’applications personnalisées, notamment :

* Obtention de réponses en tant que flux en mémoire
* Personnalisation du taux d’échantillonnage de sortie et de la vitesse de transmission
* Envoi de demandes de synthèse à l’aide de SSML (Speech Synthesis Markup Language)
* Utilisation de voix neurales

## <a name="prerequisites"></a>Prérequis

Cet article part du principe que vous disposez d’un compte Azure et d’un abonnement au service Speech. Si vous n’avez pas de compte et d’abonnement, [essayez le service Speech gratuitement](../../../overview.md#try-the-speech-service-for-free).

[!INCLUDE [SPX Setup](../../spx-setup.md)]

## <a name="synthesize-speech-to-a-speaker"></a>Synthétiser la voix vers un haut-parleur

Vous êtes maintenant prêt à exécuter l’interface CLI Speech pour synthétiser la voix à partir du texte. À partir de la ligne de commande, accédez au répertoire qui contient le fichier binaire de l'interface CLI Speech. Exécutez ensuite la commande suivante.

```bash
spx synthesize --text "The speech synthesizer greets you!"
```

L’interface CLI Speech génère un langage naturel en anglais par le biais du haut-parleur de l’ordinateur.

## <a name="synthesize-speech-to-a-file"></a>Synthétiser la voix dans un fichier

Exécutez la commande suivante pour convertir la sortie de votre haut-parleur en fichier `.wav`.

```bash
spx synthesize --text "The speech synthesizer greets you!" --audio output greetings.wav
```

L’interface CLI Speech génère un langage naturel en anglais dans le fichier audio `greetings.wav`.
Dans Windows, vous pouvez lire le fichier audio en entrant `start greetings.wav`.
