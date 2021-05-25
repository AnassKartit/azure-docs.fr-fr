---
title: Fenêtre de maintenance
description: Découvrez comment configurer la fenêtre de maintenance Azure SQL Database et Managed Instance.
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.topic: conceptual
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.reviewer: sstein
ms.custom: references_regions
ms.date: 04/28/2021
ms.openlocfilehash: ab3da3ba8764ced53f3dcd936d56a24e73cfd8a2
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2021
ms.locfileid: "108736448"
---
# <a name="maintenance-window-preview"></a>Fenêtre de maintenance (préversion)
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

La fonctionnalité de fenêtre de maintenance vous permet de configurer la planification de la maintenance pour les ressources [Azure SQL Database](sql-database-paas-overview.md) et [Azure SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md), ce qui rend les événements de maintenance à fort impact prévisibles et moins disruptifs pour votre charge de travail. 

> [!Note]
> La fonctionnalité de fenêtre de maintenance protège uniquement de l’impact planifié des mises à niveau ou de la maintenance planifiée. Elle ne protège pas de toutes les causes de basculement ; les exceptions qui peuvent provoquer de brèves interruptions de connexion en dehors d’une fenêtre de maintenance incluent les défaillances matérielles, l’équilibrage de charge de cluster et les reconfigurations de bases de données dues à des événements tels qu’une modification de l’objectif de niveau de service de base de données. 

## <a name="overview"></a>Vue d’ensemble

