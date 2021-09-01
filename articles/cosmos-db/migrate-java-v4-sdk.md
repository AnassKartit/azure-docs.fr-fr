---
title: Migration d’une application de façon à utiliser le kit SDK Java Azure Cosmos DB v4 (com.azure.cosmos)
description: Découvrez comment mettre à niveau votre application Java existante des anciens kits SDK Java Azure Cosmos DB vers la version 4.0 (package com.azure.cosmos), plus récente, pour l’API (SQL) Core.
author: anfeldma-ms
ms.custom: devx-track-java
ms.author: anfeldma
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: how-to
ms.date: 06/15/2021
ms.reviewer: sngun
ms.openlocfilehash: 4e08834237d396ae1a2ae3990b3e8d21f75ffaca
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2021
ms.locfileid: "114290486"
---
# <a name="migrate-your-application-to-use-the-azure-cosmos-db-java-sdk-v4"></a>Migration d’une application de façon à utiliser le kit SDK Java Azure Cosmos DB v4
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

> [!IMPORTANT]  
> Pour plus d’informations sur le kit SDK Java Azure Cosmos DB v4, consultez les [Notes de publication](sql-api-sdk-java-v4.md), le [Référentiel Maven](https://mvnrepository.com/artifact/com.azure/azure-cosmos), les [Conseils en matière de performances](performance-tips-java-sdk-v4-sql.md) et le [Guide de résolution des problèmes](troubleshoot-java-sdk-v4-sql.md) correspondants.
>

> [!IMPORTANT]  
> Le kit SDK Java v4 Azure Cosmos DB présentant jusqu’à 20% de débit amélioré, le mode direct basé sur TCP et la prise en charge des dernières fonctionnalités du service back-end, nous vous recommandons d’adopter v4 dès que vous en aurez la possibilité. Poursuivez votre lecture pour en savoir plus.
>

Procédez à une mise à jour vers le kit de développement logiciel (SDK) Java Azure Cosmos DB le plus récent pour tirer le meilleur parti de l’offre Azure Cosmos DB : un service de base de données non relationnelle géré avec des performances compétitives, une disponibilité de 99,999 %, une gouvernance des ressources de type « un-à-plusieurs » et bien plus encore. Cet article explique comment mettre à niveau une application Java existante qui utilise un ancien kit SDK Java Azure Cosmos DB vers la version 4.0, plus récente, pour l’API (SQL) Core. Le kit SDK Java Azure Cosmos DB v4 correspond au package `com.azure.cosmos`. Vous pouvez suivre les instructions de ce document si vous migrez votre application à partir de l’un des kits SDK Java Azure Cosmos DB suivants : 

* Kit SDK Java Sync 2.x.x
* Kit SDK Java Async 2.x.x
* Kit SDK Java 3.x.x

## <a name="azure-cosmos-db-java-sdks-and-package-mappings"></a>Correspondance entre les kits SDK Java Azure Cosmos DB et les packages

Le tableau suivant présente les différents kits SDK Java Azure Cosmos DB, le nom du package et les informations de version :

| Kit de développement logiciel (SDK) Java| Date de sortie | API groupées   | JAR Maven  | Nom du package Java  |Référence API   | Notes de publication  | Date de mise hors service |
|-------|------|-----------|-----------|--------------|-------------|---------------------------|--------|
| Async 2.x.x  | Juin 2018    | Async(RxJava)  | `com.microsoft.azure::azure-cosmosdb` | `com.microsoft.azure.cosmosdb.rx` | [API](https://azure.github.io/azure-cosmosdb-java/2.0.0/) | [Notes de publication](sql-api-sdk-async-java.md) | - |
| Sync 2.x.x     | Septembre 2018    | Synchronisation   | `com.microsoft.azure::azure-documentdb` | `com.microsoft.azure.cosmosdb` | [API](https://azure.github.io/azure-cosmosdb-java/2.0.0/) | [Notes de publication](sql-api-sdk-java.md)  | 29 février 2024 |
| 3.x.x    | Juillet 2019    | Async(Reactor)/Sync  | `com.microsoft.azure::azure-cosmos`  | `com.azure.data.cosmos` | [API](https://azure.github.io/azure-cosmosdb-java/3.0.0/) | - | - |
| 4.0   | Juin 2020   | Async(Reactor)/Sync  | `com.azure::azure-cosmos` | `com.azure.cosmos`   | [API](/java/api/overview/azure/cosmosdb) | - | - |

## <a name="sdk-level-implementation-changes"></a>Modifications apportées à l’implémentation au niveau du kit SDK

Voici les principales différences d’implémentation entre les kits SDK :

### <a name="rxjava-is-replaced-with-reactor-in-azure-cosmos-db-java-sdk-versions-3xx-and-40"></a>RxJava est remplacé par Reactor dans les versions 3.x.x et 4.0 du kit SDK Java Azure Cosmos DB

Si vous ne connaissez pas la programmation asynchrone ni la programmation réactive, consultez [Guide du modèle Reactor](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/main/reactor-pattern-guide.md) pour une présentation de la programmation asynchrone et de Project Reactor. Ce guide peut être utile si vous utilisiez le kit SDK Java Azure Cosmos DB Sync 2.x.x ou l’API Sync du kit SDK Java Azure Cosmos DB 3.x.x par le passé.

Si vous utilisiez le kit SDK Java Azure Cosmos DB Async 2.x.x et que vous envisagez de migrer vers la version 4.0 du kit SDK, consultez le [Guide comparatif Reactor-RxJava](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/main/reactor-rxjava-guide.md) pour obtenir des conseils sur la conversion du code RxJava vers Reactor.

### <a name="azure-cosmos-db-java-sdk-v4-has-direct-connectivity-mode-in-both-async-and-sync-apis"></a>Le kit SDK Java Azure Cosmos DB v4 comporte un mode de connectivité directe dans les API Async et Sync

Si vous utilisiez le kit SDK Java Azure Cosmos DB Sync 2.x.x, sachez que le mode de connexion directe sur TCP (et non HTTP) est implémenté dans le kit SDK Java Azure Cosmos DB 4.0 pour les API Async et Sync.

## <a name="api-level-changes"></a>Modifications apportées au niveau de l’API

Les modifications apportées au niveau de l’API dans le kit SDK Java Azure Cosmos DB 4.x.x par rapport aux anciens kits SDK (3.x.x, Async 2.x.x et Sync 2.x.x) sont les suivantes :

:::image type="content" source="./media/migrate-java-v4-sdk/java-sdk-naming-conventions.png" alt-text="Conventions d’affectation de noms des SDK Java Azure Cosmos DB":::

* Les versions 3.x et 4.0 du kit SDK Java Azure Cosmos DB font référence ainsi aux ressources clientes : `Cosmos<resourceName>`. Par exemple, `CosmosClient`, `CosmosDatabase`, `CosmosContainer`. À l’inverse, dans la version 2.x.x, les kits SDK Java Azure Cosmos DB ne présentent pas de modèle uniforme d’affectation de noms.

* Les versions 3.x et 4.0 du kit SDK Java Azure Cosmos DB proposent à la fois des API Sync et Async.

  * **Java SDK 4.0** : toutes les classes appartiennent à l’API Sync, sauf si le nom de la classe comporte `Async` après `Cosmos`.

  * **Java SDK 3.x.x** : toutes les classes appartiennent à l’API Async, sauf si le nom de la classe comporte `Async` après `Cosmos`.

  * **Kit SDK Java Async 2.x.x** : les noms de classe sont similaires à ceux du kit SDK Java 2.x.x, à ceci près que le nom commence par *Async*.

### <a name="hierarchical-api-structure"></a>Structure d’API hiérarchique

Les versions 4.0 et 3.x.x du kit SDK Java Azure Cosmos DB introduisent une structure d’API hiérarchique qui organise les clients, les bases de données et les conteneurs de manière imbriquée, comme l’illustre l’extrait de code du kit SDK 4.0 suivant :

```java
CosmosContainer container = client.getDatabase("MyDatabaseName").getContainer("MyContainerName");

```

Dans la version 2.x.x du kit SDK Java Azure Cosmos DB, toutes les opérations effectuées sur les ressources et les documents passent par l’instance cliente.

### <a name="representing-documents"></a>Représentation des documents

Dans la version 4.0 du kit SDK Java Azure Cosmos DB, CustomPOJO et `JsonNodes` constituent les deux options permettant de lire et d’écrire les documents à partir d’Azure Cosmos DB.

Dans la version 3.x.x du kit SDK Java Azure Cosmos DB, l’objet `CosmosItemProperties` est exposé par l’API publique et traité comme une représentation de document. Cette classe n’est plus exposée publiquement dans la version 4.0.

### <a name="imports"></a>Importations

* Les packages du kit SDK Java Azure Cosmos DB 4.0 commencent par `com.azure.cosmos`.
* Les packages du kit SDK Java Azure Cosmos DB 3.x.x commencent par `com.azure.data.cosmos`.
* Les packages de l’APO Sync du Kit de développement logiciel (SDK) Java Azure Cosmos DB 2.x.x commencent par `com.microsoft.azure.documentdb`.

* Le kit SDK Java Azure Cosmos DB 4.0 place plusieurs classes dans un package imbriqué `com.azure.cosmos.models`. Exemples de packages :

  * `CosmosContainerResponse`
  * `CosmosDatabaseResponse`
  * `CosmosItemResponse`
  * L’équivalent API Async de tous les packages ci-dessus
  * `CosmosContainerProperties`
  * `FeedOptions`
  * `PartitionKey`
  * `IndexingPolicy`
  * `IndexingMode`

### <a name="accessors"></a>Accesseurs

La version 4.0 du kit SDK Java Azure Cosmos DB expose les méthodes `get` et `set` pour accéder aux membres de l’instance. Par exemple, l’instance de `CosmosContainer` comporte les méthodes `container.getId()` et `container.setId()`.

À l’inverse, la version 3.x.x du kit SDK Java Azure Cosmos DB expose une interface Fluent. Par exemple, une instance de `CosmosSyncContainer` comporte `container.id()`, qui est surchargée pour obtenir ou définir la valeur `id`.

## <a name="code-snippet-comparisons"></a>Comparaison d’extraits de code

### <a name="create-resources"></a>Créer des ressources

L’extrait de code suivant montre les différences dans la façon dont les ressources sont créées entre les API Async 4.0, 3.x.x, Sync 2.x.x et les API ASync 2.x.x :

# <a name="java-sdk-40-async-api"></a>[API Async kit SDK Java 4.0](#tab/java-v4-async)

[!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=MigrateJavaSDKv4ResourceAsync)]

# <a name="java-sdk-3xx-async-api"></a>[API Async kit SDK Java 3.x.x](#tab/java-v3-async)

```java
ConnectionPolicy defaultPolicy = ConnectionPolicy.defaultPolicy();
//  Setting the preferred location to Cosmos DB Account region
defaultPolicy.preferredLocations(Lists.newArrayList("Your Account Location"));

//  Create async client
//  <CreateAsyncClient>
client = new CosmosClientBuilder()
        .endpoint("your.hostname")
        .key("yourmasterkey")
        .connectionPolicy(defaultPolicy)
        .consistencyLevel(ConsistencyLevel.EVENTUAL)
        .build();

// Create database with specified name
client.createDatabaseIfNotExists("YourDatabaseName")
      .flatMap(databaseResponse -> {
        database = databaseResponse.database();
        // Container properties - name and partition key
        CosmosContainerProperties containerProperties = 
            new CosmosContainerProperties("YourContainerName", "/id");
        // Create container with specified properties & provisioned throughput
        return database.createContainerIfNotExists(containerProperties, 400);
    }).flatMap(containerResponse -> {
        container = containerResponse.container();
        return Mono.empty();
}).subscribe();
```

# <a name="java-sdk-2xx-sync-api"></a>[API Sync kit SDK Java 2.x.x](#tab/java-v2-sync)

```java
ConnectionPolicy defaultPolicy = ConnectionPolicy.GetDefault();
//  Setting the preferred location to Cosmos DB Account region
defaultPolicy.setPreferredLocations(Lists.newArrayList("Your Account Location"));

//  Create document client
//  <CreateDocumentClient>
client = new DocumentClient("your.hostname", "your.masterkey", defaultPolicy, ConsistencyLevel.Eventual)

// Create database with specified name
Database databaseDefinition = new Database();
databaseDefinition.setId("YourDatabaseName");
ResourceResponse<Database> databaseResourceResponse = client.createDatabase(databaseDefinition, new RequestOptions());

// Read database with specified name
String databaseLink = "dbs/YourDatabaseName";
databaseResourceResponse = client.readDatabase(databaseLink, new RequestOptions());
Database database = databaseResourceResponse.getResource();

// Create container with specified name
DocumentCollection documentCollection = new DocumentCollection();
documentCollection.setId("YourContainerName");
documentCollection = client.createCollection(database.getSelfLink(), documentCollection, new RequestOptions()).getResource();
```

# <a name="java-sdk-2xx-async-api"></a>[API Async kit SDK Java 2.x.x](#tab/java-v2-async)

```java
// Create Async client.
// Building an async client is still a sync operation.
AsyncDocumentClient client = new Builder()
    .withServiceEndpoint("your.hostname")
    .withMasterKeyOrResourceToken("yourmasterkey")
    .withConsistencyLevel(ConsistencyLevel.Eventual)
    .build();
// Create database with specified name
Database database = new Database();
database.setId("YourDatabaseName");
client.createDatabase(database, new RequestOptions())
      .flatMap(databaseResponse -> {
          // Collection properties - name and partition key
          DocumentCollection documentCollection = new DocumentCollection();
          documentCollection.setId("YourContainerName");
          documentCollection.setPartitionKey(new PartitionKeyDefinition("/id"));
          // Create collection
          return client.createCollection(databaseResponse.getResource().getSelfLink(), documentCollection, new RequestOptions());
}).subscribe();

```

---

### <a name="item-operations"></a>Opérations d'élément

L’extrait de code suivant montre les différences dans la façon dont les opérations d’élément sont effectuées entre les API Async 4.0, 3.x.x, Sync 2.x.x et les API Async 2.x.x :

# <a name="java-sdk-40-async-api"></a>[API Async kit SDK Java 4.0](#tab/java-v4-async)

[!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=MigrateItemOpsAsync)]

# <a name="java-sdk-3xx-async-api"></a>[API Async kit SDK Java 3.x.x](#tab/java-v3-async)

```java
// Container is created. Generate many docs to insert.
int number_of_docs = 50000;
ArrayList<JsonNode> docs = generateManyDocs(number_of_docs);

// Insert many docs into container...
Flux.fromIterable(docs)
    .flatMap(doc -> container.createItem(doc))
    .subscribe(); // ...Subscribing triggers stream execution.
```

# <a name="java-sdk-2xx-sync-api"></a>[API Sync kit SDK Java 2.x.x](#tab/java-v2-sync)

```java
//  Container is created. Generate documents to insert.
Document document = new Document();
document.setId("YourDocumentId");
ResourceResponse<Document> documentResourceResponse = client.createDocument(documentCollection.getSelfLink(), document,
    new RequestOptions(), true);
Document responseDocument = documentResourceResponse.getResource();
```

# <a name="java-sdk-2xx-async-api"></a>[API Async kit SDK Java 2.x.x](#tab/java-v2-async)

```java
// Collection is created. Generate many docs to insert.
int number_of_docs = 50000;
ArrayList<Document> docs = generateManyDocs(number_of_docs);
// Insert many docs into collection...
Observable.from(docs)
    .flatMap(doc -> client.createDocument(createdCollection.getSelfLink(), doc, new RequestOptions(), false))
    .subscribe(); // ...Subscribing triggers stream execution.
```

---

### <a name="indexing"></a>Indexation

L’extrait de code suivant montre les différences dans la façon dont l’indexation est créée entre les API Async 4.0, 3.x.x, Sync 2.x.x et les API Async 2.x.x :

# <a name="java-sdk-40-async-api"></a>[API Async kit SDK Java 4.0](#tab/java-v4-async)

[!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=MigrateIndexingAsync)]

# <a name="java-sdk-3xx-async-api"></a>[API Async kit SDK Java 3.x.x](#tab/java-v3-async)

```java
CosmosContainerProperties containerProperties = new CosmosContainerProperties(containerName, "/lastName");

// Custom indexing policy
IndexingPolicy indexingPolicy = new IndexingPolicy();
indexingPolicy.setIndexingMode(IndexingMode.CONSISTENT); //To turn indexing off set IndexingMode.NONE

// Included paths
List<IncludedPath> includedPaths = new ArrayList<>();
IncludedPath includedPath = new IncludedPath();
includedPath.path("/*");
includedPaths.add(includedPath);
indexingPolicy.includedPaths(includedPaths);

// Excluded paths
List<ExcludedPath> excludedPaths = new ArrayList<>();
ExcludedPath excludedPath = new ExcludedPath();
excludedPath.path("/name/*");
excludedPaths.add(excludedPath);
indexingPolicy.excludedPaths(excludedPaths);

containerProperties.indexingPolicy(indexingPolicy);

CosmosContainer containerIfNotExists = database.createContainerIfNotExists(containerProperties, 400)
                                               .block()
                                               .container();
```

# <a name="java-sdk-2xx-sync-api"></a>[API Sync kit SDK Java 2.x.x](#tab/java-v2-sync)

```java
// Custom indexing policy
IndexingPolicy indexingPolicy = new IndexingPolicy();
indexingPolicy.setIndexingMode(IndexingMode.Consistent); //To turn indexing off set IndexingMode.NONE

// Included paths
List<IncludedPath> includedPaths = new ArrayList<>();
IncludedPath includedPath = new IncludedPath();
includedPath.setPath("/*");
includedPaths.add(includedPath);
indexingPolicy.setIncludedPaths(includedPaths);

// Excluded paths
List<ExcludedPath> excludedPaths = new ArrayList<>();
ExcludedPath excludedPath = new ExcludedPath();
excludedPath.setPath("/name/*");
excludedPaths.add(excludedPath);
indexingPolicy.setExcludedPaths(excludedPaths);

// Create container with specified name and indexing policy
DocumentCollection documentCollection = new DocumentCollection();
documentCollection.setId("YourContainerName");
documentCollection.setIndexingPolicy(indexingPolicy);
documentCollection = client.createCollection(database.getSelfLink(), documentCollection, new RequestOptions()).getResource();
```

# <a name="java-sdk-2xx-async-api"></a>[API Async kit SDK Java 2.x.x](#tab/java-v2-async)

```java
// Custom indexing policy
IndexingPolicy indexingPolicy = new IndexingPolicy();
indexingPolicy.setIndexingMode(IndexingMode.Consistent); //To turn indexing off set IndexingMode.None
// Included paths
List<IncludedPath> includedPaths = new ArrayList<>();
IncludedPath includedPath = new IncludedPath();
includedPath.setPath("/*");
includedPaths.add(includedPath);
indexingPolicy.setIncludedPaths(includedPaths);
// Excluded paths
List<ExcludedPath> excludedPaths = new ArrayList<>();
ExcludedPath excludedPath = new ExcludedPath();
excludedPath.setPath("/name/*");
excludedPaths.add(excludedPath);
indexingPolicy.setExcludedPaths(excludedPaths);
// Create container with specified name and indexing policy
DocumentCollection documentCollection = new DocumentCollection();
documentCollection.setId("YourContainerName");
documentCollection.setIndexingPolicy(indexingPolicy);
client.createCollection(database.getSelfLink(), documentCollection, new RequestOptions()).subscribe();
```

---

### <a name="stored-procedures"></a>Procédures stockées

L’extrait de code suivant montre les différences dans la façon dont les procédures stockées sont créées entre les API Async 4.0, 3.x.x, Sync 2.x.x et les API Async 2.x.x :

# <a name="java-sdk-40-async-api"></a>[API Async kit SDK Java 4.0](#tab/java-v4-async)

[!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=MigrateSprocAsync)]

