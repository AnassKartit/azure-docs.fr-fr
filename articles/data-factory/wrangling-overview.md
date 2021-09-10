---
title: Data wrangling dans Azure Data Factory
description: Vue d’ensemble du data wrangling dans Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.subservice: data-flows
ms.topic: conceptual
ms.date: 07/29/2021
ms.openlocfilehash: 133496614db862d4c1af31afb015a535ddbfd188
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122531452"
---
# <a name="what-is-data-wrangling"></a>Qu’est-ce que le data wrangling ?

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Les organisations doivent avoir la possibilité d’explorer leurs données métier critiques en vue de procéder à la préparation et au wrangling des données pour une analyse précise des données complexes qui continuent à croître quotidiennement. La préparation des données est nécessaire pour que les organisations puissent utiliser les données dans différents processus d’entreprise et accélérer la rentabilité.

Data Factory vous permet d’effectuer une préparation de données sans code à l’échelle du cloud de manière itérative à l’aide de Power Query. Data Factory s’intègre à [Power Query Online](/power-query/) et rend les fonctions Power Query M disponibles en tant qu’activité de pipeline.

Data Factory convertit le M généré par l’éditeur mashup Power Query Online en code Spark pour l’exécution à l’échelle du cloud en convertissant M en flux de données Azure Data Factory. Le data wrangling avec Power Query et les flux de données sont particulièrement utiles pour les ingénieurs de données ou les « intégrateurs de données citoyen ».

> [!NOTE]
> L’activité Power Query dans Azure Data Factory est actuellement disponible en version préliminaire publique

## <a name="use-cases"></a>Cas d'utilisation

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4MFkW]

### <a name="fast-interactive-data-exploration-and-preparation"></a>Exploration et préparation rapide des données interactives

Plusieurs ingénieurs de données et intégrateurs de données citoyen peuvent explorer et préparer de manière interactive les jeux de données à l’échelle du cloud. Avec l’augmentation du volume, de la variété et de la vélocité des données dans les lacs de données, les utilisateurs ont besoin d’un moyen efficace pour explorer et préparer les jeux de données. Par exemple, vous pourriez avoir à créer un jeu de données qui contient toutes les informations démographiques des nouveaux clients depuis 2017. Vous n’effectuez pas de mappage vers une cible connue. Vous explorez, effectuez le wrangling et préparez les jeux de données pour répondre à une exigence avant de les publier dans le lac. Le wrangling est souvent utilisé dans des scénarios analytiques moins formels. Les jeux de données préparés peuvent être utilisés pour exécuter des transformations et des opérations d’apprentissage automatique en aval.

### <a name="code-free-agile-data-preparation"></a>Préparation agile des données sans code

Les intégrateurs de données citoyen consacrent plus de 60 % de leur temps à la recherche et à la préparation des données. Ils cherchent à le faire sans code pour améliorer la productivité opérationnelle. Permettre aux intégrateurs de données citoyen d’enrichir, de mettre en forme et de publier des données à l’aide d’outils connus comme Power Query Online de façon évolutive améliore considérablement leur productivité. Le wrangling dans Azure Data Factory permet à l’éditeur mashup Power Query Online de donner les moyens aux intégrateurs de données citoyen de corriger les erreurs rapidement, de normaliser les données et de produire des données de haute qualité pour appuyer les décisions commerciales.

### <a name="data-validation-and-exploration"></a>Validation et exploration des données

Analysez visuellement vos données sans code pour supprimer les valeurs hors norme et les anomalies, et les rendre aptes à une analyse rapide.

## <a name="supported-sources"></a>Sources prises en charge

| Connecteur | Format de données | Type d'authentification |
| -- | -- | --|
| [Stockage Blob Azure](connector-azure-blob-storage.md) | CSV, Parquet | Clé du compte |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | CSV | Principal de service |
| [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md) | CSV, Parquet | Clé de compte, Principal de service |
| [Azure SQL Database](connector-azure-sql-database.md) | - | Authentification SQL |
| [Azure Synapse Analytics](connector-azure-sql-data-warehouse.md) | - | Authentification SQL |

## <a name="the-mashup-editor"></a>Editeur de mashup

Quand vous créez une activité Power Query, tous les jeux de données sources deviennent des requêtes de jeu de données et sont placés dans le dossier **ADFResource**. Par défaut, la requête utilisateur pointe vers la première requête de jeu de données. Toutes les transformations doivent être effectuées sur la requête utilisateur, car les modifications apportées aux requêtes de jeu de données ne sont pas prises en charge et ne sont pas rendues persistantes. Le renommage, l’ajout et la suppression de requêtes ne sont pas pris en charge.

![Wrangling](media/wrangling-data-flow/editor.png)

Actuellement, toutes les fonctions Power Query M ne sont pas prises en charge pour le rassemblement de données brutes à l’analyse, bien qu’elles soient disponibles pendant la création. Lors de la génération de vos activités Power Query, le message d’erreur suivant s’affiche si une fonction n’est pas prise en charge :

`The Power Query Spark Runtime does not support the function`

Pour plus d’informations sur les transformations prises en charge, consultez [Fonctions de data wrangling Power Query](wrangling-functions.md).

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment [créer une composition (mash-up) de data wrangling Power Query](wrangling-tutorial.md).
