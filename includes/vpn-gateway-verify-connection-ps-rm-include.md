---
title: Fichier include
description: Fichier include
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/17/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 49f48c2d0bf865cf8c8fde831e7a597f8701d213
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "67176891"
---
Vous pouvez vérifier que votre connexion a réussi via l’applet de commande « Get-AzVirtualNetworkGatewayConnection » avec ou sans « -Debug ». 

1. Utilisez l’exemple d’applet de commande suivant, en configurant les valeurs sur les vôtres. Si vous y êtes invité, sélectionnez A pour exécuter tout. Dans l’exemple, «  -Name » fait référence au nom de la connexion que vous voulez tester.

   ```azurepowershell-interactive
   Get-AzVirtualNetworkGatewayConnection -Name VNet1toSite1 -ResourceGroupName TestRG1
   ```
2. Une fois l’applet de commande exécutée, affichez les valeurs. Dans l’exemple ci-dessous, l’état de la connexion indique « Connecté » et vous pouvez voir les octets d’entrée et de sortie.
   
   ```
   "connectionStatus": "Connected",
   "ingressBytesTransferred": 33509044,
   "egressBytesTransferred": 4142431
   ```