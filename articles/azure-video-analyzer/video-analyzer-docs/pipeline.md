---
title: Pipeline Azure Video Analyzer
description: Un pipeline Azure Video Analyzer vous permet de définir l’emplacement où les données d’entrée doivent être capturées, la manière dont elles doivent être traitées ainsi que l’emplacement où les résultats doivent être remis. Un pipeline se compose de nœuds connectés pour obtenir le flux de données souhaité.
ms.topic: conceptual
ms.date: 05/13/2021
ms.openlocfilehash: deed7eec980272dd3dd001219016eee139f22ad6
ms.sourcegitcommit: b11257b15f7f16ed01b9a78c471debb81c30f20c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/08/2021
ms.locfileid: "111591239"
---
# <a name="pipeline"></a>Pipeline

Un pipeline Azure Video Analyzer vous permet de définir l’emplacement où les données d’entrée doivent être capturées, la manière dont elles doivent être traitées ainsi que l’emplacement où les résultats doivent être remis. Un pipeline se compose de nœuds connectés pour obtenir le flux de données souhaité. Le diagramme ci-dessous fournit une représentation graphique d’un pipeline.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/pipeline/pipeline-representation.svg" alt-text="Représentation d’un pipeline":::

Un pipeline prend en charge différents types de nœud

* Les **nœuds sources** permettent la capture des données dans le pipeline. Les données font référence au contenu audio, au contenu vidéo et/ou aux métadonnées.
* Les **nœuds processeurs** permettent le traitement du contenu multimédia dans le pipeline.
* Les **nœuds récepteurs** permettent la remise des résultats aux services et applications situés à l’extérieur du pipeline.

## <a name="suggested-pre-reading"></a>Lecture préalable suggérée

