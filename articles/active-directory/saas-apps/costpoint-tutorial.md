---
title: 'Tutoriel : Intégration de Azure Active Directory à Costpoint | Microsoft Docs'
description: Découvrez comment configurer l'authentification unique entre Azure Active Directory et Costpoint.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/11/2021
ms.author: jeedes
ms.openlocfilehash: 781d1509b69ecb09189772f4a31d4872201d0b2e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "101652927"
---
# <a name="tutorial-integrate-costpoint-with-azure-active-directory"></a>Tutoriel : Intégrer Costpoint à Azure Active Directory

Dans ce tutoriel, vous allez apprendre à intégrer Costpoint avec Azure Active Directory (Azure AD). Quand vous intégrez Costpoint à Azure AD, vous pouvez :

* Contrôler qui a accès à Costpoint dans Azure AD.
* Permettre à vos utilisateurs de se connecter automatiquement à Costpoint avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement Costpoint pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test. 

* Costpoint prend en charge l’authentification unique lancée par le **fournisseur de services et le fournisseur d’identité**.

## <a name="generate-costpoint-metadata"></a>Générer des métadonnées Costpoint

La configuration de l’authentification unique SAML Costpoint est expliquée dans le guide **DeltekCostpoint711Security. pdf**. Téléchargez ce guide à partir du site de support Deltek Costpoint et reportez-vous à la section **SAML Single Sign-on Setup** > **Configure SAML Single Sign-on between Costpoint and Microsoft Azure** (Configuration de l’authentification unique SAML -> Configurer l’authentification unique SAML entre Costpoint et Microsoft Azure). Suivez les instructions et générez un fichier **XML de métadonnées de fédération du fournisseur de services Costpoint**. 

![Capture d’écran montrant « Product Configuration Utility » avec l’onglet « Weblogic - Security » sélectionné.](./media/costpoint-tutorial/configuration-utility.png)

## <a name="add-costpoint-from-the-gallery"></a>Ajouter Costpoint à partir de la galerie

Pour configurer l’intégration de Costpoint avec Azure AD, vous devez ajouter Costpoint, disponible dans la galerie, à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Costpoint** dans la zone de recherche.
1. Sélectionnez **Costpoint** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-costpoint"></a>Configurer et tester l’authentification unique Azure AD pour Costpoint

Configurez et testez l’authentification unique Azure AD avec Costpoint à l’aide d’un utilisateur de test appelé **B. Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur Costpoint associé.

Pour configurer et tester l’authentification unique Azure AD avec Costpoint, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Costpoint](#configure-costpoint-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test Costpoint](#create-costpoint-test-user)** pour avoir un équivalent de B. Simon dans Costpoint lié à la représentation Azure AD associée.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le portail Azure :

1. Dans la page d’intégration de l’application **Costpoint**, sélectionnez **Authentification unique**.

1. Dans la section **Configuration SAML de base**, si vous disposez du **fichier de métadonnées du fournisseur de services**, suivez les étapes ci-dessous :

   > [!NOTE]
   > Vous recevez le fichier de métadonnées du fournisseur de services dans [Générer des métadonnées Costpoint](#generate-costpoint-metadata). L’utilisation du fichier est expliquée plus loin dans le tutoriel.
 
   1. Sélectionnez le bouton **Charger le fichier de métadonnées**, puis le fichier **XML de métadonnées de fédération du fournisseur de services Costpoint** généré précédemment par Costpoint, puis le bouton **Ajouter** pour charger le fichier.

      ![Charger le fichier de métadonnées](./media/costpoint-tutorial/upload-metadata.png)
    
   1. Une fois le fichier de métadonnées correctement chargé, les valeurs **Identificateur** et **URL de réponse** sont renseignées automatiquement dans la section Costpoint.

      > [!NOTE]
      > Si les valeurs **Identificateur** et **URL de réponse** ne sont pas renseignées automatiquement, entrez-les manuellement en fonction de vos besoins. Vérifiez que les valeurs **Identificateur (ID d’entité)** et **URL de réponse (URL Assertion Consumer Service)** sont correctement définies et que l’**URL ACS** est une URL Costpoint valide qui se termine par **/LoginServlet.cps**.

   1. Sélectionnez **Définir des URL supplémentaires**. Pour **État de relais**, entrez une valeur en utilisant le modèle suivant : `system=[your system]` (par exemple, **system=DELTEKCP**).

1. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, sélectionnez l’icône **Copier** pour copier l’**URL des métadonnées de fédération d’application** et enregistrez-la dans le Bloc-notes.

   ![Certificat de signature SAML](common/copy-metadataurl.png)

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

Dans cette section, vous allez autoriser B. Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Costpoint.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Costpoint**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-costpoint-sso"></a>Configurer l’authentification unique Costpoint

1. Revenez à l’utilitaire de configuration de Costpoint. Dans la zone de texte **XML de métadonnées de fédération de fournisseur d’identité**, collez le contenu du fichier *URL des métadonnées de fédération d’application*. 

   ![Utilitaire de configuration de Costpoint](./media/costpoint-tutorial/configuration-utility-metadata.png)

1. Suivez les instructions du guide **DeltekCostpoint711Security.pdf** pour terminer la configuration SAML de Costpoint.

### <a name="create-costpoint-test-user"></a>Créer un utilisateur de test Costpoint

Dans cette section, vous créez un utilisateur dans Costpoint. Partez du principe que l’ID d’utilisateur est **B.SIMON** et que le nom de l’utilisateur est **B.Simon**. Collaborez avec l’[équipe de support technique Costpoint](https://www.deltek.com/about/contact-us) pour ajouter l’utilisateur dans la plateforme Costpoint. L’utilisateur doit être créé et activé pour pouvoir utiliser l’authentification unique.

Une fois l’utilisateur créé, la sélection de la **Méthode d’authentification** de l’utilisateur doit être **Active Directory**, la case **Authentification unique SAML** doit être cochée et le nom d’utilisateur Azure Active Directory doit être **Active Directory ou ID de certificat** (situation présentée dans la capture d’écran suivante).

![Utilisateur Costpoint](./media/costpoint-tutorial/costpoint-user.png)

## <a name="test-sso"></a>Tester l’authentification unique (SSO)

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

#### <a name="sp-initiated"></a>Lancée par le fournisseur de services :

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à Costpoint, à partir de laquelle vous pouvez démarrer le flux de connexion.  

* Accédez directement à l’URL de connexion à Costpoint pour y lancer le flux de connexion.

#### <a name="idp-initiated"></a>Lancée par le fournisseur d’identité :

* Cliquez sur **Tester cette application** dans le portail Azure. Vous devez être connecté automatiquement à l’instance de Costpoint pour laquelle vous avez configuré l’authentification unique. 

Vous pouvez aussi utiliser Mes applications de Microsoft pour tester l’application dans n’importe quel mode. Si, quand vous cliquez sur la vignette Costpoint dans Mes applications, le mode Fournisseur de services est configuré, vous êtes redirigé vers la page de connexion de l’application pour lancer le flux de connexion ; s’il s’agit du mode Fournisseur d’identité, vous êtes automatiquement connecté à l’instance de Costpoint pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez configuré Costpoint, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).