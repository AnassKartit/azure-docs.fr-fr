---
title: Exemple de script Azure CLI - Démarrer une machine virtuelle dans un labo
description: Ce script Azure CLI démarre une machine virtuelle dans un laboratoire dans Azure DevTest Labs.
ms.devlang: azurecli
ms.topic: sample
ms.date: 08/11/2020
ms.openlocfilehash: f28427c13587af7654f56fb728a44ea42cce5f5a
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128557559"
---
# <a name="use-azure-cli-to-start-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>Utiliser Azure CLI pour démarrer une machine virtuelle dans un laboratoire dans Azure DevTest Labs

Ce script Azure CLI démarre une machine virtuelle dans un laboratoire. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exemple de script

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/start-connect-virtual-machine-in-lab/start-connect-virtual-machine-in-lab.sh "Start a VM")]


## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes :

| Commande | Notes |
|---|---|
| [az lab vm start](/cli/azure/lab/vm#az_lab_vm_start) | Démarre une machine virtuelle dans un laboratoire. Cet opérateur peut prendre un certain temps. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](/cli/azure).

D’autres exemples de scripts CLI Azure Lab Services sont disponibles dans les [exemples CLI pour Azure Lab Services](../samples-cli.md).
