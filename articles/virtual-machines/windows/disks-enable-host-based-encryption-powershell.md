---
title: Azure PowerShell - Activer le chiffrement de bout en bout sur votre hôte de machine virtuelle
description: Activer le chiffrement de bout en bout sur vos machines virtuelles Azure à l'aide du chiffrement sur l'hôte
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 07/01/2021
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions, devx-track-azurepowershell
ms.openlocfilehash: 3417288a0ed765d1ff53c47282379ae6cbb85254
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122694692"
---
# <a name="use-the-azure-powershell-module-to-enable-end-to-end-encryption-using-encryption-at-host"></a>Utiliser le module Azure PowerShell pour activer le chiffrement de bout en bout à l’aide du chiffrement sur l’hôte

**S’applique à :** :heavy_check_mark : Machines virtuelles Windows 

Lorsque vous activez le chiffrement sur l'hôte, les données stockées sur l'hôte de machine virtuelle sont chiffrées au repos et les flux sont chiffrés dans le service de stockage. Pour obtenir des informations conceptuelles sur le chiffrement sur l'hôte ainsi que sur d'autres types de chiffrement de disques managés, consultez [Chiffrement sur l'hôte : chiffrement de bout en bout pour vos données de machine virtuelle](../disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data).

## <a name="restrictions"></a>Restrictions

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](../../../includes/virtual-machines-disks-encryption-at-host-restrictions.md)]


### <a name="supported-vm-sizes"></a>Tailles des machines virtuelles prises en charge

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](../../../includes/virtual-machines-disks-encryption-at-host-suported-sizes.md)]

Vous pouvez également rechercher les tailles de machine virtuelle par programmation. Pour savoir comment les récupérer par programmation, reportez-vous à la section [Rechercher les tailles de machine virtuelle prises en charge](#finding-supported-vm-sizes).

## <a name="prerequisites"></a>Prérequis

Vous devez activer la fonctionnalité pour votre abonnement avant d’utiliser la propriété EncryptionAtHost pour votre machine virtuelle/groupe de machines virtuelles identiques. Suivez les étapes ci-dessous pour activer la fonctionnalité pour votre abonnement :

1.  Exécutez la commande suivante pour inscrire la fonctionnalité pour votre abonnement

    ```powershell
     Register-AzProviderFeature -FeatureName "EncryptionAtHost" -ProviderNamespace "Microsoft.Compute" 
    ```

2.  Vérifiez que l’état de l’inscription est Inscrit (cela prend quelques minutes) à l’aide de la commande ci-dessous avant d’essayer la fonctionnalité.

    ```powershell
     Get-AzProviderFeature -FeatureName "EncryptionAtHost" -ProviderNamespace "Microsoft.Compute"  
    ```


### <a name="create-an-azure-key-vault-and-diskencryptionset"></a>Créer un coffre de clés Azure Key Vault et un jeu de chiffrement DiskEncryptionSet

Une fois la fonctionnalité activée, vous devez configurer un coffre de clés Azure Key Vault et un jeu de chiffrement DiskEncryptionSet, si ce n'est déjà fait.

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-powershell](../../../includes/virtual-machines-disks-encryption-create-key-vault-powershell.md)]

## <a name="enable-encryption-at-host-for-disks-attached-to-vm-and-virtual-machine-scale-sets"></a>Activer le chiffrement sur l'hôte pour les disques attachés à des machines virtuelles et à des groupes de machines virtuelles identiques

Vous pouvez activer le chiffrement sur l'hôte en définissant une nouvelle propriété EncryptionAtHost sous le profil securityProfile des machines virtuelles ou des groupes de machines virtuelles identiques à l'aide des versions **2020-06-01** et ultérieures de l'API.

`"securityProfile": { "encryptionAtHost": "true" }`

## <a name="examples"></a>Exemples

### <a name="create-a-vm-with-encryption-at-host-enabled-with-customer-managed-keys"></a>Créer une machine virtuelle avec chiffrement sur l’hôte activé avec des clés gérées par le client. 

