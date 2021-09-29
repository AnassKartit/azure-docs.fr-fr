---
title: 'Tutoriel : Intégration d’Azure Active Directory à Abstract | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Abstract.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/31/2021
ms.author: jeedes
ms.openlocfilehash: 0148fd688622f070038378f9638b66b6d2b3c1bc
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124770868"
---
# <a name="tutorial-integrate-abstract-with-azure-active-directory"></a>Tutoriel : Intégrer Abstract à Azure Active Directory

Dans ce tutoriel, vous allez apprendre à intégrer Abstract à Azure Active Directory (Azure AD). Quand vous intégrez Abstract à Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à Abstract.
* Autoriser les utilisateurs à se connecter automatiquement à Abstract avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement Abstract pour lequel l’authentification unique est activée

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

* Abstract prend en charge l’authentification unique lancée par le **fournisseur de services et le fournisseur d’identité**.

## <a name="add-abstract-from-the-gallery"></a>Ajouter Abstract à partir de la galerie

Pour configurer l’intégration d’Abstract avec Azure AD, vous devez ajouter Abstract à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Abstract** dans la zone de recherche.
1. Sélectionnez **Abstract** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-abstract"></a>Configurer et tester l’authentification unique Azure AD pour Abstract

Configurez et testez l’authentification unique Azure AD avec Abstract pour un utilisateur de test nommé **B. Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur Abstract associé.

Pour configurer et tester l’authentification unique Azure AD avec Abstract, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Abstract](#configure-abstract-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test Abstract](#create-abstract-test-user)** pour avoir, dans Abstract, un équivalent de B.Simon lié à la représentation Azure AD de l’utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Sur le portail Azure, dans la page d’intégration de l’application **Abstract**, recherchez la section **Gérer**, puis sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon pour **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, l’application est préconfigurée en mode Lancement par le **fournisseur d’identité** et les URL nécessaires sont déjà préremplies avec Azure. L’utilisateur doit enregistrer la configuration en cliquant sur le bouton **Enregistrer**.

1. Si vous souhaitez configurer l’application en **mode démarré par le fournisseur de services**, cliquez sur **Définir des URL supplémentaires**, puis effectuez les étapes suivantes :

    Dans la zone de texte **URL de connexion**, tapez l’URL : `https://app.abstract.com/signin`

4. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur le bouton Copier pour copier l’**URL des métadonnées de fédération d’application**, puis enregistrez-la sur votre ordinateur.

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

Dans cette section, vous allez autoriser B. Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Abstract.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Abstract**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-abstract-sso"></a>Configurer l’authentification unique Abstract

Veillez à récupérer votre `App Federation Metadata Url` et l’`Azure AD Identifier` à partir du portail Azure, car vous en aurez besoin pour configurer l’authentification unique sur Abstract.

Vous trouverez ces informations dans la page **Configurer l’authentification unique avec SAML** :

* L’`App Federation Metadata Url` se trouve dans la section **Certificat de signature SAML**.
* L’`Azure AD Identifier` se trouve dans la section **Configurer Abstract**.

Vous êtes maintenant prêt à configurer l’authentification unique sur Abstract :

>[!Note]
>Vous devrez vous authentifier auprès d’un compte d’administrateur de l’organisation pour accéder aux paramètres d’authentification unique sur Abstract.

1. Ouvrez l’[application web Abstract](https://app.abstract.com/).
2. Accédez à la page **Permissions** (Autorisations) dans la barre latérale gauche.
3. Dans la section **Configure SSO** (Configurer l’authentification unique), entrez votre URL de métadonnées (**Metadata URL**) et votre ID d’entité (**Entity ID**).
4. Entrez les éventuelles exceptions manuelles. Les e-mails figurant dans la section d’exceptions manuelles ignoreront l’authentification unique et pourront se connecter avec un e-mail et un mot de passe. 
5. Cliquez sur **Enregistrer les modifications**.

>[!Note] 
>Vous devez utiliser les adresses e-mail principales dans la liste des exceptions manuelles. L’activation de l’authentification unique échoue si l’e-mail que vous spécifiez est l’e-mail secondaire d’un utilisateur. Si cela se produit, un message d’erreur s’affiche avec l’adresse e-mail principale du compte défaillant. Ajoutez cette adresse e-mail principale à la liste des exceptions manuelles une fois que vous avez vérifié que vous connaissez l’utilisateur.

### <a name="create-abstract-test-user"></a>Créer un utilisateur de test Abstract

Pour tester l’authentification unique sur Abstract

1. Ouvrez l’[application web Abstract](https://app.abstract.com/).
2. Accédez à la page **Permissions** (Autorisations) dans la barre latérale gauche.
3. Cliquez sur **Test with my Account** (Tester avec mon compte). Si le test échoue, [contactez notre équipe de support](https://help.abstract.com/hc/).

>[!Note]
>Vous devrez vous authentifier auprès d’un compte d’administrateur de l’organisation pour accéder aux paramètres d’authentification unique sur Abstract.
Ce compte d’administrateur de l’organisation doit être affecté à Abstract sur le portail Azure.

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

#### <a name="sp-initiated"></a>Lancée par le fournisseur de services :

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à Abstract à partir de laquelle vous pouvez lancer le flux de connexion.  

* Accédez directement à l’URL de connexion à Abstract et lancez le flux de connexion.

#### <a name="idp-initiated"></a>Lancée par le fournisseur d’identité :

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors automatiquement connecté à l’instance d’Abstract pour laquelle vous avez configuré l’authentification unique. 

Vous pouvez aussi utiliser Mes applications de Microsoft pour tester l’application dans n’importe quel mode. Si, quand vous cliquez sur la vignette Abstract dans Mes applications, le mode Fournisseur de services est configuré, vous êtes redirigé vers la page de connexion de l’application pour lancer le flux de connexion ; s’il s’agit du mode Fournisseur d’identité, vous êtes automatiquement connecté à l’instance d’Abstract pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré Abstract, vous pouvez appliquer le contrôle de session, qui protège en temps réel contre l’exfiltration et l’infiltration des données sensibles de votre organisation. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).