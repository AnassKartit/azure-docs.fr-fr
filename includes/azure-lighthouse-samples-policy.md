---
title: Fichier include
description: Fichier include
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 08/13/2021
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: b55711662daa7d3e476cd8a3cd3c438c7893c424
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2021
ms.locfileid: "122178160"
---
Ces exemples montrent comment utiliser Azure Policy avec des abonnements qui ont été intégrés à Azure Lighthouse.

| **Modèle** | **Description** |
|---------|---------|
| [policy-add-or-replace-tag](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-add-or-replace-tag) | Attribue une stratégie qui ajoute ou supprime une étiquette (avec l’effet de modification) dans un abonnement délégué. Pour plus d’informations, consultez [Déployer une stratégie pouvant être corrigée dans un abonnement délégué](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-allow-certain-managing-tenants](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-allow-certain-managing-tenants) | Attribue une stratégie qui limite les délégations Azure Lighthouse à des locataires de gestion spécifiques. |
| [policy-audit-delegation](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-audit-delegation) | Attribue une stratégie qui effectue un audit pour les attributions de délégation. |
| [policy-delegate-management-groups](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-delegate-management-groups) | Affecte une stratégie pour confirmer que les [abonnements au sein d’un groupe d’administration ont été délégués à un locataire gérant](../articles/lighthouse/how-to/onboard-management-group.md). Si ce n’est pas le cas, crée l’affectation.
| [policy-enforce-keyvault-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-keyvault-monitoring) | Attribue une stratégie qui active les diagnostics sur les ressources Azure Key Vault dans un abonnement délégué (avec l’effet deployIfNotExists). Pour plus d’informations, consultez [Déployer une stratégie pouvant être corrigée dans un abonnement délégué](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-enforce-sub-monitoring](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-enforce-sub-monitoring) | Attribue plusieurs stratégies pour activer les diagnostics sur un abonnement délégué, et connecte toutes les machines virtuelles Windows et Linux à l’espace de travail Log Analytics créé par la stratégie. Pour plus d’informations, consultez [Déployer une stratégie pouvant être corrigée dans un abonnement délégué](../articles/lighthouse/how-to/deploy-policy-remediation.md). |
| [policy-initiative](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/policy-initiative) | Applique une initiative (plusieurs définitions de stratégie associées) à un abonnement délégué. |

