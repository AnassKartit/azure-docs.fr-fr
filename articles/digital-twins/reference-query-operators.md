---
title: Informations de référence sur le langage de requête Azure Digital Twins - Opérateurs
titleSuffix: Azure Digital Twins
description: Documentation de référence sur les opérateurs du langage de requête Azure Digital Twins
author: baanders
ms.author: baanders
ms.date: 03/22/2021
ms.topic: article
ms.service: digital-twins
ms.openlocfilehash: 06945efc6be8700955b1a475d00d21951102c214
ms.sourcegitcommit: bc29cf4472118c8e33e20b420d3adb17226bee3f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/08/2021
ms.locfileid: "113492790"
---
# <a name="azure-digital-twins-query-language-reference-operators"></a>Informations de référence sur le langage de requête Azure Digital Twins : opérateurs

Ce document contient des informations de référence sur les **opérateurs** du [langage de requête Azure Digital Twins](concepts-query-language.md).

## <a name="comparison-operators"></a>Opérateurs de comparaison

Les opérateurs suivants de la famille « comparaison » sont pris en charge.

* `=`, `!=` : utilisés pour comparer l’égalité des expressions.
* `<`, `>` : utilisés pour la comparaison ordonnée des expressions.
* `<=`, `>=` : utilisés pour la comparaison ordonnée des expressions, y compris l’égalité.

### <a name="example"></a>Exemple

Voici un exemple utilisant `=`. La requête suivante retourne les jumeaux dont la valeur Temperature est égale à 80.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="EqualityExample":::

Voici un exemple utilisant `<`. La requête suivante retourne les jumeaux dont la valeur Temperature est inférieure à 80.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="ComparisonExample":::

Voici un exemple utilisant `<=`. La requête suivante retourne les jumeaux dont la valeur Temperature est inférieure ou égale à 80.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="OrderedComparisonExample":::

## <a name="contains-operators"></a>Opérateurs « contient »

Les opérateurs suivants de la famille « contient » sont pris en charge.

* `IN` : prend la valeur true si une valeur donnée est comprise dans un ensemble de valeurs.
* `NIN` : prend la valeur true si une valeur donnée n’est comprise pas dans un ensemble de valeurs.

### <a name="example"></a>Exemple

Voici un exemple utilisant `IN`. La requête suivante retourne les jumeaux dont la propriété `owner` correspond à l’une des options d’une liste.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="InExample":::

## <a name="logical-operators"></a>Opérateurs logiques

Les opérateurs suivants de la famille « logique » sont pris en charge :
* `AND` : utilisé pour connecter deux expressions ; prend la valeur true si elles sont toutes les deux vraies.
* `OR` : utilisé pour connecter deux expressions ; prend la valeur true si au moins l’une d’elles est vraie.
* `NOT` : utilisé pour nier une expression ; prend la valeur true si la condition de l’expression n’est pas remplie.

### <a name="example"></a>Exemple

Voici un exemple utilisant `AND`. La requête suivante retourne les jumeaux qui remplissent les deux conditions suivantes : valeur Temperature inférieure à 80 et valeur Humidity inférieure à 50.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="AndExample":::

Voici un exemple utilisant `OR`. La requête suivante retourne les jumeaux qui remplissent au moins une des conditions suivantes : valeur Temperature inférieure à 80 et valeur Humidity inférieure à 50.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="OrExample":::

Voici un exemple utilisant `NOT`. La requête suivante retourne les jumeaux qui ne remplissent pas la condition selon laquelle la valeur Temperature doit être inférieure à 80.

:::code language="sql" source="~/digital-twins-docs-samples/queries/reference.sql" id="NotExample":::

## <a name="limitations"></a>Limites

Les limites suivantes s’appliquent aux requêtes utilisant des opérateurs.
* [Limite pour IN/NIN](#limit-for-innin)

Pour des informations plus détaillées, consultez la section ci-après.

### <a name="limit-for-innin"></a>Limite pour IN/NIN

La limite du nombre de valeurs qui peuvent être incluses dans un ensemble `IN` ou `NIN` est de 100 valeurs.
