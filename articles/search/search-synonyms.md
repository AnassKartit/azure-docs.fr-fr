---
title: Synonymes pour une extension de requête sur un index de recherche
titleSuffix: Azure Cognitive Search
description: Créer une carte de synonymes pour élargir l’étendue d’une requête de recherche sur un index de Recherche cognitive Azure. L’étendue est élargie pour inclure des termes équivalents que vous fournissez dans une liste.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/11/2021
ms.openlocfilehash: ea92a5e196c809535801278631cbfdfdc5013199
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2021
ms.locfileid: "112288214"
---
# <a name="synonyms-in-azure-cognitive-search"></a>Synonymes dans Recherche cognitive Azure

Dans un service de recherche, le mappage de synonymes est une opération effectuée par une ressource globale qui associe des termes équivalents. Cela permet de développer l’étendue d’une requête sans que l’utilisateur ne soit obligé de fournir le terme. Par exemple, en partant du principe que « chien », « canin » et « chiot » sont synonymes, une requête sur « canin » correspond à un document contenant « chien ».

## <a name="create-synonyms"></a>Créer des synonymes

Une carte de synonymes est un élément multimédia qui peut être créé une seule fois et utilisé par de nombreux index. Le [niveau de service](search-limits-quotas-capacity.md#synonym-limits) détermine le nombre de cartes de synonymes que vous pouvez créer, allant de trois cartes de synonymes pour les niveaux Gratuit et De base, jusqu’à 20 pour les niveaux Standard. 

Vous pouvez créer plusieurs cartes de synonymes pour différentes langues, telles que les versions en anglais et en français, ou des lexiques si votre contenu comprend une terminologie technique ou complexe. Bien que vous puissiez créer plusieurs cartes de synonymes dans votre service de recherche, un champ ne peut en utiliser qu’un seul.

Une carte de synonymes se compose de noms, de formats et de règles qui fonctionnent comme des entrées de carte de synonymes. Le seul format pris en charge est `solr`, et le format de `solr` détermine la construction des règles.

```http
POST /synonymmaps?api-version=2020-06-30
{
    "name": "geo-synonyms",
    "format": "solr",
    "synonyms": "
        USA, United States, United States of America\n
        Washington, Wash., WA => WA\n"
}
```

Vous créez un mappage de synonymes par programmation (le portail ne prend pas en charge les définitions de mappage de synonymes) :

+ [Créer un mappage de synonymes (API REST)](/rest/api/searchservice/create-synonym-map). Cette référence est la plus descriptive.
+ [Classe SynonymMap (.NET)](/dotnet/api/azure.search.documents.indexes.models.synonymmap) et [Ajouter des synonymes en C#](search-synonyms-tutorial-sdk.md)
+ [Classe SynonymMap (Python)](/python/api/azure-search-documents/azure.search.documents.indexes.models.synonymmap)
+ [Interface SynonymMap (JavaScript)](/javascript/api/@azure/search-documents/synonymmap)
+ [Classe SynonymMap (Java)](/java/api/com.azure.search.documents.indexes.models.synonymmap)

## <a name="define-rules"></a>Définir des règles

Les règles de mappage respectent la spécification de filtre de synonyme open source d’Apache Solr, décrite dans ce document : [SynonymFilter](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions#FilterDescriptions-SynonymFilter). Le format `solr` prend en charge deux types de règles :

+ équivalence (où les termes sont des substituts égaux dans la requête)

+ mappages explicites (où les termes sont mappés à un terme explicite avant l’interrogation)

Chaque règle doit être délimitée par le caractère de nouvelle ligne (`\n`). Vous pouvez définir jusqu'à 5 000 règles par carte de synonymes dans un service gratuit et 20 000 règles par carte dans les autres références tierces. Chaque règle peut avoir jusqu'à 20 extensions (ou objets dans une règle). Pour plus d’informations, consultez [Limites des synonymes](search-limits-quotas-capacity.md#synonym-limits).

Les analyseurs de requêtes respectent la casse des termes majuscules ou mixtes, mais si vous souhaitez conserver les caractères spéciaux dans la chaîne, par exemple une virgule ou un tiret, ajoutez les caractères d’échappement appropriés lors de la création de la carte de synonymes.

### <a name="equivalency-rules"></a>Règles d’équivalence

Les règles pour des termes équivalents sont délimitées par des virgules dans la même règle. Dans le premier exemple, une requête sur `USA` se développe en `USA` OU `"United States"` OU `"United States of America"`. Notez que si vous souhaitez faire correspondre deux expressions, la requête elle-même doit être une requête d’expression entre guillemets.

Dans le cas d’équivalence, une requête pour `dog` développera la requête pour inclure également `puppy` et `canine`.

```json
{
"format": "solr",
"synonyms": "
    USA, United States, United States of America\n
    dog, puppy, canine\n
    coffee, latte, cup of joe, java\n"
}
```

### <a name="explicit-mapping"></a>Mappage explicite

Les règles d’un mappage explicite sont dénotées par une flèche `=>`. Lorsqu’elle spécifiée, une séquence de termes d’une requête de recherche qui correspond à la partie gauche de `=>` est remplacée par les alternatives sur la partie droite au moment de la requête.

Dans le cas explicite, une requête pour `Washington`, `Wash.` ou `WA` sera réécrite comme `WA`, et le moteur de requête recherchera uniquement les correspondances au terme `WA`. Le mappage explicite s’applique dans le sens spécifié uniquement et ne réécrit pas la requête `WA` pour `Washington` dans ce cas.

```json
{
"format": "solr",
"synonyms": "
    Washington, Wash., WA => WA\n
    California, Calif., CA => CA\n"
}
```

### <a name="escaping-special-characters"></a>Échappement des caractères spéciaux

Dans la recherche en texte intégral, les synonymes sont analysés durant le traitement de la requête comme tout autre terme de requête, ce qui signifie que les règles relatives aux caractères réservés et aux caractères spéciaux s’appliquent aux termes du mappage de synonymes. La liste des caractères qui nécessitent un échappement varie entre la syntaxe simple et la syntaxe complète :

+ [syntaxe simple](query-simple-syntax.md) `+ | " ( ) ' \`
+ [syntaxe complète](query-lucene-syntax.md) `+ - & | ! ( ) { } [ ] ^ " ~ * ? : \ /`

Rappelez-vous que si vous devez conserver des caractères qui sont en principe ignorés par l’analyseur par défaut durant l’indexation, vous devez utiliser un autre analyseur qui les conserve. Parmi les choix possibles figurent les [analyseurs de langage](index-add-language-analyzers.md) naturel de Microsoft, qui conservent les mots avec trait d'union, ou un analyseur personnalisé pour les modèles plus complexes. Pour plus d'informations, consultez [Termes partiels, modèles et caractères spéciaux](search-query-partial-matching.md).

L’exemple suivant montre comment placer un caractère dans une séquence d’échappement à l’aide d’une barre oblique inverse :

```json
{
"format": "solr",
"synonyms": "WA\, USA, WA, Washington\n"
}
```

Étant donné que la barre oblique inverse est elle-même un caractère spécial dans d’autres langages tels que JSON et C#, vous devrez probablement effectuer un double échappement. Par exemple, le code JSON envoyé à l’API REST pour la carte de synonymes ci-dessus ressemble à ceci :

```json
{
"format":"solr",
"synonyms": "WA\\, USA, WA, Washington"
}
```

## <a name="upload-and-manage-synonym-maps"></a>Charger et gérer des cartes de synonymes

Comme mentionné précédemment, vous pouvez créer ou mettre à jour une carte de synonymes sans interrompre les charges de travail de requête et d’indexation. Une carte de synonymes est un objet autonome (comme des index ou des sources de données), et tant qu’aucun champ ne l’utilise, les mises à jour n’entraînent pas l’échec de l’indexation ou des requêtes. Toutefois, une fois que vous avez ajouté une carte de synonymes à une définition de champ, si vous supprimez ensuite une carte de synonymes, toute requête qui comprend les champs en question échouera avec une erreur 404.

La création, la mise à jour et la suppression d’une carte de synonymes constituent des opérations qui s’appliquent au document entier, ce qui signifie que vous ne peut pas mettre à jour ou supprimer des parties de la carte de synonyme de façon incrémentielle. Même la mise à jour d’une seule règle nécessite un rechargement.

## <a name="assign-synonyms-to-fields"></a>Assigner des synonymes à des champs

Après le chargement d’une carte de synonymes, vous pouvez activer les synonymes sur les champs du type `Edm.String` ou `Collection(Edm.String)`, sur les champs qui ont `"searchable":true`. Comme indiqué, une définition de champ ne peut utiliser qu’une seule carte de synonymes.

```http
POST /indexes?api-version=2020-06-30
{
    "name":"hotels-sample-index",
    "fields":[
        {
            "name":"description",
            "type":"Edm.String",
            "searchable":true,
            "synonymMaps":[
            "en-synonyms"
            ]
        },
        {
            "name":"description_fr",
            "type":"Edm.String",
            "searchable":true,
            "analyzer":"fr.microsoft",
            "synonymMaps":[
            "fr-synonyms"
            ]
        }
    ]
}
```

## <a name="query-on-equivalent-or-mapped-fields"></a>Requête sur des champs équivalents ou mappés

L’ajout de synonymes n’impose pas de nouvelles exigences sur la construction des requêtes. Vous pouvez émettre des requêtes de terme et d’expression comme vous l’avez fait avant l’ajout de synonymes. La seule différence est que si un terme de requête existe dans la carte de synonymes, le moteur de requête développe ou réécrit le terme ou l’expression, en fonction de la règle.

## <a name="how-synonyms-are-used-during-query-execution"></a>Utilisation des synonymes durant l’exécution de la requête

Les synonymes sont une technique d’extension de requête qui complète le contenu d’un index avec des termes équivalents, mais uniquement pour des champs ayant une attribution de synonyme. Si une requête sur des champs *exclut* un champ avec synonyme, vous ne verrez pas de correspondances de la carte de synonymes.

Pour des champs avec synonymes, les synonymes sont soumis à la même analyse de texte que le champ associé. Par exemple, si un champ est analysé à l’aide de l’analyseur Lucene standard, les termes synonymes sont également soumis à l’analyseur Lucene standard au moment de la requête. Si vous souhaitez conserver les signes de ponctuation, tels que des points ou des tirets, dans le terme synonyme, appliquez un analyseur de préservation du contenu sur le champ.

En interne, la fonctionnalité de synonymes réécrit la requête d’origine avec des synonymes utilisant l’opérateur OR. Pour cette raison, la mise en surbrillance des correspondances et les profils de score traitent les synonymes et le terme d’origine comme équivalents.

Les synonymes s’appliquent uniquement aux requêtes de texte de forme libre et ne sont pas pris en charge pour les filtres, les facettes, la saisie semi-automatique ou les suggestions. L’autocomplétion et les suggestions sont basées uniquement sur le terme d’origine : les correspondances de synonymes n’apparaissent pas dans la réponse.

Les extensions de synonymes ne s’appliquent pas aux termes de recherche génériques : les préfixes, correspondances partielles et les expressions régulières ne sont pas étendus.

Si vous devez faire une requête unique qui applique l’expansion de synonymes et des recherches avec des caractères génériques, des expressions régulières ou une correspondance approximative, vous pouvez combiner les requêtes avec la syntaxe d’OR. Par exemple, pour combiner des synonymes avec des caractères génériques pour une syntaxe de requête simple, le terme serait `<query> | <query>*`.

Si vous avez un index existant dans un environnement de déploiement (non production), faites des essais avec un petit dictionnaire pour voir comment l’ajout de synonymes modifie l’expérience de recherche, notamment son impact sur les profils de score, la mise en surbrillance des correspondances et les suggestions.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Créer une carte de synonymes (API REST)](/rest/api/searchservice/create-synonym-map)