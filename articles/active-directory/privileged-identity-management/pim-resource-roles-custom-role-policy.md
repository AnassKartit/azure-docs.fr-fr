---
title: Utiliser des rôles personnalisés Azure dans PIM - Azure AD | Microsoft Docs
description: Découvrez comment utiliser des rôles Azure personnalisés dans Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: KarenH444
ms.service: active-directory
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 10/07/2021
ms.author: curtand
ms.reviewer: shaunliu
ms.collection: M365-identity-device-management
ms.openlocfilehash: a114c233372636ca26c847d819103651ad455e97
ms.sourcegitcommit: bee590555f671df96179665ecf9380c624c3a072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/07/2021
ms.locfileid: "129668786"
---
# <a name="use-azure-custom-roles-in-privileged-identity-management"></a>Utiliser des rôles personnalisés Azure dans Privileged Identity Management

Vous devrez peut-être appliquer des paramètres Privileged Identity Management (PIM) stricts à certains utilisateurs d’un rôle privilégié dans votre organisation Azure Active Directory (Azure AD), tout en offrant une plus grande autonomie aux autres utilisateurs. Par exemple, envisagez un scénario dans lequel votre organisation embauche plusieurs associés sous contrat pour aider au développement d’une application qui s’exécute dans un abonnement Azure.

En tant qu’administrateur de ressources, vous souhaitez que les employés puissent bénéficier d’un accès sans exiger d’approbation. Toutefois, tous les partenaires doivent être approuvés quand ils demandent à accéder aux ressources de l’organisation.

Suivez les étapes décrites dans la section suivante pour définir les paramètres Privileged Identity Management ciblés pour les rôles de ressources Azure.

## <a name="create-the-custom-role"></a>Créer le rôle personnalisé

Pour créer un rôle personnalisé pour une ressource, suivez les étapes décrites dans [Rôles personnalisés Azure](../../role-based-access-control/custom-roles.md).

Quand vous créez un rôle personnalisé, indiquez un nom descriptif afin de vous rappeler facilement du rôle intégré que vous souhaitez dupliquer.

> [!NOTE]
> Vérifiez que le rôle personnalisé est un doublon du rôle intégré à dupliquer, et que son étendue correspond à celle du rôle intégré.

## <a name="apply-pim-settings"></a>Appliquer les paramètres PIM

Une fois le rôle créé dans votre organisation Azure AD, accédez à la page **Privileged Identity Management – Ressources Azure** dans le Portail Azure. Sélectionnez la ressource à laquelle le rôle s’applique.

![Volet « Privileged Identity Management - Ressources Azure »](media/pim-resource-roles-custom-role-policy/aadpim-manage-azure-resource-some-there.png)

[Configurez les paramètres du rôle Privileged Identity Management](pim-resource-roles-configure-role-settings.md) qui doivent s’appliquer à ces membres du rôle.

Pour finir, [attribuez des rôles](pim-resource-roles-assign-roles.md) au groupe de membres distinct que vous souhaitez cibler avec ces paramètres.

## <a name="next-steps"></a>Étapes suivantes

- [Configurer les paramètres des rôles de ressources Azure dans Privileged Identity Management](pim-resource-roles-configure-role-settings.md)
- [Rôles personnalisés dans Azure](../../role-based-access-control/custom-roles.md)