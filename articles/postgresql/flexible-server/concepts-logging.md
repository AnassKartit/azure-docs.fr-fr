---
title: Journaux - Azure Database pour PostgreSQL - Serveur flexible
description: Décrit la configuration de la journalisation, le stockage et l’analyse dans Azure Database pour PostgreSQL - Serveur flexible
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 7c269c26710711c23dd200f688bf1eb55d3925fd
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128597965"
---
# <a name="logs-in-azure-database-for-postgresql---flexible-server"></a>Journaux dans Azure Database pour PostgreSQL - Serveur flexible



Azure Database pour PostgreSQL vous permet de configurer et d’accéder aux journaux standard de Postgres. Les journaux d’activité peuvent servir à identifier, résoudre et réparer les erreurs de configuration et les problèmes de performances. Les informations de journalisation que vous pouvez configurer et auxquelles vous pouvez accéder incluent les erreurs, les informations de requête, les enregistrements de nettoyage automatique, les connexions et les points de contrôle. (L’accès aux journaux d’activité des transactions n’est pas disponible).

L’enregistrement d’audit est mis à disposition par le biais d’une extension Postgres, `pgaudit`. Pour plus d’informations, consultez l’article [Concepts d’audit](concepts-audit.md).

## <a name="configure-logging"></a>Configuration de la journalisation

Vous pouvez configurer la journalisation standard Postgres sur votre serveur avec les paramètres de journalisation. Pour en savoir plus sur les paramètres de journal Postgres, consultez les sections [Quand journaliser](https://www.postgresql.org/docs/current/runtime-config-logging.html#RUNTIME-CONFIG-LOGGING-WHEN) et [Que journaliser](https://www.postgresql.org/docs/current/runtime-config-logging.html#RUNTIME-CONFIG-LOGGING-WHAT) de la documentation Postgres. La plupart des paramètres de journalisation Postgres, mais pas tous, peuvent être configurés dans Azure Database pour PostgreSQL.

Pour savoir comment configurer les paramètres Azure Database pour PostgreSQL, consultez la [documentation du portail](howto-configure-server-parameters-using-portal.md) ou la [documentation de la CLI](howto-configure-server-parameters-using-cli.md).

> [!NOTE]
> La configuration d’un volume élevé de journaux, par exemple la journalisation d’instructions, peut créer une surcharge significative sur les performances. 

## <a name="accessing-logs"></a>Accès aux journaux d’activité

Azure Database pour PostgreSQL est intégré aux journaux des paramètres de diagnostic d’Azure Monitor. Les paramètres de diagnostic vous permettent d’envoyer vos journaux Postgres au format JSON aux journaux Azure Monitor à des fins d’analyse et d’alerte, à Event Hubs pour la diffusion en continu et au stockage Azure pour l’archivage. 

### <a name="log-format"></a>Format de journal

Le tableau suivant décrit les champs du type **PostgreSQLLogs**. En fonction du point de terminaison de sortie choisi, les champs et l’ordre dans lequel ils apparaissent peuvent varier. 

|**Champ** | **Description** |
|---|---|
| TenantId | Votre ID d’abonné |
| SourceSystem | `Azure` |
| TimeGenerated [UTC] | Horodatage du moment où le journal a été enregistré en UTC |
| Type | Type de journal. Toujours `AzureDiagnostics` |
| SubscriptionId | GUID de l’abonnement auquel appartient le serveur |
| ResourceGroup | Nom du groupe de ressources auquel le serveur appartient |
| ResourceProvider | Nom du fournisseur de ressources. Toujours `MICROSOFT.DBFORPOSTGRESQL` |
| ResourceType | `Servers` |
| ResourceId | URI de ressource |
| Ressource | Nom du serveur |
| Category | `PostgreSQLLogs` |
| NomOpération | `LogEvent` |
| errorLevel | Niveau de journalisation, par exemple : LOG, ERROR, NOTICE |
| Message | Message de journal principal | 
| Domain | Version du serveur, par exemple : postgres-10 |
| Detail | Message du journal secondaire (le cas échéant) |
| ColumnName | Nom de la colonne (le cas échéant) |
| SchemaName | Nom du schéma (le cas échéant) |
| DatatypeName | Nom du type de données (le cas échéant) |
| LogicalServerName | Nom du serveur | 
| _ResourceId | URI de ressource |
| Préfixe | Préfixe de la ligne de journal |


## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment [Configurer et accéder aux journaux](howto-configure-and-access-logs.md).
- En savoir plus sur la [tarification Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).
- En savoir plus sur les [journaux d'audit](concepts-audit.md)
