---
title: Connecter des produits Forcepoint à Azure Sentinel | Microsoft Docs
description: Découvrez comment connecter des produits Forcepoint à Azure Sentinel.
services: sentinel
author: yelevin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: b0b734bcc540651debad35606be496fdc4069b3c
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122695108"
---
# <a name="connect-your-forcepoint-products-to-azure-sentinel"></a>Connecter vos produits Forcepoint à Azure Sentinel

> [!IMPORTANT]
> Le connecteur de données des produits Forcepoint dans Azure Sentinel est actuellement en préversion publique. Cette fonctionnalité est fournie sans contrat de niveau de service et est déconseillée pour les charges de travail de production. Certaines fonctionnalités peuvent être limitées ou non prises en charge. Pour plus d’informations, consultez [Conditions d’Utilisation Supplémentaires relatives aux Évaluations Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [reference-to-feature-availability](includes/reference-to-feature-availability.md)]

Cet article explique comment connecter vos produits Forcepoint à Azure Sentinel. 

Les connecteurs de données Forcepoint vous permettent de connecter les journaux de Forcepoint Cloud Access Security Broker (CASB) et Forcepoint Next Generation Firewall (NGFW) avec Azure Sentinel. De cette façon, vous pouvez exporter automatiquement les journaux définis par l’utilisateur dans Azure Sentinel en temps réel. Le connecteur vous offre une visibilité enrichie des activités de l’utilisateur enregistrées par les produits Forcepoint. Il permet également une corrélation plus poussée avec les données issues des charges de travail Azure et d’autres flux et améliore la capacité de surveillance des classeurs dans Azure Sentinel.

> [!NOTE]
> Les données seront stockées dans l’emplacement géographique de l’espace de travail sur lequel vous exécutez Azure Sentinel.



## <a name="forward-forcepoint-product-logs-to-the-syslog-agent"></a>Transférer les journaux des produits Forcepoint à l’agent Syslog 

Configurez le produit Forcepoint pour transférer les messages Syslog au format CEF à votre espace de travail Azure par le biais de l’agent Syslog.

1. Configurez le produit Forcepoint pour l’intégration d’Azure Sentinel comme décrit dans les guides d’installation suivants :
 - [Guide d’intégration de Forcepoint CASB](https://frcpnt.com/casb-sentinel)
 - [Guide d’intégration de Forcepoint NGFW](https://frcpnt.com/ngfw-sentinel)

2. Recherchez CommonSecurityLog pour utiliser le schéma approprié dans Log Analytics avec le nom DeviceVendor qui contient « Forcepoint ». 

3. Continuez vers : [Valider la connectivité CEF](troubleshooting-cef-syslog.md#validate-cef-connectivity).



## <a name="next-steps"></a>Étapes suivantes

Dans ce document, vous avez appris à connecter les produits Forcepoint à Azure Sentinel. Pour en savoir plus sur Azure Sentinel, voir les articles suivants :

- Découvrez comment [avoir une visibilité sur vos données et les menaces potentielles](get-visibility.md).

- Prise en main de la [détection des menaces avec Azure Sentinel](detect-threats-built-in.md).

- [Utilisez des classeurs](monitor-your-data.md) pour superviser vos données.