Créez une machine virtuelle avec disques managés à l’aide de l’URI de ressource du DiskEncryptionSet créé précédemment afin de chiffrer le cache des disques de système d’exploitation et des disques de données avec des clés gérées par le client. Les disques temporaires sont chiffrés avec des clés gérées par la plateforme. 

```powershell
$VMLocalAdminUser = "yourVMLocalAdminUserName"
$VMLocalAdminSecurePassword = ConvertTo-SecureString <password> -AsPlainText -Force
$LocationName = "yourRegion"
$ResourceGroupName = "yourResourceGroupName"
$ComputerName = "yourComputerName"
$VMName = "yourVMName"
$VMSize = "yourVMSize"
$diskEncryptionSetName="yourdiskEncryptionSetName"
    
$NetworkName = "yourNetworkName"
$NICName = "yourNICName"
$SubnetName = "yourSubnetName"
$SubnetAddressPrefix = "10.0.0.0/24"
$VnetAddressPrefix = "10.0.0.0/16"
    
$SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix
$Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroupName -Location $LocationName -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet
$NIC = New-AzNetworkInterface -Name $NICName -ResourceGroupName $ResourceGroupName -Location $LocationName -SubnetId $Vnet.Subnets[0].Id
    
$Credential = New-Object System.Management.Automation.PSCredential ($VMLocalAdminUser, $VMLocalAdminSecurePassword);

# Enable encryption at host by specifying EncryptionAtHost parameter

$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize -EncryptionAtHost
$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

# Enable encryption with a customer managed key for OS disk by setting DiskEncryptionSetId property 

$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name $($VMName +"_OSDisk") -DiskEncryptionSetId $diskEncryptionSet.Id -CreateOption FromImage

# Add a data disk encrypted with a customer managed key by setting DiskEncryptionSetId property 

$VirtualMachine = Add-AzVMDataDisk -VM $VirtualMachine -Name $($VMName +"DataDisk1") -DiskSizeInGB 128 -StorageAccountType Premium_LRS -CreateOption Empty -Lun 0 -DiskEncryptionSetId $diskEncryptionSet.Id 
    
New-AzVM -ResourceGroupName $ResourceGroupName -Location $LocationName -VM $VirtualMachine -Verbose
```

### <a name="create-a-vm-with-encryption-at-host-enabled-with-platform-managed-keys"></a>Créer une machine virtuelle avec chiffrement sur l’hôte activé avec des clés gérées par la plateforme. 

Créez une machine virtuelle avec chiffrement sur l’hôte activé afin de chiffrer le cache des disques de système d’exploitation/de données et des disques temporaires avec des clés gérées par la plateforme. 

```powershell
$VMLocalAdminUser = "yourVMLocalAdminUserName"
$VMLocalAdminSecurePassword = ConvertTo-SecureString <password> -AsPlainText -Force
$LocationName = "yourRegion"
$ResourceGroupName = "yourResourceGroupName"
$ComputerName = "yourComputerName"
$VMName = "yourVMName"
$VMSize = "yourVMSize"
    
$NetworkName = "yourNetworkName"
$NICName = "yourNICName"
$SubnetName = "yourSubnetName"
$SubnetAddressPrefix = "10.0.0.0/24"
$VnetAddressPrefix = "10.0.0.0/16"
    
$SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix
$Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroupName -Location $LocationName -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet
$NIC = New-AzNetworkInterface -Name $NICName -ResourceGroupName $ResourceGroupName -Location $LocationName -SubnetId $Vnet.Subnets[0].Id
    
$Credential = New-Object System.Management.Automation.PSCredential ($VMLocalAdminUser, $VMLocalAdminSecurePassword);

# Enable encryption at host by specifying EncryptionAtHost parameter

$VirtualMachine = New-AzVMConfig -VMName $VMName -VMSize $VMSize -EncryptionAtHost

$VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate
$VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
$VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest

$VirtualMachine = Set-AzVMOSDisk -VM $VirtualMachine -Name $($VMName +"_OSDisk") -CreateOption FromImage

$VirtualMachine = Add-AzVMDataDisk -VM $VirtualMachine -Name $($VMName +"DataDisk1") -DiskSizeInGB 128 -StorageAccountType Premium_LRS -CreateOption Empty -Lun 0
    
New-AzVM -ResourceGroupName $ResourceGroupName -Location $LocationName -VM $VirtualMachine
```

