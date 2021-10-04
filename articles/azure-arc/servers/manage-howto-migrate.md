---
title: Comment migrer des serveurs avec Azure Arc entre différentes régions
description: Découvrez comment migrer un serveur avec Azure Arc d’une région à une autre.
ms.date: 07/16/2021
ms.topic: conceptual
ms.openlocfilehash: ea44fbf060588ca3859e7f0d51b7d7449dc318fa
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124807391"
---
# <a name="how-to-migrate-azure-arc-enabled-servers-across-regions"></a>Comment migrer des serveurs avec Azure Arc entre différentes régions

Il existe des scénarios dans lesquels vous pouvez être amené à déplacer votre serveur avec Azure Arc existant d’une région à une autre. Par exemple, vous avez réalisé que la machine a été inscrite dans une région incorrecte, vous souhaitez améliorer la facilité de gestion ou le déplacement répond à des impératifs de gouvernance.

Pour migrer un serveur avec Azure Arc d’une région Azure vers une autre, vous devez désinstaller les extensions de machine virtuelle, supprimer la ressource dans Azure, puis la recréer dans l’autre région. Avant d’effectuer ces étapes, vous devez auditer la machine pour vérifier quelles extensions de machine virtuelle y sont installées.

> [!NOTE]
> Si les extensions installées continuent de s’exécuter et d’effectuer leurs opérations normales une fois cette procédure terminée, vous ne pourrez pas les gérer. Si vous tentez de redéployer les extensions sur la machine, il se peut que vous rencontriez un comportement imprévisible.

## <a name="move-machine-to-other-region"></a>Déplacer la machine vers une autre région

> [!NOTE]
> Cette opération entraîne un temps d’arrêt au cours de la migration.

1. Supprimez les extensions de machine virtuelle installées du [portail Azure](manage-vm-extensions-portal.md#uninstall-extensions) à l’aide d’[Azure CLI](manage-vm-extensions-cli.md#remove-an-installed-extension) ou d’[Azure PowerShell](manage-vm-extensions-powershell.md#remove-an-installed-extension).

2. Utilisez l’outil **azcmagent** avec le paramètre [Disconnect](manage-agent.md#disconnect) pour déconnecter la machine d’Azure Arc et supprimer la ressource machine d’Azure. La déconnexion de la machine des serveurs avec Azure Arc n’a pas pour effet de supprimer l’agent Connected Machine et vous n’avez pas besoin de supprimer l’agent dans le cadre de ce processus. Vous pouvez effectuer cette opération manuellement quand vous êtes connecté de manière interactive, l’automatiser à l’aide du principal de service utilisé pour intégrer plusieurs agents ou utiliser un [jeton d’accès](../../active-directory/develop/access-tokens.md) de la plateforme d’identités Microsoft. Si vous n’avez pas utilisé de principal de service pour inscrire la machine auprès des serveurs avec Azure Arc, consultez l’[article](onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) suivant pour créer un principal de service.

3. Réinscrivez l’agent Connected Machine auprès des serveurs avec Azure Arc dans l’autre région. Exécutez l’outil `azcmagent` avec le paramètre [Connect](manage-agent.md#connect) pour effectuer cette étape.

4. Redéployez les extensions de machine virtuelle déployées à l’origine sur la machine à partir de serveurs avec Azure Arc. Si vous avez déployé l’agent Azure Monitor pour machines virtuelles (insights) ou l’agent Log Analytics à l’aide d’une définition de stratégie Azure, les agents sont redéployés après le [cycle d’évaluation](../../governance/policy/how-to/get-compliance-data.md#evaluation-triggers) suivant.

## <a name="next-steps"></a>Étapes suivantes

* Pour plus d’informations sur la résolution des problèmes, consultez le [guide Résoudre les problèmes de l’agent Connected Machine](troubleshoot-agent-onboard.md).

* Apprenez à gérer votre machine à l’aide de [Azure Policy](../../governance/policy/overview.md), par exemple pour la [configuration invité](../../governance/policy/concepts/guest-configuration.md) des machines virtuelles, en vérifiant que la machine crée des rapports sur l’espace de travail Log Analytics prévu, pour activer l’analyse avec la stratégie [Insights de machines virtuelles](../../azure-monitor/vm/vminsights-enable-policy.md) et bien plus encore.
