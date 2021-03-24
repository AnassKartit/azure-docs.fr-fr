---
title: Connecter des données Symantec VIP à Azure Sentinel | Microsoft Docs
description: Découvrez comment connecter des données Symantec VIP à Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: c7429108f70d735cb6e314a0d4daa27ba0d31637
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100090409"
---
# <a name="connect-your-symantec-vip-to-azure-sentinel"></a>Connexion de votre Symantec VIP à Azure Sentinel

> [!IMPORTANT]
> Le connecteur de données Symantec VIP dans Azure Sentinel est actuellement en préversion publique.
> Cette fonctionnalité est fournie sans contrat de niveau de service et est déconseillée pour les charges de travail de production. Certaines fonctionnalités peuvent être limitées ou non prises en charge. Pour plus d’informations, consultez [Conditions d’Utilisation Supplémentaires relatives aux Évaluations Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Cet article explique comment connecter votre appliance [Symantec VIP](https://vip.symantec.com/) à Azure Sentinel. Le connecteur de données Symantec VIP vous permet de connecter facilement vos journaux Symantec VIP à Azure Sentinel, de consulter des tableaux de bord, de créer des alertes personnalisées et d’améliorer les enquêtes. L'intégration entre Symantec VIP et Azure Sentinel utilise Syslog.

> [!NOTE]
> Les données seront stockées dans l’emplacement géographique de l’espace de travail sur lequel vous exécutez Azure Sentinel.

## <a name="forward-symantec-vip-logs-to-the-syslog-agent"></a>Transférer des journaux Symantec VIP à l’agent Syslog  

Configurez Symantec VIP pour transférer les messages Syslog à votre espace de travail Azure par le biais de l’agent Syslog.

1. Dans le portail Azure Sentinel, cliquez sur **Data Connectors** (Connecteurs de données) et sélectionnez le connecteur **Symantec VIP**.

1. Sélectionnez **Ouvrir la page du connecteur**.

1. Suivez les instructions de la page **Symantec VIP**.

## <a name="find-your-data"></a>Recherche de données

Une fois la connexion correctement établie, les données apparaissent dans l’espace de travail Log Analytics sous Syslog.

## <a name="validate-connectivity"></a>Valider la connectivité

Jusqu’à 20 minutes peuvent être nécessaires avant que vos journaux ne commencent à apparaître dans Log Analytics. 

## <a name="next-steps"></a>Étapes suivantes

Dans ce document, vous avez appris à connecter Symantec VIP à Azure Sentinel. Pour en savoir plus sur Azure Sentinel, voir les articles suivants :

- Découvrez comment [avoir une visibilité sur vos données et les menaces potentielles](quickstart-get-visibility.md).
- Prise en main de la [détection des menaces avec Azure Sentinel](tutorial-detect-threats-built-in.md).
- [Utilisez des classeurs](tutorial-monitor-your-data.md) pour superviser vos données.