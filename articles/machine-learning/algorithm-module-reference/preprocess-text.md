---
title: 'Pré-traiter le texte : Informations de référence sur les modules'
titleSuffix: Azure Machine Learning
description: Découvrez comment utiliser le module Pré-traiter le texte dans le concepteur Azure Machine Learning pour nettoyer et simplifier le texte.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/01/2019
ms.openlocfilehash: d512a691b76cb7cbc72b4cbcb1fc821e928ea1b0
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93421224"
---
# <a name="preprocess-text"></a>Pré-traiter le texte

Cet article décrit un module dans le concepteur Azure Machine Learning.

Utilisez le module **Prétraiter le texte** pour nettoyer et simplifier le texte. Ce module prend en charge ces opérations courantes de traitement de texte :

* Suppression des mots vides
* Utilisation d’expressions régulières pour rechercher et remplacer des chaînes cibles spécifiques
* Lemmatisation, qui convertit plusieurs mots connexes en une seule forme canonique
* Normalisation de casse
* Suppression de certaines classes de caractères, comme les nombres, les caractères spéciaux et des séquences de caractères répétés comme « aaaa »
* Identification et suppression des adresses e-mail et URL

Actuellement, le module **Pré-traiter le texte** prend en charge uniquement l’anglais.

## <a name="configure-text-preprocessing"></a>Configurer le prétraitement du texte  

1.  Ajoutez le module **Pré-traiter le texte** à votre pipeline dans Azure Machine Learning. Ce module se trouve sous **Analyse de texte**.

1. Connectez un jeu de données qui comprend au moins une colonne contenant du texte.

1. Sélectionnez la langue dans la liste déroulante **Langue**.

1. **Colonne de texte à nettoyer** : Sélectionnez la colonne à prétraiter.

1. **Supprimer les mots vides** : Sélectionnez cette option si vous voulez appliquer une liste prédéfinie de mots vides à la colonne de texte. 

    Les listes de mots vides dépendent de la langue et sont personnalisables.

1. **Lemmatisation** : Sélectionnez cette option si vous voulez que les mots soient représentés sous leur forme canonique. Cette option s’avère utile pour réduire le nombre d’occurrences uniques des jetons de texte autrement similaires.

    Le processus de lemmatisation dépend fortement de la langue.

1. **Détecter les phrases** : Sélectionnez cette option si vous voulez que le module insère une marque de limite de phrase lors de l’exécution de l’analyse.

    Ce module utilise une série de trois barres verticales `|||` pour représenter la ponctuation de fin de phrase.

1. Effectuez des opérations de recherche et remplacement éventuelles à l’aide d’expressions régulières.

    * **Expression régulière personnalisée** : Définissez le texte que vous recherchez.
    * **Chaîne de remplacement personnalisée** : Définissez une valeur de remplacement unique.

1. **Normaliser la casse en minuscules** : Sélectionnez cette option si vous voulez convertir les caractères majuscules ASCII en minuscules.

    Si les caractères ne sont pas normalisés, le même mot en lettres majuscules et en lettres minuscules est considéré comme deux mots différents.

1. Vous pouvez également supprimer les types suivants de caractères ou de séquences de caractères du texte de sortie traité :

    * **Supprimer les chiffres** : Sélectionnez cette option pour supprimer tous les caractères numériques de la langue spécifiée. Les numéros d’identification dépendent du domaine et de la langue. Si les caractères numériques font partie intégrante d’un mot connu, ils ne peuvent pas être supprimés.
    
    * **Supprimer les caractères spéciaux** : Utilisez cette option pour supprimer tous les caractères spéciaux non alphanumériques.
    
    * **Supprimer les caractères en double** : Sélectionnez cette option pour supprimer les caractères supplémentaires dans toutes les séquences qui se répètent plus de deux fois. Par exemple, une séquence comme « aaaaa » serait réduite à « aa ».
    
    * **Supprimer les adresses e-mail** : Sélectionnez cette option pour supprimer toutes les séquences au format `<string>@<string>`.  
    * **Supprimer les URL** : Sélectionnez cette option pour supprimer toutes les séquences qui incluent les préfixes d’URL suivants : `http`, `https`, `ftp`, `www`
    
1. **Développer les contractions verbales** : Cette option s’applique uniquement aux langues qui utilisent les contractions verbales. Actuellement, il s’agit de l’anglais uniquement. 

    Par exemple, en sélectionnant cette option, vous pouvez remplacer la locution *« wouldn’t stay there »* par *« would not stay there »* .

1. **Normaliser les barres obliques inverses en barres obliques** : Sélectionnez cette option pour mapper toutes les instances de `\\` en `/`.

1. **Fractionner les jetons sur les caractères spéciaux** : Sélectionnez cette option si vous voulez couper des mots sur des caractères comme `&`, `-`, etc. Cette option permet également de réduire les caractères spéciaux quand ils se répètent plus de deux fois. 

    Par exemple, la chaîne `MS---WORD` est divisée en trois jetons, `MS`, `-` et `WORD`.

1. Envoyez le pipeline.

## <a name="next-steps"></a>Étapes suivantes

Consultez [l’ensemble des modules disponibles](module-reference.md) pour Azure Machine Learning. 