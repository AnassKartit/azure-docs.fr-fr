---
title: Migrer vers le Moniteur de connexion à partir du Moniteur de connexion
titleSuffix: Azure Network Watcher
description: Découvrez comment migrer vers le Moniteur de connexion à partir du Moniteur de connexion.
services: network-watcher
documentationcenter: na
author: vinynigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: vinigam
ms.openlocfilehash: c9130b83d0d4491152e05157c9cb52dfb89231a1
ms.sourcegitcommit: 98308c4b775a049a4a035ccf60c8b163f86f04ca
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/30/2021
ms.locfileid: "113105738"
---
# <a name="migrate-to-connection-monitor-from-connection-monitor-classic"></a>Migrer vers le Moniteur de connexion à partir du Moniteur de connexion (classique)

> [!IMPORTANT]
> À compter du 1er juillet 2021, vous ne pourrez plus ajouter de nouveaux moniteurs de connexion dans le Moniteur de connexion (classique), mais vous pourrez continuer à utiliser les moniteurs de connexion déjà créés avant le 1er juillet 2021. Pour réduire l’interruption de service de vos charges de travail actuelles, [migrez du Moniteur de connexion (classique) vers le nouveau Moniteur de connexion](migrate-to-connection-monitor-from-connection-monitor-classic.md) dans Azure Network Watcher avant le 29 février 2024.

En quelques clics et sans temps d’arrêt, vous pouvez migrer des moniteurs de connexion vers la nouvelle fonctionnalité Moniteur de connexion améliorée. Pour en savoir plus sur les avantages, consultez [Moniteur de connexion](./connection-monitor-overview.md).

## <a name="key-points-to-note"></a>Points clés à noter

La migration produit les résultats suivants :

* Les agents et les paramètres de pare-feu fonctionnent tels quels. Aucune modification n’est nécessaire. 
* Les moniteurs de connexion existants seront mappés vers Moniteur de connexion > Groupe de test > Format de test. En sélectionnant **Modifier**, vous pouvez afficher et modifier les propriétés de la nouvelle fonctionnalité Moniteur de connexion, télécharger un modèle pour apporter des modifications à Moniteur de connexion et le soumettre via Azure Resource Manager. 
* Les machines virtuelles Azure avec l’extension Network Watcher envoient des données à l’espace de travail et aux métriques. Le Moniteur de connexion met les données à disposition par le biais des nouvelles métriques (ChecksFailedPercent et RoundTripTimeMs) à la place des anciennes métriques (ProbesFailedPercent et AverageRoundtripMs). Les anciennes métriques seront migrées vers les nouvelles métriques en tant que ProbesFailedPercent -> ChecksFailedPercent et AverageRoundtripMs -> RoundTripTimeMs.
* Analyse des données :
   * **Alertes** : Migrées automatiquement vers les nouvelles métriques.
   * **Tableaux de bord et intégrations** : Nécessite la modification manuelle de l’ensemble de métriques. 
    
## <a name="prerequisites"></a>Prérequis

1. Si vous utilisez un espace de travail personnalisé, assurez-vous que Network Watcher est activé dans votre abonnement et dans la région de votre espace de travail Log Analytics. Si ce n’est pas le cas, vous verrez une erreur indiquant « Avant de tenter une migration, activez l’extension Network Watcher dans l’abonnement de la sélection et l’emplacement de l’espace de travail LA sélectionné. »
1. Dans le cas où l’extension Network Watcher n’est plus activée sur les machines virtuelles utilisées comme sources dans Moniteur de connexion (classique), vous verrez un message d’erreur indiquant « Les moniteurs de connexion ayant les tests suivants ne peuvent pas être importés, car l’extension Network Watcher n’est pas installée sur une ou plusieurs machines virtuelles Azure. Installez l’extension Network Watcher et cliquez sur Actualiser pour les importer. »



## <a name="migrate-the-connection-monitors"></a>Migrer les moniteurs de connexion

1. Pour migrer les anciens moniteurs de connexion vers la version plus récente, sélectionnez **Moniteur de connexion**, puis sélectionnez **Migrer les moniteurs de connexion**.

    ![Capture d’écran montrant la migration des moniteurs de connexion vers Moniteur de connexion.](./media/connection-monitor-2-preview/migrate-cm-to-cm-preview.png)
    
1. Sélectionnez votre abonnement et les moniteurs de connexion que vous souhaitez migrer, puis sélectionnez **Migrer les éléments sélectionnés**. 