# <a name="java-sdk-3xx-async-api"></a>[API Async kit SDK Java 3.x.x](#tab/java-v3-async)

```java
logger.info("Creating stored procedure...\n");

sprocId = "createMyDocument";
String sprocBody = "function createMyDocument() {\n" +
        "var documentToCreate = {\"id\":\"test_doc\"}\n" +
        "var context = getContext();\n" +
        "var collection = context.getCollection();\n" +
        "var accepted = collection.createDocument(collection.getSelfLink(), documentToCreate,\n" +
        "    function (err, documentCreated) {\n" +
        "if (err) throw new Error('Error' + err.message);\n" +
        "context.getResponse().setBody(documentCreated.id)\n" +
        "});\n" +
        "if (!accepted) return;\n" +
        "}";
CosmosStoredProcedureProperties storedProcedureDef = new CosmosStoredProcedureProperties(sprocId, sprocBody);
container.getScripts()
        .createStoredProcedure(storedProcedureDef,
                new CosmosStoredProcedureRequestOptions()).block();

// ...

logger.info(String.format("Executing stored procedure %s...\n\n", sprocId));

CosmosStoredProcedureRequestOptions options = new CosmosStoredProcedureRequestOptions();
options.partitionKey(new PartitionKey("test_doc"));

container.getScripts()
        .getStoredProcedure(sprocId)
        .execute(null, options)
        .flatMap(executeResponse -> {
            logger.info(String.format("Stored procedure %s returned %s (HTTP %d), at cost %.3f RU.\n",
                    sprocId,
                    executeResponse.responseAsString(),
                    executeResponse.statusCode(),
                    executeResponse.requestCharge()));
            return Mono.empty();
        }).block();
```

