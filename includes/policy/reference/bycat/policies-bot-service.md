---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/03/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 877315ef266dbd877aa8223c985ee8f77a070112
ms.sourcegitcommit: e8b229b3ef22068c5e7cd294785532e144b7a45a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2021
ms.locfileid: "123476872"
---
|Nom<br /><sub>(Portail Azure)</sub> |Description |Effet(s) |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Le point de terminaison du service bot doit être un URI HTTPS valide](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6164527b-e1ee-4882-8673-572f425f5e0a) |Les données peuvent être falsifiées pendant la transmission. Certains protocoles chiffrent les données pour résoudre les problèmes d’utilisation abusive et de falsification. Pour veiller à ce que vos bots communiquent uniquement sur des canaux chiffrés, définissez le point de terminaison sur un URI HTTPS valide. Ainsi, le protocole HTTPS est utilisé pour chiffrer vos données en transit, ce qui constitue souvent une exigence à des fins de conformité aux normes réglementaires ou du secteur. Consultez : [https://docs.microsoft.com/azure/bot-service/bot-builder-security-guidelines](/azure/bot-service/bot-builder-security-guidelines). |audit, deny, disabled |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Bot%20Service/BotService_ValidEndpoint_Audit.json) |
|[Bot Service doit être chiffré avec une clé gérée par le client](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F51522a96-0869-4791-82f3-981000c2c67f) |Azure Bot Service chiffre automatiquement votre ressource pour protéger vos données et satisfaire aux engagements de sécurité et de conformité de l’organisation. Par défaut, les clés de chiffrement gérées par Microsoft sont utilisées. Pour une plus grande flexibilité dans la gestion des clés ou le contrôle de l’accès à votre abonnement, sélectionnez des clés gérées par le client, option également appelée Bring Your Own Key (BYOK). En savoir plus sur le chiffrement Azure Bot Service : [https://docs.microsoft.com/azure/bot-service/bot-service-encryption](/azure/bot-service/bot-service-encryption). |audit, deny, disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Bot%20Service/BotService_CMKEnabled_Audit.json) |
|[Le mode isolé doit être activé pour le service bot](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F52152f42-0dda-40d9-976e-abb1acdd611e) |Les robots doivent être définis en mode « isolé uniquement ». Ce paramètre configure les canaux Bot Service qui nécessitent la désactivation du trafic sur l’Internet public. |audit, deny, disabled |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Bot%20Service/BotService_NetworkIsolatedEnabled_Audit.json) |
|[Bot Service doit avoir les méthodes d’authentification locales désactivées](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fffea632e-4e3a-4424-bf78-10e179bb2e1a) |La désactivation des méthodes d’authentification locales améliore la sécurité en garantissant qu’un bot utilise exclusivement AAD pour l’authentification. |Audit, Refuser, Désactivé |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Bot%20Service/BotService_DisableLocalAuth_Audit.json) |