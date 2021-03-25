---
title: 'Tutoriel : Intégration de l’authentification unique Azure Active Directory à Concur Travel and Expense | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Concur Travel and Expense.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/21/2020
ms.author: jeedes
ms.openlocfilehash: 56eab06bd1afd18bfd4e23b9acb0b44d7bd1f80b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98737030"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-concur-travel-and-expense"></a>Tutoriel : Intégration de l’authentification unique Azure Active Directory à Concur Travel and Expense

Dans ce tutoriel, vous allez apprendre à intégrer Concur Travel and Expense à Azure Active Directory (Azure AD). Quand vous intégrez Concur Travel and Expense à Azure AD, vous pouvez :

* Contrôler dans Azure AD qui a accès à Concur Travel and Expense.
* Permettre à vos utilisateurs de se connecter automatiquement à Concur Travel and Expense avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
* Abonnement Concur Travel and Expense.
* Un rôle « Company Administrator » (Administrateur d’entreprise) sous votre compte d’utilisateur Concur. Vous pouvez vérifier si vous disposez de l’accès adéquat en accédant à [Concur SSO Self-Service Tool](https://www.concursolutions.com/nui/authadmin/ssoadmin) (Outil libre-service d’authentification unique Concur). Si vous ne disposez pas de l’accès, contactez le chef de projet d’implémentation ou de support Concur. 

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD.

* Concur Travel and Expense prend en charge l’authentification unique lancée par le **fournisseur d’identité** et le **fournisseur de services**
* Concur Travel and Expense prend en charge le test de l’authentification unique dans l’environnement de production et dans l’environnement d’implémentation 

> [!NOTE]
> L’identificateur de cette application est une valeur de chaîne fixe pour chacune des trois régions : États-Unis, EMEA et Chine. Ainsi, une seule instance peut être configurée pour chaque région dans un locataire. 

## <a name="adding-concur-travel-and-expense-from-the-gallery"></a>Ajout de Concur Travel and Expense à partir de la galerie

Pour configurer l’intégration de Concur Travel and Expense à Azure AD, vous devez ajouter Concur Travel and Expense, disponible dans la galerie, à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Concur Travel and Expense** dans la zone de recherche.
1. Sélectionnez **Concur Travel and Expense** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-concur-travel-and-expense"></a>Configurer et tester l’authentification unique Azure AD pour Concur Travel and Expense

Configurez et testez l’authentification unique Azure AD avec Concur Travel and Expense à l’aide d’un utilisateur de test appelé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur Concur Travel and Expense associé.

Pour configurer et tester l’authentification unique Azure AD avec Concur Travel and Expense, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Concur Travel and Expense](#configure-concur-travel-and-expense-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test Concur Travel and Expense](#create-concur-travel-and-expense-test-user)** pour avoir un équivalent de B.Simon dans Concur Travel and Expense lié à la représentation Azure AD associée.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **Concur Travel and Expense**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de modification/stylet de **Configuration SAML de base** pour modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, l’application est préconfigurée en mode Lancement par le **fournisseur d’identité** et les URL nécessaires sont déjà préremplies avec Azure. L’utilisateur doit enregistrer la configuration en cliquant sur le bouton **Enregistrer**.

    > [!NOTE]
    > L’identificateur (ID d’entité) et l’URL de réponse (URL Assertion Consumer Service) sont spécifiques à la région. Veuillez les sélectionner en fonction du centre de données de votre entité Concur. Si vous ne connaissez pas le centre de données de votre entité Concur, contactez le support technique Concur. 

5. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de modification/stylet pour **Attribut utilisateur** afin de modifier les paramètres. L’identifiant utilisateur unique doit correspondre à l’identifiant de connexion de l’utilisateur Concur. En règle générale, vous devez remplacer **user.userprincipalname** par **user.mail**.

    ![Modifier un attribut utilisateur](common/edit-attribute.png)

6. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, recherchez **XML de métadonnées de fédération** et sélectionnez **Télécharger** pour télécharger les métadonnées et les enregistrer sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en accordant l’accès à Concur Travel and Expense.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Concur Travel and Expense**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.

1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.

1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-concur-travel-and-expense-sso"></a>Configurer l’authentification unique Concur Travel and Expense

1. Pour automatiser la configuration dans Concur Travel and Expense, vous devez installer l’**extension de navigateur My Apps Secure Sign-in** en cliquant sur **Installer l’extension**.

    ![Extension My apps](common/install-myappssecure-extension.png)

2. Une fois l’extension ajoutée au navigateur, un clic sur **Configurer Concur Travel and Expense** vous redirige vers l’application Concur Travel and Expense. À partir de là, fournissez les informations d’identification de l’administrateur pour vous connecter à Concur Travel and Expense. Cette extension de navigateur configure automatiquement l’application pour vous et automatise les étapes 3 à 7.

    ![Configuration](common/setup-sso.png)

3. Si vous souhaitez configurer Concur Travel and Expense manuellement, dans une autre fenêtre de navigateur web, vous devez charger le **fichier XML de métadonnées de fédération** vers [Concur SSO Self-Service Tool](https://www.concursolutions.com/nui/authadmin/ssoadmin) et vous connecter à votre site d’entreprise Concur Travel and Expense en tant qu’administrateur.

1. Cliquez sur **Add**.
1. Entrez un nom personnalisé pour votre fournisseur d’identité, par exemple « Azure AD (US) ». 
1. Cliquez sur **Upload XML File** (Charger le fichier XML) et joignez le fichier **XML de métadonnées de fédération** que vous avez téléchargé.
1. Cliquez sur **Add Metadata** (Ajouter les métadonnées) pour enregistrer la modification.

    ![Capture d’écran de l’outil libre-service d’authentification unique](./media/concur-travel-and-expense-tutorial/add-metadata-concur-self-service-tool.png)

### <a name="create-concur-travel-and-expense-test-user"></a>Créer un utilisateur de test Concur Travel and Expense

Dans cette section, vous créez un utilisateur appelé B.Simon dans Concur Travel and Expense. Collaborez avec l’équipe du support technique Concur pour ajouter les utilisateurs à la plateforme Concur Travel and Expense. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique. 

> [!NOTE]
> L’ID de connexion Concur de B.Simon doit correspondre à l’identificateur unique de B.Simon sur Azure AD. Par exemple, si l’identificateur unique Azure AD de B.Simon est `B.Simon@contoso.com`, l’ID de connexion Concur de B.Simon doit également être `B.Simon@contoso.com`. 

## <a name="configure-concur-mobile-sso"></a>Configurer l’authentification unique mobile Concur
Pour activer l’authentification unique mobile Concur, vous devez fournir à l’équipe de support technique Concur l’**URL de l’accès utilisateur**. Suivez les étapes ci-dessous pour obtenir l’**URL de l’accès utilisateur** auprès d’Azure AD :
1. Accédez à **Applications d’entreprise**.
1. Cliquez sur **Concur Travel and Expense**.
1. Cliquez sur **Propriétés**.
1. Copiez l’**URL de l’accès utilisateur**, puis communiquez-la à l’équipe de support.

> [!NOTE]
> L’option libre-service pour configurer l’authentification unique n’étant pas disponible, collaborez avec l’équipe du support technique Concur pour activer l’authentification unique mobile. 

## <a name="test-sso"></a>Tester l’authentification unique (SSO) 

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes.

#### <a name="sp-initiated"></a>Lancée par le fournisseur de services :

* Cliquez sur **Tester cette application** dans le portail Azure. Cette opération redirige vers l’URL de connexion Concur Travel and Expense, où vous pouvez lancer le processus de connexion.

* Accédez directement à l’URL de connexion Concur Travel and Expense pour y lancer le processus de connexion.

#### <a name="idp-initiated"></a>Lancée par le fournisseur d’identité :

* Dans le portail Azure, cliquez sur **Tester cette application** ; vous êtes alors automatiquement connecté à l’instance de Concur Travel and Expense pour laquelle vous avez configuré l’authentification unique.

Vous pouvez aussi utiliser Mes applications de Microsoft pour tester l’application dans n’importe quel mode. Si, quand vous cliquez sur la vignette Concur Travel and Expense dans Mes applications, le mode Fournisseur de services est configuré, vous êtes redirigé vers la page de connexion de l’application pour lancer le flux de connexion ; s’il s’agit du mode Fournisseur d’identité, vous êtes automatiquement connecté à l’instance de Concur Travel and Expense pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez configuré Concur Travel and Expense, vous pouvez appliquer le contrôle de session, qui protège l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).