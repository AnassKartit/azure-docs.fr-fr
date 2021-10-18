---
title: Configurer la facturation pour un Contrat client Microsoft - Azure
description: Découvrez comment configurer votre compte de facturation associé à un contrat client Microsoft. Consultez les prérequis de configuration ainsi que les autres ressources disponibles.
author: amberbhargava
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 10/07/2021
ms.author: banders
ms.openlocfilehash: 982bb3719de66d880f635dbb7c95e9ae2c4590c2
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2021
ms.locfileid: "129711156"
---
# <a name="set-up-your-billing-account-for-a-microsoft-customer-agreement"></a>Découvrez comment configurer votre compte de facturation associé à un contrat client Microsoft

Si votre inscription à un Contrat Entreprise direct a expiré ou est sur le point d’expirer, vous pouvez signer un contrat client Microsoft pour renouveler votre inscription. Cet article décrit les modifications apportées à votre facturation existante après la configuration et vous guide tout au long de ce processus. Actuellement, les contrats Entreprise indirects arrivant à expiration ne peuvent pas être renouvelés avec un contrat client Microsoft.

Ce renouvellement comprend les étapes suivantes :

1. Accepter le nouveau contrat client Microsoft. Collaborer avec votre représentant Microsoft local pour comprendre chaque détail et accepter le nouveau contrat.
2. Configurer le nouveau compte de facturation créé pour le nouveau contrat client Microsoft.

Pour configurer le compte de facturation, vous devez transférer la facturation des abonnements Azure de votre inscription à un Contrat Entreprise vers le nouveau compte de facturation. Le processus de configuration n’a aucun impact sur les services Azure qui s’exécutent dans vos abonnements. Toutefois, il modifie la façon dont vous allez gérer la facturation pour vos abonnements.

