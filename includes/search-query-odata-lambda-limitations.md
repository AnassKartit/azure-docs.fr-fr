---
author: brjohnstmsft
ms.service: cognitive-search
ms.topic: include
ms.date: 06/13/2018
ms.author: brjohnst
ms.openlocfilehash: 998d0f1a84dc9cb2a07fb55286c1089787a263e1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "80272956"
---
| Type de données | Fonctionnalités autorisées dans les expressions lambda avec `any` | Fonctionnalités autorisées dans les expressions lambda avec `all` |
|---|---|---|
| `Collection(Edm.ComplexType)` | Tout sauf `search.ismatch` et `search.ismatchscoring` | Identique |
| `Collection(Edm.String)` | Comparaisons avec `eq` ou `search.in` <br/><br/> Combinaison de sous-expressions avec `or` | Comparaisons avec `ne` ou `not search.in()` <br/><br/> Combinaison de sous-expressions avec `and` |
| `Collection(Edm.Boolean)` | Comparaisons avec `eq` ou `ne` | Identique |
| `Collection(Edm.GeographyPoint)` | Utilisation de `geo.distance` avec `lt` ou `le` <br/><br/> `geo.intersects` <br/><br/> Combinaison de sous-expressions avec `or` | Utilisation de `geo.distance` avec `gt` ou `ge` <br/><br/> `not geo.intersects(...)` <br/><br/> Combinaison de sous-expressions avec `and` |
| `Collection(Edm.DateTimeOffset)`, `Collection(Edm.Double)`, `Collection(Edm.Int32)`, `Collection(Edm.Int64)` | Comparaisons avec `eq`, `ne`, `lt`, `gt`, `le` ou `ge` <br/><br/> Combinaison de comparaisons avec d’autres sous-expressions à l’aide de `or` <br/><br/> Combinaison de comparaisons, sauf `ne`, avec d’autres sous-expressions à l’aide de `and` <br/><br/> Expressions utilisant des combinaisons de `and` et `or` en [forme normale disjonctive (FND)](https://en.wikipedia.org/wiki/Disjunctive_normal_form) | Comparaisons avec `eq`, `ne`, `lt`, `gt`, `le` ou `ge` <br/><br/> Combinaison de comparaisons avec d’autres sous-expressions à l’aide de `and` <br/><br/> Combinaison de comparaisons, sauf `eq`, avec d’autres sous-expressions à l’aide de `or` <br/><br/> Expressions utilisant des combinaisons de `and` et `or` en [forme normale conjonctive (FNC)](https://en.wikipedia.org/wiki/Conjunctive_normal_form) |
