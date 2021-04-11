---
title: Base de référence de sécurité Azure pour Azure Resource Graph
description: La base de référence de sécurité Azure Resource Graph fournit des instructions et des ressources pour la mise en œuvre des recommandations de sécurité spécifiées dans le benchmark de sécurité Azure.
author: msmbaldwin
ms.service: resource-graph
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: ad8968fdb6548da29a031f0e44bd3671f67b5553
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105557715"
---
# <a name="azure-security-baseline-for-azure-resource-graph"></a>Base de référence de sécurité Azure pour Azure Resource Graph

Cette base de référence de sécurité applique les conseils [Azure Security Benchmark version 1.0](../../../security/benchmarks/overview-v1.md) à Azure Resource Graph. Le benchmark de sécurité Azure fournit des recommandations sur la façon dont vous pouvez sécuriser vos solutions cloud sur Azure.
Le contenu est regroupé par les **contrôles de sécurité** définis par le benchmark de sécurité Azure et les conseils associés applicables à Azure Resource Graph. Les **contrôles** non applicables à Azure Resource Graph ont été exclus.

 
Pour voir comment Azure Resource Graph est entièrement mappé à Azure Security Benchmark, consultez le [fichier de mappage complet de la base de référence de sécurité d’Azure Resource Graph](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="identity-and-access-control"></a>Contrôle des accès et des identités

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : contrôle des accès et des identités](../../../security/benchmarks/security-control-identity-access-control.md).*

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10 : Examiner et rapprocher régulièrement l’accès utilisateur

**Conseil** : Azure Resource Graph permet d’accéder aux types et aux propriétés des ressources sur la base du contrôle d’accès en fonction du rôle Azure (Azure RBAC). Auditez et passez en revue régulièrement l’accès accordé aux principaux de sécurité (utilisateurs, groupes et comptes de service) pour vous assurer que les requêtes retournent les résultats des ressources appropriées.

- [Autorisations dans Azure Resource Graph](../overview.md#permissions-in-azure-resource-graph)

- [Comment utiliser les révisions d’accès des identités Azure](../../../active-directory/governance/access-reviews-overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="data-protection"></a>Protection des données

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : protection des données](../../../security/benchmarks/security-control-data-protection.md).*

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6 : Utiliser Azure RBAC pour contrôler l’accès aux ressources 

**Conseil** : utilisez Azure RBAC pour contrôler l’accès aux données et aux ressources. Pour utiliser Azure Resource Graph, vous devez également disposer d’un accès approprié aux ressources que vous souhaitez interroger. Cet accès doit être limité en lecture seule et accordé uniquement au personnel requis.

- [Autorisations dans Azure Resource Graph](../overview.md#permissions-in-azure-resource-graph)

- [Comment configurer Azure RBAC](../../../role-based-access-control/role-assignments-rest.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="inventory-and-asset-management"></a>Gestion des stocks et des ressources

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Gestion des stocks et des ressources](../../../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1 : Utiliser la solution de détection automatisée des ressources

**Aide** : Utilisez Azure Resource Graph pour interroger et découvrir toutes les ressources prises en charge au sein de vos abonnements, groupes d’administration et locataires. Vérifiez que vous disposez des autorisations appropriées dans votre locataire et pouvez répertorier tous les abonnements Azure ainsi que les ressources qu’ils contiennent.

- [Guide pratique pour créer des requêtes avec Azure Resource Graph](../first-query-portal.md)

- [Présentation d’Azure RBAC](../../../role-based-access-control/overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4 : Définir et tenir un inventaire des ressources Azure approuvées

**Conseils** : Créez un inventaire des ressources Azure et logiciels approuvés pour les ressources de calcul en fonction des besoins de votre organisation. Utilisez Azure Resource Graph pour interroger les ressources Azure approuvées et l’historique des modifications (préversion) pour passer en revue les instantanés et voir ce qui a changé.

- [Explorer les ressources Azure avec Azure Resource Graph](../first-query-portal.md)

- [Obtenir les changements des ressources](../how-to/get-resource-changes.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5 : Analyser les ressources Azure non approuvées

**Aide** : Utilisez Azure Resource Graph pour interroger et découvrir les ressources au sein de vos abonnements, groupes d’administration et locataires. Vérifiez que toutes les ressources Azure figurant dans l’environnement sont approuvées.

- [Explorer les ressources Azure avec Azure Resource Graph](../first-query-portal.md)

- [Exemples : Requêtes de démarrage pour Azure Resource Graph](../samples/starter.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="next-steps"></a>Étapes suivantes

- Consultez [Vue d’ensemble d’Azure Security Benchmark V2](../../../security/benchmarks/overview.md)
- En savoir plus sur les [bases de référence de la sécurité Azure](../../../security/benchmarks/security-baselines-overview.md)