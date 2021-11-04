---
title: 'Démarrage rapide : Utiliser Azure Cache pour Redis dans le .NET Framework'
description: Dans ce guide de démarrage rapide, apprenez à accéder au cache Azure pour Redis à partir de vos applications .NET
author: curib
ms.author: cauribeg
ms.service: cache
ms.devlang: dotnet
ms.topic: quickstart
ms.custom: devx-track-csharp, mvc
ms.date: 06/18/2020
ms.openlocfilehash: acc3fb68c7d03a8e456088401e87d6858e7e9970
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/03/2021
ms.locfileid: "131455394"
---
# <a name="quickstart-use-azure-cache-for-redis-in-net-framework"></a>Démarrage rapide : Utiliser Azure Cache pour Redis dans le .NET Framework

Dans ce guide de démarrage rapide, vous allez incorporer le cache Azure pour Redis dans une application .NET Framework pour avoir accès à un cache sécurisé et dédié accessible à partir de n’importe quelle application dans Azure. Vous utiliserez spécifiquement le client [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) avec du code C# dans une application console .NET.

## <a name="skip-to-the-code-on-github"></a>Passer au code sur GitHub

Si vous souhaitez passer directement au code, consultez le [démarrage rapide .NET Framework](https://github.com/Azure-Samples/azure-cache-redis-samples/tree/main/quickstart/dotnet) sur GitHub.

## <a name="prerequisites"></a>Prérequis

- Abonnement Azure : [créez-en un gratuitement](https://azure.microsoft.com/free/)
- [Visual Studio 2019](https://www.visualstudio.com/downloads/)
- [.NET Framework 4 ou ultérieur](https://dotnet.microsoft.com/download/dotnet-framework), qui est requis par le client StackExchange.Redis.

## <a name="create-a-cache"></a>Création d'un cache
[!INCLUDE [redis-cache-create](includes/redis-cache-create.md)]

[!INCLUDE [redis-cache-access-keys](includes/redis-cache-access-keys.md)]

Créez un fichier nommé *CacheSecrets.config* sur votre ordinateur, à un emplacement qui ne sera pas archivé avec le code source de votre exemple d’application. Pour ce guide de démarrage rapide, le fichier *CacheSecrets.config* se trouve à l’emplacement suivant : *C:\AppSecrets\CacheSecrets.config*.

Modifiez le fichier *CacheSecrets.config* et ajoutez le contenu suivant :

```xml
<appSettings>
    <add key="CacheConnection" value="<host-name>,abortConnect=false,ssl=true,allowAdmin=true,password=<access-key>"/>
</appSettings>
```

Remplacez `<host-name>` par le nom d’hôte de votre cache.

Remplacez `<access-key>` par la clé primaire de votre cache.


## <a name="create-a-console-app"></a>Créer une application console

Dans Visual Studio, sélectionnez **Fichier** > **Nouveau** > **Projet**.

Sélectionnez **Application console (.NET Framework)** et **Suivant** pour configurer votre application. Saisissez un **nom de projet**, vérifiez que **.NET Framework 4.6.1** ou une version ultérieure est sélectionné, puis sélectionnez **Créer** pour créer une application console.

<a name="configure-the-cache-clients"></a>

## <a name="configure-the-cache-client"></a>Configuration du client de cache

Dans cette section, vous allez configurer l’application console pour utiliser le client [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) pour .NET.

Dans Visual Studio, sélectionnez **Outils** > **Gestionnaire de package NuGet** > **Console du gestionnaire de package**, puis exécutez la commande suivante à partir de la fenêtre de la console du gestionnaire de package.

```powershell
Install-Package StackExchange.Redis
```

Une fois l’installation terminée, le client de cache *StackExchange.Redis* est disponible pour être utilisé avec votre projet.


## <a name="connect-to-the-cache"></a>Connexion au cache

Dans Visual Studio, ouvrez votre fichier *App.config* et mettez-le à jour pour inclure un attribut `appSettings` `file` référençant le fichier *CacheSecrets.config*.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
    </startup>

    <appSettings file="C:\AppSecrets\CacheSecrets.config"></appSettings>
</configuration>
```

Dans Explorateur de solutions, cliquez avec le bouton droit sur **Références** et sélectionnez **Ajouter une référence**. Ajoutez une référence à l’assembly **System.Configuration**.

Ajoutez les instructions `using` ci-après à *Program.cs* :

```csharp
using StackExchange.Redis;
using System.Configuration;
```

La connexion au cache Azure pour Redis est gérée par la classe `ConnectionMultiplexer`. Cette classe doit être partagée et réutilisée dans votre application cliente. Ne créez pas une nouvelle connexion pour chaque opération. 

Ne stockez jamais d’informations d’identification dans du code source. Pour garder cet exemple simple, j’utilise uniquement un fichier de configuration externe des secrets. Une meilleure approche serait d’utiliser [Azure Key Vault avec des certificats](/rest/api/keyvault/certificate-scenarios).

Dans *Program.cs*, ajoutez les membres suivants à la classe `Program` de votre application console :

```csharp
private static Lazy<ConnectionMultiplexer> lazyConnection = CreateConnection();

public static ConnectionMultiplexer Connection
{
    get
    {
        return lazyConnection.Value;
    }
}

private static Lazy<ConnectionMultiplexer> CreateConnection()
{
    return new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });
}
```

Cette approche pour partager une instance `ConnectionMultiplexer` dans votre application utilise une propriété statique qui renvoie une instance connectée. Ce code fournit une méthode thread-safe permettant d’initialiser une seule instance `ConnectionMultiplexer` connectée. `abortConnect` a la valeur false, ce qui signifie que l’appel réussit même si aucune connexion au cache Azure pour Redis n’est établie. Une fonctionnalité clé de `ConnectionMultiplexer` est qu’il restaure automatiquement la connectivité au cache une fois que le problème réseau ou d’autres causes sont résolus.

La valeur du appSetting *CacheConnection* est utilisé pour faire référence à la chaîne de connexion de cache à partir du portail Azure en tant que paramètre de mot de passe.

## <a name="handle-redisconnectionexception-and-socketexception-by-reconnecting"></a>Gérer RedisConnectionException et SocketException en rétablissant la connexion

Une bonne pratique recommandée lors de l’appel de méthodes sur `ConnectionMultiplexer` consiste à tenter de résoudre les exceptions `RedisConnectionException` et `SocketException` automatiquement en fermant la connexion et en la rétablissant.

Ajoutez les instructions `using` ci-après à *Program.cs* :

```csharp
using System.Net.Sockets;
using System.Threading;
```

Dans *Program.cs*, ajoutez les membres suivants à la classe `Program` :

```csharp
private static long _lastReconnectTicks = DateTimeOffset.MinValue.UtcTicks;
private static DateTimeOffset _firstErrorTime = DateTimeOffset.MinValue;
private static DateTimeOffset _previousErrorTime = DateTimeOffset.MinValue;
private static SemaphoreSlim _reconnectSemaphore = new SemaphoreSlim(initialCount: 1, maxCount: 1);
private static SemaphoreSlim _initSemaphore = new SemaphoreSlim(initialCount: 1, maxCount: 1);
private static ConnectionMultiplexer _connection;
private static bool _didInitialize = false;
// In general, let StackExchange.Redis handle most reconnects,
// so limit the frequency of how often ForceReconnect() will
// actually reconnect.
public static TimeSpan ReconnectMinInterval => TimeSpan.FromSeconds(60);
// If errors continue for longer than the below threshold, then the
// multiplexer seems to not be reconnecting, so ForceReconnect() will
// re-create the multiplexer.
public static TimeSpan ReconnectErrorThreshold => TimeSpan.FromSeconds(30);
public static TimeSpan RestartConnectionTimeout => TimeSpan.FromSeconds(15);
public static int RetryMaxAttempts => 5;
public static ConnectionMultiplexer Connection { get { return _connection; } }
private static async Task InitializeAsync()
{
    if (_didInitialize)
    {
        throw new InvalidOperationException("Cannot initialize more than once.");
    }
    _connection = await CreateConnectionAsync();
    _didInitialize = true;
}
// This method may return null if it fails to acquire the semaphore in time.
// Use the return value to update the "connection" field
private static async Task<ConnectionMultiplexer> CreateConnectionAsync()
{
    if (_connection != null)
    {
        // If we already have a good connection, let's re-use it
        return _connection;
    }
    try
    {
        await _initSemaphore.WaitAsync(RestartConnectionTimeout);
    }
    catch
    {
        // We failed to enter the semaphore in the given amount of time. Connection will either be null, or have a value that was created by another thread.
        return _connection;
    }
    // We entered the semaphore successfully.
    try
    {
        if (_connection != null)
        {
            // Another thread must have finished creating a new connection while we were waiting to enter the semaphore. Let's use it
            return _connection;
        }
        // Otherwise, we really need to create a new connection.
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return await ConnectionMultiplexer.ConnectAsync(cacheConnection);
    }
    finally
    {
        _initSemaphore.Release();
    }
}
private static async Task CloseConnectionAsync(ConnectionMultiplexer oldConnection)
{
    if (oldConnection == null)
    {
        return;
    }
    try
    {
        await oldConnection.CloseAsync();
    }
    catch (Exception)
    {
        // Ignore any errors from the oldConnection
    }
}
/// <summary>
/// Force a new ConnectionMultiplexer to be created.
/// NOTES:
///     1. Users of the ConnectionMultiplexer MUST handle ObjectDisposedExceptions, which can now happen as a result of calling ForceReconnectAsync().
///     2. Call ForceReconnectAsync() for RedisConnectionExceptions and RedisSocketExceptions. You can also call it for RedisTimeoutExceptions,
///         but only if you're using generous ReconnectMinInterval and ReconnectErrorThreshold. Otherwise, establishing new connections can cause
///         a cascade failure on a server that's timing out because it's already overloaded.
///     3. The code will:
///         a. wait to reconnect for at least the "ReconnectErrorThreshold" time of repeated errors before actually reconnecting
///         b. not reconnect more frequently than configured in "ReconnectMinInterval"
/// </summary>
public static async Task ForceReconnectAsync()
{
    var utcNow = DateTimeOffset.UtcNow;
    long previousTicks = Interlocked.Read(ref _lastReconnectTicks);
    var previousReconnectTime = new DateTimeOffset(previousTicks, TimeSpan.Zero);
    TimeSpan elapsedSinceLastReconnect = utcNow - previousReconnectTime;
    // If multiple threads call ForceReconnectAsync at the same time, we only want to honor one of them.
    if (elapsedSinceLastReconnect < ReconnectMinInterval)
    {
        return;
    }
    try
    {
        await _reconnectSemaphore.WaitAsync(RestartConnectionTimeout);
    }
    catch
    {
        // If we fail to enter the semaphore, then it is possible that another thread has already done so.
        // ForceReconnectAsync() can be retried while connectivity problems persist.
        return;
    }
    try
    {
        utcNow = DateTimeOffset.UtcNow;
        elapsedSinceLastReconnect = utcNow - previousReconnectTime;
        if (_firstErrorTime == DateTimeOffset.MinValue)
        {
            // We haven't seen an error since last reconnect, so set initial values.
            _firstErrorTime = utcNow;
            _previousErrorTime = utcNow;
            return;
        }
        if (elapsedSinceLastReconnect < ReconnectMinInterval)
        {
            return; // Some other thread made it through the check and the lock, so nothing to do.
        }
        TimeSpan elapsedSinceFirstError = utcNow - _firstErrorTime;
        TimeSpan elapsedSinceMostRecentError = utcNow - _previousErrorTime;
        bool shouldReconnect =
            elapsedSinceFirstError >= ReconnectErrorThreshold // Make sure we gave the multiplexer enough time to reconnect on its own if it could.
            && elapsedSinceMostRecentError <= ReconnectErrorThreshold; // Make sure we aren't working on stale data (e.g. if there was a gap in errors, don't reconnect yet).
        // Update the previousErrorTime timestamp to be now (e.g. this reconnect request).
        _previousErrorTime = utcNow;
        if (!shouldReconnect)
        {
            return;
        }
        _firstErrorTime = DateTimeOffset.MinValue;
        _previousErrorTime = DateTimeOffset.MinValue;
        ConnectionMultiplexer oldConnection = _connection;
        await CloseConnectionAsync(oldConnection);
        _connection = null;
        _connection = await CreateConnectionAsync();
        Interlocked.Exchange(ref _lastReconnectTicks, utcNow.UtcTicks);
    }
    finally
    {
        _reconnectSemaphore.Release();
    }
}
// In real applications, consider using a framework such as
// Polly to make it easier to customize the retry approach.
private static async Task<T> BasicRetryAsync<T>(Func<T> func)
{
    int reconnectRetry = 0;
    int disposedRetry = 0;
    while (true)
    {
        try
        {
            return func();
        }
        catch (Exception ex) when (ex is RedisConnectionException || ex is SocketException)
        {
            reconnectRetry++;
            if (reconnectRetry > RetryMaxAttempts)
                throw;
            await ForceReconnectAsync();
        }
        catch (ObjectDisposedException)
        {
            disposedRetry++;
            if (disposedRetry > RetryMaxAttempts)
                throw;
        }
    }
}
public static Task<IDatabase> GetDatabaseAsync()
{
    return BasicRetryAsync(() => Connection.GetDatabase());
}
public static Task<System.Net.EndPoint[]> GetEndPointsAsync()
{
    return BasicRetryAsync(() => Connection.GetEndPoints());
}
public static Task<IServer> GetServerAsync(string host, int port)
{
    return BasicRetryAsync(() => Connection.GetServer(host, port));
}
```

## <a name="executing-cache-commands"></a>Exécution des commandes de cache

Ajoutez le code suivant pour la procédure `Main` de la classe `Program` pour votre application console :

```csharp
static void Main(string[] args)
{
    IDatabase cache = GetDatabase();

    // Perform cache operations using the cache object...

    // Simple PING command
    string cacheCommand = "PING";
    Console.WriteLine("\nCache command  : " + cacheCommand);
    Console.WriteLine("Cache response : " + cache.Execute(cacheCommand).ToString());

    // Simple get and put of integral data types into the cache
    cacheCommand = "GET Message";
    Console.WriteLine("\nCache command  : " + cacheCommand + " or StringGet()");
    Console.WriteLine("Cache response : " + cache.StringGet("Message").ToString());

    cacheCommand = "SET Message \"Hello! The cache is working from a .NET console app!\"";
    Console.WriteLine("\nCache command  : " + cacheCommand + " or StringSet()");
    Console.WriteLine("Cache response : " + cache.StringSet("Message", "Hello! The cache is working from a .NET console app!").ToString());

    // Demonstrate "SET Message" executed as expected...
    cacheCommand = "GET Message";
    Console.WriteLine("\nCache command  : " + cacheCommand + " or StringGet()");
    Console.WriteLine("Cache response : " + cache.StringGet("Message").ToString());

    // Get the client list, useful to see if connection list is growing...
    // Note that this requires allowAdmin=true in the connection string
    cacheCommand = "CLIENT LIST";
    Console.WriteLine("\nCache command  : " + cacheCommand);
    var endpoint = (System.Net.DnsEndPoint)GetEndPoints()[0];
    IServer server = GetServer(endpoint.Host, endpoint.Port);
    ClientInfo[] clients = server.ClientList();

    Console.WriteLine("Cache response :");
    foreach (ClientInfo client in clients)
    {
        Console.WriteLine(client.Raw);
    }

    CloseConnection(lazyConnection);
}
```

Le cache Azure pour Redis dispose d’un nombre configurable de bases de données (16 par défaut) pouvant être utilisées pour séparer de manière logique les données dans un cache Azure pour Redis. Le code se connecte à la base de données par défaut, DB 0. Pour plus d’informations, consultez les sections [What are Redis databases?](cache-development-faq.yml#what-are-redis-databases-) (Que sont les bases de données Redis ?) et [Configuration du serveur Redis par défaut](cache-configure.md#default-redis-server-configuration).

Les éléments de cache peuvent être stockés et extraits en utilisant les méthodes `StringSet` et `StringGet`.

Redis stocke la plupart des données sous la forme de chaînes Redis, mais ces chaînes peuvent contenir de nombreux types de données, notamment des données binaires sérialisées, qui peuvent être utilisées lors du stockage d'objets .NET dans le cache.

Appuyez sur **Ctrl+F5** pour générer et exécuter l’application console.

Dans l’exemple ci-dessous, vous pouvez voir que la clé `Message` présentait auparavant une valeur mise en cache, qui avait été définie à l’aide de la console Redis du portail Azure. L’application a mis à jour cette valeur mise en cache. Elle a également exécuté les commandes `PING` et `CLIENT LIST`.

![Application console partielle](./media/cache-dotnet-how-to-use-azure-redis-cache/cache-console-app-partial.png)


## <a name="work-with-net-objects-in-the-cache"></a>Utilisation des objets .NET dans le cache

Le cache Azure pour Redis peut mettre en cache des objets .NET et des types de données primitifs, mais avant qu’un objet .NET puisse être mis en cache, il doit être sérialisé. La sérialisation d’objet .NET échoit au développeur d’applications, qui a toute latitude pour choisir le sérialiseur.

Une méthode simple pour sérialiser des objets consiste à utiliser les méthodes de sérialisation `JsonConvert` dans [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/) et à sérialiser vers et à partir de JSON. Dans cette section, vous allez ajouter un objet .NET dans le cache.

Dans Visual Studio, sélectionnez **Outils** > **Gestionnaire de package NuGet** > **Console du gestionnaire de package**, puis exécutez la commande suivante à partir de la fenêtre de la console du gestionnaire de package.

```powershell
Install-Package Newtonsoft.Json
```

Ajoutez l’instruction `using` suivante au début de *Program.cs* :

```csharp
using Newtonsoft.Json;
```

Ajoutez la définition de classe `Employee` suivante à *Program.cs* :

```csharp
class Employee
{
    public string Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }

    public Employee(string employeeId, string name, int age)
    {
        Id = employeeId;
        Name = name;
        Age = age;
    }
}
```

Au bas de la procédure `Main()` dans *Program.cs* et avant l’appel à `CloseConnection()`, ajoutez les lignes de code suivantes à mettre en cache et récupérez un objet .NET sérialisé :

```csharp
    // Store .NET object to cache
    Employee e007 = new Employee("007", "Davide Columbo", 100);
    Console.WriteLine("Cache response from storing Employee .NET object : " + 
    cache.StringSet("e007", JsonConvert.SerializeObject(e007)));

    // Retrieve .NET object from cache
    Employee e007FromCache = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e007"));
    Console.WriteLine("Deserialized Employee .NET object :\n");
    Console.WriteLine("\tEmployee.Name : " + e007FromCache.Name);
    Console.WriteLine("\tEmployee.Id   : " + e007FromCache.Id);
    Console.WriteLine("\tEmployee.Age  : " + e007FromCache.Age + "\n");
```

Appuyez sur **Ctrl + F5** pour générer et exécuter l’application console pour tester la sérialisation des objets .NET. 

![Application console terminée](./media/cache-dotnet-how-to-use-azure-redis-cache/cache-console-app-complete.png)


## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous envisagez d’exécuter le didacticiel suivant, vous pouvez conserver les ressources créées dans le cadre de ce guide de démarrage rapide afin de les réutiliser.

Sinon, si l’exemple d’application de démarrage rapide était votre dernière opération, vous pouvez supprimer les ressources Azure créées dans ce démarrage rapide afin d’éviter tout frais. 

> [!IMPORTANT]
> La suppression d’un groupe de ressources est définitive ; le groupe de ressources et l’ensemble des ressources qu’il contient sont supprimés de manière permanente. Veillez à ne pas supprimer accidentellement des ressources ou un groupe de ressources incorrects. Si vous avez créé les ressources pour l'hébergement de cet exemple dans un groupe de ressources existant contenant des ressources que vous souhaitez conserver, vous pouvez supprimer chaque ressource individuellement sur la gauche, au lieu de supprimer l'intégralité du groupe de ressources.
>

Connectez-vous au [portail Azure](https://portal.azure.com), puis sélectionnez **Groupes de ressources**.

Dans la zone de texte **Filtrer par nom.** , saisissez le nom de votre groupe de ressources. Les instructions de cet article ont utilisé un groupe de ressources nommé *TestResources*. Sur votre groupe de ressources dans la liste des résultats, cliquez sur **...** , puis sur **Supprimer le groupe de ressources**.

![DELETE](./media/cache-dotnet-how-to-use-azure-redis-cache/cache-delete-resource-group.png)

Il vous sera demandé de confirmer la suppression du groupe de ressources. Saisissez le nom de votre groupe de ressources à confirmer, puis sélectionnez **Supprimer**.

Après quelques instants, le groupe de ressources et toutes les ressources qu’il contient sont supprimés.



<a name="next-steps"></a>

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez appris à utiliser le cache Azure pour Redis à partir d’une application .NET. Passez au guide de démarrage rapide suivant pour utiliser le cache Azure pour Redis avec une application web ASP.NET.

> [!div class="nextstepaction"]
> [Créer une application web ASP.NET qui utilise un cache Azure pour Redis.](./cache-web-app-howto.md)

Vous souhaitez optimiser et réduire vos coûts de cloud ?

> [!div class="nextstepaction"]
> [Démarrer l’analyse des coûts avec Cost Management](../cost-management-billing/costs/quick-acm-cost-analysis.md?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)