* [Vue d'ensemble](overview.md)
* [Terminologie](terminology.md)

## <a name="pipeline-topologies"></a>Topologies de pipeline

Une topologie de pipeline vous permet de décrire la façon dont une vidéo en direct doit être traitée et analysée pour vos besoins personnalisés via un ensemble de nœuds interconnectés. Vous pouvez créer diverses topologies pour différents scénarios en sélectionnant les nœuds à inclure dans la topologie ainsi que leur mode de connexion, avec des paramètres en tant qu’espaces réservés pour les valeurs. Un pipeline est une instance individuelle d’une topologie de pipeline spécifique. Un pipeline représente l’emplacement où le contenu multimédia est réellement traité. Les pipelines peuvent être associés à des caméras individuelles (ou d’autres aspects) via des paramètres définis par l’utilisateur et déclarés dans la topologie de pipeline.

Par exemple, si vous souhaitez enregistrer des vidéos à partir de plusieurs caméras IP, vous pouvez définir une topologie de pipeline composée d’un nœud source RTSP et d’un nœud récepteur vidéo. Le nœud source RTSP peut avoir une URL, un nom d’utilisateur et un mot de passe RTSP en tant que paramètres. Le nœud du récepteur vidéo peut avoir le nom de la vidéo en tant que paramètre. Les valeurs de ces paramètres peuvent être fournies au moment de la création de plusieurs pipelines à partir de la même topologie, avec un pipeline par caméra.

## <a name="pipeline-states"></a>États du pipeline

Le cycle de vie d’un pipeline est représenté dans le diagramme ci-dessous.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/pipeline/pipeline-activation.svg" alt-text="Cycle de vie d’un pipeline":::

Commencez par créer la topologie de pipeline. Une fois la topologie définie, vous pouvez créer des pipelines en fournissant les valeurs des paramètres. En cas de création réussie, un pipeline est à l’état « Inactif ». Au moment de l’activation, un pipeline entre dans l’état « Activation », puis passe à l’état « Actif ». 

Les données (vidéo en direct) commencent à circuler dans le pipeline une fois qu’il est passé à l’état « Actif ». Au moment de la désactivation, un pipeline actif entre dans l’état « Désactivation », puis passe à l’état « Inactif ». Seuls les pipelines inactifs peuvent être supprimés.

Vous pouvez créer plusieurs pipelines à partir d’une seule topologie en fournissant des valeurs distinctes pour les paramètres de la topologie. Une topologie peut être supprimée quand tous les pipelines ont été supprimés.

> [!NOTE]
> Un pipeline peut être actif sans flux de données (par exemple, la source vidéo d’entrée passe hors connexion). Votre abonnement Azure est facturé quand le pipeline est à l’état actif.

## <a name="sources-processors-and-sinks"></a>Sources, processeurs et récepteurs

Le module de périphérie Video Analyzer prend en charge les types de nœud suivants dans un pipeline :

### <a name="sources"></a>Sources

#### <a name="rtsp-source"></a>Source RTSP

Un nœud source RTSP vous permet de capturer le contenu multimédia à partir d’un serveur RTSP. Le protocole RTSP définit l’établissement et le contrôle des sessions multimédias entre un serveur (par exemple une caméra IP) et un client. Dans un pipeline, le nœud source [RTSP](https://tools.ietf.org/html/rfc2326) fait office de client et peut établir une session avec un serveur RTSP. De nombreux appareils, par exemple la plupart des [caméras IP](https://en.wikipedia.org/wiki/IP_camera), disposent d’un serveur RTSP intégré. [ONVIF](https://www.onvif.org/) exige la prise en charge de RTSP dans sa définition des appareils conformes aux [profils G, S et T](https://www.onvif.org/wp-content/uploads/2019/12/ONVIF_Profile_Feature_overview_v2-3.pdf). Le nœud source RTSP vous oblige à spécifier une URL RTSP, ainsi que les informations d’identification permettant une connexion authentifiée.

#### <a name="iot-hub-message-source"></a>Source de messages IoT Hub

Comme d’autres [modules IoT Edge](../../iot-fundamentals/iot-glossary.md?view=iotedge-2020-11&preserve-view=true#iot-edge-device), le module Azure Video Analyzer peut recevoir des messages via le [hub IoT Edge](../../iot-fundamentals/iot-glossary.md?view=iotedge-2020-11&preserve-view=true#iot-edge-hub). Les messages peuvent être envoyés à partir d’autres modules, à partir d’applications en cours d’exécution sur l’appareil de périphérie ou à partir du cloud. Ces messages peuvent être remis (routés) à une [entrée nommée](../../iot-edge/module-composition.md?view=iotedge-2020-11&preserve-view=true#sink) dans le module Video Analyzer. Un nœud source de message IoT Hub permet l’ingestion de ce genre de message dans un pipeline. Les messages peuvent ensuite être utilisés dans un pipeline pour activer une porte de signal (consultez [portes de signal](#signal-gate-processor) ci-dessous).

Par exemple, vous pouvez avoir un module IoT Edge qui génère un message quand une porte est ouverte. Le message de ce module peut être routé vers un hub IoT Edge, d’où il peut ensuite être routé vers la source des messages du hub IoT d’un pipeline. Dans le pipeline, le message peut passer de la source des messages du hub IoT à un processeur de porte de signal, lequel peut ensuite activer l’enregistrement de la vidéo depuis une source RTSP dans un fichier.

### <a name="processors"></a>Processeurs

#### <a name="motion-detection-processor"></a>Processeur de détection de mouvement

Le nœud processeur de détection de mouvement vous permet de détecter les mouvements dans une vidéo en direct. Il examine les images vidéo entrantes et détermine s’il y a un mouvement dans la vidéo. Si un mouvement est détecté, il déclenche le passage de l’image vidéo au nœud suivant du pipeline, et l’émission d’un événement. Le nœud processeur de détection de mouvement (conjointement avec d’autres nœuds) peut être utilisé pour déclencher l’enregistrement de la vidéo entrante quand un mouvement est détecté.

#### <a name="http-extension-processor"></a>Processeur d’extension HTTP

Le nœud processeur d’extension HTTP vous permet d’étendre le pipeline à votre propre module IoT Edge. Ce nœud accepte les images vidéo décodées en entrée et les relaie vers un point de terminaison HTTP REST exposé par votre module, où vous pouvez les analyser à l’aide d’un modèle IA pour retourner ensuite les résultats de l’inférence. De plus, ce nœud dispose d’un formateur d’image intégré pour la mise à l’échelle et l’encodage des images vidéo avant qu’elles ne soient relayées au point de terminaison HTTP. Le processus de mise à l’échelle propose des options permettant de préserver les proportions de l’image, de remplir cette dernière ou de l’étirer. L’encodeur d’image prend en charge les formats JPEG, PNG, BMP et RAW. Découvrez plus en détail le [processeur ici](pipeline-extension.md#http-extension-processor).

#### <a name="grpc-extension-processor"></a>Processeur d’extension gRPC

Le nœud du processeur d’extension gRPC prend les images vidéo décodées comme entrée et les relaie à un point de terminaison [gRPC](terminology.md#grpc) exposé par votre module. Le nœud prend en charge le transfert de données à l’aide de la [mémoire partagée](https://en.wikipedia.org/wiki/Shared_memory) ou l’incorporation directe de l’image dans le corps des messages gRPC. Tout comme le processus d’extension HTTP, ce nœud dispose d’un formateur d’image intégré pour la mise à l’échelle et l’encodage des images vidéo avant qu’elles ne soient relayées au point de terminaison gRPC. Découvrez plus en détail le [processeur ici](pipeline-extension.md#grpc-extension-processor).

#### <a name="cognitive-services-extension-processor"></a>Processeur d’extension Cognitive Services

Le nœud processeur d’extension Cognitive Services vous permet d’étendre le pipeline au module IoT Edge d’[analyse spatiale](https://azure.microsoft.com/services/cognitive-services/computer-vision/). Ce nœud accepte les images vidéo décodées en entrée et les relaie vers un point de terminaison [gRPC](pipeline-extension.md#grpc-extension-processor) exposé, où vous pouvez les analyser à l’aide de l’analyse spatiale pour retourner ensuite les résultats de l’inférence. Découvrez plus en détail le [processeur ici](pipeline-extension.md#cognitive-services-extension-processor).

#### <a name="signal-gate-processor"></a>Processeur de porte de signal

Le nœud processeur de la porte de signal vous permet de transférer de manière conditionnelle des médias d’un nœud à un autre. Le nœud de processeur de porte de signal doit être immédiatement suivi d’un récepteur vidéo ou d’un récepteur de fichiers. Par exemple, un cas d’usage classique consiste à insérer un nœud processeur de porte de signal entre le nœud source RTSP et le nœud récepteur vidéo, puis à utiliser la sortie d’un nœud processeur de détecteur de mouvement pour déclencher la porte. Avec ce type de pipeline, vous enregistrez la vidéo uniquement quand un mouvement est détecté. Vous pouvez également utiliser la sortie du nœud d’extension HTTP ou gRPC pour déclencher la porte, à la place du nœud processeur de détection de mouvement, ce qui permet ainsi l’enregistrement de la vidéo quand un élément intéressant est détecté.

#### <a name="object-tracker-processor"></a>Processeur de suivi d’objets

Le nœud processeur de suivi d’objets vous permet d’effectuer le suivi des objets détectés dans un nœud processeur d’extension HTTP ou gRPC amont. Ce nœud s’avère utile quand vous devez détecter des objets dans chaque image, mais que l’appareil de périphérie n’a pas la puissance de calcul nécessaire pour pouvoir appliquer le modèle IA à chaque image. Si vous ne pouvez exécuter votre modèle de vision par ordinateur que toutes les 10 images, le suivi d’objets peut utiliser les résultats d’une de ces images, puis appliquer les techniques de [flux optique](https://en.wikipedia.org/wiki/Optical_flow) afin de générer des résultats pour la 2e, la 3e…, la 9e image, jusqu’à ce que le modèle soit de nouveau appliqué à l’image suivante. Il existe un compromis entre puissance de calcul et justesse quand ce nœud est utilisé. Plus les images auxquelles le modèle IA est appliqué sont proches, meilleure est la justesse. Toutefois, cela implique d’appliquer le modèle IA plus fréquemment, ce qui se traduit par une puissance de calcul plus élevée. Le nœud processeur de suivi d’objets est couramment utilisé pour détecter le moment où un objet franchit une ligne.

#### <a name="line-crossing-processor"></a>Processeur de franchissement de ligne

Le nœud processeur de franchissement de ligne vous permet de détecter le moment où un objet franchit une ligne que vous avez définie. De plus, il effectue également un décompte du nombre d’objets qui franchissent la ligne (à partir du moment où un pipeline est activé). Ce nœud doit être utilisé en aval d’un nœud processeur de suivi d’objets.

### <a name="sinks"></a>Récepteurs

#### <a name="video-sink"></a>Récepteur vidéo

Un nœud récepteur vidéo vous permet d’enregistrer une vidéo et les métadonnées associées dans votre ressource cloud Video Analyzer. La vidéo peut être enregistrée en continu ou en discontinu (en fonction des événements). Le nœud récepteur vidéo peut mettre en cache la vidéo sur l’appareil de périphérie si la connectivité au cloud est perdue, et peut reprendre le chargement quand la connectivité est rétablie. Pour plus d’informations sur la configuration des propriétés de ce nœud, consultez l’article relatif à l’[enregistrement vidéo continu](continuous-video-recording.md).

#### <a name="file-sink"></a>Récepteur de fichiers

Le nœud récepteur de fichiers vous permet d’écrire une vidéo dans un emplacement du système de fichiers local sur l’appareil de périphérie. Il ne peut exister qu’un seul nœud récepteur de fichiers dans un pipeline. Il doit se situer en aval d’un nœud processeur de porte de signal. Cela limite la durée des fichiers de sortie aux valeurs spécifiées dans les propriétés du nœud processeur de la porte de signal. Si vous souhaitez avoir la garantie que votre appareil de périphérie ne manque pas d’espace disque, vous pouvez également définir la taille maximale utilisable par le module de périphérie Video Analyzer pour mettre en cache les données.

> [!NOTE]
> Si le cache est proche de la saturation, le module de périphérie Video Analyzer se met à supprimer les données les plus anciennes pour les remplacer par les nouvelles.

#### <a name="iot-hub-message-sink"></a>Récepteur de messages IoT Hub

Un nœud récepteur de messages IoT Hub vous permet de publier des événements sur un hub IoT Edge. Vous pouvez configurer le hub IoT Edge pour router les données vers d’autres modules ou applications sur l’appareil de périphérie ou vers le hub IoT dans le cloud (en fonction des routes spécifiées dans le manifeste de déploiement). Le nœud récepteur de messages IoT Hub peut accepter des événements provenant de nœuds processeurs amont, par exemple un nœud processeur de détection de mouvement, ou provenant d’un service d’inférence externe via un nœud processeur d’extension HTTP.

## <a name="rules-on-the-use-of-nodes"></a>Règles d’utilisation des nœuds

Pour obtenir des règles supplémentaires sur la façon dont différents nœuds peuvent être utilisés dans un pipeline, consultez les [limitations relatives aux pipelines](quotas-limitations.md#limitations-on-pipeline-topologies).

## <a name="scenarios"></a>Scénarios

En combinant les sources, les processeurs et les récepteurs définis ci-dessus, vous pouvez créer des pipelines pour un large éventail de scénarios impliquant une analyse de vidéos en direct. Par exemple :

* [Enregistrement vidéo en continu](continuous-video-recording.md) 
* [Enregistrement de vidéo basé sur les événements](event-based-video-recording-concept.md) 
* [Analyse vidéo sans enregistrement vidéo](analyze-live-video-without-recording.md) 

## <a name="next-steps"></a>Étapes suivantes

Pour savoir comment exécuter la détection de mouvement sur un flux vidéo en direct, consultez [Démarrage rapide : Bien démarrer avec Azure Video Analyzer](get-started-detect-motion-emit-events.md).

