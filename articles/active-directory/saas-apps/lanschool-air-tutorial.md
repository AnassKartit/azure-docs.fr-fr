---
title: 'Tutoriel : Intégration de l’authentification unique Azure Active Directory à LanSchool Air | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et LanSchool Air.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 06/30/2021
ms.author: jeedes
ms.openlocfilehash: bb6f4ea3577ac62e221ebdd04c88721fddbe3feb
ms.sourcegitcommit: 695a33a2123429289ac316028265711a79542b1c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/01/2021
ms.locfileid: "113129121"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-lanschool-air"></a>Tutoriel : Intégration de l’authentification unique Azure Active Directory à LanSchool Air

Dans ce tutoriel, vous allez apprendre à intégrer LanSchool Air à Azure Active Directory (Azure AD). Lorsque vous intégrez LanSchool Air à Azure AD, vous pouvez :

* Contrôler, dans Azure AD, qui a accès à LanSchool Air.
* Permettre à vos utilisateurs de se connecter automatiquement à LanSchool Air avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement LanSchool Air pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

* LanSchool Air prend en charge l’authentification unique lancée par **le fournisseur de services et le fournisseur d’identité**.

## <a name="adding-lanschool-air-from-the-gallery"></a>Ajout de LanSchool Air à partir de la galerie

Pour configurer l’intégration de LanSchool Air à Azure AD, vous devez ajouter LanSchool Air, disponible dans la galerie, à votre liste d’applications SaaS managées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, entrez **LanSchool Air** dans la zone de recherche.
1. Sélectionnez **LanSchool Air** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.


## <a name="configure-and-test-azure-ad-sso-for-lanschool-air"></a>Configurer et tester l’authentification unique Azure AD pour LanSchool Air

Configurez et testez l’authentification unique Azure AD avec LanSchool Air pour un utilisateur de test appelé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur associé dans LanSchool Air.

Pour configurer et tester l’authentification unique Azure AD avec LanSchool Air, procédez comme suit :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique LanSchool Air](#configure-lanschool-air-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test LanSchool Air](#create-lanschool-air-test-user)** pour avoir un équivalent de B.Simon dans LanSchool Air, associé à sa représentation dans Azure AD.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le Portail Azure, accédez à la page d’intégration de l’application **LanSchool Air**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, l’utilisateur n’a rien à faire, car l’application est déjà intégrée à Azure.

1. Si vous souhaitez configurer l’application en **mode démarré par le fournisseur de services**, cliquez sur **Définir des URL supplémentaires**, puis effectuez les étapes suivantes :

    Dans la zone de texte **URL de connexion**, tapez l’URL : `https://lanschoolair.lenovosoftware.com`

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à LanSchool Air.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **LanSchool Air**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-lanschool-air-sso"></a>Configurer l’authentification unique LanSchool Air

Pour configurer l’authentification unique côté **LanSchool Air**, vous devez envoyer l’**URL des métadonnées de fédération de l’application** à l’[équipe du support technique LanSchool Air](mailto:support@lanschool.com). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-lanschool-air-test-user"></a>Créer un utilisateur de test LanSchool Air

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans LanSchool Air. Rapprochez-vous de l’[équipe de support LanSchool Air](mailto:support@lanschool.com) pour ajouter les utilisateurs dans la plateforme LanSchool Air. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

#### <a name="sp-initiated"></a>Lancée par le fournisseur de services :

* Cliquez sur **Tester cette application** dans le portail Azure. Cette opération redirige vers l’URL de connexion LanSchool Air, à partir de laquelle vous pouvez lancer le processus de connexion.  

* Accédez directement à l’URL de connexion LanSchool Air pour lancer le processus de connexion.

#### <a name="idp-initiated"></a>Lancée par le fournisseur d’identité :

* Cliquez sur **Tester cette application** sur le Portail Azure. Vous êtes alors connecté automatiquement à l’instance LanSchool Air pour laquelle vous avez configuré l’authentification unique. 

Vous pouvez aussi utiliser Mes applications de Microsoft pour tester l’application dans n’importe quel mode. Quand vous cliquez sur la vignette LanSchool Air dans Mes applications, si le mode Fournisseur de services est configuré, vous êtes redirigé vers la page de connexion de l’application pour initier le flux de connexion. Si le mode Fournisseur d’identité est configuré, vous êtes automatiquement connecté à l’instance de LanSchool Air pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).


## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré LanSchool Air, vous pouvez appliquer le contrôle de session qui protège l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).


