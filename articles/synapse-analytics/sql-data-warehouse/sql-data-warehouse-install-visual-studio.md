---
title: Installer Visual Studio 2019
description: Installer Visual Studio et SQL Server Data Tools (SSDT) pour Synapse SQL
ms.custom: vs-azure, azure-synapse
ms.workload: azure-vs
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 05/11/2020
ms.author: jrasnick
ms.reviewer: igorstan
ms.openlocfilehash: 378c36f1df7c83c6309be44d24ec671b64e34c08
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131054606"
---
# <a name="getting-started-with-visual-studio-2019"></a>Prise en main de Visual Studio 2019

Visual Studio **2019** SQL Server Data Tools (SSDT) est un outil unique qui vous permet d’effectuer les opérations suivantes :

- Connecter, interroger et développer des applications
- Utiliser un explorateur d’objets pour explorer visuellement tous les objets de votre modèle de données, notamment les tables, les vues, les procédures stockées, etc.
- Générer des scripts DDL (Data Definition Language) T-SQL pour vos objets
- Développer votre entrepôt de données à l’aide d’une approche basée sur l’état avec des projets de base de données SSDT
- Intégrer votre projet de base de données à des systèmes de contrôle de code source comme Git avec Azure Repos
- Configurer des pipelines d’intégration et de déploiement continus avec des serveurs d’automatisation comme Azure DevOps

## <a name="install-visual-studio-2019"></a>Installer Visual Studio 2019

Consultez [Télécharger Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) pour télécharger et installer Visual Studio **16.3 ou version ultérieure**. Lors de l’installation, sélectionnez la charge de travail de stockage et de traitement des données. L’installation SSDT autonome n’est plus obligatoire dans Visual Studio 2019.

## <a name="unsupported-features-in-ssdt"></a>Fonctionnalités non prises en charge sur SSDT

Certaines mises en production de fonctionnalités Synapse SQL peuvent ne pas prendre en charge SSDT. Actuellement, les fonctionnalités suivantes ne sont pas prises en charge :


- [Gestion des charges de travail](sql-data-warehouse-workload-management.md) : classifieurs et groupes de charge de travail
- [Sécurité au niveau des lignes](/sql/relational-databases/security/row-level-security?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest&preserve-view=true) (y compris les fonctions table)
  - Envoyez un [ticket de support ou votez](https://feedback.azure.com/d365community/idea/a9192b4c-0b25-ec11-b6e6-000d3a4f07b8) pour obtenir la fonctionnalité prise en charge.
  - Envoyez un [ticket de support ou votez](https://feedback.azure.com/d365community/idea/ab192b4c-0b25-ec11-b6e6-000d3a4f07b8) pour obtenir la fonctionnalité prise en charge.
- Certaines fonctionnalités T-SQL, telles que :
   - La clause *WITHIN GROUP* de la fonction de chaîne [STRING_AGG](/sql/t-sql/functions/string-agg-transact-sql).

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous disposez de la dernière version de SSDT, vous êtes prêt à vous [connecter](sql-data-warehouse-query-visual-studio.md) à votre pool SQL.