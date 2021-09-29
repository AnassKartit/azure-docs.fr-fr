---
title: 'Didacticiel : Intégration d’Azure Active Directory avec FileCloud | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et FileCloud.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/10/2021
ms.author: jeedes
ms.openlocfilehash: a3cec8d1b6b49171d75ae5fcc747a16375e4d46a
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124835052"
---
# <a name="tutorial-azure-active-directory-integration-with-filecloud"></a>Didacticiel : Intégration d’Azure Active Directory avec FileCloud

Dans ce tutoriel, vous allez apprendre à intégrer FileCloud avec Azure Active Directory (Azure AD). Quand vous intégrez FileCloud avec Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à FileCloud.
* Permettre à vos utilisateurs de se connecter automatiquement à FileCloud avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement FileCloud pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* FileCloud prend en charge l’authentification unique lancée par le **fournisseur de services**.

* FileCloud prend en charge le provisionnement d’utilisateurs **juste-à-temps**.

## <a name="add-filecloud-from-the-gallery"></a>Ajouter FileCloud à partir de la galerie

Pour configurer l’intégration de FileCloud à Azure AD, vous devez ajouter FileCloud à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **FileCloud** dans la zone de recherche.
1. Sélectionnez **FileCloud** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-filecloud"></a>Configurer et tester l’authentification unique Azure AD pour FileCloud

Configurez et testez l’authentification unique Azure AD avec FileCloud à l’aide d’un utilisateur de test appelé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur FileCloud associé.

Pour configurer et tester l’authentification unique Azure AD avec FileCloud, procédez comme suit :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique FileCloud](#configure-filecloud-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test FileCloud](#create-filecloud-test-user)** pour avoir un équivalent de B.Simon dans FileCloud, lié à la représentation Azure AD de l’utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, dans la page d’intégration de l’application **FileCloud**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    a. Dans la zone de texte **URL de connexion**, saisissez une URL au format suivant : `https://<SUBDOMAIN>.filecloudonline.com`

    b. Dans la zone de texte **Identificateur (ID d’entité)** , saisissez une URL au format suivant : `https://<SUBDOMAIN>.filecloudonline.com/simplesaml/module.php/saml/sp/metadata.php/default-sp`

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’URL de connexion et l’identificateur réels. Pour obtenir ces valeurs, contactez l’[équipe de prise en charge des clients FileCloud](mailto:support@codelathe.com). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

5. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies selon vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

6. Dans la section **Configurer FileCloud**, copiez les URL appropriées, selon vos besoins.

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à FileCloud.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **FileCloud**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-filecloud-sso"></a>Configurer l’authentification unique FileCloud

1. Dans une autre fenêtre de navigateur web, connectez-vous à votre client FileCloud en tant qu’administrateur.

2. Dans le volet de navigation gauche, cliquez sur **Paramètres**. 
   
    ![Capture d’écran montrant « Settings » mis en évidence dans le volet de navigation gauche.](./media/filecloud-tutorial/setting.png)

3. Dans la section Paramètres, cliquez sur l’onglet **SSO**. 
   
    ![Capture d’écran montrant la section « Settings » avec l’option « SSO » sélectionnée.](./media/filecloud-tutorial/tab.png)

4. Dans le panneau **Single Sign On (SSO) Settings** (Paramètres d’authentification unique), sélectionnez **SAML** comme **Default SSO Type** (Type d’authentification unique par défaut).
   
    ![Capture d’écran montrant le panneau « Single Sign On (SSO) » Settings avec « SAML » sélectionné.](./media/filecloud-tutorial/panel.png)

5. Dans la zone de texte **IdP End Point URL** (URL du point de terminaison IdP), collez la valeur de l’**identificateur Azure AD** que vous avez copiée à partir du Portail Azure.

    ![Capture d’écran montrant la section « SAML Settings » avec « IdP End Point URL » mis en évidence.](./media/filecloud-tutorial/identifier.png)

6. Ouvrez votre fichier de métadonnées téléchargé dans le bloc-notes, copiez le contenu de celui-ci dans le Presse-papiers, puis collez-le dans la zone de texte **IdP Meta Data** (métadonnées IdP) du panneau **SAML Settings** (Paramètres SAML).

    ![Configurer l’authentification unique côté application](./media/filecloud-tutorial/metadata.png)

7. Cliquez sur le bouton **Enregistrer** .

### <a name="create-filecloud-test-user"></a>Créer un utilisateur de test FileCloud

Dans cette section, un utilisateur appelé Britta Simon est créé dans FileCloud. FileCloud prend en charge l’attribution d’utilisateurs juste-à-temps, option activée par défaut. Vous n’avez aucune opération à effectuer dans cette section. Si l’utilisateur souhaité n’existe pas déjà dans FileCloud, il est créé après l’authentification.

>[!NOTE]
>Si vous devez créer un utilisateur manuellement, contactez l’[équipe de prise en charge des clients FileCloud](mailto:support@codelathe.com).

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à FileCloud à partir de laquelle vous pouvez lancer le flux de connexion. 

* Accédez directement à l’URL de connexion à FileCloud pour lancer le flux de connexion.

* Vous pouvez utiliser Mes applications de Microsoft. Quand vous cliquez sur la vignette FileCloud dans Mes applications, vous êtes redirigé vers l’URL de connexion à FileCloud. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré FileCloud, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).