---
title: Inscrire des applications mobiles appelant des API web | Azure
titleSuffix: Microsoft identity platform
description: Découvrez comment créer une application mobile qui appelle des API web (inscription d’application)
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 08/18/2021
ms.author: jmprieur
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: dba6dcf0dded3db4c1cf1ddc26071e9803b55cd6
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/29/2021
ms.locfileid: "129232914"
---
# <a name="register-mobile-apps-that-call-web-apis"></a>Inscrire des applications mobiles appelant des API web

Cet article contient des instructions pour vous aider à inscrire une application mobile que vous créez.

## <a name="supported-account-types"></a>Types de comptes pris en charge

Les types de comptes pris en charge par vos applications mobiles dépendent de l'expérience que vous souhaitez activer et des flux que vous souhaitez utiliser.

### <a name="audience-for-interactive-token-acquisition"></a>Audience pour l’acquisition de jetons interactive

La plupart des applications mobiles utilisent l’authentification interactive. Si votre application utilise ce formulaire d'authentification, vous pouvez connecter des utilisateurs depuis n'importe quel [type de compte](quickstart-register-app.md).

### <a name="audience-for-integrated-windows-authentication-username-password-and-b2c"></a>Audience pour Authentification Windows intégrée, nom d’utilisateur-mot de passe et B2C

Si vous disposez d’une application UWP (plateforme Windows universelle), vous pouvez utiliser Authentification Windows intégrée (IWA) pour connecter les utilisateurs. Pour utiliser l’Authentification Windows intégrée ou l’authentification par nom d’utilisateur/mot de passe, votre application doit connecter les utilisateurs dans le locataire de votre développeur d’application métier. Dans le cadre d'un scénario d'éditeur de logiciels indépendant (ISV), votre application peut connecter des utilisateurs dans des organisations Azure Active Directory. Ces flux d’authentification ne sont pas pris en charge pour des comptes Microsoft personnels.

Vous pouvez également connecter des utilisateurs à l'aide d'identités sociales qui transmettent une stratégie et une autorité B2C. Pour utiliser cette méthode, vous pouvez uniquement utiliser l'authentification interactive et l'authentification par nom d'utilisateur/mot de passe. Pour le moment, l'authentification par nom d'utilisateur/mot de passe est uniquement prise en charge sur Xamarin.iOS, Xamarin.Android et UWP.

Pour plus d'informations, consultez [Scénarios et flux d'authentification pris en charge](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows) et [Scénarios et plateformes/langues prises en charge](authentication-flows-app-scenarios.md#scenarios-and-supported-platforms-and-languages)

## <a name="platform-configuration-and-redirect-uris"></a>Configuration de la plateforme et URI de redirection

### <a name="interactive-authentication"></a>Authentification interactive

Lorsque vous créez une application mobile qui utilise l'authentification interactive, l'étape d'inscription la plus critique est celle de l'URI de redirection. Vous pouvez définir l'authentification interactive via la [configuration de la plateforme sur le panneau **Authentification**](https://aka.ms/MobileAppReg).

Cette expérience permet à votre application d'obtenir une authentification unique (SSO) au moyen de Microsoft Authenticator (et du portail d'entreprise Intune sur Android). Elle prend également en charge les stratégies de gestion des périphériques.

Le portail d'inscription des applications propose une expérience en préversion afin de vous aider à calculer l'URI de réponse répartie pour les applications iOS et Android :

1. Sur le portail d'inscription des applications, sélectionnez **Authentification** > **Essayer la nouvelle expérience**.

   ![Panneau Authentification permettant de choisir une nouvelle expérience](https://user-images.githubusercontent.com/13203188/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

2. Sélectionnez **Ajouter une plateforme**.

   ![Ajouter une plateforme](https://user-images.githubusercontent.com/13203188/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

3. Lorsque la liste des plateformes est prise en charge, sélectionnez **iOS**.

   ![Choisir une application mobile](https://user-images.githubusercontent.com/13203188/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

4. Entrez l'ID de l'offre groupée, puis sélectionnez **Inscrire**.

   ![Entrer l'ID de l'offre groupée](https://user-images.githubusercontent.com/13203188/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

Au terme de ces étapes, l'URI de redirection est calculé pour vous, comme illustré ci-dessous.

![URI de redirection obtenu](https://user-images.githubusercontent.com/13203188/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

Si vous préférez configurer manuellement l'URI de redirection, utilisez le manifeste de l'application. Voici le format recommandé pour le manifeste :

- **iOS** : `msauth.<BUNDLE_ID>://auth`
  - Par exemple, entrez `msauth.com.yourcompany.appName://auth`.
- **Android** : `msauth://<PACKAGE_NAME>/<SIGNATURE_HASH>`
  - Vous pouvez générer le hachage de la signature Android à l'aide de la clé de mise en production ou de la clé de débogage par le biais de la commande KeyTool.

### <a name="username-password-authentication"></a>Authentification par nom d'utilisateur/mot de passe

Si votre application utilise exclusivement l'authentification par nom d'utilisateur/mot de passe, il n'est pas nécessaire d'inscrire un URI de redirection pour votre application. Ce flux effectue un aller-retour vers la plateforme d'identités Microsoft. Votre application ne sera pas rappelée sur un URI spécifique.

Identifiez toutefois votre application en tant qu'application cliente publique. Pour ce faire :

1. Toujours dans le <a href="https://portal.azure.com/" target="_blank">portail Azure</a>, sélectionnez votre application dans **Inscriptions d’applications**, puis sélectionnez **Authentification**.
1. Dans **Paramètres avancés** > **Autoriser les flux de clients publics** > **Activer les flux mobiles et de bureau suivants**, sélectionnez **Oui**.

   :::image type="content" source="media/scenarios/default-client-type.png" alt-text="Activer le paramètre client public dans le volet Authentification du portail Azure":::

## <a name="api-permissions"></a>Autorisations des API

Les applications mobiles appellent des API pour le compte de l’utilisateur connecté. Votre application doit demander des autorisations déléguées. Ces autorisations sont également appelées des étendues. Selon l'expérience dont vous souhaitez bénéficier, vous pouvez demander des autorisations déléguées de manière statique via le portail Azure. Ou vous pouvez les demander de manière dynamique lors de l'exécution.

En inscrivant les autorisations de manière statique, vous permettez aux administrateurs d'approuver facilement votre application. L'inscription statique est recommandée.

## <a name="next-steps"></a>Étapes suivantes

Passez à l’article suivant de ce scénario, [Configuration du code de l’application](scenario-mobile-app-configuration.md).
