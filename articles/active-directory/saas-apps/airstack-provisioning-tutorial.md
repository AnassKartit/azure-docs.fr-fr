---
title: 'Tutoriel : Configurer Airstack pour l’approvisionnement automatique d’utilisateurs avec Azure Active Directory | Microsoft Docs'
description: Découvrez comment configurer Azure Active Directory pour approvisionner et annuler l’approvisionnement automatiquement des comptes utilisateur sur Airstack.
services: active-directory
author: twimmers
writer: twimmers
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/18/2019
ms.author: thwimmer
ms.openlocfilehash: fb0a3a2256a79c82878e4a5e97e1054c8054af55
ms.sourcegitcommit: 9339c4d47a4c7eb3621b5a31384bb0f504951712
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/14/2021
ms.locfileid: "113765182"
---
# <a name="tutorial-configure-airstack-for-automatic-user-provisioning"></a>Tutoriel : Configurer Airstack pour l’approvisionnement automatique des utilisateurs

L’objectif de ce didacticiel est de présenter les étapes à effectuer dans Airstack et Azure Active Directory (Azure AD) afin de configurer Azure AD pour approvisionner et annuler l’approvisionnement automatiquement d’utilisateurs et/ou de groupes sur Airstack.

> [!NOTE]
> Ce didacticiel décrit un connecteur reposant sur le service d’attribution d’utilisateurs Azure AD. Pour découvrir les informations importantes sur ce que fait ce service, comment il fonctionne et consulter le forum aux questions, reportez-vous à l’article [Automatiser l’attribution et l’annulation de l’attribution des utilisateurs dans les applications SaaS avec Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Ce connecteur est actuellement en préversion publique. Pour plus d’informations sur les conditions d’utilisation Microsoft Azure générales relatives aux fonctionnalités d’évaluation, consultez [Conditions d’utilisation supplémentaires des préversions Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prérequis

Le scénario décrit dans ce tutoriel part du principe que vous disposez des prérequis suivants :

* un locataire Azure AD ;
* [Un abonné Airstack](https://airstack.com/pricing/)
* Un compte d’utilisateur dans Airstack avec des autorisations d’administrateur.

## <a name="assigning-users-to-airstack"></a>Attribution d’utilisateurs à Airstack

Azure Active Directory utilise un concept appelé *affectations* pour déterminer les utilisateurs devant recevoir l’accès aux applications sélectionnées. Dans le cadre d’une attribution automatique d’utilisateurs, seuls les utilisateurs ou les groupes auxquels une application a été attribuée dans Azure AD sont synchronisés.

Avant de configurer et d’activer l’approvisionnement automatique d’utilisateurs, vous devez décider quels utilisateurs et/ou groupes dans Azure AD ont besoin d’accéder à Airstack. Une fois que vous avez choisi, vous pouvez attribuer ces utilisateurs et/ou groupes à Airstack en suivant les instructions fournies ici :
* [Affecter un utilisateur ou un groupe à une application d’entreprise](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-airstack"></a>Conseils importants pour l’attribution d’utilisateurs à Airstack

* Il est recommandé de n’attribuer qu’un seul utilisateur Azure AD à Airstack afin de tester la configuration de l’approvisionnement automatique d’utilisateurs. Les autres utilisateurs et/ou groupes peuvent être affectés ultérieurement.

* Quand vous attribuez un utilisateur à Airstack, vous devez sélectionner un rôle valide propre à l’application (si disponible) dans la boîte de dialogue d’attribution. Les utilisateurs dont le rôle est **Accès par défaut** sont exclus de l’approvisionnement.

## <a name="setup-airstack-for-provisioning"></a>Configurer Airstack pour l’approvisionnement

1. Connectez-vous à votre [Console d’administration Airstack](https://airstack-qa.lenovosoftware.com/). Accédez aux **Paramètres**.

    ![Console d’administration Airstack](media/airstack-provisioning-tutorial/airstackadminmenu.png)

2.  Accédez à **Configuration Azure** dans le menu situé sur le côté gauche de l’écran.

    ![SCIM d’ajout Airstack](media/airstack-provisioning-tutorial/azureconfig.png)

3.  Cliquez sur le bouton **Générer**. Copiez le **Jeton secret pour Azure**. Vous devez entrer cette valeur dans le champ Jeton secret de l’onglet Approvisionnement de votre application Airstack dans le portail Azure.

    ![Jeton de création Airstack](media/airstack-provisioning-tutorial/generatetoken.png)

## <a name="add-airstack-from-the-gallery"></a>Ajouter Airstack à partir de la galerie

Avant de configurer Airstack pour l’approvisionnement automatique d’utilisateurs avec Azure AD, vous devez ajouter Airstack, à partir de la galerie d’applications Azure AD, à votre liste d’applications SaaS managées.

**Pour ajouter Airstack à partir de la galerie d’applications Azure AD, procédez comme suit :**

1. Dans le panneau de navigation gauche du **[portail Azure](https://portal.azure.com)** , sélectionnez **Azure Active Directory**.

    ![Bouton Azure Active Directory](common/select-azuread.png)

2. Accédez à **Applications d’entreprise**, puis sélectionnez **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

3. Pour ajouter une nouvelle application, cliquez sur le bouton **Nouvelle application** en haut du volet.

    ![Bouton Nouvelle application](common/add-new-app.png)

4. Dans la zone de recherche, entrez **Airstack**, sélectionnez **Airstack** dans le volet de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Airstack dans la liste des résultats](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-airstack"></a>Configuration de l’approvisionnement automatique d’utilisateurs sur Airstack 

Cette section vous guide tout au long des étapes de configuration du service d’approvisionnement d’Azure AD pour créer, mettre à jour et désactiver des utilisateurs et/ou des groupes dans Airstack en fonction des attributions d’utilisateurs et/ou de groupes dans Azure AD.

> [!TIP]
> Vous pouvez aussi choisir d’activer l’authentification unique basée sur SAML pour Airstack en suivant les instructions fournies dans le [tutoriel sur l’authentification unique pour Airstack](./airstack-tutorial.md). L’authentification unique peut être configurée indépendamment de l’attribution automatique d’utilisateurs, bien que ces deux fonctionnalités se complètent

### <a name="to-configure-automatic-user-provisioning-for-airstack-in-azure-ad"></a>Pour configurer l’approvisionnement automatique d’utilisateurs pour Airstack dans Azure AD :

1. Connectez-vous au [portail Azure](https://portal.azure.com). Sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **Airstack**.

    ![Le lien AirStack dans la liste des applications](common/all-applications.png)

3. Sélectionnez l’onglet **Approvisionnement**.

    ![Capture d’écran des options Gérer avec l’option Provisionnement en évidence.](common/provisioning.png)

4. Définissez le **Mode d’approvisionnement** sur **Automatique**.

    ![Capture d’écran de la liste déroulante Mode de provisionnement avec l’option Automatique en évidence.](common/provisioning-automatic.png)

5. Dans la section **Informations d’identification de l’administrateur**, entrez `https://api-as.lenovosoftware.com/0/as/common/scim` dans **URL de locataire**. Saisissez la valeur de **Jeton d’authentification SCIM** récupérée précédemment dans **Jeton secret**. Cliquez sur **Tester la connexion** pour vous assurer qu’Azure AD peut se connecter à Airstack. Si la connexion échoue, vérifiez que votre compte Airstack dispose des autorisations d’administrateur et réessayez.

    ![URL de locataire + Jeton](common/provisioning-testconnection-tenanturltoken.png)

6. Dans le champ **E-mail de notification**, entrez l’adresse e-mail d’une personne ou d’un groupe qui doit recevoir les notifications d’erreur d’approvisionnement, puis cochez la case **Envoyer une notification par e-mail en cas de défaillance**.

    ![E-mail de notification](common/provisioning-notification-email.png)

7. Cliquez sur **Enregistrer**.

8. Dans la section **Mappages**, sélectionnez **Synchroniser les utilisateurs Azure Active Directory sur Airstack**.

    ![Mappages d’utilisateur Airstack](media/airstack-provisioning-tutorial/mappings.png)

9. Dans la section **Mappages des attributs**, passez en revue les attributs utilisateur qui sont synchronisés entre Azure AD et Airstack. Les attributs sélectionnés en tant que propriétés de **Correspondance** sont utilisés pour faire correspondre les comptes d’utilisateur dans Airstack pour les opérations de mise à jour. Cliquez sur le bouton **Enregistrer** pour valider les modifications.

    ![Attributs utilisateur Airstack](media/airstack-provisioning-tutorial/attributes.png)

10. Pour configurer des filtres d’étendue, reportez-vous aux instructions suivantes fournies dans [Approvisionnement d’applications basé sur les attributs avec filtres d’étendue](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Pour activer le service d’approvisionnement Azure AD pour Airstack, modifiez le paramètre **État d’approvisionnement** sur **Activé** dans la section **Paramètres**.

    ![État d’approvisionnement activé](common/provisioning-toggle-on.png)

12. Définissez les utilisateurs et/ou groupes que vous souhaitez approvisionner sur Airstack en choisissant les valeurs souhaitées dans **Étendue** de la section **Paramètres**.

    ![Étendue de l’approvisionnement](common/provisioning-scope.png)

13. Lorsque vous êtes prêt à effectuer l’approvisionnement, cliquez sur **Enregistrer**.

    ![Enregistrement de la configuration de l’approvisionnement](common/provisioning-configuration-save.png)

Cette opération démarre la synchronisation initiale de tous les utilisateurs et/ou groupes définis dans **Étendue** dans la section **Paramètres**. La synchronisation initiale prend plus de temps que les synchronisations suivantes, qui se produisent toutes les 40 minutes environ tant que le service de provisionnement Azure AD est en cours d’exécution. Vous pouvez utiliser la section **Détails de synchronisation** pour surveiller la progression et suivre les liens vers le rapport d’activité d’approvisionnement, qui décrit toutes les actions effectuées par le service d’approvisionnement Azure AD sur Airstack.

Pour plus d’informations sur la lecture des journaux d’activité d’approvisionnement Azure AD, consultez [Création de rapports sur l’approvisionnement automatique de comptes d’utilisateur](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Gestion de l’approvisionnement de comptes d’utilisateur pour les applications d’entreprise](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Étapes suivantes

* [Découvrez comment consulter les journaux d’activité et obtenir des rapports sur l’activité d’approvisionnement](../app-provisioning/check-status-user-account-provisioning.md)