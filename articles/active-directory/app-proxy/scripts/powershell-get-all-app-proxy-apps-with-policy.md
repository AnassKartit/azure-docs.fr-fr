---
title: Exemple PowerShell - Lister toutes les applications Proxy d’application Azure Active Directory avec une stratégie
description: Exemple PowerShell qui répertorie toutes les applications de proxy d’application Azure Active Directory (Azure AD) dans votre répertoire qui ont une stratégie de jeton de durée de vie.
services: active-directory
author: kenwith
manager: karenh444
ms.service: active-directory
ms.subservice: app-proxy
ms.workload: identity
ms.topic: sample
ms.date: 04/29/2021
ms.author: kenwith
ms.reviewer: ashishj
ms.openlocfilehash: 79a0cabfc8874ec2ad8c4bfefe9e7ef733bbdf07
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2021
ms.locfileid: "129988085"
---
# <a name="get-all-application-proxy-apps-with-a-token-lifetime-policy"></a>Obtenir toutes les applications de proxy d’application avec une stratégie de durée de vie de jeton

Cet exemple de script PowerShell répertorie toutes les applications de proxy d’application Azure Active Directory (Azure AD) dans votre répertoire qui ont une stratégie de jeton de durée de vie de jeton et répertorie des détails sur la stratégie.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Cet exemple nécessite la [version en préversion du module AzureAD v2 PowerShell pour Graph](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview&preserve-view=true) (AzureADPreview).

## <a name="sample-script"></a>Exemple de script

[!code-azurepowershell[main](~/powershell_scripts/application-proxy/get-all-appproxy-apps-with-policy.ps1 "Get all Application Proxy apps with a token lifetime policy")]

## <a name="script-explanation"></a>Explication du script

| Commande | Notes |
|---|---|
|[Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) | Permet d’obtenir un principal de service. |
|[Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication) | Permet d’obtenir une application Azure AD. |
|[Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true) | Obtient une stratégie dans Azure AD. |
|[Get-AzureADServicePrincipalPolicy](/powershell/module/azuread/get-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true) | Obtient la stratégie d’un principal de service dans Azure AD. |


## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le Module Azure AD PowerShell, consultez [Présentation du Module Azure AD PowerShell](/powershell/azure/active-directory/overview).

Pour d’autres exemples PowerShell pour le proxy d’application, consultez [Exemples Azure AD PowerShell pour le Proxy d’application Azure Active Directory](../application-proxy-powershell-samples.md).