Azure effectue régulièrement une [maintenance planifiée](planned-maintenance.md) pour les ressources SQL Database et SQL Managed Instance. Pendant l'événement de maintenance Azure SQL, les bases de données restent entièrement disponibles, mais peuvent faire l'objet de reconfigurations rapides dans le cadre de contrats SLA de disponibilité [SQL Database](https://azure.microsoft.com/support/legal/sla/sql-database) et [SQL Managed Instance](https://azure.microsoft.com/support/legal/sla/azure-sql-sql-managed-instance).

La fenêtre de maintenance est destinée aux charges de travail de production qui ne sont pas résilientes aux reconfigurations de base de données ou d'instance, et qui ne peuvent pas tolérer les interruptions de connexion de courte durée qui sont provoquées par les événements de maintenance planifiée. En choisissant une fenêtre de maintenance qui vous convient, vous pouvez réduire l’impact de la maintenance planifiée, car elle se produira en dehors des heures de pointe de votre entreprise. Les charges de travail résilientes et les charges de travail hors production peuvent reposer sur la stratégie de maintenance par défaut d’Azure SQL.

La fenêtre de maintenance peut être configurée pour des ressources Azure SQL nouvelles ou existantes. Elle peut être configurée à l’aide du portail Azure, de PowerShell, de l’interface CLI ou de l’API Azure.

> [!Important]
> La configuration de la fenêtre de maintenance est une opération asynchrone durable, similaire à la modification du niveau de service de la ressource Azure SQL. La ressource est disponible pendant l’opération, à l’exception d’une reconfiguration rapide qui se produit à la fin de l’opération et qui dure généralement jusqu’à huit secondes, même en cas de transactions durables interrompues. Pour réduire l'impact de la reconfiguration, vous devez effectuer l'opération en dehors des heures de pointe.

### <a name="gain-more-predictability-with-maintenance-window"></a>Obtenir davantage de prévisibilité avec une fenêtre de maintenance

Par défaut, la stratégie de maintenance Azure SQL bloque les mises à jour à fort impact de **8 h 00 à 17 h 00 tous les jours (heure locale)** pour éviter toute interruption pendant les heures de pointe habituelles. L’heure locale est déterminée par la localisation de la [région Azure](https://azure.microsoft.com/global-infrastructure/geographies/) qui héberge la ressource et peut respecter l’heure d’été en fonction du fuseau horaire local. 

Pour les mises à jour de maintenance, vous pouvez choisir une heure adaptée à vos ressources Azure SQL en choisissant parmi deux créneaux de fenêtres de maintenance supplémentaires :
 
* En semaine, de 22 h 00 à 6 h 00 heure locale, du lundi au jeudi
* Fenêtre week-end, de 22h00 à 6h00 heure locale, du vendredi au dimanche

Une fois que la fenêtre de maintenance est sélectionnée et que la configuration du service est terminée, les maintenances planifiées auront lieu uniquement pendant la fenêtre de maintenance de votre choix.   

> [!Important]
> Dans de rares cas où un ajournement d’action pourrait avoir un impact sérieux, comme l’application d’un correctif de sécurité critique, la fenêtre de maintenance configurée peut être temporairement remplacée. 

### <a name="cost-and-eligibility"></a>Coût et éligibilité

La configuration et l’utilisation d’une fenêtre de maintenance sont gratuites pour les [types d’offres](https://azure.microsoft.com/support/legal/offer-details/) éligibles : Paiement à l’utilisation, Fournisseur de solutions Cloud (CSP), Contrat Entreprise Microsoft ou Contrat client Microsoft.

> [!Note]
> Une offre Azure correspond au type d’abonnement Azure que vous avez. Par exemple, un abonnement avec [tarifs de paiement à l’utilisation](https://azure.microsoft.com/offers/ms-azr-0003p/), [Azure en licence Open](https://azure.microsoft.com/offers/ms-azr-0111p/) et [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p/) sont tous des offres Azure. Chaque offre ou plan présente différentes conditions et avantages. Votre offre ou plan est affiché dans la vue d’ensemble de l’abonnement. Pour plus d’informations sur la manière de changer votre abonnement et basculer vers une autre offre, consultez [Changer d’offre pour votre abonnement Azure](../../cost-management-billing/manage/switch-azure-offer.md).

## <a name="advance-notifications"></a>Notifications préalables

Vous pouvez configurer des notifications de maintenance afin d’être averti des prochains événements de maintenance planifiée Azure SQL Database 24 heures à l’avance, au moment de la maintenance et lorsque celle-ci est terminée. Pour plus d’informations, consultez [Notifications préalables](advance-notifications.md).

## <a name="availability"></a>Disponibilité

### <a name="supported-service-level-objectives"></a>Objectifs de niveau de service pris en charge

Le choix d’une fenêtre de maintenance autre que celle par défaut est disponible sur tous les objectifs de niveau de service **sauf** :
* Hyperscale 
* Pools d’instances
* Gen4 vCore hérité
* De base, S0 et S1 
* DC, Fsv2, série M

### <a name="azure-region-support"></a>Prise en charge des régions Azure

Le choix d’une fenêtre de maintenance autre que celle par défaut est actuellement disponible dans les régions suivantes :

- Australie Est
- Australie Sud-Est
- Brésil Sud
- Centre du Canada
- USA Centre
- USA Est
- USA Est 2
- Asie Est
- Japon Est
- NorthCentral US
- Europe Nord
- USA Centre Sud
- Asie Sud-Est
- Sud du Royaume-Uni
- Europe Ouest
- USA Ouest
- USA Ouest 2

## <a name="gateway-maintenance-for-azure-sql-database"></a>Maintenance de la passerelle pour Azure SQL Database

Pour tirer le meilleur parti des fenêtres de maintenance, vérifiez que vos applications clientes utilisent la stratégie de connexion de redirection. La redirection est la stratégie de connexion recommandée ; les clients établissent des connexions directement au nœud qui héberge la base de données, ce qui permet de réduire la latence et d’améliorer le débit.  

* Dans Azure SQL Database, toutes les connexions utilisant la stratégie de connexion proxy peuvent être affectées par la fenêtre de maintenance choisie et par une fenêtre de maintenance de nœud de passerelle. Toutefois, les connexions client utilisant la stratégie de connexion par redirection recommandée ne sont pas affectées par une reconfiguration de la maintenance du nœud de passerelle. 

* Dans Azure SQL Managed Instance, les nœuds de passerelle sont hébergés [au sein du cluster virtuel](../../azure-sql/managed-instance/connectivity-architecture-overview.md#virtual-cluster-connectivity-architecture) et ont la même fenêtre de maintenance que l'instance managée. Toutefois, il est toujours recommandé d'utiliser la stratégie de connexion de redirection afin de réduire le nombre de perturbations pendant l'événement de maintenance.

Pour plus d’informations sur la stratégie de connexion client dans Azure SQL Database, consultez [Stratégie de connexion Azure SQL Database](../database/connectivity-architecture.md#connection-policy). 

Pour plus d'informations sur la stratégie de connexion client dans Azure SQL Managed Instance, consultez [Types de connexion Azure SQL Managed Instance](../../azure-sql/managed-instance/connection-types-overview.md).

## <a name="considerations-for-azure-sql-managed-instance"></a>Considérations relatives à Azure SQL Managed Instance

Azure SQL Managed Instance est un ensemble de composants de service hébergés sur un groupe dédié de machines virtuelles isolées qui s'exécutent dans le sous-réseau virtuel du client. Ces machines virtuelles forment [un ou plusieurs clusters virtuels](../managed-instance/connectivity-architecture-overview.md#high-level-connectivity-architecture) qui peuvent héberger plusieurs instances managées. La fenêtre de maintenance qui est configurée sur les instances d’un sous-réseau peut influencer le nombre de clusters virtuels présents dans le sous-réseau, ainsi que la distribution des instances entre les clusters virtuels. Vous devrez alors prendre en compte plusieurs choses.

### <a name="maintenance-window-configuration-is-long-running-operation"></a>La configuration de la fenêtre de maintenance est une opération longue 
Toutes les instances qui sont hébergées dans un cluster virtuel partagent la même fenêtre de maintenance. Par défaut, toutes les instances managées sont hébergées dans le cluster virtuel avec la fenêtre de maintenance par défaut. Si vous spécifiez une autre fenêtre de maintenance pour l’instance managée lors de sa création ou après celle-ci, cela signifie qu’elle doit être placée dans le cluster virtuel avec la fenêtre de maintenance correspondante. S’il n’existe pas de cluster virtuel de ce type dans le sous-réseau, vous devez d’abord en créer un pour l’instance. L’ajout d’une instance supplémentaire dans un cluster virtuel existant peut nécessiter le redimensionnement de ce cluster. Les deux opérations contribuent à la durée de configuration de la fenêtre de maintenance d’une instance managée.
La durée prévue de la configuration de la fenêtre de maintenance d’une instance managée peut être calculée à l’aide de la [durée estimée des opérations de gestion des instances](../managed-instance/management-operations-overview.md#duration).

> [!Important]
> Une reconfiguration rapide se produit à la fin de l'opération ; celle-ci dure généralement moins de 8 secondes, même en cas de transactions durables interrompues. Pour réduire l'impact de la reconfiguration, vous devez planifier l'opération en dehors des heures de pointe.

### <a name="ip-address-space-requirements"></a>Conditions requises des espaces d’adressage IP
Chaque nouveau cluster virtuel du sous-réseau nécessite des adresses IP supplémentaires en fonction de l’[allocation des adresses IP du cluster virtuel](../managed-instance/vnet-subnet-determine-size.md#determine-subnet-size). Si vous modifiez la fenêtre de maintenance d’une instance managée existante, une [capacité IP supplémentaire temporaire](../managed-instance/vnet-subnet-determine-size.md#address-requirements-for-update-scenarios) sera également nécessaire, comme dans le scénario de mise à l’échelle de vCores pour le niveau de service correspondant.

### <a name="ip-address-change"></a>Modification d’adresse IP
La configuration et la modification de la fenêtre de maintenance entraînent la modification de l’adresse IP de l’instance dans la plage d’adresses IP du sous-réseau.

> [!Important]
>  Vérifiez que le groupe de sécurité réseau et les règles de pare-feu ne bloquent pas le trafic des données après la modification. 

## <a name="next-steps"></a>Étapes suivantes

* [Notifications préalables](advance-notifications.md)
* [Configurer la fenêtre de maintenance](maintenance-window-configure.md)

## <a name="learn-more"></a>En savoir plus

* [FAQ sur la fenêtre de maintenance](maintenance-window-faq.yml)
* [Azure SQL Database](sql-database-paas-overview.md) 
* [SQL Managed Instance](../managed-instance/sql-managed-instance-paas-overview.md)
* [Planifier les événements de maintenance Azure dans Azure SQL Database et Azure SQL Managed Instance](planned-maintenance.md)
