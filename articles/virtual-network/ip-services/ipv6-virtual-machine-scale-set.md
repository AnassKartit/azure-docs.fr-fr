---
title: Déployer des groupes de machines virtuelles identiques avec IPv6 dans Azure
titlesuffix: Azure Virtual Network
description: Cet article explique comment déployer des groupes de machines virtuelles identiques avec IPv6 dans un réseau virtuel Azure.
services: virtual-network
documentationcenter: na
author: asudbring
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/31/2020
ms.author: allensu
ms.openlocfilehash: 0f8bce99f26f175f3103ccc4824c094b2e1c4654
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2021
ms.locfileid: "129368468"
---
# <a name="deploy-virtual-machine-scale-sets-with-ipv6-in-azure"></a>Déployer des groupes de machines virtuelles identiques avec IPv6 dans Azure

Cet article explique comment déployer un groupe de machines virtuelles identiques à double pile (IPv4 + IPv6) avec un équilibreur de charge externe à double pile dans un réseau virtuel Azure. Le processus de création d’un groupe de machines virtuelles identiques compatible IPv6 est quasiment identique au processus de création des machines virtuelles individuelles décrit [ici](../../load-balancer/ipv6-configure-standard-load-balancer-template-json.md). Vous allez commencer par des étapes similaires à celles décrites pour les machines virtuelles individuelles :
1.    Créer des adresses IP publiques IPv4 et IPv6.
2.    Créer un équilibreur de charge à double pile.  
3.    Créer des règles du groupe de sécurité réseau.  

La seule étape qui diffère des machines virtuelles individuelles est la création de la configuration d’interface réseau (NIC) qui utilise la ressource de groupe de machines virtuelles identiques : networkProfile/networkInterfaceConfigurations. La structure JSON est similaire à celle de l'objet Microsoft.Network/networkInterfaces utilisé pour les machines virtuelles individuelles, avec en plus la configuration de la carte réseau et du paramètre IPv4 IpConfiguration comme interface principale à l'aide de l'attribut **"primary": true**, comme indiqué dans l'exemple suivant :

```json
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "[variables('nicName')]",
                "properties": {
                  "primary": true,
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups','VmssNsg')]"
          },                  
                  "ipConfigurations": [
                    {
                      "name": "[variables('ipConfigName')]",
                      "properties": {
                        "primary": true,
                        "subnet": {
                          "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', 'MyvirtualNetwork','Mysubnet')]"
                        },
                        "privateIPAddressVersion":"IPv4",                       
                        "publicipaddressconfiguration": {
                          "name": "pub1",
                          "properties": {
                            "idleTimeoutInMinutes": 15
                          }
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[resourceId('Microsoft.Network/loadBalancers/backendAddressPools', 'loadBalancer', 'bePool'))]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[resourceId('Microsoft.Network/loadBalancers/inboundNatPools', 'loadBalancer', 'natPool')]"
                          }
                        ]
                      }
                    },
                    {
                      "name": "[variables('ipConfigNameV6')]",
                      "properties": {
                        "subnet": {
                          "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets','MyvirtualNetwork','Mysubnet')]"
                        },
                        "privateIPAddressVersion":"IPv6",
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[resourceId('Microsoft.Network/loadBalancers/backendAddressPools', 'loadBalancer','bePoolv6')]"
                          }
                        ],                        
                      }
                    }
                  ]
                }
              }
            ]
          }

```


## <a name="sample-virtual-machine-scale-set-template-json"></a>Exemple de modèle de groupe de machines virtuelles identiques JSON

Pour déployer un groupe de machines virtuelles identiques à double pile (IPv4 + IPv6) avec un équilibreur de charge externe à double pile et un réseau virtuel Azure, consultez l’exemple de modèle [ici](https://azure.microsoft.com/resources/templates/ipv6-in-vnet-vmss/).
## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur la prise en charge du protocole IPv6 dans les réseaux virtuels Azure, voir [Qu’est-ce que le protocole IPv6 pour réseau virtuel Azure ?](ipv6-overview.md).
