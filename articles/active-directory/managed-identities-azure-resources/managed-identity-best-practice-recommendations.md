---
title: Recommandations relatives aux meilleures pratiques pour les identités système managées
description: Recommandations relatives à l’utilisation d’identités managées affectées par l’utilisateur par rapport à celles affectées par le système
services: active-directory
documentationcenter: ''
author: barclayn
manager: karenh444
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 10/15/2021
ms.author: barclayn
ms.openlocfilehash: e25ebb85071b6d0af2696083afda45a453c5841a
ms.sourcegitcommit: 147910fb817d93e0e53a36bb8d476207a2dd9e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2021
ms.locfileid: "130131016"
---
# <a name="managed-identity-best-practice-recommendations"></a>Recommandations relatives aux meilleures pratiques liées aux identités managées

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="choosing-system-or-user-assigned-managed-identities"></a>Choix d’identités managées affectées par le système ou par l’utilisateur

Les identités managées affectées par l’utilisateur sont plus efficaces dans un plus grand nombre de scénarios que les identités managées affectées par le système. Consultez le tableau ci-dessous pour découvrir quelques scénarios et les recommandations relatives aux identités affectées par l’utilisateur ou par le système.

Les identités affectées par l’utilisateur peuvent être utilisées par plusieurs ressources, et leurs cycles de vie sont dissociés des cycles de vie des ressources auxquelles elles sont associées. [Découvrez quelles ressources prennent en charge les identités managées](./services-support-managed-identities.md).

Ce cycle de vie vous permet de séparer vos responsabilités en matière de création de ressources et d’administration des identités. Les identités affectées par l’utilisateur et leurs attributions de rôle peuvent être configurées avant les ressources qui en ont besoin. Les utilisateurs qui créent les ressources ont uniquement besoin de l’accès pour attribuer une identité affectée par l’utilisateur, sans avoir à créer de nouvelles identités ou attributions de rôle.  

Comme les identités affectées par le système sont créées et supprimées en même temps que la ressource, les attributions de rôle ne peuvent pas être créées à l’avance. Cette séquence peut provoquer des échecs lors du déploiement d’une infrastructure si l’utilisateur qui crée la ressource n’a pas également accès à la création d’attributions de rôle.

Si votre infrastructure nécessite que plusieurs ressources aient accès aux mêmes ressources, une seule identité affectée par l’utilisateur peut leur être attribuée. La surcharge relative à l’administration sera moins importante, car il y a moins d’identités et d’attributions de rôle distinctes à gérer.

Si vous avez besoin que chaque ressource ait sa propre identité ou si vous avez des ressources qui nécessitent un ensemble unique d’autorisations et que vous souhaitez que l’identité soit supprimée en même temps que la ressource, vous devez utiliser une identité affectée par le système.


