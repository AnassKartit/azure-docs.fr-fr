---
title: Classeurs Azure Sentinel courants | Microsoft Docs
description: Découvrez les classeurs les plus courants pour utiliser des ressources Azure Sentinel intégrées et populaires.
services: sentinel
cloud: na
documentationcenter: na
author: batamig
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 03/07/2021
ms.author: bagol
ms.openlocfilehash: a37501498a9222025860702a7f29dccc9abfc8f7
ms.sourcegitcommit: f6193c2c6ce3b4db379c3f474fdbb40c6585553b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102450047"
---
# <a name="commonly-used-azure-sentinel-workbooks"></a>Classeurs Azure Sentinel courants

Le tableau suivant présente les classeurs Azure Sentinel intégrés les plus courants.

Accédez aux classeurs dans Azure Sentinel sous **Gestion des menaces** > **Classeurs** à gauche, puis recherchez le classeur que vous souhaitez utiliser. Pour plus d’informations, consultez [Tutoriel : Visualisation et supervision des données](tutorial-monitor-your-data.md).

|Nom du classeur  |Description  |
|---------|---------|
|**Efficacité analytique**     |  Fournit des insights sur l’efficacité de vos règles d’analytique pour vous aider à obtenir un meilleur niveau de performance du centre SOC. <br><br>Pour plus d’informations, consultez [Kit de ressources pour les centres SOC pilotés par les données](https://techcommunity.microsoft.com/t5/azure-sentinel/the-toolkit-for-data-driven-socs/ba-p/2143152).|
|**Activité Azure**     |     Fournit des insights complets sur l’activité Azure de votre organisation en analysant et en mettant en corrélation tous les événements et opérations utilisateurs. <br><br>Pour plus d’informations, consultez [Audit avec les journaux d’activité Azure](audit-sentinel-data.md#auditing-with-azure-activity-logs).    |
|**Journaux d’audit Azure AD**     |  Utilise les journaux d’audit Azure Active Directory pour fournir des insights dans les scénarios Azure AD. <br><br>Pour plus d’informations, consultez [Démarrage rapide : Bien démarrer avec Azure Sentinel](quickstart-get-visibility.md).     |
|**Journaux d’audit, d’activité et de connexion Azure AD**     |   Fournit des insights sur les données d’audit, d’activité et de connexion Azure Active Directory avec un seul classeur. Ce classeur peut être utilisé par les administrateurs de la sécurité et les administrateurs Azure.      |
|**Journaux de connexion Azure AD**     | Utilise les journaux de connexion Azure AD pour fournir des insights dans les scénarios Azure AD.        |
|**CMMC (Cybersecurity Maturity Model Certification)**     |   Fournit un mécanisme permettant d’afficher les requêtes de journal alignées sur les contrôles CMMC dans le portefeuille Microsoft, notamment les offres de sécurité Microsoft, Office 365, Teams, Intune, Windows Virtual Desktop, etc. <br><br>Pour plus d’informations, consultez [Classeur CMMC (Cybersecurity Maturity Model Certification) en préversion publique](https://techcommunity.microsoft.com/t5/azure-sentinel/what-s-new-cybersecurity-maturity-model-certification-cmmc/ba-p/2111184).|
|**Analyse du fonctionnement de la collecte de données**     |   Fournit des insights sur l’état d’ingestion des données de l’espace de travail. Supervisez et détectez les anomalies pour mieux évaluer l’intégrité de la collecte de données de votre espace de travail.  <br><br>Pour plus d’informations, consultez [Supervision de l’intégrité de connecteurs de données avec ce classeur Azure Sentinel](monitor-data-connector-health.md).    |
|**Analyseur d’événements**     |  Permet d’explorer, d’auditer et d’accélérer l’analyse du journal des événements Windows, y compris tous les détails et attributs des événements, comme la sécurité, l’application, le système, la configuration, le service d’annuaire, le système DNS, etc.       |
|**Exchange Online**     |Fournit des insights sur Microsoft Exchange Online en traçant et en analysant toutes les opérations Exchange et les activités des utilisateurs.         |
|**Identité et accès**     |   Fournit des insights sur les opérations d’identité et d’accès dans l’utilisation des produits Microsoft, au moyen de journaux de sécurité qui incluent des journaux d’audit et de connexion.     |
|**Vue d’ensemble des incidents**     |   Facilite le triage et l’investigation en fournissant des informations détaillées sur un incident, notamment des informations générales, des données d’entité, le temps de triage, le temps d’atténuation et des commentaires. <br><br>Pour plus d’informations, consultez [Kit de ressources pour les centres SOC pilotés par les données](https://techcommunity.microsoft.com/t5/azure-sentinel/the-toolkit-for-data-driven-socs/ba-p/2143152).      |
|**Insights d’investigation**     | Fournit aux analystes des insights sur les données relatives aux incidents, aux signets et aux entités. Les requêtes courantes et les visualisations détaillées peuvent les aider à examiner les activités suspectes.       |
|**Microsoft Cloud App Security – Journaux de découverte**     |   Fournit des détails sur les applications cloud utilisées dans votre organisation, ainsi que des insights tirés des tendances d’utilisation et des données d’exploration pour des utilisateurs et des applications spécifiques.  <br><br>Pour plus d’informations, consultez [Connexion de données à partir de Microsoft Cloud App Security](connect-cloud-app-security.md).|
|**Classeur MITRE ATT&CK**     |   Fournit des données sur la couverture MITRE ATT&CK pour Azure Sentinel.      |
|**Office 365**     |  Fournit des insights sur Office 365 en traçant et en analysant toutes les opérations et activités. Explorez les données SharePoint, OneDrive et Exchange.       |
|**alertes de sécurité**     |  Fournit un tableau de bord des alertes de sécurité de l’environnement Azure Sentinel. <br><br>Pour plus d’informations, consultez [Créer automatiquement des incidents à partir d’alertes de sécurité Microsoft](create-incidents-from-alerts.md).      |
|**Efficacité des opérations de sécurité**     |  Permet aux responsables de centre des opérations de sécurité (SOC) de voir des métriques et des mesures d’efficacité globales concernant le niveau de performance de leur équipe. <br><br>Pour plus d’informations, consultez [Meilleure gestion du centre SOC avec les métriques relatives aux incidents](manage-soc-with-incident-metrics.md).  |
|**Renseignement sur les menaces**     | Fournit des insights sur les indicateurs de menace, notamment le type et la gravité des menaces, l’activité des menaces dans le temps et la corrélation avec d’autres sources de données, notamment Office 365 et les pare-feu.  <br><br>Pour plus d’informations, consultez [Importation du renseignement sur les menaces dans Azure Sentinel](import-threat-intelligence.md).      |
|**Audit de l’espace de travail**     |  Fournit un rapport d’audit de l’espace de travail qui permet de comprendre les exécutions de requêtes dans l’espace de travail.   <br><br>Pour plus d’informations, consultez [Audit des requêtes et des activités Azure Sentinel](audit-sentinel-data.md).  |
|     |         |

