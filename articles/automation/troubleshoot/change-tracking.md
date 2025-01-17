---
title: Détecter les problèmes rencontrés avec la fonctionnalité « Suivi des modifications et inventaire » d’Azure Automation
description: Cet article vous explique comment détecter et résoudre les problèmes liés à la fonctionnalité « Suivi des modifications et inventaire » d’Azure Automation.
services: automation
ms.subservice: change-inventory-management
ms.date: 02/15/2021
ms.topic: troubleshooting
ms.openlocfilehash: 21699d306742c2a732155ac8df78608f5c3dd7ae
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2021
ms.locfileid: "132346019"
---
# <a name="troubleshoot-change-tracking-and-inventory-issues"></a>Résoudre les problèmes rencontrés avec Change Tracking et d’inventaire

Cet article décrit comment détecter et résoudre les problèmes liés à la fonctionnalité « Suivi des modifications et inventaire » d’Azure Automation. Pour obtenir des informations générales sur la fonctionnalité « Suivi des modifications et inventaire », consultez [Vue d’ensemble de Suivi des modifications et inventaire](../change-tracking/overview.md).

## <a name="general-errors"></a>Erreurs générales.

### <a name="scenario-machine-is-already-registered-to-a-different-account"></a><a name="machine-already-registered"></a>Scénario : La machine est déjà inscrite sur un autre compte

### <a name="issue"></a>Problème

Vous recevez le message d’erreur suivant :

