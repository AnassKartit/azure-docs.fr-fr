---
title: 'Tutoriel : Intégration de l’authentification unique Azure Active Directory à Prezi | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Prezi.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 07/23/2021
ms.author: jeedes
ms.openlocfilehash: 8c665a3c6a990f81c654f3acb7389c3c95435810
ms.sourcegitcommit: 63f3fc5791f9393f8f242e2fb4cce9faf78f4f07
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2021
ms.locfileid: "114689862"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-prezi"></a>Tutoriel : Intégration de l’authentification unique Azure Active Directory à Prezi

Dans ce tutoriel, vous allez découvrir comment intégrer Prezi à Azure Active Directory (Azure AD). En intégrant Prezi à Azure AD, vous pouvez :

* Contrôler qui a accès à Prezi dans Azure AD.
* Permettre aux utilisateurs de se connecter automatiquement à Prezi avec leur compte Azure AD.
* Gérer vos comptes dans le portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/)
* Un abonnement Prezi pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

* Prezi prend en charge l’authentification unique lancée par le fournisseur de services et le fournisseur d’identité.
* Prezi prend en charge le provisionnement d’utilisateurs juste-à-temps.

> [!NOTE]
> L’identificateur de cette application étant une valeur de chaîne fixe, une seule instance peut être configurée dans un locataire.

## <a name="add-prezi-from-the-gallery"></a>Ajouter Prezi à partir de la galerie

Pour configurer l’intégration de Prezi à Azure AD, vous devez ajouter Prezi à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Sélectionnez **Azure Active Directory** dans le volet le plus à gauche.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, entrez **Prezi** dans la zone de recherche.
1. Sélectionnez **Prezi** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-prezi"></a>Configurer et tester l’authentification unique Azure AD pour Prezi

Configurez et testez l’authentification unique Azure AD avec Prezi à l’aide d’un utilisateur de test appelé B.Simon. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur Prezi associé.

Pour configurer et tester l’authentification SSO Azure AD avec Prezi, effectuez les étapes suivantes :

