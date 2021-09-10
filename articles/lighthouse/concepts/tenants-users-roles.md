---
title: Locataires, utilisateurs et rôles dans les scénarios Azure Lighthouse
description: Découvrez comment les locataires, les utilisateurs et les rôles Azure Active Directory peuvent être utilisés dans les scénarios Azure Lighthouse.
ms.date: 06/23/2021
ms.topic: conceptual
ms.openlocfilehash: cdf8c10d52e0add4513d42a99d2054e1af0ed796
ms.sourcegitcommit: 5fabdc2ee2eb0bd5b588411f922ec58bc0d45962
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/23/2021
ms.locfileid: "112542253"
---
# <a name="tenants-users-and-roles-in-azure-lighthouse-scenarios"></a>Locataires, utilisateurs et rôles dans les scénarios Azure Lighthouse

Avant d’intégrer des clients pour la [Azure Lighthouse](../overview.md), il est important de comprendre comment les locataires, les utilisateurs et les rôles fonctionnent dans Azure Active Directory (Azure AD), ainsi que la façon dont ils peuvent être utilisés dans les scénarios Azure Lighthouse.

Un *locataire* est une instance dédiée et approuvée d’Azure AD. En général, chaque locataire représente une seule organisation. Azure Lighthouse permet d’opérer une [projection logique](architecture.md#logical-projection) des ressources d’un locataire sur un autre. Cela permet aux utilisateurs du locataire de gestion (appartenant par exemple à un fournisseur de services) d’accéder à des ressources déléguées dans le locataire d’un client, ou permet aux [entreprises avec plusieurs locataires de centraliser leurs opérations de gestion](enterprise.md).

Pour atteindre cette projection logique, un abonnement (ou un ou plusieurs groupes de ressources au sein d’un abonnement) dans le locataire client doit être *intégré* à Azure Lighthouse. Ce processus d’intégration peut être effectué [par le biais de modèles Azure Resource Manager](../how-to/onboard-customer.md) ou par [la publication d’une offre publique ou privée sur la Place de marché Azure](../how-to/publish-managed-services-offers.md).

Quelle que soit la méthode d’intégration choisie, vous devez définir des *autorisations*. Chaque autorisation spécifie un **principalId** qui aura accès aux ressources déléguées et un rôle intégré qui définit les autorisations dont chacun de ces utilisateurs aura besoin pour ces ressources. Ce **principalId** définit un utilisateur, un groupe ou un principal de service Azure AD dans le locataire gérant.

> [!NOTE]
> Sauf spécification explicite, les références à un « utilisateur » dans la documentation Azure Lighthouse peuvent s’appliquer à un utilisateur, à un groupe ou à un principal de service Azure AD dans une autorisation.

## <a name="best-practices-for-defining-users-and-roles"></a>Meilleures pratiques pour la définition des utilisateurs et des rôles

Lorsque vous créez vos autorisations, nous vous recommandons de suivre ces meilleures pratiques :

- Dans la plupart des cas, vous affectez des autorisations à un groupe d’utilisateurs ou à un principal de service Azure AD, plutôt qu’à une série de comptes d’utilisateur individuels. Cela vous permet d’ajouter ou de supprimer l’accès d’utilisateurs individuels sans devoir mettre à jour et republier le plan lorsque vos conditions d’accès changent.
- Veillez à suivre le principe du privilège minimum afin que les utilisateurs disposent uniquement des autorisations nécessaires pour accomplir leur travail, ce qui contribue à réduire le risque d’erreurs accidentelles. Pour plus d’informations, consultez les [Pratiques de sécurité recommandées](../concepts/recommended-security-practices.md).
- Incluez un utilisateur avec le [rôle Supprimer l’attribution de l’inscription des services gérés](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) afin de pouvoir [supprimer l’accès à la délégation](../how-to/remove-delegation.md) ultérieurement si nécessaire. Si ce rôle n’est pas attribué, les ressources déléguées ne peuvent être supprimées que par un utilisateur dans le locataire du client.
- Assurez-vous que tous les utilisateurs qui ont besoin [d’afficher la page Mes clients dans le portail Azure](../how-to/view-manage-customers.md) possèdent le rôle de [Lecteur](../../role-based-access-control/built-in-roles.md#reader) (ou un autre rôle intégré qui inclut l’accès Lecteur).

> [!IMPORTANT]
> Pour que vous puissiez ajouter des autorisations pour un groupe Azure AD, le **type de groupe** doit être défini sur **Sécurité**. Cette option est sélectionnée lors de la création du groupe. Pour plus d’informations, consultez [Créer un groupe de base et ajouter des membres avec Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

## <a name="role-support-for-azure-lighthouse"></a>Prise en charge des rôles pour Azure Lighthouse

Lors de la définition d’une autorisation, chaque compte utilisateur doit recevoir un des [rôles intégrés Azure](../../role-based-access-control/built-in-roles.md). Les rôles personnalisés et les [Rôles Administrateur classique de l’abonnement](../../role-based-access-control/classic-administrators.md) ne sont pas pris en charge.

Tous les [rôles intégrés](../../role-based-access-control/built-in-roles.md) sont actuellement pris en charge par Azure Lighthouse, avec les exceptions suivantes :

- Le rôle [propriétaire](../../role-based-access-control/built-in-roles.md#owner) n’est pas pris en charge.
- Les rôles intégrés disposant d’une autorisation [DataActions](../../role-based-access-control/role-definitions.md#dataactions) ne sont pas pris en charge.
- Le rôle intégré [Administrateur de l’accès utilisateur](../../role-based-access-control/built-in-roles.md#user-access-administrator) est pris en charge, mais uniquement pour les besoins limités [d’affectation de rôles à une identité gérée dans le locataire client](../how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant). Aucune autre autorisation généralement accordée par ce rôle ne s’applique. Si vous définissez un utilisateur avec ce rôle, vous devez également spécifier le ou les rôles intégrés que cet utilisateur peut affecter aux identités gérées.

> [!NOTE]
> Une fois qu'un nouveau rôle intégré applicable a été ajouté à Azure, il peut être attribué lors de l'[intégration d'un client à l'aide des modèles Azure Resource Manager](../how-to/onboard-customer.md). Un certain temps peut s’écouler avant que le nouveau rôle ne soit disponible dans l’Espace partenaires lors de la [publication d’une offre de services gérés](../how-to/publish-managed-services-offers.md).

## <a name="transferring-delegated-subscriptions-between-azure-ad-tenants"></a>Transfert d’abonnements délégués entre locataires Azure AD

Si un abonnement est [transféré à un autre compte de locataire Azure AD](../../cost-management-billing/manage/billing-subscription-transfer.md#transfer-a-subscription-to-another-azure-ad-tenant-account), la [définition de l’inscription et les ressources d’attribution de l’inscription](architecture.md#delegation-resources-created-in-the-customer-tenant) créées par le [processus d’intégration d’Azure Lighthouse](../how-to/onboard-customer.md) seront conservées. Cela signifie que l’accès accordé par Azure Lighthouse aux locataires gestionnaires restera en vigueur pour cet abonnement (ou pour les groupes de ressources délégués au sein de cet abonnement).

La seule exception est le transfert de l’abonnement à un locataire Azure AD auquel il a été délégué auparavant. Dans ce cas, les ressources de délégation pour ce locataire sont supprimées et l’accès accordé par l’intermédiaire d’Azure Lighthouse ne s’applique plus, puisque l’abonnement appartient désormais directement à ce locataire (au lieu d’être délégué par le biais d’Azure Lighthouse). Toutefois, si cet abonnement a également été délégué à d’autres locataires gestionnaires, ces derniers conserveront le même accès à l’abonnement.

## <a name="next-steps"></a>Étapes suivantes

- Consultez les [pratiques de sécurité recommandées pour Azure Lighthouse](recommended-security-practices.md).
- Intégrez vos clients à Azure Lighthouse [en utilisant des modèles Azure Resource Manager](../how-to/onboard-customer.md) ou [en publiant une offre de services managés privés ou publics sur la Place de marché Azure](../how-to/publish-managed-services-offers.md).
