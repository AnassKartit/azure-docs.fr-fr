---
title: Passer en production une application de Bureau appelant des API web | Azure
titleSuffix: Microsoft identity platform
description: Découvrir comment passer en production une application de bureau qui appelle des API web
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 759dcafe3d1c98b73d9ddcaf079cba952b9a95ca
ms.sourcegitcommit: 28cd7097390c43a73b8e45a8b4f0f540f9123a6a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2021
ms.locfileid: "122777899"
---
# <a name="desktop-app-that-calls-web-apis-move-to-production"></a>Application de bureau qui appelle des API Web : Passer en production

Dans cet article, vous allez apprendre à déplacer votre application de bureau qui appelle des API Web en production.

## <a name="handle-errors-in-desktop-applications"></a>Gérer les erreurs dans les applications de bureau

Dans les différents flux, vous avez appris à gérer les erreurs pour les flux en mode silencieux, comme indiqué dans les extraits de code. Vous avez également vu qu’il existe des cas où l’interaction est nécessaire, comme par exemple le consentement incrémentiel et l’accès conditionnel.

## <a name="have-the-user-consent-upfront-for-several-resources"></a>Demander le consentement préalable de l’utilisateur sur plusieurs ressources

> [!NOTE]
> L’obtention d’un consentement pour plusieurs ressources fonctionne pour la plateforme d’identités Microsoft, mais pas pour Azure Active Directory (Azure AD) B2C. Azure AD B2C prend en charge le consentement de l’administrateur uniquement, et pas le consentement de l’utilisateur.

Vous ne pouvez pas obtenir un jeton pour plusieurs ressources à la fois avec la plateforme d’identités Microsoft. Le paramètre `scopes` peut contenir des étendues pour une seule ressource. Vous pouvez veiller à ce que l’utilisateur consente d’avance plusieurs ressources via le paramètre `extraScopesToConsent`.

Par exemple, si vous avez deux ressources, ayant chacune deux étendues :

- `https://mytenant.onmicrosoft.com/customerapi` avec les étendues `customer.read` et `customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi` avec les étendues `vendor.read` et `vendor.write`

Dans cet exemple, utilisez le modificateur `.WithExtraScopesToConsent` qui dispose du paramètre `extraScopesToConsent`.

Exemple :

### <a name="in-msalnet"></a>Dans MSAL.NET

```csharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopesToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

### <a name="in-msal-for-ios-and-macos"></a>Dans MSAL pour iOS et macOS

Objective-C :

```objc
NSArray *scopesForCustomerApi = @[@"https://mytenant.onmicrosoft.com/customerapi/customer.read",
                                @"https://mytenant.onmicrosoft.com/customerapi/customer.write"];

NSArray *scopesForVendorApi = @[@"https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
                              @"https://mytenant.onmicrosoft.com/vendorapi/vendor.write"]

MSALInteractiveTokenParameters *interactiveParams = [[MSALInteractiveTokenParameters alloc] initWithScopes:scopesForCustomerApi webviewParameters:[MSALWebviewParameters new]];
interactiveParams.extraScopesToConsent = scopesForVendorApi;
[application acquireTokenWithParameters:interactiveParams completionBlock:^(MSALResult *result, NSError *error) { /* handle result */ }];
```

Swift :

```swift
let scopesForCustomerApi = ["https://mytenant.onmicrosoft.com/customerapi/customer.read",
                            "https://mytenant.onmicrosoft.com/customerapi/customer.write"]

let scopesForVendorApi = ["https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
                          "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"]

let interactiveParameters = MSALInteractiveTokenParameters(scopes: scopesForCustomerApi, webviewParameters: MSALWebviewParameters())
interactiveParameters.extraScopesToConsent = scopesForVendorApi
application.acquireToken(with: interactiveParameters, completionBlock: { (result, error) in /* handle result */ })
```

Cet appel vous permet d’obtenir un jeton d’accès pour la première API Web.

Lors de l’appel de la deuxième API web, appelez l’API `AcquireTokenSilent`.

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsent-each-time-the-app-runs"></a>Le compte personnel Microsoft requiert de nouveau un consentement à chaque exécution de l’application

Pour les utilisateurs de compte personnel Microsoft, il convient de demander de nouveau le consentement sur chaque client natif (ordinateur de bureau ou application mobile) pour autoriser est le comportement prévu. L’identité de client natif est fondamentalement non sécurisée, ce qui est contraire à l’identité de l’application cliente confidentielle. Les applications clientes confidentielles échangent un secret avec la plateforme d’identité Microsoft pour prouver leur identité. La plateforme d’identité Microsoft a choisi d’atténuer ce manque de sécurité pour les services aux consommateur en invitant l’utilisateur à donner son consentement à chaque fois que l’application est autorisée.

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Étapes suivantes

Pour essayer des exemples supplémentaires, voir [Applications clientes publiques de bureau](sample-v2-code.md#desktop).



