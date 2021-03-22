---
title: Tableau de bord principal de Azure Security Center ou page « de présentation »
description: En savoir plus sur les fonctionnalités de la page de présentation de Security Center
author: memildin
ms.author: memildin
ms.date: 03/04/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 768d69b626a870910d6734e1a1ddc29871f96afb
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102099775"
---
# <a name="azure-security-centers-overview-page"></a>Page de présentation de Security Center

Lorsque vous ouvrez Azure Security Center, la première page qui s’affiche est la page de présentation. 

Ce tableau de bord interactif fournit une vue unifiée de la posture de sécurité de vos charges de travail cloud hybrides. En outre, elle affiche des alertes de sécurité, des informations de couverture, etc.

Vous pouvez sélectionner n’importe quel élément de la page pour obtenir des informations plus détaillées.

:::image type="content" source="media/overview-page/overview.png" alt-text="Page Vue d’ensemble de Security Center":::

## <a name="features-of-the-overview-page"></a>Fonctionnalités de la page de présentation

:::image type="content" source="media/overview-page/top-bar-of-overview.png" alt-text="Barre supérieure de la page de présentation de Security Center":::

La **barre de menus supérieure** offre :
- **Abonnements** : vous pouvez afficher et filtrer la liste des abonnements en sélectionnant ce bouton. Security Center ajuste l’affichage pour refléter la position de sécurité des abonnements sélectionnés.
- **Nouveautés** : ouvre les [notes de publication](release-notes.md) afin que vous puissiez être informé des nouvelles fonctionnalités, des correctifs de bogues et des fonctionnalités déconseillées.
- **Les numéros de haut niveau** pour les comptes cloud connectés, afin d’afficher le contexte des informations dans les mosaïques principales ci-dessous. De même que le nombre de ressources évaluées, de recommandations actives et d’alertes de sécurité. Sélectionnez le nombre des ressources évaluées pour accéder à [Inventaire des ressources](asset-inventory.md). En savoir plus sur la connexion de vos [comptes AWS](quickstart-onboard-aws.md) et vos [projets GCP](quickstart-onboard-gcp.md).


Au centre de la page se trouvent **quatre mosaïques centrales**, chacune d’entre elles étant en lien vers un tableau de bord dédié pour plus d’informations :
- **Score de sécurité** : Security Center évalue continuellement vos ressources, vos abonnements et votre organisation en recherchant d’éventuels problèmes de sécurité. Il agrège ensuite toutes ses découvertes sous la forme d’un score qui vous permet de déterminer d’un coup d’œil votre niveau de sécurité actuel : plus le score est élevé, plus le niveau de risque identifié est faible. [Plus d’informations](secure-score-security-controls.md)
- **Conformité réglementaire** : Security Center fournit des insights relatifs à votre posture de conformité sur la base d’évaluations continues de votre environnement Azure. Security Center analyse les facteurs de risque dans votre environnement de cloud hybride conformément aux bonnes pratiques en matière de sécurité. Ces évaluations sont comparées à des contrôles de conformité provenant d’un ensemble de normes prises en charge. [Plus d’informations](security-center-compliance-dashboard.md)
- **Azure Defender** : il s’agit de la plateforme de protection de charge de travail cloud (CWPP) intégrée à Security Center pour une protection intelligente et avancée de vos charges de travail Azure et hybrides. La mosaïque affiche la couverture de vos ressources connectées (pour les abonnements actuellement sélectionnés) et les alertes récentes, avec un code de couleur par gravité. [Plus d’informations](azure-defender.md)
- **Firewall Manager** : cette vignette indique l’état de vos hubs et réseaux à partir d’[Azure Firewall Manager](../firewall-manager/overview.md). 


Le volet **Insights** offre des éléments personnalisés pour votre environnement, notamment :
- Vos ressources les plus attaquées
- Vos contrôles de sécurité qui ont le potentiel le plus élevé pour augmenter votre score de sécurisé
- Les suggestions actives avec le plus grand nombre de ressources impactées
- Récent billet de blog d’experts Azure Security Center

## <a name="next-steps"></a>Étapes suivantes

Cette page introduit la page de présentation de Security Center. Pour plus d’informations, consultez :

- [Explorez et gérez vos ressources à l’aide des outils d’inventaire et de gestion des ressources](asset-inventory.md)
- [Degré de sécurisation dans Azure Security Center](secure-score-security-controls.md)