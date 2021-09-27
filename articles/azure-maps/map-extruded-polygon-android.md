---
title: Ajouter une couche d’extrusion de polygone à une carte Android | Microsoft Azure Maps
description: Comment ajouter une couche d’extrusion de polygone à l’Android SDK Microsoft Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 02/26/2021
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
zone_pivot_groups: azure-maps-android
ms.openlocfilehash: 85e8f7e2d4d6dad05484131cc7c0cf0932fee407
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2021
ms.locfileid: "123432699"
---
# <a name="add-a-polygon-extrusion-layer-to-the-map-android-sdk"></a>Ajouter une couche d’extrusion de polygone à la carte (Android SDK)

Cet article explique comment utiliser la couche d’extrusion de polygone pour afficher des zones d’entités géométriques `Polygon` et `MultiPolygon` en tant que formes extrudées.

## <a name="use-a-polygon-extrusion-layer"></a>Utiliser une couche d’extrusion de polygone

Connectez la couche d’extrusion de polygone à une source de données. Ensuite, chargez-la sur la carte. La couche d’extrusion de polygone restitue les zones des entités `Polygon` et `MultiPolygon` en tant que formes extrudées. Les propriétés `height` et `base` de la couche d’extrusion de polygone définissent la distance de base par rapport au sol et la hauteur de la forme extrudée en **mètres**. Le code suivant montre comment créer un polygone, l’ajouter à une source de données et l’afficher à l’aide de la classe Couche d’extrusion de polygone.

> [!Note]
> La valeur de `base` définie dans la couche d’extrusion de polygone doit être inférieure ou égale à la valeur de `height`.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();
map.sources.add(source);

//Create a polygon.
source.add(Polygon.fromLngLats(
    Arrays.asList(
        Arrays.asList(
            Point.fromLngLat(-73.958383, 40.800279),
            Point.fromLngLat(-73.981547, 40.768459),
            Point.fromLngLat(-73.981246, 40.767761),
            Point.fromLngLat(-73.973618, 40.764616),
            Point.fromLngLat(-73.973060, 40.765128),
            Point.fromLngLat(-73.972599, 40.764908),
            Point.fromLngLat(-73.949446, 40.796584),
            Point.fromLngLat(-73.949661, 40.797088),
            Point.fromLngLat(-73.957815, 40.800523),
            Point.fromLngLat(-73.958383, 40.800279)
        )
    )
));

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
PolygonExtrusionLayer layer = new PolygonExtrusionLayer(source,
    fillColor("#fc0303"),
    fillOpacity(0.7f),
    height(500f)
);

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
map.layers.add(layer, "labels");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()
map.sources.add(source)

//Create a polygon.
source.add(
    Polygon.fromLngLats(
        Arrays.asList(
            Arrays.asList(
                Point.fromLngLat(-73.958383, 40.800279),
                Point.fromLngLat(-73.981547, 40.768459),
                Point.fromLngLat(-73.981246, 40.767761),
                Point.fromLngLat(-73.973618, 40.764616),
                Point.fromLngLat(-73.973060, 40.765128),
                Point.fromLngLat(-73.972599, 40.764908),
                Point.fromLngLat(-73.949446, 40.796584),
                Point.fromLngLat(-73.949661, 40.797088),
                Point.fromLngLat(-73.957815, 40.800523),
                Point.fromLngLat(-73.958383, 40.800279)
            )
        )
    )
)

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
val layer = PolygonExtrusionLayer(
    source,
    fillColor("#fc0303"),
    fillOpacity(0.7f),
    height(500f)
)

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
map.layers.add(layer, "labels")
```

::: zone-end

La capture d’écran suivante présente le code ci-dessus affichant un polygone étiré verticalement à l’aide d’une couche d’extrusion de polygone.

![Carte avec un polygone étiré verticalement à l’aide d’une couche d’extrusion de polygone](media/map-extruded-polygon-android/polygon-extrusion-layer.jpg)

## <a name="add-data-driven-polygons"></a>Ajouter des polygones pilotés par des données

Une carte choroplèthe peut être rendue à l’aide de la couche d’extrusion de polygone. Définissez les propriétés `height` et `fillColor` de la couche d’extrusion sur la mesure de la variable statistique dans les entités géométriques `Polygon` et `MultiPolygon`. L’exemple de code suivant montre une carte choroplèthe extrudée des États-Unis, basée sur la mesure de la densité de population de chaque État.

::: zone pivot="programming-language-java-android"

```java
//Create a data source and add it to the map.
DataSource source = new DataSource();

