---
title: 'Tutoriel : Intégration de l’authentification unique (SSO) Azure Active Directory avec HeyBuddy | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et HeyBuddy.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/16/2020
ms.author: jeedes
ms.openlocfilehash: b10e91a29eeece8201fcf20c9c6d108de4a1897d
ms.sourcegitcommit: 2cb7772f60599e065fff13fdecd795cce6500630
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2021
ms.locfileid: "108801601"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-heybuddy"></a>Tutoriel : Intégration de l’authentification unique (SSO) Azure Active Directory avec HeyBuddy

Ce tutoriel explique comment intégrer HeyBuddy avec Azure Active Directory (Azure AD). Quand vous intégrez HeyBuddy avec Azure AD, vous pouvez :

- Contrôler dans Azure AD qui a accès à HeyBuddy.
- Permettre à vos utilisateurs de se connecter automatiquement à HeyBuddy avec leur compte Azure AD.
- Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

- Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/).
- Abonnement HeyBuddy pour lequel l’authentification unique (SSO) est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

- HeyBuddy prend en charge l’authentification unique initiée par le **fournisseur de services**
- HeyBuddy prend en charge l’approvisionnement d’utilisateur **juste-à-temps**.

> [!NOTE]
> L’identificateur de cette application étant une valeur de chaîne fixe, une seule instance peut être configurée dans un locataire.

## <a name="adding-heybuddy-from-the-gallery"></a>Ajout de HeyBuddy à partir de la galerie

Pour configurer l’intégration de HeyBuddy à Azure AD, vous devez ajouter HeyBuddy à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **HeyBuddy** dans la zone de recherche.
1. Sélectionnez **HeyBuddy** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-heybuddy"></a>Configurer et tester l’authentification unique Azure AD pour HeyBuddy

Configurez et testez l’authentification unique Azure AD avec HeyBuddy à l’aide d’un utilisateur de test appelé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur HeyBuddy associé.

Pour configurer et tester l’authentification unique Azure AD auprès de HeyBuddy, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
   1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
   1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique HeyBuddy](#configure-heybuddy-sso)** – pour configurer les paramètres d’authentification unique côté application.
   1. **[Créer un utilisateur de test HeyBuddy](#create-heybuddy-test-user)** – pour avoir un équivalent de B.Simon dans HeyBuddy qui soit lié à la représentation Azure AD de l’utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le Portail Azure, accédez à la page d’intégration de l’application **HeyBuddy**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

1. Dans la section **Configuration SAML de base**, entrez les valeurs pour les champs suivants :

   a. Dans la zone de texte **URL de connexion**, saisissez une URL au format suivant : `https://api.heybuddy.com/auth/<ENTITY ID>`

   > [!NOTE]
   > Cette valeur n’est pas la valeur réelle. Mettez à jour la valeur avec l’URL de connexion réelle. Dans l’URL de connexion, l’`Entity ID` est généré automatiquement pour chaque organisation. Pour obtenir ces valeurs, contactez l’[équipe de support aux clients HeyBuddy](mailto:support@heybuddy.com).

1. L’application HeyBuddy attend les assertions SAML dans un format spécifique, ce qui vous oblige à ajouter des mappages d’attributs personnalisés à votre configuration des attributs de jeton SAML. La capture d’écran suivante montre la liste des attributs par défaut.

   ![image](common/edit-attribute.png)

1. En plus de ce qui précède, l’application EZOfficeInventory s’attend à ce que quelques attributs supplémentaires, indiqués ci-après, soient passés dans la réponse SAML. Ces attributs sont également préremplis, mais vous pouvez les examiner pour voir s’ils répondent à vos besoins.

   | Nom  | Attribut source   |
   | ----- | ------------------ |
   | Rôles | user.assignedroles |
   |       |                    |

   > [!NOTE]
   > Pour plus d’informations sur la configuration des rôles d’une application, cliquez sur [ce lien](../develop/howto-add-app-roles-in-azure-ad-apps.md#app-roles-ui).

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à HeyBuddy.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **HeyBuddy**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous avez configuré les rôles comme indiqué ci-dessus, vous pouvez en sélectionner un dans la liste déroulante **Sélectionner un rôle**.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-heybuddy-sso"></a>Configurer l’authentification unique HeyBuddy

Pour configurer l’authentification unique côté **HeyBuddy**, vous devez envoyer l’**URL des métadonnées de fédération de l’application** à l’[équipe du support technique HeyBuddy](mailto:support@heybuddy.com). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-heybuddy-test-user"></a>Créer un utilisateur de test HeyBuddy

Dans cette section, un utilisateur appelé Britta Simon est créé dans HeyBuddy. HeyBuddy prend en charge l’approvisionnement d’utilisateurs juste-à-temps, option activée par défaut. Vous n’avez aucune opération à effectuer dans cette section. S’il n’existe pas encore d’utilisateur dans HeyBuddy, il en est créé un après l’authentification.

> [!Note]
> Si vous devez créer un utilisateur manuellement, contactez l’[équipe de support HeyBuddy](mailto:support@heybuddy.com).

## <a name="test-sso"></a>Tester l’authentification unique (SSO)

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes.

- Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à HeyBuddy, à partir de laquelle vous pouvez lancer le flux de connexion.

- Accédez directement à l’URL de connexion à HeyBuddy pour y lancer le processus de connexion.

- Vous pouvez utiliser Mes applications de Microsoft. Un clic sur la vignette HeyBuddy dans Mes applications vous redirige vers l’URL de connexion à HeyBuddy. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré HeyBuddy, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).
