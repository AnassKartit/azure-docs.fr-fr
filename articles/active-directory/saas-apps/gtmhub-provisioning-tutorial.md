---
title: 'Tutoriel : Configurer Gtmhub pour l’attribution automatique d’utilisateurs avec Azure Active Directory | Microsoft Docs'
description: Découvrez comment provisionner et déprovisionner automatiquement des comptes d’utilisateur d’Azure AD à Gtmhub.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 10b68d00-a544-480b-9bd6-f6ac291a90d0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/03/2020
ms.author: Zhchia
ms.openlocfilehash: 32c19622218d68f094c84d9e7ae54d418ea3fc2b
ms.sourcegitcommit: d488a97dc11038d9cef77a0235d034677212c8b3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2020
ms.locfileid: "97709981"
---
# <a name="tutorial-configure-gtmhub-for-automatic-user-provisioning"></a>Tutoriel : Configurer Gtmhub pour l’attribution automatique d’utilisateurs

Ce tutoriel décrit les étapes à suivre dans Gtmhub et Azure Active Directory (Azure AD) pour configurer l’attribution automatique d’utilisateurs. Une fois configuré, Azure AD provisionne et déprovisionne automatiquement les utilisateurs et les groupes pour [Gtmhub](https://www.gtmhub.com/) à l’aide du service de provisionnement Azure AD. Pour découvrir les informations importantes sur ce que fait ce service, comment il fonctionne et consulter le forum aux questions, reportez-vous à l’article [Automatiser l’attribution et l’annulation de l’attribution des utilisateurs dans les applications SaaS avec Azure Active Directory](../manage-apps/user-provisioning.md). 

>[!NOTE]
>Actuellement, lorsque l’attribution automatique d’utilisateurs est configurée, Azure AD déprovisionne automatiquement les utilisateurs et les groupes pour Gtmhub, et mappe les utilisateurs à leurs équipes respectives à l’aide du service de provisionnement Azure AD. Toutefois, en 2021, une fois que l’authentification unique est activée avec Gtmhub, les utilisateurs sont automatiquement attribués lorsqu’ils se connectent via SSO et sont affectés à leur équipe respective.


## <a name="capabilities-supported"></a>Fonctionnalités prises en charge
> [!div class="checklist"]
> * Supprimer des utilisateurs dans Gtmhub lorsqu’ils n’ont plus besoin d’accès
> * Conserver les attributs utilisateur synchronisés entre Azure AD et Gtmhub
> * Mapper automatiquement les utilisateurs dans leurs équipes et les aligner

## <a name="prerequisites"></a>Prérequis

Le scénario décrit dans ce tutoriel part du principe que vous disposez des prérequis suivants :

* [Un locataire Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).
* Un compte d’utilisateur dans Azure AD avec l’[autorisation](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) de configurer l’approvisionnement (par exemple, administrateur d’application, administrateur d’application Cloud, propriétaire d’application ou administrateur général). 
* Un compte Gtmhub d’entreprise.

## <a name="step-1-plan-your-provisioning-deployment"></a>Étape 1. Planifier votre déploiement de l’approvisionnement
1. En savoir plus sur le [fonctionnement du service d’approvisionnement](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Déterminez qui sera dans l’[étendue pour l’approvisionnement](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Déterminez les données à [mapper entre Azure AD et Gtmhub](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-gtmhub-to-support-team-mapping-and-user-de-provisioning-with-azure-ad"></a>Étape 2. Configurer Gtmhub pour prendre en charge le mappage d’équipe et le déprovisionnement d’utilisateur avec Azure AD

Pour connecter votre application de provisionnement à votre compte Gtmhub, vous devez émettre un jeton SCIM et compiler l’URL du locataire.

### <a name="to-issue-a-new-scim-token"></a>Pour émettre un nouveau jeton SCIM :

1. Connectez-vous à votre **compte Gtmhub**. Accédez à **Paramètres > Configuration > Jetons d’API**.

    ![Onglet Jetons d’API](media/gtmhub-provisioning-tutorial/api-tokens.png)
2. Cliquez sur **Émettre un jeton** et sélectionnez **SCIM**. Entrez un nom pour le jeton et cliquez sur le bouton **Générer un jeton d’API**.

    ![Onglet Générer des jetons](media/gtmhub-provisioning-tutorial/generate-token.png)
3. Une fois le jeton généré, vous pouvez le copier et l’utiliser dans votre application de provisionnement Azure AD.

    ![Copie du jeton](media/gtmhub-provisioning-tutorial/token.png)

### <a name="to-compile-the-tenant-url"></a>Pour compiler l’URL du locataire :

1. L’URL de votre locataire doit être au format suivant :

    `https://app.gtmhub.com/api/v1/scim/azure/{account_id}`

2. Si votre compte Gtmhub se trouve dans le centre de données des États-Unis, vous devrez également ajouter le centre de données à l’URL :
    
     `https://app.us.gtmhub.com/api/v1/scim/azure/{account_id}`

3. Pour obtenir l’ID de compte, accédez à **Paramètres**, puis sélectionnez l’onglet **Jetons d’API** et copiez l’ID de compte :  ![Account ID](media/gtmhub-provisioning-tutorial/account-id.png) (ID de compte)

## <a name="step-3-add-gtmhub-from-the-azure-ad-application-gallery"></a>Étape 3. Ajouter Gtmhub à partir de la galerie d’applications Azure AD

Ajoutez Gtmhub à partir de la galerie d’applications Azure AD pour commencer à gérer le provisionnement dans Gtmhub. Si vous avez déjà configuré Gtmhub pour l’authentification SSO, vous pouvez utiliser la même application. Toutefois, il est recommandé de créer une application distincte lors du test initial de l’intégration. En savoir plus sur l’ajout d’une application à partir de la galerie [ici](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Étape 4. Définir qui sera dans l’étendue pour l’approvisionnement 

Le service d’approvisionnement Azure AD vous permet de définir l’étendue des utilisateurs approvisionnés en fonction de l’affectation à l’application et/ou en fonction des attributs de l’utilisateur/groupe. Si vous choisissez de définir l’étendue de l’approvisionnement pour votre application en fonction de l’attribution, vous pouvez utiliser les étapes de [suivantes](../manage-apps/assign-user-or-group-access-portal.md) pour affecter des utilisateurs et des groupes à l’application. Si vous choisissez de définir l’étendue de l’approvisionnement en fonction uniquement des attributs de l’utilisateur ou du groupe, vous pouvez utiliser un filtre d’étendue comme décrit [ici](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 

* Quand vous attribuez des utilisateurs et des groupes à Gtmhub, vous devez sélectionner un autre rôle que le rôle **Accès par défaut**. Les utilisateurs disposant du rôle Accès par défaut sont exclus de l’approvisionnement et sont marqués comme non autorisés dans les journaux de configuration. Si le seul rôle disponible dans l’application est le rôle d’accès par défaut, vous pouvez [mettre à jour le manifeste de l’application](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) pour ajouter des rôles supplémentaires. 

* Commencez progressivement. Testez avec un petit ensemble d’utilisateurs et de groupes avant d’effectuer un déploiement général. Lorsque l’étendue de l’approvisionnement est définie sur les utilisateurs et les groupes attribués, vous pouvez contrôler cela en affectant un ou deux utilisateurs ou groupes à l’application. Lorsque l’étendue est définie sur tous les utilisateurs et groupes, vous pouvez spécifier un [filtre d’étendue basé sur l’attribut](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 


## <a name="step-5-configure-automatic-user-provisioning-to-gtmhub"></a>Étape 5. Configurer l’attribution automatique d’utilisateurs dans Gtmhub 

Cette section vous guide tout au long des étapes de configuration du service d’approvisionnement d’Azure AD pour créer, mettre à jour et désactiver des utilisateurs et/ou des groupes dans TestApp en fonction des assignations d’utilisateurs et/ou de groupes dans Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-gtmhub-in-azure-ad"></a>Pour configurer l’attribution automatique d’utilisateurs pour Gtmhub dans Azure AD :

1. Connectez-vous au [portail Azure](https://portal.azure.com). Sélectionnez **Applications d’entreprise**, puis **Toutes les applications**.

    ![Panneau Applications d’entreprise](common/enterprise-applications.png)

2. Dans la liste des applications, sélectionnez **Gtmhub**.

    ![Lien Gtmhub dans la liste des applications](common/all-applications.png)

3. Sélectionnez l’onglet **Approvisionnement**.

    ![Onglet Approvisionnement](common/provisioning.png)

4. Définissez le **Mode d’approvisionnement** sur **Automatique**.

    ![Onglet Provisionnement automatique](common/provisioning-automatic.png)

5. Sous la section **Informations d’identification de l’administrateur**, entrez l’URL de locataire et le Jeton secret de Gtmhub. Cliquez sur **Tester la connexion** pour vérifier qu’Azure AD peut se connecter à Gtmhub. Si la connexion échoue, vérifiez que votre compte Gtmhub dispose des autorisations d’administrateur et réessayez.

    ![par jeton](common/provisioning-testconnection-tenanturltoken.png)

6. Dans le champ **E-mail de notification**, entrez l’adresse e-mail de la personne ou du groupe qui doit recevoir les notifications d’erreur de provisionnement et sélectionnez la case à cocher **Envoyer une notification par e-mail en cas de défaillance**.

    ![E-mail de notification](common/provisioning-notification-email.png)

7. Sélectionnez **Enregistrer**.

8. Dans la section **Mappages**, sélectionnez **Synchroniser les utilisateurs Azure Active Directory avec Gtmhub**.

9. Dans la section **Mappages des attributs**, vérifiez les attributs d’utilisateurs qui sont synchronisés d’Azure AD à Gtmhub. Les attributs sélectionnés en tant que propriétés de **Correspondance** sont utilisés pour faire correspondre les comptes d’utilisateur dans Gtmhub pour les opérations de mise à jour. Si vous choisissez de changer l’[attribut cible correspondant](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes), vous devez vérifier que l’API Gtmhub prend en charge le filtrage des utilisateurs en fonction de cet attribut. Cliquez sur le bouton **Enregistrer** pour valider les modifications.

   |Attribut|Type|Pris en charge pour le filtrage|
   |---|---|---|
   |userName|String|&check;|
   |externalId|String|&check;|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|Informations de référence|

10. Pour configurer des filtres d’étendue, reportez-vous aux instructions suivantes fournies dans [Approvisionnement d’applications basé sur les attributs avec filtres d’étendue](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Pour activer le service de provisionnement Azure AD pour Gtmhub, définissez le paramètre **État de l’approvisionnement** sur **Activé** dans la section **Paramètres**.

    ![État d’approvisionnement activé](common/provisioning-toggle-on.png)

14. Définissez les utilisateurs et/ou les groupes que vous souhaitez provisionner sur Gtmhub en choisissant les valeurs souhaitées dans **Étendue** de la section **Paramètres**.

    ![Étendue de l’approvisionnement](common/provisioning-scope.png)

15. Lorsque vous êtes prêt à effectuer l’approvisionnement, cliquez sur **Enregistrer**.

    ![Enregistrement de la configuration de l’approvisionnement](common/provisioning-configuration-save.png)

Cette opération démarre le cycle de synchronisation initiale de tous les utilisateurs et groupes définis dans **Étendue** dans la section **Paramètres**. Le cycle de synchronisation initiale prend plus de temps que les cycles de synchronisation suivants, qui se produisent toutes les 40 minutes environ tant que le service de provisionnement Azure AD est en cours d’exécution. 

## <a name="step-6-monitor-your-deployment"></a>Étape 6. Surveiller votre déploiement
Une fois que vous avez configuré l’approvisionnement, utilisez les ressources suivantes pour surveiller votre déploiement :

* Utilisez les [journaux d’approvisionnement](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) pour déterminer quels utilisateurs ont été configurés avec succès ou échoué.
* Consultez la [barre de progression](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) pour afficher l’état du cycle d’approvisionnement et quand il se termine
* Si la configuration de l’approvisionnement semble se trouver dans un état non sain, l’application passe en quarantaine. Pour en savoir plus sur les états de quarantaine, cliquez [ici](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).  

## <a name="additional-resources"></a>Ressources supplémentaires

* [Gestion de l’approvisionnement de comptes d’utilisateur pour les applications d’entreprise](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Étapes suivantes

* [Découvrez comment consulter les journaux d’activité et obtenir des rapports sur l’activité d’approvisionnement](../manage-apps/check-status-user-account-provisioning.md)
