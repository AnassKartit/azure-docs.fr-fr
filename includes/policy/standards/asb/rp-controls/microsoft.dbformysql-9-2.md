---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/17/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: dc56a3c5348556ad949b46b8be19f489a6515d84
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128909739"
---
|Nom<br /><sub>(Portail Azure)</sub> |Description |Effet(s) |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La sauvegarde géoredondante doit être activée pour Azure Database pour MySQL](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F82339799-d096-41ae-8538-b108becf0970) |Azure Database pour MySQL vous permet de choisir l’option de redondance pour votre serveur de base de données. Vous pouvez choisir un stockage de sauvegarde géoredondant. Dans ce cas, les données sont non seulement stockées dans la région qui héberge votre serveur, mais elles sont aussi répliquées dans une région jumelée pour offrir une possibilité de récupération en cas de défaillance régionale. La configuration du stockage géoredondant pour la sauvegarde est uniquement possible lors de la création du serveur. |Audit, Désactivé |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForMySQL_Audit.json) |
