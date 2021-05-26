---
title: Extension de pipeline - Azure Video Analyzer
description: Azure Video Analyzer vous permet d’étendre les fonctionnalités de traitement de pipeline à l’aide d’un nœud d’extension de pipeline. Cet article décrit le nœud d’extension de pipeline.
ms.service: azure-video-analyzer
ms.topic: conceptual
ms.date: 03/30/2021
ms.openlocfilehash: 59476f465d65db6e716ca5e5b43da995d8e737de
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2021
ms.locfileid: "110386124"
---
# <a name="pipeline-extension"></a>Extension de pipeline

Azure Video Analyzer vous permet d’étendre les fonctionnalités de traitement de pipeline à l’aide d’un nœud d’extension de pipeline. Votre plug-in d’extension Analytics peut utiliser des techniques de traitement d’image traditionnelles ou des modèles d’IA de vision par ordinateur. Les extensions de pipeline sont activées en ajoutant un nœud de processeur d’extension dans le flux de pipeline. Le nœud de processeur d’extension relaie les images vidéo vers le point de terminaison configuré et agit comme une interface de votre extension. La connexion peut être établie à un point de terminaison local ou distant, et peut être sécurisée par l’authentification et le chiffrement TLS, si nécessaire. Par ailleurs, le nœud de processeur d’extension de pipeline permet la mise à l’échelle et l’encodage facultatifs des images vidéo avant leur envoi à votre extension personnalisée.

Video Analyzer prend en charge les processeurs d’extensions de pipeline suivants :

* [Processeur d’extension HTTP](pipeline.md#http-extension-processor) 
* [Processeur d’extension gRPC](pipeline.md#grpc-extension-processor)
* [Processeur d’extension Cognitive Services](pipeline.md#cognitive-services-extension-processor) 
    
Le nœud d’extension de pipeline attend des résultats au format JSON de la part du plug-in d’extension analytique. Dans l’idéal, les résultats doivent suivre le [modèle d’objet du schéma des métadonnées d’inférence](inference-metadata-schema.md)

## <a name="http-extension-processor"></a>Processeur d’extension HTTP

Le processeur d’extension HTTP permet des scénarios d’extensibilité à l’aide du [protocole HTTP](http-extension-protocol.md), dans lesquels les performances et/ou l’utilisation optimale des ressources ne constituent pas la préoccupation principale. Vous pouvez exposer votre propre IA à un pipeline via un point de terminaison REST HTTP.

Utilisez le nœud de processeur d’extension HTTP lorsque :

* Vous souhaitez une meilleure interopérabilité avec les systèmes d’inférence HTTP existants.
* Un transfert de données à faibles performances est acceptable.
* Vous voulez utiliser une interface demande-réponse simple dans Video Analyzer.

## <a name="grpc-extension-processor"></a>Processeur d’extension gRPC

Le processeur d’extension gRPC permet des scénarios d’extensibilité à l’aide d’un [protocole structuré](grpc-extension-protocol.md) hautement performant, s’appuyant sur gRPC. Il est idéal pour les scénarios où les performances et/ou l’utilisation optimale des ressources sont une priorité. Le processeur d’extension gRPC permet de tirer pleinement parti des définitions de données structurées. gRPC offre de hautes performances de transfert de contenu avec :

* la [mémoire partagée intégrée](https://en.wikipedia.org/wiki/Shared_memory) ou
* l’incorporation directe du contenu dans le corps des messages gRPC.

Le processeur d’extension gRPC peut être utilisé pour envoyer des propriétés tout en échangeant des messages d’inférence. Par conséquent, utilisez un nœud de processeur d’extension gRPC lorsque :

* Vous voulez utiliser un contrat structuré (par exemple, des messages structurés pour les requêtes et les réponses)
* Vous voulez utiliser des [mémoires tampons de protocole (protobuf)](https://developers.google.com/protocol-buffers) comme format d’échange de messages sous-jacent pour la communication.
* Vous voulez communiquer avec un serveur gRPC dans une session de flux unique à la place du modèle de requête-réponse traditionnel nécessitant un gestionnaire de demandes personnalisé pour analyser les requêtes entrantes et appeler les fonctions d’implémentation appropriées.
* Vous voulez une faible latence et une communication haut débit entre Video Analyzer et votre module.

## <a name="cognitive-services-extension-processor"></a>Processeur d’extension Cognitive Services

Le processeur d’extension Cognitive Services est un processeur d’extension personnalisé qui permet à Video Analyzer de fonctionner correctement avec les fonctionnalités d’[analyse spatiale de Vision par ordinateur]../../cognitive-services/computer-vision/) à l’aide du [protocole structuré](grpc-extension-protocol.md) gRPC hautement performant. 

Utilisez le nœud de processeur d’extension Cognitive Services quand :

* Vous voulez une meilleure interopérabilité avec les [opérations d’analyse spatiale](../../cognitive-services/computer-vision/intro-to-spatial-analysis-public-preview.md) existantes.
* Vous voulez tirer parti de tous les avantages du protocole gRPC, de la précision et des performances de l’IA prise en charge et générée par Microsoft.
* Vous analysez plusieurs flux de caméra à faible latence et haut débit.

## <a name="use-your-inferencing-model"></a>Utiliser votre modèle d’inférence

Les extensions de pipeline vous permettent d’exécuter les modèles d’inférence de votre choix sur n’importe quel runtime d’inférence disponible, comme ONNX, TensorFlow, PyTorch ou autres, dans votre propre conteneur Docker. L’extension personnalisée d’inférence doit être déployée en même temps que le module périphérique Video Analyzer pour optimiser les performances. Elle est alors appelée via le processeur d’extension HTTP, le processeur d’extension gRPC ou le processeur d’extension Cognitive Services compris dans votre topologie de pipeline. Par ailleurs, la fréquence des appels dans votre extension personnalisée peut être limitée par l’ajout facultatif d’un [processeur de détection de mouvement](pipeline.md#motion-detection-processor) en amont du processeur d’extension de pipeline.

Le diagramme ci-dessous illustre le flux de données général :

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/pipeline-extension/ai-inference-results.svg" alt-text="Modèle d’inférence":::
 
## <a name="samples"></a>Exemples

Vous pouvez démarrer en utilisant l’un de nos guides de démarrage rapide qui décrivent l’utilisation de Video Analyzer avec un service d’extension prédéfini à des fréquences d’images faibles à l’aide du [processeur d’extension HTTP](pipeline.md#http-extension-processor) ou à des fréquences d’images élevées à l’aide du [processeur d’extension gRPC](pipeline.md#grpc-extension-processor).


## <a name="next-steps"></a>Étapes suivantes 

Concept : [Enregistrement vidéo basé sur un événement](event-based-video-recording-concept.md)

