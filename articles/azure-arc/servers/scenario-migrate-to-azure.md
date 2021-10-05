---
title: Migrer un serveur avec Azure Arc vers Azure
description: Découvrez comment migrer vos serveurs avec Azure Arc s’exécutant localement ou dans un autre environnement cloud vers Azure.
ms.date: 07/16/2021
ms.topic: conceptual
ms.openlocfilehash: 0ed30fac28c3491a9772466aa193f721f4fa440f
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124815997"
---
# <a name="migrate-your-on-premises-or-other-cloud-azure-arc-enabled-server-to-azure"></a>Migrer votre serveur avec Azure Arc local ou sur un autre cloud vers Azure

Cet article a pour but de vous aider à planifier et à réussir la migration de votre machine virtuelle ou serveur local géré par des serveurs avec Azure Arc vers Azure. En suivant ces étapes, vous pouvez effectuer une transition de gestion à partir de serveurs Azure Arc en fonction des extensions de machines virtuelles prises en charge installées et des services Azure basés sur l’identité de la ressource du serveur avec Arc.

Avant d’effectuer ces étapes, consultez l’article Azure Migrate [Préparer des ordinateurs locaux à une migration vers Azure](../../migrate/prepare-for-migration.md) pour comprendre les conditions requises pour se préparer à utiliser Azure Migrate.

Dans cet article, vous découvrirez comment :

* Inventorier les extensions de machine virtuelle installées prises en charge par les serveurs avec Azure Arc.
* Désinstaller toutes les extensions de machine virtuelle du serveur Azure Arc.
* Identifier les services Azure configurés pour vous authentifier avec votre identité managée par le serveur Azure Arc et vous préparer à mettre à jour ces services afin d’utiliser l’identité de machine virtuelle Azure après la migration.
* Examiner les droits d’accès Azure RBAC (contrôle d’accès en fonction du rôle Azure) accordés à la ressource du serveur Azure Arc pour gérer les utilisateurs ayant accès à la ressource après sa migration vers une machine virtuelle Azure. 
* Supprimer d’Azure l’identité de la ressource du serveur Azure Arc et supprimer l’agent du serveur Azure Arc.
* Installer l’agent invité Azure.
* Migrer le serveur ou la machine virtuelle vers Azure.

## <a name="step-1-inventory-and-remove-vm-extensions"></a>Étape 1 : Inventorier et supprimer les extensions de machine virtuelle

Pour inventorier les extensions de machine virtuelle installées sur votre serveur Azure Arc, vous pouvez les lister à l’aide d’Azure CLI ou avec Azure PowerShell.

Avec Azure PowerShell, utilisez la commande [AzConnectedMachineExtension](/powershell/module/az.connectedmachine/get-azconnectedmachineextension) avec les paramètres `-MachineName` et `-ResourceGroupName`.

