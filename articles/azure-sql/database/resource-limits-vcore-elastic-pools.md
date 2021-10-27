---
title: Limites des ressources vCore des pools élastiques
description: Cette page décrit certaines des limites de ressources vCore courantes pour des pools élastiques dans Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: sqldbrb=1, references_regions
ms.devlang: ''
ms.topic: reference
author: dimitri-furman
ms.author: dfurman
ms.reviewer: mathoma
ms.date: 10/12/2021
ms.openlocfilehash: 300a550363b01e367931ccb34efd3dd7d7d593cc
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2021
ms.locfileid: "129997285"
---
# <a name="resource-limits-for-elastic-pools-using-the-vcore-purchasing-model"></a>Limites de ressources pour les pools élastiques suivant le modèle d’achat vCore
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Cet article détaille les limites de ressources des pools élastiques Azure SQL Database et des bases de données mises en pool suivant le modèle d’achat vCore.

* Pour connaître les limites du modèle d’achat DTU pour les bases de données uniques sur un serveur, consultez [Vue d’ensemble des limites de ressources sur un serveur](resource-limits-logical-server.md).
* Pour connaître les limites de ressources du modèle d’achat DTU d’Azure SQL Database, consultez les [limites de ressources DTU pour les bases de données uniques](resource-limits-dtu-single-databases.md) et celles pour les [pools élastiques](resource-limits-dtu-elastic-pools.md).
* Pour connaître les limites de ressources vCore, consultez [Limites de ressources vCore : Azure SQL Database](resource-limits-vcore-single-databases.md) et [Limites de ressources vCore : pools élastiques](resource-limits-vcore-elastic-pools.md).
* Pour plus d’informations concernant les différents modèles d’achat, consultez l’article décrivant les [modèles d’achat et niveaux de service](purchasing-models.md).

> [!IMPORTANT]
> Dans certaines circonstances, vous devrez peut-être réduire une base de données pour récupérer l’espace inutilisé. Pour plus d’informations, consultez [Gérer l’espace des fichiers dans Azure SQL Database](file-space-manage.md).

Chaque réplica en lecture seule d’un pool élastique a ses propres ressources, comme les vCores, la mémoire, les IOPS de données, TempDB, les workers et les sessions. Chaque réplica en lecture seule est soumis aux limites de ressources du pool élastique détaillées plus loin dans cet article.

Vous pouvez définir le niveau de service, la taille de calcul (objectif de service) et la quantité de stockage via :

