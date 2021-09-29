---
title: Vue d’ensemble des connecteurs
titleSuffix: Azure Data Factory & Azure Synapse
description: Découvrez les connecteurs pris en charge dans les pipelines Azure Data Factory et Azure Synapse Analytics.
author: jianleishen
ms.service: data-factory
ms.subservice: data-movement
ms.custom: synapse
ms.topic: conceptual
ms.date: 09/09/2021
ms.author: jianleishen
ms.openlocfilehash: 9233c17070d5a4f60311501e0f42072b1e4ea125
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124767676"
---
# <a name="azure-data-factory-and-azure-synapse-analytics-connector-overview"></a>Vue d’ensemble du connecteur Azure Synapse Analytics et Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Les pipelines Azure Data Factory et Azure Synapse Analytics prennent en charge les magasins de données et les formats suivants par le biais de l’activité de copie, le flux de données, l’activité de recherche, l’activité d’extraction des métadonnées et l’activité de suppression. Cliquez sur chaque banque de données pour découvrir les fonctionnalités prises en charge et les configurations correspondantes en détail.

## <a name="supported-data-stores"></a>Magasins de données pris en charge

[!INCLUDE [Connector overview](includes/data-factory-v2-connector-overview.md)]

## <a name="integrate-with-more-data-stores"></a>Intégrer avec d’autres magasins de données

Les pipelines Azure Data Factory et Synapse peuvent atteindre un ensemble plus large de magasins de données que la liste mentionnée ci-dessus. Si vous devez déplacer des données vers/depuis un magasin de données qui ne figure pas dans la liste des connecteurs intégrés au service, voici quelques options extensibles :
- Pour les bases de données et les entrepôts de données, vous pouvez généralement trouver un pilote ODBC correspondant, avec lequel vous pouvez utiliser un [connecteur ODBC générique](connector-odbc.md).
- Pour les applications SaaS :
    - Si elle fournit des API RESTful, vous pouvez utiliser un [connecteur REST générique](connector-rest.md).
    - Si elle contient un flux OData, vous pouvez utiliser un [connecteur OData générique](connector-odata.md).
    - Si elle fournit des API SOAP, vous pouvez utiliser un [connecteur HTTP générique](connector-http.md).
    - Si elle utilise le pilote ODBC, vous pouvez utiliser un [connecteur ODBC générique](connector-odbc.md).
- Pour les autres, vérifiez si vous pouvez charger des données ou exposer des données comme n’importe quel magasin de données pris en charge, par exemple Azure blob/File/FTP/SFTP/etc., puis laissez le service se charger du reste. Vous pouvez appeler un mécanisme personnalisé de chargement des données via [une fonction Azure](control-flow-azure-function-activity.md), [une activité personnalisée](transform-data-using-dotnet-custom-activity.md), [Databricks](transform-data-databricks-notebook.md)/[HDInsight](transform-data-using-hadoop-hive.md), [une activité web](control-flow-web-activity.md), etc.

## <a name="supported-file-formats"></a>Formats de fichiers pris en charge

Les formats de fichier suivants sont pris en charge. Reportez-vous à chaque article pour les paramètres basés sur le format.

- [Format Avro](format-avro.md)
- [Format binaire](format-binary.md)
- [Format Common Data Model](format-common-data-model.md)
- [Format de texte délimité](format-delimited-text.md)
- [Format Delta](format-delta.md)
- [Format Excel](format-excel.md)
- [Format JSON](format-json.md)
- [Format ORC](format-orc.md)
- [Format Parquet](format-parquet.md)
- [Format XML](format-xml.md)

## <a name="next-steps"></a>Étapes suivantes

- [Activité de copie](copy-activity-overview.md)
- [Mappage de flux de données](concepts-data-flow-overview.md)
- [Activité de recherche](control-flow-lookup-activity.md)
- [Activité d’obtention des métadonnées](control-flow-get-metadata-activity.md)
- [Supprimer l’activité](delete-activity.md)
