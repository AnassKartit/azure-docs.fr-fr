---
title: Utilisation de la configuration d’état souhaité avec des groupes de machines virtuelles identiques
description: Utilisation de groupes de machines virtuelles identiques avec l’extension de configuration d’état souhaité Azure pour configurer des machines virtuelles.
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: extensions
ms.date: 6/25/2020
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 23313c1c06e01d69021f7ab17002773cc90062a4
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122697439"
---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Utilisation des groupes identiques de machines virtuelles avec l’extension DSC Azure

**S’applique à :** :heavy_check_mark: Machines virtuelles Linux :heavy_check_mark: Machines virtuelles Windows :heavy_check_mark: Groupes identiques uniformes

Les [groupes identiques de machines virtuelles](./overview.md) peuvent être utilisés avec le gestionnaire d’extensions [Configuration d’état souhaité (DSC) Azure](../virtual-machines/extensions/dsc-overview.md?toc=/azure/virtual-machines/windows/toc.json). Les groupes identiques de machines virtuelles offrent un moyen de déployer et gérer un grand nombre de machines virtuelles ; vous pouvez réduire ou augmenter la taille des instances avec flexibilité pour faire face à la charge. La DSC est utilisée pour configurer les machines virtuelles à mesure de leur mise en ligne afin qu’elles exécutent le logiciel en production.

## <a name="differences-between-deploying-to-virtual-machines-and-virtual-machine-scale-sets"></a>Différences entre le déploiement de machines virtuelles et de groupes identiques de machines virtuelles
La structure sous-jacente du modèle pour un groupe identique de machines virtuelles est légèrement différente de celle d’une machine virtuelle seule. Plus précisément, une machine virtuelle unique déploie les extensions sous le nœud « virtualMachines ». Il existe une entrée de type « extensions » où la configuration DSC est ajoutée au modèle

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Un groupe identique de machines virtuelles a une section « propriétés » avec l’attribut « VirtualMachineProfile », « extensionProfile ». La configuration DSC est ajoutée sous « extensions »

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-a-virtual-machine-scale-set"></a>Comportement d’un groupe identique de machines virtuelles
Le comportement d’un groupe identique de machines virtuelles est identique à celui d’une machine virtuelle seule. Lorsqu’une machine virtuelle est créée, elle est automatiquement configurée avec l’extension DSC. Si une version plus récente du WMF est requise par l’extension, la machine virtuelle redémarre avant la mise en ligne. Une fois en ligne, elle télécharge le fichier .zip de la configuration DSC et l’approvisionne sur la machine virtuelle. Vous trouverez plus de détails dans [la présentation de l’extension DSC d’Azure](../virtual-machines/extensions/dsc-overview.md?toc=/azure/virtual-machines/windows/toc.json).

## <a name="next-steps"></a>Étapes suivantes
Examinez le [modèle Azure Resource Manager pour l’extension DSC](../virtual-machines/extensions/dsc-template.md?toc=/azure/virtual-machines/windows/toc.json).

Découvrez comment [l’extension DSC gère en toute sécurité les informations d’identification](../virtual-machines/extensions/dsc-credentials.md?toc=/azure/virtual-machines/windows/toc.json). 

Pour plus d’informations sur le gestionnaire d’extensions DSC Azure, voir [Présentation du gestionnaire d’extensions de configuration d’état souhaité Microsoft Azure](../virtual-machines/extensions/dsc-overview.md?toc=/azure/virtual-machines/windows/toc.json). 

Pour plus informations sur DSC PowerShell, [voir le centre de documentation PowerShell](/powershell/scripting/dsc/overview/overview). 
