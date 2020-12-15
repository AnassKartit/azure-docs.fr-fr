---
title: Fichier include
description: Fichier include
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 11/02/2020
ms.openlocfilehash: 1f7abcdd1439fe5e6eeb2f718862f4875c61230c
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96509150"
---
La cible de calcul que vous utilisez pour héberger votre modèle aura une incidence sur le coût et la disponibilité de votre point de terminaison déployé. Utilisez ce tableau pour choisir une cible de calcul appropriée.

| Cible de calcul | Utilisé pour | Prise en charge GPU | Prise en charge FPGA | Description |
| ----- | ----- | ----- | ----- | ----- |
| [Service&nbsp;web&nbsp;local](../articles/machine-learning/how-to-deploy-local-container-notebook-vm.md) | Test/débogage | &nbsp; | &nbsp; | Pour les tests et la résolution des problèmes limités. L’accélération matérielle dépend de l’utilisation de bibliothèques dans le système local.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/how-to-deploy-azure-kubernetes-service.md) | Inférence en temps réel |  [Oui](../articles/machine-learning/how-to-deploy-inferencing-gpus.md) (déploiement de services web) | [Oui](../articles/machine-learning/how-to-deploy-fpga-web-service.md)   |Pour les déploiements de production à grande échelle. Fournit un temps de réponse et une mise à l’échelle automatique rapides du service déployé. La mise à l’échelle automatique du cluster n’est pas prise en charge via le Kit de développement logiciel (SDK) Azure Machine Learning. Pour modifier les nœuds du cluster AKS, utilisez l’interface utilisateur de votre cluster AKS dans le portail Azure. <br/><br/> Pris en charge dans le concepteur. |
| [Azure Container Instances](../articles/machine-learning/how-to-deploy-azure-container-instance.md) | Test ou développement | &nbsp;  | &nbsp; | Pour les charges de travail à faible échelle basées sur le processeur qui nécessitent moins de 48 Go de RAM. <br/><br/> Pris en charge dans le concepteur. |
| [Clusters de calcul Azure Machine Learning](../articles/machine-learning/tutorial-pipeline-batch-scoring-classification.md) | Inférence&nbsp;Batch | [Oui](../articles/machine-learning/tutorial-pipeline-batch-scoring-classification.md) (pipeline d’apprentissage automatique) | &nbsp;  | Exécutez le scoring par lots sur un calcul sans serveur. Prend en charge des machines virtuelles normales et basse priorité. |

> [!NOTE]
> Même si des cibles de calcul telles que des cibles locales ainsi que des instances et clusters de calcul Azure Machine Learning prennent en charge de GPU pour la formation et l’expérimentation, l’utilisation d’un GPU pour l’inférence _lors d’un déploiement en tant que service web_ est prise en charge uniquement sur AKS.
>
> L’utilisation d’un GPU pour l’inférence _lors du scoring avec un pipeline d’apprentissage automatique_ est prise en charge uniquement sur un calcul Azure Machine Learning.

> [!NOTE]
> * Des instances de conteneur conviennent uniquement pour des modèles d’une taille inférieure à 1 Go.
> * Pour le développement et le test de modèles plus volumineux, utilisez des clusters AKS à nœud unique.