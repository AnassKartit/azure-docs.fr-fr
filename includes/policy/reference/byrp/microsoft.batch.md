---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 11/17/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 1e2d66f19d38316e2cf71398bc1e8566bd7922dc
ms.sourcegitcommit: c2dd51aeaec24cd18f2e4e77d268de5bcc89e4a7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94741998"
---
|Nom<br /><sub>(Portail Azure)</sub> |Description |Effet(s) |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Déployer les paramètres de diagnostic de compte Batch sur Event Hub](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fdb51110f-0865-4a6e-b274-e2e07a5b2cd7) |Déploie les paramètres de diagnostic de compte Batch à envoyer en streaming à un hub d’événements régional quand un compte Batch nouveau ou mis à jour n’a pas ces paramètres de diagnostic. |DeployIfNotExists, Désactivé |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Batch_DeployDiagnosticLog_Deploy_EventHub.json) |
|[Déployer les paramètres de diagnostic de compte batch sur l’espace de travail Log Analytics](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc84e5349-db6d-4769-805e-e14037dab9b5) |Déploie les paramètres de diagnostic de compte Batch à envoyer en streaming à un espace de travail Log Analytics régional quand un compte Batch nouveau ou mis à jour n’a pas ces paramètres de diagnostic. |DeployIfNotExists, Désactivé |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Batch_DeployDiagnosticLog_Deploy_LogAnalytics.json) |
|[Les journaux de diagnostic dans les comptes Batch doivent être activés](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F428256e6-1fac-4f48-a757-df34c2b3336d) |Auditer l’activation des journaux de diagnostic Permet de recréer les pistes d’activité à utiliser à des fins d’investigation en cas d’incident de sécurité ou de compromission du réseau. |AuditIfNotExists, Désactivé |[3.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Batch/Batch_AuditDiagnosticLog_Audit.json) |
|[Des règles d’alerte de métrique doivent être configurées sur les comptes Batch](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F26ee67a2-f81a-4ba8-b9ce-8550bd5ee1a7) |Auditez la configuration des règles d’alerte de métrique sur le compte Batch pour activer la métrique requise. |AuditIfNotExists, Désactivé |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Batch/Batch_AuditMetricAlerts_Audit.json) |