# <a name="java-sdk-2xx-sync-api"></a>[API Sync kit SDK Java 2.x.x](#tab/java-v2-sync)

```java
logger.info("Creating stored procedure...\n");

String sprocId = "createMyDocument";
String sprocBody = "function createMyDocument() {\n" +
    "var documentToCreate = {\"id\":\"test_doc\"}\n" +
    "var context = getContext();\n" +
    "var collection = context.getCollection();\n" +
    "var accepted = collection.createDocument(collection.getSelfLink(), documentToCreate,\n" +
    "    function (err, documentCreated) {\n" +
    "if (err) throw new Error('Error' + err.message);\n" +
    "context.getResponse().setBody(documentCreated.id)\n" +
    "});\n" +
    "if (!accepted) return;\n" +
    "}";
StoredProcedure storedProcedureDef = new StoredProcedure();
storedProcedureDef.setId(sprocId);
storedProcedureDef.setBody(sprocBody);
StoredProcedure storedProcedure = client.createStoredProcedure(documentCollection.getSelfLink(), storedProcedureDef, new RequestOptions())
                                        .getResource();

// ...

logger.info(String.format("Executing stored procedure %s...\n\n", sprocId));

RequestOptions options = new RequestOptions();
options.setPartitionKey(new PartitionKey("test_doc"));

StoredProcedureResponse storedProcedureResponse =
    client.executeStoredProcedure(storedProcedure.getSelfLink(), options, null);
logger.info(String.format("Stored procedure %s returned %s (HTTP %d), at cost %.3f RU.\n",
    sprocId,
    storedProcedureResponse.getResponseAsString(),
    storedProcedureResponse.getStatusCode(),
    storedProcedureResponse.getRequestCharge()));
```

