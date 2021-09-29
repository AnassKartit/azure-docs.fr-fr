---
title: 'Tutoriel : Intégration de l’authentification unique Azure Active Directory à Spotinst | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Spotinst.
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
ms.openlocfilehash: fde459409568884e45c497c3cccb1925694d3277
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124801122"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-spotinst"></a>Tutoriel : Intégration de l’authentification unique Azure Active Directory à Spotinst

Dans ce tutoriel, vous allez apprendre à intégrer Spotinst à Azure Active Directory (Azure AD). Quand vous intégrez Spotinst à Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à Spotinst.
* Permettre à vos utilisateurs de se connecter automatiquement à Spotinst avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Abonnement Spotinst pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

* Spotinst prend en charge l’authentification unique lancée par **le fournisseur de services et le fournisseur d’identité**.

> [!NOTE]
> L’identificateur de cette application étant une valeur de chaîne fixe, une seule instance peut être configurée dans un locataire.

## <a name="add-spotinst-from-the-gallery"></a>Ajouter Spotinst à partir de la galerie

Pour configurer l’intégration de Spotinst à Azure AD, vous devez ajouter Spotinst, disponible dans la galerie, à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Spotinst** dans la zone de recherche.
1. Sélectionnez **Spotinst** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-spotinst"></a>Configurer et tester l’authentification unique Azure AD pour Spotinst

Configurez et testez l’authentification unique Azure AD avec Spotinst à l’aide d’un utilisateur de test appelé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur Spotinst associé.

Pour configurer et tester l’authentification unique Azure AD avec Spotinst, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Spotinst](#configure-spotinst-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test Spotinst](#create-spotinst-test-user)** pour avoir un équivalent de B.Simon dans Spotinst lié à la représentation Azure AD de l’utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **Spotinst**, recherchez la section **Gérer**, puis sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, si vous souhaitez configurer l’application en mode lancé par le fournisseur d’identité, effectuez les étapes suivantes :

   1. Vérifiez que l’**URL de réponse** est définie sur : https://console.spotinst.com/auth/saml.
   1. Dans **État de relais**, entrez votre ID d’organisation Spotinst, que vous pouvez également vérifier sous l’onglet **SSO**.
   1. L’**URL de connexion** doit être vide.

1. Cliquez sur **Enregistrer**.

1. L’application Spotinst s’attend à recevoir les assertions SAML dans un format spécifique, ce qui vous oblige à ajouter des mappages d’attributs personnalisés à votre configuration d’attributs de jeton SAML. La capture d’écran suivante montre la liste des attributs par défaut.

    ![image](common/default-attributes.png)

1. En plus de ce qui précède, l’application Spotinst s’attend à ce que quelques attributs supplémentaires (présentés ci-dessous) soient repassés dans la réponse SAML. Ces attributs sont également préremplis, mais vous pouvez les examiner pour voir s’ils répondent à vos besoins.

    | Nom | Attribut source|
    | -----| --------------- |
    | E-mail | user.mail |
    | FirstName | user.givenname |
    | LastName | user.surname |

1. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, recherchez **XML de métadonnées de fédération** et sélectionnez **Télécharger** pour télécharger le certificat et l’enregistrer sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

1. Dans la section **Configurer Spotinst**, copiez l’URL ou les URL appropriées en fonction de vos besoins.

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Spotinst.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Spotinst**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-spotinst-sso"></a>Configurer l’authentification unique Spotinst

1. Dans une autre fenêtre de navigateur web, connectez-vous à Spotinst en tant qu’administrateur de la sécurité.

2. Cliquez sur **l’icône de l’utilisateur** dans le coin supérieur droit de l’écran et cliquez sur **Settings** (Paramètres).

    ![Capture d’écran montrant l’élément Settings sélectionné à partir de l’icône User.](./media/spotinst-tutorial/settings.png)

3. Cliquez sur l’onglet **SECURITY** (SÉCURITÉ) en haut, puis sélectionnez **Identity Providers** (Fournisseurs d’identité) et suivez ces étapes :

    ![Sécurité de Spotinst](./media/spotinst-tutorial/security.png)

    a. Copiez la valeur de **Relay State** (État de relais) et collez-la dans la zone de texte **État de relais** de la section **Configuration SAML de base** dans le Portail Azure.

    b. Cliquez sur **BROWSE** (PARCOURIR) pour charger le fichier xml de métadonnées que vous avez téléchargé du portail Azure.

    c. Cliquez sur **ENREGISTRER**.

### <a name="create-spotinst-test-user"></a>Créer un utilisateur de test Spotinst

L’objectif de cette section est de créer un utilisateur appelé Britta Simon dans Spotinst.

1. Si vous avez configuré l’application en mode initié par le **fournisseur de service**, procédez comme suit :

   a. Dans une autre fenêtre de navigateur web, connectez-vous à Spotinst en tant qu’administrateur de la sécurité.

   b. Cliquez sur **l’icône de l’utilisateur** dans le coin supérieur droit de l’écran et cliquez sur **Settings** (Paramètres).

    ![Capture d’écran montrant l’élément Settings sélectionné à partir de l’icône User.](./media/spotinst-tutorial/settings.png)

    c. Cliquez sur **Users** (Utilisateurs) et sélectionnez **ADD USER** (AJOUTER UN UTILISATEUR).

    ![Capture d’écran montrant l’élément ADD USER sélectionné à partir de Users.](./media/spotinst-tutorial/add-user.png)

    d. Dans la section d’ajout d’un utilisateur, procédez comme suit :

    ![Capture d’écran affichant la section Add user dans laquelle vous pouvez indiquer les valeurs décrites.](./media/spotinst-tutorial/new-user.png)

    1. Dans la zone de texte **Full Name** (Nom complet), entrez le nom d’un utilisateur, par exemple `BrittaSimon`.

    1. Dans la zone de texte **Email** (E-mail), entrez l’adresse e-mail de l’utilisateur, par exemple `brittasimon@contoso.com`.

    1. Sélectionnez les détails propres à votre organisation pour **Organization Role (Rôle de l’organisation), Account Role (Rôle de compte) et Accounts (Comptes)**.

2. Si vous avez configuré l’application en mode initié par le **fournisseur d’identité**, vous n’avez aucune opération à effectuer dans cette section. Spotinst prend en charge le provisionnement juste-à-temps, option qui est activée par défaut. S’il n’existe pas déjà, un utilisateur est créé lors d’une tentative d’accès à Spotinst.

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

#### <a name="sp-initiated"></a>Lancée par le fournisseur de services :

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à Spotinst, d’où vous pouvez lancer le flux de connexion.  

* Accédez directement à l’URL de connexion à Spotinst pour lancer le flux de connexion.

#### <a name="idp-initiated"></a>Lancée par le fournisseur d’identité :

* Cliquez sur **Tester cette application** dans le portail Azure. Vous devez alors être automatiquement connecté à l’instance de Spotinst pour laquelle vous avez configuré l’authentification unique. 

Vous pouvez aussi utiliser Mes applications de Microsoft pour tester l’application dans n’importe quel mode. Si, quand vous cliquez sur la vignette Spotinst dans Mes applications, le mode Fournisseur de services est configuré, vous êtes redirigé vers la page de connexion de l’application pour lancer le flux de connexion ; s’il s’agit du mode Fournisseur d’identité, vous êtes automatiquement connecté à l’instance de Spotinst pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré Spotinst, vous pouvez appliquer le contrôle de session, qui protège en temps réel contre l’exfiltration et l’infiltration de données sensibles de votre organisation. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).