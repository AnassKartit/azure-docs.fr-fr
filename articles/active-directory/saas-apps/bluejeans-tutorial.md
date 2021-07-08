---
title: 'Tutoriel : Intégration de l’authentification unique Azure Active Directory à BlueJeans for Azure AD | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et BlueJeans for Azure AD.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/03/2021
ms.author: jeedes
ms.openlocfilehash: 273d82c5d9427dd7717b567c04b8c18141e6eee2
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2021
ms.locfileid: "110063703"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-bluejeans-for-azure-ad"></a>Tutoriel : Intégration de l’authentification unique Azure Active Directory à BlueJeans for Azure AD

Dans ce tutoriel, vous allez apprendre à intégrer BlueJeans for Azure AD à Azure Active Directory (Azure AD). Quand vous intégrez BlueJeans for Azure avec Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à BlueJeans for Azure AD.
* Permettre à vos utilisateurs de se connecter automatiquement à BlueJeans for Azure AD avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement BlueJeans for Azure AD pour lequel l’authentification unique (SSO) est activée

> [!NOTE]
> Cette intégration peut également être utilisée à partir de l’environnement cloud US Government Azure AD. Cette application est disponible dans la Galerie d’applications cloud US Government Azure AD et peut être configurée de la même façon que dans le cloud public.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

* BlueJeans for Azure AD prend en charge l’authentification unique initiée par le **fournisseur de services**.

* BlueJeans for Azure AD prend en charge [**le provisionnement d’utilisateurs** automatisé](bluejeans-provisioning-tutorial.md).

> [!NOTE]
> L’identificateur de cette application étant une valeur de chaîne fixe, une seule instance peut être configurée dans un locataire.

## <a name="add-bluejeans-for-azure-ad-from-the-gallery"></a>Ajouter BlueJeans for Azure AD à partir de la galerie

Pour configurer l’intégration de BlueJeans for Azure AD à Azure AD, vous devez ajouter BlueJeans for Azure à partir de la galerie à votre liste d’applications SaaS managées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **BlueJeans for Azure AD** dans la zone de recherche.
1. Sélectionnez **BlueJeans for Azure AD** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-bluejeans-for-azure-ad"></a>Configurer et tester l’authentification unique Azure AD pour BlueJeans for Azure AD

Configurez et testez l’authentification unique Azure AD avec BlueJeans for Azure AD pour un utilisateur de test nommé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur BlueJeans for Azure AD associé.

