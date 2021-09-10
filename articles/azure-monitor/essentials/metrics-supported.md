---
title: Métriques prises en charge par Azure Monitor par type de ressource
description: Liste des métriques disponibles pour chaque type de ressource avec Azure Monitor.
author: rboucher
services: azure-monitor
ms.topic: reference
ms.date: 08/04/2021
ms.author: robb
ms.openlocfilehash: 4975d83773edba94676b7beeff166c6edb86248d
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2021
ms.locfileid: "122533337"
---
# <a name="supported-metrics-with-azure-monitor"></a>Métriques prises en charge avec Azure Monitor

> [!NOTE]
> Cette liste est en grande partie générée automatiquement. Toute modification apportée à cette liste via GitHub peut être remplacée sans avertissement. Pour plus d’informations sur la façon d’effectuer des mises à jour permanentes, contactez l’auteur de cet article.

Azure Monitor offre plusieurs moyens d’interagir avec les métriques, y compris en créant des graphiques dans le portail, en y accédant via l’API REST ou en envoyant des requêtes avec PowerShell ou l’interface CLI. 

Cet article est une liste complète de toutes les métriques de plateforme (c’est-à-dire collectées automatiquement) actuellement disponibles avec le pipeline de métriques consolidées d’Azure Monitor. Les mesures modifiées ou ajoutées après la date figurant en haut de cet article peuvent ne pas encore s’afficher ci-dessous. Pour interroger cette liste de métriques et y accéder programmatiquement, veuillez utiliser [2018-01-01 api-version](/rest/api/monitor/metricdefinitions). D’autres métriques ne figurant pas dans cette liste peuvent être disponibles dans le portail ou via les API héritées.

Les métriques sont organisées par fournisseurs de ressources et par type de ressource. Pour obtenir la liste des services ainsi que des fournisseurs de ressources et types qui leur appartiennent, consultez [Fournisseurs de ressources pour les services Azure](../../azure-resource-manager/management/azure-services-resource-providers.md).  

## <a name="exporting-platform-metrics-to-other-locations"></a>Exportation des métriques de plateforme vers d’autres emplacements

Vous pouvez exporter les métriques de plateforme à partir du pipeline Azure Monitor vers d’autres emplacements de l’une des deux façons suivantes.
1. Utilisation de l’[API REST pour les métriques](/rest/api/monitor/metrics/list)
2. Utilisez les [paramètres de diagnostic](../essentials/diagnostic-settings.md) pour acheminer les métriques de plateforme vers les emplacements suivants : 
    - Stockage Azure
    - Journaux Azure Monitor (et donc Log Analytics)
    - Event Hubs, qui permet de les transmettre à des systèmes autres que Microsoft 

Si le recours aux paramètres de diagnostic constitue le moyen le plus simple d’acheminer les métriques, certaines limitations s’appliquent : 

- **Métriques non exportables** : toutes les métriques sont exportables à l’aide de l’API REST, mais certaines ne peuvent pas être exportées à l’aide des paramètres de diagnostic en raison de subtilités dans le système backend Azure Monitor. La colonne *Exportable au moyen des paramètres de diagnostic* des tableaux ci-dessous indique quelles métriques peuvent être exportées de cette façon.  

- **Métriques multidimensionnelles** : il n’est pas possible à l’heure actuelle d’envoyer des métriques multidimensionnelles à d’autres emplacements par le biais des paramètres de diagnostic. Les métriques à plusieurs dimensions sont exportées en tant que métriques dimensionnelles uniques aplaties, puis agrégées dans les valeurs de la dimension. *Par exemple* : La métrique « Messages entrants » sur un Event Hub peut être examinée et représentée sur un niveau par file d’attente. Toutefois, lors de l’exportation via les paramètres de diagnostic, la métrique est représentée sous la forme de tous les messages entrants, dans toutes les files d’attente de l’Event Hub.

## <a name="guest-os-and-host-os-metrics"></a>Métriques du système d’exploitation invité et du système d’exploitation hôte

> [!WARNING]
> Les métriques du système d’exploitation invité (SE invité) qui s’exécutent dans les Machines virtuelles Azure, Service Fabric et les Services cloud ne sont **PAS** répertoriées ici. Les métriques du SE invité doivent être collectées au moyen du ou des agents qui s’exécutent sur ou dans le cadre du SE invité.  Parmi ces métriques, on peut citer les compteurs de performances qui effectuent le suivi du pourcentage de processeur invité et de l’utilisation de la mémoire, fréquemment utilisés pour la mise à l’échelle automatique et la génération d’alertes. 
>
> **Les métriques du SE hôte SONT disponibles et répertoriées ci-dessous.** Ce ne sont pas les mêmes. Les métriques du SE hôte sont liées à la session Hyper-V qui héberge la session du SE invité. 

> [!TIP]
> Il est recommandé d’utiliser et de configurer l’agent Azure Monitor pour envoyer les métriques de performance du SE invité dans la base de données de métriques Azure Monitor où sont stockées les métriques de la plateforme. L’agent achemine les métriques du SE invité via l’API de [métriques personnalisées](../essentials/metrics-custom-overview.md). Vous pouvez alors représenter ces métriques (par exemple, les métriques de plateforme) sous forme graphique, générer des alertes, etc. En guise d’alternative ou en plus, vous pouvez envoyer les métriques du SE invité aux journaux d’activité d’Azure Monitor à l’aide du même agent. Vous pouvez alors interroger ces métriques en association avec d’autres types de données en utilisant Log Analytics. 

L’agent Azure Monitor remplace l’extension Diagnostics Azure et l’agent Log Analytics qui étaient précédemment utilisés pour ce routage. Pour obtenir des informations complémentaires importantes, consultez [Vue d’ensemble des agents de surveillance](../agents/agents-overview.md).

## <a name="table-formatting"></a>Mise en forme des tableaux

> [!IMPORTANT] 
> Dans cette dernière mise à jour a été ajoutée une nouvelle colonne ; les métriques ont par ailleurs été réorganisées par ordre alphabétique. En raison de ces informations complémentaires, une barre de défilement horizontale est susceptible d’apparaître en bas des tableaux ci-dessous, en fonction de la largeur de votre fenêtre de navigateur. Si vous pensez qu’il manque des informations, utilisez la barre de défilement pour afficher l’intégralité du tableau.


## <a name="microsoftaadiamazureadmetrics"></a>microsoft.aadiam/azureADMetrics

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ThrottledRequests|Non|ThrottledRequests|Count|Average|Métrique de type azureADMetrics|Aucune dimension|


## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|CleanerCurrentPrice|Oui|Mémoire : prix actuel du nettoyage|Count|Average|Prix actuel de la mémoire, $/octet/temps, normalisé à 1000.|ServerResourceType|
|CleanerMemoryNonshrinkable|Oui|Mémoire : mémoire de nettoyage non réductible|Octets|Average|Quantité de mémoire, en octets, qui ne doit pas être vidée par le nettoyage en arrière-plan.|ServerResourceType|
|CleanerMemoryShrinkable|Oui|Mémoire : mémoire de nettoyage réductible|Octets|Average|Quantité de mémoire, en octets, qui doit être vidée par le nettoyage en arrière-plan.|ServerResourceType|
|CommandPoolBusyThreads|Oui|Threads : threads occupés du pool de commandes|Count|Average|Nombre de threads occupés dans le pool de threads de commandes.|ServerResourceType|
|CommandPoolIdleThreads|Oui|Threads : threads inactifs du pool de commandes|Count|Average|Nombre de threads inactifs dans le pool de threads de commandes.|ServerResourceType|
|CommandPoolJobQueueLength|Oui|Longueur de la file d’attente des travaux du pool de commandes|Count|Average|Nombre de travaux contenus dans la file d’attente du pool de threads de commandes.|ServerResourceType|
|CurrentConnections|Oui|Connexion : Connexions en cours|Count|Average|Nombre actuel de connexions client établies.|ServerResourceType|
|CurrentUserSessions|Oui|Sessions utilisateur actuelles|Count|Average|Nombre actuel de sessions utilisateur établies.|ServerResourceType|
|LongParsingBusyThreads|Oui|Threads : threads occupés d'analyse longue|Count|Average|Nombre de threads occupés dans le pool de threads d’analyse longue.|ServerResourceType|
|LongParsingIdleThreads|Oui|Threads : threads inactifs d'analyse longue|Count|Average|Nombre de threads inactifs dans le pool de threads d’analyse longue.|ServerResourceType|
|LongParsingJobQueueLength|Oui|Threads : longueur de la file d'attente des travaux d'analyse longue|Count|Average|Nombre de travaux contenus dans la file d’attente du pool de threads d’analyse longue.|ServerResourceType|
|mashup_engine_memory_metric|Oui|Mémoire du moteur M|Octets|Average|Utilisation de la mémoire par les processus de moteur mashup|ServerResourceType|
|mashup_engine_private_bytes_metric|Oui|Octets privés du moteur M|Octets|Average|Utilisation des octets privés par les processus de moteur mashup.|ServerResourceType|
|mashup_engine_qpu_metric|Oui|QPU du moteur M|Count|Average|Utilisation des QPU par les processus de moteur mashup|ServerResourceType|
|mashup_engine_virtual_bytes_metric|Oui|Octets virtuels du moteur M|Octets|Average|Utilisation des octets virtuels par les processus de moteur mashup.|ServerResourceType|
|memory_metric|Oui|Mémoire|Octets|Average|Mémoire. Plage de 0 à 25 Go pour S1, de 0 à 50 Go pour S2 et de 0 à 100 Go pour S4|ServerResourceType|
|memory_thrashing_metric|Oui|Vidage de mémoire|Pourcentage|Average|Vidage de mémoire moyenne.|ServerResourceType|
|MemoryLimitHard|Oui|Mémoire : limite inconditionnelle de la mémoire|Octets|Average|Limite de mémoire physique, du fichier de configuration.|ServerResourceType|
|MemoryLimitHigh|Oui|Mémoire : limite haute de la mémoire|Octets|Average|Limite de mémoire élevée, du fichier de configuration.|ServerResourceType|
|MemoryLimitLow|Oui|Mémoire : limite basse de la mémoire|Octets|Average|Limite de mémoire basse, du fichier de configuration.|ServerResourceType|
|MemoryLimitVertiPaq|Oui|Mémoire : limite de la mémoire VertiPaq|Octets|Average|Limite en mémoire, du fichier de configuration.|ServerResourceType|
|MemoryUsage|Oui|Mémoire : Utilisation de la mémoire|Octets|Average|Utilisation de la mémoire du processus serveur telle qu’utilisée dans le calcul du coût de la mémoire de nettoyage. Équivaut au compteur Process\PrivateBytes, plus la taille des données mappées en mémoire, en ignorant la mémoire mappée ou allouée par le moteur d’analyse de mémoire xVelocity (VertiPaq) dépassant la limite de mémoire du moteur xVelocity.|ServerResourceType|
|private_bytes_metric|Oui|Octets privés|Octets|Average|Octets privés.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Oui|Threads : threads des travaux d'E/S occupés du pool de traitement|Count|Average|Nombre de threads pour les travaux d’E/S en cours d’exécution dans le pool de threads de traitement.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Oui|Threads : threads autres que les threads d'E/S occupés du pool de traitement|Count|Average|Nombre de threads pour les travaux autres que d’E/S en cours d’exécution dans le pool de threads de traitement.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Oui|Threads : threads des travaux d'E/S inactifs du pool de traitement|Count|Average|Nombre de threads inactifs pour les travaux d’E/S le pool de threads de traitement.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Oui|Threads : threads autres que les threads d'E/S inactifs du pool de traitement|Count|Average|Nombre de threads inactifs le pool de threads de traitement dédiés aux travaux autres qu’E/S.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Oui|Threads : longueur de la file des travaux d'E/S du pool de traitement|Count|Average|Nombre de travaux d’E/S contenus dans la file d’attente du pool de threads de traitement.|ServerResourceType|
|ProcessingPoolJobQueueLength|Oui|Longueur de la file d’attente des travaux du pool de traitement|Count|Average|Nombre de travaux autres que d’E/S contenus dans la file d’attente du pool de threads de traitement.|ServerResourceType|
|qpu_metric|Oui|QPU|Count|Average|QPU. Plage de 0 à 100 pour S1, de 0 à 200 pour S2 et de 0 à 400 pour S4|ServerResourceType|
|QueryPoolBusyThreads|Oui|Threads occupés du pool de threads de requêtes|Count|Average|Nombre de threads occupés dans le pool de threads de requêtes.|ServerResourceType|
|QueryPoolIdleThreads|Oui|Threads : threads inactifs du pool de requêtes|Count|Average|Nombre de threads inactifs pour les travaux d’E/S le pool de threads de traitement.|ServerResourceType|
|QueryPoolJobQueueLength|Oui|Threads : longueur de la file d'attente des travaux du pool de requêtes|Count|Average|Nombre de travaux contenus dans la file d’attente du pool de threads de requêtes.|ServerResourceType|
|Quota|Oui|Mémoire : Quota|Octets|Average|Quota de mémoire actuel, en octets. Le quota de mémoire est également appelé réserve de mémoire ou d’allocation.|ServerResourceType|
|QuotaBlocked|Oui|Mémoire : quota bloqué|Count|Average|Nombre actuel de requêtes de quota qui sont bloquées en attendant la libération d’autres quotas de mémoire.|ServerResourceType|
|RowsConvertedPerSec|Oui|Traitement : lignes converties par seconde|CountPerSecond|Average|Taux de lignes converties lors du traitement.|ServerResourceType|
|RowsReadPerSec|Oui|Traitement : lignes lues par seconde|CountPerSecond|Average|Taux de lignes lues à partir de toutes les bases de données relationnelles.|ServerResourceType|
|RowsWrittenPerSec|Oui|Traitement : lignes écrites par seconde|CountPerSecond|Average|Taux de lignes écrites lors du traitement.|ServerResourceType|
|ShortParsingBusyThreads|Oui|Threads : threads occupés d'analyse courte|Count|Average|Nombre de threads occupés dans le pool de threads d’analyse courte.|ServerResourceType|
|ShortParsingIdleThreads|Oui|Threads : threads inactifs d'analyse courte|Count|Average|Nombre de threads inactifs dans le pool de threads d’analyse courte.|ServerResourceType|
|ShortParsingJobQueueLength|Oui|Threads : longueur de la file d'attente des travaux d'analyse courte|Count|Average|Nombre de travaux contenus dans la file d’attente du pool de threads d’analyse courte.|ServerResourceType|
|SuccessfullConnectionsPerSec|Oui|Connexions réussies par seconde|CountPerSecond|Average|Taux de connexions terminées réussies.|ServerResourceType|
|TotalConnectionFailures|Oui|Nombre total d’échecs de connexion|Count|Average|Total des échecs de tentatives de connexion.|ServerResourceType|
|TotalConnectionRequests|Oui|Nombre total de demandes de connexion|Count|Average|Nombre total de demandes de connexion. Il s’agit des arrivées.|ServerResourceType|
|VertiPaqNonpaged|Oui|Mémoire : mémoire non paginée VertiPaq|Octets|Average|Octets de mémoire verrouillée dans la plage de travail pour utilisation par le moteur en mémoire.|ServerResourceType|
|VertiPaqPaged|Oui|Mémoire : mémoire paginée VertiPaq|Octets|Average|Octets de mémoire paginée utilisée pour les données en mémoire.|ServerResourceType|
|virtual_bytes_metric|Oui|Octets virtuels|Octets|Average|Octets virtuels.|ServerResourceType|


## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BackendDuration|Oui|Durées des demandes back-end|Millisecondes|Average|Durée des demandes back-end, en millisecondes|Emplacement, nom d’hôte|
|Capacité|Oui|Capacité|Pourcentage|Average|Métrique d’utilisation pour le service ApiManagement. Remarque : Pour les SKU autres que Premium, l’agrégation « Max » affiche la valeur 0|Emplacement|
|Duration|Oui|Durée globale des demandes de passerelle|Millisecondes|Average|Durée globale des demandes de passerelle en millisecondes|Emplacement, nom d’hôte|
|EventHubDroppedEvents|Oui|Événements EventHub supprimés|Count|Total|Nombre d’événements ignorés car la limite de taille de la file d’attente a été atteinte|Emplacement|
|EventHubRejectedEvents|Oui|Événements EventHub rejetés|Count|Total|Nombre d’événements EventHub rejetés (configuration incorrecte ou non autorisée)|Emplacement|
|EventHubSuccessfulEvents|Oui|Événements EventHub réussis|Count|Total|Nombre d’événements EventHub réussis|Emplacement|
|EventHubThrottledEvents|Oui|Événements EventHub limités|Count|Total|Nombre d’événements EventHub limités|Emplacement|
|EventHubTimedoutEvents|Oui|Événements EventHub expirés|Count|Total|Nombre d'événements EventHub expirés|Emplacement|
|EventHubTotalBytesSent|Oui|Taille des événements EventHub|Octets|Total|Taille totale des événements EventHub en octets|Emplacement|
|EventHubTotalEvents|Oui|Nombre total d'événements EventHub|Count|Total|Nombre d’événements envoyés à EventHub|Emplacement|
|EventHubTotalFailedEvents|Oui|Événements EventHub non réussis|Count|Total|Nombre d’événements EventHub non réussis|Emplacement|
|FailedRequests|Oui|Demandes de la passerelle ayant échoué (déprécié)|Count|Total|Nombre de défaillances des demandes de la passerelle : utilisez une métrique de demande multi-dimension avec la dimension GatewayResponseCodeCategory à la place|Emplacement, nom d’hôte|
|NetworkConnectivity|Oui|État de connectivité réseau des ressources (préversion)|Count|Average|État de connectivité réseau des types de ressources dépendants du service Gestion des API|Location, ResourceType|
|OtherRequests|Oui|Autres demandes de la passerelle (déprécié)|Count|Total|Nombre d’autres demandes de la passerelle : utilisez une métrique de demande multi-dimension avec la dimension GatewayResponseCodeCategory à la place|Emplacement, nom d’hôte|
|Demandes|Oui|Demandes|Count|Total|Métriques de demande de la passerelle avec plusieurs dimensions|Location, Hostname, LastErrorReason, BackendResponseCode, GatewayResponseCode, BackendResponseCodeCategory, GatewayResponseCodeCategory|
|SuccessfulRequests|Oui|Demandes de la passerelle ayant abouti (déprécié)|Count|Total|Nombre de demandes de la passerelle ayant abouti : utilisez une métrique de demande multi-dimension avec la dimension GatewayResponseCodeCategory à la place|Emplacement, nom d’hôte|
|TotalRequests|Oui|Nombre total de demandes de la passerelle (déprécié)|Count|Total|Nombre de demandes de la passerelle : utilisez une métrique de demande multi-dimension avec la dimension GatewayResponseCodeCategory à la place|Emplacement, nom d’hôte|
|UnauthorizedRequests|Oui|Demandes de la passerelle non autorisées (déprécié)|Count|Total|Nombre de demandes de la passerelle non autorisées : utilisez une métrique de demande multi-dimension avec la dimension GatewayResponseCodeCategory à la place|Emplacement, nom d’hôte|


## <a name="microsoftappconfigurationconfigurationstores"></a>Microsoft.AppConfiguration/configurationStores

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|HttpIncomingRequestCount|Oui|HttpIncomingRequestCount|Count|Count|Nombre total de requêtes HTTP entrantes.|StatusCode, Authentication|
|HttpIncomingRequestDuration|Oui|HttpIncomingRequestDuration|Count|Average|Latence sur une requête HTTP.|StatusCode, Authentication|
|ThrottledHttpRequestCount|Oui|ThrottledHttpRequestCount|Count|Count|Requêtes HTTP limitées.|Aucune dimension|


## <a name="microsoftappplatformspring"></a>Microsoft.AppPlatform/Spring

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|active-timer-count|Oui|active-timer-count|Count|Average|Nombre de minuteurs actuellement actifs|Deployment, AppName, Pod|
|alloc-rate|Oui|alloc-rate|Octets|Average|Nombre d’octets alloués dans le tas managé|Deployment, AppName, Pod|
|AppCpuUsage|Oui|Utilisation du processeur de l’application |Pourcentage|Average|Utilisation récente du processeur pour l’application|Deployment, AppName, Pod|
|assembly-count|Oui|assembly-count|Count|Average|Nombre d’assemblys chargés|Deployment, AppName, Pod|
|cpu-usage|Oui|cpu-usage|Pourcentage|Average|% de temps d’utilisation du processeur par le processus|Deployment, AppName, Pod|
|current-requests|Oui|current-requests|Count|Average|Nombre total de demandes en traitement au cours de la durée de vie du processus|Deployment, AppName, Pod|
|exception-count|Oui|exception-count|Count|Total|Nombre d’exceptions|Deployment, AppName, Pod|
|failed-requests|Oui|failed-requests|Count|Average|Nombre total de demandes en échec au cours de la durée de vie du processus|Deployment, AppName, Pod|
|gc-heap-size|Oui|gc-heap-size|Count|Average|Taille totale du tas indiquée par le nettoyage de la mémoire (Mo)|Deployment, AppName, Pod|
|gen-0-gc-count|Oui|gen-0-gc-count|Count|Average|Nombre de nettoyages de la mémoire de génération 0|Deployment, AppName, Pod|
|gen-0-size|Oui|gen-0-size|Octets|Average|Taille du tas de génération 0|Deployment, AppName, Pod|
|gen-1-gc-count|Oui|gen-1-gc-count|Count|Average|Nombre de nettoyages de la mémoire de génération 1|Deployment, AppName, Pod|
|gen-1-size|Oui|gen-1-size|Octets|Average|Taille du tas de génération 1|Deployment, AppName, Pod|
|gen-2-gc-count|Oui|gen-2-gc-count|Count|Average|Nombre de nettoyages de la mémoire de génération 2|Deployment, AppName, Pod|
|gen-2-size|Oui|gen-2-size|Octets|Average|Taille du tas de génération 2|Deployment, AppName, Pod|
|jvm.gc.live.data.size|Oui|jvm.gc.live.data.size|Octets|Average|Taille du pool de mémoire d’ancienne génération après un GC complet|Deployment, AppName, Pod|
|jvm.gc.max.data.size|Oui|jvm.gc.max.data.size|Octets|Average|Taille maximale du pool de mémoire d’ancienne génération|Deployment, AppName, Pod|
|jvm.gc.memory.allocated|Oui|jvm.gc.memory.allocated|Octets|Maximale|Incrémenté pour une augmentation de la taille du pool de mémoire de nouvelle génération après un GC avant le suivant|Deployment, AppName, Pod|
|jvm.gc.memory.promoted|Oui|jvm.gc.memory.promoted|Octets|Maximale|Nombre d’augmentations positives de la taille du pool de mémoire d’ancienne génération avant l’application du GC jusqu’au terme de cette application|Deployment, AppName, Pod|
|jvm.gc.pause.total.count|Oui|jvm.gc.pause.total.count|Count|Total|Nombre d’interruptions du GC|Deployment, AppName, Pod|
|jvm.gc.pause.total.count|Oui|jvm.gc.pause.total.count|Millisecondes|Total|Durée totale de pause du GC|Deployment, AppName, Pod|
|jvm.memory.committed|Oui|jvm.memory.committed|Octets|Average|Mémoire affectée à JVM en octets|Deployment, AppName, Pod|
|jvm.memory.max|Oui|jvm.memory.max|Octets|Maximale|Quantité maximale de mémoire en octets utilisable pour la gestion de la mémoire|Deployment, AppName, Pod|
|jvm.memory.used|Oui|jvm.memory.used|Octets|Average|Mémoire d’application utilisée en octets|Deployment, AppName, Pod|
|loh-size|Oui|loh-size|Octets|Average|Taille du tas LOH|Deployment, AppName, Pod|
|monitor-lock-contention-count|Oui|monitor-lock-contention-count|Count|Average|Nombre de fois où il y a eu de la contention lors d’une tentative pour prendre le verrou du moniteur|Deployment, AppName, Pod|
|PodCpuUsage|Oui|Utilisation du processeur de l’application|Pourcentage|Average|Utilisation récente du processeur pour l’application|Deployment, AppName, Pod|
|PodMemoryUsage|Yes|Utilisation de la mémoire de l’application|Pourcentage|Average|Utilisation récente de la mémoire pour l’application|Deployment, AppName, Pod|
|process.cpu.usage|Oui|process.cpu.usage|Pourcentage|Average|Utilisation récente du processeur pour le processus JVM|Deployment, AppName, Pod|
|requests-per-second|Oui|requests-rate|Count|Average|Taux de demandes|Deployment, AppName, Pod|
|system.cpu.usage|Oui|system.cpu.usage|Pourcentage|Average|Utilisation récente du processeur pour l’ensemble du système|Deployment, AppName, Pod|
|threadpool-completed-items-count|Oui|threadpool-completed-items-count|Count|Average|Nombre d’éléments de travail terminés dans le pool de threads|Deployment, AppName, Pod|
|threadpool-queue-length|Oui|threadpool-queue-length|Count|Average|Longueur de la file d’attente des éléments de travail du pool de threads|Deployment, AppName, Pod|
|threadpool-thread-count|Oui|threadpool-thread-count|Count|Average|Nombre de threads du pool de threads|Deployment, AppName, Pod|
|time-in-gc|Oui|time-in-gc|Pourcentage|Average|Pourcentage de temps dans le nettoyage de la mémoire depuis le dernier nettoyage de la mémoire|Deployment, AppName, Pod|
|tomcat.global.error|Oui|tomcat.global.error|Count|Total|Erreur globale Tomcat|Deployment, AppName, Pod|
|tomcat.global.received|Oui|tomcat.global.received|Octets|Total|Nombre total d’octets reçus par Tomcat|Deployment, AppName, Pod|
|tomcat.global.request.avg.time|Oui|tomcat.global.request.avg.time|Millisecondes|Average|Durée moyenne des demandes Tomcat|Deployment, AppName, Pod|
|tomcat.global.request.max|Oui|tomcat.global.request.max|Millisecondes|Maximale|Durée maximale des demandes Tomcat|Deployment, AppName, Pod|
|tomcat.global.request.total.count|Oui|tomcat.global.request.total.count|Count|Total|Nombre total de demandes Tomcat|Deployment, AppName, Pod|
|tomcat.global.request.total.time|Oui|tomcat.global.request.total.time|Millisecondes|Total|Durée totale des requêtes Tomcat|Deployment, AppName, Pod|
|tomcat.global.sent|Oui|tomcat.global.sent|Octets|Total|Nombre total d’octets envoyés par Tomcat|Deployment, AppName, Pod|
|tomcat.sessions.active.current|Oui|tomcat.sessions.active.current|Count|Total|Nombre de sessions actives Tomcat|Deployment, AppName, Pod|
|tomcat.sessions.active.max|Oui|tomcat.sessions.active.max|Count|Total|Nombre maximal de sessions actives Tomcat|Deployment, AppName, Pod|
|tomcat.sessions.alive.max|Oui|tomcat.sessions.alive.max|Millisecondes|Maximale|Durée maximale des sessions actives Tomcat|Deployment, AppName, Pod|
|tomcat.sessions.created|Oui|tomcat.sessions.created|Count|Total|Nombre de sessions Tomcat créées|Deployment, AppName, Pod|
|tomcat.sessions.expired|Oui|tomcat.sessions.expired|Count|Total|Nombre de sessions Tomcat ayant expiré|Deployment, AppName, Pod|
|tomcat.sessions.rejected|Oui|tomcat.sessions.rejected|Count|Total|Nombre de sessions Tomcat rejetées|Deployment, AppName, Pod|
|tomcat.threads.config.max|Oui|tomcat.threads.config.max|Count|Total|Nombre de threads maximal de la configuration Tomcat|Deployment, AppName, Pod|
|tomcat.threads.current|Oui|tomcat.threads.current|Count|Total|Nombre de threads actuel de Tomcat|Deployment, AppName, Pod|
|total-requests|Oui|total-requests|Count|Average|Nombre total de demandes au cours de la durée de vie du processus|Deployment, AppName, Pod|
|working-set|Oui|working-set|Count|Average|Quantité de la plage de travail utilisée par le processus (Mo)|Deployment, AppName, Pod|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|TotalJob|Oui|Nombre total de travaux|Count|Total|Nombre total de travaux|Runbook, état|
|TotalUpdateDeploymentMachineRuns|Oui|Nombre total d’exécutions de déploiement de mises à jour de machines|Count|Total|Nombre total d'exécutions de déploiement de mises à jour logicielles de machines lors d'une exécution de déploiement de mises à jour logicielles|SoftwareUpdateConfigurationName, Status, TargetComputer, SoftwareUpdateConfigurationRunId|
|TotalUpdateDeploymentRuns|Oui|Nombre total d’exécutions de déploiement de mises à jour|Count|Total|Nombre total d’exécutions de déploiement de mises à jour logicielles|SoftwareUpdateConfigurationName, Status|

## <a name="microsoftavsprivateclouds"></a>Microsoft.AVS/privateClouds

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|CapacityLatest|Oui|Capacité totale du disque du magasin de données|Octets|Average|Capacité totale du disque dans le magasin de données|dsname|
|DiskUsedPercentage|Oui| Pourcentage du disque de magasin de données utilisé|Pourcentage|Average|Pourcentage du disque disponible utilisé dans le magasin de données|dsname|
|EffectiveCpuAverage|Oui|Percentage CPU|Pourcentage|Average|Pourcentage des ressources processeur utilisées dans le cluster|clustername|
|EffectiveMemAverage|Oui|Mémoire effective moyenne|Octets|Average|Quantité totale disponible de mémoire machine dans le cluster|clustername|
|OverheadAverage|Oui|Surcharge moyenne de la mémoire|Octets|Average|Mémoire physique de l’hôte consommée par l’infrastructure de virtualisation|clustername|
|TotalMbAverage|Oui|Mémoire totale moyenne|Octets|Average|Mémoire totale du cluster|clustername|
|UsageAverage|Oui|Utilisation moyenne de la mémoire|Pourcentage|Average|Utilisation de la mémoire sous forme de pourcentage du total configuré ou de la mémoire disponible|clustername|
|UsedLatest|Oui|Disque de magasin de données utilisé|Octets|Average|Quantité totale du disque utilisée dans le magasin de données|dsname|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|CoreCount|Non|Nombre de cœurs dédiés|Count|Total|Nombre total de cœurs dédiés dans le compte Batch|Aucune dimension|
|CreatingNodeCount|Non|Nombre de nœuds créés|Count|Total|Nombre de nœuds en cours de création|Aucune dimension|
|IdleNodeCount|Non|Nombre de nœuds inactifs|Count|Total|Le nombre de nœuds inactifs|Aucune dimension|
|JobDeleteCompleteEvent|Oui|Événements de fin de suppression de travail|Count|Total|Nombre total de travaux correctement supprimés.|jobId|
|JobDeleteStartEvent|Oui|Événements de démarrage de suppression de travail|Count|Total|Nombre total de travaux demandés pour être supprimés.|jobId|
|JobDisableCompleteEvent|Oui|Événements de fin de désactivation de travail|Count|Total|Nombre total de travaux correctement supprimés.|jobId|
|JobDisableStartEvent|Oui|Événements de démarrage de désactivation de travail|Count|Total|Nombre total de travaux demandés pour être désactivés.|jobId|
|JobStartEvent|Oui|Événements de démarrage de travail|Count|Total|Nombre total de travaux correctement démarrés.|jobId|
|JobTerminateCompleteEvent|Oui|Événements de fin de travail terminé|Count|Total|Nombre total de travaux correctement terminés.|jobId|
|JobTerminateCompleteEvent|Oui|Événements de démarrage de fin de travail|Count|Total|Nombre total de travaux demandés pour être terminés.|jobId|
|LeavingPoolNodeCount|Non|Nombre de nœuds quittant le pool|Count|Total|Le nombre de nœuds quittant le pool|Aucune dimension|
|LowPriorityCoreCount|Non|Nombre de cœurs à priorité basse|Count|Total|Nombre total de cœurs à priorité basse dans le compte Batch|Aucune dimension|
|OfflineNodeCount|Non|Nombre de nœuds hors ligne|Count|Total|Le nombre de nœuds hors ligne|Aucune dimension|
|PoolCreateEvent|Oui|Événements de création de pool|Count|Total|Nombre total de pools créés|poolId|
|PoolDeleteCompleteEvent|Oui|Événements de suppression de pool terminés|Count|Total|Nombre total de suppressions de pool terminées|poolId|
|PoolDeleteStartEvent|Oui|Événements de démarrage de suppression de pool|Count|Total|Nombre total de suppressions de pool ayant démarré|poolId|
|PoolResizeCompleteEvent|Oui|Événements de fin de redimensionnement de pool|Count|Total|Nombre total de redimensionnements de pool terminés|poolId|
|PoolResizeStartEvent|Oui|Événements de démarrage de redimensionnement de pool|Count|Total|Nombre total de redimensionnements de pool ayant démarré|poolId|
|PreemptedNodeCount|Non|Nombre de nœuds reportés|Count|Total|Nombre de nœuds reportés|Aucune dimension|
|RebootingNodeCount|Non|Nombre de nœuds en cours de redémarrage|Count|Total|Le nombre de nœuds en cours de redémarrage|Aucune dimension|
|ReimagingNodeCount|Non|Nombre de nœuds en cours de réimageage|Count|Total|Le nombre de nœuds en cours de réimageage|Aucune dimension|
|RunningNodeCount|Non|Nombre de nœuds en cours d’exécution|Count|Total|Le nombre de nœuds en cours d’exécution|Aucune dimension|
|StartingNodeCount|Non|Nombre de nœuds de départ|Count|Total|Nœuds de nœuds de départ|Aucune dimension|
|StartTaskFailedNodeCount|Non|Nombre de nœuds pour lesquels le démarrage d’une tâche a échoué|Count|Total|Le nombre de nœuds pour lesquels le démarrage d’une tâche a échoué|Aucune dimension|
|TaskCompleteEvent|Oui|Événements de fin de tâche|Count|Total|Le nombre total de tâches terminées|poolId, jobId|
|TaskFailEvent|Oui|Événements d’échec de tâches|Count|Total|Le nombre total de tâches terminées dans un état d’échec|poolId, jobId|
|TaskStartEvent|Oui|Événements de lancement de tâche|Count|Total|Nombre total de tâches ayant démarré|poolId, jobId|
|TotalLowPriorityNodeCount|Non|Nombre de nœuds à priorité basse|Count|Total|Nombre total de nœuds à priorité basse dans le compte Batch|Aucune dimension|
|TotalNodeCount|Non|Nombre de nœuds dédiés|Count|Total|Nombre total de nœuds dédiés dans le compte Batch|Aucune dimension|
|UnusableNodeCount|Non|Nombre de nœuds inutilisables|Count|Total|Le nombre de nœuds inutilisables|Aucune dimension|
|WaitingForStartTaskNodeCount|Non|Nombre de nœuds en attente de démarrage de tâche|Count|Total|Nombre de nœuds en attente de la fin d’une tâche de démarrage|Aucune dimension|

## <a name="microsoftbatchaiworkspaces"></a>Microsoft.BatchAI/workspaces

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Cœurs actifs|Oui|Cœurs actifs|Count|Average|Nombre de cœurs actifs|Scenario, ClusterName|
|Nœuds actifs|Oui|Nœuds actifs|Count|Average|Le nombre de nœuds en cours d’exécution|Scenario, ClusterName|
|Cœurs inactifs|Oui|Cœurs inactifs|Count|Average|Nombre de cœurs inactifs|Scenario, ClusterName|
|Nœuds inactifs|Oui|Nœuds inactifs|Count|Average|Le nombre de nœuds inactifs|Scenario, ClusterName|
|Travail effectué|Oui|Travail effectué|Count|Total|Nombre de travaux effectués|Scenario, ClusterName, ResultType|
|Travail envoyé|Oui|Travail envoyé|Count|Total|Nombre de travaux envoyés|Scenario, ClusterName|
|Cœurs sortants|Oui|Cœurs sortants|Count|Average|Nombre de cœurs sortants|Scenario, ClusterName|
|Nœuds sortants|Oui|Nœuds sortants|Count|Average|Nombre de nœuds sortants|Scenario, ClusterName|
|Cœurs réquisitionnés|Oui|Cœurs réquisitionnés|Count|Average|Nombre de cœurs réquisitionnés|Scenario, ClusterName|
|Nœuds reportés|Oui|Nœuds reportés|Count|Average|Nombre de nœuds reportés|Scenario, ClusterName|
|Pourcentage d’utilisation du quota|Oui|Pourcentage d’utilisation du quota|Count|Average|Pourcentage de quota utilisé|Scenario, ClusterName, VmFamilyName, VmPriority|
|Nombre total de cœurs|Oui|Nombre total de cœurs|Count|Average|Nombre total de cœurs|Scenario, ClusterName|
|Nombre total de nœuds|Oui|Nombre total de nœuds|Count|Average|Nombre total de nœuds|Scenario, ClusterName|
|Cœurs inutilisables|Oui|Cœurs inutilisables|Count|Average|Nombre de cœurs inutilisables|Scenario, ClusterName|
|Nœuds inutilisables|Oui|Nœuds inutilisables|Count|Average|Le nombre de nœuds inutilisables|Scenario, ClusterName|

## <a name="microsoftbingaccounts"></a>microsoft.bing/accounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BlockedCalls|Oui|Appels bloqués|Count|Total|Nombre d’appels ayant dépassé la limite de débit ou de quota|ApiName, ServingRegion, StatusCode|
|ClientErrors|Oui|Erreurs de client|Count|Total|Nombre d’appels avec une erreur client (code d’état HTTP 4xx)|ApiName, ServingRegion, StatusCode|
|DataIn|Oui|Données entrantes|Octets|Total|Contenu de la requête entrante, longueur en octets|ApiName, ServingRegion, StatusCode|
|DataOut|Oui|Données sortantes|Octets|Total|Contenu de la réponse sortante, longueur en octets|ApiName, ServingRegion, StatusCode|
|Latence|Oui|Latence|Millisecondes|Average|Latence en millisecondes|ApiName, ServingRegion, StatusCode|
|ServerErrors|Oui|Erreurs de serveur|Count|Total|Nombre d’appels avec une erreur serveur (code d’état HTTP 5xx)|ApiName, ServingRegion, StatusCode|
|SuccessfulCalls|Oui|Appels réussis|Count|Total|Nombre d’appels réussis (code d’état HTTP 2xx)|ApiName, ServingRegion, StatusCode|
|TotalCalls|Oui|Nombre total d’appels|Count|Total|Nombre total d’appels|ApiName, ServingRegion, StatusCode|
|TotalErrors|Oui|Nombre total d’erreurs|Count|Total|Nombre d’appels avec une erreur (code d’état HTTP 4xx ou 5xx)|ApiName, ServingRegion, StatusCode|


## <a name="microsoftblockchainblockchainmembers"></a>Microsoft.Blockchain/blockchainMembers

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BroadcastProcessedCount|Oui|BroadcastProcessedCountDisplayName|Count|Average|Nombre de transactions traitées.|Node, channel, type, status|
|ChaincodeExecuteTimeouts|Oui|ChaincodeExecuteTimeoutsDisplayName|Count|Average|Nombre d’exécutions de chaincode (Init ou Invoke) dont le délai d’attente a expiré.|Nœud, chaincode|
|ChaincodeLaunchFailures|Oui|ChaincodeLaunchFailuresDisplayName|Count|Average|Nombre de lancements de chaincode ayant échoué.|Nœud, chaincode|
|ChaincodeLaunchTimeouts|Oui|ChaincodeLaunchTimeoutsDisplayName|Count|Average|Nombre de lancements de chaincode ayant dépassé le délai d’expiration.|Nœud, chaincode|
|ChaincodeShimRequestsCompleted|Oui|ChaincodeShimRequestsCompletedDisplayName|Count|Average|Nombre de demandes shim de chaincode effectuées.|Nœud, type, canal, chaincode, réussite|
|ChaincodeShimRequestsReceived|Oui|ChaincodeShimRequestsReceivedDisplayName|Count|Average|Nombre de demandes shim de chaincode reçues.|Nœud, type, canal, chaincode|
|ClusterCommEgressQueueCapacity|Oui|ClusterCommEgressQueueCapacityDisplayName|Count|Average|Capacité de la file d’attente de sortie.|Nœud, hôte, msg_type, canal|
|ClusterCommEgressQueueLength|Oui|ClusterCommEgressQueueLengthDisplayName|Count|Average|Longueur de la file d’attente de sortie.|Nœud, hôte, msg_type, canal|
|ClusterCommEgressQueueWorkers|Oui|ClusterCommEgressQueueWorkersDisplayName|Count|Average|Nombre de workers de la file d’attente de sortie.|Node, channel|
|ClusterCommEgressStreamCount|Oui|ClusterCommEgressStreamCountDisplayName|Count|Average|Nombre de flux vers d’autres nœuds.|Node, channel|
|ClusterCommEgressTlsConnectionCount|Oui|ClusterCommEgressTlsConnectionCountDisplayName|Count|Average|Nombre de connexions TLS à d’autres nœuds.|Nœud|
|ClusterCommIngressStreamCount|Oui|ClusterCommIngressStreamCountDisplayName|Count|Average|Nombre de flux depuis d’autres nœuds.|Nœud|
|ClusterCommMsgDroppedCount|Oui|ClusterCommMsgDroppedCountDisplayName|Count|Average|Nombre de messages supprimés.|Nœud, hôte, canal|
|ConnectionAccepted|Oui|Connexions acceptées|Count|Total|Connexions acceptées|Nœud|
|ConnectionActive|Oui|Connexions actives|Count|Average|Connexions actives|Nœud|
|ConnectionHandled|Oui|Connexions gérées|Count|Total|Connexions gérées|Nœud|
|ConsensusEtcdraftActiveNodes|Oui|ConsensusEtcdraftActiveNodesDisplayName|Count|Average|Nombre de nœuds actifs dans ce canal.|Node, channel|
|ConsensusEtcdraftClusterSize|Oui|ConsensusEtcdraftClusterSizeDisplayName|Count|Average|Nombre de nœuds dans ce canal.|Node, channel|
|ConsensusEtcdraftCommittedBlockNumber|Oui|ConsensusEtcdraftCommittedBlockNumberDisplayName|Count|Average|Numéro du dernier bloc validé.|Node, channel|
|ConsensusEtcdraftConfigProposalsReceived|Oui|ConsensusEtcdraftConfigProposalsReceivedDisplayName|Count|Average|Nombre total de propositions reçues pour les transactions de type de configuration.|Node, channel|
|ConsensusEtcdraftIsLeader|Oui|ConsensusEtcdraftIsLeaderDisplayName|Count|Average|État de prééminence du nœud actuel : 1 s’il est prééminent, sinon 0.|Node, channel|
|ConsensusEtcdraftLeaderChanges|Oui|ConsensusEtcdraftLeaderChangesDisplayName|Count|Average|Nombre de changements de prééminence depuis le démarrage du processus.|Node, channel|
|ConsensusEtcdraftNormalProposalsReceived|Oui|ConsensusEtcdraftNormalProposalsReceivedDisplayName|Count|Average|Nombre total de propositions reçues pour les transactions de type normal.|Node, channel|
|ConsensusEtcdraftProposalFailures|Oui|ConsensusEtcdraftProposalFailuresDisplayName|Count|Average|Nombre d’échecs de proposition.|Node, channel|
|ConsensusEtcdraftSnapshotBlockNumber|Oui|ConsensusEtcdraftSnapshotBlockNumberDisplayName|Count|Average|Numéro de bloc de l’instantané le plus récent.|Node, channel|
|ConsensusKafkaBatchSize|Oui|ConsensusKafkaBatchSizeDisplayName|Count|Average|Taille moyenne en octets des lots envoyés aux rubriques.|Nœud, rubrique|
|ConsensusKafkaCompressionRatio|Oui|ConsensusKafkaCompressionRatioDisplayName|Count|Average|Taux de compression moyen (en pourcentage) pour les rubriques.|Nœud, rubrique|
|ConsensusKafkaIncomingByteRate|Oui|ConsensusKafkaIncomingByteRateDisplayName|Count|Average|Octets/seconde lus sur les répartiteurs.|Nœud, broker_id|
|ConsensusKafkaLastOffsetPersisted|Oui|ConsensusKafkaLastOffsetPersistedDisplayName|Count|Average|Décalage spécifié dans les métadonnées de bloc du bloc le plus récemment validé.|Node, channel|
|ConsensusKafkaOutgoingByteRate|Oui|ConsensusKafkaOutgoingByteRateDisplayName|Count|Average|Octets/seconde écrits sur les répartiteurs.|Nœud, broker_id|
|ConsensusKafkaRecordSendRate|Oui|ConsensusKafkaRecordSendRateDisplayName|Count|Average|Nombre d’enregistrements par seconde envoyés aux rubriques.|Nœud, rubrique|
|ConsensusKafkaRecordsPerRequest|Oui|ConsensusKafkaRecordsPerRequestDisplayName|Count|Average|Nombre moyen d’enregistrements envoyés par demande à des rubriques.|Nœud, rubrique|
|ConsensusKafkaRequestLatency|Oui|ConsensusKafkaRequestLatencyDisplayName|Count|Average|Latence moyenne en ms des demandes faites aux répartiteurs.|Nœud, broker_id|
|ConsensusKafkaRequestRate|Oui|ConsensusKafkaRequestRateDisplayName|Count|Average|Demandes/seconde envoyées aux répartiteurs.|Nœud, broker_id|
|ConsensusKafkaRequestSize|Oui|ConsensusKafkaRequestSizeDisplayName|Count|Average|Taille moyenne en octets des demandes adressées aux répartiteurs.|Nœud, broker_id|
|ConsensusKafkaResponseRate|Oui|ConsensusKafkaResponseRateDisplayName|Count|Average|Demandes/seconde envoyées aux répartiteurs.|Nœud, broker_id|
|ConsensusKafkaResponseSize|Oui|ConsensusKafkaResponseSizeDisplayName|Count|Average|Taille moyenne en octets des réponses des répartiteurs.|Nœud, broker_id|
|CpuUsagePercentageInDouble|Oui|Pourcentage d’utilisation du processeur|Pourcentage|Maximale|Pourcentage d’utilisation du processeur|Nœud|
|DeliverBlocksSent|Oui|DeliverBlocksSentDisplayName|Count|Average|Nombre de blocs envoyés par le service de remise.|Nœud, canal, filtré, data_type|
|DeliverRequestsCompleted|Oui|DeliverRequestsCompletedDisplayName|Count|Average|Nombre de demandes de remise qui ont été effectuées.|Nœud, canal, filtré, data_type, réussite|
|DeliverRequestsReceived|Oui|DeliverRequestsReceivedDisplayName|Count|Average|Nombre de demandes de remise qui ont été reçues.|Nœud, canal, filtré, data_type|
|DeliverStreamsClosed|Oui|DeliverStreamsClosedDisplayName|Count|Average|Nombre de flux gRPC qui ont été fermés pour le service de remise.|Nœud|
|DeliverStreamsOpened|Oui|DeliverStreamsOpenedDisplayName|Count|Average|Nombre de flux gRPC qui ont été ouverts pour le service de remise.|Nœud|
|EndorserChaincodeInstantiationFailures|Oui|EndorserChaincodeInstantiationFailuresDisplayName|Count|Average|Nombre d’instanciations ou de mises à niveau de chaincode qui ont échoué.|Nœud, canal, chaincode|
|EndorserDuplicateTransactionFailures|Oui|EndorserDuplicateTransactionFailuresDisplayName|Count|Average|Nombre de propositions ayant échoué en raison d’un ID de transaction en doublon.|Nœud, canal, chaincode|
|EndorserEndorsementFailures|Oui|EndorserEndorsementFailuresDisplayName|Count|Average|Nombre d’approbations ayant échoué|Node, channel, chaincode, chaincodeerror|
|EndorserProposalAclFailures|Oui|EndorserProposalAclFailuresDisplayName|Count|Average|Nombre de propositions qui ont échoué à la vérification de la liste de contrôle d’accès.|Nœud, canal, chaincode|
|EndorserProposalSimulationFailures|Oui|EndorserProposalSimulationFailuresDisplayName|Count|Average|Nombre de simulations de proposition ayant échoué.|Nœud, canal, chaincode|
|EndorserProposalsReceived|Oui|EndorserProposalsReceivedDisplayName|Count|Average|Nombre de propositions reçues.|Nœud|
|EndorserProposalValidationFailures|Oui|EndorserProposalValidationFailuresDisplayName|Count|Average|Nombre de propositions ayant échoué à la validation initiale.|Nœud|
|EndorserSuccessfulProposals|Oui|EndorserSuccessfulProposalsDisplayName|Count|Average|Nombre de propositions ayant réussi.|Nœud|
|FabricVersion|Oui|FabricVersionDisplayName|Count|Average|Version active de Fabric.|Nœud, version|
|GossipCommMessagesReceived|Oui|GossipCommMessagesReceivedDisplayName|Count|Average|Nombre de messages reçus.|Nœud|
|GossipCommMessagesSent|Oui|GossipCommMessagesSentDisplayName|Count|Average|Nombre de messages envoyés.|Nœud|
|GossipCommOverflowCount|Oui|GossipCommOverflowCountDisplayName|Count|Average|Nombre de dépassements de la mémoire tampon de la file d’attente sortante.|Nœud|
|GossipLeaderElectionLeader|Oui|GossipLeaderElectionLeaderDisplayName|Count|Average|Le pair est leader (1) ou suiveur (0).|Node, channel|
|GossipMembershipTotalPeersKnown|Oui|GossipMembershipTotalPeersKnownDisplayName|Count|Average|Nombre total de pairs connus.|Node, channel|
|GossipPayloadBufferSize|Oui|GossipPayloadBufferSizeDisplayName|Count|Average|Taille de la mémoire tampon des charges utiles.|Node, channel|
|GossipStateHeight|Oui|GossipStateHeightDisplayName|Count|Average|Hauteur actuelle du registre.|Node, channel|
|GrpcCommConnClosed|Oui|GrpcCommConnClosedDisplayName|Count|Average|Connexions gRPC fermées. Nombre de connexions actives = nombre de connexions ouvertes - nombre de connexions fermées.|Nœud|
|GrpcCommConnOpened|Oui|GrpcCommConnOpenedDisplayName|Count|Average|Connexions gRPC ouvertes. Nombre de connexions actives = nombre de connexions ouvertes - nombre de connexions fermées.|Nœud|
|GrpcServerStreamMessagesReceived|Oui|GrpcServerStreamMessagesReceivedDisplayName|Count|Average|Nombre de messages de flux reçus.|Nœud, service, méthode|
|GrpcServerStreamMessagesSent|Oui|GrpcServerStreamMessagesSentDisplayName|Count|Average|Nombre de messages de flux envoyés.|Nœud, service, méthode|
|GrpcServerStreamRequestsCompleted|Oui|GrpcServerStreamRequestsCompletedDisplayName|Count|Average|Nombre de demandes de flux effectuées.|Nœud, service, méthode, code|
|GrpcServerUnaryRequestsReceived|Oui|GrpcServerUnaryRequestsReceivedDisplayName|Count|Average|Nombre de demandes unaires reçues.|Nœud, service, méthode|
|IOReadBytes|Oui|E/S de lecture d’octets|Octets|Total|E/S de lecture d’octets|Nœud|
|IOWriteBytes|Oui|E/S d’écriture d’octets|Octets|Total|E/S d’écriture d’octets|Nœud|
|LedgerBlockchainHeight|Oui|LedgerBlockchainHeightDisplayName|Count|Average|Hauteur de la chaîne en blocs.|Node, channel|
|LedgerTransactionCount|Oui|LedgerTransactionCountDisplayName|Count|Average|Nombre de transactions traitées.|Node, channel, transaction_type, chaincode, validation_code|
|LoggingEntriesChecked|Oui|LoggingEntriesCheckedDisplayName|Count|Average|Nombre d’entrées de journal vérifiées par rapport au niveau de journalisation actif.|Nœud, niveau|
|LoggingEntriesWritten|Oui|LoggingEntriesWrittenDisplayName|Count|Average|Nombre d’entrées de journal écrites.|Nœud, niveau|
|MemoryLimit|Oui|Limite de mémoire|Octets|Average|Limite de mémoire|Nœud|
|MemoryUsage|Oui|Utilisation de la mémoire|Octets|Average|Utilisation de la mémoire|Nœud|
|MemoryUsagePercentageInDouble|Oui|Pourcentage d’utilisation de la mémoire|Pourcentage|Average|Pourcentage d’utilisation de la mémoire|Nœud|
|PendingTransactions|Oui|Transactions en attente|Count|Average|Transactions en attente|Nœud|
|ProcessedBlocks|Oui|Blocs traités|Count|Total|Blocs traités|Nœud|
|ProcessedTransactions|Oui|Transactions traitées|Count|Total|Transactions traitées|Nœud|
|QueuedTransactions|Oui|Transactions en file d’attente|Count|Average|Transactions en file d’attente|Nœud|
|RequestHandled|Oui|Requêtes traitées|Count|Total|Demandes traitées|Nœud|
|StorageUsage|Oui|Utilisation du stockage|Octets|Average|Utilisation du stockage|Nœud|


## <a name="microsoftbotservicebotservices"></a>microsoft.botservice/botservices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|RequestLatency|Oui|Latence des demandes|Millisecondes|Total|Temps pris par le serveur pour traiter la demande.|Opération, Authentification, Protocole, Centre de données|
|RequestsTraffic|Oui|Trafic des demandes|Pourcentage|Count|Nombre de demandes effectuées|Opération, Authentification, Protocole, StatusCode, StatusCodeClass, Centre de données|


## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|allcachehits|Oui|Présences dans le cache (basées sur une instance)|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allcachemisses|Oui|Absences dans le cache (basées sur une instance)|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allcacheRead|Oui|Lecture du cache (basée sur une instance)|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allcacheWrite|Oui|Écriture dans le cache (basée sur une instance)|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allconnectedclients|Oui|Clients connectés (basés sur une instance)|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allevictedkeys|Oui|Clés exclues (basées sur une instance)|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allexpiredkeys|Oui|Clés expirées (basées sur une instance)|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allgetcommands|Oui|Opérations Get (basées sur une instance)|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|alloperationsPerSecond|Oui|Opérations par seconde (basées sur une instance)|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allpercentprocessortime|Yes|UC (basée sur une instance)|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allserverLoad|Oui|Charge du serveur (basée sur une instance)|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allsetcommands|Oui|Jeux (basés sur une instance)|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|alltotalcommandsprocessed|Oui|Nombre total d’opérations (basé sur une instance)|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|alltotalkeys|Oui|Nombre total de clés (basé sur une instance)|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allusedmemory|Oui|Mémoire utilisée (basée sur une instance)|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allusedmemorypercentage|Oui|Pourcentage de mémoire utilisé (basé sur une instance)|Pourcentage|Maximale|Pourcentage de cache processeur utilisé pour les paires clé-valeur. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|allusedmemoryRss|Oui|Taille de la mémoire résidente utilisée (basée sur une instance)|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, Port, Primary|
|cachehits|Oui|Présences dans le cache|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|cachehits0|Oui|Présences dans le cache (Shard 0)|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachehits1|Oui|Présences dans le cache (Shard 1)|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachehits2|Oui|Présences dans le cache (Shard 2)|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachehits3|Oui|Présences dans le cache (Shard 3)|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachehits4|Oui|Présences dans le cache (Shard 4)|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachehits5|Oui|Présences dans le cache (Shard 5)|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachehits6|Oui|Présences dans le cache (Shard 6)|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachehits7|Oui|Présences dans le cache (Shard 7)|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachehits8|Oui|Présences dans le cache (Shard 8)|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachehits9|Oui|Présences dans le cache (Shard 9)|Count|Total|Nombre de recherches de clé ayant réussi. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheLatency|Oui|Latence de cache en microsecondes (préversion)|Count|Average|Latence du cache en microsecondes. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|cachemisses|Oui|Absences dans le cache|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|cachemisses0|Oui|Absences dans le cache (Shard 0)|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachemisses1|Oui|Absences dans le cache (Shard 1)|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachemisses2|Oui|Absences dans le cache (Shard 2)|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachemisses3|Oui|Absences dans le cache (Shard 3)|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachemisses4|Oui|Absences dans le cache (Shard 4)|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachemisses5|Oui|Absences dans le cache (Shard 5)|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachemisses6|Oui|Absences dans le cache (Shard 6)|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachemisses7|Oui|Absences dans le cache (Shard 7)|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachemisses8|Oui|Absences dans le cache (Shard 8)|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachemisses9|Oui|Absences dans le cache (Shard 9)|Count|Total|Nombre de recherches de clé ayant échoué. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cachemissrate|Oui|Taux d’échec d’accès au cache|Pourcentage|cachemissrate|Pourcentage des requêtes get dont l’accès échoue. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|cacheRead|Oui|Lecture du cache|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|cacheRead0|Oui|Cache de lecture (Shard 0)|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheRead1|Oui|Lecture du cache (Shard 1)|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheRead2|Oui|Lecture du cache (Shard 2)|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheRead3|Oui|Lecture du cache (Shard 3)|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheRead4|Oui|Lecture du cache (Shard 4)|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheRead5|Oui|Lecture du cache (Shard 5)|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheRead6|Oui|Lecture du cache (Shard 6)|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheRead7|Oui|Lecture du cache (Shard 7)|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheRead8|Oui|Lecture du cache (Shard 8)|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheRead9|Oui|Cache de lecture (Shard 9)|BytesPerSecond|Maximale|Quantité de données lues dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheWrite|Oui|Cache d’écriture|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|cacheWrite0|Oui|Cache d’écriture (Shard 0)|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheWrite1|Oui|Cache d’écriture (Shard 1)|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheWrite2|Oui|Cache d’écriture (Shard 2)|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheWrite3|Oui|Cache d’écriture (Shard 3)|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheWrite4|Oui|Cache d’écriture (Shard 4)|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheWrite5|Oui|Cache d’écriture (Shard 5)|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheWrite6|Oui|Cache d’écriture (Shard 6)|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheWrite7|Oui|Cache d’écriture (Shard 7)|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheWrite8|Oui|Cache d’écriture (Shard 8)|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|cacheWrite9|Oui|Cache d’écriture (Shard 9)|BytesPerSecond|Maximale|Quantité de données écrites dans le cache en mégaoctets par seconde (Mo/s). Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|connectedclients|Oui|Clients connectés|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|connectedclients0|Oui|Clients connectés (Shard 0)|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|connectedclients1|Oui|Clients connectés (Shard 1)|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|connectedclients2|Oui|Clients connectés (Shard 2)|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|connectedclients3|Oui|Clients connectés (Shard 3)|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|connectedclients4|Oui|Clients connectés (Shard 4)|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|connectedclients5|Oui|Clients connectés (Shard 5)|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|connectedclients6|Oui|Clients connectés (Shard 6)|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|connectedclients7|Oui|Clients connectés (Shard 7)|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|connectedclients8|Oui|Clients connectés (Shard 8)|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|connectedclients9|Oui|Clients connectés (Shard 9)|Count|Maximale|Nombre de connexions client au cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|erreurs|Oui|Erreurs|Count|Maximale|Nombre d’erreurs qui se sont produites dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId, ErrorType|
|evictedkeys|Oui|Clés exclues|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|evictedkeys0|Oui|Clés exclues (Shard 0)|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|evictedkeys1|Oui|Clés exclues (Shard 1)|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|evictedkeys2|Oui|Clés exclues (Shard 2)|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|evictedkeys3|Oui|Clés exclues (Shard 3)|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|evictedkeys4|Oui|Clés exclues (Shard 4)|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|evictedkeys5|Oui|Clés exclues (Shard 5)|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|evictedkeys6|Oui|Clés exclues (Shard 6)|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|evictedkeys7|Oui|Clés exclues (Shard 7)|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|evictedkeys8|Oui|Clés exclues (Shard 8)|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|evictedkeys9|Oui|Clés exclues (Shard 9)|Count|Total|Nombre d’éléments exclus du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|expiredkeys|Oui|Clés expirées|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|expiredkeys0|Oui|Clés expirées (Shard 0)|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|expiredkeys1|Oui|Clés expirées (Shard 1)|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|expiredkeys2|Oui|Clés expirées (Shard 2)|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|expiredkeys3|Oui|Clés expirées (Shard 3)|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|expiredkeys4|Oui|Clés expirées (Shard 4)|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|expiredkeys5|Oui|Clés expirées (Shard 5)|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|expiredkeys6|Oui|Clés expirées (Shard 6)|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|expiredkeys7|Oui|Clés expirées (Shard 7)|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|expiredkeys8|Oui|Clés expirées (Shard 8)|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|expiredkeys9|Oui|Clés expirées (Shard 9)|Count|Total|Nombre d’éléments expirés du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|getcommands|Oui|Gets|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|getcommands0|Oui|Gets (Shard 0)|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|getcommands1|Oui|Gets (Shard 1)|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|getcommands2|Oui|Gets (Shard 2)|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|getcommands3|Oui|Gets (Shard 3)|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|getcommands4|Oui|Gets (Shard 4)|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|getcommands5|Oui|Gets (Shard 5)|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|getcommands6|Oui|Gets (Shard 6)|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|getcommands7|Oui|Gets (Shard 7)|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|getcommands8|Oui|Gets (Shard 8)|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|getcommands9|Oui|Gets (Shard 9)|Count|Total|Nombre d’opérations Get du cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|operationsPerSecond|Oui|Opérations par seconde|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|operationsPerSecond0|Oui|Opérations par seconde (Partition 0)|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|operationsPerSecond1|Oui|Opérations par seconde (Partition 1)|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|operationsPerSecond2|Oui|Opérations par seconde (Partition 2)|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|operationsPerSecond3|Oui|Opérations par seconde (Partition 3)|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|operationsPerSecond4|Oui|Opérations par seconde (Partition 4)|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|operationsPerSecond5|Oui|Opérations par seconde (Partition 5)|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|operationsPerSecond6|Oui|Opérations par seconde (Partition 6)|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|operationsPerSecond7|Oui|Opérations par seconde (Partition 7)|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|operationsPerSecond8|Oui|Opérations par seconde (Partition 8)|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|operationsPerSecond9|Oui|Opérations par seconde (Partition 9)|Count|Maximale|Nombre d’opérations instantanées par seconde exécutées sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|percentProcessorTime|Oui|UC|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|percentProcessorTime0|Oui|UC (Shard 0)|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|percentProcessorTime1|Oui|UC (Shard 1)|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|percentProcessorTime2|Oui|UC (Shard 2)|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|percentProcessorTime3|Oui|UC (Shard 3)|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|percentProcessorTime4|Oui|UC (Shard 4)|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|percentProcessorTime5|Oui|UC (Shard 5)|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|percentProcessorTime6|Oui|UC (Shard 6)|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|percentProcessorTime7|Oui|UC (Shard 7)|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|percentProcessorTime8|Oui|UC (Shard 8)|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|percentProcessorTime9|Oui|UC (Shard 9)|Pourcentage|Maximale|Utilisation de l’UC du serveur Azure Cache pour Redis en pourcentage. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|serverLoad|Oui|Charge du serveur|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|serverLoad0|Oui|Charge du serveur (Shard 0)|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|serverLoad1|Oui|Charge du serveur (Shard 1)|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|serverLoad2|Oui|Charge du serveur (Shard 2)|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|serverLoad3|Oui|Charge du serveur (Shard 3)|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|serverLoad4|Oui|Charge du serveur (Shard 4)|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|serverLoad5|Oui|Charge du serveur (Shard 5)|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|serverLoad6|Oui|Charge du serveur (Shard 6)|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|serverLoad7|Oui|Charge du serveur (Shard 7)|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|serverLoad8|Oui|Charge du serveur (Shard 8)|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|serverLoad9|Oui|Charge du serveur (Shard 9)|Pourcentage|Maximale|Pourcentage de cycles dans lesquels le serveur Redis est occupé par le traitement et n’est pas inactif, en attente de messages. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|setcommands|Oui|Jeux|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|setcommands0|Oui|Sets (Shard 0)|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|setcommands1|Oui|Sets (Shard 1)|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|setcommands2|Oui|Sets (Shard 2)|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|setcommands3|Oui|Sets (Shard 3)|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|setcommands4|Oui|Sets (Shard 4)|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|setcommands5|Oui|Sets (Shard 5)|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|setcommands6|Oui|Sets (Shard 6)|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|setcommands7|Oui|Sets (Shard 7)|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|setcommands8|Oui|Sets (Shard 8)|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|setcommands9|Oui|Sets (Shard 9)|Count|Total|Nombre d’opérations ensemblistes sur le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalcommandsprocessed|Oui|Total des opérations|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|totalcommandsprocessed0|Oui|Total des opérations (Shard 0)|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalcommandsprocessed1|Oui|Total des opérations (Shard 1)|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalcommandsprocessed2|Oui|Total des opérations (Shard 2)|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalcommandsprocessed3|Oui|Total des opérations (Shard 3)|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalcommandsprocessed4|Oui|Total des opérations (Shard 4)|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalcommandsprocessed5|Oui|Total des opérations (Shard 5)|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalcommandsprocessed6|Oui|Total des opérations (Shard 6)|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalcommandsprocessed7|Oui|Total des opérations (Shard 7)|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalcommandsprocessed8|Oui|Total des opérations (Shard 8)|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalcommandsprocessed9|Oui|Total des opérations (Shard 9)|Count|Total|Nombre total de commandes traitées par le serveur de cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalkeys|Oui|Nombre total de clés|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|totalkeys0|Oui|Nombre total de clés (Shard 0)|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalkeys1|Oui|Nombre total de clés (Shard 1)|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalkeys2|Oui|Nombre total de clés (Shard 2)|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalkeys3|Oui|Nombre total de clés (Shard 3)|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalkeys4|Oui|Nombre total de clés (Shard 4)|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalkeys5|Oui|Nombre total de clés (Shard 5)|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalkeys6|Oui|Nombre total de clés (Shard 6)|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalkeys7|Oui|Nombre total de clés (Shard 7)|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalkeys8|Oui|Nombre total de clés (Shard 8)|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|totalkeys9|Oui|Nombre total de clés (Shard 9)|Count|Maximale|Nombre total d’éléments dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemory|Oui|Mémoire utilisée|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|usedmemory0|Oui|Mémoire utilisée (Shard 0)|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemory1|Oui|Mémoire utilisée (Shard 1)|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemory2|Oui|Mémoire utilisée (Shard 2)|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemory3|Oui|Mémoire utilisée (Shard 3)|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemory4|Oui|Mémoire utilisée (Shard 4)|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemory5|Oui|Mémoire utilisée (Shard 5)|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemory6|Oui|Mémoire utilisée (Shard 6)|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemory7|Oui|Mémoire utilisée (Shard 7)|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemory8|Oui|Mémoire utilisée (Shard 8)|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemory9|Oui|Mémoire utilisée (Shard 9)|Octets|Maximale|Quantité de cache processeur, exprimée en Mo, utilisée pour les paires clé-valeur dans le cache. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemorypercentage|Oui|Pourcentage de mémoire utilisé|Pourcentage|Maximale|Pourcentage de cache processeur utilisé pour les paires clé-valeur. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|usedmemoryRss|Oui|Taille de la mémoire résidente utilisée|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|ShardId|
|usedmemoryRss0|Oui|Mémoire résidente utilisée (Shard 0)|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemoryRss1|Oui|Mémoire résidente utilisée (Shard 1)|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemoryRss2|Oui|Mémoire résidente utilisée (Shard 2)|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemoryRss3|Oui|Mémoire résidente utilisée (Shard 3)|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemoryRss4|Oui|Mémoire résidente utilisée (Shard 4)|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemoryRss5|Oui|Mémoire résidente utilisée (Shard 5)|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemoryRss6|Oui|Mémoire résidente utilisée (Shard 6)|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemoryRss7|Oui|Mémoire résidente utilisée (Shard 7)|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemoryRss8|Oui|Mémoire résidente utilisée (Shard 8)|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|
|usedmemoryRss9|Oui|Mémoire résidente utilisée (Shard 9)|Octets|Maximale|Quantité de cache processeur utilisée (exprimée en Mo), fragmentation et métadonnées comprises. Pour plus d'informations, consultez https://aka.ms/redis/metrics.|Aucune dimension|


## <a name="microsoftcacheredisenterprise"></a>Microsoft.Cache/redisEnterprise

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|cachehits|Oui|Présences dans le cache|Count|Total||Aucune dimension|
|cacheLatency|Oui|Latence de cache en microsecondes (préversion)|Count|Average||InstanceId|
|cachemisses|Oui|Absences dans le cache|Count|Total||InstanceId|
|cacheRead|Oui|Lecture du cache|BytesPerSecond|Maximale||InstanceId|
|cacheWrite|Oui|Cache d’écriture|BytesPerSecond|Maximale||InstanceId|
|connectedclients|Oui|Clients connectés|Count|Maximale||InstanceId|
|erreurs|Oui|Erreurs|Count|Maximale||InstanceId, ErrorType|
|evictedkeys|Oui|Clés exclues|Count|Total||Aucune dimension|
|expiredkeys|Oui|Clés expirées|Count|Total||Aucune dimension|
|getcommands|Oui|Gets|Count|Total||Aucune dimension|
|operationsPerSecond|Oui|Opérations par seconde|Count|Maximale||Aucune dimension|
|percentProcessorTime|Oui|UC|Pourcentage|Maximale||InstanceId|
|serverLoad|Oui|Charge du serveur|Pourcentage|Maximale||Aucune dimension|
|setcommands|Oui|Jeux|Count|Total||Aucune dimension|
|totalcommandsprocessed|Oui|Total des opérations|Count|Total||Aucune dimension|
|totalkeys|Oui|Nombre total de clés|Count|Maximale||Aucune dimension|
|usedmemory|Oui|Mémoire utilisée|Octets|Maximale||Aucune dimension|
|usedmemorypercentage|Oui|Pourcentage de mémoire utilisé|Pourcentage|Maximale||InstanceId|


## <a name="microsoftcdncdnwebapplicationfirewallpolicies"></a>Microsoft.Cdn/cdnwebapplicationfirewallpolicies

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|WebApplicationFirewallRequestCount|Oui|Nombre de requêtes du pare-feu d’applications web|Count|Total|Nombre de requêtes clients traitées par le pare-feu d’applications web|PolicyName, RuleName, Action|


## <a name="microsoftcdnprofiles"></a>Microsoft.Cdn/profiles

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ByteHitRatio|Oui|Taux de réussite en octets|Pourcentage|Average|Rapport entre le nombre total d’octets trouvés dans le cache et le nombre total d’octets des réponses|Point de terminaison|
|OriginHealthPercentage|Oui|Pourcentage d’intégrité de l’origine|Pourcentage|Average|Pourcentage de sondes d’intégrité réussies de AFDX vers les backends.|Origin, OriginGroup|
|OriginLatency|Oui|latence de l’origine|Millisecondes|Average|Temps calculé à partir du moment où la demande est envoyée par la périphérie AFDX au back-end jusqu’à ce qu’AFDX reçoive le dernier octet de la réponse du back-end.|Origine, Point de terminaison|
|OriginRequestCount|Oui|Nombre de demandes de l’origine|Count|Total|Nombre de demandes envoyées depuis AFDX à l’origine.|HttpStatus, HttpStatusGroup, Origine, Point de terminaison|
|Percentage4XX|Oui|Pourcentage de 4XX|Pourcentage|Average|Pourcentage de toutes les demandes client pour lesquelles le code d’état de la réponse est 4XX|Point de terminaison, ClientRegion, ClientCountry|
|Percentage5XX|Oui|Pourcentage de 5XX|Pourcentage|Average|Pourcentage de toutes les demandes client pour lesquelles le code d’état de la réponse est 5XX|Point de terminaison, ClientRegion, ClientCountry|
|RequestCount|Oui|Nombre de requêtes|Count|Total|Nombre de requêtes clients prises en charge par le proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry, Point de terminaison|
|RequestSize|Oui|Taille de la requête|Octets|Total|Nombre d’octets envoyés en tant que demandes de clients à AFDX.|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry, Point de terminaison|
|ResponseSize|Oui|Taille de la réponse|Octets|Total|Nombre d’octets envoyés en tant que réponses du proxy HTTP/S aux clients|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry, Point de terminaison|
|TotalLatency|Oui|Latence totale|Millisecondes|Average|Temps calculé à partir du moment où la requête du client est reçue par le proxy HTTP/S jusqu’à ce que le client accuse réception du dernier octet de la réponse du proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry, Point de terminaison|
|WebApplicationFirewallRequestCount|Oui|Nombre de requêtes du pare-feu d’applications web|Count|Total|Nombre de requêtes clients traitées par le pare-feu d’applications web|PolicyName, RuleName, Action|


## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disk Read Bytes/Sec|Non|Lecture du disque|BytesPerSecond|Average|Moyenne d’octets lus à partir du disque pendant la période d’analyse.|RoleInstanceId|
|Disk Read Operations/Sec|Oui|Disk Read Operations/Sec|CountPerSecond|Average|E/S de lecture disque par seconde.|RoleInstanceId|
|Disk Write Bytes/Sec|Non|Écriture sur le disque|BytesPerSecond|Average|Moyenne d’octets écrits sur le disque pendant la période d’analyse.|RoleInstanceId|
|Disk Write Operations/Sec|Oui|Disk Write Operations/Sec|CountPerSecond|Average|E/S d’écriture sur disque par seconde.|RoleInstanceId|
|Network In|Oui|Network In|Octets|Total|Nombre d’octets reçus sur toutes les interfaces réseau par les machines virtuelles (trafic entrant).|RoleInstanceId|
|Network Out|Oui|Network Out|Octets|Total|Nombre d’octets envoyés sur toutes les interfaces réseau par les machines virtuelles (trafic sortant).|RoleInstanceId|
|Percentage CPU|Oui|Percentage CPU|Pourcentage|Average|Pourcentage d’unités de calcul affectées actuellement utilisées par des machines virtuelles.|RoleInstanceId|


## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disk Read Bytes/Sec|Non|Lecture du disque|BytesPerSecond|Average|Moyenne d’octets lus à partir du disque pendant la période d’analyse.|Aucune dimension|
|Disk Read Operations/Sec|Oui|Disk Read Operations/Sec|CountPerSecond|Average|E/S de lecture disque par seconde.|Aucune dimension|
|Disk Write Bytes/Sec|Non|Écriture sur le disque|BytesPerSecond|Average|Moyenne d’octets écrits sur le disque pendant la période d’analyse.|Aucune dimension|
|Disk Write Operations/Sec|Oui|Disk Write Operations/Sec|CountPerSecond|Average|E/S d’écriture sur disque par seconde.|Aucune dimension|
|Network In|Oui|Network In|Octets|Total|Nombre d’octets reçus sur toutes les interfaces réseau par les machines virtuelles (trafic entrant).|Aucune dimension|
|Network Out|Oui|Network Out|Octets|Total|Nombre d’octets envoyés sur toutes les interfaces réseau par les machines virtuelles (trafic sortant).|Aucune dimension|
|Percentage CPU|Oui|Percentage CPU|Pourcentage|Average|Pourcentage d’unités de calcul affectées actuellement utilisées par des machines virtuelles.|Aucune dimension|


## <a name="microsoftclassicstoragestorageaccounts"></a>Microsoft.ClassicStorage/storageAccounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Pourcentage de disponibilité pour le service de stockage ou l’opération API spécifiée. La disponibilité est calculée en prenant la valeur TotalBillableRequests puis en la divisant par le nombre de requêtes applicables, y compris celles qui ont généré des erreurs inattendues. Toutes erreurs inattendues réduisent la disponibilité du service de stockage ou de l’opération API spécifiée.|GeoType, ApiName, Authentication|
|Sortie|Oui|Sortie|Octets|Total|Quantité de données de sortie, en octets. Ce nombre inclut les sorties d’un client externe dans Stockage Microsoft Azure ainsi que les sorties dans Azure. Par conséquent, ce nombre ne reflète pas les sorties facturables.|GeoType, ApiName, Authentication|
|Entrée|Oui|Entrée|Octets|Total|Quantité de données d’entrée, en octets. Ce nombre inclut les entrées d’un client externe dans Stockage Microsoft Azure ainsi que les entrées dans Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Oui|Latence E2E de réussite|Millisecondes|Average|Latence de bout en bout des requêtes réussies envoyées à un service de stockage ou à l’opération API spécifiée, en millisecondes. Cette valeur inclut le temps de traitement requis au sein de Stockage Microsoft Azure pour lire la requête, envoyer la réponse et recevoir un accusé de réception de la réponse.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Oui|Latence du serveur avec requête réussie|Millisecondes|Average|Latence utilisée par Stockage Microsoft Azure pour traiter une requête réussie, en millisecondes. Cette valeur n’inclut pas la latence réseau spécifiée dans SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transactions|Oui|Transactions|Count|Total|Nombre de requêtes envoyées à un service de stockage ou à l’opération API spécifiée. Ce nombre inclut les requêtes réussies et celles ayant échoué, ainsi que les requêtes qui ont généré des erreurs. Utilisez la dimension ResponseType pour connaître le nombre des différents types de réponses.|ResponseType, GeoType, ApiName, Authentication|
|UsedCapacity|Oui|Capacité utilisée|Octets|Average|Capacité de compte utilisée|Aucune dimension|


## <a name="microsoftclassicstoragestorageaccountsblobservices"></a>Microsoft.ClassicStorage/storageAccounts/blobServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Pourcentage de disponibilité pour le service de stockage ou l’opération API spécifiée. La disponibilité est calculée en prenant la valeur TotalBillableRequests puis en la divisant par le nombre de requêtes applicables, y compris celles qui ont généré des erreurs inattendues. Toutes erreurs inattendues réduisent la disponibilité du service de stockage ou de l’opération API spécifiée.|GeoType, ApiName, Authentication|
|BlobCapacity|Non|Capacité d’objet blob|Octets|Average|Quantité de stockage utilisée par le service BLOB du compte de stockage, en octets.|BlobType, Tier|
|BlobCount|Non|Nombre d’objets blob|Count|Average|Nombre d’objets blob dans le service Blob du compte de stockage.|BlobType, Tier|
|ContainerCount|Oui|Nombre de conteneurs d’objets blob|Count|Average|Nombre de conteneurs dans le service Blob du compte de stockage.|Aucune dimension|
|Sortie|Oui|Sortie|Octets|Total|Quantité de données de sortie, en octets. Ce nombre inclut les sorties d’un client externe dans Stockage Microsoft Azure ainsi que les sorties dans Azure. Par conséquent, ce nombre ne reflète pas les sorties facturables.|GeoType, ApiName, Authentication|
|IndexCapacity|Non|Capacité d'index|Octets|Average|Stockage utilisé par l'index (hiérarchique) d'ADLS Gen2 en octets.|Aucune dimension|
|Entrée|Oui|Entrée|Octets|Total|Quantité de données d’entrée, en octets. Ce nombre inclut les entrées d’un client externe dans Stockage Microsoft Azure ainsi que les entrées dans Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Oui|Latence E2E de réussite|Millisecondes|Average|Latence de bout en bout des requêtes réussies envoyées à un service de stockage ou à l’opération API spécifiée, en millisecondes. Cette valeur inclut le temps de traitement requis au sein de Stockage Microsoft Azure pour lire la requête, envoyer la réponse et recevoir un accusé de réception de la réponse.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Oui|Latence du serveur avec requête réussie|Millisecondes|Average|Latence utilisée par Stockage Microsoft Azure pour traiter une requête réussie, en millisecondes. Cette valeur n’inclut pas la latence réseau spécifiée dans SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transactions|Oui|Transactions|Count|Total|Nombre de requêtes envoyées à un service de stockage ou à l’opération API spécifiée. Ce nombre inclut les requêtes réussies et celles ayant échoué, ainsi que les requêtes qui ont généré des erreurs. Utilisez la dimension ResponseType pour connaître le nombre des différents types de réponses.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftclassicstoragestorageaccountsfileservices"></a>Microsoft.ClassicStorage/storageAccounts/fileServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Pourcentage de disponibilité pour le service de stockage ou l’opération API spécifiée. La disponibilité est calculée en prenant la valeur TotalBillableRequests puis en la divisant par le nombre de requêtes applicables, y compris celles qui ont généré des erreurs inattendues. Toutes erreurs inattendues réduisent la disponibilité du service de stockage ou de l’opération API spécifiée.|GeoType, ApiName, Authentication, FileShare|
|Sortie|Oui|Sortie|Octets|Total|Quantité de données de sortie, en octets. Ce nombre inclut les sorties d’un client externe dans Stockage Microsoft Azure ainsi que les sorties dans Azure. Par conséquent, ce nombre ne reflète pas les sorties facturables.|GeoType, ApiName, Authentication, FileShare|
|FileCapacity|Non|Capacité de fichiers|Octets|Average|Quantité de stockage utilisée par le service Fichier du compte de stockage, en octets.|FileShare|
|FileCount|Non|Nombre de fichiers|Count|Average|Nombre de fichiers dans le service Fichier du compte de stockage.|FileShare|
|FileShareCount|Non|Nombre de partages de fichiers|Count|Average|Nombre de partages de fichiers dans le service Fichier du compte de stockage.|Aucune dimension|
|FileShareQuota|Non|Taille du quota de partage de fichiers|Octets|Average|Limite supérieure de la quantité de stockage pouvant être utilisée par le service Azure Files, en octets.|FileShare|
|FileShareSnapshotCount|Non|Nombre d’instantanés de partage de fichiers|Count|Average|Nombre de captures instantanées présentes sur le partage dans le service Fichier du compte de stockage.|FileShare|
|FileShareSnapshotSize|Non|Taille d’instantané de partage de fichiers|Octets|Average|Quantité de stockage utilisée par les instantanés dans le service Fichier du compte de stockage, en octets.|FileShare|
|Entrée|Oui|Entrée|Octets|Total|Quantité de données d’entrée, en octets. Ce nombre inclut les entrées d’un client externe dans Stockage Microsoft Azure ainsi que les entrées dans Azure.|GeoType, ApiName, Authentication, FileShare|
|SuccessE2ELatency|Oui|Latence E2E de réussite|Millisecondes|Average|Latence de bout en bout des requêtes réussies envoyées à un service de stockage ou à l’opération API spécifiée, en millisecondes. Cette valeur inclut le temps de traitement requis au sein de Stockage Microsoft Azure pour lire la requête, envoyer la réponse et recevoir un accusé de réception de la réponse.|GeoType, ApiName, Authentication, FileShare|
|SuccessServerLatency|Oui|Latence du serveur avec requête réussie|Millisecondes|Average|Latence utilisée par Stockage Microsoft Azure pour traiter une requête réussie, en millisecondes. Cette valeur n’inclut pas la latence réseau spécifiée dans SuccessE2ELatency.|GeoType, ApiName, Authentication, FileShare|
|Transactions|Oui|Transactions|Count|Total|Nombre de requêtes envoyées à un service de stockage ou à l’opération API spécifiée. Ce nombre inclut les requêtes réussies et celles ayant échoué, ainsi que les requêtes qui ont généré des erreurs. Utilisez la dimension ResponseType pour connaître le nombre des différents types de réponses.|ResponseType, GeoType, ApiName, Authentication, FileShare|


## <a name="microsoftclassicstoragestorageaccountsqueueservices"></a>Microsoft.ClassicStorage/storageAccounts/queueServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Pourcentage de disponibilité pour le service de stockage ou l’opération API spécifiée. La disponibilité est calculée en prenant la valeur TotalBillableRequests puis en la divisant par le nombre de requêtes applicables, y compris celles qui ont généré des erreurs inattendues. Toutes erreurs inattendues réduisent la disponibilité du service de stockage ou de l’opération API spécifiée.|GeoType, ApiName, Authentication|
|Sortie|Oui|Sortie|Octets|Total|Quantité de données de sortie, en octets. Ce nombre inclut les sorties d’un client externe dans Stockage Microsoft Azure ainsi que les sorties dans Azure. Par conséquent, ce nombre ne reflète pas les sorties facturables.|GeoType, ApiName, Authentication|
|Entrée|Oui|Entrée|Octets|Total|Quantité de données d’entrée, en octets. Ce nombre inclut les entrées d’un client externe dans Stockage Microsoft Azure ainsi que les entrées dans Azure.|GeoType, ApiName, Authentication|
|QueueCapacity|Oui|Capacité de la file d’attente|Octets|Average|Quantité de stockage utilisée par le service File d’attente du compte de stockage, en octets.|Aucune dimension|
|QueueCount|Oui|Nombre de files d’attente|Count|Average|Nombre de files d’attente dans le service File d’attente du compte de stockage.|Aucune dimension|
|QueueMessageCount|Oui|Nombre de messages dans la file d’attente|Count|Average|Nombre approximatif de messages en file d’attente dans le service File d’attente du compte de stockage.|Aucune dimension|
|SuccessE2ELatency|Oui|Latence E2E de réussite|Millisecondes|Average|Latence de bout en bout des requêtes réussies envoyées à un service de stockage ou à l’opération API spécifiée, en millisecondes. Cette valeur inclut le temps de traitement requis au sein de Stockage Microsoft Azure pour lire la requête, envoyer la réponse et recevoir un accusé de réception de la réponse.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Oui|Latence du serveur avec requête réussie|Millisecondes|Average|Latence utilisée par Stockage Microsoft Azure pour traiter une requête réussie, en millisecondes. Cette valeur n’inclut pas la latence réseau spécifiée dans SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transactions|Oui|Transactions|Count|Total|Nombre de requêtes envoyées à un service de stockage ou à l’opération API spécifiée. Ce nombre inclut les requêtes réussies et celles ayant échoué, ainsi que les requêtes qui ont généré des erreurs. Utilisez la dimension ResponseType pour connaître le nombre des différents types de réponses.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftclassicstoragestorageaccountstableservices"></a>Microsoft.ClassicStorage/storageAccounts/tableServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Pourcentage de disponibilité pour le service de stockage ou l’opération API spécifiée. La disponibilité est calculée en prenant la valeur TotalBillableRequests puis en la divisant par le nombre de requêtes applicables, y compris celles qui ont généré des erreurs inattendues. Toutes erreurs inattendues réduisent la disponibilité du service de stockage ou de l’opération API spécifiée.|GeoType, ApiName, Authentication|
|Sortie|Oui|Sortie|Octets|Total|Quantité de données de sortie, en octets. Ce nombre inclut les sorties d’un client externe dans Stockage Microsoft Azure ainsi que les sorties dans Azure. Par conséquent, ce nombre ne reflète pas les sorties facturables.|GeoType, ApiName, Authentication|
|Entrée|Oui|Entrée|Octets|Total|Quantité de données d’entrée, en octets. Ce nombre inclut les entrées d’un client externe dans Stockage Microsoft Azure ainsi que les entrées dans Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Oui|Latence E2E de réussite|Millisecondes|Average|Latence de bout en bout des requêtes réussies envoyées à un service de stockage ou à l’opération API spécifiée, en millisecondes. Cette valeur inclut le temps de traitement requis au sein de Stockage Microsoft Azure pour lire la requête, envoyer la réponse et recevoir un accusé de réception de la réponse.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Oui|Latence du serveur avec requête réussie|Millisecondes|Average|Latence utilisée par Stockage Microsoft Azure pour traiter une requête réussie, en millisecondes. Cette valeur n’inclut pas la latence réseau spécifiée dans SuccessE2ELatency.|GeoType, ApiName, Authentication|
|TableCapacity|Oui|Capacité de la table|Octets|Average|Quantité de stockage utilisée par le service de Table du compte de stockage, en octets.|Aucune dimension|
|TableCount|Oui|Nombre de tables|Count|Average|Nombre de tables dans le service de Table du compte de stockage.|Aucune dimension|
|TableEntityCount|Oui|Nombre d’entités de table|Count|Average|Nombre d’entités de table dans le service de Table du compte de stockage.|Aucune dimension|
|Transactions|Oui|Transactions|Count|Total|Nombre de requêtes envoyées à un service de stockage ou à l’opération API spécifiée. Ce nombre inclut les requêtes réussies et celles ayant échoué, ainsi que les requêtes qui ont généré des erreurs. Utilisez la dimension ResponseType pour connaître le nombre des différents types de réponses.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftcloudtesthostedpools"></a>Microsoft.Cloudtest/hostedpools

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Allocated|Yes|Allocated|Count|Average|Ressources allouées|PoolId, SKU, Images, ProviderName|
|AllocationDurationMs|Yes|AllocationDurationMs|Millisecondes|Average|Temps moyen d’allocation des requêtes (en millisecondes)|PoolId, Type, ResourceRequestType, Image|
|Nombre|Oui|Count|Count|Count|Nombre de requêtes dans la dernière sauvegarde|RequestType, Status, PoolId, Type, ErrorCode, FailureStage|
|NotReady|Yes|NotReady|Count|Average|Ressources qui ne sont pas prêtes à être utilisées|PoolId, SKU, Images, ProviderName|
|PendingReimage|Yes|PendingReimage|Count|Average|Ressources en attente de réinitialisation|PoolId, SKU, Images, ProviderName|
|PendingReturn|Yes|PendingReturn|Count|Average|Ressources en attente de retour|PoolId, SKU, Images, ProviderName|
|approvisionné|Yes|approvisionné|Count|Average|Ressources approvisionnées|PoolId, SKU, Images, ProviderName|
|Ready|Oui|Ready|Count|Average|Ressources prêtes à être utilisées|PoolId, SKU, Images, ProviderName|
|Démarrage en cours|Yes|Démarrage en cours|Count|Average|Ressources qui démarrent|PoolId, SKU, Images, ProviderName|
|Total|Yes|Total|Count|Average|Nombre total de ressources|PoolId, SKU, Images, ProviderName|


## <a name="microsoftcloudtestpools"></a>Microsoft.Cloudtest/pools

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Allocated|Yes|Allocated|Count|Average|Ressources allouées|PoolId, SKU, Images, ProviderName|
|AllocationDurationMs|Yes|AllocationDurationMs|Millisecondes|Average|Temps moyen d’allocation des requêtes (en millisecondes)|PoolId, Type, ResourceRequestType, Image|
|Nombre|Oui|Count|Count|Count|Nombre de requêtes dans la dernière sauvegarde|RequestType, Status, PoolId, Type, ErrorCode, FailureStage|
|NotReady|Yes|NotReady|Count|Average|Ressources qui ne sont pas prêtes à être utilisées|PoolId, SKU, Images, ProviderName|
|PendingReimage|Yes|PendingReimage|Count|Average|Ressources en attente de réinitialisation|PoolId, SKU, Images, ProviderName|
|PendingReturn|Yes|PendingReturn|Count|Average|Ressources en attente de retour|PoolId, SKU, Images, ProviderName|
|approvisionné|Yes|approvisionné|Count|Average|Ressources approvisionnées|PoolId, SKU, Images, ProviderName|
|Ready|Oui|Ready|Count|Average|Ressources prêtes à être utilisées|PoolId, SKU, Images, ProviderName|
|Démarrage en cours|Yes|Démarrage en cours|Count|Average|Ressources qui démarrent|PoolId, SKU, Images, ProviderName|
|Total|Yes|Total|Count|Average|Nombre total de ressources|PoolId, SKU, Images, ProviderName|


## <a name="microsoftclusterstornodes"></a>Microsoft.ClusterStor/nodes

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|TotalCapacityAvailable|No|TotalCapacityAvailable|Octets|Average|Capacité totale disponible dans le système de fichiers Lustre|filesystem_name, category, system|
|TotalCapacityUsed|No|TotalCapacityUsed|Octets|Average|Capacité totale utilisée dans le système de fichiers Lustre|filesystem_name, category, system|
|TotalRead|No|TotalRead|BytesPerSecond|Average|Nombre total de lectures du système de fichiers Lustre par seconde|filesystem_name, category, system|
|TotalWrite|No|TotalWrite|BytesPerSecond|Average|Nombre total d’écritures dans le système de fichiers Lustre par seconde|filesystem_name, category, system|


## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BlockedCalls|Oui|Appels bloqués|Count|Total|Nombre d’appels ayant dépassé la limite de débit ou de quota.|ApiName, OperationName, Region|
|CharactersTrained|Oui|Caractères formés|Count|Total|Nombre total de caractères formés.|ApiName, OperationName, Region|
|CharactersTranslated|Oui|Caractères traduits|Count|Total|Nombre total de caractères dans la requête de texte entrante.|ApiName, OperationName, Region|
|ClientErrors|Oui|Erreurs de client|Count|Total|Nombre d’appels avec erreur côté client (code de réponse HTTP : 4xx).|ApiName, OperationName, Region|
|DataIn|Oui|Données entrantes|Octets|Total|Taille des données entrantes en octets.|ApiName, OperationName, Region|
|DataOut|Oui|Données sortantes|Octets|Total|Taille des données sortantes en octets.|ApiName, OperationName, Region|
|Latence|Oui|Latence|Millisecondes|Average|Latence en millisecondes.|ApiName, OperationName, Region|
|LearnedEvents|Oui|Événements appris|Count|Total|Nombre d’événements appris.|IsMatchBaseline, Mode, RunId|
|MatchedRewards|Oui|Récompenses en correspondance|Count|Total| Nombre de récompenses en correspondance.|Mode, RunId|
|ObservedRewards|Oui|Récompenses observées|Count|Total|Nombre de récompenses observées.|Mode, RunId|
|ProcessedCharacters|Oui|Caractères traités|Count|Total|Nombre de caractères.|ApiName, FeatureName, UsageChannel, Region|
|ProcessedTextRecords|Oui|Enregistrements texte traités|Count|Total|Nombre d’enregistrements texte.|ApiName, FeatureName, UsageChannel, Region|
|ServerErrors|Oui|Erreurs de serveur|Count|Total|Nombre d’appels avec erreur interne du service (code de réponse HTTP : 5xx).|ApiName, OperationName, Region|
|SpeechSessionDuration|Oui|Durée de la session vocale|Secondes|Total|Durée totale de la session vocale en secondes.|ApiName, OperationName, Region|
|SuccessfulCalls|Oui|Appels réussis|Count|Total|Nombre d’appels réussis.|ApiName, OperationName, Region|
|SynthesizedCharacters|Yes|Caractères synthétisés|Nombre|Total|Nombre de caractères.|ApiName, FeatureName, UsageChannel, Region|
|TotalCalls|Oui|Nombre total d’appels|Count|Total|Nombre total d’appels.|ApiName, OperationName, Region|
|TotalErrors|Oui|Nombre total d’erreurs|Count|Total|Nombre total d’appels avec réponse d’erreur (code de réponse HTTP : 4xx ou 5xx).|ApiName, OperationName, Region|
|TotalTokenCalls|Oui|Total d’appels de jeton|Count|Total|Nombre total d’appels de jeton.|ApiName, OperationName, Region|
|TotalTransactions|Oui|Nombre total de transactions|Count|Total|Nombre total de transactions.|Aucune dimension|
|VoiceModelHostingHours|Yes|Heures d’hébergement du modèle vocal|Nombre|Total|Nombre d’heures.|ApiName, FeatureName, UsageChannel, Region|
|VoiceModelTrainingMinutes|Yes|Minutes de formation du modèle vocal|Nombre|Total|Nombre de minutes.|ApiName, FeatureName, UsageChannel, Region|


## <a name="microsoftcommunicationcommunicationservices"></a>Microsoft.Communication/CommunicationServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|APIRequestAuthentication|Non|Demandes d’API d’authentification|Count|Count|Nombre total de demandes sur le point de terminaison d’authentification de Communication Services.|Opération, StatusCode, StatusCodeClass|
|APIRequestChat|Oui|Demandes d’API de conversation|Count|Count|Nombre total de demandes sur le point de terminaison de conversation de Communication Services.|Opération, StatusCode, StatusCodeClass|
|APIRequestSMS|Oui|Demandes d’API SMS|Count|Count|Nombre total de demandes sur le point de terminaison SMS de Communication Services.|Opération, StatusCode, StatusCodeClass|


## <a name="microsoftcomputecloudservices"></a>Microsoft.Compute/cloudServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Octets de mémoire disponibles|Yes|Octets de mémoire disponibles (préversion)|Octets|Average|Quantité de mémoire physique, en octets, immédiatement disponible pour l’allocation à un processus ou pour une utilisation système dans la machine virtuelle|RoleInstanceId, RoleId|
|Disk Read Bytes|Oui|Disk Read Bytes|Octets|Total|Octets lus à partir du disque pendant la période d’analyse|RoleInstanceId, RoleId|
|Disk Read Operations/Sec|Oui|Disk Read Operations/Sec|CountPerSecond|Average|E/S de lecture disque par seconde|RoleInstanceId, RoleId|
|Disk Write Bytes|Oui|Disk Write Bytes|Octets|Total|Octets écrits sur le disque pendant la période d’analyse|RoleInstanceId, RoleId|
|Disk Write Operations/Sec|Oui|Disk Write Operations/Sec|CountPerSecond|Average|E/S d’écriture disque par seconde|RoleInstanceId, RoleId|
|Octets entrants réseau totaux|Oui|Octets entrants réseau totaux|Octets|Total|Le nombre d’octets reçus sur toutes les interfaces réseau par les ordinateurs virtuels (trafic entrant)|RoleInstanceId, RoleId|
|Octets sortants réseau totaux|Oui|Octets sortants réseau totaux|Octets|Total|Le nombre d’octets envoyés sur toutes les interfaces réseau par les ordinateurs virtuels (trafic sortant)|RoleInstanceId, RoleId|
|Percentage CPU|Oui|Percentage CPU|Pourcentage|Average|Le pourcentage d’unités de calcul affectées actuellement utilisées par des machines virtuelles|RoleInstanceId, RoleId|


## <a name="microsoftcomputecloudservicesroles"></a>Microsoft.Compute/cloudServices/roles

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Octets de mémoire disponibles|Yes|Octets de mémoire disponibles (préversion)|Octets|Average|Quantité de mémoire physique, en octets, immédiatement disponible pour l’allocation à un processus ou pour une utilisation système dans la machine virtuelle|RoleInstanceId, RoleId|
|Disk Read Bytes|Oui|Disk Read Bytes|Octets|Total|Octets lus à partir du disque pendant la période d’analyse|RoleInstanceId, RoleId|
|Disk Read Operations/Sec|Oui|Disk Read Operations/Sec|CountPerSecond|Average|E/S de lecture disque par seconde|RoleInstanceId, RoleId|
|Disk Write Bytes|Oui|Disk Write Bytes|Octets|Total|Octets écrits sur le disque pendant la période d’analyse|RoleInstanceId, RoleId|
|Disk Write Operations/Sec|Oui|Disk Write Operations/Sec|CountPerSecond|Average|E/S d’écriture disque par seconde|RoleInstanceId, RoleId|
|Octets entrants réseau totaux|Oui|Octets entrants réseau totaux|Octets|Total|Le nombre d’octets reçus sur toutes les interfaces réseau par les ordinateurs virtuels (trafic entrant)|RoleInstanceId, RoleId|
|Octets sortants réseau totaux|Oui|Octets sortants réseau totaux|Octets|Total|Le nombre d’octets envoyés sur toutes les interfaces réseau par les ordinateurs virtuels (trafic sortant)|RoleInstanceId, RoleId|
|Percentage CPU|Oui|Percentage CPU|Pourcentage|Average|Le pourcentage d’unités de calcul affectées actuellement utilisées par des machines virtuelles|RoleInstanceId, RoleId|


## <a name="microsoftcomputedisks"></a>microsoft.compute/disks

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Octets lus/s sur disque composite|Non|Octets lus/s sur disque (préversion)|Octets|Average|Octets lus par seconde à partir du disque pendant la période de supervision. Notez que cette métrique est en préversion et qu’elle est susceptible d’être modifiée avant d’être en disponibilité générale|Aucune dimension|
|Opérations de lecture/s sur disque composite|Non|Opérations de lecture/s sur disque (préversion)|Octets|Average|Nombre d’E/S en lecture effectuées sur un disque pendant la période de supervision. Notez que cette métrique est en préversion et qu’elle est susceptible d’être modifiée avant d’être en disponibilité générale|Aucune dimension|
|Octets écrits/s sur disque composite|Non|Octets écrits/s sur disque (préversion)|Octets|Average|Octets écrits par seconde sur le disque pendant la période de supervision. Notez que cette métrique est en préversion et qu’elle est susceptible d’être modifiée avant d’être en disponibilité générale|Aucune dimension|
|Opérations d’écriture/s sur disque composite|Non|Opérations d’écriture/s sur disque (préversion)|Octets|Average|Nombre d’E/S en écriture effectuées sur un disque pendant la période de supervision. Notez que cette métrique est en préversion et qu’elle est susceptible d’être modifiée avant d’être en disponibilité générale|Aucune dimension|


## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Octets de mémoire disponibles|Yes|Octets de mémoire disponibles (préversion)|Octets|Average|Quantité de mémoire physique, en octets, immédiatement disponible pour l’allocation à un processus ou pour une utilisation système dans la machine virtuelle|Aucune dimension|
|Crédits de processeur consommés|Oui|Crédits de processeur consommés|Count|Average|Nombre total de crédits consommés par la machine virtuelle. Disponible uniquement sur des machines virtuelles extensibles de la série B|Aucune dimension|
|Crédits de processeurs restants|Oui|Crédits de processeurs restants|Count|Average|Nombre total de crédits pouvant être consommés. Disponible uniquement sur des machines virtuelles extensibles de la série B|Aucune dimension|
|Pourcentage de bande passante du disque de données consommée|Oui|Pourcentage de bande passante du disque de données consommée|Pourcentage|Average|Pourcentage de bande passante du disque de données consommée|Numéro d'unité logique|
|Pourcentage d’IOPS du disque de données consommées|Oui|Pourcentage d’IOPS du disque de données consommées|Pourcentage|Average|Pourcentage d'E/S de disque de données consommées par minute|Numéro d'unité logique|
|Bande passante maximale en mode rafale du disque de données|Oui|Bande passante maximale en mode rafale du disque de données|Count|Average|Débit maximal en octets par seconde que le disque de données peut atteindre avec le mode rafale|Numéro d'unité logique|
|IOPS maximales en mode rafale du disque de données|Oui|IOPS maximales en mode rafale du disque de données|Count|Average|IOPS maximales que le disque de données peut atteindre avec le mode rafale|Numéro d'unité logique|
|Longueur de file d’attente du disque de données|Oui|Longueur de file d’attente du disque de données|Count|Average|Longueur de file d’attente (ou QD) du disque de données|Numéro d'unité logique|
|Octets lus/s sur disque de données|Oui|Octets lus/s sur disque de données|BytesPerSecond|Average|Octets/s lus sur un seul disque pendant la période d'analyse|Numéro d'unité logique|
|Opérations de lecture/s sur disque de données|Oui|Opérations de lecture/s sur disque de données|CountPerSecond|Average|IOPS en lecture effectuées sur un seul disque pendant la période d'analyse|Numéro d'unité logique|
|Bande passante cible du disque de données|Oui|Bande passante cible du disque de données|Count|Average|Débit d’octets par seconde de base de référence que le disque de données peut atteindre sans le mode rafale|Numéro d'unité logique|
|IOPS cibles du disque de données|Oui|IOPS cibles du disque de données|Count|Average|IOPS de base de référence que le disque de données peut atteindre sans le mode rafale|Numéro d'unité logique|
|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de données|Oui|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de données|Pourcentage|Average|Pourcentage de crédits de bande passante en mode rafale utilisés jusqu’à présent par le disque de données|Numéro d'unité logique|
|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de données|Oui|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de données|Pourcentage|Average|Pourcentage de crédits d’E/S en mode rafale utilisés jusqu’à présent par le disque de données|Numéro d'unité logique|
|Octets écrits/s sur disque de données|Oui|Octets écrits/s sur disque de données|BytesPerSecond|Average|Octets/s écrits sur un seul disque pendant la période d'analyse|Numéro d'unité logique|
|Opérations d’écriture/s sur disque de données|Oui|Opérations d’écriture/s sur disque de données|CountPerSecond|Average|IOPS en écriture effectuées sur un seul disque pendant la période d'analyse|Numéro d'unité logique|
|Disk Read Bytes|Oui|Disk Read Bytes|Octets|Total|Octets lus à partir du disque pendant la période d’analyse|Aucune dimension|
|Disk Read Operations/Sec|Oui|Disk Read Operations/Sec|CountPerSecond|Average|E/S de lecture disque par seconde|Aucune dimension|
|Disk Write Bytes|Oui|Disk Write Bytes|Octets|Total|Octets écrits sur le disque pendant la période d’analyse|Aucune dimension|
|Disk Write Operations/Sec|Oui|Disk Write Operations/Sec|CountPerSecond|Average|E/S d’écriture disque par seconde|Aucune dimension|
|Flux entrants|Oui|Flux entrants|Count|Average|Les flux entrants correspondent aux flux actuels dans le sens entrant (trafic entrant dans la machine virtuelle)|Aucune dimension|
|Taux de création maximal de flux entrants|Oui|Taux de création maximal de flux entrants|CountPerSecond|Average|Taux de création maximal de flux entrants (trafic entrant dans la machine virtuelle)|Aucune dimension|
|Network In|Oui|Octets entrants réseau facturables (déprécié)|Octets|Total|Nombre d’octets facturables reçus sur toutes les interfaces réseau par la ou les machines virtuelles (trafic entrant) (déprécié)|Aucune dimension|
|Octets entrants réseau totaux|Oui|Octets entrants réseau totaux|Octets|Total|Le nombre d’octets reçus sur toutes les interfaces réseau par les ordinateurs virtuels (trafic entrant)|Aucune dimension|
|Network Out|Oui|Octets sortants réseau facturables (déprécié)|Octets|Total|Nombre d’octets facturables envoyés sur toutes les interfaces réseau par la ou les machines virtuelles (trafic sortant) (déprécié)|Aucune dimension|
|Octets sortants réseau totaux|Oui|Octets sortants réseau totaux|Octets|Total|Le nombre d’octets envoyés sur toutes les interfaces réseau par les ordinateurs virtuels (trafic sortant)|Aucune dimension|
|Pourcentage de bande passante du disque de système d’exploitation consommée|Oui|Pourcentage de bande passante du disque de système d’exploitation consommée|Pourcentage|Average|Pourcentage de bande passante de disque de système d'exploitation consommée par minute|Numéro d'unité logique|
|Pourcentage d’IOPS du disque de système d’exploitation consommées|Oui|Pourcentage d’IOPS du disque de système d’exploitation consommées|Pourcentage|Average|Pourcentage d'E/S de disque de système d'exploitation consommées par minute|Numéro d'unité logique|
|Bande passante maximale en mode rafale du disque de système d’exploitation|Oui|Bande passante maximale en mode rafale du disque de système d’exploitation|Count|Average|Débit d’octets maximal par seconde que le disque de système d’exploitation peut atteindre avec le mode rafale|Numéro d'unité logique|
|IOPS maximales en mode rafale du disque de système d’exploitation|Oui|IOPS maximales en mode rafale du disque de système d’exploitation|Count|Average|IOPS maximales que le disque de système d’exploitation peut atteindre avec le mode rafale|Numéro d'unité logique|
|Longueur de file d’attente du disque du système d’exploitation|Oui|Longueur de file d’attente du disque du système d’exploitation|Count|Average|Longueur de file d’attente (ou QD) du disque du système d’exploitation|Aucune dimension|
|Octets lus/s sur disque du système d’exploitation|Oui|Octets lus/s sur disque du système d’exploitation|BytesPerSecond|Average|Octets/s lus sur un seul disque pendant la période d'analyse du disque du système d’exploitation|Aucune dimension|
|Opérations de lecture/s sur disque du système d’exploitation|Oui|Opérations de lecture/s sur disque du système d’exploitation|CountPerSecond|Average|IOPS en lecture effectuées sur un seul disque pendant la période d'analyse du disque du système d’exploitation|Aucune dimension|
|Bande passante cible du disque de système d’exploitation|Oui|Bande passante cible du disque de système d’exploitation|Count|Average|Débit d’octets par seconde de base de référence que le disque de système d’exploitation peut atteindre sans le mode rafale|Numéro d'unité logique|
|IOPS cibles du disque de système d’exploitation|Oui|IOPS cibles du disque de système d’exploitation|Count|Average|IOPS de base de référence que le disque de système d’exploitation peut atteindre sans le mode rafale|Numéro d'unité logique|
|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de système d’exploitation|Oui|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de système d’exploitation|Pourcentage|Average|Pourcentage de crédits de bande passante en mode rafale utilisés par le disque de système d’exploitation jusqu’à présent|Numéro d'unité logique|
|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de système d’exploitation|Oui|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de système d’exploitation|Pourcentage|Average|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de système d’exploitation jusqu’à présent|Numéro d'unité logique|
|Octets/s écrits sur disque du système d’exploitation|Oui|Octets écrits/s sur le disque de système d’exploitation|BytesPerSecond|Average|Octets/s écrits sur un seul disque pendant la période d'analyse du disque du système d’exploitation|Aucune dimension|
|Opérations d’écriture/s sur disque du système d’exploitation|Oui|Opérations d’écriture/s sur disque du système d’exploitation|CountPerSecond|Average|IOPS en écriture effectuées sur un seul disque pendant la période d'analyse du disque du système d’exploitation|Aucune dimension|
|Flux sortants|Oui|Flux sortants|Count|Average|Les flux sortants correspondent aux flux actuels dans le sens sortant (trafic sortant de la machine virtuelle)|Aucune dimension|
|Taux de création maximal de flux sortants|Oui|Taux de création maximal de flux sortants|CountPerSecond|Average|Taux de création maximal de flux sortants (trafic sortant de la machine virtuelle)|Aucune dimension|
|Percentage CPU|Oui|Percentage CPU|Pourcentage|Average|Le pourcentage d’unités de calcul affectées actuellement utilisées par des machines virtuelles|Aucune dimension|
|Accès en lecture dans le cache de disque de données Premium|Oui|Accès en lecture dans le cache de disque de données Premium|Pourcentage|Average|Accès en lecture dans le cache de disque de données Premium|Numéro d'unité logique|
|Échec de lecture dans le cache de disque de données Premium|Oui|Échec de lecture dans le cache de disque de données Premium|Pourcentage|Average|Échec de lecture dans le cache de disque de données Premium|Numéro d'unité logique|
|Accès en lecture dans le cache de disque OS Premium|Oui|Accès en lecture dans le cache de disque OS Premium|Pourcentage|Average|Accès en lecture dans le cache de disque OS Premium|Aucune dimension|
|Échec de lecture dans le cache de disque OS Premium|Oui|Échec de lecture dans le cache de disque OS Premium|Pourcentage|Average|Échec de lecture dans le cache de disque OS Premium|Aucune dimension|
|Pourcentage de bande passante en cache consommée par la machine virtuelle|Oui|Pourcentage de bande passante en cache consommée par la machine virtuelle|Pourcentage|Average|Pourcentage de bande passante de disque en cache consommée par la machine virtuelle|Aucune dimension|
|Pourcentage d’IOPS en cache de machine virtuelle consommées|Oui|Pourcentage d’IOPS en cache de machine virtuelle consommées|Pourcentage|Average|Pourcentage d'IOPS de disque en cache consommées par la machine virtuelle|Aucune dimension|
|Pourcentage de bande passante non mise en cache consommée par la machine virtuelle|Oui|Pourcentage de bande passante non mise en cache consommée par la machine virtuelle|Pourcentage|Average|Pourcentage de bande passante de disque non mise en cache consommée par la machine virtuelle|Aucune dimension|
|Pourcentage d’IOPS non mises en cache de machine virtuelle consommées|Oui|Pourcentage d’IOPS non mises en cache de machine virtuelle consommées|Pourcentage|Average|Pourcentage d'IOPS de disque non mises en cache consommées par la machine virtuelle|Aucune dimension|


## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Octets de mémoire disponibles|Yes|Octets de mémoire disponibles (préversion)|Octets|Average|Quantité de mémoire physique, en octets, immédiatement disponible pour l’allocation à un processus ou pour une utilisation système dans la machine virtuelle|VMName|
|Crédits de processeur consommés|Oui|Crédits de processeur consommés|Count|Average|Nombre total de crédits consommés par la machine virtuelle. Disponible uniquement sur des machines virtuelles extensibles de la série B|Aucune dimension|
|Crédits de processeurs restants|Oui|Crédits de processeurs restants|Count|Average|Nombre total de crédits pouvant être consommés. Disponible uniquement sur des machines virtuelles extensibles de la série B|Aucune dimension|
|Pourcentage de bande passante du disque de données consommée|Oui|Pourcentage de bande passante du disque de données consommée|Pourcentage|Average|Pourcentage de bande passante du disque de données consommée|Numéro d'unité logique, VMName|
|Pourcentage d’IOPS du disque de données consommées|Oui|Pourcentage d’IOPS du disque de données consommées|Pourcentage|Average|Pourcentage d'E/S de disque de données consommées par minute|Numéro d'unité logique, VMName|
|Bande passante maximale en mode rafale du disque de données|Oui|Bande passante maximale en mode rafale du disque de données|Count|Average|Débit maximal en octets par seconde que le disque de données peut atteindre avec le mode rafale|Numéro d'unité logique, VMName|
|IOPS maximales en mode rafale du disque de données|Oui|IOPS maximales en mode rafale du disque de données|Count|Average|IOPS maximales que le disque de données peut atteindre avec le mode rafale|Numéro d'unité logique, VMName|
|Longueur de file d’attente du disque de données|Oui|Longueur de file d’attente du disque de données|Count|Average|Longueur de file d’attente (ou QD) du disque de données|Numéro d'unité logique, VMName|
|Octets lus/s sur disque de données|Oui|Octets lus/s sur disque de données|BytesPerSecond|Average|Octets/s lus sur un seul disque pendant la période d'analyse|Numéro d'unité logique, VMName|
|Opérations de lecture/s sur disque de données|Oui|Opérations de lecture/s sur disque de données|CountPerSecond|Average|IOPS en lecture effectuées sur un seul disque pendant la période d'analyse|Numéro d'unité logique, VMName|
|Bande passante cible du disque de données|Oui|Bande passante cible du disque de données|Count|Average|Débit d’octets par seconde de base de référence que le disque de données peut atteindre sans le mode rafale|Numéro d'unité logique, VMName|
|IOPS cibles du disque de données|Oui|IOPS cibles du disque de données|Count|Average|IOPS de base de référence que le disque de données peut atteindre sans le mode rafale|Numéro d'unité logique, VMName|
|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de données|Oui|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de données|Pourcentage|Average|Pourcentage de crédits de bande passante en mode rafale utilisés jusqu’à présent par le disque de données|Numéro d'unité logique, VMName|
|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de données|Oui|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de données|Pourcentage|Average|Pourcentage de crédits d’E/S en mode rafale utilisés jusqu’à présent par le disque de données|Numéro d'unité logique, VMName|
|Octets écrits/s sur disque de données|Oui|Octets écrits/s sur disque de données|BytesPerSecond|Average|Octets/s écrits sur un seul disque pendant la période d'analyse|Numéro d'unité logique, VMName|
|Opérations d’écriture/s sur disque de données|Oui|Opérations d’écriture/s sur disque de données|CountPerSecond|Average|IOPS en écriture effectuées sur un seul disque pendant la période d'analyse|Numéro d'unité logique, VMName|
|Disk Read Bytes|Oui|Disk Read Bytes|Octets|Total|Octets lus à partir du disque pendant la période d’analyse|VMName|
|Disk Read Operations/Sec|Oui|Disk Read Operations/Sec|CountPerSecond|Average|E/S de lecture disque par seconde|VMName|
|Disk Write Bytes|Oui|Disk Write Bytes|Octets|Total|Octets écrits sur le disque pendant la période d’analyse|VMName|
|Disk Write Operations/Sec|Oui|Disk Write Operations/Sec|CountPerSecond|Average|E/S d’écriture disque par seconde|VMName|
|Flux entrants|Oui|Flux entrants|Count|Average|Les flux entrants correspondent aux flux actuels dans le sens entrant (trafic entrant dans la machine virtuelle)|VMName|
|Taux de création maximal de flux entrants|Oui|Taux de création maximal de flux entrants|CountPerSecond|Average|Taux de création maximal de flux entrants (trafic entrant dans la machine virtuelle)|VMName|
|Network In|Oui|Octets entrants réseau facturables (déprécié)|Octets|Total|Nombre d’octets facturables reçus sur toutes les interfaces réseau par la ou les machines virtuelles (trafic entrant) (déprécié)|VMName|
|Octets entrants réseau totaux|Oui|Octets entrants réseau totaux|Octets|Total|Le nombre d’octets reçus sur toutes les interfaces réseau par les ordinateurs virtuels (trafic entrant)|VMName|
|Network Out|Oui|Octets sortants réseau facturables (déprécié)|Octets|Total|Nombre d’octets facturables envoyés sur toutes les interfaces réseau par la ou les machines virtuelles (trafic sortant) (déprécié)|VMName|
|Octets sortants réseau totaux|Oui|Octets sortants réseau totaux|Octets|Total|Le nombre d’octets envoyés sur toutes les interfaces réseau par les ordinateurs virtuels (trafic sortant)|VMName|
|Pourcentage de bande passante du disque de système d’exploitation consommée|Oui|Pourcentage de bande passante du disque de système d’exploitation consommée|Pourcentage|Average|Pourcentage de bande passante de disque de système d'exploitation consommée par minute|Numéro d'unité logique, VMName|
|Pourcentage d’IOPS du disque de système d’exploitation consommées|Oui|Pourcentage d’IOPS du disque de système d’exploitation consommées|Pourcentage|Average|Pourcentage d'E/S de disque de système d'exploitation consommées par minute|Numéro d'unité logique, VMName|
|Bande passante maximale en mode rafale du disque de système d’exploitation|Oui|Bande passante maximale en mode rafale du disque de système d’exploitation|Count|Average|Débit d’octets maximal par seconde que le disque de système d’exploitation peut atteindre avec le mode rafale|Numéro d'unité logique, VMName|
|IOPS maximales en mode rafale du disque de système d’exploitation|Oui|IOPS maximales en mode rafale du disque de système d’exploitation|Count|Average|IOPS maximales que le disque de système d’exploitation peut atteindre avec le mode rafale|Numéro d'unité logique, VMName|
|Longueur de file d’attente du disque du système d’exploitation|Oui|Longueur de file d’attente du disque du système d’exploitation|Count|Average|Longueur de file d’attente (ou QD) du disque du système d’exploitation|VMName|
|Octets lus/s sur disque du système d’exploitation|Oui|Octets lus/s sur disque du système d’exploitation|BytesPerSecond|Average|Octets/s lus sur un seul disque pendant la période d'analyse du disque du système d’exploitation|VMName|
|Opérations de lecture/s sur disque du système d’exploitation|Oui|Opérations de lecture/s sur disque du système d’exploitation|CountPerSecond|Average|IOPS en lecture effectuées sur un seul disque pendant la période d'analyse du disque du système d’exploitation|VMName|
|Bande passante cible du disque de système d’exploitation|Oui|Bande passante cible du disque de système d’exploitation|Count|Average|Débit d’octets par seconde de base de référence que le disque de système d’exploitation peut atteindre sans le mode rafale|Numéro d'unité logique, VMName|
|IOPS cibles du disque de système d’exploitation|Oui|IOPS cibles du disque de système d’exploitation|Count|Average|IOPS de base de référence que le disque de système d’exploitation peut atteindre sans le mode rafale|Numéro d'unité logique, VMName|
|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de système d’exploitation|Oui|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de système d’exploitation|Pourcentage|Average|Pourcentage de crédits de bande passante en mode rafale utilisés par le disque de système d’exploitation jusqu’à présent|Numéro d'unité logique, VMName|
|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de système d’exploitation|Oui|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de système d’exploitation|Pourcentage|Average|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de système d’exploitation jusqu’à présent|Numéro d'unité logique, VMName|
|Octets/s écrits sur disque du système d’exploitation|Oui|Octets écrits/s sur le disque de système d’exploitation|BytesPerSecond|Average|Octets/s écrits sur un seul disque pendant la période d'analyse du disque du système d’exploitation|VMName|
|Opérations d’écriture/s sur disque du système d’exploitation|Oui|Opérations d’écriture/s sur disque du système d’exploitation|CountPerSecond|Average|IOPS en écriture effectuées sur un seul disque pendant la période d'analyse du disque du système d’exploitation|VMName|
|Flux sortants|Oui|Flux sortants|Count|Average|Les flux sortants correspondent aux flux actuels dans le sens sortant (trafic sortant de la machine virtuelle)|VMName|
|Taux de création maximal de flux sortants|Oui|Taux de création maximal de flux sortants|CountPerSecond|Average|Taux de création maximal de flux sortants (trafic sortant de la machine virtuelle)|VMName|
|Percentage CPU|Oui|Percentage CPU|Pourcentage|Average|Le pourcentage d’unités de calcul affectées actuellement utilisées par des machines virtuelles|VMName|
|Accès en lecture dans le cache de disque de données Premium|Oui|Accès en lecture dans le cache de disque de données Premium|Pourcentage|Average|Accès en lecture dans le cache de disque de données Premium|Numéro d'unité logique, VMName|
|Échec de lecture dans le cache de disque de données Premium|Oui|Échec de lecture dans le cache de disque de données Premium|Pourcentage|Average|Échec de lecture dans le cache de disque de données Premium|Numéro d'unité logique, VMName|
|Accès en lecture dans le cache de disque OS Premium|Oui|Accès en lecture dans le cache de disque OS Premium|Pourcentage|Average|Accès en lecture dans le cache de disque OS Premium|VMName|
|Échec de lecture dans le cache de disque OS Premium|Oui|Échec de lecture dans le cache de disque OS Premium|Pourcentage|Average|Échec de lecture dans le cache de disque OS Premium|VMName|
|Pourcentage de bande passante en cache consommée par la machine virtuelle|Oui|Pourcentage de bande passante en cache consommée par la machine virtuelle|Pourcentage|Average|Pourcentage de bande passante de disque en cache consommée par la machine virtuelle|VMName|
|Pourcentage d’IOPS en cache de machine virtuelle consommées|Oui|Pourcentage d’IOPS en cache de machine virtuelle consommées|Pourcentage|Average|Pourcentage d'IOPS de disque en cache consommées par la machine virtuelle|VMName|
|Pourcentage de bande passante non mise en cache consommée par la machine virtuelle|Oui|Pourcentage de bande passante non mise en cache consommée par la machine virtuelle|Pourcentage|Average|Pourcentage de bande passante de disque non mise en cache consommée par la machine virtuelle|VMName|
|Pourcentage d’IOPS non mises en cache de machine virtuelle consommées|Oui|Pourcentage d’IOPS non mises en cache de machine virtuelle consommées|Pourcentage|Average|Pourcentage d'IOPS de disque non mises en cache consommées par la machine virtuelle|VMName|


## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Octets de mémoire disponibles|Yes|Octets de mémoire disponibles (préversion)|Octets|Average|Quantité de mémoire physique, en octets, immédiatement disponible pour l’allocation à un processus ou pour une utilisation système dans la machine virtuelle|Aucune dimension|
|Crédits de processeur consommés|Oui|Crédits de processeur consommés|Count|Average|Nombre total de crédits consommés par la machine virtuelle. Disponible uniquement sur des machines virtuelles extensibles de la série B|Aucune dimension|
|Crédits de processeurs restants|Oui|Crédits de processeurs restants|Count|Average|Nombre total de crédits pouvant être consommés. Disponible uniquement sur des machines virtuelles extensibles de la série B|Aucune dimension|
|Pourcentage de bande passante du disque de données consommée|Oui|Pourcentage de bande passante du disque de données consommée|Pourcentage|Average|Pourcentage de bande passante du disque de données consommée|Numéro d'unité logique|
|Pourcentage d’IOPS du disque de données consommées|Oui|Pourcentage d’IOPS du disque de données consommées|Pourcentage|Average|Pourcentage d'E/S de disque de données consommées par minute|Numéro d'unité logique|
|Bande passante maximale en mode rafale du disque de données|Oui|Bande passante maximale en mode rafale du disque de données|Count|Average|Débit maximal en octets par seconde que le disque de données peut atteindre avec le mode rafale|Numéro d'unité logique|
|IOPS maximales en mode rafale du disque de données|Oui|IOPS maximales en mode rafale du disque de données|Count|Average|IOPS maximales que le disque de données peut atteindre avec le mode rafale|Numéro d'unité logique|
|Longueur de file d’attente du disque de données|Oui|Longueur de file d’attente du disque de données|Count|Average|Longueur de file d’attente (ou QD) du disque de données|Numéro d'unité logique|
|Octets lus/s sur disque de données|Oui|Octets lus/s sur disque de données|BytesPerSecond|Average|Octets/s lus sur un seul disque pendant la période d'analyse|Numéro d'unité logique|
|Opérations de lecture/s sur disque de données|Oui|Opérations de lecture/s sur disque de données|CountPerSecond|Average|IOPS en lecture effectuées sur un seul disque pendant la période d'analyse|Numéro d'unité logique|
|Bande passante cible du disque de données|Oui|Bande passante cible du disque de données|Count|Average|Débit d’octets par seconde de base de référence que le disque de données peut atteindre sans le mode rafale|Numéro d'unité logique|
|IOPS cibles du disque de données|Oui|IOPS cibles du disque de données|Count|Average|IOPS de base de référence que le disque de données peut atteindre sans le mode rafale|Numéro d'unité logique|
|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de données|Oui|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de données|Pourcentage|Average|Pourcentage de crédits de bande passante en mode rafale utilisés jusqu’à présent par le disque de données|Numéro d'unité logique|
|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de données|Oui|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de données|Pourcentage|Average|Pourcentage de crédits d’E/S en mode rafale utilisés jusqu’à présent par le disque de données|Numéro d'unité logique|
|Octets écrits/s sur disque de données|Oui|Octets écrits/s sur disque de données|BytesPerSecond|Average|Octets/s écrits sur un seul disque pendant la période d'analyse|Numéro d'unité logique|
|Opérations d’écriture/s sur disque de données|Oui|Opérations d’écriture/s sur disque de données|CountPerSecond|Average|IOPS en écriture effectuées sur un seul disque pendant la période d'analyse|Numéro d'unité logique|
|Disk Read Bytes|Oui|Disk Read Bytes|Octets|Total|Octets lus à partir du disque pendant la période d’analyse|Aucune dimension|
|Disk Read Operations/Sec|Oui|Disk Read Operations/Sec|CountPerSecond|Average|E/S de lecture disque par seconde|Aucune dimension|
|Disk Write Bytes|Oui|Disk Write Bytes|Octets|Total|Octets écrits sur le disque pendant la période d’analyse|Aucune dimension|
|Disk Write Operations/Sec|Oui|Disk Write Operations/Sec|CountPerSecond|Average|E/S d’écriture disque par seconde|Aucune dimension|
|Flux entrants|Oui|Flux entrants|Count|Average|Les flux entrants correspondent aux flux actuels dans le sens entrant (trafic entrant dans la machine virtuelle)|Aucune dimension|
|Taux de création maximal de flux entrants|Oui|Taux de création maximal de flux entrants|CountPerSecond|Average|Taux de création maximal de flux entrants (trafic entrant dans la machine virtuelle)|Aucune dimension|
|Network In|Oui|Octets entrants réseau facturables (déprécié)|Octets|Total|Nombre d’octets facturables reçus sur toutes les interfaces réseau par la ou les machines virtuelles (trafic entrant) (déprécié)|Aucune dimension|
|Octets entrants réseau totaux|Oui|Octets entrants réseau totaux|Octets|Total|Le nombre d’octets reçus sur toutes les interfaces réseau par les ordinateurs virtuels (trafic entrant)|Aucune dimension|
|Network Out|Oui|Octets sortants réseau facturables (déprécié)|Octets|Total|Nombre d’octets facturables envoyés sur toutes les interfaces réseau par la ou les machines virtuelles (trafic sortant) (déprécié)|Aucune dimension|
|Octets sortants réseau totaux|Oui|Octets sortants réseau totaux|Octets|Total|Le nombre d’octets envoyés sur toutes les interfaces réseau par les ordinateurs virtuels (trafic sortant)|Aucune dimension|
|Pourcentage de bande passante du disque de système d’exploitation consommée|Oui|Pourcentage de bande passante du disque de système d’exploitation consommée|Pourcentage|Average|Pourcentage de bande passante de disque de système d'exploitation consommée par minute|Numéro d'unité logique|
|Pourcentage d’IOPS du disque de système d’exploitation consommées|Oui|Pourcentage d’IOPS du disque de système d’exploitation consommées|Pourcentage|Average|Pourcentage d'E/S de disque de système d'exploitation consommées par minute|Numéro d'unité logique|
|Bande passante maximale en mode rafale du disque de système d’exploitation|Oui|Bande passante maximale en mode rafale du disque de système d’exploitation|Count|Average|Débit d’octets maximal par seconde que le disque de système d’exploitation peut atteindre avec le mode rafale|Numéro d'unité logique|
|IOPS maximales en mode rafale du disque de système d’exploitation|Oui|IOPS maximales en mode rafale du disque de système d’exploitation|Count|Average|IOPS maximales que le disque de système d’exploitation peut atteindre avec le mode rafale|Numéro d'unité logique|
|Longueur de file d’attente du disque du système d’exploitation|Oui|Longueur de file d’attente du disque du système d’exploitation|Count|Average|Longueur de file d’attente (ou QD) du disque du système d’exploitation|Aucune dimension|
|Octets lus/s sur disque du système d’exploitation|Oui|Octets lus/s sur disque du système d’exploitation|BytesPerSecond|Average|Octets/s lus sur un seul disque pendant la période d'analyse du disque du système d’exploitation|Aucune dimension|
|Opérations de lecture/s sur disque du système d’exploitation|Oui|Opérations de lecture/s sur disque du système d’exploitation|CountPerSecond|Average|IOPS en lecture effectuées sur un seul disque pendant la période d'analyse du disque du système d’exploitation|Aucune dimension|
|Bande passante cible du disque de système d’exploitation|Oui|Bande passante cible du disque de système d’exploitation|Count|Average|Débit d’octets par seconde de base de référence que le disque de système d’exploitation peut atteindre sans le mode rafale|Numéro d'unité logique|
|IOPS cibles du disque de système d’exploitation|Oui|IOPS cibles du disque de système d’exploitation|Count|Average|IOPS de base de référence que le disque de système d’exploitation peut atteindre sans le mode rafale|Numéro d'unité logique|
|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de système d’exploitation|Oui|Pourcentage de crédits d’octets par seconde en mode rafale utilisés par le disque de système d’exploitation|Pourcentage|Average|Pourcentage de crédits de bande passante en mode rafale utilisés par le disque de système d’exploitation jusqu’à présent|Numéro d'unité logique|
|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de système d’exploitation|Oui|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de système d’exploitation|Pourcentage|Average|Pourcentage de crédits d’E/S en mode rafale utilisés par le disque de système d’exploitation jusqu’à présent|Numéro d'unité logique|
|Octets/s écrits sur disque du système d’exploitation|Oui|Octets écrits/s sur le disque de système d’exploitation|BytesPerSecond|Average|Octets/s écrits sur un seul disque pendant la période d'analyse du disque du système d’exploitation|Aucune dimension|
|Opérations d’écriture/s sur disque du système d’exploitation|Oui|Opérations d’écriture/s sur disque du système d’exploitation|CountPerSecond|Average|IOPS en écriture effectuées sur un seul disque pendant la période d'analyse du disque du système d’exploitation|Aucune dimension|
|Flux sortants|Oui|Flux sortants|Count|Average|Les flux sortants correspondent aux flux actuels dans le sens sortant (trafic sortant de la machine virtuelle)|Aucune dimension|
|Taux de création maximal de flux sortants|Oui|Taux de création maximal de flux sortants|CountPerSecond|Average|Taux de création maximal de flux sortants (trafic sortant de la machine virtuelle)|Aucune dimension|
|Percentage CPU|Oui|Percentage CPU|Pourcentage|Average|Le pourcentage d’unités de calcul affectées actuellement utilisées par des machines virtuelles|Aucune dimension|
|Accès en lecture dans le cache de disque de données Premium|Oui|Accès en lecture dans le cache de disque de données Premium|Pourcentage|Average|Accès en lecture dans le cache de disque de données Premium|Numéro d'unité logique|
|Échec de lecture dans le cache de disque de données Premium|Oui|Échec de lecture dans le cache de disque de données Premium|Pourcentage|Average|Échec de lecture dans le cache de disque de données Premium|Numéro d'unité logique|
|Accès en lecture dans le cache de disque OS Premium|Oui|Accès en lecture dans le cache de disque OS Premium|Pourcentage|Average|Accès en lecture dans le cache de disque OS Premium|Aucune dimension|
|Échec de lecture dans le cache de disque OS Premium|Oui|Échec de lecture dans le cache de disque OS Premium|Pourcentage|Average|Échec de lecture dans le cache de disque OS Premium|Aucune dimension|
|Pourcentage de bande passante en cache consommée par la machine virtuelle|Oui|Pourcentage de bande passante en cache consommée par la machine virtuelle|Pourcentage|Average|Pourcentage de bande passante de disque en cache consommée par la machine virtuelle|Aucune dimension|
|Pourcentage d’IOPS en cache de machine virtuelle consommées|Oui|Pourcentage d’IOPS en cache de machine virtuelle consommées|Pourcentage|Average|Pourcentage d'IOPS de disque en cache consommées par la machine virtuelle|Aucune dimension|
|Pourcentage de bande passante non mise en cache consommée par la machine virtuelle|Oui|Pourcentage de bande passante non mise en cache consommée par la machine virtuelle|Pourcentage|Average|Pourcentage de bande passante de disque non mise en cache consommée par la machine virtuelle|Aucune dimension|
|Pourcentage d’IOPS non mises en cache de machine virtuelle consommées|Oui|Pourcentage d’IOPS non mises en cache de machine virtuelle consommées|Pourcentage|Average|Pourcentage d'IOPS de disque non mises en cache consommées par la machine virtuelle|Aucune dimension|


## <a name="microsoftconnectedvehicleplatformaccounts"></a>Microsoft.ConnectedVehicle/platformAccounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ClaimsProviderRequestLatency|Yes|Durée d’exécution des requêtes de revendication|Millisecondes|Average|Durée d’exécution moyenne des requêtes au point de terminaison du fournisseur de revendications client, en millisecondes|VehicleId, DeviceName|
|ClaimsProviderRequests|Yes|Requêtes au fournisseur de revendications|Nombre|Total|Nombre de requêtes adressées au fournisseur de revendications|VehicleId, DeviceName|
|ConnectionServiceRequestRuntime|Yes|Durée d’exécution de la demande de service de connexion de véhicule|Millisecondes|Average|Durée moyenne d’exécution de la demande de connexion au véhicule, en millisecondes|VehicleId, DeviceName|
|ConnectionServiceRequests|Yes|Demandes de service de connexion au véhicule|Nombre|Total|Nombre total de demandes de connexion au véhicule|VehicleId, DeviceName|
|ProvisionerServiceRequestRuntime|Yes|Durée d’exécution de l’approvisionnement du véhicule|Millisecondes|Average|Durée d’exécution moyenne des demandes d’approvisionnement de véhicule, en millisecondes|VehicleId, DeviceName|
|ProvisionerServiceRequests|Yes|Demandes de service d’approvisionnement de véhicule|Nombre|Total|Nombre total de demandes d’approvisionnement de véhicule|VehicleId, DeviceName|
|StateStoreReadRequestLatency|Yes|Durée d’exécution des lectures du magasin d’état|Millisecondes|Average|Durée moyenne d’exécution des demandes de lecture du magasin d’état, en millisecondes|VehicleId, DeviceName|
|StateStoreReadRequests|Yes|Demandes de lecture du magasin d’état|Nombre|Total|Nombre de demandes de lecture adressées au magasin d’état|VehicleId, DeviceName|
|StateStoreWriteRequestLatency|Yes|Durée d’exécution des écritures dans le magasin d’état|Millisecondes|Average|Durée moyenne d’exécution des demandes d’écriture dans le magasin d’état, en millisecondes|VehicleId, DeviceName|
|StateStoreWriteRequests|Yes|Demandes d’écriture dans le magasin d’état|Nombre|Total|Nombre de demandes d’écriture adressées au magasin d’état|VehicleId, DeviceName|


## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|CpuUsage|Oui|Utilisation du processeur|Count|Average|Utilisation du processeur sur tous les cœurs en millicores.|containerName|
|MemoryUsage|Oui|Utilisation de la mémoire|Octets|Average|Utilisation de la mémoire totale en octets.|containerName|
|NetworkBytesReceivedPerSecond|Oui|Octets réseau reçus par seconde|Octets|Average|Octets réseau reçus par seconde.|Aucune dimension|
|NetworkBytesTransmittedPerSecond|Oui|Octets réseau transmis par seconde|Octets|Average|Octets réseau transmis par seconde.|Aucune dimension|


## <a name="microsoftcontainerregistryregistries"></a>Microsoft.ContainerRegistry/registries

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AgentPoolCPUTime|Oui|Temps processeur du pool d’agents|Secondes|Total|Temps processeur du pool d’agents en secondes|Aucune dimension|
|RunDuration|Oui|Durée d’exécution|Millisecondes|Total|Durée d’exécution en millisecondes|Aucune dimension|
|StorageUsed|Oui|Stockage utilisé|Octets|Average|Quantité de stockage utilisée par le registre de conteneurs. Pour un compte de registre, il s’agit de la somme de la capacité utilisée par tous les référentiels au sein d’un registre. Il s’agit de la somme de la capacité utilisée par les couches partagées, les fichiers manifeste et les copies de réplica dans chacun de ses référentiels|Géolocalisation|
|SuccessfulPullCount|Oui|Nombre d'extractions réussies|Count|Total|Nombre d'images extraites réussies|Aucune dimension|
|SuccessfulPushCount|Oui|Nombre d'envois (push) réussis|Count|Total|Nombre d'images envoyées (push) réussies|Aucune dimension|
|TotalPullCount|Oui|Nombre total d'extractions|Count|Total|Nombre total d'images extraites|Aucune dimension|
|TotalPushCount|Oui|Nombre total d'envois (push)|Count|Total|Nombre total d'images envoyées (push)|Aucune dimension|


## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|apiserver_current_inflight_requests|Non|Demandes en cours|Count|Average|Nombre maximal de demandes en cours actuellement utilisées sur le serveur d’API par type de demande au cours de la dernière seconde|requestKind|
|cluster_autoscaler_cluster_safe_to_autoscale|Non|Intégrité des clusters|Count|Average|Détermine si l’autoscaler de cluster entreprend ou non une action sur le cluster|Aucune dimension|
|cluster_autoscaler_scale_down_in_cooldown|Non|Recharge de scale-down|Count|Average|Détermine si le scale-down est en recharge. Aucun nœud n’est supprimé au cours de cette période|Aucune dimension|
|cluster_autoscaler_unneeded_nodes_count|Non|Nœuds inutiles|Count|Average|L’autoscaler de cluster marque ces nœuds comme candidats à la suppression et ils sont finalement supprimés|Aucune dimension|
|cluster_autoscaler_unschedulable_pods_count|Non|Pods non planifiables|Count|Average|Nombre de pods actuellement non planifiables dans le cluster|Aucune dimension|
|kube_node_status_allocatable_cpu_cores|Non|Nombre total de cœurs d’unité centrale disponibles dans un cluster géré|Count|Average|Nombre total de cœurs d’unité centrale disponibles dans un cluster géré|Aucune dimension|
|kube_node_status_allocatable_memory_bytes|Non|Quantité totale de mémoire disponible dans un cluster géré|Octets|Average|Quantité totale de mémoire disponible dans un cluster géré|Aucune dimension|
|kube_node_status_condition|Non|États pour les différentes conditions de nœud|Count|Average|États pour les différentes conditions de nœud|condition, status, status2, node|
|kube_pod_status_phase|Non|Nombre de pods par phase|Count|Average|Nombre de pods par phase|phase, espace de noms, pod|
|kube_pod_status_ready|Non|Nombre de pods en état Prêt|Count|Average|Nombre de pods en état Prêt|espace de noms, pod, condition|
|node_cpu_usage_millicores|Oui|Utilisation de l’UC en millicœurs|MilliCores|Moyenne|Mesure agrégée de l’utilisation du processeur sur le cluster en millicœurs|node, nodepool|
|node_cpu_usage_percentage|Oui|Pourcentage d’utilisation du processeur|Pourcentage|Average|Moyenne agrégée d’utilisation du processeur, mesurée en pourcentage sur le cluster|node, nodepool|
|node_disk_usage_bytes|Oui|Octets utilisés sur le disque|Octets|Average|Espace disque utilisé en octets par appareil|node, nodepool, device|
|node_disk_usage_percentage|Oui|Pourcentage d’utilisation du disque|Pourcentage|Average|Espace disque utilisé en pourcentage par appareil|node, nodepool, device|
|node_memory_rss_bytes|Oui|Mémoire RSS en octets|Octets|Average|Mémoire RSS du conteneur utilisée, en octets|node, nodepool|
|node_memory_rss_percentage|Oui|Mémoire RSS en pourcentage|Pourcentage|Average|Mémoire RSS du conteneur utilisée, en pourcentage|node, nodepool|
|node_memory_working_set_bytes|Oui|Plage de travail de la mémoire en octets|Octets|Average|Mémoire de plage de travail du conteneur utilisée, en octets|node, nodepool|
|node_memory_working_set_percentage|Oui|Mémoire de plage de travail en pourcentage|Pourcentage|Average|Mémoire de plage de travail du conteneur utilisée, en pourcentage|node, nodepool|
|node_network_in_bytes|Oui|Réseau en octets|Octets|Average|Octets reçus sur le réseau|node, nodepool|
|node_network_out_bytes|Oui|Octets de sortie réseau|Octets|Average|Octets réseau transmis|node, nodepool|


## <a name="microsoftcustomprovidersresourceproviders"></a>Microsoft.CustomProviders/resourceproviders

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|FailedRequests|Oui|Demandes ayant échoué|Count|Total|Obtient les journaux disponibles pour les fournisseurs de ressources personnalisés|HttpMethod, CallPath, StatusCode|
|SuccessfullRequests|Oui|Requêtes ayant réussi|Count|Total|Demandes réussies effectuées par le fournisseur personnalisé|HttpMethod, CallPath, StatusCode|


## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft.DataBoxEdge/dataBoxEdgeDevices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AvailableCapacity|Oui|Capacité disponible|Octets|Average|Capacité disponible en octets pendant la période du rapport.|Aucune dimension|
|BytesUploadedToCloud|Oui|Octets cloud chargés (appareil)|Octets|Average|Nombre total d’octets chargés vers Azure à partir d’un appareil pendant la période du rapport.|Aucune dimension|
|BytesUploadedToCloudPerShare|Oui|Octets cloud chargés (partage)|Octets|Average|Nombre total d’octets chargés vers Azure à partir d’un partage pendant la période du rapport.|Partager|
|CloudReadThroughput|Oui|Débit de téléchargement cloud|BytesPerSecond|Average|Débit de téléchargement cloud vers Azure pendant la période du rapport.|Aucune dimension|
|CloudReadThroughputPerShare|Oui|Débit de téléchargement cloud (partage)|BytesPerSecond|Average|Débit de téléchargement vers Azure à partir d’un partage pendant la période du rapport.|Partager|
|CloudUploadThroughput|Oui|Débit de chargement cloud|BytesPerSecond|Average|Débit de chargement cloud vers Azure pendant la période du rapport.|Aucune dimension|
|CloudUploadThroughputPerShare|Oui|Débit de chargement cloud (partage)|BytesPerSecond|Average|Débit de chargement vers Azure à partir d’un partage pendant la période du rapport.|Partager|
|HyperVMemoryUtilization|Oui|Computing en périphérie - Utilisation de la mémoire|Pourcentage|Average|Quantité de RAM utilisée|InstanceName|
|HyperVVirtualProcessorUtilization|Oui|Computing en périphérie - Pourcentage du processeur|Pourcentage|Average|Pourcentage d’utilisation du processeur|InstanceName|
|NICReadThroughput|Oui|Débit de lecture (réseau)|BytesPerSecond|Average|Débit de lecture de l’interface réseau de l’appareil dans la période du rapport pour tous les volumes de la passerelle.|InstanceName|
|NICWriteThroughput|Oui|Débit d’écriture (réseau)|BytesPerSecond|Average|Débit d'écriture de l’interface réseau de l’appareil dans la période du rapport pour tous les volumes de la passerelle.|InstanceName|
|TotalCapacity|Oui|Capacité totale|Octets|Average|Capacité totale de l’appareil en octets pendant la période de reporting.|Aucune dimension|


## <a name="microsoftdatacollaborationworkspaces"></a>Microsoft.DataCollaboration/workspaces

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|DataAssetCount|Oui|Ressources de données créées|Count|Maximale|Nombre de ressources de données créées|DataAssetName|
|PipelineCount|Oui|Pipelines créés|Count|Maximale|Nombre de pipelines créés|PipelineName|
|ProposalCount|Oui|Propositions créées|Count|Maximale|Nombre de propositions créées|ProposalName|
|ScriptCount|Oui|Scripts créés|Count|Maximale|Nombre de scripts créés|ScriptName|


## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|FailedRuns|Oui|Exécutions échouées|Count|Total||pipelineName, activityName|
|SuccessfulRuns|Oui|Exécutions réussies|Count|Total||pipelineName, activityName|


## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActivityCancelledRuns|Oui|Métriques d’exécutions d’activité annulées|Count|Total||ActivityType, PipelineName, FailureType, Name|
|ActivityFailedRuns|Oui|Métriques d’exécutions d’activité ayant échoué|Count|Total||ActivityType, PipelineName, FailureType, Name|
|ActivitySucceededRuns|Oui|Métriques d’exécutions d’activité ayant abouti|Count|Total||ActivityType, PipelineName, FailureType, Name|
|FactorySizeInGbUnits|Oui|Taille de fabrique totale (unité Go)|Count|Maximale||Aucune dimension|
|IntegrationRuntimeAvailableMemory|Oui|Mémoire disponible du runtime d’intégration|Octets|Average||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeAvailableNodeNumber|Oui|Nombre de nœuds disponibles pour le runtime d’intégration|Count|Average||IntegrationRuntimeName|
|IntegrationRuntimeAverageTaskPickupDelay|Oui|Durée de la file d’attente du runtime d’intégration|Secondes|Average||IntegrationRuntimeName|
|IntegrationRuntimeCpuPercentage|Oui|Utilisation du processeur du runtime d’intégration|Pourcentage|Average||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeQueueLength|Oui|Longueur de la file d’attente du runtime d’intégration|Count|Average||IntegrationRuntimeName|
|MaxAllowedFactorySizeInGbUnits|Oui|Taille maximale de fabrique autorisée (unité Go)|Count|Maximale||Aucune dimension|
|MaxAllowedResourceCount|Oui|Nombre maximal d’entités autorisées|Count|Maximale||Aucune dimension|
|PipelineCancelledRuns|Oui|Métriques d’exécutions de pipeline annulées|Count|Total||FailureType, Name|
|PipelineElapsedTimeRuns|Oui|Métriques d’exécutions de pipeline - Temps écoulé|Count|Total||RunId, Nom|
|PipelineFailedRuns|Oui|Métriques d’exécutions de pipeline ayant échoué|Count|Total||FailureType, Name|
|PipelineSucceededRuns|Oui|Métriques d’exécutions de pipeline ayant abouti|Count|Total||FailureType, Name|
|ResourceCount|Oui|Nombre total d’entités|Count|Maximale||Aucune dimension|
|SSISIntegrationRuntimeStartCancel|Oui|Métriques de démarrages du runtime d’intégration de SSIS annulés|Count|Total||IntegrationRuntimeName|
|SSISIntegrationRuntimeStartFailed|Oui|Métriques de démarrages du runtime d’intégration de SSIS ayant échoué|Count|Total||IntegrationRuntimeName|
|SSISIntegrationRuntimeStartSucceeded|Oui|Métriques de démarrages du runtime d’intégration de SSIS réussis|Count|Total||IntegrationRuntimeName|
|SSISIntegrationRuntimeStopStuck|Oui|Métriques d’arrêts du runtime d’intégration de SSIS bloqués|Count|Total||IntegrationRuntimeName|
|SSISIntegrationRuntimeStopSucceeded|Oui|Métriques d’arrêts du runtime d’intégration de SSIS réussis|Count|Total||IntegrationRuntimeName|
|SSISPackageExecutionCancel|Oui|Métriques d’exécutions du package SSIS annulées|Count|Total||IntegrationRuntimeName|
|SSISPackageExecutionFailed|Oui|Métriques d’exécutions de package SSIS ayant échouées|Count|Total||IntegrationRuntimeName|
|SSISPackageExecutionSucceeded|Oui|Métriques d’exécutions de package SSIS ayant réussies|Count|Total||IntegrationRuntimeName|
|TriggerCancelledRuns|Oui|Métriques d’exécutions de déclencheur annulées|Count|Total||Name, FailureType|
|TriggerFailedRuns|Oui|Métriques d’exécutions de déclencheur ayant échoué|Count|Total||Name, FailureType|
|TriggerSucceededRuns|Oui|Métriques d’exécutions de déclencheur ayant abouti|Count|Total||Name, FailureType|


## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|JobAUEndedCancelled|Oui|Durée AU d’annulation|Secondes|Total|Durée AU totale des travaux annulés|Aucune dimension|
|JobAUEndedFailure|Oui|Durée AU d’échec|Secondes|Total|Durée AU totale des travaux en échec|Aucune dimension|
|JobAUEndedSuccess|Oui|Durée AU de réussite|Secondes|Total|Durée AU totale des travaux réussis.|Aucune dimension|
|JobEndedCancelled|Oui|Travaux annulés|Count|Total|Nombre de travaux annulés|Aucune dimension|
|JobEndedFailure|Oui|Travaux en échec|Count|Total|Nombre de travaux en échec|Aucune dimension|
|JobEndedSuccess|Oui|Travaux réussis|Count|Total|Nombre de travaux réussis|Aucune dimension|
|JobStage|Oui|Travaux dans la phase|Count|Total|Nombre de travaux à chaque phase.|Aucune dimension|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|DataRead|Oui|Données lues|Octets|Total|Volume total de données lues à partir du compte.|Aucune dimension|
|DataWritten|Oui|Données écrites|Octets|Total|Volume total de données écrites dans le compte.|Aucune dimension|
|ReadRequests|Oui|Demandes de lecture|Count|Total|Nombre de demandes de lecture de données sur le compte.|Aucune dimension|
|TotalStorage|Oui|Stockage total|Octets|Maximale|Volume total de données stockées dans le compte.|Aucune dimension|
|WriteRequests|Oui|Demandes d’écriture|Count|Total|Nombre de demandes d’écriture de données sur le compte.|Aucune dimension|


## <a name="microsoftdatashareaccounts"></a>Microsoft.DataShare/accounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|FailedShareSubscriptionSynchronizations|Oui|Captures instantanées de partages reçus ayant échoué|Count|Count|Nombre de captures instantanées de partages reçus ayant échoué dans le compte|Aucune dimension|
|FailedShareSynchronizations|Oui|Captures instantanées de partages envoyés ayant échoué|Count|Count|Nombre de captures instantanées de partages envoyés ayant échoué dans le compte|Aucune dimension|
|ShareCount|Oui|Partages envoyés|Count|Maximale|Nombre de partages envoyés dans le compte|ShareName|
|ShareSubscriptionCount|Oui|Partages reçus|Count|Maximale|Nombre de partages reçus dans le compte|ShareSubscriptionCount|
|SucceededShareSubscriptionSynchronizations|Oui|Captures instantanées de partages reçus ayant abouti|Count|Count|Nombre de captures instantanées de partages reçus ayant abouti dans le compte|Aucune dimension|
|SucceededShareSynchronizations|Oui|Captures instantanées de partages envoyés ayant abouti|Count|Count|Nombre de captures instantanées de partages envoyés ayant abouti dans le compte|Aucune dimension|


## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Oui|Connexions actives|Count|Average|Connexions actives|Aucune dimension|
|backup_storage_used|Oui|Stockage de sauvegarde utilisé|Octets|Average|Stockage de sauvegarde utilisé|Aucune dimension|
|connections_failed|Oui|Connexions ayant échoué|Count|Total|Connexions ayant échoué|Aucune dimension|
|cpu_percent|Oui|Pourcentage d’UC|Pourcentage|Average|Pourcentage d’UC|Aucune dimension|
|io_consumption_percent|Oui|Pourcentage d’E/S|Pourcentage|Average|Pourcentage d’E/S|Aucune dimension|
|memory_percent|Oui|Pourcentage de mémoire|Pourcentage|Average|Pourcentage de mémoire|Aucune dimension|
|network_bytes_egress|Oui|Network Out|Octets|Total|Sortie réseau entre connexions actives|Aucune dimension|
|network_bytes_ingress|Oui|Network In|Octets|Total|Entrée réseau entre connexions actives|Aucune dimension|
|seconds_behind_master|Oui|Décalage de la réplication en secondes|Count|Maximale|Décalage de la réplication en secondes|Aucune dimension|
|serverlog_storage_limit|Oui|Limite de stockage du journal du serveur|Octets|Maximale|Limite de stockage du journal du serveur|Aucune dimension|
|serverlog_storage_percent|Oui|Pourcentage de stockage du journal du serveur|Pourcentage|Average|Pourcentage de stockage du journal du serveur|Aucune dimension|
|serverlog_storage_usage|Oui|Stockage du journal du serveur utilisé|Octets|Average|Stockage du journal du serveur utilisé|Aucune dimension|
|storage_limit|Oui|Limite de stockage|Octets|Maximale|Limite de stockage|Aucune dimension|
|storage_percent|Oui|Pourcentage de stockage|Pourcentage|Average|Pourcentage de stockage|Aucune dimension|
|storage_used|Oui|Stockage utilisé|Octets|Average|Stockage utilisé|Aucune dimension|


## <a name="microsoftdbformysqlflexibleservers"></a>Microsoft.DBforMySQL/flexibleServers

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|aborted_connections|Oui|Connexions abandonnées|Count|Total|Connexions abandonnées|Aucune dimension|
|active_connections|Oui|Connexions actives|Count|Maximale|Connexions actives|Aucune dimension|
|backup_storage_used|Oui|Stockage de sauvegarde utilisé|Octets|Maximale|Stockage de sauvegarde utilisé|Aucune dimension|
|cpu_percent|Oui|Pourcentage de processeur hôte|Pourcentage|Maximale|Pourcentage de processeur hôte|Aucune dimension|
|io_consumption_percent|Oui|Pourcentage d’E/S|Pourcentage|Maximale|Pourcentage d’E/S|Aucune dimension|
|memory_percent|Oui|Pourcentage de mémoire hôte|Pourcentage|Maximale|Pourcentage de mémoire hôte|Aucune dimension|
|network_bytes_egress|Oui|Sortie du réseau hôte|Octets|Total|Sortie du réseau hôte en octets|Aucune dimension|
|network_bytes_ingress|Oui|Réseau entrant hôte|Octets|Total|Entrée du réseau hôte en octets|Aucune dimension|
|Requêtes|Oui|Requêtes|Count|Total|Requêtes|Aucune dimension|
|replication_lag|Oui|Décalage de la réplication en secondes|Secondes|Maximale|Décalage de la réplication en secondes|Aucune dimension|
|storage_limit|Oui|Limite de stockage|Octets|Maximale|Limite de stockage|Aucune dimension|
|storage_percent|Oui|Pourcentage de stockage|Pourcentage|Maximale|Pourcentage de stockage|Aucune dimension|
|storage_used|Oui|Stockage utilisé|Octets|Maximale|Stockage utilisé|Aucune dimension|
|total_connections|Oui|Nombre total de connexions|Count|Total|Nombre total de connexions|Aucune dimension|


## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Oui|Connexions actives|Count|Average|Connexions actives|Aucune dimension|
|backup_storage_used|Oui|Stockage de sauvegarde utilisé|Octets|Average|Stockage de sauvegarde utilisé|Aucune dimension|
|connections_failed|Oui|Connexions ayant échoué|Count|Total|Connexions ayant échoué|Aucune dimension|
|cpu_percent|Oui|Pourcentage d’UC|Pourcentage|Average|Pourcentage d’UC|Aucune dimension|
|io_consumption_percent|Oui|Pourcentage d’E/S|Pourcentage|Average|Pourcentage d’E/S|Aucune dimension|
|memory_percent|Oui|Pourcentage de mémoire|Pourcentage|Average|Pourcentage de mémoire|Aucune dimension|
|network_bytes_egress|Oui|Network Out|Octets|Total|Sortie réseau entre connexions actives|Aucune dimension|
|network_bytes_ingress|Oui|Network In|Octets|Total|Entrée réseau entre connexions actives|Aucune dimension|
|seconds_behind_master|Oui|Décalage de la réplication en secondes|Count|Maximale|Décalage de la réplication en secondes|Aucune dimension|
|serverlog_storage_limit|Oui|Limite de stockage du journal du serveur|Octets|Maximale|Limite de stockage du journal du serveur|Aucune dimension|
|serverlog_storage_percent|Oui|Pourcentage de stockage du journal du serveur|Pourcentage|Average|Pourcentage de stockage du journal du serveur|Aucune dimension|
|serverlog_storage_usage|Oui|Stockage du journal du serveur utilisé|Octets|Average|Stockage du journal du serveur utilisé|Aucune dimension|
|storage_limit|Oui|Limite de stockage|Octets|Maximale|Limite de stockage|Aucune dimension|
|storage_percent|Oui|Pourcentage de stockage|Pourcentage|Average|Pourcentage de stockage|Aucune dimension|
|storage_used|Oui|Stockage utilisé|Octets|Average|Stockage utilisé|Aucune dimension|


## <a name="microsoftdbforpostgresqlflexibleservers"></a>Microsoft.DBforPostgreSQL/flexibleServers

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Oui|Connexions actives|Count|Average|Connexions actives|Aucune dimension|
|backup_storage_used|Oui|Stockage de sauvegarde utilisé|Octets|Average|Stockage de sauvegarde utilisé|Aucune dimension|
|connections_failed|Oui|Connexions ayant échoué|Count|Total|Connexions ayant échoué|Aucune dimension|
|connections_succeeded|Oui|Connexions ayant abouti|Count|Total|Connexions ayant abouti|Aucune dimension|
|cpu_credits_consumed|Oui|Crédits de processeur consommés|Count|Average|Nombre total de crédits consommés par le serveur de base de données|Aucune dimension|
|cpu_credits_remaining|Oui|Crédits de processeurs restants|Count|Average|Nombre total de crédits pouvant être consommés|Aucune dimension|
|cpu_percent|Oui|Pourcentage d’UC|Pourcentage|Average|Pourcentage d’UC|Aucune dimension|
|disk_queue_depth|Oui|Longueur de file d’attente de disque|Count|Average|Nombre d’opérations d’E/S en attente sur le disque de données|Aucune dimension|
|iops|Oui|E/S par seconde|Count|Average|Opérations d'E/S par seconde|Aucune dimension|
|maximum_used_transactionIDs|Oui|Nombre maximal d’ID de transaction utilisés|Count|Average|Nombre maximal d’ID de transaction utilisés|Aucune dimension|
|memory_percent|Oui|Pourcentage de mémoire|Pourcentage|Average|Pourcentage de mémoire|Aucune dimension|
|network_bytes_egress|Oui|Network Out|Octets|Total|Sortie réseau entre connexions actives|Aucune dimension|
|network_bytes_ingress|Oui|Network In|Octets|Total|Entrée réseau entre connexions actives|Aucune dimension|
|read_iops|Oui|E/S par seconde en lecture|Count|Average|Nombre d’opérations de lecture d’E/S de disque de données par seconde|Aucune dimension|
|read_throughput|Oui|Débit de lecture en octets/s|Count|Average|Octets lus par seconde à partir du disque de données pendant la période d’analyse|Aucune dimension|
|storage_free|Oui|Espace de stockage libre|Octets|Average|Espace de stockage libre|Aucune dimension|
|storage_percent|Oui|Pourcentage de stockage|Pourcentage|Average|Pourcentage de stockage|Aucune dimension|
|storage_used|Oui|Stockage utilisé|Octets|Average|Stockage utilisé|Aucune dimension|
|txlogs_storage_used|Oui|Stockage des journaux des transactions utilisé|Octets|Average|Stockage des journaux des transactions utilisé|Aucune dimension|
|write_iops|Oui|E/S par seconde en écriture|Count|Average|Nombre d’opérations d’écriture d’E/S de disque de données par seconde|Aucune dimension|
|write_throughput|Oui|Débit d’écriture en octets/s|Count|Average|Octets écrits par seconde sur le disque de données pendant la période d’analyse|Aucune dimension|


## <a name="microsoftdbforpostgresqlservergroupsv2"></a>Microsoft.DBForPostgreSQL/serverGroupsv2

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Oui|Connexions actives|Count|Average|Connexions actives|ServerName|
|cpu_percent|Oui|Pourcentage d’UC|Pourcentage|Average|Pourcentage d’UC|ServerName|
|iops|Oui|E/S par seconde|Count|Average|Opérations d’E/S par seconde|ServerName|
|memory_percent|Oui|Pourcentage de mémoire|Pourcentage|Average|Pourcentage de mémoire|ServerName|
|network_bytes_egress|Oui|Network Out|Octets|Total|Sortie réseau entre connexions actives|ServerName|
|network_bytes_ingress|Oui|Network In|Octets|Total|Entrée réseau entre connexions actives|ServerName|
|storage_percent|Oui|Pourcentage de stockage|Pourcentage|Average|Pourcentage de stockage|ServerName|
|storage_used|Oui|Stockage utilisé|Octets|Average|Stockage utilisé|ServerName|


## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Oui|Connexions actives|Count|Average|Connexions actives|Aucune dimension|
|backup_storage_used|Oui|Stockage de sauvegarde utilisé|Octets|Average|Stockage de sauvegarde utilisé|Aucune dimension|
|connections_failed|Oui|Connexions ayant échoué|Count|Total|Connexions ayant échoué|Aucune dimension|
|cpu_percent|Oui|Pourcentage d’UC|Pourcentage|Average|Pourcentage d’UC|Aucune dimension|
|io_consumption_percent|Oui|Pourcentage d’E/S|Pourcentage|Average|Pourcentage d’E/S|Aucune dimension|
|memory_percent|Oui|Pourcentage de mémoire|Pourcentage|Average|Pourcentage de mémoire|Aucune dimension|
|network_bytes_egress|Oui|Network Out|Octets|Total|Sortie réseau entre connexions actives|Aucune dimension|
|network_bytes_ingress|Oui|Network In|Octets|Total|Entrée réseau entre connexions actives|Aucune dimension|
|pg_replica_log_delay_in_bytes|Oui|Retard maximum entre réplicas|Octets|Maximale|Retard en octets du réplica le plus en retard|Aucune dimension|
|pg_replica_log_delay_in_seconds|Oui|Retard du réplica|Secondes|Maximale|Retard du réplica en secondes|Aucune dimension|
|serverlog_storage_limit|Oui|Limite de stockage du journal du serveur|Octets|Maximale|Limite de stockage du journal du serveur|Aucune dimension|
|serverlog_storage_percent|Oui|Pourcentage de stockage du journal du serveur|Pourcentage|Average|Pourcentage de stockage du journal du serveur|Aucune dimension|
|serverlog_storage_usage|Oui|Stockage du journal du serveur utilisé|Octets|Average|Stockage du journal du serveur utilisé|Aucune dimension|
|storage_limit|Oui|Limite de stockage|Octets|Maximale|Limite de stockage|Aucune dimension|
|storage_percent|Oui|Pourcentage de stockage|Pourcentage|Average|Pourcentage de stockage|Aucune dimension|
|storage_used|Oui|Stockage utilisé|Octets|Average|Stockage utilisé|Aucune dimension|


## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|active_connections|Oui|Connexions actives|Count|Average|Connexions actives|Aucune dimension|
|cpu_percent|Oui|Pourcentage d’UC|Pourcentage|Average|Pourcentage d’UC|Aucune dimension|
|iops|Oui|E/S par seconde|Count|Average|Opérations d'E/S par seconde|Aucune dimension|
|memory_percent|Oui|Pourcentage de mémoire|Pourcentage|Average|Pourcentage de mémoire|Aucune dimension|
|network_bytes_egress|Oui|Network Out|Octets|Total|Sortie réseau entre connexions actives|Aucune dimension|
|network_bytes_ingress|Oui|Network In|Octets|Total|Entrée réseau entre connexions actives|Aucune dimension|
|storage_percent|Oui|Pourcentage de stockage|Pourcentage|Average|Pourcentage de stockage|Aucune dimension|
|storage_used|Oui|Stockage utilisé|Octets|Average|Stockage utilisé|Aucune dimension|


## <a name="microsoftdeviceselasticpools"></a>Microsoft.Devices/ElasticPools

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|elasticPool.requestedUsageRate|Oui|Taux d’utilisation demandée|Pourcentage|Average|Taux d’utilisation demandée|Aucune dimension|


## <a name="microsoftdeviceselasticpoolsiothubtenants"></a>Microsoft.Devices/ElasticPools/IotHubTenants

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|c2d.commands.egress.abandon.success|Oui|Messages cloud vers appareil abandonnés|Count|Total|Nombre de messages cloud vers appareil abandonnés par l’appareil|Aucune dimension|
|c2d.commands.egress.complete.success|Oui|Remises de messages cloud vers appareil terminées|Count|Total|Nombre de remises de messages cloud vers appareil terminées avec succès par l’appareil|Aucune dimension|
|c2d.commands.egress.reject.success|Oui|Messages cloud vers appareil rejetés|Count|Total|Nombre de messages cloud vers appareil rejetés par l’appareil|Aucune dimension|
|c2d.methods.failure|Oui|Appels de méthode directe en échec|Count|Total|Total des appels de méthode directe en échec.|Aucune dimension|
|c2d.methods.requestSize|Oui|Taille de demande des appels de méthode directe|Octets|Average|Moyenne, minimum et maximum de toutes les demandes de méthode directe réussies.|Aucune dimension|
|c2d.methods.responseSize|Oui|Taille de réponse des appels de méthode directe|Octets|Average|Moyenne, minimum et maximum de toutes les réponses de méthode directe réussies.|Aucune dimension|
|c2d.methods.success|Oui|Appels de méthode directe réussis|Count|Total|Total des appels de méthode directe réussis.|Aucune dimension|
|c2d.twin.read.failure|Oui|Lectures de représentations de serveur principal en échec|Count|Total|Total des lectures de représentations en échec initiées par un serveur principal.|Aucune dimension|
|c2d.twin.read.size|Oui|Taille de la réponse des lectures de représentations de serveur principal|Octets|Average|Moyenne, minimum et maximum de toutes les lectures de représentations réussies initiées par un serveur principal.|Aucune dimension|
|c2d.twin.read.success|Oui|Lectures de représentations réussies de serveur principal|Count|Total|Total des lectures de représentations réussies initiées par un serveur principal.|Aucune dimension|
|c2d.twin.update.failure|Oui|Mises à jour de représentations de serveur principal en échec|Count|Total|Total des mises à jour de représentations en échec initiées par un serveur principal.|Aucune dimension|
|c2d.twin.update.size|Oui|Taille des mises à jour de représentations de serveur principal|Octets|Average|Taille moyenne, minimale et maximale de toutes les mises à jour de représentations réussies initiées par un serveur principal.|Aucune dimension|
|c2d.twin.update.success|Oui|Mises à jour de représentations réussies de serveur principal|Count|Total|Total des mises à jour de représentations réussies initiées par un serveur principal.|Aucune dimension|
|C2DMessagesExpired|Oui|Messages cloud vers appareil ayant expiré|Count|Total|Nombre de messages cloud vers appareil ayant expiré|Aucune dimension|
|configurations|Oui|Métriques de configuration|Count|Total|Métriques pour les opérations de configuration|Aucune dimension|
|connectedDeviceCount|Oui|Appareils connectés|Count|Average|Nombre d’appareils connectés à votre hub IoT|Aucune dimension|
|d2c.endpoints.egress.builtIn.events|Oui|Routage : messages remis à des messages/événements|Count|Total|Nombre de fois où le routage IoT Hub a correctement remis des messages au point de terminaison intégré (messages/événements).|Aucune dimension|
|d2c.endpoints.egress.eventHubs|Oui|Routage : messages remis à Event Hub|Count|Total|Nombre de fois où le routage IoT Hub a correctement remis des messages à des points de terminaison Event Hub.|Aucune dimension|
|d2c.endpoints.egress.serviceBusQueues|Oui|Routage : messages remis à la file d’attente Service Bus|Count|Total|Nombre de fois où le routage IoT Hub a correctement remis des messages aux points de terminaison de file d’attente Service Bus.|Aucune dimension|
|d2c.endpoints.egress.serviceBusTopics|Oui|Routage : messages remis à la rubrique Service Bus|Count|Total|Nombre de fois où le routage IoT Hub a correctement remis des messages à des points de terminaison de rubrique Service Bus.|Aucune dimension|
|d2c.endpoints.egress.storage|Oui|Routage : messages remis au stockage|Count|Total|Nombre de fois où le routage IoT Hub a correctement remis des messages à des points de terminaison de stockage.|Aucune dimension|
|d2c.endpoints.egress.storage.blobs|Oui|Routage : objets blob remis au stockage|Count|Total|Nombre de fois où le routage IoT Hub a remis des objets blob à des points de terminaison de stockage.|Aucune dimension|
|d2c.endpoints.egress.storage.bytes|Oui|Routage : données remises au stockage|Octets|Total|Quantité de données (octets) que le routage IoT Hub a remis aux points de terminaison de stockage.|Aucune dimension|
|d2c.endpoints.latency.builtIn.events|Oui|Routage : latence des messages de messages/d’événements|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et de télémétrie dans le point de terminaison intégré (messages/événements).|Aucune dimension|
|d2c.endpoints.latency.eventHubs|Oui|Routage : latence des messages d’Event Hub|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et dans un point de terminaison Event Hub.|Aucune dimension|
|d2c.endpoints.latency.serviceBusQueues|Oui|Routage : latence des messages de la file d’attente Service Bus|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et de télémétrie dans un point de terminaison de file d’attente Service Bus.|Aucune dimension|
|d2c.endpoints.latency.serviceBusTopics|Oui|Routage : latence des messages de la rubrique Service Bus|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et de télémétrie dans un point de terminaison de rubrique Service Bus.|Aucune dimension|
|d2c.endpoints.latency.storage|Oui|Routage : latence des messages du stockage|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et de télémétrie dans un point de terminaison de stockage.|Aucune dimension|
|d2c.telemetry.egress.dropped|Oui|Routage : messages de télémétrie annulés|Nombre|Total|Nombre de fois où des messages ont été annulés par le routage IoT Hub en raison de points de terminaison morts. Cette valeur ne compte pas les messages remis à un itinéraire de secours, car les messages annulés n’y sont pas remis.|Aucune dimension|
|d2c.telemetry.egress.fallback|Oui|Routage : messages remis à l’itinéraire de secours|Count|Total|Nombre de fois où le routage IoT Hub a remis des messages au point de terminaison associé à l’itinéraire de secours.|Aucune dimension|
|d2c.telemetry.egress.invalid|Oui|Routage : messages de télémétrie incompatibles|Count|Total|Nombre de fois où le routage IoT Hub n’a pas réussi à remettre des messages en raison d’une incompatibilité avec le point de terminaison. Cette valeur n’inclut pas les nouvelles tentatives.|Aucune dimension|
|d2c.telemetry.egress.orphaned|Oui|Routage : messages de télémétrie orphelins|Nombre|Total|Nombre de fois où des messages ont été définis comme orphelins par le routage IoT Hub, car ils ne correspondaient à aucune règle d’acheminement (y compris la règle de secours).|Aucune dimension|
|d2c.telemetry.egress.success|Oui|Routage : messages de télémétrie remis|Count|Total|Nombre de fois où des messages ont été correctement remis à tous les points de terminaison à l’aide du routage IoT Hub. Si un message est routé vers plusieurs points de terminaison, cette valeur augmente d’une unité pour chaque remise réussie. Si un message est routé plusieurs fois vers le même point de terminaison, cette valeur augmente d’une unité pour chaque remise réussie.|Aucune dimension|
|d2c.telemetry.ingress.allProtocol|Oui|Tentatives d’envoi de message de télémétrie|Count|Total|Nombre de tentatives d’envoi de messages de télémétrie appareil vers cloud à votre hub IoT|Aucune dimension|
|d2c.telemetry.ingress.sendThrottle|Oui|Nombre d’erreurs de limitation|Count|Total|Nombre d’erreurs de limitation causées par des limitations de débit d’appareil|Aucune dimension|
|d2c.telemetry.ingress.success|Oui|Messages de télémétrie envoyés|Count|Total|Nombre de messages de télémétrie appareil vers cloud envoyés avec succès à votre hub IoT|Aucune dimension|
|d2c.twin.read.failure|Oui|Lectures de représentations d’appareils en échec|Count|Total|Total des lectures de représentations en échec initiées par un appareil.|Aucune dimension|
|d2c.twin.read.size|Oui|Taille de la réponse des lectures de représentations des appareils|Octets|Average|Moyenne, minimum et maximum de toutes les lectures de représentations réussies initiées par un appareil.|Aucune dimension|
|d2c.twin.read.success|Oui|Lectures de représentations réussies d’appareils|Count|Total|Total des lectures de représentations réussies initiées par un appareil.|Aucune dimension|
|d2c.twin.update.failure|Oui|Mises à jour de représentations d’appareils en échec|Count|Total|Total des mises à jour de représentations en échec initiées par un appareil.|Aucune dimension|
|d2c.twin.update.size|Oui|Taille des mises à jour de représentations d’appareils|Octets|Average|Taille moyenne, minimale et maximale de toutes les mises à jour de représentations réussies initiées par un appareil.|Aucune dimension|
|d2c.twin.update.success|Oui|Mises à jour de représentations réussies d’appareils|Count|Total|Total des mises à jour de représentations réussies initiées par un appareil.|Aucune dimension|
|dailyMessageQuotaUsed|Oui|Nombre total de messages utilisés|Count|Maximale|Nombre total de messages utilisés aujourd'hui|Aucune dimension|
|deviceDataUsage|Oui|Utilisation totale des données d’appareil|Octets|Total|Nombre d’octets transférés vers et depuis tous les appareils connectés à IotHub|Aucune dimension|
|deviceDataUsageV2|Oui|Utilisation totale des données d’appareil (préversion)|Octets|Total|Nombre d’octets transférés vers et depuis tous les appareils connectés à IotHub|Aucune dimension|
|devices.connectedDevices.allProtocol|Oui|Appareils connectés (déconseillé) |Count|Total|Nombre d’appareils connectés à votre hub IoT|Aucune dimension|
|devices.totalDevices|Oui|Nombre total d’appareils (déconseillé)|Count|Total|Nombre d’appareils enregistrés sur votre hub IoT|Aucune dimension|
|EventGridDeliveries|Oui|Remises Event Grid|Count|Total|Nombre d’événements IoT Hub publiés dans Event Grid. Utilisez la dimension Résultat pour le nombre de requêtes ayant réussi et ayant échoué. La dimension EventType représente le type de l’événement (https://aka.ms/ioteventgrid) ).|Résultat, Type d’événement|
|EventGridLatency|Oui|Latence d’Event Grid|Millisecondes|Average|Latence moyenne (en millisecondes) entre le moment où l’événement Iot Hub a été généré et le moment où l’événement a été publié dans Event Grid. Ce nombre est une moyenne de tous les types d’événement. Utilisez la dimension Type d’événement pour afficher la latence d’un type d’événement spécifique.|Type d’événement|
|jobs.cancelJob.failure|Oui|Annulations de travaux en échec|Count|Total|Total des appels en échec pour annuler un travail.|Aucune dimension|
|jobs.cancelJob.success|Oui|Annulations de travaux réussies|Count|Total|Total des appels réussis pour annuler un travail.|Aucune dimension|
|jobs.completed|Oui|Travaux terminés|Count|Total|Total des travaux terminés.|Aucune dimension|
|jobs.createDirectMethodJob.failure|Oui|Créations des travaux d’appel de méthode en échec|Count|Total|Total des créations en échec des travaux d’appel de méthode directe.|Aucune dimension|
|jobs.createDirectMethodJob.success|Oui|Créations réussies des travaux d’appel de méthode|Count|Total|Total des créations réussies des travaux d’appel de méthode directe.|Aucune dimension|
|jobs.createTwinUpdateJob.failure|Oui|Créations des travaux de mises à jour de représentations en échec|Count|Total|Total des créations en échec des travaux de mises à jour de représentations.|Aucune dimension|
|jobs.createTwinUpdateJob.success|Oui|Créations réussies des travaux de mises à jour de représentations|Count|Total|Total des créations réussies de travaux de mises à jour de représentations.|Aucune dimension|
|jobs.failed|Oui|Travaux en échec|Count|Total|Total des travaux en échec.|Aucune dimension|
|jobs.listJobs.failure|Oui|Appels en échec pour répertorier les travaux|Count|Total|Total des appels en échec pour répertorier les travaux.|Aucune dimension|
|jobs.listJobs.success|Oui|Appels réussis pour répertorier les travaux|Count|Total|Total des appels réussis pour répertorier les travaux.|Aucune dimension|
|jobs.queryJobs.failure|Oui|Requêtes de travaux en échec|Count|Total|Total des appels en échec pour interroger les travaux.|Aucune dimension|
|jobs.queryJobs.success|Oui|Requêtes de travaux réussies|Count|Total|Total des appels réussis pour interroger les travaux.|Aucune dimension|
|RoutingDataSizeInBytesDelivered|Oui|Taille des messages de distribution de l’acheminement en octets (préversion)|Octets|Total|Taille totale, en octets, des messages remis par IoT Hub à un point de terminaison. Vous pouvez utiliser les dimensions EndpointName et EndpointType pour afficher la taille, en octets, des messages remis à vos différents points de terminaison. La valeur de la métrique augmente à chaque message remis, y compris à plusieurs points de terminaison ou plusieurs fois au même point de terminaison.|EndpointType, EndpointName, RoutingSource|
|RoutingDeliveries|Oui|Livraisons de routage (préversion)|Count|Total|Nombre de fois où IoT Hub a tenté de remettre des messages à tous les points de terminaison à l’aide du routage. Pour afficher le nombre de tentatives ayant abouti ou échoué, utilisez la dimension Résultat. Pour voir la raison de l’échec, comme non valide, supprimé ou orphelin, utilisez la dimension FailureReasonCategory. Vous pouvez également utiliser les dimensions EndpointName et EndpointType pour connaître le nombre de messages remis à vos différents points de terminaison. La valeur de métrique augmente d’une unité à chaque tentative de remise, notamment si le message est remis à plusieurs points de terminaison ou plusieurs fois au même point de terminaison.|EndpointType, EndpointName, FailureReasonCategory, Result, RoutingSource|
|RoutingDeliveryLatency|Oui|Latence de livraison du routage (préversion)|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et de télémétrie dans un point de terminaison. Vous pouvez utiliser les dimensions EndpointName et EndpointType pour connaître la latence à vos différents points de terminaison.|EndpointType, EndpointName, RoutingSource|
|tenantHub.requestedUsageRate|Oui|Taux d’utilisation demandée|Pourcentage|Average|Taux d’utilisation demandée|Aucune dimension|
|totalDeviceCount|Oui|Nombre total d’appareils|Count|Average|Nombre d’appareils enregistrés sur votre hub IoT|Aucune dimension|
|twinQueries.failure|Oui|Requêtes de représentations en échec|Count|Total|Total des requêtes de représentations en échec.|Aucune dimension|
|twinQueries.resultSize|Oui|Taille du résultat des requêtes de représentations|Octets|Average|Moyenne, minimum et maximum de la taille du résultat de toutes les requêtes de représentations réussies.|Aucune dimension|
|twinQueries.success|Oui|Requêtes de représentations réussies|Count|Total|Total des requêtes de représentations réussies.|Aucune dimension|


## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|c2d.commands.egress.abandon.success|Oui|Messages cloud vers appareil abandonnés|Count|Total|Nombre de messages cloud vers appareil abandonnés par l’appareil|Aucune dimension|
|c2d.commands.egress.complete.success|Oui|Remises de messages cloud vers appareil terminées|Count|Total|Nombre de remises de messages cloud vers appareil terminées avec succès par l’appareil|Aucune dimension|
|c2d.commands.egress.reject.success|Oui|Messages cloud vers appareil rejetés|Count|Total|Nombre de messages cloud vers appareil rejetés par l’appareil|Aucune dimension|
|c2d.methods.failure|Oui|Appels de méthode directe en échec|Count|Total|Total des appels de méthode directe en échec.|Aucune dimension|
|c2d.methods.requestSize|Oui|Taille de demande des appels de méthode directe|Octets|Average|Moyenne, minimum et maximum de toutes les demandes de méthode directe réussies.|Aucune dimension|
|c2d.methods.responseSize|Oui|Taille de réponse des appels de méthode directe|Octets|Average|Moyenne, minimum et maximum de toutes les réponses de méthode directe réussies.|Aucune dimension|
|c2d.methods.success|Oui|Appels de méthode directe réussis|Count|Total|Total des appels de méthode directe réussis.|Aucune dimension|
|c2d.twin.read.failure|Oui|Lectures de représentations de serveur principal en échec|Count|Total|Total des lectures de représentations en échec initiées par un serveur principal.|Aucune dimension|
|c2d.twin.read.size|Oui|Taille de la réponse des lectures de représentations de serveur principal|Octets|Average|Moyenne, minimum et maximum de toutes les lectures de représentations réussies initiées par un serveur principal.|Aucune dimension|
|c2d.twin.read.success|Oui|Lectures de représentations réussies de serveur principal|Count|Total|Total des lectures de représentations réussies initiées par un serveur principal.|Aucune dimension|
|c2d.twin.update.failure|Oui|Mises à jour de représentations de serveur principal en échec|Count|Total|Total des mises à jour de représentations en échec initiées par un serveur principal.|Aucune dimension|
|c2d.twin.update.size|Oui|Taille des mises à jour de représentations de serveur principal|Octets|Average|Taille moyenne, minimale et maximale de toutes les mises à jour de représentations réussies initiées par un serveur principal.|Aucune dimension|
|c2d.twin.update.success|Oui|Mises à jour de représentations réussies de serveur principal|Count|Total|Total des mises à jour de représentations réussies initiées par un serveur principal.|Aucune dimension|
|C2DMessagesExpired|Oui|Messages cloud vers appareil ayant expiré|Count|Total|Nombre de messages cloud vers appareil ayant expiré|Aucune dimension|
|configurations|Oui|Métriques de configuration|Count|Total|Métriques pour les opérations de configuration|Aucune dimension|
|connectedDeviceCount|Non|Appareils connectés|Count|Average|Nombre d’appareils connectés à votre hub IoT|Aucune dimension|
|d2c.endpoints.egress.builtIn.events|Oui|Routage : messages remis à des messages/événements|Count|Total|Nombre de fois où le routage IoT Hub a correctement remis des messages au point de terminaison intégré (messages/événements).|Aucune dimension|
|d2c.endpoints.egress.eventHubs|Oui|Routage : messages remis à Event Hub|Count|Total|Nombre de fois où le routage IoT Hub a correctement remis des messages à des points de terminaison Event Hub.|Aucune dimension|
|d2c.endpoints.egress.serviceBusQueues|Oui|Routage : messages remis à la file d’attente Service Bus|Count|Total|Nombre de fois où le routage IoT Hub a correctement remis des messages aux points de terminaison de file d’attente Service Bus.|Aucune dimension|
|d2c.endpoints.egress.serviceBusTopics|Oui|Routage : messages remis à la rubrique Service Bus|Count|Total|Nombre de fois où le routage IoT Hub a correctement remis des messages à des points de terminaison de rubrique Service Bus.|Aucune dimension|
|d2c.endpoints.egress.storage|Oui|Routage : messages remis au stockage|Count|Total|Nombre de fois où le routage IoT Hub a correctement remis des messages à des points de terminaison de stockage.|Aucune dimension|
|d2c.endpoints.egress.storage.blobs|Oui|Routage : objets blob remis au stockage|Count|Total|Nombre de fois où le routage IoT Hub a remis des objets blob à des points de terminaison de stockage.|Aucune dimension|
|d2c.endpoints.egress.storage.bytes|Oui|Routage : données remises au stockage|Octets|Total|Quantité de données (octets) que le routage IoT Hub a remis aux points de terminaison de stockage.|Aucune dimension|
|d2c.endpoints.latency.builtIn.events|Oui|Routage : latence des messages de messages/d’événements|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et de télémétrie dans le point de terminaison intégré (messages/événements).|Aucune dimension|
|d2c.endpoints.latency.eventHubs|Oui|Routage : latence des messages d’Event Hub|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et dans un point de terminaison Event Hub.|Aucune dimension|
|d2c.endpoints.latency.serviceBusQueues|Oui|Routage : latence des messages de la file d’attente Service Bus|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et de télémétrie dans un point de terminaison de file d’attente Service Bus.|Aucune dimension|
|d2c.endpoints.latency.serviceBusTopics|Oui|Routage : latence des messages de la rubrique Service Bus|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et de télémétrie dans un point de terminaison de rubrique Service Bus.|Aucune dimension|
|d2c.endpoints.latency.storage|Oui|Routage : latence des messages du stockage|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et de télémétrie dans un point de terminaison de stockage.|Aucune dimension|
|d2c.telemetry.egress.dropped|Oui|Routage : messages de télémétrie annulés |Count|Total|Nombre de fois où des messages ont été annulés par le routage IoT Hub en raison de points de terminaison morts. Cette valeur ne compte pas les messages remis à un itinéraire de secours, car les messages annulés n’y sont pas remis.|Aucune dimension|
|d2c.telemetry.egress.fallback|Oui|Routage : messages remis à l’itinéraire de secours|Count|Total|Nombre de fois où le routage IoT Hub a remis des messages au point de terminaison associé à l’itinéraire de secours.|Aucune dimension|
|d2c.telemetry.egress.invalid|Oui|Routage : messages de télémétrie incompatibles|Count|Total|Nombre de fois où le routage IoT Hub n’a pas réussi à remettre des messages en raison d’une incompatibilité avec le point de terminaison. Cette valeur n’inclut pas les nouvelles tentatives.|Aucune dimension|
|d2c.telemetry.egress.orphaned|Oui|Routage : messages de télémétrie orphelins |Count|Total|Nombre de fois où des messages ont été définis comme orphelins par le routage IoT Hub car ils ne correspondaient à aucune règle de routage (y compris la règle de secours). |Aucune dimension|
|d2c.telemetry.egress.success|Oui|Routage : messages de télémétrie remis|Count|Total|Nombre de fois où des messages ont été correctement remis à tous les points de terminaison à l’aide du routage IoT Hub. Si un message est routé vers plusieurs points de terminaison, cette valeur augmente d’une unité pour chaque remise réussie. Si un message est routé plusieurs fois vers le même point de terminaison, cette valeur augmente d’une unité pour chaque remise réussie.|Aucune dimension|
|d2c.telemetry.ingress.allProtocol|Oui|Tentatives d’envoi de message de télémétrie|Count|Total|Nombre de tentatives d’envoi de messages de télémétrie appareil vers cloud à votre hub IoT|Aucune dimension|
|d2c.telemetry.ingress.sendThrottle|Oui|Nombre d’erreurs de limitation|Count|Total|Nombre d’erreurs de limitation causées par des limitations de débit d’appareil|Aucune dimension|
|d2c.telemetry.ingress.success|Oui|Messages de télémétrie envoyés|Count|Total|Nombre de messages de télémétrie appareil vers cloud envoyés avec succès à votre hub IoT|Aucune dimension|
|d2c.twin.read.failure|Oui|Lectures de représentations d’appareils en échec|Count|Total|Total des lectures de représentations en échec initiées par un appareil.|Aucune dimension|
|d2c.twin.read.size|Oui|Taille de la réponse des lectures de représentations des appareils|Octets|Average|Moyenne, minimum et maximum de toutes les lectures de représentations réussies initiées par un appareil.|Aucune dimension|
|d2c.twin.read.success|Oui|Lectures de représentations réussies d’appareils|Count|Total|Total des lectures de représentations réussies initiées par un appareil.|Aucune dimension|
|d2c.twin.update.failure|Oui|Mises à jour de représentations d’appareils en échec|Count|Total|Total des mises à jour de représentations en échec initiées par un appareil.|Aucune dimension|
|d2c.twin.update.size|Oui|Taille des mises à jour de représentations d’appareils|Octets|Average|Taille moyenne, minimale et maximale de toutes les mises à jour de représentations réussies initiées par un appareil.|Aucune dimension|
|d2c.twin.update.success|Oui|Mises à jour de représentations réussies d’appareils|Count|Total|Total des mises à jour de représentations réussies initiées par un appareil.|Aucune dimension|
|dailyMessageQuotaUsed|Oui|Nombre total de messages utilisés|Count|Maximale|Nombre total de messages utilisés aujourd'hui|Aucune dimension|
|deviceDataUsage|Oui|Utilisation totale des données d’appareil|Octets|Total|Nombre d’octets transférés vers et depuis tous les appareils connectés à IotHub|Aucune dimension|
|deviceDataUsageV2|Oui|Utilisation totale des données d’appareil (préversion)|Octets|Total|Nombre d’octets transférés vers et depuis tous les appareils connectés à IotHub|Aucune dimension|
|devices.connectedDevices.allProtocol|Oui|Appareils connectés (déconseillé) |Count|Total|Nombre d’appareils connectés à votre hub IoT|Aucune dimension|
|devices.totalDevices|Oui|Nombre total d’appareils (déconseillé)|Count|Total|Nombre d’appareils enregistrés sur votre hub IoT|Aucune dimension|
|EventGridDeliveries|Oui|Remises Event Grid|Count|Total|Nombre d’événements IoT Hub publiés dans Event Grid. Utilisez la dimension Résultat pour le nombre de requêtes ayant réussi et ayant échoué. La dimension EventType représente le type de l’événement (https://aka.ms/ioteventgrid) ).|Résultat, Type d’événement|
|EventGridLatency|Oui|Latence d’Event Grid|Millisecondes|Average|Latence moyenne (en millisecondes) entre le moment où l’événement Iot Hub a été généré et le moment où l’événement a été publié dans Event Grid. Ce nombre est une moyenne de tous les types d’événement. Utilisez la dimension Type d’événement pour afficher la latence d’un type d’événement spécifique.|Type d’événement|
|jobs.cancelJob.failure|Oui|Annulations de travaux en échec|Count|Total|Total des appels en échec pour annuler un travail.|Aucune dimension|
|jobs.cancelJob.success|Oui|Annulations de travaux réussies|Count|Total|Total des appels réussis pour annuler un travail.|Aucune dimension|
|jobs.completed|Oui|Travaux terminés|Count|Total|Total des travaux terminés.|Aucune dimension|
|jobs.createDirectMethodJob.failure|Oui|Créations des travaux d’appel de méthode en échec|Count|Total|Total des créations en échec des travaux d’appel de méthode directe.|Aucune dimension|
|jobs.createDirectMethodJob.success|Oui|Créations réussies des travaux d’appel de méthode|Count|Total|Total des créations réussies des travaux d’appel de méthode directe.|Aucune dimension|
|jobs.createTwinUpdateJob.failure|Oui|Créations des travaux de mises à jour de représentations en échec|Count|Total|Total des créations en échec des travaux de mises à jour de représentations.|Aucune dimension|
|jobs.createTwinUpdateJob.success|Oui|Créations réussies des travaux de mises à jour de représentations|Count|Total|Total des créations réussies de travaux de mises à jour de représentations.|Aucune dimension|
|jobs.failed|Oui|Travaux en échec|Count|Total|Total des travaux en échec.|Aucune dimension|
|jobs.listJobs.failure|Oui|Appels en échec pour répertorier les travaux|Count|Total|Total des appels en échec pour répertorier les travaux.|Aucune dimension|
|jobs.listJobs.success|Oui|Appels réussis pour répertorier les travaux|Count|Total|Total des appels réussis pour répertorier les travaux.|Aucune dimension|
|jobs.queryJobs.failure|Oui|Requêtes de travaux en échec|Count|Total|Total des appels en échec pour interroger les travaux.|Aucune dimension|
|jobs.queryJobs.success|Oui|Requêtes de travaux réussies|Count|Total|Total des appels réussis pour interroger les travaux.|Aucune dimension|
|RoutingDataSizeInBytesDelivered|Oui|Taille des messages de distribution de l’acheminement en octets (préversion)|Octets|Total|Taille totale, en octets, des messages remis par IoT Hub à un point de terminaison. Vous pouvez utiliser les dimensions EndpointName et EndpointType pour afficher la taille, en octets, des messages remis à vos différents points de terminaison. La valeur de la métrique augmente à chaque message remis, y compris à plusieurs points de terminaison ou plusieurs fois au même point de terminaison.|EndpointType, EndpointName, RoutingSource|
|RoutingDeliveries|Oui|Livraisons de routage (préversion)|Count|Total|Nombre de fois où IoT Hub a tenté de remettre des messages à tous les points de terminaison à l’aide du routage. Pour afficher le nombre de tentatives ayant abouti ou échoué, utilisez la dimension Résultat. Pour voir la raison de l’échec, comme non valide, supprimé ou orphelin, utilisez la dimension FailureReasonCategory. Vous pouvez également utiliser les dimensions EndpointName et EndpointType pour connaître le nombre de messages remis à vos différents points de terminaison. La valeur de métrique augmente d’une unité à chaque tentative de remise, notamment si le message est remis à plusieurs points de terminaison ou plusieurs fois au même point de terminaison.|EndpointType, EndpointName, FailureReasonCategory, Result, RoutingSource|
|RoutingDeliveryLatency|Oui|Latence de livraison du routage (préversion)|Millisecondes|Average|Latence moyenne (en millisecondes) entre les entrées de messages vers IoT Hub et de télémétrie dans un point de terminaison. Vous pouvez utiliser les dimensions EndpointName et EndpointType pour connaître la latence à vos différents points de terminaison.|EndpointType, EndpointName, RoutingSource|
|totalDeviceCount|Non|Nombre total d’appareils|Count|Average|Nombre d’appareils enregistrés sur votre hub IoT|Aucune dimension|
|twinQueries.failure|Oui|Requêtes de représentations en échec|Count|Total|Total des requêtes de représentations en échec.|Aucune dimension|
|twinQueries.resultSize|Oui|Taille du résultat des requêtes de représentations|Octets|Average|Moyenne, minimum et maximum de la taille du résultat de toutes les requêtes de représentations réussies.|Aucune dimension|
|twinQueries.success|Oui|Requêtes de représentations réussies|Count|Total|Total des requêtes de représentations réussies.|Aucune dimension|


## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AttestationAttempts|Oui|Tentatives d’attestation|Count|Total|Nombre d’attestations d’appareils tentées|ProvisioningServiceName, Status, Protocol|
|DeviceAssignments|Oui|Appareils attribués|Count|Total|Nombre d’appareils affectés à un hub IoT|ProvisioningServiceName, IotHubName|
|RegistrationAttempts|Oui|Tentatives d’enregistrement|Count|Total|Nombre d’inscriptions d’appareils tentées|ProvisioningServiceName, IotHubName, Status|


## <a name="microsoftdigitaltwinsdigitaltwinsinstances"></a>Microsoft.DigitalTwins/digitalTwinsInstances

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ApiRequests|Oui|Requêtes d’API|Count|Total|Nombre de demandes d’API effectuées pour des opérations de lecture, d’écriture, de suppression et de requête de Digital Twins.|Opération, Authentification, Protocole, StatusCode, StatusCodeClass, StatusText|
|ApiRequestsFailureRate|Oui|Taux d’échec des demandes d’API|Pourcentage|Average|Pourcentage de demandes d’API que le service reçoit pour votre instance, qui retournent un code de réponse d’erreur interne (500) pour une opération de lecture, d’écriture, de suppression ou de requête de Digital Twins.|Opération, authentification, protocole|
|ApiRequestsLatency|Oui|Latence des demandes d’API|Millisecondes|Average|Temps de réponse pour les demandes d’API, c’est-à-dire entre le moment où Azure Digital Twins reçoit la demande et le moment où le service envoie un résultat de réussite ou d’échec d’une opération de lecture, d’écriture, de suppression ou de requête de Digital Twins.|Opération, Authentification, Protocole, StatusCode, StatusCodeClass, StatusText|
|BillingApiOperations|Oui|Opérations de l’API de facturation|Count|Total|Métrique de facturation pour le nombre total des demandes d’API adressées au service Azure Digital Twins.|ID du compteur|
|BillingMessagesProcessed|Oui|Messages de facturation traités|Count|Total|Métrique de facturation pour le nombre de messages envoyés à partir d’Azure Digital Twins vers des points de terminaison externes.|ID du compteur|
|BillingQueryUnits|Oui|Unités de requête de facturation|Count|Total|Nombre d’unités de requête, mesure calculée en interne de l’utilisation des ressources des services, consommées pour exécuter des requêtes.|ID du compteur|
|IngressEvents|Oui|Événements d’entrée|Count|Total|Nombre d’événements de télémétrie entrants dans Azure Digital Twins.|Résultats|
|IngressEventsFailureRate|Oui|Taux d’échec des demandes des événement d’entrée|Pourcentage|Average|Pourcentage d’événements de télémétrie entrants pour lesquels le service retourne un code de réponse d’erreur interne (500).|Aucune dimension|
|IngressEventsLatency|Oui|Latence des événement d’entrée|Millisecondes|Average|Heure à laquelle un événement arrive lorsqu’il est prêt à être sortie par Azure Digital Twins, auquel cas le service envoie un résultat de réussite/échec.|Résultats|
|ModelCount|Oui|Nombre de modèles|Count|Total|Nombre total de modèles dans l’instance Azure Digital Twins. Utilisez cette mesure pour déterminer si vous approchez de la limite du service concernant le nombre maximal de modèles autorisés par instance.|Aucune dimension|
|Routage|Oui|Messages routés|Count|Total|Nombre de messages routés vers un service Azure de point de terminaison, comme Event Hub, Service Bus ou Event Grid.|EndpointType, Résultat|
|RoutingFailureRate|Oui|Taux d’échec du routage|Pourcentage|Average|Pourcentage d’événements qui génèrent une erreur lors de leur routage à partir d’Azure Digital Twins vers un service Azure de point de terminaison, comme Event Hub, Service Bus ou Event Grid.|EndpointType|
|RoutingLatency|Oui|Latence du routage|Millisecondes|Average|Temps écoulé entre le routage d’un événement à partir d’Azure Digital Twins et sa publication sur le service Azure de point de terminaison, comme Event Hub, Service Bus ou Event Grid.|EndpointType, Résultat|
|TwinCount|Oui|Nombre de jumeaux|Count|Total|Nombre total de jumeaux dans une instance Azure Digital Twins. Cette métrique vous permet de déterminer si vous approchez de la limite du service relative au nombre maximal de jumeaux autorisés par instance.|Aucune dimension|


## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/DatabaseAccounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AddRegion|Oui|Région ajoutée|Count|Count|Région ajoutée|Région|
|AutoscaleMaxThroughput|Non|Débit maximal de mise à l’échelle automatique|Count|Maximale|Débit maximal de mise à l’échelle automatique|DatabaseName, CollectionName|
|AvailableStorage|Non|(déconseillé) Stockage disponible|Octets|Total|« Stockage disponible » sera supprimé d’Azure Monitor à la fin du mois de septembre 2023. La taille de stockage de collection Cosmos DB est maintenant illimitée. La seule restriction est que la taille de stockage est de 20 Go par clé de partition logique. Vous pouvez activer PartitionKeyStatistics dans le journal de diagnostic pour connaître la consommation de stockage des principales clés de partition. Pour plus d’informations sur le quota de stockage de Cosmos DB, consultez ce document : [https://docs.microsoft.com/azure/cosmos-db/concepts-limits](/azure/cosmos-db/concepts-limits). À la date où la métrique sera déconseillée, les règles d’alerte qui seront encore définies dessus seront automatiquement désactivées.|CollectionName, DatabaseName, Region|
|CassandraConnectionClosures|Non|Fermetures de connexion Cassandra|Count|Total|Nombre de connexions de Cassandra fermées, signalées à une granularité d'une minute|APIType, Region, ClosureReason|
|CassandraConnectorAvgReplicationLatency|Non|Latence de réplication moyenne du connecteur Cassandra|Millisecondes|Average|Latence de réplication moyenne du connecteur Cassandra|Aucune dimension|
|CassandraConnectorReplicationHealthStatus|Non|État d’intégrité de la réplication du connecteur Cassandra|Count|Count|État d’intégrité de la réplication du connecteur Cassandra|NotStarted, ReplicationInProgress, Error|
|CassandraKeyspaceCreate|Non|Espace de clés Cassandra créé|Count|Count|Espace de clés Cassandra créé|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|CassandraKeyspaceDelete|Non|Espace de clés Cassandra supprimé|Count|Count|Espace de clés Cassandra supprimé|ResourceName, ApiKind, ApiKindResourceType, OperationType|
|CassandraKeyspaceThroughputUpdate|Non|Débit d’espace de clés Cassandra mis à jour|Count|Count|Débit d’espace de clés Cassandra mis à jour|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|CassandraKeyspaceUpdate|Non|Espace de clés Cassandra mis à jour|Count|Count|Espace de clés Cassandra mis à jour|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|CassandraRequestCharges|Non|Frais de requête Cassandra|Count|Total|Unités de requête consommées pour les requêtes Cassandra effectuées|APIType, DatabaseName, CollectionName, Region, OperationType, ResourceType|
|CassandraRequests|Non|Requêtes Cassandra|Count|Count|Nombre de requêtes Cassandra effectuées|APIType, DatabaseName, CollectionName, Region, OperationType, ResourceType, ErrorCode|
|CassandraTableCreate|Non|Table Cassandra créée|Count|Count|Table Cassandra créée|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|CassandraTableDelete|Non|Table Cassandra supprimée|Count|Count|Table Cassandra supprimée|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, OperationType|
|CassandraTableThroughputUpdate|Non|Débit de table Cassandra mis à jour|Count|Count|Débit de table Cassandra mis à jour|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|CassandraTableUpdate|Non|Table Cassandra mise à jour|Count|Count|Table Cassandra mise à jour|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|CreateAccount|Oui|Compte créé|Count|Count|Compte créé|Aucune dimension|
|DataUsage|Non|Utilisation des données|Octets|Total|Utilisation totale des données signalée à une granularité de 5 minutes|CollectionName, DatabaseName, Region|
|DedicatedGatewayAverageCPUUsage|No|DedicatedGatewayAverageCPUUsage|Pourcentage|Average|Utilisation moyenne de l’UC sur les instances de passerelle dédiée|Region, MetricType|
|DedicatedGatewayAverageMemoryUsage|No|DedicatedGatewayAverageMemoryUsage|Octets|Average|Utilisation moyenne de la mémoire sur les instances de passerelle dédiée, qui est utilisée à la fois pour le routage des requêtes et la mise en cache des données|Région|
|DedicatedGatewayMaximumCPUUsage|No|DedicatedGatewayMaximumCPUUsage|Pourcentage|Average|Utilisation maximale moyenne de l’UC sur les instances de passerelle dédiée|Region, MetricType|
|DedicatedGatewayRequests|Oui|DedicatedGatewayRequests|Count|Count|Demandes au niveau de la passerelle dédiée|DatabaseName, CollectionName, CacheExercised, OperationName, Region|
|DeleteAccount|Oui|Compte supprimé|Count|Count|Compte supprimé|Aucune dimension|
|DocumentCount|Non|Nombre de documents|Count|Total|Nombre total de documents rapporté à une granularité de 5 minutes, 1 heure et 1 jour|CollectionName, DatabaseName, Region|
|DocumentQuota|Non|Quota de document|Octets|Total|Quota de stockage total signalé à une granularité de 5 minutes|CollectionName, DatabaseName, Region|
|GremlinDatabaseCreate|Non|Base de données Gremlin créée|Count|Count|Base de données Gremlin créée|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|GremlinDatabaseDelete|Non|Base de données Gremlin supprimée|Count|Count|Base de données Gremlin supprimée|ResourceName, ApiKind, ApiKindResourceType, OperationType|
|GremlinDatabaseThroughputUpdate|Non|Débit de base de données Gremlin mis à jour|Count|Count|Débit de base de données Gremlin mis à jour|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|GremlinDatabaseUpdate|Non|Base de données Gremlin mise à jour|Count|Count|Base de données Gremlin mise à jour|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|GremlinGraphCreate|Non|Graphique Gremlin créé|Count|Count|Graphique Gremlin créé|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|GremlinGraphDelete|Non|Graphique Gremlin supprimé|Count|Count|Graphique Gremlin supprimé|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, OperationType|
|GremlinGraphThroughputUpdate|Non|Débit de graphique Gremlin mis à jour|Count|Count|Débit de graphique Gremlin mis à jour|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|GremlinGraphUpdate|Non|Graphique Gremlin mis à jour|Count|Count|Graphique Gremlin mis à jour|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|IndexUsage|Non|Utilisation d'index|Octets|Total|Utilisation d’index totale signalée à une granularité de 5 minutes|CollectionName, DatabaseName, Region|
|IntegratedCacheEvictedEntriesSize|Non|IntegratedCacheEvictedEntriesSize|Octets|Average|Taille des entrées supprimées du cache intégré|Région|
|IntegratedCacheItemExpirationCount|No|IntegratedCacheItemExpirationCount|Count|Average|Nombre d’éléments exclus du cache intégré en raison de l’expiration de la durée de vie|Region, CacheEntryType|
|IntegratedCacheItemHitRate|No|IntegratedCacheItemHitRate|Pourcentage|Average|Nombre de lectures de points qui ont utilisé le cache intégré divisé par le nombre de lectures de points acheminées par la passerelle dédiée avec cohérence éventuelle|Region, CacheEntryType|
|IntegratedCacheQueryExpirationCount|No|IntegratedCacheQueryExpirationCount|Count|Average|Nombre de requêtes exclues du cache intégré en raison de l’expiration de la durée de vie|Region, CacheEntryType|
|IntegratedCacheQueryHitRate|No|IntegratedCacheQueryHitRate|Pourcentage|Average|Nombre de requêtes qui ont utilisé le cache intégré divisé par le nombre de requêtes acheminées par la passerelle dédiée avec cohérence éventuelle|Region, CacheEntryType|
|MetadataRequests|Non|Demandes de métadonnées|Count|Count|Nombre de demandes de métadonnées. Cosmos DB gère la collection des métadonnées système pour chaque compte, ce qui vous permet d’énumérer les collections, les bases de données, etc., ainsi que leur configuration, et ce gratuitement.|DatabaseName, CollectionName, Region, StatusCode, Role|
|MongoCollectionCreate|Non|Collection Mongo créée|Count|Count|Collection Mongo créée|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|MongoCollectionDelete|Non|Collection Mongo supprimée|Count|Count|Collection Mongo supprimée|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, OperationType|
|MongoCollectionThroughputUpdate|Non|Débit de collection Mongo mis à jour|Count|Count|Débit de collection Mongo mis à jour|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|MongoCollectionUpdate|Non|Collection Mongo mise à jour|Count|Count|Collection Mongo mise à jour|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|MongoDBDatabaseUpdate|Non|Base de données Mongo supprimée|Count|Count|Base de données Mongo supprimée|ResourceName, ApiKind, ApiKindResourceType, OperationType|
|MongoDatabaseThroughputUpdate|Non|Débit de base de données Mongo mis à jour|Count|Count|Débit de base de données Mongo mis à jour|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|MongoDBDatabaseCreate|Non|Base de données Mongo créée|Count|Count|Base de données Mongo créée|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|MongoDBDatabaseUpdate|Non|Base de données Mongo mise à jour|Count|Count|Base de données Mongo mise à jour|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|MongoRequestCharge|Oui|Frais des requêtes Mongo|Count|Total|Unités de requête Mongo consommées|DatabaseName, CollectionName, Region, CommandName, ErrorCode, Status|
|MongoRequests|Oui|Requêtes Mongo|Count|Count|Nombre de requêtes Mongo effectuées|DatabaseName, CollectionName, Region, CommandName, ErrorCode, Status|
|NormalizedRUConsumption|Non|Consommation d’unités de requête normalisée|Pourcentage|Maximale|Pourcentage maximal de consommation d’unités de requête par minute|CollectionName, DatabaseName, Region, PartitionKeyRangeId|
|ProvisionedThroughput|Non|Débit approvisionné|Count|Maximale|Débit approvisionné|DatabaseName, CollectionName|
|RegionFailover|Oui|Région basculée|Count|Count|Région basculée|Aucune dimension|
|RemoveRegion|Oui|Région supprimée|Count|Count|Région supprimée|Région|
|ReplicationLatency|Oui|Latence de réplication P99|Millisecondes|Average|Latence de réplication P99 des régions source et cible pour le compte géolocalisé|SourceRegion, TargetRegion|
|ServerSideLatency|Non|Latence côté serveur|Millisecondes|Average|Latence côté serveur|DatabaseName, CollectionName, Region, ConnectionMode, OperationType, PublicAPIType|
|ServiceAvailability|Non|Disponibilité des services|Pourcentage|Average|Disponibilité des requêtes de compte à une granularité d’une heure, d'un jour ou d'un mois|Aucune dimension|
|SqlContainerCreate|Non|Conteneur SQL créé|Count|Count|Conteneur SQL créé|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|SqlContainerDelete|Non|Conteneur SQL supprimé|Count|Count|Conteneur SQL supprimé|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, OperationType|
|SqlContainerThroughputUpdate|Non|Débit de conteneur SQL mis à jour|Count|Count|Débit de conteneur SQL mis à jour|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|SqlContainerUpdate|Non|Conteneur SQL mis à jour|Count|Count|Conteneur SQL mis à jour|ResourceName, ChildResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|SqlDatabaseCreate|Non|Base de données SQL créée|Count|Count|Base de données SQL créée|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|SqlDatabaseDelete|Non|Base de données SQL supprimée|Count|Count|Base de données SQL supprimée|ResourceName, ApiKind, ApiKindResourceType, OperationType|
|SqlDatabaseThroughputUpdate|Non|Débit de base de données SQL mis à jour|Count|Count|Débit de base de données SQL mis à jour|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|SqlDatabaseUpdate|Non|Base de données SQL mise à jour|Count|Count|Base de données SQL mise à jour|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|TableTableCreate|Non|Table AzureTable créée|Count|Count|Table AzureTable créée|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|TableTableDelete|Non|Table AzureTable supprimée|Count|Count|Table AzureTable supprimée|ResourceName, ApiKind, ApiKindResourceType, OperationType|
|TableTableThroughputUpdate|Non|Débit de table AzureTable mis à jour|Count|Count|Débit de table AzureTable mis à jour|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest|
|TableTableUpdate|Non|Table AzureTable mise à jour|Count|Count|Table AzureTable mise à jour|ResourceName, ApiKind, ApiKindResourceType, IsThroughputRequest, OperationType|
|TotalRequests|Oui|Total de requêtes|Count|Count|Nombre de requêtes effectuées|DatabaseName, CollectionName, Region, StatusCode, OperationType, Status|
|TotalRequestUnits|Oui|Unités de requête totales|Count|Total|Unités de requête consommées|DatabaseName, CollectionName, Region, StatusCode, OperationType, Status|
|UpdateAccountKeys|Oui|Clés de compte mises à jour|Count|Count|Clés de compte mises à jour|KeyType|
|UpdateAccountNetworkSettings|Oui|Paramètres réseau de compte mis à jour|Count|Count|Paramètres réseau de compte mis à jour|Aucune dimension|
|UpdateAccountReplicationSettings|Oui|Paramètres de réplication de compte mis à jour|Count|Count|Paramètres de réplication de compte mis à jour|Aucune dimension|
|UpdateDiagnosticsSettings|Non|Paramètres de diagnostic du compte mis à jour|Count|Count|Paramètres de diagnostic du compte mis à jour|DiagnosticSettingsName, ResourceGroupName|


## <a name="microsofteventgriddomains"></a>Microsoft.EventGrid/domains

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Oui|Évaluations de filtres avancés|Count|Total|Nombre total de filtres avancés évalués entre les abonnements aux événements pour cette rubrique.|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DeadLetteredCount|Oui|Événements de lettres mortes|Count|Total|Nombre total d’événements de lettres mortes correspondant à cet abonnement aux événements|Topic, EventSubscriptionName, DomainEventSubscriptionName, DeadLetterReason|
|DeliveryAttemptFailCount|Non|Événements d’échec de la remise|Count|Total|Nombre total d’événements ayant échoué dans la remise à cet abonnement aux événements|Topic, EventSubscriptionName, DomainEventSubscriptionName, Error, ErrorType|
|DeliverySuccessCount|Oui|Événements remis|Count|Total|Nombre total d’événements remis à cet abonnement aux événements|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Non|Durée du traitement de la destination|Millisecondes|Average|Durée du traitement de la destination en millisecondes|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Oui|Événements annulés|Count|Total|Nombre total d’événements annulés correspondant à cet abonnement aux événements|Topic, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|MatchedEventCount|Oui|Événements correspondants|Count|Total|Nombre total d’événements correspondant à cet abonnement aux événements|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|PublishFailCount|Oui|Événements d'échec de la publication|Count|Total|Nombre total d’événements ayant échoué à publier dans cette rubrique|Topic, ErrorType, Error|
|PublishSuccessCount|Oui|Événements publiés|Count|Total|Nombre total d’événements publiés dans cette rubrique|Rubrique|
|PublishSuccessLatencyInMs|Oui|Latence de réussite de la publication|Millisecondes|Total|Latence de réussite de la publication en millisecondes|Aucune dimension|


## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Oui|Événements de lettres mortes|Count|Total|Nombre total d’événements de lettres mortes correspondant à cet abonnement aux événements|DeadLetterReason|
|DeliveryAttemptFailCount|Non|Événements d’échec de la remise|Count|Total|Nombre total d’événements ayant échoué dans la remise à cet abonnement aux événements|Error, ErrorType|
|DeliverySuccessCount|Oui|Événements remis|Count|Total|Nombre total d’événements remis à cet abonnement aux événements|Aucune dimension|
|DestinationProcessingDurationInMs|Non|Durée du traitement de la destination|Millisecondes|Average|Durée du traitement de la destination en millisecondes|Aucune dimension|
|DroppedEventCount|Oui|Événements annulés|Count|Total|Nombre total d’événements annulés correspondant à cet abonnement aux événements|DropReason|
|MatchedEventCount|Oui|Événements correspondants|Count|Total|Nombre total d’événements correspondant à cet abonnement aux événements|Aucune dimension|


## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|PublishFailCount|Oui|Événements d'échec de la publication|Count|Total|Nombre total d’événements ayant échoué à publier dans cette rubrique|ErrorType, Error|
|PublishSuccessCount|Oui|Événements publiés|Count|Total|Nombre total d’événements publiés dans cette rubrique|Aucune dimension|
|PublishSuccessLatencyInMs|Oui|Latence de réussite de la publication|Millisecondes|Total|Latence de réussite de la publication en millisecondes|Aucune dimension|
|UnmatchedEventCount|Oui|Événements sans correspondance|Count|Total|Nombre total d’événements ne correspondant à aucun des abonnements aux événements pour cette rubrique|Aucune dimension|


## <a name="microsofteventgridpartnernamespaces"></a>Microsoft.EventGrid/partnerNamespaces

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Oui|Événements de lettres mortes|Count|Total|Nombre total d’événements de lettres mortes correspondant à cet abonnement aux événements|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|Non|Événements d’échec de la remise|Count|Total|Nombre total d’événements ayant échoué dans la remise à cet abonnement aux événements|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Oui|Événements remis|Count|Total|Nombre total d’événements remis à cet abonnement aux événements|EventSubscriptionName|
|DestinationProcessingDurationInMs|Non|Durée du traitement de la destination|Millisecondes|Average|Durée du traitement de la destination en millisecondes|EventSubscriptionName|
|DroppedEventCount|Oui|Événements annulés|Count|Total|Nombre total d’événements annulés correspondant à cet abonnement aux événements|DropReason, EventSubscriptionName|
|MatchedEventCount|Oui|Événements correspondants|Count|Total|Nombre total d’événements correspondant à cet abonnement aux événements|EventSubscriptionName|
|PublishFailCount|Oui|Événements d'échec de la publication|Count|Total|Nombre total d’événements ayant échoué à publier dans cette rubrique|ErrorType, Error|
|PublishSuccessCount|Oui|Événements publiés|Count|Total|Nombre total d’événements publiés dans cette rubrique|Aucune dimension|
|PublishSuccessLatencyInMs|Oui|Latence de réussite de la publication|Millisecondes|Total|Latence de réussite de la publication en millisecondes|Aucune dimension|
|UnmatchedEventCount|Oui|Événements sans correspondance|Count|Total|Nombre total d’événements ne correspondant à aucun des abonnements aux événements pour cette rubrique|Aucune dimension|


## <a name="microsofteventgridpartnertopics"></a>Microsoft.EventGrid/partnerTopics

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Oui|Évaluations de filtres avancés|Count|Total|Nombre total de filtres avancés évalués entre les abonnements aux événements pour cette rubrique.|EventSubscriptionName|
|DeadLetteredCount|Oui|Événements de lettres mortes|Count|Total|Nombre total d’événements de lettres mortes correspondant à cet abonnement aux événements|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|Non|Événements d’échec de la remise|Count|Total|Nombre total d’événements ayant échoué dans la remise à cet abonnement aux événements|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Oui|Événements remis|Count|Total|Nombre total d’événements remis à cet abonnement aux événements|EventSubscriptionName|
|DestinationProcessingDurationInMs|Non|Durée du traitement de la destination|Millisecondes|Average|Durée du traitement de la destination en millisecondes|EventSubscriptionName|
|DroppedEventCount|Oui|Événements annulés|Count|Total|Nombre total d’événements annulés correspondant à cet abonnement aux événements|DropReason, EventSubscriptionName|
|MatchedEventCount|Oui|Événements correspondants|Count|Total|Nombre total d’événements correspondant à cet abonnement aux événements|EventSubscriptionName|
|PublishFailCount|Oui|Événements d'échec de la publication|Count|Total|Nombre total d’événements ayant échoué à publier dans cette rubrique|ErrorType, Error|
|PublishSuccessCount|Oui|Événements publiés|Count|Total|Nombre total d’événements publiés dans cette rubrique|Aucune dimension|
|UnmatchedEventCount|Oui|Événements sans correspondance|Count|Total|Nombre total d’événements ne correspondant à aucun des abonnements aux événements pour cette rubrique|Aucune dimension|


## <a name="microsofteventgridsystemtopics"></a>Microsoft.EventGrid/systemTopics

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Oui|Évaluations de filtres avancés|Count|Total|Nombre total de filtres avancés évalués entre les abonnements aux événements pour cette rubrique.|EventSubscriptionName|
|DeadLetteredCount|Oui|Événements de lettres mortes|Count|Total|Nombre total d’événements de lettres mortes correspondant à cet abonnement aux événements|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|Non|Événements d’échec de la remise|Count|Total|Nombre total d’événements ayant échoué dans la remise à cet abonnement aux événements|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Oui|Événements remis|Count|Total|Nombre total d’événements remis à cet abonnement aux événements|EventSubscriptionName|
|DestinationProcessingDurationInMs|Non|Durée du traitement de la destination|Millisecondes|Average|Durée du traitement de la destination en millisecondes|EventSubscriptionName|
|DroppedEventCount|Oui|Événements annulés|Count|Total|Nombre total d’événements annulés correspondant à cet abonnement aux événements|DropReason, EventSubscriptionName|
|MatchedEventCount|Oui|Événements correspondants|Count|Total|Nombre total d’événements correspondant à cet abonnement aux événements|EventSubscriptionName|
|PublishFailCount|Oui|Événements d'échec de la publication|Count|Total|Nombre total d’événements ayant échoué à publier dans cette rubrique|ErrorType, Error|
|PublishSuccessCount|Oui|Événements publiés|Count|Total|Nombre total d’événements publiés dans cette rubrique|Aucune dimension|
|PublishSuccessLatencyInMs|Oui|Latence de réussite de la publication|Millisecondes|Total|Latence de réussite de la publication en millisecondes|Aucune dimension|
|UnmatchedEventCount|Oui|Événements sans correspondance|Count|Total|Nombre total d’événements ne correspondant à aucun des abonnements aux événements pour cette rubrique|Aucune dimension|


## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Oui|Évaluations de filtres avancés|Count|Total|Nombre total de filtres avancés évalués entre les abonnements aux événements pour cette rubrique.|EventSubscriptionName|
|DeadLetteredCount|Oui|Événements de lettres mortes|Count|Total|Nombre total d’événements de lettres mortes correspondant à cet abonnement aux événements|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|Non|Événements d’échec de la remise|Count|Total|Nombre total d’événements ayant échoué dans la remise à cet abonnement aux événements|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Oui|Événements remis|Count|Total|Nombre total d’événements remis à cet abonnement aux événements|EventSubscriptionName|
|DestinationProcessingDurationInMs|Non|Durée du traitement de la destination|Millisecondes|Average|Durée du traitement de la destination en millisecondes|EventSubscriptionName|
|DroppedEventCount|Oui|Événements annulés|Count|Total|Nombre total d’événements annulés correspondant à cet abonnement aux événements|DropReason, EventSubscriptionName|
|MatchedEventCount|Oui|Événements correspondants|Count|Total|Nombre total d’événements correspondant à cet abonnement aux événements|EventSubscriptionName|
|PublishFailCount|Oui|Événements d'échec de la publication|Count|Total|Nombre total d’événements ayant échoué à publier dans cette rubrique|ErrorType, Error|
|PublishSuccessCount|Oui|Événements publiés|Count|Total|Nombre total d’événements publiés dans cette rubrique|Aucune dimension|
|PublishSuccessLatencyInMs|Oui|Latence de réussite de la publication|Millisecondes|Total|Latence de réussite de la publication en millisecondes|Aucune dimension|
|UnmatchedEventCount|Oui|Événements sans correspondance|Count|Total|Nombre total d’événements ne correspondant à aucun des abonnements aux événements pour cette rubrique|Aucune dimension|


## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActiveConnections|Non|ActiveConnections|Count|Average|Nombre total de connexions actives pour Microsoft.EventHub.|Aucune dimension|
|AvailableMemory|Non|Mémoire disponible|Pourcentage|Maximale|Mémoire disponible pour le cluster Event Hub, en pourcentage de la mémoire totale.|Role|
|CaptureBacklog|Non|Backlog des captures.|Count|Total|Backlog des captures de Microsoft.EventHub.|Aucune dimension|
|CapturedBytes|Non|Octets capturés.|Octets|Total|Octets capturés pour Microsoft.EventHub.|Aucune dimension|
|CapturedMessages|Non|Messages capturés.|Count|Total|Messages capturés pour Microsoft.EventHub.|Aucune dimension|
|ConnectionsClosed|Non|Connexions fermées.|Count|Average|Connexions fermées pour Microsoft.EventHub.|Aucune dimension|
|ConnectionsOpened|Non|Connexions ouvertes.|Count|Average|Connexions ouvertes pour Microsoft.EventHub.|Aucune dimension|
|UC|Non|UC|Pourcentage|Maximale|Utilisation de l’UC en pourcentage du cluster Event Hub|Role|
|IncomingBytes|Oui|Octets entrants.|Octets|Total|Octets entrants pour Microsoft.EventHub.|Aucune dimension|
|IncomingMessages|Oui|Messages entrants|Count|Total|Messages entrants pour Microsoft.EventHub.|Aucune dimension|
|IncomingRequests|Oui|Demandes entrantes|Count|Total|Demandes entrantes pour Microsoft.EventHub.|Aucune dimension|
|OutgoingBytes|Oui|Octets sortants.|Octets|Total|Octets sortants pour Microsoft.EventHub.|Aucune dimension|
|OutgoingMessages|Oui|Messages sortants|Count|Total|Messages sortants pour Microsoft.EventHub.|Aucune dimension|
|QuotaExceededErrors|Non|Erreurs de dépassement de quota.|Count|Total|Erreurs de dépassement de quota pour Microsoft.EventHub.|OperationResult|
|ServerErrors|Non|Erreurs de serveur.|Count|Total|Erreurs de serveur pour Microsoft.EventHub.|OperationResult|
|Taille|Non|Taille|Octets|Average|Taille d’un EventHub en octets.|Role|
|SuccessfulRequests|Non|Requêtes ayant réussi|Count|Total|Requête réussies pour Microsoft.EventHub.|OperationResult|
|ThrottledRequests|Non|Demandes limitées.|Count|Total|Demandes limitées pour Microsoft.EventHub.|OperationResult|
|UserErrors|Non|Erreurs d’utilisateur.|Count|Total|Erreurs d’utilisateur pour Microsoft.EventHub.|OperationResult|


## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActiveConnections|Non|ActiveConnections|Count|Average|Nombre total de connexions actives pour Microsoft.EventHub.|Aucune dimension|
|CaptureBacklog|Non|Backlog des captures.|Count|Total|Backlog des captures de Microsoft.EventHub.|EntityName|
|CapturedBytes|Non|Octets capturés.|Octets|Total|Octets capturés pour Microsoft.EventHub.|EntityName|
|CapturedMessages|Non|Messages capturés.|Count|Total|Messages capturés pour Microsoft.EventHub.|EntityName|
|ConnectionsClosed|Non|Connexions fermées.|Count|Average|Connexions fermées pour Microsoft.EventHub.|EntityName|
|ConnectionsOpened|Non|Connexions ouvertes.|Count|Average|Connexions ouvertes pour Microsoft.EventHub.|EntityName|
|EHABL|Oui|Archiver les messages de backlog (déconseillé)|Count|Total|Messages archivés Event Hub dans le backlog pour un espace de noms (déconseillé)|Aucune dimension|
|EHAMBS|Oui|Archiver le débit des messages (déconseillé)|Octets|Total|Débit de messages archivés Event Hub dans un espace de noms (déconseillé)|Aucune dimension|
|EHAMSGS|Oui|Messages archivés (déconseillé)|Count|Total|Messages archivés Event Hub pour un espace de noms (déconseillé)|Aucune dimension|
|EHINBYTES|Oui|Octets entrants (déconseillé)|Octets|Total|Débit de messages entrants Event Hub pour un espace de noms (déconseillé)|Aucune dimension|
|EHINMBS|Oui|Octets entrants (obsolète) (déprécié)|Octets|Total|Débit de messages entrants Event Hub pour un espace de noms. Cette métrique est déconseillée. Utilisez plutôt la métrique Octets entrants (déconseillé)|Aucune dimension|
|EHINMSGS|Oui|Messages entrants (déconseillé)|Count|Total|Nombre total de messages entrants pour un espace de noms (déconseillé)|Aucune dimension|
|EHOUTBYTES|Oui|Octets sortants (déconseillé)|Octets|Total|Débit de messages sortants Event Hub pour un espace de noms (déconseillé)|Aucune dimension|
|EHOUTMBS|Oui|Octets sortants (obsolète) (déprécié)|Octets|Total|Débit de messages sortants Event Hub pour un espace de noms. Cette métrique est déconseillée. Utilisez plutôt la métrique Octets sortants (déconseillé)|Aucune dimension|
|EHOUTMSGS|Oui|Messages sortants (déconseillé)|Count|Total|Nombre total de messages sortants pour un espace de noms (déconseillé)|Aucune dimension|
|FAILREQ|Oui|Requêtes non réussies (déconseillé)|Count|Total|Nombre total de requêtes non réussies pour un espace de noms (déconseillé)|Aucune dimension|
|IncomingBytes|Oui|Octets entrants.|Octets|Total|Octets entrants pour Microsoft.EventHub.|EntityName|
|IncomingMessages|Oui|Messages entrants|Count|Total|Messages entrants pour Microsoft.EventHub.|EntityName|
|IncomingRequests|Oui|Demandes entrantes|Count|Total|Demandes entrantes pour Microsoft.EventHub.|EntityName|
|INMSGS|Oui|Messages entrants (obsolète) (déprécié)|Count|Total|Nombre total de messages entrants pour un espace de noms. Cette métrique est déconseillée. Utilisez plutôt la métrique Messages entrants (déconseillé)|Aucune dimension|
|INREQS|Oui|Requêtes entrantes (déconseillé)|Count|Total|Nombre total des requêtes d’envoi entrantes pour un espace de noms (déconseillé)|Aucune dimension|
|INTERR|Oui|Erreurs internes du serveur (déconseillé)|Count|Total|Nombre total d’erreurs internes du serveur pour un espace de noms (déconseillé)|Aucune dimension|
|MISCERR|Oui|Autres erreurs (déconseillé)|Count|Total|Nombre total de requêtes non réussies pour un espace de noms (déconseillé)|Aucune dimension|
|OutgoingBytes|Oui|Octets sortants.|Octets|Total|Octets sortants pour Microsoft.EventHub.|EntityName|
|OutgoingMessages|Oui|Messages sortants|Count|Total|Messages sortants pour Microsoft.EventHub.|EntityName|
|OUTMSGS|Oui|Messages sortants (obsolète) (déprécié)|Count|Total|Nombre total de messages sortants pour un espace de noms. Cette métrique est déconseillée. Utilisez plutôt la métrique Messages sortants (déconseillé)|Aucune dimension|
|QuotaExceededErrors|Non|Erreurs de dépassement de quota.|Count|Total|Erreurs de dépassement de quota pour Microsoft.EventHub.|EntityName, OperationResult|
|ServerErrors|Non|Erreurs de serveur.|Count|Total|Erreurs de serveur pour Microsoft.EventHub.|EntityName, OperationResult|
|Taille|Non|Taille|Octets|Average|Taille d’un EventHub en octets.|EntityName|
|SuccessfulRequests|Non|Requêtes ayant réussi|Count|Total|Requête réussies pour Microsoft.EventHub.|EntityName, OperationResult|
|SUCCREQ|Oui|Requêtes réussies (déconseillé)|Count|Total|Nombre total de requêtes réussies pour un espace de noms (déconseillé)|Aucune dimension|
|SVRBSY|Oui|Erreurs de serveur occupé (déconseillé)|Count|Total|Nombre total d’erreurs de serveur occupé pour un espace de noms (déconseillé)|Aucune dimension|
|ThrottledRequests|Non|Demandes limitées.|Count|Total|Demandes limitées pour Microsoft.EventHub.|EntityName, OperationResult|
|UserErrors|Non|Erreurs d’utilisateur.|Count|Total|Erreurs d’utilisateur pour Microsoft.EventHub.|EntityName, OperationResult|


## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|CategorizedGatewayRequests|Oui|Demandes de la passerelle par catégorie|Count|Total|Nombre de demandes de la passerelle par catégories (1xx/2xx/3xx/4xx/5xx)|httpStatus|
|GatewayRequests|Oui|Demandes de la passerelle|Count|Total|Nombre de demandes de la passerelle|httpStatus|
|KafkaRestProxy.ConsumerRequest.m1_delta|Oui|Débit des demandes de consommateur du proxy REST|CountPerSecond|Total|Nombre de demandes de consommateur au proxy REST Kafka|Machine, Rubrique|
|KafkaRestProxy.ConsumerRequestFail.m1_delta|Oui|Échecs de demandes de consommateur de proxy REST|CountPerSecond|Total|Exceptions de demande de consommateur|Machine, Rubrique|
|KafkaRestProxy.ConsumerRequestTime.p95|Oui|Latence des demandes de consommateur du proxy REST|Millisecondes|Average|Latence des messages dans une demande de consommateur via le proxy REST Kafka|Machine, Rubrique|
|KafkaRestProxy.ConsumerRequestWaitingInQueueTime.p95|Oui|Backlog de demande de consommateur de proxy REST|Millisecondes|Average|Longueur de file d’attente de proxy REST de consommateur|Machine, Rubrique|
|KafkaRestProxy.MessagesIn.m1_delta|Oui|Débit des messages de producteur du proxy REST|CountPerSecond|Total|Nombre de messages de producteur via le proxy REST Kafka|Machine, Rubrique|
|KafkaRestProxy.MessagesOut.m1_delta|Oui|Débit des messages de consommateur du proxy REST|CountPerSecond|Total|Nombre de messages de consommateur via le proxy REST Kafka|Machine, Rubrique|
|KafkaRestProxy.OpenConnections|Oui|Connexions simultanées du proxy REST|Count|Total|Nombre de connexions simultanées via le proxy REST Kafka|Machine, Rubrique|
|KafkaRestProxy.ProducerRequest.m1_delta|Oui|Débit des demandes de producteur du proxy REST|CountPerSecond|Total|Nombre de demandes de producteur au proxy REST Kafka|Machine, Rubrique|
|KafkaRestProxy.ProducerRequestFail.m1_delta|Oui|Échecs de demandes de producteur de proxy REST|CountPerSecond|Total|Exceptions de demande de producteur|Machine, Rubrique|
|KafkaRestProxy.ProducerRequestTime.p95|Oui|Latence des demandes de producteur du proxy REST|Millisecondes|Average|Latence des messages dans une demande de producteur via le proxy REST Kafka|Machine, Rubrique|
|KafkaRestProxy.ProducerRequestWaitingInQueueTime.p95|Oui|Backlog de demande de producteur de proxy REST|Millisecondes|Average|Longueur de file d’attente de proxy REST de producteur|Machine, Rubrique|
|NumActiveWorkers|Oui|Nombre de collaborateurs actifs|Count|Maximale|Nombre de collaborateurs actifs|MetricName|
|PendingCPU|Yes|UC en attente|Count|Maximale|Demandes d’UC en attente dans YARN|Aucune dimension|
|PendingMemory|Yes|Mémoire en attente|Count|Maximale|Demandes de mémoire en attente dans YARN|Aucune dimension|


## <a name="microsofthealthcareapisservices"></a>Microsoft.HealthcareApis/services

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Taux de disponibilité du service.|Aucune dimension|
|CosmosDbCollectionSize|Oui|Taille de la collection Cosmos DB|Octets|Total|Taille de la collection Cosmos DB sous-jacente, en octets.|Aucune dimension|
|CosmosDbIndexSize|Oui|Taille de l’index Cosmos DB|Octets|Total|Taille en octets de l’index de la collection Cosmos DB sous-jacente.|Aucune dimension|
|CosmosDbRequestCharge|Oui|Utilisation des RU Cosmos DB|Count|Total|L’utilisation en RU des demandes à la base de données Cosmos DB du service.|Opération, ResourceType|
|CosmosDbRequests|Oui|Demandes Cosmos DB du service|Count|SUM|Nombre total de demandes effectuées à la base de données Cosmos DB sous-jacente du service.|Opération, ResourceType|
|CosmosDbThrottleRate|Oui|Taux de limitation Cosmos DB du service|Count|SUM|Nombre total de réponses 429 de la base de données Cosmos DB sous-jacente du service.|Opération, ResourceType|
|IoTConnectorDeviceEvent|Oui|Nombre de messages entrants|Count|SUM|Nombre total de messages reçus par le connecteur Azure IoT pour FHIR avant normalisation.|Opération, ConnectorName|
|IoTConnectorDeviceEventProcessingLatencyMs|Oui|Latence moyenne de la phase de normalisation|Millisecondes|Average|Délai moyen entre le moment de l’ingestion d’un événement et le moment où l’événement est traité pour normalisation.|Opération, ConnectorName|
|IoTConnectorMeasurement|Oui|Nombre de mesures|Count|SUM|Nombre de lectures de valeurs normalisées reçues par l’étape de conversion FHIR du connecteur Azure IoT pour FHIR.|Opération, ConnectorName|
|IoTConnectorMeasurementGroup|Oui|Nombre de groupes de messages|Count|SUM|Nombre total de regroupements uniques de mesures sur le type, l’appareil, le patient et la période configurée, générés par l’étape de conversion FHIR.|Opération, ConnectorName|
|IoTConnectorMeasurementIngestionLatencyMs|Oui|Latence moyenne de la phase de regroupement|Millisecondes|Average|Délai entre le moment où le connecteur IoT a reçu les données de l’appareil et le moment où les données sont traitées par l’étape de conversion FHIR.|Opération, ConnectorName|
|IoTConnectorNormalizedEvent|Oui|Nombre de messages normalisés|Count|SUM|Nombre total de valeurs normalisées mappées produites par la phase de normalisation du connecteur Azure IoT pour FHIR.|Opération, ConnectorName|
|IoTConnectorTotalErrors|Oui|Nombre total d'erreurs|Count|SUM|Nombre total d’erreurs consignées par le connecteur Azure IoT pour FHIR|Nom, opération, ErrorType, ErrorSeverity, ConnectorName|
|ServiceApiErrors|Oui|Erreurs du service|Count|SUM|Nombre total d’erreurs de serveur internes générées par le service.|Protocole, Authentification, Opération, ResourceType, StatusCode, StatusCodeClass, StatusCodeText|
|ServiceApiLatency|Oui|Latence du service|Millisecondes|Average|Latence de réponse du service.|Protocole, Authentification, Opération, ResourceType, StatusCode, StatusCodeClass, StatusCodeText|
|ServiceApiRequests|Oui|Service Requests|Count|SUM|Nombre total de demandes reçues par le service.|Protocole, Authentification, Opération, ResourceType, StatusCode, StatusCodeClass, StatusCodeText|
|TotalErrors|Oui|Nombre total d’erreurs|Count|SUM|Nombre total d’erreurs de serveur internes rencontrées par le service.|Protocole, StatusCode, StatusCodeClass, StatusCodeText|
|TotalLatency|Oui|Latence totale|Millisecondes|Average|Latence de réponse du service.|Protocol|
|TotalRequests|Oui|Total de requêtes|Count|SUM|Nombre total de demandes reçues par le service.|Protocol|


## <a name="microsofthealthcareapisworkspacesiotconnectors"></a>Microsoft.HealthcareApis/workspaces/iotconnectors

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|DeviceEvent|Oui|Nombre de messages entrants|Count|SUM|Nombre total de messages reçus par le connecteur Azure IoT pour FHIR avant normalisation.|Operation, ResourceName|
|DeviceEventProcessingLatencyMs|Oui|Latence moyenne de la phase de normalisation|Millisecondes|Average|Délai moyen entre le moment de l’ingestion d’un événement et le moment où l’événement est traité pour normalisation.|Operation, ResourceName|
|IoTConnectorTotalErrors|Oui|Nombre total d'erreurs|Count|SUM|Nombre total d’erreurs consignées par le connecteur Azure IoT pour FHIR|Name, Operation, ErrorType, ErrorSeverity, ResourceName|
|Mesure|Oui|Nombre de mesures|Count|SUM|Nombre de lectures de valeurs normalisées reçues par l’étape de conversion FHIR du connecteur Azure IoT pour FHIR.|Operation, ResourceName|
|MeasurementGroup|Oui|Nombre de groupes de messages|Count|SUM|Nombre total de regroupements uniques de mesures sur le type, l’appareil, le patient et la période configurée, générés par l’étape de conversion FHIR.|Operation, ResourceName|
|MeasurementIngestionLatencyMs|Oui|Latence moyenne de la phase de regroupement|Millisecondes|Average|Délai entre le moment où le connecteur IoT a reçu les données de l’appareil et le moment où les données sont traitées par l’étape de conversion FHIR.|Operation, ResourceName|
|NormalizedEvent|Oui|Nombre de messages normalisés|Count|SUM|Nombre total de valeurs normalisées mappées produites par la phase de normalisation du connecteur Azure IoT pour FHIR.|Operation, ResourceName|


## <a name="microsofthybridnetworknetworkfunctions"></a>microsoft.hybridnetwork/networkfunctions

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|HyperVVirtualProcessorUtilization|Oui|Utilisation moyenne de l'UC|Pourcentage|Average|Pourcentage moyen total d’utilisation du processeur virtuel à un intervalle d’une minute. Le nombre total de processeurs virtuels est basé sur la valeur configurée par l’utilisateur dans la définition de la référence SKU. Un filtre supplémentaire peut être appliqué en fonction du RoleName défini dans la référence SKU.|InstanceName|


## <a name="microsofthybridnetworkvirtualnetworkfunctions"></a>microsoft.hybridnetwork/virtualnetworkfunctions

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|HyperVVirtualProcessorUtilization|Oui|Utilisation moyenne de l'UC|Pourcentage|Average|Pourcentage moyen total d’utilisation du processeur virtuel à un intervalle d’une minute. Le nombre total de processeurs virtuels est basé sur la valeur configurée par l’utilisateur dans la définition de la référence SKU. Un filtre supplémentaire peut être appliqué en fonction du RoleName défini dans la référence SKU.|InstanceName|


## <a name="microsoftinsightsautoscalesettings"></a>microsoft.insights/autoscalesettings

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|MetricThreshold|Oui|Seuil de métrique|Count|Average|Seuil de mise à l’échelle automatique configurée lors de l’exécution de la mise à l’échelle automatique.|MetricTriggerRule|
|ObservedCapacity|Oui|Capacité observée|Count|Average|Capacité envoyée à la mise à l’échelle automatique lors de l’exécution.|Aucune dimension|
|ObservedMetricValue|Oui|Valeur de métrique observée|Count|Average|Valeur calculée par mise à l’échelle automatique lors de l’exécution|MetricTriggerSource|
|ScaleActionsInitiated|Oui|Actions de mise à l’échelle initiées|Count|Total|Direction de l’opération de mise à l’échelle.|ScaleDirection|


## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|availabilityResults/availabilityPercentage|Oui|Disponibilité|Pourcentage|Average|Pourcentage de tests de disponibilité terminés|availabilityResult/name, availabilityResult/location|
|availabilityResults/count|Non|Tests de disponibilité|Count|Count|Nombre de tests de disponibilité|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|availabilityResults/duration|Oui|Durée du test de disponibilité|Millisecondes|Average|Durée du test de disponibilité|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|browserTimings/networkDuration|Oui|Temps de connexion au réseau pour le chargement de page|Millisecondes|Average|Temps écoulé entre la requête de l’utilisateur et la connexion réseau. Inclut la connexion de recherche DNS et de transport.|Aucune dimension|
|browserTimings/processingDuration|Oui|Temps de traitement du client|Millisecondes|Average|Temps écoulé entre la réception du dernier octet d’un document et le chargement du modèle DOM. Les demandes asynchrones peuvent encore être en cours de traitement.|Aucune dimension|
|browserTimings/receiveDuration|Oui|Temps de réception de réponse|Millisecondes|Average|Temps écoulé entre le premier et le dernier octet, ou jusqu’à la déconnexion.|Aucune dimension|
|browserTimings/sendDuration|Oui|Temps d’envoi de demande|Millisecondes|Average|Temps écoulé entre la connexion réseau et la réception du premier octet.|Aucune dimension|
|browserTimings/totalDuration|Oui|Temps de chargement de la page de navigateur|Millisecondes|Average|Temps s’écoulant entre la demande de l’utilisateur et le chargement du DOM, des feuilles de style, des scripts et des images.|Aucune dimension|
|dependencies/count|Non|Appels de dépendance|Count|Count|Nombre d’appels effectués par l’application à des ressources externes.|dependency/type, dependency/performanceBucket, dependency/success, dependency/target, dependency/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/duration|Oui|Durée de la dépendance|Millisecondes|Average|Durée des appels effectués par l’application aux ressources externes.|dependency/type, dependency/performanceBucket, dependency/success, dependency/target, dependency/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/failed|Non|Échecs d'appel de dépendance|Count|Count|Nombre d’échecs des appels de dépendance effectués par l’application à des ressources externes.|dependency/type, dependency/performanceBucket, dependency/target, dependency/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|exceptions/browser|Non|Exceptions du navigateur|Count|Count|Nombre d’exceptions non interceptées levées dans le navigateur.|cloud/roleName|
|exceptions/count|Oui|Exceptions|Count|Count|Nombre combiné de toutes les exceptions non interceptées.|cloud/roleName, cloud/roleInstance, client/type|
|exceptions/server|Non|Exceptions de serveur|Count|Count|Nombre d’exceptions non interceptées levées dans l’application serveur.|cloud/roleName, cloud/roleInstance|
|pageViews/count|Oui|Affichages de page|Count|Count|Nombre de pages consultées.|operation/synthetic, cloud/roleName|
|pageViews/duration|Oui|Temps de chargement de la page consultée|Millisecondes|Average|Temps de chargement de la page consultée|operation/synthetic, cloud/roleName|
|performanceCounters/exceptionsPerSecond|Oui|Taux d’exceptions|CountPerSecond|Average|Nombre d’exceptions prises en charge et non prises en charge signalées à Windows, notamment les exceptions .NET et les exceptions non managées converties en exceptions .NET.|cloud/roleInstance|
|performanceCounters/memoryAvailableBytes|Oui|Mémoire disponible|Octets|Average|Mémoire physique prête à être allouée immédiatement pour un processus ou pour le système.|cloud/roleInstance|
|performanceCounters/processCpuPercentage|Oui|Processeur de processus|Pourcentage|Average|Temps écoulé en pourcentage que tous les threads de processus a passé à utiliser le processeur pour exécuter des instructions. Il peut varier de 0 à 100. Cette métrique indique les performances du processus w3wp uniquement.|cloud/roleInstance|
|performanceCounters/processIOBytesPerSecond|Oui|Taux d’E/S du processus|BytesPerSecond|Average|Nombre total d’octets par seconde lus et écrits sur des fichiers, un réseau et des appareils.|cloud/roleInstance|
|performanceCounters/processorCpuPercentage|Oui|Temps processeur|Pourcentage|Average|Temps en pourcentage que le processus a passé dans des threads actifs.|cloud/roleInstance|
|performanceCounters/processPrivateBytes|Oui|Octets privés du processus|Octets|Average|Mémoire allouée exclusivement aux processus de l’application analysée.|cloud/roleInstance|
|performanceCounters/requestExecutionTime|Oui|Durée d’exécution de la requête HTTP|Millisecondes|Average|Durée d’exécution de la demande la plus récente.|cloud/roleInstance|
|performanceCounters/requestsInQueue|Oui|Requêtes HTTP dans la file d’attente de l’application|Count|Average|Longueur de la file d'attente des demandes d'application.|cloud/roleInstance|
|performanceCounters/requestsPerSecond|Oui|Taux de requêtes HTTP|CountPerSecond|Average|Taux par seconde de l'ensemble des demandes à l'application provenant d'ASP.NET.|cloud/roleInstance|
|requests/count|Non|Requêtes serveur|Count|Count|Nombre de requêtes HTTP terminées.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/duration|Oui|Temps de réponse du serveur|Millisecondes|Average|Temps écoulé entre la réception d'une requête HTTP et la fin de l'envoi de la réponse.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/failed|Non|Demandes ayant échoué|Count|Count|Nombre de requêtes HTTP marquées comme ayant échoué. Dans la plupart des cas, il s’agit de requêtes avec un code de réponse >= 400 et différent de 401.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|requests/taux|Non|Taux de requêtes du serveur|CountPerSecond|Average|Taux de requêtes du serveur par seconde|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|traces/count|Oui|Traces|Count|Count|Nombre de documents de traces|trace/severityLevel, operation/synthetic, cloud/roleName, cloud/roleInstance|


## <a name="microsoftiotcentraliotapps"></a>Microsoft.IoTCentral/IoTApps

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|c2d.commands.failure|Oui|Appels de commande en échec|Count|Total|Nombre total de demandes de commande en échec lancées à partir d’IoT Central|Aucune dimension|
|c2d.commands.requestSize|Oui|Taille de la demande des appels de commande|Octets|Total|Taille de toutes les demandes de commande lancées à partir d’IoT Central|Aucune dimension|
|c2d.commands.responseSize|Oui|Taille de la réponse des appels de commande|Octets|Total|Taille de toutes les réponses de commande lancées à partir d’IoT Central|Aucune dimension|
|c2d.commands.success|Oui|Appels de commande réussis|Count|Total|Nombre total de demandes de commande réussies lancées à partir d’IoT Central|Aucune dimension|
|c2d.property.read.failure|Oui|Lectures des propriétés d’appareil ayant échoué à partir d’IoT Central|Count|Total|Nombre total de lectures des propriétés lancées à partir d’IoT Central ayant échoué|Aucune dimension|
|c2d.property.read.success|Oui|Lectures des propriétés d’appareil ayant abouti à partir d’IoT Central|Count|Total|Nombre total de lectures des propriétés lancées à partir d’IoT Central ayant abouti|Aucune dimension|
|c2d.property.update.failure|Oui|Mises à jour des propriétés d’appareil ayant échoué à partir d’IoT Central|Count|Total|Nombre total de mises à jour des propriétés lancées à partir d’IoT Central ayant échoué|Aucune dimension|
|c2d.property.update.success|Oui|Mises à jour des propriétés d’appareil ayant abouti à partir d’IoT Central|Count|Total|Nombre total de mises à jour des propriétés lancées à partir d’IoT Central ayant abouti|Aucune dimension|
|connectedDeviceCount|Non|Nombre total d’appareils connectés|Count|Average|Nombre d’appareils connectés à IoT Central|Aucune dimension|
|d2c.property.read.failure|Oui|Lectures des propriétés d’appareil ayant échoué à partir d’appareils|Count|Total|Nombre total de lectures des propriétés lancées à partir d’appareils ayant échoué|Aucune dimension|
|d2c.property.read.success|Oui|Lectures des propriétés d’appareil ayant abouti à partir d’appareils|Count|Total|Nombre total de lectures des propriétés lancées à partir d’appareils ayant abouti|Aucune dimension|
|d2c.property.update.failure|Oui|Mises à jour des propriétés d’appareil ayant échoué à partir d’appareils|Count|Total|Nombre total de mises à jour des propriétés lancées à partir d’appareils ayant échoué|Aucune dimension|
|d2c.property.update.success|Oui|Mises à jour des propriétés d’appareil ayant abouti à partir d’appareils|Count|Total|Nombre total de mises à jour des propriétés lancées à partir d’appareils ayant abouti|Aucune dimension|
|d2c.telemetry.ingress.allProtocol|Oui|Nombre total de tentatives d’envoi de messages de télémétrie|Count|Total|Nombre de tentatives d’envoi de messages de télémétrie appareil-à-cloud à l’application IoT Central|Aucune dimension|
|d2c.telemetry.ingress.success|Oui|Nombre total de messages de télémétrie envoyés|Count|Total|Nombre de messages de télémétrie appareil-à-cloud envoyés à l’application IoT Central|Aucune dimension|
|dataExport.error|Oui|Erreurs d’exportation de données|Count|Total|Nombre d’erreurs rencontrées pour l’exportation de données|exportId, exportDisplayName, destinationId, destinationDisplayName|
|dataExport.messages.filtered|Oui|Messages d’exportation de données filtrés|Count|Total|Nombre de messages passés par des filtres dans l’exportation de données|exportId, exportDisplayName, destinationId, destinationDisplayName|
|dataExport.messages.received|Oui|Messages d’exportation de données reçus|Count|Total|Nombre de messages entrants pour l’exportation de données, avant le filtrage et le traitement de l’enrichissement|exportId, exportDisplayName, destinationId, destinationDisplayName|
|dataExport.messages.written|Oui|Messages d’exportation de données écrits|Count|Total|Nombre de messages écrits sur une destination|exportId, exportDisplayName, destinationId, destinationDisplayName|
|dataExport.statusChange|Oui|Modification de l’état de l’exportation des données|Count|Total|Nombre de modifications d’état|exportId, exportDisplayName, destinationId, destinationDisplayName, status|
|deviceDataUsage|Oui|Utilisation totale des données d’appareil|Octets|Total|Octets transférés vers et depuis tous les appareils connectés à l’application IoT Central|Aucune dimension|
|provisionedDeviceCount|Non|Nombre total d’appareils approvisionnés|Count|Average|Nombre d’appareils approvisionnés dans l’application IoT Central|Aucune dimension|


## <a name="microsoftkeyvaultmanagedhsms"></a>microsoft.keyvault/managedhsms

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Non|Disponibilité globale du service|Pourcentage|Average|Disponibilité des demandes du service|ActivityType, ActivityName, StatusCode, StatusCodeClass|
|ServiceApiHit|Oui|Correspondances totales de l'API de service|Count|Count|Nombre total de correspondances de l'API de service|ActivityType, ActivityName|
|ServiceApiLatency|Non|Latence globale de l'API de service|Millisecondes|Average|Latence globale des demandes de l'API de service|ActivityType, ActivityName, StatusCode, StatusCodeClass|


## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité globale du coffre|Pourcentage|Average|Disponibilité des demandes de coffre|ActivityType, ActivityName, StatusCode, StatusCodeClass|
|SaturationShoebox|Non|Saturation globale du coffre|Pourcentage|Average|Capacité de coffre utilisée|ActivityType, ActivityName, TransactionType|
|ServiceApiHit|Oui|Correspondances totales de l'API de service|Count|Count|Nombre total de correspondances de l'API de service|ActivityType, ActivityName|
|ServiceApiLatency|Oui|Latence globale de l'API de service|Millisecondes|Average|Latence globale des demandes de l'API de service|ActivityType, ActivityName, StatusCode, StatusCodeClass|
|ServiceApiResult|Oui|Résultats totaux de l'API de service|Count|Count|Nombre total de résultats de l'API de service|ActivityType, ActivityName, StatusCode, StatusCodeClass|


## <a name="microsoftkubernetesconnectedclusters"></a>microsoft.kubernetes/connectedClusters

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|capacity_cpu_cores|Oui|Nombre total de cœurs de processeur dans un cluster connecté|Count|Total|Nombre total de cœurs de processeur dans un cluster connecté|Aucune dimension|


## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BatchBlobCount|Oui|Nombre d’objets blob du lot|Count|Average|Nombre de sources de données d’un lot agrégé pour l’ingestion|Base de données|
|BatchDuration|Oui|Durée du lot|Secondes|Average|Durée de la phase d’agrégation du flux d’ingestion|Base de données|
|BatchesProcessed|Oui|Lots traités|Count|Total|Nombre de lots agrégés pour l’ingestion. Type de traitement par lot : indique si le lot a atteint la limite concernant le temps de traitement par lot, la taille des données ou le nombre de fichiers qui est définie par la stratégie de traitement par lot|Database, SealReason|
|BatchSize|Oui|Taille du lot|Octets|Average|Taille de données attendue non compressée dans un lot agrégé pour l’ingestion|Base de données|
|BlobsDropped|Oui|Objets blob supprimés|Count|Total|Nombre d’objets blob définitivement rejetés par un composant.|Database, ComponentType, ComponentName|
|BlobsProcessed|Oui|Objets blob traités|Count|Total|Nombre d’objets blob traités par un composant.|Database, ComponentType, ComponentName|
|BlobsReceived|Oui|Objets blob reçus|Count|Total|Nombre d’objets blob reçus du flux d’entrée par un composant.|Database, ComponentType, ComponentName|
|CacheUtilization|Oui|Utilisation du cache|Pourcentage|Average|Niveau d’utilisation dans l’étendue du cluster|Aucune dimension|
|CacheUtilizationFactor|Yes|Facteur d’utilisation du cache|Pourcentage|Average|Différence en pourcentage entre le nombre actuel d’instances et le nombre optimal d’instances (par utilisation du cache)|Aucune dimension|
|ContinuousExportMaxLatenessMinutes|Oui|Retard max. pour l’exportation continue|Count|Maximale|Retard (en minutes) signalé par les travaux d’exportation continue dans le cluster|Aucune dimension|
|ContinuousExportNumOfRecordsExported|Oui|Exportation continue du nombre d’enregistrements exportés|Nombre|Total|Nombre d’enregistrements exportés, déclenchés pour chaque artefact de stockage écrit pendant l’opération d’exportation|ContinuousExportName, Database|
|ContinuousExportPendingCount|Oui|Nombre en attente d’exportations continues|Count|Maximale|Nombre de travaux d’exportation continue en attente prêts pour l’exécution|Aucune dimension|
|ContinuousExportResult|Oui|Résultat de l’exportation continue|Count|Count|Indique si l’exportation continue a réussi ou échoué|ContinuousExportName, Result, Database|
|UC|Oui|UC|Pourcentage|Average|Niveau d’utilisation de l’UC|Aucune dimension|
|DiscoveryLatency|Oui|Latence de découverte|Secondes|Average|Signalé par les connexions de données (si elles existent). Durée en secondes entre la mise en file d’attente d’un message ou la création d’un événement et sa découverte par la connexion de données. Cette durée n’est pas incluse dans la durée totale d’ingestion d’Azure Data Explorer.|ComponentType, ComponentName|
|EventsDropped|Oui|Événements supprimés|Count|Total|Nombre d’événements supprimés de façon permanente par la connexion de données. Une mesure du résultat de l’ingestion avec une raison de l’échec sera envoyée.|ComponentType, ComponentName|
|EventsProcessed|Oui|Événements traités|Count|Total|Nombre d’événements traités par le cluster|ComponentType, ComponentName|
|EventsProcessedForEventHubs|Oui|Événements traités (pour Event Hubs/IoT Hub)|Count|Total|Nombre d’événements traités par le cluster lors de l’ingestion à partir d’Event/IoT Hub|EventStatus|
|EventsReceived|Oui|Événements reçus|Count|Total|Nombre d’événements reçus par la connexion de données.|ComponentType, ComponentName|
|ExportUtilization|Oui|Utilisation de l’exportation|Pourcentage|Maximale|Utilisation de l’exportation|Aucune dimension|
|IngestionLatencyInSeconds|Oui|Latence d’ingestion|Secondes|Average|Latence des données ingérées, depuis la réception des données dans le cluster jusqu’à ce qu’elles soient prêtes à être interrogées. La période de latence d’ingestion varie en fonction du scénario d’ingestion.|Aucune dimension|
|IngestionResult|Oui|Résultat de l’ingestion|Count|Total|Nombre total de sources dont l’ingestion a été une réussite ou un échec. En divisant la métrique par état, vous pouvez obtenir des informations détaillées sur l’état des opérations d’ingestion.|IngestionResultDetails, FailureKind|
|IngestionUtilization|Oui|Utilisation de l’ingestion|Pourcentage|Average|Ratio d’emplacements d’ingestion utilisés dans le cluster|Aucune dimension|
|IngestionVolumeInMB|Oui|Volume d’ingestion|Octets|Total|Volume global de données ingérées dans le cluster|Base de données|
|InstanceCount|Oui|Nombre d'instances|Count|Average|Nombre total d’instances|Aucune dimension|
|KeepAlive|Oui|Keep alive|Count|Average|Contrôle d’intégrité indiquant que le cluster répond aux requêtes|Aucune dimension|
|MaterializedViewAgeMinutes|Oui|Âge de la vue matérialisée|Count|Average|Âge de la vue matérialisée en minutes|Base de données, MaterializedViewName|
|MaterializedViewAgeSeconds|Oui|Âge de la vue matérialisée|Secondes|Average|Âge de la vue matérialisée en secondes|Base de données, MaterializedViewName|
|MaterializedViewDataLoss|Oui|Perte de données de la vue matérialisée|Count|Maximale|Indique une perte de données potentielle dans une vue matérialisée|Base de données, MaterializedViewName, Genre|
|MaterializedViewExtentsRebuild|Oui|Régénération des extensions de vue matérialisée|Count|Average|Nombre d’étendues regénérées|Base de données, MaterializedViewName|
|MaterializedViewHealth|Oui|Intégrité de la vue matérialisée|Count|Average|Intégrité de la vue matérialisée (1 pour saine, 0 pour non saine)|Base de données, MaterializedViewName|
|MaterializedViewRecordsInDelta|Oui|Enregistrements de vue matérialisée dans delta|Count|Average|Nombre d’enregistrements dans la partie non matérialisée de la vue|Base de données, MaterializedViewName|
|MaterializedViewResult|Oui|Résultat de la vue matérialisée|Count|Average|Résultat du processus de matérialisation|Base de données, MaterializedViewName, Résultat|
|QueryDuration|Oui|Durée de la requête|Millisecondes|Average|Durée des requêtes en secondes|QueryStatus|
|QueryResult|Non|Résultat de la requête|Count|Count|Nombre total de requêtes.|QueryStatus|
|QueueLength|Oui|Longueur de la file d’attente|Count|Average|Nombre de messages en attente dans la file d’attente d’un composant.|ComponentType|
|QueueOldestMessage|Oui|Message le plus ancien de la file d’attente|Count|Average|Temps écoulé en secondes depuis le moment où le message le plus ancien dans la file d’attente a été inséré.|ComponentType|
|ReceivedDataSizeBytes|Oui|Taille des données reçues en octets|Octets|Average|Taille des données reçues par la connexion de données. Il s’agit de la taille du flux de données, ou de la taille des données brutes si elle est fournie.|ComponentType, ComponentName|
|StageLatency|Oui|Latence des phases|Secondes|Average|Temps cumulé entre la découverte d’un message et sa réception par le composant de création de rapports pour le traitement (l’heure de découverte est définie lorsque le message est mis en file d’attente pour la file d’attente d’ingestion, ou lorsqu’il est détecté par la connexion de données).|Database, ComponentType|
|SteamingIngestRequestRate|Oui|Taux de demandes d’ingestion en streaming|Count|RateRequestsPerSecond|Taux de demandes d’ingestion en streaming (Mo par seconde)|Aucune dimension|
|StreamingIngestDataRate|Oui|Taux de données d’ingestion en streaming|Count|Average|Taux de données d’ingestion en streaming (Mo par seconde)|Aucune dimension|
|StreamingIngestDuration|Oui|Durée de l’ingestion en streaming|Millisecondes|Average|Durée de l’ingestion en streaming en millisecondes|Aucune dimension|
|StreamingIngestResults|Oui|Résultat de l’ingestion en streaming|Count|Average|Résultat de l’ingestion en streaming|Résultats|
|TotalNumberOfConcurrentQueries|Oui|Nombre total de demandes simultanées|Count|Maximale|Nombre total de demandes simultanées|Aucune dimension|
|TotalNumberOfExtents|Oui|Nombre total d’étendues|Count|Total|Nombre total d’étendues de données|Aucune dimension|
|TotalNumberOfThrottledCommands|Oui|Nombre total de commandes limitées|Count|Total|Nombre total de commandes limitées|CommandType|
|TotalNumberOfThrottledQueries|Oui|Nombre total de demandes limitées|Count|Maximale|Nombre total de demandes limitées|Aucune dimension|
|WeakConsistencyLatency|Yes|Latence de cohérence faible|Secondes|Average|Latence maximale entre la synchronisation précédente des métadonnées et la suivante (dans l’étendue de la base de métadonnées/du nœud)|Database, RoleInstance|


## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft.Logic/IntegrationServiceEnvironments

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActionLatency|Oui|Latence de l’action |Secondes|Average|Latence des actions de flux de travail terminés.|Aucune dimension|
|ActionsCompleted|Oui|Actions terminées |Count|Total|Nombre d’actions de flux de travail terminées.|Aucune dimension|
|ActionsFailed|Oui|Actions ayant échoué |Count|Total|Nombre d’actions de flux de travail ayant échoué.|Aucune dimension|
|ActionsSkipped|Oui|Actions ignorées |Count|Total|Nombre d’actions de flux de travail ignorées.|Aucune dimension|
|ActionsStarted|Oui|Actions démarrées |Count|Total|Nombre d’actions de flux de travail démarrées.|Aucune dimension|
|ActionsSucceeded|Oui|Actions ayant réussi |Count|Total|Nombre d’actions de flux de travail ayant réussi.|Aucune dimension|
|ActionSuccessLatency|Oui|Latence de réussite d’action |Secondes|Average|Latence des actions de flux de travail ayant réussi.|Aucune dimension|
|IntegrationServiceEnvironmentConnectorMemoryUsage|Oui|Utilisation de la mémoire du connecteur pour l’environnement de service d’intégration|Pourcentage|Average|Utilisation de la mémoire du connecteur pour l’environnement de service d’intégration.|Aucune dimension|
|IntegrationServiceEnvironmentConnectorProcessorUsage|Oui|Utilisation du processeur du connecteur pour l’environnement de service d’intégration|Pourcentage|Average|Utilisation du processeur du connecteur pour l’environnement de service d’intégration.|Aucune dimension|
|IntegrationServiceEnvironmentWorkflowMemoryUsage|Oui|Utilisation de la mémoire de flux de travail pour l’environnement de service d’intégration|Pourcentage|Average|Utilisation de la mémoire de flux de travail pour l’environnement de service d’intégration.|Aucune dimension|
|IntegrationServiceEnvironmentWorkflowProcessorUsage|Oui|Utilisation du processeur de flux de travail pour l’environnement de service d’intégration|Pourcentage|Average|Utilisation du processeur de flux de travail pour l’environnement de service d’intégration.|Aucune dimension|
|RunLatency|Oui|Latence d’exécution|Secondes|Average|Latence des exécutions de flux de travail terminées.|Aucune dimension|
|RunsCancelled|Oui|Exécutions annulées|Count|Total|Nombre d’exécutions de flux de travail annulées.|Aucune dimension|
|RunsCompleted|Oui|Exécutions terminées|Count|Total|Nombre d’exécutions de flux de travail terminées.|Aucune dimension|
|RunsFailed|Oui|Exécutions ayant échoué|Count|Total|Nombre d’exécutions de flux de travail ayant échoué.|Aucune dimension|
|RunsStarted|Oui|Exécutions démarrées|Count|Total|Nombre d’exécutions de flux de travail démarrées.|Aucune dimension|
|RunsSucceeded|Oui|Exécutions réussies|Count|Total|Nombre d’exécutions de flux de travail ayant réussi.|Aucune dimension|
|RunSuccessLatency|Oui|Latence d’exécution ayant réussi|Secondes|Average|Latence des exécutions de flux de travail ayant réussi.|Aucune dimension|
|TriggerFireLatency|Oui|Latence d’activation de déclencheur |Secondes|Average|Latence des déclencheurs de flux de travail activés.|Aucune dimension|
|TriggerLatency|Oui|Latence de déclencheur |Secondes|Average|Latence des déclenchements de flux de travail terminés.|Aucune dimension|
|TriggersCompleted|Oui|Déclenchements terminés |Count|Total|Nombre de déclencheurs de flux de travail terminés.|Aucune dimension|
|TriggersFailed|Oui|Déclenchements ayant échoué |Count|Total|Nombre de déclencheurs de flux de travail ayant échoué.|Aucune dimension|
|TriggersFired|Oui|Déclenchements activés |Count|Total|Nombre de déclencheurs de flux de travail activés.|Aucune dimension|
|TriggersSkipped|Oui|Déclenchements ignorés|Count|Total|Nombre de déclencheurs de flux de travail ignorés.|Aucune dimension|
|TriggersStarted|Oui|Déclenchements lancés |Count|Total|Nombre de déclencheurs de flux de travail démarrés.|Aucune dimension|
|TriggersSucceeded|Oui|Déclenchements ayant réussi |Count|Total|Nombre de déclencheurs de flux de travail ayant réussi.|Aucune dimension|
|TriggerSuccessLatency|Oui|Latence déclencheur ayant réussi |Secondes|Average|Latence des déclencheurs de flux de travail ayant réussi.|Aucune dimension|


## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/Workflows

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActionLatency|Oui|Latence de l’action |Secondes|Average|Latence des actions de flux de travail terminés.|Aucune dimension|
|ActionsCompleted|Oui|Actions terminées |Count|Total|Nombre d’actions de flux de travail terminées.|Aucune dimension|
|ActionsFailed|Oui|Actions ayant échoué |Count|Total|Nombre d’actions de flux de travail ayant échoué.|Aucune dimension|
|ActionsSkipped|Oui|Actions ignorées |Count|Total|Nombre d’actions de flux de travail ignorées.|Aucune dimension|
|ActionsStarted|Oui|Actions démarrées |Count|Total|Nombre d’actions de flux de travail démarrées.|Aucune dimension|
|ActionsSucceeded|Oui|Actions ayant réussi |Count|Total|Nombre d’actions de flux de travail ayant réussi.|Aucune dimension|
|ActionSuccessLatency|Oui|Latence de réussite d’action |Secondes|Average|Latence des actions de flux de travail ayant réussi.|Aucune dimension|
|ActionThrottledEvents|Oui|Événements d’action limitée|Count|Total|Nombre d’événements limités d’action de flux de travail.|Aucune dimension|
|BillableActionExecutions|Oui|Exécutions d’actions facturables|Count|Total|Nombre d’exécutions d’actions de flux de travail facturées.|Aucune dimension|
|BillableTriggerExecutions|Oui|Exécutions de déclencheurs facturables|Count|Total|Nombre d’exécutions de déclencheurs de flux de travail facturées.|Aucune dimension|
|BillingUsageNativeOperation|Oui|Utilisation de la facturation pour les exécutions d’opérations natives|Count|Total|Nombre d’exécutions d’opérations natives facturées.|Aucune dimension|
|BillingUsageStandardConnector|Oui|Utilisation de la facturation pour les exécutions de connecteurs Standard|Count|Total|Nombre d’exécutions de connecteurs Standard facturées.|Aucune dimension|
|BillingUsageStorageConsumption|Oui|Utilisation de la facturation pour les exécutions de consommation du stockage|Count|Total|Nombre d’exécutions de consommation du stockage facturées.|Aucune dimension|
|RunFailurePercentage|Oui|Pourcentage d’échec des exécutions|Pourcentage|Total|Pourcentage d’exécutions de flux de travail ayant échoué.|Aucune dimension|
|RunLatency|Oui|Latence d’exécution|Secondes|Average|Latence des exécutions de flux de travail terminées.|Aucune dimension|
|RunsCancelled|Oui|Exécutions annulées|Count|Total|Nombre d’exécutions de flux de travail annulées.|Aucune dimension|
|RunsCompleted|Oui|Exécutions terminées|Count|Total|Nombre d’exécutions de flux de travail terminées.|Aucune dimension|
|RunsFailed|Oui|Exécutions ayant échoué|Count|Total|Nombre d’exécutions de flux de travail ayant échoué.|Aucune dimension|
|RunsStarted|Oui|Exécutions démarrées|Count|Total|Nombre d’exécutions de flux de travail démarrées.|Aucune dimension|
|RunsSucceeded|Oui|Exécutions réussies|Count|Total|Nombre d’exécutions de flux de travail ayant réussi.|Aucune dimension|
|RunStartThrottledEvents|Oui|Événements limités démarrés par l’exécution|Count|Total|Nombre d’événements limités de démarrage de l’exécution de flux de travail.|Aucune dimension|
|RunSuccessLatency|Oui|Latence d’exécution ayant réussi|Secondes|Average|Latence des exécutions de flux de travail ayant réussi.|Aucune dimension|
|RunThrottledEvents|Oui|Exécuter événements limités|Count|Total|Nombre d’actions de flux de travail ou d’événements limités par déclencheur.|Aucune dimension|
|TotalBillableExecutions|Oui|Nombre total d’exécutions facturables|Count|Total|Nombre d’exécutions de flux de travail facturées.|Aucune dimension|
|TriggerFireLatency|Oui|Latence d’activation de déclencheur |Secondes|Average|Latence des déclencheurs de flux de travail activés.|Aucune dimension|
|TriggerLatency|Oui|Latence de déclencheur |Secondes|Average|Latence des déclenchements de flux de travail terminés.|Aucune dimension|
|TriggersCompleted|Oui|Déclenchements terminés |Count|Total|Nombre de déclencheurs de flux de travail terminés.|Aucune dimension|
|TriggersFailed|Oui|Déclenchements ayant échoué |Count|Total|Nombre de déclencheurs de flux de travail ayant échoué.|Aucune dimension|
|TriggersFired|Oui|Déclenchements activés |Count|Total|Nombre de déclencheurs de flux de travail activés.|Aucune dimension|
|TriggersSkipped|Oui|Déclenchements ignorés|Count|Total|Nombre de déclencheurs de flux de travail ignorés.|Aucune dimension|
|TriggersStarted|Oui|Déclenchements lancés |Count|Total|Nombre de déclencheurs de flux de travail démarrés.|Aucune dimension|
|TriggersSucceeded|Oui|Déclenchements ayant réussi |Count|Total|Nombre de déclencheurs de flux de travail ayant réussi.|Aucune dimension|
|TriggerSuccessLatency|Oui|Latence déclencheur ayant réussi |Secondes|Average|Latence des déclencheurs de flux de travail ayant réussi.|Aucune dimension|
|TriggerThrottledEvents|Oui|Événements limités par déclencheur|Count|Total|Nombre d’événements de flux de travail limités par déclencheur.|Aucune dimension|


## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/workspaces

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Cœurs actifs|Oui|Cœurs actifs|Count|Average|Nombre de cœurs actifs|Scenario, ClusterName|
|Nœuds actifs|Oui|Nœuds actifs|Count|Average|Nombre de nœuds actifs. Ce sont les nœuds qui exécutent activement un travail.|Scenario, ClusterName|
|Annuler les exécutions demandées|Oui|Annuler les exécutions demandées|Count|Total|Nombre d’exécutions pour lesquelles l’annulation a été demandée dans cet espace de travail. Le nombre est mis à jour à la réception d’une demande d’annulation pour une exécution.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Exécutions annulées|Oui|Exécutions annulées|Count|Total|Nombre d’exécutions annulées pour cet espace de travail. Le nombre est mis à jour lorsque l’annulation d’une exécution aboutit.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Exécutions terminées|Oui|Exécutions terminées|Count|Total|Nombre d’exécutions réussies pour cet espace de travail. Le nombre est mis à jour lorsqu’une exécution est terminée et que la sortie a été collectée.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|CpuCapacityMillicores|Yes|CpuCapacityMillicores|Count|Average|Capacité maximale d’un nœud de processeur en millicœurs. La capacité est agrégée par intervalles d’une minute.|RunId, InstanceId, ComputeName|
|CpuMemoryCapacityMegabytes|Yes|CpuMemoryCapacityMegabytes|Count|Average|Utilisation maximale de la mémoire d’un nœud de processeur en mégaoctets. L’utilisation est agrégée par intervalles d’une minute.|RunId, InstanceId, ComputeName|
|CpuMemoryUtilizationMegabytes|Yes|CpuMemoryUtilizationMegabytes|Count|Average|Utilisation de la mémoire d’un nœud de processeur en mégaoctets. L’utilisation est agrégée par intervalles d’une minute.|RunId, InstanceId, ComputeName|
|CpuMemoryUtilizationPercentage|Yes|CpuMemoryUtilizationPercentage|Count|Average|Pourcentage d’utilisation de la mémoire d’un nœud de processeur. L’utilisation est agrégée par intervalles d’une minute.|RunId, InstanceId, ComputeName|
|CpuUtilization|Oui|CpuUtilization|Count|Average|Pourcentage d’utilisation de la mémoire sur un nœud de processeur. L’utilisation est rapportée à intervalles d’une minute.|Scenario, runId, NodeId, ClusterName|
|CpuUtilizationMillicores|Yes|CpuUtilizationMillicores|Count|Average|Utilisation d’un nœud de processeur en millicœurs. L’utilisation est agrégée par intervalles d’une minute.|RunId, InstanceId, ComputeName|
|CpuUtilizationPercentage|Yes|CpuUtilizationPercentage|Count|Average|Pourcentage d’utilisation d’un nœud de processeur. L’utilisation est agrégée par intervalles d’une minute.|RunId, InstanceId, ComputeName|
|Erreurs|Oui|Erreurs|Count|Total|Nombre d’erreurs d’exécution dans cet espace de travail. Le nombre est mis à jour chaque fois que l’exécution rencontre une erreur.|Scénario|
|Exécutions échouées|Oui|Exécutions échouées|Count|Total|Nombre d’exécutions en échec pour cet espace de travail. Le nombre est mis à jour lorsqu’une exécution échoue.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Exécutions en cours de finalisation|Oui|Exécutions en cours de finalisation|Count|Total|Nombre d’exécutions entrées en état de finalisation pour cet espace de travail. Le nombre est mis à jour lorsqu’une exécution est terminée, mais que la collecte de sortie est toujours en cours.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|GpuCapacityMilliGPUs|Yes|GpuCapacityMilliGPUs|Count|Average|Capacité maximale d’un périphérique GPU en milli-GPU. La capacité est agrégée par intervalles d’une minute.|RunId, InstanceId, DeviceId, ComputeName|
|GpuEnergyJoules|Yes|GpuEnergyJoules|Nombre|Total|Énergie par intervalle en joules sur un nœud GPU. L’énergie est rapportée à intervalles d’une minute.|Scenario, runId, rootRunId, InstanceId, DeviceId, ComputeName|
|GpuMemoryCapacityMegabytes|Yes|GpuMemoryCapacityMegabytes|Count|Average|Capacité de mémoire maximale d’un périphérique GPU en mégaoctets. La capacité est agrégée par intervalles d’une minute.|RunId, InstanceId, DeviceId, ComputeName|
|GpuMemoryUtilization|Oui|GpuMemoryUtilization|Count|Average|Pourcentage d’utilisation de la mémoire sur un nœud de GPU. L’utilisation est rapportée à intervalles d’une minute.|Scenario, runId, NodeId, DeviceId, ClusterName|
|GpuMemoryUtilizationMegabytes|Yes|GpuMemoryUtilizationMegabytes|Count|Average|Utilisation de la mémoire d’un périphérique GPU en mégaoctets. L’utilisation est agrégée par intervalles d’une minute.|RunId, InstanceId, DeviceId, ComputeName|
|GpuMemoryUtilizationPercentage|Yes|GpuMemoryUtilizationPercentage|Count|Average|Pourcentage d’utilisation de la mémoire d’un périphérique GPU. L’utilisation est agrégée par intervalles d’une minute.|RunId, InstanceId, DeviceId, ComputeName|
|GpuUtilization|Oui|GpuUtilization|Count|Average|Pourcentage d’utilisation sur un nœud de GPU. L’utilisation est rapportée à intervalles d’une minute.|Scenario, runId, NodeId, DeviceId, ClusterName|
|GpuUtilizationMilliGPUs|Yes|GpuUtilizationMilliGPUs|Count|Average|Utilisation d’un périphérique GPU en milli-GPU. L’utilisation est agrégée par intervalles d’une minute.|RunId, InstanceId, DeviceId, ComputeName|
|GpuUtilizationPercentage|Yes|GpuUtilizationPercentage|Count|Average|Pourcentage d’utilisation d’un périphérique GPU. L’utilisation est agrégée par intervalles d’une minute.|RunId, InstanceId, DeviceId, ComputeName|
|IBReceiveMegabytes|Yes|IBReceiveMegabytes|Count|Average|Données réseau reçues par InfiniBand en mégaoctets. Les métriques sont agrégées par intervalles d’une minute.|RunId, InstanceId, ComputeName|
|IBTransmitMegabytes|Yes|IBTransmitMegabytes|Count|Average|Données réseau envoyées par InfiniBand en mégaoctets. Les métriques sont agrégées par intervalles d’une minute.|RunId, InstanceId, ComputeName|
|Cœurs inactifs|Oui|Cœurs inactifs|Count|Average|Nombre de cœurs inactifs|Scenario, ClusterName|
|Nœuds inactifs|Oui|Nœuds inactifs|Count|Average|Nombre de nœuds inactifs. Les nœuds inactifs sont les nœuds qui n’exécutent aucun travail, mais qui peuvent accepter un nouveau travail, le cas échéant.|Scenario, ClusterName|
|Cœurs sortants|Oui|Cœurs sortants|Count|Average|Nombre de cœurs sortants|Scenario, ClusterName|
|Nœuds sortants|Oui|Nœuds sortants|Count|Average|Nombre de nœuds sortants. Les nœuds sortants sont les nœuds qui viennent de terminer le traitement d’un travail et qui passent à l’état inactif.|Scenario, ClusterName|
|Échec du déploiement de modèle|Oui|Échec du déploiement de modèle|Count|Total|Nombre de déploiements de modèles ayant échoué dans cet espace de travail|Scenario, StatusCode|
|Déploiements de modèles commencés|Oui|Déploiements de modèles commencés|Count|Total|Nombre de déploiements de modèles ayant commencé dans cet espace de travail|Scénario|
|Déploiements de modèles réussis|Oui|Déploiements de modèles réussis|Count|Total|Nombre de déploiements de modèles ayant réussi dans cet espace de travail|Scénario|
|Enregistrements de modèles ayant échoué|Oui|Enregistrements de modèles ayant échoué|Count|Total|Nombre d’enregistrements de modèles ayant échoué dans cet espace de travail|Scenario, StatusCode|
|Enregistrements de modèles réussis|Oui|Enregistrements de modèles réussis|Count|Total|Nombre d’enregistrements de modèles ayant réussi dans cet espace de travail|Scénario|
|NetworkInputMegabytes|Yes|NetworkInputMegabytes|Count|Average|Données réseau reçues en mégaoctets. Les métriques sont agrégées par intervalles d’une minute.|RunId, InstanceId, ComputeName|
|NetworkOutputMegabytes|Yes|NetworkOutputMegabytes|Count|Average|Données réseau envoyées en mégaoctets. Les métriques sont agrégées par intervalles d’une minute.|RunId, InstanceId, ComputeName|
|Exécutions sans réponse|Oui|Exécutions sans réponse|Count|Total|Nombre d’exécutions sans réponse pour cet espace de travail. Le nombre est mis à jour lorsqu’une exécution passe à l’état Sans réponse.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Exécutions non démarrées|Oui|Exécutions non démarrées|Count|Total|Nombre d’exécutions à l’état Non démarré pour cet espace de travail. Le nombre est mis à jour lorsqu’une demande de création d’une exécution est reçue, mais que les informations d’exécution n’ont pas encore été remplies. |Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Cœurs réquisitionnés|Oui|Cœurs réquisitionnés|Count|Average|Nombre de cœurs réquisitionnés|Scenario, ClusterName|
|Nœuds reportés|Oui|Nœuds reportés|Count|Average|Nombre de nœuds reportés. Ces nœuds sont les nœuds de basse priorité qui sont éloignés du pool de nœuds disponibles.|Scenario, ClusterName|
|Exécutions en cours de préparation|Oui|Exécutions en cours de préparation|Count|Total|Nombre d’exécutions en cours de préparation pour cet espace de travail. Le nombre est mis à jour lorsqu’une exécution passe à l’état En préparation pendant la préparation de l’environnement d’exécution.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Exécutions en cours de provisionnement|Oui|Exécutions en cours de provisionnement|Count|Total|Nombre d’exécutions en cours de provisionnement pour cet espace de travail. Le nombre est mis à jour lorsqu’une exécution attend la création ou le provisionnement d’une cible de calcul.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Exécutions en file d’attente|Oui|Exécutions en file d’attente|Count|Total|Nombre d’exécutions mises en file d’attente pour cet espace de travail. Le nombre est mis à jour lorsqu’une exécution est mise en file d’attente dans la cible de calcul. Peut se produire en attendant que les nœuds de calcul requis soit prêts.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Pourcentage d’utilisation du quota|Oui|Pourcentage d’utilisation du quota|Count|Average|Pourcentage de quota utilisé|Scenario, ClusterName, VmFamilyName, VmPriority|
|Exécutions démarrées|Oui|Exécutions démarrées|Count|Total|Nombre d’exécutions en cours pour cet espace de travail. Le nombre est mis à jour lorsque l’exécution commence sur les ressources requises.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Exécutions en cours de démarrage|Oui|Exécutions en cours de démarrage|Count|Total|Nombre d’exécutions démarrées pour cet espace de travail. Le nombre est mis à jour après que la demande de création de l’exécution et des informations d’exécution (par exemple, l’ID d’exécution) a été remplie.|Scenario, RunType, PublishedPipelineId, ComputeType, PipelineStepType, ExperimentName|
|Nombre total de cœurs|Oui|Nombre total de cœurs|Count|Average|Nombre total de cœurs|Scenario, ClusterName|
|Nombre total de nœuds|Oui|Nombre total de nœuds|Count|Average|Nombre total de nœuds. Ce total comprend des nœuds actifs, des nœuds inactifs, des nœuds inutilisables, des nœuds reportés et des nœuds sortants.|Scenario, ClusterName|
|Cœurs inutilisables|Oui|Cœurs inutilisables|Count|Average|Nombre de cœurs inutilisables|Scenario, ClusterName|
|Nœuds inutilisables|Oui|Nœuds inutilisables|Count|Average|Nombre de nœuds inutilisables. Les nœuds inutilisables ne sont pas fonctionnels en raison d’un problème insoluble. Azure recycle ces nœuds.|Scenario, ClusterName|
|Avertissements|Oui|Avertissements|Count|Total|Nombre d’avertissements d’exécution dans cet espace de travail. Le nombre est mis à jour chaque fois qu’une exécution rencontre un avertissement.|Scénario|


## <a name="microsoftmapsaccounts"></a>Microsoft.Maps/accounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Disponibilité des API|ApiCategory, ApiName|
|CreatorUsage|No|Utilisation de Creator|Octets|Average|Statistiques d’utilisation d’Azure Maps Creator|NomService|
|Usage|Non|Usage|Count|Count|Nombre d’appels d’API|ApiCategory, ApiName, ResultType, ResponseCode|


## <a name="microsoftmediamediaservices"></a>Microsoft.Media/mediaservices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AssetCount|Oui|Nombre de ressources|Count|Average|Nombre de ressources déjà créées dans le compte de service multimédia actuel|Aucune dimension|
|AssetQuota|Oui|Quota de ressources|Count|Average|Nombre de ressources autorisées pour le compte de service multimédia actuel|Aucune dimension|
|AssetQuotaUsedPercentage|Oui|Pourcentage du quota de ressources utilisé|Pourcentage|Average|Pourcentage de ressources utilisées dans le compte de service multimédia actuel|Aucune dimension|
|ChannelsAndLiveEventsCount|Oui|Nombre d’événements en direct|Count|Average|Nombre total d’événements en direct dans le compte Media Services actif|Aucune dimension|
|ContentKeyPolicyCount|Oui|Nombre de stratégies de clé de contenu|Count|Average|Nombre de stratégies de clé de contenu déjà créées dans le compte de service multimédia actuel|Aucune dimension|
|ContentKeyPolicyQuota|Oui|Quota de stratégies de clé de contenu|Count|Average|Nombre de stratégies de clé de contenu autorisées pour le compte de service multimédia actuel|Aucune dimension|
|ContentKeyPolicyQuotaUsedPercentage|Oui|Pourcentage du quota de stratégies de clé de contenu utilisé|Pourcentage|Average|Pourcentage de stratégies de clé de contenu utilisées dans le compte de service multimédia actuel|Aucune dimension|
|JobsScheduled|Yes|Travaux planifiés|Count|Average|Nombre de travaux à l’état Planifié. Les chiffres de cette métrique reflètent uniquement les travaux soumis via l’API v3. Les travaux soumis via l’API v2 (héritée) ne sont pas comptabilisés.|Aucune dimension|
|MaxChannelsAndLiveEventsCount|Oui|Quota maximum d’événements en direct|Count|Average|Nombre maximum d’événements en direct autorisés dans le compte Media Services actif|Aucune dimension|
|MaxRunningChannelsAndLiveEventsCount|Oui|Quota maximum d’événements en direct en cours d’exécution|Count|Average|Nombre maximum d’événements en direct en cours d’exécution autorisés dans le compte Media Services actif|Aucune dimension|
|RunningChannelsAndLiveEventsCount|Oui|Nombre d’événements en direct en cours d’exécution|Count|Average|Nombre total d’événements en direct en cours d’exécution dans le compte Media Services actif|Aucune dimension|
|StreamingPolicyCount|Oui|Nombre de stratégies de diffusion en continu|Count|Average|Nombre de stratégies de streaming déjà créées dans le compte de service multimédia actuel|Aucune dimension|
|StreamingPolicyQuota|Oui|Quota de stratégies de diffusion en continu|Count|Average|Nombre de stratégies de streaming autorisées pour le compte de service multimédia actuel|Aucune dimension|
|StreamingPolicyQuotaUsedPercentage|Oui|Pourcentage du quota de stratégies de diffusion en continu utilisé|Pourcentage|Average|Pourcentage de stratégies de streaming utilisées dans le compte de service multimédia actuel|Aucune dimension|


## <a name="microsoftmediamediaservicesliveevents"></a>Microsoft.Media/mediaservices/liveEvents

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|IngestBitrate|Oui|Débit d’ingestion des événements en direct|BitsPerSecond|Average|Le débit entrant ingéré pour un événement en direct, en bits par seconde.|TrackName|
|IngestDriftValue|Oui|Valeur de la dérive de l’ingestion des événements en direct|Secondes|Maximale|Dérive entre l’horodatage du contenu ingéré et l’horloge système, mesurée en secondes par minute. Une valeur différente de zéro indique que le contenu ingéré arrive plus lentement que l’horloge système.|TrackName|
|IngestLastTimestamp|Oui|Dernier horodatage de réception d’événement en direct|Millisecondes|Maximale|Dernier horodatage ingéré pour un événement en direct.|TrackName|
|LiveOutputLastTimestamp|Oui|Horodatage de la dernière sortie|Millisecondes|Maximale|Horodatage du dernier fragment téléchargé vers le stockage pour une sortie d’événement en direct.|TrackName|


## <a name="microsoftmediamediaservicesstreamingendpoints"></a>Microsoft.Media/mediaservices/streamingEndpoints

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|UC|Oui|Utilisation de l’UC|Pourcentage|Average|Utilisation de l’UC pour les points de terminaison de streaming Premium. Ces données ne sont pas disponibles pour les points de terminaison de streaming Standard.|Aucune dimension|
|Sortie|Oui|Sortie|Octets|Total|Quantité de données de sortie, en octets.|OutputFormat|
|EgressBandwidth|Non|Bande passante en sortie|BitsPerSecond|Average|Sortie de la bande passante en bits par seconde.|Aucune dimension|
|Demandes|Oui|Demandes|Count|Total|Demandes à un point de terminaison de streaming.|OutputFormat, HttpStatusCode, ErrorCode|
|SuccessE2ELatency|Oui|Latence de réussite de bout en bout|Millisecondes|Average|Latence moyenne des demandes réussies, en millisecondes.|OutputFormat|


## <a name="microsoftmixedrealityremoterenderingaccounts"></a>Microsoft.MixedReality/remoteRenderingAccounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActiveRenderingSessions|Oui|Sessions de rendu actives|Count|Average|Nombre total de sessions de rendu actives|SessionType, SDKVersion|
|AssetsConverted|Oui|Ressources converties|Count|Total|Nombre total de ressources converties|SDKVersion|


## <a name="microsoftmixedrealityspatialanchorsaccounts"></a>Microsoft.MixedReality/spatialAnchorsAccounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AnchorsCreated|Oui|Ancres créées|Count|Total|Nombre d’ancres créées|DeviceFamily, SDKVersion|
|AnchorsDeleted|Oui|Ancres supprimées|Count|Total|Nombre d’ancres supprimées|DeviceFamily, SDKVersion|
|AnchorsQueried|Oui|Ancres interrogées|Count|Total|Nombre d’ancres spatiales interrogées|DeviceFamily, SDKVersion|
|AnchorsUpdated|Oui|Ancres mises à jour|Count|Total|Nombre d’ancres mises à jour|DeviceFamily, SDKVersion|
|PosesFound|Oui|Poses trouvées|Count|Total|Nombre de poses retournées|DeviceFamily, SDKVersion|
|TotalDailyAnchors|Oui|Total des ancres quotidiennes|Count|Average|Nombre total d’ancres - Quotidiennes|DeviceFamily, SDKVersion|


## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|VolumePoolAllocatedSize|Oui|Taille allouée au pool|Octets|Average|Taille provisionnée de ce pool|Aucune dimension|
|VolumePoolAllocatedToVolumeThroughput|Oui|Débit alloué du pool|BytesPerSecond|Average|Somme du débit de tous les volumes appartenant au pool|Aucune dimension|
|VolumePoolAllocatedUsed|Oui|Pool alloué à la taille du volume|Octets|Average|Taille allouée utilisée du pool|Aucune dimension|
|VolumePoolProvisionedThroughput|Oui|Débit provisionné pour le pool|BytesPerSecond|Average|Débit provisionné de ce pool|Aucune dimension|
|VolumePoolTotalLogicalSize|Oui|Taille du pool consommée|Octets|Average|Somme de la taille logique de tous les volumes appartenant au pool|Aucune dimension|
|VolumePoolTotalSnapshotSize|Oui|Taille totale des instantanés du pool|Octets|Average|Somme de la taille des instantanés de tous les volumes de ce pool|Aucune dimension|


## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/volumes

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AverageReadLatency|Oui|Latence de lecture moyenne|Millisecondes|Average|Latence de lecture moyenne en millisecondes par opération|Aucune dimension|
|AverageWriteLatency|Oui|Latence d’écriture moyenne|Millisecondes|Average|Latence d’écriture moyenne en millisecondes par opération|Aucune dimension|
|CbsVolumeBackupActive|Oui|Indique si la sauvegarde du volume est suspendue|Count|Average|La stratégie de sauvegarde est-elle suspendue pour le volume ? 0 si oui, 1 si non.|Aucune dimension|
|CbsVolumeLogicalBackupBytes|Oui|Nombre d’octets de la sauvegarde du volume|Octets|Average|Nombre total d’octets sauvegardés pour ce volume.|Aucune dimension|
|CbsVolumeOperationComplete|Oui|Indique si l’opération de sauvegarde du volume est terminée|Count|Average|La dernière opération de sauvegarde ou de restauration du volume s’est-elle terminée correctement ? 1 si oui, sinon 0.|Aucune dimension|
|CbsVolumeOperationTransferredBytes|Oui|Nombre d’octets transférés lors de la dernière sauvegarde du volume|Octets|Average|Nombre total d’octets transférés pour la dernière opération de sauvegarde ou de restauration|Aucune dimension|
|CbsVolumeProtected|Oui|Indique si la sauvegarde du volume est activée|Count|Average|La sauvegarde est-elle activée pour le volume ? 1 si oui, sinon 0.|Aucune dimension|
|OtherThroughput|Oui|Autre débit|BytesPerSecond|Average|Autre débit (qui n’est pas en lecture ou en écriture) en octets par seconde|Aucune dimension|
|ReadIops|Oui|E/S par seconde en lecture|CountPerSecond|Average|Opérations d’E/S en lecture par seconde|Aucune dimension|
|ReadThroughput|Oui|Débit de lecture|BytesPerSecond|Average|Débit de lecture en octets par seconde.|Aucune dimension|
|TotalThroughput|Oui|Débit total|BytesPerSecond|Average|Somme de tous les débits en octets par seconde|Aucune dimension|
|VolumeAllocatedSize|Oui|Taille allouée de volume|Octets|Average|Taille provisionnée d’un volume|Aucune dimension|
|VolumeConsumedSizePercentage|Oui|Taille du volume consommée en pourcentage|Pourcentage|Average|Pourcentage du volume consommé, y compris les instantanés.|Aucune dimension|
|VolumeCoolTierDataReadSize|Yes|Taille de lecture des données du niveau froid du volume|Octets|Average|Données lues à l’aide de l’instruction GET par volume|Aucune dimension|
|VolumeCoolTierDataWriteSize|Yes|Taille d’écriture des données du niveau froid du volume|Octets|Average|Données hiérarchisées à l’aide de l’instruction PUT par volume|Aucune dimension|
|VolumeCoolTierSize|Yes|Taille du niveau froid du volume|Octets|Average|Empreinte de volume pour le niveau froid|Aucune dimension|
|VolumeLogicalSize|Oui|Taille du volume consommée|Octets|Average|Taille logique du volume (octets utilisés)|Aucune dimension|
|VolumeSnapshotSize|Oui|Taille d’instantané du volume|Octets|Average|Taille de tous les instantanés dans le volume|Aucune dimension|
|WriteIops|Oui|E/S par seconde en écriture|CountPerSecond|Average|Opérations d’E/S en écriture par seconde|Aucune dimension|
|WriteThroughput|Oui|Débit d’écriture|BytesPerSecond|Average|Débit d’écriture en octets par seconde|Aucune dimension|
|XregionReplicationHealthy|Oui|L’état de la réplication de volume est-il sain|Count|Average|État de la relation (1 ou 0)|Aucune dimension|
|XregionReplicationLagTime|Oui|Décalage de la réplication de volume|Secondes|Average|Durée, en secondes, du retard des données présentes sur le miroir par rapport à la source|Aucune dimension|
|XregionReplicationLastTransferDuration|Oui|Durée du dernier transfert de réplication de volume|Secondes|Average|Durée, en secondes, nécessaire au dernier transfert|Aucune dimension|
|XregionReplicationLastTransferSize|Oui|Taille du dernier transfert de réplication de volume|Octets|Average|Nombre total d’octets transférés dans le cadre du dernier transfert|Aucune dimension|
|XregionReplicationRelationshipProgress|Oui|Progression de la réplication de volume|Octets|Average|Volume total de données transférées pour l’opération de transfert en cours|Aucune dimension|
|XregionReplicationRelationshipTransferring|Oui|La réplication de volume est-elle en cours de transfert|Count|Average|L’état de la réplication de volume est-il « En cours de transfert »|Aucune dimension|
|XregionReplicationTotalTransferBytes|Oui|Transfert total de la réplication de volume|Octets|Average|Octets cumulés transférés pour la relation|Aucune dimension|


## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationgateways

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ApplicationGatewayTotalTime|Non|Durée totale d’Application Gateway|Millisecondes|Average|Temps moyen nécessaire pour le traitement d’une requête et l’envoi de la réponse. Elle est calculée en fonction de l’intervalle moyen entre le moment où Application Gateway reçoit le premier octet d’une requête HTTP et le moment où l’opération d’envoi d’une réponse se termine. Il est important de noter que cela implique généralement le temps de traitement d’Application Gateway, le temps pendant lequel les paquets de requête et de réponse transitent sur le réseau et le temps que le serveur principal a mis pour répondre.|Écouteur|
|AvgRequestCountPerHealthyHost|Non|Demandes par minute par hôte sain|Count|Average|Nombre moyen de demandes par minute par hôte back-end sain dans un pool|BackendSettingsPool|
|BackendConnectTime|Non|Temps de connexion au back-end|Millisecondes|Average|Temps passé à établir une connexion avec un back-end|Listener, BackendServer, BackendPool, BackendHttpSetting|
|BackendFirstByteResponseTime|Non|Temps de réponse du premier octet du back-end|Millisecondes|Average|Intervalle de temps entre le début de l’établissement d’une connexion au principal et la réception du premier octet de l’en-tête de réponse, ce qui correspond approximativement au temps de traitement du principal|Listener, BackendServer, BackendPool, BackendHttpSetting|
|BackendLastByteResponseTime|Non|Temps de réponse du dernier octet du back-end|Millisecondes|Average|Intervalle de temps entre le début de l’établissement d’une connexion au principal et la réception du dernier octet du corps de la réponse|Listener, BackendServer, BackendPool, BackendHttpSetting|
|BackendResponseStatus|Oui|État de la réponse du back-end|Count|Total|Nombre de codes de réponse HTTP générés par les membres du back-end. Cela n’inclut pas les codes de réponse générés par Application Gateway.|BackendServer, BackendPool, BackendHttpSetting, HttpStatusGroup|
|BackendTlsNegotiationError|Yes|Erreurs de connexion TLS du serveur principal|Nombre|Total|Erreurs de connexion TLS pour le serveur principal de la passerelle applicative|BackendHttpSetting, BackendPool, ErrorType|
|BlockedCount|Oui|Distribution des règles de demandes bloquées du pare-feu d’applications web|Count|Total|Distribution des règles de demandes bloquées du pare-feu d’applications web|RuleGroup, RuleId|
|BlockedReqCount|Oui|Nombre de demandes bloquées du pare-feu d’applications web|Count|Total|Nombre de demandes bloquées du pare-feu d’applications web|Aucune dimension|
|BytesReceived|Oui|Octets reçus|Octets|Total|Nombre total d’octets reçus par Application Gateway à partir des clients|Écouteur|
|BytesSent|Oui|Octets envoyés|Octets|Total|Nombre total d’octets envoyés par Application Gateway aux clients|Écouteur|
|CapacityUnits|Non|Unités de capacité actuelles|Count|Average|Unités de capacité consommées|Aucune dimension|
|ClientRtt|Non|RTT client|Millisecondes|Average|Durée moyenne des allers-retours entre les clients et Application Gateway. Cette métrique indique le temps nécessaire à l’établissement des connexions et au retour des accusés de réception|Écouteur|
|ComputeUnits|Non|Unités Compute actuelles|Count|Average|Unités Compute consommées|Aucune dimension|
|CpuUtilization|Non|Utilisation du processeur|Pourcentage|Average|Utilisation actuelle du processeur d’Application Gateway|Aucune dimension|
|CurrentConnections|Oui|Connexions courantes|Count|Total|Nombre de connexions courantes établies avec Application Gateway|Aucune dimension|
|EstimatedBilledCapacityUnits|Non|Unités de capacité facturées estimées|Count|Average|Unités de capacité estimées qui seront facturées|Aucune dimension|
|FailedRequests|Oui|Demandes ayant échoué|Count|Total|Nombre de requêtes en échec servies par Application Gateway|BackendSettingsPool|
|FixedBillableCapacityUnits|Non|Unités de capacité facturables fixes|Count|Average|Unités de capacité minimales qui seront facturées|Aucune dimension|
|HealthyHostCount|Oui|Nombre d’hôtes intègres|Count|Average|Nombre d’hôtes principaux intègres|BackendSettingsPool|
|MatchedCount|Oui|Distribution totale des règles du pare-feu d’applications web|Count|Total|Distribution totale des règles du pare-feu d’applications web pour le trafic entrant|RuleGroup, RuleId|
|NewConnectionsPerSecond|Non|Nouvelles connexions par seconde|CountPerSecond|Average|Nouvelles connexions par seconde établies avec Application Gateway|Aucune dimension|
|RejectedConnections|Yes|Connexions rejetées|Nombre|Total|Nombre de connexions rejetées pour le serveur frontal de la passerelle applicative|Aucune dimension|
|ResponseStatus|Oui|État de la réponse|Count|Total|État de la réponse HTTP retourné par Application Gateway|HttpStatusGroup|
|Débit|Non|Débit|BytesPerSecond|Average|Nombre d’octets par seconde servis par Application Gateway|Aucune dimension|
|TlsProtocol|Oui|Protocole TLS du client|Count|Total|Nombre de demandes TLS et non-TLS lancées par le client qui a établi la connexion avec Application Gateway. Pour afficher la distribution du protocole TLS, filtrez par la dimension Protocole TLS.|Listener, TlsProtocol|
|TotalRequests|Oui|Total de requêtes|Count|Total|Nombre de requêtes réussies servies par Application Gateway|BackendSettingsPool|
|UnhealthyHostCount|Oui|Nombre d’hôtes non intègres|Count|Average|Nombre d’hôtes principaux non intègres|BackendSettingsPool|


## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ApplicationRuleHit|Oui|Nombre d'accès aux règles d'application|Count|Total|Nombre d'accès aux règles d'application|Status, Reason, Protocol|
|DataProcessed|Oui|Données traitées|Octets|Total|Volume total de données traitées par ce pare-feu|Aucune dimension|
|FirewallHealth|Oui|État d’intégrité du pare-feu|Pourcentage|Average|Intégrité globale de ce pare-feu|Statut, Motif|
|NetworkRuleHit|Oui|Nombre d’accès aux règles de réseau|Count|Total|Nombre d’accès aux règles de réseau|Status, Reason, Protocol|
|SNATPortUtilization|Oui|Utilisation des ports SNAT|Pourcentage|Average|Pourcentage de ports SNAT sortants en cours d’utilisation|Protocol|
|Débit|Non|Débit|BitsPerSecond|Average|Débit traité par ce pare-feu|Aucune dimension|


## <a name="microsoftnetworkbastionhosts"></a>microsoft.network/bastionHosts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|pingmesh|Non|État de communication Bastion|Count|Average|L’état de la communication affiche 1 si toute la communication est bonne, et 0 si elle est mauvaise.|Aucune dimension|
|sessions|Non|Nombre de sessions|Count|Total|Nombre de sessions pour le Bastion. Afficher en somme et par instance.|host|
|total|Oui|Mémoire totale|Count|Average|Statistiques de mémoire totale.|host|
|usage_user|Non|Utilisation du processeur|Count|Average|Statistiques d’utilisation de l’UC.|cpu, host|
|utilisés|Oui|Utilisation de la mémoire|Count|Average|Statistiques d’utilisation de la mémoire.|host|


## <a name="microsoftnetworkconnections"></a>Microsoft.Network/connections

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BitsInPerSecond|Oui|BitsInPerSecond|BitsPerSecond|Average|Bits entrant dans Azure par seconde|Aucune dimension|
|BitsOutPerSecond|Oui|BitsOutPerSecond|BitsPerSecond|Average|Bits sortant d’Azure par seconde|Aucune dimension|


## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|QueryVolume|Non|Volume de requête|Count|Total|Nombre de requêtes servies pour une zone DNS|Aucune dimension|
|RecordSetCapacityUtilization|Non|Utilisation de la capacité du jeu d’enregistrements|Pourcentage|Maximale|Pourcentage de la capacité du jeu d’enregistrements utilisée par une zone DNS|Aucune dimension|
|RecordSetCount|Non|Nombre de jeux d’enregistrements|Count|Maximale|Nombre de jeux d’enregistrements dans une zone DNS|Aucune dimension|


## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ArpAvailability|Oui|Disponibilité du protocole ARP|Pourcentage|Average|Disponibilité du protocole ARP entre MSEE et tous les pairs.|PeeringType, Peer|
|BgpAvailability|Oui|Disponibilité du protocole BGP|Pourcentage|Average|Disponibilité du protocole BGP entre MSEE et tous les pairs.|PeeringType, Peer|
|BitsInPerSecond|Oui|BitsInPerSecond|BitsPerSecond|Average|Bits entrant dans Azure par seconde|PeeringType, DeviceRole|
|BitsOutPerSecond|Oui|BitsOutPerSecond|BitsPerSecond|Average|Bits sortant d’Azure par seconde|PeeringType, DeviceRole|
|GlobalReachBitsInPerSecond|Non|GlobalReachBitsInPerSecond|BitsPerSecond|Average|Bits entrant dans Azure par seconde|PeeredCircuitSKey|
|GlobalReachBitsOutPerSecond|Non|GlobalReachBitsOutPerSecond|BitsPerSecond|Average|Bits sortant d’Azure par seconde|PeeredCircuitSKey|
|QosDropBitsInPerSecond|Oui|DroppedInBitsPerSecond|BitsPerSecond|Average|Octets de données d’entrée abandonnés par seconde|Aucune dimension|
|QosDropBitsOutPerSecond|Oui|DroppedOutBitsPerSecond|BitsPerSecond|Average|Octets de données de sortie abandonnés par seconde|Aucune dimension|


## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BitsInPerSecond|Oui|BitsInPerSecond|BitsPerSecond|Average|Bits entrant dans Azure par seconde|Aucune dimension|
|BitsOutPerSecond|Oui|BitsOutPerSecond|BitsPerSecond|Average|Bits sortant d’Azure par seconde|Aucune dimension|


## <a name="microsoftnetworkexpressroutegateways"></a>Microsoft.Network/expressRouteGateways

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ErGatewayConnectionBitsInPerSecond|Non|BitsInPerSecond|BitsPerSecond|Average|Bits entrant dans Azure par seconde|ConnectionName|
|ErGatewayConnectionBitsOutPerSecond|Non|BitsOutPerSecond|BitsPerSecond|Average|Bits sortant d’Azure par seconde|ConnectionName|
|ExpressRouteGatewayCountOfRoutesAdvertisedToPeer|Oui|Nombre de routes publiées pour le pair (préversion)|Count|Maximale|Nombre de routes publiées pour le pair par ExpressRouteGateway|roleInstance|
|ExpressRouteGatewayCountOfRoutesLearnedFromPeer|Oui|Nombre de routes apprises du pair (préversion)|Count|Maximale|Nombre de routes apprises du pair par ExpressRouteGateway|roleInstance|
|ExpressRouteGatewayCpuUtilization|Oui|Utilisation du processeur|Pourcentage|Average|Utilisation du processeur de la passerelle ExpressRoute|roleInstance|
|ExpressRouteGatewayFrequencyOfRoutesChanged|Non|Fréquence de changement des routes (préversion)|Count|Total|Fréquence de changement des routes dans la passerelle ExpressRoute|roleInstance|
|ExpressRouteGatewayNumberOfVmInVnet|Non|Nombre de machines virtuelles dans le réseau virtuel (préversion)|Count|Maximale|Nombre de machines virtuelles dans le réseau virtuel|Aucune dimension|
|ExpressRouteGatewayPacketsPerSecond|Non|Paquets par seconde|CountPerSecond|Average|Nombre de paquets de la passerelle ExpressRoute|roleInstance|


## <a name="microsoftnetworkexpressrouteports"></a>Microsoft.Network/expressRoutePorts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AdminState|Oui|AdminState|Count|Average|État administrateur du port|Lien|
|LineProtocol|Oui|LineProtocol|Count|Average|État du protocole de ligne du port|Lien|
|PortBitsInPerSecond|Non|BitsInPerSecond|BitsPerSecond|Average|Bits entrant dans Azure par seconde|Lien|
|PortBitsOutPerSecond|Non|BitsOutPerSecond|BitsPerSecond|Average|Bits sortant d’Azure par seconde|Lien|
|RxLightLevel|Oui|RxLightLevel|Count|Average|Niveau d’éclairage de réception en dBm|Link, Lane|
|TxLightLevel|Oui|TxLightLevel|Count|Average|Niveau d’éclairage de transmission en dBm|Link, Lane|


## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BackendHealthPercentage|Oui|Pourcentage d’intégrité du backend|Pourcentage|Average|Pourcentage de sondes d’intégrité réussies du proxy HTTP/S vers les serveurs principaux|Backend, BackendPool|
|BackendRequestCount|Oui|Nombre de requêtes de backend|Count|Total|Nombre de requêtes envoyées du proxy HTTP/S aux serveurs principaux|HttpStatus, HttpStatusGroup, Backend|
|BackendRequestLatency|Oui|Latence de requête du backend|Millisecondes|Average|Temps calculé à partir du moment où la requête est envoyée par le proxy HTTP/S au serveur principal jusqu’à ce que le proxy HTTP/S reçoive le dernier octet de la réponse du serveur principal|Backend|
|BillableResponseSize|Oui|Taille de la réponse facturable|Octets|Total|Nombre d’octets facturables (2 Ko minimum par demande) envoyés en tant que réponses du proxy HTTP/S aux clients.|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestCount|Oui|Nombre de requêtes|Count|Total|Nombre de requêtes clients prises en charge par le proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|Oui|Taille de la requête|Octets|Total|Nombre d’octets envoyés en tant que requêtes de clients au proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Oui|Taille de la réponse|Octets|Total|Nombre d’octets envoyés en tant que réponses du proxy HTTP/S aux clients|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|TotalLatency|Oui|Latence totale|Millisecondes|Average|Temps calculé à partir du moment où la requête du client est reçue par le proxy HTTP/S jusqu’à ce que le client accuse réception du dernier octet de la réponse du proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|WebApplicationFirewallRequestCount|Oui|Nombre de requêtes du pare-feu d’applications web|Count|Total|Nombre de requêtes clients traitées par le pare-feu d’applications web|PolicyName, RuleName, Action|


## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AllocatedSnatPorts|Non|Ports SNAT alloués|Count|Average|Nombre total de ports SNAT alloués pendant la période|FrontendIPAddress, BackendIPAddress, ProtocolType, IsAwaitingRemoval|
|ByteCount|Oui|Nombre d’octets|Octets|Total|Nombre total d’octets transmis dans une période de temps|FrontendIPAddress, FrontendPort, Direction|
|DipAvailability|Oui|État de la sonde d’intégrité|Count|Average|État de la sonde d’intégrité d’équilibreur de charge moyen par durée|ProtocolType, BackendPort, FrontendIPAddress, FrontendPort, BackendIPAddress|
|PacketCount|Oui|Nombre de paquets|Count|Total|Nombre total de paquets transmis dans une période de temps|FrontendIPAddress, FrontendPort, Direction|
|SnatConnectionCount|Oui|Nombre de connexions SNAT|Count|Total|Nombre total de connexions SNAT créées dans une période de temps|FrontendIPAddress, BackendIPAddress, ConnectionState|
|SYNCount|Oui|Nombre de SYN|Count|Total|Nombre total de paquets SYN transmis dans une période de temps|FrontendIPAddress, FrontendPort, Direction|
|UsedSnatPorts|Non|Ports SNAT utilisés|Count|Average|Nombre total de ports SNAT utilisés pendant une période|FrontendIPAddress, BackendIPAddress, ProtocolType, IsAwaitingRemoval|
|VipAvailability|Oui|Disponibilité du chemin d’accès aux données|Count|Average|Disponibilité du chemin d’accès aux données d’équilibreur de charge moyenne par durée|FrontendIPAddress, FrontendPort|


## <a name="microsoftnetworknatgateways"></a>Microsoft.Network/natGateways

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ByteCount|Oui|Octets|Octets|Total|Nombre total d’octets transmis dans une période de temps|Protocole, Direction|
|DatapathAvailability|Oui|Disponibilité du chemin de données (préversion)|Count|Average|Disponibilité du chemin de données de NAT Gateway|Aucune dimension|
|PacketCount|Oui|Paquets|Count|Total|Nombre total de paquets transmis dans une période de temps|Protocole, Direction|
|PacketDropCount|Oui|Paquets ignorés|Count|Total|Nombre de paquets ignorés|Aucune dimension|
|SNATConnectionCount|Oui|Nombre de connexions SNAT|Count|Total|Nombre total de connexions actives simultanées|Protocole, ConnectionState|
|TotalConnectionCount|Oui|Nombre total de connexions SNAT|Count|Total|Nombre de connexions SNAT actives|Protocol|


## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BytesReceivedRate|Oui|Octets reçus|Octets|Total|Nombre d’octets reçus par l’interface réseau|Aucune dimension|
|BytesSentRate|Oui|Octets envoyés|Octets|Total|Nombre d’octets envoyés par l’interface réseau|Aucune dimension|
|PacketsReceivedRate|Oui|Paquets reçus|Count|Total|Nombre de paquets reçus par l’interface réseau|Aucune dimension|
|PacketsSentRate|Oui|Paquets envoyés|Count|Total|Nombre de paquets envoyés par l’interface réseau|Aucune dimension|


## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AverageRoundtripMs|Oui|Avg. Durée d’aller-retour (ms) (classique)|Millisecondes|Average|Durée aller-retour réseau moyenne (ms) pour les sondes de surveillance de connectivité envoyées entre la source et la destination|Aucune dimension|
|ChecksFailedPercent|Oui|Pourcentage de vérifications ayant échoué|Pourcentage|Average|% de vérifications de supervision de connectivité ayant échoué|SourceAddress, SourceName, SourceResourceId, SourceType, Protocol, DestinationAddress, DestinationName, DestinationResourceId, DestinationType, DestinationPort, TestGroupName, TestConfigurationName, SourceIP, DestinationIP, SourceSubnet, DestinationSubnet|
|ProbesFailedPercent|Oui|% de sondes ayant échoué (classique)|Pourcentage|Average|% de sondes de surveillance de connectivité ayant échoué|Aucune dimension|
|RoundTripTimeMs|Oui|Durée d’aller-retour (ms)|Millisecondes|Average|Durée aller-retour en millisecondes pour les vérifications de supervision de la connectivité|SourceAddress, SourceName, SourceResourceId, SourceType, Protocol, DestinationAddress, DestinationName, DestinationResourceId, DestinationType, DestinationPort, TestGroupName, TestConfigurationName, SourceIP, DestinationIP, SourceSubnet, DestinationSubnet|
|TestResult|Oui|Résultat de test|Count|Average|Résultat du test du moniteur de connexion|SourceAddress, SourceName, SourceResourceId, SourceType, Protocole, DestinationAddress, DestinationName, DestinationResourceId, DestinationType, DestinationPort, TestGroupName, TestConfigurationName, TestResultCriterion, SourceIP, DestinationIP, SourceSubnet, DestinationSubnet|


## <a name="microsoftnetworkp2svpngateways"></a>microsoft.network/p2svpngateways

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|P2SBandwidth|Oui|Bande passante P2S de passerelle|BytesPerSecond|Average|Bande passante point à site d’une passerelle en octets par seconde|Instance|
|P2SConnectionCount|Oui|Nombre de connexions P2S|Count|Total|Nombre de connexions point à site d’une passerelle|Protocol, Instance|
|UserVpnRouteCount|No|Nombre d’itinéraires VPN utilisateur|Nombre|Total|Nombre d’itinéraires VPN utilisateur P2S appris par la passerelle|RouteType, Instance|


## <a name="microsoftnetworkprivatednszones"></a>Microsoft.Network/privateDnsZones

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|QueryVolume|Oui|Volume de requête|Count|Total|Nombre de requêtes servies pour une zone DNS privée|Aucune dimension|
|RecordSetCapacityUtilization|Non|Utilisation de la capacité du jeu d’enregistrements|Pourcentage|Maximale|Pourcentage de la capacité du jeu d’enregistrements utilisée par une zone DNS privée|Aucune dimension|
|RecordSetCount|Oui|Nombre de jeux d’enregistrements|Count|Maximale|Nombre de jeux d’enregistrements dans une zone DNS privée|Aucune dimension|
|VirtualNetworkLinkCapacityUtilization|Non|Utilisation de la capacité de liens de réseau virtuel|Pourcentage|Maximale|Pourcentage de la capacité de liens de réseau virtuel utilisée par une zone DNS privée|Aucune dimension|
|VirtualNetworkLinkCount|Oui|Nombre de liens de réseau virtuel|Count|Maximale|Nombre de réseaux virtuels liés à une zone DNS privée|Aucune dimension|
|VirtualNetworkWithRegistrationCapacityUtilization|Non|Utilisation de la capacité de liens d’inscription de réseau virtuel|Pourcentage|Maximale|Pourcentage de la capacité de liens de réseau virtuel avec l’inscription automatique utilisée par une zone DNS privée|Aucune dimension|
|VirtualNetworkWithRegistrationLinkCount|Oui|Nombre de liens d’inscription de réseau virtuel|Count|Maximale|Nombre de réseaux virtuels liés à une zone DNS privée avec l’inscription automatique activée|Aucune dimension|


## <a name="microsoftnetworkprivateendpoints"></a>Microsoft.Network/privateEndpoints

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|PEBytesIn|No|Bytes In|Count|Total|Nombre d’octets lus en sortie|Aucune dimension|
|PEBytesOut|No|Bytes Out|Count|Total|Nombre d’octets lus en sortie|Aucune dimension|


## <a name="microsoftnetworkprivatelinkservices"></a>Microsoft.Network/privateLinkServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|PLSBytesIn|Oui|Bytes In|Count|Total|Nombre d’octets lus en sortie|PrivateLinkServiceId|
|PLSBytesOut|Oui|Bytes Out|Count|Total|Nombre d’octets lus en sortie|PrivateLinkServiceId|
|PLSNatPortsUsage|Oui|Utilisation des ports NAT|Pourcentage|Average|Utilisation des ports NAT|PrivateLinkServiceId, PrivateLinkServiceIPAddress|


## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ByteCount|Oui|Nombre d’octets|Octets|Total|Nombre total d’octets transmis dans une période de temps|Port, Direction|
|BytesDroppedDDoS|Oui|Octets entrants supprimés DDoS|BytesPerSecond|Maximale|Octets entrants supprimés DDoS|Aucune dimension|
|BytesForwardedDDoS|Oui|Octets entrants transférés DDoS|BytesPerSecond|Maximale|Octets entrants transférés DDoS|Aucune dimension|
|BytesInDDoS|Oui|Octets entrants DDoS|BytesPerSecond|Maximale|Octets entrants DDoS|Aucune dimension|
|DDoSTriggerSYNPackets|Oui|Paquets SYN entrants pour déclencher l’atténuation DDoS|CountPerSecond|Maximale|Paquets SYN entrants pour déclencher l’atténuation DDoS|Aucune dimension|
|DDoSTriggerTCPPackets|Oui|Paquets TCP entrants pour déclencher l’atténuation DDoS|CountPerSecond|Maximale|Paquets TCP entrants pour déclencher l’atténuation DDoS|Aucune dimension|
|DDoSTriggerUDPPackets|Oui|Paquets UDP entrants pour déclencher l’atténuation DDoS|CountPerSecond|Maximale|Paquets UDP entrants pour déclencher l’atténuation DDoS|Aucune dimension|
|IfUnderDDoSAttack|Oui|Sous attaque DDoS ou non|Count|Maximale|Sous attaque DDoS ou non|Aucune dimension|
|PacketCount|Oui|Nombre de paquets|Count|Total|Nombre total de paquets transmis dans une période de temps|Port, Direction|
|PacketsDroppedDDoS|Oui|Paquets entrants ignorés DDoS|CountPerSecond|Maximale|Paquets entrants ignorés DDoS|Aucune dimension|
|PacketsForwardedDDoS|Oui|Paquets entrants transférés DDoS|CountPerSecond|Maximale|Paquets entrants transférés DDoS|Aucune dimension|
|PacketsInDDoS|Oui|Paquets entrants DDoS|CountPerSecond|Maximale|Paquets entrants DDoS|Aucune dimension|
|SynCount|Oui|Nombre de SYN|Count|Total|Nombre total de paquets SYN transmis dans une période de temps|Port, Direction|
|TCPBytesDroppedDDoS|Oui|Octets TCP entrants supprimés DDoS|BytesPerSecond|Maximale|Octets TCP entrants supprimés DDoS|Aucune dimension|
|TCPBytesForwardedDDoS|Oui|Octets TCP entrants transférés DDoS|BytesPerSecond|Maximale|Octets TCP entrants transférés DDoS|Aucune dimension|
|TCPBytesInDDoS|Oui|Octets TCP entrants DDoS|BytesPerSecond|Maximale|Octets TCP entrants DDoS|Aucune dimension|
|TCPPacketsDroppedDDoS|Oui|Paquets TCP entrants TCP ignorés DDoS|CountPerSecond|Maximale|Paquets TCP entrants TCP ignorés DDoS|Aucune dimension|
|TCPPacketsForwardedDDoS|Oui|Paquets TCP entrants transférés DDoS|CountPerSecond|Maximale|Paquets TCP entrants transférés DDoS|Aucune dimension|
|TCPPacketsInDDoS|Oui|Paquets TCP entrants DDoS|CountPerSecond|Maximale|Paquets TCP entrants DDoS|Aucune dimension|
|UDPBytesDroppedDDoS|Oui|Octets UDP entrants supprimés DDoS|BytesPerSecond|Maximale|Octets UDP entrants supprimés DDoS|Aucune dimension|
|UDPBytesForwardedDDoS|Oui|Octets UDP entrants transférés DDoS|BytesPerSecond|Maximale|Octets UDP entrants transférés DDoS|Aucune dimension|
|UDPBytesInDDoS|Oui|Octets UDP entrants DDoS|BytesPerSecond|Maximale|Octets UDP entrants DDoS|Aucune dimension|
|UDPPacketsDroppedDDoS|Oui|Paquets UDP entrants ignorés DDoS|CountPerSecond|Maximale|Paquets UDP entrants ignorés DDoS|Aucune dimension|
|UDPPacketsForwardedDDoS|Oui|Paquets UDP entrants transférés DDoS|CountPerSecond|Maximale|Paquets UDP entrants transférés DDoS|Aucune dimension|
|UDPPacketsInDDoS|Oui|Paquets UDP entrants DDoS|CountPerSecond|Maximale|Paquets UDP entrants DDoS|Aucune dimension|
|VipAvailability|Oui|Disponibilité du chemin d’accès aux données|Count|Average|Disponibilité d’adresse IP moyenne par durée|Port|


## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Oui|État du point de terminaison par point de terminaison|Count|Maximale|1 si l’état de sondage d’un point de terminaison est « activé », 0 dans le cas contraire.|EndpointName|
|QpsByEndpoint|Oui|Requêtes par point de terminaison renvoyé|Count|Total|Nombre de fois où un point de terminaison Traffic Manager a été renvoyé dans le laps de temps donné|EndpointName|


## <a name="microsoftnetworkvirtualhubs"></a>Microsoft.Network/virtualHubs

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BgpPeerStatus|No|État du pair BGP|Count|Maximale|1 = Connecté, 0 = Non connecté|routeserviceinstance, bgppeerip, bgppeertype|
|CountOfRoutesAdvertisedToPeer|No|Nombre d’itinéraires publiés pour le pair|Count|Maximale|Nombre total d’itinéraires publiés pour le pair|routeserviceinstance, bgppeerip, bgppeertype|
|CountOfRoutesLearnedFromPeer|No|Nombre d’itinéraires appris du pair|Count|Maximale|Nombre total d’itinéraires appris du pair|routeserviceinstance, bgppeerip, bgppeertype|


## <a name="microsoftnetworkvirtualnetworkgateways"></a>microsoft.network/virtualnetworkgateways

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AverageBandwidth|Oui|Bande passante S2S de passerelle|BytesPerSecond|Average|Bande passante site à site d’une passerelle en octets par seconde|Instance|
|BgpPeerStatus|No|État du pair BGP|Count|Average|État du pair BGP|BgpPeerAddress, Instance|
|BgpRoutesAdvertised|Yes|Itinéraires BGP publiés|Nombre|Total|Nombre d’itinéraires BGP publiés par tunnel|BgpPeerAddress, Instance|
|BgpRoutesLearned|Yes|Itinéraires BGP appris|Nombre|Total|Nombre d’itinéraires BGP appris par tunnel|BgpPeerAddress, Instance|
|ExpressRouteGatewayCountOfRoutesAdvertisedToPeer|Oui|Nombre de routes publiées pour le pair (préversion)|Count|Maximale|Nombre de routes publiées pour le pair par ExpressRouteGateway|roleInstance|
|ExpressRouteGatewayCountOfRoutesLearnedFromPeer|Oui|Nombre de routes apprises du pair (préversion)|Count|Maximale|Nombre de routes apprises du pair par ExpressRouteGateway|roleInstance|
|ExpressRouteGatewayCpuUtilization|Oui|Utilisation du processeur|Pourcentage|Average|Utilisation du processeur de la passerelle ExpressRoute|roleInstance|
|ExpressRouteGatewayFrequencyOfRoutesChanged|Non|Fréquence de changement des routes (préversion)|Count|Total|Fréquence de changement des routes dans la passerelle ExpressRoute|roleInstance|
|ExpressRouteGatewayNumberOfVmInVnet|Non|Nombre de machines virtuelles dans le réseau virtuel (préversion)|Count|Maximale|Nombre de machines virtuelles dans le réseau virtuel|roleInstance|
|ExpressRouteGatewayPacketsPerSecond|Non|Paquets par seconde|CountPerSecond|Average|Nombre de paquets de la passerelle ExpressRoute|roleInstance|
|MmsaCount|Yes|Nombre de tunnels MMSA|Nombre|Total|Nombre de MMSA|ConnectionName, RemoteIP, Instance|
|P2SBandwidth|Oui|Bande passante P2S de passerelle|BytesPerSecond|Average|Bande passante point à site d’une passerelle en octets par seconde|Instance|
|P2SConnectionCount|Oui|Nombre de connexions P2S|Count|Total|Nombre de connexions point à site d’une passerelle|Protocol, Instance|
|QmsaCount|Yes|Nombre de tunnels QMSA|Nombre|Total|Nombre de QMSA|ConnectionName, RemoteIP, Instance|
|TunnelAverageBandwidth|Oui|Bande passante de tunnel|BytesPerSecond|Average|Bande passante moyenne d’un tunnel en octets par seconde|ConnectionName, RemoteIP, Instance|
|TunnelEgressBytes|Oui|Octets de sortie de tunnel|Octets|Total|Octets sortants d’un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPacketDropCount|Yes|Nombre d’annulations de paquets de sortie par tunnel|Nombre|Total|Nombre de paquets sortants annulés par tunnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPacketDropTSMismatch|Oui|Rejet de paquets TS de sortie de tunnel pour incompatibilité|Count|Total|Nombre de rejets de paquets sortants du sélecteur de trafic pour incompatibilité dans un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPackets|Oui|Paquets de sortie de tunnel|Count|Total|Nombre de paquets sortants d’un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressBytes|Oui|Octets d’entrée de tunnel|Octets|Total|Octets entrants d’un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPacketDropCount|Yes|Nombre d’annulations de paquets d’entrée par tunnel|Nombre|Total|Nombre de paquets entrants annulés par tunnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPacketDropTSMismatch|Oui|Rejet de paquets TS d’entrée de tunnel pour incompatibilité|Count|Total|Nombre de rejets de paquets entrants du sélecteur de trafic pour incompatibilité dans un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPackets|Oui|Paquets en entrée de tunnel|Count|Total|Nombre de paquets entrants d’un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelNatAllocations|Non|Allocations NAT de tunnel|Count|Total|Nombre d’allocations pour une règle NAT sur un tunnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatedBytes|Non|Octets traduits de tunnel|Octets|Total|Nombre d’octets traduits sur un tunnel par une règle NAT|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatedPackets|Non|Paquets traduits de tunnel|Count|Total|Nombre de paquets traduits sur un tunnel par une règle NAT|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatFlowCount|Non|Flux NAT de tunnel|Count|Total|Nombre de flux NAT sur un tunnel par type de flux et règle NAT|NatRule, FlowType, ConnectionName, RemoteIP, Instance|
|TunnelNatPacketDrop|Non|Rejets de paquets NAT de tunnel|Count|Total|Nombre de paquets traduits sur un tunnel qui ont été rejetés, par type de rejet et règle NAT|NatRule, DropType, ConnectionName, RemoteIP, Instance|
|TunnelPeakPackets|Yes|Pic PPS dans le tunnel|Count|Maximale|Pic de paquets par seconde dans le tunnel|ConnectionName, RemoteIP, Instance|
|TunnelReverseNatedBytes|Non|Octets traduits en sens inverse de tunnel|Octets|Total|Nombre d’octets traduits en sens inverse par une règle NAT sur un tunnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelReverseNatedPackets|Non|Paquets traduits en sens inverse de tunnel|Count|Total|Nombre de paquets traduits en sens inverse par une règle NAT sur un tunnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelTotalFlowCount|Yes|Nombre total de flux d’un tunnel|Nombre|Total|Nombre total de flux sur un tunnel|ConnectionName, RemoteIP, Instance|
|UserVpnRouteCount|No|Nombre d’itinéraires VPN utilisateur|Nombre|Total|Nombre d’itinéraires VPN utilisateur P2S appris par la passerelle|RouteType, Instance|
|VnetAddressPrefixCount|Yes|Nombre de préfixes d’adresse de réseau virtuel|Nombre|Total|Nombre de préfixes d’adresse de réseau virtuel derrière la passerelle|Instance|


## <a name="microsoftnetworkvirtualnetworks"></a>Microsoft.Network/virtualNetworks

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BytesDroppedDDoS|Oui|Octets entrants supprimés DDoS|BytesPerSecond|Maximale|Octets entrants supprimés DDoS|ProtectedIPAddress|
|BytesForwardedDDoS|Oui|Octets entrants transférés DDoS|BytesPerSecond|Maximale|Octets entrants transférés DDoS|ProtectedIPAddress|
|BytesInDDoS|Oui|Octets entrants DDoS|BytesPerSecond|Maximale|Octets entrants DDoS|ProtectedIPAddress|
|DDoSTriggerSYNPackets|Oui|Paquets SYN entrants pour déclencher l’atténuation DDoS|CountPerSecond|Maximale|Paquets SYN entrants pour déclencher l’atténuation DDoS|ProtectedIPAddress|
|DDoSTriggerTCPPackets|Oui|Paquets TCP entrants pour déclencher l’atténuation DDoS|CountPerSecond|Maximale|Paquets TCP entrants pour déclencher l’atténuation DDoS|ProtectedIPAddress|
|DDoSTriggerUDPPackets|Oui|Paquets UDP entrants pour déclencher l’atténuation DDoS|CountPerSecond|Maximale|Paquets UDP entrants pour déclencher l’atténuation DDoS|ProtectedIPAddress|
|IfUnderDDoSAttack|Oui|Sous attaque DDoS ou non|Count|Maximale|Sous attaque DDoS ou non|ProtectedIPAddress|
|PacketsDroppedDDoS|Oui|Paquets entrants ignorés DDoS|CountPerSecond|Maximale|Paquets entrants ignorés DDoS|ProtectedIPAddress|
|PacketsForwardedDDoS|Oui|Paquets entrants transférés DDoS|CountPerSecond|Maximale|Paquets entrants transférés DDoS|ProtectedIPAddress|
|PacketsInDDoS|Oui|Paquets entrants DDoS|CountPerSecond|Maximale|Paquets entrants DDoS|ProtectedIPAddress|
|PingMeshAverageRoundtripMs|Oui|Durée aller-retour pour les commandes ping à destination d’une machine virtuelle|Millisecondes|Average|Durée aller-retour pour les commandes ping envoyées à une machine virtuelle de destination|SourceCustomerAddress, DestinationCustomerAddress|
|PingMeshProbesFailedPercent|Oui|Commandes ping à destination d’une machine virtuelle qui ont échoué|Pourcentage|Average|Pourcentage du nombre d’échecs de commandes ping par rapport au total de commandes ping envoyées à une machine virtuelle de destination|SourceCustomerAddress, DestinationCustomerAddress|
|TCPBytesDroppedDDoS|Oui|Octets TCP entrants supprimés DDoS|BytesPerSecond|Maximale|Octets TCP entrants supprimés DDoS|ProtectedIPAddress|
|TCPBytesForwardedDDoS|Oui|Octets TCP entrants transférés DDoS|BytesPerSecond|Maximale|Octets TCP entrants transférés DDoS|ProtectedIPAddress|
|TCPBytesInDDoS|Oui|Octets TCP entrants DDoS|BytesPerSecond|Maximale|Octets TCP entrants DDoS|ProtectedIPAddress|
|TCPPacketsDroppedDDoS|Oui|Paquets TCP entrants TCP ignorés DDoS|CountPerSecond|Maximale|Paquets TCP entrants TCP ignorés DDoS|ProtectedIPAddress|
|TCPPacketsForwardedDDoS|Oui|Paquets TCP entrants transférés DDoS|CountPerSecond|Maximale|Paquets TCP entrants transférés DDoS|ProtectedIPAddress|
|TCPPacketsInDDoS|Oui|Paquets TCP entrants DDoS|CountPerSecond|Maximale|Paquets TCP entrants DDoS|ProtectedIPAddress|
|UDPBytesDroppedDDoS|Oui|Octets UDP entrants supprimés DDoS|BytesPerSecond|Maximale|Octets UDP entrants supprimés DDoS|ProtectedIPAddress|
|UDPBytesForwardedDDoS|Oui|Octets UDP entrants transférés DDoS|BytesPerSecond|Maximale|Octets UDP entrants transférés DDoS|ProtectedIPAddress|
|UDPBytesInDDoS|Oui|Octets UDP entrants DDoS|BytesPerSecond|Maximale|Octets UDP entrants DDoS|ProtectedIPAddress|
|UDPPacketsDroppedDDoS|Oui|Paquets UDP entrants ignorés DDoS|CountPerSecond|Maximale|Paquets UDP entrants ignorés DDoS|ProtectedIPAddress|
|UDPPacketsForwardedDDoS|Oui|Paquets UDP entrants transférés DDoS|CountPerSecond|Maximale|Paquets UDP entrants transférés DDoS|ProtectedIPAddress|
|UDPPacketsInDDoS|Oui|Paquets UDP entrants DDoS|CountPerSecond|Maximale|Paquets UDP entrants DDoS|ProtectedIPAddress|


## <a name="microsoftnetworkvirtualrouters"></a>Microsoft.Network/virtualRouters

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|PeeringAvailability|Oui|Disponibilité du protocole BGP|Pourcentage|Average|Disponibilité BGP entre VirtualRouter et les pairs distants|Homologue|


## <a name="microsoftnetworkvpngateways"></a>microsoft.network/vpngateways

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AverageBandwidth|Oui|Bande passante S2S de passerelle|BytesPerSecond|Average|Bande passante site à site d’une passerelle en octets par seconde|Instance|
|BgpPeerStatus|No|État du pair BGP|Count|Average|État du pair BGP|BgpPeerAddress, Instance|
|BgpRoutesAdvertised|Yes|Itinéraires BGP publiés|Nombre|Total|Nombre d’itinéraires BGP publiés par tunnel|BgpPeerAddress, Instance|
|BgpRoutesLearned|Yes|Itinéraires BGP appris|Nombre|Total|Nombre d’itinéraires BGP appris par tunnel|BgpPeerAddress, Instance|
|MmsaCount|Yes|Nombre de tunnels MMSA|Nombre|Total|Nombre de MMSA|ConnectionName, RemoteIP, Instance|
|QmsaCount|Yes|Nombre de tunnels QMSA|Nombre|Total|Nombre de QMSA|ConnectionName, RemoteIP, Instance|
|TunnelAverageBandwidth|Oui|Bande passante de tunnel|BytesPerSecond|Average|Bande passante moyenne d’un tunnel en octets par seconde|ConnectionName, RemoteIP, Instance|
|TunnelEgressBytes|Oui|Octets de sortie de tunnel|Octets|Total|Octets sortants d’un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPacketDropCount|Yes|Nombre d’annulations de paquets de sortie par tunnel|Nombre|Total|Nombre de paquets sortants annulés par tunnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPacketDropTSMismatch|Oui|Rejet de paquets TS de sortie de tunnel pour incompatibilité|Count|Total|Nombre de rejets de paquets sortants du sélecteur de trafic pour incompatibilité dans un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelEgressPackets|Oui|Paquets de sortie de tunnel|Count|Total|Nombre de paquets sortants d’un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressBytes|Oui|Octets d’entrée de tunnel|Octets|Total|Octets entrants d’un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPacketDropCount|Yes|Nombre d’annulations de paquets d’entrée par tunnel|Nombre|Total|Nombre de paquets entrants annulés par tunnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPacketDropTSMismatch|Oui|Rejet de paquets TS d’entrée de tunnel pour incompatibilité|Count|Total|Nombre de rejets de paquets entrants du sélecteur de trafic pour incompatibilité dans un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelIngressPackets|Oui|Paquets en entrée de tunnel|Count|Total|Nombre de paquets entrants d’un tunnel|ConnectionName, RemoteIP, Instance|
|TunnelNatAllocations|Non|Allocations NAT de tunnel|Count|Total|Nombre d’allocations pour une règle NAT sur un tunnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatedBytes|Non|Octets traduits de tunnel|Octets|Total|Nombre d’octets traduits sur un tunnel par une règle NAT|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatedPackets|Non|Paquets traduits de tunnel|Count|Total|Nombre de paquets traduits sur un tunnel par une règle NAT|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelNatFlowCount|Non|Flux NAT de tunnel|Count|Total|Nombre de flux NAT sur un tunnel par type de flux et règle NAT|NatRule, FlowType, ConnectionName, RemoteIP, Instance|
|TunnelNatPacketDrop|Non|Rejets de paquets NAT de tunnel|Count|Total|Nombre de paquets traduits sur un tunnel qui ont été rejetés, par type de rejet et règle NAT|NatRule, DropType, ConnectionName, RemoteIP, Instance|
|TunnelPeakPackets|Yes|Pic PPS dans le tunnel|Count|Maximale|Pic de paquets par seconde dans le tunnel|ConnectionName, RemoteIP, Instance|
|TunnelReverseNatedBytes|Non|Octets traduits en sens inverse de tunnel|Octets|Total|Nombre d’octets traduits en sens inverse par une règle NAT sur un tunnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelReverseNatedPackets|Non|Paquets traduits en sens inverse de tunnel|Count|Total|Nombre de paquets traduits en sens inverse par une règle NAT sur un tunnel|NatRule, ConnectionName, RemoteIP, Instance|
|TunnelTotalFlowCount|Yes|Nombre total de flux d’un tunnel|Nombre|Total|Nombre total de flux sur un tunnel|ConnectionName, RemoteIP, Instance|
|VnetAddressPrefixCount|Yes|Nombre de préfixes d’adresse de réseau virtuel|Nombre|Total|Nombre de préfixes d’adresse de réseau virtuel derrière la passerelle|Instance|


## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|incoming|Oui|Messages entrants|Count|Total|Total des appels d’API d’envoi réussis. |Aucune dimension|
|incoming.all.failedrequests|Oui|Toutes les requêtes entrantes ayant échoué|Count|Total|Nombre total des demandes entrantes ayant échoué pour un hub de notification|Aucune dimension|
|incoming.all.requests|Oui|Toutes les demandes entrantes|Count|Total|Nombre total des demandes entrantes pour un hub de notification|Aucune dimension|
|incoming.scheduled|Oui|Notifications Push planifiées envoyées|Count|Total|Notifications Push planifiées annulées|Aucune dimension|
|incoming.scheduled.cancel|Oui|Notifications Push planifiées annulées|Count|Total|Notifications Push planifiées annulées|Aucune dimension|
|installation.all|Oui|Opérations de gestion de l’installation|Count|Total|Opérations de gestion de l’installation|Aucune dimension|
|installation.delete|Oui|Opérations de suppression d’installation|Count|Total|Opérations de suppression d’installation|Aucune dimension|
|installation.get|Oui|Opérations d’obtention d’installation|Count|Total|Opérations d’obtention d’installation|Aucune dimension|
|installation.patch|Oui|Opérations d’installation de correctif|Count|Total|Opérations d’installation de correctif|Aucune dimension|
|installation.upsert|Oui|Opérations de création ou de mise à jour d’installation|Count|Total|Opérations de création ou de mise à jour d’installation|Aucune dimension|
|notificationhub.pushes|Oui|Toutes les notifications sortantes|Count|Total|Toutes les notifications sortantes du hub de notification|Aucune dimension|
|outgoing.allpns.badorexpiredchannel|Oui|Erreurs de canal incorrect ou arrivé à expiration|Count|Total|Nombre d’opérations Push en échec, car le canal/jeton/registrationId indiqué dans l’inscription est arrivé à expiration ou non valide.|Aucune dimension|
|outgoing.allpns.channelerror|Oui|Erreurs de canal|Count|Total|Nombre d’opérations Push en échec, car le canal n’était pas valide ni associé à l’application appropriée, limité ou arrivé à expiration.|Aucune dimension|
|outgoing.allpns.invalidpayload|Oui|Erreurs de charge utile|Count|Total|Nombre d’opérations Push en échec, car le PNS a retourné une erreur de charge utile incorrecte.|Aucune dimension|
|outgoing.allpns.pnserror|Oui|Erreurs système de notification externe|Count|Total|Nombre d’opérations Push en échec en raison d’un problème de communication avec le PNS (à l’exclusion des problèmes d’authentification).|Aucune dimension|
|outgoing.allpns.success|Oui|Notifications réussies|Count|Total|Total des notifications réussies.|Aucune dimension|
|outgoing.apns.badchannel|Oui|Erreur de canal APNS incorrect|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car le jeton n'est pas valide (code d'état APNS : 8).|Aucune dimension|
|outgoing.apns.expiredchannel|Oui|Erreur de canal APNS arrivé à expiration|Count|Total|Nombre de jetons qui ont été invalidés par le canal de commentaires APNS.|Aucune dimension|
|outgoing.apns.invalidcredentials|Oui|Erreurs d’autorisation APNS|Count|Total|Nombre d’opérations Push en échec, car le PNS n’a pas accepté les informations d’identification fournies ou celles-ci sont bloquées.|Aucune dimension|
|outgoing.apns.invalidnotificationsize|Oui|Erreur de taille de notification APNS non valide|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car la charge utile était trop importante (code d'état APNS : 7).|Aucune dimension|
|outgoing.apns.pnserror|Oui|Erreurs APNS|Count|Total|Nombre d’opérations Push en échec en raison d’erreurs de communication avec APNS.|Aucune dimension|
|outgoing.apns.success|Oui|Notifications APNS réussies|Count|Total|Total des notifications réussies.|Aucune dimension|
|outgoing.gcm.authenticationerror|Oui|Erreurs d’authentification GCM|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car le PNS n'a pas accepté les informations d'identification fournies, celles-ci sont bloquées ou l'élément SenderId n'est pas correctement configuré dans l'application (résultat GCM : MismatchedSenderId).|Aucune dimension|
|outgoing.gcm.badchannel|Oui|Erreur de canal GCM incorrect|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car l'élément registrationId contenu dans l'inscription n'a pas été reconnu (résultat GCM : inscription non valide).|Aucune dimension|
|outgoing.gcm.expiredchannel|Oui|Erreur de canal GCM arrivé à expiration|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car l'élément registrationId contenu dans l'inscription n'a pas été reconnu (résultat GCM : NotRegistered).|Aucune dimension|
|outgoing.gcm.invalidcredentials|Oui|Erreurs d’autorisation GCM (informations d’identification non valides)|Count|Total|Nombre d’opérations Push en échec, car le PNS n’a pas accepté les informations d’identification fournies ou celles-ci sont bloquées.|Aucune dimension|
|outgoing.gcm.invalidnotificationformat|Oui|Format de notification GCM non valide|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car la charge utile n'était pas correctement mise en forme (résultat GCM : InvalidDataKey ou InvalidTtl).|Aucune dimension|
|outgoing.gcm.invalidnotificationsize|Oui|Erreur de taille de notification GCM non valide|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car la charge utile était trop importante (résultat GCM : MessageTooBig).|Aucune dimension|
|outgoing.gcm.pnserror|Oui|Erreurs GCM|Count|Total|Nombre d’opérations Push en échec en raison d’erreurs de communication avec GCM.|Aucune dimension|
|outgoing.gcm.success|Oui|Notifications GCM réussies|Count|Total|Total des notifications réussies.|Aucune dimension|
|outgoing.gcm.throttled|Oui|Notifications GCM limitées|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car GCM a limité cette application (code d'état GCM : 501-599 ou résultat : non disponible).|Aucune dimension|
|outgoing.gcm.wrongchannel|Oui|Erreur de canal GCM incorrect|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car l'élément registrationId contenu dans l'inscription n'est pas associé à l'application actuelle (résultat GCM : InvalidPackageName).|Aucune dimension|
|outgoing.mpns.authenticationerror|Oui|Erreurs d’authentification MPNS|Count|Total|Nombre d’opérations Push en échec, car le PNS n’a pas accepté les informations d’identification fournies ou celles-ci sont bloquées.|Aucune dimension|
|outgoing.mpns.badchannel|Oui|Erreur de canal incorrect MPNS|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car l'élément ChannelURI contenu dans l'inscription n'a pas été reconnu (état MPNS : 404 - Introuvable).|Aucune dimension|
|outgoing.mpns.channeldisconnected|Oui|Canal MPNS déconnecté|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car l'élément ChannelURI contenu dans l'inscription était déconnecté (état MPNS : 412 introuvable).|Aucune dimension|
|outgoing.mpns.dropped|Oui|Notifications MPNS supprimées|Count|Total|Nombre d'opérations d'envoi (push) qui ont été abandonnées par MPNS (en-tête de réponse MPNS : X-NotificationStatus: QueueFull ou Suppressed).|Aucune dimension|
|outgoing.mpns.invalidcredentials|Oui|Informations d’identification MPNS non valides|Count|Total|Nombre d’opérations Push en échec, car le PNS n’a pas accepté les informations d’identification fournies ou celles-ci sont bloquées.|Aucune dimension|
|outgoing.mpns.invalidnotificationformat|Oui|Format de notification MPNS non valide|Count|Total|Nombre d’opérations Push en échec, car la charge utile de la notification était trop importante.|Aucune dimension|
|outgoing.mpns.pnserror|Oui|Erreurs MPNS|Count|Total|Nombre d’opérations Push en échec en raison d’erreurs de communication avec MPNS.|Aucune dimension|
|outgoing.mpns.success|Oui|Notifications MPNS réussies|Count|Total|Total des notifications réussies.|Aucune dimension|
|outgoing.mpns.throttled|Oui|Notifications MPNS limitées|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car MPNS limite cette application (WNS MPNS : 406 Non acceptable).|Aucune dimension|
|outgoing.wns.authenticationerror|Oui|Erreurs d’authentification WNS|Count|Total|La notification n’a pas été remise en raison d’erreurs de communication avec Windows Live, d’informations d’identification non valides ou d’un jeton incorrect.|Aucune dimension|
|outgoing.wns.badchannel|Oui|Erreur de canal WNS incorrect|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car l'élément ChannelURI indiqué dans l'inscription n'a pas été reconnu (état WNS : 404 - Introuvable).|Aucune dimension|
|outgoing.wns.channeldisconnected|Oui|Canal WNS déconnecté|Count|Total|La notification a été supprimée car l'élément ChannelURI contenu dans l'inscription est limité (en-tête de réponse WNS : X-WNS-DeviceConnectionStatus: déconnecté).|Aucune dimension|
|outgoing.wns.channelthrottled|Oui|Canal WNS limité|Count|Total|La notification a été supprimée car l'élément ChannelURI contenu dans l'inscription est limité (en-tête de réponse WNS : X-WNS-NotificationStatus:channelThrottled).|Aucune dimension|
|outgoing.wns.dropped|Oui|Notifications WNS supprimées|Count|Total|La notification a été supprimée, car l’élément ChannelURI indiqué dans l’inscription est limité (X-WNS-NotificationStatus : supprimé, mais pas X-WNS-DeviceConnectionStatus : déconnecté).|Aucune dimension|
|outgoing.wns.expiredchannel|Oui|Erreur de canal WNS arrivé à expiration|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car l'élément ChannelURI est arrivé à expiration (état WNS : 410 Supprimé).|Aucune dimension|
|outgoing.wns.invalidcredentials|Oui|Erreurs d’autorisation WNS (informations d’identification non valides)|Count|Total|Nombre d’opérations Push en échec, car le PNS n’a pas accepté les informations d’identification fournies ou celles-ci sont bloquées. (Windows Live ne reconnaît pas les informations d’identification).|Aucune dimension|
|outgoing.wns.invalidnotificationformat|Oui|Format de notification WNS non valide|Count|Total|Le format de la notification n'est pas valide (état WNS : 400). Notez que WNS ne rejette pas toutes les charges utiles non valides.|Aucune dimension|
|outgoing.wns.invalidnotificationsize|Oui|Erreur de taille de notification WNS non valide|Count|Total|La charge utile de notification est trop grande (état WNS : 413).|Aucune dimension|
|outgoing.wns.invalidtoken|Oui|Erreurs d’autorisation WNS (jeton non valide)|Count|Total|Le jeton fourni à WNS n'est pas valide (état WNS : 401 - Non autorisé).|Aucune dimension|
|outgoing.wns.pnserror|Oui|Erreurs WNS|Count|Total|La notification n’a pas été remise en raison d’erreurs de communication avec WNS.|Aucune dimension|
|outgoing.wns.success|Oui|Notifications WNS réussies|Count|Total|Total des notifications réussies.|Aucune dimension|
|outgoing.wns.throttled|Oui|Notifications WNS limitées|Count|Total|Nombre d'opérations d'envoi (push) qui ont échoué car WNS limite cette application (état WNS : 406 Non acceptable).|Aucune dimension|
|outgoing.wns.tokenproviderunreachable|Oui|Erreurs d’autorisation WNS (inaccessible)|Count|Total|Windows Live n’est pas accessible.|Aucune dimension|
|outgoing.wns.wrongtoken|Oui|Erreurs d’autorisation WNS (jeton incorrect)|Count|Total|Le jeton fourni à WNS est valide mais il concerne une autre application (état WNS : 403 - Interdit). Cela peut se produire si l’élément ChannelURI indiqué dans l’inscription est associé à une autre application. Vérifiez que l’application cliente est associée à l’application dont les informations d’identification figurent dans le hub de notification.|Aucune dimension|
|registration.all|Oui|Opérations d’inscription|Count|Total|Total des opérations d’inscription réussies (requêtes de mises à jour de créations et suppressions). |Aucune dimension|
|registration.create|Oui|Opérations de création d’inscription|Count|Total|Total des créations d’inscription réussies.|Aucune dimension|
|registration.delete|Oui|Opérations de suppression d’inscription|Count|Total|Total des suppressions d’inscription réussies.|Aucune dimension|
|registration.get|Oui|Opérations de lecture d’inscription|Count|Total|Total des requêtes d’inscription réussies.|Aucune dimension|
|registration.update|Oui|Opérations de mise à jour d’inscription|Count|Total|Total des mises à jour d’inscription réussies.|Aucune dimension|
|scheduled.pending|Oui|Notifications planifiées en attente|Count|Total|Notifications planifiées en attente|Aucune dimension|


## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Average_% Available Memory|Oui|% Available Memory|Count|Average|Average_% Available Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Swap Space|Oui|% Available Swap Space|Count|Average|Average_% Available Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Octets validés en cours d’utilisation Average_%|Oui|% d’octets validés utilisés|Count|Average|Octets validés en cours d’utilisation Average_%|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% DPC Time|Oui|% DPC Time|Count|Average|Average_% DPC Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Inodes libres Average_%|Oui|% Free Inodes|Count|Average|Inodes libres Average_%|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|Oui|% Free Space|Count|Average|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Idle Time|Oui|% Idle Time|Count|Average|Average_% Idle Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Interrupt Time|Oui|% Interrupt Time|Count|Average|Average_% Interrupt Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% IO Wait Time|Oui|% IO Wait Time|Count|Average|Average_% IO Wait Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Nice Time|Oui|% Nice Time|Count|Average|Average_% Nice Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Privileged Time|Oui|% Privileged Time|Count|Average|Average_% Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Temps processeur Average_%|Oui|% temps processeur|Count|Average|Temps processeur Average_%|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Inodes utilisés Average_%|Oui|% Used Inodes|Count|Average|Inodes utilisés Average_%|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Memory|Oui|% Used Memory|Count|Average|Average_% Used Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Inodes utilisés Average_%|Oui|% Used Space|Count|Average|Inodes utilisés Average_%|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Swap Space|Oui|% Used Swap Space|Count|Average|Average_% Used Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% User Time|Oui|% User Time|Count|Average|Average_% User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes|Oui|Nombre d’octets disponibles|Count|Average|Average_Available MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Memory|Oui|Available MBytes Memory|Count|Average|Average_Available MBytes Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Swap|Oui|Available MBytes Swap|Count|Average|Average_Available MBytes Swap|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Disk sec/Read|Oui|Avg. Disk sec/Read|Count|Average|Average_Avg. Disk sec/Read|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Disk sec/Transfer|Oui|Avg. Disk sec/Transfer|Count|Average|Average_Avg. Disk sec/Transfer|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. Disk sec/Write|Oui|Avg. Disk sec/Write|Count|Average|Average_Avg. Disk sec/Write|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Received/sec|Oui|Octets reçus/s|Count|Average|Average_Bytes Received/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Sent/sec|Oui|Octets envoyés/s|Count|Average|Average_Bytes Sent/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Total/sec|Oui|Nombre total d’octets/s|Count|Average|Average_Bytes Total/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Longueur de file d’attente du disque Average_Current|Oui|Longueur actuelle de la file d'attente du disque|Count|Average|Longueur de file d’attente du disque Average_Current|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Octets de lecture/s Average_Disk|Oui|Nb d’octets de lecture de disque/s|Count|Average|Octets de lecture/s Average_Disk|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Oui|Nb d’opérations de lectures de disque/s|Count|Average|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Oui|Disk Transfers/sec|Count|Average|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Octets d’écriture/s Average_Disk|Oui|Nb d’octets d’écriture de disque/s|Count|Average|Octets d’écriture/s Average_Disk|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec|Oui|Nb d’opération d’écriture de disque/s|Count|Average|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Mégaoctets Average_Free|Oui|Free Megabytes|Count|Average|Mégaoctets Average_Free|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Physical Memory|Oui|Free Physical Memory|Count|Average|Average_Free Physical Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Space in Paging Files|Oui|Free Space in Paging Files|Count|Average|Average_Free Space in Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Virtual Memory|Oui|Free Virtual Memory|Count|Average|Average_Free Virtual Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Logical Disk Bytes/sec|Oui|Logical Disk Bytes/sec|Count|Average|Average_Logical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Reads/sec|Oui|Page Reads/sec|Count|Average|Average_Page Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Writes/sec|Oui|Page Writes/sec|Count|Average|Average_Page Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pages/sec|Oui|Pages/sec|Count|Average|Average_Pages/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct Privileged Time|Oui|Pct Privileged Time|Count|Average|Average_Pct Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct User Time|Oui|Pct User Time|Count|Average|Average_Pct User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Physical Disk Bytes/sec|Oui|Physical Disk Bytes/sec|Count|Average|Average_Physical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processes|Oui|Processus|Count|Average|Average_Processes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Longueur de la file d’attente Average_Processor|Oui|Longueur de la file d’attente du processeur|Count|Average|Longueur de la file d’attente Average_Processor|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Size Stored In Paging Files|Oui|Size Stored In Paging Files|Count|Average|Average_Size Stored In Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes|Oui|Total Bytes|Count|Average|Average_Total Bytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Received|Oui|Total Bytes Received|Count|Average|Average_Total Bytes Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Transmitted|Oui|Total Bytes Transmitted|Count|Average|Average_Total Bytes Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Collisions|Oui|Total Collisions|Count|Average|Average_Total Collisions|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Received|Oui|Total Packets Received|Count|Average|Average_Total Bytes Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Transmitted|Oui|Total Packets Transmitted|Count|Average|Average_Total Packets Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Rx Errors|Oui|Total Rx Errors|Count|Average|Average_Total Rx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Rx Errors|Oui|Total Tx Errors|Count|Average|Average_Total Rx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Uptime|Oui|Uptime|Count|Average|Average_Uptime|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used MBytes Swap Space|Oui|Used MBytes Swap Space|Count|Average|Average_Used MBytes Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory MBytes|Oui|Used Memory kBytes|Count|Average|Average_Used Memory MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory MBytes|Oui|Used Memory MBytes|Count|Average|Average_Used Memory MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Users|Oui|Utilisateurs|Count|Average|Average_Users|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Virtual Shared Memory|Oui|Virtual Shared Memory|Count|Average|Average_Virtual Shared Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Événement|Oui|Événement|Count|Average|Événement|Source, EventLog, Computer, EventCategory, EventLevel, EventLevelName, EventID|
|Heartbeat|Oui|Heartbeat|Count|Total|Heartbeat|Computer, OSType, Version, SourceComputerId|
|Update|Oui|Update|Count|Average|Update|Computer, Product, Classification, UpdateState, Optional, Approved|


## <a name="microsoftpeeringpeerings"></a>Microsoft.Peering/peerings

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|EgressTrafficRate|Oui|Débit du trafic de sortie|BitsPerSecond|Average|Débit du trafic de sortie en bits par seconde|ConnectionId, SessionIp, TrafficClass|
|IngressTrafficRate|Oui|Débit du trafic d’entrée|BitsPerSecond|Average|Débit du trafic d’entrée en bits par seconde|ConnectionId, SessionIp, TrafficClass|
|SessionAvailability|Oui|Disponibilité de la session|Count|Average|Disponibilité de la session d’appairage|ConnectionId, SessionIp|


## <a name="microsoftpeeringpeeringservices"></a>Microsoft.Peering/peeringServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|PrefixLatency|Oui|Latence du préfixe|Millisecondes|Average|Latence du préfixe médiane|PrefixName|


## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|CleanerCurrentPrice|Oui|Mémoire : prix actuel du nettoyage|Count|Average|Prix actuel de la mémoire, $/octet/temps, normalisé à 1000.|Aucune dimension|
|CleanerMemoryNonshrinkable|Oui|Mémoire : mémoire de nettoyage non réductible|Octets|Average|Quantité de mémoire, en octets, qui ne doit pas être vidée par le nettoyage en arrière-plan.|Aucune dimension|
|CleanerMemoryShrinkable|Oui|Mémoire : mémoire de nettoyage réductible|Octets|Average|Quantité de mémoire, en octets, qui doit être vidée par le nettoyage en arrière-plan.|Aucune dimension|
|CommandPoolBusyThreads|Oui|Threads : threads occupés du pool de commandes|Count|Average|Nombre de threads occupés dans le pool de threads de commandes.|Aucune dimension|
|CommandPoolIdleThreads|Oui|Threads : threads inactifs du pool de commandes|Count|Average|Nombre de threads inactifs dans le pool de threads de commandes.|Aucune dimension|
|CommandPoolJobQueueLength|Oui|Longueur de la file d’attente des travaux du pool de commandes|Count|Average|Nombre de travaux contenus dans la file d’attente du pool de threads de commandes.|Aucune dimension|
|cpu_metric|Yes|Processeur (Gen2)|Pourcentage|Average|Utilisation de l’UC. Prise en charge uniquement pour les ressources Power BI Embedded Gen2.|Aucune dimension|
|cpu_workload_metric|Yes|UC par charge de travail (Gen2)|Pourcentage|Average|Utilisation de l’UC par charge de travail. Prise en charge uniquement pour les ressources Power BI Embedded Gen2.|Charge de travail|
|CurrentConnections|Oui|Connexion : Connexions en cours|Count|Average|Nombre actuel de connexions client établies.|Aucune dimension|
|CurrentUserSessions|Oui|Sessions utilisateur actuelles|Count|Average|Nombre actuel de sessions utilisateur établies.|Aucune dimension|
|LongParsingBusyThreads|Oui|Threads : threads occupés d'analyse longue|Count|Average|Nombre de threads occupés dans le pool de threads d’analyse longue.|Aucune dimension|
|LongParsingIdleThreads|Oui|Threads : threads inactifs d'analyse longue|Count|Average|Nombre de threads inactifs dans le pool de threads d’analyse longue.|Aucune dimension|
|LongParsingJobQueueLength|Oui|Threads : longueur de la file d'attente des travaux d'analyse longue|Count|Average|Nombre de travaux contenus dans la file d’attente du pool de threads d’analyse longue.|Aucune dimension|
|memory_metric|Oui|Mémoire (Gen1)|Octets|Average|Mémoire. Plage 0-3 Go pour A1, 0-5 Go pour A2, 0-10 Go pour A3, 0-25 Go pour A4, 0-50 Go pour A5 et 0-100 GB for A6. Prise en charge uniquement pour les ressources Power BI Embedded de génération 1.|Aucune dimension|
|memory_thrashing_metric|Oui|Écroulement de la mémoire (jeux de données) (Gen1)|Pourcentage|Average|Vidage de mémoire moyenne. Prise en charge uniquement pour les ressources Power BI Embedded de génération 1.|Aucune dimension|
|MemoryLimitHard|Oui|Mémoire : limite inconditionnelle de la mémoire|Octets|Average|Limite de mémoire physique, du fichier de configuration.|Aucune dimension|
|MemoryLimitHigh|Oui|Mémoire : limite haute de la mémoire|Octets|Average|Limite de mémoire élevée, du fichier de configuration.|Aucune dimension|
|MemoryLimitLow|Oui|Mémoire : limite basse de la mémoire|Octets|Average|Limite de mémoire basse, du fichier de configuration.|Aucune dimension|
|MemoryLimitVertiPaq|Oui|Mémoire : limite de la mémoire VertiPaq|Octets|Average|Limite en mémoire, du fichier de configuration.|Aucune dimension|
|MemoryUsage|Oui|Mémoire : Utilisation de la mémoire|Octets|Average|Utilisation de la mémoire du processus serveur telle qu’utilisée dans le calcul du coût de la mémoire de nettoyage. Équivaut au compteur Process\PrivateBytes, plus la taille des données mappées en mémoire, en ignorant la mémoire mappée ou allouée par le moteur d’analyse de mémoire xVelocity (VertiPaq) dépassant la limite de mémoire du moteur xVelocity.|Aucune dimension|
|overload_metric|Yes|Surcharge (Gen2)|Count|Average|Surcharge de ressource, 1 si la ressource est surchargée ; sinon, 0. Prise en charge uniquement pour les ressources Power BI Embedded Gen2.|Aucune dimension|
|ProcessingPoolBusyIOJobThreads|Oui|Threads : threads des travaux d'E/S occupés du pool de traitement|Count|Average|Nombre de threads pour les travaux d’E/S en cours d’exécution dans le pool de threads de traitement.|Aucune dimension|
|ProcessingPoolBusyNonIOThreads|Oui|Threads : threads autres que les threads d'E/S occupés du pool de traitement|Count|Average|Nombre de threads pour les travaux autres que d’E/S en cours d’exécution dans le pool de threads de traitement.|Aucune dimension|
|ProcessingPoolIdleIOJobThreads|Oui|Threads : threads des travaux d'E/S inactifs du pool de traitement|Count|Average|Nombre de threads inactifs pour les travaux d’E/S le pool de threads de traitement.|Aucune dimension|
|ProcessingPoolIdleNonIOThreads|Oui|Threads : threads autres que les threads d'E/S inactifs du pool de traitement|Count|Average|Nombre de threads inactifs le pool de threads de traitement dédiés aux travaux autres qu’E/S.|Aucune dimension|
|ProcessingPoolIOJobQueueLength|Oui|Threads : longueur de la file des travaux d'E/S du pool de traitement|Count|Average|Nombre de travaux d’E/S contenus dans la file d’attente du pool de threads de traitement.|Aucune dimension|
|ProcessingPoolJobQueueLength|Oui|Longueur de la file d’attente des travaux du pool de traitement|Count|Average|Nombre de travaux autres que d’E/S contenus dans la file d’attente du pool de threads de traitement.|Aucune dimension|
|qpu_high_utilization_metric|Oui|Utilisation élevée de l’UC (Gen1)|Count|Total|Utilisation élevée de l’UC pendant la dernière minute, 1 pour une utilisation élevée de l’UC, sinon 0. Prise en charge uniquement pour les ressources Power BI Embedded de génération 1.|Aucune dimension|
|qpu_metric|Oui|QPU (Gen1)|Count|Average|QPU. Les plages sont les suivantes : 0 à 20 pour A1, 0 à 40 pour A2, 0 à 40 pour A3, 0 à 80 pour A4, 0 à 160 pour A5, 0 à 320 pour A6. Prise en charge uniquement pour les ressources Power BI Embedded de génération 1.|Aucune dimension|
|QueryDuration|Oui|Durée de la requête (jeux de données) (Gen1)|Millisecondes|Average|Durée de la requête DAX dans le dernier intervalle. Prise en charge uniquement pour les ressources Power BI Embedded de génération 1.|Aucune dimension|
|QueryPoolBusyThreads|Oui|Threads occupés du pool de threads de requêtes|Count|Average|Nombre de threads occupés dans le pool de threads de requêtes.|Aucune dimension|
|QueryPoolIdleThreads|Oui|Threads : threads inactifs du pool de requêtes|Count|Average|Nombre de threads inactifs pour les travaux d’E/S le pool de threads de traitement.|Aucune dimension|
|QueryPoolJobQueueLength|Oui|Longueur de la file d'attente des travaux du pool de requêtes (jeux de données) (Gen1)|Count|Average|Nombre de travaux contenus dans la file d’attente du pool de threads de requêtes. Prise en charge uniquement pour les ressources Power BI Embedded de génération 1.|Aucune dimension|
|Quota|Oui|Mémoire : Quota|Octets|Average|Quota de mémoire actuel, en octets. Le quota de mémoire est également appelé réserve de mémoire ou d’allocation.|Aucune dimension|
|QuotaBlocked|Oui|Mémoire : quota bloqué|Count|Average|Nombre actuel de requêtes de quota qui sont bloquées en attendant la libération d’autres quotas de mémoire.|Aucune dimension|
|RowsConvertedPerSec|Oui|Traitement : lignes converties par seconde|CountPerSecond|Average|Taux de lignes converties lors du traitement.|Aucune dimension|
|RowsReadPerSec|Oui|Traitement : lignes lues par seconde|CountPerSecond|Average|Taux de lignes lues à partir de toutes les bases de données relationnelles.|Aucune dimension|
|RowsWrittenPerSec|Oui|Traitement : lignes écrites par seconde|CountPerSecond|Average|Taux de lignes écrites lors du traitement.|Aucune dimension|
|ShortParsingBusyThreads|Oui|Threads : threads occupés d'analyse courte|Count|Average|Nombre de threads occupés dans le pool de threads d’analyse courte.|Aucune dimension|
|ShortParsingIdleThreads|Oui|Threads : threads inactifs d'analyse courte|Count|Average|Nombre de threads inactifs dans le pool de threads d’analyse courte.|Aucune dimension|
|ShortParsingJobQueueLength|Oui|Threads : longueur de la file d'attente des travaux d'analyse courte|Count|Average|Nombre de travaux contenus dans la file d’attente du pool de threads d’analyse courte.|Aucune dimension|
|SuccessfullConnectionsPerSec|Oui|Connexions réussies par seconde|CountPerSecond|Average|Taux de connexions terminées réussies.|Aucune dimension|
|TotalConnectionFailures|Oui|Nombre total d’échecs de connexion|Count|Average|Total des échecs de tentatives de connexion.|Aucune dimension|
|TotalConnectionRequests|Oui|Nombre total de demandes de connexion|Count|Average|Nombre total de demandes de connexion. Il s’agit des arrivées.|Aucune dimension|
|VertiPaqNonpaged|Oui|Mémoire : mémoire non paginée VertiPaq|Octets|Average|Octets de mémoire verrouillée dans la plage de travail pour utilisation par le moteur en mémoire.|Aucune dimension|
|VertiPaqPaged|Oui|Mémoire : mémoire paginée VertiPaq|Octets|Average|Octets de mémoire paginée utilisée pour les données en mémoire.|Aucune dimension|
|workload_memory_metric|Yes|Mémoire par charge de travail (Gen1)|Octets|Average|Mémoire par charge de travail. Prise en charge uniquement pour les ressources Power BI Embedded de génération 1.|Charge de travail|
|workload_qpu_metric|Yes|QPU par charge de travail (Gen1)|Count|Average|QPU par charge de travail. Les plages sont les suivantes : 0 à 20 pour A1, 0 à 40 pour A2, 0 à 40 pour A3, 0 à 80 pour A4, 0 à 160 pour A5, 0 à 320 pour A6. Prise en charge uniquement pour les ressources Power BI Embedded de génération 1.|Charge de travail|


## <a name="microsoftpurviewaccounts"></a>microsoft.purview/accounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ScanCancelled|Oui|Analyse annulée|Count|Total|Indique le nombre d’analyses annulées.|Aucune dimension|
|ScanCompleted|Oui|Analyse effectuée|Count|Total|Indique le nombre d’analyses correctement effectuées.|Aucune dimension|
|ScanFailed|Oui|Analyse ayant échoué|Count|Total|Indique le nombre d’analyses ayant échoué.|Aucune dimension|
|ScanTimeTaken|Oui|Durée de l’analyse|Secondes|Total|Indique la durée totale d’analyse en secondes.|Aucune dimension|


## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActiveConnections|Non|ActiveConnections|Count|Total|Nombre total d’ActiveConnections pour Microsoft.Relay.|EntityName|
|ActiveListeners|Non|ActiveListeners|Count|Total|Nombre total d’ActiveListeners pour Microsoft.Relay.|EntityName|
|BytesTransferred|Oui|BytesTransferred|Octets|Total|Nombre total de BytesTransferred pour Microsoft.Relay.|EntityName|
|ListenerConnections-ClientError|Non|ListenerConnections-ClientError|Count|Total|ClientError sur ListenerConnections pour Microsoft.Relay.|EntityName, OperationResult|
|ListenerConnections-ServerError|Non|ListenerConnections-ServerError|Count|Total|ServerError sur ListenerConnections pour Microsoft.Relay.|EntityName, OperationResult|
|ListenerConnections-Success|Non|ListenerConnections-Success|Count|Total|ListenerConnections ayant abouti pour Microsoft.Relay.|EntityName, OperationResult|
|ListenerConnections-TotalRequests|Non|ListenerConnections-TotalRequests|Count|Total|Nombre total de ListenerConnections pour Microsoft.Relay.|EntityName|
|ListenerDisconnects|Non|ListenerDisconnects|Count|Total|Nombre total de ListenerDisconnects pour Microsoft.Relay.|EntityName|
|SenderConnections-ClientError|Non|SenderConnections-ClientError|Count|Total|ClientError sur SenderConnections pour Microsoft.Relay.|EntityName, OperationResult|
|SenderConnections-ServerError|Non|SenderConnections-ServerError|Count|Total|ServerError sur SenderConnections pour Microsoft.Relay.|EntityName, OperationResult|
|SenderConnections-Success|Non|SenderConnections-Success|Count|Total|SenderConnections ayant abouti pour Microsoft.Relay.|EntityName, OperationResult|
|SenderConnections-TotalRequests|Non|SenderConnections-TotalRequests|Count|Total|Nombre total de demandes SenderConnections pour Microsoft.Relay.|EntityName|
|SenderDisconnects|Non|SenderDisconnects|Count|Total|Nombre total de SenderDisconnects pour Microsoft.Relay.|EntityName|


## <a name="microsoftresourcessubscriptions"></a>microsoft.resources/subscriptions

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Latence|Oui|Données de latence des demandes HTTP entrantes|Count|Average|Données de latence des demandes HTTP entrantes|Méthode, Espace de noms, RequestRegion, ResourceType, Microsoft.SubscriptionId|
|Trafic|Oui|Trafic|Count|Average|Trafic HTTP|RequestRegion, StatusCode, StatusCodeClass, ResourceType, Espaces de noms, Microsoft.SubscriptionId|


## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|DocumentsProcessedCount|Yes|Nombre de documents traités|Nombre|Total|Nombre de documents traités|DataSourceName, Failed, IndexerName, IndexName, SkillsetName|
|SearchLatency|Oui|Latence de recherche|Secondes|Average|Latence moyenne de recherche du service de recherche|Aucune dimension|
|SearchQueriesPerSecond|Oui|Requêtes de recherche par seconde|CountPerSecond|Average|Requêtes de recherche par seconde pour le service de recherche|Aucune dimension|
|SkillExecutionCount|Yes|Nombre d’appels d’exécution de compétences|Nombre|Total|Nombre d’exécutions de compétences|DataSourceName, Failed, IndexerName, SkillName, SkillsetName, SkillType|
|ThrottledSearchQueriesPercentage|Oui|Pourcentage de requêtes de recherche limitées|Pourcentage|Average|Pourcentage de requêtes de recherche limitées par le service de recherche|Aucune dimension|


## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActiveConnections|Non|ActiveConnections|Count|Total|Nombre total de connexions actives pour Microsoft.ServiceBus.|Aucune dimension|
|ActiveMessages|Non|Nombre de messages actifs dans une file d’attente/rubrique.|Count|Average|Nombre de messages actifs dans une file d’attente/rubrique.|EntityName|
|ConnectionsClosed|Non|Connexions fermées.|Count|Average|Connexions fermées pour Microsoft.ServiceBus.|EntityName|
|ConnectionsOpened|Non|Connexions ouvertes.|Count|Average|Connexions ouvertes pour Microsoft.ServiceBus.|EntityName|
|CPUXNS|Non|Processeur (déprécié)|Pourcentage|Maximale|Métrique d’utilisation du processeur de l’espace de noms Service Bus Premium. Cette métrique est dépréciée. Utilisez la métrique de processeur (NamespaceCpuUsage) à la place.|Réplica|
|DeadletteredMessages|Non|Nombre de messages de lettres mortes dans une file d’attente/rubrique.|Count|Average|Nombre de messages de lettres mortes dans une file d’attente/rubrique.|EntityName|
|IncomingMessages|Oui|Messages entrants|Count|Total|Messages entrants pour Microsoft.ServiceBus.|EntityName|
|IncomingRequests|Oui|Demandes entrantes|Count|Total|Demandes entrantes pour Microsoft.ServiceBus.|EntityName|
|Messages|Non|Nombre de messages dans une file d’attente/rubrique.|Count|Average|Nombre de messages dans une file d’attente/rubrique.|EntityName|
|NamespaceCpuUsage|Non|UC|Pourcentage|Maximale|Métrique d’utilisation du processeur de l’espace de noms Service Bus Premium.|Réplica|
|NamespaceMemoryUsage|Non|Utilisation de la mémoire|Pourcentage|Maximale|Métrique d’utilisation de la mémoire de l’espace de noms Service Bus Premium|Réplica|
|OutgoingMessages|Oui|Messages sortants|Count|Total|Messages sortants pour Microsoft.ServiceBus.|EntityName|
|ScheduledMessages|Non|Nombre de messages planifiés dans une file d’attente/rubrique.|Count|Average|Nombre de messages planifiés dans une file d’attente/rubrique.|EntityName|
|ServerErrors|Non|Erreurs de serveur.|Count|Total|Erreurs de serveur pour Microsoft.ServiceBus.|EntityName, OperationResult|
|Taille|Non|Taille|Octets|Average|Taille d’une file d’attente/rubrique en octets.|EntityName|
|SuccessfulRequests|Non|Requêtes ayant réussi|Count|Total|Nombre total de demandes ayant réussi pour un espace de noms|EntityName, OperationResult|
|ThrottledRequests|Non|Demandes limitées.|Count|Total|Demandes limitées pour Microsoft.ServiceBus.|EntityName, OperationResult|
|UserErrors|Non|Erreurs d’utilisateur.|Count|Total|Erreurs d’utilisateur pour Microsoft.ServiceBus.|EntityName, OperationResult|
|WSXNS|Non|Utilisation de la mémoire (déprécié)|Pourcentage|Maximale|Métrique d’utilisation de la mémoire de l’espace de noms Service Bus Premium Cette métrique est déconseillée. Utilisez la métrique d’utilisation de la mémoire (NamespaceMemoryUsage) à la place.|Réplica|


## <a name="microsoftservicefabricmeshapplications"></a>Microsoft.ServiceFabricMesh/applications

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActualCpu|Non|ActualCpu|Count|Average|Utilisation réelle du processeur en millicoeurs|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ActualMemory|Non|ActualMemory|Octets|Average|Utilisation réelle de la mémoire en octets|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|AllocatedCpu|Non|AllocatedCpu|Count|Average|Processeur alloué à ce conteneur en millicoeurs|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|AllocatedMemory|Non|AllocatedMemory|Octets|Average|Mémoire allouée à ce conteneur en Mo|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|ApplicationStatus|Non|ApplicationStatus|Count|Average|État de l'application Service Fabric Mesh|ApplicationName, Status|
|ContainerStatus|Non|ContainerStatus|Count|Average|État du conteneur dans l'application Service Fabric Mesh|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName, Status|
|CpuUtilization|Non|CpuUtilization|Pourcentage|Average|Utilisation du processeur pour ce conteneur en pourcentage de AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|MemoryUtilization|Non|MemoryUtilization|Pourcentage|Average|Utilisation du processeur pour ce conteneur en pourcentage de AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceReplicaName|
|RestartCount|Non|RestartCount|Count|Average|Nombre de redémarrage d'un conteneur dans l'application Service Fabric Mesh|ApplicationName, Status, ServiceName, ServiceReplicaName, CodePackageName|
|ServiceReplicaStatus|Non|ServiceReplicaStatus|Count|Average|État d’intégrité d’un réplica de service dans l'application Service Fabric Mesh|ApplicationName, Status, ServiceName, ServiceReplicaName|
|ServiceStatus|Non|ServiceStatus|Count|Average|État d’intégrité d’un service dans l'application Service Fabric Mesh|ApplicationName, Status, ServiceName|


## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ConnectionCount|Oui|Nombre de connexions|Count|Maximale|Nombre de connexions utilisateur|Point de terminaison|
|InboundTraffic|Oui|Trafic entrant|Octets|Total|Trafic entrant de service|Aucune dimension|
|MessageCount|Oui|Nombre de messages|Count|Total|Nombre total de messages.|Aucune dimension|
|OutboundTraffic|Oui|Trafic sortant|Octets|Total|Trafic sortant de service|Aucune dimension|
|SystemErrors|Oui|Erreurs système|Pourcentage|Maximale|Pourcentage d’erreurs système|Aucune dimension|
|UserErrors|Oui|Erreurs d’utilisateur|Pourcentage|Maximale|Pourcentage d’erreurs d’utilisateur|Aucune dimension|


## <a name="microsoftsignalrservicewebpubsub"></a>Microsoft.SignalRService/WebPubSub

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|InboundTraffic|Oui|Trafic entrant|Octets|Total|Trafic provenant de l’extérieur vers l’intérieur du service. Il est agrégé en additionnant tous les octets du trafic.|Aucune dimension|
|OutboundTraffic|Oui|Trafic sortant|Octets|Total|Trafic provenant de l’intérieur vers l’extérieur du service. Il est agrégé en additionnant tous les octets du trafic.|Aucune dimension|
|TotalConnectionCount|Oui|Nombre de connexions|Count|Maximale|Nombre de connexions utilisateur établies avec le service. Il est agrégé en additionnant toutes les connexions en ligne.|Aucune dimension|


## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|avg_cpu_percent|Oui|Pourcentage d’UC moyenne|Pourcentage|Average|Pourcentage d’UC moyenne|Aucune dimension|
|io_bytes_read|Oui|Octets d’E/S lus|Octets|Average|Octets d’E/S lus|Aucune dimension|
|io_bytes_written|Oui|Octets d’E/S écrits|Octets|Average|Octets d’E/S écrits|Aucune dimension|
|io_requests|Oui|Nombre de requêtes d’E/S|Count|Average|Nombre de requêtes d’E/S|Aucune dimension|
|reserved_storage_mb|Oui|Espace de stockage réservé|Count|Average|Espace de stockage réservé|Aucune dimension|
|storage_space_used_mb|Oui|Espace de stockage utilisé|Count|Average|Espace de stockage utilisé|Aucune dimension|
|virtual_core_count|Oui|Nombre de cœurs virtuels|Count|Average|Nombre de cœurs virtuels|Aucune dimension|


## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|database_dtu_consumption_percent|Non|Pourcentage DTU|Pourcentage|Average|Pourcentage DTU|DatabaseResourceId, ElasticPoolResourceId|
|database_storage_used|Non|Espace de données utilisé|Octets|Average|Espace de données utilisé|DatabaseResourceId, ElasticPoolResourceId|
|dtu_consumption_percent|Oui|Pourcentage DTU|Pourcentage|Average|Pourcentage DTU|ElasticPoolResourceId|
|dtu_used|Oui|DTU utilisé|Count|Average|DTU utilisé|DatabaseResourceId|
|storage_used|Oui|Espace de données utilisé|Octets|Average|Espace de données utilisé|ElasticPoolResourceId|


## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|active_queries|Oui|Requêtes actives|Count|Total|Requêtes actives sur tous les groupes de charges de travail. S’applique uniquement aux entrepôts de données.|Aucune dimension|
|allocated_data_storage|Oui|Espace de données alloué|Octets|Average|Stockage des données alloué. Non applicable aux entrepôts de données.|Aucune dimension|
|app_cpu_billed|Oui|Processeur d'application facturé|Count|Total|Processeur d’application facturé. S’applique aux bases de données serverless.|Aucune dimension|
|app_cpu_percent|Oui|Pourcentage processeur d'application|Pourcentage|Average|Pourcentage processeur d'application. S’applique aux bases de données serverless.|Aucune dimension|
|app_memory_percent|Oui|Pourcentage de mémoire de l’application|Pourcentage|Average|Pourcentage de mémoire de l’application. S’applique aux bases de données serverless.|Aucune dimension|
|base_blob_size_bytes|Oui|Taille de stockage d’objet blob de base|Octets|Maximale|Taille de stockage d’objet blob de base. S’applique aux bases de données Hyperscale.|Aucune dimension|
|blocked_by_firewall|Oui|Bloqué par le pare-feu|Count|Total|Bloqué par le pare-feu|Aucune dimension|
|cache_hit_percent|Oui|Pourcentage d’accès au cache|Pourcentage|Maximale|Pourcentage d’accès au cache. S’applique uniquement aux entrepôts de données.|Aucune dimension|
|cache_used_percent|Oui|Pourcentage de cache utilisé|Pourcentage|Maximale|Pourcentage de cache utilisé. S’applique uniquement aux entrepôts de données.|Aucune dimension|
|connection_failed|Oui|Connexions ayant échoué|Count|Total|Connexions ayant échoué|Aucune dimension|
|connection_successful|Oui|Connexions réussies|Count|Total|Connexions réussies|Aucune dimension|
|cpu_limit|Oui|Limite UC|Count|Average|Limite UC. S’applique aux bases de données basées sur vCore.|Aucune dimension|
|cpu_percent|Oui|Pourcentage UC|Pourcentage|Average|Pourcentage UC|Aucune dimension|
|cpu_used|Oui|UC utilisée|Count|Average|UC utilisée. S’applique aux bases de données basées sur vCore.|Aucune dimension|
|deadlock|Oui|Blocages|Count|Total|Interblocages. Non applicable aux entrepôts de données.|Aucune dimension|
|delta_num_of_bytes_read|Yes|Lectures de données distantes|Octets|Total|IOPS des lectures de données. Les unités sont en IOPS, ce qui équivaut à des octets divisés par 8 192.|Aucune dimension|
|delta_num_of_bytes_written|Yes|Écritures de journaux à distance|Octets|Total|IOPS des écritures de journaux. Les unités sont en IOPS, ce qui équivaut à des octets divisés par 8 192.|Aucune dimension|
|diff_backup_size_bytes|Oui|Taille de stockage de sauvegarde différentielle|Octets|Maximale|Taille de stockage de sauvegarde différentielle cumulée. S’applique aux bases de données basées sur vCore. Ne s'applique pas aux bases de données Hyperscale.|Aucune dimension|
|dtu_consumption_percent|Oui|Pourcentage DTU|Pourcentage|Average|Pourcentage DTU. S’applique aux bases de données basées sur DTU.|Aucune dimension|
|dtu_limit|Oui|Limite DTU|Count|Average|Limite DTU. S’applique aux bases de données basées sur DTU.|Aucune dimension|
|dtu_used|Oui|DTU utilisé|Count|Average|DTU utilisé. S’applique aux bases de données basées sur DTU.|Aucune dimension|
|dwu_consumption_percent|Oui|Pourcentage DWU|Pourcentage|Maximale|Pourcentage DWU. S’applique uniquement aux entrepôts de données.|Aucune dimension|
|dwu_limit|Oui|Limite DWU|Count|Maximale|Limite DWU. S’applique uniquement aux entrepôts de données.|Aucune dimension|
|dwu_used|Oui|DWU utilisé|Count|Maximale|DWU utilisé. S’applique uniquement aux entrepôts de données.|Aucune dimension|
|full_backup_size_bytes|Oui|Taille de stockage de sauvegarde complète|Octets|Maximale|Taille de stockage de sauvegarde complète cumulée. S’applique aux bases de données basées sur vCore. Ne s'applique pas aux bases de données Hyperscale.|Aucune dimension|
|local_tempdb_usage_percent|Oui|Pourcentage de tempdb locale|Pourcentage|Average|Pourcentage de tempdb locale. S’applique uniquement aux entrepôts de données.|Aucune dimension|
|log_backup_size_bytes|Oui|Taille de stockage de fichier journal|Octets|Maximale|Taille de stockage de fichier journal cumulée. S’applique aux bases de données vCore et Hyperscale.|Aucune dimension|
|log_write_percent|Oui|Pourcentage E/S du journal|Pourcentage|Average|Pourcentage E/S du journal. Non applicable aux entrepôts de données.|Aucune dimension|
|memory_usage_percent|Oui|Pourcentage de mémoire|Pourcentage|Maximale|Pourcentage de mémoire. S’applique uniquement aux entrepôts de données.|Aucune dimension|
|physical_data_read_percent|Oui|Pourcentage E/S des données|Pourcentage|Average|Pourcentage E/S des données|Aucune dimension|
|queued_queries|Oui|Requêtes mises en file d’attente|Count|Total|Requêtes mises en file d’attente sur tous les groupes de charges de travail. S’applique uniquement aux entrepôts de données.|Aucune dimension|
|sessions_percent|Oui|Pourcentage de sessions|Pourcentage|Average|Pourcentage de sessions. Non applicable aux entrepôts de données.|Aucune dimension|
|snapshot_backup_size_bytes|Oui|Taille de stockage de sauvegarde instantanée|Octets|Maximale|Taille de stockage de sauvegarde instantanée cumulée. S’applique aux bases de données Hyperscale.|Aucune dimension|
|sqlserver_process_core_percent|Oui|Pourcentage de cœurs de processus SQL Server|Pourcentage|Maximale|Utilisation du processeur en pourcentage du processus SQL DB. Non applicable aux entrepôts de données.|Aucune dimension|
|sqlserver_process_memory_percent|Oui|Pourcentage de mémoire de processus SQL Server|Pourcentage|Maximale|Utilisation de la mémoire en pourcentage du processus SQL DB. Non applicable aux entrepôts de données.|Aucune dimension|
|storage|Oui|Espace de données utilisé|Octets|Maximale|Espace de données utilisé. Non applicable aux entrepôts de données.|Aucune dimension|
|storage_percent|Oui|Pourcentage d'espace de données utilisé|Pourcentage|Maximale|Pourcentage d’espace de données utilisé. Non applicable aux entrepôts de données ou base de données hyperscale.|Aucune dimension|
|tempdb_data_size|Oui|Taille du fichier de données tempdb en kilo-octets|Count|Maximale|Espace utilisé dans les fichiers de données tempdb, en kilo-octets. Non applicable aux entrepôts de données.|Aucune dimension|
|tempdb_log_size|Oui|Taille du fichier journal de tempdb en kilo-octets|Count|Maximale|Espace utilisé dans le fichier journal des transactions tempdb, en kilo-octets. Non applicable aux entrepôts de données.|Aucune dimension|
|tempdb_log_used_percent|Oui|Pourcentage d’utilisation du journal tempdb|Pourcentage|Maximale|Pourcentage d’espace utilisé dans le fichier journal des transactions tempdb. Non applicable aux entrepôts de données.|Aucune dimension|
|wlg_active_queries|Oui|Requêtes actives du groupe de charge de travail|Count|Total|Requêtes actives dans le groupe de charges de travail. S’applique uniquement aux entrepôts de données.|WorkloadGroupName, IsUserDefined|
|wlg_active_queries_timeouts|Oui|Délais d’expiration des requêtes du groupe de charge de travail|Count|Total|Requêtes ayant dépassé le délai d’attente pour le groupe de charges de travail. S’applique uniquement aux entrepôts de données.|WorkloadGroupName, IsUserDefined|
|wlg_allocation_relative_to_system_percent|Oui|Allocation du groupe de charge de travail par pourcentage système|Pourcentage|Maximale|Pourcentage alloué de ressources par rapport à l’ensemble du système par groupe de charges de travail. S’applique uniquement aux entrepôts de données.|WorkloadGroupName, IsUserDefined|
|wlg_allocation_relative_to_wlg_effective_cap_percent|Oui|Allocation de groupe de charges de travail par pourcentage de ressources limitées|Pourcentage|Maximale|Pourcentage alloué de ressources par rapport aux ressources limitées indiquées par groupe de charges de travail. S’applique uniquement aux entrepôts de données.|WorkloadGroupName, IsUserDefined|
|wlg_effective_cap_resource_percent|Oui|Pourcentage de ressources limitées réelles|Pourcentage|Maximale|Limite inconditionnelle sur le pourcentage de ressources autorisées pour le groupe de charges de travail, en tenant compte du pourcentage minimal de ressources réelles alloué pour d’autres groupes de charges de travail. S’applique uniquement aux entrepôts de données.|WorkloadGroupName, IsUserDefined|
|wlg_effective_min_resource_percent|Oui|Pourcentage minimal de ressources réelles|Pourcentage|Maximale|Pourcentage minimal de ressources réservées et isolées pour le groupe de charges de travail, en tenant compte du niveau de service minimum. S’applique uniquement aux entrepôts de données.|WorkloadGroupName, IsUserDefined|
|wlg_queued_queries|Oui|Requêtes en file d’attente du groupe de charge de travail|Count|Total|Requêtes mises en file d’attente dans le groupe de charges de travail. S’applique uniquement aux entrepôts de données.|WorkloadGroupName, IsUserDefined|
|workers_percent|Oui|Pourcentage de travaux|Pourcentage|Average|Pourcentage de Workers. Non applicable aux entrepôts de données.|Aucune dimension|
|xtp_storage_percent|Oui|Pourcentage de stockage OLTP en mémoire|Pourcentage|Average|Pourcentage de stockage OLTP en mémoire. Non applicable aux entrepôts de données.|Aucune dimension|


## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|allocated_data_storage|Oui|Espace de données alloué|Octets|Average|Espace de données alloué|Aucune dimension|
|allocated_data_storage_percent|Oui|Pourcentage d'espace de données alloué|Pourcentage|Maximale|Pourcentage d'espace de données alloué|Aucune dimension|
|cpu_limit|Oui|Limite UC|Count|Average|Limite UC. S’applique aux pools élastiques basés sur vCore.|Aucune dimension|
|cpu_percent|Oui|Pourcentage UC|Pourcentage|Average|Pourcentage UC|Aucune dimension|
|cpu_used|Oui|UC utilisée|Count|Average|UC utilisée. S’applique aux pools élastiques basés sur vCore.|Aucune dimension|
|database_allocated_data_storage|Non|Espace de données alloué|Octets|Average|Espace de données alloué|DatabaseResourceId|
|database_cpu_limit|Non|Limite UC|Count|Average|Limite UC|DatabaseResourceId|
|database_cpu_percent|Non|Pourcentage UC|Pourcentage|Average|Pourcentage UC|DatabaseResourceId|
|database_cpu_used|Non|UC utilisée|Count|Average|UC utilisée|DatabaseResourceId|
|database_dtu_consumption_percent|Non|Pourcentage DTU|Pourcentage|Average|Pourcentage DTU|DatabaseResourceId|
|database_eDTU_used|Non|eDTU utilisé|Count|Average|eDTU utilisé|DatabaseResourceId|
|database_log_write_percent|Non|Pourcentage E/S du journal|Pourcentage|Average|Pourcentage E/S du journal|DatabaseResourceId|
|database_physical_data_read_percent|Non|Pourcentage E/S des données|Pourcentage|Average|Pourcentage E/S des données|DatabaseResourceId|
|database_sessions_percent|Non|Pourcentage de sessions|Pourcentage|Average|Pourcentage de sessions|DatabaseResourceId|
|database_storage_used|Non|Espace de données utilisé|Octets|Average|Espace de données utilisé|DatabaseResourceId|
|database_workers_percent|Non|Pourcentage de travaux|Pourcentage|Average|Pourcentage de travaux|DatabaseResourceId|
|dtu_consumption_percent|Oui|Pourcentage DTU|Pourcentage|Average|Pourcentage DTU. S’applique aux pools élastiques basés sur DTU.|Aucune dimension|
|eDTU_limit|Oui|Limite eDTU|Count|Average|Limite eDTU. S’applique aux pools élastiques basés sur DTU.|Aucune dimension|
|eDTU_used|Oui|eDTU utilisé|Count|Average|eDTU utilisé. S’applique aux pools élastiques basés sur DTU.|Aucune dimension|
|log_write_percent|Oui|Pourcentage E/S du journal|Pourcentage|Average|Pourcentage E/S du journal|Aucune dimension|
|physical_data_read_percent|Oui|Pourcentage E/S des données|Pourcentage|Average|Pourcentage E/S des données|Aucune dimension|
|sessions_percent|Oui|Pourcentage de sessions|Pourcentage|Average|Pourcentage de sessions|Aucune dimension|
|sqlserver_process_core_percent|Oui|Pourcentage de cœurs de processus SQL Server|Pourcentage|Maximale|Utilisation du processeur en pourcentage du processus SQL DB. S’applique aux pools élastiques.|Aucune dimension|
|sqlserver_process_memory_percent|Oui|Pourcentage de mémoire de processus SQL Server|Pourcentage|Maximale|Utilisation de la mémoire en pourcentage du processus SQL DB. S’applique aux pools élastiques.|Aucune dimension|
|storage_limit|Oui|Taille maximale des données|Octets|Average|Taille maximale des données|Aucune dimension|
|storage_percent|Oui|Pourcentage d'espace de données utilisé|Pourcentage|Average|Pourcentage d'espace de données utilisé|Aucune dimension|
|storage_used|Oui|Espace de données utilisé|Octets|Average|Espace de données utilisé|Aucune dimension|
|tempdb_data_size|Oui|Taille du fichier de données tempdb en kilo-octets|Count|Maximale|Espace utilisé dans les fichiers de données tempdb, en kilo-octets.|Aucune dimension|
|tempdb_log_size|Oui|Taille du fichier journal de tempdb en kilo-octets|Count|Maximale|Espace utilisé dans le fichier journal des transactions tempdb, en kilo-octets.|Aucune dimension|
|tempdb_log_used_percent|Oui|Pourcentage d’utilisation du journal tempdb|Pourcentage|Maximale|Pourcentage d’espace utilisé dans le fichier journal des transactions tempdb|Aucune dimension|
|workers_percent|Oui|Pourcentage de travaux|Pourcentage|Average|Pourcentage de travaux|Aucune dimension|
|xtp_storage_percent|Oui|Pourcentage de stockage OLTP en mémoire|Pourcentage|Average|Pourcentage de stockage OLTP en mémoire|Aucune dimension|


## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Pourcentage de disponibilité pour le service de stockage ou l’opération API spécifiée. La disponibilité est calculée en prenant la valeur TotalBillableRequests puis en la divisant par le nombre de requêtes applicables, y compris celles qui ont généré des erreurs inattendues. Toutes erreurs inattendues réduisent la disponibilité du service de stockage ou de l’opération API spécifiée.|GeoType, ApiName, Authentication|
|Sortie|Oui|Sortie|Octets|Total|Quantité de données de sortie. Ce volume inclut les sorties vers un client externe provenant du Stockage Azure ainsi que les sorties dans Azure. Par conséquent, ce nombre ne reflète pas les sorties facturables.|GeoType, ApiName, Authentication|
|Entrée|Oui|Entrée|Octets|Total|Quantité de données d’entrée, en octets. Ce nombre inclut les entrées d’un client externe dans Stockage Microsoft Azure ainsi que les entrées dans Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Oui|Latence E2E de réussite|Millisecondes|Average|Latence moyenne de bout en bout des requêtes réussies envoyées à un service de stockage ou à l’opération API spécifiée, en millisecondes. Cette valeur inclut le temps de traitement requis au sein de Stockage Microsoft Azure pour lire la requête, envoyer la réponse et recevoir un accusé de réception de la réponse.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Oui|Latence du serveur avec requête réussie|Millisecondes|Average|Durée moyenne utilisée pour traiter une requête réussie par Stockage Azure. Cette valeur n’inclut pas la latence réseau spécifiée dans SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transactions|Oui|Transactions|Count|Total|Nombre de requêtes envoyées à un service de stockage ou à l’opération API spécifiée. Ce nombre inclut les requêtes réussies et celles ayant échoué, ainsi que les requêtes qui ont généré des erreurs. Utilisez la dimension ResponseType pour connaître le nombre des différents types de réponses.|ResponseType, GeoType, ApiName, Authentication|
|UsedCapacity|Oui|Capacité utilisée|Octets|Average|Quantité de stockage utilisée par le compte de stockage. Pour les comptes de stockage standard, il s’agit de la somme de la capacité utilisée par les objets blob, tables, fichiers et files d’attente. Pour les comptes de Stockage Premium et Blob, elle équivaut à BlobCapacity ou à FileCapacity.|Aucune dimension|


## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Pourcentage de disponibilité pour le service de stockage ou l’opération API spécifiée. La disponibilité est calculée en prenant la valeur TotalBillableRequests puis en la divisant par le nombre de requêtes applicables, y compris celles qui ont généré des erreurs inattendues. Toutes erreurs inattendues réduisent la disponibilité du service de stockage ou de l’opération API spécifiée.|GeoType, ApiName, Authentication|
|BlobCapacity|Non|Capacité d’objet blob|Octets|Average|Quantité de stockage utilisée par le service BLOB du compte de stockage, en octets.|BlobType, Tier|
|BlobCount|Non|Nombre d’objets blob|Count|Average|Nombre d’objets blob stockés dans le compte de stockage.|BlobType, Tier|
|BlobProvisionedSize|Non|Taille provisionnée du service blob|Octets|Average|Quantité de stockage approvisionnée dans le service BLOB du compte de stockage, en octets.|BlobType, Tier|
|ContainerCount|Oui|Nombre de conteneurs d’objets blob|Count|Average|Nombre de conteneurs dans le compte de stockage.|Aucune dimension|
|Sortie|Oui|Sortie|Octets|Total|Quantité de données de sortie. Ce volume inclut les sorties vers un client externe provenant du Stockage Azure ainsi que les sorties dans Azure. Par conséquent, ce nombre ne reflète pas les sorties facturables.|GeoType, ApiName, Authentication|
|IndexCapacity|Non|Capacité d'index|Octets|Average|Quantité de stockage utilisée par l’index hiérarchique Azure Data Lake Storage Gen2|Aucune dimension|
|Entrée|Oui|Entrée|Octets|Total|Quantité de données d’entrée, en octets. Ce nombre inclut les entrées d’un client externe dans Stockage Microsoft Azure ainsi que les entrées dans Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Oui|Latence E2E de réussite|Millisecondes|Average|Latence moyenne de bout en bout des requêtes réussies envoyées à un service de stockage ou à l’opération API spécifiée, en millisecondes. Cette valeur inclut le temps de traitement requis au sein de Stockage Microsoft Azure pour lire la requête, envoyer la réponse et recevoir un accusé de réception de la réponse.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Oui|Latence du serveur avec requête réussie|Millisecondes|Average|Durée moyenne utilisée pour traiter une requête réussie par Stockage Azure. Cette valeur n’inclut pas la latence réseau spécifiée dans SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transactions|Oui|Transactions|Count|Total|Nombre de requêtes envoyées à un service de stockage ou à l’opération API spécifiée. Ce nombre inclut les requêtes réussies et celles ayant échoué, ainsi que les requêtes qui ont généré des erreurs. Utilisez la dimension ResponseType pour connaître le nombre des différents types de réponses.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Pourcentage de disponibilité pour le service de stockage ou l’opération API spécifiée. La disponibilité est calculée en prenant la valeur TotalBillableRequests puis en la divisant par le nombre de requêtes applicables, y compris celles qui ont généré des erreurs inattendues. Toutes erreurs inattendues réduisent la disponibilité du service de stockage ou de l’opération API spécifiée.|GeoType, ApiName, Authentication, FileShare|
|Sortie|Oui|Sortie|Octets|Total|Quantité de données de sortie. Ce volume inclut les sorties vers un client externe provenant du Stockage Azure ainsi que les sorties dans Azure. Par conséquent, ce nombre ne reflète pas les sorties facturables.|GeoType, ApiName, Authentication, FileShare|
|FileCapacity|Non|Capacité de fichiers|Octets|Average|Quantité de stockage de fichiers utilisée par le compte de stockage.|FileShare|
|FileCount|Non|Nombre de fichiers|Count|Average|Nombre de fichiers dans le compte de stockage.|FileShare|
|FileShareCapacityQuota|Non|Quota de capacité du partage de fichiers|Octets|Average|Limite supérieure de la quantité de stockage pouvant être utilisée par le service Azure Files, en octets.|FileShare|
|FileShareCount|Non|Nombre de partages de fichiers|Count|Average|Nombre de partages de fichiers dans le compte de stockage.|Aucune dimension|
|FileShareProvisionedIOPS|Non|IOPS provisionnées du partage de fichiers|CountPerSecond|Average|Nombre de référence d’IOPS provisionnées pour le partage de fichiers Premium dans le compte de stockage de fichiers Premium. Ce nombre est calculé en fonction de la taille provisionnée (quota) de la capacité de partage.|FileShare|
|FileShareSnapshotCount|Non|Nombre d’instantanés de partage de fichiers|Count|Average|Nombre de captures instantanées présentes sur le partage dans le service Fichier du compte de stockage.|FileShare|
|FileShareSnapshotSize|Non|Taille d’instantané de partage de fichiers|Octets|Average|Quantité de stockage utilisée par les instantanés dans le service Fichier du compte de stockage, en octets.|FileShare|
|Entrée|Oui|Entrée|Octets|Total|Quantité de données d’entrée, en octets. Ce nombre inclut les entrées d’un client externe dans Stockage Microsoft Azure ainsi que les entrées dans Azure.|GeoType, ApiName, Authentication, FileShare|
|SuccessE2ELatency|Oui|Latence E2E de réussite|Millisecondes|Average|Latence moyenne de bout en bout des requêtes réussies envoyées à un service de stockage ou à l’opération API spécifiée, en millisecondes. Cette valeur inclut le temps de traitement requis au sein de Stockage Microsoft Azure pour lire la requête, envoyer la réponse et recevoir un accusé de réception de la réponse.|GeoType, ApiName, Authentication, FileShare|
|SuccessServerLatency|Oui|Latence du serveur avec requête réussie|Millisecondes|Average|Durée moyenne utilisée pour traiter une requête réussie par Stockage Azure. Cette valeur n’inclut pas la latence réseau spécifiée dans SuccessE2ELatency.|GeoType, ApiName, Authentication, FileShare|
|Transactions|Oui|Transactions|Count|Total|Nombre de requêtes envoyées à un service de stockage ou à l’opération API spécifiée. Ce nombre inclut les requêtes réussies et celles ayant échoué, ainsi que les requêtes qui ont généré des erreurs. Utilisez la dimension ResponseType pour connaître le nombre des différents types de réponses.|ResponseType, GeoType, ApiName, Authentication, FileShare|


## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Pourcentage de disponibilité pour le service de stockage ou l’opération API spécifiée. La disponibilité est calculée en prenant la valeur TotalBillableRequests puis en la divisant par le nombre de requêtes applicables, y compris celles qui ont généré des erreurs inattendues. Toutes erreurs inattendues réduisent la disponibilité du service de stockage ou de l’opération API spécifiée.|GeoType, ApiName, Authentication|
|Sortie|Oui|Sortie|Octets|Total|Quantité de données de sortie. Ce volume inclut les sorties vers un client externe provenant du Stockage Azure ainsi que les sorties dans Azure. Par conséquent, ce nombre ne reflète pas les sorties facturables.|GeoType, ApiName, Authentication|
|Entrée|Oui|Entrée|Octets|Total|Quantité de données d’entrée, en octets. Ce nombre inclut les entrées d’un client externe dans Stockage Microsoft Azure ainsi que les entrées dans Azure.|GeoType, ApiName, Authentication|
|QueueCapacity|Oui|Capacité de la file d’attente|Octets|Average|Quantité de stockage de files d’attente utilisée par le compte de stockage.|Aucune dimension|
|QueueCount|Oui|Nombre de files d’attente|Count|Average|Nombre de files d’attente dans le compte de stockage.|Aucune dimension|
|QueueMessageCount|Oui|Nombre de messages dans la file d’attente|Count|Average|Nombre de messages de file d’attente non expirés dans le compte de stockage.|Aucune dimension|
|SuccessE2ELatency|Oui|Latence E2E de réussite|Millisecondes|Average|Latence moyenne de bout en bout des requêtes réussies envoyées à un service de stockage ou à l’opération API spécifiée, en millisecondes. Cette valeur inclut le temps de traitement requis au sein de Stockage Microsoft Azure pour lire la requête, envoyer la réponse et recevoir un accusé de réception de la réponse.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Oui|Latence du serveur avec requête réussie|Millisecondes|Average|Durée moyenne utilisée pour traiter une requête réussie par Stockage Azure. Cette valeur n’inclut pas la latence réseau spécifiée dans SuccessE2ELatency.|GeoType, ApiName, Authentication|
|Transactions|Oui|Transactions|Count|Total|Nombre de requêtes envoyées à un service de stockage ou à l’opération API spécifiée. Ce nombre inclut les requêtes réussies et celles ayant échoué, ainsi que les requêtes qui ont généré des erreurs. Utilisez la dimension ResponseType pour connaître le nombre des différents types de réponses.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disponibilité|Oui|Disponibilité|Pourcentage|Average|Pourcentage de disponibilité pour le service de stockage ou l’opération API spécifiée. La disponibilité est calculée en prenant la valeur TotalBillableRequests puis en la divisant par le nombre de requêtes applicables, y compris celles qui ont généré des erreurs inattendues. Toutes erreurs inattendues réduisent la disponibilité du service de stockage ou de l’opération API spécifiée.|GeoType, ApiName, Authentication|
|Sortie|Oui|Sortie|Octets|Total|Quantité de données de sortie. Ce volume inclut les sorties vers un client externe provenant du Stockage Azure ainsi que les sorties dans Azure. Par conséquent, ce nombre ne reflète pas les sorties facturables.|GeoType, ApiName, Authentication|
|Entrée|Oui|Entrée|Octets|Total|Quantité de données d’entrée, en octets. Ce nombre inclut les entrées d’un client externe dans Stockage Microsoft Azure ainsi que les entrées dans Azure.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Oui|Latence E2E de réussite|Millisecondes|Average|Latence moyenne de bout en bout des requêtes réussies envoyées à un service de stockage ou à l’opération API spécifiée, en millisecondes. Cette valeur inclut le temps de traitement requis au sein de Stockage Microsoft Azure pour lire la requête, envoyer la réponse et recevoir un accusé de réception de la réponse.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Oui|Latence du serveur avec requête réussie|Millisecondes|Average|Durée moyenne utilisée pour traiter une requête réussie par Stockage Azure. Cette valeur n’inclut pas la latence réseau spécifiée dans SuccessE2ELatency.|GeoType, ApiName, Authentication|
|TableCapacity|Oui|Capacité de la table|Octets|Average|Quantité de stockage de tables utilisée par le compte de stockage.|Aucune dimension|
|TableCount|Oui|Nombre de tables|Count|Average|Nombre de tables dans le compte de stockage.|Aucune dimension|
|TableEntityCount|Oui|Nombre d’entités de table|Count|Average|Nombre d’entités de table dans le compte de stockage.|Aucune dimension|
|Transactions|Oui|Transactions|Count|Total|Nombre de requêtes envoyées à un service de stockage ou à l’opération API spécifiée. Ce nombre inclut les requêtes réussies et celles ayant échoué, ainsi que les requêtes qui ont généré des erreurs. Utilisez la dimension ResponseType pour connaître le nombre des différents types de réponses.|ResponseType, GeoType, ApiName, Authentication|


## <a name="microsoftstoragecachecaches"></a>Microsoft.StorageCache/caches

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ClientIOPS|Oui|Total des IOPS du client|Count|Average|Proportion des opérations de fichier client traitées par le cache.|Aucune dimension|
|ClientLatency|Oui|Latence moyenne du client|Millisecondes|Average|Latence moyenne des opérations de fichier client dans le cache.|Aucune dimension|
|ClientLockIOPS|Oui|IOPS de verrouillage du client|CountPerSecond|Average|Opérations de verrouillage de fichier client par seconde.|Aucune dimension|
|ClientMetadataReadIOPS|Oui|IOPS de lecture de métadonnées client|CountPerSecond|Average|Taux des opérations de fichier client envoyées au cache, à l’exclusion des lectures de données qui ne modifient pas l’état persistant.|Aucune dimension|
|ClientMetadataWriteIOPS|Oui|IOPS d’écriture de métadonnées client|CountPerSecond|Average|Taux des opérations de fichier client envoyées au cache, à l’exclusion des écritures de données qui modifient l’état persistant.|Aucune dimension|
|ClientReadIOPS|Oui|IOPS de lecture du client|CountPerSecond|Average|Opérations de lecture du client par seconde.|Aucune dimension|
|ClientReadThroughput|Oui|Débit moyen de lecture du cache|BytesPerSecond|Average|Taux de transfert des données de lecture du client.|Aucune dimension|
|ClientStatus|Yes|État du client|Nombre|Total|Informations relatives à la connexion du client.|ClientSource, CacheAddress, ClientAddress, Protocol, ConnectionType|
|ClientWriteIOPS|Oui|IOPS d’écriture du client|CountPerSecond|Average|Opérations d’écriture du client par seconde.|Aucune dimension|
|ClientWriteThroughput|Oui|Débit moyen d’écriture dans le cache|BytesPerSecond|Average|Taux de transfert des données d’écriture du client.|Aucune dimension|
|FileOps|Yes|Opérations sur les fichiers|CountPerSecond|Average|Nombre d’opérations sur les fichiers par seconde.|SourceFile, Rank, FileType|
|FileReads|Yes|Lectures de fichiers|BytesPerSecond|Average|Nombre d’octets par seconde lus à partir d’un fichier.|SourceFile, Rank, FileType|
|FileUpdates|Yes|Mises à jour de fichiers|CountPerSecond|Average|Nombre de mises à jour de répertoire et d’opérations sur les métadonnées par seconde.|SourceFile, Rank, FileType|
|FileWrites|Yes|Écritures de fichiers|BytesPerSecond|Average|Nombre d’octets par seconde écrits dans un fichier.|SourceFile, Rank, FileType|
|StorageTargetAsyncWriteThroughput|Oui|Débit d’écriture asynchrone dans StorageTarget|BytesPerSecond|Average|Vitesse à laquelle le cache écrit les données dans un StorageTarget particulier de manière asynchrone. Il s’agit d’écritures opportunistes qui n’entraînent pas le blocage des clients.|StorageTarget|
|StorageTargetBlocksRecycled|Yes|Blocs de cibles de stockage recyclés|Count|Average|Nombre total de blocs de cache 16K recyclés (libérés) par cible de stockage.|StorageTarget|
|StorageTargetFillThroughput|Oui|Débit de remplissage de StorageTarget|BytesPerSecond|Average|Vitesse à laquelle le cache lit les données dans StorageTarget pour gérer une opération non réussie dans le cache.|StorageTarget|
|StorageTargetFreeReadSpace|Yes|Espace de lecture libre de la cible de stockage|Octets|Average|Espace de lecture disponible pour la mise en cache des fichiers associés à une cible de stockage.|StorageTarget|
|StorageTargetFreeWriteSpace|Yes|Espace d’écriture libre de la cible de stockage|Octets|Average|Espace d’écriture disponible pour les données incorrectes associées à une cible de stockage.|StorageTarget|
|StorageTargetHealth|Oui|Intégrité de la cible de stockage|Count|Average|Résultats booléens du test de connectivité entre le cache et les cibles de stockage.|Aucune dimension|
|StorageTargetIOPS|Oui|Total des IOPS dans StorageTarget|Count|Average|Taux de toutes les opérations de fichier que le cache envoie à un StorageTarget particulier.|StorageTarget|
|StorageTargetLatency|Oui|Latence de StorageTarget|Millisecondes|Average|Latence moyenne de l’aller-retour de toutes les opérations de fichier que le cache envoie à un StorageTarget particulier.|StorageTarget|
|StorageTargetMetadataReadIOPS|Oui|IOPS de lecture des métadonnées dans StorageTarget|CountPerSecond|Average|Taux des opérations de fichier qui ne modifient pas l’état persistant, hormis l’opération de lecture, que le cache envoie à un StorageTarget particulier.|StorageTarget|
|StorageTargetMetadataWriteIOPS|Oui|IOPS d’écriture des métadonnées dans StorageTarget|CountPerSecond|Average|Taux des opérations de fichier qui modifient l’état persistant, hormis l’opération d’écriture, que le cache envoie à un StorageTarget particulier.|StorageTarget|
|StorageTargetReadAheadThroughput|Oui|Débit de lecture anticipée dans StorageTarget|BytesPerSecond|Average|Vitesse à laquelle le cache lit de manière opportuniste les données dans StorageTarget.|StorageTarget|
|StorageTargetReadIOPS|Oui|IOPS de lecture dans StorageTarget|CountPerSecond|Average|Taux des opérations de lecture de fichier que le cache envoie à un StorageTarget particulier.|StorageTarget|
|StorageTargetRecycleRate|Yes|Fréquence de recyclage des cibles de stockage|BytesPerSecond|Average|Fréquence de recyclage de l’espace de cache associé à une cible de stockage dans le cache HPC. Il s’agit de la vitesse à laquelle les données existantes sont effacées du cache pour faire de la place aux nouvelles données.|StorageTarget|
|StorageTargetSyncWriteThroughput|Oui|Débit d’écriture synchrone dans StorageTarget|BytesPerSecond|Average|Vitesse à laquelle le cache écrit les données dans un StorageTarget particulier de manière synchrone. Il s’agit d’écritures qui provoquent le blocage des clients.|StorageTarget|
|StorageTargetTotalReadThroughput|Oui|Débit total de lecture dans StorageTarget|BytesPerSecond|Average|Vitesse totale à laquelle le cache lit les données dans un StorageTarget particulier.|StorageTarget|
|StorageTargetTotalWriteThroughput|Oui|Débit total d’écriture dans StorageTarget|BytesPerSecond|Average|Vitesse totale à laquelle le cache écrit les données dans un StorageTarget particulier.|StorageTarget|
|StorageTargetUsedReadSpace|Yes|Espace de lecture utilisé par la cible de stockage|Octets|Average|Espace de lecture utilisé par les fichiers mis en cache associés à une cible de stockage.|StorageTarget|
|StorageTargetUsedWriteSpace|Yes|Espace d’écriture utilisé par la cible de stockage|Octets|Average|Espace d’écriture utilisé par les données incorrectes associées à une cible de stockage.|StorageTarget|
|StorageTargetWriteIOPS|Oui|IOPS d’écriture dans StorageTarget|Count|Average|Taux des opérations d’écriture de fichier que le cache envoie à un StorageTarget particulier.|StorageTarget|
|TotalBlocksRecycled|Yes|Nombre total de blocs recyclés|Count|Average|Nombre total de blocs de cache 16K recyclés (libérés) pour le cache HPC.|Aucune dimension|
|TotalFreeReadSpace|Yes|Espace de lecture libre|Octets|Average|Espace total disponible pour la mise en cache des fichiers lus.|Aucune dimension|
|TotalFreeWriteSpace|Yes|Espace d’écriture libre|Octets|Average|Espace d’écriture total disponible pour stocker les données modifiées dans le cache.|Aucune dimension|
|TotalRecycleRate|Yes|Fréquence de recyclage|BytesPerSecond|Average|Fréquence de recyclage de l’espace de cache total dans le cache HPC. Il s’agit de la vitesse à laquelle les données existantes sont effacées du cache pour faire de la place aux nouvelles données.|Aucune dimension|
|TotalUsedReadSpace|Yes|Espace de lecture utilisé|Octets|Average|Espace de lecture total utilisé par les données incorrectes pour le cache HPC.|Aucune dimension|
|TotalUsedWriteSpace|Yes|Espace d’écriture utilisé|Octets|Average|Espace d’écriture total utilisé par les données incorrectes pour le cache HPC.|Aucune dimension|
|Uptime|Oui|Uptime|Count|Average|Résultats booléens du test de connectivité entre le cache et le système de supervision.|Aucune dimension|


## <a name="microsoftstoragesyncstoragesyncservices"></a>microsoft.storagesync/storageSyncServices

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ServerSyncSessionResult|Oui|Résultat de session de synchronisation|Count|Average|Métrique consignant une valeur de 1 chaque fois que le point de terminaison de serveur termine une session de synchronisation avec le point de terminaison cloud|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncBatchTransferredFileBytes|Oui|Octets synchronisés|Octets|Total|Taille totale des fichiers transférés pour les sessions de synchronisation|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncComputedCacheHitRate|Yes|Taux de correspondance dans le cache de la hiérarchisation cloud|Pourcentage|Average|Pourcentage d’octets servis à partir du cache|SyncGroupName, ServerName, ServerEndpointName|
|StorageSyncRecallComputedSuccessRate|Oui|Taux de réussite de rappel de hiérarchisation cloud|Pourcentage|Average|Pourcentage de tous les rappels réussis|SyncGroupName, ServerName, ServerEndpointName|
|StorageSyncRecalledNetworkBytesByApplication|Oui|Taille de rappel de hiérarchisation cloud par application|Octets|Total|Taille des données rappelées par application|SyncGroupName, ServerName, ApplicationName|
|StorageSyncRecalledTotalNetworkBytes|Oui|Taille de rappel de la hiérarchisation cloud|Octets|Total|taille des données rappelées ;|SyncGroupName, ServerName, ServerEndpointName|
|StorageSyncRecallThroughputBytesPerSecond|Oui|Débit de rappel de la hiérarchisation cloud|BytesPerSecond|Average|Taille de débit de rappel des données|SyncGroupName, ServerName, ServerEndpointName|
|StorageSyncServerHeartbeat|Oui|État du serveur en ligne|Count|Maximale|Métrique consignant une valeur de 1 chaque fois que le serveur inscrit enregistre une pulsation avec le point de terminaison cloud|ServerName|
|StorageSyncSyncSessionAppliedFilesCount|Oui|Fichiers synchronisés|Count|Total|Nombre de fichiers synchronisés|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Oui|Fichiers ne se synchronisant pas|Count|Average|Nombre de fichiers dont la synchronisation a échoué|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncTieringCacheSizeBytes|Yes|Taille du cache du serveur|Octets|Average|Taille des données mises en cache sur le serveur|SyncGroupName, ServerName, ServerEndpointName|


## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AMLCalloutFailedRequests|Oui|Requêtes de fonction ayant échoué|Count|Total|Requêtes de fonction ayant échoué|LogicalName, PartitionId, ProcessorInstance|
|AMLCalloutInputEvents|Oui|Événements de fonction|Count|Total|Événements de fonction|LogicalName, PartitionId, ProcessorInstance|
|AMLCalloutRequests|Oui|Requêtes de fonction|Count|Total|Requêtes de fonction|LogicalName, PartitionId, ProcessorInstance|
|ConversionErrors|Oui|Erreurs de conversion de données|Count|Total|Erreurs de conversion de données|LogicalName, PartitionId, ProcessorInstance|
|DeserializationError|Oui|Erreurs de désérialisation d’entrée|Count|Total|Erreurs de désérialisation d’entrée|LogicalName, PartitionId, ProcessorInstance|
|DroppedOrAdjustedEvents|Oui|Événements en désordre|Count|Total|Événements en désordre|LogicalName, PartitionId, ProcessorInstance|
|EarlyInputEvents|Oui|Événements d’entrée précoces|Count|Total|Événements d’entrée précoces|LogicalName, PartitionId, ProcessorInstance|
|Erreurs|Oui|Erreurs d’exécution|Count|Total|Erreurs d’exécution|LogicalName, PartitionId, ProcessorInstance|
|InputEventBytes|Oui|Octets des événements d’entrée|Octets|Total|Octets des événements d’entrée|LogicalName, PartitionId, ProcessorInstance|
|InputEvents|Oui|Événements d’entrée|Count|Total|Événements d’entrée|LogicalName, PartitionId, ProcessorInstance|
|InputEventsSourcesBacklogged|Oui|Événements d'entrée en backlog|Count|Maximale|Événements d'entrée en backlog|LogicalName, PartitionId, ProcessorInstance|
|InputEventsSourcesPerSecond|Oui|Sources d'entrée reçues|Count|Total|Sources d'entrée reçues|LogicalName, PartitionId, ProcessorInstance|
|LateInputEvents|Oui|Événements d’entrée tardifs|Count|Total|Événements d’entrée tardifs|LogicalName, PartitionId, ProcessorInstance|
|OutputEvents|Oui|Événements de sortie|Count|Total|Événements de sortie|LogicalName, PartitionId, ProcessorInstance|
|OutputWatermarkDelaySeconds|Oui|Délai en filigrane|Secondes|Maximale|Délai en filigrane|LogicalName, PartitionId, ProcessorInstance|
|ProcessCPUUsagePercentage|Oui|% d’utilisation de l’UC (préversion)|Pourcentage|Maximale|% d’utilisation de l’UC (préversion)|LogicalName, PartitionId, ProcessorInstance|
|ResourceUtilization|Oui|Utilisation de % d’unités de diffusion|Pourcentage|Maximale|Utilisation de % d’unités de diffusion|LogicalName, PartitionId, ProcessorInstance|


## <a name="microsoftsynapseworkspaces"></a>Microsoft.Synapse/workspaces

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BuiltinSqlPoolDataProcessedBytes|Non|Données traitées (octets)|Octets|Total|Quantité de données traitées par les requêtes|Aucune dimension|
|BuiltinSqlPoolLoginAttempts|Non|Tentatives de connexion|Count|Total|Nombre de tentatives de connexion ayant abouti ou échoué|Résultats|
|BuiltinSqlPoolRequestsEnded|Non|Requêtes terminées|Count|Total|Nombre de requêtes ayant abouti, échoué ou qui ont été annulées|Résultat|
|IntegrationActivityRunsEnded|Non|Exécutions d’activité terminées|Count|Total|Nombre d’activités d’intégration ayant abouti, échoué ou qui ont été annulées|Result, FailureType, Activity, ActivityType, Pipeline|
|IntegrationPipelineRunsEnded|Non|Exécutions de pipeline terminées|Count|Total|Nombre d’exécutions de pipeline d’intégration ayant abouti, échoué ou qui ont été annulées|Result, FailureType, Pipeline|
|IntegrationTriggerRunsEnded|Non|Exécutions du déclencheur terminées|Count|Total|Nombre de déclencheurs d’intégration ayant abouti, échoué ou qui ont été annulés|Result, FailureType, Trigger|
|SQLStreamingBackloggedInputEventSources|Non|Événements d’entrée en backlog (préversion)|Count|Total|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Nombre de sources d’événements d’entrée en backlog.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingConversionErrors|Non|Erreurs de conversion de données (préversion)|Count|Total|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Nombre d’événements de sortie qui n’ont pas pu être convertis dans le schéma de sortie attendu. La stratégie de l’erreur peut être modifiée sur 'Drop' pour supprimer les événements confrontés à ce scénario.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingDeserializationError|Non|Erreurs de désérialisation d’entrée (préversion)|Count|Total|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Nombre d’événements d’entrée qui n’ont pas pu être désérialisés.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingEarlyInputEvents|Non|Événements d’entrée précoces (préversion)|Count|Total|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Nombre d’événements d’entrée dont l’heure d’application est considérée comme précoce par rapport à l’heure d’arrivée, selon la stratégie d’arrivée précoce.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingInputEventBytes|Non|Octets des événements d’entrée (préversion)|Count|Total|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Quantité de données reçues par le travail de streaming, en octets. Cela permet de valider que les événements sont envoyés à la source d’entrée.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingInputEvents|Non|Événements d’entrée (préversion)|Count|Total|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Nombre d’événements d’entrée.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingInputEventsSourcesPerSecond|Non|Sources d’entrée reçues (préversion)|Count|Total|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Nombre de sources d’événements d’entrée par seconde.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingLateInputEvents|Non|Événements d’entrée tardifs (préversion)|Count|Total|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Nombre d’événements d’entrée dont l’heure d’application est considérée comme tardive par rapport à l’heure d’arrivée, selon la stratégie d’arrivée tardive.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingOutOfOrderEvents|Non|Événements dans le désordre (préversion)|Count|Total|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Nombre d’événements de l’Event Hub (messages sérialisés) reçus par l’adaptateur d’entrée de l’Event Hub, reçus dans le désordre qui ont été supprimés ou dont l’horodatage a été réglé, selon la stratégie de classement des événements.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingOutputEvents|Non|Événements de sortie (préversion)|Count|Total|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Nombre d’événements de sortie.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingOutputWatermarkDelaySeconds|Non|Délai en filigrane (préversion)|Count|Maximale|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Délai en filigrane de la sortie, en secondes.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingResourceUtilization|Non|Utilisation des ressources en pourcentage (préversion)|Pourcentage|Maximale|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest
 Utilisation des ressources exprimée en pourcentage. Une utilisation intensive indique que la tâche atteint une limite proche de la quantité maximale de ressources allouées.|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|
|SQLStreamingRuntimeErrors|Non|Erreurs d’exécution (préversion)|Count|Total|Il s’agit d’une métrique préliminaire disponible dans les régions USA Est et Europe Ouest Nombre total d’erreurs liées au traitement des requêtes (à l’exception des erreurs détectées lors de l’ingestion d’événements ou de la génération de résultats).|SQLPoolName, SQLDatabaseName, JobName, LogicalName, PartitionId, ProcessorInstance|


## <a name="microsoftsynapseworkspacesbigdatapools"></a>Microsoft.Synapse/workspaces/bigDataPools

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BigDataPoolAllocatedCores|Non|vCores alloués|Count|Maximale|VCores alloués pour un pool Apache Spark|SubmitterId|
|BigDataPoolAllocatedMemory|Non|Mémoire allouée (Go)|Count|Maximale|Mémoire allouée au pool Apache Spark (Go)|SubmitterId|
|BigDataPoolApplicationsActive|Non|Applications Apache Spark actives|Count|Maximale|Nombre d’applications actives du pool Apache Spark|JobState|
|BigDataPoolApplicationsEnded|Non|Applications Apache Spark terminées|Nombre|Total|Nombre d’applications de pool Apache Spark terminées|JobType, JobResult|


## <a name="microsoftsynapseworkspacessqlpools"></a>Microsoft.Synapse/workspaces/sqlPools

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActiveQueries|Non|Requêtes actives|Count|Total|Requêtes actives. L’utilisation de cette métrique non filtrée et non fractionnée affiche toutes les requêtes actives en cours d’exécution sur le système|IsUserDefined|
|AdaptiveCacheHitPercent|Non|Pourcentage d’accès au cache adaptatif|Pourcentage|Maximale|Mesure avec quelle efficacité les charges de travail utilisent le cache adaptatif. Utilisez cette métrique avec celle du pourcentage d’accès au cache pour déterminer s’il convient d’effectuer une mise à l’échelle pour une capacité supplémentaire ou de réexécuter les charges de travail pour alimenter le cache|Aucune dimension|
|AdaptiveCacheUsedPercent|Non|Pourcentage d’utilisation du cache adaptatif|Pourcentage|Maximale|Mesure avec quelle efficacité les charges de travail utilisent le cache adaptatif. Utilisez cette métrique avec celle du pourcentage d’utilisation du cache pour déterminer s’il convient d’effectuer une mise à l’échelle pour une capacité supplémentaire ou de réexécuter les charges de travail pour alimenter le cache|Aucune dimension|
|Connexions|Oui|Connexions|Count|Total|Nombre total de connexions au pool SQL|Résultats|
|ConnectionsBlockedByFirewall|Non|Connexions bloquées par le pare-feu|Count|Total|Nombre de connexions bloquées par les règles de pare-feu. Revisitez les stratégies de contrôle d’accès pour votre pool SQL et supervisez ces connexions si le nombre est élevé|Aucune dimension|
|CPUPercent|Non|Pourcentage d’utilisation de l’UC|Pourcentage|Maximale|Utilisation de l’UC sur tous les nœuds du pool SQL|Aucune dimension|
|DWULimit|Non|Limite DWU|Count|Maximale|Objectif de niveau de service du pool SQL|Aucune dimension|
|DWUUsed|Non|DWU utilisé|Count|Maximale|Constitue une représentation générale de l’utilisation dans le pool SQL. Mesurée par Limite DWU * Pourcentage DWU|Aucune dimension|
|DWUUsedPercent|Non|Pourcentage d’utilisation de DWU|Pourcentage|Maximale|Constitue une représentation générale de l’utilisation dans le pool SQL. Mesurée en prenant la valeur maximale entre le pourcentage de processeur et le pourcentage d’E/S des données|Aucune dimension|
|LocalTempDBUsedPercent|Non|Pourcentage utilisé de tempdb locale|Pourcentage|Maximale|Utilisation de la tempdb locale sur tous les nœuds de calcul. Des valeurs sont émises toutes les cinq minutes|Aucune dimension|
|MemoryUsedPercent|Non|Pourcentage de mémoire utilisé|Pourcentage|Maximale|Utilisation de la mémoire sur tous les nœuds du pool SQL|Aucune dimension|
|QueuedQueries|Non|Requêtes mises en file d’attente|Count|Total|Nombre cumulatif de demandes mises en file d’attente une fois la limite de concurrence maximale atteinte|IsUserDefined|
|WLGActiveQueries|Non|Requêtes actives du groupe de charge de travail|Count|Total|Requêtes actives dans le groupe de charges de travail. L’utilisation de cette métrique non filtrée et non fractionnée affiche toutes les requêtes actives en cours d’exécution sur le système|IsUserDefined, WorkloadGroup|
|WLGActiveQueriesTimeouts|Non|Délais d’expiration des requêtes du groupe de charge de travail|Count|Total|Requêtes pour le groupe de charge de travail dont le délai a expiré. Les délais d’expiration des requêtes signalés par cette métrique démarrent uniquement une fois que l’exécution de la requête a commencé (cela n’inclut pas le temps d’attente dû au verrouillage ou à l’attente des ressources)|IsUserDefined, WorkloadGroup|
|WLGAllocationByEffectiveCapResourcePercent|Non|Allocation du groupe de charge de travail par pourcentage maximal de ressources|Pourcentage|Maximale|Affiche l’allocation de pourcentage des ressources par rapport au pourcentage de ressources limitées réelles par groupe de charges de travail. Cette métrique fournit l’utilisation effective du groupe de charges de travail|IsUserDefined, WorkloadGroup|
|WLGAllocationBySystemPercent|Non|Allocation du groupe de charge de travail par pourcentage système|Pourcentage|Maximale|Allocation de pourcentage de ressources par rapport à l’ensemble du système|IsUserDefined, WorkloadGroup|
|WLGEffectiveCapResourcePercent|Non|Pourcentage de ressources limitées réelles|Pourcentage|Maximale|Pourcentage de ressources limitées réelles pour le groupe de charges de travail. S’il existe d’autres groupes de charges de travail avec min_percentage_resource > 0, effective_cap_percentage_resource est abaissé proportionnellement|IsUserDefined, WorkloadGroup|
|WLGEffectiveMinResourcePercent|Non|Pourcentage minimal de ressources réelles|Pourcentage|Maximale|Paramètre de pourcentage minimal de ressources réelles autorisé en tenant compte du niveau de service et des paramètres de groupe de charges de travail. Le min_percentage_resource effectif peut être ajusté à une valeur supérieure sur des niveaux de service inférieurs|IsUserDefined, WorkloadGroup|
|WLGQueuedQueries|Non|Requêtes en file d’attente du groupe de charge de travail|Count|Total|Nombre cumulatif de demandes mises en file d’attente une fois la limite de concurrence maximale atteinte|IsUserDefined, WorkloadGroup|


## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|IngressReceivedBytes|Oui|Octets reçus en entrée|Octets|Total|Nombre d’octets lus à partir de toutes les sources d’événements|Aucune dimension|
|IngressReceivedInvalidMessages|Oui|Messages non valides reçus en entrée|Count|Total|Nombre de messages non valides lus à partir de la totalité des Event Hubs ou des sources d’événements IoT Hub|Aucune dimension|
|IngressReceivedMessages|Oui|Messages reçus en entrée|Count|Total|Nombre de messages lus à partir de la totalité des Event Hubs ou des sources d’événements IoT Hub|Aucune dimension|
|IngressReceivedMessagesCountLag|Oui|Décalage de nombre des messages reçus en entrée|Count|Average|Différence entre le numéro de séquence du dernier message placé en file d’attente dans la partition source de l’événement et le numéro de séquence des messages traités en entrée|Aucune dimension|
|IngressReceivedMessagesTimeLag|Oui|Retard des messages reçus en entrée|Secondes|Maximale|Différence entre l’heure à laquelle le message est placé en file d’attente dans la source d’événement et l’heure à laquelle il est traité en entrée|Aucune dimension|
|IngressStoredBytes|Oui|Octets stockés en entrée|Octets|Total|Taille totale des événements traités correctement et disponibles pour une requête|Aucune dimension|
|IngressStoredEvents|Oui|Événements stockés en entrée|Count|Total|Nombre d’événements aplatis traités correctement et disponibles pour une requête|Aucune dimension|
|WarmStorageMaxProperties|Oui|Propriétés max de stockage chaud|Count|Maximale|Nombre maximal de propriétés utilisées autorisées par l’environnement pour la référence SKU S1/S2 et nombre maximal de propriétés autorisées par le magasin Warm pour la référence SKU PAYG|Aucune dimension|
|WarmStorageUsedProperties|Oui|Propriétés de stockage chaud utilisées |Count|Maximale|Nombre de propriétés utilisées par l’environnement pour la référence SKU S1/S2 et nombre de propriétés utilisées par le magasin Warm pour la référence SKU PAYG|Aucune dimension|


## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|IngressReceivedBytes|Oui|Octets reçus en entrée|Octets|Total|Nombre d’octets lus à partir de la source de l’événement|Aucune dimension|
|IngressReceivedInvalidMessages|Oui|Messages non valides reçus en entrée|Count|Total|Nombre de messages non valides lus à partir de la source de l’événement|Aucune dimension|
|IngressReceivedMessages|Oui|Messages reçus en entrée|Count|Total|Nombre de messages lus à partir de la source de l’événement|Aucune dimension|
|IngressReceivedMessagesCountLag|Oui|Décalage de nombre des messages reçus en entrée|Count|Average|Différence entre le numéro de séquence du dernier message placé en file d’attente dans la partition source de l’événement et le numéro de séquence des messages traités en entrée|Aucune dimension|
|IngressReceivedMessagesTimeLag|Oui|Retard des messages reçus en entrée|Secondes|Maximale|Différence entre l’heure à laquelle le message est placé en file d’attente dans la source d’événement et l’heure à laquelle il est traité en entrée|Aucune dimension|
|IngressStoredBytes|Oui|Octets stockés en entrée|Octets|Total|Taille totale des événements traités correctement et disponibles pour une requête|Aucune dimension|
|IngressStoredEvents|Oui|Événements stockés en entrée|Count|Total|Nombre d’événements aplatis traités correctement et disponibles pour une requête|Aucune dimension|
|WarmStorageMaxProperties|Oui|Propriétés max de stockage chaud|Count|Maximale|Nombre maximal de propriétés utilisées autorisées par l’environnement pour la référence SKU S1/S2 et nombre maximal de propriétés autorisées par le magasin Warm pour la référence SKU PAYG|Aucune dimension|
|WarmStorageUsedProperties|Oui|Propriétés de stockage chaud utilisées |Count|Maximale|Nombre de propriétés utilisées par l’environnement pour la référence SKU S1/S2 et nombre de propriétés utilisées par le magasin Warm pour la référence SKU PAYG|Aucune dimension|


## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft.VMwareCloudSimple/virtualMachines

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Disk Read Bytes|Oui|Disk Read Bytes|Octets|Total|Débit total du disque en raison des opérations de lecture au cours de la période d’échantillonnage.|Aucune dimension|
|Disk Read Operations/Sec|Oui|Disk Read Operations/Sec|CountPerSecond|Average|Nombre moyen d'opérations de lecture (E/S) au cours de la précédente période d’échantillonnage. Notez que ces opérations peuvent varier en taille.|Aucune dimension|
|Disk Write Bytes|Oui|Disk Write Bytes|Octets|Total|Débit total du disque en raison des opérations d'écriture au cours de la période d’échantillonnage.|Aucune dimension|
|Disk Write Operations/Sec|Oui|Disk Write Operations/Sec|CountPerSecond|Average|Nombre moyen d'opérations d'écriture (E/S) au cours de la précédente période d’échantillonnage. Notez que ces opérations peuvent varier en taille.|Aucune dimension|
|DiskReadBytesPerSecond|Oui|Disk Read Bytes/Sec|BytesPerSecond|Average|Débit moyen du disque en raison des opérations de lecture pendant la période d’échantillonnage.|Aucune dimension|
|DiskReadLatency|Oui|Latence de lecture sur disque|Millisecondes|Average|Latence de lecture totale. Somme des latences de lecture sur l'appareil et le noyau.|Aucune dimension|
|DiskReadOperations|Oui|Opérations de lecture sur disque|Count|Total|Nombre d'opérations de lecture (E/S) au cours de la précédente période d’échantillonnage. Notez que ces opérations peuvent varier en taille.|Aucune dimension|
|DiskWriteBytesPerSecond|Oui|Disk Write Bytes/Sec|BytesPerSecond|Average|Débit moyen du disque en raison des opérations d'écriture pendant la période d’échantillonnage.|Aucune dimension|
|DiskWriteLatency|Oui|Latence d'écriture sur disque|Millisecondes|Average|Latence d'écriture totale. Somme des latences d'écriture sur l'appareil et le noyau.|Aucune dimension|
|DiskWriteOperations|Oui|Opérations d'écriture sur disque|Count|Total|Nombre d'opérations d'écriture (E/S) au cours de la précédente période d’échantillonnage. Notez que ces opérations peuvent varier en taille.|Aucune dimension|
|MemoryActive|Oui|Mémoire active|Octets|Average|Quantité de mémoire utilisée par la machine virtuelle dans la petite fenêtre de temps passée. Elle correspond à la quantité de mémoire « vraie » dont la machine virtuelle a actuellement besoin. La mémoire supplémentaire non utilisée peut être échangée ou désallouée sans incidence sur le niveau de performance de l'invité.|Aucune dimension|
|MemoryGranted|Oui|Mémoire allouée|Octets|Average|Quantité de mémoire allouée par l'hôte à la machine virtuelle. La mémoire n’est pas allouée à l’hôte avant un premier accès et la mémoire allouée peut être échangée ou désallouée si le VMkernel a besoin de mémoire.|Aucune dimension|
|MemoryUsed|Oui|Mémoire utilisée|Octets|Average|Quantité de mémoire utilisée par la machine virtuelle.|Aucune dimension|
|Network In|Oui|Network In|Octets|Total|Débit réseau total pour le trafic reçu.|Aucune dimension|
|Network Out|Oui|Network Out|Octets|Total|Débit réseau total pour le trafic transmis.|Aucune dimension|
|NetworkInBytesPerSecond|Oui|Octets entrants réseau/s|BytesPerSecond|Average|Débit réseau moyen pour le trafic reçu.|Aucune dimension|
|NetworkOutBytesPerSecond|Oui|Octets sortants réseau/s|BytesPerSecond|Average|Débit réseau moyen pour le trafic transmis.|Aucune dimension|
|Percentage CPU|Oui|Percentage CPU|Pourcentage|Average|Utilisation du processeur. Cette valeur est signalée avec 100 % représentant tous les cœurs de processeur sur le système. Par exemple, une machine virtuelle bidirectionnelle utilisant 50 % d'un système à quatre cœurs utilise complètement deux cœurs.|Aucune dimension|
|PercentageCpuReady|Oui|Pourcentage de processeurs prêts|Millisecondes|Total|Correspond au temps passé à attendre que les processeurs soient disponibles dans l'intervalle de la dernière mise à jour.|Aucune dimension|


## <a name="microsoftwebconnections"></a>Microsoft.Web/connections

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|Demandes|Non|Demandes|Count|Total|Demandes de connexion à l’API|HttpStatusCode, ClientIPAddress|


## <a name="microsoftwebhostingenvironments"></a>Microsoft.Web/hostingEnvironments

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActiveRequests|Oui|Requêtes actives (déconseillé)|Count|Total|ActiveRequests|Instance|
|AverageResponseTime|Oui|Temps de réponse moyen (déconseillé)|Secondes|Average|AverageResponseTime|Instance|
|BytesReceived|Oui|Données entrantes|Octets|Total|BytesReceived|Instance|
|BytesSent|Oui|Données sortantes|Octets|Total|BytesSent|Instance|
|CpuPercentage|Oui|Pourcentage UC|Pourcentage|Average|CpuPercentage|Instance|
|DiskQueueLength|Oui|Longueur de file d'attente de disque|Count|Average|DiskQueueLength|Instance|
|Http101|Oui|HTTP 101|Count|Total|Http101|Instance|
|Http2xx|Oui|Http 2xx|Count|Total|Http2xx|Instance|
|Http3xx|Oui|Http 3xx|Count|Total|Http3xx|Instance|
|Http401|Oui|Http 401|Count|Total|Http401|Instance|
|Http403|Oui|Http 403|Count|Total|Http403|Instance|
|Http404|Oui|Http 404|Count|Total|Http404|Instance|
|Http406|Oui|Http 406|Count|Total|Http406|Instance|
|Http4xx|Oui|Http 4xx|Count|Total|Http4xx|Instance|
|Http5xx|Oui|Erreurs de serveur http|Count|Total|Http5xx|Instance|
|HttpQueueLength|Oui|Longueur de la file d’attente HTTP|Count|Average|HttpQueueLength|Instance|
|HttpResponseTime|Oui|Temps de réponse|Secondes|Average|HttpResponseTime|Instance|
|LargeAppServicePlanInstances|Oui|Workers de plan App Service de grande taille|Count|Average|Workers de plan App Service de grande taille|Aucune dimension|
|MediumAppServicePlanInstances|Oui|Workers de plan App Service de taille moyenne|Count|Average|Workers de plan App Service de taille moyenne|Aucune dimension|
|MemoryPercentage|Oui|Pourcentage de mémoire|Pourcentage|Average|MemoryPercentage|Instance|
|Demandes|Oui|Demandes|Count|Total|Demandes|Instance|
|SmallAppServicePlanInstances|Oui|Workers de plan App Service de petite taille|Count|Average|Workers de plan App Service de petite taille|Aucune dimension|
|TotalFrontEnds|Oui|Nombre total de serveurs frontaux|Count|Average|Nombre total de serveurs frontaux|Aucune dimension|


## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|ActiveRequests|Oui|Requêtes actives (déconseillé)|Count|Total|Requêtes actives|Instance|
|AverageResponseTime|Oui|Temps de réponse moyen (déconseillé)|Secondes|Average|Temps moyen nécessaire au front-end pour répondre aux demandes, en secondes.|Instance|
|BytesReceived|Oui|Données entrantes|Octets|Total|Bande passante entrante moyenne utilisée sur tous les front-ends, en Mio.|Instance|
|BytesSent|Oui|Données sortantes|Octets|Total|Bande passante entrante moyenne utilisée sur tous les front-ends, en Mio.|Instance|
|CpuPercentage|Oui|Pourcentage UC|Pourcentage|Average|Processeur moyen utilisé sur toutes les instances du front-end.|Instance|
|DiskQueueLength|Oui|Longueur de file d'attente de disque|Count|Average|Nombre moyen de requêtes de lecture et d’écriture mises en file d’attente sur le stockage. Une longueur de file d’attente de disque élevée est une indication d’une application susceptible d’être ralentie en raison d’un nombre d’E/S de disque excessif.|Instance|
|Http101|Oui|HTTP 101|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 101.|Instance|
|Http2xx|Oui|Http 2xx|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 200, mais < 300.|Instance|
|Http3xx|Oui|Http 3xx|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 300, mais < 400.|Instance|
|Http401|Oui|Http 401|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 401.|Instance|
|Http403|Oui|Http 403|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 403.|Instance|
|Http404|Oui|Http 404|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 404.|Instance|
|Http406|Oui|Http 406|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 406.|Instance|
|Http4xx|Oui|Http 4xx|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 400, mais < 500.|Instance|
|Http5xx|Oui|Erreurs de serveur http|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 500, mais < 600.|Instance|
|HttpQueueLength|Oui|Longueur de la file d’attente HTTP|Count|Average|Nombre moyen de requêtes HTTP qui devaient se trouver dans la file d’attente avant d’être exécutées. Une longueur de file d’attente HTTP élevée ou croissante est le symptôme d’un plan surchargé.|Instance|
|HttpResponseTime|Oui|Temps de réponse|Secondes|Average|Temps nécessaire au front-end pour traiter les demandes, en secondes.|Instance|
|LargeAppServicePlanInstances|Oui|Workers de plan App Service de grande taille|Count|Average|Workers de plan App Service de grande taille|Aucune dimension|
|MediumAppServicePlanInstances|Oui|Workers de plan App Service de taille moyenne|Count|Average|Workers de plan App Service de taille moyenne|Aucune dimension|
|MemoryPercentage|Oui|Pourcentage de mémoire|Pourcentage|Average|Mémoire moyenne utilisée sur toutes les instances du front-end.|Instance|
|Demandes|Oui|Demandes|Count|Total|Nombre total de requêtes, quel que soit leur code d’état HTTP résultant.|Instance|
|SmallAppServicePlanInstances|Oui|Workers de plan App Service de petite taille|Count|Average|Workers de plan App Service de petite taille|Aucune dimension|
|TotalFrontEnds|Oui|Nombre total de serveurs frontaux|Count|Average|Nombre total de serveurs frontaux|Aucune dimension|


## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|CpuPercentage|Oui|Pourcentage UC|Pourcentage|Average|Processeur moyen utilisé sur toutes les instances du pool de workers.|Instance|
|MemoryPercentage|Oui|Pourcentage de mémoire|Pourcentage|Average|Mémoire moyenne utilisée sur toutes les instances du pool de workers.|Instance|
|WorkersAvailable|Oui|Workers disponibles|Count|Average|Workers disponibles|Aucune dimension|
|WorkersTotal|Oui|Nombre total de workers|Count|Average|Nombre total de workers|Aucune dimension|
|WorkersUsed|Oui|Workers utilisés|Count|Average|Workers utilisés|Aucune dimension|


## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BytesReceived|Oui|Données entrantes|Octets|Total|Utilisation moyenne de la bande passante entrante dans toutes les instances du plan.|Instance|
|BytesSent|Oui|Données sortantes|Octets|Total|Utilisation moyenne de la bande passante sortante dans toutes les instances du plan.|Instance|
|CpuPercentage|Oui|Pourcentage UC|Pourcentage|Average|Utilisation moyenne de l’UC dans toutes les instances du plan.|Instance|
|DiskQueueLength|Oui|Longueur de file d'attente de disque|Count|Average|Nombre moyen de requêtes de lecture et d’écriture mises en file d’attente sur le stockage. Une longueur de file d’attente de disque élevée est une indication d’une application susceptible d’être ralentie en raison d’un nombre d’E/S de disque excessif.|Instance|
|HttpQueueLength|Oui|Longueur de la file d’attente HTTP|Count|Average|Nombre moyen de requêtes HTTP qui devaient se trouver dans la file d’attente avant d’être exécutées. Une longueur de file d’attente HTTP élevée ou croissante est le symptôme d’un plan surchargé.|Instance|
|MemoryPercentage|Oui|Pourcentage de mémoire|Pourcentage|Average|Utilisation moyenne de la mémoire dans toutes les instances du plan.|Instance|
|SocketInboundAll|Oui|Nombre de sockets pour les requêtes entrantes|Count|Average|Nombre moyen de sockets utilisés pour les requêtes HTTP entrantes dans toutes les instances du plan.|Instance|
|SocketLoopback|Oui|Nombre de sockets pour les connexions de bouclage|Count|Average|Nombre moyen de sockets utilisés pour les connexions de bouclage dans toutes les instances du plan.|Instance|
|SocketOutboundAll|Oui|Nombre de sockets pour les requêtes sortantes|Count|Average|Nombre moyen de sockets utilisés pour les connections sortantes dans toutes les instances du plan, quel que soit leur état TCP. Un trop grand nombre de connexions sortantes peut entraîner des erreurs de connectivité.|Instance|
|SocketOutboundEstablished|Oui|Nombre de sockets établis pour les requêtes sortantes|Count|Average|Nombre moyen de sockets à l’état ESTABLISHED utilisés pour les connexions sortantes dans toutes les instances du plan.|Instance|
|SocketOutboundTimeWait|Oui|Nombre de sockets Time Wait pour les requêtes sortantes|Count|Average|Nombre moyen de sockets à l’état TIME_WAIT utilisés pour les connexions sortantes dans toutes les instances du plan. Un nombre élevé ou croissant de sockets sortants à l’état TIME_WAIT peut entraîner des erreurs de connectivité.|Instance|
|TcpCloseWait|Oui|Close Wait - TCP|Count|Average|Nombre moyen de sockets à l’état CLOSE_WAIT dans toutes les instances du plan.|Instance|
|TcpClosing|Oui|Closing - TCP|Count|Average|Nombre moyen de sockets à l’état CLOSING dans toutes les instances du plan.|Instance|
|TcpEstablished|Oui|Established - TCP|Count|Average|Nombre moyen de sockets à l’état ESTABLISHED dans toutes les instances du plan.|Instance|
|TcpFinWait1|Oui|Fin Wait 1 - TCP|Count|Average|Nombre moyen de sockets à l’état FIN_WAIT_1 dans toutes les instances du plan.|Instance|
|TcpFinWait2|Oui|Fin Wait 2 - TCP|Count|Average|Nombre moyen de sockets à l’état FIN_WAIT_2 dans toutes les instances du plan.|Instance|
|TcpLastAck|Oui|Last Ack - TCP|Count|Average|Nombre moyen de sockets à l’état LAST_ACK dans toutes les instances du plan.|Instance|
|TcpSynReceived|Oui|Syn Received - TCP|Count|Average|Nombre moyen de sockets à l’état SYN_RCVD dans toutes les instances du plan.|Instance|
|TcpSynSent|Oui|Syn Sent - TCP|Count|Average|Nombre moyen de sockets à l’état SYN_SENT dans toutes les instances du plan.|Instance|
|TcpTimeWait|Oui|Time Wait - TCP|Count|Average|Nombre moyen de sockets à l’état TIME_WAIT dans toutes les instances du plan.|Instance|


## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AppConnections|Oui|Connexions|Count|Average|Nombre de sockets liés existants dans le bac à sable (w3wp.exe et ses processus enfants). Un socket lié est créé en appelant les API bind()/connect(), et persiste jusqu’à être fermé avec CloseHandle()/closesocket(). Pour WebApps et FunctionApps.|Instance|
|AverageMemoryWorkingSet|Oui|Plage de travail moyenne de la mémoire|Octets|Average|Quantité moyenne de mémoire, en mégaoctets (Mio), utilisée par l’application. Pour WebApps et FunctionApps.|Instance|
|AverageResponseTime|Oui|Temps de réponse moyen (déconseillé)|Secondes|Average|Temps moyen, en secondes, nécessaire à l’application pour traiter les requêtes. Pour WebApps et FunctionApps.|Instance|
|BytesReceived|Oui|Données entrantes|Octets|Total|Quantité de bande passante entrante, en Mio, consommée par l’application. Pour WebApps et FunctionApps.|Instance|
|BytesSent|Oui|Données sortantes|Octets|Total|Quantité de bande passante sortante, en Mio, consommée par l’application. Pour WebApps et FunctionApps.|Instance|
|CpuTime|Oui|Temps processeur|Secondes|Total|Temps processeur, en secondes, consommée par l’application. Pour plus d’informations sur cette métrique, Voir https://aka.ms/website-monitor-cpu-time-vs-cpu-percentage (Temps processeur et pourcentage UC). Pour WebApps uniquement.|Instance|
|CurrentAssemblies|Oui|Assemblys actuels|Count|Average|Nombre d’assemblys actuellement chargés dans tous les AppDomains dans cette application. Pour WebApps et FunctionApps.|Instance|
|FileSystemUsage|Oui|Utilisation de systèmes de fichiers|Octets|Average|Pourcentage du quota de systèmes de fichiers consommé par l’application. Pour WebApps et FunctionApps.|Aucune dimension|
|FunctionExecutionCount|Oui|Nombre d’exécutions de fonctions|Count|Total|Nombre d’exécutions de fonctions. Pour FunctionApps uniquement.|Instance|
|FunctionExecutionUnits|Oui|Unités d’exécution de fonctions|Count|Total|Unités d’exécution de fonctions. Pour FunctionApps uniquement.|Instance|
|Gen0Collections|Oui|Garbage collections de génération 0|Count|Total|Nombre de fois que les objets de génération 0 ont été récupérés par le Garbage Collector depuis le début du processus d’application. Les garbage collections de génération supérieure comprennent toutes celles de génération inférieure. Pour WebApps et FunctionApps.|Instance|
|Gen1Collections|Oui|Garbage collections de génération 1|Count|Total|Nombre de fois que les objets de génération 1 ont été récupérés par le Garbage Collector depuis le début du processus d’application. Les garbage collections de génération supérieure comprennent toutes celles de génération inférieure. Pour WebApps et FunctionApps.|Instance|
|Gen2Collections|Oui|Garbage collections de génération 2|Count|Total|Nombre de fois que les objets de génération 2 ont été récupérés par le Garbage Collector depuis le début du processus d’application. Pour WebApps et FunctionApps.|Instance|
|Poignées|Oui|Nombre de descripteurs|Count|Average|Nombre total de handles actuellement ouverts par le processus d’application. Pour WebApps et FunctionApps.|Instance|
|HealthCheckStatus|Oui|État de contrôle d’intégrité|Count|Average|État du contrôle d’intégrité. Pour WebApps et FunctionApps.|Instance|
|Http101|Oui|HTTP 101|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 101. Pour WebApps et FunctionApps.|Instance|
|Http2xx|Oui|Http 2xx|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 200, mais < 300. Pour WebApps et FunctionApps.|Instance|
|Http3xx|Oui|Http 3xx|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 300, mais < 400. Pour WebApps et FunctionApps.|Instance|
|Http401|Oui|Http 401|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 401. Pour WebApps et FunctionApps.|Instance|
|Http403|Oui|Http 403|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 403. Pour WebApps et FunctionApps.|Instance|
|Http404|Oui|Http 404|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 404. Pour WebApps et FunctionApps.|Instance|
|Http406|Oui|Http 406|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 406. Pour WebApps et FunctionApps.|Instance|
|Http4xx|Oui|Http 4xx|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 400, mais < 500. Pour WebApps et FunctionApps.|Instance|
|Http5xx|Oui|Erreurs de serveur http|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 500, mais < 600. Pour WebApps et FunctionApps.|Instance|
|HttpResponseTime|Oui|Temps de réponse|Secondes|Average|Temps nécessaire à l’application pour traiter les requêtes (en secondes). Pour WebApps et FunctionApps.|Instance|
|IoOtherBytesPerSecond|Oui|Autres octets par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application émet des octets pour les opérations d’E/S qui n’impliquent pas de données, telles que les opérations de contrôle. Pour WebApps et FunctionApps.|Instance|
|IoOtherOperationsPerSecond|Oui|Autres opérations par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application émet des opérations d’E/S qui ne sont ni des opérations de lecture, ni des opérations d’écriture. Pour WebApps et FunctionApps.|Instance|
|IoReadBytesPerSecond|Oui|Octets lus par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application lit les octets à partir des opérations d’E/S. Pour WebApps et FunctionApps.|Instance|
|IoReadOperationsPerSecond|Oui|Opérations de lecture par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application émet des opérations d’E/S de lecture. Pour WebApps et FunctionApps.|Instance|
|IoWriteBytesPerSecond|Oui|Octets écrits par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application écrit des octets dans des opérations d’E/S. Pour WebApps et FunctionApps.|Instance|
|IoWriteOperationsPerSecond|Oui|Opérations d’écriture par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application émet des opérations d’E/S d’écriture. Pour WebApps et FunctionApps.|Instance|
|MemoryWorkingSet|Oui|Plage de travail de la mémoire|Octets|Average|Quantité actuelle de mémoire, en Mio, utilisée par l’application. Pour WebApps et FunctionApps.|Instance|
|PrivateBytes|Oui|Octets privés|Octets|Average|Taille actuelle (en octets) de mémoire allouée par le processus d’application qui ne peut pas être partagée avec d’autres processus. Pour WebApps et FunctionApps.|Instance|
|Demandes|Oui|Demandes|Count|Total|Nombre total de requêtes, quel que soit leur code d’état HTTP résultant. Pour WebApps et FunctionApps.|Instance|
|RequestsInApplicationQueue|Oui|Demandes dans la file d’attente d’application|Count|Average|Nombre de requêtes dans la file d’attente de requêtes de l’application. Pour WebApps et FunctionApps.|Instance|
|Threads|Oui|Nombre de threads|Count|Average|Nombre de threads actuellement actifs dans le processus d’application. Pour WebApps et FunctionApps.|Instance|
|TotalAppDomains|Oui|Total des domaines d’application|Count|Average|Nombre actuel de domaines d’application chargés dans cette application. Pour WebApps et FunctionApps.|Instance|
|TotalAppDomainsUnloaded|Oui|Total des domaines d’application déchargés|Count|Average|Nombre total de domaines d’application déchargés depuis le démarrage de l’application. Pour WebApps et FunctionApps.|Instance|


## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|AppConnections|Oui|Connexions|Count|Average|Nombre de sockets liés existants dans le bac à sable (w3wp.exe et ses processus enfants). Un socket lié est créé en appelant les API bind()/connect(), et persiste jusqu’à être fermé avec CloseHandle()/closesocket().|Instance|
|AverageMemoryWorkingSet|Oui|Plage de travail moyenne de la mémoire|Octets|Average|Quantité moyenne de mémoire, en mégaoctets (Mio), utilisée par l’application.|Instance|
|AverageResponseTime|Oui|Temps de réponse moyen (déconseillé)|Secondes|Average|Temps moyen, en secondes, nécessaire à l’application pour traiter les requêtes.|Instance|
|BytesReceived|Oui|Données entrantes|Octets|Total|Quantité de bande passante entrante, en Mio, consommée par l’application.|Instance|
|BytesSent|Oui|Données sortantes|Octets|Total|Quantité de bande passante sortante, en Mio, consommée par l’application.|Instance|
|CpuTime|Oui|Temps processeur|Secondes|Total|Temps processeur, en secondes, consommée par l’application. Pour plus d’informations sur cette métrique, Voir https://aka.ms/website-monitor-cpu-time-vs-cpu-percentage (Temps processeur et pourcentage UC).|Instance|
|CurrentAssemblies|Oui|Assemblys actuels|Count|Average|Nombre d’assemblys actuellement chargés dans tous les AppDomains dans cette application.|Instance|
|FileSystemUsage|Oui|Utilisation de systèmes de fichiers|Octets|Average|Pourcentage du quota de systèmes de fichiers consommé par l’application.|Aucune dimension|
|FunctionExecutionCount|Oui|Nombre d’exécutions de fonctions|Count|Total|Nombre d’exécutions de fonctions|Instance|
|FunctionExecutionUnits|Oui|Unités d’exécution de fonctions|Count|Total|Unités d’exécution de fonctions|Instance|
|Gen0Collections|Oui|Garbage collections de génération 0|Count|Total|Nombre de fois que les objets de génération 0 ont été récupérés par le Garbage Collector depuis le début du processus d’application. Les garbage collections de génération supérieure comprennent toutes celles de génération inférieure.|Instance|
|Gen1Collections|Oui|Garbage collections de génération 1|Count|Total|Nombre de fois que les objets de génération 1 ont été récupérés par le Garbage Collector depuis le début du processus d’application. Les garbage collections de génération supérieure comprennent toutes celles de génération inférieure.|Instance|
|Gen2Collections|Oui|Garbage collections de génération 2|Count|Total|Nombre de fois que les objets de génération 2 ont été récupérés par le Garbage Collector depuis le début du processus d’application.|Instance|
|Poignées|Oui|Nombre de descripteurs|Count|Average|Nombre total de handles actuellement ouverts par le processus d’application.|Instance|
|HealthCheckStatus|Oui|État de contrôle d’intégrité|Count|Average|État de contrôle d’intégrité|Instance|
|Http101|Oui|HTTP 101|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 101.|Instance|
|Http2xx|Oui|Http 2xx|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 200, mais < 300.|Instance|
|Http3xx|Oui|Http 3xx|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 300, mais < 400.|Instance|
|Http401|Oui|Http 401|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 401.|Instance|
|Http403|Oui|Http 403|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 403.|Instance|
|Http404|Oui|Http 404|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 404.|Instance|
|Http406|Oui|Http 406|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP 406.|Instance|
|Http4xx|Oui|Http 4xx|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 400, mais < 500.|Instance|
|Http5xx|Oui|Erreurs de serveur http|Count|Total|Nombre de requêtes donnant lieu à un code d’état HTTP = 500, mais < 600.|Instance|
|HttpResponseTime|Oui|Temps de réponse|Secondes|Average|Temps nécessaire à l’application pour traiter les requêtes (en secondes).|Instance|
|IoOtherBytesPerSecond|Oui|Autres octets par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application émet des octets pour les opérations d’E/S qui n’impliquent pas de données, telles que les opérations de contrôle.|Instance|
|IoOtherOperationsPerSecond|Oui|Autres opérations par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application émet des opérations d’E/S qui ne sont ni des opérations de lecture, ni des opérations d’écriture.|Instance|
|IoReadBytesPerSecond|Oui|Octets lus par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application lit les octets à partir des opérations d’E/S.|Instance|
|IoReadOperationsPerSecond|Oui|Opérations de lecture par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application émet des opérations d’E/S de lecture.|Instance|
|IoWriteBytesPerSecond|Oui|Octets écrits par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application écrit des octets dans des opérations d’E/S.|Instance|
|IoWriteOperationsPerSecond|Oui|Opérations d’écriture par seconde (E/S)|BytesPerSecond|Total|Débit auquel le processus d’application émet des opérations d’E/S d’écriture.|Instance|
|MemoryWorkingSet|Oui|Plage de travail de la mémoire|Octets|Average|Quantité actuelle de mémoire, en Mio, utilisée par l’application.|Instance|
|PrivateBytes|Oui|Octets privés|Octets|Average|Taille actuelle (en octets) de mémoire allouée par le processus d’application qui ne peut pas être partagée avec d’autres processus.|Instance|
|Demandes|Oui|Demandes|Count|Total|Nombre total de requêtes, quel que soit leur code d’état HTTP résultant.|Instance|
|RequestsInApplicationQueue|Oui|Demandes dans la file d’attente d’application|Count|Average|Nombre de requêtes dans la file d’attente de requêtes de l’application.|Instance|
|Threads|Oui|Nombre de threads|Count|Average|Nombre de threads actuellement actifs dans le processus d’application.|Instance|
|TotalAppDomains|Oui|Total des domaines d’application|Count|Average|Nombre actuel de domaines d’application chargés dans cette application.|Instance|
|TotalAppDomainsUnloaded|Oui|Total des domaines d’application déchargés|Count|Average|Nombre total de domaines d’application déchargés depuis le démarrage de l’application.|Instance|


## <a name="microsoftwebstaticsites"></a>Microsoft.Web/staticSites

|Métrique|Exportable par le biais des paramètres de diagnostic ?|Nom d’affichage de la métrique|Unité|Type d’agrégation|Description|Dimensions|
|---|---|---|---|---|---|---|
|BytesSent|Oui|Données sortantes|Octets|Total|BytesSent|Instance|
|FunctionErrors|Oui|FunctionErrors|Count|Total|FunctionErrors|Instance|
|FunctionHits|Oui|FunctionHits|Count|Total|FunctionHits|Instance|
|SiteErrors|Oui|SiteErrors|Count|Total|SiteErrors|Instance|
|SiteHits|Oui|SiteHits|Count|Total|SiteHits|Instance|

## <a name="next-steps"></a>Étapes suivantes

- [En savoir plus sur les métriques dans Azure Monitor](../data-platform.md)
- [Créer des alertes sur les métriques](../alerts/alerts-overview.md)
- [Exporter des métriques vers le stockage, un hub d’événements ou Log Analytics](../essentials/platform-logs-overview.md)
