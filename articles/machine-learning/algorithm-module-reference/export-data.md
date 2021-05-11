---
title: 'Exporter des données : Informations de référence sur les modules'
titleSuffix: Azure Machine Learning
description: Utilisez le module Exporter des données dans le concepteur Azure Machine Learning pour enregistrer les résultats et les données intermédiaires en dehors d’Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/19/2021
ms.openlocfilehash: 82821b29669139f378d4dd24e4a96ab66f3d56e1
ms.sourcegitcommit: 52491b361b1cd51c4785c91e6f4acb2f3c76f0d5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2021
ms.locfileid: "108321930"
---
# <a name="export-data-module"></a>Module Exporter les données

Cet article décrit un module dans le concepteur Azure Machine Learning.

Utilisez ce module pour enregistrer les résultats, les données intermédiaires et les données de travail de vos pipelines dans les destinations de stockage cloud. 

Ce module prend en charge l’exportation de vos données dans les services de données cloud suivants :

- Conteneur d’objets blob Azure
- Partage de fichiers Azure
- Azure Data Lake Storage Gen1
- Azure Data Lake Storage Gen2
- Base de données Azure SQL

Avant d’exporter vos données, vous devez d’abord inscrire un magasin de données dans votre espace de travail Azure Machine Learning. Pour plus d'informations, consultez la page [Accéder aux données dans les services de stockage Azure](../how-to-access-data.md).

## <a name="how-to-configure-export-data"></a>Configuration de l’exportation des données

1. Ajoutez le module **Exporter des données** à votre pipeline dans le concepteur. Ce module figure dans la catégorie **Entrée et sortie**.

1. Connectez **Exporter des données** au module qui contient les données à exporter.

1. Sélectionnez **Exporter les données** pour ouvrir le volet **Propriétés**.

1. Pour **Magasin de données**, sélectionnez un magasin de données existant dans la liste déroulante. Vous pouvez également créer un magasin de données. Découvrez comment en consultant [Accéder aux données dans les services de stockage Azure](../how-to-access-data.md).

    > [!NOTE]
    > Il n’est pas possible d’exporter des données d’un certain type vers une colonne de base de données SQL spécifiée d’un autre type. La table cible n’a pas besoin d’exister en premier.

1. La case à cocher **Régénérer la sortie** détermine s’il faut exécuter le module pour régénérer la sortie au moment de l’exécution. 

    Elle est désélectionnée par défaut, ce qui signifie que, si le module a été exécuté avec les mêmes paramètres par le passé, le système réutilisera la sortie de la dernière exécution pour réduire le temps d’exécution. 

    Si elle est sélectionnée, le système exécutera à nouveau le module pour régénérer la sortie.

1. Définissez le chemin d’accès du magasin de données où se trouvent les données. Le chemin d’accès est un chemin d’accès relatif. Prenons `data/testoutput` comme exemple, ce qui signifie que les données d’entrée d’**Exporter les données** seront exportées vers `data/testoutput` dans le magasin de données que vous avez défini dans les **Paramètres de sortie** du module.

    > [!NOTE]
    > Les chemins d’accès vides ou les **chemins d’accès d’URL** ne sont pas autorisés.


1. Pour **Format de fichier**, sélectionnez le format dans lequel les données doivent être stockées.
 
1. Envoyez le pipeline.

## <a name="limitations"></a>Limites

En raison de la limitation de l’accès au magasin de données, si votre pipeline d’inférence contient un module **Exporter des données**, il est automatiquement supprimé lors du déploiement sur le point de terminaison en temps réel.

## <a name="next-steps"></a>Étapes suivantes

Consultez [l’ensemble des modules disponibles](module-reference.md) pour Azure Machine Learning. 
