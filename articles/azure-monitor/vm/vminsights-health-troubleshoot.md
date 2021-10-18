---
title: Résoudre des problèmes d’intégrité de l’invité VM Insights (préversion)
description: Décrit les étapes de dépannage que vous pouvez suivre lorsque vous rencontrez des problèmes d’intégrité sur VM Insights.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/25/2021
ms.openlocfilehash: 13de7b055820ac680e8b513198224bcfd6421e11
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2021
ms.locfileid: "129708876"
---
# <a name="troubleshoot-vm-insights-guest-health-preview"></a>Résoudre des problèmes d’intégrité de l’invité VM Insights (préversion)
Cet article décrit les étapes de dépannage que vous pouvez suivre lorsque vous rencontrez des problèmes d’intégrité sur VM Insights.

## <a name="installation-errors"></a>Erreurs d'installation
Si l’une des solutions suivantes ne résout pas le problème d’installation, collectez le journal de l’agent d’intégrité de la machine virtuelle situé dans `/var/log/azure/Microsoft.Azure.Monitor.VirtualMachines.GuestHealthLinuxAgent/*.log` et contactez Microsoft pour approfondir votre investigation.

### <a name="error-message-showing-db5-error"></a>Message d’erreur indiquant l’erreur db5
Votre installation n’a pas réussi et votre message d’erreur d’installation ressemble à ce qui suit :

