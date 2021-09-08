---
title: Déléguer un sous-réseau à Azure NetApp Files | Microsoft Docs
description: Apprenez à déléguer un sous-réseau à Azure NetApp Files. Spécifiez le sous-réseau délégué lorsque vous créez un volume.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/25/2021
ms.author: b-juche
ms.openlocfilehash: cdb184d4b96e4cfee2b5450f35c947efb768da9b
ms.sourcegitcommit: 7854045df93e28949e79765a638ec86f83d28ebc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2021
ms.locfileid: "122866975"
---
# <a name="delegate-a-subnet-to-azure-netapp-files"></a>Déléguer un sous-réseau à Azure NetApp Files 

Vous devez déléguer un sous-réseau à Azure NetApp Files.   Lorsque vous créez un volume, vous devez spécifier le sous-réseau délégué.

## <a name="considerations"></a>Considérations

* L’Assistant de création d’un sous-réseau applique par défaut un masque réseau /24, qui fournit 251 adresses IP disponibles. L’utilisation d’un masque de réseau /28 fournissant 11 adresses IP utilisables est suffisante pour le plupart des cas d'usage. Vous devez envisager un plus grand sous-réseau (par exemple, le masque de réseau /26) dans des scénarios tels que SAP HANA où de nombreux volumes et points de terminaison de stockage sont anticipés. Vous pouvez également conserver le masque réseau par défaut /24 comme proposé par l’Assistant si vous n’avez pas besoin de réserver de nombreuses adresses IP de clients ou de machines virtuelles dans votre réseau virtuel Azure (VNet). Notez que le masque de réseau du réseau délégué ne peut pas être modifié après la création initiale. 
* Dans chaque réseau virtuel, un seul sous-réseau peut être délégué à Azure NetApp Files.   
   Azure vous permet de créer plusieurs sous-réseaux délégués dans un réseau virtuel.  Cependant, toute tentative de création d’un nouveau volume échoue si vous utilisez plusieurs sous-réseaux délégués.  
   Vous ne pouvez avoir qu’un seul sous-réseau délégué dans un réseau virtuel. Un compte NetApp peut déployer des volumes dans plusieurs réseaux virtuels, chacun disposant de son propre sous-réseau délégué.  
* Vous ne pouvez pas désigner un groupe de sécurité ou un point de terminaison de service réseau dans le sous-réseau délégué. Cette opération fait échouer la délégation du sous-réseau.
* L’accès à un volume depuis un réseau virtuel homologué globalement n’est pas pris en charge actuellement.
* Les groupes de sécurité réseau (NSG) et les [itinéraires définis par l’utilisateur](../virtual-network/virtual-networks-udr-overview.md#custom-routes) (UDR) ne sont pas pris en charge sur les sous-réseaux délégués pour Azure NetApp Files. Toutefois, vous pouvez appliquer des itinéraires définis par l’utilisateur et des groupes de sécurité réseau à d’autres sous-réseaux, même au sein du même réseau virtuel que le sous-réseau délégué à Azure NetApp Files.  
   Azure NetApp Files crée un itinéraire système vers le sous-réseau délégué. Si vous en avez besoin pour le dépannage, l’itinéraire est affiché dans **Itinéraires effectifs** dans la table de routage.

## <a name="steps"></a>Étapes

1.  Accédez au panneau **Réseaux virtuels** dans le portail Azure, puis sélectionnez le réseau virtuel que vous souhaitez utiliser pour Azure NetApp Files.    

1. Sélectionnez **Sous-réseaux** dans le panneau Réseau virtuel et cliquez sur le bouton **+Sous-réseau**. 

1. Créez un sous-réseau à utiliser pour Azure NetApp Files en renseignant les champs obligatoires suivants dans la page Ajouter un sous-réseau :
    * **Name** : spécifiez le nom du sous-réseau.
    * **Plage d’adresses** : spécifiez la plage d’adresses IP.
    * **Délégation de sous-réseau** : sélectionnez **Microsoft.NetApp/volumes**. 

      ![Délégation de sous-réseau](../media/azure-netapp-files/azure-netapp-files-subnet-delegation.png)
    
Vous pouvez également créer et déléguer un sous-réseau lorsque vous [créez un volume pour Azure NetApp Files](azure-netapp-files-create-volumes.md). 

## <a name="next-steps"></a>Étapes suivantes

* [Créer un volume pour Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [En savoir plus sur l’intégration d’un réseau virtuel pour les services Azure](../virtual-network/virtual-network-for-azure-services.md)