```error
Unable to Register Machine for Change Tracking, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

### <a name="cause"></a>Cause

La machine est déjà déployée sur un autre espace de travail pour Change Tracking.

### <a name="resolution"></a>Résolution

1. Assurez-vous que votre ordinateur est associé à l’espace de travail approprié. Pour obtenir des conseils sur la façon de vérifier cela, consultez l’article [Vérifier la connectivité de l’agent à Azure Monitor](../../azure-monitor/agents/agent-windows.md#verify-agent-connectivity-to-azure-monitor). Assurez-vous également que cet espace de travail est associé à votre compte Azure Automation. Pour le confirmer, accédez à votre compte Automation et sélectionnez **Espace de travail lié** sous **Ressources associées**.

1. Vérifiez que les ordinateurs apparaissent dans l’espace de travail Log Analytics comme étant liés à votre compte Automation. Exécutez la requête suivante dans l’espace de travail Log Analytics.

   ```kusto
   Heartbeat
   | summarize by Computer, Solutions
   ```

   Si votre machine ne figure pas dans les résultats de la requête, cela signifie qu’elle n’a pas été archivée récemment. Il existe probablement un problème de configuration locale. Vous devez réinstaller l’agent Log Analytics.

   Si votre machine est répertoriée dans les résultats de la requête, vérifiez que **changeTracking** est répertorié dans la propriété Solutions. Cela vérifie qu’elle est enregistrée avec la fonction Suivi des modifications et inventaire. Si ce n’est pas le cas, recherchez les problèmes de configuration de l’étendue. La configuration de l’étendue permet de déterminer les machines qui sont configurées pour la fonction Suivi des modifications et inventaire. Pour configurer la configuration d’étendue pour la machine cible, consultez [Activer Suivi des modifications et inventaire à partir d’un compte Automation](../change-tracking/enable-from-automation-account.md).

   Dans votre espace de travail, exécutez cette requête.

   ```kusto
   Operation
   | where OperationCategory == 'Data Collection Status'
   | sort by TimeGenerated desc
   ```

1. Si vous obtenez un résultat ```Data collection stopped due to daily limit of free data reached. Ingestion status = OverQuota```, le quota défini dans votre espace de travail a été atteint, ce qui a fait cesser l’enregistrement des données. Dans votre espace de travail, accédez à **Utilisation et estimation des coûts**. Sélectionnez un nouveau **Niveau tarifaire** qui vous permet d’utiliser plus de données ou cliquez sur **Limite quotidienne** et supprimez la limite.

:::image type="content" source="./media/change-tracking/change-tracking-usage.png" alt-text="Utilisation et estimation des coûts." lightbox="./media/change-tracking/change-tracking-usage.png":::

Si votre problème n’est toujours pas résolu, suivez les étapes dans [Déployer un Runbook Worker hybride Windows](../automation-windows-hrw-install.md) pour réinstaller le Worker hybride pour Windows. Pour Linux, suivez les étapes décrites dans [Déployer un Runbook Worker hybride Linux](../automation-linux-hrw-install.md).

## <a name="windows"></a>Windows

### <a name="scenario-change-tracking-and-inventory-records-arent-showing-for-windows-machines"></a><a name="records-not-showing-windows"></a>Scénario : Les enregistrements Change Tracking et de l’inventaire n’apparaissent pas pour les ordinateurs Windows

#### <a name="issue"></a>Problème

Les résultats de la fonctionnalité « Suivi des modifications et inventaire » n’apparaissent pas sur les ordinateurs Windows sur lesquels la fonctionnalité a été activée.

#### <a name="cause"></a>Cause

Cette erreur peut avoir les causes suivantes :

* L’agent Azure Log Analytics pour Windows n’est pas en cours d’exécution.
* La communication avec le compte Automation est bloquée.
* Les packs d’administration de Change Tracking et de l’inventaire n’ont pas été téléchargés.
* Il se peut que la machine virtuelle en cours d’activation provienne d’un ordinateur cloné qui n’a pas été préparé avec System Preparation (sysprep) à l’aide de l’agent Log Analytics pour Windows installé.

#### <a name="resolution"></a>Résolution

Sur l’ordinateur agent Log Analytics, accédez à **C:\Program Files\Microsoft Monitoring Agent\Agent\Tools** et exécutez les commandes suivantes :

```cmd
net stop healthservice
StopTracing.cmd
StartTracing.cmd VER
net start healthservice
```

Si vous avez encore besoin d’aide, vous pouvez recueillir des informations de diagnostic et contacter le support.

> [!NOTE]
> L’agent log Analytics active le suivi des erreurs par défaut. Pour activer les messages d’erreur détaillés comme dans l’exemple précédent, utilisez le paramètre `VER`. Pour les suivis d’informations, utilisez `INF` lorsque vous appelez `StartTracing.cmd`.

##### <a name="log-analytics-agent-for-windows-not-running"></a>L’agent Log Analytics pour Windows n’est pas exécuté

Vérifiez que l’agent Log Analytics pour Windows (**HealthService.exe**) est en cours d’exécution sur l’ordinateur.

##### <a name="communication-to-automation-account-blocked"></a>Communication bloquée vers le compte Automation

Dans l’observateur d’événements sur la machine, recherchez les événements comportant le mot `changetracking`.

Pour savoir quelles adresses et quels ports doivent être autorisés pour le fonctionnement de Suivi des modifications et inventaire, consultez [Network planning (Planification du réseau)](../automation-hybrid-runbook-worker.md#network-planning).

##### <a name="management-packs-not-downloaded"></a>Packs d’administration non téléchargés

Vérifiez que les packs d’administration Change Tracking et Inventaire suivants sont installés localement :

* `Microsoft.IntelligencePacks.ChangeTrackingDirectAgent.*`
* `Microsoft.IntelligencePacks.InventoryChangeTracking.*`
* `Microsoft.IntelligencePacks.SingletonInventoryCollection.*`

##### <a name="vm-from-cloned-machine-that-has-not-been-sysprepped"></a>Machine virtuelle issue d’un ordinateur cloné qui n’a pas été préparé avec Sysprep

Si vous utilisez une image clonée, commencez par exécuter Sysprep sur l’image et installez ensuite l’agent Log Analytics pour Windows.

## <a name="linux"></a>Linux

### <a name="scenario-no-change-tracking-and-inventory-results-on-linux-machines"></a>Scénario : Aucun résultat de Change Tracking ni de l’inventaire sur les ordinateurs Linux

#### <a name="issue"></a>Problème

Les résultats de la fonctionnalité « Suivi des modifications et inventaire » n’apparaissent pas alors que vous avez activé la fonctionnalité sur les ordinateurs Linux. 

#### <a name="cause"></a>Cause
Voici les causes possibles de ce problème :
* L’agent Log Analytics pour Linux n’est pas en cours d’exécution.
* L’agent Log Analytics pour Linux n’est pas configuré correctement.
* Il existe des conflits de supervision de l’intégrité des fichiers (FIM).

#### <a name="resolution"></a>Résolution 

##### <a name="log-analytics-agent-for-linux-not-running"></a>L’agent Log Analytics pour Linux n’est pas en cours d’exécution

Vérifiez si le démon de l’agent Log Analytics pour Linux (**omsagent**) est en cours d’exécution sur votre machine. Exécutez la requête suivante dans l’espace de travail Log Analytics lié à votre compte Automation.

```loganalytics
Copy
Heartbeat
| summarize by Computer, Solutions
```

Si votre ordinateur ne figure pas dans les résultats de la requête, il n’a pas été enregistré récemment. Il y a probablement un problème de configuration locale et vous devez réinstaller l’agent. Pour plus d’informations sur l’installation et la configuration, consultez [Collecter des données de journal avec l’agent Log Analytics](../../azure-monitor/agents/log-analytics-agent.md).

Si votre ordinateur s’affiche dans les résultats de la requête, vérifiez la configuration de l’étendue. Consultez [Ciblage des solutions de supervision dans Azure Monitor](../../azure-monitor/insights/solution-targeting.md).

Pour mieux résoudre ce problème, consultez [Problème : vous ne pouvez pas voir de données Linux](../../azure-monitor/agents/agent-linux-troubleshoot.md#issue-you-are-not-seeing-any-linux-data).

##### <a name="log-analytics-agent-for-linux-not-configured-correctly"></a>L’agent Log Analytics pour Linux n’est pas configuré correctement

L’agent Log Analytics pour Linux n’est peut-être pas configuré correctement pour la collecte des journaux et des sorties en ligne de commande à l’aide de l’outil Log Collector d’OMS. Consultez [Vue d’ensemble de Suivi des modifications et inventaire](../change-tracking/overview.md).

##### <a name="fim-conflicts"></a>Conflits FIM

La fonctionnalité FIM de Microsoft Defender pour le cloud peut ne pas valider correctement l’intégrité de vos fichiers Linux. Vérifiez que FIM est opérationnel et correctement configuré pour l’analyse des fichiers Linux. Consultez [Vue d’ensemble de Suivi des modifications et inventaire](../change-tracking/overview.md).

## <a name="next-steps"></a>Étapes suivantes

Si votre problème ne figure pas ici ou que vous ne pouvez pas le résoudre, utilisez un des canaux suivants pour obtenir une aide supplémentaire :

* Obtenez des réponses de la part d’experts Azure via les [Forums Azure](https://azure.microsoft.com/support/forums/).
* Connectez-vous à [@AzureSupport](https://twitter.com/azuresupport), le compte Microsoft Azure officiel pour améliorer l’expérience client. Le Support Azure fournit à la communauté Azure des réponses, un support technique et des experts.
* Signaler un incident au support Azure Accédez au [site du Support Azure](https://azure.microsoft.com/support/options/), puis sélectionnez **Obtenir de l’aide**.
