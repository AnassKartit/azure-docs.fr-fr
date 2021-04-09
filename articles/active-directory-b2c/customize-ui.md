---
title: Personnaliser l’interface utilisateur
titleSuffix: Azure AD B2C
description: Découvrez comment personnaliser l’interface utilisateur de vos applications qui utilisent Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 01/28/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: ecece3a00a788b67f6c831804bf5b00372fef93d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99056762"
---
# <a name="customize-the-user-interface-in-azure-active-directory-b2c"></a>Personnaliser l’interface utilisateur dans Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

La personnalisation de l'interface utilisateur qu'Azure Active Directory B2C (Azure AD B2C) présente à vos clients permet d'offrir une expérience utilisateur fluide au sein de votre application. Ces expériences incluent la modification du profil, l’inscription, la connexion et la réinitialisation du mot de passe. Cet article vous explique comment personnaliser vos pages Azure AD B2C à l'aide d'un modèle de page et d'une marque de société. 

> [!TIP]
> Pour personnaliser d'autres aspects de vos pages de flux d'utilisateurs en plus du modèle de page, du logo de la bannière, de l'image et la couleur d'arrière-plan, consultez [Guide pratique pour personnaliser l'interface utilisateur avec un modèle HTML](customize-ui-with-html.md).

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [active-directory-b2c-customization-prerequisites](../../includes/active-directory-b2c-customization-prerequisites.md)]

## <a name="overview"></a>Vue d’ensemble

Azure AD B2C fournit différents modèles intégrés pour vous permettre de donner un aspect professionnel aux pages de votre interface utilisateur. Ces modèles de page peuvent également servir de point de départ à votre propre personnalisation, à l'aide de la fonctionnalité [Marque de société](#company-branding).

### <a name="ocean-blue"></a>Bleu océan

Exemple de modèle Bleu océan sur la page d'inscription et de connexion :

