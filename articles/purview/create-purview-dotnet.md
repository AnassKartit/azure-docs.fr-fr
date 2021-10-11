---
title: 'Démarrage rapide : Créer un compte Purview avec le kit SDK .NET'
description: Créez un compte Azure Purview à l'aide du kit de développement logiciel (SDK) .NET.
author: nayenama
ms.author: nayenama
ms.service: purview
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 09/27/2021
ms.openlocfilehash: 0bbecc10139616bcf33f1d553536f3bfc674c5a1
ms.sourcegitcommit: e8c34354266d00e85364cf07e1e39600f7eb71cd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/29/2021
ms.locfileid: "129215811"
---
# <a name="quickstart-create-a-purview-account-using-net-sdk"></a>Démarrage rapide : Créer un compte Purview avec le kit SDK .NET

Dans ce guide de démarrage rapide, vous allez utiliser le [kit SDK .NET](/dotnet/api/overview/azure/purviewresourceprovider) pour créer un compte Azure Purview.

Azure Purview est un service de gouvernance de données qui vous aide à gérer et régir votre paysage de données. En vous connectant aux données de vos sources locales, multiclouds et SaaS (software-as-a-service), Purview crée un mappage à jour de vos informations. Il identifie et classe les données sensibles, et fournit un lignage de bout en bout. Les consommateurs de données peuvent découvrir des données au sein de votre organisation, et les administrateurs de données sont en mesure d’auditer, de sécuriser et de garantir l’utilisation appropriée de vos données.

Pour plus d’informations sur Purview, [consultez notre page de présentation](overview.md). Pour plus d’informations sur le déploiement de Purview dans votre organisation, [consultez nos bonnes pratiques de déploiement](deployment-best-practices.md).

[!INCLUDE [purview-quickstart-prerequisites](includes/purview-quickstart-prerequisites.md)]

### <a name="visual-studio"></a>Visual Studio

La procédure pas à pas décrite dans cet article utilise Visual Studio 2019. Les procédures pour Visual Studio 2013, 2015 ou 2017 peuvent différer légèrement.

### <a name="azure-net-sdk"></a>Kit de développement logiciel (SDK) .NET Azure