Pour configurer et tester l’authentification unique Azure AD auprès de BlueJeans for Azure AD, procédez comme suit :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique BlueJeans for Azure AD](#configure-bluejeans-for-azure-ad-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test BlueJeans for Azure AD](#create-bluejeans-for-azure-ad-test-user)** pour avoir un équivalent de B.Simon dans BlueJeans for Azure AD lié à la représentation Azure AD de l’utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **BlueJeans for Azure AD**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, entrez les valeurs pour les champs suivants :

    a. Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://<companyname>.bluejeans.com`.

    a. Dans la zone de texte **Identificateur (ID d’entité)** , tapez l’URL suivante : `http://samlsp.bluejeans.com`

    a. Dans la zone **URL de réponse**, tapez l’URL : `https://bluejeans.com/sso/saml2/`

    > [!NOTE]
    > La valeur de l’URL de connexion n’est pas réelle. Mettez à jour la valeur avec l’URL de connexion réelle. Pour obtenir cette valeur, contactez l’[équipe de support client BlueJeans for Azure AD](https://support.bluejeans.com/contact). Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

1. L’application BlueJeans s’attend à recevoir les assertions SAML dans un format spécifique, ce qui vous oblige à ajouter des mappages d’attributs personnalisés à votre configuration des attributs de jetons SAML. La capture d’écran suivante montre la liste des attributs par défaut.

    ![image](common/default-attributes.png)

1. En plus de ce qui précède, l’application BlueJeans s’attend à ce que quelques attributs supplémentaires (présentés ci-dessous) soient repassés dans la réponse SAML. Ces attributs sont également préremplis, mais vous pouvez les examiner pour voir s’ils répondent à vos besoins.

    | Nom |  Attribut source|
    | ---------| --------- |
    | Téléphone | user.telephonenumber |
    | title | user.jobtitle |

1. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, recherchez **Certificat (en base64)** , puis sélectionnez **Télécharger** pour télécharger le certificat et l’enregistrer sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/certificatebase64.png)

1. Dans la section **Configurer BlueJeans for Azure AD**, copiez la ou les URL appropriées en fonction de vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à BlueJeans for Azure AD.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **BlueJeans for Azure AD**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name=&quot;configure-bluejeans-for-azure-ad-sso&quot;></a>Configurer l’authentification unique BlueJeans for Azure AD

1. Dans une autre fenêtre de navigateur web, connectez-vous à votre site d’entreprise **BlueJeans for Azure AD** en tant qu’administrateur.

2. Accédez à **ADMIN \> GROUP SETTINGS \> SECURITY**.

    ![Capture d’écran montrant une partie d’une fenêtre de navigateur avec l’onglet Admin sélectionné, avec Group Setting et Security sélectionnés.](./media/bluejeans-tutorial/admin.png &quot;Admin")

3. Dans la section **SECURITY**, procédez comme suit :

    ![SAML Single Sign On](./media/bluejeans-tutorial/security.png "SAML Single Sign On") (Authentification unique SAML)

    a. Sélectionnez **SAML Single Sign On**.

    b. Sélectionnez **Enable automatic provisioning**.

4. Poursuivez en procédant comme suit :

    ![Certificate Path](./media/bluejeans-tutorial/certificate.png "Chemin d’accès du certificat") (Chemin de certificat)

    a. Cliquez sur **Choose File** (Choisir un fichier) pour charger le certificat codé en base 64 que vous avez téléchargé à partir du portail Azure.

    b. Dans la zone de texte **Login URL** (URL de connexion), collez la valeur **URL de connexion** que vous avez copiée dans le portail Azure.

    c. Dans la zone de texte **Password Change URL** (URL de modification de mot de passe), collez la valeur de l’**URL de modification de mot de passe** que vous avez copiée à partir du portail Azure.

    d. Dans la zone de texte **Logout URL** (URL de déconnexion), collez la valeur de l’**URL de déconnexion** que vous avez copiée à partir du portail Azure.

5. Poursuivez en procédant comme suit :

    ![Save Changes](./media/bluejeans-tutorial/changes.png "Enregistrer les modifications")

    a. Dans la zone de texte **User ID** (ID utilisateur), tapez `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. Dans la zone de texte **Email** (E-mail), tapez `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Cliquez sur **ENREGISTRER LES MODIFICATIONS**.

### <a name="create-bluejeans-for-azure-ad-test-user"></a>Créer un utilisateur de test BlueJeans for Azure AD

L’objectif de cette section est de créer un utilisateur appelé B.Simon dans BlueJeans for Azure AD. BlueJeans for Azure AD prend en charge le provisionnement automatique d’utilisateurs, qui est activé par défaut. Vous trouverez plus d’informations [ici](bluejeans-provisioning-tutorial.md) sur la façon de configurer l’attribution automatique d’utilisateurs.

**Si vous avez besoin de créer un utilisateur manuellement, procédez comme suit :**

1. Connectez-vous à votre site d’entreprise **BlueJeans for Azure AD** en tant qu’administrateur.

2. Accédez à **ADMIN \> MANAGE USERS \> ADD USER**.

    ![Capture d’écran montrant une partie d’une fenêtre de navigateur avec l’onglet Admin sélectionné, avec Manage Users et Add Users sélectionnés.](./media/bluejeans-tutorial/add-user.png "Admin")

    > [!IMPORTANT]
    > L’onglet **ADD USER** est disponible seulement si, sous l’onglet **SECURITY**, l’option **Enable automatic provisioning** (Activer l’approvisionnement automatique) est décochée.

3. Dans la section **ADD USER** (Ajouter un utilisateur), procédez comme suit :

    ![Capture d’écran montrant la section Add user, où vous entrez les informations décrites dans cette étape.](./media/bluejeans-tutorial/new-user.png "Ajouter un utilisateur")

    a. Dans la zone de texte **First Name** (Prénom), tapez le prénom de l’utilisateur, par exemple **B**.

    b. Dans la zone de texte **Nom**, entrez le nom de l’utilisateur, par exemple **Simon**.

    c. Dans la zone de texte **Pick a BlueJeans for Azure AD Username** (Choisir un nom d’utilisateur BlueJeans for Azure AD), entrez le nom de l’utilisateur, par exemple **Brittasimon**.

    d. Dans la zone de texte **Create a Password** (Créer un mot de passe), entrez votre mot de passe.

    e. Dans la zone de texte **Company** (Entreprise), entrez votre entreprise.

    f. Dans la zone de texte **Adresse e-mail**, entrez l’adresse e-mail de l’utilisateur, par exemple `b.simon@contoso.com`.

    g. Dans la zone de texte **Create a BlueJeans for Azure AD Meeting I.D**  (Créer un ID de réunion BlueJeans for Azure AD), entrez votre ID de réunion.

    h. Dans la zone de texte **Pick a Moderator Passcode** (Choisir un code secret de modérateur), entrez votre code secret.

    i. Cliquez sur **CONTINUE** (Continuer).

    ![Capture d’écran montrant la section Add user où vous pouvez voir des paramètres et des fonctionnalités, avec le bouton Add User sélectionné.](./media/bluejeans-tutorial/settings.png "Ajouter un utilisateur")

    J. Cliquez sur **ADD USER** (Ajouter un utilisateur).

> [!NOTE]
> Vous pouvez utiliser un autre outil ou une autre API de création de compte d’utilisateur fournis par BlueJeans for Azure AD pour provisionner des comptes d’utilisateur Azure AD.

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à BlueJeans for Azure AD à partir de laquelle vous pouvez lancer le flux de connexion. 

* Accédez directement à l’URL de connexion à BlueJeans for Azure AD pour initier le flux de connexion.

* Vous pouvez utiliser Mes applications de Microsoft. Quand vous cliquez sur la vignette BlueJeans for Azure AD dans Mes applications, cette opération redirige vers l’URL de connexion à BlueJeans for Azure AD. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez configuré BlueJeans for Azure AD, vous pouvez appliquer le contrôle de session qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).
