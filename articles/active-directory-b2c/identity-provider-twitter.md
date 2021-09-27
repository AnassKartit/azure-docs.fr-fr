---
title: Configurer l’inscription et la connexion avec un compte Twitter
titleSuffix: Azure AD B2C
description: Proposez l’inscription et la connexion aux clients disposant d’un compte Twitter dans vos applications avec Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/16/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: fb345a170f32cdf33ef069d4944a0d8d28aad631
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128571862"
---
# <a name="set-up-sign-up-and-sign-in-with-a-twitter-account-using-azure-active-directory-b2c"></a>Configurer l’inscription et la connexion avec un compte Twitter à l’aide d’Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]
::: zone pivot="b2c-custom-policy"

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

::: zone-end

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-an-application"></a>Créer une application

Pour permettre aux utilisateurs disposant d'un compte Twitter dans Azure AD B2C de se connecter, vous devez créer une application Twitter. Si vous ne disposez pas encore d’un compte Twitter, vous pouvez en créer un à l’adresse [https://twitter.com/signup](https://twitter.com/signup). Vous devez également [Effectuer une demande de compte de développeur](https://developer.twitter.com/en/apply/user.html). Pour plus d'informations, consultez [Demande d'accès](https://developer.twitter.com/en/apply-for-access).

1. Connectez-vous au [Portail des développeurs Twitter](https://developer.twitter.com/portal/projects-and-apps) avec les informations d'identification de votre compte Twitter.
1. Sous **Applications autonomes**, sélectionnez **+ Créer une application**.
1. Entrez un **Nom d'application**, puis sélectionnez **Terminé**.
1. Copiez les valeurs de la **Clé d'application** et de la **Clé secrète API**.  Vous utiliserez ces deux valeurs pour configurer Twitter en tant que fournisseur d'identité dans votre locataire. 
1. Sous **Configurer votre application**, sélectionnez **Paramètres de l'application**.
1. Sous **Paramètres d'authentification**, sélectionnez **Modifier**.
    1. Cochez la case **Activer l'authentification OAuth à 3 branches**.
    1. Cochez la case **Demander une adresse e-mail aux utilisateurs**.
    1. Dans le champ **URL de rappel**, entrez `https://your-tenant.b2clogin.com/your-tenant-name.onmicrosoft.com/your-user-flow-Id/oauth1/authresp`.  Si vous utilisez un [domaine personnalisé](custom-domain.md), entrez `https://your-domain-name/your-tenant-name.onmicrosoft.com/your-user-flow-Id/oauth1/authresp`. Lorsque vous entrez le nom de votre locataire et l'ID du flux d'utilisateurs, n’utilisez que des minuscules, même s'ils sont en majuscules dans Azure AD B2C. Remplacez :
        - `your-tenant-name` par le nom de votre locataire.
        - `your-domain-name` par votre domaine personnalisé.
        - `your-user-flow-Id` par l’identificateur de votre flux d’utilisateur. Par exemple : `b2c_1a_signup_signin_twitter`. 
    
    1. Dans le champ **URL du site web**, entrez `https://your-tenant.b2clogin.com`. Remplacez `your-tenant` par le nom de votre locataire. Par exemple : `https://contosob2c.b2clogin.com`. Si vous utilisez un [domaine personnalisé](custom-domain.md), entrez `https://your-domain-name`.
    1. Entrez une URL pour les **Conditions d'utilisation du service**, par exemple `http://www.contoso.com/tos`. L’URL de stratégie est une page que vous tenez à jour pour fournir les conditions générales de votre application.
    1. Entrez une URL pour la **Politique de confidentialité**, par exemple `http://www.contoso.com/privacy`. L’URL de stratégie est une page que vous tenez à jour pour fournir des informations de confidentialité pour votre application.
    1. Sélectionnez **Enregistrer**.

::: zone pivot="b2c-user-flow"

## <a name="configure-twitter-as-an-identity-provider"></a>Configurer Twitter en tant que fournisseur d'identité

1. Connectez-vous au [portail Azure](https://portal.azure.com/) en tant qu’administrateur général de votre locataire Azure AD B2C.
1. Veillez à bien utiliser l’annuaire qui contient votre locataire Azure AD B2C. Sélectionnez l’icône **Répertoires + abonnements** dans la barre d’outils du portail.
1. Sur la page **Paramètres du portail | Répertoires + abonnements**, recherchez votre répertoire AD B2C Azure dans la liste **Nom de répertoire**, puis sélectionnez **Basculer**.
1. Choisissez **Tous les services** dans le coin supérieur gauche du Portail Azure, recherchez et sélectionnez **Azure Active Directory B2C**.
1. Sélectionnez **Fournisseurs d’identité**, puis **Twitter**.
1. Saisissez un **Nom**. Par exemple, *Twitter*.
1. Dans le champ **ID client**, entrez la *Clé API* de l'application Twitter que vous avez créée précédemment.
1. Dans le champ **Clé secrète client**, entrez la *Clé secrète API* que vous avez consignée.
1. Sélectionnez **Enregistrer**.

## <a name="add-twitter-identity-provider-to-a-user-flow"></a>Ajouter un fournisseur d’identité Twitter à un workflow d’utilisateur 

À ce stade, le fournisseur d’identité Twitter a été configuré, mais il n’est encore disponible dans aucune des pages de connexion. Pour ajouter le fournisseur d’identité Twitter à un flux d’utilisateur :

1. Dans votre locataire Azure AD B2C, sélectionnez **Flux d’utilisateur**.
1. Sélectionnez le flux d'utilisateurs que vous souhaitez ajouter au fournisseur d'identité Twitter.
1. Sous **Fournisseurs d’identité sociale**, sélectionnez **Twitter**.
1. Sélectionnez **Enregistrer**.
1. Pour tester votre stratégie, sélectionnez **Exécuter le flux d’utilisateur**.
1. Pour **Application**, sélectionnez l’application web *testapp1* que vous avez précédemment inscrite. L’**URL de réponse** doit être `https://jwt.ms`.
1. Sélectionnez le bouton **Exécuter le flux d’utilisateur**.
1. À partir de la page d’inscription ou de connexion, sélectionnez **Twitter** pour vous connecter avec un compte Twitter.

Si le processus de connexion réussit, votre navigateur est redirigé vers `https://jwt.ms`, qui affiche le contenu du jeton retourné par Azure AD B2C.

::: zone-end

::: zone pivot="b2c-custom-policy"

## <a name="create-a-policy-key"></a>Création d’une clé de stratégie

Vous devez stocker la clé secrète que vous avez enregistré dans votre locataire Azure AD B2C.

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
1. Veillez à bien utiliser l’annuaire qui contient votre locataire Azure AD B2C. Sélectionnez l’icône **Répertoires + abonnements** dans la barre d’outils du portail.
1. Sur la page **Paramètres du portail | Répertoires + abonnements**, recherchez votre répertoire AD B2C Azure dans la liste **Nom de répertoire**, puis sélectionnez **Basculer**.
1. Choisissez **Tous les services** dans le coin supérieur gauche du portail Azure, puis recherchez et sélectionnez **Azure AD B2C**.
1. Dans la page de vue d’ensemble, sélectionnez **Infrastructure d’expérience d’identité**.
1. Sélectionnez **Clés de stratégie**, puis **Ajouter**.
1. Pour **Options**, choisissez `Manual`.
1. Entrez un **nom** pour la clé de stratégie. Par exemple : `TwitterSecret`. Le préfixe `B2C_1A_` est ajouté automatiquement au nom de votre clé.
1. Dans **Secret**, entrez la clé secrète client que vous avez enregistrée.
1. Pour **Utilisation de la clé**, sélectionnez `Encryption`.
1. Cliquez sur **Créer**.

## <a name="configure-twitter-as-an-identity-provider"></a>Configurer Twitter en tant que fournisseur d'identité

Pour permettre aux utilisateurs de se connecter à l'aide d'un compte Twitter, vous devez définir le compte en tant que fournisseur de revendications avec lequel Azure AD B2C peut communiquer par le biais d'un point de terminaison. Le point de terminaison fournit un ensemble de revendications utilisées par Azure AD B2C pour vérifier qu’un utilisateur spécifique s’est authentifié.

Vous pouvez définir un compte Twitter en tant que fournisseur de revendications en l’ajoutant à l’élément **ClaimsProviders** dans le fichier d’extension de votre stratégie.

1. Ouvrez le fichier *TrustFrameworkExtensions.xml*.
2. Recherchez l’élément **ClaimsProviders**. S’il n’existe pas, ajoutez-le sous l’élément racine.
3. Ajoutez un nouveau **ClaimsProvider** comme suit :

    ```xml
    <ClaimsProvider>
      <Domain>twitter.com</Domain>
      <DisplayName>Twitter</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Twitter-OAuth1">
          <DisplayName>Twitter</DisplayName>
          <Protocol Name="OAuth1" />
          <Metadata>
            <Item Key="ProviderName">Twitter</Item>
            <Item Key="authorization_endpoint">https://api.twitter.com/oauth/authenticate</Item>
            <Item Key="access_token_endpoint">https://api.twitter.com/oauth/access_token</Item>
            <Item Key="request_token_endpoint">https://api.twitter.com/oauth/request_token</Item>
            <Item Key="ClaimsEndpoint">https://api.twitter.com/1.1/account/verify_credentials.json?include_email=true</Item>
            <Item Key="ClaimsResponseFormat">json</Item>
            <Item Key="client_id">Your Twitter application API key</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="client_secret" StorageReferenceId="B2C_1A_TwitterSecret" />
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="user_id" />
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
            <OutputClaim ClaimTypeReferenceId="email" />
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

4. Remplacez la valeur de **client_id** par la *Clé secrète API* que vous avez enregistrée précédemment.
5. Enregistrez le fichier .

[!INCLUDE [active-directory-b2c-add-identity-provider-to-user-journey](../../includes/active-directory-b2c-add-identity-provider-to-user-journey.md)]


```xml
<OrchestrationStep Order="1" Type="CombinedSignInAndSignUp" ContentDefinitionReferenceId="api.signuporsignin">
  <ClaimsProviderSelections>
    ...
    <ClaimsProviderSelection TargetClaimsExchangeId="TwitterExchange" />
  </ClaimsProviderSelections>
  ...
</OrchestrationStep>

<OrchestrationStep Order="2" Type="ClaimsExchange">
  ...
  <ClaimsExchanges>
    <ClaimsExchange Id="TwitterExchange" TechnicalProfileReferenceId="Twitter-OAuth1" />
  </ClaimsExchanges>
</OrchestrationStep>
```

[!INCLUDE [active-directory-b2c-configure-relying-party-policy](../../includes/active-directory-b2c-configure-relying-party-policy-user-journey.md)]

## <a name="test-your-custom-policy"></a>Tester votre stratégie personnalisée

1. Sélectionnez votre stratégie de partie de confiance, par exemple `B2C_1A_signup_signin`.
1. Pour **Application**, sélectionnez une application web que vous avez [précédemment inscrite](tutorial-register-applications.md). L’**URL de réponse** doit être `https://jwt.ms`.
1. Sélectionnez le bouton **Exécuter maintenant**.
1. À partir de la page d’inscription ou de connexion, sélectionnez **Twitter** pour vous connecter avec un compte Twitter.

Si le processus de connexion réussit, votre navigateur est redirigé vers `https://jwt.ms`, qui affiche le contenu du jeton retourné par Azure AD B2C.

::: zone-end
