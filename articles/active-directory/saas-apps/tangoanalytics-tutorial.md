---
title: 'Didacticiel : intégration d’Azure Active Directory à Tango Analytics | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Tango Analytics.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/07/2021
ms.author: jeedes
ms.openlocfilehash: 8a654d33b73e0346b468c7401f1c4b0fd5988637
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124772407"
---
# <a name="tutorial-azure-active-directory-integration-with-tango-analytics"></a>Didacticiel : intégration d’Azure Active Directory à Tango Analytics

Dans ce tutoriel, vous allez apprendre à intégrer Tango Analytics à Azure Active Directory (Azure AD). Quand vous intégrez Tango Analytics à Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à Tango Analytics.
* Permettre à vos utilisateurs de se connecter automatiquement à Tango Analytics avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement Tango Analytics pour lequel l’authentification unique (SSO) est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Tango Analytics prend en charge l’authentification unique lancée par le **fournisseur d’identité**.

> [!NOTE]
> L’identificateur de cette application étant une valeur de chaîne fixe, une seule instance peut être configurée dans un locataire.

## <a name="add-tango-analytics-from-the-gallery"></a>Ajout de Tango Analytics à partir de la galerie

Pour configurer l’intégration de Tango Analytics à Azure AD, vous devez ajouter Tango Analytics à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Tango Analytics** dans la zone de recherche.
1. Sélectionnez **Tango Analytics** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-tango-analytics"></a>Configurer et tester Azure AD SSO pour Tango Analytics

Configurez et testez Azure AD SSO avec Tango Analytics pour un utilisateur de test nommé **B.Simon**. Pour que l’authentification unique (SSO) fonctionne, vous devez établir une relation entre un utilisateur Azure AD et l’utilisateur associé dans Tango Analytics.

Pour configurer et tester Azure AD SSO avec Tango Analytics, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Tango Analytics](#configure-tango-analytics-sso)**  : pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test Tango Analytics](#create-tango-analytics-test-user)** pour avoir un équivalent de B.Simon dans Tango Analytics lié à la représentation de l’utilisateur Azure AD.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **Tango Analytics**, recherchez la section **Gérer**, puis sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Sur la page **Configurer l’authentification unique avec SAML**, effectuez les étapes suivantes :

    a. Dans la zone de texte **Identificateur**, tapez la valeur : `TACORE_SSO`

    b. Dans la zone **URL de réponse**, tapez l’URL : `https://mts.tangoanalytics.com/saml2/sp/acs/post`

5. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies selon vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

6. Dans la section **Configurer Tango Analytics**, copiez l’URL ou les URL appropriées, selon vos besoins.

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Tango Analytics.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Tango Analytics**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-tango-analytics-sso"></a>Configurer l’authentification unique Tango Analytics

Pour configurer l’authentification unique côté **Tango Analytics**, vous devez envoyer le **XML des métadonnées de fédération** téléchargé et les URL copiées dans le portail Azure à l’[équipe du support technique Tango Analytics](mailto:support@tangoanalytics.com). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-tango-analytics-test-user"></a>Créer un utilisateur de test Tango Analytics

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans Tango Analytics. Travailler avec [l’équipe de support technique Tango Analytics](mailto:support@tangoanalytics.com) pour ajouter des utilisateurs dans la plateforme Tango Analytics. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

## <a name="test-sso"></a>Tester l’authentification unique (SSO)

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes.

* Cliquez sur Tester cette application dans le portail Azure. Vous devez être connecté automatiquement à l’instance de Tango Analytics pour laquelle vous avez configuré l’authentification unique.

* Vous pouvez utiliser Mes applications de Microsoft. Le fait de cliquer sur la vignette Tango Analytics dans Mes applications doit vous connecter automatiquement à l’application Tango Analytics pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez configuré Tango Analytics, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).