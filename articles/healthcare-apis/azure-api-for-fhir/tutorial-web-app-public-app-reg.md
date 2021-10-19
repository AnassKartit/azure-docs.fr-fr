---
title: Didacticiel d’application Web-installation d’application cliente-API Azure pour FHIR
description: Ce tutoriel vous accompagne tout au long des étapes d’inscription d’une application publique en vue de préparer le déploiement d’une application web
services: healthcare-apis
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: tutorial
ms.reviewer: matjazl
ms.author: cavoeg
author: caitlinv39
ms.date: 01/03/2020
ms.openlocfilehash: 438ed15df54aefe299cbda0c45fe168f5857c93c
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "121784691"
---
# <a name="client-application-registration-for-azure-api-for-fhir"></a>Inscription de l’application cliente pour l’API Azure pour FHIR
Dans le tutoriel précédent, vous avez déployé et configuré votre API Azure pour FHIR. Maintenant que votre API Azure pour FHIR est configurée, nous allons inscrire une application cliente publique. Vous pouvez parcourir entièrement le guide pratique qui explique comment [inscrire une application cliente publique](register-public-azure-ad-client-app.md) ou comment résoudre les problèmes, mais nous en avons extrait les principales étapes dans le tutoriel ci-dessous.

1. Accéder à Azure Active Directory
1. Sélectionnez **Inscription des applications** --> **Nouvelle inscription**.
1. Nommer votre application
1. Sélectionnez **Client public/natif (Bureau et mobile)** et définissez l’URI de redirection sur `https://www.getpostman.com/oauth2/callback`.

   :::image type="content" source="media/tutorial-web-app/register-public-app.png" alt-text="Capture d’écran du volet Inscrire une application avec des exemples de nom d’application et d’URL de redirection":::

## <a name="client-application-settings"></a>Paramètres de l’application cliente

Une fois l’application cliente inscrite, copiez l’ID d’application (client) et l’ID de locataire à partir de la page Vue d’ensemble. Vous aurez besoin de ces deux valeurs par la suite, quand vous accéderez au client.

:::image type="content" source="media/tutorial-web-app/client-id-tenant-id.png" alt-text="Capture d’écran du volet de paramètres de l’application cliente avec les ID d’application et d’annuaire mis en évidence":::

### <a name="connect-with-web-app"></a>Se connecter à une application web

Si vous avez [écrit votre application web](tutorial-web-app-write-web-app.md) pour qu’elle se connecte à l’API Azure pour FHIR, vous devez également définir les options d’authentification correctes. 

1. Dans le menu de gauche, sous **Gérer**, sélectionnez **Authentification**. 

1. Pour ajouter une nouvelle configuration de plateforme, sélectionnez **Web**.

1. Configurez l’URI de redirection en préparation de la création de votre application web dans la quatrième partie de ce tutoriel. Pour cela, ajoutez `https://\<WEB-APP-NAME>.azurewebsites.net` à la liste des URI de redirection. Si vous avez choisi un nom différent au moment d’[écrire votre application web](tutorial-web-app-write-web-app.md), vous devez revenir en arrière et le mettre à jour.

1. Activez les cases à cocher **Jeton d’accès** et **Jeton d’ID**.

   :::image type="content" source="media/tutorial-web-app/web-app-authentication.png" alt-text="Capture d’écran du panneau de paramètres d’authentification d’application avec les étapes d’ajout de plateforme mises en évidence":::

## <a name="add-api-permissions"></a>Ajouter des autorisations d’API

Vous avez configuré l’authentification appropriée. Vous allez à présent définir les autorisations de l’API :

1. Sélectionnez **Autorisations de l’API**, puis cliquez sur **Ajouter une autorisation**.
1. Sous **API utilisées par mon organisation**, effectuez une recherche sur Azure Healthcare APIs.
1. Sélectionnez **user_impersonation**, puis cliquez sur **Ajouter des autorisations**.

:::image type="content" source="media/tutorial-web-app/api-permissions.png" alt-text="Capture d’écran du panneau d’ajout d’autorisations d’API avec les étapes d’ajout d’autorisations d’API mises en évidence":::

## <a name="next-steps"></a>Étapes suivantes
Vous disposez à présent d’une application cliente publique. Dans le prochain tutoriel, nous allons voir en détail comment tester et accéder à cette application via Postman.

>[!div class="nextstepaction"]
>[Tester l’application cliente dans Postman](tutorial-web-app-test-postman.md)
