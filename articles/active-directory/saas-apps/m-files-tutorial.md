---
title: 'Didacticiel : Intégration d’Azure Active Directory dans M-Files | Documentation Microsoft'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et M-Files.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/24/2021
ms.author: jeedes
ms.openlocfilehash: 26ca0b1c4115da14821c1ba8147d571e36104fd6
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124790355"
---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a>Didacticiel : Intégration d’Azure AD à M-Files

Dans ce tutoriel, vous allez apprendre à intégrer M-Files à Azure Active Directory (Azure AD). Quand vous intégrez M-Files à Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à M-Files.
* Permettre à vos utilisateurs de se connecter automatiquement à M-Files avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement à M-Files pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* M-Files prend en charge l’authentification unique lancée par le **fournisseur de services**.

## <a name="add-m-files-from-the-gallery"></a>Ajouter M-Files à partir de la galerie

Pour configurer l’intégration de M-Files avec Azure AD, vous devez ajouter M-Files à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **M-Files** dans la zone de recherche.
1. Sélectionnez **M-Files** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-m-files"></a>Configurer et tester l’authentification unique Azure AD pour M-Files

Configurez et testez l’authentification unique Azure AD avec M-Files pour un utilisateur de test nommé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur associé dans M-Files.

Pour configurer et tester l’authentification unique Azure AD avec M-Files, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique M-Files](#configure-m-files-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test M-Files](#create-m-files-test-user)** pour avoir un équivalent de B.Simon dans M-Files lié à la représentation Azure AD associée.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, dans la page d’intégration de l’application **M-Files**, recherchez la section **Gérer**, puis sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    a. Dans la zone de texte **URL de connexion**, saisissez une URL au format suivant : `https://<tenantname>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`

    b. Dans la zone de texte **Identificateur (ID d’entité)** , saisissez une URL au format suivant : `https://<tenantname>.cloudvault.m-files.com`

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’URL de connexion et l’identificateur réels. Pour obtenir ces valeurs, contactez [l’équipe de support technique M-Files](mailto:support@m-files.com). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

5. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies selon vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

6. Dans la section **Configurer M-Files**, copiez les URL appropriées, selon vos besoins.

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à M-Files.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **M-Files**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-m-files-sso"></a>Configurer l’authentification unique M-Files

1. Pour obtenir la configuration de l’authentification unique pour votre application, contactez [l’équipe de support technique M-Files](mailto:support@m-files.com), en lui fournissant les métadonnées téléchargées.
   
    >[!NOTE]
    >Suivez les étapes suivantes si vous souhaitez configurer l’authentification unique pour votre application de bureau M-Files. Aucune étape supplémentaire n’est nécessaire si vous souhaitez configurer l’authentification unique pour la version web de M-Files uniquement.  

1. Suivez la procédure suivante pour configurer l’application de bureau M-Files de sorte à activer l’authentification unique avec Azure AD. Pour télécharger M-Files, accédez à la page [Téléchargement de M-Files](https://www.m-files.com/customers/product-downloads/download-update-links/).

1. Ouvrez la fenêtre **Paramètres de bureau M-Files**. Cliquez ensuite sur **Ajouter**.
   
    ![Capture d’écran montrant la zone Paramètres de bureau M-Files dans laquelle vous pouvez sélectionner Ajouter.](./media/m-files-tutorial/settings.png)

1. Dans la fenêtre **Propriétés de connexion au coffre de documents**, procédez comme suit :
   
    ![Capture d’écran montrant la zone Propriétés de connexion au coffre de documents, dans laquelle vous pouvez entrer les valeurs décrites.](./media/m-files-tutorial/general.png)  

    Sous la section Serveur, saisissez les valeurs comme suit :  

    a. Pour **Nom**, saisissez `<tenant-name>.cloudvault.m-files.com`. 
 
    b. Pour **Numéro de port**, saisissez **4466**. 

    c. Pour **Protocole**, sélectionnez **HTTPS**. 

    d. Dans le champ **Authentification**, sélectionnez **Utilisateur Windows spécifique**. Ensuite, une page de signature s’affiche. Entrez vos informations d’identification Azure AD. 

    e. Pour **Coffre sur le serveur**,  sélectionnez le coffre correspondant sur le serveur.
 
    f. Cliquez sur **OK**.

### <a name="create-m-files-test-user"></a>Créer un utilisateur de test M-Files

L’objectif de cette section est de créer un utilisateur appelé Britta Simon dans M-Files. Travailler avec [l’équipe de support technique M-Files](mailto:support@m-files.com) pour ajouter des utilisateurs dans les M-Files.

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à M-Files, d’où vous pouvez lancer le flux de connexion. 

* Accédez directement à l’URL de connexion à M-Files et lancez le flux de connexion.

* Vous pouvez utiliser Mes applications de Microsoft. Un clic sur la vignette M-Files dans Mes applications vous redirige vers l’URL de connexion à M-Files. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré M-Files, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).