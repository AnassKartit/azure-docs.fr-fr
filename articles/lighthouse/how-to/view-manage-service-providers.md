---
title: Voir et gérer les fournisseurs de services
description: Dans le portail Azure, es clients peuvent afficher des informations sur les fournisseurs de services Azure Lighthouse, les offres de fournisseurs de services et les ressources déléguées.
ms.date: 07/16/2021
ms.topic: how-to
ms.openlocfilehash: fef9d23209e0551f105263c019297df8208e940a
ms.sourcegitcommit: e2fa73b682a30048907e2acb5c890495ad397bd3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2021
ms.locfileid: "114389166"
---
# <a name="view-and-manage-service-providers"></a>Voir et gérer les fournisseurs de services

La page **Fournisseurs de services** dans le [portail Azure](https://portal.azure.com) offre aux clients le contrôle et la visibilité de leurs fournisseurs de services qui utilisent [Azure Lighthouse](../overview.md). Les clients peuvent déléguer des ressources spécifiques, passer en revue les offres nouvelles ou mises à jour, supprimer l’accès d’un fournisseur de services, et bien plus encore.

Pour accéder à cette page sur le portail Azure, sélectionnez **Tous les services**, puis rechercher et sélectionner **Fournisseurs de services**. Vous pouvez également trouver cette page en tapant « Fournisseurs de services » ou « Azure Lighthouse » dans la zone de recherche dans la partie supérieure du portail Azure.

> [!NOTE]
> Pour afficher la page **Fournisseurs de services**, un utilisateur du locataire du client doit disposer du [rôle intégré Lecteur](../../role-based-access-control/built-in-roles.md#reader) (ou d’un autre rôle intégré qui comprend l’accès en lecture).
>
> Pour ajouter ou mettre à jour des offres, déléguer des ressources et supprimer des offres, l’utilisateur doit disposer d’un rôle avec l’autorisation `Microsoft.Authorization/roleAssignments/write`, comme le rôle [Propriété](../../role-based-access-control/built-in-roles.md#owner).

N’oubliez pas que la page **Fournisseurs de services** affiche des informations uniquement sur les fournisseurs de services qui ont accès aux abonnements ou aux groupes de ressources du client via Azure Lighthouse. Elle n’affiche pas d’informations sur les fournisseurs de services supplémentaires qui n’utilisent pas Azure Lighthouse.

## <a name="view-service-provider-details"></a>Afficher les détails du fournisseur de services

Pour afficher les détails des fournisseurs de services actuels qui utilisent Azure Lighthouse pour travailler sur le locataire du client, sélectionnez **Offres de fournisseur de services** sur le côté gauche de la page **Fournisseurs de services**.

Pour chaque offre, vous verrez le nom du fournisseur de services, ainsi que l’offre associée. Vous pouvez sélectionner une offre pour afficher une description et d’autres détails, y compris les attributions de rôles accordées par le fournisseur de services.

Dans la colonne **Délégations**, vous pouvez voir combien d’abonnements ou groupes de ressources ont été délégués au fournisseur de services pour cette offre. Le fournisseur de services est en mesure d’accéder à ces abonnements ou groupes de ressources, ainsi que de les gérer, en fonction des niveaux d’accès spécifiés dans l’offre.

## <a name="add-service-provider-offers"></a>Ajouter des offres de fournisseur de services

Pour ajouter une nouvelle offre de fournisseur de services à partir de la page **Offres de fournisseur de services**, sélectionnez **Ajouter une offre**. Sélectionnez **offres privées** pour afficher les offres que le fournisseur de services a publiées pour ce client. Vous pouvez ensuite sélectionner une offre, puis sélectionner **Configurer + s’abonner**.

## <a name="update-service-provider-offers"></a>Mettre à jour les offres de fournisseur de services

Une fois qu’un client a ajouté une offre, un fournisseur de services peut publier une version mise à jour de cette même offre sur la place de marché Microsoft Azure, par exemple pour ajouter une nouvelle définition de rôle. Si une nouvelle version de l’offre a été publiée, la page **Offres de fournisseur de services** comprend une icône de « mise à jour » dans la ligne de l’offre en question. Sélectionnez cette icône pour voir les différences entre la version actuelle de l'offre et la nouvelle.

 ![Icône Mettre à jour une offre](../media/update-offer.jpg)

Après examen des les modifications, le client peut décider de mettre à jour vers la nouvelle version. Les autorisations et autres paramètres spécifiés dans la nouvelle version s’appliquent alors à tous les abonnements et/ou groupes de ressources qui ont été délégués pour cette offre.

## <a name="remove-service-provider-offers"></a>Supprimer des offres de fournisseur de services

Vous pouvez supprimer une offre de fournisseur de services à tout moment, en sélectionnant l’icône Corbeille sur la ligne de cette offre.

Une fois que vous avez confirmé la suppression, ce fournisseur de services n’a plus accès aux ressources qui étaient déléguées pour cette offre.

## <a name="delegate-resources"></a>Déléguer des ressources

Pour qu’un fournisseur de services puisse accéder aux ressources d’un client et les gérer, un ou plusieurs abonnements et/ou groupes de ressources doivent être délégués. Si un client a ajouté une offre mais n’a pas encore délégué de ressources, une note apparaît en haut de la section des **Offres de fournisseur de services**. Le fournisseur de services ne peut pas travailler sur les ressources du locataire du client tant que la délégation n’est pas effectuée.

Pour déléguer des abonnements ou des groupes de ressources :

1. Cochez la case correspondant à la ligne contenant le fournisseur de services, l’offre et le nom. Sélectionnez ensuite **Déléguer des ressources** en haut de l’écran.
1. Dans la section **Détails de l’offre** de la page **Déléguer des ressources**, examinez les détails relatifs au fournisseur de services et à l’offre. Pour réviser les attributions de rôles pour l’offre, sélectionnez **Cliquer ici pour voir les détails de l’offre sélectionnée**.
1. Dans la section **Déléguer**, sélectionnez **Déléguer des abonnements** ou **Déléguer des groupes de ressources**.
1. Choisissez les abonnements ou groupes de ressources que aimeriez déléguer pour cette offre, puis sélectionnez **Ajouter**.
1. Activez la case à cocher en bas de la page pour confirmer que vous souhaitez accorder à ce fournisseur de services l’accès aux ressources que vous avez sélectionnées, puis sélectionnez **Déléguer**.

## <a name="view-delegations"></a>Afficher les délégations

Les délégations représentent une association de ressources client spécifiques (abonnements et/ou groupes de ressources) à des attributions de rôles qui accordent des autorisations au fournisseur de services pour ces ressources. Pour voir les détails d’une délégation, sélectionnez **Délégations** sur le côté gauche de la page **Fournisseurs de services**.

Les filtres du haut de la page vous permettent de trier et regrouper vos informations de délégation. Vous pouvez également filtrer sur des clients, offres ou mots clés spécifiques.

> [!NOTE]
> Lors de l’[affichage des informations d’attribution de rôle pour l’étendue déléguée dans le portail Azure](../../role-based-access-control/role-assignments-list-portal.md#list-role-assignments-at-a-scope) ou par le biais des API, les clients ne voient pas ces attributions de rôles, ni les utilisateurs du locataire du fournisseur de services auxquels ces rôles ont été attribués.

## <a name="audit-and-restrict-delegations-in-your-environment"></a>Auditer et restreindre des délégations dans votre environnement

Les clients peuvent souhaiter passer en revue tous les abonnements et/ou groupes de ressources délégués à Azure Lighthouse. Cela s’avère particulièrement utile pour les clients disposant d’un grand nombre d’abonnements ou en présence de nombreux utilisateurs effectuant des tâches de gestion.

Nous fournissons une [définition de stratégie intégrée Azure Policy](../../governance/policy/samples/built-in-policies.md#lighthouse) pour [auditer la délégation d’étendues sur un locataire gestionnaire](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Lighthouse/Lighthouse_Delegations_Audit.json). Vous pouvez attribuer cette stratégie à un groupe d’administration comprenant tous les abonnements que vous souhaitez auditer. Lorsque vous utilisez cette stratégie pour vérifier la conformité, tous les abonnements et/ou groupes de ressources délégués (dans le groupe d’administration auquel la stratégie est attribuée) apparaissent dans un état non conforme. Vous pouvez ensuite passer en revue les résultats afin de vous assurer de l'absence de délégations inattendues.

Une autre [définition de stratégie intégrée](../../governance/policy/samples/built-in-policies.md#lighthouse) vous permet de [limiter les délégations à des locataires gestionnaires spécifiques](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Lighthouse/AllowCertainManagingTenantIds_Deny.json). Cette stratégie peut être attribuée à un groupe d’administration qui comprend tous les abonnements pour lesquels vous souhaitez limiter les délégations. Une fois la stratégie déployée, toute tentative de déléguer un abonnement à un locataire en dehors de ceux que vous spécifiez est refusée.

Pour plus d’informations sur l’attribution d’une stratégie et l’affichage des résultats de l’état de conformité, consultez [Démarrage rapide : Créer une attribution de stratégie](../../governance/policy/assign-policy-portal.md).

## <a name="next-steps"></a>Étapes suivantes

- Apprenez-en davantage sur [Azure Lighthouse](../overview.md).
- Découvrez comment [auditer l’activité d’un fournisseur de services](view-service-provider-activity.md).
- Découvrez comment les fournisseurs de services peuvent [voir et gérer les clients](view-manage-customers.md) dans la page **Mes clients** sur le portail Azure.
- Découvrez comment les [entreprises qui gèrent plusieurs locataires](../concepts/enterprise.md) peuvent utiliser Azure Lighthouse pour consolider leur expérience de gestion.
