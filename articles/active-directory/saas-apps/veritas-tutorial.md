---
title: 'Didacticiel : intégration d’Azure Active Directory à Veritas Enterprise Vault.cloud SSO | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Veritas Enterprise Vault.cloud SSO.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/22/2021
ms.author: jeedes
ms.openlocfilehash: 2796928316197bd48723253dbc97dfcd34ac95e8
ms.sourcegitcommit: 3f684a803cd0ccd6f0fb1b87744644a45ace750d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2021
ms.locfileid: "106222280"
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a>Didacticiel : intégration d’Azure Active Directory à Veritas Enterprise Vault.cloud SSO

Dans ce didacticiel, vous allez apprendre à intégrer Veritas Enterprise Vault.cloud SSO à Azure Active Directory (Azure AD). Lorsque vous intégrez Veritas Enterprise Vault.cloud SSO à Azure AD, vous pouvez :

* Dans Azure AD, contrôlez qui a accès à Veritas Enterprise Vault.cloud SSO.
* Autoriser les utilisateurs à se connecter automatiquement à Veritas Enterprise Vault.cloud SSO avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à Veritas Enterprise Vault.cloud SSO, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un [compte gratuit](https://azure.microsoft.com/free/).
* Abonnement Veritas Enterprise Vault.cloud SSO pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Veritas Enterprise Vault.cloud SSO prend en charge l’authentification unique lancée par le **fournisseur de services**.

> [!NOTE]
> L’identificateur de cette application étant une valeur de chaîne fixe, une seule instance peut être configurée dans un locataire.

## <a name="add-veritas-enterprise-vaultcloud-sso-from-the-gallery"></a>Ajouter Veritas Enterprise SSO Vault.cloud à partir de la galerie

Pour configurer son intégration à Azure AD, vous devez ajouter Veritas Enterprise Vault.cloud SSO à votre liste d’applications SaaS gérées, à partir de la galerie.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, entrez **Veritas Enterprise Vault.cloud SSO** dans la zone de recherche.
1. Sélectionnez **Veritas Enterprise Vault.cloud SSO** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-veritas-enterprise-vaultcloud-sso"></a>Configurer et tester l’authentification unique Azure AD pour Veritas Enterprise Vault.cloud SSO

Configurez et testez l’authentification unique Azure AD avec Veritas Enterprise Vault.cloud SSO à l’aide d’un utilisateur de test appelé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur associé dans Veritas Enterprise Vault.cloud SSO.

Pour configurer et tester l’authentification unique Azure AD avec Veritas Enterprise Vault.cloud SSO, vous devez :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Veritas Enterprise Vault.cloud SSO](#configure-veritas-enterprise-vaultcloud-sso-sso)** pour configurer les paramètres d’authentification unique côté application.
    1. **[Créer un utilisateur de test Veritas Enterprise Vault.cloud SSO](#create-veritas-enterprise-vaultcloud-sso-test-user)** pour avoir un équivalent de B. Simon dans Veritas Enterprise Vault.cloud SSO lié à la représentation Azure AD associée.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Sur le portail Azure, dans la page d’intégration d’application **Veritas Enterprise Vault.cloud SSO**, recherchez la section **Gérer**, puis sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    a. Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`.

    b. Dans la zone **Identificateur**, entrez l’une des URL du Centre de données :

    | Centre de données| URL |
    |----------|----|
    | Amérique du Nord| `https://auth.lax.archivecloud.net` |
    | Europe | `https://auth.ams.archivecloud.net` |
    | Asie-Pacifique| `https://auth.syd.archivecloud.net`|

    c. Dans la zone de texte **URL de réponse**, entrez l’une des URL du Centre de données :

    | Centre de données| URL |
    |----------|----|
    | Amérique du Nord| `https://auth.lax.archivecloud.net` |
    | Europe | `https://auth.ams.archivecloud.net` |
    | Asie-Pacifique| `https://auth.syd.archivecloud.net`|

    > [!NOTE]
    > Cette valeur n’est pas la valeur réelle. Mettez à jour cette valeur avec l’URL d’authentification réelle. Contactez l’[équipe de support Veritas Enterprise Vault.cloud SSO](https://www.veritas.com/support/.html) pour obtenir cette valeur. Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

5. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le **Certificat (Base64)** en fonction des options définies par rapport à vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/certificatebase64.png)

6. Dans la section **Configurer Veritas Enterprise Vault.cloud SSO**, copiez la ou les URL appropriées en fonction de vos besoins.

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

Dans cette section, vous allez autoriser B. Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Veritas Enterprise Vault.cloud SSO.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Veritas Enterprise Vault.cloud SSO**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-veritas-enterprise-vaultcloud-sso-sso"></a>Configurer l’authentification unique Veritas Enterprise Vault.cloud SSO

Pour configurer l’authentification unique côté **Veritas Enterprise Vault.cloud SSO**, vous devez envoyer le **Certificat (Base64)** téléchargé et les URL copiées correspondantes du portail Azure à l’[équipe du support technique Veritas Enterprise Vault.cloud SSO](https://www.veritas.com/support/.html). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-veritas-enterprise-vaultcloud-sso-test-user"></a>Créer un utilisateur de test Veritas Enterprise Vault.cloud SSO

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans Veritas Enterprise Vault.cloud SSO. Collaborez avec l’[équipe du support technique Veritas Enterprise Vault.cloud SSO](https://www.veritas.com/support/.html) pour ajouter les utilisateurs à la plateforme Veritas Enterprise Vault.cloud SSO. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

## <a name="test-sso"></a>Tester l’authentification unique (SSO)

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à Veritas Enterprise Vault.cloud SSO, d’où vous pouvez lancer le flux de connexion. 

* Accédez directement à l’URL de connexion à Veritas Enterprise Vault.cloud SSO pour lancer le flux de connexion.

* Vous pouvez utiliser Mes applications de Microsoft. Lorsque vous cliquez sur la vignette Veritas Enterprise Vault.cloud SSO dans Mes applications, vous êtes redirigé vers l’URL d’authentification unique de Veritas Enterprise Vault.cloud SSO. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré Veritas Enterprise Vault.cloud SSO, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).
