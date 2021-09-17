---
title: 'Didacticiel : Intégration d’Azure Active Directory à YouEarnedIt | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et YouEarnedIt.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/10/2021
ms.author: jeedes
ms.openlocfilehash: e3e258af181c4e3633f6a8b478380e6a1808486f
ms.sourcegitcommit: 0396ddf79f21d0c5a1f662a755d03b30ade56905
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2021
ms.locfileid: "122271722"
---
# <a name="tutorial-azure-active-directory-integration-with-youearnedit"></a>Didacticiel : Intégration d’Azure Active Directory à YouEarnedIt

Dans ce tutoriel, vous allez apprendre à intégrer YouEarnedIt à Azure Active Directory (Azure AD). Quand vous intégrez youEarnedIt à Azure AD, vous pouvez :

* dans Azure AD, contrôler qui a accès à YouEarnedIt.
* permettre à vos utilisateurs de se connecter automatiquement à YouEarnedIt avec leurs comptes Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour commencer, vous devez disposer de ce qui suit :

* Un abonnement Azure AD Si vous ne disposez d’aucun abonnement, vous pouvez obtenir [un compte gratuit](https://azure.microsoft.com/free/)
* Abonnement YouEarnedIt pour lequel l’authentification unique (SSO) est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce didacticiel, vous configurez et testez l’authentification unique Azure AD dans un environnement de test.

* YouEarnedIt prend en charge l’authentification unique (SSO) lancée par le **fournisseur de services**.

## <a name="add-youearnedit-from-the-gallery"></a>Ajouter YouEarnedIt depuis la galerie

Pour configurer l’intégration de YouEarnedIt à Azure AD, vous devez ajouter YouEarnedIt à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **YouEarnedIt** dans la zone de recherche.
1. Sélectionnez **YouEarnedIt** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-youearnedit"></a>Configurer et tester l’authentification unique Azure AD pour YouEarnedIt

Configurez et testez l’authentification unique Azure AD avec YouEarnedIt à l’aide d’un utilisateur de test appelé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur associé dans YouEarnedIt.

Pour configurer et tester l’authentification unique Azure AD avec YouEarnedIt, effectuez les étapes suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique YouEarnedIt](#configure-youearnedit-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Créer un utilisateur de test YouEarnedIt](#create-youearnedit-test-user)** pour disposer, dans YouEarnedIt, d’un équivalent de B.Simon lié à la représentation de l’utilisateur Azure AD.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans la page d’intégration de l’application **YouEarnedIt** du Portail Azure, recherchez la section **Gérer**, puis sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    a. Dans la zone de texte **Identificateur**, tapez la valeur en utilisant un des modèles suivants :

    | Environnement  | Modèle  |
    |:--- |:--- |
    | Production | `<company name>.youearnedit.com` |
    | Bac à sable  |`<company name>.sandbox.youearnedit.com` |

    b. Dans la zone de texte **URL de connexion**, tapez une URL en respectant l’un des formats suivants :

    | Environnement  | Modèle  |
    |:--- |:--- |
    | Production | `https://<company name>.youearnedit.com/users/sign_in` |
    | Bac à sable  |`https://<company name>.sandbox.youearnedit.com/users/sign_in` |

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’identificateur et l’URL de connexion réels. Contactez le responsable de la réussite des clients YouEarnedIt qui vous a été attribué pour obtenir ces valeurs.

5. Dans la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le **Certificat (Base64)** en fonction des options définies par rapport à vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/certificatebase64.png)

6. Dans la section **Configurer YouEarnedIt**, copiez la ou les URL appropriées, selon vos besoins.

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à YouEarnedIt.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **YouEarnedIt**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-youearnedit-sso"></a>Configurer l’authentification unique YouEarnedIt

Pour configurer l’authentification unique côté **YouEarnedIt**, vous devez envoyer le **Certificat (Base64)** téléchargé et les URL copiées appropriées à partir du portail Azure au responsable de la réussite des clients YouEarnedIt qui vous a été attribué. Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-youearnedit-test-user"></a>Créer un utilisateur de test YouEarnedIt

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans YouEarnedIt. Collaborez avec le responsable de la réussite des clients YouEarnedIt qui vous a été attribué pour ajouter des utilisateurs à la plateforme YouEarnedIt.

> [!NOTE]
> YouEarnedIt attend du fournisseur d'identité qu’il fournisse une valeur de nom d’utilisateur ou d’adresse e-mail dans l’attribut NameID. L’authentification échoue si aucune valeur de nom d’utilisateur ou d’adresse e-mail n’est trouvée dans la base de données ou ne correspond exactement. Cette opération nécessite l’importation des comptes dans le système de YouEarnedIt avant l’intégration de l’authentification unique (en général, via l’importation CSV ou API).

## <a name="test-sso"></a>Tester l’authentification unique (SSO)

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion à YouEarnedIt à partir de laquelle vous pouvez lancer le flux de connexion. 

* Accédez directement à l’URL de connexion à YouEarnedIt et lancez le flux de connexion.

* Vous pouvez utiliser Mes applications de Microsoft. Le fait de cliquer sur la mosaïque YouEarnedIt dans Mes applications vous redirige vers l’URL de connexion à YouEarnedIt. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré YouEarnedIt, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-aad).