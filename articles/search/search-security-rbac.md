---
title: Autorisation basée sur des rôles
titleSuffix: Azure Cognitive Search
description: Utilisez le contrôle d’accès en fonction du rôle Azure (Azure RBAC) pour obtenir des autorisations granulaires sur l’administration du service et les tâches de contenu.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/22/2021
ms.openlocfilehash: 3a9669b2c569947c76f4f2b92fa316f3b09ab517
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128577901"
---
# <a name="use-role-based-authorization-in-azure-cognitive-search"></a>Utiliser l’autorisation basée sur les rôles dans Azure Recherche cognitive

Azure fournit un système d'autorisation global de[contrôle d'accès basé sur les rôles](../role-based-access-control/role-assignments-portal.md) (RBAC) pour tous les services fonctionnant sur la plateforme. Dans Recherche cognitive, vous pouvez utiliser les rôles des manières suivantes :

+ Utilisez les rôles généralement disponibles pour l’administration des services.

+ Utilisez les nouveaux rôles d’aperçu pour la gestion de contenu (création et gestion d’index et autres objets de niveau supérieur), [**disponible par demande**](https://aka.ms/azure-cognitive-search/rbac-preview).

> [!NOTe]
> Le Contributeur du Service de recherche est un rôle de « disponibilité générale » qui dispose de fonctionnalités de « préversion ». Il s’agit du seul rôle qui prend en charge un véritable hybride de tâches de gestion de service et de contenu, ce qui permet d’effectuer toutes les opérations sur un service de recherche donné. Pour obtenir la préversion de la gestion de contenu sur ce rôle, [**inscrivez-vous à la préversion**](https://aka.ms/azure-cognitive-search/rbac-preview).

Certains scénarios RBAC ne sont **pas** pris en charge ou ne sont pas traités dans cet article :

+ Les connexions de l’indexeur sortant sont documentées dans [« Configurer une connexion d’indexeur à une source de données à l’aide d’une identité managée »](search-howto-managed-identities-data-sources.md). Pour un service de recherche auquel une identité managée est affectée, vous pouvez créer des attributions de rôles qui étendent les services de données externes, tels que le Stockage d’objets Blob Azure, l’accès en lecture sur les objets blob par votre service de recherche approuvé.

+ Les [Rôles personnalisés](../role-based-access-control/custom-roles.md) ne sont pas pris en charge.

+ L’accès à l’identité de l’utilisateur via les résultats de la recherche (parfois appelé sécurité au niveau des lignes ou sécurité au niveau du document) n’est pas pris en charge. Pour un accès en fonction de l’identité sur les résultats de recherche, vous pouvez créer des [filtres de sécurité](search-security-trimming-for-azure-search.md) pour ajuster les résultats par identité, en supprimant les documents auxquels le demandeur ne doit pas avoir accès.

## <a name="built-in-roles-used-in-search"></a>Rôles intégrés utilisés dans la Recherche

Dans la Recherche cognitive, les rôles intégrés incluent les rôles généralement disponibles et en préversion, dont l’appartenance affectée est constituée des utilisateurs et groupes du Répertoire actif Azure.

Les attributions de rôles sont cumulatives et omniprésentes pour tous les outils et bibliothèques clientes utilisés pour créer ou gérer un service de recherche. Ces clients incluent le portail Azure, l’API REST de Gestion, PowerShell Azure, l’interface CLI Azure et la bibliothèque cliente de gestion des kits de développement logiciel (sdk) Azure.

Les rôles s’appliquent au service de recherche dans son ensemble et doivent être attribués par un Propriétaire. Vous ne pouvez pas assigner de rôles à des index spécifiques ou à d’autres objets de niveau supérieur.

Il n’existe aucune restriction régionale, de niveau ou de tarification pour l’utilisation de RBAC sur Azure Recherche cognitive, mais votre service de recherche doit se trouver dans le cloud public Azure.

| Role | S’applique à | Description |
| ---- | ---------- | ----------- |
| [Propriétaire](../role-based-access-control/built-in-roles.md#owner) | Opérations de service (généralement disponibles) | Accès complet à la ressource de recherche, y compris la possibilité d’affecter des rôles Azure. Les administrateurs d’abonnements sont membres par défaut. |
| [Contributeur](../role-based-access-control/built-in-roles.md#contributor) | Opérations de service (généralement disponibles) | Même niveau d’accès que le Propriétaire, moins la possibilité d’affecter des rôles ou de modifier les options d’autorisation. |
| [Lecteur](../role-based-access-control/built-in-roles.md#reader) | Opérations de service (généralement disponibles) | Accès limité à des informations de service partielles. Sur le portail, le rôle Lecteur permet d'accéder aux informations de la page de présentation du service, de la section Éléments principaux et de l'onglet Surveillance. Tous les autres onglets et pages sont inaccessibles. </br></br>Ce rôle a accès aux informations de service : groupe de ressources, état du service, emplacement, nom et ID de l’abonnement, balises, URL, niveau tarifaire, réplicas, partitions et unités de recherche. </br></br>Ce rôle a aussi accès aux métriques de service : latence de recherche, pourcentage de demandes limitées, nombre moyen de requêtes par seconde. </br></br>Il n’y a pas d’accès aux clés API, aux attributions de rôles, au contenu (index ou cartes de synonymes) ou aux métriques de contenu (stockage consommé, nombre d’objets). |
| [Contributeur du service de recherche](../role-based-access-control/built-in-roles.md#search-service-contributor) | Service OPS (généralement disponible), objets et contenu de niveau supérieur (préversion) | Ce rôle est une combinaison du Contributeur au niveau du service, mais avec un accès complet à toutes les actions sur les index, les cartes de synonymes, les indexeurs, les sources de données et les compétences dans [`Microsoft.Search/searchServices/*`](/azure/role-based-access-control/resource-provider-operations#microsoftsearch) au niveau du contenu. Ce rôle est destiné aux administrateurs de service de recherche qui ont besoin de gérer intégralement le service et son contenu. Pour la gestion de contenu, vous devez vous inscrire à la préversion. </br></br>Comme le Contributeur, les membres de ce rôle ne peuvent pas créer ou gérer des attributions de rôles, ni modifier les options d’autorisation. |
| [Contributeur de données d’index de la Recherche](../role-based-access-control/built-in-roles.md#search-index-data-contributor) | Collection de documents (préversion) | Fournit un accès complet au contenu de tous les index sur le service de recherche. Ce rôle est destiné aux développeurs ou aux propriétaires d’index qui ont besoin d’importer, d’actualiser ou d’interroger la collection de documents d’un index. |
| [Lecteur de données d’index de la Recherche](../role-based-access-control/built-in-roles.md#search-index-data-reader) | Collection de documents (préversion) | Fournit un accès en lecture seule aux index de recherche sur le service de recherche. Ce rôle est destiné aux applications et utilisateurs qui exécutent des requêtes. |

> [!NOTE]
> Les ressources Azure ont le concept de catégories d'opérations du [plan de contrôle et du plan de données](../azure-resource-manager/management/control-plane-and-data-plane.md). Dans Recherche cognitive, « plan de contrôle » fait référence à toute opération prise en charge dans l'[API REST de gestion](/rest/api/searchmanagement/) ou les bibliothèques clientes équivalentes. Le « plan de données » fait référence aux opérations sur le point de terminaison du service de recherche, telles que l’indexation ou les requêtes, ou toute autre opération spécifiée dans l'[API REST de recherche](/rest/api/searchservice/) ou les bibliothèques clientes équivalentes. La plupart des rôles s’appliquent à un seul plan. L’exception est le Contributeur du Service de recherche qui prend en charge les actions dans les deux.

## <a name="step-1-preview-sign-up"></a>Étape 1 : inscription à la préversion

**S’applique à :** Recherche dans l’index de données Contributeur, Lecteur de données d’index de recherche, Contributeur du Service de recherche

Ignorez cette étape si vous utilisez des rôles généralement disponibles (Propriétaire, Contributeur, Lecteur) ou uniquement les actions de niveau de service du Contributeur du Service de recherche.

Les nouveaux rôles intégrés en préversion fournissent un ensemble granulaire d’autorisations sur le contenu du service de recherche. Bien que les rôles intégrés soient toujours visibles dans le portail Azure, l’inscription du service est nécessaire pour qu’ils soient opérationnels.

Pour l’inscription dans le programme de préversion :

+ [Remplir ce formulaire](https://aka.ms/azure-cognitive-search/rbac-preview)

Le traitement de la demande d’inscription de processus peut prendre jusqu’à deux jours ouvrables. Vous recevrez un courriel lorsque votre service sera prêt.

## <a name="step-2-preview-configuration"></a>Étape 2 : Aperçu de la configuration

**S’applique à :** Recherche dans l’index de données Contributeur, Lecteur de données d’index de recherche, Contributeur du Service de recherche

Ignorez cette étape si vous utilisez des rôles généralement disponibles (Propriétaire, Contributeur, Lecteur) ou uniquement les actions de niveau de service du Contributeur du Service de recherche.

Dans cette étape, configurez votre service de recherche afin qu’il reconnaisse un en-tête d’**autorisation** sur les demandes de données qui fournissent un jeton d’accès OAuth2.

### <a name="azure-portal"></a>[**Portail Azure**](#tab/config-svc-portal)

1. Ouvrez le portail avec la syntaxe suivante : [https://ms.portal.azure.com/?feature.enableRbac=true](https://ms.portal.azure.com/?feature.enableRbac=true).

1. Accéder à votre service de recherche.

1. Sélectionnez **Clés** dans le volet de navigation de gauche.

1. Choisissez un mécanisme de **contrôle d'accès à l'API**. 

   | Option | Statut | Description |
   |--------|--------|-------------|
   | Clé d’API | Généralement disponible (par défaut) | Requiert un [administrateur ou des clés API](search-security-api-keys.md) de requête dans l'en-tête de la demande pour l'autorisation. Aucun rôle n’est utilisé. |
   | Contrôle d’accès en fonction du rôle | Préversion | Nécessite l’appartenance à une attribution de rôle pour terminer la tâche, décrite dans l’étape suivante. Chaque requête nécessite un en-tête d’autorisation. Le choix de cette option vous limite aux clients qui prennent en charge l’API REST 2021-04-30-preview. |
   | Les deux | Préversion | Les demandes sont valides à l’aide d’une clé API ou d’un jeton d’autorisation. |

Si vous ne voyez pas les options, vérifiez l’URL du portail.

Si vous ne pouvez pas enregistrer votre sélection ou si vous recevez « échec de la mise à jour du contrôle d’accès d’API pour le service de recherche `<name>` . DisableLocalAuth est en préversion et n’est pas activé pour cet abonnement », votre inscription n’a pas été lancée ou n’a pas été traitée.

### <a name="rest-api"></a>[**API REST**](#tab/config-svc-rest)

Utilisez l’API REST de gestion version 2021-04-01-Préversion, [Créer ou mettre à jour le service](/rest/api/searchmanagement/2021-04-01-preview/services/create-or-update), pour configurer votre service.

Si vous utilisez le billet ou un autre outil de test Web, consultez le Conseil ci-dessous pour obtenir de l’aide sur la configuration de la demande.

1. Définissez [« AuthOptions »](/rest/api/searchmanagement/2021-04-01-preview/services/create-or-update#dataplaneauthoptions) sur « aadOrApiKey ».

   Si vous le souhaitez, définissez [« AadAuthFailureMode »](/rest/api/searchmanagement/2021-04-01-preview/services/create-or-update#aadauthfailuremode) pour spécifier si 401 est retourné à la place de 403 lorsque l’authentification échoue. La valeur par défaut de « disableLocalAuth » est false. Vous n’avez donc pas besoin de la définir, mais elle est indiquée ci-dessous pour insister sur le fait que la valeur doit être false chaque fois que authOptions est défini.

    ```http
    PUT https://management.azure.com/subscriptions/{{subscriptionId}}/resourcegroups/{{resource-group}}/providers/Microsoft.Search/searchServices/{{search-service-name}}?api-version=2021-04-01-Preview
    {
      "location": "{{region}}",
      "sku": {
        "name": "standard"
      },
      "properties": {
        "disableLocalAuth": false,
        "authOptions": {
          "aadOrApiKey": {
            "aadAuthFailureMode": "http401WithBearerChallenge"
          }
        }
      }
   }
    ```

1. [Assignez des rôles](#assign-roles) sur le service et vérifiez qu’ils fonctionnent correctement dans le plan de données.

> [!TIP]
> Les appels de l’API REST de gestion sont authentifiés via le Répertoire actif Azure. Pour obtenir des conseils sur la configuration d’un principe de sécurité et d’une demande, consultez ce billet de blog [API REST Azure avec Billet (2021)](https://blog.jongallant.com/2021/02/azure-rest-apis-postman-2021/). L’exemple précédent a été testé à l’aide des instructions et de la collection de Billets fournies dans le billet de blog.

---

<a name="assign-roles"></a>

## <a name="step-3-assign-roles"></a>Étape 3 : Attribution des rôles

Les rôles peuvent être attribués à l'aide de l'une des [approches prises](../role-based-access-control/role-assignments-steps.md) en charge décrites dans la documentation sur le contrôle d'accès basé sur les rôles d'Azure.

Vous devez être un **Propriétaire** ou avoir des autorisations[Microsoft.Authorization/roleAssignments/write](/azure/templates/microsoft.authorization/roleassignments) pour gérer les attributions de rôles.

### <a name="azure-portal"></a>[**Portail Azure**](#tab/roles-portal)

1. Pour les rôles en préversion, ouvrez le portail avec la syntaxe suivante : [https://ms.portal.azure.com/?feature.enableRbac=true](https://ms.portal.azure.com/?feature.enableRbac=true). Vous devriez voir `feature.enableRbac=true` dans l'URL.

   > [!NOTE]
   > Pour les utilisateurs et les groupes affectés à un rôle de préversion, le contenu du portail, tel que les index et les indexeurs, est visible uniquement si vous ouvrez le portail avec l’indicateur de fonctionnalité. 

1. Accéder à votre service de recherche.

1. Sélectionnez **Contrôle d’accès (IAM)** dans le menu de navigation de gauche.

1. Sur le côté droit, sous **Autoriser l'accès à cette ressource**, sélectionnez **Ajouter une attribution de rôle**.

1. Trouvez un rôle applicable, puis affecter une identité d’utilisateur ou de groupe de Répertoire actif Azure.

   + Propriétaire
   + Contributeur
   + Lecteur
   + Contributeur du service de recherche
   + Contributeur de données d’index de recherche (préversion)
   + Lecteur de données d’index de recherche (préversion)

### <a name="powershell"></a>[**PowerShell**](#tab/roles-powershell)

Lorsque vous [utilisez PowerShell pour attribuer des rôles](../role-based-access-control/role-assignments-powershell.md), appelez [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) en indiquant le nom de l'utilisateur ou du groupe Azure et la portée de l'attribution.

Avant de commencer, veillez à charger les modules Azure et AzureAD et à vous connecter à Azure :

```powershell
Import-Module -Name Az
Import-Module -Name AzureAD
Connect-AzAccount
```

Dans le cadre du service, votre syntaxe doit ressembler à l’exemple suivant :

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Search Index Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Search/searchServices/<search-service>"
```

Limité à un index individuel :

```powershell
New-AzRoleAssignment -SignInName <email> `
    -RoleDefinitionName "Search Index Data Contributor" `
    -Scope  "/subscriptions/<subscription>/resourceGroups/<resource-group>/providers/Microsoft.Search/searchServices/<search-service>/indexes/<index-name>"
```

Rappelez-vous que vous pouvez uniquement limiter l’accès à des ressources de niveau supérieur, telles que des index, des cartes de synonymes, des indexeurs, des sources de données et des compétences. Vous ne pouvez pas contrôler l’accès aux documents de recherche (contenu d’index) avec des rôles Azure.

---

## <a name="step-4-test"></a>Étape 4 : Tester

### <a name="azure-portal"></a>[**Portail Azure**](#tab/test-portal)

1. Pour les rôles en préversion, ouvrez le portail avec la syntaxe suivante : [https://ms.portal.azure.com/?feature.enableRbac=true](https://ms.portal.azure.com/?feature.enableRbac=true). 

   > [!NOTE]
   > Pour les utilisateurs et les groupes affectés à un rôle de préversion, le contenu du portail, tel que les index et les indexeurs, est visible uniquement si vous ouvrez le portail avec l’indicateur de fonctionnalité. 

1. Accéder à votre service de recherche.

1. Dans la page de la vue d’ensemble, sélectionnez l’onglet **Index** :

   + Les membres du Lecteur de données d’index de recherche peuvent utiliser l’Explorateur de recherche pour interroger l'index. Vous pouvez utiliser n’importe quelle version d’API pour vérifier l’accès. Vous devez être en mesure d’émettre des requêtes et d’afficher les résultats, mais vous ne devriez pas être en mesure d’afficher la définition de l’index.

   + Les membres du Contributeur de données d'index de recherche peuvent sélectionner **Nouvel Index** pour créer un nouvel index. L’enregistrement d’un nouvel index permet de vérifier l’accès en écriture au service.

### <a name="rest-api"></a>[**API REST**](#tab/test-rest)

+ Inscrivez votre application auprès d’Azure Active Directory.

+ Révisez votre code pour utiliser une [API REST de recherche](/rest/api/searchservice/) (toute version prise en charge) et définissez l’en-tête d’**Autorisation** sur les requêtes, en remplaçant l’en-tête **API-Key** .

  :::image type="content" source="media/search-security-rbac/rest-authorization-header.png" alt-text="Capture d’écran d’une requête HTTP avec un en-tête d’Autorisation" border="true":::

Pour plus d’informations sur l’acquisition d’un jeton pour un environnement spécifique, consultez les [Bibliothèques d’authentification de la plateforme d’identités Microsoft](/azure/active-directory/develop/reference-v2-libraries).

### <a name="net-sdk"></a>[**Kit de développement logiciel (SDK) .NET**](#tab/test-dotnet)

Le kit de développement logiciel (SDK) Azure pour .NET prend en charge un en-tête d’autorisation dans le package de la[Galerie NuGet | Azure.Search.Documents 11.4.0-beta. 2](https://www.nuget.org/packages/Azure.Search.Documents/11.4.0-beta.2).

Une configuration supplémentaire est requise pour inscrire une application auprès du Répertoire actif Azure ou obtenir et passer des jetons d’autorisation.

+ Lors de l’obtention du jeton OAuth, l’étendue est « https://search.azure.com/.default ». Le kit de développement logiciel (SDK) exige que l’audience soit « https://search.azure.com ». « . default » est une convention d’Azure AD.

+ Le kit de développement logiciel (SDK) vérifie que l’utilisateur dispose de l’étendue « user_impersonation », qui doit être accordée par votre application, mais le kit de développement logiciel (SDK) lui-même demande « https://search.azure.com/.default ».

Exemple d’utilisation des [informations d’identification de la clé secrète client](/dotnet/api/azure.core.tokencredential):

```csharp
var tokenCredential =  new ClientSecretCredential(aadTenantId, aadClientId, aadSecret);
SearchClient srchclient = new SearchClient(serviceEndpoint, indexName, tokenCredential);
```

> [!NOTE]
> Si vous recevez une erreur 403, vérifiez que votre service de recherche est inscrit dans le programme d’évaluation et que votre service est configuré pour afficher un aperçu des attributions de rôle.

---

## <a name="disable-api-key-authentication"></a>Désactiver l'authentification par clé API

Les clés API ne peuvent pas être supprimées, mais elles peuvent être désactivées sur votre service. Si vous utilisez les rôles de Contributeur du Service de recherche, de Contributeur de données d'index de recherche et de Lecteur de données d'index de recherche et l'authentification Azure AD, vous pouvez désactiver les clés API, ce qui a pour effet que le service de recherche refuse toutes les demandes liées aux données qui fournissent une clé.

Pour désactiver [l’authentification basée sur les clés](search-security-api-keys.md), utilisez l’API REST de gestion version 2021-04-01-Préversion et envoyez deux demandes consécutives pour le [Service de mise à jour](/rest/api/searchmanagement/2021-04-01-preview/services/create-or-update).

Des autorisations de Propriétaire ou de Contributeur sont requises pour désactiver les fonctionnalités. Utilisez le billet ou un autre outil de test Web pour effectuer les étapes suivantes (voir le Conseil ci-dessous) :

1. Lors de la première requête, définissez [« AuthOptions »](/rest/api/searchmanagement/2021-04-01-preview/services/create-or-update#dataplaneauthoptions) sur « aadOrApiKey » pour activer l’authentification Azure AD. Notez que l’option indique la disponibilité des deux approches : Azure AD ou les clés API natives.

    ```http
    PUT https://management.azure.com/subscriptions/{{subscriptionId}}/resourcegroups/{{resource-group}}/providers/Microsoft.Search/searchServices/{{search-service-name}}?api-version=2021-04-01-Preview
    {
      "location": "{{region}}",
      "sku": {
        "name": "standard"
      },
      "properties": {
        "authOptions": {
          "aadOrApiKey": {
            "aadAuthFailureMode": "http401WithBearerChallenge"
          }
        }
      }
   }
    ```

1. Sur la deuxième requête, affectez la valeur true à [« disableLocalAuth »](/rest/api/searchmanagement/2021-04-01-preview/services/create-or-update#request-body). Cette étape désactive la partie clé API de l’option « aadOrApiKey », vous laissant simplement l’authentification Azure AD.

    ```http
    PUT https://management.azure.com/subscriptions/{{subscriptionId}}/resourcegroups/{{resource-group}}/providers/Microsoft.Search/searchServices/{{search-service-name}}?api-version=2021-04-01-Preview
    {
      "location": "{{region}}",
      "sku": {
        "name": "standard"
      },
      "properties": {
        "disableLocalAuth": true
      }
    }
    ```

Vous ne pouvez pas combiner les étapes 1 et 2. À l’étape 1, « disableLocalAuth » doit avoir la valeur false pour respecter la configuration requise pour le paramètre « AuthOptions », tandis que l’étape 2 modifie cette valeur en true.

Pour réactiver l’authentification de la clé, réexécutez la dernière demande, en affectant la valeur false à « disableLocalAuth ». Le service de recherche recommencera à accepter automatiquement les clés API sur la requête (en supposant qu'elles soient spécifiées).

> [!TIP]
> Les appels de l’API REST de gestion sont authentifiés via le Répertoire actif Azure. Pour obtenir des conseils sur la configuration d’un principe de sécurité et d’une demande, consultez ce billet de blog [API REST Azure avec Billet (2021)](https://blog.jongallant.com/2021/02/azure-rest-apis-postman-2021/). L’exemple précédent a été testé à l’aide des instructions et de la collection de Billets fournies dans le billet de blog.
