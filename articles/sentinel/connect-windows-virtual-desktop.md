---
title: Connecter Windows Virtual Desktop à Azure Sentinel | Microsoft Docs
description: Découvrez comment connecter vos données Windows Virtual Desktop à Azure Sentinel.
services: sentinel
documentationcenter: na
author: batamig
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/22/2021
ms.author: bagol
ms.custom: ignite-fall-2021
ms.openlocfilehash: 6614cc08a53314a701a0db5e6396024bbfb58a28
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131009412"
---
# <a name="connect-windows-virtual-desktop-data-to-azure-sentinel"></a>Connecter des données Windows Virtual Desktop à Azure Sentinel

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Cet article décrit comment superviser un environnement Windows Virtual Desktop (WVD) à l’aide d’Azure Sentinel.

La supervision de vos environnements Windows Virtual Desktop peut par exemple vous permettre de fournir plus de travail à distance à l’aide de bureaux virtualisés, tout en conservant la posture de sécurité de votre organisation.

## <a name="windows-virtual-desktop-data-in-azure-sentinel"></a>Données Windows Virtual Desktop dans Azure Sentinel

Les données Windows Virtual Desktop dans Azure Sentinel incluent les types suivants :


|Données  |Description  |
|---------|---------|
|**Journaux des événements Windows**     |  Les journaux des événements Windows de l’environnement WVD sont streamés vers un espace de travail Log Analytics activé pour Azure Sentinel, de la même manière que les journaux des événements Windows issus d’autres machines Windows en dehors de l’environnement WVD. <br><br>Installez l’agent Log Analytics sur votre ordinateur Windows et configurez les journaux des événements Windows à envoyer à l’espace de travail Log Analytics.<br><br>Pour plus d'informations, consultez les pages suivantes :<br>- [Installer l’agent Log Analytics sur des ordinateurs Windows](../azure-monitor/agents/agent-windows.md)<br>- [Collecter les sources de données du journal des événements Windows avec l’agent Log Analytics](../azure-monitor/agents/data-sources-windows-events.md)<br>- [Connecter les événements de sécurité Windows](connect-windows-security-events.md)       |
|**Microsoft Defender pour les alertes point de terminaison**     |  Pour configurer Defender pour point de terminaison pour Windows Virtual Desktop, appliquez la même procédure que pour n’importe quel autre point de terminaison Windows. <br><br>Pour plus d'informations, consultez les pages suivantes : <br>- [Configurer le déploiement de Microsoft Defender pour point de terminaison](/windows/security/threat-protection/microsoft-defender-atp/production-deployment)<br>- [Connecter des données de Microsoft 365 Defender à Azure Sentinel](connect-microsoft-365-defender.md)       |
|**Diagnostics Windows Virtual Desktop**     | Les diagnostics Windows Virtual Desktop sont une fonctionnalité du service PaaS Windows Virtual Desktop qui enregistre des informations dans un journal chaque fois qu’un utilisateur disposant du rôle Windows Virtual Desktop utilise le service. <br><br>Chaque journal contient des informations sur le rôle Windows Virtual Desktop impliqué dans l'activité, les éventuels messages d'erreur qui apparaissent pendant la session, les informations sur le locataire et les informations sur l'utilisateur. <br><br>La fonctionnalité de diagnostic crée des journaux d'activité pour les actions des utilisateurs et des administrateurs. <br><br>Pour plus d’informations, consultez [Utiliser Log Analytics pour la fonctionnalité de diagnostic dans Windows Virtual Desktop](../virtual-desktop/virtual-desktop-fall-2019/diagnostics-log-analytics-2019.md).        |
|     |         |

## <a name="connect-windows-virtual-desktop-data"></a>Connecter des données Windows Virtual Desktop

Pour commencer à ingérer des données Windows Virtual Desktop dans Azure Sentinel, suivez les instructions fournies dans la documentation de Windows Virtual Desktop.

Pour plus d’informations, consultez [Pousser des données Windows Virtual Desktop vers votre espace de travail Log Analytics](../virtual-desktop/diagnostics-log-analytics.md).

## <a name="find-your-data"></a>Recherche de données

Une fois la connexion établie, exécutez des requêtes dans Azure Sentinel sur vos données Log Analytics.

Consultez par exemple les exemples de requêtes fournis dans la [documentation de Windows Virtual Desktop](../virtual-desktop/diagnostics-log-analytics.md).


Azure Sentinel fournit également des requêtes intégrées dans la zone **Général** > **Journaux** > **WINDOWS VIRTUAL DESKTOP** :

[![Requêtes intégrées Windows Virtual Desktop dans Azure Sentinel.](media/connect-windows-virtual-desktop/windows-virtual-desktop-queries.png) ](media/connect-windows-virtual-desktop/windows-virtual-desktop-queries.png#lightbox)

## <a name="next-steps"></a>Étapes suivantes


Pour plus d’informations, consultez le [glossaire Azure Monitor pour Windows Virtual Desktop](../virtual-desktop/azure-monitor-glossary.md).