- Plutôt que d’utiliser le [portail EA](https://ea.azure.com) pour gérer vos services Azure et la facturation, vous allez vous servir du [portail Azure](https://portal.azure.com).
- Vous obtiendrez une facture mensuelle et numérique répertoriant tous vos frais. Vous pouvez voir et analyser la facture dans la page Cost Management + Billing.
- Plutôt que des départements et un compte dans votre inscription à un Accord Entreprise, vous allez utiliser les étendues et la structure de facturation du nouveau compte pour gérer et organiser votre facturation.

Avant de commencer l’installation, nous vous recommandons d’effectuer les actions suivantes :

- **Découvrez votre nouveau compte de facturation**
  - Votre nouveau compte simplifie la facturation pour votre organisation. [Jetez un œil à votre nouveau compte de facturation](../understand/mca-overview.md)
- **Vérifiez votre accès pour terminer la configuration**
  - Seuls les utilisateurs avec certaines autorisations administratives peuvent terminer la configuration. Vérifiez si vous avez [l’accès requis pour terminer la configuration](#access-required-to-complete-the-setup).
- **Explorez les modifications apportées à votre hiérarchie de facturation**
  - Votre nouveau compte de facturation est organisé différemment de votre inscription à un Contrat Entreprise. [Explorez les modifications apportées à votre hiérarchie de facturation dans le nouveau compte](#understand-changes-to-your-billing-hierarchy).
- **Explorez les modifications apportées à l’accès de vos administrateurs pour la facturation**
  - Les administrateurs de votre inscription à un Contrat Entreprise ont accès aux étendues de facturation dans le nouveau compte. [Découvrez les modifications apportées à leur accès](#changes-to-billing-administrator-access).
- **Consultez les fonctionnalités du Contrat Entreprise qui sont remplacées par le nouveau compte**
  - Consultez les fonctionnalités de l’inscription au Contrat Entreprise qui sont remplacées par des fonctionnalités dans le nouveau compte.
- **Lisez les réponses aux questions les plus fréquemment posées**
  - Lisez les [informations supplémentaires](#additional-information) pour en apprendre davantage sur la configuration.

## <a name="access-required-to-complete-the-setup"></a>Accès requis pour terminer la configuration

Pour terminer la configuration, vous avez besoin de l’accès suivant :

- Propriétaire du profil de facturation qui a été créé lors de la signature du contrat client Microsoft. Pour en savoir plus sur les profils de facturation, consultez [Comprendre les profils de facturation](../understand/mca-overview.md#billing-profiles).  
&mdash; et &mdash;
- Administrateur d’entreprise pour l’inscription qui est renouvelée.

### <a name="start-migration-and-get-permission-needed-to-complete-setup"></a>Démarrer la migration et obtenir l’autorisation nécessaire pour terminer la configuration

Vous pouvez utiliser les options suivantes pour démarrer la migration de votre inscription EA vers votre Contrat client Microsoft.


- Connectez-vous au portail Azure à l’aide du lien se trouvant dans l’e-mail qui vous a été envoyé lorsque vous avez signé le contrat client Microsoft.

- Si vous ne trouvez pas cet e-mail, connectez-vous à l’aide du lien suivant.

  `https://portal.azure.com/#blade/Microsoft_Azure_SubscriptionManagement/TransitionEnrollment`

Si vous avez les deux rôles de propriétaire du compte de facturation et d’administrateur d’entreprise ou le rôle de profil de facturation, la page suivante s’affiche dans le portail Azure. Vous pouvez poursuivre la configuration de vos inscriptions EA et de votre compte de facturation du Contrat client Microsoft pour la transition.

:::image type="content" source="./media/mca-setup-account/setup-billing-account-page.png" alt-text="Capture d’écran montrant la page Configurer votre compte de facturation" lightbox="./media/mca-setup-account/setup-billing-account-page.png" :::

Si vous n’avez pas le rôle d’administrateur d’entreprise pour le Contrat Entreprise ni le rôle de propriétaire du profil de facturation pour le Contrat client Microsoft, aidez-vous des informations suivantes pour obtenir l’accès dont vous avez besoin pour terminer la configuration.

#### <a name="if-youre-not-an-enterprise-administrator-on-the-enrollment"></a>Si vous n’êtes pas un administrateur d’entreprise pour l’inscription

La page suivante s’affiche dans le portail Azure si vous avez un rôle de compte de facturation ou un rôle de propriétaire de profil de facturation, mais que vous n’avez pas de rôle d’administrateur d’entreprise.

:::image type="content" source="./media/mca-setup-account/setup-billing-account-page-not-ea-administrator.png" alt-text="Capture d’écran montrant la page Configurer votre compte de facturation - Préparer vos inscriptions Contrat Entreprise pour la transition." lightbox="./media/mca-setup-account/setup-billing-account-page-not-ea-administrator.png" :::

Deux options s'offrent à vous :

- Demandez à l’administrateur d’entreprise de l’inscription de vous attribuer le rôle d’administrateur d’entreprise. Pour plus d’informations, consultez [Créer un autre administrateur d’entreprise](ea-portal-administration.md#create-another-enterprise-administrator).
-  Vous pouvez attribuer à un administrateur d’entreprise le rôle de propriétaire du compte de facturation ou de propriétaire du profil de facturation. Pour plus d’informations, consultez [Gérer les rôles de facturation sur le portail Azure](understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).

Si vous avez le rôle d’administrateur d’entreprise, copiez le lien fourni dans la page Configurer votre compte de facturation. Ouvrez-le dans votre navigateur web pour poursuivre la configuration de votre Contrat client Microsoft. Vous pouvez sinon envoyer le lien à l’administrateur d’entreprise.

#### <a name="if-youre-not-an-owner-of-the-billing-profile"></a>Si vous n’êtes pas un propriétaire du profil de facturation

Si vous êtes administrateur d’entreprise, mais que vous n’avez pas de compte de facturation, l’erreur suivante s’affiche dans le portail Azure et empêche la transition.

Si vous savez que vous avez un rôle de propriétaire du profil de facturation pour le Contrat client Microsoft en question et que vous voyez le message suivant, vérifiez que vous êtes dans le bon locataire pour votre organisation. Vous devrez peut-être changer d’annuaire.

:::image type="content" source="./media/mca-setup-account/setup-billing-account-page-not-billing-account-profile-owner.png" alt-text="Capture d’écran montrant la page Configurer votre compte de facturation - Compte de facturation du Contrat client Microsoft." lightbox="./media/mca-setup-account/setup-billing-account-page-not-billing-account-profile-owner.png" :::

Deux options s'offrent à vous :

- Demandez à un propriétaire de compte de facturation existant de vous attribuer un rôle de propriétaire du compte de facturation ou de propriétaire du profil de facturation. Pour plus d’informations, consultez [Gérer les rôles de facturation sur le portail Azure](understand-mca-roles.md#manage-billing-roles-in-the-azure-portal).
- Attribuez le rôle d’administrateur d’entreprise à un propriétaire de compte de facturation existant. Pour plus d’informations, consultez [Créer un autre administrateur d’entreprise](ea-portal-administration.md#create-another-enterprise-administrator).

Si vous obtenez le rôle de propriétaire du compte de facturation ou de propriétaire du profil de facturation, copiez le lien fourni dans la page Configurer votre compte de facturation. Ouvrez-le dans votre navigateur web pour poursuivre la configuration de votre Contrat client Microsoft. Vous pouvez sinon envoyer le lien au propriétaire du compte de facturation.

#### <a name="prepare-enrollment-for-transition"></a>Préparer l’inscription pour la transition

Une fois que vous disposez d’un accès propriétaire à vos profils d’inscription et de facturation EA, vous les préparez pour la transition.

Ouvrez la migration qui vous a été présentée précédemment ou le lien qui vous a été envoyé par e-mail. Le lien est `https://portal.azure.com/#blade/Microsoft_Azure_SubscriptionManagement/TransitionEnrollment`.

L’image suivante montre un exemple de la fenêtre Préparer vos inscriptions Contrat Entreprise pour la transition.

:::image type="content" source="./media/mca-setup-account/setup-billing-account-prepare-enrollment-transition.png" alt-text="Capture d’écran montrant la page Configurer votre compte de facturation – Préparer vos inscriptions Contrat Entreprise pour la transition" lightbox="./media/mca-setup-account/setup-billing-account-prepare-enrollment-transition.png" :::

Ensuite, sélectionnez l’inscription source pour la transition. Sélectionnez alors le compte de facturation et le profil de facturation. Si la validation réussit sans aucun problème similaire à l’écran suivant, sélectionnez **Continuer** pour continuer.

:::image type="content" source="./media/mca-setup-account/setup-billing-account-prepare-enrollment-transition-continue.png" alt-text="Capture d’écran montrant la page Configurer votre compte de facturation – Préparer vos inscriptions Contrat Entreprise pour la transition avec des choix validés" lightbox="./media/mca-setup-account/setup-billing-account-prepare-enrollment-transition-continue.png" :::

**Conditions d’erreur**

Si vous avez le rôle Administrateur d’entreprise (lecture seule), l’erreur suivante empêchera la transition. Vous devez disposer du rôle Administrateur d’entreprise pour pouvoir effectuer la transition de votre inscription.

`Select another enrollment. You do not hve Enterprise Administrator write permission to the enrollment.`

S’il reste plus de 60 jours jusqu’à la date de fin de votre inscription, l’erreur suivante empêchera la transition. À la date actuelle, il doit rester moins de 60 jours avant la fin de l’inscription pour que vous puissiez effectuer la transition de votre inscription.

`Select another enrollment. This enrollment has more than 60 days before its end date.`

Si votre inscription dispose toujours de crédits, l’erreur suivante empêchera la transition. Vous devez utiliser tous vos crédits avant de pouvoir effectuer la transition de votre inscription.

`Select another enrollment. This enrollment still has credits and can't be transitioned to a billing account.`

Si vous ne disposez pas d’une autorisation propriétaire sur le profil de facturation, l’erreur suivante empêchera la transition. Vous devez avoir le rôle de propriétaire du profil de facturation pour pouvoir effectuer la transition de votre inscription.

`Select another Billing Profile. You do not have owner permission to this profile.`

Si le nouveau plan n’est pas activé pour votre nouveau profil de facturation, l’erreur suivante s’affichera. Vous devez activer le plan avant de pouvoir effectuer la transition de votre inscription.

`Select another Billing Profile. The current selection does not have Azure Plan and Azure dev test plan enabled on it.`

## <a name="understand-changes-to-your-billing-hierarchy"></a>Explorer les modifications apportées à votre hiérarchie de facturation

Votre nouveau compte de facturation simplifie la facturation pour votre organisation tout en vous proposant des fonctionnalités de facturation et de gestion des coûts améliorées. Le schéma suivant explique comment la facturation est organisée dans le nouveau compte de facturation.

![Image de la hiérarchie-post-transition-mca-ea](./media/mca-setup-account/mca-post-transition-hierarchy.png)

1. Le compte de facturation vous permet de gérer la facturation de votre contrat client Microsoft. Les administrateurs d’entreprise deviennent propriétaires du compte de facturation. Pour en savoir plus sur le compte de facturation, consultez [Comprendre le compte de facturation](../understand/mca-overview.md#your-billing-account).
2. Le profil de facturation vous permet de gérer la facturation de votre organisation, de la même façon que votre inscription à un Contrat Entreprise. Les administrateurs d’entreprise deviennent propriétaires du profil de facturation. Pour en savoir plus sur les profils de facturation, consultez [Comprendre les profils de facturation](../understand/mca-overview.md#billing-profiles).
3. Une section de facture permet d’organiser vos coûts en fonction de vos besoins, de la même façon que les départements dans votre inscription à un Contrat Entreprise. Les départements deviennent les sections de facture et les administrateurs de départements deviennent les propriétaires des sections de facture respectives. Pour en savoir plus sur les sections de facture, consultez [Comprendre les sections de facture](../understand/mca-overview.md#invoice-sections).
4. Les comptes qui ont été créés dans votre Contrat Entreprise ne sont pas pris en charge dans le nouveau compte de facturation. Les abonnements du compte appartiennent à la section de facture respective du département. Les propriétaires de comptes peuvent créer et gérer des abonnements pour leurs sections de facture.

## <a name="changes-to-billing-administrator-access"></a>Modifications apportées à l’accès administrateur à la facturation

Selon leur accès, les administrateurs de facturation sur votre inscription à un Contrat Entreprise ont accès aux étendues de facturation dans le nouveau compte. Le tableau suivant explique les modifications en termes d’accès lors de la configuration :

| Rôle existant | Rôle post-transition |
| --- | --- |
| **Administrateur d’entreprise (lecture seule = non)** | **- Propriétaire du compte de facturation** </br> Gérer tout le contenu du compte de facturation </br> **- Propriétaire du profil de facturation** </br> Gérer tout le contenu du profil de facturation </br> **- Propriétaire de la section de facture sur toutes les sections de facture** </br> Gérer tout le contenu des sections de facture |
| **Administrateur d’entreprise (lecture seule = oui)** | **- Lecteur du compte de facturation** </br> Affichage en lecture seule de tout ce qui se trouve sur le compte de facturation</br> **- Lecteur du profil de facturation** </br> Affichage en lecture seule de tout ce qui se trouve sur le profil de facturation</br>**- Lecteur de la section de facture sur toutes les sections de facture**</br> Affichage en lecture seule de tout le contenu des sections de facture|
| **Administrateur de département (lecture seule = non)** |**- Propriétaire de la section de facture sur la section de facture créée pour son département respectif** </br>Gérer tout le contenu de la section de facture|
| **Administrateur de département (lecture seule = oui)**|**- Lecteur de la section de facture sur la section de facture créée pour son département respectif**</br> Affichage en lecture seule de tout le contenu de la section de facture|
| **Propriétaire du compte** | **- Créateur de l’abonnement Azure sur la section de facture créée pour son département respectif** </br>  Créer les abonnements Azure associés à sa section de facture|

Un locataire Azure Active Directory (AD) est sélectionné pour le nouveau compte de facturation lors de l’acceptation de votre Contrat Client Microsoft. Si vous n’avez pas de locataire pour votre organisation, un locataire est créé. Le locataire représente votre organisation dans Azure Active Directory. Les administrateurs de locataires d’entreprise de votre organisation utilisent le locataire pour gérer l’accès aux applications et données de votre organisation.

Votre nouveau compte prend uniquement en charge les utilisateurs du locataire qui a été sélectionné lors de la signature du contrat client Microsoft. Si des utilisateurs avec des autorisations administratives sur votre Contrat Entreprise font partie du locataire, ils peuvent accéder au nouveau compte de facturation pendant la configuration. S’ils ne font pas partie du locataire, ils ne peuvent pas accéder au nouveau compte de facturation, sauf si vous les invitez.

Quand vous invitez les utilisateurs, ils sont ajoutés au locataire en tant qu’utilisateurs invités et obtiennent l’accès au compte de facturation. Pour inviter les utilisateurs, l’accès invité doit être activé pour le locataire. Pour plus d’informations, consultez [Contrôler l’accès invité dans Azure Active Directory](/microsoftteams/teams-dependencies#control-guest-access-in-azure-active-directory). Si l’accès invité est désactivé, contactez les administrateurs d’entreprise de votre locataire pour l’activer. <!-- Todo - How can they find their global administrator -->

## <a name="view-replaced-features"></a>Afficher les fonctionnalités remplacées

Les fonctionnalités suivantes du Contrat Entreprise sont remplacées par de nouvelles fonctionnalités dans le compte de facturation d’un contrat client Microsoft.

### <a name="enterprise-agreement-accounts"></a>Comptes Contrat Entreprise

Les comptes qui ont été créés dans votre inscription à un Contrat Entreprise ne sont pas pris en charge dans le nouveau compte de facturation. Les abonnements du compte appartiennent à la section de facture créée pour son département respectif. Les propriétaires de comptes deviennent des créateurs d’abonnement Azure et peuvent créer et gérer les abonnements pour leurs sections de facturation.

### <a name="notification-contacts"></a>Contacts de notification

Les contacts de notification reçoivent des communications par e-mail à propos du Contrat Entreprise Azure. Ils ne sont pas pris en charge dans le nouveau compte de facturation. Des e-mails au sujet des factures et des crédits Azure sont envoyés aux utilisateurs qui ont accès aux profils de facturation dans votre compte de facturation.

### <a name="spending-quotas"></a>Quotas de dépenses

Les quotas de dépenses qui ont été définis pour les départements dans votre inscription à un Contrat Entreprise sont remplacés par des budgets dans le nouveau compte de facturation. Un budget est créé pour chaque quota de dépenses défini pour les départements dans votre inscription. Pour plus d’informations sur les budgets, consultez [Tutoriel : Créer et gérer des budgets Azure](../costs/tutorial-acm-create-budgets.md).

### <a name="cost-centers"></a>Centres de coûts

Les centres de coûts qui ont été définis pour les abonnements Azure dans votre inscription à un Contrat Entreprise sont reportés dans le nouveau compte de facturation. Toutefois, les centres de coût pour les départements et les comptes Contrat Entreprise ne sont pas pris en charge.

## <a name="additional-information"></a>Informations supplémentaires

Les sections suivantes fournissent des informations supplémentaires sur la configuration de votre compte de facturation.

### <a name="no-service-downtime"></a>Aucune interruption de service.

Les services Azure compris dans votre abonnement continuent de s’exécuter sans aucune interruption. Nous transférons uniquement la relation de facturation pour vos abonnements Azure. Les ressources, groupes de ressources ou groupes d’administration existants ne sont pas affectés.

### <a name="user-access-to-azure-resources"></a>Accès utilisateur aux ressources Azure

L’accès aux ressources Azure qui a été défini à l’aide du contrôle d’accès en fonction du rôle Azure (Azure RBAC) n’est pas affecté lors de la transition.

### <a name="azure-reservations"></a>Réservations Azure

Toute réservation Azure dans votre inscription à un Contrat Entreprise est transférée vers votre nouveau compte de facturation. Pendant la transition, les remises de réservation appliquées à vos abonnements ne subiront aucune modification.

### <a name="azure-marketplace-products"></a>Produits de la Place de marché Azure

Les produits de la Place de marché Azure dans votre inscription à un Contrat Entreprise sont transférés avec les abonnements. Aucune modification ne sera apportée à l’accès au service des produits de la Place de marché Azure pendant la transition.

### <a name="support-plan"></a>Plan de support

Les avantages relatifs au support ne sont pas transférés dans le cadre de la transition. Achetez un nouveau plan de support technique afin de bénéficier d’avantages pour les abonnements Azure dans votre nouveau compte de facturation.

### <a name="past-charges-and-balance"></a>Soldes et anciens frais

Les frais et le solde des crédits avant la transition peuvent être affichés dans votre inscription à un Contrat Entreprise via le portail Azure. <!--Todo - Add a link for this-->

### <a name="when-should-the-setup-be-completed"></a>Quand doit être terminée la configuration ?

Terminez la configuration de votre compte de facturation avant l’expiration de votre inscription à un Contrat Entreprise. Si votre inscription expire, les services dans vos abonnements Azure continueront de s’exécuter sans interruption. Toutefois, les services vous seront facturés aux tarifs du paiement à l’utilisation.

### <a name="changes-to-the-enterprise-agreement-enrollment-after-the-setup"></a>Modifications apportées à l’inscription à un Contrat Entreprise après la configuration

Les abonnements Azure créés pour l’inscription à un Contrat Entreprise après la transition peuvent être déplacés manuellement vers le nouveau compte de facturation. Pour plus d’informations, consultez [Get billing ownership of Azure subscriptions from other users](mca-request-billing-ownership.md) (Obtenir la propriété de facturation des abonnements Azure des autres utilisateurs). Pour déplacer des réservations Azure achetées après la transition, [contactez le support Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Vous pouvez également fournir aux utilisateurs l’accès au compte de facturation après la transition. Pour plus d’informations, consultez [manage billing roles in the Azure portal](understand-mca-roles.md#manage-billing-roles-in-the-azure-portal) (gérer les rôles de facturation dans le portail Azure).

### <a name="revert-the-transition"></a>Annuler la transition

La transition ne peut pas être annulée. Une fois la facturation de vos abonnements Azure transférée vers le nouveau compte de facturation, vous ne pouvez pas la faire revenir dans votre inscription à un Contrat Entreprise.

### <a name="closing-your-browser-during-setup"></a>Fermeture de votre navigateur pendant la configuration

Avant de sélectionner **Démarrer la transition**, vous pouvez fermer le navigateur. Vous pouvez revenir à la configuration en utilisant le lien que vous avez obtenu dans l’e-mail et démarrer la transition. Si vous fermez le navigateur, après le démarrage de la transition, votre transition continue à s’exécuter. Revenez à la page d’état de la transition pour la surveiller. Un e-mail vous sera envoyé une fois la transition terminée.

## <a name="complete-the-setup-in-the-azure-portal"></a>Terminer la configuration dans le portail Azure

Pour terminer la configuration, vous devez accéder à la fois au nouveau compte de facturation et à l’inscription à un Contrat Entreprise. Pour plus d’informations, consultez [Access required to complete the setup](#access-required-to-complete-the-setup) (Accès requis pour terminer la configuration).

1. Connectez-vous au portail Azure à l’aide du lien se trouvant dans l’e-mail qui vous a été envoyé lorsque vous avez signé le contrat client Microsoft.

2. Si vous ne trouvez pas cet e-mail, connectez-vous à l’aide du lien suivant.

   `https://portal.azure.com/#blade/Microsoft_Azure_SubscriptionManagement/TransitionEnrollment`

3. Sélectionnez **Démarrer la transition** dans la dernière étape de la configuration. Une fois que vous avez cliqué sur Démarrer la transition :

    ![Capture d’écran montrant l’Assistant Configuration](./media/mca-setup-account/ea-mca-set-up-wizard.png)

    - Une hiérarchie de facturation correspondant à votre hiérarchie de Contrat Entreprise est créée dans le nouveau compte de facturation. Pour plus d’informations, consultez [explorer les modifications apportées à votre hiérarchie de facturation](#understand-changes-to-your-billing-hierarchy).
    - Les administrateurs de votre inscription à un Contrat Entreprise obtiennent un accès au nouveau compte de facturation afin qu’ils continuent à gérer la facturation pour votre organisation.
    - La facturation de vos abonnements Azure est transférée vers le nouveau compte. **Vos services Azure ne seront pas affectés pendant cette transition. Ils continueront de s’exécuter sans interruption**.
    - Si vous avez des réservations Azure, elles sont transférées vers votre nouveau compte de facturation avec les mêmes avantages et conditions.

4. Vous pouvez surveiller l’état de la transition sur la page **État de la transition**.

   ![Capture d’écran illustrant l’état de la transition](./media/mca-setup-account/ea-mca-set-up-status.png)

## <a name="validate-billing-account-setup"></a>Valider la configuration du compte de facturation

 Validez les points suivants pour vous assurer que votre nouveau compte de facturation est correctement configuré :

### <a name="azure-subscriptions"></a>Abonnements Azure

1. Connectez-vous au [portail Azure](https://portal.azure.com).

2. Recherchez **Gestion des coûts + facturation**.

   ![Capture d’écran montrant la recherche dans le portail Azure](./media/mca-setup-account/search-cmb.png)

3. Sélectionnez le compte de facturation. Le compte de facturation sera du type **Contrat Client Microsoft**.

4. Sélectionnez **Abonnements Azure** sur le côté gauche.

   ![Capture d’écran qui montre la liste de tous les abonnements](./media/mca-setup-account/mca-subscriptions-post-transition.png)

Les abonnements Azure transférés de votre inscription à un Contrat Entreprise vers le nouveau compte de facturation sont affichés sur la page des abonnements Azure. Si vous pensez qu’il manque un abonnement, effectuez manuellement la transition de la facturation de l’abonnement dans le portail Azure. Pour plus d’informations, consultez [Get billing ownership of Azure subscriptions from other users](mca-request-billing-ownership.md) (Obtenir la propriété de facturation des abonnements Azure des autres utilisateurs)

### <a name="azure-reservations"></a>Réservations Azure

Les réservations Azure associées à votre inscription à un Contrat Entreprise seront transférées vers votre nouveau compte de facturation avec les mêmes avantages et conditions. Les transactions terminées avant le transfert n’apparaîtront pas dans votre nouveau compte de facturation. Toutefois, vous pouvez vérifier que les avantages de vos réservations sont bien appliqués à vos abonnements en visitant la [page Réservations Azure](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade).

### <a name="access-of-enterprise-administrators-on-the-billing-account"></a>Accès des administrateurs d’entreprise sur le compte de facturation

1. Connectez-vous au [portail Azure](https://portal.azure.com).

2. Recherchez **Gestion des coûts + facturation**.

   ![Capture d’écran montrant la recherche dans le portail Azure](./media/mca-setup-account/search-cmb.png)

3. Sélectionnez le compte de facturation de votre **Contrat Client Microsoft**.

4. Sélectionnez **Contrôle d’accès (IAM)** sur le côté gauche.

   ![Capture d’écran qui illustre l’accès des administrateurs d’entreprise répertoriés comme propriétaires de comptes de facturation post-transition.](./media/mca-setup-account/mca-ea-admins-ba-access-post-transition.png)

Les administrateurs d’entreprise sont listés en tant que propriétaires du compte de facturation, tandis que les administrateurs d’entreprise avec des autorisations en lecture seule sont listés en tant que lecteurs du compte de facturation. Si vous pensez qu’il manque un accès pour un administrateur d’entreprise, vous pouvez octroyer cet accès dans le portail Azure. Pour plus d’informations, consultez [Manage billing roles in the Azure portal](understand-mca-roles.md#manage-billing-roles-in-the-azure-portal) (Gérer les rôles de facturation dans le portail Azure).

### <a name="access-of-enterprise-administrators-on-the-billing-profile"></a>Accès des administrateurs d’entreprise sur le profil de facturation

1. Connectez-vous au [portail Azure](https://portal.azure.com).

2. Recherchez **Gestion des coûts + facturation**.

   ![Capture d’écran montrant la recherche dans le portail Azure](./media/mca-setup-account/search-cmb.png)

3. Sélectionnez le profil de facturation créé pour votre inscription. Selon votre accès, vous devrez peut-être sélectionner un compte de facturation. Dans le compte de facturation, sélectionnez Profils de facturation, puis le profil de facturation.

4. Sélectionnez **Contrôle d’accès (IAM)** sur le côté gauche.

   ![Capture d’écran qui illustre l’accès des administrateurs d’entreprise répertoriés comme propriétaires de profils de facturation post-transition.](./media/mca-setup-account/mca-ea-admins-bp-access-post-transition.png)

Les administrateurs d’entreprise sont répertoriés en tant que propriétaires du profil de facturation, tandis que les administrateurs d’entreprise avec des autorisations en lecture seule sont répertoriés en tant que lecteurs du profil de facturation. Si vous pensez qu’il manque un accès pour un administrateur d’entreprise, vous pouvez octroyer cet accès dans le portail Azure. Pour plus d’informations, consultez [Manage billing roles in the Azure portal](understand-mca-roles.md#manage-billing-roles-in-the-azure-portal) (Gérer les rôles de facturation dans le portail Azure).

### <a name="access-of-enterprise-administrators-department-administrators-and-account-owners-on-invoice-sections"></a>Accès des administrateurs d’entreprise, des administrateurs de département et des propriétaires de comptes sur des sections de facture

1. Connectez-vous au [portail Azure](https://portal.azure.com).

2. Recherchez **Gestion des coûts + facturation**.

   ![Capture d’écran montrant la recherche dans le portail Azure](./media/mca-setup-account/search-cmb.png).

3. Sélectionnez une section de facture. Les sections de facture ont le même nom que leur département respectif dans les inscriptions à un Contrat Entreprise. Selon votre accès, vous devrez peut-être sélectionner un compte de facturation. Dans le compte de facturation, sélectionnez **Profils de facturation**, puis sélectionnez **Sections de facture**. Dans la liste des sections de facture, sélectionnez une section de facture.

   ![Capture d’écran montrant la liste des sections de facture post transition](./media/mca-setup-account/mca-invoice-sections-post-transition.png)

4. Sélectionnez **Contrôle d’accès (IAM)** sur le côté gauche.

    ![Capture d’écran qui illustre l’accès des administrateurs de département et de compte post-transition](./media/mca-setup-account/mca-department-account-admins-access-post-transition.png)

Les administrateurs d’entreprise et de département sont répertoriés comme propriétaires de la section de facture ou lecteurs de la section de facture, tandis que les propriétaires de comptes dans le département sont répertoriés en tant que créateurs d’abonnements Azure. Répétez l’étape pour toutes les sections de facture, afin de vérifier l’accès à tous les départements de votre inscription à un Contrat Entreprise. Les propriétaires de compte qui ne faisaient pas partie d’un département obtiendront une autorisation sur une section de facture nommée **Default invoice section** (Section de facture par défaut). Si vous pensez qu’il manque un accès pour un administrateur, vous pouvez octroyer cet accès dans le portail Azure. Pour plus d’informations, consultez [Manage billing roles in the Azure portal](understand-mca-roles.md#manage-billing-roles-in-the-azure-portal) (Gérer les rôles de facturation dans le portail Azure).

## <a name="need-help-contact-support"></a>Vous avez besoin d’aide ? Contacter le support technique

Si vous avez toujours besoin d’aide, [contactez le support technique](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) pour obtenir une prise en charge rapide de votre problème.

## <a name="next-steps"></a>Étapes suivantes

- [Get started with your billing account for a Microsoft Customer Agreement](../understand/mca-overview.md) (Bien démarrer avec votre compte de facturation pour un contrat client Microsoft)
- [Complete Enterprise Agreement tasks in your billing account for a Microsoft Customer Agreement](mca-enterprise-operations.md) (Effectuer des tâches Contrat Entreprise dans votre compte de facturation pour un contrat client Microsoft)
- [Understand Microsoft Customer Agreement administrative roles in Azure](understand-mca-roles.md) (Comprendre les rôles administratifs de contrat client Microsoft dans Azure)