# <a name="java-sdk-2xx-async-api"></a>[API Async kit SDK Java 2.x.x](#tab/java-v2-async)

```java
logger.info("Creating stored procedure...\n");
String sprocId = "createMyDocument";
String sprocBody = "function createMyDocument() {\n" +
    "var documentToCreate = {\"id\":\"test_doc\"}\n" +
    "var context = getContext();\n" +
    "var collection = context.getCollection();\n" +
    "var accepted = collection.createDocument(collection.getSelfLink(), documentToCreate,\n" +
    "    function (err, documentCreated) {\n" +
    "if (err) throw new Error('Error' + err.message);\n" +
    "context.getResponse().setBody(documentCreated.id)\n" +
    "});\n" +
    "if (!accepted) return;\n" +
    "}";
StoredProcedure storedProcedureDef = new StoredProcedure();
storedProcedureDef.setId(sprocId);
storedProcedureDef.setBody(sprocBody);
StoredProcedure storedProcedure = client
    .createStoredProcedure(documentCollection.getSelfLink(), storedProcedureDef, new RequestOptions())
    .toBlocking()
    .single()
    .getResource();
// ...
logger.info(String.format("Executing stored procedure %s...\n\n", sprocId));
RequestOptions options = new RequestOptions();
options.setPartitionKey(new PartitionKey("test_doc"));
StoredProcedureResponse storedProcedureResponse =
    client.executeStoredProcedure(storedProcedure.getSelfLink(), options, null)
    .toBlocking().single();
logger.info(String.format("Stored procedure %s returned %s (HTTP %d), at cost %.3f RU.\n",
    sprocId,
    storedProcedureResponse.getResponseAsString(),
    storedProcedureResponse.getStatusCode(),
    storedProcedureResponse.getRequestCharge()));
```