Téléchargez et installez le [Kit de développement logiciel (SDK) .NET Azure](https://azure.microsoft.com/downloads/) sur votre machine.

## <a name="create-an-application-in-azure-active-directory"></a>Créer une application dans Azure Active Directory

1. Dans [Créer une application Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal), créez une application qui représente l’application .NET que vous créez dans ce didacticiel. Pour l’URL de connexion, vous pouvez fournir une URL factice, comme indiqué dans l’article (`https://contoso.org/exampleapp`).
1. Dans [Obtenir les valeurs pour la connexion](../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in), récupérez l’**ID d’application** et l’**ID de locataire**, puis notez ces valeurs que vous allez utiliser ultérieurement dans ce didacticiel.
1. Dans [Certificats et secrets](../active-directory/develop/howto-create-service-principal-portal.md#authentication-two-options), récupérez la **clé d’authentification**, puis notez cette valeur que vous allez utiliser ultérieurement dans ce didacticiel.
1. Dans [Affecter l’application à un rôle](../active-directory/develop/howto-create-service-principal-portal.md#assign-a-role-to-the-application), affectez l’application au rôle **Contributeur** au niveau de l’abonnement de sorte que l’application puisse créer des fabriques de données dans l’abonnement.

## <a name="create-a-visual-studio-project"></a>Créer un projet Visual Studio

Créez ensuite une application console .NET en C# dans Visual Studio :

1. Lancez **Visual Studio**.
2. Dans la fenêtre Démarrer, sélectionnez **Créer un projet** > **Application console (.NET Framework)** . .NET version 4.5.2 ou ultérieure est nécessaire.
3. Dans **Nom du projet**, entrez **PurviewQuickStart**.
4. Sélectionnez **Créer** pour créer le projet.

## <a name="install-nuget-packages"></a>Installer les packages NuGet

1. Cliquez sur **Outils** > **Gestionnaire de package NuGet** > **Console du Gestionnaire de package**.
2. Dans le volet **Console du Gestionnaire de package**, exécutez les commandes suivantes pour installer les packages. Pour plus d'informations, consultez [Package NuGet Microsoft.Azure.Management.Purview](https://www.nuget.org/packages/Microsoft.Azure.Management.Purview/).

    ```powershell
    Install-Package Microsoft.Azure.Management.Purview
    Install-Package Microsoft.Azure.Management.ResourceManager -IncludePrerelease
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

## <a name="create-a-purview-client"></a>Créer un client Purview

1. Ouvrez **Program.cs**, insérez les instructions suivantes pour ajouter des références aux espaces de noms.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Rest;
    using Microsoft.Rest.Serialization;
      using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.Purview;
      using Microsoft.Azure.Management.Purview.Models;
      using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

2. Ajoutez le code suivant à la méthode **Main** qui définit les variables. Remplacez les espaces réservés par vos valeurs. Pour obtenir la liste des régions Azure dans lesquelles Purview est actuellement disponible, recherchez **Azure Purview** et sélectionnez les régions qui vous intéressent dans la page suivante : [Disponibilité des produits par région](https://azure.microsoft.com/global-infrastructure/services/).

   ```csharp
   // Set variables
   string tenantID = "<your tenant ID>";
   string applicationId = "<your application ID>";
   string authenticationKey = "<your authentication key for the application>";
   string subscriptionId = "<your subscription ID where the data factory resides>";
   string resourceGroup = "<your resource group where the data factory resides>";
   string region = "<the location of your resource group>";
   string purviewAccountName = 
       "<specify the name of purview account to create. It must be globally unique.>";
   ```

3. Ajoutez le code suivant à la méthode **Main** qui crée une instance de la classe **PurviewManagementClient**. Vous devez utiliser cet objet pour créer un compte Purview.

   ```csharp
   // Authenticate and create a purview management client
   var context = new AuthenticationContext("https://login.windows.net/" + tenantID);
   ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
   AuthenticationResult result = context.AcquireTokenAsync(
   "https://management.azure.com/", cc).Result;
   ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
   var client = new PurviewManagementClient(cred)
   {
      SubscriptionId = subscriptionId           
   };
   ```

## <a name="create-a-purview-account"></a>Créer un compte Purview

Ajoutez le code suivant à la méthode **Main** qui crée un **compte Purview**.

```csharp
// Create a purview Account
Console.WriteLine("Creating Purview Account " + purviewAccountName + "...");
Account account = new Account()
{
Location = region,
Identity = new Identity(type: "SystemAssigned"),
Sku = new AccountSku(name: "Standard", capacity: 4)
};            
try
{
  client.Accounts.CreateOrUpdate(resourceGroup, purviewAccountName, account);
  Console.WriteLine(client.Accounts.Get(resourceGroup, purviewAccountName).ProvisioningState);                
}
catch (ErrorResponseModelException purviewException)
{
Console.WriteLine(purviewException.StackTrace);
  }
  Console.WriteLine(
    SafeJsonConvert.SerializeObject(account, client.SerializationSettings));
  while (client.Accounts.Get(resourceGroup, purviewAccountName).ProvisioningState ==
         "PendingCreation")
  {
    System.Threading.Thread.Sleep(1000);
  }
Console.WriteLine("\nPress any key to exit...");
Console.ReadKey();
```

## <a name="run-the-code"></a>Exécuter le code

Créez et démarrez l'application, puis vérifiez l'exécution.

La console affiche la progression de la création du compte Purview.

### <a name="sample-output"></a>Exemple de sortie

```json
Creating Purview Account testpurview...
Succeeded
{
  "sku": {
    "capacity": 4,
    "name": "Standard"
  },
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "southcentralus"
}

Press any key to exit...
```

## <a name="verify-the-output"></a>Vérifier la sortie

Accédez à la page **Comptes Purview** du [portail Azure](https://portal.azure.com) et vérifiez le compte créé à l'aide du code ci-dessus.

## <a name="delete-purview-account"></a>Supprimer un compte Purview

Pour supprimer par programmation un compte Purview, ajoutez les lignes de code suivantes au programme :

```csharp
Console.WriteLine("Deleting the Purview Account");
client.Accounts.Delete(resourceGroup, purviewAccountName);
```

## <a name="check-if-purview-account-name-is-available"></a>Vérifier la disponibilité d'un compte Purview

Pour vérifier la disponibilité d'un compte Purview, utilisez le code suivant :

```csharp
CheckNameAvailabilityRequest checkNameAvailabilityRequest = newCheckNameAvailabilityRequest()
{
    Name = purviewAccountName,
    Type =  "Microsoft.Purview/accounts"
};
Console.WriteLine("Check Purview account name");
Console.WriteLine(client.Accounts.CheckNameAvailability(checkNameAvailabilityRequest).NameAvailable);
```

Le code ci-dessus affiche « True » si le nom est disponible et « False » s'il n'est pas disponible.

## <a name="next-steps"></a>Étapes suivantes

Le code proposé dans ce tutoriel crée un compte Purview, supprime un compte Purview et vérifie la disponibilité du nom du compte Purview. Vous pouvez maintenant télécharger le kit de développement logiciel (SDK) .NET et découvrir d'autres actions du fournisseur de ressources que vous pouvez effectuer pour un compte Purview.

Lisez les articles suivants pour apprendre à naviguer dans Purview Studio, à créer une collection et à accorder l’accès à Purview.

* [Comment utiliser Purview Studio](use-purview-studio.md)
* [Création d'une collection](quickstart-create-collection.md)
* [Ajouter des utilisateurs à votre compte Azure Purview](catalog-permissions.md)
