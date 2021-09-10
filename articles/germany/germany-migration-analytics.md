---
title: Migrer des ressources d’analytique Azure d’Azure Allemagne vers Azure global
description: Cet article fournit des informations sur la migration de vos ressources d’analytique Azure depuis Azure Germany vers Azure global.
ms.topic: article
ms.date: 10/16/2020
author: gitralf
ms.author: ralfwi
ms.service: germany
ms.custom: bfmigrate
ms.openlocfilehash: 93630f536127971e922ddceaf5d1e5fa82ea3ad4
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "122531352"
---
# <a name="migrate-analytics-resources-to-global-azure"></a>Migrer des ressources d’analytique vers Azure global

[!INCLUDE [closureinfo](../../includes/germany-closure-info.md)]


Cet article contient des informations qui peuvent vous aider à migrer des ressources d’analytique Azure depuis Azure Germany vers Azure global.
  
## <a name="event-hubs"></a>Event Hubs

Vous ne pouvez pas migrer directement des ressources Microsoft Azure Event Hubs depuis Azure Germany vers Azure global. Le service Event Hubs ne dispose d’aucune fonctionnalité d’exportation ou d’importation des données. Toutefois, vous pouvez exporter des ressources Event Hubs [en tant que modèle](../azure-resource-manager/templates/export-template-portal.md). Ensuite, adaptez le modèle exporté à Azure global et recréez les ressources.

> [!NOTE]
> L’exportation d’un modèle d’Event Hubs ne copie pas les données (par exemple, les messages) ; elle recrée uniquement les métadonnées Event Hubs.

> [!IMPORTANT]
> Modifiez l’emplacement, les secrets Azure Key Vault, les certificats et autres GUID de façon à assurer la cohérence avec la nouvelle région.

### <a name="event-hubs-metadata"></a>Métadonnées Event Hubs

Les éléments de métadonnées suivants sont recréés lorsque vous exportez un modèle Event Hubs :

- Espaces de noms
- Hubs d'événements
- Groupes de consommateurs
- Règles d’autorisation

Pour plus d’informations :