---

### <a name="change-feed"></a>Modifier le flux

L’extrait de code suivant montre les différences dans la façon dont les opérations de flux de modification sont exécutées entre les API Async 4.0 et 3.x.x :

# <a name="java-sdk-40-async-api"></a>[API Async kit SDK Java 4.0](#tab/java-v4-async)

[!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=MigrateCFAsync)]

# <a name="java-sdk-3xx-async-api"></a>[API Async kit SDK Java 3.x.x](#tab/java-v3-async)

```java
ChangeFeedProcessor changeFeedProcessorInstance = 
ChangeFeedProcessor.Builder()
        .hostName(hostName)
        .feedContainer(feedContainer)
        .leaseContainer(leaseContainer)
        .handleChanges((List<CosmosItemProperties> docs) -> {
            logger.info("--->setHandleChanges() START");

            for (CosmosItemProperties document : docs) {
                try {

                    // You are given the document as a CosmosItemProperties instance which you may
                    // cast to the desired type.
                    CustomPOJO pojo_doc = document.getObject(CustomPOJO.class);
                    logger.info("----=>id: " + pojo_doc.id());

                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
            logger.info("--->handleChanges() END");

        })
        .build();

// ...

 changeFeedProcessorInstance.start()
                            .subscribeOn(Schedulers.elastic())
                            .subscribe();
```

