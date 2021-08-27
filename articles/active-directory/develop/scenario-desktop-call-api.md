---
title: Appeler des API web à partir d’une application de bureau | Azure
titleSuffix: Microsoft identity platform
description: Découvrez comment construire une application de bureau qui appelle des API web
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
ms.openlocfilehash: 9cfd26b8cca62c106cd389e4e9e33d058a46c842
ms.sourcegitcommit: deb5717df5a3c952115e452f206052737366df46
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122681273"
---
# <a name="desktop-app-that-calls-web-apis-call-a-web-api"></a>Application de bureau qui appelle des API web : Appeler une API web

Maintenant que vous avez un jeton, vous pouvez appeler une API web protégée.

## <a name="call-a-web-api"></a>Appeler une API web

# <a name="net"></a>[.NET](#tab/dotnet)

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

# <a name="java"></a>[Java](#tab/java)

```Java
HttpURLConnection conn = (HttpURLConnection) url.openConnection();

PublicClientApplication pca = PublicClientApplication.builder(clientId)
        .authority(authority)
        .build();

// Acquire a token, acquireTokenHelper would call publicClientApplication's acquireTokenSilently then acquireToken
// see https://github.com/Azure-Samples/ms-identity-java-desktop for a full example
IAuthenticationResult authenticationResult = acquireTokenHelper(pca);

// Set the appropriate header fields in the request header.
conn.setRequestProperty("Authorization", "Bearer " + authenticationResult.accessToken);
conn.setRequestProperty("Accept", "application/json");

String response = HttpClientHelper.getResponseStringFromConn(conn);

int responseCode = conn.getResponseCode();
if(responseCode != HttpURLConnection.HTTP_OK) {
    throw new IOException(response);
}

JSONObject responseObject = HttpClientHelper.processResponse(responseCode, response);
```

# <a name="macos"></a>[MacOS](#tab/macOS)

## <a name="call-a-web-api-in-msal-for-ios-and-macos"></a>Appeler une API web dans MSAL pour iOS et macOS

Les méthodes d'acquisition de jetons renvoient un objet `MSALResult`. `MSALResult` expose une propriété `accessToken` qui peut être utilisée pour appeler une API web. Ajoutez un jeton d’accès à l’en-tête d’autorisation HTTP avant de passer l’appel pour accéder à l’API web protégée.

Objective-C :

```objc
NSMutableURLRequest *urlRequest = [NSMutableURLRequest new];
urlRequest.URL = [NSURL URLWithString:"https://contoso.api.com"];
urlRequest.HTTPMethod = @"GET";
urlRequest.allHTTPHeaderFields = @{ @"Authorization" : [NSString stringWithFormat:@"Bearer %@", accessToken] };

NSURLSessionDataTask *task =
[[NSURLSession sharedSession] dataTaskWithRequest:urlRequest
     completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {}];
[task resume];
```

Swift :

```swift
let urlRequest = NSMutableURLRequest()
urlRequest.url = URL(string: "https://contoso.api.com")!
urlRequest.httpMethod = "GET"
urlRequest.allHTTPHeaderFields = [ "Authorization" : "Bearer \(accessToken)" ]

let task = URLSession.shared.dataTask(with: urlRequest as URLRequest) { (data: Data?, response: URLResponse?, error: Error?) in }
task.resume()
```

## <a name="call-several-apis-incremental-consent-and-conditional-access"></a>Appeler plusieurs API : Consentement incrémentiel et accès conditionnel

Si vous souhaitez appeler plusieurs API pour le même utilisateur, appelez `AcquireTokenSilent` après avoir obtenu un jeton pour la première API. La plupart du temps, vous obtenez un jeton en mode silencieux pour les autres API.

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Une interaction est obligatoire quand :

- L’utilisateur a donné son consentement pour la première API, mais doit également le donner pour d’autres d’étendues Ce type de consentement est connu sous le nom de consentement incrémentiel.
- La première API ne nécessitait aucune authentification multifacteur, contrairement à celle qui suit.

```csharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```

# <a name="nodejs"></a>[Node.JS](#tab/nodejs)

À l’aide d’un client HTTP comme [Axios](https://www.npmjs.com/package/axios), appelez l’URI du point de terminaison de l’API avec un jeton d’accès en tant que *porteur des autorisations*.

```javascript
const axios = require('axios');

async function callEndpointWithToken(endpoint, accessToken) {
    const options = {
        headers: {
            Authorization: `Bearer ${accessToken}`
        }
    };

    console.log('Request made at: ' + new Date().toString());

    const response = await axios.default.get(endpoint, options);

    return response.data;
}

```

<!--
More includes will come later for Python and Java
-->
# <a name="python"></a>[Python](#tab/python)

```Python
endpoint = "url to the API"
http_headers = {'Authorization': 'Bearer ' + result['access_token'],
                'Accept': 'application/json',
                'Content-Type': 'application/json'}
data = requests.get(endpoint, headers=http_headers, stream=False).json()
```

---

## <a name="next-steps"></a>Étapes suivantes

Passez à l’article suivant de ce scénario, [Passer en production](scenario-desktop-production.md).
