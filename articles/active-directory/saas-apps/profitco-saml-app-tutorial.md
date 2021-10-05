---
title: 'Tutoriel : Intégration de l’authentification unique Azure AD à Profit.co'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Profit.co.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/20/2021
ms.author: jeedes
ms.openlocfilehash: c95eae59edc75e5d4394d4bbd9b07d6168f7b566
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128548765"
---
# <a name="tutorial-azure-ad-sso-integration-with-profitco"></a>Tutoriel : Intégration de l’authentification unique Azure AD à Profit.co

Dans ce tutoriel, vous allez découvrir comment intégrer Profit.co à Azure Active Directory (Azure AD). Quand vous intégrez Profit.co à Azure AD, vous pouvez :

* Contrôler qui dans Azure AD a accès à Profit.co.
* Permettre aux utilisateurs de se connecter automatiquement à Profit.co avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central, le portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement Profit.co pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

* Profit.co prend en charge l’authentification unique initiée par le fournisseur d’identité.

## <a name="add-profitco-from-the-gallery"></a>Ajouter Profit.co à partir de la galerie

Pour configurer l’intégration de Profit.co dans Azure AD, vous devez ajouter Profit.co à votre liste d’applications SaaS gérées à partir de la galerie.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire, ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Profit.co** dans la zone de recherche.
1. Sélectionnez **Profit.co** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-profitco"></a>Configurer et tester l’authentification unique Azure AD pour Profit.co

Configurez et testez l’authentification unique (SSO) Azure AD avec Profit.co à l’aide d’un utilisateur de test appelé **B.Simon**. Pour que l’authentification unique fonctionne, établissez un lien entre un utilisateur Azure AD et l’utilisateur Profit.co associé.

Voici la procédure générale pour configurer et tester l’authentification unique Azure AD avec Profit.co :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Profit.co](#configure-profitco-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test Profit.co](#create-a-profitco-test-user)** pour avoir un équivalent de B.Simon dans Profit.co lié à la représentation Azure AD de l’utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **Profit.co** et recherchez la section **Gérer**. Sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, sélectionnez l’icône de crayon pour **Configuration SAML de base** afin de modifier les paramètres.

   ![Capture d’écran de la page Configurer l’authentification unique avec SAML, avec l’icône de crayon mise en évidence](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, l’application est préconfigurée et les URL nécessaires sont déjà préremplies dans Azure. Les utilisateurs doivent enregistrer la configuration en sélectionnant le bouton **Enregistrer**.

1. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, sélectionnez le bouton **Copier**. Cela copie l’**URL des métadonnées de fédération d’application** et l’enregistre sur votre ordinateur.

    ![Capture d’écran du certificat de signature SAML, avec le bouton Copier mis en évidence](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

Dans cette section, vous allez créer un utilisateur de test appelé B. Simon sur le portail Azure.

1. Dans le volet gauche du portail Azure, sélectionnez **Azure Active Directory** > **Utilisateurs** > **Tous les utilisateurs**.
1. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.
1. Dans les propriétés **Utilisateur**, effectuez les étapes suivantes :
   1. Dans le champ **Nom**, entrez `B.Simon`.  
   1. Dans le champ **Nom de l’utilisateur**, entrez username@companydomain.extension. Par exemple : `B.Simon@contoso.com`.
   1. Cochez la case **Afficher le mot de passe**, puis notez la valeur figurant dans le champ **Mot de passe**.
   1. Sélectionnez **Create** (Créer).

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant un accès à Profit.co.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise** > **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Profit.co**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**. Dans la boîte de dialogue **Ajouter une attribution**, sélectionnez **Utilisateurs et groupes**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B.Simon** dans la liste des utilisateurs. Ensuite, choisissez le bouton **Sélectionner** en bas de l’écran.
1. Si vous attendez une valeur de rôle dans l’assertion SAML, dans la boîte de dialogue **Sélectionner un rôle**, sélectionnez le rôle approprié pour l’utilisateur dans la liste. Ensuite, choisissez le bouton **Sélectionner** en bas de l’écran.
1. Dans la boîte de dialogue **Ajouter une attribution**, sélectionnez **Affecter**.

## <a name="configure-profitco-sso"></a>Configurer l’authentification unique pour Profit.co

Pour configurer l’authentification unique côté Profit.co, vous devez envoyer l’URL des métadonnées de fédération de l’application à l’[équipe du support technique Profit.co](mailto:support@profit.co). Celle-ci configure ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-a-profitco-test-user"></a>Créer un utilisateur de test Profit.co

Dans cette section, vous allez créer un utilisateur appelé B.Simon dans Profit.co. Rapprochez-vous de l’[équipe du support technique Profit.co](mailto:support@profit.co) pour ajouter les utilisateurs dans la plateforme Profit.co. Vous ne pouvez pas utiliser l’authentification unique avant d’avoir créé et activé les utilisateurs.

## <a name="test-sso"></a>Tester l’authentification unique (SSO)

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes.

* Cliquez sur Tester cette application dans le portail Azure. Vous êtes alors automatiquement connecté à l’instance Profit.co pour laquelle vous avez configuré l’authentification unique.

* Vous pouvez utiliser Mes applications de Microsoft. Quand vous cliquez sur la vignette Profit.co dans Mes applications, vous devez être connecté automatiquement à l’application Profit.co pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré Profit.co, vous pouvez appliquer le contrôle de session, qui protège votre organisation contre l’exfiltration et l’infiltration de ses données sensibles en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).