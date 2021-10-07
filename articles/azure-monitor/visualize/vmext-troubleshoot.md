---
title: Résoudre les problèmes liés à l’extension de machine virtuelle Azure Log Analytics
description: Décrit les symptômes, les causes et les solutions aux problèmes courants liés à l’extension de machine virtuelle Log Analytics pour les machines virtuelles Azure Windows et Linux.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/06/2019
ms.openlocfilehash: 2983304c7cb1515c783037a48bb78aecfc364e53
ms.sourcegitcommit: f2d0e1e91a6c345858d3c21b387b15e3b1fa8b4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2021
ms.locfileid: "123535578"
---
# <a name="troubleshooting-the-log-analytics-vm-extension-in-azure-monitor"></a>Résolution des problèmes de l’extension de machine virtuelle Log Analytics dans Azure Monitor
Cet article présente les problèmes qui peuvent survenir avec l’extension de machine virtuelle Log Analytics sur les machines virtuelles Windows et Linux en cours d’exécution sur Microsoft Azure, puis propose des solutions possibles pour les résoudre.

Pour vérifier l’état de l’extension, effectuez les étapes suivantes à partir du portail Azure.

1. Connectez-vous au [portail Azure](https://portal.azure.com).
2. Dans le portail Azure, cliquez sur **Tous les services**. Dans la liste des ressources, tapez **machines virtuelles**. Au fur et à mesure de la saisie, la liste est filtrée. Sélectionnez **Machines virtuelles**.
3. Dans la liste de vos machines virtuelles, recherchez et sélectionnez-la.
3. Sur la machine virtuelle, cliquez sur **Extensions**.
4. Dans la liste, vérifiez si l’extension Log Analytics est activée.  Pour Linux, l’agent apparaît sous le nom **OMSAgentforLinux**. Pour Windows, il apparaît sous le nom **MicrosoftMonitoringAgent**.

   ![Vue Extension de machine virtuelle](./media/vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Cliquez sur l’extension pour afficher les détails. 

   ![Détails de l’extension de machine virtuelle](./media/vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Dépannage de l’extension de machine virtuelle Windows dans Azure

Si l’extension de machine virtuelle *Microsoft Monitoring Agent* ne s’installe pas ou ne génère pas de rapports, vous pouvez effectuer les étapes suivantes pour résoudre le problème.

1. Vérifiez si l’agent de machine virtuelle Azure est installé et fonctionne correctement en suivant la procédure décrite dans l’article [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * Vous pouvez également consulter le fichier journal de l’agent de machine virtuelle `C:\WindowsAzure\logs\WaAppAgent.log`.
   * Si le journal n’existe pas, l’agent de machine virtuelle n’est pas installé.
   * [Installer l’agent de machine virtuelle Azure](../vm/monitor-virtual-machine.md#agents)
2. Examinez les fichiers de journaux d’extension de machine virtuelle Microsoft Monitoring Agent dans `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`.
3. Vérifiez que la machine virtuelle peut exécuter des scripts PowerShell.
4. Vérifiez que les autorisations sur C:\Windows\temp n’ont pas été modifiées.
5. Affichez l’état de Microsoft Monitoring Agent en tapant ce qui suit dans une fenêtre PowerShell avec des privilèges élevés sur la machine virtuelle `(New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`.
6. Examinez les fichiers des journaux d’installation de l’Agent de surveillance Microsoft dans `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.18053.0\`. Notez que ce chemin d’accès changera en fonction du numéro de version de l’agent.

Pour plus d’informations, consultez [Résolution des problèmes des extensions Windows](../../virtual-machines/extensions/oms-windows.md).

## <a name="troubleshooting-linux-vm-extension"></a>Dépannage de l’extension de machine virtuelle Linux
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 
Si l’extension de machine virtuelle *Agent Log Analytics pour Linux* ne s’installe pas ou ne génère pas de rapports, vous pouvez effectuer les étapes suivantes pour résoudre le problème.

1. Si l’état de l’extension est *Inconnu*, vérifiez si l’agent de machine virtuelle Azure est installé et fonctionne correctement en examinant le fichier journal de l’agent de machine virtuelle `/var/log/waagent.log`
   * Si le journal n’existe pas, l’agent de machine virtuelle n’est pas installé.
   * [Installez l’agent de machine virtuelle Azure sur les machines virtuelles Linux](../../virtual-machines/extensions/agent-linux.md#installation).
2. Pour les autres états défectueux, examinez les fichiers journaux d’activité de l’extension de machine virtuelle Agent Log Analytics pour Linux dans `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` et `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`.
3. Si l’extension est intègre, mais que les données ne sont pas chargées, examinez les fichiers journaux de l’Agent Log Analytics pour Linux dans `/var/opt/microsoft/omsagent/log/omsagent.log`.

Pour plus d’informations, consultez [Résolution des problèmes des extensions Linux](../../virtual-machines/extensions/oms-linux.md).

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir d’autres conseils de dépannage de l’Agent Log Analytics pour Linux hébergé sur des ordinateurs en dehors d’Azure, consultez [Dépanner l’agent Linux Azure Log Analytics](../agents/agent-linux-troubleshoot.md).
