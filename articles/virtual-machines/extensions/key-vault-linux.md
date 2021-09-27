---
title: Extension de machine virtuelle Azure Key Vault pour Linux
description: Déployez un agent réalisant une actualisation automatique des certificats Key Vault sur des machines virtuelles à l’aide d’une extension de machine virtuelle.
services: virtual-machines
author: msmbaldwin
tags: keyvault
ms.service: virtual-machines
ms.subservice: extensions
ms.collection: linux
ms.topic: article
ms.date: 12/02/2019
ms.author: mbaldwin
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: a55a49232e18c61f1c5b1915c06cd61e1f13ab0b
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128674430"
---
# <a name="key-vault-virtual-machine-extension-for-linux"></a>Extension de machine virtuelle Key Vault pour Linux

L’extension de machine virtuelle Key Vault assure l’actualisation automatique des certificats stockés dans un coffre de clés Azure. Plus précisément, l’extension surveille une liste de certificats observés stockés dans des coffres de clés.  Lors de la détection d’une modification, l’extension récupère et installe les certificats correspondants. Actuellement, l’extension de machine virtuelle Key Vault est publiée et prise en charge par Microsoft sur les machines virtuelles Linux. Ce document présente les plateformes, configurations et options de déploiement qui sont prises en charge pour l’extension de machine virtuelle Key Vault pour Linux. 

### <a name="operating-system"></a>Système d’exploitation

L’extension de machine virtuelle Key Vault prend en charge les distributions Linux suivantes :