# <a name="java-sdk-2xx-sync-api"></a>[API Sync kit SDK Java 2.x.x](#tab/java-v2-sync)

* Cette fonctionnalité n’est pas prise en charge en tant que synchronisation du Kit de développement logiciel (SDK) Java v2. 

# <a name="java-sdk-2xx-async-api"></a>[API Async kit SDK Java 2.x.x](#tab/java-v2-async)

* Cette fonctionnalité n’est pas prise en charge dans l’API Async Java SDK v2. 

---

### <a name="container-level-time-to-livettl"></a>Durée de vie (TTL) au niveau du conteneur

L’extrait de code suivant montre les différences dans la façon dont la durée de vie des données du conteneur est créée entre les API Async 4.0, 3.x.x, Sync 2.x.x et les API Async 2.x.x :

# <a name="java-sdk-40-async-api"></a>[API Async kit SDK Java 4.0](#tab/java-v4-async)

[!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=MigrateContainerTTLAsync)]

# <a name="java-sdk-3xx-async-api"></a>[API Async kit SDK Java 3.x.x](#tab/java-v3-async)

```java
CosmosContainer container;

// Create a new container with TTL enabled with default expiration value
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");
containerProperties.defaultTimeToLive(90 * 60 * 60 * 24);
container = database.createContainerIfNotExists(containerProperties, 400).block().container();
```