### <a name="update-a-vm-to-enable-encryption-at-host"></a>Mettre à jour une machine virtuelle pour activer le chiffrement sur l’hôte. 

```powershell
$ResourceGroupName = "yourResourceGroupName"
$VMName = "yourVMName"

$VM = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName

Stop-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName -Force

Update-AzVM -VM $VM -ResourceGroupName $ResourceGroupName -EncryptionAtHost $true
```

### <a name="check-the-status-of-encryption-at-host-for-a-vm"></a>Vérifier l’état du chiffrement sur l’hôte pour une machine virtuelle

```powershell
$ResourceGroupName = "yourResourceGroupName"
$VMName = "yourVMName"

$VM = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName

$VM.SecurityProfile.EncryptionAtHost
```

### <a name="disable-encryption-at-host"></a>Désactiver le chiffrement sur l’hôte

Vous devez désallouer votre machine virtuelle pour pouvoir désactiver le chiffrement sur l’ordinateur hôte.

```powershell
$ResourceGroupName = "yourResourceGroupName"
$VMName = "yourVMName"

$VM = Get-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName

Stop-AzVM -ResourceGroupName $ResourceGroupName -Name $VMName -Force

Update-AzVM -VM $VM -ResourceGroupName $ResourceGroupName -EncryptionAtHost $false
```

### <a name="create-a-virtual-machine-scale-set-with-encryption-at-host-enabled-with-customer-managed-keys"></a>Créer un groupe de machines virtuelles identiques avec chiffrement sur l’hôte activé avec des clés gérées par le client. 

Créez un groupe de machines virtuelles identiques avec disques managés à l’aide de l’URI de ressource du DiskEncryptionSet créé précédemment afin de chiffrer le cache des disques de système d’exploitation et des disques de données avec des clés gérées par le client. Les disques temporaires sont chiffrés avec des clés gérées par la plateforme. 

```powershell
$VMLocalAdminUser = "yourLocalAdminUser"
$VMLocalAdminSecurePassword = ConvertTo-SecureString Password@123 -AsPlainText -Force
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$ComputerNamePrefix = "yourComputerNamePrefix"
$VMScaleSetName = "yourVMSSName"
$VMSize = "Standard_DS3_v2"
$diskEncryptionSetName="yourDiskEncryptionSetName"
    
$NetworkName = "yourVNETName"
$SubnetName = "yourSubnetName"
$SubnetAddressPrefix = "10.0.0.0/24"
$VnetAddressPrefix = "10.0.0.0/16"
    
$SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix

$Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroupName -Location $LocationName -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet

$ipConfig = New-AzVmssIpConfig -Name "myIPConfig" -SubnetId $Vnet.Subnets[0].Id 

# Enable encryption at host by specifying EncryptionAtHost parameter

$VMSS = New-AzVmssConfig -Location $LocationName -SkuCapacity 2 -SkuName $VMSize -UpgradePolicyMode 'Automatic' -EncryptionAtHost

$VMSS = Add-AzVmssNetworkInterfaceConfiguration -Name "myVMSSNetworkConfig" -VirtualMachineScaleSet $VMSS -Primary $true -IpConfiguration $ipConfig

$diskEncryptionSet=Get-AzDiskEncryptionSet -ResourceGroupName $ResourceGroupName -Name $diskEncryptionSetName

# Enable encryption with a customer managed key for the OS disk by setting DiskEncryptionSetId property 

$VMSS = Set-AzVmssStorageProfile $VMSS -OsDiskCreateOption "FromImage" -DiskEncryptionSetId $diskEncryptionSet.Id -ImageReferenceOffer 'WindowsServer' -ImageReferenceSku '2012-R2-Datacenter' -ImageReferenceVersion latest -ImageReferencePublisher 'MicrosoftWindowsServer'

$VMSS = Set-AzVmssOsProfile $VMSS -ComputerNamePrefix $ComputerNamePrefix -AdminUsername $VMLocalAdminUser -AdminPassword $VMLocalAdminSecurePassword

# Add a data disk encrypted with a customer managed key by setting DiskEncryptionSetId property 

$VMSS = Add-AzVmssDataDisk -VirtualMachineScaleSet $VMSS -CreateOption Empty -Lun 1 -DiskSizeGB 128 -StorageAccountType Premium_LRS -DiskEncryptionSetId $diskEncryptionSet.Id
```

