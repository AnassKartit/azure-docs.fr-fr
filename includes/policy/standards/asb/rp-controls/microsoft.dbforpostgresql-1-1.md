---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/27/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 0d8cc859c25c9f6456e03be7c3906f027534c757
ms.sourcegitcommit: dcf1defb393104f8afc6b707fc748e0ff4c81830
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2021
ms.locfileid: "123123167"
---
|Nom<br /><sub>(Portail Azure)</sub> |Description |Effet(s) |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Le point de terminaison privé doit être activé pour les serveurs PostgreSQL](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0564d078-92f5-4f97-8398-b9f58a51f70b) |Les connexions des points de terminaison privés permettent de sécuriser les communications en rendant la connectivité à Azure Database pour PostgreSQL privée. Configurez une connexion de point de terminaison privé pour autoriser l’accès uniquement au trafic provenant de réseaux connus et empêcher l’accès à partir de toutes les autres adresses IP, y compris dans Azure. |AuditIfNotExists, Désactivé |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_EnablePrivateEndPoint_Audit.json) |
