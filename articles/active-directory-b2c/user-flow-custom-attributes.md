---
title: Définir des attributs personnalisés dans Azure Active Directory B2C
description: Définissez des attributs personnalisés pour votre application dans Azure Active Directory B2C afin de recueillir des informations sur vos clients.
services: active-directory-b2c
author: kengaderdus
manager: CelesteDG
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 10/08/2021
ms.custom: project-no-code
ms.author: kengaderdus
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: ad60fd02ac8707af6bc5dfff5bb23ce7223d48b5
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2021
ms.locfileid: "130231873"
---
# <a name="define-custom-attributes-in-azure-active-directory-b2c"></a>Définir des attributs personnalisés dans Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

Dans l’article [Ajouter des revendications et personnaliser une entrée d’utilisateur à l’aide de stratégies personnalisées](configure-user-input.md), vous apprenez à utiliser des [attributs de profil utilisateur](user-profile-attributes.md) intégrés. Dans cet article, vous allez activer un attribut personnalisé dans votre annuaire Azure Active Directory B2C (Azure AD B2C). Plus tard, vous pourrez utiliser le nouvel attribut simultanément en tant que revendication personnalisée dans les [flux d’utilisateurs](user-flow-overview.md) ou les [stratégies personnalisées](user-flow-overview.md) .

Votre annuaire Azure AD B2C comprend un [ensemble intégré d’attributs](user-profile-attributes.md). Toutefois, vous devez souvent créer vos propres attributs pour gérer votre scénario spécifique, par exemple dans les cas suivants :

* Une application côté client a besoin de conserver un attribut tel que **loyaltyId**.
* Un fournisseur d’identité a un identificateur d’utilisateur unique, **uniqueUserGUID**, qui doit être enregistré.
* Un parcours utilisateur personnalisé doit enregistrer l’état de l’utilisateur, **migrationStatus**, pour que d’autres logiques fonctionnent dessus.

Dans cet article, les termes *propriété d’extension*, *attribut personnalisé* et *revendication personnalisée* font référence à la même chose. Le nom varie en fonction du contexte (application, objet, stratégie).

Azure AD B2C vous permet d’étendre l’ensemble d’attributs stocké sur chaque compte d’utilisateur. Vous pouvez également lire et écrire ces attributs à l’aide de [l’API Microsoft Graph](microsoft-graph-operations.md).

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="create-a-custom-attribute"></a>Création d’un attribut personnalisé

