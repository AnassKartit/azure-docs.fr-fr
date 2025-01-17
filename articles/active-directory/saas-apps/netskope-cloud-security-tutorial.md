---
title: 'Tutoriel : Intégration de l’authentification unique Azure Active Directory à Netskope Administrator Console | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Netskope Administrator Console.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/02/2021
ms.author: jeedes
ms.openlocfilehash: eb3415c17781611e0bbea69112f6f620d818fb4a
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2021
ms.locfileid: "132307408"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-netskope-administrator-console"></a>Tutoriel : Intégration de l’authentification unique Azure Active Directory à Netskope Administrator Console

Dans ce tutoriel, vous allez découvrir comment intégrer Netskope Administrator Console à Azure Active Directory (Azure AD). En intégrant Netskope Administrator Console à Azure AD, vous pouvez :

- Contrôler, dans Azure AD, qui a accès à Netskope Administrator Console
- Permettre aux utilisateurs de se connecter automatiquement à Netskope Administrator Console avec leur compte Azure AD
- Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

- Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
- Un abonnement Netskope Administrator Console pour lequel l’authentification unique (SSO) est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

- Netskope Administrator Console prend en charge l'authentification unique initiée par **le fournisseur de services et le fournisseur d'identité**.

> [!NOTE]
> L’identificateur de cette application étant une valeur de chaîne fixe, une seule instance peut être configurée dans un locataire.

## <a name="add-netskope-administrator-console-from-the-gallery"></a>Ajouter Netskope Administrator Console à partir de la galerie

Pour configurer l’intégration de Netskope Administrator Console à Azure AD, vous devez ajouter Netskope Administrator Console à votre liste d’applications SaaS managées à partir de la galerie.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **Netskope Administrator Console** dans la zone de recherche.
1. Sélectionnez **Netskope Administrator Console** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-netskope-administrator-console"></a>Configurer et tester l’authentification unique Azure AD pour Netskope Administrator Console

Configurez et testez l’authentification unique Azure AD auprès de Netskope Administrator Console avec un utilisateur de test appelé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur Netskope Administrator Console associé.

