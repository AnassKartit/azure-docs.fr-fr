---
title: Code d’ingestion de données
titleSuffix: Azure Data Science Virtual Machine
description: En savoir plus sur les outils et utilitaires d’ingestion des données préinstallés sur la Data Science Virtual Machine.
keywords: outils de science des données, machine virtuelle science des données, outils pour la science des données, science des données linux
services: machine-learning
ms.service: data-science-vm
author: timoklimmer
ms.author: tklimmer
ms.topic: conceptual
ms.date: 05/12/2021
ms.openlocfilehash: 77968c7a611cdfade3d2f1eb2cb6e9b0a3768728
ms.sourcegitcommit: 17345cc21e7b14e3e31cbf920f191875bf3c5914
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2021
ms.locfileid: "110070945"
---
# <a name="data-science-virtual-machine-data-ingestion-tools"></a>Outils d’ingestion de données de la machine virtuelle DSVM

En tant l’une des premières étapes techniques dans un projet de science de données ou d’intelligence artificielle, vous devez identifier les jeux de données à utiliser, puis les importer dans votre environnement analytique. La machine virtuelle DSVM (Data Science Virtual Machine) fournit des outils et des bibliothèques pour importer des données à partir de différentes sources dans un stockage de données analytiques sur la machine DSVM ou dans une plateforme de données sur le cloud ou localement.

Voici quelques outils de déplacement de données qui sont disponibles sur la machine virtuelle DSVM.

## <a name="azure-cli"></a>Azure CLI

| Category | Valeur |
|--|--|
| Qu’est-ce que c’est ? | Outil de gestion pour Azure. Il contient également des verbes de commande pour déplacer des données à partir de plateformes de données Azure, telles que le stockage Blob Azure et Azure Data Lake Storage. |
| Versions DSVM prises en charge | Windows, Linux |
| Utilisations classiques | Importation et exportation de données vers et depuis le Stockage Azure et Azure Data Lake Store |
| Comment l’utiliser/l’exécuter ? | Ouvrez une invite de commandes et tapez `az` pour obtenir de l’aide. |
| Liens vers des exemples | [Utilisation de l’interface de ligne de commande Azure](/cli/azure) |


## <a name="azcopy"></a>AzCopy

| Category | Valeur |
|--|--|
| Qu’est-ce que c’est ? | Outil permettant de copier des données vers et depuis des fichiers locaux, du stockage, des fichiers et des tables Blob Azure. |
| Versions DSVM prises en charge | Windows |
| Utilisations classiques | Copie de fichiers vers le Stockage Blob Azure et copie d’objets blob entre plusieurs comptes. |
| Comment l’utiliser/l’exécuter ? | Ouvrez une invite de commandes et tapez `azcopy` pour obtenir de l’aide. |
| Liens vers des exemples | [AzCopy sur Windows](../../storage/common/storage-use-azcopy-v10.md) |


## <a name="azure-cosmos-db-data-migration-tool"></a>Outil de migration de données Azure Cosmos DB

|--|--|
| ------------- | ------------- |
| Qu’est-ce que c’est ? | Outil permettant d’importer des données à partir de différentes sources dans Azure Cosmos DB, une base de données NoSQL dans le cloud. Sources incluses : fichiers JSON, fichiers CSV, SQL, MongoDB, Stockage de table Azure, Amazon DynamoDB et collections d’API SQL Azure Cosmos DB. |
| Versions DSVM prises en charge | Windows |
| Utilisations classiques | Importation de fichiers à partir d’une machine virtuelle vers CosmosDB, importation de données à partir du Stockage de table Azure vers CosmosDB et importation de données à partir d’une base de données Microsoft SQL Server vers CosmosDB. |
| Comment l’utiliser/l’exécuter ? | Pour utiliser la version en ligne de commande, ouvrez une invite de commandes et tapez `dt`. Pour utiliser l’outil GUI, ouvrez une invite de commandes et tapez `dtui`. |
| Liens vers des exemples | [Importation de données dans CosmosDB](../../cosmos-db/import-data.md) |

## <a name="azure-storage-explorer"></a>Explorateur de stockage Azure

| Category | Valeur |
|--|--|
| Qu’est-ce que c’est ? | Interface graphique utilisateur pour l’interaction avec les fichiers stockés dans le cloud Azure. |
| Versions DSVM prises en charge | Windows |
| Utilisations classiques | Importation et exportation de données à partir de DSVM. |
| Comment l’utiliser/l’exécuter ? | Recherchez « Explorateur Stockage Azure » dans le menu Démarrer. |
| Liens vers des exemples | [Azure Storage Explorer](vm-do-ten-things.md#access-azure-data-and-analytics-services) |

## <a name="bcp"></a>bcp

| Category | Valeur |
|--|--|
| Qu’est-ce que c’est ? | Outil SQL Server permettant de copier des données entre SQL Server et un fichier de données. |
| Versions DSVM prises en charge | Windows |
| Utilisations classiques | Importation d’un fichier CSV dans une table SQL Server et exportation d’une table SQL Server dans un fichier. |
| Comment l’utiliser/l’exécuter ? | Ouvrez une invite de commandes et tapez `bcp` pour obtenir de l’aide. |
| Liens vers des exemples | [Utilitaire bcp](/sql/tools/bcp-utility) |

## <a name="blobfuse"></a>blobfuse

| Category | Valeur |
|--|--|
| Qu’est-ce que c’est ? | Outil permettant de monter un conteneur de stockage blob Azure dans le système de fichiers Linux. |
| Versions DSVM prises en charge | Linux |
| Utilisations classiques | Lecture et écriture dans des objets blob dans un conteneur. |
| Comment l’utiliser et l’exécuter ? | Exécutez _blobfuse_ sur un terminal. |
| Liens vers des exemples | [blobfuse sur GitHub](https://github.com/Azure/azure-storage-fuse) |