| Scénario| Recommandation|Notes|
|---|---|---|
| Création rapide de ressources (par exemple, calcul éphémère) avec des identités managées |  Identité attribuée par l’utilisateur | Si vous tentez de créer plusieurs identités managées dans un court laps de temps (par exemple, en déployant plusieurs machines virtuelles avec leur propre identité affectée par le système), vous risquez de dépasser la limite de débit pour les créations d’objets Azure Active Directory, et la demande échouera avec une erreur HTTP 429. <br/><br/>Si des ressources sont créées ou supprimées rapidement, vous risquez également de dépasser la limite du nombre de ressources dans Azure Active Directory si vous utilisez des identités affectées par le système. Bien qu’une identité affectée par le système qui a été supprimée ne soit plus accessible par aucune ressource, elle sera comptabilisée dans votre limite jusqu’à ce qu’elle soit supprimée définitivement après 30 jours.<br/><br/>Le déploiement de ressources associées à une seule identité affectée par l’utilisateur nécessite la création d’un seul principal de service dans Azure Active Directory, ce qui permet d’éviter la limite du débit. L’utilisation d’une seule identité créée à l’avance réduira également le risque de retards de réplication qui pourraient se produire si plusieurs ressources sont créées, chacune avec sa propre identité.<br/><br/>En savoir plus sur les [limites du service d’abonnement Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md#managed-identity-limits). |
| Ressources/applications répliquées | Identité attribuée par l’utilisateur  |  Les ressources qui effectuent la même tâche, par exemple, des serveurs web dupliqués ou des fonctionnalités identiques qui s’exécutent dans un service d’application et dans une application sur une machine virtuelle, requièrent généralement les mêmes autorisations. <br/><br/>En utilisant la même identité affectée par l’utilisateur, moins d’attributions de rôle sont nécessaires, ce qui réduit la charge de gestion. Les ressources n’ont pas besoin d’être du même type. 
|Conformité| Identité attribuée par l’utilisateur  | Si votre organisation exige que toute création d’identité passe par un processus d’approbation, l’utilisation d’une seule identité affectée par l’utilisateur sur plusieurs ressources nécessitera moins d’approbations que les identités affectées par le système, qui sont créées à mesure que de nouvelles ressources sont créées. |
Accès requis avant le déploiement d’une ressource |Identité attribuée par l’utilisateur| Des ressources peuvent nécessiter un accès à certaines ressources Azure dans le cadre de leur déploiement.<br/><br/>Dans ce cas, il se peut qu’une identité affectée par le système ne soit pas créée à temps ; il convient donc d’utiliser une identité affectée par l’utilisateur préexistante.|
Journalisation d’audit|Identité attribuée par le système|Si vous devez enregistrer quelle ressource spécifique a effectué une action, plutôt que quelle identité, utilisez une identité affectée par le système.|
Gestion du cycle de vie des autorisations|Identité attribuée par le système|Si vous souhaitez que les autorisations d’une ressource soient supprimées en même temps que cette ressource, utilisez une identité affectée par le système.

### <a name="using-user-assigned-identities-to-reduce-administration"></a>Utilisation des identités affectées par l’utilisateur pour réduire l’administration

Les diagrammes illustrent la différence entre les identités affectées par le système et celles affectées par l’utilisateur lorsqu’elles sont utilisées pour permettre à plusieurs machines virtuelles d’accéder à deux comptes de stockage. 

Le diagramme montre quatre machines virtuelles avec des identités affectées par le système. Chaque machine virtuelle a les mêmes attributions de rôle qui lui permettent d’accéder à deux comptes de stockage.

:::image type="content" source="media/managed-identity-best-practice-recommendations/system-assigned-identities.png" alt-text="Quatre machines virtuelles utilisant des identités affectées par le système pour accéder à un compte de stockage et à un coffre de clés.":::

Lorsqu’une identité affectée par l’utilisateur est associée aux quatre machines virtuelles, seules deux attributions de rôle sont requises, contre huit avec des identités affectées par le système. Si l’identité des machines virtuelles requiert davantage d’attributions de rôle, elles seront accordées à toutes les ressources associées à cette identité.

:::image type="content" source="media/managed-identity-best-practice-recommendations/user-assigned-identities.png" alt-text="Quatre machines virtuelles utilisant une identité affectée par l’utilisateur pour accéder à un compte de stockage et à un coffre de clés.":::

Des groupes de sécurité peuvent également être utilisés pour réduire le nombre d’attributions de rôle requises. Ce diagramme illustre quatre machines virtuelles avec des identités affectées par le système, qui ont été ajoutées à un groupe de sécurité, avec les attributions de rôle ajoutées au groupe au lieu des identités affectées par le système. Bien que le résultat soit similaire, cette configuration n’offre pas les mêmes capacités de modèle Resource Manager que les identités affectées par l’utilisateur.

:::image type="content" source="media/managed-identity-best-practice-recommendations/system-assigned-identities-in-a-group.png" alt-text="Quatre machines virtuelles avec leurs identités affectées par le système ajoutées à un groupe de sécurité qui a des attributions de rôle.":::

### <a name="multiple-managed-identities"></a>Plusieurs identités managées

Les ressources qui prennent en charge les identités managées peuvent avoir une identité affectée par le système et une ou plusieurs identités affectées par l’utilisateur. 

Ce modèle offre la flexibilité nécessaire pour utiliser une identité affectée par l’utilisateur partagée et appliquer des autorisations granulaires si nécessaire.

Dans l’exemple ci-dessous, « Machine virtuelle 3 » et « Machine virtuelle 4 » peuvent accéder à la fois aux comptes de stockage et aux coffres de clés, selon l’identité affectée par l’utilisateur qu’elles utilisent lors de l’authentification.

:::image type="content" source="media/managed-identity-best-practice-recommendations/multiple-user-assigned-identities.png" alt-text="Quatre machines virtuelles, deux avec plusieurs identités affectées par l’utilisateur.":::

Dans l’exemple ci-dessous, « Machine virtuelle 4 » possède à la fois une identité affectée par l’utilisateur et une autre affectée par le système, ce qui lui donne accès aux comptes de stockage et aux coffres de clés, selon l’identité qu’elle utilise lors de l’authentification. Les attributions de rôle pour l’identité affectée par le système sont spécifiques à cette machine virtuelle.

:::image type="content" source="media/managed-identity-best-practice-recommendations/system-and-user-assigned-identities.png" alt-text="Quatre machines virtuelles, dont une avec des identités affectées par le système et par l’utilisateur.":::

## <a name="limits"></a>limites 

Consultez les limites des [identités managées](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-rbac-limits) et des [rôles personnalisés et attributions de rôle](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-rbac-limits).

## <a name="follow-the-principle-of-least-privilege-when-granting-access"></a>Suivre le principe du moindre privilège lors de l’octroi de l’accès

Lorsque vous accordez une identité, y compris une identité managée, aux services, accordez toujours les autorisations minimales nécessaires pour effectuer les actions souhaitées. Par exemple, si une identité managée est utilisée pour lire des données à partir d’un compte de stockage, il n’est pas nécessaire d’autoriser les autorisations d’accès à écrire des données dans le compte de stockage. En accordant des autorisations supplémentaires, par exemple, en faisant de l’identité managée un contributeur sur un abonnement Azure lorsqu’il n’est pas nécessaire, augmente le rayon de sécurité associé à l’identité. L’un doit toujours réduire le rayon de sécurité pour compromettre l’identité.

### <a name="consider-the-effect-of-assigning-managed-identities-to-azure-resources"></a>Considérez l’effet de l’attribution d’identités managées aux ressources Azure

Il est important de noter que lorsqu’une ressource Azure, telle qu’une application logique Azure, une fonction Azure ou une machine virtuelle, etc. reçoit une identité managée, toutes les autorisations accordées à l’identité managée sont désormais disponibles pour la ressource Azure. Ceci est particulièrement important, car si un utilisateur a accès à l’installation ou à l’exécution de code sur cette ressource, il a accès à toutes les identités affectées/associées à la ressource Azure. L’objectif d’une identité managée est de permettre au code s’exécutant sur un accès aux ressources Azure d’accéder à d’autres ressources, sans que les développeurs aient besoin de gérer ou de placer directement les informations d’identification dans le code pour obtenir cet accès.

Par exemple, si une identité managée (ClientId = 1234) a reçu un accès en lecture/écriture à ***StorageAccount7755** _ et qu’elle a été attribuée à _*_LogicApp3388_*_, Alice, qui n’a pas d’autorisations directes sur l’identité managée ou le compte de stockage, mais qui a l’autorisation d’exécuter du code dans _*_LogicApp3388_*_ peut également lire/écrire des données vers/à partir de _ *_StorageAccount7755_** en exécutant le code utilisant l’identité managée.

:::image type="content" source="media/managed-identity-best-practice-recommendations/security-considerations.png" alt-text="scénario de sécurité":::

En général, lorsqu'on accorde à un utilisateur un accès administratif à une ressource qui peut exécuter du code (comme une Logic App) et qui a une identité gérée, il faut se demander si le rôle attribué à l'utilisateur peut installer ou exécuter du code sur la ressource, et dans l’affirmative, n'attribuer ce rôle que si l'utilisateur en a vraiment besoin.


## <a name="maintenance"></a>Maintenance

Les identités affectées par le système sont automatiquement supprimées lorsque la ressource est supprimée, tandis que le cycle de vie d’une identité affectée par l’utilisateur est indépendant des ressources auxquelles elle est associée.

Vous devez supprimer manuellement une identité affectée par l’utilisateur lorsqu’elle n’est plus nécessaire, même si aucune ressource ne lui est associée.

Les attributions de rôle ne sont pas supprimées automatiquement lorsque les identités managées affectées par le système ou par l’utilisateur sont supprimées. Ces attributions de rôle doivent être supprimées manuellement afin de ne pas dépasser la limite d’attributions de rôle par abonnement. 

Les attributions de rôle associées aux identités managées supprimées apparaissent avec le message « Identité introuvable » lorsqu’elles sont affichées dans le portail. [En savoir plus](../../role-based-access-control/troubleshooting.md#role-assignments-with-identity-not-found).

:::image type="content" source="media/managed-identity-best-practice-recommendations/identity-not-found.png" alt-text="Identité introuvable pour l’attribution de rôle.":::