1. Connectez-vous au [portail Azure](https://portal.azure.com/) en tant qu’administrateur général de votre locataire Azure AD B2C.
1. Assurez-vous que vous utilisez le répertoire qui contient votre locataire Azure AD B2C en l’activant dans l’angle supérieur droit du portail Azure. Sélectionnez les informations sur votre abonnement, puis cliquez sur **Changer de répertoire**.
1. Choisissez **Tous les services** dans le coin supérieur gauche du Portail Azure, recherchez et sélectionnez **Azure Active Directory B2C**.
1. Sélectionnez **Attributs utilisateur**, puis **Ajouter**.
1. Fournissez un **nom** pour l’attribut personnalisé (par exemple, « ShoeSize »).
1. Choisissez un **Type de données**. Seules les valeurs **String**, **Boolean** et **Int** sont disponibles.
1. Si vous le souhaitez, entrez une **Description** à titre d’information.
1. Sélectionnez **Create** (Créer).

L’attribut personnalisé est actuellement disponible dans la liste des **attributs utilisateur**, et pour une utilisation dans vos flux d’utilisateur. Un attribut personnalisé est créé lors de sa première utilisation dans un flux d’utilisateur, et non pas quand vous l’ajoutez à la liste des **Attributs utilisateur**.

::: zone pivot="b2c-user-flow"

## <a name="use-a-custom-attribute-in-your-user-flow"></a>Utilisation d’un attribut personnalisé dans votre flux d’utilisateur

1. Dans votre locataire Azure AD B2C, sélectionnez **Flux d’utilisateur**.
1. Sélectionnez votre stratégie (par exemple, « B2C_1_SignupSignin ») pour l’ouvrir.
1. Sélectionnez **Attributs d’utilisateur**, puis sélectionnez l’attribut personnalisé (par exemple, « ShoeSize »). Sélectionnez **Enregistrer**.
1. Sélectionnez **Revendications d’applications**, puis sélectionnez l’attribut personnalisé.
1. Sélectionnez **Enregistrer**.

Une fois que vous avez créé un utilisateur à l’aide d’un flux d’utilisateur qui utilise le nouvel attribut personnalisé, l’objet peut être interrogé dans [l’Afficheur Microsoft Graph](https://developer.microsoft.com/graph/graph-explorer). Vous pouvez également utiliser la fonctionnalité [Exécuter le flux d’utilisateur](./tutorial-create-user-flows.md) sur le flux d’utilisateur pour vérifier l’expérience utilisateur. Vous devez maintenant voir **ShoeSize** dans la liste d’attributs collectés lors de l’inscription, et le voir dans le jeton retourné à votre application.

::: zone-end

## <a name="azure-ad-b2c-extensions-app"></a>Application d’extensions Azure AD B2C

Les attributs d’extension ne peuvent être inscrits que pour un objet application, même s’ils peuvent contenir les données d’un utilisateur. L’attribut d’extension est attaché à l’application appelée `b2c-extensions-app`. Ne modifiez pas cette application, car elle est utilisée par Azure AD B2C pour le stockage des données utilisateurs. Vous trouverez cette application sous Azure AD B2C, inscriptions d’applications. 

::: zone pivot="b2c-user-flow"

### <a name="get-extensions-apps-application-id"></a>Obtenir l’ID d’application de l’application d’extensions

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Veillez à bien utiliser l’annuaire qui contient votre locataire Azure AD B2C. Sélectionnez l’icône **Répertoires + abonnements** dans la barre d’outils du portail.
1. Sur la page **Paramètres du portail | Répertoires + abonnements**, recherchez votre répertoire AD B2C Azure dans la liste **Nom de répertoire**, puis sélectionnez **Basculer**.
1. Dans le menu de gauche, sélectionnez **Azure AD B2C**. Ou sélectionnez **Tous les services**, puis recherchez et sélectionnez **Azure AD B2C**.
1. Sélectionnez **Inscriptions d’applications**, puis sélectionnez **Toutes les applications**.
1. Sélectionner l’application `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.`.
1. Copiez **l’ID de l’application**. Exemple : `11111111-1111-1111-1111-111111111111`.
 
::: zone-end

::: zone pivot="b2c-custom-policy"

### <a name="get-extensions-apps-application-properties"></a>Obtenir les propriétés d’application de l’application d’extensions

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Veillez à bien utiliser l’annuaire qui contient votre locataire Azure AD B2C. Sélectionnez l’icône **Répertoires + abonnements** dans la barre d’outils du portail.
1. Sur la page **Paramètres du portail | Répertoires + abonnements**, recherchez votre répertoire AD B2C Azure dans la liste **Nom de répertoire**, puis sélectionnez **Basculer**.
1. Dans le menu de gauche, sélectionnez **Azure AD B2C**. Ou sélectionnez **Tous les services**, puis recherchez et sélectionnez **Azure AD B2C**.
1. Sélectionnez **Inscriptions d’applications**, puis sélectionnez **Toutes les applications**.
1. Sélectionner l’application `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.`.
1. Copiez les identificateurs suivants dans le Presse-papiers, puis enregistrez-les :
    * **ID de l’application**. Exemple : `11111111-1111-1111-1111-111111111111`.
    * **ID objet**. Exemple : `22222222-2222-2222-2222-222222222222`.

## <a name="modify-your-custom-policy"></a>Modifier votre stratégie personnalisée

Pour activer des attributs personnalisés dans votre stratégie, fournissez l’**ID de l’application** et l’**ID d’objet** de l’application dans les métadonnées du profil technique AAD-Common. Le profil technique *AAD-Common* se trouve dans le profil technique de base [Azure Active Directory](active-directory-technical-profile.md) et prend en charge la gestion des utilisateurs Azure AD. D’autres profils techniques Azure AD incluent AAD-Common pour tirer parti de cette configuration. Remplacez le profil technique AAD-Common dans le fichier d’extension.

1. Ouvrez le fichier d’extensions de votre stratégie. Par exemple <em>`SocialAndLocalAccounts/`**`TrustFrameworkExtensions.xml`**</em>.
1. Recherchez l’élément ClaimsProviders. Ajoutez un nouveau ClaimsProvider à l’élément ClaimsProviders.
1. Insérez **l’ID d’application** que vous avez enregistré précédemment, entre les éléments d’ouverture `<Item Key="ClientId">` et de fermeture `</Item>`.
1. Insérez **l’ID d’objet d’application** que vous avez enregistré précédemment, entre les éléments d’ouverture `<Item Key="ApplicationObjectId">` et de fermeture `</Item>`.

    ```xml
    <!-- 
    <ClaimsProviders> -->
      <ClaimsProvider>
        <DisplayName>Azure Active Directory</DisplayName>
        <TechnicalProfiles>
          <TechnicalProfile Id="AAD-Common">
            <Metadata>
              <!--Insert b2c-extensions-app application ID here, for example: 11111111-1111-1111-1111-111111111111-->  
              <Item Key="ClientId"></Item>
              <!--Insert b2c-extensions-app application ObjectId here, for example: 22222222-2222-2222-2222-222222222222-->
              <Item Key="ApplicationObjectId"></Item>
            </Metadata>
          </TechnicalProfile>
        </TechnicalProfiles> 
      </ClaimsProvider>
    <!-- 
    </ClaimsProviders> -->
    ```

## <a name="upload-your-custom-policy"></a>Télécharger votre stratégie personnalisée

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Veillez à bien utiliser le répertoire qui contient votre locataire Azure AD B2C. Sélectionnez l’icône **Répertoires + abonnements** dans la barre d’outils du portail.
1. Sur la page **Paramètres du portail | Répertoires + abonnements**, recherchez votre répertoire AD B2C Azure dans la liste **Nom de répertoire**, puis sélectionnez **Basculer**.
1. Choisissez **Tous les services** dans le coin supérieur gauche du portail Azure, puis recherchez et sélectionnez **Inscriptions d’applications**.
1. Sélectionnez **Infrastructure d’expérience d’identité**.
1. Sélectionnez **Charger une stratégie personnalisée**, puis chargez les fichiers de stratégie TrustFrameworkExtensions.xml que vous avez modifiés.

> [!NOTE]
> La première fois que le profil technique Azure AD enregistre la revendication dans l’annuaire, il vérifie si l’attribut personnalisé existe. Si ce n’est pas le cas, il crée l’attribut personnalisé.  

## <a name="create-a-custom-attribute-through-azure-portal"></a>Créer un attribut personnalisé via le portail Azure

Les mêmes attributs d’extension sont partagés entre les stratégies intégrées et les stratégies personnalisées. Lorsque vous ajoutez des attributs personnalisés via le portail, ils sont inscrits à l’aide de **b2c-extensions-app**, qui se trouve dans chaque locataire B2C.

Vous pouvez créer ces attributs dans l’interface utilisateur du portail avant ou après leur utilisation dans vos stratégies personnalisées. Lorsque vous créez un attribut **loyaltyId** dans le portail, vous devez y faire référence de la façon suivante :

|Nom     |Utilisé dans |
|---------|---------|
|`extension_loyaltyId`  | Stratégie personnalisée|
|`extension_<b2c-extensions-app-guid>_loyaltyId`  | [API Microsoft Graph](microsoft-graph-operations.md)|

L’exemple suivant illustre l’utilisation d’attributs personnalisés dans une définition de revendication de stratégie personnalisée Azure AD B2C.

```xml
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="extension_loyaltyId">
      <DisplayName>Loyalty Identification</DisplayName>
      <DataType>string</DataType>
      <UserHelpText>Your loyalty number from your membership card</UserHelpText>
      <UserInputType>TextBox</UserInputType>
    </ClaimType>
  </ClaimsSchema>
</BuildingBlocks>
```

L’exemple suivant illustre l’utilisation d’un attribut personnalisé au sein d’une stratégie personnalisée Azure AD B2C dans un profil technique, une entrée, une sortie et des revendications persistantes.

```xml
<InputClaims>
  <InputClaim ClaimTypeReferenceId="extension_loyaltyId"  />
</InputClaims>
<PersistedClaims>
  <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />
</PersistedClaims>
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
</OutputClaims>
```

::: zone-end

## <a name="using-custom-attribute-with-ms-graph-api"></a>Utilisation d’un attribut personnalisé avec l’API MS Graph

L’API Microsoft Graph prend en charge la création et la mise à jour d’un utilisateur avec des attributs d’extension. Les attributs d’extension dans l’API Graph sont nommés d’après la convention `extension_ApplicationClientID_attributename`, où `ApplicationClientID` est l’**ID d’application (client)** de l’application `b2c-extensions-app`. Notez que l’**ID d’application (client)** tel qu’il est représenté dans le nom de l’attribut d’extension ne comprend aucun trait d’union. Par exemple :

```json
"extension_831374b3bd5041bfaa54263ec9e050fc_loyaltyId": "212342" 
``` 

## <a name="remove-extension-attribute"></a>Supprimer l’attribut d’extension

Contrairement aux attributs intégrés, les attributs d’extension/personnalisés peuvent être supprimés. Les valeurs des attributs d’extension peuvent également être supprimées. 

> [!Important]
> Avant de supprimer l’attribut d’extension/personnalisé, pour chaque compte du répertoire, définissez la valeur de l’attribut extension sur null.  De cette façon, vous supprimez de manière explicite les valeurs des attributs d’extension. Continuez ensuite à supprimer l’attribut d’extension lui-même. L’attribut d’extension/personnalisé peut être interrogé à l’aide de l’API MS Graph. 

::: zone pivot="b2c-user-flow"

Procédez comme suit pour supprimer un attribut d’extension/personnalisé d’un flux d’utilisateur :

1. Connectez-vous au [portail Azure](https://portal.azure.com/) en tant qu’administrateur général de votre locataire Azure AD B2C.
2. Veillez à bien utiliser le répertoire qui contient votre locataire Azure AD B2C :
    1.  Sélectionnez l’icône **Répertoires + abonnements** dans la barre d’outils du portail.
    1. Sur la page **Paramètres du portail | Répertoires + abonnements**, recherchez votre répertoire Azure AD B2C dans la liste Nom de répertoire, puis sélectionnez **Basculer**.
1. Choisissez **Tous les services** dans le coin supérieur gauche du Portail Azure, recherchez et sélectionnez **Azure Active Directory B2C**.
1. Sélectionnez **Attributs utilisateur**, puis l’attribut que vous voulez supprimer.
1. Sélectionnez **Supprimer**, puis cliquez sur **Oui** pour confirmer.

::: zone-end

::: zone pivot="b2c-custom-policy"

Pour supprimer un attribut personnalisé, utilisez l’[API MS Graph](microsoft-graph-operations.md), puis la commande [Supprimer](/graph/api/application-delete-extensionproperty).

::: zone-end

 


## <a name="next-steps"></a>Étapes suivantes

Suivez les instructions pour [ajouter des revendications et personnaliser l’entrée utilisateur avec des stratégies personnalisées](configure-user-input.md). Cet exemple utilise une revendication intégrée « City ». Pour utiliser un attribut personnalisé, remplacez « City » par vos propres attributs personnalisés.
