---
title: Extension d’ontologies
titleSuffix: Azure Digital Twins
description: En savoir plus sur les raisons et les stratégies sous-jacentes à l’extension d’une ontologie
author: baanders
ms.author: baanders
ms.date: 2/12/2021
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 30d7282d6f6f30b34b522991d6fd4e79b194d7dd
ms.sourcegitcommit: 2da83b54b4adce2f9aeeed9f485bb3dbec6b8023
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2021
ms.locfileid: "122771585"
---
# <a name="extending-ontologies"></a>Extension d’ontologies 

Une [ontologie](concepts-ontologies.md) conforme aux standards d’un secteur, par exemple l’[ontologie RealEstateCore basée sur le langage DTDL pour les bâtiments intelligents](https://github.com/Azure/opendigitaltwins-building), est un excellent moyen de commencer à créer votre solution IoT. Les ontologies de secteur fournissent un ensemble complet d’interfaces de base conçues pour votre domaine. Celles-ci sont prêtes à l’emploi dans les services Azure IoT, par exemple Azure Digital Twins. 

Toutefois, il est possible que votre solution ait des besoins spécifiques non couverts par l’ontologie du secteur. Par exemple, vous pouvez souhaiter lier vos jumeaux numériques à des modèles 3D stockés dans un système distinct. Dans ce cas, vous pouvez étendre l’une de ces ontologies pour ajouter vos propres fonctionnalités tout en conservant tous les avantages de l’ontologie d’origine.

Cet article utilise l’ontologie [RealEstateCore](https://www.realestatecore.io/) DTDL pour illustrer des exemples d’extension d’ontologies avec de nouvelles propriétés DTDL. Les techniques décrites ici sont générales. Toutefois, elles peuvent être appliquées à n’importe quelle partie d’une ontologie DTDL avec n’importe quelle fonctionnalité DTDL (télémétrie, propriété, relation, composant ou commande). 

## <a name="realestatecore-space-hierarchy"></a>Hiérarchie Space de RealEstateCore 

Dans l’ontologie RealEstateCore DTDL, la hiérarchie Space permet de définir différents genres d’espace : pièces, bâtiments, zones, etc. La hiérarchie s’étend à partir de chacun de ces modèles pour définir différents genres de pièce, bâtiment et zone. 

Une partie de la hiérarchie ressemble au diagramme ci-dessous. 

:::image type="content" source="media/concepts-ontologies-extend/real-estate-core-original.png" alt-text="Diagramme illustrant une partie de la hiérarchie des espaces RealEstateCore. Il montre des éléments pour Space, Room, ConferenceRoom et Office."::: 

Pour plus d’informations sur l’ontologie RealEstateCore, consultez [Adoption d’ontologies conformes aux normes du secteur](concepts-ontologies-adopt.md#realestatecore-smart-building-ontology).

## <a name="extending-the-realestatecore-space-hierarchy"></a>Extension de la hiérarchie Space de RealEstateCore 

Parfois, votre solution a des besoins spécifiques qui ne sont pas couverts par l’ontologie du secteur. Dans ce cas, l’extension de la hiérarchie vous permet de continuer à utiliser l’ontologie du secteur tout en la personnalisant selon vos besoins. 

Dans cet article, nous abordons deux cas distincts où l’extension de la hiérarchie de l’ontologie est utile : 

* Ajout de nouvelles interfaces pour les concepts qui ne font pas partie de l’ontologie du secteur. 
* Ajout de propriétés supplémentaires (ou relations, composants, télémétries ou commandes) aux interfaces existantes.

### <a name="add-new-interfaces-for-new-concepts"></a>Ajouter de nouvelles interfaces pour de nouveaux concepts 

Dans le cas présent, vous souhaitez ajouter des interfaces pour les concepts nécessaires à votre solution mais qui ne font pas partie de l’ontologie du secteur. Par exemple, si votre solution comporte d’autres types de pièce qui ne sont pas représentés dans l’ontologie RealEstateCore DTDL, vous pouvez les ajouter en les étendant directement à partir des interfaces RealEstateCore. 

L’exemple ci-dessous décrit une solution qui doit représenter des « salles de concentration », lesquelles ne font pas partie de l’ontologie RealEstateCore. Une salle de concentration est un petit espace conçu pour permettre aux gens de se focaliser sur une tâche pendant quelques heures. 

Pour étendre l’ontologie du secteur avec ce nouveau concept, créez une interface qui [s’étend à partir](concepts-models.md#model-inheritance) des interfaces de l’ontologie du secteur. 

Une fois l’interface de la salle de concentration ajoutée, la hiérarchie étendue montre le nouveau type de salle. 

:::image type="content" source="media/concepts-ontologies-extend/real-estate-core-extended-1.png" alt-text="Diagramme illustrant une partie de la hiérarchie des espaces RealEstateCore, y compris le nouvel ajout"::: 

### <a name="add-extra-capabilities-to-existing-interfaces"></a>Ajouter des fonctionnalités supplémentaires à des interfaces existantes 

Dans le cas présent, vous souhaitez ajouter d’autres propriétés (relations, composants, télémétries ou commandes) aux interfaces qui font partie de l’ontologie du secteur.

Dans cette section, vous allez voir deux exemples : 
* Si vous créez une solution qui affiche des dessins 3D d’espaces qui existent déjà dans un système, vous pouvez associer chaque jumeau numérique à son dessin 3D (par ID). Ainsi, quand la solution affiche des informations relatives à l’espace, elle peut également récupérer le dessin 3D du système existant. 
* Si votre solution doit effectuer le suivi de l’état en ligne/hors connexion des salles de conférence, vous pouvez suivre l’état d’une salle de conférence pour l’afficher ou l’utiliser dans des requêtes. 

Vous pouvez implémenter les deux exemples avec de nouvelles propriétés : une propriété `drawingId` qui associe le dessin 3D au jumeau numérique, et une propriété « online » (en ligne) qui indique si la salle de conférence est en ligne ou non. 

En règle générale, il est déconseillé de modifier directement l’ontologie du secteur, car vous pouvez être amené à incorporer plus tard des mises à jour dans votre solution (ce qui entraîne le remplacement de vos ajouts). À la place, vous pouvez effectuer ce genre d’ajout dans votre propre hiérarchie d’interface, laquelle est une extension de l’ontologie RealEstateCore DTDL. Chaque interface que vous créez utilise plusieurs héritages d’interface pour étendre son interface RealEstateCore parente et son interface parente à partir de votre hiérarchie d’interface étendue. Cette approche vous permet d’utiliser à la fois l’ontologie du secteur et vos ajouts. 

Pour étendre l’ontologie du secteur, vous créez vos propres interfaces qui s’étendent à partir des interfaces de l’ontologie du secteur, et vous ajoutez les nouvelles fonctionnalités à vos interfaces étendues. Pour chaque interface à étendre, vous créez une interface. Les interfaces étendues sont écrites en DTDL (consultez la section relative au langage DTDL pour les interfaces étendues plus loin dans ce document). 

Après l’extension de la partie de la hiérarchie présentée ci-dessus, la hiérarchie étendue ressemble au diagramme ci-dessous. Ici, l’interface Space étendue ajoute la propriété `drawingId` qui va contenir un ID associant le jumeau numérique au dessin 3D. De plus, l’interface ConferenceRoom ajoute une propriété « online », qui contient l’état en ligne de la salle de conférence. Grâce à l’héritage, l’interface ConferenceRoom contient toutes les fonctionnalités de l’interface ConferenceRoom de RealEstateCore ainsi que toutes les fonctionnalités de l’interface Space étendue. 

:::image type="content" source="media/concepts-ontologies-extend/real-estate-core-extended-2.png" alt-text="Diagramme illustrant la hiérarchie étendue des espaces RealEstateCore, avec les nouveaux ajouts selon la description"::: 

## <a name="using-the-extended-space-hierarchy"></a>Utilisation de la hiérarchie Space étendue 

Quand vous créez des jumeaux numériques à l’aide de la hiérarchie Space étendue, le modèle de chaque jumeau numérique est celui de la hiérarchie Space étendue (et non celui de l’ontologie du secteur d’origine). Il comprend toutes les fonctionnalités de l’ontologie du secteur ainsi que les interfaces étendues via l’héritage d’interface.

Le modèle de chaque jumeau numérique correspond à une interface provenant de la hiérarchie étendue, comme illustré dans le diagramme ci-dessous. 
 
:::image type="content" source="media/concepts-ontologies-extend/ontology-with-models.png" alt-text="Diagramme illustrant la hiérarchie étendue des espaces RealEstateCore, y compris les modèles connectés Space, Room, ConferenceRoom, Office et FocusRoom"::: 

Durant l’interrogation de jumeaux numériques à l’aide de l’ID de modèle (opérateur `IS_OF_MODEL`), les ID de modèle de la hiérarchie étendue doivent être utilisés. Par exemple : `SELECT * FROM DIGITALTWINS WHERE IS_OF_MODEL('dtmi:com:example:Office;1')`. 

## <a name="contributing-back-to-the-original-ontology"></a>Contribution à l’ontologie d’origine 

Parfois, vous allez étendre l’ontologie du secteur de manière très utile pour la plupart des utilisateurs de l’ontologie. Dans ce cas, vos extensions peuvent contribuer à l’ontologie d’origine. Dans la mesure où chaque ontologie a un processus de contribution différent, consultez le dépôt GitHub de l’ontologie pour plus d’informations sur cet aspect. 

## <a name="dtdl-for-new-interfaces"></a>Langage DTDL des nouvelles interfaces 

Le langage DTDL des nouvelles interfaces qui s’étendent directement à partir de l’ontologie du secteur ressemble à ce qui suit. 

```json
{
  "@id": "dtmi:com:example:FocusRoom;1", 
  "@type": "interface", 
  "extends": "dtmi:digitaltwins:rec_3_3:building:Office;1", 
  "@context": "dtmi:dtdl:context;2" 
} 
```

## <a name="dtdl-for-extended-interfaces"></a>Langage DTDL des interfaces étendues 

Le langage DTDL des interfaces étendues, limité à la partie décrite ci-dessus, ressemble à ceci. 

```json
[
  {
    "@id": "dtmi:com:example:Space;1",
    "@type": "Interface",
    "extends": "dtmi:digitaltwins:rec_3_3:core:Space;1",
    "contents": [
      {
        "@type": "Property",
        "name": "drawingid",
        "schema": "string"
      }
    ],
    "@context": "dtmi:dtdl:context;2"
  },
  {
    "@id": "dtmi:com:example:Room;1",
    "@type": "Interface",
    "extends": [
      "dtmi:digitaltwins:rec_3_3:core:Room;1",
      "dtmi:com:example:Space;1"
    ],
    "@context": "dtmi:dtdl:context;2"
  },
  {
    "@id": "dtmi:com:example:ConferenceRoom;1",
    "@type": "Interface",
    "extends": [
      "dtmi:digitaltwins:rec_3_3:building:ConferenceRoom;1",
      "dtmi:com:example:Room;1"
    ],
    "contents": [
      {
        "@type": "Property",
        "name": "online",
        "schema": "boolean"
      }
    ],
    "@context": "dtmi:dtdl:context;2"
  },
  {
    "@id": "dtmi:com:example:Office;1",
    "@type": "Interface",
    "extends": [
      "dtmi:digitaltwins:rec_3_3:building:Office;1", 
      "dtmi:com:example:Room;1" 
    ],
    "@context": "dtmi:dtdl:context;2" 
  }, 
  {
    "@id": "dtmi:com:example:FocusRoom;1", 
    "@type": "Interface", 
    "extends": "dtmi:com:example:Office;1", 
    "@context": "dtmi:dtdl:context;2" 
  }
]
``` 

## <a name="next-steps"></a>Étapes suivantes

Passez au développement de modèles à partir d’ontologies : [Utilisation de stratégies d’ontologie pour le développement de modèles](concepts-ontologies.md#using-ontology-strategies-in-a-model-development-path).