//Import the geojson data and add it to the data source.
source.importDataFromUrl("https://azuremapscodesamples.azurewebsites.net/Common/data/geojson/US_States_Population_Density.json");

//Add data source to the map.
map.sources.add(source);

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
PolygonExtrusionLayer layer = new PolygonExtrusionLayer(source,
    fillOpacity(0.7f),
    fillColor(
        step(
            get("density"),
            literal("rgba(0, 255, 128, 1)"),
            stop(10, "rgba(9, 224, 118, 1)"),
            stop(20, "rgba(11, 191, 103, 1)"),
            stop(50, "rgba(247, 227, 5, 1)"),
            stop(100, "rgba(247, 199, 7, 1)"),
            stop(200, "rgba(247, 130, 5, 1)"),
            stop(500, "rgba(247, 94, 5, 1)"),
            stop(1000, "rgba(247, 37, 5, 1)")
        )
    ),
    height(
        interpolate(
            linear(),
            get("density"),
            stop(0, 100),
            stop(1200, 960000)
        )
    )
);

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
map.layers.add(layer, "labels");
```

::: zone-end

::: zone pivot="programming-language-kotlin"

```kotlin
//Create a data source and add it to the map.
val source = DataSource()

//Import the geojson data and add it to the data source.
source.importDataFromUrl("https://azuremapscodesamples.azurewebsites.net/Common/data/geojson/US_States_Population_Density.json")

//Add data source to the map.
map.sources.add(source)

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
val layer = PolygonExtrusionLayer(
    source,
    fillOpacity(0.7f),
    fillColor(
        step(
            get("density"),
            literal("rgba(0, 255, 128, 1)"),
            stop(10, "rgba(9, 224, 118, 1)"),
            stop(20, "rgba(11, 191, 103, 1)"),
            stop(50, "rgba(247, 227, 5, 1)"),
            stop(100, "rgba(247, 199, 7, 1)"),
            stop(200, "rgba(247, 130, 5, 1)"),
            stop(500, "rgba(247, 94, 5, 1)"),
            stop(1000, "rgba(247, 37, 5, 1)")
        )
    ),
    height(
        interpolate(
            linear(),
            get("density"),
            stop(0, 100),
            stop(1200, 960000)
        )
    )
)

//Create and add a polygon extrusion layer to the map below the labels so that they are still readable.
map.layers.add(layer, "labels")
```

::: zone-end

La capture d’écran suivante montre une carte choroplèthe des États américains, colorée et étirée verticalement sous la forme de polygones extrudés en fonction de la densité de la population.

![Carte choroplèthe des États américains, colorée et étirée verticalement sous la forme de polygones extrudés en fonction de la densité de la population.](media/map-extruded-polygon-android/android-extruded-choropleth.jpg)

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir plus d’exemples de code à ajouter à vos cartes, consultez les articles suivants :

> [!div class="nextstepaction"]
> [Créer une source de données](create-data-source-android-sdk.md)

> [!div class="nextstepaction"]
> [Utiliser des expressions de style basées sur les données](data-driven-style-expressions-android-sdk.md)

> [!div class="nextstepaction"]
> [Ajouter une couche de polygones](how-to-add-shapes-to-android-map.md)
