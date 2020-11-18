---
title: Informations de référence sur le module d’extraction des caractéristiques de N-grammes du texte
titleSuffix: Azure Machine Learning
description: Découvrez comment caractériser des données texte à l’aide du module d’extraction des caractéristiques de N-grammes du texte dans le concepteur Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/01/2019
ms.openlocfilehash: c4d9c7c2cb7a0a86824a373f1b64044b6dcd6c20
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93420799"
---
# <a name="extract-n-gram-features-from-text-module-reference"></a>Informations de référence sur le module d’extraction des caractéristiques de N-grammes du texte

Cet article décrit un module dans le concepteur Azure Machine Learning. Utilisez le module Extract N-Gram Features from Text (Extraire les caractéristiques de N-grammes du texte) pour *caractériser* des données texte non structurées. 

## <a name="configuration-of-the-extract-n-gram-features-from-text-module"></a>Configuration du module Extract N-Gram Features from Text

Le module prend en charge les scénarios suivants pour l’utilisation d’un dictionnaire de N-grammes :

* [Créer un dictionnaire de N-grammes](#create-a-new-n-gram-dictionary) à partir d’une colonne de texte libre.

* [Utiliser un ensemble existant de caractéristiques de texte](#use-an-existing-n-gram-dictionary) pour caractériser une colonne de texte libre.

* [Noter ou publier un modèle](#score-or-publish-a-model-that-uses-n-grams) utilisant N-grammes.

### <a name="create-a-new-n-gram-dictionary"></a>Créer un dictionnaire de N-grammes

1.  Ajoutez le module Extraire les caractéristiques de N-grammes du texte à votre pipeline et connectez le jeu de données contenant le texte que vous souhaitez traiter.

1.  Utilisez **Text column** (Colonne Texte) pour choisir une colonne de type chaîne contenant le texte à extraire. Étant donné que les résultats sont détaillés, vous ne pouvez traiter qu’une seule colonne à la fois.

1. Définissez **Vocabulary mode** (Mode vocabulaire) sur **Create** (Créer) pour indiquer que vous créez une liste de caractéristiques de N-grammes. 

1. Définissez **N-Grams size** (Taille de N-grammes) pour indiquer la taille *maximale* de N-grammes à extraire et à stocker. 

    Par exemple, si vous entrez 3, des unigrammes, des digrammes et des trigrammes sont créés.

1. **Weighting function** de (Fonction de pondération) spécifie comment générer le vecteur de caractéristique de document et comment extraire le vocabulaire des documents.

    * **Binary Weight** (Pondération binaire) : affecte une valeur de présence binaire aux N-grammes extraits. La valeur de chaque N-gramme est 1 quand celui-ci existe dans le document, et 0 dans le cas contraire.

    * **TF Weight** (Pondération TF) : attribue un score de fréquence du terme (TF) aux N-grammes extraits. La valeur de chaque N-gramme est sa fréquence d’occurrence dans le document.

    * **IDF Weight** (Pondération IDF) : attribue un score de fréquence de document inverse (IDF) aux N-grammes extraits. La valeur de chaque N-grammes est le journal de taille de corpus divisé par sa fréquence d’occurrence dans le corpus.
    
      `IDF = log of corpus_size / document_frequency`
 
    *  **TF-IDF Weight** (Pondération TF-IDF) : attribue un score de fréquence de terme/fréquence de document inverse (TF/IDF) aux N-grammes extraits. La valeur de chaque N-grammes est son score TF multiplié par son score IDF.

1. Définissez **Minimum word length** (Longueur minimale du mot) sur le nombre minimal de lettres qui peuvent être utilisées dans un *mot unique* dans un N-gramme.

1. Utlisez **Minimum word length** (Longueur minimale du mot) pour définir le nombre minimal de lettres qui peuvent être utilisées dans un *mot unique* dans un N-gramme.

    Par défaut, jusqu’à 25 caractères par mot ou par jeton sont autorisés.

1. Avec l’option **Minimum n-gram document absolute frequency** (Fréquence absolue minimale de document N-grammes), définissez le nombre minimal d’occurrences d’un N-gramme pour que celui-ci soit inclus dans le dictionnaire de N-grammes. 

    Par exemple, si vous utilisez la valeur par défaut 5, tout N-grammes doit apparaître au moins cinq fois dans le corpus pour être inclus dans le dictionnaire de N-grammes. 

1.  Définissez **Maximum n-gram document ratio** (Ratio maximal de document N-grammes) sur le ratio maximal du nombre de lignes contenant un N-grammes particulier, sur le nombre de lignes dans le corpus global.

    Par exemple, un ratio de 1 indique que, même si un N-grammes spécifique est présent dans chaque ligne, le N-grammes peut être ajouté au dictionnaire de N-grammes. Plus généralement, un mot qui apparaît dans chaque ligne est considéré comme un mot parasite et est supprimé. Pour filtrer les mots parasites dépendants du domaine, essayez de réduire ce ratio.

    > [!IMPORTANT]
    > Le taux d’occurrence de mots particuliers n’est pas uniforme. Il varie d’un document à l’autre. Par exemple, si vous analysez des commentaires de clients sur un produit spécifique, la fréquence du nom du produit peut être très élevée et se rapprocher de celle d’un mot parasite, mais être un terme significatif dans d’autres contextes.

1. Sélectionnez l’option **Normalize n-gram feature vectors** (Normaliser les vecteurs de caractéristique N-grammes) pour normaliser les vecteurs de caractéristique. Si cette option est activée, chaque vecteur de caractéristiques de N-grammes est divisé par sa norme L2.

1. Envoyez le pipeline.

### <a name="use-an-existing-n-gram-dictionary"></a>Utiliser un dictionnaire de N-grammes existant

1.  Ajoutez le module Extraire les caractéristiques de N-grammes du texte à votre pipeline et connectez le jeu de données contenant le texte à traiter au port **Jeu de données**.

1.  Utilisez **Text column** (Colonne Texte) pour sélectionner la colonne de texte contenant le texte que vous souhaitez caractériser. Par défaut, le module sélectionne toutes les colonnes de type **chaîne**. Pour des résultats optimaux, traitez une seule colonne à la fois.

1. Ajoutez le jeu de données enregistré qui contient un dictionnaire de N-grammes généré précédemment et connectez-le au port **Input vocabulary** (Vocabulaire d’entrée). Vous pouvez également connecter la sortie **Result vocabulary** (Vocabulaire de résultat) d’une instance en amont du module Extract N-Gram Features from Text.

1. Pour **Vocabulary mode** (Mode vocabulaire), sélectionnez l’option de mise à jour **ReadOnly** (Lecture seule) dans la liste déroulante.

   L’option **ReadOnly** représente le corpus d’entrée pour le vocabulaire d’entrée. Au lieu de calculer les fréquences de termes à partir du nouveau jeu de données texte (sur l’entrée de gauche), les pondérations N-grammes du vocabulaire d’entrée sont appliquées telles quelles.

   > [!TIP]
   > Utilisez cette option lorsque vous calculez le score d’un classifieur de texte.

1.  Pour toutes les autres options, consultez les descriptions des propriétés dans la [section précédente](#create-a-new-n-gram-dictionary).

1.  Envoyez le pipeline.

### <a name="score-or-publish-a-model-that-uses-n-grams"></a>Noter ou publier un modèle qui utilise des N-grammes

1.  Copiez le module **Extract N-Gram Features from Text** (Extraire les caractéristiques de N-grammes du texte) du flux de données d’apprentissage dans le flux de données de notation.

1.  Connectez la sortie **Result Vocabulary** (Vocabulaire de résultat) du flux de données d’apprentissage à **Input Vocabulary** (Vocabulaire d’entrée) sur le flux de travail de scoring.

1.  Dans le flux de travail de scoring, modifiez le module Extract N-Gram Features from Text et définissez le paramètre **Vocabulary mode** sur **ReadOnly**. Laissez tout le reste identique.

1.  Pour publier le pipeline, enregistrez la sortie **Vocabulaire de résultat** en tant que jeu de données.

1.  Connectez le jeu de données enregistré au module Extract N-Gram Features from Text dans votre graphe de scoring.

## <a name="results"></a>Résultats

Le module Extract N-Gram Features from Text crée deux types de sortie : 

* **Results dataset** (Jeu de données de résultats) : cette sortie est un résumé du texte analysé combiné avec les N-grammes extraits. Les colonnes que vous n’avez pas sélectionnées dans l’option **Text column** (Colonne Texte) sont transmises à la sortie. Pour chaque colonne de texte que vous analysez, le module génère les colonnes suivantes :

  * **Matrix of n-gram occurrences** (Matrice d’occurrences de N-grammes) : Le module génère une colonne pour chaque N-grammes trouvé dans le corpus total, et ajoute une note dans chaque colonne pour indiquer la pondération de N-grammes pour cette ligne. 

* **Result vocabulary** (Vocabulaire de résultat) : Le vocabulaire contient le dictionnaire de N-grammes réel, ainsi que les notes de fréquence de termes qui sont générées dans le cadre de l’analyse. Vous pouvez enregistrer le jeu de données pour le réutiliser avec un autre ensemble d’entrées ou en vue d’une mise à jour ultérieure. Vous pouvez également réutiliser le vocabulaire pour la modélisation et la notation.

### <a name="result-vocabulary"></a>Vocabulaire de résultat

Le vocabulaire contient le dictionnaire de N-grammes, ainsi que les notes de fréquence de termes qui sont générées dans le cadre de l’analyse. Les scores DF et IDF sont générés indépendamment des autres options.

+ **ID** : Identificateur généré pour chaque N-grammes.
+ **nGram** (N-grammes) : le N-grammes. Les espaces ou autres séparateurs de mots sont remplacés par le caractère de soulignement.
+ **DF** : Note de fréquence de terme pour le N-grammes dans le corpus d’origine.
+ **IDF** : Note de fréquence de document inverse pour le N-grammes dans le corpus d’origine.

Vous pouvez mettre à jour ce jeu de données manuellement, mais vous risquez d’introduire des erreurs. Par exemple :

* Une erreur est signalée si le module trouve des lignes en double avec la même clé dans le vocabulaire d’entrée. Veillez à ce qu’il n’y ait pas deux lignes contenant le même mot dans vocabulaire.
* Le schéma d’entrée des jeux de données de vocabulaire doit correspondre exactement, y compris les noms de colonnes et les types de colonnes. 
* La colonne **ID** et la colonne **DF** doivent être de type entier (integer). 
* La colonne **IDF** doit être de type flottant (float).

> [!Note]
> Ne connectez pas la sortie de données directement au module Train Model (Entraîner le modèle). Vous devez supprimer les colonnes de texte libre avant qu’elles ne soient incluses dans le module. Sinon, les colonnes de texte libre seront traitées comme des caractéristiques de catégorie.

## <a name="next-steps"></a>Étapes suivantes

Consultez [l’ensemble des modules disponibles](module-reference.md) pour Azure Machine Learning.
