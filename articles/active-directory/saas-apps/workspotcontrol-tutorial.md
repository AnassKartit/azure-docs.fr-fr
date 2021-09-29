---
title: "Tutoriel : Intégration d'Azure Active Directory à Workspot Control | Microsoft Docs"
description: Découvrez comment configurer l’authentification unique pour Azure Active Directory et Workspot Control.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/25/2021
ms.author: jeedes
ms.openlocfilehash: 261278419ffb82d73bd3bc09e1bf6ffab772ebc9
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124744982"
---
# <a name="tutorial-azure-active-directory-integration-with-workspot-control"></a>Tutoriel : Intégration d'Azure Active Directory à Workspot Control

Dans ce tutoriel, vous allez apprendre à intégrer Workspot Control à Azure Active Directory (Azure AD). Quand vous intégrez Workspot Control à Azure AD, vous pouvez :

* Contrôler, dans Azure AD, qui a accès à Workspot Control.
* Permettre à vos utilisateurs de se connecter automatiquement à Workspot Control avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à Workspot Control, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un [compte gratuit](https://azure.microsoft.com/free/).

* Un abonnement Workspot Control pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Workspot Control prend en charge l’authentification unique lancée par le fournisseur de services et le fournisseur d’identité.

## <a name="add-workspot-control-from-the-gallery"></a>Ajouter Workspot Control depuis la galerie

Pour configurer l’intégration de Workspot Control à Azure AD, vous devez ajouter Workspot Control à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Workspot Control** au niveau de la zone de recherche.
1. Sélectionnez **Workspot Control** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-workspot-control"></a>Configurer et tester l’authentification unique Azure AD pour Workspot Control

Configurez et testez l’authentification unique Azure AD avec Workspot Control au moyen d’un utilisateur de test nommé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur Workspot Control associé.

Pour configurer et tester l’authentification unique Azure AD avec Workspot Control, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Workspot Control](#configure-workspot-control-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test Workspot Control](#create-workspot-control-test-user)** pour avoir, dans Workspot Control, un équivalent de B. Simon lié à la représentation Azure AD associée.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **Workspot Control**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, si vous souhaitez configurer l’application en mode lancé par le fournisseur d’identité, effectuez ces étapes :

    1. Dans la zone de texte **identificateur**, tapez une URL au format suivant :<br/>
    `https://<<i></i>INSTANCENAME>-saml.workspot.com/saml/metadata`

    1. Dans la zone de texte **URL de réponse**, tapez une URL au format suivant :<br/>
    `https://<<i></i>INSTANCENAME>-saml.workspot.com/saml/assertion`

5. Si vous voulez configurer l’application en mode lancé par le fournisseur de services, sélectionnez **Définir des URL supplémentaires**.

    Dans la zone de texte **URL d’authentification**, tapez une URL au format suivant :<br/>
    `https://<<i></i>INSTANCENAME>-saml.workspot.com/`

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Remplacez ces valeurs par l’identificateur, l’URL de réponse et l’URL de connexion réels. Pour obtenir ces valeurs, contactez l’[équipe du support technique Workspot Control](mailto:support@workspot.com). Vous pouvez également consulter les modèles présentés dans la section **Configuration SAML de base** du portail Azure.

6. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, sélectionnez **Télécharger** pour télécharger le **certificat (Base64)** approprié parmi les options disponibles. Enregistrez-le sur votre ordinateur.

    ![Lien de téléchargement du certificat (en base64)](common/certificatebase64.png)

7. Dans la section **Configurer Workspot Control**, copiez les URL appropriées en fonction de vos besoins :

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en accordant l’accès à Workspot Control.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Workspot Control**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-workspot-control-sso"></a>Configurer l’authentification unique pour Workspot Control

1. Ouvrez une autre fenêtre de navigateur web, puis connectez-vous à Workspot Control en tant qu’administrateur de la sécurité.

2. Dans la barre d’outils en haut de la page, sélectionnez **Setup** (Configuration), puis **SAML**.

    ![Options d’installation](./media/workspotcontrol-tutorial/setup.png)

3. Dans la fenêtre **Security Assertion Markup Language Configuration** (Configuration SAML), effectuez ces étapes :
 
    ![Fenêtre Security Assertion Markup Language Configuration](./media/workspotcontrol-tutorial/security.png)

    1. Dans la zone **Entity ID** (ID d’entité), collez la valeur **Azure AD Identifier** (Identificateur Azure AD) que vous avez copiée à partir du portail Azure.

    1. Dans la zone **Signon Service URL** (URL du service de connexion), collez l’**URL de connexion** que vous avez copiée à partir du portail Azure.

    1. Dans la zone **Logout Service URL** (URL du service de déconnexion), collez l’**URL de déconnexion** que vous avez copiée à partir du portail Azure.

    1. Sélectionnez **Update File** (Mettre à jour le fichier) pour charger le certificat X.509 encodé en base 64 que vous avez téléchargé à partir du portail Azure.

    1. Sélectionnez **Enregistrer**.

### <a name="create-workspot-control-test-user"></a>Créer un utilisateur de test Workspot Control

Pour pouvoir se connecter à Workspot Control, les utilisateurs Azure AD doivent être provisionnés dans Workspot Control. Le provisionnement est une tâche manuelle.

**Pour configurer un compte d’utilisateur, effectuez ces étapes :**

1. Connectez-vous à Workspot Control en tant qu’administrateur de la sécurité.

2. Dans la barre d’outils en haut de la page, sélectionnez **Users** (Utilisateurs), puis **Add User** (Ajouter un utilisateur).

    ![Options « Utilisateurs »](./media/workspotcontrol-tutorial/user.png)

3. Dans la fenêtre **Add a New User** (Ajouter un nouvel utilisateur), effectuez ces étapes :

    ![Fenêtre « Add a New User »](./media/workspotcontrol-tutorial/new-user.png)

    1. Dans la zone **First name** (Prénom), entrez le prénom d’un utilisateur, par exemple **Britta**.

    1. Dans la zone de texte **Last name** (Nom), entrez le nom de famille de l’utilisateur, par exemple **Simon**.

    1. Dans la zone **Email** (E-mail), entrez l’adresse e-mail de l’utilisateur, par exemple **Brittasimon@contoso.<i></i>com**.

    1. Sélectionnez le rôle d’utilisateur approprié dans la liste déroulante **Role** (Rôle).

    1. Sélectionnez le groupe d’utilisateurs approprié dans la liste déroulante **Group** (Groupe).

    1. Sélectionnez **Add User** (Ajouter un utilisateur).

## <a name="test-sso"></a>Tester l’authentification unique (SSO)

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

#### <a name="sp-initiated"></a>Lancée par le fournisseur de services :

* Cliquez sur **Tester cette application** dans le portail Azure. Une redirection est effectuée vers l’URL de connexion à Workspot Control, d’où vous pouvez lancer le flux de connexion.  

* Accédez directement à l’URL de connexion à Workspot Control et lancez-y le flux de connexion.

#### <a name="idp-initiated"></a>Lancée par le fournisseur d’identité :

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors automatiquement connecté à l’instance de Workspot Control pour laquelle vous avez configuré l’authentification unique. 

Vous pouvez aussi utiliser Mes applications de Microsoft pour tester l’application dans n’importe quel mode. Si, quand vous cliquez sur la vignette Workspot Control dans Mes applications, le mode Fournisseur de services est configuré, vous êtes redirigé vers la page de connexion de l’application pour lancer le flux de connexion ; si c’est le mode Fournisseur d’identité qui est configuré, vous êtes automatiquement connecté à l’instance de Workspot Control pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez configuré Workspot Control, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).