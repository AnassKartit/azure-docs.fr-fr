---
title: 'Didacticiel : Intégration d’Azure Active Directory à itslearning | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et itslearning.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 05/06/2021
ms.author: jeedes
ms.openlocfilehash: 4b333a8910277236c4d4f5f7f044f21f6ee03f74
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131067046"
---
# <a name="tutorial-azure-active-directory-integration-with-itslearning"></a>Didacticiel : Intégration d’Azure Active Directory à itslearning

Dans ce didacticiel, vous allez apprendre à intégrer itslearning à Azure Active Directory (Azure AD). Quand vous intégrez itslearning à Azure AD, vous pouvez :

* Contrôlez dans Azure AD qui a accès à itslearning.
* Permettre à vos utilisateurs de se connecter automatiquement à itslearning avec leur compte Azure AD.
* Gérer vos comptes à un emplacement central : le Portail Azure.

## <a name="prerequisites"></a>Prérequis

Pour configurer l’intégration d’Azure AD à itslearning, vous avez besoin des éléments suivants :

* Un abonnement Azure AD Si vous n’avez pas d’environnement Azure AD, vous pouvez obtenir un [compte gratuit](https://azure.microsoft.com/free/).
* Un abonnement itslearning pour lequel l’authentification unique est activée.

## <a name="scenario-description"></a>Description du scénario

Dans ce tutoriel, vous allez configurer et tester l’authentification unique Azure AD dans un environnement de test.

* itslearning prend en charge l’authentification unique lancée par le **fournisseur de services**.

> [!NOTE]
> L’identificateur de cette application étant une valeur de chaîne fixe, une seule instance peut être configurée dans un locataire.

## <a name="adding-itslearning-from-the-gallery"></a>Ajout d’itslearning à partir de la galerie

Pour configurer l’intégration d’itslearning à Azure AD, vous devez ajouter itslearning à partir de la galerie à votre liste d’applications SaaS gérées.

1. Connectez-vous au portail Azure avec un compte professionnel ou scolaire ou avec un compte personnel Microsoft.
1. Dans le panneau de navigation gauche, sélectionnez le service **Azure Active Directory**.
1. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.
1. Pour ajouter une nouvelle application, sélectionnez **Nouvelle application**.
1. Dans la section **Ajouter à partir de la galerie**, tapez **itslearning** dans la zone de recherche.
1. Sélectionnez **itslearning** dans le volet de résultats, puis ajoutez l’application. Patientez quelques secondes pendant que l’application est ajoutée à votre locataire.

## <a name="configure-and-test-azure-ad-sso-for-itslearning"></a>Configurer et tester l’authentification unique Azure AD pour itslearning

Configurez et testez l’authentification unique Azure AD avec itslearning pour un utilisateur test nommé **B.Simon**. Pour que l’authentification unique fonctionne, vous devez établir un lien entre un utilisateur Azure AD et l’utilisateur associé dans itslearning.

Pour configurer et tester l’authentification unique Azure AD avec itslearning, procédez comme suit :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-sso)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
    1. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec B. Simon.
    1. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à B. Simon d’utiliser l’authentification unique Azure AD.
