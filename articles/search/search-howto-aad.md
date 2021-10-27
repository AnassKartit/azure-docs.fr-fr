---
title: Autoriser les demandes de recherche à l’aide d’Azure AD
titleSuffix: Azure Cognitive Search
description: Acquérir un jeton d’Azure AD pour autoriser les demandes de recherche
author: dereklegenzoff
ms.author: delegenz
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/04/2021
ms.openlocfilehash: ece84cc15c9945d8f39e3163ab0da2941d4f1349
ms.sourcegitcommit: 01dcf169b71589228d615e3cb49ae284e3e058cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2021
ms.locfileid: "130162009"
---
# <a name="authorize-search-requests-using-azure-ad-preview"></a>Autoriser les demandes de recherche à l’aide d’Azure AD (préversion)

> [!IMPORTANT]
> Le contrôle d’accès en fonction du rôle pour les opérations du plan de données, telles que la création d’un index ou l’interrogation d’un index, est actuellement en préversion publique et disponible sous [des Conditions d’utilisation supplémentaires](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Cette fonctionnalité est uniquement disponible dans les clouds publics et peut avoir un impact sur la latence de vos opérations pendant la période de préversion. 

Grâce à Azure Active Directory (Azure AD), vous pouvez utiliser le contrôle d’accès en fonction du rôle (RBAC) pour accorder l’accès à vos services Recherche cognitive Azure. L’un des principaux avantages de l’utilisation d’Azure AD est que vos informations d’identification n’ont plus besoin d’être stockées dans votre code. Azure AD authentifie le principal de sécurité (un utilisateur, un groupe ou un principal de service) qui exécute l’application. Si l’authentification réussit, Azure AD renvoie le jeton d’accès à l’application, et l’application peut ensuite l’utiliser pour autoriser les requêtes adressées à Recherche cognitive Azure. Pour en savoir plus sur les avantages de l’utilisation d’Azure AD dans vos applications, consultez [Intégration à Azure Active Directory](/azure/active-directory/develop/active-directory-how-to-integrate#benefits-of-integration).

Cet article vous montre comment configurer votre application pour l’authentification à l’aide de le plateforme d’identités Microsoft. Pour en savoir plus sur la plateforme d’identités Microsoft, consultez [Vue d’ensemble de la plateforme d’identités Microsoft](/azure/active-directory/develop/v2-overview). Pour en savoir plus sur le flux d’octroi de code OAuth 2.0 utilisé par Azure AD, consultez [Autoriser l’accès aux applications web Azure Active Directory à l’aide du flux d’octroi de code OAuth 2.0](/azure/active-directory/develop/v2-oauth2-auth-code-flow).

## <a name="prepare-your-search-service"></a>Préparer votre service de recherche

Dans un premier temps, [créez un service de recherche](search-create-service-portal.md) et configurez-le pour utiliser le contrôle d’accès en fonction du rôle (RBAC).

### <a name="sign-up-for-the-preview"></a>S’inscrire à la version préliminaire

Les parties des capacités RBAC de Recherche cognitive Azure nécessaires à l’utilisation d’Azure AD pour interroger le service de recherche sont encore en phase de préversion. 

Pour vous inscrire à la préversion, [remplissez ce formulaire](https://aka.ms/azure-cognitive-search/rbac-preview).

Le traitement de votre demande peut prendre quelques jours ouvrables. 

### <a name="enable-rbac-for-data-plane-operations"></a>Activer le RBAC pour les opérations du plan de données

Une fois votre abonnement intégré à la préversion, vous devrez activer le RBAC pour les opérations du plan de données afin de pouvoir utiliser l’authentification Azure AD. Par défaut, Recherche cognitive Azure utilise l’authentification par clé pour les opérations du plan de données, mais vous pouvez modifier ce paramètre pour autoriser le contrôle d’accès en fonction du rôle. 

Pour activer le contrôle d’accès en fonction du rôle :

1. Accédez au portail Azure à l’aide de ce lien de préversion : [https://ms.portal.azure.com/?feature.enableRbac=true](https://ms.portal.azure.com/?feature.enableRbac=true). 
1. Dans le volet de navigation gauche, sélectionnez **Clés**.
1. Déterminez si vous souhaitez autoriser le contrôle d’accès en fonction de la clé et du rôle ou uniquement le contrôle d’accès en fonction du rôle.

![options d’authentification pour recherche cognitive Azure dans le portail](media/search-howto-aad/portal-api-access-control.png)

Vous pouvez également modifier ces paramètres par programmation, comme décrit dans la [documentation de Recherche cognitive Azure relative au RBAC](/azure/search/search-security-rbac?tabs=config-svc-rest%2Croles-powershell%2Ctest-rest#step-2-preview-configuration).

## <a name="register-an-application-with-azure-ad"></a>inscrire une application auprès d’Azure AD ;

L’étape suivante pour utiliser Azure AD pour l’authentification consiste à inscrire une application auprès de la [plateforme d’identités Microsoft](/azure/active-directory/develop/quickstart-register-app). Si vous rencontrez des problèmes lors de la création de l’application, vérifiez que vous disposez des [autorisations nécessaires pour inscrire une application](/azure/active-directory/develop/howto-create-service-principal-portal#permissions-required-for-registering-an-app).

Pour inscrire une application auprès d’Azure AD :

1. Connectez-vous à votre compte Azure sur le [portail Azure](https://portal.azure.com).
1. Sélectionnez **Azure Active Directory**.
1. Sélectionner **Inscriptions des applications**.
1. Sélectionnez **Nouvelle inscription**.
1. Donnez un nom à votre application et sélectionnez un type de compte pris en charge, qui détermine qui peut utiliser l’application. Sélectionnez ensuite **Inscription**.

![Assistant Inscription d’une application](media/search-howto-aad/register-app.png)

À ce stade, vous avez créé votre application Azure AD et le principal de service. Notez l’ID du locataire (ou du répertoire) et l’ID client (ou de l’application) affichés sur la page de vue d’ensemble de l’inscription de votre application. Vous aurez besoin de ces valeurs dans une étape ultérieure.

## <a name="create-a-client-secret"></a>Créer une clé secrète client

L’application aura également besoin d’une clé secrète client pour prouver son identité lors de la demande d’un jeton. Dans ce document, nous allons montrer comment utiliser une clé secrète client.

1. Accédez à l’inscription d’application que vous venez de créer.
1. Sélectionnez **Certificats et secrets**.
1. Sous **Secrets client**, cliquez **+ Nouveau secret client**.
1. Fournissez une description du secret et sélectionnez l’intervalle d’expiration souhaité.

![assistant création d’une clé secrète client](media/search-howto-aad/create-secret.png)

Veillez à enregistrer la valeur du secret dans un endroit sûr, car vous ne pourrez plus y accéder. 

## <a name="grant-your-application-permissions-to-azure-cognitive-search"></a>Accordez vos permissions d’application à Recherche cognitive Azure.

Ensuite, vous devez accorder à votre application Azure AD l’accès à votre service de recherche. Recherche cognitive Azure dispose de plusieurs [rôles intégrés](/azure/search/search-security-rbac?tabs=config-svc-portal%2Croles-portal%2Ctest-portal#built-in-roles-used-in-search) qui peuvent être utilisés en fonction de l’accès requis par votre application.

En général, il est préférable de fournir à votre application uniquement l’accès requis. Par exemple, si votre application doit uniquement être en mesure d’interroger l’index de recherche, vous pouvez lui accorder le rôle [Lecteur de données d’index de recherche (préversion)](/azure/role-based-access-control/built-in-roles#search-index-data-reader). Par contre, si elle doit pouvoir lire et écrire dans un index de recherche, vous pouvez utiliser le rôle [Contributeur de données d’index de recherche (préversion)](/azure/role-based-access-control/built-in-roles#search-index-data-contributor).

Pour attribuer un rôle à votre inscription d’application :

1. Ouvrez le portail Azure et accédez à votre service de recherche.
1. Sélectionnez **Contrôle d’accès (IAM)** dans le menu de navigation de gauche.
1. Sur le côté droit, sous **Autoriser l’accès à cette ressource**, sélectionnez **Ajouter une attribution de rôle**.
1. Sélectionnez le rôle que vous souhaitez utiliser, puis cliquez sur **Suivant**.
1. Sur la page suivante, cliquez sur **Sélectionner les membres** et recherchez l’application que vous avez créée précédemment. 
1. Enfin, cliquez sur **Vérifier + attribuer**.

![Ajouter une attribution de rôle dans le portail Azure](media/search-howto-aad/role-assignment.png)

Vous pouvez également [attribuer des rôles à l’aide de PowerShell](/azure/search/search-security-rbac?tabs=config-svc-rest%2Croles-powershell%2Ctest-rest#step-3-assign-roles).

### <a name="create-a-custom-role"></a>Créer un rôle personnalisé

Outre l’utilisation de [rôles intégrés](/azure/search/search-security-rbac?tabs=config-svc-portal%2Croles-portal%2Ctest-portal#built-in-roles-used-in-search), vous pouvez également créer un [rôle personnalisé](/azure/role-based-access-control/custom-roles) pour définir exactement ce que vous souhaitez que votre application soit en mesure de faire.

Par exemple, si vous souhaitez un rôle qui a la capacité de gérer intégralement des index, y compris la capacité de créer des index et de lire des données à partir de ceux-ci, vous pouvez définir le rôle indiqué ci-dessous :

```json
{
  "Name": "Search Index Manager",
  "Id": "88888888-8888-8888-8888-888888888888",
  "IsCustom": true,
  "Description": "Can manage search indexes and read or write to them",
  "Actions": [
    "Microsoft.Search/searchServices/indexes/*",
    
  ],
  "NotActions": [],
  "DataActions": [
      "Microsoft.Search/searchServices/indexes/documents/*"
  ],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/{subscriptionId1}"
  ]
}
```

Vous pouvez créer des rôles personnalisés à l’aide du [portail Azure](/azure/role-based-access-control/custom-roles-portal), d’[Azure PowerShell](/azure/role-based-access-control/custom-roles-powershell), d’[Azure CLI](/azure/role-based-access-control/custom-roles-cli) ou de l’[API REST](/azure/role-based-access-control/custom-roles-rest). Le JSON ci-dessus montre la syntaxe permettant de créer un rôle personnalisé avec PowerShell.

Pour obtenir la liste complète des opérations disponibles, consultez [Opérations du fournisseur de ressources Microsoft.Search](/azure/role-based-access-control/resource-provider-operations#microsoftsearch).


### <a name="grant-access-to-only-a-single-index"></a>Accorder l’accès à un seul index

Dans certains scénarios, vous souhaiterez peut-être limiter l’accès d’une application à une seule ressource, telle qu’un index. 

Le portail ne prend actuellement pas en charge l’octroi de l’accès à un seul index, mais il est possible de le faire avec [PowerShell](../role-based-access-control/role-assignments-powershell.md) ou [Azure CLI](../role-based-access-control/role-assignments-cli.md).

Dans PowerShell, vous devez utiliser [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment), en indiquant le nom de l’utilisateur ou du groupe Azure et l’étendue de l’attribution.

Avant de commencer, veillez à charger les modules Azure et AzureAD et à vous connecter à votre compte Azure :

```powershell
Import-Module -Name Az
Import-Module -Name AzureAD
Connect-AzAccount
```

Pour ajouter une attribution de rôle limitée à un index individuel, vous devez exécuter la commande suivante :

```powershell
New-AzRoleAssignment -ObjectId <objectId> `
    -RoleDefinitionName "Search Index Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Search/searchServices/<search-service>/indexes/<index-name>"
```

## <a name="set-up-azure-ad-authentication-in-your-client"></a>Configurer l’authentification Azure AD dans votre client

Une fois que vous avez créé une application Azure AD et que vous lui avez accordé des autorisations pour accéder à votre service de recherche, vous pouvez ajouter du code à votre application pour authentifier un principal de sécurité et acquérir un jeton OAuth 2.0.

### <a name="azure-ad-authentication-with-the-net-sdk"></a>Authentification Azure AD avec le Kit de développement logiciel (SDK) .NET

Les Kits de développement logiciel (SDK) Azure facilitent l’intégration à Azure AD. Les versions [11.4.0-beta.2](https://www.nuget.org/packages/Azure.Search.Documents/11.4.0-beta.2) et ultérieures du Kit de développement logiciel (SDK) .NET prennent en charge l’authentification Azure AD. L’authentification Azure AD est également prise en charge dans la préversion des Kits de développement logiciel (SDK) pour [Java](https://search.maven.org/artifact/com.azure/azure-search-documents/11.5.0-beta.3/jar), [Python](https://pypi.org/project/azure-search-documents/11.3.0b3/) et [JavaScript](https://www.npmjs.com/package/@azure/search-documents/v/11.3.0-beta.3).

Comme point de départ, clonez le [code source](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/quickstart/v11) pour le [démarrage rapide C#](search-get-started-dotnet.md).  Le démarrage rapide utilise actuellement l’authentification par clé pour créer `SearchClient` et `SearchIndexClient`, mais vous pouvez apporter une petite modification pour basculer sur une authentification par rôle. Au lieu d’utiliser `AzureKeyCredential` au début de `Main()` dans [Program.cs](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/quickstart/v11/AzureSearchQuickstart-v11/Program.cs) : 

```dotnet
AzureKeyCredential credential = new AzureKeyCredential(apiKey);

// Create a SearchIndexClient to send create/delete index commands
SearchIndexClient adminClient = new SearchIndexClient(serviceEndpoint, credential);
// Create a SearchClient to load and query documents
SearchClient srchclient = new SearchClient(serviceEndpoint, indexName, credential);
```

Vous pouvez utiliser `ClientSecretCredential` pour vous authentifier auprès du service de recherche.

Avec cela, vous aurez besoin des éléments suivants :
+ L’ID du locataire (ou répertoire). Vous pouvez le retrouver sur la page Vue d’ensemble de votre inscription d’application.
+ L’ID client (ou d’application). Vous pouvez le retrouver sur la page Vue d’ensemble de votre inscription d’application.
+ La valeur de la clé secrète client que vous avez copiée dans une étape précédente.

```dotnet
var tokenCredential =  new ClientSecretCredential(aadTenantId, aadClientId, aadSecret);
SearchIndexClient adminClient = new SearchIndexClient(serviceEndpoint, tokenCredential);
```

Vous devez importer la bibliothèque [Azure.Identity](https://www.nuget.org/packages/Azure.Identity/) pour utiliser `ClientSecretCredential`.

La documentation d’Azure.Identity contient également des détails supplémentaires sur l’utilisation de l’[authentification Azure AD avec le Kit de développement logiciel (SDK) Azure pour .NET](/dotnet/api/overview/azure/identity-readme).

### <a name="azure-ad-authentication-with-the-rest-api"></a>Authentification Azure AD avec l’API REST

L’utilisation d’un Kit de développement logiciel (SDK) Azure simplifie le flux OAuth 2.0, mais vous pouvez également programmer directement dans le protocole de votre application. Tous les détails sont disponibles dans [Plateforme d’identités Microsoft et flux d’informations d’identification du client OAuth 2.0](/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow).

#### <a name="get-a-token"></a>Obtention d’un jeton

Commencez par [obtenir un jeton](/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow#get-a-token) auprès de la plateforme d’identités Microsoft :

```
POST /[tenant id]/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=[client id]
&scope=https%3A%2F%2Fsearch.azure.com%2F.default
&client_secret=[client secret]
&grant_type=client_credentials
```

L’étendue requise est « https://search.azure.com/.default  ». 

#### <a name="use-a-token"></a>Utilisation d’un jeton

Maintenant que vous avez un jeton, vous êtes prêt à envoyer une demande au service de recherche. 

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
Content-Type: application/json   
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="see-also"></a>Voir aussi

+ [Utiliser l’autorisation basée sur les rôles dans Azure Recherche cognitive](search-security-rbac.md)
+ [Autoriser l’accès aux applications web Azure Active Directory à l’aide du flux d’octroi de code OAuth 2.0](/azure/active-directory/develop/v2-oauth2-auth-code-flow)
+ [Intégration à Azure Active Directory](/azure/active-directory/develop/active-directory-how-to-integrate#benefits-of-integration)
+ [Rôle personnalisés Azure](/azure/role-based-access-control/custom-roles)

