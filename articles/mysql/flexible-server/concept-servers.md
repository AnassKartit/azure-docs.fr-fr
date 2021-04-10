---
title: Concepts de serveur - Serveur flexible Azure Database pour MySQL
description: Cette rubrique propose des considérations et des instructions relatives à l’utilisation de serveurs flexibles Azure Database pour MySQL
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: a7978410dbe28a5da5dae81cb380d118fe13a159
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/23/2021
ms.locfileid: "104869376"
---
# <a name="server-concepts-in-azure-database-for-mysql-flexible-server-preview"></a>Concepts de serveur dans le serveur flexible (préversion) Azure Database pour MySQL

> [!IMPORTANT] 
> Le serveur flexible Azure Database pour MySQL est actuellement en préversion publique.

Cet article présente des considérations et des instructions relatives à l’utilisation de serveurs flexibles Azure Database pour MySQL.

## <a name="what-is-an-azure-database-for-mysql-flexible-server"></a>Qu’est-ce qu’un serveur flexible Azure Database pour MySQL ?

Le serveur flexible Azure Database pour MySQL est un service de base de données complètement managé exécutant la version Communauté de MySQL. En général, le service est conçu pour fournir des personnalisations en termes de flexibilité et configuration suivant les besoins de l’utilisateur. Il s’agit de la structure de serveur MySQL que vous connaissez peut-être en local. Plus précisément, le serveur flexible est géré, fournit des performances prêtes à l’emploi, une facilité de gestion et un contrôle du serveur améliorés, et expose l’accès et les fonctionnalités au niveau du serveur.

Un serveur flexible Azure Database pour MySQL :

- est créé dans un abonnement Azure ;
- représente la ressource parente des bases de données ;
- autorise la configuration MySQL exposée par le biais des paramètres du serveur (lien vers les concepts de paramètres du serveur) ;
- effectue des sauvegardes automatisées et prend en charge les limites de restauration dans le temps ;
- fournit un espace de noms aux bases de données ;
- constitue un conteneur avec une sémantique à durée de vie longue (la suppression d’un serveur entraîne la suppression des bases de données autonomes qu’il contient) ;
- colocalise les ressources d’une région ;
- prise en charge du calendrier de maintenance des serveurs fournis par le client ;
- possibilité de déployer des serveurs flexibles dans une installation redondante interzone pour améliorer la haute disponibilité ;
- fournit une intégration du réseau virtuel pour l’accès au serveur de base de données ;
- offre un moyen de réduire les coûts en suspendant le serveur flexible lorsqu’il n’est pas utilisé ;
- fournit l’étendue des stratégies de gestion qui s’appliquent à ses bases de données : connexions, pare-feu, utilisateurs, rôles, configurations, etc. ;
- Prend en charge les versions majeures de MySQL 5.7 et MySQL 8.0. Pour plus d’informations, consultez la page [Versions prises en charge du moteur Azure Database pour MySQL](./../concepts-supported-versions.md).

Dans un serveur flexible Azure Database pour MySQL, vous pouvez créer une ou plusieurs bases de données. Vous pouvez choisir de créer une seule base de données par serveur pour utiliser toutes les ressources, ou de créer plusieurs bases de données pour partager les ressources. Les tarifs sont structurés par serveur, en fonction de la configuration du niveau de calcul, des vCores et du stockage (Go). Pour plus d’informations, consultez [Calcul et stockage](./concepts-compute-storage.md).

## <a name="stopstart-an-azure-database-for-mysql-flexible-server"></a>Arrêter/Démarrer un serveur flexible Azure Database pour MySQL

Le serveur flexible Azure Database pour MySQL vous donne la possibilité d’**arrêter** le serveur en cas de non utilisation et de le **démarrer** lorsque vous reprenez l’activité. Cela permet essentiellement de réduire les coûts sur les serveurs de base de données et de payer uniquement la ressource en cours d’utilisation. Cela devient encore plus important pour les charges de travail de développement et de test, et lorsque vous utilisez uniquement le serveur pour une partie de la journée. Lorsque vous arrêtez le serveur, toutes les connexions actives sont supprimées. Plus tard, lorsque vous souhaitez remettre le serveur en ligne, vous pouvez utiliser le [portail Azure](how-to-stop-start-server-portal.md) ou l’interface de ligne de commande.

Lorsque le serveur se trouve à l’état **Arrêté**, le calcul du serveur n’est pas facturé. Toutefois, le stockage continue à être facturé tant que le stockage du serveur est conservé pour s’assurer que les fichiers de données sont disponibles lors du redémarrage du serveur.

> [!IMPORTANT]
> Lorsque vous **arrêtez** le serveur, il reste dans cet état durant les 7 jours suivants. Si vous ne le **démarrez** pas manuellement pendant cette période, le serveur sera automatiquement démarré à la fin des 7 jours. Vous pouvez choisir de l’**arrêter** de nouveau si vous n’utilisez pas le serveur.

Pendant l’arrêt du serveur, aucune opération de gestion ne peut être effectuée sur le serveur. Afin de modifier les paramètres de configuration sur le serveur, vous devez [démarrer le serveur](how-to-stop-start-server-portal.md). Reportez-vous aux [contraintes liées à l’arrêt et au démarrage](./concepts-limitations.md#stopstart-operation).

## <a name="how-do-i-manage-a-server"></a>Comment gérer un serveur ?

Vous pouvez gérer la création, la suppression, la configuration des paramètres du serveur (my.cnf), la mise à l’échelle, la mise en réseau, la sécurité, la haute disponibilité, la sauvegarde et la restauration, ainsi que la surveillance de votre serveur flexible Azure Database pour MySQL à l’aide du [portail Azure](./quickstart-create-server-portal.md) ou d’[Azure CLI](./quickstart-create-server-cli.md). En outre, les procédures stockées suivantes disponibles dans Azure Database pour MySQL permettent d’effectuer certaines tâches d’administration de base de données requises parce que le privilège de SUPER utilisateur n’est pas pris en charge sur le serveur.

|**Nom de la procédure stockée**|**Paramètres d’entrée**|**Paramètres de sortie**|**Remarque sur l’utilisation**|
|-----|-----|-----|-----|
|*mysql.az_kill*|processlist_id|N/A|Équivaut à la commande [`KILL CONNECTION`](https://dev.mysql.com/doc/refman/8.0/en/kill.html). Met fin à la connexion associée au processlist_id fourni après avoir mis fin à l’exécution de toute instruction exécutée actuellement par la connexion.|
|*mysql.az_kill_query*|processlist_id|N/A|Équivaut à la commande [`KILL QUERY`](https://dev.mysql.com/doc/refman/8.0/en/kill.html). Met fin à l’instruction exécutée actuellement par la connexion. Laisse la connexion active.|
|*mysql.az_load_timezone*|N/A|N/A|Charge les [tables de fuseau horaire](../howto-server-parameters.md#working-with-the-time-zone-parameter) pour permettre d’attribuer des valeurs nommées au paramètre `time_zone` (par exemple, « US/Pacific »).|


## <a name="next-steps"></a>Étapes suivantes

-   En savoir plus sur [Créer un serveur](./quickstart-create-server-portal.md)
-   En savoir plus sur [Supervision et alertes](./how-to-alert-on-metric.md)