# <a name="java-sdk-2xx-sync-api"></a>[API Sync kit SDK Java 2.x.x](#tab/java-v2-sync)

```java
DocumentCollection documentCollection;

// Create a new container with TTL enabled with default expiration value
documentCollection.setDefaultTimeToLive(90 * 60 * 60 * 24);
documentCollection = client.createCollection(database.getSelfLink(), documentCollection, new RequestOptions()).getResource();
```

# <a name="java-sdk-2xx-async-api"></a>[API Async kit SDK Java 2.x.x](#tab/java-v2-async)

```java
DocumentCollection collection = new DocumentCollection();
// Create a new container with TTL enabled with default expiration value
collection.setDefaultTimeToLive(90 * 60 * 60 * 24);
collection = client
    .createCollection(database.getSelfLink(), documentCollection, new RequestOptions())
    .toBlocking()
    .single()
    .getResource();
```

---

### <a name="item-level-time-to-livettl"></a>Durée de vie (TTL) au niveau de l’élément

L’extrait de code suivant montre les différences dans la façon dont la durée de vie d’un élément est créée entre les API Async 4.0, 3.x.x, Sync 2.x.x et les API Async 2.x.x :

# <a name="java-sdk-40-async-api"></a>[API Async kit SDK Java 4.0](#tab/java-v4-async)

