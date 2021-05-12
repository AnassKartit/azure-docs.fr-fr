---
title: Vue d’ensemble de l’exemple de blueprint de base du framework d’adoption du cloud
description: Vue d’ensemble et architecture de l’exemple de blueprint de base du framework d’adoption du cloud pour Azure.
ms.date: 03/12/2021
ms.topic: sample
ms.openlocfilehash: b605c1af5fcb2a37663d71fecceea169692a5ab9
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2021
ms.locfileid: "108757832"
---
# <a name="overview-of-the-microsoft-cloud-adoption-framework-for-azure-foundation-blueprint-sample"></a>Vue d’ensemble de l’exemple de blueprint de base du framework d’adoption de Microsoft Cloud pour Azure.

Le blueprint de base du framework d’adoption de Microsoft Cloud pour Azure déploie un ensemble de ressources d’infrastructure et de contrôles de stratégie de base requis pour votre première application Azure de niveau production. Ce blueprint de base s’appuie sur le modèle recommandé disponible dans le framework d’adoption du cloud.

## <a name="architecture"></a>Architecture

L’exemple de blueprint de base du framework d’adoption du cloud déploie dans Azure des ressources d’infrastructure recommandées que les organisations peuvent utiliser pour mettre en place les contrôles de base nécessaires à la gestion de leur parc cloud. Cet exemple déploie et applique des ressources, stratégies et modèles qui permettent à une organisation de commencer à utiliser Azure en toute confiance.

:::image type="complex" source="../../media/caf-blueprints/caf-foundation-architecture.png" alt-text="Base du C A F, image décrivant ce qui est installé conformément aux instructions du C A F pour créer une base vous permettant de commencer à utiliser Azure." border="false":::
   Décrit une architecture Azure qui est obtenue en déployant le blueprint Base C A F. Il s’applique à un abonnement comprenant des groupes de ressources constitués d’un compte de stockage pour stocker des journaux et de Log Analytics configuré pour stocker dans le compte de stockage. Il décrit également Azure Key Vault configuré avec une configuration standard d’Azure Security Center. Toutes ces infrastructures de base sont accessibles avec Azure Active Directory et appliquées avec Azure Policy.
:::image-end:::

Cette implémentation intègre plusieurs services Azure utilisés pour fournir une base sécurisée, entièrement supervisée et prête pour les entreprises. Cet environnement comporte les éléments suivants :

- Une instance [Azure Key Vault](../../../../key-vault/general/overview.md) utilisée pour héberger les secrets utilisés pour les machines virtuelles déployées dans l’environnement des services partagés
- [Log Analytics](../../../../azure-monitor/overview.md) est déployé pour s’assurer que l’ensemble des actions et services journalisent dans un emplacement central dès l’instant où vous démarrez votre déploiement sécurisé dans [Comptes de stockage](../../../../storage/common/storage-introduction.md) pour la journalisation des diagnostics.
- [Azure Security Center](../../../../security-center/security-center-introduction.md) (version standard) assure la protection contre les menaces de vos charges de travail migrées.
- Le blueprint définit et déploie également des définitions [Azure Policy](../../../policy/overview.md) :
  - Définitions de stratégie :
    - Catégorisation (CostCenter) appliquée aux groupes de ressources
    - Ajouter des ressources dans un groupe de ressources avec la balise CostCenter
    - Autoriser une région Azure pour les ressources et groupes de ressources
    - Autoriser des références SKU de compte de stockage (choisir lors du déploiement)
    - Autoriser des références SKU de machines virtuelles Azure (choisir lors du déploiement)
    - Exiger le déploiement de Network Watcher
    - Exiger un chiffrement de transfert sécurisé de compte de stockage Azure
    - Refuser des types de ressources (choisir lors du déploiement)
  - Initiatives de stratégie :
    - Activer la supervision dans Azure Security Center (plus de 100 définitions de stratégie)

Tous ces éléments se conforment aux pratiques éprouvées publiés dans [Centre des architectures Azure - Architectures de référence](/azure/architecture/reference-architectures/).

> [!NOTE]
> La base du framework d’adoption du cloud constitue une architecture fondamentale pour les charges de travail.
> Vous devez quand même déployer des charges de travail derrière cette architecture de base.

Pour plus d’informations, voir [Framework d’adoption de Microsoft Cloud pour Azure – Prêt](/azure/cloud-adoption-framework/ready/).

## <a name="next-steps"></a>Étapes suivantes

Vous avez découvert la vue d’ensemble et l’architecture de l’exemple de blueprint de base du framework d’adoption du cloud.

> [!div class="nextstepaction"]
> [Blueprint de base du framework d’adoption du cloud – Étapes de déploiement](./deploy.md)

Autres articles sur les blueprints et la manière de les utiliser :

- En savoir plus sur le [cycle de vie des blueprints](../../concepts/lifecycle.md)
- Comprendre comment utiliser les [paramètres statiques et dynamiques](../../concepts/parameters.md).
- Apprendre à personnaliser l’[ordre de séquencement des blueprints](../../concepts/sequencing-order.md).
- Découvrir comment utiliser le [verrouillage de ressources de blueprint](../../concepts/resource-locking.md).
- Découvrir comment [mettre à jour des affectations existantes](../../how-to/update-existing-assignments.md).
