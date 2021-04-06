---
title: Utiliser Windows PowerShell pour se connecter à un appareil Azure Data Box Gateway et le gérer
description: Décrit comment connecter et gérer Data Box Gateway via l’interface Windows PowerShell.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: how-to
ms.date: 10/20/2020
ms.author: alkohli
ms.openlocfilehash: f3b93bfc9af9bce50c301c10bd372567360d7223
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96580918"
---
# <a name="manage-an-azure-data-box-gateway-device-via-windows-powershell"></a>Gérer un appareil Azure Data Box Gateway via Windows PowerShell

La solution Azure Data Box Gateway vous permet d’envoyer des données via le réseau vers Azure. Cet article décrit certaines des tâches de gestion et de configuration pour votre appareil Data Box Gateway. Vous pouvez utiliser le portail Azure, l'interface utilisateur locale ou l'interface Windows PowerShell pour gérer votre appareil.

Cet article traite des tâches que vous effectuez à l’aide de l’interface PowerShell.

Cet article inclut les procédures suivantes :

- Connexion à l’interface PowerShell
- Création d’un package de prise en charge
- Téléchargement d’un certificat
- Démarrage dans un environnement non DHCP
- Affichage des informations sur l’appareil

## <a name="connect-to-the-powershell-interface"></a>Connexion à l’interface PowerShell

[!INCLUDE [Connect to admin runspace](../../includes/data-box-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Création d’un package de prise en charge

[!INCLUDE [Create a support package](../../includes/data-box-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Téléchargement d’un certificat

[!INCLUDE [Upload certificate](../../includes/data-box-gateway-upload-certificate.md)]

## <a name="boot-up-in-non-dhcp-environment"></a>Démarrage dans un environnement non DHCP

[!INCLUDE [Boot up in non-DHCP environment](../../includes/data-box-gateway-boot-non-dhcp.md)]

## <a name="view-device-information"></a>Affichage des informations sur l’appareil

[!INCLUDE [View device information](../../includes/data-box-gateway-view-device-info.md)]

## <a name="next-steps"></a>Étapes suivantes

- Déployer [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) sur le portail Azure.
