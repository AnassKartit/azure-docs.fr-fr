---
title: Ressources utiles lorsque vous travaillez avec Azure Sentinel
description: Ce document vous fournit une liste des ressources utiles lorsque vous travaillez avec Azure Sentinel.
services: sentinel
author: yelevin
manager: rkarlin
ms.assetid: 9b4c8e38-c986-4223-aa24-a71b01cb15ae
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 03/03/2021
ms.author: yelevin
ms.custom: ignite-fall-2021
ms.openlocfilehash: f8f4dc553bcb9750199d38c640c70012b73bf4bf
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131055100"
---
# <a name="useful-resources-for-working-with-azure-sentinel"></a>Ressources utiles lorsque vous travaillez avec Azure Sentinel

[!INCLUDE [Banner for top of topics](./includes/banner.md)]

Cet article répertorie les ressources qui peuvent vous aider à obtenir plus d’informations sur l’utilisation d’Azure Sentinel.

## <a name="learn-more-about-creating-queries"></a>En savoir plus sur la création de requêtes

Azure Sentinel utilise le langage de requête Kusto (KQL) d’Azure Monitor Log Analytics pour créer des requêtes. Pour plus d'informations, consultez les pages suivantes :

- [Concepts KQL](/azure/data-explorer/kusto/concepts/)
- [Requêtes KQL](/azure/data-explorer/kusto/query/)
- [Aide-mémoire KQL](/azure/data-explorer/kql-quick-reference)
- [Prise en main des requêtes KQL](../azure-monitor/logs/get-started-queries.md)

## <a name="learn-more-about-creating-automation"></a>En savoir plus sur la création d’automatisation

Créez une automatisation dans Azure Sentinel à l’aide d’Azure Logic Apps, avec une galerie croissante de playbooks intégrés. 

Pour plus d’informations, consultez [Connecteurs Azure Logic Apps](/connectors/).

## <a name="compare-playbooks-workbooks-and-notebooks"></a>Comparer des playbooks, des classeurs et des notebooks

Le tableau suivant décrit les différences entre les playbooks, les classeurs et les notebooks dans Azure Sentinel :

| Category |Playbooks  |Workbooks  |Notebooks  |
|---------|---------|---------|---------|
|**Rôles**     |   <ul><li>Ingénieurs du SOC</li><li>Analystes de tous les niveaux</li></ul>      | <ul><li> Ingénieurs du SOC</li><li>Analystes de tous les niveaux</li></ul>       | <ul><li>Chasseurs de menaces et analystes de niveau 2 ou 3</li><li>Enquêteurs sur les incidents</li><li>Scientifiques des données</li><li>Chercheurs en sécurité</li></ul>       |
|**Utilisations**     | Automatisation des tâches simples et reproductibles :<ul><li>Ingestion de données externes </li><li>Enrichissement de données avec TI, recherches GeoIP et plus encore </li><li> Examen </li><li>Correction </li></ul>       | <ul><li>Visualisation</li></ul>        |   <ul><li>Interrogation des données Azure Sentinel et des données externes </li><li>Enrichissement de données avec TI, recherches GeoIP, recherches WhoIs et plus encore </li><li> Examen </li><li> Visualisation </li><li> Chasse </li><li>Machine Learning et analytique du Big Data </li></ul>      |
|**Avantages**     |<ul><li> Idéal pour les tâches uniques et reproductibles </li><li>Aucune connaissance en codage n’est requise  </li></ul>      |<ul><li>Idéal pour une vue élémentaire des données Azure Sentinel </li><li>Aucune connaissance en codage n’est requise</li></ul>       | <ul><li>Idéal pour les chaînes complexes de tâches reproductibles </li><li>Contrôle ad hoc, plus procédural</li><li>Ajout plus facile d’un tableau croisé dynamique grâce aux fonctionnalités interactives </li><li>Bibliothèques Python riches pour la manipulation et la visualisation des données </li><li>Machine Learning et analyse personnalisée </li><li>Facilité de documentation et de partage des preuves d’analyse </li></ul>       |
|**Défis**     | <ul><li>Non adapté aux chaînes ad hoc et complexes de tâches </li><li>Ne convient pas à la documentation et au partage de preuves</li></ul>        |   <ul><li>Intégration impossible à des données externes </li></ul>     |    <ul><li> La courbe d’apprentissage est élevée et nécessite des connaissances en codage </li></ul>   |
|  **Plus d’informations**   | [Automatiser la réponse aux menaces à l’aide de playbooks dans Azure Sentinel](automate-responses-with-playbooks.md)        | [Visualiser les données collectées](get-visibility.md)        | [Utiliser des notebooks Jupyter pour repérer des menaces de sécurité](notebooks.md)        |
|     |         |         |         |

## <a name="comment-on-our-blogs-and-forums"></a>Commentaire sur nos blogs et forums

Nous aimons recevoir des commentaires de nos utilisateurs.

Dans l’espace TechCommunity pour Azure Sentinel :

- [Affichez et commentez les billets de blog récents](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bg-p/AzureSentinelBlog)
- [Publiez vos propres questions sur Azure Sentinel](https://techcommunity.microsoft.com/t5/Azure-Sentinel/bd-p/AzureSentinel)

Vous pouvez également envoyer des suggestions d’améliorations via notre programme [User Voice](https://feedback.azure.com/forums/920458-azure-sentinel).

## <a name="join-the-azure-sentinel-github-community"></a>Rejoindre la communauté GitHub pour Azure Sentinel

Le [Dépôt GitHub Azure Sentinel](https://github.com/Azure/Azure-Sentinel) est une ressource puissante pour la détection des menaces et l’automatisation. 

Nos analystes de sécurité Microsoft créent et ajoutent constamment de nouveaux classeurs, de nouveaux playbooks, de nouvelles requêtes de recherche, entre autres, et les publient dans la communauté pour que vous puissiez les utiliser dans votre environnement. 

Télécharger l’exemple de contenu à partir du dépôt GitHub de la communauté privée pour créer des classeurs, requêtes de recherche, notebooks et playbooks personnalisés pour Azure Sentinel.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Obtenir une certification](/learn/paths/security-ops-sentinel/)

> [!div class="nextstepaction"]
> [Lire les récits de cas d’usage client](https://customers.microsoft.com/en-us/search?sq=%22Azure%20Sentinel%20%22&ff=&p=0&so=story_publish_date%20desc)
