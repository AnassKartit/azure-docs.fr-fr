---
title: Créer une machine virtuelle Windows à partir d’un modèle dans Azure
description: Utilisez un modèle Resource Manager et PowerShell pour créer facilement une machine virtuelle Windows.
author: cynthn
ms.service: virtual-machines
ms.topic: how-to
ms.date: 03/22/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017, devx-track-azurepowershell
ms.openlocfilehash: 1be03bee5005cbcc4b2eec3d469631d504d3c504
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122690152"
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Créer une machine virtuelle Windows à partir d’un modèle Resource Manager

**S’applique à :** :heavy_check_mark : Machines virtuelles Windows 

Découvrez comment créer une machine virtuelle Windows en utilisant un modèle Azure Resource Manager et Azure PowerShell à partir d’Azure Cloud Shell. Le modèle utilisé dans cet article déploie une machine virtuelle unique exécutant Windows Server dans un nouveau réseau virtuel constitué d’un seul sous-réseau. Pour créer une machine virtuelle Linux, consultez [Guide pratique pour créer une machine virtuelle Linux avec des modèles Azure Resource Manager](../linux/create-ssh-secured-vm-from-template.md).

Une alternative consiste à déployer le modèle à partir du portail Azure. Pour ouvrir le modèle dans le portail, sélectionnez le bouton **Déployer sur Azure**.

[![Déployer sur Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fquickstarts%2Fmicrosoft.compute%2Fvm-simple-windows%2Fazuredeploy.json)

## <a name="create-a-virtual-machine"></a>Création d'une machine virtuelle

La création d'une machine virtuelle Azure s'effectue généralement en deux étapes :

- Créez un groupe de ressources. Un groupe de ressources Azure est un conteneur logique dans lequel les ressources Azure sont déployées et gérées. Un groupe de ressources doit être créé avant les machines virtuelles.
- Création d’une machine virtuelle

L'exemple suivant crée une machine virtuelle à partir d'un [modèle de démarrage rapide Azure](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.compute/vm-simple-windows/azuredeploy.json). Voici une copie du modèle :

[!code-json[create-windows-vm](~/quickstart-templates/quickstarts/microsoft.compute/vm-simple-windows/azuredeploy.json)]

Pour exécuter le script PowerShell, sélectionnez **Essayer** pour ouvrir Azure Cloud Shell. Pour coller le script, cliquez avec le bouton droit dans l’interpréteur de commandes, puis sélectionnez **Coller** :

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUsername = Read-Host -Prompt "Enter the administrator username"
$adminPassword = Read-Host -Prompt "Enter the administrator password" -AsSecureString
$dnsLabelPrefix = Read-Host -Prompt "Enter an unique DNS name for the public IP"

New-AzResourceGroup -Name $resourceGroupName -Location "$location"
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.compute/vm-simple-windows/azuredeploy.json" `
    -adminUsername $adminUsername `
    -adminPassword $adminPassword `
    -dnsLabelPrefix $dnsLabelPrefix

 (Get-AzVm -ResourceGroupName $resourceGroupName).name

```

Si vous choisissez d’installer et d’utiliser PowerShell localement et non à partir d’Azure Cloud Shell, ce tutoriel vous impose d’utiliser le module Azure PowerShell. Exécutez `Get-Module -ListAvailable Az` pour trouver la version. Si vous devez effectuer une mise à niveau, consultez [Installer le module Azure PowerShell](/powershell/azure/install-az-ps). Si vous exécutez PowerShell en local, vous devez également exécuter `Connect-AzAccount` pour créer une connexion avec Azure.

Dans l’exemple précédent, vous avez spécifié un modèle stocké dans GitHub. Vous pouvez également télécharger ou créer un modèle, et spécifier le chemin d’accès local avec le paramètre `--template-file`.

Voici quelques ressources supplémentaires :

- Pour savoir comment développer des modèles Resource Manager, consultez la [documentation Azure Resource Manager](../../azure-resource-manager/index.yml).
- Pour examiner les schémas des machines virtuelles Azure, consultez [Informations de référence sur les modèles Azure](/azure/templates/microsoft.compute/allversions).
- Pour accéder à d'autres exemples de modèles de machine virtuelle, consultez [Modèles de démarrage rapide Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Compute&pageNumber=1&sort=Popular).

## <a name="connect-to-the-virtual-machine"></a>Connectez-vous à la machine virtuelle.

La dernière commande PowerShell du script précédent affiche le nom de la machine virtuelle. Pour vous connecter à la machine virtuelle, consultez [Guide pratique pour se connecter à une machine virtuelle Azure exécutant Windows](./connect-logon.md).

## <a name="next-steps"></a>Étapes suivantes

- Si vous rencontrez des problèmes lors du déploiement, nous vous conseillons de consulter la rubrique [Résolution des erreurs courantes dans un déploiement Azure avec Azure Resource Manager](../../azure-resource-manager/templates/common-deployment-errors.md).
- Découvrez comment créer et gérer une machine virtuelle dans la rubrique [Créer et gérer des machines virtuelles Windows avec le module Azure PowerShell](tutorial-manage-vm.md).

Pour en savoir plus sur la création de modèles, consultez la syntaxe et les propriétés JSON des types de ressource que vous avez déployés :

- [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
- [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
- [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
- [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)
