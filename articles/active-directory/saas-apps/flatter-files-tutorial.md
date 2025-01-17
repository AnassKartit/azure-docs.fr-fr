---
title: 'Didacticiel : Intégration d’Azure AD à Flatter Files | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Flatter Files.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/15/2019
ms.author: jeedes
ms.openlocfilehash: 26ce0b43047596426c2a3a708dd336d9ca33b0c5
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131046467"
---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Didacticiel : Intégration d’Azure Active Directory à Flatter Files

Dans ce didacticiel, vous allez apprendre à intégrer Flatter Files à Azure Active Directory (Azure AD).
L’intégration de Flatter Files dans Azure AD offre les avantages suivants :

* Dans Azure AD, vous pouvez contrôler l’accès des utilisateurs à Flatter Files.
* Vous pouvez permettre aux utilisateurs de se connecter automatiquement à Flatter Files (via l’authentification unique) avec leurs comptes Azure AD.
* Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).
Si vous ne disposez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.

## <a name="prerequisites"></a>Conditions préalables requises

Pour configurer l’intégration d’Azure AD avec Flatter Files, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un essai d’un mois [ici](https://azure.microsoft.com/pricing/free-trial/).
* Abonnement Flatter Files pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Flatter Files prend en charge l’authentification unique lancée par le **fournisseur d’identité**

## <a name="adding-flatter-files-from-the-gallery"></a>Ajout de Flatter Files à partir de la galerie

Pour configurer l’intégration de Flatter Files avec Azure AD, vous devez ajouter Flatter Files à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Flatter Files à partir de la galerie, procédez comme suit :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)** , cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez l’option **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, tapez **Flatter Files**, sélectionnez **Flatter Files** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

     ![Flatter Files dans la liste des résultats](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Flatter Files pour un utilisateur de test appelé **Britta Simon**.
Pour que l’authentification unique fonctionne, une relation entre un utilisateur Azure AD et l’utilisateur Flatter Files associé doit être établie.

Pour configurer et tester l’authentification unique Azure AD avec Flatter Files, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Configurer l’authentification unique Flatter Files](#configure-flatter-files-single-sign-on)** pour configurer les paramètres de l’authentification unique côté application.
3. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Créer un utilisateur de test Flatter Files](#create-flatter-files-test-user)** pour avoir un équivalent de Britta Simon dans Flatter Files lié à la représentation Azure AD associée.
6. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous activez l’authentification unique Azure AD dans le portail Azure.

Pour configurer l’authentification unique Azure AD avec Flatter Files, effectuez les étapes suivantes :

1. Dans le [Portail Azure](https://portal.azure.com/), dans la page d’intégration de l’application **Flatter Files**, sélectionnez **Authentification unique**.

    ![Lien Configurer l’authentification unique](common/select-sso.png)

2. Dans la boîte de dialogue **Sélectionner une méthode d’authentification unique**, sélectionnez le mode **SAML/WS-Fed** afin d’activer l’authentification unique.

    ![Mode de sélection de l’authentification unique](common/select-saml-option.png)

3. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône **Modifier** pour ouvrir la boîte de dialogue **Configuration SAML de base**.

    ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, l’utilisateur n’a rien à faire, car l’application est déjà intégrée à Azure.

    ![Informations d’authentification unique dans Domaine et URL Flatter Files](common/preintegrated.png)

5. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le **Certificat (Base64)** en fonction des options définies par rapport à vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/certificatebase64.png)

6. Dans la section **Configurer Flatter Files**, copiez les URL appropriées, selon vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

    a. URL de connexion

    b. Identificateur Azure AD

    c. URL de déconnexion

### <a name="configure-flatter-files-single-sign-on"></a>Configurer l’authentification unique Flatter Files

1. Connectez-vous à votre application Flatter Files en tant qu’administrateur.

2. Cliquez sur **TABLEAU DE BORD**. 
   
    ![Capture d’écran montrant « DASHBOARD » sélectionné dans l’application « Flatter Files ».](./media/flatter-files-tutorial/tutorial_flatter_files_05.png)  

3. Cliquez sur **Settings**, puis procédez comme suit dans l’onglet **Company** : 

    ![Capture d’écran montrant l’onglet « Company » avec l’option « Use SAML 2.0 for Authentication » activée et le bouton « Configure SAML » sélectionné.](./media/flatter-files-tutorial/tutorial_flatter_files_06.png)  

    1. Sélectionnez **Use SAML 2.0 for Authentication**.
    
    1. Cliquez sur **Configure SAML**.

4. Dans la boîte de dialogue **SAML Configuration** , procédez comme suit : 
   
    ![Configure Single Sign-On](./media/flatter-files-tutorial/tutorial_flatter_files_08.png)  
   
    a. Dans la zone de texte **Domaine**, entrez votre domaine enregistré.
   
   > [!NOTE]
   > Si vous n’avez pas encore de domaine enregistré, contactez votre équipe de support Flatter Files via [support@flatterfiles.com](mailto:support@flatterfiles.com). 
    
    b. Dans la zone de texte **URL du fournisseur d’identité**, collez la valeur de l’**URL de connexion** que vous avez copiée à partir du Portail Azure.
   
    c.  Ouvrez votre certificat codé en base 64 dans le Bloc-notes, copiez son contenu dans le Presse-papiers, puis collez-le dans la zone de texte **Identity provider certificate (Certificat du fournisseur d’identité)** .

    d. Cliquez sur **Update**.

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD 

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

1. Dans le volet gauche du portail Azure, sélectionnez **Azure Active Directory**, sélectionnez **Utilisateurs**, puis sélectionnez **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](common/users.png)

2. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.

    ![Bouton Nouvel utilisateur](common/new-user.png)

3. Dans les propriétés de l’utilisateur, effectuez les étapes suivantes.

    ![Boîte de dialogue Utilisateur](common/user-properties.png)

    a. Dans le champ **Nom**, entrez **BrittaSimon**.
  
    b. Dans le champ **Nom d’utilisateur**, tapez **brittasimon\@domainedevotreentreprise.extension**.  
    Par exemple : BrittaSimon@contoso.com

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ Mot de passe.

    d. Cliquez sur **Créer**.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Flatter Files.

1. Dans le Portail Azure, sélectionnez **Applications d’entreprise**, **Toutes les applications**, puis **Flatter Files**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **Flatter Files**.

    ![Lien Flatter Files dans la liste des applications](common/all-applications.png)

3. Dans le menu de gauche, sélectionnez **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »](common/users-groups-blade.png)

4. Cliquez sur le bouton **Ajouter un utilisateur**, puis sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

    ![Volet Ajouter une attribution](common/add-assign-user.png)

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

6. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.

7. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-flatter-files-test-user"></a>Créer un utilisateur de test Flatter Files

L’objectif de cette section est de créer un utilisateur appelé Britta Simon dans Flatter Files.

**Pour créer un utilisateur appelé Britta Simon dans Flatter Files, procédez comme suit :**

1. Connectez-vous à votre site d’entreprise **Flatter Files** en tant qu’administrateur.

2. Dans le volet de navigation situé à gauche, cliquez sur **Paramètres**, puis sur l’onglet **Utilisateurs**.
   
    ![Capture d’écran montrant la page « Settings » avec l’onglet « Users » sélectionné.](./media/flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Cliquez sur **Add User**. 

4. Dans la boîte de dialogue **Ajouter un utilisateur**, procédez comme suit :
   
    ![Créer un utilisateur Flatter Files](./media/flatter-files-tutorial/tutorial_flatter_files_10.png)

    a. Dans la zone de texte **First Name**, tapez **Britta**.
   
    b. Dans la zone de texte **Last Name**, tapez **Simon**. 
   
    c. Dans la zone de texte **Adresse e-mail**, tapez l’adresse de messagerie de Britta indiquée dans le portail Azure.
   
    d. Cliquez sur **Envoyer**.   


### <a name="test-single-sign-on"></a>Tester l’authentification unique 

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Quand vous cliquez sur la vignette Flatter Files dans le volet d’accès, vous devez être automatiquement connecté à l’application Flatter Files pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="additional-resources"></a>Ressources supplémentaires

- [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](./tutorial-list.md)

- [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

- [Qu’est-ce que l’accès conditionnel dans Azure Active Directory ?](../conditional-access/overview.md)