En quelques clics seulement, vous avez migré les moniteurs de connexion existants vers Moniteur de connexion. Après la migration de CM (classique) vers CM, vous ne pourrez pas voir le l’analyse sous CM (classique)

Vous pouvez désormais personnaliser les propriétés de Moniteur de connexion, modifier l’espace de travail par défaut, télécharger des modèles et vérifier l’état de la migration. 

Après le début de la migration, les modifications suivantes ont lieu : 
* La ressource Azure Resource Manager passe au moniteur de connecter plus récent.
    * Le nom, la région et l’abonnement du moniteur de connexion restent inchangés. L’ID de ressource n’est pas concerné.
    * Sauf si le moniteur de connexion est personnalisé, un espace de travail Log Analytics par défaut est créé dans l’abonnement et dans la région du moniteur de connexion. Cet espace de travail est l’emplacement où sont stockées les données d’analyse. Les données du résultat de test sont également stockées dans les métriques.
    * Chaque test est migré vers un groupe de test appelé *defaultTestGroup*.
    * Les points de terminaison source et de destination sont créés et utilisés dans le groupe de test. Les noms par défaut sont *defaultSourceEndpoint* et *defaultDestinationEndpoint*.
    * Le port de destination et l’intervalle de détection sont déplacés vers la configuration de test appelée *defaultTestConfiguration*. Le protocole est défini sur la base des valeurs de port. Les seuils de réussite et autres propriétés facultatives sont laissés en blanc.
* Les alertes de métriques sont migrées vers les alertes de métriques de Moniteur de connexion. Les métriques sont différentes, d’où la modification. Pour plus d’informations, consultez [Surveillance de la connectivité réseau à l’aide de Moniteur de connexion](./connection-monitor-overview.md#metrics-in-azure-monitor).
* Les moniteurs de connexion migrés ne sont plus affichés comme l’ancienne solution de moniteur de connexion. Ils sont désormais disponibles uniquement dans Moniteur de connexion.
* Toutes les intégrations externes, telles que les tableaux de bord dans Power BI et Grafana, et les intégrations aux systèmes Security Information and Event Management (SIEM) doivent être migrées manuellement. Il s’agit de la seule étape manuelle que vous devez effectuer pour migrer votre installation.

## <a name="common-errors-encountered"></a>Erreurs courantes rencontrées

Voici quelques-unes des erreurs courantes rencontrées lors de la migration : 

| Error  |    Motif   |
|---|---|
|Les moniteurs de connexion suivants ne peuvent pas être importés, car Network Watcher n’est pas activée pour une ou plusieurs combinaisons abonnement-région. Activez Network Watcher et cliquez sur Actualiser pour les importer. Liste de moniteurs de connexion  {0}   |  Cette erreur se produit lorsque l’utilisateur migre des tests de Moniteur de connexion (classique) vers Moniteur de connexion et que l’extension Network Watcher n’est pas activée dans un ou plusieurs abonnements et régions de Moniteur de connexion (classique). L’utilisateur doit activer l’extension NW dans l’abonnement et actualiser pour les importer avant de procéder à nouveau à la migration   |
|Les moniteurs de connexion ayant les tests suivants ne peuvent pas être importés, car l’extension Network Watcher n’est pas installée sur une ou plusieurs machines virtuelles Azure. Installez l’extension Network Watcher et cliquez sur Actualiser pour les importer. Liste des tests : {0} |    Cette erreur se produit lorsque l’utilisateur migre des tests de Moniteur de connexion (classique) vers Moniteur de connexion et que l’extension Network Watcher n’est pas installée sur une ou plusieurs machines virtuelles Azure de Moniteur de connexion (classique). L’utilisateur doit installer l’extension NW sur la machine virtuelle Azure et actualiser avant de procéder à nouveau à la migration |
|Aucune ligne à afficher   |  Cette erreur se produit lorsque l’utilisateur tente de migrer des abonnements de Moniteur de connexion (classique) vers Moniteur de connexion, mais qu’aucun moniteur de connexion (classique) n’est créé dans les abonnements |

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur le Moniteur de connexion, consultez :
* [Migration de Network Performance Monitor vers Moniteur de connexion](./migrate-to-connection-monitor-from-network-performance-monitor.md)
* [Créer un Moniteur de connexion à l’aide du portail Azure](./connection-monitor-create-using-portal.md)


    
 
    