- Consultez la [vue d’ensemble d’Event Hubs](../event-hubs/event-hubs-about.md).
- Actualisez vos connaissances en effectuant les [didacticiels sur Event Hubs](../event-hubs/index.yml).
- Vérifiez les étapes de migration pour [Azure Service Bus](./germany-migration-integration.md#service-bus).
- Découvrez comment [exporter des modèles Microsoft Azure Resource Manager](../azure-resource-manager/templates/export-template-portal.md) ou lisez la présentation [d’Azure Resource Manager](../azure-resource-manager/management/overview.md).

## <a name="hdinsight"></a>HDInsight

Pour migrer des clusters Azure HDInsight depuis Azure Germany vers Azure global, procédez comme suit :

1. Arrêtez le cluster HDInsight.
2. Migrez les données du compte de stockage Microsoft Azure vers la nouvelle région à l’aide d’AzCopy ou d’un outil similaire.
3. Créez des ressources de calcul dans Azure global et associez les ressources de stockage migrées en tant que stockage principal joint.

Pour les clusters plus spécialisés et qui s’exécutent sur le long terme (comme Kafka, la diffusion en continu Spark, Storm ou HBase), nous vous recommandons d’orchestrer la transition des charges de travail vers la nouvelle région.

Pour plus d’informations :

- Consultez la [documentation relative à Azure HDInsight](../hdinsight/index.yml).
- Actualisez vos connaissances en effectuant les [didacticiels sur HDInsight](../hdinsight/index.yml).
- Pour savoir comment [mettre à l’échelle des clusters HDInsight](../hdinsight/hdinsight-administer-use-powershell.md#scale-clusters), voir [Gestion des clusters Apache Hadoop dans HDInsight au moyen d’Azure PowerShell](../hdinsight/hdinsight-administer-use-powershell.md).
- Apprenez à utiliser [AzCopy](../storage/common/storage-use-azcopy-v10.md).

## <a name="stream-analytics"></a>Stream Analytics

Pour migrer les services Azure Stream Analytics depuis Azure Germany vers Azure global, recréez manuellement la configuration complète dans une région Azure globale à l’aide du Portail Microsoft Azure ou de PowerShell. Les sources d’entrées et de sorties d’un travail Stream Analytics peuvent se trouver dans n’importe quelle région.

Pour plus d’informations :

- Actualisez vos connaissances en effectuant les [didacticiels sur Stream Analytics](../stream-analytics/stream-analytics-real-time-fraud-detection.md).
- Consultez la section [Stream Analytics Overview](../stream-analytics/stream-analytics-introduction.md) (Vue d’ensemble de Stream Analytics).
- Apprenez comment [créer un travail Stream Analytics à l’aide de PowerShell](../stream-analytics/stream-analytics-quick-create-powershell.md).

## <a name="sql-database"></a>Base de données SQL

Pour migrer de plus petites charges de travail Azure SQL Database, utilisez la fonction d’exportation afin de créer un fichier BACPAC. Un fichier BACPAC est un fichier (zippé) compressé qui contient des métadonnées et des données issues de la base de données SQL Server. Après avoir créé le fichier BACPAC, vous pouvez le copier dans l’environnement cible (par exemple, en utilisant AzCopy) et utiliser la fonction d’importation pour régénérer la base de données. Tenez compte des points suivants :

- Pour qu’une exportation soit cohérente au niveau transactionnel, assurez-vous que l’une des conditions suivantes est remplie :
  - Aucune activité d’écriture ne survient lors de l’exportation.
  - Vous exportez une copie cohérente au niveau transactionnel d’une base de données SQL.
- Pour que vous puissiez l’exporter vers le stockage Blob Azure, la taille du fichier BACPAC ne doit pas dépasser 200 Go. Si vous voulez exporter un fichier BACPAC plus volumineux, utilisez le stockage local.
- Si l’opération d’exportation depuis la base de données SQL Database prend plus de 20 heures, il se peut que l’opération soit annulée. Consultez les articles suivants pour obtenir des conseils sur la façon d’améliorer les performances.

> [!NOTE]
> La chaîne de connexion change après l’opération d’exportation, car le nom DNS du serveur change pendant l’exportation.

Pour plus d’informations :

- Découvrez comment [exporter une base de données vers un fichier BACPAC](../azure-sql/database/database-export.md).
- Découvrez comment [importer un fichier BACPAC dans une base de données](../azure-sql/database/database-import.md).
- Consultez la [documentation relative à Azure SQL Database](/azure/sql-database/).

## <a name="analysis-services"></a>Analysis Services

Pour migrer vos modèles Azure Analysis Services depuis Azure Germany vers Azure global, utilisez des opérations de [sauvegarde et de restauration](../analysis-services/analysis-services-backup.md).

Si vous souhaitez migrer uniquement les métadonnées du modèle, et non les données, vous pouvez aussi [redéployer le modèle à partir de SQL Server Data Tools](../analysis-services/analysis-services-deploy.md).

Pour plus d’informations :

- Découvrez comment effectuer une [sauvegarde et une restauration via Analysis Services](../analysis-services/analysis-services-backup.md).
- Consultez la [vue d’ensemble de Microsoft Azure Analysis Services](../analysis-services/analysis-services-overview.md).

## <a name="next-steps"></a>Étapes suivantes

Informez-vous sur les outils, techniques et suggestions pour migrer des ressources dans les catégories de service suivantes :

- [Calcul](./germany-migration-compute.md)
- [Réseau](./germany-migration-networking.md)
- [Stockage](./germany-migration-storage.md)
- [Web](./germany-migration-web.md)
- [Bases de données](./germany-migration-databases.md)
- [IoT](./germany-migration-iot.md)
- [Intégration](./germany-migration-integration.md)
- [Identité](./germany-migration-identity.md)
- [Sécurité](./germany-migration-security.md)
- [Outils d'administration](./germany-migration-management-tools.md)
- [Média](./germany-migration-media.md)