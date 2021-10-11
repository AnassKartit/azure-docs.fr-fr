---
title: 'Tutoriel : Intégration de l’authentification unique Azure AD à Hiretual-SSO'
description: Apprenez à configurer l’authentification unique entre Azure Active Directory et Hiretual-SSO.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/29/2021
ms.author: jeedes
ms.openlocfilehash: c459db2d895042d9edfa4bada8b730c96b95ed55
ms.sourcegitcommit: 03e84c3112b03bf7a2bc14525ddbc4f5adc99b85
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/03/2021
ms.locfileid: "129403199"
---
# <a name="tutorial-azure-ad-sso-integration-with-hiretual-sso"></a>Tutoriel : Intégration de l’authentification unique Azure AD à Hiretual-SSO

Dans ce tutoriel, vous allez apprendre à intégrer Hiretual-SSO à Azure Active Directory (Azure AD). Quand vous intégrez Hiretual-SSO à Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à Hiretual-SSO.
* Permettre aux utilisateurs de se connecter automatiquement à Hiretual-SSO avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement Hiretual-SSO pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

* Hiretual-SSO prend en charge l’authentification unique lancée par **le fournisseur de services et le fournisseur d’identité**.

> [!NOTE]
> L’identificateur de cette application étant une valeur de chaîne fixe, une seule instance peut être configurée dans un locataire.

## <a name="add-hiretual-sso-from-the-gallery"></a>Ajouter Hiretual-SSO à partir de la galerie

Pour configurer l’intégration d’Hiretual-SSO à Azure AD, vous devez ajouter Hiretual-SSO à partir de la galerie à votre liste d’applications SaaS managées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Hiretual-SSO** dans la zone de recherche.
1. Sélectionnez **Hiretual-SSO** dans le panneau de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-hiretual-sso"></a>Configurer et tester l’authentification unique Azure AD pour Hiretual-SSO

Configurez et testez l’authentification unique Azure AD avec Hiretual-SSO pour un utilisateur test appelé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur Hiretual-SSO associé.

Pour configurer et tester l’authentification unique Azure AD avec Hiretual-SSO, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l'authentification unique Hiretual-SSO](#configure-hiretual-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur test Hiretual-SSO](#create-hiretual-sso-test-user)** pour avoir un équivalent de B.Simon dans Hiretual-SSO, lié à la représentation Azure AD de l’utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **Hiretual-SSO**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, si vous souhaitez configurer l’application en mode lancé par le **fournisseur d’identité**, effectuez l’étape suivante :

    a. Dans la zone de texte **URL de réponse**, tapez une URL au format suivant : `https://api.hiretual.com/v1/users/saml/login/<teamId>`

1. Si vous souhaitez configurer l’application en **mode démarré par le fournisseur de services**, cliquez sur **Définir des URL supplémentaires**, puis effectuez les étapes suivantes :

    Dans la zone de texte **URL de connexion**, tapez l’URL : `https://app.hiretual.com/`

    > [!NOTE]
    > Cette valeur n’est pas la valeur réelle. Mettez à jour la valeur avec l’URL de réponse réelle. Pour obtenir ces valeurs, contactez l’[équipe du support de Hiretual-SSO](mailto:support@hiretual.com). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

1. L’application Hiretual-SSO attend les assertions SAML dans un format spécifique, ce qui vous oblige à ajouter des mappages d’attributs personnalisés à votre configuration d’attributs de jetons SAML. La capture d’écran suivante montre la liste des attributs par défaut.

    ![image](common/default-attributes.png)

1. En plus de ce qui précède, l’application Hiretual-SSO s’attend à ce que quelques attributs supplémentaires soient repassés dans la réponse SAML, comme indiqué ci-dessous. Ces attributs sont également préremplis, mais vous pouvez les examiner pour voir s’ils répondent à vos besoins.
    
    | Nom | Attribut source |
    | ---------- | --------- |
    | firstName | user.givenname |
    | title | user.jobtitle |
    | lastName | user.surname |

1. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur le bouton Copier pour copier l’**URL des métadonnées de fédération d’application**, puis enregistrez-la sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user&quot;></a>Créer un utilisateur de test Azure AD

Dans cette section, vous allez créer un utilisateur de test appelé B. Simon dans le portail Azure.

1. Dans le volet gauche du Portail Azure, sélectionnez **Azure Active Directory**, **Utilisateurs**, puis **Tous les utilisateurs**.
1. Sélectionnez **Nouvel utilisateur** dans la partie supérieure de l’écran.
1. Dans les propriétés **Utilisateur**, effectuez les étapes suivantes :
   1. Dans le champ **Nom**, entrez `B.Simon`.  
   1. Dans le champ **Nom de l’utilisateur**, entrez username@companydomain.extension. Par exemple : `B.Simon@contoso.com`.
   1. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ **Mot de passe**.
   1. Cliquez sur **Créer**.

### <a name=&quot;assign-the-azure-ad-test-user&quot;></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Hiretual-SSO.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Hiretual-SSO**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name=&quot;configure-hiretual-sso&quot;></a>Configurer Hiretual-SSO

1. Connectez-vous à votre site d’entreprise Hiretual-SSO en tant qu’administrateur.

1. Accédez à **Security & Compliance** > **Single Sign-On**.

1. Dans la page **SAML2.0 Authentication**, effectuez les étapes suivantes :

    ![Capture d’écran montrant la page configuration SSO.](./media/hiretual-tutorial/configuration.png &quot;SSO Configuration")

    1. Dans la zone de texte **SAML2.0 SSO URL**, collez la valeur **User access URL** que vous avez copiée dans le portail Azure.

    1. Copiez la valeur de l’**ID d’entité** à partir du fichier de métadonnées et collez-la dans la zone de texte **Identity Provider Issuer**.

    1. Copiez la valeur de **X509 Certificate** à partir du fichier de métadonnées et collez le contenu dans la zone de texte **Certificate**.

    1. Remplissez manuellement les attributs requis en fonction de vos besoins, puis cliquez sur **Save**.

    1. Activez le bouton **Single Sign-On Connection Status**.

    1. Testez d’abord votre intégration d’authentification unique, puis activez le bouton **Admin SP-Initiated Single Sign-On**. 

    > [!NOTE]
    > Si votre configuration d’authentification unique contient des erreurs ou si vous avez des difficultés à vous connecter à l’application web/extension Hiretual-SSO après avoir connecté l’authentification unique lancée par le fournisseur de services de l’administrateur, contactez l’[équipe de support Hiretual-SSO](mailto:support@hiretual.com).
    
### <a name="create-hiretual-sso-test-user"></a>Créer un utilisateur test Hiretual-SSO

Dans cette section, vous allez créer une utilisatrice appelée Britta Simon dans Hiretual-SSO. Collaborez avec l’[équipe de support Hiretual-SSO](mailto:support@hiretual.com) pour ajouter les utilisateurs à la plateforme Hiretual-SSO. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

#### <a name="sp-initiated"></a>Lancée par le fournisseur de services :

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL d’authentification Hiretual-SSO, à partir de laquelle vous pouvez lancer le processus de connexion.  

* Accédez directement à l’URL d’authentification Hiretual-SSO et lancez le processus de connexion.

#### <a name="idp-initiated"></a>Lancée par le fournisseur d’identité :

* Cliquez sur **Tester cette application** dans le portail Azure, ce qui devrait vous connecter automatiquement à l’application Hiretual-SSO pour laquelle vous avez configuré l’authentification unique. 

Vous pouvez aussi utiliser Mes applications de Microsoft pour tester l’application dans n’importe quel mode. Quand vous cliquez sur la vignette Hiretual-SSO dans Mes applications, si le mode Fournisseur de services est configuré, vous êtes redirigé vers la page d’authentification de l’application pour lancer le processus de connexion. Si le mode Fournisseur d’identité est configuré, vous êtes automatiquement connecté à l’application Hiretual-SSO pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez configuré l’authentification unique Hiretual-SSO, vous pouvez appliquer le contrôle de session, qui protège l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).