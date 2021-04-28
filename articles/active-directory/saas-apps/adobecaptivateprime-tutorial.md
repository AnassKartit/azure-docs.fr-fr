---
title: 'Tutoriel : Intégration d’Azure Active Directory à Adobe Captivate Prime | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Adobe Captivate Prime.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/31/2021
ms.author: jeedes
ms.openlocfilehash: 527b2bf9e7f97e0ffffa3d2cab1fae45a6393f49
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2021
ms.locfileid: "108125478"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-captivate-prime"></a>Tutoriel : Intégration d’Azure Active Directory à Adobe Captivate Prime

Dans ce tutoriel, vous allez apprendre à intégrer Adobe Captivate Prime à Azure Active Directory (Azure AD). En intégrant Adobe Captivate Prime à Azure AD, vous pouvez :

* Utiliser Azure AD pour contrôler qui a accès à Adobe Captivate Prime.
* Permettre aux utilisateurs de se connecter automatiquement à Adobe Captivate Prime avec leur compte Azure AD
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement Adobe Captivate Prime pour lequel l'authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* Adobe Captivate Prime prend en charge l'authentification unique initiée par le **fournisseur d'identité**

> [!NOTE]
> L’identificateur de cette application étant une valeur de chaîne fixe, une seule instance peut être configurée dans un locataire.

## <a name="add-adobe-captivate-prime-from-the-gallery"></a>Ajouter d'Adobe Captivate Prime à partir de la galerie

Pour configurer l’intégration d’Adobe Captivate Prime à Azure AD, vous devez ajouter Adobe Captivate Prime de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Entrez **Adobe Captivate Prime** dans la zone de recherche de la section **Ajouter à partir de la galerie**.
1. Sélectionnez **Adobe Captivate Prime** dans le volet de résultats, puis ajoutez l'application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-adobe-captivate-prime"></a>Configurer et tester l'authentification unique Azure AD pour Adobe Captivate Prime

Configurez et testez l'authentification unique Azure AD auprès d'Adobe Captivate Prime avec un utilisateur de test appelé **B.Simon**. Pour que l'authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l'utilisateur Adobe Captivate Prime associé.

Pour configurer et tester l'authentification unique Azure AD auprès d'Adobe Captivate Prime, procédez comme suit :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l'authentification unique Adobe Captivate Prime](#configure-adobe-captivate-prime-sso)** pour configurer les paramètres de l'authentification unique côté application.
    1. **[Créer un utilisateur de test Adobe Captivate Prime](#create-adobe-captivate-prime-test-user)** pour avoir dans Adobe Captivate Prime un équivalent de B.Simon lié à la représentation Azure AD de l'utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Sur le portail Azure, accédez à la page d'intégration de l'application **Adobe Captivate Prime**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Sur la page **Configurer l’authentification unique avec SAML**, effectuez les étapes suivantes :

    a. Dans la zone de texte **Identificateur**, tapez une l’URL : `https://captivateprime.adobe.com`

    b. Dans la zone **URL de réponse**, tapez l’URL : `https://captivateprime.adobe.com/saml/SSO`

5. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies selon vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

6. Dans la section **Configurer Adobe Captivate Prime**, copiez la ou les URL appropriées correspondant à vos besoins.

    ![Copier les URL de configuration](common/copy-configuration-urls.png)

7. Accédez à l’onglet **Propriétés**, copiez l’**URL de l’accès utilisateur** et collez-la dans le Bloc-notes.

    ![Lien d’accès utilisateur](./media/adobecaptivateprime-tutorial/adobe.png)

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

Dans cette section, vous allez autoriser B.Simon à utiliser l'authentification unique Azure en lui accordant l'accès à Adobe Captivate Prime.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Adobe Captivate Prime**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-adobe-captivate-prime-sso"></a>Configurer l'authentification unique Adobe Captivate Prime

Pour configurer l’authentification unique du côté **Adobe Captivate Prime**, vous devez envoyer le **XML des métadonnées de fédération** téléchargé, l’**URL d’accès utilisateur** copiée et les URL appropriées copiées à partir du portail Azure à l’[équipe de support d’Adobe Captivate Prime](mailto:captivateprimesupport@adobe.com). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-adobe-captivate-prime-test-user"></a>Créer un utilisateur de test Adobe Captivate Prime

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans Adobe Captivate Prime. Collaborez avec l’[équipe de support d’Adobe Captivate Prime](mailto:captivateprimesupport@adobe.com) pour ajouter des utilisateurs dans la plateforme Adobe Captivate Prime. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

## <a name="test-sso"></a>Tester l’authentification unique (SSO)

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes.

* Cliquez sur Tester cette application sur le portail Azure : vous devez être connecté automatiquement à l'instance Adobe Captivate Prime pour laquelle vous avez configuré l'authentification unique.

* Vous pouvez utiliser Mes applications de Microsoft. Lorsque vous cliquez sur la vignette Adobe Captivate Prime dans Mes applications, vous devez être automatiquement connecté à l'application Adobe Captivate Prime pour laquelle vous avez configuré l'authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez configuré Adobe Captivate Prime, vous pouvez appliquer le contrôle de session, qui protège en temps réel contre l'exfiltration et l'infiltration des données sensibles de votre organisation. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).