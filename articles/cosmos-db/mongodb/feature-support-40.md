---
title: Fonctionnalités et syntaxe de la version serveur 4.0 prises en charge dans l’API Azure Cosmos DB pour MongoDB
description: Découvrez les fonctionnalités et la syntaxe de la version serveur 4.0 prises en charge dans l’API Azure Cosmos DB pour MongoDB. Découvrez les commandes de base de données, la prise en charge du langage de requête, les types de données, les commandes de pipeline d’agrégation et les opérateurs pris en charge.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: overview
ms.date: 03/02/2021
author: gahl-levy
ms.author: gahllevy
ms.openlocfilehash: d9e55357af2a3acd551b502245445e7cf851bd74
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "121784115"
---
# <a name="azure-cosmos-dbs-api-for-mongodb-40-server-version-supported-features-and-syntax"></a>API Azure Cosmos DB pour MongoDB (version serveur 4.0) : fonctionnalités et syntaxe prises en charge
[!INCLUDE[appliesto-mongodb-api](../includes/appliesto-mongodb-api.md)]

Azure Cosmos DB est le service de base de données multi-modèle de Microsoft distribué à l’échelle mondiale. Vous pouvez communiquer avec l’API Azure Cosmos DB pour MongoDB par le biais de n’importe quel [pilote](https://docs.mongodb.org/ecosystem/drivers) du client open source MongoDB. L’API Azure Cosmos DB pour MongoDB permet d’utiliser les pilotes clients existants en adhérant au [protocole Wire](https://docs.mongodb.org/manual/reference/mongodb-wire-protocol) MongoDB.

À l’aide de l’API Azure Cosmos DB pour MongoDB, vous pouvez profiter des avantages de MongoDB que vous connaissez déjà, ainsi que de toutes les fonctionnalités d’entreprise fournies par Cosmos DB, comme la [distribution mondiale](../distribute-data-globally.md), le [partitionnement automatique](../partitioning-overview.md), les garanties de disponibilité et de latence, le chiffrement au repos, les sauvegardes et bien d’autres encore.

## <a name="protocol-support"></a>Prise en charge du protocole

Les opérateurs pris en charge, ainsi que les limitations ou exceptions sont répertoriés ci-dessous. Les pilotes clients comprenant ces protocoles doivent pouvoir se connecter à l’API Azure Cosmos DB pour MongoDB. Quand vous utilisez des comptes API Azure Cosmos DB pour MongoDB, le point de terminaison des versions 3.6 et ultérieures des comptes est au format `*.mongo.cosmos.azure.com`, tandis que celui de la version 3.2 des comptes est au format `*.documents.azure.com`.

> [!NOTE]
> Cet article liste uniquement les commandes de serveur prises en charge et exclut les fonctions wrapper côté client. Les fonctions wrapper côté client telles que `deleteMany()` et `updateMany()` utilisent en interne les commandes de serveur `delete()` et `update()`. Les fonctions utilisant des commandes de serveur prises en charge sont compatibles avec l’API d’Azure Cosmos DB pour MongoDB.

## <a name="query-language-support"></a>Prise en charge du langage de requêtes

L’API Azure Cosmos DB pour MongoDB permet la prise en charge complète des constructions de langage de requête MongoDB. Vous trouverez ci-dessous la liste détaillée des opérations, opérateurs, étapes, commandes et options actuellement pris en charge.

## <a name="database-commands"></a>Commandes de base de données

L’API Azure Cosmos DB pour MongoDB prend en charge les commandes de base de données suivantes :

### <a name="query-and-write-operation-commands"></a>Commandes d’opérations de requête et d’écriture

| Commande | Prise en charge |
|---------|---------|
| [changements de flux](change-streams.md) | Oui |
| supprimer | Oui |
| eval | Non |
| trouver | Oui |
| findAndModify | Oui |
| getLastError | Oui |
| getMore | Oui |
| getPrevError | Non |
| insert | Oui |
| parallelCollectionScan | Non |
| resetError | Non |
| update | Oui |

### <a name="transaction-commands"></a>Commandes de transaction

| Commande | Prise en charge |
|---------|---------|
| abortTransaction | Oui |
| commitTransaction | Oui |

### <a name="authentication-commands"></a>Commandes d’authentification

| Commande | Prise en charge |
|---------|---------|
| authentifier | Oui |
| getnonce | Oui |
| logout | Oui |

### <a name="administration-commands"></a>Commandes d’administration

| Commande | Prise en charge |
|---------|---------|
| cloneCollectionAsCapped | Non |
| collMod | Non |
| ConnectionStatus | Non |
| convertToCapped | Non |
| copydb | Non |
| create | Oui |
| createIndexes | Oui |
| currentOp | Oui |
| drop | Oui |
| dropDatabase | Oui |
| dropIndexes | Oui |
| filemd5 | Oui |
| killCursors | Oui |
| killOp | Non |
| listCollections | Oui |
| listDatabases | Oui |
| listIndexes | Oui |
| reIndex | Oui |
| renameCollection | Non |

### <a name="diagnostics-commands"></a>Commandes de diagnostic

| Commande | Prise en charge |
|---------|---------|
| buildInfo | Oui |
| collStats | Oui |
| connPoolStats | Non |
| ConnectionStatus | Non |
| dataSize | Non |
| dbHash | Non |
| dbStats | Oui |
| explain | Oui |
| features | Non |
| hostInfo | Oui |
| listDatabases | Oui |
| listCommands | Non |
| profiler | Non |
| serverStatus | Non |
| top | Non |
| whatsmyuri | Oui |

## <a name="aggregation-pipeline"></a><a name="aggregation-pipeline"></a>Pipeline d’agrégation

### <a name="aggregation-commands"></a>Commandes d’agrégation

| Commande | Prise en charge |
|---------|---------|
| aggregate | Oui |
| count | Oui |
| distinct | Oui |
| mapReduce | Non |

### <a name="aggregation-stages"></a>Étapes d’agrégation

| Commande | Prise en charge |
|---------|---------|
| $addFields | Oui |
| $bucket | Non |
| $bucketAuto | Non |
| $changeStream | Oui |
| $collStats | Non |
| $count | Oui |
| $currentOp | Non |
| $facet | Oui |
| $geoNear | Oui |
| $graphLookup | Oui |
| $group | Oui |
| $indexStats | Non |
| $limit | Oui |
| $listLocalSessions | Non |
| $listSessions | Non |
| $lookup | Partiel |
| $match | Oui |
| $out | Oui |
| $project | Oui |
| $redact | Oui |
| $replaceRoot | Oui |
| $replaceWith | Non |
| $sample | Oui |
| $skip | Oui |
| $sort | Oui |
| $sortByCount | Oui |
| $unwind | Oui |

> [!NOTE]
> `$lookup` ne prend pas encore en charge la fonctionnalité de [sous-requêtes non corrélées](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/#join-conditions-and-uncorrelated-sub-queries) introduite dans la version serveur 3.6. Vous recevez une erreur avec un message contenant `let is not supported` si vous tentez d’utiliser l’opérateur `$lookup` avec les champs `let` et `pipeline`.

### <a name="boolean-expressions"></a>Expressions booléennes

| Commande | Prise en charge |
|---------|---------|
| $and | Oui |
| $not | Oui |
| $or | Oui |

### <a name="conversion-expressions"></a>Expressions de conversion

| Commande | Prise en charge |
|---------|---------|
| $convert | Oui |
| $toBool | Oui |
| $toDate | Oui |
| $toDecimal | Oui |
| $toDouble | Oui |
| $toInt | Oui |
| $toLong | Oui |
| $toObjectId | Oui |
| $toString | Oui |

### <a name="set-expressions"></a>Expressions SET

| Commande | Prise en charge |
|---------|---------|
| $setEquals | Oui |
| $setIntersection | Oui |
| $setUnion | Oui |
| $setDifference | Oui |
| $setIsSubset | Oui |
| $anyElementTrue | Oui |
| $allElementsTrue | Oui |

### <a name="comparison-expressions"></a>Expressions de comparaison

| Commande | Prise en charge |
|---------|---------|
| $cmp | Oui |
| $eq | Oui | 
| $gt | Oui | 
| $gte | Oui | 
| $lt | Oui |
| $lte | Oui | 
| $ne | Oui | 
| $in | Oui | 
| $nin | Oui | 

### <a name="arithmetic-expressions"></a>Expressions arithmétiques

| Commande | Prise en charge |
|---------|---------|
| $abs | Oui |
| $add | Oui |
| $ceil | Oui |
| $divide | Oui |
| $exp | Oui |
| $floor | Oui |
| $ln | Oui |
| $log | Oui |
| $log10 | Oui |
| $mod | Oui |
| $multiply | Oui |
| $pow | Oui |
| $sqrt | Oui |
| $subtract | Oui |
| $trunc | Oui |

### <a name="string-expressions"></a>Expressions de chaîne

| Commande | Prise en charge |
|---------|---------|
| $concat | Oui |
| $indexOfBytes | Oui |
| $indexOfCP | Oui |
| $ltrim | Oui |
| $rtrim | Oui |
| $trim | Oui |
| $split | Oui |
| $strLenBytes | Oui |
| $strLenCP | Oui |
| $strcasecmp | Oui |
| $substr | Oui |
| $substrBytes | Oui |
| $substrCP | Oui |
| $toLower | Oui |
| $toUpper | Oui |

### <a name="text-search-operator"></a>Opérateur de recherche de texte

| Commande | Prise en charge |
|---------|---------|
| $meta | Non |

### <a name="array-expressions"></a>Expressions de tableau

| Commande | Prise en charge |
|---------|---------|
| $arrayElemAt | Oui |
| $arrayToObject | Oui |
| $concatArrays | Oui |
| $filter | Oui |
| $indexOfArray | Oui |
| $isArray | Oui |
| $objectToArray | Oui |
| $range | Oui |
| $reverseArray | Oui |
| $reduce | Oui |
| $size | Oui |
| $slice | Oui |
| $zip | Oui |
| $in | Oui |

### <a name="variable-operators"></a>Opérateurs de variable

| Commande | Prise en charge |
|---------|---------|
| $map | Oui |
| $let | Oui |

### <a name="system-variables"></a>Variables système

| Commande | Prise en charge |
|---------|---------|
| $$CURRENT | Oui |
| $$DESCEND | Oui |
| $$KEEP | Oui |
| $$PRUNE | Oui |
| $$REMOVE | Oui |
| $$ROOT | Oui |

### <a name="literal-operator"></a>Opérateur littéral

| Commande | Prise en charge |
|---------|---------|
| $literal | Oui |

### <a name="date-expressions"></a>Expressions de date

| Commande | Prise en charge |
|---------|---------|
| $dayOfYear | Oui |
| $dayOfMonth | Oui |
| $dayOfWeek | Oui |
| $year | Oui |
| $month | Oui | 
| $week | Oui |
| $hour | Oui |
| $minute | Oui | 
| $second | Oui |
| $millisecond | Oui | 
| $dateToString | Oui |
| $isoDayOfWeek | Oui |
| $isoWeek | Oui |
| $dateFromParts | Non | 
| $dateToParts | Non |
| $dateFromString | Non |
| $isoWeekYear | Oui |

### <a name="conditional-expressions"></a>Expressions conditionnelles

| Commande | Prise en charge |
|---------|---------|
| $cond | Oui |
| $ifNull | Oui |
| $switch | Oui |

### <a name="data-type-operator"></a>Opérateur de type de données

| Commande | Prise en charge |
|---------|---------|
| $type | Oui |

### <a name="accumulator-expressions"></a>Expressions d’accumulation

| Commande | Prise en charge |
|---------|---------|
| $sum | Oui |
| $avg | Oui |
| $first | Oui |
| $last | Oui |
| $max | Oui |
| $min | Oui |
| $push | Oui |
| $addToSet | Oui |
| $stdDevPop | Oui |
| $stdDevSamp | Oui |

### <a name="merge-operator"></a>Opérateur Merge

| Commande | Prise en charge |
|---------|---------|
| $mergeObjects | Oui |

## <a name="data-types"></a>Types de données

L’API Azure Cosmos DB pour MongoDB prend en charge les documents encodés au format BSON MongoDB. La version 4.0 de l’API permet une meilleure utilisation interne de ce format pour améliorer les performances et réduire les coûts. Les documents écrits ou mis à jour par le biais d’un point de terminaison exécutant la version 4.0 profitent de ces avantages.
 
Dans un [scénario de mise à niveau](upgrade-mongodb-version.md), les documents qui ont été écrits avant la mise à niveau vers la version 4.0 ne bénéficient pas de l’amélioration des performances ; pour cela, ils doivent être mis à niveau par une opération d’écriture effectuée via le point de terminaison 4.0.

| Commande | Prise en charge |
|---------|---------|
| Double | Oui |
| String | Oui |
| Object | Oui |
| Array | Oui |
| Binary Data | Oui | 
| ObjectId | Oui |
| Boolean | Oui |
| Date | Oui |
| Null | Oui |
| Entier de 32 bits (int) | Oui |
| Timestamp | Oui |
| Entier de 64 bits (long) | Oui |
| MinKey | Oui |
| MaxKey | Oui |
| Decimal128 | Oui | 
| Expression régulière | Oui |
| JavaScript | Oui |
| JavaScript (avec étendue)| Oui |
| Indéfini | Oui |

## <a name="indexes-and-index-properties"></a>Index et propriétés d’index

### <a name="indexes"></a>Index

| Commande | Prise en charge |
|---------|---------|
| Index à champ unique | Oui |
| Index composé | Oui |
| Index à plusieurs clés | Oui |
| Index de texte | Non |
| 2dsphere | Oui |
| Index 2D | Non |
| Index de hachage | Oui |

### <a name="index-properties"></a>Propriétés d’index

| Commande | Prise en charge |
|---------|---------|
| TTL | Oui |
| Unique | Oui |
| Partiel | Non |
| Insensible à la casse | Non |
| Partiellement alloué | Non |
| Arrière-plan | Oui |

## <a name="operators"></a>Opérateurs

### <a name="logical-operators"></a>Opérateurs logiques

| Commande | Prise en charge |
|---------|---------|
| $or | Oui |
| $and | Oui |
| $not | Oui |
| $nor | Oui | 

### <a name="element-operators"></a>Opérateurs d’élément

| Commande | Prise en charge |
|---------|---------|
| $exists | Oui |
| $type | Oui |

### <a name="evaluation-query-operators"></a>Opérateurs de requête d’évaluation

| Commande | Prise en charge |
|---------|---------|
| $expr | Non |
| $jsonSchema | Non |
| $mod | Oui |
| $regex | Oui |
| $text | Non (Non pris en charge. Utilisez plutôt $regex.)| 
| $where | Non | 

Dans les requêtes $regex, les expressions ancrées à gauche autorisent la recherche d’index. Toutefois, l’utilisation des modificateurs « i » (non sensible à la casse) et « m » (multiligne), provoquent l’analyse de la collection pour toutes les expressions.

Lorsqu’il est nécessaire d’inclure « $ » ou « | », il est préférable de créer deux requêtes regex (ou plus). Par exemple, étant donné la requête d’origine suivante : `find({x:{$regex: /^abc$/})`, elle doit être modifiée comme suit :

`find({x:{$regex: /^abc/, x:{$regex:/^abc$/}})`

La première partie utilise l’index pour limiter la recherche aux documents commençant par ^abc et la deuxième partie correspond aux entrées exactes. L’opérateur à barre « | » agit comme une fonction « OR », la requête `find({x:{$regex: /^abc |^def/})` correspond aux documents dont le champ « x » comporte une valeur commençant par « abc » ou « def ». Pour utiliser l’index, il est recommandé de diviser la requête en deux requêtes différentes jointes par l’opérateur $or : `find( {$or : [{x: $regex: /^abc/}, {$regex: /^def/}] })`.

### <a name="array-operators"></a>Opérateurs de tableau

| Commande | Prise en charge | 
|---------|---------|
| $all | Oui | 
| $elemMatch | Oui | 
| $size | Oui | 

### <a name="comment-operator"></a>Opérateur de commentaire

| Commande | Prise en charge | 
|---------|---------|
| $comment | Oui | 

### <a name="projection-operators"></a>Opérateurs de projection

| Commande | Prise en charge |
|---------|---------|
| $elemMatch | Oui |
| $meta | Non |
| $slice | Oui |

### <a name="update-operators"></a>Opérateurs de mise à jour

#### <a name="field-update-operators"></a>Opérateurs de mise à jour de champ

| Commande | Prise en charge |
|---------|---------|
| $inc | Oui |
| $mul | Oui |
| $rename | Oui |
| $setOnInsert | Oui |
| $set | Oui |
| $unset | Oui |
| $min | Oui |
| $max | Oui |
| $currentDate | Oui |

#### <a name="array-update-operators"></a>Opérateurs de mise à jour de tableau

| Commande | Prise en charge |
|---------|---------|
| $ | Oui |
| $[]| Oui |
| $[\<identifier\>]| Oui |
| $addToSet | Oui |
| $pop | Oui |
| $pullAll | Oui |
| $pull | Oui |
| $push | Oui |
| $pushAll | Oui |

#### <a name="update-modifiers"></a>Modificateurs de mise à jour

| Commande | Prise en charge |
|---------|---------|
| $each | Oui |
| $slice | Oui |
| $sort | Oui |
| $position | Oui |

#### <a name="bitwise-update-operator"></a>Opérateur de mise à jour au niveau du bit

| Commande | Prise en charge |
|---------|---------|
| $bit | Oui | 
| $bitsAllSet | Non |
| $bitsAnySet | Non |
| $bitsAllClear | Non |
| $bitsAnyClear | Non |

### <a name="geospatial-operators"></a>Opérateurs géospatiaux

Opérateur | Prise en charge | 
--- | --- |
$geoWithin | Oui |
$geoIntersects | Oui | 
$near | Oui |
$nearSphere | Oui |
$geometry | Oui |
$minDistance | Oui |
$maxDistance | Oui |
$center | Non |
$centerSphere | Non |
$box | Non |
$polygon | Non |

## <a name="sort-operations"></a>Opérations de tri

Lorsque vous utilisez l’opération `findOneAndUpdate`, les opérations de tri sur un champ unique sont prises en charge, mais les opérations à effectuer sur plusieurs champs ne le sont pas.

## <a name="unique-indexes"></a>Index uniques

Les [index uniques](mongodb-indexing.md#unique-indexes) garantissent qu’un champ spécifique ne présente aucune valeur en double dans l’ensemble des documents d’une collection. Cette approche est semblable à la façon dont l’unicité est conservée dans la clé « _id » par défaut. Vous pouvez créer des index uniques dans Azure Cosmos DB à l’aide de la commande `createIndex` avec le paramètre de contrainte `unique` :

```javascript
globaldb:PRIMARY> db.coll.createIndex( { "amount" : 1 }, {unique:true} )
{
    "_t" : "CreateIndexesResponse",
    "ok" : 1,
    "createdCollectionAutomatically" : false,
    "numIndexesBefore" : 1,
    "numIndexesAfter" : 4
}
```

## <a name="compound-indexes"></a>Index composés

Les [index composés](mongodb-indexing.md#compound-indexes-mongodb-server-version-36) permettent de créer un index pour des groupes de huit champs au maximum. Ce type d’index diffère des index composés MongoDB natifs. Dans Azure Cosmos DB, les index composés sont utilisés pour le tri des opérations appliquées à plusieurs champs. Pour créer un index composé, vous devez spécifier plusieurs propriétés en tant que paramètre :

```javascript
globaldb:PRIMARY> db.coll.createIndex({"amount": 1, "other":1})
{
    "createdCollectionAutomatically" : false, 
    "numIndexesBefore" : 1,
    "numIndexesAfter" : 2,
    "ok" : 1
}
```

## <a name="gridfs"></a>GridFS

Azure Cosmos DB prend en charge GridFS par le biais de n’importe quel pilote Mongo compatible GridFS.

## <a name="replication"></a>Réplication

Azure Cosmos DB prend en charge la réplication automatique et native des couches inférieures. Cette logique est prolongée pour obtenir également la réplication globale et à faible latence. Cosmos DB ne prend pas en charge les commandes de réplication manuelle.

## <a name="retryable-writes"></a>Écritures renouvelables

Cosmos DB ne prend pas en charge les écritures renouvelables. Les pilotes clients doivent ajouter le paramètre d’URL 'retryWrites=false' à leur chaîne de connexion. Vous pouvez ajouter d’autres paramètres d’URL en les faisant précéder d’un signe « & ». 

## <a name="sharding"></a>Partitionnement

Azure Cosmos DB prend en charge le partitionnement automatique côté serveur. Il gère automatiquement la création, le positionnement et l’équilibrage de partitions. Azure Cosmos DB ne prend pas en charge les commandes de partitionnement manuel, ce qui signifie que vous n’avez pas besoin d’appeler de commandes comme addShard, balancerStart, moveChunk, etc. Vous devez uniquement spécifier la clé de partition au moment de créer les conteneurs ou d’interroger les données.

## <a name="sessions"></a>Sessions

Azure Cosmos DB ne prend pas encore en charge les commandes de sessions côté serveur.

## <a name="time-to-live-ttl"></a>Durée de vie (TTL)

Azure Cosmos DB prend en charge une durée de vie (TTL) en fonction du timestamp du document. La TTL peut être activée pour les collections via le [Portail Azure](https://portal.azure.com).

## <a name="transactions"></a>Transactions

Les transactions multidocuments sont prises en charge dans une collection non partitionnée. Les transactions multidocuments ne sont pas prises en charge sur plusieurs collections ou au sein de collections partitionnées. Le délai d’expiration des transactions est fixé à cinq secondes.

## <a name="user-and-role-management"></a>Gestion des rôles et des utilisateurs

Azure Cosmos DB ne prend pas encore en charge les utilisateurs et les rôles. Cosmos DB prend cependant en charge le contrôle d'accès en fonction du rôle Azure (Azure RBAC) et les mots de passe/clés en lecture-écriture et en lecture seule, qui peuvent être obtenus par le biais du [portail Azure](https://portal.azure.com) (page de la chaîne de connexion).

## <a name="write-concern"></a>Élément Write Concern

Certaines applications utilisent un élément [Write Concern](https://docs.mongodb.com/manual/reference/write-concern/), qui indique le nombre de réponses nécessaires au cours d’une opération d’écriture. En raison de la façon dont Cosmos DB gère la réplication en arrière-plan, toutes les écritures atteignent automatiquement le quorum par défaut. Tout élément Write Concern spécifié par le code client est ignoré. Pour en savoir plus, consultez [Niveaux de cohérence des données analysables dans Azure Cosmos DB](../consistency-levels.md).

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment [utiliser Studio 3T](connect-using-mongochef.md) avec l’API Azure Cosmos DB pour MongoDB.
- Découvrez comment [utiliser Robo 3T](connect-using-robomongo.md) avec l’API Azure Cosmos DB pour MongoDB.
- Explorez les [exemples](nodejs-console-app.md) MongoDB avec l’API Azure Cosmos DB pour MongoDB.
