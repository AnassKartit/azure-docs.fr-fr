---
title: Connexion à Windows Virtual Desktop (classique) via Android – Azure
description: Comment se connecter à Windows Virtual Desktop (classique) à l’aide du client Android.
author: Heidilohr
ms.topic: how-to
ms.date: 03/30/2020
ms.author: helohr
manager: femila
ms.openlocfilehash: 7de94bdd28b51b87b3b06d62685dcc1921a1c6f3
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/06/2021
ms.locfileid: "106445293"
---
# <a name="connect-to-windows-virtual-desktop-classic-with-the-android-client"></a>Comment se connecter à Windows Virtual Desktop (classique) à l’aide du client Android.

> S’applique à : Android 4.1 et versions ultérieures, Chromebooks avec ChromeOS 53 et versions ultérieures.

>[!IMPORTANT]
>Ce contenu s’applique à Windows Virtual Desktop (classique), qui ne prend pas en charge les objets Windows Virtual Desktop Azure Resource Manager. Si vous essayez de gérer des objets Windows Virtual Desktop Azure Resource Manager, consultez [cet article](../connect-android.md).

Vous pouvez accéder aux ressources Windows Virtual Desktop à partir de votre appareil Android avec notre client téléchargeable. Vous pouvez également utiliser le client Android sur des appareils Chromebook qui prennent en charge le Google Play Store. Ce guide explique comment configurer le client Android.

## <a name="install-the-android-client"></a>Installer le client Android

Pour commencer, [téléchargez](https://play.google.com/store/apps/details?id=com.microsoft.rdc.androidx) et installez le client sur votre appareil Android.

## <a name="subscribe-to-a-feed"></a>S’abonner à un flux

Abonnez-vous au flux fourni par votre administrateur pour obtenir la liste des ressources managées auxquelles vous pouvez accéder sur votre appareil Android.

Pour s’abonner à un flux :

1. Dans Connection Center, appuyez sur **+** , puis sur **Remote Resource Feed** (Flux de ressources à distance).
2. Entrez l’URL du flux dans le champ **Feed URL** (URL du flux). Il peut s’agir d’une URL ou d’une adresse e-mail.
   - Si vous utilisez une URL, choisissez celle que votre administrateur vous a donnée, normalement <https://rdweb.wvd.microsoft.com>.
   - Pour utiliser l’e-mail, entrez votre adresse e-mail. Le client recherche une URL associée à votre adresse de adresse e-mail si votre administrateur a configuré le serveur de cette façon.
3. Appuyez sur **SUIVANT**.
4. À l’invite, indiquez vos informations d’identification.
   - Pour **Nom d’utilisateur**, indiquez le nom de l’utilisateur ayant l’autorisation d’accéder aux ressources.
   - Pour **Mot de passe**, indiquez le mot de passe associé au nom d’utilisateur.
   - Vous pouvez également être invité à fournir des facteurs supplémentaires si votre administrateur a configuré l’authentification de cette manière.

Une fois l’abonnement souscrit, le Connection Center doit afficher les ressources distantes.

Une fois que vous êtes abonné à un flux, son contenu est mis à jour automatiquement et régulièrement. Des ressources peuvent être ajoutées, modifiées ou supprimées en fonction des modifications apportées par votre administrateur.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur l’utilisation du client Android, voir [Prise en main du client Android](/windows-server/remote/remote-desktop-services/clients/remote-desktop-android/).