Avec Azure CLI, utilisez la commande [az connectedmachine extension list](/cli/azure/ext/connectedmachine/connectedmachine/extension#ext_connectedmachine_az_connectedmachine_extension_list) avec les paramètres `--machine-name` et `--resource-group`. Par défaut, la sortie des commandes Azure CLI est au format JSON (JavaScript Object Notation). Pour remplacer la sortie par défaut pour une liste ou une table, par exemple, utilisez [az configure --output](/cli/azure/reference-index). Vous pouvez également ajouter `--output` à n’importe quelle commande pour modifier ponctuellement le format de sortie.

Après avoir identifié les extensions de machine virtuelle déployées, vous pouvez les supprimer à l’aide du [portail Azure](manage-vm-extensions-portal.md), d’[Azure PowerShell](manage-vm-extensions-powershell.md) ou d’[Azure CLI](manage-vm-extensions-cli.md). Si l’extension de machine virtuelle Log Analytics ou Dependency Agent a été déployée à l’aide d’Azure Policy et de l’[initiative Insights de machine virtuelle](../../azure-monitor/vm/vminsights-enable-policy.md), il est nécessaire de [créer une exclusion](../../governance/policy/tutorials/create-and-manage.md#remove-a-non-compliant-or-denied-resource-from-the-scope-with-an-exclusion) pour empêcher la réévaluation et le déploiement des extensions sur le serveur Azure Arc avant la fin de la migration.

## <a name="step-2-review-access-rights"></a>Étape 2 : Vérifier les droits d’accès 

Listez les affectations de rôles pour la ressource de serveurs Azure Arc. En utilisant [Azure PowerShell](../../role-based-access-control/role-assignments-list-powershell.md#list-role-assignments-for-a-resource) et d’autres codes PowerShell, vous pouvez exporter les résultats au format CSV ou autre. 

Si vous utilisez une identité managée pour une application ou un processus s’exécutant sur un serveur Azure Arc, vous devez vous assurer qu’une identité managée est attribuée à la machine virtuelle Azure. Pour afficher l’attribution de rôle d’une identité managée, vous pouvez utiliser la cmdlet Azure PowerShell `Get-AzADServicePrincipal`. Pour plus d’informations, consultez [Lister les attributions de rôles pour une identité managée](../../role-based-access-control/role-assignments-list-powershell.md#list-role-assignments-for-a-managed-identity). 

Une identité gérée par le système est également utilisée lorsque Azure Policy est utilisé pour auditer ou configurer les paramètres d'une machine ou d'un serveur. Avec les serveurs Azure compatibles avec Arc, le service d'agent de configuration invité est inclus et effectue la validation des paramètres d'audit. Après la migration, consultez la section [Déployer les exigences pour les machines virtuelles Azure](../../governance/policy/concepts/guest-configuration.md#deploy-requirements-for-azure-virtual-machines) pour obtenir des informations sur la manière de configurer votre VM Azure manuellement ou à l'aide d'une stratégie avec l'extension de configuration d'invité.

Mettez à jour l’attribution de rôle avec toutes les ressources accessibles par l’identité managée pour permettre à la nouvelle identité de machine virtuelle Azure de s’authentifier auprès de ces services. Consultez ce qui suit pour savoir [comment les identités managées pour les ressources Azure fonctionnent pour une machine virtuelle Azure](../../active-directory/managed-identities-azure-resources/how-managed-identities-work-vm.md).

## <a name="step-3-disconnect-from-azure-arc-and-uninstall-agent"></a>Étape 3 : Se déconnecter d’Azure Arc et désinstaller l’agent

Supprimez l’ID de ressource du serveur Azure Arc dans Azure à l’aide de l’une des méthodes suivantes :

   * Exécutez la commande `azcmagent disconnect` sur la machine ou le serveur.

   * Sur le serveur Azure Arc inscrit que vous avez sélectionné dans le Portail Azure, sélectionnez **Supprimer** dans la barre supérieure.

   * Utilisez [Azure CLI](../../azure-resource-manager/management/delete-resource-group.md?tabs=azure-cli#delete-resource) ou [Azure PowerShell](../../azure-resource-manager/management/delete-resource-group.md?tabs=azure-powershell#delete-resource). Pour le paramètre`ResourceType`, utilisez `Microsoft.HybridCompute/machines`.

Ensuite, supprimez l’agent Windows ou Linux des serveurs avec Azure Arc en suivant les étapes de la section [Supprimer l’agent](manage-agent.md#remove-the-agent).

## <a name="step-4-install-the-azure-guest-agent"></a>Étape 4 : Installer l’agent invité Azure

L’agent invité Azure Linux ou Windows n’est pas installé sur la machine virtuelle migrée vers Azure à partir d’un emplacement local. Dans ces scénarios, vous devez installer manuellement l’agent de machine virtuelle. Pour plus d’informations sur l’installation de l’agent de machine virtuelle, consultez [Vue d’ensemble de l’agent Windows de machine virtuelle Azure](../../virtual-machines/extensions/agent-windows.md) ou [Vue d’ensemble de l’agent Linux de machine virtuelle Azure](../../virtual-machines/extensions/agent-linux.md).

## <a name="step-5-migrate-server-or-machine-to-azure"></a>Étape 5 : Migrer un serveur ou un ordinateur vers Azure

Avant de procéder à la migration avec Azure Migration, consultez l’article [Préparer des ordinateurs locaux à une migration vers Azure](../../migrate/prepare-for-migration.md) pour en savoir plus sur les conditions requises pour utiliser Azure Migrate. Pour terminer la migration vers Azure, consultez les [options de migration](../../migrate/prepare-for-migration.md#next-steps) Azure Migrate en fonction de votre environnement.

## <a name="step-6-deploy-azure-vm-extensions"></a>Étape 6 : Déployer les extensions de machine virtuelle Azure

Après la migration et la réalisation de toutes les étapes de configuration post-migration, vous pouvez déployer les extensions de machine virtuelle Azure en fonction des extensions de machine virtuelle installées à l’origine sur votre serveur Azure Arc. Consultez les [fonctionnalités et extensions de machine virtuelle Azure](../../virtual-machines/extensions/overview.md) pour vous aider à planifier le déploiement de vos extensions. 

Pour reprendre l'utilisation des paramètres d'audit à l'intérieur d'une machine avec des définitions de politique de configuration d'invité, voir [Activer la configuration d'invité](../../governance/policy/concepts/guest-configuration.md#enable-guest-configuration).

Si l’extension de machine virtuelle Log Analytics ou l’extension de machine virtuelle Dependency Agent a été déployée à l’aide d’Azure Policy et de l’[initiative VM Insights](../../azure-monitor/vm/vminsights-enable-policy.md), supprimez l’[exclusion](../../governance/policy/tutorials/create-and-manage.md#remove-a-non-compliant-or-denied-resource-from-the-scope-with-an-exclusion) créée précédemment. Pour utiliser Azure Policy afin d’activer des machines virtuelles Azure, consultez [Déployer Azure Monitor à grande échelle à l’aide d’Azure Policy](../../azure-monitor/deploy-scale.md#vm-insights). 

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la résolution des problèmes, consultez le guide [Résoudre les problèmes de l’agent Connected Machine](troubleshoot-agent-onboard.md).
