---
title: 'Langage de requête Azure Cosmos DB : INDEX_OF'
description: Découvrez la fonction système SQL INDEX_OF dans Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: bc397718d2a25e02c5d93bcf64cd2866c4b7edd5
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2021
ms.locfileid: "122525728"
---
# <a name="index_of-azure-cosmos-db"></a>INDEX_OF (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](../includes/appliesto-sql-api.md)]

 Retourne la position de départ de la première occurrence de la seconde expression de chaîne dans la première expression de chaîne spécifiée, ou -1 si la chaîne est introuvable.  
  
## <a name="syntax"></a>Syntaxe
  
```sql
INDEX_OF(<str_expr1>, <str_expr2> [, <numeric_expr>])  
```  
  
## <a name="arguments"></a>Arguments
  
*str_expr1*  
   Est l’expression de chaîne faisant l’objet de la recherche.  
  
*str_expr2*  
   Est l’expression de chaîne à rechercher.  

*numeric_expr* est une expression numérique facultative qui définit la position de début de la recherche. La première position dans *str_expr1* est 0. 
  
## <a name="return-types"></a>Types de retour
  
  Renvoie une expression numérique.  
  
## <a name="examples"></a>Exemples
  
  L’exemple suivant retourne l’index de diverses sous-chaînes à l’intérieur de « abc ».  
  
```sql
SELECT INDEX_OF("abc", "ab") AS i1, INDEX_OF("abc", "b") AS i2, INDEX_OF("abc", "c") AS i3 
```  
  
 Voici le jeu de résultats obtenu.  
  
```json
[{"i1": 0, "i2": 1, "i3": -1}]  
```  

## <a name="next-steps"></a>Étapes suivantes

- [Fonctions de chaîne Azure Cosmos DB](sql-query-string-functions.md)
- [Fonctions système Azure Cosmos DB](sql-query-system-functions.md)
- [Présentation d’Azure Cosmos DB](../introduction.md)