* [Transact-SQL](elastic-pool-scale.md) via [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql#overview-sql-database)
* [Azure portal](elastic-pool-manage.md#azure-portal)
* [PowerShell](elastic-pool-manage.md#powershell)
* [Azure CLI](elastic-pool-manage.md#azure-cli)
* [REST API](elastic-pool-manage.md#rest-api)

> [!IMPORTANT]
> Pour des instructions et des informations sur la mise à l’échelle, consultez [Mettre à l’échelle un pool élastique](elastic-pool-scale.md).

Si tous les vCore d’un pool élastique sont occupés, chaque base de données du pool reçoit une quantité égale de ressources de calcul pour traiter les requêtes. Azure SQL Database assure un partage équitable des ressources entre les bases de données en garantissant des tranches de temps de calcul égales. Le partage équitable des ressources du pool élastique s’ajoute à n’importe quelle quantité de ressources garantie pour chaque base de données lorsque le nombre minimal de vCore par base de données est défini sur une valeur différente de zéro.

## <a name="general-purpose---provisioned-compute---gen4"></a>Usage général - calcul provisionné - Gen4

> [!IMPORTANT]
> Les nouvelles bases de données Gen4 ne sont plus prises en charge dans les régions Australie Est et Brésil Sud.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Niveau de service d’usage général : Plateforme de calcul de génération 4 (partie 1)

|Taille de calcul (objectif de service)|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|Génération de calcul|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|1|2|3|4|5|6|
|Mémoire (Go)|7|14|21|28|35|42|
|Nombre maximal de bases de données par pool <sup>1</sup>|100|200|500|500|500|500|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|N/A|N/A|N/A|N/A|N/A|N/A|
|Taille maximale des données (Go)|512|756|1536|1536|1536|2 048|
|Taille maximale du journal <sup>2</sup>|154|227|461|461|461|614|
|Taille maximale des données TempDB (Go)|32|64|96|128|160|192|
|Type de stockage|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|
|Latence d’E/S (approximative)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup> |400|800|1200|1 600|2000|2 400|
|Taux maximal de journaux par pool (Mbits/s)|6|12|18|24|30|36|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup> |210|420|630|840|1050|1 260|
|Nombre maximal de connexions simultanées par pool <sup>4</sup> |210|420|630|840|1050|1 260|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|0 ; 0,25 ; 0,5 ; 1|0 ; 0,25 ; 0,5 ; 1 ; 2|0 ; 0,25 ; 0,5 ; 1...3|0 ; 0,25 ; 0,5, 1...4|0 ; 0,25 ; 0,5 ; 1...5|0 ; 0,25 ; 0,5 ; 1...6|
|Nombre de réplicas|1|1|1|1|1|1|
|Plusieurs zones de disponibilités|N/A|N/A|N/A|N/A|N/A|N/A|
|Lecture du Scale-out|N/A|N/A|N/A|N/A|N/A|N/A|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Niveau de service d’usage général : Plateforme de calcul de génération 4 (partie 2)

|Taille de calcul (objectif de service)|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Génération de calcul|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|7|8|9|10|16|24|
|Mémoire (Go)|49|56|63|70|112|159,5|
|Nombre maximal de bases de données par pool <sup>1</sup>|500|500|500|500|500|500|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|N/A|N/A|N/A|N/A|N/A|N/A|
|Taille maximale des données (Go)|2 048|2 048|2 048|2 048|3584|4096|
|Taille maximale du journal (Go) <sup>2</sup>|614|614|614|614|1075|1229|
|Taille maximale des données TempDB (Go)|224|256|288|320|512|768|
|Type de stockage|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|
|Latence d’E/S (approximative)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|2 800|3200|3600|4000|6 400|9 600|
|Taux maximal de journaux par pool (Mbits/s)|42|48|54|60|62.5|62.5|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|1470|1680|1890|2100|3360|5040|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|1470|1680|1890|2100|3360|5040|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|0 ; 0,25 ; 0,5 ; 1... 7|0 ; 0,25 ; 0,5 ; 1... 8|0 ; 0,25 ; 0,5 ; 1... 9|0 ; 0,25 ; 0,5 ; 1... 10|0 ; 0,25 ; 0,5 ; 1... 10 ; 16|0 ; 0,25 ; 0,5 ; 1... 10 ; 16 ; 24|
|Nombre de réplicas|1|1|1|1|1|1|
|Plusieurs zones de disponibilités|N/A|N/A|N/A|N/A|N/A|N/A|
|Lecture du Scale-out|N/A|N/A|N/A|N/A|N/A|N/A|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).    

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

## <a name="general-purpose---provisioned-compute---gen5"></a>Usage général - calcul provisionné - Gen5

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Niveau de service d’usage général : Plateforme de calcul de génération 5 (partie 1)

|Taille de calcul (objectif de service)|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Génération de calcul|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|2|4|6|8|10|12|14|
|Mémoire (Go)|10,4|20,8|31,1|41,5|51,9|62,3|72,7|
|Nombre maximal de bases de données par pool <sup>1</sup>|100|200|500|500|500|500|500|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Taille maximale des données (Go)|512|756|1536|1536|1536|2 048|2 048|
|Taille maximale du journal (Go) <sup>2</sup>|154|227|461|461|461|614|614|
|Taille maximale des données TempDB (Go)|64|128|192|256|320|384|448|
|Type de stockage|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|
|Latence d’E/S (approximative)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|800|1 600|2 400|3200|4000|4 800|5600|
|Taux maximal de journaux par pool (Mbits/s)|12|24|36|48|60|62.5|62.5|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|210|420|630|840|1050|1 260|1470|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|210|420|630|840|1050|1 260|1470|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|0 ; 0,25 ; 0,5 ; 1 ; 2|0 ; 0,25 ; 0,5, 1...4|0 ; 0,25 ; 0,5 ; 1...6|0 ; 0,25 ; 0,5 ; 1... 8|0 ; 0,25 ; 0,5 ; 1... 10|0 ; 0,25 ; 0,5 ; 1... 12|0 ; 0,25 ; 0,5 ; 1... 14|
|Nombre de réplicas|1|1|1|1|1|1|1|
|Plusieurs zones de disponibilités|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|
|Lecture du Scale-out|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Niveau de service d’usage général : Plateforme de calcul de génération 5 (partie 2)

|Taille de calcul (objectif de service)|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Génération de calcul|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|16|18|20|24|32|40|80|
|Mémoire (Go)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|Nombre maximal de bases de données par pool <sup>1</sup>|500|500|500|500|500|500|500|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Taille maximale des données (Go)|2 048|3 072|3 072|3 072|4096|4096|4096|
|Taille maximale du journal (Go) <sup>2</sup>|614|922|922|922|1229|1229|1229|
|Taille maximale des données TempDB (Go)|512|576|640|768|1 024|1 280|2560|
|Type de stockage|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|
|Latence d’E/S (approximative)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup> |6 400|7 200|8,000|9 600|12 800|16 000|16 000|
|Taux maximal de journaux par pool (Mbits/s)|62.5|62.5|62.5|62.5|62.5|62.5|62.5|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|1680|1890|2100|2520|3360|4200|8400|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|1680|1890|2100|2520|3360|4200|8400|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|0 ; 0,25 ; 0,5 ; 1... 16|0 ; 0,25 ; 0,5 ; 1... 18|0 ; 0,25 ; 0,5 ; 1... 20|0 ; 0,25 ; 0,5 ; 1... 20 ; 24|0 ; 0,25 ; 0,5 ; 1... 20 ; 24 ; 32|0 ; 0,25 ; 0,5 ; 1... 16 ; 24 ; 32 ; 40|0 ; 0,25 ; 0,5 ; 1... 16 ; 24 ; 32 ; 40 ; 80|
|Nombre de réplicas|1|1|1|1|1|1|1|
|Plusieurs zones de disponibilités|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|[Disponible en préversion](high-availability-sla.md#general-purpose-service-tier-zone-redundant-availability-preview)|
|Lecture du Scale-out|N/A|N/A|N/A|N/A|N/A|N/A|N/A|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

## <a name="general-purpose---provisioned-compute---fsv2-series"></a>Usage général - calcul provisionné - série Fsv2

### <a name="fsv2-series-compute-generation-part-1"></a>Génération de calcul de série Fsv2 (partie 1)

|Taille de calcul (objectif de service)|GP_Fsv2_8|GP_Fsv2_10|GP_Fsv2_12|GP_Fsv2_14| GP_Fsv2_16|
|:---| ---:|---:|---:|---:|---:|
|Génération de calcul|Série Fsv2|Série Fsv2|Série Fsv2|Série Fsv2|Série Fsv2|
|vCores|8|10|12|14|16|
|Mémoire (Go)|15,1|18,9|22,7|26,5|30,2|
|Nombre maximal de bases de données par pool <sup>1</sup>|500|500|500|500|500|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|N/A|N/A|N/A|N/A|N/A|
|Taille maximale des données (Go)|1 024|1 024|1 024|1 024|1536|
|Taille maximale du journal (Go) <sup>2</sup>|336|336|336|336|512|
|Taille maximale des données TempDB (Go)|37|46|56|65|74|
|Type de stockage|SSD distant|SSD distant|SSD distant|SSD distant|SSD distant|
|Latence d’E/S (approximative)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|2560|3200|3840|4480|5120|
|Taux maximal de journaux par pool (Mbits/s)|48|60|62.5|62.5|62.5|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|400|500|600|700|800|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|800|1 000|1200|1400|1 600|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|0-8|0-10|0-12|0-14|0-16|
|Nombre de réplicas|1|1|1|1|1|
|Plusieurs zones de disponibilités|N/A|N/A|N/A|N/A|N/A|
|Lecture du Scale-out|N/A|N/A|N/A|N/A|N/A|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

### <a name="fsv2-series-compute-generation-part-2"></a>Génération de calcul de série Fsv2 (partie 2)

|Taille de calcul (objectif de service)|GP_Fsv2_18|GP_Fsv2_20|GP_Fsv2_24|GP_Fsv2_32| GP_Fsv2_36|GP_Fsv2_72|
|:---| ---:|---:|---:|---:|---:|---:|
|Génération de calcul|Série Fsv2|Série Fsv2|Série Fsv2|Série Fsv2|Série Fsv2|Série Fsv2|
|vCores|18|20|24|32|36|72|
|Mémoire (Go)|34,0|37,8|45.4|60,5|68,0|136,0|
|Nombre maximal de bases de données par pool <sup>1</sup>|500|500|500|500|500|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|N/A|N/A|N/A|N/A|N/A|N/A|
|Taille maximale des données (Go)|1536|1536|1536|3 072|3 072|4096|
|Taille maximale du journal (Go) <sup>2</sup>|512|512|512|1 024|1 024|1 024|
|Taille maximale des données TempDB (Go)|83|93|111|148|167|333|
|Type de stockage|SSD distant|SSD distant|SSD distant|SSD distant|SSD distant|SSD distant|
|Latence d’E/S (approximative)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|5760|6 400|7680|10240|11520|12800|
|Taux maximal de journaux par pool (Mbits/s)|62.5|62.5|62.5|62.5|62.5|62.5|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|900|1 000|1200|1 600|1800|3600|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|1800|2000|2 400|3200|3600|7200|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|0-18|0-20|0-24|0-32|0-36|0-72|
|Nombre de réplicas|1|1|1|1|1|1|
|Plusieurs zones de disponibilités|N/A|N/A|N/A|N/A|N/A|N/A|
|Lecture du Scale-out|N/A|N/A|N/A|N/A|N/A|N/A|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

## <a name="general-purpose---provisioned-compute---dc-series"></a>Usage général – calcul approvisionné – série DC

|Taille de calcul (objectif de service)|GP_DC_2|GP_DC_4|GP_DC_6|GP_DC_8|
|:--- | --: |--: |--: |--: |
|Génération de calcul|DC|DC|DC|DC|
|vCores|2|4|6|8|
|Mémoire (Go)|9|18|27|36|
|Nombre maximal de bases de données par pool <sup>1</sup>|100|400|400|400|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|N/A|N/A|N/A|N/A|
|Taille maximale des données (Go)|756|1536|2 048|2 048|
|Taille maximale du journal (Go) <sup>2</sup>|227|461|614|614|
|Taille maximale des données TempDB (Go)|64|128|192|256|
|Type de stockage|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|Stockage (distant) Premium|
|Latence d’E/S (approximative)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|5-7 ms (écriture)<br>5-10 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|800|1 600|2 400|3200|
|Taux maximal de journaux par pool (Mbits/s)|12|24|36|48|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|168|336|504|672|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|168|336|504|672|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|2|2 à 4|2 à 6|2 à 8|
|Nombre de réplicas|1|1|1|1|
|Plusieurs zones de disponibilités|N/A|N/A|N/A|N/A|
|Lecture du Scale-out|N/A|N/A|N/A|N/A|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

## <a name="business-critical---provisioned-compute---gen4"></a>Vital pour l’entreprise - calcul provisionné - Gen4

> [!IMPORTANT]
> Les nouvelles bases de données Gen4 ne sont plus prises en charge dans les régions Australie Est et Brésil Sud.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-1"></a>Niveau de service vital pour l’entreprise : Plateforme de calcul de génération 4 (partie 1)

|Taille de calcul (objectif de service)|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|Génération de calcul|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|2|3|4|5|6|
|Mémoire (Go)|14|21|28|35|42|
|Nombre maximal de bases de données par pool <sup>1</sup>|50|100|100|100|100|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|2|3|4|5|6|
|Type de stockage|SSD local|SSD local|SSD local|SSD local|SSD local|
|Taille maximale des données (Go)|1 024|1 024|1 024|1 024|1 024|
|Taille maximale du journal (Go) <sup>2</sup>|307|307|307|307|307|
|Taille maximale des données TempDB (Go)|64|96|128|160|192|
|[Taille maximale du stockage local](resource-limits-logical-server.md#storage-space-governance) (Go)|1356|1356|1356|1356|1356|
|Latence d’E/S (approximative)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|9 000|13 500|18 000|22 500|27 000|
|Taux maximal de journaux par pool (Mbits/s)|20|30|40|50|60|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|420|630|840|1050|1 260|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|420|630|840|1050|1 260|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|0 ; 0,25 ; 0,5 ; 1 ; 2|0 ; 0,25 ; 0,5 ; 1...3|0 ; 0,25 ; 0,5, 1...4|0 ; 0,25 ; 0,5 ; 1...5|0 ; 0,25 ; 0,5 ; 1...6|
|Nombre de réplicas|4|4|4|4|4|
|Plusieurs zones de disponibilités|Oui|Oui|Oui|Oui|Oui|
|Lecture du Scale-out|Oui|Oui|Oui|Oui|Oui|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>Niveau de service vital pour l’entreprise : Plateforme de calcul de génération 4 (partie 2)

|Taille de calcul (objectif de service)|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Génération de calcul|Gen4|Gen4|Gen4|Gen4|Gen4|Gen4|
|vCores|7|8|9|10|16|24|
|Mémoire (Go)|49|56|63|70|112|159,5|
|Nombre maximal de bases de données par pool <sup>1</sup>|100|100|100|100|100|100|
|Prise en charge de ColumnStore|N/A|N/A|N/A|N/A|N/A|N/A|
|Stockage In-Memory OLTP (Go)|7|8|9.5|11|20|36|
|Type de stockage|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Taille maximale des données (Go)|1 024|1 024|1 024|1 024|1 024|1 024|
|Taille maximale du journal (Go) <sup>2</sup>|307|307|307|307|307|307|
|Taille maximale des données TempDB (Go)|224|256|288|320|512|768|
|[Taille maximale du stockage local](resource-limits-logical-server.md#storage-space-governance) (Go)|1356|1356|1356|1356|1356|1356|
|Latence d’E/S (approximative)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|31 500|36 000|40 500|45,000|72 000|96 000|
|Taux maximal de journaux par pool (Mbits/s)|70|80|80|80|80|80|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|1470|1680|1890|2100|3360|5040|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|1470|1680|1890|2100|3360|5040|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|0 ; 0,25 ; 0,5 ; 1... 7|0 ; 0,25 ; 0,5 ; 1... 8|0 ; 0,25 ; 0,5 ; 1... 9|0 ; 0,25 ; 0,5 ; 1... 10|0 ; 0,25 ; 0,5 ; 1... 10 ; 16|0 ; 0,25 ; 0,5 ; 1... 10 ; 16 ; 24|
|Nombre de réplicas|4|4|4|4|4|4|
|Plusieurs zones de disponibilités|Oui|Oui|Oui|Oui|Oui|Oui|
|Lecture du Scale-out|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

## <a name="business-critical---provisioned-compute---gen5"></a>Vital pour l’entreprise - calcul provisionné - Gen5

### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>Niveau de service vital pour l’entreprise : Plateforme de calcul de génération 5 (partie 1)

|Taille de calcul (objectif de service)|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Génération de calcul|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|4|6|8|10|12|14|
|Mémoire (Go)|20,8|31,1|41,5|51,9|62,3|72,7|
|Nombre maximal de bases de données par pool <sup>1</sup>|50|100|100|100|100|100|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|3,14|4.71|6,28|8,65|11,02|13,39|
|Taille maximale des données (Go)|1 024|1536|1536|1536|3 072|3 072|
|Taille maximale du journal (Go) <sup>2</sup>|307|307|461|461|922|922|
|Taille maximale des données TempDB (Go)|128|192|256|320|384|448|
|[Taille maximale du stockage local](resource-limits-logical-server.md#storage-space-governance) (Go)|4829|4829|4829|4829|4829|4829|
|Type de stockage|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latence d’E/S (approximative)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|18 000|27 000|36 000|45,000|54 000|63 000|
|Taux maximal de journaux par pool (Mbits/s)|60|90|120|120|120|120|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|420|630|840|1050|1 260|1470|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|420|630|840|1050|1 260|1470|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|0 ; 0,25 ; 0,5, 1...4|0 ; 0,25 ; 0,5 ; 1...6|0 ; 0,25 ; 0,5 ; 1... 8|0 ; 0,25 ; 0,5 ; 1... 10|0 ; 0,25 ; 0,5 ; 1... 12|0 ; 0,25 ; 0,5 ; 1... 14|
|Nombre de réplicas|4|4|4|4|4|4|
|Plusieurs zones de disponibilités|Oui|Oui|Oui|Oui|Oui|Oui|
|Lecture du Scale-out|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>Niveau de service vital pour l’entreprise : Plateforme de calcul de génération 5 (partie 2)

|Taille de calcul (objectif de service)|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|Génération de calcul|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|Gen5|
|vCores|16|18|20|24|32|40|80|
|Mémoire (Go)|83|93,4|103,8|124,6|166,1|207,6|415,2|
|Nombre maximal de bases de données par pool <sup>1</sup>|100|100|100|100|100|100|100|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|15,77|18,14|20,51|25,25|37,94|52,23|131,68|
|Taille maximale des données (Go)|3 072|3 072|3 072|4096|4096|4096|4096|
|Taille maximale du journal (Go) <sup>2</sup>|922|922|922|1229|1229|1229|1229|
|Taille maximale des données TempDB (Go)|512|576|640|768|1 024|1 280|2560|
|[Taille maximale du stockage local](resource-limits-logical-server.md#storage-space-governance) (Go)|4829|4829|4829|4829|4829|4829|4829|
|Type de stockage|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latence d’E/S (approximative)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|72 000|81 000|90 000|108 000|144 000|180 000|256 000|
|Taux maximal de journaux par pool (Mbits/s)|120|120|120|120|120|120|120|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|1680|1890|2100|2520|3360|4200|8400|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|1680|1890|2100|2520|3360|4200|8400|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|0 ; 0,25 ; 0,5 ; 1... 16|0 ; 0,25 ; 0,5 ; 1... 18|0 ; 0,25 ; 0,5 ; 1... 20|0 ; 0,25 ; 0,5 ; 1... 20 ; 24|0 ; 0,25 ; 0,5 ; 1... 20 ; 24 ; 32|0 ; 0,25 ; 0,5 ; 1... 20 ; 24 ; 32 ; 40|0 ; 0,25 ; 0,5 ; 1... 20 ; 24 ; 32 ; 40 ; 80|
|Nombre de réplicas|4|4|4|4|4|4|4|
|Plusieurs zones de disponibilités|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|Lecture du Scale-out|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

## <a name="business-critical---provisioned-compute---m-series"></a>Vital pour l’entreprise - calcul provisionné - série M

### <a name="m-series-compute-generation-part-1"></a>Génération de calcul de série M (partie 1)

|Taille de calcul (objectif de service)|BC_M_8|BC_M_10|BC_M_12|BC_M_14|BC_M_16|BC_M_18|
|:---| ---:|---:|---:|---:|---:|---:|
|Génération de calcul|Série M|Série M|Série M|Série M|Série M|Série M|
|vCores|8|10|12|14|16|18|
|Mémoire (Go)|235,4|294,3|353,2|412,0|470,9|529,7|
|Nombre maximal de bases de données par pool <sup>1</sup>|100|100|100|100|100|100|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|64|80|96|112|128|150|
|Taille maximale des données (Go)|512|640|768|896|1 024|1152|
|Taille maximale du journal (Go) <sup>2</sup>|171|213|256|299|341|384|
|Taille maximale des données TempDB (Go)|256|320|384|448|512|576|
|[Taille maximale du stockage local](resource-limits-logical-server.md#storage-space-governance) (Go)|13836|13836|13836|13836|13836|13836|
|Type de stockage|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latence d’E/S (approximative)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|12 499|15 624|18 748|21 873|24 998|28 123|
|Taux maximal de journaux par pool (Mbits/s)|48|60|72|84|96|108|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|800|1 000|1,200|1 400|1 600|1 800|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|800|1 000|1,200|1 400|1 600|1 800|
|Nombre maximal de sessions simultanées|30000|30000|30000|30000|30000|30000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|0-8|0-10|0-12|0-14|0-16|0-18|
|Nombre de réplicas|4|4|4|4|4|4|
|Plusieurs zones de disponibilités|Non|Non|Non|Non|Non|Non|
|Lecture du Scale-out|Oui|Oui|Oui|Oui|Oui|Oui|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

### <a name="m-series-compute-generation-part-2"></a>Génération de calcul de série M (partie 2)

|Taille de calcul (objectif de service)|BC_M_20|BC_M_24|BC_M_32|BC_M_64|BC_M_128|
|:---| ---:|---:|---:|---:|---:|
|Génération de calcul|Série M|Série M|Série M|Série M|Série M|
|vCores|20|24|32|64|128|
|Mémoire (Go)|588,6|706,3|941,8|1883,5|3767,0|
|Nombre maximal de bases de données par pool <sup>1</sup>|100|100|100|100|100|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|172|216|304|704|1768|
|Taille maximale des données (Go)|1 280|1536|2 048|4096|4096|
|Taille maximale du journal (Go) <sup>2</sup>|427|512|683|1 024|1 024|
|Taille maximale des données TempDB (Go)|640|768|1 024|2 048|4096|
|[Taille maximale du stockage local](resource-limits-logical-server.md#storage-space-governance) (Go)|13836|13836|13836|13836|13836|
|Type de stockage|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latence d’E/S (approximative)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|31 248|37 497|49 996|99 993|160 000|
|Taux maximal de journaux par pool (Mbits/s)|120|144|192|264|264|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|2 000|2 400|3 200|6 400|12 800|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|2 000|2 400|3 200|6 400|12 800|
|Nombre maximal de sessions simultanées|30000|30000|30000|30000|30000|
|Nombre de réplicas|4|4|4|4|4|
|Plusieurs zones de disponibilités|Non|Non|Non|Non|Non|
|Lecture du Scale-out|Oui|Oui|Oui|Oui|Oui|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

## <a name="business-critical---provisioned-compute---dc-series"></a>Vital pour l’entreprise – Calcul approvisionné – Série DC

|Taille de calcul (objectif de service)|BC_DC_2|BC_DC_4|BC_DC_6|BC_DC_8|
|:--- | --: |--: |--: |--: |
|Génération de calcul|DC|DC|DC|DC|
|vCores|2|4|6|8|
|Mémoire (Go)|9|18|27|36|
|Nombre maximal de bases de données par pool <sup>1</sup>|50|100|100|100|
|Prise en charge de ColumnStore|Oui|Oui|Oui|Oui|
|Stockage In-Memory OLTP (Go)|1.7|3.7|5.9|8,2|
|Taille maximale des données (Go)|768|768|768|768|
|Taille maximale du journal (Go) <sup>2</sup>|230|230|230|230|
|Taille maximale des données TempDB (Go)|64|128|192|256|
|[Taille maximale du stockage local](resource-limits-logical-server.md#storage-space-governance) (Go)|1406|1406|1406|1406|
|Type de stockage|SSD local|SSD local|SSD local|SSD local|
|Latence d’E/S (approximative)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|1-2 ms (écriture)<br>1-2 ms (lecture)|
|Nombre maximal d’IOPS de données par pool <sup>3</sup>|15 750|31 500|47 250|56 000|
|Taux maximal de journaux par pool (Mbits/s)|20|60|90|120|
|Nombre maximal de Workers simultanés par pool (requêtes) <sup>4</sup>|168|336|504|672|
|Nombre maximal de connexions simultanées par pool (requêtes) <sup>4</sup>|168|336|504|672|
|Nombre maximal de sessions simultanées|30,000|30,000|30,000|30,000|
|Choix du nombre minimal/maximal de cœurs virtuels de pool élastique par base de données|2|2 à 4|2 à 6|2 à 8|
|Nombre de réplicas|4|4|4|4|
|Plusieurs zones de disponibilités|Non|Non|Non|Non|
|Lecture du Scale-out|Oui|Oui|Oui|Oui|
|Stockage de sauvegarde inclus|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|1X taille de la base de données|

<sup>1</sup> Consultez [Gestion des ressources dans les pools élastiques denses](elastic-pool-resource-management.md) pour obtenir des informations supplémentaires.

<sup>2</sup> Pour les valeurs de tailles de données maximales documentées. La réduction de la taille maximale des données réduit proportionnellement la taille maximale du journal.

<sup>3</sup> La valeur maximale pour les tailles d’E/S est comprise entre 8 Ko et 64 Ko. Les IOPS réelles dépendent de la charge de travail. Pour plus d’informations, consultez [Gouvernance des E/S de données](resource-limits-logical-server.md#resource-governance).

<sup>4</sup> Pour connaître le nombre maximal de Workers simultanés (requêtes) pour une base de données individuelle, consultez [Limites de ressources des bases de données uniques](resource-limits-vcore-single-databases.md). Par exemple, si le pool élastique utilise Gen5 et que le nombre maximal de vCores par base de données est défini sur 2, le nombre maximal de Workers simultanés est de 200.  Si le nombre maximal de vCores par base de données est défini sur 0,5, le nombre maximal de Workers simultanés est de 50, puisque le nombre maximal de Workers est de 100 sur Gen5. Pour les autres paramètres de nombre maximal de vCores par base de données qui sont inférieurs ou égaux à 1 vCore, le nombre maximum de Workers simultanés est adapté en conséquence.

## <a name="database-properties-for-pooled-databases"></a>Propriétés de base de données pour les bases de données mises en pool

Pour chaque pool élastique, vous pouvez éventuellement spécifier le nombre minimal et maximal de vCores par base de données pour modifier les modèles de consommation de ressources dans le pool. Les valeurs mini et maxi spécifiées s’appliquent à toutes les bases de données du pool. La personnalisation des vCores mini et maxi pour des bases de données individuelles dans le pool n’est pas prise en charge. 

Vous pouvez également définir un stockage maximal par base de données, par exemple pour empêcher une base de données de consommer tout le stockage du pool. Ce paramètre peut être configuré indépendamment pour chaque base de données.

La table suivante décrit les propriétés de base de données pour les bases de données mises en pool. 

| Propriété | Description |
|:--- |:--- |
| Nombre maximal de vCore par base de données |Nombre maximal de vCore pouvant être utilisés par une des bases de données du pool en fonction du nombre de vCore utilisés par les autres bases de données du pool. Le nombre maximal de vCore par base de données n’est pas une garantie concernant l’octroi des ressources pour une base de données. Si la charge de travail de chaque base de données n’a pas besoin de toutes les ressources de pool disponibles pour s’exécuter correctement, envisagez de définir le nombre maximal de vCores par base de données pour empêcher qu’une base de données unique monopolise les ressources du pool. Une certaine allocation excessive est attendue dans la mesure où le pool prend généralement en compte des modèles de creux et de pics d’utilisation des bases de données, dans lesquels toutes les bases de données ne connaissent pas simultanément des pics d’utilisation. |
| Nombre minimal de vCore par base de données |Nombre minimal de vCores réservés pour toute base de données dans le pool. Envisagez de définir un nombre minimal de vCores par base de données lorsque vous souhaitez garantir la disponibilité des ressources pour chaque base de données, quelle que soit la consommation des ressources par les autres bases de données du pool. Le nombre minimal de vCore par base de données peut être défini sur 0, qui est également la valeur par défaut. Cette propriété est définie sur une valeur comprise entre 0 et le nombre moyen de vCore utilisés par base de données.|
| Espace de stockage maximal par base de données |La taille de base de données maximale définie par l’utilisateur pour une base de données dans un pool. Les bases de données mises en pool se partagent le stockage du pool alloué. Par conséquent, la taille qu’une base de données peut atteindre est limitée au stockage de pool minimal restant et à la taille maximale de la base de données. La taille maximale de la base de données fait référence à la taille maximale des fichiers de données et n’inclut pas l’espace utilisé par le fichier journal. |
|||

> [!IMPORTANT]
> Étant donné que les ressources d’un pool élastique sont limitées, l’affectation de vCores mini par base de données à une valeur supérieure à 0 limite implicitement l’utilisation des ressources par chaque base de données. Si, à un moment donné, la plupart des bases de données d’un pool sont inactives, les ressources réservées pour satisfaire la garantie de vCores mini ne sont pas disponibles pour les bases de données actives à ce moment précis.
>
> En outre, le paramétrage de vCores mini par base de données sur une valeur supérieure à 0 limite implicitement le nombre de bases de données qui peuvent être ajoutées au pool. Par exemple, si vous définissez les vCores mini sur 2 dans un pool de 20 vCores, cela signifie que vous ne pourrez pas ajouter plus de 10 bases de données au pool, car 2 vCores sont réservées pour chaque base de données.
> 

Même si les propriétés par base de données sont exprimées en vCores, elles régissent également la consommation d’autres types de ressources, comme les E/S de données, les E/S de journal, la mémoire du pool de mémoires tampons et les threads de travail. Lorsque vous ajustez les valeurs min et max de vCores par base de données, les réservations et les limites de tous les types de ressources sont ajustées proportionnellement.

Les valeurs minimales et maximales de vCores par base de données s’appliquent à la consommation de ressources par les charges de travail utilisateur, mais pas à la consommation de ressources par les processus internes. Par exemple, pour une base de données dont la valeur maximale de vCores par base de données est fixée à la moitié du nombre de vCores du pool, la charge de travail utilisateur ne peut pas consommer plus de la moitié de la mémoire du pool de mémoires tampons. Toutefois, cette base de données peut toujours tirer parti des pages du pool de mémoires tampons qui ont été chargées par des processus internes. Pour plus d’informations, consultez [Consommation de ressources par les charges de travail utilisateur et les processus internes](resource-limits-logical-server.md#resource-consumption-by-user-workloads-and-internal-processes).

> [!NOTE]
> Les limites de ressources des bases de données individuelles dans les pools élastiques sont généralement identiques à celles des bases de données uniques situées hors des pools qui ont la même taille de calcul (objectif de service). Par exemple, le nombre maximal de workers simultanés dans une base de données GP_Gen4_1 est de 200. Par conséquent, le nombre maximal de workers simultanés pour une base de données dans un pool GP_Gen4_1 est aussi de 200. Notez que le nombre total de workers simultanés dans le pool GP_Gen4_1 est de 210.

## <a name="next-steps"></a>Étapes suivantes

- Pour connaître les limites de ressources vCore d’une base de données unique, consultez l’article consacré aux [limites de ressources pour les bases de données uniques suivant le modèle d’achat vCore](resource-limits-vcore-single-databases.md)
- Pour connaître les limites de ressources DTU d’une base de données unique, consultez l’article consacré aux [limites de ressources pour les bases de données uniques suivant le modèle d’achat DTU](resource-limits-dtu-single-databases.md)
- Pour connaître les limites de ressources DTU des pools élastiques, consultez l’article consacré aux [limites de ressources pour les pools élastiques suivant le modèle d’achat DTU](resource-limits-dtu-elastic-pools.md)
- Pour connaître les limites de ressources des instances gérées, consultez l'article consacré aux [limites de ressources des instances gérées](../managed-instance/resource-limits.md).
- Pour plus d’informations sur les limites générales d’Azure, consultez [Abonnement Azure et limites, quotas et contraintes du service](../../azure-resource-manager/management/azure-subscription-service-limits.md).
- Pour plus d'informations sur les limites de ressources sur un serveur SQL logique et au niveau de l’abonnement, consultez la [vue d'ensemble des limites de ressources sur un serveur SQL logique](resource-limits-logical-server.md).
