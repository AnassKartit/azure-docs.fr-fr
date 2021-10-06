---
title: Transformer des données avec Databricks Python
titleSuffix: Azure Data Factory & Azure Synapse
description: Découvrez comment traiter ou transformer des données en exécutant une activité Databricks Python dans un pipeline Azure Data Factory ou Synapse Analytics.
ms.service: data-factory
ms.subservice: tutorials
ms.topic: conceptual
ms.date: 09/09/2021
author: nabhishek
ms.author: abnarain
ms.custom: devx-track-python, synapse
ms.openlocfilehash: b178a54d71d7f004f66e15f455c0b618842d765e
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124806352"
---
# <a name="transform-data-by-running-a-python-activity-in-azure-databricks"></a>Transformer des données en exécutant une activité Python dans Azure Databricks
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

L'activité Azure Databricks Python dans un [pipeline](concepts-pipelines-activities.md) exécute un fichier Python dans votre cluster Azure Databricks. Cet article s'appuie sur l'article [Activités de transformation des données](transform-data.md) qui présente une vue d'ensemble de la transformation des données et les activités de transformation prises en charge.  Azure Databricks est une plateforme gérée pour exécuter Apache Spark.

Pour une présentation de onze minutes et la démonstration de cette fonctionnalité, regardez la vidéo suivante :

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Execute-Jars-and-Python-scripts-on-Azure-Databricks-using-Data-Factory/player]

## <a name="databricks-python-activity-definition"></a>Définition de l’activité Databricks Python

Voici l’exemple de définition JSON d’une activité Databricks Python :

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksSparkPython",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "pythonFile": "dbfs:/docs/pi.py",
            "parameters": [
                "10"
            ],
            "libraries": [
                {
                    "pypi": {
                        "package": "tensorflow"
                    }
                }
            ]
        }
    }
}
```

## <a name="databricks-python-activity-properties"></a>Propriétés de l’activité Databricks Python

Le tableau suivant décrit les propriétés JSON utilisées dans la définition JSON :

|Propriété|Description|Obligatoire|
|---|---|---|
|name|Nom de l'activité dans le pipeline.|Oui|
|description|Texte décrivant l’activité.|Non|
|type|Pour l’activité Databricks Python, le type d’activité est DatabricksSparkPython.|Oui|
|linkedServiceName|Nom du service lié Databricks sur lequel s’exécute l’activité Python. Pour en savoir plus sur ce service lié, consultez l’article [Services liés de calcul](compute-linked-services.md).|Oui|
|pythonFile|L’URI du fichier Python à exécuter. Seuls les chemins DBFS sont pris en charge.|Oui|
|parameters|Les paramètres de ligne de commande qui sont passés au fichier Python. C’est un tableau de chaînes.|Non|
|libraries|Liste de bibliothèques à installer sur le cluster qui exécute le travail. Il peut s’agir d’un tableau de < chaîne, objet >|Non|

## <a name="supported-libraries-for-databricks-activities"></a>Bibliothèques prises en charge pour les activités Databricks

Dans la définition d’activité Databricks ci-dessus, vous précisez ces types de bibliothèques : *jar*, *egg*, *maven*, *pypi*,  *cran*.

```json
{
    "libraries": [
        {
            "jar": "dbfs:/mnt/libraries/library.jar"
        },
        {
            "egg": "dbfs:/mnt/libraries/library.egg"
        },
        {
            "maven": {
                "coordinates": "org.jsoup:jsoup:1.7.2",
                "exclusions": [ "slf4j:slf4j" ]
            }
        },
        {
            "pypi": {
                "package": "simplejson",
                "repo": "http://my-pypi-mirror.com"
            }
        },
        {
            "cran": {
                "package": "ada",
                "repo": "https://cran.us.r-project.org"
            }
        }
    ]
}

```

Pour plus d’informations, consultez la [documentation Databricks](/azure/databricks/dev-tools/api/latest/libraries#managedlibrarieslibrary) pour les types de bibliothèques.

## <a name="how-to-upload-a-library-in-databricks"></a>Comment charger une bibliothèque dans Databricks

### <a name="you-can-use-the-workspace-ui"></a>Vous pouvez utiliser l’interface utilisateur de l’espace de travail :

1. [Utiliser l’interface utilisateur de l’espace de travail Databricks](/azure/databricks/libraries/#create-a-library)

2. Pour obtenir le chemin dbfs de la bibliothèque ajoutée par le biais de l’interface utilisateur, vous pouvez utiliser l’[interface CLI de Databricks](/azure/databricks/dev-tools/cli/#install-the-cli).

   En général, les bibliothèques Jar sont stockées sous dbfs:/FileStore/jars lors de l’utilisation de l’interface utilisateur. Vous pouvez répertorier toutes les répertorier à l’aide de l’interface CLI : *databricks fs ls dbfs:/FileStore/job-jars*

### <a name="or-you-can-use-the-databricks-cli"></a>Vous pouvez utiliser l’interface CLI de Databricks :

1. Suivez [Copier la bibliothèque avec l’interface CLI de Databricks](/azure/databricks/dev-tools/cli/#copy-a-file-to-dbfs).

2. Utilisez l’interface CLI de Databricks [(étapes d’installation)](/azure/databricks/dev-tools/cli/#install-the-cli).

   Par exemple, pour copier un fichier JAR sur dbfs : `dbfs cp SparkPi-assembly-0.1.jar dbfs:/docs/sparkpi.jar`
