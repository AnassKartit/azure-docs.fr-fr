---
title: Genome Aggregation Database (gnomAD)
titleSuffix: Azure Open Datasets
description: Découvrez comment utiliser le jeu de données Genome Aggregation Database (gnomAD) dans Azure Open Datasets.
ms.service: open-datasets
ms.topic: sample
author: peterclu
ms.author: peterlu
ms.date: 04/16/2021
ms.openlocfilehash: 779f39afa7ac9b9890013db546223274cdf37ad6
ms.sourcegitcommit: 7854045df93e28949e79765a638ec86f83d28ebc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2021
ms.locfileid: "122867662"
---
# <a name="genome-aggregation-database-gnomad"></a>Genome Aggregation Database (gnomAD)

[Genome Aggregation Database (gnomAD)](https://gnomad.broadinstitute.org/) est une ressource développée par une coalition internationale de chercheurs, dans le but de regrouper et d’harmoniser les données de séquençage tant des exomes que des génomes provenant d’un large éventail de projets de séquençage à grande échelle, et de mettre des données de synthèse à la disposition de la communauté scientifique au sens large.

[!INCLUDE [Open Dataset usage notice](../../includes/open-datasets-usage-note.md)]

## <a name="data-source"></a>Source de données

Ce jeu de données est hébergé en collaboration avec le Broad Institute, et le catalogue complet des données gnomAD peut être consulté à l’adresse https://gnomad.broadinstitute.org/downloads

## <a name="data-volumes-and-update-frequency"></a>Volumes de données et fréquence de mise à jour

Ce jeu de données contient environ 30 To de données et est mis à jour à chaque publication de gnomAD.

## <a name="storage-location"></a>Emplacement de stockage

Le compte de stockage qui héberge ce jeu de données se trouve dans la région Azure USA Est. L’allocation de ressources de calcul dans la région USA Est est recommandée à des fins d’affinité.

## <a name="data-access"></a>Accès aux données

Compte de stockage : « https://datasetgnomad.blob.core.windows.net/dataset/  »

Les données sont publiquement accessibles sans restrictions. L’outil AzCopy est recommandé pour les opérations en bloc. Par exemple, pour afficher les fichiers VCF dans la version 3.0 de gnomAD :

```powershell
$ azcopy ls https://datasetgnomad.blob.core.windows.net/dataset/release/3.0/vcf/genomes
```

Pour télécharger tous les fichiers VCF de manière récursive :

```powershell
$ azcopy cp --recursive=true https://datasetgnomad.blob.core.windows.net/dataset/release/3.0/vcf/genomes .
```

**NOUVEAU : format Parquet des fichiers VCF de gnomAD v2.1.1 (exomes et génomes)**

Pour afficher les fichiers Parquet :

```powershell
$ azcopy ls https://datasetgnomadparquet.blob.core.windows.net/dataset
```

Pour télécharger l’ensemble des fichiers de manière récursive :

```powershell
$ cp --recursive=true https://datasetgnomadparquet.blob.core.windows.net/dataset
```

L’[Explorateur Stockage Azure](https://azure.microsoft.com/features/storage-explorer/) est également un outil utile pour parcourir la liste des fichiers dans la version gnomAD.

## <a name="use-terms"></a>Conditions d’utilisation

Les données sont disponibles sans restrictions. Pour plus d’informations, consultez la [page « À propos » de gnomAD](https://gnomad.broadinstitute.org/about).

## <a name="contact"></a>Contact

Si vous avez des questions ou des commentaires sur ce jeu de données, contactez l’[équipe gnomAD](https://gnomad.broadinstitute.org/contact).

## <a name="next-steps"></a>Étapes suivantes

Consultez les autres jeux de données du [catalogue Open Datasets](dataset-catalog.md).
