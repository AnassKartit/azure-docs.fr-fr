---
author: kengaderdus
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 04/05/2021
ms.author: kengaderdus
ms.openlocfilehash: 894da75a364a6e15cc53fe39427327713680a28c
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2021
ms.locfileid: "130036409"
---
#### <a name="app-registrations"></a>[Inscriptions des applications](#tab/app-reg-ga/) 

1. Sous **Gérer**, sélectionnez **Autorisations de l’API**.
1. Sous **Autorisations configurées**, sélectionnez **Ajouter une autorisation**.
1. Sélectionnez l’onglet **API Microsoft**, puis **Microsoft Graph**.
1. Sélectionnez **Autorisations de l’application**.
1. Développez le groupe d’autorisations approprié et activez la case à cocher de l’autorisation à accorder à votre application de gestion. Par exemple :
    * **User** > **User.ReadWrite.All** : Pour les scénarios de migration ou de gestion d’utilisateurs.
    * **Group** > **Group.ReadWrite.All**: Pour créer des groupes, lire et mettre à jour les appartenances aux groupes et supprimer des groupes.
    * **AuditLog** > **AuditLog.Read.All** : Pour lire les journaux d’audit de l’annuaire.
    * **Policy** > **Policy.ReadWrite.TrustFramework** : Pour les scénarios d’intégration continue/livraison continue (CI/CD). Par exemple, pour le déploiement de stratégies personnalisées avec Azure Pipelines.
1. Sélectionnez **Ajouter des autorisations**. Comme vous l’indiquent les instructions, patientez quelques minutes avant de passer à l’étape suivante.
1. Sélectionnez **Accorder le consentement de l’administrateur pour (nom de votre abonné)** .
1. Si vous n’êtes pas connecté avec un compte Administrateur général, connectez-vous avec un compte de votre locataire Azure AD B2C ayant au minimum le rôle *Administrateur d’application cloud*, puis sélectionnez **Accorder le consentement administrateur pour (nom de votre locataire)** .
1. Sélectionnez **Actualiser**, puis vérifiez que la mention « Accordé pour ... » apparaît sous **État**. La propagation des autorisations peut prendre quelques minutes.

#### <a name="applications-legacy"></a>[Applications (héritées)](#tab/applications-legacy/)

1. Dans la page de vue d’ensemble **Application inscrite**, sélectionnez **Paramètres**.
1. Sous **Accès d’API**, sélectionnez **Autorisations requises**.
1. Sélectionnez **Microsoft Graph**.
1. Sous **Autorisations de l’application**, activez la case à cocher de l’autorisation à accorder à votre application de gestion. Par exemple :
    * **Lire toutes les données du journal d’audit** : Sélectionnez cette autorisation pour lire les journaux d’audit de l’annuaire.
    * **Accéder en lecture et en écriture aux données de l’annuaire** : Sélectionnez cette autorisation pour les scénarios de migration utilisateur ou de gestion des utilisateurs.
    * **Accéder en lecture et en écriture aux stratégies d’infrastructure de confiance de votre organisation** : Sélectionnez cette autorisation pour les scénarios d’intégration continue/livraison continue (CI/CD). Par exemple, pour le déploiement de stratégies personnalisées avec Azure Pipelines.
1. Sélectionnez **Enregistrer**.
1. Sélectionnez **Accorder des autorisations**, puis **Oui**. La propagation complète des autorisations peut prendre quelques minutes.
