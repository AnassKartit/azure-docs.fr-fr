---
title: Résoudre les problèmes liés aux codes d’erreur Azure Stream Analytics
description: Résolvez les problèmes Azure Stream Analytics liés aux codes d'erreur interne.
ms.author: sidram
author: sidramadoss
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: ddea74997850ff080f6c82d55d1be3cfb851deda
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98019378"
---
# <a name="azure-stream-analytics-internal-error-codes"></a>Codes d’erreur interne Azure Stream Analytics

Vous pouvez utiliser les journaux d’activité et les journaux de ressources pour déboguer les comportements inattendus à partir de votre tâche Azure Stream Analytics. Cet article répertorie la description de chaque code d’erreur interne. Les erreurs internes sont des erreurs génériques déclenchées au sein de la plateforme Stream Analytics lorsque Stream Analytics ne peut déterminer s'il s'agit d'une erreur de disponibilité interne ou d'un bogue dans le système.

## <a name="cosmosdboutputbatchsizetoolarge"></a>CosmosDBOutputBatchSizeTooLarge

* **Cause** : La taille de lot utilisée pour écrire dans Cosmos DB est trop grande. 
* **Recommandation** : Réessayez en utilisant une taille de lot plus petite.

## <a name="next-steps"></a>Étapes suivantes

* [Troubleshoot input connections](stream-analytics-troubleshoot-input.md) (Résoudre les problèmes liés aux connexions d’entrée)
* [Résoudre les problèmes liés aux sorties Azure Stream Analytics](stream-analytics-troubleshoot-output.md)
* [Résoudre les problèmes liés aux requêtes Azure Stream Analytics](stream-analytics-troubleshoot-query.md)
* [Résoudre les problèmes liés à Azure Stream Analytics à l’aide des journaux de ressources](stream-analytics-job-diagnostic-logs.md)
* [Erreurs de données Azure Stream Analytics](data-errors.md)