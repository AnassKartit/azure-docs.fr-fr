---
title: Connecter Cisco Umbrella à Azure Sentinel | Microsoft Docs
description: Découvrez comment utiliser le connecteur de données Cisco Umbrella pour extraire des données Cisco Umbrella dans Azure Sentinel. Affichez les données Umbrella dans des classeurs, créez des alertes et améliorez l’investigation.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2021
ms.author: yelevin
ms.openlocfilehash: a6a32f33eb88ac82793445badef25227f93fd809
ms.sourcegitcommit: 05dd6452632e00645ec0716a5943c7ac6c9bec7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2021
ms.locfileid: "122525959"
---
# <a name="connect-your-cisco-umbrella-to-azure-sentinel"></a>Connecter Cisco Umbrella à Azure Sentinel

> [!IMPORTANT]
> Le connecteur Cisco Umbrella est actuellement en **PRÉVERSION**. Consultez l’[Avenant aux conditions d’utilisation pour les préversions de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) pour connaître les conditions juridiques supplémentaires s’appliquant aux fonctionnalités Azure sont en version bêta, en préversion ou non encore en disponibilité générale.

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

Le connecteur Cisco Umbrella vous permet de vous connecter à des compartiments Amazon Web Services S3, par le biais de l’API S3, pour ingérer les journaux de la solution de sécurité Cisco Umbrella dans Azure Sentinel.

> [!NOTE]
> Les données seront stockées dans l’emplacement géographique de l’espace de travail sur lequel vous exécutez Azure Sentinel.

## <a name="configure-and-connect-cisco-umbrella"></a>Configurer et connecter Cisco Umbrella

Cisco Umbrella peut intégrer et exporter des journaux dans Azure Sentinel par le biais de l’API Amazon S3.

1. Dans le menu de navigation d’Azure Sentinel, sélectionnez **Connecteurs de données**.

1. Dans la galerie **Connecteurs de données**, sélectionnez **Cisco Umbrella (préversion)** , puis sélectionnez **Ouvrir la page du connecteur**.

1. Suivez les étapes décrites dans la section **Configuration** de la page du connecteur.

## <a name="find-your-data"></a>Recherche de données

Après l’établissement d’une connexion réussie, les données s’affichent dans **Journaux** sous la section **CustomLogs**, dans une ou plusieurs des tables suivantes :
- `Cisco_Umbrella_dns_CL`
- `Cisco_Umbrella_proxy_CL`
- `Cisco_Umbrella_ip_CL`
- `Cisco_Umbrella_cloudfirewall_CL`

## <a name="validate-connectivity"></a>Valider la connectivité

Jusqu’à 20 minutes peuvent être nécessaires avant que vos journaux ne commencent à apparaître dans Log Analytics.

## <a name="next-steps"></a>Étapes suivantes

Dans ce document, vous avez découvert comment connecter des données Cisco Umbrella à Azure Sentinel. Pour en savoir plus sur Azure Sentinel, voir les articles suivants :

- Découvrez comment [avoir une visibilité sur vos données et les menaces potentielles](get-visibility.md).
- Prise en main de la [détection des menaces avec Azure Sentinel](detect-threats-built-in.md).
- [Utilisez des classeurs](monitor-your-data.md) pour superviser vos données.