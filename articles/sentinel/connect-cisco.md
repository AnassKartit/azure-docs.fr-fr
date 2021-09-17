---
title: Connecter des données Cisco à Azure Sentinel | Microsoft Docs
description: Découvrez comment connecter votre appliance Cisco ASA à Azure Sentinel pour consulter des tableaux de bord, créer des alertes personnalisées et améliorer les enquêtes.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 62029b5c-29d3-4336-8a22-a9db8214eb7e
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: db10c9814c04f6e29a017b9a473cd117a0cbf2ae
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122695198"
---
# <a name="connect-cisco-asa-to-azure-sentinel"></a>Connecter Cisco ASA à Azure Sentinel



Cet article explique comment connecter votre appliance Cisco ASA à Azure Sentinel. Le connecteur de données Cisco ASA vous permet de connecter facilement vos journaux Cisco ASA à Azure Sentinel, de consulter des tableaux de bord, de créer des alertes personnalisées et d’améliorer les enquêtes. L’utilisation de Cisco ASA sur Azure Sentinel vous permettra d’obtenir davantage d’informations sur l’utilisation d’Internet au sein de votre organisation et améliorera ses fonctionnalités de sécurité. 

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]


## <a name="forward-cisco-asa-logs-to-the-syslog-agent"></a>Transférer les journaux Cisco ASA à l’agent Syslog

Comme Cisco ASA ne prend pas en charge le format CEF, les journaux sont envoyés en tant que Syslog et l’agent Azure Sentinel les analyse comme s’ils s’agissait de journaux CEF. Configurez Cisco ASA pour transférer les messages Syslog à votre espace de travail Azure par le biais de l’agent Syslog :

1. accédez à [Envoyer des messages Syslog à un serveur Syslog externe](https://aka.ms/asi-syslog-cisco-forwarding) et suivez les instructions pour configurer la connexion. Utilisez ces paramètres lorsque vous y êtes invité :
    - définissez le **port** sur 514 ou le port défini dans l’agent.
    - définissez **syslog_ip** sur l’adresse IP de l’agent.

1. Pour utiliser le schéma pertinent dans Log Analytics pour les événements Cisco, recherchez `CommonSecurityLog`.

1. Passez à [Valider la connectivité CEF](troubleshooting-cef-syslog.md#validate-cef-connectivity).




## <a name="next-steps"></a>Étapes suivantes
Dans ce document, vous avez appris à connecter les appliances Cisco ASA à Azure Sentinel. Pour en savoir plus sur Azure Sentinel, voir les articles suivants :
- Découvrez comment [avoir une visibilité sur vos données et les menaces potentielles](get-visibility.md).
- Prise en main de la [détection des menaces avec Azure Sentinel](detect-threats-built-in.md).
- [Utilisez des classeurs](monitor-your-data.md) pour superviser vos données.