- Ubuntu-1804
- Suse-15 
- [CBL-Mariner](https://github.com/microsoft/CBL-Mariner)

> [!NOTE]
> Pour bénéficier de fonctionnalités de sécurité étendues, préparez la mise à niveau des systèmes Ubuntu 16.04 et Debian 9, car ces versions sont en train d’atteindre la fin de la période de support désignée.
> 

### <a name="supported-certificate-content-types"></a>Types de contenu de certificat pris en charge

- PKCS#12
- PEM


## <a name="prerequisities"></a>Conditions préalables
  - Instance Key Vault avec un certificat. Consultez [Créer un Key Vault](../../key-vault/general/quick-create-portal.md)
  - Une [identité managée](../../active-directory/managed-identities-azure-resources/overview.md) doit être attribuée à la machine virtuelle/VMSS
  - La stratégie d’accès à Key Vault doit être définie avec des secrets `get` et l’autorisation `list` pour l’identité managée de machine virtuelle/VMSS afin de récupérer la partie du secret d’un certificat. Consultez [Comment s’authentifier auprès de Key Vault](../../key-vault/general/authentication.md) et [Attribuer une stratégie d’accès Key Vault](../../key-vault/general/assign-access-policy-cli.md).
  -  VMSS doit avoir le paramètre d’identité suivant : ` 
  "identity": {
  "type": "UserAssigned",
  "userAssignedIdentities": {
  "[parameters('userAssignedIdentityResourceId')]": {}
  }
  }
  `
  
 - L’extension AKV doit avoir ce paramètre : `
                 "authenticationSettings": {
                    "msiEndpoint": "[parameters('userAssignedIdentityEndpoint')]",
                    "msiClientId": "[reference(parameters('userAssignedIdentityResourceId'), variables('msiApiVersion')).clientId]"
                  }
   `
## <a name="key-vault-vm-extension-version"></a>Version d’extension de machine virtuelle Key Vault
* Les utilisateurs Ubuntu-18.04 et SUSE-15 peuvent choisir de mettre à niveau leur version d’extension de machine virtuelle du coffre de clés vers `V2.0` pour utiliser la fonctionnalité de téléchargement de chaîne d’approbation complète. Les certificats d’émetteur (intermédiaire et racine) seront ajoutés au certificat feuille dans le fichier PEM.

* Si vous préférez mettre à niveau vers `v2.0`, vous devez d'abord supprimer `v1.0`, puis installer `v2.0`.
```
  az vm extension delete --name KeyVaultForLinux --resource-group ${resourceGroup} --vm-name ${vmName}
  az vm extension set -n "KeyVaultForLinux" --publisher Microsoft.Azure.KeyVault --resource-group "${resourceGroup}" --vm-name "${vmName}" –settings .\akvvm.json –version 2.0
```  
  L’indicateur --version 2.0 est facultatif, car la version la plus récente sera installée par défaut.   

* Si la machine virtuelle dispose de certificats téléchargés par v1.0, la suppression de l’extension AKVVM v1.0 ne supprimera PAS les certificats téléchargés.  Après l’installation de v2.0, les certificats existants ne seront PAS modifiés.  Vous devez supprimer les fichiers de certificat ou restaurer le certificat pour obtenir le fichier PEM avec une chaîne complète sur la machine virtuelle.




## <a name="extension-schema"></a>Schéma d’extensions

L’extrait JSON suivant illustre le schéma de l’extension de machine virtuelle Key Vault. L’extension ne nécessite aucun paramètre protégé, car tous ses paramètres sont considérés comme des informations sans impact sur la sécurité. Par contre, la liste des secrets surveillés, la fréquence d’interrogation et le magasin de certificats de destination lui sont indispensables. Plus précisément :  
```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KVVMExtensionForLinux",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForLinux",
      "typeHandlerVersion": "2.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
        "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g. "3600">,
          "certificateStoreName": <It is ignored on Linux>,
          "linkOnRenewal": <Not available on Linux e.g.: false>,
          "certificateStoreLocation": <disk path where certificate is stored, default: "/var/lib/waagent/Microsoft.Azure.KeyVault">,
          "requireInitialSync": <initial synchronization of certificates e..g: true>,
          "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: ["https://myvault.vault.azure.net/secrets/mycertificate", "https://myvault.vault.azure.net/secrets/mycertificate2"]>
        },
        "authenticationSettings": {
                "msiEndpoint":  <Optional MSI endpoint e.g.: "http://169.254.169.254/metadata/identity">,
                "msiClientId":  <Optional MSI identity e.g.: "c7373ae5-91c2-4165-8ab6-7381d6e75619">
        }
       }
      }
    }
```

> [!NOTE]
> Vos URL de certificats observés doivent être de la forme `https://myVaultName.vault.azure.net/secrets/myCertName`.
> 
> Cela est dû au fait que le chemin `/secrets` retourne le certificat complet, y compris la clé privée, contrairement au chemin `/certificates`. Vous trouverez plus d’informations sur les certificats ici : [Certificats Key Vault](../../key-vault/general/about-keys-secrets-certificates.md)

> [!IMPORTANT]
> La propriété 'authenticationSettings' est **requise** uniquement pour les machines virtuelles avec des **identités affectées par l’utilisateur**.
> Elle spécifie l'identité à utiliser pour l'authentification auprès de Key Vault.


### <a name="property-values"></a>Valeurs de propriétés

| Nom | Valeur/Exemple | Type de données |
| ---- | ---- | ---- |
| apiVersion | 2019-07-01 | Date |
| publisher | Microsoft.Azure.KeyVault | string |
| type | KeyVaultForLinux | string |
| typeHandlerVersion | 2.0 | int |
| pollingIntervalInS | 3600 | string |
| certificateStoreName | Ignoré sur Linux | string |
| linkOnRenewal | false | boolean |
| certificateStoreLocation  | /var/lib/waagent/Microsoft.Azure.KeyVault | string |
| requireInitialSync | true | boolean |
| observedCertificates  | ["https://myvault.vault.azure.net/secrets/mycertificate", "https://myvault.vault.azure.net/secrets/mycertificate2"] | tableau de chaînes
| msiEndpoint | http://169.254.169.254/metadata/identity | string |
| msiClientId | c7373ae5-91c2-4165-8ab6-7381d6e75619 | string |


## <a name="template-deployment"></a>Déploiement de modèle

Les extensions de machines virtuelles Azure peuvent être déployées avec des modèles Azure Resource Manager. Les modèles sont idéaux lorsque vous déployez une ou plusieurs machines virtuelles nécessitant une actualisation de certificats après le déploiement. Vous pouvez déployer l’extension sur des machines virtuelles individuelles ou dans des groupes de machines virtuelles identiques. La configuration et le schéma sont communs aux deux types de modèle. 

La configuration JSON d’une extension de machine virtuelle doit être imbriquée dans le fragment de la ressource de machine virtuelle du modèle, plus précisément dans l’objet `"resources": []` de la machine virtuelle et, dans le cas d’un groupe de machines virtuelles identiques, sous l’objet `"virtualMachineProfile":"extensionProfile":{"extensions" :[]`.

 > [!NOTE]
> L’extension de machine virtuelle nécessite l’attribution d’une identité managée par le système ou l’utilisateur pour s’authentifier auprès du coffre de clés.  Consultez [Comment s’authentifier auprès de Key Vault et attribuer une stratégie d’accès Key Vault.](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)
> 

```json
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "KeyVaultForLinux",
      "apiVersion": "2019-07-01",
      "location": "<location>",
      "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/', <vmName>)]"
      ],
      "properties": {
      "publisher": "Microsoft.Azure.KeyVault",
      "type": "KeyVaultForLinux",
      "typeHandlerVersion": "2.0",
      "autoUpgradeMinorVersion": true,
      "settings": {
          "secretsManagementSettings": {
          "pollingIntervalInS": <polling interval in seconds, e.g. "3600">,
          "certificateStoreName": <ingnored on linux>,
          "certificateStoreLocation": <disk path where certificate is stored, default: "/var/lib/waagent/Microsoft.Azure.KeyVault">,
          "observedCertificates": <list of KeyVault URIs representing monitored certificates, e.g.: "https://myvault.vault.azure.net/secrets/mycertificate"
        }      
      }
      }
    }
```

### <a name="extension-dependency-ordering"></a>Tri des dépendances d’extension
L’extension de machine virtuelle Key Vault prend en charge le tri des extensions si elle est configurée. Par défaut, l’extension signale qu’elle a démarré correctement dès qu’elle a démarré l’interrogation. Toutefois, elle peut être configurée de façon à attendre d’avoir téléchargé la liste complète des certificats avant de signaler un démarrage réussi. Si d’autres extensions dépendent de l’installation du jeu complet de certificats avant de démarrer, alors l’activation de ce paramètre permettra à ces extensions de déclarer une dépendance vis-à-vis de l’extension Key Vault. Cela empêchera ces extensions de démarrer avant que tous les certificats dont elles dépendent aient été installés. L’extension retentera le téléchargement initial indéfiniment et restera à l’état `Transitioning`.

Pour activer cette fonction, définissez ce qui suit :
```
"secretsManagementSettings": {
    "requireInitialSync": true,
    ...
}
```
> [Note] L’utilisation de cette fonctionnalité n’est pas compatible avec un modèle ARM qui crée une identité affectée par le système et met à jour une stratégie d’accès Key Vault avec celle-ci. Cela conduit à une impasse, car la stratégie d’accès du coffre ne peut pas être mise à jour tant que toutes les extensions n’ont pas démarré. Vous devez utiliser à la place une *identité MSI unique affectée par l’utilisateur* et inscrire vos coffres au préalable sur une liste de contrôle d’accès en utilisant cette identité avant de les déployer.

## <a name="azure-powershell-deployment"></a>Déploiement d’Azure PowerShell
> [!WARNING]
> Les clients PowerShell ajoutent souvent `\` à `"` dans settings.json, ce qui entraîne l’échec de akvvm_service avec l’erreur suivante : `[CertificateManagementConfiguration] Failed to parse the configuration settings with:not an object.`

Azure PowerShell vous permet de déployer l’extension de machine virtuelle Key Vault sur une machine virtuelle ou un groupe de machines virtuelles identiques existants. 

* Pour déployer l’extension sur une machine virtuelle :
    
    ```powershell
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCert1> + '","' + <observedCert2> + '"] } }'
        $extName =  "KeyVaultForLinux"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForLinux"
       
    
        # Start the deployment
        Set-AzVmExtension -TypeHandlerVersion "2.0" -ResourceGroupName <ResourceGroupName> -Location <Location> -VMName <VMName> -Name $extName -Publisher $extPublisher -Type $extType -SettingString $settings
    
    ```

* Pour déployer l’extension sur un groupe de machines virtuelles identiques :

    ```powershell
    
        # Build settings
        $settings = '{"secretsManagementSettings": 
        { "pollingIntervalInS": "' + <pollingInterval> + 
        '", "certificateStoreName": "' + <certStoreName> + 
        '", "certificateStoreLocation": "' + <certStoreLoc> + 
        '", "observedCertificates": ["' + <observedCert1> + '","' + <observedCert2> + '"] } }'
        $extName = "KeyVaultForLinux"
        $extPublisher = "Microsoft.Azure.KeyVault"
        $extType = "KeyVaultForLinux"
        
        # Add Extension to VMSS
        $vmss = Get-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName>
        Add-AzVmssExtension -VirtualMachineScaleSet $vmss  -Name $extName -Publisher $extPublisher -Type $extType -TypeHandlerVersion "2.0" -Setting $settings

        # Start the deployment
        Update-AzVmss -ResourceGroupName <ResourceGroupName> -VMScaleSetName <VmssName> -VirtualMachineScaleSet $vmss 
    
    ```

## <a name="azure-cli-deployment"></a>Déploiement de l’interface de ligne de commande Azure

L’interface de ligne de commande Azure permet de déployer l’extension de machine virtuelle Key Vault sur une machine virtuelle ou un groupe de machines virtuelles identiques existants. 
 
* Pour déployer l’extension sur une machine virtuelle :
    
    ```azurecli
       # Start the deployment
         az vm extension set -n "KeyVaultForLinux" `
         --publisher Microsoft.Azure.KeyVault `
         -g "<resourcegroup>" `
         --vm-name "<vmName>" `
         --version 2.0 `
         --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\" <observedCert1> \", \" <observedCert2> \"] }}'
    ```

* Pour déployer l’extension sur un groupe de machines virtuelles identiques :

   ```azurecli
        # Start the deployment
        az vmss extension set -n "KeyVaultForLinux" `
        --publisher Microsoft.Azure.KeyVault `
        -g "<resourcegroup>" `
        --vmss-name "<vmssName>" `
        --version 2.0 `
        --settings '{\"secretsManagementSettings\": { \"pollingIntervalInS\": \"<pollingInterval>\", \"certificateStoreName\": \"<certStoreName>\", \"certificateStoreLocation\": \"<certStoreLoc>\", \"observedCertificates\": [\" <observedCert1> \", \" <observedCert2> \"] }}'
    ```
Tenez compte des restrictions et conditions suivantes :
- Restrictions de Key Vault :
  - Doit exister au moment du déploiement 
  - La stratégie d’accès Key Vault doit être définie pour l’identité VM/VMSS à l’aide d’une identité managée. Consultez [Comment s’authentifier auprès de Key Vault](../../key-vault/general/authentication.md) et [Attribuer une stratégie d’accès Key Vault](../../key-vault/general/assign-access-policy-cli.md).


## <a name="troubleshoot-and-support"></a>Dépannage et support

Vous pouvez récupérer des données sur l’état des déploiements des extensions à partir du portail Azure et par le biais d’Azure PowerShell. Pour voir l’état de déploiement des extensions d’une machine virtuelle donnée, exécutez la commande suivante à l’aide d’Azure PowerShell.

**Azure PowerShell**
```powershell
Get-AzVMExtension -VMName <vmName> -ResourceGroupname <resource group name>
```

**Azure CLI**
```azurecli
 az vm get-instance-view --resource-group <resource group name> --name  <vmName> --query "instanceView.extensions"
```
### <a name="logs-and-configuration"></a>Journaux et configuration

Les journaux d’extension de machines virtuelles Key Vault existent uniquement localement sur la machine virtuelle et sont plus instructifs lorsqu’il s’agit de résoudre des problèmes.

|Emplacement|Description|
|--|--|
| /var/log/waagent.log  | Indique quand une mise à jour de l’extension s’est produite. |
| /var/log/azure/Microsoft.Azure.KeyVault.KeyVaultForLinux/*    | Examinez les journaux du service d’extension de machines virtuelles Key Vault pour déterminer l’état du service akvvm_service et du télécharger du certificat. L’emplacement de téléchargement des fichiers PEM se trouve également dans ces fichiers avec une entrée appelée nom du fichier de certificat. Si certificateStoreLocation n’est pas spécifié, sa valeur par défaut est /var/lib/waagent/Microsoft.Azure.KeyVault.Store/ |
| /var/lib/waagent/Microsoft.Azure.KeyVault.KeyVaultForLinux-\<most recent version\>/config/*   | La configuration et fichiers binaires pour le service d’extension de machines virtuelles Key Vault. |
|||
  
### <a name="using-symlink"></a>Liens symboliques

Les liens symboliques (« symkink ») sont en quelque sorte des raccourcis avancés. Vous pouvez utiliser ce lien symbolique `([VaultName].[CertificateName])` pour récupérer automatiquement la dernière version du certificat sur Linux sans avoir à analyser le dossier.

### <a name="frequently-asked-questions"></a>Forum Aux Questions (FAQ)

* Le nombre de observedCertificates que vous pouvez configurer est-il limité ?
  Non, l'extension de machine virtuelle Key Vault ne limite pas le nombre de observedCertificates.
  

### <a name="support"></a>Support

Si vous avez besoin d’une aide supplémentaire à quelque étape que ce soit dans cet article, vous pouvez contacter les experts Azure sur les [forums MSDN Azure et Stack Overflow](https://azure.microsoft.com/support/forums/). Vous pouvez également signaler un incident au support Azure. Accédez au [site du support Azure](https://azure.microsoft.com/support/options/) , puis cliquez sur Obtenir un support. Pour plus d’informations sur l’utilisation du support Azure, lisez le [FAQ du support Microsoft Azure](https://azure.microsoft.com/support/faq/).
