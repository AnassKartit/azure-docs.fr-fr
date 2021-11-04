---
author: Juliako
ms.service: azure-video-analyzer
ms.topic: include
ms.date: 04/07/2021
ms.author: juliako
ms.openlocfilehash: 48168a651cb87af56b114821e029e1bba3b3eb4b
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131097170"
---
Dans le cadre des prérequis, vous avez téléchargé l’exemple de code dans un dossier. Effectuez les étapes suivantes pour examiner et modifier les exemples de fichiers.

1. Dans Visual Studio Code, accédez à *src/edge*. Votre fichier  *.env* et quelques fichiers de modèle de déploiement s’affichent.
Le modèle de déploiement fait référence au manifeste de déploiement de l’appareil de périphérie. Il inclut des valeurs d’espace réservé. Le fichier .env inclut les valeurs de ces variables.
1. Accédez au dossier *src/cloud-to-device-console-app*. Ce dernier contient votre fichier *appsettings.json* et quelques autres fichiers :
    
    * c2d-console-app.csproj : fichier projet pour Visual Studio Code.
    * operations.json : liste des opérations que vous voulez que le programme exécute.
    * Program.cs : exemple de code de programme. Ce code :
    
        * Il charge les paramètres de l’application.
        * Appelle les méthodes directes exposées par le module Azure Video Analyzer. Vous pouvez utiliser le module pour analyser des flux vidéo en direct en invoquant ses méthodes directes.
        * S’interrompt pour vous permettre d’examiner la sortie du programme dans la fenêtre **TERMINAL** et d’examiner les événements qui ont été générés par le module dans la fenêtre **SORTIE**.
        * Invoque des méthodes directes pour nettoyer des ressources.
1. Modifiez le fichier operations.json :

    * Changez le lien vers pipelineTopology<br/>`"pipelineTopologyUrl" : "https://raw.githubusercontent.com/Azure/video-analyzer/main/pipelines/live/topologies/motion-with-httpExtension/topology.json"`
    * Sous livePipelineSet, modifiez le nom de la pipelineTopology pour le faire correspondre à la valeur du lien précédent :<br/>`"pipelineTopologyName" : "EVROnMotionPlusHttpExtension"`
    * Sous PipelineTopologyDelete, modifiez le nom :<br/>`"name": "EVROnMotionPlusHttpExtension"`
    
