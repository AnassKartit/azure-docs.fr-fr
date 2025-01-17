---
title: Vue d’ensemble des alertes et de la surveillance des notifications dans Azure
description: Vue d’ensemble des alertes dans Azure Monitor
ms.topic: conceptual
ms.date: 02/14/2021
ms.openlocfilehash: ee0d6cd4c895bbcd78767776a86bd63cfa4f5242
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122525132"
---
# <a name="overview-of-alerts-in-microsoft-azure"></a>Vue d’ensemble des alertes dans Microsoft Azure 

Cet article décrit les alertes Microsoft Azure ainsi que leurs avantages, et comment commencer à les utiliser.  

## <a name="what-are-alerts-in-microsoft-azure"></a>Que sont les alertes dans Microsoft Azure ?

Les alertes vous informent de façon proactive lorsque des problèmes sont détectés avec votre infrastructure ou votre application utilisant vos données de surveillance dans Azure Monitor. Elles permettent d’identifier et de résoudre les problèmes avant que les utilisateurs de votre système ne les remarquent. 

## <a name="overview"></a>Vue d’ensemble

Le diagramme ci-dessous représente le flux des alertes. 

![Diagramme du flux des alertes](media/alerts-overview/Azure-Monitor-Alerts.svg)

Les règles d’alerte sont séparées des alertes et des actions effectuées lors du déclenchement d’une alerte. La règle d’alerte capture la cible et les critères d’alerte. La règle d’alerte peut être activée ou désactivée. Les alertes se déclenchent uniquement lorsqu’elles sont activées. 

Les principaux attributs d’une règle d’alerte sont les suivants :

La **ressource cible** définit l’étendue et les signaux disponibles pour les fonctions de génération d’alertes. Une cible peut être n’importe quelle ressource Azure. Exemples de cibles :

- Machines virtuelles
- Comptes de stockage.
- Espace de travail Log Analytics.
- Application Insights. 

Pour certaines ressources (par exemple, les machines virtuelles), vous pouvez spécifier plusieurs ressources comme cibles de la règle d’alerte.

**Signal** : émis par la ressource cible. Les signaux peuvent être des types suivants : Métrique, Journal d’activité, Application Insights et journal.

**Critères** : combinaison du signal et de la logique appliqués à une ressource cible. Exemples : 

- Pourcentage d’UC > 70 %
- Temps de réponse du serveur > 4 ms 
- Nombre de résultats d’une requête de journal > 100

**Nom de l’alerte** : nom spécifique pour la règle d’alerte configurée par l’utilisateur.

**Description de l’alerte** : description de la règle d’alerte configurée par l’utilisateur.

**Gravité** : gravité de l’alerte une fois que les critères spécifiés dans la règle d’alerte réunis. La gravité peut être comprise entre 0 et 4.

- Sev 0 = Critique
- Sev 1 = Erreur
- Sev 2 = Avertissement
- Sev 3 = Informative
- Sev 4 = Détaillée 

**Action** : action spécifique mise en œuvre quand l’alerte est déclenchée. Pour plus d’informations, consultez [Groupes d’actions](../alerts/action-groups.md).

## <a name="what-you-can-alert-on"></a>Sur quoi portent les alertes ?

Vous pouvez déclencher des alertes sur des métriques et des journaux, comme décrit dans [Sources de données de supervision](./../agents/data-sources.md). Les signaux incluent les suivants :

- Valeurs de métrique
- Requêtes de recherche de journal
- Événements du journal d’activité
- Contrôle d’intégrité de la plateforme Azure sous-jacente
- Tests de disponibilité de site web

## <a name="manage-alerts"></a>Gérer les alertes

Vous pouvez définir l’état d’une alerte afin d’indiquer où elle se situe dans le processus de résolution. Lorsque les critères spécifiés dans la règle d’alerte sont réunis, une alerte est créée ou déclenchée, dont le statut est *Nouvelle*. Vous pouvez changer son état après l’avoir reconnue ou fermée. Les changements d’état sont stockés dans l’historique de l’alerte.

Les états d’alerte suivants sont pris en charge.

| State | Description |
|:---|:---|
| Nouveau | Le problème a été détecté et n’a pas encore été examiné. |
| Reconnu | Un administrateur a révisé l’alerte et a commencé à travailler sur celle-ci. |
| Fermés | Le problème a été résolu. Après qu’une alerte a été fermée, vous pouvez la rouvrir en modifiant son état. |

