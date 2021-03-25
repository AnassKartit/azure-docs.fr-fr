---
title: Acquérir un jeton à partir du cache (MSAL.NET)
titleSuffix: Microsoft identity platform
description: Découvrez comment acquérir un jeton d’accès en mode silencieux (à partir du cache de jeton) à l’aide de la bibliothèque d’authentification Microsoft pour .NET (MSAL.NET).
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 07/16/2019
ms.author: marsma
ms.reviewer: saeeda
ms.custom: devx-track-csharp, aaddev
ms.openlocfilehash: 94e4a4d5b80e246ce822e977deafe5c41c9ff4ad
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98064774"
---
# <a name="get-a-token-from-the-token-cache-using-msalnet"></a>Obtenir un jeton à partir du cache de jeton à l’aide de MSAL.NET

Lorsque vous acquérez un jeton d’accès à l’aide de la bibliothèque d’authentification Microsoft pour .NET (MSAL.NET), le jeton est mis en cache. Lorsque l’application a besoin d’un jeton, elle doit tout d’abord appeler la méthode `AcquireTokenSilent` pour vérifier si un jeton acceptable est dans le cache. Dans de nombreux cas, il est possible d’acquérir un autre jeton avec d’autres étendues basées sur un jeton dans le cache. Il est aussi possible d’actualiser un jeton quand il arrive à expiration (étant donné que le cache de jetons contient également un jeton d’actualisation).

Le modèle recommandé consiste à commencer par appeler la méthode `AcquireTokenSilent`.  Si `AcquireTokenSilent` échoue, utilisez d’autres méthodes pour acquérir un jeton.

Dans l’exemple suivant, l’application essaie d’abord d’acquérir un jeton à partir du cache de jeton.  Si une exception `MsalUiRequiredException` est générée, l’application acquiert un jeton de manière interactive. 

```csharp
AuthenticationResult result = null;
var accounts = await app.GetAccountsAsync();

try
{
 result = await app.AcquireTokenSilent(scopes, accounts.FirstOrDefault())
        .ExecuteAsync();
}
catch (MsalUiRequiredException ex)
{
 // A MsalUiRequiredException happened on AcquireTokenSilent.
 // This indicates you need to call AcquireTokenInteractive to acquire a token
 System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

 try
 {
    result = await app.AcquireTokenInteractive(scopes)
          .ExecuteAsync();
 }
 catch (MsalException msalex)
 {
    ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
 }
}
catch (Exception ex)
{
 ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
 return;
}

if (result != null)
{
 string accessToken = result.AccessToken;
 // Use the token
}
```