1. [Configurer l’authentification unique Azure AD](#configure-azure-ad-sso) pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. [Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user) pour tester l’authentification unique (SSO) Azure AD avec B.Simon.
    1. [Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user) pour permettre à B.Simon d’utiliser l’authentification unique Azure AD.
1. [Configurer Prezi](#configure-prezi-sso) pour configurer les paramètres de l’authentification unique côté application.
    1. [Créer un utilisateur de test Prezi](#create-a-prezi-test-user) pour avoir un équivalent de B.Simon dans Prezi lié à la représentation Azure AD associée.
1. [Tester l’authentification unique](#test-sso) pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Pour activer l’authentification unique Azure AD dans le portail Azure

1. Dans le portail Azure, accédez à la page d’intégration de l’application **Prezi**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, sélectionnez l’icône de stylo pour modifier les paramètres dans **Configuration SAML de base**.

   ![Modifier les paramètres Configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, l’utilisateur n’a rien à faire, car l’application est déjà intégrée à Azure.

1. Sélectionnez **Définir des URL supplémentaires** et effectuez l’étape suivante si vous souhaitez configurer l’application en mode lancé par le **fournisseur de services** :

    Dans la zone **URL de connexion**, tapez l’URL :  `https://prezi.com/login/sso/`.

1. Sélectionnez **Enregistrer**.

1. L’application Prezi s’attend à recevoir les assertions SAML dans un format spécifique, ce qui vous oblige à ajouter des mappages d’attributs personnalisés à votre configuration des attributs de jetons SAML. La capture d’écran suivante montre la liste des attributs par défaut.

    ![Attributs utilisateur et revendications](common/default-attributes.png)

1. L’application Prezi s’attend aussi à ce que quelques attributs supplémentaires soient repassés dans la réponse SAML, comme indiqué ici. Ces attributs sont également préremplis, mais vous pouvez les examiner pour voir s’ils répondent à vos besoins.
    
    | Nom | Attribut source|
    | ---------------| --------------- |
    | given_name | user.givenname |
    | family_name | user.surname |

1. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, recherchez **Certificat (Base64)** . Sélectionnez **Télécharger** pour télécharger le certificat et l’enregistrer sur votre ordinateur.

    ![Lien de téléchargement du certificat](common/certificatebase64.png)

1. Dans la section **Configurer Prezi**, copiez la ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

Dans cette section, vous allez créer un utilisateur de test appelé B. Simon dans le portail Azure.

1. Dans le portail Azure, dans le volet le plus à gauche, sélectionnez **Azure Active Directory**. Accédez à **Utilisateurs**, puis sélectionnez **Tous les utilisateurs**.
1. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.
1. Dans les propriétés utilisateur, effectuez les étapes suivantes :
   1. Dans la zone **Nom**, entrez **B.Simon**.
   1. Dans la zone **Nom d’utilisateur**, entrez `username@companydomain.extension` (par exemple `B.Simon@contoso.com`).
   1. Cochez la case **Afficher le mot de passe**. Notez la valeur affichée dans la zone **Mot de passe**.
   1. Sélectionnez **Create** (Créer).

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Prezi.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise** > **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Prezi**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B.Simon** dans la liste des utilisateurs, puis cliquez sur le bouton **Sélectionner** en bas de l’écran.
1. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste, puis cliquez sur **Sélectionner** en bas de l’écran.
1. Dans la boîte de dialogue **Ajouter une attribution**, sélectionnez **Affecter**.

## <a name="configure-prezi-sso"></a>Configurer l’authentification unique Prezi

1. Dans une autre fenêtre du navigateur web, connectez-vous à Prezi avec votre compte d’équipe et accédez à la [Console d’administration](https://prezi.com/organizations/manage).

1. Dans la **Console d’administration**, cliquez sur l’onglet **Settings** (Paramètres).

    ![Onglet Paramètres](./media/prezi-tutorial/settings-image.png)

1. Accédez à la section **Single Sign-On (SSO)** (Authentification unique), puis activez le bouton bascule pour activer l’authentification unique.
    
    ![Bouton bascule Authentification unique](./media/prezi-tutorial/single-sign-on.png)

1. Dans la section **Single Sign-On (SSO)** , effectuez les étapes suivantes :

    ![Section Single Sign-On (SSO)](./media/prezi-tutorial/configuration.png)

    1. Dans la zone de texte **Identifier or Issuer URL** (Identificateur ou URL de l’émetteur), collez la valeur de l’**Identificateur Azure AD** que vous avez copié à partir du portail Azure.

    1. Dans la zone **SAML 2.0 Endpoint(HTTP)** (Point de terminaison SAML 2.0 (HTTP)), collez la valeur de l’**URL de connexion** que vous avez copiée à partir du portail Azure.

    1. Ouvrez le **Certificat (base64)** téléchargé à partir du portail Azure dans le Bloc-notes. Copiez le contenu du certificat, puis collez-le dans la zone **Certificate (X.509)** .

    1. Sélectionnez **Enregistrer**.

### <a name="create-a-prezi-test-user"></a>Créer un utilisateur de test Prezi

Dans cette section, un utilisateur appelé Britta Simon est créé dans Prezi. Prezi prend en charge le provisionnement d’utilisateurs juste-à-temps, option activée par défaut. Vous n’avez aucune opération à effectuer dans cette section. S’il n’existe pas encore d’utilisateur dans Prezi, il en est créé un après l’authentification.

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

#### <a name="sp-initiated"></a>Lancée par le fournisseur de services :

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à Prezi, d’où vous pouvez lancer le flux de connexion.  

* Accédez directement à l’URL de connexion à Prezi pour lancer le flux de connexion.

#### <a name="idp-initiated"></a>Lancée par le fournisseur d’identité :

* En cliquant sur **Tester cette application** dans le portail Azure, vous devez être connecté automatiquement à l’instance de Prezi pour laquelle vous avez configuré l’authentification unique. 

Vous pouvez aussi utiliser Mes applications de Microsoft pour tester l’application dans n’importe quel mode. Si, quand vous cliquez sur la vignette Prezi dans Mes applications, le mode Fournisseur de services est configuré, vous êtes redirigé vers la page de connexion de l’application pour lancer le flux de connexion ; s’il s’agit du mode Fournisseur d’identité, vous êtes automatiquement connecté à l’instance de Prezi pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré Prezi, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).