L’*état d’alerte* est différent et indépendant de la *condition d’analyse*. L’état de l’alerte est défini par l’utilisateur. La condition de l’analyse est définie par le système. Quand une alerte se déclenche, la condition de surveillance de l’alerte est définie sur *« déclenchée »* et, lorsque la condition sous-jacente à l’origine du déclenchement de l’alerte disparaît, la condition de surveillance est définie sur *« résolue »* . 

L’état de l’alerte n’est pas modifié jusqu’à ce que l’utilisateur la modifie. Découvrez comment [modifier l’état de vos alertes et de vos groupes intelligents](./alerts-managing-alert-states.md?toc=%2fazure%2fazure-monitor%2ftoc.json).

## <a name="alerts-experience"></a>Expérience d’alertes 
La page Alertes par défaut fournit un résumé des alertes qui sont créées dans un intervalle de temps spécifique. Elle affiche le nombre total d’alertes pour chaque niveau de gravité avec des colonnes identifiant le nombre total d’alertes dans chaque état pour chaque niveau de gravité. Sélectionnez l’un des niveaux de gravité pour ouvrir la page [Toutes les alertes](#all-alerts-page) filtrée sur ce niveau de gravité.

À la place, vous pouvez [énumérer par programmation les instances d’alertes générées sur vos abonnements à l’aide d’API REST](#manage-your-alert-instances-programmatically).

> [!NOTE]
   >  Vous ne pouvez accéder qu’aux alertes générées au cours des 30 derniers jours.

Vous pouvez modifier les abonnements ou filtrer des paramètres pour mettre à jour la page.

![Capture d’écran de la page Alertes](media/alerts-overview/alerts-page.png)

Vous pouvez filtrer cette vue en sélectionnant des valeurs dans les menus déroulants en haut de la page.

| Colonne | Description |
|:---|:---|
| Abonnement | Sélectionnez les abonnements Azure pour lesquels vous souhaitez afficher les alertes. Vous pouvez sélectionner tous vos abonnements, si vous le souhaitez. Seules les alertes auxquelles vous pouvez accéder dans les abonnements sélectionnés sont incluses dans la vue. |
| Resource group | Sélectionnez un seul groupe de ressources. Seules les alertes avec des cibles dans le groupe de ressources sélectionné sont incluses dans la vue. |
| Plage temporelle | Seules les alertes déclenchées dans l’intervalle de temps sélectionné sont incluses dans l’affichage. Les valeurs prises en charge sont : dernière heure, dernières 24 heures, 7 derniers jours et 30 derniers jours. |

Sélectionnez les valeurs suivantes en haut de la page Alertes pour ouvrir une autre page :

| Valeur | Description |
|:---|:---|
| Nombre total d’alertes | Nombre total d’alertes correspondant aux critères sélectionnés. Sélectionnez cette valeur pour ouvrir l’affichage Toutes les alertes sans filtre. |
| Groupes intelligents | Nombre total de groupes intelligents créés par les alertes correspondant aux critères sélectionnés. Sélectionnez cette valeur pour ouvrir la liste des groupes intelligents dans l’affichage Toutes les alertes.
| Nombre total de règles d’alerte | Nombre total de règles d’alerte dans l’abonnement et le groupe de ressources sélectionnés. Sélectionnez cette valeur pour ouvrir l’affichage Règles, filtré sur l’abonnement et le groupe de ressources sélectionnés.


## <a name="manage-alert-rules"></a>Gérer les règles d’alerte
Pour afficher la page **Règles**, sélectionnez **Gérer les règles d’alerte**. La page Règles est un emplacement unique permettant de gérer toutes les règles d’alerte de vos abonnements Azure. Elle répertorie toutes les règles d’alerte et peut être triée en fonction des ressources cibles, des groupes de ressources, du nom de règle ou de l’état. Vous pouvez également modifier, activer ou désactiver des règles d’alerte à partir de cette page.  

 ![Capture d’écran de la page Règles](./media/alerts-overview/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Création d'une règle d'alerte
Vous pouvez créer des règles d’alerte de manière cohérente, quel que soit le service de surveillance ou le type de signal.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4tflw]

 
Voici comment créer une règle d’alerte :
1. Choisissez la _cible_ de l’alerte.
1. Sélectionnez le _signal_ parmi les signaux disponibles pour la cible.
1. Spécifiez la _logique_ à appliquer aux données du signal.

Ce processus de création simplifié ne nécessite plus que vous connaissiez la source de la surveillance ou les signaux pris en charge avant de sélectionner une ressource Azure. La liste des signaux disponibles est automatiquement filtrée en fonction de la ressource cible sélectionnée. Toujours sur la base de cette cible, vous êtes automatiquement guidé dans la définition de la logique de la règle d’alerte.  

Pour plus d’informations sur la création de règles d’alerte, voir [Créer, afficher et gérer des alertes à l’aide d’Azure Monitor](../alerts/alerts-metric.md).

Les alertes sont disponibles dans plusieurs services de surveillance Azure. Pour plus d’informations sur la façon d’utiliser chacun de ces services et le moment opportun pour le faire, voir [Surveillance des applications et des ressources Azure](../overview.md). 


## <a name="all-alerts-page"></a>Page Toutes les alertes 
Pour afficher la page **Toutes les alertes**, sélectionnez **Nombre total d’alertes**. Vous obtenez la liste des alertes créées durant la période sélectionnée. Vous pouvez afficher une liste des alertes individuelles ou une liste des groupes intelligents contenant les alertes. Sélectionnez la bannière en haut de la page pour basculer entre les affichages.

![Capture d’écran de la page Toutes les alertes](media/alerts-overview/all-alerts-page.png)

Vous pouvez filtrer l’affichage en sélectionnant les valeurs suivantes dans les menus déroulants en haut de la page :

| Colonne | Description |
|:---|:---|
| Abonnement | Sélectionnez les abonnements Azure pour lesquels vous souhaitez afficher les alertes. Vous pouvez sélectionner tous vos abonnements, si vous le souhaitez. Seules les alertes auxquelles vous pouvez accéder dans les abonnements sélectionnés sont incluses dans la vue. |
| Resource group | Sélectionnez un seul groupe de ressources. Seules les alertes avec des cibles dans le groupe de ressources sélectionné sont incluses dans la vue. |
| Type de ressource | Sélectionnez un ou plusieurs types de ressources. Seules les alertes avec des cibles du type sélectionné sont incluses dans la vue. Cette colonne n’est disponible qu’après qu’un groupe de ressources a été spécifié. |
| Ressource | Sélectionnez une ressource. Seules les alertes ayant ces ressources pour cible sont incluses dans l’affichage. Cette colonne n’est disponible qu’après qu’un type de ressource a été spécifié. |
| Gravité | Sélectionnez un niveau de gravité d’alerte ou **Tous** pour inclure les alertes de tous les niveaux de gravité. |
| Condition de surveillance | Sélectionnez une condition d’analyse ou **Toutes** pour inclure les alertes correspondant à toutes les conditions. |
| État d’alerte | Sélectionnez un état d’alerte ou **Toutes** pour inclure les alertes correspondant à tous les états. |
| Service de surveillance | Sélectionnez un service ou **Tous** pour inclure tous les services. Seules les alertes créées par des règles utilisant ce service comme cible sont incluses. |
| Plage temporelle | Seules les alertes déclenchées dans l’intervalle de temps sélectionné sont incluses dans l’affichage. Les valeurs prises en charge sont : dernière heure, dernières 24 heures, 7 derniers jours et 30 derniers jours. |

Cliquez sur **Colonnes** en haut de la page pour sélectionner les colonnes à afficher. 

## <a name="alert-details-page"></a>Page Détails de l’alerte
Lorsque vous sélectionnez une alerte, cette page affiche les détails de l’alerte et vous permet de modifier l’état de celle-ci.

![Capture d’écran de la page Détails de l’alerte](media/alerts-overview/alert-detail2.png)

La page Détails de l’alerte comprend les sections suivantes :

| Section | Description |
|:---|:---|
| Résumé | Affiche les propriétés et autres informations importantes sur l’alerte. |
| Historique | Répertorie chaque action effectuée par l’alerte et toutes les modifications apportées à l’alerte. Actuellement limité aux changements d’état. |
| Diagnostics | Informations sur le groupe intelligent contenant l’alerte. Le *nombre d’alertes* fait référence au nombre d’alertes incluses dans le groupe intelligent. Cela comprend les autres alertes du même groupe intelligent qui ont été créées au cours des 30 derniers jours, quel que soit le filtre de temps dans la page de la liste des alertes. Sélectionnez une alerte pour afficher ses détails. |

## <a name="azure-role-based-access-control-azure-rbac-for-your-alert-instances"></a>Contrôle d’accès en fonction du rôle Azure (Azure RBAC) pour vos instances d’alertes

Pour pouvoir utiliser et gérer des instances d’alerte, l’utilisateur doit disposer du rôle Azure intégré [Contributeur d’analyse](../../role-based-access-control/built-in-roles.md#monitoring-contributor) ou [Lecteur d’analyse](../../role-based-access-control/built-in-roles.md#monitoring-reader). Ces rôles sont pris en charge à tout niveau de Microsoft Azure Resource Manager, du niveau de l’abonnement aux affectations granulaires, en passant par le niveau des ressources. Par exemple, si un utilisateur dispose uniquement d’un accès de type Contributeur d’analyse à la machine virtuelle `ContosoVM1`, il ne peut utiliser et gérer que les alertes générées sur `ContosoVM1`.

## <a name="manage-your-alert-instances-programmatically"></a>Gérer vos instances d’alerte par programmation

Vous pouvez interroger par programmation les alertes générées pour votre abonnement. Des requêtes peut consister à créer des vues personnalisées en dehors du portail Microsoft Azure, ou à analyser vos alertes pour identifier des tendances et modèles.

Il est recommandé d’utiliser [Azure Resource Graph](../../governance/resource-graph/overview.md) avec le schéma `AlertsManagementResources` pour interroger les alertes déclenchées. Resource Graph est recommandé lorsque vous devez gérer des alertes générées pour plusieurs abonnements.

L’exemple suivant de demande adressée à l’API REST Resource Graph renvoie les alertes au sein d’un seul abonnement au cours du dernier jour :

```json
{
  "subscriptions": [
    <subscriptionId>
  ],
  "query": "alertsmanagementresources | where properties.essentials.lastModifiedDateTime > ago(1d) | project alertInstanceId = id, parentRuleId = tolower(tostring(properties['essentials']['alertRule'])), sourceId = properties['essentials']['sourceCreatedId'], alertName = name, severity = properties.essentials.severity, status = properties.essentials.monitorCondition, state = properties.essentials.alertState, affectedResource = properties.essentials.targetResourceName, monitorService = properties.essentials.monitorService, signalType = properties.essentials.signalType, firedTime = properties['essentials']['startDateTime'], lastModifiedDate = properties.essentials.lastModifiedDateTime, lastModifiedBy = properties.essentials.lastModifiedUserName"
}
```

Vous pouvez également voir le résultat de cette requête Resource Graph dans le portail avec Azure Resource Graph Explorer : [portal.azure.com](https://portal.azure.com/?feature.customportal=false#blade/HubsExtension/ArgQueryBlade/query/alertsmanagementresources%0A%7C%20where%20properties.essentials.lastModifiedDateTime%20%3E%20ago(1d)%0A%7C%20project%20alertInstanceId%20%3D%20id%2C%20parentRuleId%20%3D%20tolower(tostring(properties%5B 'essentials'%5D%5B'alertRule'%5D))%2C%20sourceId%20%3D%20properties%5B'essentials'%5D%5B'sourceCreatedId'%5D%2C%20alertName%20%3D%20name%2C%20severity%20%3D%20properties.essentials.severity%2C%20status%20%3D%20properties.essentials.monitorCondition%2C%20state%20%3D%20properties.essentials.alertState%2C%20affectedResource%20%3D%20properties.essentials.targetResourceName%2C%20monitorService%20%3D%20properties.essentials.monitorService%2C%20signalType%20%3D%20properties.essentials.signalType%2C%20firedTime%20%3D%20properties%5B'essentials'%5D%5B'startDateTime'%5D%2C%20lastModifiedDate%20%3D%20properties.essentials.lastModifiedDateTime%2C%20lastModifiedBy%20%3D%20properties.essentials.lastModifiedUserName)

Vous pouvez également utiliser l’[API REST Alert Management](/rest/api/monitor/alertsmanagement/alerts) dans les scénarios d’interrogation à l’échelle inférieure ou pour mettre à jour les alertes déclenchées.

## <a name="smart-groups"></a>Groupes intelligents

Les groupes intelligents sont des agrégations d’alertes reposant sur des algorithmes de Machine Learning qui permettent de réduire le bruit des alertes et de faciliter la résolution des problèmes. [En savoir plus sur les groupes intelligents](./alerts-smartgroups-overview.md?toc=%2fazure%2fazure-monitor%2ftoc.json) et sur [leur gestion](./alerts-managing-smart-groups.md?toc=%2fazure%2fazure-monitor%2ftoc.json).

## <a name="next-steps"></a>Étapes suivantes

- [En savoir plus sur les groupes intelligents](./alerts-smartgroups-overview.md?toc=%2fazure%2fazure-monitor%2ftoc.json)
- [En savoir plus sur les groupes d’actions](../alerts/action-groups.md)
- [Gestion des instances d’alertes dans Azure](./alerts-managing-alert-instances.md?toc=%2fazure%2fazure-monitor%2ftoc.json)
- [Gestion des groupes intelligents](./alerts-managing-smart-groups.md?toc=%2fazure%2fazure-monitor%2ftoc.json)
- [Découvrir les tarifs des alertes Azure](https://azure.microsoft.com/pricing/details/monitor/)
