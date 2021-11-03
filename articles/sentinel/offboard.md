---
title: Supprimer Azure Sentinel | Microsoft Docs
description: Guide pratique pour supprimer votre instance Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2020
ms.author: yelevin
ms.custom: ignite-fall-2021
ms.openlocfilehash: afb17cf0cc999680d2cd60a0be8e4a227aa146bf
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131009184"
---
# <a name="remove-azure-sentinel-from-your-workspace"></a>Supprimer Azure Sentinel de votre espace de travail

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Si vous ne voulez plus utiliser Azure Sentinel, cet article explique comment le supprimer de votre espace de travail.

## <a name="how-to-remove-azure-sentinel"></a>Comment supprimer Azure Sentinel

Suivez ce processus pour supprimer Azure Sentinel de votre espace de travail :

1. Accédez à **Azure Sentinel**, **Paramètres**, puis sélectionnez l’onglet **Supprimer Azure Sentinel**.

1. Avant de supprimer Azure Sentinel, utilisez les cases à cocher pour nous indiquer pourquoi vous le supprimez.

1. Sélectionnez **Supprimer Azure Sentinel de votre espace de travail**.
    
    ![Supprimer la solution SecurityInsights](media/offboard/delete-solution.png)

## <a name="what-happens-behind-the-scenes"></a>Que se passe-t-il en arrière-plan ?

Quand vous supprimez la solution, l’exécution de la première phase du processus de suppression par Azure Sentinel prend jusqu’à 48 heures.

Une fois la déconnexion identifiée, le processus d’annulation de l’intégration commence.

**La configuration de ces connecteurs est supprimée :**
-   Office 365

-   AWS

-   Alertes de sécurité des services Microsoft : Alertes de sécurité Azure Security Center de Microsoft Defender pour Identity (*anciennement Azure ATP*), Microsoft Cloud App Security, ce qui inclut la génération de rapports Cloud Discovery Shadow IT, Azure AD Identity Protection, Microsoft Defender for Endpoint (*anciennement Microsoft Defender ATP*) Azure Defender

-   Informations sur les menaces

-   Journaux de sécurité courants (notamment les journaux CEF, Barracuda et Syslog) (Si vous recevez les alertes Azure Defender depuis Azure Security Center, ces journaux continueront d’être collectés.)

-   Événements de sécurité Windows (Si vous recevez les alertes Azure Defender depuis Azure Security Center, ces journaux continueront d’être collectés.)

Au cours des 48 premières heures, les données et les règles d’analyse (dont la configuration de l’automatisation en temps réel) ne seront plus accessibles ou ne pourront plus être interrogées dans Azure Sentinel.

**Au bout de 30 jours, ces ressources sont supprimées :**

-   Incidents (dont les métadonnées d’enquête)

-   Règles analytiques

-   Signets

Vos playbooks, classeurs enregistrés, requêtes de repérage enregistrées et notebooks ne sont pas supprimés. **Certains peuvent cesser de fonctionner en raison des données supprimées. Vous pouvez les supprimer manuellement.**

Une fois que vous avez supprimé le service, il y a une période de grâce de 30 jours pendant laquelle vous pouvez réactiver la solution. Vos données et règles d’analyse sont alors restaurées, mais les connecteurs configurés qui étaient déconnectés doivent être reconnectés.

> [!NOTE]
> Si vous supprimez la solution, votre abonnement reste inscrit auprès du fournisseur de ressources Azure Sentinel. **Vous pouvez le supprimer manuellement.**




## <a name="next-steps"></a>Étapes suivantes
Dans ce document, vous avez appris à supprimer le service Azure Sentinel. Si vous changez d’avis et que vous voulez le réinstaller :
- Bien démarrer avec l’[intégration d’Azure Sentinel](quickstart-onboard.md).
