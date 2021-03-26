---
title: 'Tutoriel : Intégration d’Azure Active Directory à HubSpot | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et HubSpot.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/27/2020
ms.author: jeedes
ms.openlocfilehash: 91dfdcef01a121c8282b8fad2e75f67989b75dc3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98733231"
---
# <a name="tutorial-azure-active-directory-integration-with-hubspot"></a>Tutoriel : Intégration d’Azure AD à HubSpot

Dans ce tutoriel, vous allez découvrir comment intégrer HubSpot à Azure Active Directory (Azure AD). Quand vous intégrez HubSpot à Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à HubSpot.
* Autoriser les utilisateurs à se connecter automatiquement à HubSpot avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD avec HubSpot, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’abonnement Azure AD, créez un [compte gratuit](https://azure.microsoft.com/free/) avant de commencer.
* Un abonnement HubSpot pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test et intégrer HubSpot à Azure AD.

HubSpot prend en charge les fonctionnalités suivantes :

* **Authentification unique lancée par le fournisseur de services**
* **Authentification unique démarrée par le fournisseur d’identité**

## <a name="adding-hubspot-from-the-gallery"></a>Ajout de HubSpot à partir de la galerie

Pour configurer l’intégration de HubSpot à Azure AD, vous devez ajouter HubSpot à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **HubSpot** dans la zone de recherche.
1. Sélectionnez **HubSpot** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-hubspot"></a>Configurer et tester l’authentification unique Azure AD pour HubSpot

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec HubSpot sur un utilisateur de test appelé **Britta Simon**. Pour que l’authentification unique fonctionne, vous devez établir une relation entre un utilisateur Azure AD et l’utilisateur HubSpot associé.

Pour configurer et tester l’authentification unique Azure AD avec HubSpot, vous devez suivre les indications des sections suivantes :

| Tâche | Description |
| --- | --- |
| **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** | Autorise les utilisateurs à utiliser cette fonctionnalité. |
| **[Configurer l’authentification unique HubSpot](#configure-hubspot-single-sign-on)** | Configure les paramètres d’authentification unique dans l’application. |
| **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** | Teste l’authentification unique Azure AD pour un utilisateur appelé Britta Simon. |
| **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** | Active Britta Simon pour lui permettre d'utiliser l’authentification unique Azure AD. |
| **[Créer un utilisateur de test HubSpot](#create-a-hubspot-test-user)** | Crée un équivalent de Britta Simon dans HubSpot lié à la représentation Azure AD associée. |
| **[Tester l’authentification unique](#test-single-sign-on)** | Vérifie que la configuration fonctionne. |

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

1. Dans le portail Azure, accédez à la page d’intégration de l’application **HubSpot**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon pour **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans le volet **Configuration SAML de base**, pour configurer le *mode initié par le fournisseur d’identité*, effectuez les étapes suivantes :

    1. Dans le champ **Identificateur**, entrez une URL au format suivant : https:\//api.hubspot.com/login-api/v1/saml/login?portalId=\<CUSTOMER ID\>.

    1. Dans le champ **URL de réponse**, entrez une URL au format suivant : https:\//api.hubspot.com/login-api/v1/saml/acs?portalId=\<CUSTOMER ID\>.

    ![Informations d’authentification unique dans Domaine et URL HubSpot](common/idp-intiated.png)

    > [!NOTE]
    > Pour le format des URL, vous pouvez aussi vous référer aux modèles figurant dans le volet **Configuration SAML de base** sur le portail Azure.

1. Pour configurer l’application en mode *initié par le fournisseur de services* :

    1. Sélectionnez **Définir des URL supplémentaires**.

    1. Dans le champ **URL de connexion**, entrez **https:\//app.hubspot.com/login**.

    ![Option Définir des URL supplémentaires](common/metadata-upload-additional-signon.png)

1. Dans le volet **Configurer l’authentification unique avec SAML**, sous la section **Certificat de signature SAML**, sélectionnez **Télécharger** en regard de **Certificat (Base64)** . Sélectionnez une option de téléchargement en fonction de vos exigences. Enregistrez le certificat sur votre ordinateur.

    ![Option de téléchargement du certificat (Base64)](common/certificatebase64.png)

1. Dans la section **Configurer HubSpot**, copiez les URL suivantes en fonction de vos besoins :

    * URL de connexion
    * Identificateur Azure AD
    * URL de déconnexion

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

### <a name="configure-hubspot-single-sign-on"></a>Configurer l’authentification unique HubSpot

1. Ouvrez un nouvel onglet dans votre navigateur et connectez-vous à votre compte d’administrateur HubSpot.

1. Sélectionnez l’icône des **paramètres** dans le coin supérieur droit de la page.

    ![Icône des paramètres dans HubSpot](./media/hubspot-tutorial/config1.png)

1. Sélectionnez **Account Defaults** (Valeurs par défaut du compte).

    ![Option Account Defaults (Valeurs par défaut du compte) dans HubSpot](./media/hubspot-tutorial/config2.png)

1. Faites défiler jusqu’à la section **Security** (Sécurité), puis sélectionnez **Set up** (Configurer).

    ![Option Set up (Configurer) dans HubSpot](./media/hubspot-tutorial/config3.png)

1. Dans la section **Set up single sign-on** (Configurer l’authentification unique), effectuez les étapes suivantes :

    1. Dans le champ **Audience URl (Service Provider Entity ID)** (URI de l’audience (ID de l’entité de fournisseur de services)), sélectionnez **Copy** (Copier) pour copier la valeur. Sur le portail Azure, dans le volet **Configuration SAML de base**, collez la valeur dans la zone **Identificateur**.

    1. Dans le champ **URl, ACS, Recipient, or Redirect** (URL de connexion, ACS, destinataire ou redirection), sélectionnez **Copy** (Copier) pour copier la valeur. Sur le portail Azure, dans le volet **Configuration SAML de base**, collez la valeur dans la zone **URL de réponse**.

    1. Dans HubSpot, dans le champ **Identity Provider Identifier or Issuer URL** (URL de l’identificateur ou l’émetteur du fournisseur d’identité), collez l’**identificateur Azure AD** que vous avez copié sur le portail Azure.

    1. Dans HubSpot, dans le champ **Identity Provider Single Sign-On URL** (URL d’authentification unique du fournisseur d’identité), collez l’**URL de connexion** que vous avez copiée dans le portail Azure.

    1. Dans le Bloc-notes Windows, ouvrez le fichier Certificate(Base64) que vous avez téléchargé. Sélectionnez puis copiez le contenu du fichier. Ensuite, dans HubSpot, collez-le dans le champ **X.509 Certificate**.

    1. Sélectionnez **Vérifier**.

        ![Section Set up single sign-on (Configurer l’authentification unique) dans HubSpot](./media/hubspot-tutorial/config4.png)

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à HubSpot.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **HubSpot**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

### <a name="create-a-hubspot-test-user"></a>Créer un utilisateur de test HubSpot

Pour se connecter à HubSpot, les utilisateurs d’Azure AD doivent être attribués dans HubSpot. Dans HubSpot, l’attribution des utilisateurs doit s’effectuer manuellement.

Pour provisionner un compte d’utilisateur dans HubSpot :

1. Connectez-vous à votre site d’entreprise HubSpot en tant qu’administrateur.

1. Sélectionnez l’icône des **paramètres** dans le coin supérieur droit de la page.

    ![Icône des paramètres dans HubSpot](./media/hubspot-tutorial/config1.png)

1. Sélectionnez **Users & Teams** (Utilisateurs et équipes).

    ![Option Users & Teams (Utilisateurs et équipes) dans HubSpot](./media/hubspot-tutorial/user1.png)

1. Sélectionnez **Create user** (Créer un utilisateur).

    ![Option Create user (Créer un utilisateur) dans HubSpot](./media/hubspot-tutorial/user2.png)

1. Dans le champ **Add email addess(es)** (Ajouter des adresses e-mail), entrez l’adresse e-mail de l’utilisateur au format brittasimon\@contoso.com, puis sélectionnez **Next** (Suivant).

    ![Champ Add email addess(es) (Ajouter des adresses e-mail) dans HubSpot](./media/hubspot-tutorial/user3.png)

1. Dans la section **Create users** (Créer des utilisateurs), sélectionnez chaque onglet. Sous chaque onglet, définissez les options et les autorisations nécessaires pour l’utilisateur. Ensuite, sélectionnez **Suivant**.

    ![Onglets de la section Create users (Créer des utilisateurs) dans HubSpot](./media/hubspot-tutorial/user4.png)

1. Pour envoyer l’invitation à l’utilisateur, sélectionnez **Send** (Envoyer).

    ![Option Send (Envoyer) dans HubSpot](./media/hubspot-tutorial/user5.png)

    > [!NOTE]
    > L’utilisateur est activé dès qu’il accepte l’invitation.

### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

#### <a name="sp-initiated"></a>Lancée par le fournisseur de services :

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à HubSpot, à partir de laquelle vous pouvez lancer le flux de connexion.  

* Accédez directement à l’URL de connexion HubSpot pour lancer le processus de connexion.

#### <a name="idp-initiated"></a>Lancée par le fournisseur d’identité :

* Dans le portail Azure, cliquez sur **Tester cette application**. Vous êtes alors automatiquement connecté à l’instance HubSpot pour laquelle vous avez configuré l’authentification unique 

Vous pouvez aussi utiliser Mes applications de Microsoft pour tester l’application dans n’importe quel mode. Si, quand vous cliquez sur la vignette HubSpot dans Mes applications, le mode Fournisseur de services est configuré, vous êtes redirigé vers la page de connexion de l’application pour lancer le flux de connexion ; s’il s’agit du mode Fournisseur d’identité, vous êtes automatiquement connecté à l’instance HubSpot pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).


## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré HubSpot, vous pouvez appliquer le contrôle de session qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).