![Capture d'écran du modèle Bleu océan](media/customize-ui/template-ocean-blue.png)

### <a name="slate-gray"></a>Gris ardoise

Exemple de modèle Gris ardoise sur la page d'inscription et de connexion :

![Capture d'écran du modèle Gris ardoise](media/customize-ui/template-slate-gray.png)

### <a name="classic"></a>Classique

Exemple de modèle Classique sur la page d'inscription et de connexion :

![Capture d'écran du modèle Classique](media/customize-ui/template-classic.png)

### <a name="company-branding"></a>Marque de société

Vous pouvez personnaliser vos pages Azure AD B2C avec un logo de bannière, une image et une couleur d’arrière-plan avec la fonctionnalité [Personnalisation de la société](../active-directory/fundamentals/customize-branding.md) d’Azure Active Directory. La personnalisation de la société comprend l’inscription, la connexion, la modification de profil et la réinitialisation de mot de passe. 

L'exemple suivant présente une page *Inscription et connexion* comportant un logo personnalisé, une image d'arrière-plan, et utilisant le modèle Blue océan :

![Page d’inscription/connexion personnalisée servie par Azure AD B2C](media/customize-ui/template-ocean-blue-branded.png)


## <a name="select-a-page-template"></a>Sélectionner un modèle de page

::: zone pivot="b2c-user-flow"

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Sélectionnez l’icône **Annuaire et abonnement** dans la barre d’outils du portail, puis sélectionnez l’annuaire qui contient votre locataire Azure AD B2C.
1. Dans la Portail Azure, recherchez et sélectionnez **Azure AD B2C**.
1. Sélectionnez **Flux d’utilisateurs**.
1. Sélectionnez le flux d'utilisateurs que vous souhaitez personnaliser.
1. Sous **Personnaliser** dans le menu de gauche, sélectionnez **Mises en page**, puis choisissez un **Modèle**.
    ![Liste déroulante de sélection de modèle dans la page de flux d’utilisateur du portail Azure](./media/customize-ui/select-page-template.png)

Le modèle que vous sélectionnez est appliqué à toutes les pages de votre flux d'utilisateurs. L'URI de chaque page apparaît dans le champ **URI de page personnalisée**.

::: zone-end

::: zone pivot="b2c-custom-policy"

Pour sélectionner un modèle de page, définissez l'élément `LoadUri` des [définitions de contenu](contentdefinitions.md). L'exemple suivant illustre les identificateurs de définition de contenu et les `LoadUri` correspondants. 

Bleu océan :

```xml
<ContentDefinitions>
  <ContentDefinition Id="api.error">
    <LoadUri>~/tenant/templates/AzureBlue/exception.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections">
    <LoadUri>~/tenant/templates/AzureBlue/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections.signup">
    <LoadUri>~/tenant/templates/AzureBlue/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.signuporsignin">
    <LoadUri>~/tenant/templates/AzureBlue/unified.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted">
    <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted.profileupdate">
    <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountsignup">
    <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountpasswordreset">
    <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.phonefactor">
    <LoadUri>~/tenant/templates/AzureBlue/multifactor-1.0.0.cshtml</LoadUri>
  </ContentDefinition>
</ContentDefinitions>
```

Gris ardoise :

```xml
<ContentDefinitions>
  <ContentDefinition Id="api.error">
    <LoadUri>~/tenant/templates/MSA/exception.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections">
    <LoadUri>~/tenant/templates/MSA/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections.signup">
    <LoadUri>~/tenant/templates/MSA/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.signuporsignin">
    <LoadUri>~/tenant/templates/MSA/unified.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted">
    <LoadUri>~/tenant/templates/MSA/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted.profileupdate">
    <LoadUri>~/tenant/templates/MSA/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountsignup">
    <LoadUri>~/tenant/templates/MSA/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountpasswordreset">
    <LoadUri>~/tenant/templates/MSA/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.phonefactor">
    <LoadUri>~/tenant/templates/MSA/multifactor-1.0.0.cshtml</LoadUri>
  </ContentDefinition>
</ContentDefinitions>
```

Classique : 

```xml
<ContentDefinitions>
  <ContentDefinition Id="api.error">
    <LoadUri>~/tenant/templates/default/exception.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections">
    <LoadUri>~/tenant/templates/default/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections.signup">
    <LoadUri>~/tenant/templates/default/idpSelector.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.signuporsignin">
    <LoadUri>~/tenant/templates/AzureBlue/unified.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted">
    <LoadUri>~/tenant/templates/default/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted.profileupdate">
    <LoadUri>~/tenant/templates/default/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountsignup">
    <LoadUri>~/tenant/templates/default/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountpasswordreset">
    <LoadUri>~/tenant/templates/default/selfAsserted.cshtml</LoadUri>
  </ContentDefinition>
  <ContentDefinition Id="api.phonefactor">
    <LoadUri>~/tenant/templates/default/multifactor-1.0.0.cshtml</LoadUri>
  </ContentDefinition>
</ContentDefinitions>
```
::: zone-end


## <a name="configure-company-branding"></a>Configurer la marque de la société

Pour personnaliser vos pages de flux d'utilisateurs, vous devez d'abord configurer la marque de la société dans Azure Active Directory, puis l'activer dans vos flux d'utilisateurs Azure AD B2C.

Commencez par définir le logo de bannière, l’image d’arrière-plan et la couleur d’arrière-plan dans **Marque de la société**.

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Sélectionnez le filtre **Annuaire et abonnement** dans le menu supérieur, puis l’annuaire qui contient votre locataire Azure AD B2C.
1. Dans le portail Azure, recherchez et sélectionnez **Azure AD B2C**.
1. Sous **Gérer**, sélectionnez **Marque de la société**.
1. Suivez les étapes décrites dans [Personnaliser la page de connexion Azure Active Directory de votre organisation](../active-directory/fundamentals/customize-branding.md).

Lorsque vous configurez la marque de la société dans Azure AD B2C, gardez à l’esprit les points suivants :

* La personnalisation de la marque société dans Azure AD B2C est actuellement limitée à l’**image d’arrière-plan**, au **logo de bannière** et à la **couleur d’arrière-plan**. Les autres propriétés du volet Marque de la société, telles que les **Paramètres avancés**, ne sont *pas prises en charge*.
* Dans vos pages de flux utilisateur, la couleur d’arrière-plan est affichée avant le chargement de l’image d’arrière-plan. Nous vous suggérons de choisir une couleur d’arrière-plan proche des couleurs de votre image d’arrière-plan pour offrir une expérience de chargement homogène.
* Le logo de bannière apparaît dans les e-mails de vérification envoyés aux utilisateurs quand ceux-ci démarrent un flux utilisateur d’inscription.



## <a name="enable-company-branding-in-user-flow-pages"></a>Activer la marque de la société dans les pages de flux d'utilisateurs

::: zone pivot="b2c-user-flow"

Une fois que vous avez configuré la marque de la société, activez-la dans vos flux utilisateur.

1. Dans le menu de gauche du portail Azure, sélectionnez **Azure AD B2C**.
1. Sous **Stratégies**, sélectionnez **Flux utilisateur (stratégies)** .
1. Sélectionnez le flux utilisateur pour lequel vous souhaitez activer la marque de la société. La marque de la société n’est **pas prise en charge** pour les types de flux utilisateur *Connexion* et *Modification de profil* standard.
1. Sous **Personnaliser**, sélectionnez **Mises en page**, puis sélectionnez la page à personnaliser. Par exemple, sélectionnez **Page de connexion ou d’inscription unifiée**.
1. Pour **Version Mise en page (préversion)** , choisissez la version **1.2.0** ou version ultérieure.
1. Sélectionnez **Enregistrer**.

Si vous souhaitez personnaliser toutes les pages du flux utilisateur, définissez la version de mise en page pour chaque mise en page dans le flux utilisateur.

![Sélection de mise en page dans Azure AD B2C sur le portail Azure](media/customize-ui/portal-02-page-layout-select.png)

::: zone-end

::: zone pivot="b2c-custom-policy"

Après avoir configuré la marque de la société, activez-la dans votre stratégie personnalisée. Configurez la [version de la mise en page](contentdefinitions.md#migrating-to-page-layout) avec la version `contract` de la page pour *toutes* les définitions de contenu de votre stratégie personnalisée. Le format de la valeur doit contenir le mot `contract`: _urn:com:microsoft:aad:b2c:elements:**contract**:page-name:version_. Pour spécifier une mise en page dans vos stratégies personnalisées qui utilisent une ancienne valeur de **DataUri**, procédez comme suit. Pour plus d'informations, apprenez à [migrer vers la mise en page](contentdefinitions.md#migrating-to-page-layout) avec la version de la page.

L'exemple suivant illustre les définitions de contenu avec le contrat de page correspondant, et le modèle de page *Blue océan* : 

```xml
<ContentDefinitions>
  <ContentDefinition Id="api.error">
    <LoadUri>~/tenant/templates/AzureBlue/exception.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections">
    <LoadUri>~/tenant/templates/AzureBlue/idpSelector.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.idpselections.signup">
    <LoadUri>~/tenant/templates/AzureBlue/idpSelector.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.signuporsignin">
    <LoadUri>~/tenant/templates/AzureBlue/unified.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted">
    <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.selfasserted.profileupdate">
     <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountsignup">
     <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.localaccountpasswordreset">
     <LoadUri>~/tenant/templates/AzureBlue/selfAsserted.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.2.0</DataUri>
  </ContentDefinition>
  <ContentDefinition Id="api.phonefactor">
    <LoadUri>~/tenant/templates/AzureBlue/multifactor-1.0.0.cshtml</LoadUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.2.0</DataUri>
  </ContentDefinition>
</ContentDefinitions>
```

::: zone-end

## <a name="next-steps"></a>Étapes suivantes

Pour savoir comment personnaliser l’interface utilisateur de vos applications, consultez [Personnalisation de l’interface utilisateur d’une application dans Azure Active Directory B2C](customize-ui-with-html.md).
