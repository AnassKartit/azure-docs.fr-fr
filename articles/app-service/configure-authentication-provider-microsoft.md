---
title: Configurer l’authentification Microsoft
description: Découvrez comment configurer l’authentification avec un compte Microsoft pour en faire un fournisseur d’identité pour votre application App Service ou Azure Functions.
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.topic: article
ms.date: 03/29/2021
ms.custom:
- seodec18
- fasttrack-edit
ms.openlocfilehash: 5e7b4c7ed1950b2fa585639a3097cc1f5688d739
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106077934"
---
# <a name="configure-your-app-service-or-azure-functions-app-to-use-microsoft-account-login"></a>Configurer votre application App Service ou Azure Functions pour utiliser une connexion de compte Microsoft

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Cette rubrique montre comment configurer Azure App Service ou Azure Functions afin d’utiliser AAD pour prendre en charge les connexions de comptes Microsoft personnels.

> [!IMPORTANT]
> Bien que le fournisseur de comptes Microsoft soit toujours pris en charge, il est recommandé d’utiliser à la place les applications qui utilisent le [fournisseur de plateforme d’identité Microsoft (Azure AD)](./configure-authentication-provider-aad.md). La plateforme d’identité Microsoft fournit une prise en charge des comptes professionnels et des comptes Microsoft personnels.

## <a name="register-your-app-with-microsoft-account"></a><a name="register-microsoft-account"> </a>Inscription de votre application avec un compte Microsoft

1. Accédez à [**Inscriptions d’applications**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) dans le portail Azure. Si besoin, connectez-vous avec votre compte Microsoft.
1. Sélectionnez **Nouvelle inscription**, puis entrez un nom d’application.
1. Sous **Types de comptes pris en charge**, sélectionnez **Comptes dans un annuaire organisationnel (tout annuaire Azure AD mutualisé) et comptes personnels Microsoft (par exemple, Skype, Xbox)**
1. Dans **URI de redirection**, sélectionnez **Web**, puis entrez `https://<app-domain-name>/.auth/login/aad/callback`. Remplacez *\<app-domain-name>* par le nom de votre application de votre application.  Par exemple : `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Veillez à utiliser le schéma HTTPS dans l’URL.

1. Sélectionnez **Inscription**.
1. Copier l’**ID d’application (client)** . Vous en aurez besoin ultérieurement.
1. Dans le volet gauche, sélectionnez **Certificats et secrets** > **Nouvelle clé secrète client**. Entrez une description, sélectionnez la durée de validité, puis sélectionnez **Ajouter**.
1. Copiez la valeur qui apparaît dans la page **Certificats et secrets**. Une fois que vous avez quitté la page, elle ne s’affiche plus.

    > [!IMPORTANT]
    > La valeur de la clé secrète client (mot de passe) est une information d’identification de sécurité importante. Ne partagez le mot de passe avec personne et ne le distribuez pas dans une application cliente.

## <a name="add-microsoft-account-information-to-your-app-service-application"></a><a name="secrets"> </a>Ajout des informations de compte Microsoft à votre application App Service

1. Accédez à votre application dans le [Azure portal].
1. Sélectionnez **Paramètres** > **Authentification/Autorisation**, puis vérifiez que **l’authentification App Service** est activée, sur **On**.
1. Sous **Fournisseurs d’authentification**, cliquez sur **Azure Active Directory**. Sélectionnez **Avancé** sous **Mode d’administration**. Collez l’ID d’application (client) et la clé secrète client que vous avez obtenus précédemment. Utilisez **`https://login.microsoftonline.com/9188040d-6c67-4c5b-b112-36a304b66dad/v2.0`** pour le champ **URL de l’émetteur**.
1. Sélectionnez **OK**.

   App Service fournit une authentification, mais ne restreint pas l’accès autorisé au contenu et aux API de votre site. Vous devez autoriser les utilisateurs dans votre code d'application.

1. (Facultatif) Pour restreindre l’accès aux utilisateurs de compte Microsoft, définissez **Action à exécuter quand une demande n’est pas authentifiée** sur **Se connecter avec Azure Active Directory**. Quand vous définissez cette fonctionnalité, votre application exige que toutes les demandes soient authentifiées. Elle redirige également toutes les demandes non authentifiées de façon à utiliser pour l’authentification. Comme vous avez configuré l’**URL de l’émetteur** de façon à utiliser un locataire de compte Microsoft, seuls des comptes personnels seront correctement authentifiés.

   > [!CAUTION]
   > Cette manière de restreindre l’accès s’applique à tous les appels à votre application qui peuvent ne pas être souhaitables pour les applications qui ont une page d’accès publique disponible, comme dans de nombreuses applications monopages. Pour de telles applications, préférez **Autoriser les requêtes anonymes (aucune action)** . L’application démarre alors elle-même manuellement l’authentification. Pour plus d’informations, consultez [Flux d’authentification](overview-authentication-authorization.md#authentication-flow).

1. Sélectionnez **Enregistrer**.

Vous êtes maintenant prêt à utiliser un compte Microsoft pour l’authentification dans votre application.

## <a name="next-steps"></a><a name="related-content"> </a>Étapes suivantes

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portal]: https://portal.azure.com/
