---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 08/27/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 649578b574f6d00964e9489fcbd25e1848ab3323
ms.sourcegitcommit: dcf1defb393104f8afc6b707fc748e0ff4c81830
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2021
ms.locfileid: "123097920"
---
|Nom<br /><sub>(Portail Azure)</sub> |Description |Effet(s) |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[Le service Gestion des API doit utiliser une référence SKU qui prend en charge les réseaux virtuels](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F73ef9241-5d81-4cd4-b483-8443d1730fe5) |Avec les références SKU prises en charge pour la gestion des API, le déploiement de service dans un réseau virtuel déverrouille les fonctionnalités de sécurité et de mise en réseau avancées de la gestion des API, ce qui vous permet de mieux contrôler la configuration de la sécurité de votre réseau. Pour en savoir plus : [https://aka.ms/apimvnet](../../../../articles/api-management/api-management-using-with-vnet.md). |Audit, Refuser, Désactivé |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/API%20Management/ApiManagement_AllowedVNETSkus_AuditDeny.json) |
|[Les services Gestion des API doivent utiliser un réseau virtuel](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fef619a2c-cc4d-4d03-b2ba-8c94a834d85b) |Le déploiement d’un réseau virtuel Azure offre une sécurité renforcée et une isolation, et vous permet de placer votre service Gestion des API dans un réseau routable non-Internet auquel vous contrôlez l’accès. Ces réseaux peuvent ensuite être connectés à vos réseaux locaux à l’aide de différentes technologies VPN, ce qui permet d’accéder à vos services back-end au sein du réseau et/ou localement. Le portail des développeurs et la passerelle API peuvent être configurés pour être accessibles depuis Internet ou uniquement au sein du réseau virtuel. |Audit, Désactivé |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/API%20Management/ApiManagement_VNETEnabled_Audit.json) |