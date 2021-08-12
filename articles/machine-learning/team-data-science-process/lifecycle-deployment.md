---
title: Phase de déploiement du cycle de vie du processus TDSP (Team Data Science Process)
description: Objectifs, tâches et livrables associés à la phase de déploiement de vos projets de science des données
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: dd28d1a111c6655588b2e1254e2e503ad3f3ad28
ms.sourcegitcommit: a434cfeee5f4ed01d6df897d01e569e213ad1e6f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/09/2021
ms.locfileid: "111813535"
---
# <a name="deployment-stage-of-the-team-data-science-process-lifecycle"></a>Phase de déploiement du cycle de vie du processus TDSP (Team Data Science Process)

Cet article présente les objectifs, tâches et livrables associés au déploiement du processus TDSP. Ce processus indique un cycle de vie recommandé que vous pouvez utiliser pour structurer vos projets de science des données. Le cycle de vie expose les principales phases que les projets exécutent généralement, souvent de manière itérative :

   1. **Présentation de l’entreprise**
   2. **Acquisition et compréhension des données**
   3. **Modélisation**
   4. **Déploiement** (cet article)
   5. **Acceptation du client**

Pour une vue d’ensemble et une représentation visuelle du cycle de vie TDSP, consultez [Cycle de vie du processus Team Data Science Process](./lifecycle.md).

## <a name="goal"></a>Objectif
Déployer des modèles comportant un pipeline de données dans un environnement de production ou similaire en vue de leur acceptation par l’utilisateur final. 

## <a name="how-to-do-it"></a>Marche à suivre
Une tâche principale est traitée dans cette phase :

**Faire fonctionner le modèle** : déployez le modèle et le pipeline sur un environnement de production ou similaire en vue de l’utilisation de l’application.

### <a name="operationalize-a-model"></a>Faire fonctionner un modèle
Une fois que votre ensemble de modèles fonctionne correctement, vous pouvez l’utiliser pour les autres applications. En fonction des exigences de l’entreprise, les prédictions sont faites en temps réel ou par lot. Pour déployer des modèles, vous les exposez avec une interface API ouverte. Cette interface permet au modèle d’être facilement utilisé dans différentes applications :

   * Sites web en ligne
   * Tableurs 
   * Tableaux de bord
   * applications métier ; 
   * Applications principales 

Pour obtenir des exemples de mise en œuvre de modèle avec un service web Azure Machine Learning, consultez [Déploiement d’un service web Azure Machine Learning](../classic/deploy-a-machine-learning-web-service.md). Il est recommandé de générer des données de télémétrie et de surveillance dans le modèle de production et le pipeline de données que vous déployez. Cette pratique facilite la création de rapports sur l’état subséquent du système et le dépannage de ce dernier.  

## <a name="artifacts"></a>Artefacts

* Tableau de bord d’état affichant les mesures clés et l’intégrité du système
* Rapport de modélisation final contenant les détails du déploiement
* Document final de l’architecture de la solution


## <a name="next-steps"></a>Étapes suivantes

Voici les liens vers chaque étape du cycle de vie TDSP :

   1. [Présentation de l’entreprise](lifecycle-business-understanding.md)
   2. [Acquisition et compréhension des données](lifecycle-data.md)
   3. [Modélisation](lifecycle-modeling.md)
   4. [Déploiement](lifecycle-deployment.md)
   5. [Acceptation du client](lifecycle-acceptance.md)

Nous fournissons des procédures pas à pas complètes qui illustrent toutes les étapes du processus correspondant à des scénarios spécifiques. L’article [Example walkthroughs](walkthroughs.md) (Exemples de procédures pas à pas) contient une liste des scénarios ainsi que des liens et des descriptions de miniatures. Les procédures pas à pas montrent comment combiner les outils et services dans le cloud et sur site dans un flux de travail ou un pipeline pour créer une application intelligente. 

Pour obtenir des exemples sur l’exécution de procédures dans les processus TDSP utilisant Azure Machine Learning Studio, consultez [Utilisation du processus de science des données avec Azure Machine Learning](./index.yml).