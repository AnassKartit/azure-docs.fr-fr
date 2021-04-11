---
title: Configurer et accéder aux journaux - Serveur flexible - Azure Database pour PostgreSQL
description: Comment accéder aux journaux de base de données Azure Database pour PostgreSQL - Serveur flexible
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 59a2ddcc68a7c5a3b6fa3a3b315f4294d1625204
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105607403"
---
# <a name="configure-and-access-logs-in-azure-database-for-postgresql---flexible-server"></a>Configurer et accéder aux journaux dans Azure Database pour PostgreSQL - Serveur flexible

> [!IMPORTANT]
> Azure Database pour PostgreSQL - Serveur flexible est en préversion

Les journaux PostgreSQL sont disponibles sur chaque nœud d’un serveur flexible. Vous pouvez envoyer des journaux à un serveur de stockage ou à un service d’analytique. Les journaux d’activité peuvent servir à identifier, résoudre et réparer les erreurs de configuration et les problèmes de performances.

## <a name="configure-diagnostic-settings"></a>Configurer les paramètres de diagnostic

Vous pouvez activer les paramètres de diagnostic pour votre serveur Postgres à l’aide du portail Azure, de l’interface CLI, de l’API REST et de PowerShell. La catégorie de journal à sélectionner est **PostgreSQLLogs**.

Pour activer les journaux de ressources à l’aide du portail Azure :

1. Sur le portail, accédez à *Paramètres de diagnostic* dans le menu de navigation de votre serveur Postgres.
   
2. Sélectionnez *Ajouter le paramètre de diagnostic*.
   :::image type="content" source="media/howto-logging/diagnostic-settings.png" alt-text="Ajouter des paramètres de diagnostic - Bouton":::

3. Donnez un nom à ce paramètre. 

4. Sélectionnez le point de terminaison de votre choix (compte de stockage, hub d’événements, analytique des journaux). 

5. Sélectionnez le type de journal **PostgreSQLLogs**.
   :::image type="content" source="media/howto-logging/diagnostic-create-setting.png" alt-text="Choisir des journaux PostgreSQL":::

7. Enregistrez votre paramètre.

Pour activer les journaux de ressources avec PowerShell, l’interface CLI ou l’API REST, consultez l’article [Paramètres de diagnostic](../../azure-monitor/essentials/diagnostic-settings.md).

### <a name="access-resource-logs"></a>Accéder aux journaux de ressources

La façon dont vous accédez aux journaux dépend du point de terminaison que vous choisissez. Pour le stockage Azure, consultez l’article [Compte de stockage des journaux](../../azure-monitor/essentials/resource-logs.md#send-to-azure-storage). Pour Event Hubs, consultez l’article [Diffusion des journaux Azure](../../azure-monitor/essentials/resource-logs.md#send-to-azure-event-hubs).

Pour les journaux Azure Monitor, les journaux sont envoyés à l’espace de travail que vous avez sélectionné. Les journaux Postgres utilisent le mode de collecte **AzureDiagnostics**, pour qu’ils puissent être interrogés à partir de la table AzureDiagnostics. Les champs de la table sont décrits ci-dessous. En savoir plus sur l’interrogation et la génération d’alertes dans la vue d’ensemble [Interroger les journaux Azure Monitor](../../azure-monitor/logs/log-query-overview.md).

Voici des requêtes que vous pouvez essayer pour commencer. Vous pouvez configurer des alertes basées sur les requêtes.

Rechercher tous les journaux Postgres pour un serveur particulier au cours du dernier jour

```kusto
AzureDiagnostics
| where LogicalServerName_s == "myservername"
| where Category == "PostgreSQLLogs"
| where TimeGenerated > ago(1d) 
```

Rechercher toutes les tentatives de connexion autres que localhost

```kusto
AzureDiagnostics
| where Message contains "connection received" and Message !contains "host=127.0.0.1"
| where Category == "PostgreSQLLogs" and TimeGenerated > ago(6h)
```

La requête ci-dessus affiche les résultats au cours des six dernières heures de toutes les journalisations de serveur PostgreSQL dans cet espace de travail.

## <a name="next-steps"></a>Étapes suivantes

- [Bien démarrer avec les requêtes Log Analytics](../../azure-monitor/logs/log-analytics-tutorial.md)
- En savoir plus sur les [hubs d’événements Azure](../../event-hubs/event-hubs-about.md)