### <a name="create-a-virtual-machine-scale-set-with-encryption-at-host-enabled-with-platform-managed-keys"></a>Créer un groupe de machines virtuelles identiques avec chiffrement sur l’hôte activé avec des clés gérées par la plateforme. 

Créez un groupe de machines virtuelles identiques avec chiffrement sur l’hôte activé afin de chiffrer le cache des disques de système d’exploitation/de données et des disques temporaires avec des clés gérées par la plateforme. 

```powershell
$VMLocalAdminUser = "yourLocalAdminUser"
$VMLocalAdminSecurePassword = ConvertTo-SecureString Password@123 -AsPlainText -Force
$LocationName = "westcentralus"
$ResourceGroupName = "yourResourceGroupName"
$ComputerNamePrefix = "yourComputerNamePrefix"
$VMScaleSetName = "yourVMSSName"
$VMSize = "Standard_DS3_v2"
    
$NetworkName = "yourVNETName"
$SubnetName = "yourSubnetName"
$SubnetAddressPrefix = "10.0.0.0/24"
$VnetAddressPrefix = "10.0.0.0/16"
    
$SingleSubnet = New-AzVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $SubnetAddressPrefix

$Vnet = New-AzVirtualNetwork -Name $NetworkName -ResourceGroupName $ResourceGroupName -Location $LocationName -AddressPrefix $VnetAddressPrefix -Subnet $SingleSubnet

$ipConfig = New-AzVmssIpConfig -Name "myIPConfig" -SubnetId $Vnet.Subnets[0].Id 

# Enable encryption at host by specifying EncryptionAtHost parameter

$VMSS = New-AzVmssConfig -Location $LocationName -SkuCapacity 2 -SkuName $VMSize -UpgradePolicyMode 'Automatic' -EncryptionAtHost

$VMSS = Add-AzVmssNetworkInterfaceConfiguration -Name "myVMSSNetworkConfig" -VirtualMachineScaleSet $VMSS -Primary $true -IpConfiguration $ipConfig
 
$VMSS = Set-AzVmssStorageProfile $VMSS -OsDiskCreateOption "FromImage" -ImageReferenceOffer 'WindowsServer' -ImageReferenceSku '2012-R2-Datacenter' -ImageReferenceVersion latest -ImageReferencePublisher 'MicrosoftWindowsServer'

$VMSS = Set-AzVmssOsProfile $VMSS -ComputerNamePrefix $ComputerNamePrefix -AdminUsername $VMLocalAdminUser -AdminPassword $VMLocalAdminSecurePassword

$VMSS = Add-AzVmssDataDisk -VirtualMachineScaleSet $VMSS -CreateOption Empty -Lun 1 -DiskSizeGB 128 -StorageAccountType Premium_LRS 

$Credential = New-Object System.Management.Automation.PSCredential ($VMLocalAdminUser, $VMLocalAdminSecurePassword);

New-AzVmss -VirtualMachineScaleSet $VMSS -ResourceGroupName $ResourceGroupName -VMScaleSetName $VMScaleSetName
```

