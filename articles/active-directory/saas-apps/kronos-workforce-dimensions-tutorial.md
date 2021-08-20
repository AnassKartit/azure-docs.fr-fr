---
title: 'Tutoriel : Intégration de l’authentification unique Azure Active Directory à Kronos Workforce Dimensions | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Kronos Workforce Dimensions.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/19/2021
ms.author: jeedes
ms.openlocfilehash: 53bcb759ceebf83b87f4042a9bf7e95e97f26595
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/22/2021
ms.locfileid: "114479642"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-kronos-workforce-dimensions"></a>Tutoriel : Intégration de l’authentification unique Azure Active Directory à Kronos Workforce Dimensions

Dans ce tutoriel, vous allez découvrir comment intégrer Kronos Workforce Dimensions à Azure Active Directory (Azure AD). Quand vous intégrez Kronos Workforce Dimensions à Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à Kronos Workforce Dimensions.
* Permettre à vos utilisateurs de se connecter automatiquement à Kronos Workforce Dimensions avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement Kronos Workforce Dimensions pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

* Kronos Workforce Dimensions prend en charge l’authentification unique lancée par le **fournisseur de services**.

## <a name="add-kronos-workforce-dimensions-from-the-gallery"></a>Ajouter Kronos Workforce Dimensions à partir de la galerie

Pour configurer l’intégration de Kronos Workforce Dimensions à Azure AD, vous devez ajouter Kronos Workforce Dimensions à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Kronos Workforce Dimensions** dans la zone de recherche.
1. Sélectionnez **Kronos Workforce Dimensions** dans le volet des résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-kronos-workforce-dimensions"></a>Configurer et tester l’authentification unique Azure AD pour Kronos Workforce Dimensions

Configurez et testez l’authentification unique Azure AD avec Kronos Workforce Dimensions pour un utilisateur de test nommé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur Kronos Workforce Dimensions associé.

Pour configurer et tester l’authentification unique Azure AD avec Kronos Workforce Dimensions, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Kronos Workforce Dimensions](#configure-kronos-workforce-dimensions-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test Kronos Workforce Dimensions](#create-kronos-workforce-dimensions-test-user)** pour avoir un équivalent de B.Simon dans Kronos Workforce Dimensions lié à la représentation Azure AD de l’utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, dans la page d’intégration de l’application **Kronos Workforce Dimensions**, recherchez la section **Gérer**, puis sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    a. Dans la zone de texte **Identificateur (ID d’entité)** , saisissez une URL au format suivant : `https://<SUBDOMAIN>.<ENVIRONMENT>.mykronos.com/authn/<TENANT_ID/hsp/<TENANT_NUMBER>`

    b. Dans la zone de texte **URL de connexion**, tapez une URL en utilisant un des modèles suivants :
    
    | **URL d’authentification** |
    |-----|
    | `https://<CUSTOMER>-<ENVIRONMENT>-sso.<ENVIRONMENT>.mykronos.com/` |
    | `https://<CUSTOMER>-sso.<ENVIRONMENT>.mykronos.com/` |

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’identificateur et l’URL de connexion réels. Pour obtenir ces valeurs, contactez l’[équipe de support technique Kronos Workforce Dimensions](mailto:support@kronos.com). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

1. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur le bouton Copier pour copier l’**URL des métadonnées de fédération d’application**, puis enregistrez-la sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

Dans cette section, vous allez créer un utilisateur de test appelé B. Simon dans le portail Azure.

1. Dans le volet gauche du Portail Azure, sélectionnez **Azure Active Directory**, **Utilisateurs**, puis **Tous les utilisateurs**.
1. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.
1. Dans les propriétés **Utilisateur**, effectuez les étapes suivantes :
   1. Dans le champ **Nom**, entrez `B.Simon`.  
   1. Dans le champ **Nom de l’utilisateur**, entrez username@companydomain.extension. Par exemple : `B.Simon@contoso.com`.
   1. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ **Mot de passe**.
   1. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Kronos Workforce Dimensions.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Kronos Workforce Dimensions**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-kronos-workforce-dimensions-sso"></a>Configurer l’authentification unique Kronos Workforce Dimensions

Pour configurer l’authentification unique côté **Kronos Workforce Dimensions**, vous devez envoyer l’**URL des métadonnées de fédération d’application** à l’[équipe du support technique Kronos Workforce Dimensions](mailto:support@kronos.com). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-kronos-workforce-dimensions-test-user"></a>Créer un utilisateur de test Kronos Workforce Dimensions

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans Kronos Workforce Dimensions. Collaborez avec l’[équipe du support technique Kronos Workforce Dimensions](mailto:support@kronos.com) pour ajouter des utilisateurs à la plateforme Kronos Workforce Dimensions. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

* Cliquez sur **Tester cette application** dans le portail Azure. Cette opération redirige vers l’URL de connexion Kronos Workforce Dimensions où vous pouvez lancer le flux de connexion. 

* Accédez directement à l’URL de connexion Kronos Workforce Dimensions pour lancer le flux de connexion.

* Vous pouvez utiliser Mes applications de Microsoft. Un clic sur la vignette Kronos Workforce Dimensions dans Mes applications vous redirige vers l’URL de connexion à Kronos Workforce Dimensions. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré Kronos Workforce Dimensions, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).