Pour configurer et tester l’authentification unique Azure AD auprès de Netskope Administrator Console, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
   1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
   1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique Netskope Administrator Console](#configure-netskope-administrator-console-sso)** pour configurer les paramètres de l’authentification unique côté application.
   1. **[Créer un utilisateur de test Netskope Administrator Console](#create-netskope-administrator-console-test-user)** pour avoir un équivalent de B.Simon dans Netskope Administrator Console lié à la représentation Azure AD associée.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **Netskope Administrator Console**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, si vous souhaitez configurer l’application en mode Initié par le **fournisseur d’identité**, entrez les valeurs pour les champs suivants :

   Dans la zone de texte **URL de réponse**, tapez une URL au format suivant : `https://<tenant_host_name>/saml/acs`

   > [!NOTE]
   > Cette valeur n’est pas la valeur réelle. Mettez à jour la valeur avec l’URL de réponse réelle. La valeur vous sera expliquée plus loin dans le tutoriel.

1. Si vous souhaitez configurer l’application en **mode démarré par le fournisseur de services**, cliquez sur **Définir des URL supplémentaires**, puis effectuez les étapes suivantes :

   Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://<tenantname>.goskope.com`.

   > [!NOTE]
   > La valeur de l’URL de connexion n’est pas réelle. Modifiez-la selon l’URL de connexion réelle. Contactez l’[équipe du support technique de Netskope Administrator Console](mailto:support@netskope.com) pour connaître la valeur de l’URL de connexion. Vous pouvez également consulter les modèles figurant à la section **Configuration SAML de base** dans le portail Azure.

1. L’application Netskope Administrator Console s’attend à recevoir les assertions SAML dans un format spécifique, ce qui vous oblige à ajouter des mappages d’attributs personnalisés à la configuration des attributs du jeton SAML. La capture d’écran suivante montre la liste des attributs par défaut.

   ![image](common/default-attributes.png)

1. En plus de ce qui précède, l’application Netskope Administrator Console s’attend à ce que quelques attributs supplémentaires (présentés ci-dessous) soient repassés dans la réponse SAML. Ces attributs sont également préremplis, mais vous pouvez les examiner pour voir s’ils répondent à vos besoins.

   | Nom       | Attribut source   |
   | ---------- | ------------------ |
   | admin-role | user.assignedroles |

   > [!NOTE]
   > Cliquez [ici](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui) pour découvrir comment créer des rôles dans Azure AD.

1. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, recherchez **Certificat (Base64)** , puis sélectionnez **Télécharger** pour télécharger le certificat et l’enregistrer sur votre ordinateur.

   ![Lien Téléchargement de certificat](common/certificatebase64.png)

1. Dans la section **Configurer Netskope Administrator Console**, copiez l’URL ou les URL appropriées en fonction de vos besoins.

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Netskope Administrator Console.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **Netskope Administrator Console**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous avez configuré les rôles comme indiqué ci-dessus, vous pouvez en sélectionner un dans la liste déroulante **Sélectionner un rôle**.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-netskope-administrator-console-sso"></a>Configurer l’authentification unique Netskope Administrator Console

1. Ouvrez un nouvel onglet dans votre navigateur et connectez-vous à votre site d’entreprise Netskope Administrator Console en tant qu’administrateur.

1. Cliquez sur l’onglet **Settings** dans le volet de navigation de gauche.

   ![Capture d’écran montrant l’élément Settings sélectionné dans le volet de navigation.](./media/netskope-cloud-security-tutorial/configure-settings.png)

1. Cliquez sur l’onglet **Administration**.

   ![Capture d’écran montrant l’élément Administration sélectionné à partir de la section Settings.](./media/netskope-cloud-security-tutorial/administration.png)

1. Cliquez sur l’onglet **SSO**.

   ![Capture d’écran montrant l’option SSO sélectionnée dans Administration.](./media/netskope-cloud-security-tutorial/tab.png)

1. Dans la section **Network Settings**, effectuez les étapes suivantes :

   ![Capture d’écran montrant la zone Network Settings dans laquelle vous pouvez indiquer les valeurs décrites.](./media/netskope-cloud-security-tutorial/network.png)

   a. Copiez la valeur **Assertion Consumer Service URL**, puis collez-la dans la zone **URL de réponse** de la section **Configuration SAML de base** du portail Azure.

   b. Copiez la valeur **Service Provider Entity ID**, puis collez-la dans la zone de texte **Identificateur** de la section **Configuration SAML de base** du portail Azure.

1. Cliquez sur **EDIT SETTINGS** sous la section **SSO/SLO Settings**.

   ![Capture d’écran montrant la section SSO/SLO Settings, dans laquelle vous pouvez sélectionner EDIT SETTINGS.](./media/netskope-cloud-security-tutorial/settings.png)

1. Dans la fenêtre contextuelle **Settings**, effectuez les étapes suivantes :

   ![Capture d’écran montrant la boîte de dialogue Settings où vous pouvez entrer les valeurs décrites.](./media/netskope-cloud-security-tutorial/configuration.png)

   a. Sélectionnez **Enable SSO**.

   b. Dans la zone de texte **IDP URL** (URL du fournisseur d’identité), collez la valeur de l’**URL de connexion** que vous avez copiée sur le portail Azure.

   c. Dans la zone de texte **IDP ENTITY ID** (ID d’entité de fournisseur d’identité), collez la valeur **Identificateur Azure AD** que vous avez copiée dans le portail Azure.

   d. Ouvrez le certificat codé en Base64 que vous avez téléchargé dans le Bloc-notes, copiez son contenu dans le Presse-papiers, puis collez-le dans la zone de texte **IDP CERTIFICATE**.

   e. Sélectionnez **Enable SSO**.

   f. Dans la zone de texte **IDP SLO URL**, collez la valeur de l’**URL de déconnexion** que vous avez copiée sur le portail Azure.

   g. Cliquez sur **ENVOYER**.

### <a name="create-netskope-administrator-console-test-user"></a>Créer un utilisateur de test Netskope Administrator Console

1. Ouvrez un nouvel onglet dans votre navigateur et connectez-vous à votre site d’entreprise Netskope Administrator Console en tant qu’administrateur.

1. Cliquez sur l’onglet **Settings** dans le volet de navigation de gauche.

   ![Capture d’écran montrant l’élément Settings sélectionné.](./media/netskope-cloud-security-tutorial/configure-settings.png)

1. Cliquez sur l’onglet **Active Platform**.

   ![Capture d’écran montrant l’élément Active Platform sélectionné à partir de Settings.](./media/netskope-cloud-security-tutorial/user-1.png)

1. Cliquez sur l’onglet **Users** (Utilisateurs).

   ![Capture d’écran montrant l’option Users sélectionnée à partir de Active Platform.](./media/netskope-cloud-security-tutorial/add-user.png)

1. Cliquez sur **ADD USERS**.

   ![Capture d’écran montrant la boîte de dialogue Users dans laquelle vous pouvez sélectionner ADD USERS.](./media/netskope-cloud-security-tutorial/user-add.png)

1. Entrez l’adresse e-mail de l’utilisateur à ajouter, puis cliquez sur **ADD**.

   ![Capture d’écran montrant la zone Add Users dans laquelle vous pouvez entrer une liste d’utilisateurs.](./media/netskope-cloud-security-tutorial/add-user-popup.png)

## <a name="test-sso"></a>Tester l’authentification unique (SSO)

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes.

#### <a name="sp-initiated"></a>Lancée par le fournisseur de services :

- Cliquez sur **Tester cette application** dans le portail Azure. Vous allez être redirigé vers l’URL de connexion à Netskope Administrator Console, où vous pouvez lancer le flux de connexion.

- Accédez directement à l’URL de connexion à Netskope Administrator Console pour y lancer le flux de connexion.

#### <a name="idp-initiated"></a>Lancée par le fournisseur d’identité :

- Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors connecté automatiquement à l’instance de Netskope Administrator Console pour laquelle vous avez configuré l’authentification unique

Vous pouvez aussi utiliser Mes applications de Microsoft pour tester l’application dans n’importe quel mode. Si, quand vous cliquez sur la vignette Netskope Administrator Console dans Mes applications, le mode Fournisseur de services est configuré, vous êtes redirigé vers la page de connexion de l’application pour lancer le flux de connexion ; s’il s’agit du mode Fournisseur d’identité, vous êtes automatiquement connecté à l’instance de Netskope Administrator Console pour laquelle vous avez configuré l’authentification unique. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Étapes suivantes

Une fois que vous avez configuré Netskope Administrator Console, vous pouvez appliquer le contrôle de session qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Defender for Cloud Apps](/cloud-app-security/proxy-deployment-any-app).
