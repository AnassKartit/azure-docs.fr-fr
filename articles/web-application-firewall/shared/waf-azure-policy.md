---
title: Pare-feu d’applications web Azure et Azure Policy
description: Le pare-feu d’applications web (WAF) Azure associé à Azure Policy peut aider à appliquer les normes organisationnelles et à évaluer la conformité à l’échelle des ressources WAF
author: tremansdoerfer
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: conceptual
ms.date: 07/07/2020
ms.author: rimansdo
ms.openlocfilehash: 7738976cbc3f7b87862cab73a5b5f52c6d499d47
ms.sourcegitcommit: 0ede6bcb140fe805daa75d4b5bdd2c0ee040ef4d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2021
ms.locfileid: "122606111"
---
# <a name="azure-web-application-firewall-and-azure-policy"></a>Pare-feu d’applications web Azure et Azure Policy

Le pare-feu d’applications web (WAF) Azure associé à Azure Policy peut aider à appliquer les normes organisationnelles et à évaluer la conformité à l’échelle des ressources WAF. Azure Policy est un outil de gouvernance qui fournit une vue agrégée permettant d’évaluer l’état général de l’environnement, avec la possibilité d’explorer minutieusement au niveau de chaque ressource et stratégie. Azure Policy vous aide également à mettre vos ressources en conformité par le biais de la correction en bloc pour les ressources existantes, et de la correction automatique pour les nouvelles ressources.

## <a name="azure-policy-for-web-application-firewall"></a>Azure Policy pour Web Application Firewall

Il existe plusieurs définitions Azure Policy intégrées pour gérer les ressources WAF. La répartition des définitions de stratégie et leurs fonctionnalités sont les suivantes :

1. **Le pare-feu d’applications web (WAF) doit être activé pour le service Azure Front Door** : Les services Azure Front Door sont évalués si un WAF est présent sur la création de ressources. La définition de stratégie a trois effets : Audit, refus et désactivation. L’« Audit » permet de suivre les cas où un service Azure Front Door n’a pas de WAF et permet aux utilisateurs de voir ce que le service Azure Front Door ne respecte pas. Le « Refus » empêche la création d’un service Azure Front Door si un WAF n’est pas attaché. Désactivé désactive l’affectation de stratégies.

2. **Le pare-feu d’applications web (WAF) doit être activé pour Application Gateway** : Les passerelles d’applications sont évaluées si un WAF est présent sur la création de ressources. La définition de stratégie a trois effets : Audit, refus et désactivation. L’« Audit » permet de suivre les cas où une Application Gateway n’a pas de WAF et permet aux utilisateurs de voir ce que la passerelle d’application ne respecte pas. Le « Refus » empêche la création d’Application Gateway si un WAF n’est pas attaché. Désactivé désactive l’affectation de stratégies.

3. **Le pare-feu d’applications web (WAF) doit utiliser le mode spécifié pour le service Azure Front Door** : Impose l’utilisation du mode de « détection » ou de « blocage » pour être actif sur toutes les stratégies de pare-feu d’applications web pour le service Azure Front Door. La définition de stratégie a trois effets : Audit, refus et désactivation. L’« Audit » permet de suivre les cas où un WAF ne correspond pas au mode spécifié. Le « Refus » empêche la création d’un WAF s’il n’est pas dans le bon mode. Désactivé désactive l’affectation de stratégies.

4. **Le pare-feu d’applications web (WAF) doit utiliser le mode spécifié pour Application Gateway** : Impose l’utilisation du mode de « détection » ou de « blocage » pour être actif sur toutes les stratégies de pare-feu d’applications web pour Application Gateway. La définition de stratégie a trois effets : Audit, refus et désactivation. L’« Audit » permet de suivre les cas où un WAF ne correspond pas au mode spécifié. Le « Refus » empêche la création d’un WAF s’il n’est pas dans le bon mode. Désactivé désactive l’affectation de stratégies.

## <a name="launch-an-azure-policy"></a>Lancement d’Azure Policy

1.  Sur la page d’accueil Azure, taper « Stratégie » dans la barre de recherche, puis cliquer sur l’icône Azure Policy

2.  Dans le service Azure Policy, sous **Création**, sélectionnez **Affectations**.

:::image type="content" source="../media/waf-azure-policy/policy-home.png" alt-text="Onglet Affectations dans Azure Policy":::

3.  Sur la page « Affectations », sélectionnez l’icône **Attribuer une stratégie** en haut.

:::image type="content" source="../media/waf-azure-policy/assign-policy.png" alt-text="Onglet Informations de base sur la page Affecter une stratégie":::

4.  Dans l’onglet « Informations de base » de la page « Attribuer une stratégie », mettez à jour les champs suivants :
    1.  **Étendue** : sélectionnez les abonnements Azure et les groupes de ressources qui doivent être affectés par la définition de stratégie.
    2.  **Exclusions** : sélectionnez toutes les ressources de l’étendue à exclure de l’affectation de stratégie.
    3.  **Définition de la stratégie** : sélectionnez la définition de stratégie à appliquer à l’étendue avec des exclusions. Tapez « Web Application Firewall » dans la barre de recherche pour choisir la stratégie Azure Web Application Firewall appropriée.

:::image type="content" source="../media/waf-azure-policy/policy-listing.png" alt-text="Capture d’écran montrant l’onglet « Définitions de stratégie » dans la page « Définitions disponibles ».":::

5.  Sélectionnez l’onglet **Paramètres** et mettez à jour les paramètres d’affectation de stratégie. Pour plus de précisions sur la fonction du paramètre, survolez l’icône d’informations à côté du nom du paramètre.

6.  Sélectionnez **Vérifier + créer** pour finaliser votre affectation de stratégie. Attendez environ 15 minutes pour que l’affectation de stratégie soit active pour les nouvelles ressources.
