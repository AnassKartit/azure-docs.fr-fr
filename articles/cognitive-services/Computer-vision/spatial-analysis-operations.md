---
title: Opérations d’analyse spatiale
titleSuffix: Azure Cognitive Services
description: Les opérations d’analyse spatiale.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 06/08/2021
ms.author: pafarley
ms.openlocfilehash: 6f4625f87bddfdffca19c3a6b8ebdf7ca4586c70
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2021
ms.locfileid: "129997646"
---
# <a name="spatial-analysis-operations"></a>Opérations d’analyse spatiale

L’analyse spatiale permet l’analyse du streaming vidéo en temps réel à partir des caméras. Pour chaque caméra que vous configurez, les opérations d’analyse spatiale génèrent un flux de sortie de messages JSON envoyés à votre instance Azure IoT Hub. 

Le conteneur d’analyse spatiale implémente les opérations suivantes :

| Identificateur d'opération| Description|
|---------|---------|
| cognitiveservices.vision.spatialanalysis-personcount | Compte les personnes dans une zone désignée dans le champ de vue de la caméra. La zone doit être entièrement couverte par une seule caméra pour permettre à PersonCount d’enregistrer un total précis. <br> Émet un événement initial _personCountEvent_, puis des événements _personCountEvent_ lorsque le nombre change.  |
| cognitiveservices.vision.spatialanalysis-personcrossingline | Effectue le suivi lorsqu’une personne traverse une ligne désignée dans le champ de vue de la caméra. <br>Émet un événement _personLineEvent_ lorsque la personne traverse la ligne, et fournit des informations directionnelles. 
| cognitiveservices.vision.spatialanalysis-personcrossingpolygon | Émet un événement _personZoneEnterExitEvent_ quand une personne entre ou quitte la zone et fournit des informations directionnelles avec le côté numéroté de la zone qui a été franchie. Émet un événement _personZoneDwellTimeEvent_ quand la personne quitte la zone et fournit des informations directionnelles ainsi que le nombre de millisecondes que la personne a passées à l’intérieur de la zone. |
| cognitiveservices.vision.spatialanalysis-persondistance | Effectue le suivi lorsqu’une personne enfreint une règle de distance. <br> Émet régulièrement un _personDistanceEvent_ avec l’emplacement de chaque infraction de distance. |
| cognitiveservices.vision.spatialanalysis | Opération générique qui peut être utilisée pour exécuter tous les scénarios mentionnés ci-dessus. Cette option est plus utile lorsque vous souhaitez exécuter plusieurs scénarios sur la même caméra ou utiliser des ressources système (par exemple, GPU) de manière plus efficace. |

Toutes les opérations ci-dessus sont également disponibles dans la version `.debug`, qui permet de visualiser les images vidéo au fur et à mesure de leur traitement. Vous devrez exécuter `xhost +` sur l’ordinateur hôte pour activer la visualisation des trames et des événements vidéo.

Identificateur d'opération| Description
|---------|---------|
| cognitiveservices.vision.spatialanalysis-personcount.debug | Compte les personnes dans une zone désignée dans le champ de vue de la caméra. <br> Émet un événement initial _personCountEvent_, puis des événements _personCountEvent_ lorsque le nombre change.  |
| cognitiveservices.vision.spatialanalysis-personcrossingline.debug | Effectue le suivi lorsqu’une personne traverse une ligne désignée dans le champ de vue de la caméra. <br>Émet un événement _personLineEvent_ lorsque la personne traverse la ligne, et fournit des informations directionnelles. 
| cognitiveservices.vision.spatialanalysis-personcrossingpolygon.debug | Émet un événement _personZoneEnterExitEvent_ quand une personne entre ou quitte la zone et fournit des informations directionnelles avec le côté numéroté de la zone qui a été franchie. Émet un événement _personZoneDwellTimeEvent_ quand la personne quitte la zone et fournit des informations directionnelles ainsi que le nombre de millisecondes que la personne a passées à l’intérieur de la zone. |
| cognitiveservices.vision.spatialanalysis-persondistance.debug | Effectue le suivi lorsqu’une personne enfreint une règle de distance. <br> Émet régulièrement un _personDistanceEvent_ avec l’emplacement de chaque infraction de distance. |
| cognitiveservices.vision.spatialanalysis.debug | Opération générique qui peut être utilisée pour exécuter tous les scénarios mentionnés ci-dessus. Cette option est plus utile lorsque vous souhaitez exécuter plusieurs scénarios sur la même caméra ou utiliser des ressources système (par exemple, GPU) de manière plus efficace. |

L’analyse spatiale peut également être exécutée avec [Live Video Analytics](../../media-services/live-video-analytics-edge/spatial-analysis-tutorial.md) en tant que module d’intelligence artificielle vidéo. 

<!--more details on the setup can be found in the [LVA Setup page](LVA-Setup.md). Below is the list of the operations supported with Live Video Analytics. -->

| Identificateur d'opération| Description|
|---------|---------|
| cognitiveservices.vision.spatialanalysis-personcount.livevideoanalytics | Compte les personnes dans une zone désignée dans le champ de vue de la caméra. <br> Émet un événement initial _personCountEvent_, puis des événements _personCountEvent_ lorsque le nombre change.  |
| cognitiveservices.vision.spatialanalysis-personcrossingline.livevideoanalytics | Effectue le suivi lorsqu’une personne traverse une ligne désignée dans le champ de vue de la caméra. <br>Émet un événement _personLineEvent_ lorsque la personne traverse la ligne, et fournit des informations directionnelles. 
| cognitiveservices.vision.spatialanalysis-personcrossingpolygon.livevideoanalytics | Émet un événement _personZoneEnterExitEvent_ quand une personne entre ou quitte la zone et fournit des informations directionnelles avec le côté numéroté de la zone qui a été franchie. Émet un événement _personZoneDwellTimeEvent_ quand la personne quitte la zone et fournit des informations directionnelles ainsi que le nombre de millisecondes que la personne a passées à l’intérieur de la zone.  |
| cognitiveservices.vision.spatialanalysis-persondistance.livevideoanalytics | Effectue le suivi lorsqu’une personne enfreint une règle de distance. <br> Émet régulièrement un _personDistanceEvent_ avec l’emplacement de chaque infraction de distance. |
| cognitiveservices.vision.spatialanalysis.livevideoanalytics | Opération générique qui peut être utilisée pour exécuter tous les scénarios mentionnés ci-dessus. Cette option est plus utile lorsque vous souhaitez exécuter plusieurs scénarios sur la même caméra ou utiliser des ressources système (par exemple, GPU) de manière plus efficace. |

Les opérations Live Video Analytics sont également disponibles dans la version `.debug` (par exemple, cognitiveservices.vision.spatialanalysis-personcount.livevideoanalytics.debug) qui a la capacité de visualiser les images vidéo en cours de traitement. Vous devez exécuter `xhost +` sur l’ordinateur hôte pour activer la visualisation des trames et des événements vidéo

> [!IMPORTANT]
> Les modèles d’IA de vision par ordinateur détectent et localisent la présence humaine dans les images et la sortie vidéo à l’aide d’un cadre englobant un corps humain. Les modèles d’IA n’essaient pas de découvrir les identités ou données démographiques des individus.

Il s’agit des paramètres requis par chacune de ces opérations d’analyse spatiale.

