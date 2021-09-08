---
title: Concepts concernant les pools Apache Spark
description: Présentation des tailles et configurations des pools Apache Spark dans Azure Synapse Analytics.
services: synapse-analytics
author: mlee3gsd
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: spark
ms.date: 08/19/2021
ms.author: martinle
ms.reviewer: euang
ms.openlocfilehash: c674813aee7702e9887e909e4a8baf7ace074a2c
ms.sourcegitcommit: 5d605bb65ad2933e03b605e794cbf7cb3d1145f6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2021
ms.locfileid: "122597644"
---
# <a name="apache-spark-pool-configurations-in-azure-synapse-analytics"></a>Configurations des pools Apache Spark dans Azure Synapse Analytics

Un pool Spark est un ensemble de métadonnées qui définit les besoins en ressources de calcul et les caractéristiques de comportement associées lorsqu’une instance Spark est instanciée. Ces caractéristiques incluent entre autres le nom, le nombre de nœuds, la taille de nœud, le comportement de mise à l’échelle et la durée de vie. Un pool Spark en lui-même ne consomme pas de ressources. La création de pools Spark est gratuite. Les frais ne sont facturés qu’une fois qu’un travail Spark est exécuté sur le pool Spark cible et que l’instance Spark est instanciée à la demande.

Pour découvrir comment créer un pool Spark et afficher toutes ses propriétés, consultez [Bien démarrer avec les pools Spark dans Synapse Analytics](../quickstart-create-apache-spark-pool-portal.md).

## <a name="isolated-compute"></a>Calcul isolé

L’option de calcul isolé offre une sécurité supplémentaire aux ressources de calcul Spark à partir de services non fiables en dédiant la ressource de calcul physique à un client unique.
L’option de calcul isolé est la plus adaptée aux charges de travail qui nécessitent un niveau élevé d’isolement par rapport aux charges de travail des autres clients pour des raisons de conformité et d’exigences réglementaires, entre autres.  
L’option de calcul isolé est disponible uniquement avec la taille de nœud XXXLarge (80 vCPU/504 Go) et n’est disponible que dans les régions suivantes.  L’option de calcul isolé peut être activée ou désactivée après la création du pool, bien que l’instance doive peut-être être redémarrée.  Si vous envisagez d’activer cette fonctionnalité à l’avenir, assurez-vous que votre espace de travail Synapse est créé dans une région prise en charge par le calcul isolé.

* USA Est
* USA Ouest 2
* États-Unis - partie centrale méridionale
* Gouvernement des États-Unis – Arizona
* Gouvernement américain - Virginie

## <a name="nodes"></a>Nœuds

Une instance de pool Apache Spark se compose d’un nœud principal et de deux nœuds Worker ou plus avec un minimum de trois nœuds dans une instance Spark.  Le nœud principal exécute des services de gestion supplémentaires tels que Livy, Yarn Resource Manager, Zookeeper et le pilote Spark.  Tous les nœuds exécutent des services tels que l’agent de nœud et Yarn Node Manager. Tous les nœuds Worker exécutent le service Spark Executor.

## <a name="node-sizes"></a>Tailles de nœuds

Un pool Spark peut être défini avec des tailles de nœuds allant d’un petit nœud de calcul avec 8 vCore et 64 Go de mémoire jusqu’à un nœud de calcul XXLarge avec 64 vCore et 432 Go de mémoire par nœud. Les tailles de nœud peuvent être modifiées après la création du pool bien que l’instance doive être redémarrée.

|Taille | vCore | Mémoire|
|-----|------|-------|
|Small|4|32 Go|
|Moyenne|8|64 Go|
|Large|16|128 Go|
|XLarge|32|256 Go|
|XXLarge|64|432 Go|
|XXX grande (calcul isolé)|80|504 Go|

## <a name="autoscale"></a>Mise à l’échelle automatique

Les pools Apache Spark offrent la possibilité de mettre à l’échelle automatiquement les ressources de calcul en fonction de la quantité d’activité.  Lorsque la fonctionnalité de mise à l’échelle automatique est activée, vous pouvez définir le nombre minimal et maximal de nœuds à mettre à l’échelle.
Lorsque la fonctionnalité de mise à l’échelle automatique est désactivée, le nombre de nœuds définis reste fixe.  Ce paramètre peut être modifié après la création du pool bien que l’instance doive être redémarrée.

## <a name="automatic-pause"></a>Pause automatique

La fonctionnalité de pause automatique libère les ressources après une période d’inactivité définie, réduisant ainsi le coût global d’un pool Apache Spark.  Le nombre de minutes d’inactivité peut être défini une fois que cette fonctionnalité est activée.  La fonctionnalité de pause automatique est indépendante de la fonctionnalité de mise à l’échelle automatique. Les ressources peuvent être suspendues si la mise à l’échelle automatique est activée ou désactivée.  Ce paramètre peut être modifié après la création du pool bien que l’instance doive être redémarrée.

## <a name="next-steps"></a>Étapes suivantes

* [Azure Synapse Analytics](../index.yml)
* [Documentation Apache Spark](https://spark.apache.org/docs/2.4.5/)