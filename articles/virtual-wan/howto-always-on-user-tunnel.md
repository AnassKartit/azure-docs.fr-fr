---
title: Configurer un tunnel utilisateur VPN Always On
titleSuffix: Azure Virtual WAN
description: Cet article explique comment configurer un tunnel utilisateur VPN Always On pour votre WAN virtuel
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: e83ca64d2b0e50ec02007a3cd878e6bf034d0961
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "91313584"
---
# <a name="configure-an-always-on-vpn-user-tunnel-for-virtual-wan"></a>Configurer un tunnel utilisateur VPN Always On pour un WAN virtuel

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Prérequis

Vous devez créer une configuration de point à site et modifier l’affectation du hub virtuel. Consultez les sections suivantes pour obtenir des instructions :

* [Créer une configuration P2S](virtual-wan-point-to-site-portal.md#p2sconfig)
* [Créer un hub avec la passerelle P2S](virtual-wan-point-to-site-portal.md#hub)

## <a name="configure-a-user-tunnel"></a>Configurer un tunnel utilisateur

[!INCLUDE [user tunnel](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>Pour supprimer un profil

Pour supprimer un profil, effectuez les étapes suivantes :

1. Exécutez la commande suivante :

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Arrêtez la connexion et désactivez la case à cocher **Se connecter automatiquement**.

   ![Nettoyage](./media/howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur Virtual WAN, consultez la [FAQ](virtual-wan-faq.md).