| Paramètres d’opération| Description|
|---------|---------|
| ID de l'opération | Identificateur d’opération de la table ci-dessus.|
| enabled | Booléen : true ou false|
| VIDEO_URL| URL RTSP de la caméra (exemple : `rtsp://username:password@url`). L’analyse spatiale prend en charge les flux encodés en H.264 via RTSP, http ou mp4. Video_URL peut être fourni en tant que valeur de chaîne base64 masquée à l’aide du chiffrement AES, et si l’URL de la vidéo est masquée, `KEY_ENV` et `IV_ENV` doivent être fournis en tant que variables d’environnement. L’exemple d’utilitaire permettant de générer des clés et un chiffrement est disponible [ici](/dotnet/api/system.security.cryptography.aesmanaged). |
| VIDEO_SOURCE_ID | Nom convivial de la caméra ou du flux vidéo. Retourné avec la sortie JSON de l’événement.|
| VIDEO_IS_LIVE| True pour les caméras ; false pour les vidéos enregistrées.|
| VIDEO_DECODE_GPU_INDEX| Le GPU pour décoder la trame vidéo. 0 par défaut. Doit être identique à `gpu_index` dans les autres configurations de nœud, comme `DETECTOR_NODE_CONFIG` et `CAMERACALIBRATOR_NODE_CONFIG`.|
| INPUT_VIDEO_WIDTH | Largeur d’image de la vidéo/du flux d’entrée (par exemple, 1920). Il s’agit d’un champ facultatif. S’il est fourni, l’image est mise à l’échelle avec cette dimension, tout en préservant les proportions.|
| DETECTOR_NODE_CONFIG | JSON indiquant le GPU sur lequel exécuter le nœud de détecteur. Il doit être au format suivant : `"{ \"gpu_index\": 0 }",`|
| TRACKER_NODE_CONFIG | JSON indiquant s’il faut, ou non, calculer la vitesse dans le nœud de suivi. Il doit être au format suivant : `"{ \"enable_speed\": true }",`|
| CAMERA_CONFIG | JSON indiquant les paramètres de caméra étalonnée pour plusieurs caméras. Si la compétence que vous avez utilisée nécessite un étalonnage et que vous avez déjà les paramètres de la caméra, vous pouvez utiliser cette configuration pour les fournir directement. Doit respecter le format suivant : `"{ \"cameras\": [{\"source_id\": \"endcomputer.0.persondistancegraph.detector+end_computer1\", \"camera_height\": 13.105561256408691, \"camera_focal_length\": 297.60003662109375, \"camera_tiltup_angle\": 0.9738943576812744}] }"` ; `source_id` est utilisé pour identifier chaque caméra. Il peut être obtenu à partir de la valeur `source_info` de l’événement que nous avons publié. Il ne prend effet que quand `do_calibration=false` dans `DETECTOR_NODE_CONFIG`.|
| CAMERACALIBRATOR_NODE_CONFIG | JSON indiquant le GPU sur lequel exécuter le nœud de calibrage de la caméra et s’il faut ou non utiliser le calibrage. Il doit être au format suivant : `"{ \"gpu_index\": 0, \"do_calibration\": true, \"enable_orientation\": true}",`|
| CALIBRATION_CONFIG | JSON indiquant les paramètres permettant de contrôler le fonctionnement du calibrage de la caméra. Il doit être au format suivant : `"{\"enable_recalibration\": true, \"quality_check_frequency_seconds\": 86400}",`|
| SPACEANALYTICS_CONFIG | Configuration JSON pour la zone et la ligne, comme indiqué ci-dessous.|
| ENABLE_FACE_MASK_CLASSIFIER | `True` pour activer la détection des personnes qui portent des masques dans le flux vidéo, `False` pour la désactiver. Par défaut, cette fonctionnalité est désactivée. La détection des masques nécessite que le paramètre de largeur de vidéo d’entrée soit égal à 1920 (`"INPUT_VIDEO_WIDTH": 1920`). L’attribut de masque n’est pas retourné si les personnes détectées ne font pas face à la caméra ou sont trop éloignées de celle-ci. Pour plus d’informations, consultez le guide de [positionnement de la caméra](spatial-analysis-camera-placement.md). |

### <a name="detector-node-parameter-settings"></a>Paramètres du nœud de détection
Il s’agit d’un exemple de paramètres DETECTOR_NODE_CONFIG pour toutes les opérations d’analyse spatiale.

```json
{
"gpu_index": 0,
"enable_breakpad": false
}
```

