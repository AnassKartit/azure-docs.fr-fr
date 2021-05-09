---
title: Tutoriel - Superviser une machine hybride avec les insights VM Azure Monitor
description: Découvrez comment collecter et analyser les données d’une machine hybride dans Azure Monitor.
ms.topic: tutorial
ms.date: 04/21/2021
ms.openlocfilehash: f59ad448440110e2c5e6dd1fa1b2858d9cf42e91
ms.sourcegitcommit: 3c460886f53a84ae104d8a09d94acb3444a23cdc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/21/2021
ms.locfileid: "107834261"
---
# <a name="tutorial-monitor-a-hybrid-machine-with-vm-insights"></a>Tutoriel : Superviser une machine hybride avec les insights VM

[Azure Monitor](../../../azure-monitor/overview.md) est capable de collecter directement les données de vos machines hybrides, et les afficher dans un espace de travail Log Analytics en vue d’une analyse et d’une mise en corrélation détaillées. En général, cela implique l’installation de l’[agent Log Analytics](../../../azure-monitor/agents/agents-overview.md#log-analytics-agent) sur la machine à l’aide d’un script, manuellement ou d’une méthode automatisée, selon vos normes de gestion de la configuration. Les serveurs avec Arc ont récemment introduit la prise en charge de l’installation des [extensions VM](../manage-vm-extensions.md) d’agent Log Analytics et Dependency Agent pour Windows et Linux, ce qui permet aux [insights VM](../../../azure-monitor/vm/vminsights-overview.md) de collecter les données sur vos machines virtuelles non-Azure.

Ce tutoriel vous montre comment configurer et collecter les données de vos machines Linux ou Windows en activant les insights VM à l’aide d’un ensemble d’étapes simplifié qui simplifie l’expérience et permet de gagner du temps.  

## <a name="prerequisites"></a>Prérequis

* Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

* La fonctionnalité d’extension de machine virtuelle n’est disponible que dans la liste des [régions prises en charge](../overview.md#supported-regions).

* Consultez [Systèmes d’exploitation pris en charge](../../../azure-monitor/vm/vminsights-enable-overview.md#supported-operating-systems) pour vérifier que le système d’exploitation des serveurs que vous activez est pris en charge par les insights VM.

* Examinez la configuration requise du pare-feu pour l’agent Log Analytics dans [Présentation de l’agent Log Analytics](../../../azure-monitor/agents/log-analytics-agent.md#network-requirements). L’agent Map Dependency de VM Insights ne transmet pas de données par lui-même et ne requiert pas de modifications des pare-feu ou des ports.

## <a name="sign-in-to-azure-portal"></a>Se connecter au portail Azure

Connectez-vous au [portail Azure](https://portal.azure.com).

## <a name="enable-vm-insights"></a>Activer VM Insights

1. Lancez le service Azure Arc dans le portail Azure en cliquant sur **Tous les services**, puis en recherchant et en cliquant sur **Machines - Azure Arc**.

    :::image type="content" source="./media/quick-enable-hybrid-vm/search-machines.png" alt-text="Rechercher Serveurs avec Azure Arc dans Tous les services" border="false":::

1. Dans la page **Machines - Azure Arc**, sélectionnez la machine connectée que vous avez créée dans l’article de [démarrage rapide](quick-enable-hybrid-vm.md).

1. Dans le volet de gauche sous la section **Supervision**, sélectionnez **Insights**, puis **Activer**.

    :::image type="content" source="./media/tutorial-enable-vm-insights/insights-option.png" alt-text="Sélectionnez l’option Insights dans le menu de gauche" border="false":::

1. Dans la page **Intégration d’insights** d’Azure Monitor, vous êtes invité à créer un espace de travail. Pour ce tutoriel, nous vous déconseillons de sélectionner un espace de travail Log Analytics existant si vous en avez déjà un. Sélectionnez la valeur par défaut, c’est-à-dire un espace de travail ayant un nom unique dans la même région que votre machine connectée inscrite. Cet espace de travail est créé et configuré pour vous.

    :::image type="content" source="./media/tutorial-enable-vm-insights/enable-vm-insights.png" alt-text="Page Activer les insights VM" border="false":::

1. Vous recevez des messages d’état à mesure que la configuration est effectuée. Ce processus prend quelques minutes, car les extensions sont installées sur votre machine connectée.

    :::image type="content" source="./media/tutorial-enable-vm-insights/onboard-vminsights-vm-portal-status.png" alt-text="Message de l’état de progression de l’activation des insights VM" border="false":::

    Une fois l’opération terminée, vous recevez un message indiquant que la machine a été correctement intégrée et que l’insight a été correctement déployé.

## <a name="view-data-collected"></a>Afficher les données collectées

Une fois le déploiement et la configuration terminés, sélectionnez **Insights**, puis sélectionnez l’onglet **Performances**. Cet onglet montre un groupe spécifique de compteurs de performances collectés à partir du système d’exploitation invité de votre machine. Faites défiler l’écran vers le bas pour afficher plus de compteurs et déplacez la souris sur un graphique pour afficher la moyenne et les centiles pris à partir du moment où l’extension de machine virtuelle Log Analytics a été installée sur la machine.

:::image type="content" source="./media/tutorial-enable-vm-insights/insights-performance-charts.png" alt-text="Graphiques de performances des insights VM pour la machine sélectionnée" border="false":::

Sélectionnez **Carte** pour ouvrir la fonctionnalité relative aux cartes. Elle montre les processus en cours d’exécution et leurs dépendances sur la machine. Sélectionnez **Propriétés** pour ouvrir le volet Propriétés, s’il n’est pas déjà ouvert.

:::image type="content" source="./media/tutorial-enable-vm-insights/insights-map.png" alt-text="Carte des insights VM pour la machine sélectionnée" border="false":::

Développez les processus de votre machine. Sélectionnez l’un des processus pour voir ses détails et mettre en évidence ses dépendances.

Resélectionnez votre machine, puis sélectionnez **Événements de journal**. Vous voyez une liste de tables qui sont stockées dans l’espace de travail Log Analytics de la machine. Cette liste diffère selon que vous utilisez une machine Windows ou Linux. Sélectionnez le tableau **Événement**. Le tableau **Événement** inclut tous les événements du journal des événements Windows. Log Analytics s’ouvre avec une simple requête permettant de récupérer les entrées du journal des événements collectées.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur Azure Monitor, consultez l’article suivant :

> [!div class="nextstepaction"]
> [Vue d’ensemble d’Azure Monitor](../../../azure-monitor/overview.md)