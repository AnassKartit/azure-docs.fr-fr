---
title: Configurer l’appareil Azure Percept Audio
description: Découvrez comment connecter votre appareil Azure Percept Audio au DK Azure Percept
author: MrHamlet
ms.author: amiyouss
ms.service: azure-percept
ms.topic: quickstart
ms.date: 03/25/2021
ms.openlocfilehash: 8b6ec3ba8473be8e924d3c4b8ae17ccbdcf6568f
ms.sourcegitcommit: 40866facf800a09574f97cc486b5f64fced67eb2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2021
ms.locfileid: "123221834"
---
# <a name="set-up-the-azure-percept-audio-device"></a>Configurer l’appareil Azure Percept Audio

Azure Percept Audio est prêt à l’emploi avec le DK Azure Percept. Aucune configuration unique n’est nécessaire.

## <a name="prerequisites"></a>Prérequis

- DK (devkit) Azure Percept
- Azure Percept Audio
- [Abonnement Azure](https://azure.microsoft.com/free/)
- [Expérience d’installation du DK Azure Percept](./quickstart-percept-dk-set-up.md) : vous avez connecté votre devkit à un réseau Wi-Fi, créé un hub IoT et connecté votre devkit au hub IoT.
- Haut-parleur ou casque qui peut être branché à une prise audio de 3,5 mm (facultatif)

## <a name="connecting-your-devices"></a>Connexion de vos appareils

1. Raccordez l’appareil Azure Percept Audio à la carte de base d’Azure Percept DK à l’aide du câble Micro USB vers USB Type A inclus. Connectez l’extrémité du câble Micro USB à la carte d’interposeur (développeur) audio, et l’extrémité du câble Type A à la carte de base Percept DK.

1. (Facultatif) Branchez votre haut-parleur ou votre casque sur l’appareil Azure Percept Audio en utilisant la prise audio étiquetée « Line Out » (sortie de ligne). De cette façon, vous pourrez entendre les réponses audio.

1. Mettez le kit de développement sous tension en le connectant à l’adaptateur d’alimentation. La LED L02 passe au blanc clignotant pour indiquer que l’appareil a été mis sous tension et qu’il s’authentifie.

1. Attendez que le processus d’authentification se termine, ce qui prend jusqu’à 5 minutes.

1. Vous pouvez commencer le prototypage quand vous voyez l’un des états suivants de la LED :

    - La LED L02 passe au blanc fixe, indiquant que l’authentification est terminée et que le devkit est configuré sans mot clé.
    - Les trois LED deviennent bleues, indiquant que l’authentification est terminée et que le devkit a été configuré avec un mot clé.

## <a name="next-steps"></a>Étapes suivantes

Créez une [solution de reconnaissance vocale sans code](./tutorial-no-code-speech.md) dans [Azure Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819).
