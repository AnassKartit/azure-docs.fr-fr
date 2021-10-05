---
title: 'Tutoriel : Intégration de l’authentification unique Azure AD à LiquidFiles'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et LiquidFiles.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/13/2021
ms.author: jeedes
ms.openlocfilehash: 2c20c03e61d7fa7055d1044d69cacc8b234ad8f4
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128636645"
---
# <a name="tutorial-azure-ad-sso-integration-with-liquidfiles"></a>Tutoriel : Intégration de l’authentification unique Azure AD à LiquidFiles

Dans ce tutoriel, vous allez apprendre à intégrer LiquidFiles à Azure Active Directory (Azure AD). Quand vous intégrez LiquidFiles à Azure AD, vous pouvez :

* Contrôler, dans Azure AD, qui a accès à LiquidFiles.
* Permettre à vos utilisateurs de se connecter automatiquement à LiquidFiles avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec LiquidFiles, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un [compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement LiquidFiles pour lequel l’authentification unique est activée.

> [!NOTE]
> Cette intégration peut également être utilisée à partir de l’environnement cloud US Government Azure AD. Cette application est disponible dans la Galerie d’applications cloud US Government Azure AD et peut être configurée de la même façon que dans le cloud public.

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* LiquidFiles prend en charge l’authentification unique lancée par le **fournisseur de services**.

## <a name="add-liquidfiles-from-the-gallery"></a>Ajouter LiquidFiles à partir de la galerie

Pour configurer l’intégration de LiquidFiles avec Azure AD, vous devez ajouter LiquidFiles à partir de la galerie à votre liste d’applications SaaS managées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **LiquidFiles** dans la zone de recherche.
1. Sélectionnez **LiquidFiles** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-liquidfiles"></a>Configurer et tester l’authentification unique Azure AD pour LiquidFiles

Configurez et testez l’authentification unique Azure AD avec LiquidFiles à l’aide d’un utilisateur de test appelé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir une liaison entre un utilisateur Azure AD et l’utilisateur associé dans LiquidFiles.

Pour configurer et tester l’authentification unique Azure AD avec LiquidFiles, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique LiquidFiles](#configure-liquidfiles-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test LiquidFiles](#create-liquidfiles-test-user)** pour avoir, dans LiquidFiles, un équivalent de B.Simon lié à la représentation Azure AD de l’utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Sur le portail Azure, dans la page d’intégration de l’application **LiquidFiles**, recherchez la section **Gérer**, puis sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    a. Dans la zone de texte **Identificateur (ID d’entité)** , saisissez une URL au format suivant : `https://<YOUR_SERVER_URL>`

    b. Dans la zone de texte **URL de réponse** , tapez une URL au format suivant : `https://<YOUR_SERVER_URL>/saml/consume`

    c. Dans la zone de texte **URL de connexion**, saisissez une URL au format suivant : `https://<YOUR_SERVER_URL>/saml/init`

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’identificateur, l’URL de réponse et l’URL de connexion réels. Pour obtenir ces valeurs, contactez [l’équipe de support technique LiquidFiles](https://www.liquidfiles.com/support.html). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

5. Dans la section **Certificat de signature SAML**, cliquez sur le bouton **Modifier** pour ouvrir la boîte de dialogue **Certificat de signature SAML**.

    ![Modifier le certificat de signature SAML](common/edit-certificate.png)

6. Dans la section **Certificat de signature SAML**, copiez l’**EMPREINTE** et enregistrez-la sur votre ordinateur.

    ![Copier la valeur de l’empreinte](common/copy-thumbprint.png)

7. Dans la section **Configurer LiquidFiles**, copiez la ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à LiquidFiles.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **LiquidFiles**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-liquidfiles-sso"></a>Configurer l’authentification unique LiquidFiles

1. Connectez-vous à votre site d’entreprise LiquidFiles en tant qu’administrateur.

1. Dans le menu **Administration > Configuration**, cliquez sur **Single Sign-On (Authentification unique)**.

1. Dans la page **Configuration de l’authentification unique**, effectuez les étapes suivantes.

    ![Configure Single Sign-On](./media/liquidfiles-tutorial/configuration.png)

    a. Pour **Single Sign On Method** (Méthode d’authentification unique), sélectionnez **SAML 2**.

    b. Dans la zone de texte **IDP Login URL** (URL de connexion du fournisseur d’identité), collez la valeur **URL de connexion** que vous avez copiée dans le Portail Azure.

    c. Dans la zone de texte **IDP Logout URL** (URL de déconnexion du fournisseur d’identité), collez la valeur de l’**URL de déconnexion** que vous avez copiée à partir du Portail Azure.

    d. Dans la zone de texte **IDP Cert Fingerprint** (Empreinte du certificat IDP), collez la valeur **THUMBPRINT** que vous avez copiée à partir du portail Azure.

    e. Dans la zone de texte Format de l’identificateur de nom, saisissez la valeur `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`.

    f. Dans la zone de texte Authn Context (Contexte d’authentification), tapez la valeur `urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport`.

    g. Cliquez sur **Enregistrer**.

### <a name="create-liquidfiles-test-user"></a>Créer un utilisateur de test LiquidFiles

L’objectif de cette section est de créer un utilisateur nommé Britta Simon dans LiquidFiles. Demandez à votre administrateur de serveur LiquidFiles de vous ajouter en tant qu’utilisateur avant de vous connecter à votre application LiquidFiles.

## <a name="test-sso"></a>Tester l’authentification unique (SSO)

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à LiquidFiles à partir de laquelle vous pouvez lancer le flux de connexion. 

* Accédez directement à l’URL de connexion à LiquidFiles et lancez le flux de connexion.

* Vous pouvez utiliser Mes applications de Microsoft. Le fait de cliquer sur la vignette LiquidFiles dans Mes applications vous redirige vers l’URL de connexion à LiquidFiles. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré LiquidFiles, vous pouvez appliquer le contrôle de session, qui protège en temps réel contre l’exfiltration et l’infiltration des données sensibles de votre organisation. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).