[!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=MigrateItemTTLClassAsync)]

[!code-java[](~/azure-cosmos-java-sql-api-samples/src/main/java/com/azure/cosmos/examples/documentationsnippets/async/SampleDocumentationSnippetsAsync.java?name=MigrateItemTTLAsync)]

# <a name="java-sdk-3xx-async-api"></a>[API Async kit SDK Java 3.x.x](#tab/java-v3-async)

```java
// Include a property that serializes to "ttl" in JSON
public class SalesOrder
{
    private String id;
    private String customerId;
    private Integer ttl;

    public SalesOrder(String id, String customerId, Integer ttl) {
        this.id = id;
        this.customerId = customerId;
        this.ttl = ttl;
    }

    public String id() {return this.id;}
    public SalesOrder id(String new_id) {this.id = new_id; return this;}
    public String customerId() {return this.customerId; return this;}
    public SalesOrder customerId(String new_cid) {this.customerId = new_cid;}
    public Integer ttl() {return this.ttl;}
    public SalesOrder ttl(Integer new_ttl) {this.ttl = new_ttl; return this;}

    //...
}

// Set the value to the expiration in seconds
SalesOrder salesOrder = new SalesOrder(
    "SO05",
    "CO18009186470",
    60 * 60 * 24 * 30  // Expire sales orders in 30 days
);
```

# <a name="java-sdk-2xx-sync-api"></a>[API Sync kit SDK Java 2.x.x](#tab/java-v2-sync)

```java
Document document = new Document();
document.setId("YourDocumentId");
document.setTimeToLive(60 * 60 * 24 * 30 ); // Expire document in 30 days
ResourceResponse<Document> documentResourceResponse = client.createDocument(documentCollection.getSelfLink(), document,
    new RequestOptions(), true);
Document responseDocument = documentResourceResponse.getResource();
```

# <a name="java-sdk-2xx-async-api"></a>[API Async kit SDK Java 2.x.x](#tab/java-v2-async)

```java
Document document = new Document();
document.setId("YourDocumentId");
document.setTimeToLive(60 * 60 * 24 * 30 ); // Expire document in 30 days
ResourceResponse<Document> documentResourceResponse = client.createDocument(documentCollection.getSelfLink(), document,
    new RequestOptions(), true).toBlocking().single();
Document responseDocument = documentResourceResponse.getResource();
```

---

## <a name="next-steps"></a>Étapes suivantes

* [Créer une application Java](create-sql-api-java.md) pour gérer les données de l’API SQL Azure Cosmos DB avec la version 4 du kit SDK
* En savoir plus sur les [Kits SDK Java basés sur Reactor](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/main/reactor-pattern-guide.md)
* En savoir plus sur la conversion du code Async RxJava en code Async Reactor avec le [Guide comparatif Reactor-RxJava](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/main/reactor-rxjava-guide.md)