1. **[Configurer l’authentification unique itslearning](#configure-itslearning-sso)** pour configurer les paramètres de l’authentification unique côté application.
    1. **[Création d’un utilisateur de test itslearning](#create-itslearning-test-user)** pour avoir un équivalent de B. Simon dans itslearning lié à la représentation Azure AD de l’utilisateur.
1. **[Tester l’authentification unique](#test-sso)** pour vérifier si la configuration fonctionne.

## <a name="configure-azure-ad-sso"></a>Configurer l’authentification unique Azure AD

Effectuez les étapes suivantes pour activer l’authentification unique Azure AD dans le Portail Azure.

1. Dans le portail Azure, accédez à la page d’intégration de l’application **itslearning**, recherchez la section **Gérer** et sélectionnez **Authentification unique**.
1. Dans la page **Sélectionner une méthode d’authentification unique**, sélectionnez **SAML**.
1. Dans la page **Configurer l’authentification unique avec SAML**, cliquez sur l’icône de crayon de **Configuration SAML de base** afin de modifier les paramètres.

   ![Modifier la configuration SAML de base](common/edit-urls.png)

4. Dans la section **Configuration SAML de base**, effectuez les étapes suivantes :

    1. Dans la zone de texte **Identificateur (ID d’entité)** , tapez l’URL suivante : `urn:mace:saml2v2.no:services:com.itslearning`

    1. Dans la zone de texte **URL de réponse**, tapez l’une des URL suivantes :

       | URL de réponse |
       |-------------|
       | `https://www.itsltest.com/elogin/AssertionConsumerService.aspx` |
       | `https://www.itslearning.com/elogin/AssertionConsumerService.aspx` |

    1. Dans la zone de texte **Sign on URL** (URL d’authentification), tapez l’une des URL suivantes :

       | URL d’authentification |
       |-------------|
       | `https://www.itslearning.com/index.aspx` |
       | `https://us1.itslearning.com/index.aspx` |

5. Sur la page **Configurer l’authentification unique avec SAML**, dans la section **Certificat de signature SAML**, cliquez sur **Télécharger** pour télécharger le fichier **XML de métadonnées de fédération** en fonction des options définies selon vos besoins, puis enregistrez-le sur votre ordinateur.

    ![Lien Téléchargement de certificat](common/metadataxml.png)

6. Dans la section **Configurer itslearning**, copiez la ou les URL appropriées en fonction de vos besoins.

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

Dans cette section, vous allez autoriser B.Simon à utiliser l’authentification unique Azure en lui accordant l’accès à itslearning.

1. Dans le portail Azure, sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.
1. Dans la liste des applications, sélectionnez **itslearning**.
1. Dans la page de vue d’ensemble de l’application, recherchez la section **Gérer** et sélectionnez **Utilisateurs et groupes**.
1. Sélectionnez **Ajouter un utilisateur**, puis **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une attribution**.
1. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **B. Simon** dans la liste Utilisateurs, puis cliquez sur le bouton **Sélectionner** au bas de l’écran.
1. Si vous attendez qu’un rôle soit attribué aux utilisateurs, vous pouvez le sélectionner dans la liste déroulante **Sélectionner un rôle** . Si aucun rôle n’a été configuré pour cette application, vous voyez le rôle « Accès par défaut » sélectionné.
1. Dans la boîte de dialogue **Ajouter une attribution**, cliquez sur le bouton **Attribuer**.

## <a name="configure-itslearning-sso"></a>Configurer l’authentification unique itslearning

Pour configurer l’authentification unique côté **itslearning**, vous devez envoyer le fichier **XML des métadonnées de fédération** téléchargé et les URL appropriées copiées depuis le portail Azure à l’[équipe du support technique itslearning](mailto:support@itslearning.com). Celles-ci configurent ensuite ce paramètre pour que la connexion SSO SAML soit définie correctement des deux côtés.

### <a name="create-itslearning-test-user"></a>Création d’un utilisateur de test itslearning

Dans cette section, vous allez créer un utilisateur appelé Britta Simon dans itslearning. Collaborez avec l’[équipe de support technique itslearning](mailto:support@itslearning.com) pour ajouter les utilisateurs à la plateforme itslearning. Les utilisateurs doivent être créés et activés avant que vous utilisiez l’authentification unique.

## <a name="test-sso"></a>Tester l’authentification unique (SSO)

Dans cette section, vous allez tester votre configuration de l’authentification unique Azure AD avec les options suivantes. 

* Cliquez sur **Tester cette application** dans le portail Azure. Vous êtes alors redirigé vers l’URL de connexion itslearning, à partir de laquelle vous pouvez démarrer le flux de connexion. 

* Accédez directement à l’URL de connexion itslearning pour démarrer le flux de connexion.

* Vous pouvez utiliser Mes applications de Microsoft. Un clic sur la vignette itslearning dans Mes applications vous redirige vers l’URL de connexion à itslearning. Pour plus d’informations sur Mes applications, consultez [Présentation de Mes applications](https://support.microsoft.com/account-billing/sign-in-and-start-apps-from-the-my-apps-portal-2f3b1bae-0e5a-4a86-a33e-876fbd2a4510).

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré itslearning, vous pouvez appliquer le contrôle de session, qui protège contre l’exfiltration et l’infiltration des données sensibles de votre organisation en temps réel. Le contrôle de session est étendu à partir de l’accès conditionnel. [Découvrez comment appliquer un contrôle de session avec Microsoft Cloud App Security](/cloud-app-security/proxy-deployment-any-app).