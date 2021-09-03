---
title: Nouveautés de l’agent des serveurs avec Azure Arc
description: Cet article contient des notes de publication pour l’agent des serveurs avec Azure Arc. Pour la plupart des problèmes résumés, des liens mènent à des informations supplémentaires.
ms.topic: conceptual
ms.date: 07/16/2021
ms.openlocfilehash: d53ebd32c870ce8ec26bca7bcb811fbdd45c58b2
ms.sourcegitcommit: e2fa73b682a30048907e2acb5c890495ad397bd3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2021
ms.locfileid: "114392371"
---
# <a name="whats-new-with-azure-arc-enabled-servers-agent"></a>Nouveautés de l’agent des serveurs avec Azure Arc

L’agent Connected Machine des serveurs avec Azure Arc reçoit des améliorations de manière continue. Pour vous informer des développements les plus récents, cet article détaille les thèmes suivants :

- Versions les plus récentes
- Problèmes connus
- Résolution des bogues

## <a name="july-2021"></a>Juillet 2021

Version 1.8

### <a name="new-features"></a>Nouvelles fonctionnalités

- Amélioration de la fiabilité lors de l’installation de l’extension de l’agent Azure Monitor sur les systèmes Red Hat et CentOS
- Ajout de la mise en œuvre côté agent de la longueur maximale du nom de la ressource (54 caractères)
- Améliorations de la stratégie de configuration d’invité (Guest Configuration) :
  - Ajout de la prise en charge des stratégies de configuration d’invité basées sur PowerShell sur les systèmes d’exploitation Linux
  - Ajout de la prise en charge de plusieurs attributions de la même stratégie de configuration d’invité sur le même serveur
  - Mise à niveau de PowerShell Core vers la version 7.1 sur les systèmes d’exploitation Windows

### <a name="fixed"></a>Fixe

- L’agent continue de s’exécuter s’il ne parvient pas à écrire les événements de démarrage/d’arrêt du service dans le journal des événements de l’application Windows

## <a name="june-2021"></a>Juin 2021

Version 1.7

### <a name="new-features"></a>Nouvelles fonctionnalités

- Amélioration de la fiabilité lors de l’intégration :
  - Amélioration de la logique de nouvelle tentative quand HIMDS n’est pas disponible
  - Désormais, l’intégration se poursuit au lieu de s’arrêter si les informations du système d’exploitation ne peuvent pas être obtenues
- Amélioration de la fiabilité lors de l’installation de l’extension de l’agent OMS sur les systèmes Red Hat et CentOS

## <a name="may-2021"></a>Mai 2021

Version 1.6

### <a name="new-features"></a>Nouvelles fonctionnalités

- Ajout de la prise en charge de SUSE Enterprise Linux 12
- Mise à jour de l’agent de configuration invité vers la version 1.26.12.0 pour inclure les éléments suivants :

   - Les stratégies sont exécutées dans un processus distinct.
   - Ajout de la prise en charge de signature V2 pour la validation de l’extension.
   - Mise à jour mineure de la journalisation des données.

## <a name="april-2021"></a>Avril 2021

Version 1.5

### <a name="new-features"></a>Nouvelles fonctionnalités

- Ajout de la prise en charge de Red Hat Enterprise Linux 8 et CentOS Linux 8.
- Nouveau paramètre `-useStderr` pour diriger l’erreur et la sortie détaillée vers stderr.
- Nouveau paramètre `-json` pour diriger les résultats de sortie au format JSON (en cas d’utilisation avec -useStderr).
- Collecter d’autres métadonnées d’instance : fabricant, modèle et ID de la ressource de cluster (pour les nœuds Azure Stack HCI).
 
## <a name="march-2021"></a>Mars 2021

Version 1.4

### <a name="new-features"></a>Nouvelles fonctionnalités

- Ajout de la prise en charge des points de terminaison privés, actuellement en préversion limitée.
- Liste élargie des codes de sortie pour azcmagent.
- Les paramètres de configuration de l’agent peuvent maintenant être lus à partir d’un fichier avec le paramètre `--config`.
- Collecter de nouvelles métadonnées d’instance pour déterminer si Microsoft SQL Server est installé sur le serveur

### <a name="fixed"></a>Fixe

Les vérifications des points de terminaison réseau sont maintenant plus rapides.

## <a name="december-2020"></a>Décembre 2020

Version : 1.3

### <a name="new-features"></a>Nouvelles fonctionnalités

Ajout de la prise en charge de Windows Server 2008 R2 SP1.

### <a name="fixed"></a>Fixe

