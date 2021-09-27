---
title: Présentation des calques dans le visuel Power BI Azure Maps | Microsoft Azure Maps
description: Dans cet article, vous allez découvrir les différents calques disponibles dans le visuel Microsoft Azure Maps pour Power BI.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/26/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: 6014bbf5ed5f3600bc79d5083f5fcc85d323acd5
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2021
ms.locfileid: "123428899"
---
# <a name="understanding-layers-in-the-azure-maps-power-bi-visual"></a>Présentation des calques dans le visuel Power BI Azure Maps

Il existe deux types de calques disponibles dans le visuel Azure Maps. Le premier type se concentre sur le rendu des données passées dans le volet **Champs** du visuel et est constitué des calques suivants, appelés calques de rendu de données.

:::row:::
    :::column span="":::
        **Calque de bulles**

        Affiche les points sous forme de cercles à l’échelle sur la carte.

        ![Calque de bulles sur la carte](media/power-bi-visual/bubble-layer-thumb.png)
    :::column-end:::
    :::column span="":::
        **Calque de graphique à barres**

        Affiche les points sous forme de barres 3D sur la carte.
        
        ![Calque de graphique à barres sur la carte](media/power-bi-visual/bar-chart-layer-thumb.png)
    :::column-end:::
:::row-end:::

Le second type de calque connecte d’autres sources externes de données à la carte pour fournir davantage de contexte et est constitué des calques suivants.

:::row:::
    :::column span="":::
        **Calque de référence**

        Superpose un fichier GeoJSON chargé à la carte.

        ![Calque de référence sur la carte](media/power-bi-visual/reference-layer-thumb.png)
    :::column-end:::
    :::column span="":::
        **Calque de vignettes**

        Superpose un calque de vignettes personnalisé à la carte.
        
        ![Calque de vignettes sur la carte](media/power-bi-visual/tile-layer-thumb.png)
    :::column-end:::
    :::column span="":::
        **Calque de trafic**

        Superpose des informations de trafic en temps réel à la carte.
        
        ![Calque de trafic sur la carte](media/power-bi-visual/traffic-layer-thumb.png)
    :::column-end:::
:::row-end:::

Tous les calques de rendu de données ainsi que le **calque de vignettes** présentent des options pour les niveaux de zoom minimal et maximal utilisés pour spécifier une plage de niveau de zoom à laquelle ces calques doivent être affichés. Cela permet d’utiliser un type de calque de rendu à un niveau de zoom et une transition vers un autre calque de rendu à un autre niveau de zoom.

Il est également possible de positionner ces calques par rapport aux autres calques de la carte. Quand plusieurs calques de rendu de données sont utilisés, l’ordre dans lequel ils sont ajoutés à la carte détermine leur ordre de superposition relatif lorsqu’ils ont la même valeur pour **Position du calque**.

## <a name="general-layer-settings"></a>Paramètres de calques généraux

La section des calques généraux du volet **Format** contient des paramètres communs qui s’appliquent aux calques qui sont connectés au jeu de données Power BI dans le volet **Champs** (calque de bulles, de graphique à barres).

| Paramètre     | Description   |
|-------------|---------------|
| Transparence non sélectionnée | Transparence des formes qui ne sont pas sélectionnées quand une ou plusieurs formes sont sélectionnées.  |
| Afficher les zéros              | Spécifie si les points qui ont une valeur de taille égale à zéro doivent être affichés sur la carte avec le rayon minimal. |
| Afficher les négatifs          | Spécifie si la valeur absolue des valeurs de tailles négatives doit être tracée.   |
| Valeur de données minimale          | Valeur minimale des données d’entrée par rapport à laquelle ajuster l’échelle. Idéal pour le découpage des valeurs hors norme.  |
| Valeur de données maximale          | Valeur maximale des données d’entrée par rapport à laquelle ajuster l’échelle. Idéal pour le découpage des valeurs hors norme.  |

## <a name="next-steps"></a>Étapes suivantes

Changez l’affichage de vos données sur la carte :

> [!div class="nextstepaction"]
> [Ajouter une couche de bulles](power-bi-visual-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Ajouter un calque de graphique à barres](power-bi-visual-add-bar-chart-layer.md)

Ajouter davantage de contexte à la carte :

> [!div class="nextstepaction"]
> [Ajouter une couche de référence](power-bi-visual-add-reference-layer.md)

> [!div class="nextstepaction"]
> [Ajouter une couche de mosaïques](power-bi-visual-add-tile-layer.md)

> [!div class="nextstepaction"]
> [Afficher le trafic en temps réel](power-bi-visual-show-real-time-traffic.md)
