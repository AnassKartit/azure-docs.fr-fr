---
title: Configurer l’inscription et la connexion avec un compte Facebook
titleSuffix: Azure AD B2C
description: Proposez l’inscription et la connexion aux clients disposant de comptes Facebook dans vos applications à l’aide d’Azure Active Directory B2C.
services: active-directory-b2c
author: kengaderdus
manager: CelesteDG
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/16/2021
ms.custom: project-no-code
ms.author: kengaderdus
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: d9fb37bd05c6cddf1509d40433783692864e588d
ms.sourcegitcommit: 5361d9fe40d5c00f19409649e5e8fed660ba4800
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2021
ms.locfileid: "130137832"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Configurer l’inscription et la connexion avec un compte Facebook à l’aide d’Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-a-facebook-application"></a>Création d’une application Facebook

Pour autoriser la connexion des utilisateurs avec un compte Facebook dans Azure Active Directory B2C (Azure AD B2C), vous devez créer une application sur le [Tableau de bord des applications Facebook](https://developers.facebook.com/). Pour plus d’informations, consultez l’article [Développement d’applications](https://developers.facebook.com/docs/development).

Si vous n'avez pas encore de compte Facebook, inscrivez-vous sur [https://www.facebook.com](https://www.facebook.com). Une fois inscrit ou connecté avec votre compte Facebook, lancez le [processus d'inscription pour créer un compte de développeur Facebook](https://developers.facebook.com/async/registration). Pour plus d'informations, consultez [S'inscrire en tant que développeur Facebook](https://developers.facebook.com/docs/development/register).

1. Connectez-vous à [Facebook pour les développeurs](https://developers.facebook.com/apps) avec les informations d'identification de votre compte de développeur Facebook.
1. Sélectionnez **Créer une application**.
1. Dans le champ **Sélectionner un type d’application**, sélectionnez **Consommateur**, puis **Suivant**.
1. Entrez un **nom d'affichage** et une **adresse e-mail de contact** valide pour l'application.
1. Sélectionnez **Créer une application**. Au cours de cette étape, vous devrez peut-être accepter les politiques de la plateforme Facebook et effectuer une vérification de sécurité en ligne.
1. Sélectionnez **Paramètres** > **Base**.
    1. Copiez la valeur **ID de l’application**.
    1. Sélectionnez **Afficher**, puis copiez la valeur **Clé secrète de l’application**. Vous avez besoin de ces deux valeurs pour configurer Facebook en tant que fournisseur d’identité dans votre client. **App Secret** est une information d’identification de sécurité importante.
    1. Entrez une URL pour l’**URL de stratégie de confidentialité**, par exemple `https://www.contoso.com/privacy`. L’URL de stratégie est une page que vous tenez à jour pour fournir des informations de confidentialité pour votre application.
    1. Entrez une URL pour l’**URL des conditions d’utilisation du service**, par exemple `https://www.contoso.com/tos`. L’URL de stratégie est une page que vous tenez à jour pour fournir les conditions générales de votre application.
    1. Entrez une URL dans le champ **Suppression des données utilisateur**, par exemple `https://www.contoso.com/delete_my_data`. L'URL de suppression des données utilisateur est une page que vous gérez pour permettre aux utilisateurs de demander la suppression de leurs données. 
    1. Choisissez une **Catégorie**, par exemple `Business and Pages`. Cette valeur est requise par Facebook, mais elle n’est pas utilisée pour Azure AD B2C.
1. Au bas de la page, sélectionnez **Ajouter une plateforme**, puis sélectionnez **Site web**.
1. Dans **URL du site**, entrez l’adresse de votre site web, par exemple `https://contoso.com`. 
1. Sélectionnez **Enregistrer les modifications**.
1. Dans le menu, sélectionnez le signe **plus** ou le lien **Ajouter un produit** à côté de **PRODUITS**. Sous l’option **Ajouter des produits à votre application**, sélectionnez **Configurer** sous **Connexion Facebook**.
1. Dans le menu, sélectionnez **Connexion Facebook**, puis **Paramètres**.
1. Dans **URI de redirection OAuth valides**, entrez `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Si vous utilisez un [domaine personnalisé](custom-domain.md), entrez `https://your-domain-name/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Remplacez `your-tenant-name` par le nom de votre locataire et `your-domain-name` par le nom de votre domaine personnalisé. 
1. Sélectionnez **Enregistrer les modifications** en bas de la page.
1. Pour rendre votre application Facebook disponible sur Azure AD B2C, sélectionnez le sélecteur État dans la partie supérieure droite de la page et **activez-le** pour rendre l’application publique, puis sélectionnez **Changer de mode**. À ce stade, l’état doit passer de **Développement** à **Production**. Pour plus d'informations, consultez [Développement d'applications Facebook](https://developers.facebook.com/docs/development/release).

::: zone pivot="b2c-user-flow"

## <a name="configure-facebook-as-an-identity-provider"></a>Configuration de Facebook comme fournisseur d’identité

1. Connectez-vous au [portail Azure](https://portal.azure.com/) en tant qu’administrateur général de votre locataire Azure AD B2C.
1. Veillez à bien utiliser l’annuaire qui contient votre locataire Azure AD B2C. Sélectionnez l’icône **Répertoires + abonnements** dans la barre d’outils du portail.
1. Sur la page **Paramètres du portail | Répertoires + abonnements**, recherchez votre répertoire AD B2C Azure dans la liste **Nom de répertoire**, puis sélectionnez **Basculer**.
1. Choisissez **Tous les services** dans le coin supérieur gauche du Portail Azure, recherchez et sélectionnez **Azure Active Directory B2C**.
1. Sélectionnez **Fournisseurs d’identité**, puis **Facebook**.
1. Saisissez un **Nom**. Par exemple, *Facebook*.
1. Dans **ID client**, entrez l’ID de l’application Facebook que vous avez créée précédemment.
1. Dans **Clé secrète client**, entrez la clé secrète d’application que vous avez consignée.
1. Sélectionnez **Enregistrer**.

## <a name="add-facebook-identity-provider-to-a-user-flow"></a>Ajouter le fournisseur d’identité Facebook à un flux d’utilisateur 

À ce stade, le fournisseur d’identité Facebook a été configuré, mais il n’est encore disponible dans aucune des pages de connexion. Pour ajouter le fournisseur d’identité Facebook à un flux d’utilisateur :

1. Dans votre locataire Azure AD B2C, sélectionnez **Flux d’utilisateur**.
1. Cliquez sur le flux d’utilisateurs que vous souhaitez ajouter au fournisseur d’identité Facebook.
1. Sous **Fournisseurs d’identité sociale**, sélectionnez **Facebook**.
1. Sélectionnez **Enregistrer**.
1. Pour tester votre stratégie, sélectionnez **Exécuter le flux d’utilisateur**.
1. Pour **Application**, sélectionnez l’application web *testapp1* que vous avez précédemment inscrite. L’**URL de réponse** doit être `https://jwt.ms`.
1. Sélectionnez le bouton **Exécuter le flux d’utilisateur**.
1. Dans la page d’inscription ou de connexion, sélectionnez **Facebook** pour vous connecter avec un compte Facebook.

Si le processus de connexion réussit, votre navigateur est redirigé vers `https://jwt.ms`, qui affiche le contenu du jeton retourné par Azure AD B2C.


::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-policy-key"></a>Création d’une clé de stratégie

Vous devez stocker le secret d’application que vous avez enregistré dans votre tenant Azure AD B2C.

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
1. Veillez à bien utiliser l’annuaire qui contient votre locataire Azure AD B2C. Sélectionnez l’icône **Répertoires + abonnements** dans la barre d’outils du portail.
1. Sur la page **Paramètres du portail | Répertoires + abonnements**, recherchez votre répertoire AD B2C Azure dans la liste **Nom de répertoire**, puis sélectionnez **Basculer**.
1. Choisissez **Tous les services** dans le coin supérieur gauche du portail Azure, puis recherchez et sélectionnez **Azure AD B2C**.
1. Dans la page de vue d’ensemble, sélectionnez **Infrastructure d’expérience d’identité**.
1. Sélectionnez **Clés de stratégie**, puis **Ajouter**.
1. Pour **Options**, choisissez `Manual`.
1. Entrez un **nom** pour la clé de stratégie. Par exemple : `FacebookSecret`. Le préfixe `B2C_1A_` est ajouté automatiquement au nom de votre clé.
1. Dans **Secret**, entrez le secret d’application que vous avez enregistré.
1. Pour **Utilisation de la clé**, sélectionnez `Signature`.
1. Cliquez sur **Créer**.

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Configuration d’un compte Facebook en tant que fournisseur d’identité

1. Dans le fichier `SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`** , remplacez la valeur de `client_id` par l’ID d’application Facebook :

   ```xml
   <TechnicalProfile Id="Facebook-OAUTH">
     <Metadata>
     <!--Replace the value of client_id in this technical profile with the Facebook app ID"-->
       <Item Key="client_id">00000000000000</Item>
   ```

## <a name="upload-and-test-the-policy"></a>Chargez et testez la stratégie.

Mettez à jour le fichier de partie de confiance qui lance le parcours utilisateur que vous avez créé.

1. Chargez le fichier *TrustFrameworkExtensions.xml* sur votre locataire.
1. Sous **Stratégies personnalisées**, sélectionnez **B2C_1A_signup_signin**.
1. Pour **Sélectionner une application**, sélectionnez l’application web *testapp1* que vous avez précédemment inscrite. L’**URL de réponse** doit être `https://jwt.ms`.
1. Sélectionnez le bouton **Exécuter maintenant**.
1. Dans la page d’inscription ou de connexion, sélectionnez **Facebook** pour vous connecter avec un compte Facebook.

Si le processus de connexion réussit, votre navigateur est redirigé vers `https://jwt.ms`, qui affiche le contenu du jeton retourné par Azure AD B2C.

::: zone-end

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment [transmettre un jeton Facebook à votre application](idp-pass-through-user-flow.md).