### <a name="update-a-virtual-machine-scale-set-to-enable-encryption-at-host"></a>Mettre à jour un groupe de machines virtuelles identiques pour activer le chiffrement sur l’hôte. 

```powershell
$ResourceGroupName = "yourResourceGroupName"
$VMScaleSetName = "yourVMSSName"

$VMSS = Get-AzVmss -ResourceGroupName $ResourceGroupName -Name $VMScaleSetName

Update-AzVmss -VirtualMachineScaleSet $VMSS -Name $VMScaleSetName -ResourceGroupName $ResourceGroupName -EncryptionAtHost $true
```

### <a name="check-the-status-of-encryption-at-host-for-a-virtual-machine-scale-set"></a>Vérifier l’état du chiffrement sur l’hôte pour un groupe de machines virtuelles identiques

```powershell
$ResourceGroupName = "yourResourceGroupName"
$VMScaleSetName = "yourVMSSName"

$VMSS = Get-AzVmss -ResourceGroupName $ResourceGroupName -Name $VMScaleSetName

$VMSS.VirtualMachineProfile.SecurityProfile.EncryptionAtHost
```

### <a name="update-a-virtual-machine-scale-set-to-disable-encryption-at-host"></a>Mettre à jour un groupe de machines virtuelles identiques pour désactiver le chiffrement sur l’hôte. 

Vous pouvez désactiver le chiffrement sur l’hôte sur votre groupe de machines virtuelles identiques, mais cela n’affectera que les machines virtuelles créées après la désactivation du chiffrement sur l’ordinateur hôte. Pour les machines virtuelles existantes, vous devez désallouer la machine virtuelle, [désactiver le chiffrement à l’hôte sur cette machine virtuelle](#disable-encryption-at-host), puis réallouer la machine virtuelle.

```powershell
$ResourceGroupName = "yourResourceGroupName"
$VMScaleSetName = "yourVMSSName"

$VMSS = Get-AzVmss -ResourceGroupName $ResourceGroupName -Name $VMScaleSetName

Update-AzVmss -VirtualMachineScaleSet $VMSS -Name $VMScaleSetName -ResourceGroupName $ResourceGroupName -EncryptionAtHost $false
```

## <a name="finding-supported-vm-sizes"></a>Recherche des tailles de machine virtuelle prises en charge

Les tailles de machine virtuelle héritées ne sont pas prises en charge. Pour accéder à la liste des tailles de machines virtuelles prises en charge, procédez comme suit :

Appelez l'[API des références SKU de ressource](/rest/api/compute/resourceskus/list) et vérifiez que la capacité `EncryptionAtHostSupported` est définie sur **True**.

```json
    {
        "resourceType": "virtualMachines",
        "name": "Standard_DS1_v2",
        "tier": "Standard",
        "size": "DS1_v2",
        "family": "standardDSv2Family",
        "locations": [
        "CentralUSEUAP"
        ],
        "capabilities": [
        {
            "name": "EncryptionAtHostSupported",
            "value": "True"
        }
        ]
    }
```

Ou appelez la cmdlet PowerShell [Get-AzComputeResourceSku](/powershell/module/az.compute/get-azcomputeresourcesku).

```powershell
$vmSizes=Get-AzComputeResourceSku | where{$_.ResourceType -eq 'virtualMachines' -and $_.Locations.Contains('CentralUSEUAP')} 

foreach($vmSize in $vmSizes)
{
    foreach($capability in $vmSize.capabilities)
    {
        if($capability.Name -eq 'EncryptionAtHostSupported' -and $capability.Value -eq 'true')
        {
            $vmSize

        }

    }
}
```

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous avez créé et configuré ces ressources, vous pouvez les utiliser pour sécuriser vos disques managés. Le lien suivant contient des exemples de scripts, chacun correspondant à un scénario différent, que vous pouvez utiliser pour sécuriser vos disques managés.

[Exemples de modèles Azure Resource Manager](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/EncryptionAtHost)