```
script execution exit with error: error: db5 error(5) from dbenv->open: Input/output error
error: cannot open Packages index using db5 - Input/output error (5)
error: cannot open Packages database in /var/lib/rpm
error: db5 error(5) from dbenv->open: Input/output error
error: cannot open Packages database in /var/lib/rpm
```
C’est parce que la base de données RPM du gestionnaire de package est endommagée. Essayez de suivre les instructions de [récupération de base de données RPM](https://rpm.org/user_doc/db_recovery.html) pour la récupérer. Une fois votre base de données RPM récupérée, réessayez d’effectuer l’installation.

### <a name="init-file-already-exist-error"></a>Erreur indiquant que le fichier d’initialisation existe déjà
Votre installation n’a pas réussi et votre message d’erreur d’installation ressemble à ce qui suit :

```
Exiting with the following error: "Failed to install VM Guest Health Agent: Init already exists: /etc/systemd/system/vmGuestHealthAgent.service"install vmGuestHealthAgent service execution failed with exit code 37
```

L’agent d’intégrité de la machine virtuelle va désinstaller le service existant avant d’installer la version actuelle. La raison de cette erreur tient probablement à ce que le fichier de service précédent n’a pas été nettoyé pour une raison ou une autre. Connectez-vous à la machine virtuelle et exécutez la commande suivante pour sauvegarder le fichier de service existant et réessayez d’effectuer l’installation.

```
sudo mv /etc/systemd/system/vmGuestHealthAgent.service  /etc/systemd/system/vmGuestHealthAgent.service.bak
```

Si l’installation a réussi, exécutez la commande suivante pour supprimer le fichier de sauvegarde.

```
sudo rm /etc/systemd/system/vmGuestHealthAgent.service.bak
```

### <a name="installation-failed-to-exit-code-37"></a>L’installation n’a pas pu quitter le code 37
Votre installation n’a pas réussi et votre message d’erreur d’installation ressemble à ce qui suit : 

```
Exiting with the following error: "Failed to install VM Guest Health Agent: exit status 1"install vmGuestHealthAgent service execution failed with exit code 37
```
C’est probablement parce que l’Agent invité de la machine virtuelle n’a pas pu acquérir le verrou pour le fichier de service. Essayez de redémarrer votre machine virtuelle, ce qui libérera le verrou.


## <a name="upgrade-errors"></a>Erreurs de mise à niveau

### <a name="upgrade-available-message-is-still-displayed-after-upgrading-guest-health"></a>Le message « Mise à niveau disponible » s’affiche toujours après la mise à niveau de l’intégrité de l’invité 

- Vérifiez que la machine virtuelle fonctionne dans Global Azure. Les serveurs avec Azure Arc ne sont pas encore pris en charge.
- Vérifiez que la région et la version du système d’exploitation de la machine virtuelle sont prises en charge, comme décrit dans [Activer l’intégrité des invités d’Azure Monitor pour machines virtuelles (préversion)](vminsights-health-enable.md).
- Vérifiez que l’extension d’intégrité de l’invité a été installée avec le code de sortie 0.
- Vérifiez que l’extension de l’agent Azure Monitor est bien installée.
- Vérifiez que l’identité managée affectée par le système est activée pour la machine virtuelle.
- Vérifiez qu’aucune identité managée affectée par l’utilisateur n’est spécifiée pour la machine virtuelle.
- Vérifiez les machines virtuelles Windows dont les paramètres régionaux sont *Anglais (États-Unis)* . La localisation n’est actuellement pas prise en charge par l’agent Azure Monitor.
- Vérifiez que la machine virtuelle n’utilise pas le proxy. L’agent Azure Monitor ne prend pas actuellement en charge les proxys.
- Vérifiez que l’agent d’extension d’intégrité a démarré sans erreur. Si l’agent ne peut pas démarrer, l’état de l’agent est peut-être endommagé. Supprimez le contenu du dossier d’état de l’agent et redémarrez l’agent.
  - Pour Linux : Le démon est *vmGuestHealthAgent*. Le dossier d’état est */var/opt/vmGuestHealthAgent/* *.
  - Pour Windows : Le service est *VM Guest Health agent*. Le dossier d’état est _%ProgramData%\Microsoft\VMGuestHealthAgent\\*_ .
- Vérifiez que l’agent Azure Monitor dispose d’une connectivité réseau. 
  - À partir de la machine virtuelle, essayez d’effectuer un test ping sur _\<region\>.handler.control.monitor.azure.com_. Par exemple, pour une machine virtuelle dans la région westeurope, essayez d’effectuer un test ping sur _westeurope.handler.control.monitor.azure.com:443_.
- Vérifiez que la machine virtuelle est associée à une règle de collecte de données dans la même région que l’espace de travail Log Analytics.
  -  Reportez-vous à la rubrique **Créer une règle de collecte de données** dans [Activer l’intégrité des invités d’Azure Monitor pour machines virtuelles (préversion)](vminsights-health-enable.md) pour vous assurer que la structure de la règle de collecte de données est correcte. Faites particulièrement attention à la présence de la section de source de données *performanceCounters* configurée pour échantillonner trois compteurs et à la présence de la section *inputDataSources* dans la configuration de l’extension d’intégrité pour envoyer des compteurs à l’extension.
-  Vérifiez la machine virtuelle à la recherche d’erreurs de l’extension d’intégrité.
   -  Pour Linux : Vérifiez les journaux dans _/var/log/azure/Microsoft.Azure.Monitor.VirtualMachines.GuestHealthLinuxAgent/*.log_.
   -  Pour Windows : Vérifiez les journaux dans _C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Monitor.VirtualMachines.GuestHealthWindowsAgent\{extension version}\*.log_.
-  Vérifiez la machine virtuelle à la recherche d’erreurs de l’agent Azure Monitor.
   -  Pour Linux : Vérifiez les journaux dans _/var/log/mdsd.*_ .
   -  Pour Windows : Vérifiez les journaux dans _C:\WindowsAzure\Resources\*{vmName}.AMADataStore_.
 

## <a name="usage-errors"></a>Erreurs d’utilisation

### <a name="error-message-that-no-data-is-available"></a>Message d’erreur indiquant qu’aucune donnée n’est disponible 

![Pas de données](media/vminsights-health-troubleshoot/no-data.png)


#### <a name="verify-that-the-virtual-machine-meets-configuration-requirements"></a>Vérifier que la machine virtuelle respecte la configuration requise

1. Vérifiez que la machine virtuelle est une machine virtuelle Azure. Azure Arc pour serveurs n’est pas pris en charge actuellement.
2. Vérifiez que la machine virtuelle exécute un [système d’exploitation pris en charge](vminsights-health-enable.md?current-limitations.md).
3. Vérifiez que la machine virtuelle est installée dans une [région prise en charge](vminsights-health-enable.md?current-limitations.md).
4. Vérifiez que l’espace de travail Log Analytics est installé dans une [région prise en charge](vminsights-health-enable.md?current-limitations.md).

#### <a name="verify-that-the-vm-is-properly-onboarded"></a>Vérifier que la machine virtuelle est correctement intégrée
Vérifiez que l’extension de l’agent Azure Monitor et l’agent Intégrité de la machine virtuelle invitée sont configurés sur la machine virtuelle. Sélectionnez **Extensions** dans le menu de la machine virtuelle dans le Portail Azure et assurez-vous que les deux agents sont répertoriés.

![Extensions de machine virtuelle](media/vminsights-health-troubleshoot/extensions.png)

#### <a name="verify-the-system-assigned-identity-is-enabled-on-the-virtual-machine"></a>Vérifier que l’identité affectée par le système est activée sur la machine virtuelle
Vérifiez que l’identité affectée par le système est activée sur la machine virtuelle. Sélectionnez **Identité** dans le menu de la machine virtuelle sur le portail Azure. Si l’identité managée par l’utilisateur est activée, quel que soit l’état de l’identité gérée par le système, l’agent Azure Monitor ne pourra pas communiquer avec le service de configuration, et l’extension d’intégrité de l’invité ne fonctionnera pas.

![Identité attribuée au système](media/vminsights-health-troubleshoot/system-identity.png)

#### <a name="verify-data-collection-rule"></a>Vérifier la règle de collecte des données
Vérifiez que la règle de collecte de données spécifiant l’extension d’intégrité comme source de données est associée à la machine virtuelle.

### <a name="error-message-for-bad-request-due-to-insufficient-permissions"></a>Message d’erreur pour une demande incorrecte en raison d’autorisations insuffisantes
Cette erreur indique que le fournisseur de ressources **Microsoft.WorkloadMonitor** n’a pas été inscrit dans l’abonnement. Pour plus d’informations sur l’inscription de ce fournisseur de ressources, consultez [Fournisseurs et types de ressources Azure](../../azure-resource-manager/management/resource-providers-and-types.md#register-resource-provider). 

![Demande incorrecte](media/vminsights-health-troubleshoot/bad-request.png)

### <a name="health-shows-as-unknown-after-guest-health-is-enabled"></a>L’intégrité s’affiche comme « inconnue » après l’activation de l’intégrité de l’invité.

#### <a name="verify-that-performance-counters-on-windows-nodes-are-working-correctly"></a>Vérifier que les compteurs de performances sur les nœuds Windows fonctionnent correctement 
L’intégrité de l’invité dépend de la capacité de l’agent à collecter les compteurs de performances du nœud. L’ensemble de base des bibliothèques de compteurs de performances peut être endommagé et doit être régénéré. Suivez les instructions dans [Régénérer manuellement des valeurs de la bibliothèque des compteurs de performances](/troubleshoot/windows-server/performance/rebuild-performance-counter-library-values) pour régénérer les compteurs de performances.





## <a name="next-steps"></a>Étapes suivantes

- [Découvrir une vue d’ensemble de la fonctionnalité d’intégrité des invités de VM Insights](vminsights-health-overview.md)