Résolution d’un problème empêchant l’installation de l’extension de script personnalisé sur Linux.

## <a name="november-2020"></a>Novembre 2020

Version : 1.2

### <a name="fixed"></a>Fixe

Résolution d’un problème où la configuration du proxy pouvait être perdue après la mise à niveau des distributions RPM.

## <a name="october-2020"></a>Octobre 2020

Version : 1.1

### <a name="fixed"></a>Fixe

- Correction d’un script proxy pour gérer l’emplacement alternatif du fichier d’unité de démon GC.
- Modification de la fiabilité de l’agent GuestConfig.
- Prise en charge de l’agent GuestConfig pour la région US Gov Virginie.
- Les messages de rapport de l’extension de l’agent GuestConfig sont plus détaillés en cas d’échec.

## <a name="september-2020"></a>Septembre 2020

Version : 1.0 (Disponibilité générale)

### <a name="plan-for-change"></a>Modification planifiée

- La prise en charge des agents en préversion (toutes les versions antérieures à 1.0) sera supprimée dans une prochaine mise à jour de service.
- Suppression de la prise en charge du point de terminaison de secours `.azure-automation.net`. Si vous avez un proxy, vous devez autoriser le point de terminaison `*.his.arc.azure.com`.
- Si l’agent Connected Machine est installé sur une machine virtuelle hébergée dans Azure, il n’est pas possible d’installer ou de modifier les extensions de machine virtuelle à partir de la ressource des serveurs avec Arc. Cela permet d’éviter les opérations d’extension conflictuelles effectuées à partir des ressources **Microsoft.Compute** et **Microsoft.HybridCompute** de l’ordinateur virtuel. Utilisez la ressource **Microsoft.Compute** pour l’ordinateur pour toutes les opérations d’extension.
- Le nom du processus Configuration des invité a changé, passant de *gcd* à *gcad* sur Linux, et de *gcservice* à *gcarcservice* sur Windows.

### <a name="new-features"></a>Nouvelles fonctionnalités

- Ajout de l’option `azcmagent logs` pour collecter des informations à des fins de support.
- Ajout de l’option `azcmagent license` pour afficher le CLUF.
- Ajout de l’option `azcmagent show --json` pour sortir l’état de l’agent dans un format facile à analyser.
- Ajout de l’indicateur dans la sortie `azcmagent show` pour indiquer si le serveur se trouve sur une machine virtuelle hébergée dans Azure.
- Ajout de l’option `azcmagent disconnect --force-local-only` pour autoriser la réinitialisation de l’état de l’agent local lorsque le service Azure est inaccessible.
- Ajout de l’option `azcmagent connect --cloud` pour prendre en charge d’autres clouds. Dans cette version, seul Azure est pris en charge par le service au moment de la mise en production de l’agent.
- L’agent a été localisé dans les langues prises en charge par Azure.

### <a name="fixed"></a>Fixe

- Améliorations de la vérification de la connectivité.
- Correction du problème lié à la perte des paramètres du serveur proxy lors de la mise à niveau de l’agent sur Linux.
- Problèmes résolus lors de la tentative d’installation de l’agent sur un serveur exécutant Windows Server 2012 R2.
- Améliorations de la fiabilité d’installation des extensions

## <a name="august-2020"></a>Août 2020

Version : 0.11

- Cette mise en production a annoncé précédemment la prise en charge d’Ubuntu 20.04. Étant donné que certaines extensions de machine virtuelle Azure ne prennent pas en charge Ubuntu 20.04, la prise en charge de cette version d’Ubuntu est en cours de suppression.

- Améliorations de la fiabilité des déploiements d’extension.

### <a name="known-issues"></a>Problèmes connus

Si vous utilisez une version antérieure de l’agent Linux et qu’elle est configurée pour utiliser un serveur proxy, vous devez reconfigurer le paramètre de serveur proxy après la mise à niveau. Pour cela, exécutez `sudo azcmagent_proxy add http://proxyserver.local:83`.

## <a name="next-steps"></a>Étapes suivantes

- Avant d’évaluer ou d’activer des serveurs avec Azure Arc sur plusieurs machines hybrides, consultez [Vue d’ensemble de l’agent Machine connectée](agent-overview.md) pour en savoir plus sur les exigences et les détails techniques relatifs à l’agent, ainsi que sur les méthodes de déploiement.

- Consultez le [Guide de planification et de déploiement](plan-at-scale-deployment.md) pour planifier le déploiement de serveurs avec Azure Arc à n’importe quelle échelle et implémenter la gestion et la supervision centralisées.
