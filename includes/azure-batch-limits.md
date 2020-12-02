---
title: Fichier include
description: Fichier include
services: batch
author: JnHs
ms.service: batch
ms.topic: include
ms.date: 06/03/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 54a789aba793d9a6b44d5fe9a1e0f42023002b5a
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027627"
---
| **Ressource** | **Limite par défaut** | **Limite maximale** |
| --- | --- | --- |
| Comptes Azure Batch par région et par abonnement | 1-3 |50 |
| Cœurs dédiés par compte Batch | 90-900 | Contacter le support technique |
| Cœurs à priorité basse par compte Batch | 10-100 | Contacter le support technique |
| Travaux et planifications de travaux **[actifs](/rest/api/batchservice/job/get#jobstate)** par compte Batch (les travaux **terminés** n’ont aucune limite) | 100-300 | 1 000<sup>1</sup> |
| Pools par compte Batch | 20-100 | 500<sup>1</sup> |

<sup>1</sup>Si vous voulez solliciter un dépassement de cette limite, contactez le support Azure.

> [!IMPORTANT]
> Nous avons changé le mode de demande et de gestion de quota dédié.  Le total des processeurs virtuels dédiés est la valeur actuellement appliquée, mais nous allons bientôt appliquer un quota dédié par série de machines virtuelles. Le quota de faible priorité continuera à être appliqué en fonction de la limite totale ; il ne sera pas appliqué par série de machines virtuelles.

> [!NOTE]
> Les limites par défaut varient selon le type d’abonnement que vous utilisez pour créer un compte Batch. Les quotas de cœurs affichés correspondent aux comptes Batch en mode service Batch. [Affichez les quotas dans votre compte Batch](../articles/batch/batch-quota-limit.md#view-batch-quotas).

> [!IMPORTANT]
> Pour mieux gérer la capacité au cours de la pandémie mondiale, nous avons réduit les quotas de cœurs par défaut des nouveaux comptes Batch par rapport à la plage de valeurs ci-dessus dans certaines régions et pour certains types d’abonnements (quotas réduits à zéro cœur dans certains cas). Quand vous créez un compte Batch, [vérifiez votre quota de cœurs](../articles/batch/batch-quota-limit.md#view-batch-quotas) et [demandez une augmentation du quota de cœurs](../articles/batch/batch-quota-limit.md#increase-a-quota) si nécessaire. Vous pouvez aussi envisager de réutiliser des comptes batch qui disposent déjà d’un quota suffisant.