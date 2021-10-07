---
title: Guide pratique pour administrer le Cache Azure pour Redis
description: Découvrez comment effectuer des tâches d’administration comme le redémarrage et la planification de mises à jour pour le Cache Azure pour Redis.
author: curib
ms.service: cache
ms.topic: conceptual
ms.date: 07/05/2017
ms.author: cauribeg
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: d4532b13389e85be2b506bdc433cf3dc650fdb38
ms.sourcegitcommit: c27f71f890ecba96b42d58604c556505897a34f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2021
ms.locfileid: "129538882"
---
# <a name="how-to-administer-azure-cache-for-redis"></a>Guide pratique pour administrer le Cache Azure pour Redis

Cet article explique comment effectuer des tâches d’administration, telles que le [redémarrage](#reboot) et la [planification des mises à jour](#schedule-updates) de vos instances Azure Cache pour Redis.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="reboot"></a>Redémarrer

À gauche, le panneau **Redémarrer** vous permet de redémarrer un ou plusieurs nœuds de votre cache. Cette fonctionnalité de redémarrage vous permet de tester votre application pour garantir la résilience en cas de panne d’un nœud de cache.

:::image type="content" source="media/cache-administration/cache-administration-reboot-2.png" alt-text="Capture d’écran qui met en évidence l’option de menu Redémarrer":::

Sélectionnez les nœuds à relancer, puis sélectionnez **Redémarrer**.

:::image type="content" source="media/cache-administration/redis-cache-reboot-2.png" alt-text="Capture d’écran montrant les nœuds que vous pouvez redémarrer":::

Si vous avez un cache premium avec activation du clustering, vous pouvez sélectionner les partitions du cache à redémarrer.

:::image type="content" source="media/cache-administration/redis-cache-reboot-cluster-2.png" alt-text="capture d’écran des options de partition":::

Pour redémarrer un ou plusieurs nœuds de votre cache, sélectionnez les nœuds concernés, puis sélectionnez **Redémarrer**. Si vous disposez d’un cache Premium avec clustering, sélectionnez les partitions à redémarrer, puis sélectionnez **Redémarrer**. Après quelques minutes, les nœuds sélectionnés sont redémarrés et sont de nouveau en ligne.

L’impact sur les applications clientes varie selon les nœuds que vous redémarrez.

* **Maître** - lorsque le nœud principal est redémarré, Azure Cache pour Redis bascule sur le nœud réplica et le promeut au niveau principal. Pendant ce basculement, il peut y avoir un court intervalle pendant lequel les connexions au cache peuvent échouer.
* **Réplica** : lorsque le nœud de réplica est redémarré, il n’y a généralement aucun effet sur les clients du cache.
* **Principal et réplica** : lorsque les deux nœuds du cache sont redémarrés, vous perdez toutes les données dans le cache, et les connexions au cache échouent jusqu’à ce que le nœud principal soit de nouveau disponible. Si vous avez configuré la [persistance des données](cache-how-to-premium-persistence.md), la sauvegarde la plus récente est restaurée lorsque le cache repasse en ligne. En revanche, toutes les opérations d’écriture dans le cache qui se sont produites après la dernière sauvegarde seront perdues.
* **Nœuds d’un cache Premium avec activation du clustering** : lorsque vous redémarrez le ou les nœuds d’un cache Premium et que le clustering est activé, le comportement des nœuds sélectionnés est le même que lorsque vous redémarrez un ou plusieurs nœuds correspondants d’un cache non cluster.

## <a name="reboot-faq"></a>Forum aux questions sur le redémarrage

* [Quel nœud dois-je redémarrer pour tester mon application ?](#which-node-should-i-reboot-to-test-my-application)
* [Est-il possible de redémarrer le cache pour effacer les connexions client ?](#can-i-reboot-the-cache-to-clear-client-connections)
* [Vais-je perdre les données dans mon cache si je redémarre ?](#will-i-lose-data-from-my-cache-if-i-do-a-reboot)
* [Est-il possible de redémarrer mon cache à l’aide de PowerShell, de l’interface de ligne de commande ou d’autres outils de gestion ?](#can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools)

### <a name="which-node-should-i-reboot-to-test-my-application"></a>Quel nœud dois-je redémarrer pour tester mon application ?

Pour tester la résilience de votre application en cas de défaillance du nœud principal de votre cache, redémarrez le nœud **Master** . Pour tester la résilience de votre application en cas de défaillance du nœud réplica, redémarrez le nœud **réplica**. Pour tester la résilience de votre application en cas de défaillance totale de votre cache, redémarrez les **deux** nœuds.

### <a name="can-i-reboot-the-cache-to-clear-client-connections"></a>Est-il possible de redémarrer le cache pour effacer les connexions client ?

Oui, toutes les connexions clientes sont effacées si vous redémarrez le cache. Le redémarrage est pertinent lorsque chaque connexion cliente est utilisée en raison d’une erreur logique ou d’un bogue dans l’application cliente. Chaque niveau tarifaire a différentes [limites de connexion client](cache-configure.md#default-redis-server-configuration) pour les différentes tailles. Une fois ces limites sont atteintes, aucune autre connexion client supplémentaire n’est acceptée. Redémarrer le cache permet d’effacer toutes les connexions client.

> [!IMPORTANT]
> Si vous réinitialisez votre cache pour effacer les connexions client, StackExchange.Redis se reconnecte automatiquement une fois le nœud Redis en ligne. Si le problème sous-jacent n’est pas résolu, les connexions client peuvent continuer à être utilisées.
>
>

### <a name="will-i-lose-data-from-my-cache-if-i-do-a-reboot"></a>Vais-je perdre les données dans mon cache si je redémarre ?

Si vous redémarrez à la fois les nœuds **Maître** et **Réplica**, toutes les données du cache (ou dans cette partition lorsque vous utilisez un cache Premium avec le clustering activé) risquent d’être perdues. Elles peuvent toutefois ne pas l’être. Si vous avez configuré la [persistance des données](cache-how-to-premium-persistence.md), la sauvegarde la plus récente est restaurée lorsque le cache repasse en ligne. Par contre, toutes les opération d’écriture dans le cache qui se sont produites après la sauvegarde seront perdues.

Si vous ne redémarrez qu’un seul nœud, les données ne sont généralement pas perdues, mais il y a un risque qu’elles le soient quand même. Par exemple, si le nœud principal est redémarré et qu’une opération d’écriture dans le cache est en cours, les données écrites dans le cache sont perdues. Vous êtes également susceptible de perdre des données lorsque vous redémarrez un nœud et que l’autre nœud se met hors service en même temps en raison d’une défaillance. Pour plus d’informations sur les causes possibles d’une perte de données, voir [Qu’est-il arrivé à mes données dans Redis ?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md)

### <a name="can-i-reboot-my-cache-using-powershell-cli-or-other-management-tools"></a>Est-il possible de redémarrer mon cache à l’aide de PowerShell, de l’interface de ligne de commande ou d’autres outils de gestion ?

Oui. Pour connaître les instructions PowerShell, voir [Redémarrer un Cache Azure pour Redis](cache-how-to-manage-redis-cache-powershell.md#to-reboot-an-azure-cache-for-redis).

## <a name="schedule-updates"></a>Planifier les mises à jour

Sur la gauche, **Planifier les mises à jour** vous permet de choisir une fenêtre de maintenance pour votre instance de cache. Une fenêtre de maintenance vous permet de contrôler le ou les jours et l’heure ou les heures d’une semaine où la ou les machines virtuelles hébergeant votre cache peuvent être mises à jour. Le cache Azure Cache pour Redis s’efforce de démarrer et de terminer la mise à jour du logiciel serveur Redis dans la fenêtre de temps que vous avez définie.

> [!NOTE]
> La fenêtre de maintenance s’applique aux mises à jour du serveur Redis, ainsi qu’aux mises à jour du système d’exploitation des machines virtuelles qui hébergent le cache. La fenêtre de maintenance ne s’applique pas aux mises à jour du système d’exploitation hôte pour les hôtes qui hébergent des machines virtuelles de cache ou d’autres composants Azure Networking. Dans de rares cas, lorsque des caches sont hébergés sur des modèles plus anciens (si votre cache se trouve sur un modèle plus ancien, le nom DNS du cache correspondra au suffixe « cloudapp.net », « chinacloudapp.cn », « usgovcloudapi.net » ou « cloudapi.de »), la fenêtre de maintenance ne s’appliquera pas non plus aux mises à jour du système d’exploitation invité.
>
> Actuellement, aucune option n’est disponible pour configurer un redémarrage ou des mises à jour planifiées d’un cache de niveau Entreprise.
>

:::image type="content" source="media/cache-administration/redis-schedule-updates-2.png" alt-text="Capture d’écran montrant Planifier les mises à jour":::

Pour spécifier une fenêtre de maintenance, cochez les jours qui vous intéressent et indiquez l’heure de début de la fenêtre de maintenance pour chaque jour. Ensuite, sélectionnez **OK**. L’heure de la maintenance est au format UTC.

La fenêtre de maintenance minimale et par défaut pour les mises à jour est de cinq heures. Cette valeur n’est pas configurable à partir du portail Azure, mais vous pouvez la configurer dans PowerShell à l’aide du paramètre `MaintenanceWindow` de l’applet de commande [New-AzRedisCacheScheduleEntry](/powershell/module/az.rediscache/new-azrediscachescheduleentry). Pour plus d’informations, voir [Puis-je gérer les mises à jour planifiées à l’aide de PowerShell, de l’interface de ligne de commande ou de tout autre outil de gestion ?](#can-i-manage-scheduled-updates-using-powershell-cli-or-other-management-tools)

## <a name="schedule-updates-faq"></a>Forum aux questions de la planification des mises à jour

* [Quand les mises à jour sont-elles effectuées si je n’utilise pas la fonctionnalité de planification des mises à jour ?](#when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature)
* [Quels types de mises à jour sont exécutés au cours de la fenêtre de maintenance planifiée ?](#what-type-of-updates-are-made-during-the-scheduled-maintenance-window)
* [Puis-je gérer les mises à jour planifiées à l’aide de PowerShell, de l’interface de ligne de commande ou de tout autre outil de gestion ?](#can-i-manage-scheduled-updates-using-powershell-cli-or-other-management-tools)
* [Une mise à jour qui est couverte et gérée par la fonctionnalité « Mises à jour planifiées » peut-elle se produire en dehors de la fenêtre « Mises à jour planifiées » ?](#can-an-update-that-is-covered-and-managed-by-the-scheduled-updates-feature-happen-outside-the-scheduled-updates-window)

### <a name="when-do-updates-occur-if-i-dont-use-the-schedule-updates-feature"></a>Quand les mises à jour sont-elles effectuées si je n’utilise pas la fonctionnalité de planification des mises à jour ?

Si vous ne spécifiez pas une fenêtre de maintenance, les mises à jour peuvent être effectuées à tout moment.

### <a name="what-type-of-updates-are-made-during-the-scheduled-maintenance-window"></a>Quels types de mises à jour sont exécutés au cours de la fenêtre de maintenance planifiée ?

Seules les mises à jour du serveur Redis sont exécutées au cours de la fenêtre de maintenance planifiée. La fenêtre de maintenance ne s’applique pas aux mises à jour d’Azure ni aux mises à jour du système d’exploitation de la machine virtuelle.

### <a name="can-i-manage-scheduled-updates-using-powershell-cli-or-other-management-tools"></a>Puis-je gérer les mises à jour planifiées à l’aide de PowerShell, de l’interface de ligne de commande ou de tout autre outil de gestion ?

Oui. Vous pouvez gérer vos mises à jour planifiées à l’aide des cmdlets de commande PowerShell suivantes :

* [Get-AzRedisCachePatchSchedule](/powershell/module/az.rediscache/get-azrediscachepatchschedule)
* [New-AzRedisCachePatchSchedule](/powershell/module/az.rediscache/new-azrediscachepatchschedule)
* [New-AzRedisCacheScheduleEntry](/powershell/module/az.rediscache/new-azrediscachescheduleentry)
* [Remove-AzRedisCachePatchSchedule](/powershell/module/az.rediscache/remove-azrediscachepatchschedule)

### <a name="can-an-update-that-is-covered-and-managed-by-the-scheduled-updates-feature-happen-outside-the-scheduled-updates-window"></a>Une mise à jour qui est couverte et gérée par la fonctionnalité Mises à jour planifiées peut-elle se produire en dehors de la fenêtre Mises à jour planifiées ?

Oui. En général, les mises à jour ne sont pas appliquées en dehors de la fenêtre Mises à jour planifiées configurée. De rares mises à jour de sécurité critiques peuvent être appliquées en dehors de la planification de mises à jour correctives, dans le cadre de notre stratégie de sécurité.

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur les fonctionnalités d’Azure Cache pour Redis.

* [Niveaux de service Azure Cache pour Redis](cache-overview.md#service-tiers)
