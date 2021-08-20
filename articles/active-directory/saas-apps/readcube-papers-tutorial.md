---
title: 'Tutoriel : Intégration de l’authentification unique Azure Active Directory à ReadCube Papers | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et ReadCube Papers.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/07/2021
ms.author: jeedes
ms.openlocfilehash: 42d25619ef8f704e49e9001969b2b76dd921e9fb
ms.sourcegitcommit: 30e3eaaa8852a2fe9c454c0dd1967d824e5d6f81
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/22/2021
ms.locfileid: "112463798"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-readcube-papers"></a>Tutoriel : Intégration de l’authentification unique Azure Active Directory à ReadCube Papers

Dans ce tutoriel, vous allez découvrir comment intégrer ReadCube Papers à Azure Active Directory (Azure AD). Quand vous intégrez ReadCube Papers à Azure AD, vous pouvez :

* Contrôler qui a accès à ReadCube Papers dans Azure AD.
* Permettre à vos utilisateurs de se connecter automatiquement à ReadCube Papers avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement ReadCube Papers pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

* ReadCube Papers prend en charge l’authentification unique lancée par le **fournisseur de services**.
* ReadCube Papers prend en charge l’attribution d’utilisateurs **Juste-à-temps**.

> [!NOTE]
> L’identificateur de cette application étant une valeur de chaîne fixe, une seule instance peut être configurée dans un locataire.

## <a name="add-readcube-papers-from-the-gallery"></a>Ajouter ReadCube Papers à partir de la galerie

Pour configurer l’intégration de ReadCube Papers à Azure AD, vous devez ajouter ReadCube Papers à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **ReadCube Papers** dans la zone de recherche.
1. Sélectionnez **ReadCube Papers** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-readcube-papers"></a>Configurer et tester l’authentification unique Azure AD pour ReadCube Papers

Configurez et testez l’authentification unique Azure AD avec ReadCube Papers pour une utilisatrice de test appelée **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir une relation entre un utilisateur Azure AD et l’utilisateur associé dans ReadCube Papers.

Pour configurer et tester l’authentification unique Azure AD avec ReadCube Papers, procédez comme suit :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique ReadCube Papers](#configure-readcube-papers-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer une utilisatrice de test pour ReadCube Papers](#create-readcube-papers-test-user)** afin de disposer dans ReadCube Papers d’un équivalent de B.Simon lié à la représentation Azure AD de l’utilisatrice.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **ReadCube Papers**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, effectuez l’étape suivante :

    a. Dans la zone de texte **URL de connexion**, tapez l’URL : `https://app.readcube.com`

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à ReadCube Papers.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **ReadCube Papers**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-readcube-papers-sso"></a>Configurer l’authentification unique ReadCube Papers

Pour configurer l’authentification unique côté **ReadCube Papers**, vous devez envoyer l’**URL des métadonnées de fédération d’application** à l’[équipe de support technique ReadCube Papers](mailto:sso-support@readcube.com). Celle-ci change ce paramètre afin que la connexion SSO SAML fonctionne correctement des deux côtés.

### <a name="create-readcube-papers-test-user"></a>Créer un utilisateur de test ReadCube Papers

Dans cette section, un utilisateur appelé Britta Simon est créé dans ReadCube Papers. ReadCube Papers prend en charge l’attribution d’utilisateurs juste-à-temps (option activée par défaut). Vous n’avez aucune opération à effectuer dans cette section. Si l’utilisateur souhaité n’existe pas déjà dans ReadCube Papers, un utilisateur est créé après l’authentification.

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

> [!NOTE]
> Avant de procéder au test, vérifiez auprès de l’[équipe de support technique ReadCube Papers](mailto:sso-support@readcube.com) que l’authentification unique est configurée côté ReadCube.

* Cliquez sur **Tester cette application** dans le portail Azure. Une redirection est effectuée vers l’URL de connexion à ReadCube Papers, où vous pouvez lancer le flux de connexion. 

* Accédez directement à l’URL de connexion à ReadCube Papers, puis lancez le flux de connexion à partir de cet emplacement.

* Vous pouvez utiliser Mes applications de Microsoft. Un clic sur la vignette ReadCube Papers dans Mes applications vous redirige vers l’URL de connexion à ReadCube Papers. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré ReadCube Papers, vous pouvez appliquer le contrôle de session qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).
