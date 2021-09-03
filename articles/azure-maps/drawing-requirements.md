---
title: Conditions du package de dessin dans Microsoft Azure Maps Creator
description: Découvrez les exigences du package de dessin pour convertir les fichiers de conception de votre bâtiment en données cartographiques.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/02/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
ms.openlocfilehash: 2e6b380b040697a56179f5fec7f7d10796f035fd
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122532606"
---
# <a name="drawing-package-requirements"></a>Exigences du package de dessin

Vous pouvez convertir les packages de dessin chargés en données cartographiques à l’aide du [service de conversion d’Azure Maps](/rest/api/maps/v2/conversion). Cet article décrit les exigences du package de dessin pour l’API de conversion. Pour voir un exemple de package, vous pouvez télécharger l’exemple [Package de dessin](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

Pour obtenir un guide sur la préparation de votre package de dessin, consultez le [Guide du package de dessin de conversion](drawing-package-guide.md).


## <a name="prerequisites"></a>Prérequis

Le package Dessin comprend des dessins enregistrés au format DWG. Il s’agit du format de fichier natif du logiciel AutoCAD® d’Autodesk.

Vous pouvez choisir n’importe quel logiciel de CAO pour produire les dessins du package de dessin.  

Le [service de conversion d’Azure Maps](/rest/api/maps/v2/conversion) convertit le package de dessin en données cartographiques. Le service de conversion fonctionne avec le format de fichier DWG AutoCAD `AC1032`.


## <a name="glossary-of-terms"></a>Glossaire des termes

Voici quelques termes et définitions importants pour faciliter la lecture de cet article.

| Terme  | Définition |
|:-------|:------------|
| Couche | Couche AutoCAD DWG du fichier de dessin.|
| Entité | Entité AutoCAD DWG du fichier de dessin. |
| Xref  | Fichier au format DWG AutoCAD attaché au dessin principal en tant que référence externe.  |
| Level | Zone d’un immeuble à une élévation définie. Par exemple, l’étage d’un immeuble. |
| Fonctionnalité | Instance d’un objet produit à partir du service de conversion qui associe une géométrie à des informations de métadonnées. |
| Classes de caractéristiques | Blueprint commun pour les caractéristiques. Par exemple, une *unité* est une classe de caractéristiques et un *bureau* est une caractéristique. |

## <a name="drawing-package-structure"></a>Structure de package de dessin

Un package de dessin est une archive. zip contenant les fichiers suivants :

- Fichiers DWG au format de fichier DWG AutoCAD.
- Fichier _manifest.json_ qui décrit les fichiers DWG dans le package de dessin.

Le package de dessin doit être compressé dans un fichier d’archive unique, avec l’extension .zip. Les fichiers DWG peuvent être organisés d’une façon quelconque dans le package, mais le fichier manifeste doit se trouver dans le répertoire racine du package compressé. Les sections suivantes détaillent les exigences relatives aux fichiers DWG, au fichier manifeste et au contenu de ces fichiers. Pour consulter un exemple de package, vous pouvez télécharger l’exemple [Package de dessin](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

## <a name="dwg-file-conversion-process"></a>Processus de conversion de fichier DWG

Le [service de Conversion Azure Maps](/rest/api/maps/v2/conversion) effectue les opérations suivantes sur chaque fichier DWG :

- Extrait les classes de fonctionnalités :
    - Niveaux
    - Unités
    - Zones
    - Ouvertures
    - Murs
    - Pénétrations verticales
- Produit une fonctionnalité *Installation*.  
- Produit un ensemble minimal de fonctionnalités de catégorie par défaut à référencer par d’autres fonctionnalités :
    - room
    - structure
    - wall
    - opening.door
    - zone
    - installation
    
## <a name="dwg-file-requirements"></a>Exigences du fichier DWG

Un fichier DWG unique est requis pour chaque niveau du bâtiment. Toutes les données d’un même niveau doivent être contenues dans un seul fichier DWG.  Toute référence externe (_xref_) doit être liée au dessin parent. Par exemple, une installation à trois niveaux présentera trois fichiers DWG dans le package de dessin.

Chaque fichier DWG doit respecter les conditions suivantes :

- Le fichier DWG doit définir les calques _Extérieur_ et _Unité_. Il peut éventuellement définir les calques facultatifs suivants : _Mur_, _Porte_, _UnitLabel_, _Zone_ et _ZoneLabel_.
- Le fichier DWG ne peut pas contenir de fonctionnalités à partir de plusieurs niveaux.
- Le fichier DWG ne peut pas contenir de fonctionnalités à partir de plusieurs installations.
- Le DWG doit faire référence au même système de mesure et à la même unité de mesure que les autres fichiers DWG dans le package de dessin.

## <a name="dwg-layer-requirements"></a>Exigences relatives au calque DWG

Chaque calque DWG doit respecter les règles suivantes :

- Un calque DWG doit contenir les fonctionnalités d’une seule classe. Par exemple, les unités et les murs ne peuvent pas se trouver dans le même calque.
- Une seule classe de fonctionnalités peut être représentée par plusieurs calques.
- Les polygones à intersection automatique sont autorisés, mais automatiquement réparés. Lors de cette réparation, le [service de conversion Azure Maps Conversion](/rest/api/maps/v2/conversion) génère un avertissement. Il est recommandé d’inspecter manuellement les résultats réparés, car ils peuvent ne pas correspondre aux résultats attendus.
- Chaque calque contient une liste de types d’entités pris en charge. Tous les autres types d’entités sont ignorés. Par exemple, les entités de texte ne sont pas prises en charge sur le calque de mur.

Le tableau ci-dessous présente les types d’entités pris en charge et les caractéristiques cartographiques converties pour chaque couche. Si un calque contient des types d’entités non pris en charge, le [service de conversion d’Azure Maps](/rest/api/maps/v2/conversion) ignore ces entités.  

| Couche | Types d’entités | Caractéristiques converties |
| :----- | :-------------------| :-------
| [Extérieur](#exterior-layer) | Polygone, Polyligne (fermée), Cercle, Ellipse (fermée) | Niveaux
| [Unité](#unit-layer) |  Polygone, Polyligne (fermée), Cercle, Ellipse (fermée) |  Unités et Pénétrations verticales
| [Mur](#wall-layer)  | Polygone, Polyligne (fermée), Cercle, Ellipse (fermée), Structures |
| [Porte](#door-layer) | Polygone, Polyligne, Ligne, Arc circulaire, Cercle | Ouvertures
| [Zone](#zone-layer) | Polygone, Polyligne (fermée), Cercle, Ellipse (fermée) | Zones
| [UnitLabel](#unitlabel-layer) | Texte (ligne unique) | Non applicable. Ce calque ne peut ajouter des propriétés aux caractéristiques de l’unité qu’à partir du calque Unités. Pour plus d’informations, consultez [Calque UnitLabel](#unitlabel-layer).
| [ZoneLabel](#zonelabel-layer) | Texte (ligne unique) | Non applicable. Ce calque ne peut ajouter des propriétés aux caractéristiques de la zone qu’à partir du calque ZonesLayer. Pour plus d’informations, consultez [Calque ZoneLabel](#zonelabel-layer).

Les sections suivantes détaillent les exigences de chaque calque.

### <a name="exterior-layer"></a>Calque Extérieur

Le fichier DWG pour chaque niveau doit contenir un calque pour définir le périmètre de ce niveau. Ce calque est appelé calque *Extérieur*. Par exemple, si un bâtiment contient deux niveaux, il doit avoir deux fichiers DWG, avec un calque Extérieur pour chaque fichier.

Quel que soit le nombre de dessins d’entité dans le calque Extérieur, le [jeu de données du bâtiment obtenu](tutorial-creator-indoor-maps.md#create-a-feature-stateset) ne contient qu’une seule caractéristique de niveau pour chaque fichier DWG. De plus :

- Les calques extérieurs doivent être dessinés en tant que Polygone, Polyligne (fermée), Cercle ou Ellipse (fermée).
- Les calques extérieurs peuvent se chevaucher, mais sont fusionnés dans une seule géométrie.
- La caractéristique Niveau obtenue doit être d’au moins 4 mètres carrés.
- La caractéristique Niveau obtenue ne doit pas être supérieure à 400 000 mètres carrés.

Si le calque contient plusieurs polylignes qui se chevauchent, celles-ci sont fusionnées en une seule caractéristique Niveau. Si le calque contient plusieurs polylignes ne se chevauchant pas, la caractéristique Niveau obtenue a une représentation multi-polygonale.

Vous pouvez consulter un exemple de calque Extérieur en tant que calque contour dans l’[exemple de package de dessin](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

### <a name="unit-layer"></a>Calque Unité

Le fichier DWG pour chaque niveau doit définir un calque contenant des unités. Les unités sont des espaces navigables dans le bâtiment, tels que des bureaux, des couloirs, des escaliers et des ascenseurs. Si la propriété `VerticalPenetrationCategory` est définie, les unités navigables qui s’étendent sur plusieurs niveaux, telles que les ascenseurs et les escaliers, sont converties en caractéristiques Pénétration verticale. Une valeur `setid` est attribuée aux caractéristiques de pénétration verticale qui se chevauchent.

Le calque Unités doit respecter les exigences suivantes :

- Les unités doivent être dessinées en tant que Polygone, Polyligne (fermée), Cercle ou Ellipse (fermée).
- Les unités doivent se trouver à l’intérieur des limites du périmètre extérieur du bâtiment.
- Les unités ne doivent pas se chevaucher partiellement.
- Les unités ne doivent pas contenir de géométrie avec auto-intersection.

Nommez une unité en créant un objet texte dans le calque UnitLabel, puis placez l’objet à l’intérieur des limites de l’unité. Pour plus d’informations, consultez [Calque UnitLabel](#unitlabel-layer).

Vous pouvez consulter un exemple de calque Unités dans l’[exemple de package de dessin](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

### <a name="wall-layer"></a>Calque Mur

Le fichier DWG pour chaque niveau peut contenir un calque qui définit les étendues physiques de murs, de colonnes et d’autres structures de bâtiment.

- Les murs doivent être dessinés en tant que Polygone, Polyligne (fermée), Cercle ou Ellipse (fermée).
- Les calques Mur doivent contenir uniquement une géométrie interprétée comme une structure de bâtiment.

Vous pouvez consulter un exemple de calque Murs dans l’[exemple de package de dessin](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

### <a name="door-layer"></a>Calque Porte

Vous pouvez inclure un calque DWG contenant des portes. Chaque porte doit chevaucher le bord d’une unité du calque Unités.

Les ouvertures de portes d’un jeu de données Azure Maps sont représentées sous la forme d’un segment d’une seule ligne qui chevauche plusieurs limites d’unité. Les images suivantes montrent comment convertir une géométrie du calque de porte en caractéristiques d’ouverture dans un jeu de données.

![Quatre graphiques illustrant les étapes pour générer des ouvertures](./media/drawing-requirements/opening-steps.png)

### <a name="zone-layer"></a>Calque Zones

Le fichier DWG pour chaque niveau peut contenir un calque Zones qui définit les étendues physiques de zones. Une zone est un espace non navigable qui peut être nommé et rendu. Les zones peuvent s’étendre sur plusieurs niveaux et sont regroupées à l’aide de la propriété zoneSetId.

- Les zones doivent être dessinées en tant que Polygone, Polyligne (fermée) ou Ellipse (fermée).
- Les zones peuvent se chevaucher.
- Les zones peuvent se trouver à l’intérieur ou à l’extérieur du périmètre extérieur du bâtiment.

Nommez une zone en créant un objet texte dans le calque ZoneLabel et en plaçant l’objet texte à l’intérieur des limites de la zone. Pour plus d’informations, consultez [Calque ZoneLabel](#zonelabel-layer).

Vous pouvez consulter un exemple de calque Zone dans l’[exemple de package de dessin](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

### <a name="unitlabel-layer"></a>Claque UnitLabel

Le fichier DWG pour chaque niveau peut contenir un calque UnitLabel. Le calque UnitLabel ajoute une propriété de nom aux unités extraites du calque Unité. Les unités ayant une propriété de nom peuvent avoir plus de détails spécifiés dans le fichier manifeste.

- Les étiquettes d’unité doivent être des entités texte d’une seule ligne.
- Les étiquettes d’unité doivent entièrement se trouver dans les limites de leur unité.
- Les unités ne doivent pas contenir plusieurs entités texte dans le calque UnitLabel.

Vous pouvez consulter un exemple de calque UnitLabel dans l’[exemple de package de dessin](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

### <a name="zonelabel-layer"></a>Calque ZoneLabel

Le fichier DWG pour chaque niveau peut contenir un calque ZoneLabel. Ce calque ajoute une propriété de nom aux zones extraites du claque Zones. Les zones ayant une propriété de nom peuvent avoir plus de détails spécifiés dans le fichier manifeste.

- Les étiquettes de zone doivent être des entités texte d’une seule ligne.
- Les étiquettes de zone doivent se trouver dans les limites de leur zone.
- Les zones ne doivent pas contenir plusieurs entités texte dans le calque ZoneLabel.

Vous pouvez consulter un exemple de calque ZoneLabel dans l’[exemple de package de dessin](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

## <a name="manifest-file-requirements"></a>Exigences du fichier manifeste

Le dossier zip doit contenir un fichier manifeste au niveau racine du répertoire, et le fichier doit être nommé **manifest.json**. Il décrit les fichiers DWG pour permettre au [service de conversion d’Azure Maps](/rest/api/maps/v2/conversion) d’analyser leur contenu. Seuls les fichiers identifiés par le manifeste sont ingérés. Les fichiers qui se trouvent dans le dossier zip mais qui ne sont pas correctement répertoriés dans le manifeste sont ignorés.

Les chemins d’accès aux fichiers, dans l’objet `buildingLevels` du fichier manifeste doivent être relatifs à la racine du dossier zip. Le nom du fichier DWG doit correspondre exactement au nom du niveau du bâtiment. Par exemple, un fichier DWG pour le niveau « sous-sol » est nommé « sous-sol.dwg ». Un fichier DWG pour le niveau 2 est nommé « niveau_2.dwg ». Si votre nom de niveau comporte une espace, remplacez-la par un trait de soulignement.

Bien que des exigences s’appliquent à l’utilisation des objets de manifeste, tous les objets ne sont pas obligatoires. Le tableau suivant répertorie les objets obligatoires et facultatifs pour la version 1.1 du [service de conversion d’Azure Maps](/rest/api/maps/v2/conversion).

>[!NOTE]
> Sauf indication contraire, toutes les propriétés dotées d’un type de propriété de chaîne autorisent 1 000 caractères.


| Object | Obligatoire | Description |
| :----- | :------- | :------- |
| `version` | true |Version du schéma du manifeste. Actuellement, seule la version 1.1 est prise en charge.|
| `directoryInfo` | true | Décrit les coordonnées géographiques du bâtiment et les informations de contact. Peut également être utilisée pour décrire les coordonnées géographiques et les informations de contact d’un occupant. |
| `buildingLevels` | true | Spécifie les niveaux des bâtiments et les fichiers contenant la conception des niveaux. |
| `georeference` | true | Contient des informations géographiques numériques pour le dessin du bâtiment. |
| `dwgLayers` | true | Répertorie les noms des calques, et chaque calque répertorie les noms de ses propres caractéristiques. |
| `unitProperties` | false | Peut être utilisé pour insérer plus de métadonnées pour les caractéristiques d’unité. |
| `zoneProperties` | false | Peut être utilisé pour insérer plus de métadonnées pour les caractéristiques de zone. |

Les sections suivantes détaillent les exigences pour chaque objet.

### `directoryInfo`

| Propriété  | Type | Obligatoire | Description |
|-----------|------|----------|-------------|
| `name`      | string | true   |  Nom du bâtiment. |
| `streetAddress`|    string |    false    | Adresse du bâtiment. |
|`unit`     | string    |  false    |  Unité dans le bâtiment. |
| `locality` |    string |    false |    Nom d’une ville, d’une zone, d’un quartier ou d’une région.|
| `adminDivisions` |    Tableau de chaînes JSON |    false     | Tableau contenant les désignations d’adresses. Par exemple : (Pays, état) Utilisez les codes de pays ISO 3166 et les codes d’état/territoire ISO 3166-2. |
| `postalCode` |    string    | false    | Code de tri de courrier postal. |
| `hoursOfOperation` |    string |     false | Suit le format d’[heures d’ouvertures OSM](https://wiki.openstreetmap.org/wiki/Key:opening_hours/specification). |
| `phone`    | string |    false |    Numéro de téléphone associé au bâtiment. |
| `website`    | string |    false    | Site web associé au bâtiment. |
| `nonPublic` |    bool    | false | Indicateur spécifiant si le bâtiment est ouvert au public. |
| `anchorLatitude` | numeric |    false | Latitude d’une ancre de bâtiment (punaise). |
| `anchorLongitude` | numeric |    false | Longitude d’une ancre de bâtiment (punaise). |
| `anchorHeightAboveSeaLevel`  | numeric | false | Hauteur du rez-de-chaussée du bâtiment par rapport au niveau de la mer, exprimée en mètres. |
| `defaultLevelVerticalExtent` | numeric | false | Hauteur par défaut (épaisseur) d’un niveau de ce bâtiment à utiliser quand la valeur `verticalExtent` d’un niveau n’est pas définie. |

### `buildingLevels`

L’objet `buildingLevels` contient un tableau JSON de niveaux de bâtiments.

| Propriété  | Type | Obligatoire | Description |
|-----------|------|----------|-------------|
|`levelName`    |string    |true |    Nom de niveau descriptif. Par exemple : Étage 1, Hall, Zone de stationnement bleue ou Sous-sol.|
|`ordinal` | entier |    true | Détermine l’ordre vertical des niveaux. Toute bâtiment doit avoir un niveau dont la valeur ordinale est 0. |
|`heightAboveFacilityAnchor` | numeric | false |    Hauteur de niveau au-dessus de l’ancre, exprimée en mètres. |
| `verticalExtent` | numeric | false | Hauteur du sol au plafond (épaisseur) du niveau, exprimée en mètres. |
|`filename` |    string |    true |    Chemin d’accès dans le système de fichiers du dessin de CAO d’un niveau de bâtiment. Il doit être relatif à la racine du fichier zip du bâtiment. |

### `georeference`

| Propriété  | Type | Obligatoire | Description |
|-----------|------|----------|-------------|
|`lat`    | numeric |    true |    Représentation décimale de la latitude en degrés à l’origine du dessin du bâtiment. Les coordonnées de l’origine doivent être conformes à la norme WGS84 Web Mercator (`EPSG:3857`).|
|`lon`    |numeric|    true|    Représentation décimale de la longitude en degrés à l’origine du dessin du bâtiment. Les coordonnées de l’origine doivent être conformes à la norme WGS84 Web Mercator (`EPSG:3857`). |
|`angle`|    numeric|    true|   Angle, exprimé en degrés, entre le nord réel et l’axe vertical (Y) du dessin dans le sens des aiguilles d’une montre.   |

### `dwgLayers`

| Propriété  | Type | Obligatoire | Description |
|-----------|------|----------|-------------|
|`exterior`    |tableau de chaînes|    true|    Noms des calques définissant le profil extérieur du bâtiment.|
|`unit`|    tableau de chaînes|    true|    Noms des calques définissant les unités.|
|`wall`|    tableau de chaînes    |false|    Noms des calques définissant les murs.|
|`door`    |tableau de chaînes|    false   | Noms des calques définissant les portes.|
|`unitLabel`    |tableau de chaînes|    false    |Noms des calques définissant les noms d’unités.|
|`zone` | tableau de chaînes    | false    | Noms des calques définissant les zones.|
|`zoneLabel` | tableau de chaînes |     false |    Noms des calques définissant les noms de zones.|

### `unitProperties`

L’objet `unitProperties` contient un tableau JSON de propriétés d’unité.

| Propriété  | Type | Obligatoire | Description |
|-----------|------|----------|-------------|
|`unitName`    |string    |true    |Nom de l’unité à associer à cet enregistrement de `unitProperty`. Cet enregistrement n’est valide que si une étiquette correspondant à `unitName` est trouvée dans le ou les calques `unitLabel`. |
|`categoryName`|    string|    false    |Objectif de l’unité. Une liste de valeurs que les styles de rendu fournis peuvent utiliser est disponible [ici](https://atlas.microsoft.com/sdk/javascript/indoor/0.1/categories.json). |
|`occupants`    |Tableau d’objets directoryInfo |false    |Liste d’occupants de l’unité. |
|`nameAlt`|    string|    false|    Autre nom de l’unité. |
|`nameSubtitle`|    string    |false|    Sous-titre de l’unité. |
|`addressRoomNumber`|    string|    false|    Numéro de salle, d’appartement ou de suite de l’unité.|
|`verticalPenetrationCategory`|    string|    false| Lorsque cette propriété est définie, la caractéristique qui en résulte est une pénétration verticale (VRT) plutôt qu’une unité. Vous pouvez utiliser les pénétrations verticales pour accéder à d’autres caractéristiques de pénétrations verticales dans les niveaux supérieurs ou inférieurs. Pénétration verticale est un nom de [Catégorie](https://aka.ms/pa-indoor-spacecategories). Si cette propriété est définie, la propriété `categoryName` est remplacée par `verticalPenetrationCategory`. |
|`verticalPenetrationDirection`|    string|    false    |Si la valeur `verticalPenetrationCategory` est définie, définissez éventuellement la direction de déplacement valide. Les valeurs autorisées sont `lowToHigh`, `highToLow`, `both` et `closed`. La valeur par défaut est `both`. Cette valeur respecte la casse.|
| `nonPublic` | bool | false | Indique si l’unité est ouverte au public. |
| `isRoutable` | bool | false | Lorsque cette propriété est définie sur `false`, vous ne pouvez pas accéder ou traverser l’unité. La valeur par défaut est `true`. |
| `isOpenArea` | bool | false | Permet à l’agent de navigation d’entrer dans l’unité sans qu’il soit nécessaire d’attacher une ouverture à celle-ci. Par défaut, cette valeur est définie sur `true` pour les unités sans ouvertures, et sur `false` pour les unités avec ouvertures. La définition manuelle de `isOpenArea` sur `false` sur une unité sans ouvertures entraîne un avertissement, car l’unité résultante n’est pas accessible par un agent de navigation.|

### `zoneProperties`

L’objet `zoneProperties` contient un tableau JSON de propriétés de zone.

| Propriété  | Type | Obligatoire | Description |
|-----------|------|----------|-------------|
|zoneName        |string    |true    |Nom de zone à associer à l’enregistrement `zoneProperty`. Cet enregistrement n’est valide que si une étiquette correspondant à `zoneName` est trouvée dans le calque `zoneLabel` de la zone.  |
|categoryName|    string|    false    |Objectif de la zone. Une liste de valeurs que les styles de rendu fournis peuvent utiliser est disponible [ici](https://atlas.microsoft.com/sdk/javascript/indoor/0.1/categories.json).|
|zoneNameAlt|    string|    false    |Autre nom de la zone.  |
|zoneNameSubtitle|    string |    false    |Sous-titre de la zone. |
|zoneSetId|    string |    false    | ID défini pour établir une relation entre plusieurs zones pour qu’elles puissent être interrogées ou sélectionnées en tant que groupe. Il peut s’agir, par exemple, de zones qui s’étendent sur plusieurs niveaux. |

### <a name="sample-drawing-package-manifest"></a>Exemple de manifeste du package de dessin

Voici le fichier manifeste pour l’exemple de package de dessin. Pour télécharger l’intégralité du package, consultez l’[exemple de package de dessin](https://github.com/Azure-Samples/am-creator-indoor-data-examples).

#### <a name="manifest-file"></a>Fichier manifeste

```JSON
{
    "version": "1.1", 
    "directoryInfo": { 
        "name": "Contoso Building", 
        "streetAddress": "Contoso Way", 
        "unit": "1", 
        "locality": "Contoso eastside", 
        "postalCode": "98052", 
        "adminDivisions": [ 
            "Contoso city", 
            "Contoso state", 
            "Contoso country" 
        ], 
        "hoursOfOperation": "Mo-Fr 08:00-17:00 open", 
        "phone": "1 (425) 555-1234", 
        "website": "www.contoso.com", 
        "nonPublic": false, 
        "anchorLatitude": 47.636152, 
        "anchorLongitude": -122.132600, 
        "anchorHeightAboveSeaLevel": 1000, 
        "defaultLevelVerticalExtent": 3  
    }, 
    "buildingLevels": { 
        "levels": [ 
            { 
                "levelName": "Basement", 
                "ordinal": -1, 
                "filename": "./Basement.dwg" 
            }, { 
                "levelName": "Ground", 
                "ordinal": 0, 
                "verticalExtent": 5, 
                "filename": "./Ground.dwg" 
            }, { 
                "levelName": "Level 2", 
                "ordinal": 1, 
                "heightAboveFacilityAnchor": 3.5, 
                "filename": "./Level_2.dwg" 
            } 
        ] 
    }, 
    "georeference": { 
        "lat": 47.636152, 
        "lon": -122.132600, 
        "angle": 0 
    }, 
    "dwgLayers": { 
        "exterior": [ 
            "OUTLINE", "WINDOWS" 
        ], 
        "unit": [ 
            "UNITS" 
        ], 
        "wall": [ 
            "WALLS" 
        ], 
        "door": [ 
            "DOORS" 
        ], 
        "unitLabel": [ 
            "UNITLABELS" 
        ], 
        "zone": [ 
            "ZONES" 
        ], 
        "zoneLabel": [ 
            "ZONELABELS" 
        ] 
    }, 
    "unitProperties": [ 
        { 
            "unitName": "B01", 
            "categoryName": "room.office", 
            "occupants": [ 
                { 
                    "name": "Joe's Office", 
                    "phone": "1 (425) 555-1234" 
                } 
            ], 
            "nameAlt": "Basement01", 
            "nameSubtitle": "01", 
            "addressRoomNumber": "B01", 
            "nonPublic": true, 
            "isRoutable": true, 
            "isOpenArea": true 
        }, 
        { 
            "unitName": "B02" 
        }, 
        { 
            "unitName": "B05", 
            "categoryName": "room.office" 
        }, 
        { 
            "unitName": "STRB01", 
            "verticalPenetrationCategory": "verticalPenetration.stairs", 
            "verticalPenetrationDirection": "both" 
        }, 
        { 
            "unitName": "ELVB01", 
            "verticalPenetrationCategory": "verticalPenetration.elevator", 
            "verticalPenetrationDirection": "high_to_low" 
        } 
    ], 
    "zoneProperties": 
    [ 
        { 
            "zoneName": "WifiB01", 
            "categoryName": "Zone", 
            "zoneNameAlt": "MyZone", 
            "zoneNameSubtitle": "Wifi", 
            "zoneSetId": "1234" 
        }, 
        { 
            "zoneName": "Wifi101",
            "categoryName": "Zone",
            "zoneNameAlt": "MyZone",
            "zoneNameSubtitle": "Wifi",
            "zoneSetId": "1234"
        }
    ]
}
```

## <a name="next-steps"></a>Étapes suivantes

Lorsque votre package de dessin répond aux exigences, vous pouvez utiliser le [service de conversion d’Azure Maps](/rest/api/maps/v2/conversion) pour convertir le package en un jeu de données cartographiques. Ensuite, vous pouvez utiliser le jeu de données pour générer une carte d’intérieur à l’aide du module de cartes d’intérieur.

> [!div class="nextstepaction"]
> [Creator Facility Ontology](creator-facility-ontology.md)

> [!div class="nextstepaction"]
> [Créateur pour cartes d’intérieur](creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Guide sur les packages de dessins](drawing-package-guide.md)

> [!div class="nextstepaction"]
>[Créateur pour cartes d’intérieur](creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Tutoriel : Création d’une carte d’intérieur du Créateur](tutorial-creator-indoor-maps.md)

> [!div class="nextstepaction"]
> [Application de style dynamique de cartes d’intérieur](indoor-map-dynamic-styling.md)