| Nom | Type| Description|
|---------|---------|---------|
| `gpu_index` | string| Index GPU sur lequel cette opération s’exécutera.|
| `enable_breakpad`| bool | Indique s’il faut activer la fonctionnalité breakpad, qui est utilisée pour générer le vidage sur incident à des fins de débogage. La valeur par défaut de ce paramètre est `false`. Si vous le définissez sur `true`, vous devez également ajouter `"CapAdd": ["SYS_PTRACE"]` dans la `HostConfig` partie du conteneur `createOptions`. Par défaut, le vidage sur incident est chargé sur l’application AppCenter [RealTimePersonTracking](https://appcenter.ms/orgs/Microsoft-Organization/apps/RealTimePersonTracking/crashes/errors?version=&appBuild=&period=last90Days&status=&errorType=all&sortCol=lastError&sortDir=desc) ; si vous souhaitez que les vidages sur incident soient chargés sur votre application AppCenter, vous pouvez remplacer la variable d’environnement `RTPT_APPCENTER_APP_SECRET` par le secret d’application de votre application.|

### <a name="camera-calibration-node-parameter-settings"></a>Paramètres du nœud de calibrage de la caméra
Il s’agit d’un exemple de paramètres `CAMERACALIBRATOR_NODE_CONFIG` pour toutes les opérations d’analyse spatiale.

```
{
"gpu_index": 0,
"do_calibration": true,
"enable_breakpad": false,
"enable_orientation": true
}
```

| Nom | Type| Description|
|---------|---------|---------|
| `do_calibration` | string | Indique que l’étalonnage est activé. `do_calibration` doit avoir la valeur true pour que **cognitiveservices.vision.spatialanalysis-persondistance** fonctionne correctement. `do_calibration` est défini par défaut sur `True`. |
| `enable_breakpad`| bool | Indique s’il faut activer la fonctionnalité breakpad, qui est utilisée pour générer le vidage sur incident à des fins de débogage. La valeur par défaut de ce paramètre est `false`. Si vous le définissez sur `true`, vous devez également ajouter `"CapAdd": ["SYS_PTRACE"]` dans la `HostConfig` partie du conteneur `createOptions`. Par défaut, le vidage sur incident est chargé sur l’application AppCenter [RealTimePersonTracking](https://appcenter.ms/orgs/Microsoft-Organization/apps/RealTimePersonTracking/crashes/errors?version=&appBuild=&period=last90Days&status=&errorType=all&sortCol=lastError&sortDir=desc) ; si vous souhaitez que les vidages sur incident soient chargés sur votre application AppCenter, vous pouvez remplacer la variable d’environnement `RTPT_APPCENTER_APP_SECRET` par le secret d’application de votre application.
| `enable_orientation` | bool | Indique si vous voulez calculer, ou non, l’orientation pour les personnes détectées. `enable_orientation` est défini par défaut sur `True`. |

### <a name="calibration-config"></a>Configuration du calibrage
Il s’agit d’un exemple de paramètres `CALIBRATION_CONFIG` pour toutes les opérations d’analyse spatiale.

```
{
"enable_recalibration": true,
"calibration_quality_check_frequency_seconds": 86400,
"calibration_quality_check_sample_collect_frequency_seconds": 300,
"calibration_quality_check_one_round_sample_collect_num": 10,
"calibration_quality_check_queue_max_size": 1000,
"calibration_event_frequency_seconds": -1
}
```

| Nom | Type| Description|
|---------|---------|---------|
| `enable_recalibration` | bool | Indique si le réétalonnage automatique est activé. La valeur par défaut est `true`.|
| `calibration_quality_check_frequency_seconds` | int | Nombre minimal de secondes entre chaque contrôle de qualité pour déterminer si le réétalonnage est nécessaire. La valeur par défaut est `86400` (24 heures). Utilisé uniquement si `enable_recalibration=True`.|
| `calibration_quality_check_sample_collect_frequency_seconds` | int | Nombre minimal de secondes entre la collecte de nouveaux échantillons de données pour le réétalonnage et le contrôle de la qualité. La valeur par défaut est `300` (5 minutes). Utilisé uniquement si `enable_recalibration=True`.|
| `calibration_quality_check_one_round_sample_collect_num` | int | Nombre minimal de nouveaux échantillons de données à collecter par cycle de collecte d’échantillons. La valeur par défaut est `10`. Utilisé uniquement si `enable_recalibration=True`.|
| `calibration_quality_check_queue_max_size` | int | Nombre maximal d’échantillons de données à stocker quand le modèle de caméra est étalonné. La valeur par défaut est `1000`. Utilisé uniquement si `enable_recalibration=True`.|
| `calibration_event_frequency_seconds` | int | Fréquence de sortie (en secondes) des événements d’étalonnage de l’appareil photo. La valeur `-1` indique que l’étalonnage de l’appareil photo ne doit pas être envoyé sauf si les informations d’étalonnage de celui-ci ont été modifiées. La valeur par défaut est `-1`.|


### <a name="camera-calibration-output"></a>Sortie d’étalonnage de l’appareil photo
Il s’agit d’un exemple de la sortie de l’étalonnage de l’appareil photo si elle est activée. Les points de suspension indiquent la présence d’objets du même type dans une liste.
```
{
  "type": "cameraCalibrationEvent",
  "sourceInfo": {
    "id": "camera1",
    "timestamp": "2021-04-20T21:15:59.100Z",
    "width": 640,
    "height": 360,
    "frameId": 531,
    "cameraCalibrationInfo": {
      "status": "Calibrated",
      "cameraHeight": 13.294151306152344,
      "focalLength": 372.0000305175781,
      "tiltupAngle": 0.9581864476203918,
      "lastCalibratedTime": "2021-04-20T21:15:59.058"
    }
  },
  "zonePlacementInfo": {
    "optimalZoneRegion": {
      "type": "POLYGON",
       "points": [
        {
          "x": 0.8403755868544601,
          "y": 0.5515320334261838
        },
        {
          "x": 0.15805946791862285,
          "y": 0.5487465181058496
        },
        ...
      ],
      "name": "optimal_zone_region"
    },
    "fairZoneRegion": {
      "type": "POLYGON",
      "points": [
        {
          "x": 0.7871674491392802,
          "y": 0.7437325905292479
        },
        {
          "x": 0.22065727699530516,
          "y": 0.7325905292479109
        },
        ...
      ],
      "name": "fair_zone_region"
    },
    "uniformlySpacedPersonBoundingBoxes": [
      {
        "type": "RECTANGLE",
        "points": [
          {
            "x": 0.0297339593114241,
            "y": 0.0807799442896936
          },
          {
            "x": 0.10015649452269171,
            "y": 0.2757660167130919
          }
        ]
      },
      ...
    ],
    "personBoundingBoxGroundPoints": [
      {
        "x": -22.944068908691406,
        "y": 31.487680435180664
      },
      ...
    ]
  }
}
```

Consultez [Sortie de l’opération d’analyse spatiale](#spatial-analysis-operation-output) pour plus de détails sur `source_info`.

| Nom du champ ZonePlacementInfo | Type| Description|
|---------|---------|---------|
| `optimalZonePolygon` | object| Polygone dans l’image de l’appareil photo dans lequel il est possible de placer des lignes ou des zones pour vos opérations en vue de résultats optimaux. <br/> Chaque paire de valeurs représente x et y pour les vertex d’un polygone. Le polygone représente les zones dans lesquelles les personnes sont suivies ou comptées, et les points de polygone sont basés sur des coordonnées normalisées (0-1), où le coin supérieur gauche est (0,0, 0,0) et le coin inférieur droit est (1,0, 1,0).|
| `fairZonePolygon` | object| Polygone dans l’image de l’appareil photo dans lequel il est possible de placer des lignes ou des zones pour vos opérations en vue de résultats corrects, mais éventuellement pas optimaux. <br/> Consultez `optimalZonePolygon` ci-dessus pour obtenir une explication détaillée du contenu. |
| `uniformlySpacedPersonBoundingBoxes` | list | Liste des zones englobantes des personnes dans l’image de l’appareil photo, distribuées uniformément dans l’espace réel. Les valeurs sont basées sur les coordonnées normalisées (0-1).|
| `personBoundingBoxGroundPoints` | list | Liste de coordonnées au niveau du sol par rapport à l’appareil photo. Chaque coordonnée correspond au coin inférieur droit du cadre englobant dans `uniformlySpacedPersonBoundingBoxes` avec le même index. <br/> Consultez le champ `centerGroundPoint` de la section [Format JSON pour les insights d’intelligence artificielle cognitiveservices.vision.spatialanalysis-persondistance](#json-format-for-cognitiveservicesvisionspatialanalysis-persondistance-ai-insights) pour plus de détails sur le calcul des coordonnées au niveau du sol. |

Exemple de sortie d’informations de placement de zone visualisées sur une image vidéo : ![visualisation des informations d’emplacement de zone](./media/spatial-analysis/zone-placement-info-visualization.png)

Les informations sur le placement des zones fournissent des suggestions pour vos configurations, mais il convient de suivre scrupuleusement les directives de [Configuration de l’appareil photo](#camera-configuration) pour obtenir les meilleurs résultats.

### <a name="speed-parameter-settings"></a>Paramètres de vitesse
Vous pouvez configurer le calcul de la vitesse par le biais des paramètres du nœud de suivi.
```
{
"enable_speed": true,
}
```
| Nom | Type| Description|
|---------|---------|---------|
| `enable_speed` | bool | Indique si vous voulez calculer, ou non, la vitesse pour les personnes détectées. `enable_speed` est défini par défaut sur `True`. Il est fortement recommandé d’activer à la fois la vitesse et l’orientation pour obtenir les meilleures valeurs estimées. |


## <a name="spatial-analysis-operations-configuration-and-output"></a>Configuration et sortie des opérations d’analyse spatiale
### <a name="zone-configuration-for-cognitiveservicesvisionspatialanalysis-personcount"></a>Configuration de la zone pour cognitiveservices.vision.spatialanalysis-personcount

 Voici un exemple d’entrée JSON pour le paramètre SPACEANALYTICS_CONFIG qui configure une zone. Vous pouvez configurer plusieurs zones pour cette opération.

```json
{
"zones":[{
       "name": "lobbycamera",
       "polygon": [[0.3,0.3], [0.3,0.9], [0.6,0.9], [0.6,0.3], [0.3,0.3]],
       "events":[{
              "type": "count",
              "config":{
                     "trigger": "event",
            "threshold": 16.00,
            "focus": "footprint"
      }
       }]
}
```

| Nom | Type| Description|
|---------|---------|---------|
| `zones` | list| Liste de zones. |
| `name` | string| Nom convivial de cette zone.|
| `polygon` | list| Chaque paire de valeurs représente x et y pour les vertex d’un polygone. Le polygone représente les zones dans lesquelles les personnes sont suivies ou comptées, et les points de polygone sont basés sur des coordonnées normalisées (0-1), où le coin supérieur gauche est (0,0, 0,0) et le coin inférieur droit est (1,0, 1,0).   
| `threshold` | float| Les événements sont envoyés lorsque la personne dépasse ce nombre de pixels à l’intérieur de la zone. |
| `type` | string| Pour **cognitiveservices.vision.spatialanalysis-personcount**, cela doit être `count`.|
| `trigger` | string| Type de déclencheur pour l’envoi d’un événement. Les valeurs prises en charge sont `event` pour l’envoi d’événements lorsque le nombre change ou `interval` pour envoyer des événements régulièrement, que le nombre ait changé ou non.
| `output_frequency` | int | Vitesse à laquelle les événements sont émis. Lorsque `output_frequency` = X, un envoi est effectué tous les X événements, par ex. `output_frequency` = 2 signifie qu’un événement sur deux fait l’objet d’une sortie. La valeur `output_frequency` s’applique à la fois à `event` et à `interval`. |
| `focus` | string| Position du point dans le cadre englobant de la personne utilisé pour calculer les événements. La valeur de focus peut être `footprint` (l’encombrement de la personne), `bottom_center` (la partie centrale inférieure du cadre englobant de la personne) ou `center` (le centre du cadre englobant de la personne).|

### <a name="line-configuration-for-cognitiveservicesvisionspatialanalysis-personcrossingline"></a>Configuration de ligne pour cognitiveservices.vision.spatialanalysis-personcrossingline

Voici un exemple d’entrée JSON pour le paramètre SPACEANALYTICS_CONFIG qui configure une ligne. Vous pouvez configurer plusieurs lignes de croisement pour cette opération.

```json
{
   "lines": [
       {
           "name": "doorcamera",
           "line": {
               "start": {
                   "x": 0,
                   "y": 0.5
               },
               "end": {
                   "x": 1,
                   "y": 0.5
               }
           },
           "events": [
               {
                   "type": "linecrossing",
                   "config": {
                       "trigger": "event",
                       "threshold": 16.00,
                       "focus": "footprint"
                   }
               }
           ]
       }
   ]
}
```

| Nom | Type| Description|
|---------|---------|---------|
| `lines` | list| Liste de lignes.|
| `name` | string| Nom convivial de cette ligne.|
| `line` | list| Définition de la ligne. Il s’agit d’une ligne directionnelle qui vous permet de comprendre les « Enter » par rapport aux « Exit ».|
| `start` | Paire de valeurs| Coordonnées x, y pour le point de départ de la ligne. Les valeurs float représentent la position du vertex par rapport au coin supérieur gauche. Pour calculer les valeurs x, y absolues, vous multipliez ces valeurs par la taille de cadre. |
| `end` | Paire de valeurs| Coordonnées x, y pour le point de fin de la ligne. Les valeurs float représentent la position du vertex par rapport au coin supérieur gauche. Pour calculer les valeurs x, y absolues, vous multipliez ces valeurs par la taille de cadre. |
| `threshold` | float| Les événements sont envoyés lorsque la personne dépasse ce nombre de pixels à l’intérieur de la zone. La valeur par défaut est 16. Il s’agit de la valeur recommandée pour obtenir une précision maximale. |
| `type` | string| Pour **cognitiveservices.vision.spatialanalysis-personcrossingline**, cela doit être `linecrossing`.|
|`trigger`|string|Type de déclencheur pour l’envoi d’un événement.<br>Valeurs prises en charge : « event » : se déclenche quand quelqu’un traverse la ligne.|
| `focus` | string| Position du point dans le cadre englobant de la personne utilisé pour calculer les événements. La valeur de focus peut être `footprint` (l’encombrement de la personne), `bottom_center` (la partie centrale inférieure du cadre englobant de la personne) ou `center` (le centre du cadre englobant de la personne). La valeur par défaut est footprint.|

### <a name="zone-configuration-for-cognitiveservicesvisionspatialanalysis-personcrossingpolygon"></a>Configuration de la zone pour cognitiveservices.vision.spatialanalysis-personcrossingpolygon

Voici un exemple d’entrée JSON pour le paramètre SPACEANALYTICS_CONFIG qui configure une zone. Vous pouvez configurer plusieurs zones pour cette opération.

 ```json
{
"zones":[
   {
       "name": "queuecamera",
       "polygon": [[0.3,0.3], [0.3,0.9], [0.6,0.9], [0.6,0.3], [0.3,0.3]],
       "events":[{
           "type": "zonecrossing",
           "config":{
               "trigger": "event",
               "threshold": 48.00,
               "focus": "footprint"
               }
           }]
   },
   {
       "name": "queuecamera1",
       "polygon": [[0.3,0.3], [0.3,0.9], [0.6,0.9], [0.6,0.3], [0.3,0.3]],
       "events":[{
           "type": "zonedwelltime",
           "config":{
               "trigger": "event",
               "threshold": 16.00,
               "focus": "footprint"
               }
           }]
   }]
}
```

| Nom | Type| Description|
|---------|---------|---------|
| `zones` | list| Liste de zones. |
| `name` | string| Nom convivial de cette zone.|
| `polygon` | list| Chaque paire de valeurs représente x et y pour les vertex d’un polygone. Le polygone représente les zones dans lesquelles les personnes sont suivies ou comptées. Les valeurs float représentent la position du vertex par rapport au coin supérieur gauche. Pour calculer les valeurs x, y absolues, vous multipliez ces valeurs par la taille de cadre. 
| `target_side` | int| Spécifie un côté de la zone définie par `polygon` pour mesurer la durée pendant laquelle les utilisateurs sont tournés de ce côté pendant qu’ils se trouvent dans la zone. « dwellTimeForTargetSide » génère cette durée estimée. Chaque côté est un bord numéroté entre les deux sommets du polygone qui représente votre zone. Par exemple, le bord entre les deux premiers sommets du polygone représente le premier côté : 'side'=1. La valeur de `target_side` est comprise entre `[0,N-1]``N` est le nombre de côtés de l’élément `polygon`. Ce champ est facultatif.  |
| `threshold` | float| Les événements sont envoyés lorsque la personne dépasse ce nombre de pixels à l’intérieur de la zone. La valeur par défaut est 48 quand le type est zonecrossing et 16 quand l’heure est DwellTime. Il s’agit des valeurs recommandées pour obtenir une précision maximale.  |
| `type` | string| Pour **cognitiveservices.vision.spatialanalysis-personcrossingpolygon**, cela doit être `zonecrossing` ou `zonedwelltime`.|
| `trigger`|string|Type de déclencheur pour l’envoi d’un événement<br>Valeurs prises en charge : « event » : se déclenche quand une personne entre ou quitte la zone.|
| `focus` | string| Position du point dans le cadre englobant de la personne utilisé pour calculer les événements. La valeur de focus peut être `footprint` (l’encombrement de la personne), `bottom_center` (la partie centrale inférieure du cadre englobant de la personne) ou `center` (le centre du cadre englobant de la personne). La valeur par défaut est footprint.|

### <a name="zone-configuration-for-cognitiveservicesvisionspatialanalysis-persondistance"></a>Configuration de la zone pour cognitiveservices.vision.spatialanalysis-persondistance

Voici un exemple d’entrée JSON pour le paramètre SPACEANALYTICS_CONFIG qui configure une zone pour **cognitiveservices.vision.spatialanalysis-persondistance**. Vous pouvez configurer plusieurs zones pour cette opération.

```json
{
"zones":[{
   "name": "lobbycamera",
   "polygon": [[0.3,0.3], [0.3,0.9], [0.6,0.9], [0.6,0.3], [0.3,0.3]],
   "events":[{
       "type": "persondistance",
       "config":{
           "trigger": "event",
           "output_frequency":1,
           "minimum_distance_threshold":6.0,
           "maximum_distance_threshold":35.0,
        "aggregation_method": "average"
           "threshold": 16.00,
           "focus": "footprint"
                   }
          }]
   }]
}
```

| Nom | Type| Description|
|---------|---------|---------|
| `zones` | list| Liste de zones. |
| `name` | string| Nom convivial de cette zone.|
| `polygon` | list| Chaque paire de valeurs représente x et y pour les vertex d’un polygone. Le polygone représente les zones dans lesquelles les utilisateurs sont comptés et la distance entre les personnes. Les valeurs float représentent la position du vertex par rapport au coin supérieur gauche. Pour calculer les valeurs x, y absolues, vous multipliez ces valeurs par la taille de cadre. 
| `threshold` | float| Les événements sont envoyés lorsque la personne dépasse ce nombre de pixels à l’intérieur de la zone. |
| `type` | string| Pour **cognitiveservices.vision.spatialanalysis-persondistance**, cela doit être `persondistance`.|
| `trigger` | string| Type de déclencheur pour l’envoi d’un événement. Les valeurs prises en charge sont `event` pour l’envoi d’événements lorsque le nombre change ou `interval` pour envoyer des événements régulièrement, que le nombre ait changé ou non.
| `output_frequency` | int | Vitesse à laquelle les événements sont émis. Lorsque `output_frequency` = X, un envoi est effectué tous les X événements, par ex. `output_frequency` = 2 signifie qu’un événement sur deux fait l’objet d’une sortie. La valeur `output_frequency` s’applique à la fois à `event` et à `interval`.|
| `minimum_distance_threshold` | float| Distance en pieds qui déclenchera un événement « TooClose » lorsque les personnes sont moins éloignées que cette distance.|
| `maximum_distance_threshold` | float| Distance en pieds qui déclenchera un événement « TooFar » lorsque les personnes sont plus éloignées que cette distance.|
| `aggregation_method` | string| Méthode d’agrégation du résultat persondistance. La méthode d’agrégation s’applique à la fois à `mode` et à `average`.|
| `focus` | string| Position du point dans le cadre englobant de la personne utilisé pour calculer les événements. La valeur de focus peut être `footprint` (l’encombrement de la personne), `bottom_center` (la partie centrale inférieure du cadre englobant de la personne) ou `center` (le centre du cadre englobant de la personne).|

### <a name="configuration-for-cognitiveservicesvisionspatialanalysis"></a>Configuration de cognitiveservices.vision.spatialanalysis
Il s’agit d’un exemple d’entrée JSON pour le paramètre SPACEANALYTICS_CONFIG qui configure une ligne et une zone pour **cognitiveservices.vision.spatialanalysis**. Vous pouvez configurer plusieurs lignes/zones pour cette opération et chaque ligne/zone peut avoir des événements différents.

 ```
{
  "lines": [
    {
      "name": "doorcamera",
      "line": {
        "start": {
          "x": 0,
          "y": 0.5
        },
        "end": {
          "x": 1,
          "y": 0.5
        }
      },
      "events": [
        {
          "type": "linecrossing",
          "config": {
            "trigger": "event",
            "threshold": 16.00,
            "focus": "footprint"
          }
        }
      ]
    }
  ],
  "zones": [
    {
      "name": "lobbycamera",
      "polygon": [[0.3, 0.3],[0.3, 0.9],[0.6, 0.9],[0.6, 0.3],[0.3, 0.3]],
      "events": [
        {
          "type": "persondistance",
          "config": {
            "trigger": "event",
            "output_frequency": 1,
            "minimum_distance_threshold": 6.0,
            "maximum_distance_threshold": 35.0,
            "threshold": 16.00,
            "focus": "footprint"
          }
        },
        {
          "type": "count",
          "config": {
            "trigger": "event",
            "output_frequency": 1,
            "threshold": 16.00,
            "focus": "footprint"
          }
        },
        {
          "type": "zonecrossing",
          "config": {
            "threshold": 48.00,
            "focus": "footprint"
          }
        },
        {
          "type": "zonedwelltime",
          "config": {
            "threshold": 16.00,
            "focus": "footprint"
          }
        }
      ]
    }
  ]
}
```
## <a name="camera-configuration"></a>Configuration de la caméra

Pour en savoir plus sur la configuration des zones et des lignes, consultez les instructions de [positionnement de la caméra](spatial-analysis-camera-placement.md).

## <a name="spatial-analysis-operation-output"></a>Sortie de l’opération d’analyse spatiale

Les événements de chaque opération sont émis vers Azure IoT Hub au format JSON.

### <a name="json-format-for-cognitiveservicesvisionspatialanalysis-personcount-ai-insights"></a>Format JSON pour les insights d’intelligence artificielle cognitiveservices.vision.spatialanalysis-personcount

Exemple de code JSON pour une sortie d’événement par cette opération.

```json
{
    "events": [
        {
            "id": "b013c2059577418caa826844223bb50b",
            "type": "personCountEvent",
            "detectionIds": [
                "bc796b0fc2534bc59f13138af3dd7027",
                "60add228e5274158897c135905b5a019"
            ],
            "properties": {
                "personCount": 2
            },
            "zone": "lobbycamera",
            "trigger": "event"
        }
    ],
    "sourceInfo": {
        "id": "camera_id",
        "timestamp": "2020-08-24T06:06:57.224Z",
        "width": 608,
        "height": 342,
        "frameId": "1400",
        "cameraCalibrationInfo": {
            "status": "Calibrated",
            "cameraHeight": 10.306597709655762,
            "focalLength": 385.3199462890625,
            "tiltupAngle": 1.0969393253326416
        },
        "imagePath": ""
    },
    "detections": [
        {
            "type": "person",
            "id": "bc796b0fc2534bc59f13138af3dd7027",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.612683747944079,
                        "y": 0.25340268765276636
                    },
                    {
                        "x": 0.7185954043739721,
                        "y": 0.6425260577285499
                    }
                ]
            },
            "confidence": 0.9559211134910583,
            "centerGroundPoint": {
                "x": 0.0,
                "y": 0.0
            },
            "metadata": {
            "attributes": {
                "face_mask": 0.99
            }
        }
        },
        {
            "type": "person",
            "id": "60add228e5274158897c135905b5a019",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.22326200886776573,
                        "y": 0.17830915618361087
                    },
                    {
                        "x": 0.34922296122500773,
                        "y": 0.6297955429344847
                    }
                ]
            },
            "confidence": 0.9389744400978088,
            "centerGroundPoint": {
                "x": 0.0,
                "y": 0.0
            },
            "metadata":{
            "attributes": {
            "face_nomask": 0.99
            }
            }
       }
    ],
    "schemaVersion": "1.0"
}
```

| Nom de champ d'événement | Type| Description|
|---------|---------|---------|
| `id` | string| ID de l’événement|
| `type` | string| Type d'événement|
| `detectionsId` | tableau| Tableau de taille 1 de l’identificateur unique de la détection de personne qui a déclenché cet événement|
| `properties` | collection| Collection de valeurs|
| `trackinId` | string| Identificateur unique de la personne détectée|
| `zone` | string | Le champ « name » du polygone qui représente la zone qui a été franchie|
| `trigger` | string| Le type de déclencheur est « event » ou « interval » en fonction de la valeur de `trigger` dans SPACEANALYTICS_CONFIG|

| Nom du champ de détection | Type| Description|
|---------|---------|---------|
| `id` | string| ID de détection|
| `type` | string| Type de détection|
| `region` | collection| Collection de valeurs|
| `type` | string| Type de région|
| `points` | collection| Points supérieur gauche et inférieur droit lorsque le type de région est RECTANGLE |
| `confidence` | float| Confiance de l’algorithme|
| `face_mask` | float | La valeur de confiance de l’attribut avec la plage (0-1) indique que la personne détectée porte un masque |
| `face_nomask` | float | La valeur de confiance de l’attribut avec la plage (0-1) indique que la personne détectée ne porte **pas** de masque |

| Nom du champ SourceInfo | Type| Description|
|---------|---------|---------|
| `id` | string| ID de la caméra|
| `timestamp` | date| Date UTC à laquelle la charge utile JSON a été émise|
| `width` | int | Largeur de l’image vidéo|
| `height` | int | Hauteur de l’image vidéo|
| `frameId` | int | Identificateur du frame|
| `cameraCallibrationInfo` | collection | Collection de valeurs|
| `status` | string | État de l’étalonnage au format `state[;progress description]`. L’état peut être `Calibrating`, `Recalibrating` (si le réétalonnage est activé) ou `Calibrated`. La partie description de la progression est valide uniquement quand elle est dans l’état `Calibrating` et `Recalibrating`, qui est utilisé pour afficher la progression du processus d’étalonnage actuel.|
| `cameraHeight` | float | Hauteur de la caméra au-dessus du sol, en mètres. Cette valeur est déduite de l’étalonnage automatique. |
| `focalLength` | float | Longueur focale de la caméra, en pixels. Cette valeur est déduite de l’étalonnage automatique. |
| `tiltUpAngle` | float | Angle d’inclinaison de la caméra par rapport à la verticale. Cette valeur est déduite de l’étalonnage automatique.|


### <a name="json-format-for-cognitiveservicesvisionspatialanalysis-personcrossingline-ai-insights"></a>Format JSON pour les insights d’intelligence artificielle cognitiveservices.vision.spatialanalysis-personcrossingline

Exemple de code JSON pour une sortie de détections par cette opération.

```json
{
    "events": [
        {
            "id": "3733eb36935e4d73800a9cf36185d5a2",
            "type": "personLineEvent",
            "detectionIds": [
                "90d55bfc64c54bfd98226697ad8445ca"
            ],
            "properties": {
                "trackingId": "90d55bfc64c54bfd98226697ad8445ca",
                "status": "CrossLeft"
            },
            "zone": "doorcamera"
        }
    ],
    "sourceInfo": {
        "id": "camera_id",
        "timestamp": "2020-08-24T06:06:53.261Z",
        "width": 608,
        "height": 342,
        "frameId": "1340",
        "imagePath": ""
    },
    "detections": [
        {
            "type": "person",
            "id": "90d55bfc64c54bfd98226697ad8445ca",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.491627341822574,
                        "y": 0.2385801348769874
                    },
                    {
                        "x": 0.588894994635331,
                        "y": 0.6395559924387793
                    }
                ]
            },
            "confidence": 0.9005028605461121,
            "metadata": {
            "attributes": {
                "face_mask": 0.99
            }
        }
        }
    ],
    "schemaVersion": "1.0"
}
```
| Nom de champ d'événement | Type| Description|
|---------|---------|---------|
| `id` | string| ID de l’événement|
| `type` | string| Type d'événement|
| `detectionsId` | tableau| Tableau de taille 1 de l’identificateur unique de la détection de personne qui a déclenché cet événement|
| `properties` | collection| Collection de valeurs|
| `trackinId` | string| Identificateur unique de la personne détectée|
| `status` | string| Direction des traversées de lignes, 'CrossLeft' ou 'CrossRight'. La direction est exprimée comme si vous étiez placé au « début » faisant face à la « fin » de la ligne. CrossRight va de gauche à droite. CrossLeft va de droite à gauche.|
| `orientationDirection` | string| Direction d’orientation de la personne détectée après qu’elle a franchi la ligne. La valeur peut être « Left », « Right » ou « Straight ». Cette valeur est générée si `enable_orientation` a la valeur `True` dans `CAMERACALIBRATOR_NODE_CONFIG` |
| `zone` | string | Le champ « Name » de la ligne qui a été franchie|

| Nom du champ de détection | Type| Description|
|---------|---------|---------|
| `id` | string| ID de détection|
| `type` | string| Type de détection|
| `region` | collection| Collection de valeurs|
| `type` | string| Type de région|
| `points` | collection| Points supérieur gauche et inférieur droit lorsque le type de région est RECTANGLE |
| `groundOrientationAngle` | float| Angle en radians dans le sens horaire de l’orientation de la personne sur le plan de sol déduit |
| `mappedImageOrientation` | float| L’angle projeté en radians dans le sens horaire de l’orientation de la personne sur l’espace de l’image 2D |
| `speed` | float| Vitesse estimée de la personne détectée. L’unité est `foot per second (ft/s)`|
| `confidence` | float| Confiance de l’algorithme|
| `face_mask` | float | La valeur de confiance de l’attribut avec la plage (0-1) indique que la personne détectée porte un masque |
| `face_nomask` | float | La valeur de confiance de l’attribut avec la plage (0-1) indique que la personne détectée ne porte **pas** de masque |

| Nom du champ SourceInfo | Type| Description|
|---------|---------|---------|
| `id` | string| ID de la caméra|
| `timestamp` | date| Date UTC à laquelle la charge utile JSON a été émise|
| `width` | int | Largeur de l’image vidéo|
| `height` | int | Hauteur de l’image vidéo|
| `frameId` | int | Identificateur du frame|


> [!IMPORTANT]
> Le modèle d’IA détecte une personne qu’elle soit orientée ou non vers la caméra. Le modèle IA n’exécute pas la reconnaissance des visages et n’émet aucune information biométrique. 

### <a name="json-format-for-cognitiveservicesvisionspatialanalysis-personcrossingpolygon-ai-insights"></a>Format JSON pour les insights d’intelligence artificielle cognitiveservices.vision.spatialanalysis-personcrossingpolygon

Exemple de code JSON pour une sortie de détections par cette opération avec le type `zonecrossing` SPACEANALYTICS_CONFIG.

```json
{
    "events": [
        {
            "id": "f095d6fe8cfb4ffaa8c934882fb257a5",
            "type": "personZoneEnterExitEvent",
            "detectionIds": [
                "afcc2e2a32a6480288e24381f9c5d00e"
            ],
            "properties": {
                "trackingId": "afcc2e2a32a6480288e24381f9c5d00e",
                "status": "Enter",
                "side": "1"
            },
            "zone": "queuecamera"
        }
    ],
    "sourceInfo": {
        "id": "camera_id",
        "timestamp": "2020-08-24T06:15:09.680Z",
        "width": 608,
        "height": 342,
        "frameId": "428",
        "imagePath": ""
    },
    "detections": [
        {
            "type": "person",
            "id": "afcc2e2a32a6480288e24381f9c5d00e",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.8135572734631991,
                        "y": 0.6653949670624315
                    },
                    {
                        "x": 0.9937645761590255,
                        "y": 0.9925406829655519
                    }
                ]
            },
            "confidence": 0.6267998814582825,
        "metadata": {
        "attributes": {
        "face_mask": 0.99
        }
        }
           
        }
    ],
    "schemaVersion": "1.0"
}
```

Exemple de code JSON pour une sortie de détections par cette opération avec le type `zonedwelltime` SPACEANALYTICS_CONFIG.

```json
{
    "events": [
        {
            "id": "f095d6fe8cfb4ffaa8c934882fb257a5",
            "type": "personZoneDwellTimeEvent",
            "detectionIds": [
                "afcc2e2a32a6480288e24381f9c5d00e"
            ],
            "properties": {
                "trackingId": "afcc2e2a32a6480288e24381f9c5d00e",
                "status": "Exit",
                "side": "1",
                      "dwellTime": 7132.0,
                      "dwellFrames": 20            
            },
            "zone": "queuecamera"
        }
    ],
    "sourceInfo": {
        "id": "camera_id",
        "timestamp": "2020-08-24T06:15:09.680Z",
        "width": 608,
        "height": 342,
        "frameId": "428",
        "imagePath": ""
    },
    "detections": [
        {
            "type": "person",
            "id": "afcc2e2a32a6480288e24381f9c5d00e",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.8135572734631991,
                        "y": 0.6653949670624315
                    },
                    {
                        "x": 0.9937645761590255,
                        "y": 0.9925406829655519
                    }
                ]
            },
            "confidence": 0.6267998814582825,
            "metadataType": "",
             "metadata": { 
                     "groundOrientationAngle": 1.2,
                     "mappedImageOrientation": 0.3,
                     "speed": 1.2
               },
        }
    ],
    "schemaVersion": "1.0"
}
```

| Nom de champ d'événement | Type| Description|
|---------|---------|---------|
| `id` | string| ID de l’événement|
| `type` | string| Type d'événement. La valeur peut être _personZoneDwellTimeEvent_ ou _personZoneEnterExitEvent_.|
| `detectionsId` | tableau| Tableau de taille 1 de l’identificateur unique de la détection de personne qui a déclenché cet événement|
| `properties` | collection| Collection de valeurs|
| `trackinId` | string| Identificateur unique de la personne détectée|
| `status` | string| Direction des polygones croisés, « Enter » ou « Exit »|
| `side` | int| Numéro du côté du polygone traversé par la personne. Chaque côté est un bord numéroté entre les deux sommets du polygone qui représente votre zone. Le bord entre les deux premiers sommets du polygone représente le premier côté. Le « côté » est vide lorsque l’événement n’est pas associé à un côté spécifique en raison d’une occlusion. Par exemple, une sortie s’est produite quand une personne a disparu mais n’a pas été observée traversant un côté de la zone ou une entrée s’est produite quand une personne est apparue dans la zone, mais n’a pas été observée traversant un côté.|
| `dwellTime` | float | Nombre de millisecondes qui représentent le temps passé par la personne dans la zone. Ce champ est fourni quand le type d’événement est personZoneDwellTimeEvent|
| `dwellFrames` | int | Nombre de frames que la personne a passées à la zone. Ce champ est fourni quand le type d’événement est personZoneDwellTimeEvent|
| `dwellTimeForTargetSide` | float | Nombre de millisecondes représentant le temps que la personne a passé dans la zone et pendant lequel elle était tournée du côté `target_side`. Ce champ est fourni quand `enable_orientation` a la valeur `True` dans `CAMERACALIBRATOR_NODE_CONFIG ` et que la valeur de `target_side` est définie dans `SPACEANALYTICS_CONFIG`|
| `avgSpeed` | float| Vitesse moyenne de la personne dans la zone. L’unité est `foot per second (ft/s)`|
| `minSpeed` | float| Vitesse minimale de la personne dans la zone. L’unité est `foot per second (ft/s)`|
| `zone` | string | Le champ « name » du polygone qui représente la zone qui a été franchie|

| Nom du champ de détection | Type| Description|
|---------|---------|---------|
| `id` | string| ID de détection|
| `type` | string| Type de détection|
| `region` | collection| Collection de valeurs|
| `type` | string| Type de région|
| `points` | collection| Points supérieur gauche et inférieur droit lorsque le type de région est RECTANGLE |
| `groundOrientationAngle` | float| Angle en radians dans le sens horaire de l’orientation de la personne sur le plan de sol déduit |
| `mappedImageOrientation` | float| L’angle projeté en radians dans le sens horaire de l’orientation de la personne sur l’espace de l’image 2D |
| `speed` | float| Vitesse estimée de la personne détectée. L’unité est `foot per second (ft/s)`|
| `confidence` | float| Confiance de l’algorithme|
| `face_mask` | float | La valeur de confiance de l’attribut avec la plage (0-1) indique que la personne détectée porte un masque |
| `face_nomask` | float | La valeur de confiance de l’attribut avec la plage (0-1) indique que la personne détectée ne porte **pas** de masque |

### <a name="json-format-for-cognitiveservicesvisionspatialanalysis-persondistance-ai-insights"></a>Format JSON pour les insights d’intelligence artificielle cognitiveservices.vision.spatialanalysis-persondistance

Exemple de code JSON pour une sortie de détections par cette opération.

```json
{
    "events": [
        {
            "id": "9c15619926ef417aa93c1faf00717d36",
            "type": "personDistanceEvent",
            "detectionIds": [
                "9037c65fa3b74070869ee5110fcd23ca",
                "7ad7f43fd1a64971ae1a30dbeeffc38a"
            ],
            "properties": {
                "personCount": 5,
                "averageDistance": 20.807043981552123,
                "minimumDistanceThreshold": 6.0,
                "maximumDistanceThreshold": "Infinity",
                "eventName": "TooClose",
                "distanceViolationPersonCount": 2
            },
            "zone": "lobbycamera",
            "trigger": "event"
        }
    ],
    "sourceInfo": {
        "id": "camera_id",
        "timestamp": "2020-08-24T06:17:25.309Z",
        "width": 608,
        "height": 342,
        "frameId": "1199",
        "cameraCalibrationInfo": {
            "status": "Calibrated",
            "cameraHeight": 12.9940824508667,
            "focalLength": 401.2800598144531,
            "tiltupAngle": 1.057669997215271
        },
        "imagePath": ""
    },
    "detections": [
        {
            "type": "person",
            "id": "9037c65fa3b74070869ee5110fcd23ca",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.39988183975219727,
                        "y": 0.2719132942065858
                    },
                    {
                        "x": 0.5051516984638414,
                        "y": 0.6488402517218339
                    }
                ]
            },
            "confidence": 0.948630690574646,
            "centerGroundPoint": {
                "x": -1.4638760089874268,
                "y": 18.29732322692871
            },
            "metadataType": ""
        },
        {
            "type": "person",
            "id": "7ad7f43fd1a64971ae1a30dbeeffc38a",
            "region": {
                "type": "RECTANGLE",
                "points": [
                    {
                        "x": 0.5200299714740954,
                        "y": 0.2875368218672903
                    },
                    {
                        "x": 0.6457497446160567,
                        "y": 0.6183311060855263
                    }
                ]
            },
            "confidence": 0.8235412240028381,
            "centerGroundPoint": {
                "x": 2.6310102939605713,
                "y": 18.635927200317383
            },
            "metadataType": ""
        }
    ],
    "schemaVersion": "1.0"
}
```

| Nom de champ d'événement | Type| Description|
|---------|---------|---------|
| `id` | string| ID de l’événement|
| `type` | string| Type d'événement|
| `detectionsId` | tableau| Tableau de taille 1 de l’identificateur unique de la détection de personne qui a déclenché cet événement|
| `properties` | collection| Collection de valeurs|
| `personCount` | int| Nombre de personnes détectées lors de l’émission de l’événement|
| `averageDistance` | float| Distance moyenne entre tous les individus détectés, en mètres|
| `minimumDistanceThreshold` | float| La distance en pieds qui déclenchera un événement « TooClose » lorsque les personnes sont moins éloignées que cette distance.|
| `maximumDistanceThreshold` | float| La distance en pieds qui déclenchera un événement « TooFar » lorsque les personnes sont plus éloignées que cette distance.|
| `eventName` | string| Le nom de l’événement est `TooClose` avec le `minimumDistanceThreshold` enfreint, `TooFar` lorsque le `maximumDistanceThreshold` n’est pas respecté, ou `unknown` lorsque l’étalonnage automatique n’est pas terminé|
| `distanceViolationPersonCount` | int| Nombre de personnes détectées en violation de `minimumDistanceThreshold` ou `maximumDistanceThreshold`|
| `zone` | string | Le champ « name » du polygone qui représente la zone surveillée pour la distance entre personnes|
| `trigger` | string| Le type de déclencheur est « event » ou « interval » en fonction de la valeur de `trigger` dans SPACEANALYTICS_CONFIG|

| Nom du champ de détection | Type| Description|
|---------|---------|---------|
| `id` | string| ID de détection|
| `type` | string| Type de détection|
| `region` | collection| Collection de valeurs|
| `type` | string| Type de région|
| `points` | collection| Points supérieur gauche et inférieur droit lorsque le type de région est RECTANGLE |
| `confidence` | float| Confiance de l’algorithme|
| `centerGroundPoint` | 2 valeurs float| Valeurs `x`, `y` avec les coordonnées de l’emplacement déduit de la personne sur le sol, en mètres. `x` et `y` sont des coordonnées sur le plan de sol, en supposant que le sol est plat. La position de la caméra est l’origine. |

Lors du calcul de `centerGroundPoint`, `x` est la distance entre la caméra et la personne sur une ligne perpendiculaire au plan d’image de la caméra. `y` est la distance entre la caméra et la personne sur une ligne parallèle au plan d’image de la caméra. 

![Exemple de point au sol central](./media/spatial-analysis/x-y-chart.png) 

Dans cet exemple, `centerGroundPoint` est `{x: 4, y: 5}`. Cela signifie qu’il y a une personne située à 4 pieds (1,2 mètre) de la caméra et à 5 pieds (1,5 mètre) vers la droite, en regardant la salle du dessus.


| Nom du champ SourceInfo | Type| Description|
|---------|---------|---------|
| `id` | string| ID de la caméra|
| `timestamp` | date| Date UTC à laquelle la charge utile JSON a été émise|
| `width` | int | Largeur de l’image vidéo|
| `height` | int | Hauteur de l’image vidéo|
| `frameId` | int | Identificateur du frame|
| `cameraCallibrationInfo` | collection | Collection de valeurs|
| `status` | string | État de l’étalonnage au format `state[;progress description]`. L’état peut être `Calibrating`, `Recalibrating` (si le réétalonnage est activé) ou `Calibrated`. La partie description de la progression est valide uniquement quand elle est dans l’état `Calibrating` et `Recalibrating`, qui est utilisé pour afficher la progression du processus d’étalonnage actuel.|
| `cameraHeight` | float | Hauteur de la caméra au-dessus du sol, en mètres. Cette valeur est déduite de l’étalonnage automatique. |
| `focalLength` | float | Longueur focale de la caméra, en pixels. Cette valeur est déduite de l’étalonnage automatique. |
| `tiltUpAngle` | float | Angle d’inclinaison de la caméra par rapport à la verticale. Cette valeur est déduite de l’étalonnage automatique.|

### <a name="json-format-for-cognitiveservicesvisionspatialanalysis-ai-insights"></a>Format JSON pour les insights d’intelligence artificielle cognitiveservices.vision.spatialanalysis

La sortie de cette opération dépend du paramètre `events` configuré, par exemple s’il existe un événement `zonecrossing` configuré pour cette opération, alors la sortie est identique à `cognitiveservices.vision.spatialanalysis-personcrossingpolygon`.

## <a name="use-the-output-generated-by-the-container"></a>Utiliser la sortie générée par le conteneur

Vous pouvez intégrer la détection ou les événements d’analyse spatiale dans votre application. Voici quelques approches à envisager : 

* Utilisez le kit de développement logiciel (SDK) Azure Event Hub pour le langage de programmation que vous avez choisi pour vous connecter au point de terminaison Azure IoT Hub et recevoir les événements. Pour plus d’informations, consultez [Lire des messages appareil-à-cloud à partir du point de terminaison intégré](../../iot-hub/iot-hub-devguide-messages-read-builtin.md). 
* Configurez le **routage des messages** sur votre instance IoT Hub Azure pour envoyer les événements à d’autres points de terminaison ou enregistrer les événements dans votre stockage de données. Pour plus d’informations, consultez [Routage des messages IoT Hub](../../iot-hub/iot-hub-devguide-messages-d2c.md). 
* Configurez un travail Azure Stream Analytics pour traiter les événements en temps réel à mesure qu’ils arrivent et créer des visualisations. 

## <a name="deploying-spatial-analysis-operations-at-scale-multiple-cameras"></a>Déploiement d’opérations d’analyse spatiale à grande échelle (plusieurs caméras)

Pour optimiser les performances et l’utilisation des GPU, vous pouvez déployer toutes les opérations d’analyse spatiale sur plusieurs caméras à l’aide d’instances de graphe. Voici un exemple d’exécution de l’opération `cognitiveservices.vision.spatialanalysis-personcrossingline` sur quinze caméras.

```json
  "properties.desired": {
      "globalSettings": {
          "PlatformTelemetryEnabled": false,
          "CustomerTelemetryEnabled": true
      },
      "graphs": {
        "personzonelinecrossing": {
        "operationId": "cognitiveservices.vision.spatialanalysis-personcrossingline",
        "version": 1,
        "enabled": true,
        "sharedNodes": {
            "shared_detector0": {
                "node": "PersonCrossingLineGraph.detector",
                "parameters": {
                    "DETECTOR_NODE_CONFIG": "{ \"gpu_index\": 0, \"batch_size\": 7, \"do_calibration\": true}",
                }
            },
            "shared_calibrator0": {
                "node": "PersonCrossingLineGraph/cameracalibrator",
                "parameters": {
                    "CAMERACALIBRATOR_NODE_CONFIG": "{ \"gpu_index\": 0, \"do_calibration\": true, \"enable_zone_placement\": true}",
                    "CALIBRATION_CONFIG": "{\"enable_recalibration\": true, \"quality_check_frequency_seconds\": 86400}",
                }
        },
        "parameters": {
            "VIDEO_DECODE_GPU_INDEX": 0,
            "VIDEO_IS_LIVE": true
        },
        "instances": {
            "1": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 1>",
                    "VIDEO_SOURCE_ID": "camera 1",
                    "SPACEANALYTICS_CONFIG": "{\"zones\":[{\"name\":\"queue\",\"polygon\":[[0,0],[1,0],[0,1],[1,1],[0,0]]}]}"
                }
            },
            "2": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 2>",
                    "VIDEO_SOURCE_ID": "camera 2",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "3": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 3>",
                    "VIDEO_SOURCE_ID": "camera 3",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "4": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 4>",
                    "VIDEO_SOURCE_ID": "camera 4",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "5": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 5>",
                    "VIDEO_SOURCE_ID": "camera 5",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "6": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 6>",
                    "VIDEO_SOURCE_ID": "camera 6",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "7": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 7>",
                    "VIDEO_SOURCE_ID": "camera 7",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "8": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 8>",
                    "VIDEO_SOURCE_ID": "camera 8",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "9": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 9>",
                    "VIDEO_SOURCE_ID": "camera 9",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "10": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 10>",
                    "VIDEO_SOURCE_ID": "camera 10",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "11": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 11>",
                    "VIDEO_SOURCE_ID": "camera 11",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "12": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 12>",
                    "VIDEO_SOURCE_ID": "camera 12",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "13": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 13>",
                    "VIDEO_SOURCE_ID": "camera 13",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "14": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 14>",
                    "VIDEO_SOURCE_ID": "camera 14",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            },
            "15": {
                "sharedNodeMap": {
                    "PersonCrossingLineGraph/detector": "shared_detector0",
            "PersonCrossingLineGraph/cameracalibrator": "shared_calibrator0",
                },
                "parameters": {
                    "VIDEO_URL": "<Replace RTSP URL for camera 15>",
                    "VIDEO_SOURCE_ID": "camera 15",
                    "SPACEANALYTICS_CONFIG": "<Replace the zone config value, same format as above>"
                }
            }
          }
        },
      }
  }
  ```
| Nom | Type| Description|
|---------|---------|---------|
| `batch_size` | int | Si toutes les caméras ont la même résolution, définissez `batch_size` sur le nombre de caméras qui seront utilisées dans cette opération ; sinon, attribuez la valeur 1 à `batch_size` ou laissez la valeur par défaut (1), ce qui indique qu’aucun lot n’est pris en charge. |

## <a name="next-steps"></a>Étapes suivantes

* [Déployer une application web de comptage de personnes](spatial-analysis-web-app.md)
* [Journalisation et résolution des problèmes](spatial-analysis-logging.md)
* [Guide de positionnement de la caméra](spatial-analysis-camera-placement.md)
* [Guide de positionnement de zone et de ligne](spatial-analysis-zone-line-placement.md)
