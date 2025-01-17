---
title: Créer un hub de notification Azure à l’aide du portail Azure | Microsoft Docs
description: Dans ce tutoriel, vous allez apprendre à créer un hub de notification Azure à l’aide du portail Azure.
services: notification-hubs
author: sethmanheim
ms.author: sethm
manager: femila
ms.reviewer: thsomasu
ms.date: 08/04/2020
ms.lastreviewed: 02/14/2019
ms.topic: quickstart
ms.service: notification-hubs
ms.workload: mobile
ms.custom: mode-portal
ms.openlocfilehash: 1f9ba3f32a2b7f5b0845593ec3439cf826b50ce6
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131070125"
---
# <a name="quickstart-create-an-azure-notification-hub-in-the-azure-portal"></a>Démarrage rapide : Créer un hub de notification Azure dans le portail Azure

Azure Notification Hubs fournit un moteur d’envoi de notifications Push facile à utiliser et à grande échelle qui vous permet d’envoyer des notifications à n’importe quelle plateforme (iOS, Android, Windows, Kindle, Baidu, etc.) à partir de n’importe quel serveur principal (cloud ou local). Pour plus d’informations sur le service, consultez [Présentation d’Azure Notification Hubs](notification-hubs-push-notification-overview.md).

Dans ce guide de démarrage rapide, vous créez un hub de notification dans le portail Azure. La première section vous présente les étapes pour créer un espace de noms Notification Hubs et un hub dans cet espace de noms. La deuxième section vous présente les étapes pour créer un hub de notification dans un espace de noms Notification Hubs existant.

## <a name="create-a-namespace-and-a-notification-hub"></a>Créer un espace de noms et un hub de notification

Dans cette section, vous créez un espace de noms et un hub dans l’espace de noms.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

## <a name="create-a-notification-hub-in-an-existing-namespace"></a>Créer un hub de notification dans un espace de noms existant

Dans cette section, vous créez un hub de notification dans un espace de noms existant.

1. Connectez-vous au [portail Azure](https://portal.azure.com).
2. Sélectionnez **Tous les services** dans le menu de gauche, recherchez **Hub de notification**, sélectionnez l’**étoile** (`*`) en regard d’**Espaces de noms du hub de notification** pour l’ajouter à la section **FAVORIS** dans le menu de gauche. Sélectionnez **Espaces de noms du hub de notification**.

      ![Portail Azure - sélectionner Espaces de noms du hub de notification](./media/create-notification-hub-portal/select-notification-hub-namespaces-all-services.png)
3. Dans la page **Espaces de noms du hub de notification**, sélectionnez votre espace de noms dans la liste.

      ![Sélectionner votre espace de noms dans la liste](./media/create-notification-hub-portal/select-namespace.png)
4. Dans la page **Espaces de noms du hub de notification**, sélectionnez **Ajouter un hub** dans la barre d’outils.

      ![Espaces de noms du hub de notification - bouton Ajouter un hub](./media/create-notification-hub-portal/add-hub-button.png)
5. Dans la page **Nouveau hub de notification**, entrez un nom pour le hub de notification, puis sélectionnez **OK**.

      ![Page Nouveau hub de notification -> entrer un nom pour votre hub](./media/create-notification-hub-portal/new-notification-hub-page.png)
6. Sélectionnez **Notifications** (icône de cloche) en haut pour voir l’état du déploiement du nouveau hub. Sélectionnez **X** dans l’angle droit pour fermer la fenêtre de notification.

      ![Notification de déploiement](./media/create-notification-hub-portal/deployment-notification.png)
7. Actualisez la page web **Espaces de noms du hub de notification** pour voir votre nouveau hub dans la liste.

      ![Capture d’écran qui montre la page web Espaces de noms du hub de notification avec le nouveau hub dans la liste.](./media/create-notification-hub-portal/new-hub-in-list.png)
8. Sélectionnez votre **hub de notification** pour afficher sa page d’accueil.

      ![Capture d’écran qui montre la page d’accueil de votre hub de notification.](./media/create-notification-hub-portal/hub-home-page.png)

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez créé un hub de notification. Pour savoir comment configurer le hub avec les paramètres PNS (Platform Notification System), consultez [Configurer un hub de notification avec les paramètres PNS](configure-notification-hub-portal-pns-settings.md).
