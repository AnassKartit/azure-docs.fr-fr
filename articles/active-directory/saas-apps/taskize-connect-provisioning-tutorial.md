---
title: 'Tutoriel : Configurer Taskize Connect pour l’attribution automatique d’utilisateurs avec Azure Active Directory | Microsoft Docs'
description: Découvrez comment attribuer et désattribuer automatiquement des comptes d’utilisateur d’Azure AD dans Taskize Connect.
author: twimmers
writer: twimmers
manager: beatrizd
ms.assetid: 295b6542-879d-4330-afd7-e8867d83464d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/20/2021
ms.author: thwimmer
ms.openlocfilehash: e24c7600b34f4daabf15710a4d6a3791bba7b0a8
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128701493"
---
# <a name="tutorial-configure-taskize-connect-for-automatic-user-provisioning"></a>Tutoriel : Configurer Taskize Connect pour l’attribution automatique d’utilisateurs

Ce tutoriel décrit les étapes à effectuer à la fois dans Taskize Connect et dans Azure Active Directory (Azure AD) pour configurer l’attribution automatique d’utilisateurs. Une fois la configuration terminée, Azure AD utilise son service de provisionnement pour attribuer automatiquement des utilisateurs et des groupes à [Taskize Connect](https://www.taskize.com/), ainsi que pour annuler automatiquement des attributions d’utilisateurs et de groupes. Pour découvrir les informations importantes sur ce que fait ce service, comment il fonctionne et consulter le forum aux questions, reportez-vous à l’article [Automatiser l’attribution et l’annulation de l’attribution des utilisateurs dans les applications SaaS avec Azure Active Directory](../app-provisioning/user-provisioning.md). 


## <a name="capabilities-supported"></a>Fonctionnalités prises en charge
> [!div class="checklist"]
> * Créer des utilisateurs dans Taskize Connect.
> * Supprimer les utilisateurs dans Taskize Connect quand ils n’ont plus besoin d’y accéder.
> * Maintenir la synchronisation des attributs utilisateur entre Azure AD et Taskize Connect.
> * Provisionner des groupes et des appartenances aux groupes dans Taskize Connect
> * [Authentification unique](taskize-connect-tutorial.md) auprès de Taskize Connect (recommandé).

## <a name="prerequisites"></a>Prérequis

Le scénario décrit dans ce tutoriel part du principe que vous disposez des prérequis suivants :

* [Un locataire Azure AD](../develop/quickstart-create-new-tenant.md). 
* Un compte d’utilisateur dans Azure AD avec l’[autorisation](../roles/permissions-reference.md) de configurer l’approvisionnement (par exemple, administrateur d’application, administrateur d’application Cloud, propriétaire d’application ou administrateur général). 
* Un locataire [Taskize Connect](https://www.taskize.com/).
* Un compte d’utilisateur, dans Taskize Connect, disposant d’une autorisation d’administrateur.


## <a name="step-1-plan-your-provisioning-deployment"></a>Étape 1. Planifier votre déploiement de l’approvisionnement
1. En savoir plus sur le [fonctionnement du service d’approvisionnement](../app-provisioning/user-provisioning.md).
1. Déterminez qui sera dans l’[étendue pour l’approvisionnement](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).
1. Déterminez les données à [mapper entre Azure AD et Taskize Connect](../app-provisioning/customize-application-attributes.md). 

## <a name="step-2-configure-taskize-connect-to-support-provisioning-with-azure-ad"></a>Étape 2. Configurer Taskize Connect pour prendre en charge le provisionnement avec Azure AD

Pour configurer Taskize Connect afin de prendre en charge le provisionnement avec Azure AD, contactez l’[équipe du support technique Taskize Connect](mailto:support@taskize.com).

## <a name="step-3-add-taskize-connect-from-the-azure-ad-application-gallery"></a>Étape 3. Ajouter Taskize Connect à partir de la galerie d’applications Azure AD



Ajoutez Taskize Connect à partir de la galerie d’applications Azure AD pour commencer à gérer le provisionnement dans Taskize Connect. Si vous avez déjà configuré Taskize Connect pour l’authentification unique, vous pouvez utiliser la même application. Toutefois, il est recommandé de créer une application distincte lors du test initial de l’intégration. En savoir plus sur l’ajout d’une application à partir de la galerie [ici](../manage-apps/add-application-portal.md).

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Étape 4. Définir qui sera dans l’étendue pour l’approvisionnement 

Le service d’approvisionnement Azure AD vous permet de définir l’étendue des utilisateurs approvisionnés en fonction de l’affectation à l’application et/ou en fonction des attributs de l’utilisateur/groupe. Si vous choisissez de définir l’étendue de l’approvisionnement pour votre application en fonction de l’attribution, vous pouvez utiliser les étapes de [suivantes](../manage-apps/assign-user-or-group-access-portal.md) pour affecter des utilisateurs et des groupes à l’application. Si vous choisissez de définir l’étendue de l’approvisionnement en fonction uniquement des attributs de l’utilisateur ou du groupe, vous pouvez utiliser un filtre d’étendue comme décrit [ici](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 

* Lors de l’attribution d’utilisateurs et de groupes à Taskize Connect, vous devez sélectionner un rôle autre que le rôle **Accès par défaut**. Les utilisateurs disposant du rôle Accès par défaut sont exclus de l’approvisionnement et sont marqués comme non autorisés dans les journaux de configuration. Si le seul rôle disponible sur l’application est le rôle d’accès par défaut, vous pouvez [mettre à jour le manifeste d’application](../develop/howto-add-app-roles-in-azure-ad-apps.md) pour ajouter plus de rôles. 

* Commencez progressivement. Testez avec un petit ensemble d’utilisateurs et de groupes avant d’effectuer un déploiement général. Lorsque l’étendue de l’approvisionnement est définie sur les utilisateurs et les groupes attribués, vous pouvez contrôler cela en affectant un ou deux utilisateurs ou groupes à l’application. Lorsque l’étendue est définie sur tous les utilisateurs et groupes, vous pouvez spécifier un [filtre d’étendue basé sur l’attribut](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md). 


## <a name="step-5-configure-automatic-user-provisioning-to-taskize-connect"></a>Étape 5. Configurer l’attribution automatique d’utilisateurs dans Taskize Connect 

Cette section vous guide tout au long des étapes de configuration du service de provisionnement d’Azure AD pour créer, mettre à jour et désactiver des utilisateurs et/ou des groupes dans Taskize Connect en fonction des affectations d’utilisateurs et/ou de groupes dans Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-taskize-connect-in-azure-ad"></a>Pour configurer l’attribution automatique d’utilisateurs pour Taskize Connect dans Azure AD :

1. Connectez-vous au [portail Azure](https://portal.azure.com). Sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

1. Dans la liste des applications, sélectionnez **Taskize Connect**.

    ![Lien Taskize Connect dans la liste des applications](common/all-applications.png)

1. Sélectionnez l’onglet **Approvisionnement**.

    ![Onglet Approvisionnement](common/provisioning.png)

1. Définissez le **Mode d’approvisionnement** sur **Automatique**.

    ![Onglet Provisionnement automatique](common/provisioning-automatic.png)

1. Dans la section **Informations d’identification de l’administrateur**, cliquez sur Autoriser, puis veillez à entrer les informations d’identification d’administrateur de votre compte Taskize Connect. Cliquez sur **Tester la connexion** pour vérifier qu’Azure AD peut se connecter à Taskize Connect. Si la connexion échoue, vérifiez que votre compte Taskize Connect dispose des autorisations d’administrateur, puis réessayez.

   ![par jeton](media/taskize-connect-provisioning-tutorial/taskize-connect-authorize.png)
1. Dans le champ **E-mail de notification**, entrez l’adresse e-mail de la personne ou du groupe qui doit recevoir les notifications d’erreur de provisionnement et sélectionnez la case à cocher **Envoyer une notification par e-mail en cas de défaillance**.

    ![E-mail de notification](common/provisioning-notification-email.png)

1. Sélectionnez **Enregistrer**.

1. Dans la section **Mappages**, sélectionnez **Synchroniser les utilisateurs Azure Active Directory avec Taskize Connect**.

1. Passez en revue les attributs d’utilisateur qui sont synchronisés d’Azure AD vers Taskize Connect dans la section **Mappage d’attributs**. Les attributs sélectionnés comme propriétés de **Correspondance** sont utilisés pour faire correspondre les comptes d’utilisateur dans Taskize Connect pour les opérations de mise à jour. Si vous choisissez de changer l’[attribut cible correspondant](../app-provisioning/customize-application-attributes.md), vérifiez que l’API Taskize Connect prend en charge le filtrage des utilisateurs en fonction de cet attribut. Cliquez sur le bouton **Enregistrer** pour valider les modifications.

   |Attribut|Type|Pris en charge pour le filtrage|
   |---|---|---|
   |userName|String|&check;
   |active|Boolean|   
   |displayName|String|
   |name.formatted|String|
   |externalId|String|
   
1. Dans la section **Mappages**, sélectionnez **Synchroniser les groupes Azure Active Directory avec Taskize Connect**.

1. Passez en revue les attributs de groupe qui sont synchronisés d’Azure AD vers Taskize Connect dans la section **Mappage d’attributs**. Les attributs sélectionnés comme propriétés de **Correspondance** sont utilisés pour faire correspondre les groupes dans Taskize Connect pour les opérations de mise à jour. Cliquez sur le bouton **Enregistrer** pour valider les modifications.

      |Attribut|Type|Pris en charge pour le filtrage|
      |---|---|---|
      |displayName|String|&check;
      |externalId|String|
      |membres|Informations de référence|

1. Pour configurer des filtres d’étendue, reportez-vous aux instructions suivantes fournies dans [Approvisionnement d’applications basé sur les attributs avec filtres d’étendue](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

1. Pour activer le service de provisionnement Azure AD pour Taskize Connect, changez l’option **État du provisionnement** en lui affectant la valeur **Activé** dans la section **Paramètres**.

    ![État d’approvisionnement activé](common/provisioning-toggle-on.png)

1. Définissez les utilisateurs et/ou groupes que vous voulez attribuer dans Taskize Connect en choisissant les valeurs souhaitées dans **Étendue**, dans la section **Paramètres**.

    ![Étendue de l’approvisionnement](common/provisioning-scope.png)

1. Lorsque vous êtes prêt à effectuer l’approvisionnement, cliquez sur **Enregistrer**.

    ![Enregistrement de la configuration de l’approvisionnement](common/provisioning-configuration-save.png)

Cette opération démarre le cycle de synchronisation initiale de tous les utilisateurs et groupes définis dans **Étendue** dans la section **Paramètres**. Le cycle de synchronisation initiale prend plus de temps que les cycles de synchronisation suivants, qui se produisent toutes les 40 minutes environ tant que le service de provisionnement Azure AD est en cours d’exécution. 

## <a name="step-6-monitor-your-deployment"></a>Étape 6. Surveiller votre déploiement
Une fois que vous avez configuré l’approvisionnement, utilisez les ressources suivantes pour surveiller votre déploiement :

* Utilisez les [journaux d’approvisionnement](../reports-monitoring/concept-provisioning-logs.md) pour déterminer quels utilisateurs ont été configurés avec succès ou échoué.
* Consultez la [barre de progression](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) pour afficher l’état du cycle d’approvisionnement et quand il se termine
* Si la configuration de l’approvisionnement semble se trouver dans un état non sain, l’application passe en quarantaine. Pour en savoir plus sur les états de quarantaine, cliquez [ici](../app-provisioning/application-provisioning-quarantine-status.md).  

## <a name="more-resources"></a>Plus de ressources

* [Gestion de l’approvisionnement de comptes d’utilisateur pour les applications d’entreprise](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Étapes suivantes

* [Découvrez comment consulter les journaux d’activité et obtenir des rapports sur l’activité d’approvisionnement](../app-provisioning/check-status-user-account-provisioning.md)