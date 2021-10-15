---
title: Résolution des problèmes liés aux alertes de métrique Azure
description: Problèmes couramment rencontrés avec les alertes de métrique Azure Monitor et solutions possibles
author: harelbr
ms.author: harelbr
ms.topic: troubleshooting
ms.date: 09/30/2021
ms.openlocfilehash: df925bd149c8516f4c6af8b49a65969737aaffa2
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2021
ms.locfileid: "129352379"
---
# <a name="troubleshooting-problems-in-azure-monitor-metric-alerts"></a>Résolution des problèmes liés aux alertes de métrique dans Azure Monitor 

Cet article décrit les problèmes couramment rencontrés avec les alertes de métrique dans [Azure Monitor](alerts-metric-overview.md) et explique comment y remédier.

Azure Monitor vous avertit de façon proactive lorsque des conditions significatives sont détectées dans vos données de surveillance. Elles permettent d’identifier et de résoudre les problèmes avant que les utilisateurs de votre système ne les remarquent. Pour plus d’informations sur les alertes, consultez [Vue d’ensemble des alertes dans Microsoft Azure](./alerts-overview.md).

## <a name="metric-alert-should-have-fired-but-didnt"></a>Une alerte de métrique ne s'est pas déclenchée alors qu'elle aurait dû 

Si vous pensez qu'une alerte de métrique aurait dû se déclencher mais que cela n'a pas été le cas et qu'elle est introuvable sur le portail Azure, procédez comme suit :

1. **Configuration** - Vérifiez la configuration de la règle d'alerte de métrique pour vous assurer qu'elle est correcte :
    - Vérifiez que les valeurs spécifiées dans les champs **Type d'agrégation**, **Granularité d'agrégation (période)** sont configurées comme prévu. Le paramètre **Type d’agrégation** détermine la manière dont les valeurs de métriques sont agrégées (plus d’informations [ici](../essentials/metrics-aggregation-explained.md#aggregation-types)), et le paramètre **Granularité d’agrégation (période)** spécifie jusqu’où l’évaluation doit remonter pour agréger les valeurs de métriques à chaque exécution de la règle d’alerte.
    -  Vérifiez que les paramètres **Valeur de seuil** et **Sensibilité** sont configurés comme prévu.
    - Pour une règle d'alerte qui utilise des seuils dynamiques, vérifiez si certains paramètres avancés sont configurés, car l'option **Nombre de violations** peut filtrer les alertes et l'option **Ignorer les données avant** peut avoir une incidence sur le calcul des seuils.

       > [!NOTE] 
       > Les seuils dynamiques nécessitent au moins 3 jours et 30 échantillons de métriques avant de devenir actifs.

2. **Déclenchée mais pas de notification** - Consultez la [liste des alertes déclenchées](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/alertsV2) pour essayer de localiser l'alerte déclenchée. Si l'alerte figure dans la liste, mais que vous rencontrez des problèmes avec certaines de ses actions ou notifications, cliquez [ici](./alerts-troubleshoot.md#action-or-notification-on-my-alert-did-not-work-as-expected) pour en savoir plus.

3. **Déjà active** - Vérifiez si une alerte a déjà été déclenchée sur la série chronologique de métrique pour laquelle vous vous attendiez à recevoir une alerte. Les alertes de métriques sont avec état. Cela qui signifie qu’une fois qu’une alerte est déclenchée sur une série chronologique de métrique spécifique, des alertes supplémentaires sur cette série chronologique ne sont pas déclenchées tant que le problème n’est plus observé. Ce choix de conception réduit le bruit. L'alerte est automatiquement résolue lorsque la condition d'alerte n'est pas remplie au bout de trois évaluations consécutives.

4. **Dimensions utilisées** - Si vous avez sélectionné des [valeurs de dimension pour une métrique](./alerts-metric-overview.md#using-dimensions), la règle d’alerte supervise chaque série chronologique de métrique (comme défini par la combinaison de valeurs de dimension) pour détecter un dépassement de seuil. Pour superviser également la série chronologique de métrique agrégée (sans aucune dimension sélectionnée), configurez une règle d'alerte supplémentaire sur la métrique sans sélectionner de dimensions.

5. **Agrégation et granularité temporelle** - Si vous visualisez la métrique à l’aide de [graphiques de métriques](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/metrics), assurez-vous que :
    * La valeur d'**Agrégation** sélectionnée dans le graphique de métriques est identique à la valeur de **Type d'agrégation** de votre règle d'alerte
    * La valeur de **Granularité temporelle** sélectionnée est identique à la valeur de **Granularité d'agrégation (période)** de votre règle d'alerte (et qu'elle n'est pas définie sur « Automatique »)

## <a name="metric-alert-fired-when-it-shouldnt-have"></a>Alerte de métrique déclenchée à tort

Si vous pensez que votre alerte de métrique a été déclenchée à tort, la procédure ci-dessous peut vous aider à résoudre le problème.

1. Passez en revue la [liste des alertes déclenchées](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/alertsV2) pour localiser l'alerte déclenchée, puis cliquez pour afficher les détails correspondants. Consultez les informations fournies sous **Pourquoi cette alerte a-t-elle été déclenchée ?** pour voir le graphique des métriques, la **Valeur métrique** et la **Valeur seuil** au moment où l'alerte a été déclenchée.

    > [!NOTE] 
    > Si vous utilisez un type de condition Seuils dynamiques et pensez que les seuils utilisés ne sont pas corrects, envoyez un smiley mécontent en guise de commentaire. Ce commentaire aura un impact sur la recherche algorithmique de l'apprentissage automatique et contribuera à améliorer les détections futures.

2. Si vous avez sélectionné plusieurs valeurs de dimension pour une métrique, l’alerte sera déclenchée lorsqu’**une** métrique de la série chronologique (comme défini par la combinaison de valeurs de dimension) dépassera le seuil. Pour plus d’informations sur l’utilisation des dimensions dans les alertes de métrique, consultez [cette page](./alerts-metric-overview.md#using-dimensions).

3. Vérifiez la configuration de la règle d'alerte pour vous assurer qu'elle est correcte :
    - Vérifiez que les valeurs spécifiées dans les champs **Type d'agrégation**, **Granularité d'agrégation (période)** ainsi que **Valeur de seuil** ou **Sensibilité** sont configurées comme prévu.
    - Pour une règle d'alerte qui utilise des seuils dynamiques, vérifiez si certains paramètres avancés sont configurés, car l'option **Nombre de violations** peut filtrer les alertes et l'option **Ignorer les données avant** peut impacter le calcul des seuils.

   > [!NOTE]
   > Les seuils dynamiques nécessitent au moins 3 jours et 30 échantillons de métriques avant de devenir actifs.

4. Si vous visualisez la métrique à l’aide d’un [graphique de métriques](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/metrics), assurez-vous que :
    - La valeur d'**Agrégation** sélectionnée dans le graphique de métriques est identique à la valeur de **Type d'agrégation** de votre règle d'alerte
    - La valeur de **Granularité temporelle** sélectionnée est identique à la valeur de **Granularité d'agrégation (période)** de votre règle d'alerte (et qu'elle n'est pas définie sur « Automatique »)

5. Si l’alerte se déclenche alors que d'autres alertes (non résolues) surveillant les mêmes critères se sont déjà déclenchées, vérifiez si la règle d'alerte a été configurée pour ne pas résoudre les alertes automatiquement. Une telle configuration a pour effet que la règle d’alerte devient sans état, ce qui signifie qu’elle ne résout pas automatiquement les alertes déclenchées et n’exige pas qu’une alerte déclenchée soit résolue avant de se déclencher à nouveau sur la même série chronologique.
    Vous pouvez vérifier si la règle d’alerte est configurée pour ne pas résoudre les alertes automatiquement de l’une des manières suivantes :
    - En modifiant la règle d’alerte dans le portail Azure et en vérifiant que la case à cocher « Résoudre automatiquement les alertes » est désactivée (dans la section « Détails des règles d’alerte »).
    - En examinant le script utilisé pour déployer la règle d’alerte, ou en extrayant la définition de la règle d’alerte et en vérifiant si la propriété *AutoMitigate* est définie sur **false**.


## <a name="cant-find-the-metric-to-alert-on---virtual-machines-guest-metrics"></a>Impossible de trouver la métrique sur laquelle porte l’alerte – métriques d’invités de machines virtuelles

Pour le bon fonctionnement des alertes relatives aux métriques du système d’exploitation invité de machines virtuelles (par exemple, la mémoire, l’espace disque), assurez-vous d’avoir installé l’agent requis pour collecter ces données dans Azure Monitor Metrics :
- [Pour les machines virtuelles Windows](../essentials/collect-custom-metrics-guestos-resource-manager-vm.md)
- [Pour les machines virtuelles Linux](../essentials/collect-custom-metrics-linux-telegraf.md)

Pour plus d’informations sur la collecte de données à partir du système d’exploitation invité d’une machine virtuelle, voir [ici](../vm/monitor-vm-azure.md#guest-operating-system).

> [!NOTE] 
> Si vous avez configuré les métriques invitées pour qu'elles soient envoyées à un espace de travail Log Analytics, ces métriques apparaissent sous la ressource de l'espace de travail Log Analytics. Elles commencent à afficher les données **uniquement** après la création d'une règle d'alerte qui les supervise. Pour ce faire, suivez les étapes permettant de [configurer une alerte de métrique pour les journaux](./alerts-metric-logs.md#configuring-metric-alert-for-logs).

> [!NOTE] 
> La surveillance d’une métrique invitée pour plusieurs machines virtuelles avec une seule règle d’alerte n’est actuellement pas prise en charge par les alertes de métrique. Vous pouvez réaliser cela avec une [règle d’alerte de journal](./alerts-unified-log.md). Pour ce faire, assurez-vous que les métriques invitées sont collectées dans un espace de travail Log Analytics et créez une règle d’alerte de journal sur cet espace de travail.

## <a name="cant-find-the-metric-to-alert-on"></a>Métrique introuvable pour déclencher l'alerte

Si vous envisagez de déclencher une alerte sur une métrique spécifique, mais ne la voyez pas au moment de créer une règle d’alerte, vérifiez ce qui suit :
- Si vous ne voyez aucune métrique pour la ressource, [vérifiez si le type de ressource est pris en charge pour les alertes de métrique](./alerts-metric-near-real-time.md).
- Si des métriques existent pour la ressource mais qu’une métrique spécifique est introuvable, [vérifiez si cette métrique est disponible](../essentials/metrics-supported.md), et si oui, consultez sa description pour savoir si elle est uniquement disponible dans des versions ou éditions spécifiques de la ressource.
- Si la métrique n’est pas disponible pour la ressource, elle peut être disponible dans les journaux de ressources et peut être supervisée à l’aide d’alertes de journal. Pour plus d’informations, consultez cet article sur la façon [de collecter et d’analyser les journaux des ressources à partir d’une ressources Azure](../essentials/tutorial-resource-logs.md).

## <a name="cant-find-the-metric-dimension-to-alert-on"></a>Dimension de métrique introuvable pour déclencher l'alerte

Si vous souhaitez déclencher des alertes liées aux [valeurs de dimension spécifiques d’une métrique](./alerts-metric-overview.md#using-dimensions), mais que vous ne trouvez pas ces valeurs, notez ce qui suit :

1. L’affichage des valeurs de dimension sous la liste **Valeurs de dimension** peut prendre quelques minutes.
2. Les valeurs de dimension affichées se basent sur les données de métriques collectées au cours du dernier jour.
3. Si la valeur de dimension n’est pas encore émise ou n’est pas affichée, vous pouvez utiliser l’option « Ajouter une valeur personnalisée » pour ajouter une valeur de dimension personnalisée.
4. Si vous souhaitez déclencher une alerte sur l'ensemble des valeurs possibles d'une dimension (y compris les valeurs futures), choisissez l’option « Sélectionner toutes les valeurs actuelles et futures ».
5. Les dimensions de métriques personnalisées des ressources Application Insights sont désactivées par défaut. Pour activer la collecte de dimensions pour ces métriques personnalisées, consultez [ceci](../app/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation).

## <a name="metric-alert-rules-still-defined-on-a-deleted-resource"></a>Règles d'alerte de métrique toujours définies sur une ressource supprimée 

Lors de la suppression d’une ressource Azure, les règles d’alerte de métrique associées ne sont pas supprimées automatiquement. Pour supprimer les règles d'alerte associées à une ressource qui a été supprimée :

1. Ouvrez le groupe de ressources dans lequel la ressource supprimée a été définie.
1. Dans la liste affichant les ressources, cochez la case **Afficher les types masqués**.
1. Filtrez la liste par Type == **microsoft.insights/metricalerts**.
1. Sélectionnez les règles d'alerte appropriées, puis **Supprimer**.

## <a name="make-metric-alerts-occur-every-time-my-condition-is-met"></a>Générer des alertes de métrique chaque fois que ma condition est remplie

Par défaut, un état est attribué aux alertes de métrique. Par conséquent, aucune alerte supplémentaire n'est déclenchée si une alerte est déjà déclenchée sur une série chronologique donnée. Si vous souhaitez créer une règle d’alerte de métrique spécifique et recevoir une alerte pour chaque évaluation dans laquelle la condition d’alerte est remplie, procédez de l’une des façons suivantes :
- Si vous créez la règle d’alerte par programme (par exemple, via [Resource Manager](./alerts-metric-create-templates.md), [PowerShell](/powershell/module/az.monitor/), [REST](/rest/api/monitor/metricalerts/createorupdate), [CLI](/cli/azure/monitor/metrics/alert)), affectez la valeur « false » à la propriété *AutoMitigate*.
- Si vous créez la règle d’alerte via le portail Azure, désactivez l’option « Résoudre automatiquement les alertes » (disponible dans la section « Détails des règles d’alerte »).

> [!NOTE] 
> La création d'une règle d'alerte de métrique sans état empêche la résolution des alertes déclenchées. Par conséquent, même si la condition n'est plus remplie, les alertes restent déclenchées jusqu'à la fin de la période de rétention de 30 jours.

## <a name="define-an-alert-rule-on-a-custom-metric-that-isnt-emitted-yet"></a>Définir une règle d’alerte sur une métrique personnalisée qui n’est pas encore émise

Lors de la création d’une règle d’alerte de métrique, le nom de la métrique est validé par rapport à l’[API de définitions de métriques](/rest/api/monitor/metricdefinitions/list) pour s’assurer qu’elle existe. Dans certains cas, vous pouvez être amené à créer une règle d’alerte sur une métrique personnalisée avant qu’elle soit émise. Par exemple, lors de la création (à l’aide d’un modèle Resource Manager) d’une ressource Application Insights qui émettra une métrique personnalisée, en même temps qu’une règle d’alerte qui surveille cette métrique.

Pour éviter que le déploiement échoue quand vous tentez de valider les définitions de la métrique personnalisée, vous pouvez utiliser le paramètre *skipMetricValidation* dans la section des critères de la règle d’alerte, qui a pour effet que la validation de la métrique est ignorée. Consultez l’exemple ci-dessous pour savoir comment utiliser ce paramètre dans un modèle Resource Manager. Pour plus d’informations, consultez les [exemples de modèles Resource Manager complets pour la création de règles d’alerte de métrique](./alerts-metric-create-templates.md).

```json
"criteria": {
    "odata.type": "Microsoft.Azure.Monitor.SingleResourceMultipleMetricCriteria",
        "allOf": [
            {
                "name" : "condition1",
                "metricName": "myCustomMetric",
                "metricNamespace": "myCustomMetricNamespace",
                "dimensions":[],
                "operator": "GreaterThan",
                "threshold" : 10,
                "timeAggregation": "Average",
                "skipMetricValidation": true
            }
        ]
    }
```
> [!NOTE] 
> L’utilisation du paramètre *skipMetricValidation* est également requise lors de la définition d’une règle d’alerte sur une métrique personnalisée existante qui n’a pas été émise en plus de trois jours.

## <a name="export-the-azure-resource-manager-template-of-a-metric-alert-rule-via-the-azure-portal"></a>Exporter le modèle Azure Resource Manager d’une règle d’alerte de métrique via le Portail Azure

L’exportation du modèle Azure Resource Manager d’une règle d’alerte de métrique vous aide à comprendre sa syntaxe et ses propriétés JSON, et elle peut être utilisée pour automatiser les déploiements futurs.
1. Dans le portail Azure, ouvrez la règle d’alerte pour en voir les détails.
2. Cliquez sur **Propriétés**.
3. Sous **Automation**, sélectionnez **Exporter le modèle**.

## <a name="metric-alert-rules-quota-too-small"></a>Quota de règles d'alerte de métrique trop bas

Le nombre autorisé de règles d'alerte de métrique par abonnement est soumis à des [limites de quota](../service-limits.md).

Si vous avez atteint la limite de quota, les étapes suivantes peuvent vous aider à résoudre le problème :
1. Essayez de supprimer ou de désactiver les règles d'alerte de métrique qui ne sont plus utilisées.

2. Utilisez plutôt des règles d’alerte de métrique qui supervisent plusieurs ressources. Avec cette fonctionnalité, une même règle d'alerte peut superviser plusieurs ressources en utilisant une seule règle d'alerte comptabilisée dans le quota. Pour plus d’informations sur cette fonctionnalité et les types de ressources pris en charge, consultez [cette page](./alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor).

3. Si vous avez besoin d'augmenter la limite de quota, ouvrez une demande de support et fournissez les informations suivantes :

    - ID d'abonnement pour lesquels la limite de quota doit être relevée
    - Type de ressource pour l’augmentation de quota : **Alertes de métrique** ou **Alertes de métrique (classiques)**
    - Limite de quota demandée

## <a name="check-total-number-of-metric-alert-rules"></a>Vérifier le nombre total de règles d'alerte de métrique

Pour vérifier l'utilisation actuelle des règles d'alerte de métrique, procédez comme suit.

### <a name="from-the-azure-portal"></a>À partir du portail Azure

1. Ouvrez l’écran **Alertes**, puis cliquez sur **Gérer les règles d’alerte**.
2. Filtrez sur l'abonnement approprié à l'aide du contrôle de liste déroulante **Abonnement**.
3. Veillez à ne PAS filtrer sur un groupe de ressources, un type de ressource ou une ressource spécifique.
4. Dans le contrôle de liste déroulante **Type de signal**, sélectionnez **Métriques**.
5. Vérifiez que le contrôle de liste déroulante **État** est défini sur la valeur **Activé**.
6. Le nombre total de règles d’alerte de métrique est affiché au-dessus de la liste des règles.

### <a name="from-api"></a>À partir de l’API

- PowerShell - [Get-AzMetricAlertRuleV2](/powershell/module/az.monitor/get-azmetricalertrulev2)
- API REST - [Répertorier par abonnement](/rest/api/monitor/metricalerts/listbysubscription)
- Azure CLI - [az monitor metrics alert list](/cli/azure/monitor/metrics/alert#az_monitor_metrics_alert_list)

## <a name="managing-alert-rules-using-resource-manager-templates-rest-api-powershell-or-azure-cli"></a>Gérer les règles d'alerte à l'aide de modèles Resource Manager, de l'API REST, de PowerShell ou d'Azure CLI

Si vous rencontrez des problèmes lors de la création, de la mise à jour, de la récupération ou de la suppression d’alertes de métrique avec des modèles Resource Manager, l’API REST, PowerShell ou l’interface de ligne de commande (CLI) Azure, les étapes suivantes peuvent vous aider à les résoudre.

### <a name="resource-manager-templates"></a>Modèles Resource Manager

- Consultez la liste des [erreurs courantes de déploiement Azure](../../azure-resource-manager/templates/common-deployment-errors.md) et procédez aux mesures correctives en conséquence
- Consultez les [exemples de modèles Azure Resource Manager d’alertes de métrique](./alerts-metric-create-templates.md) pour vous assurer que vous transmettez correctement tous les paramètres

### <a name="rest-api"></a>API REST

Consultez le [guide de l’API REST](/rest/api/monitor/metricalerts/) pour vérifier que vous passez tous les paramètres correctement.

### <a name="powershell"></a>PowerShell

Veillez à utiliser les cmdlets PowerShell appropriées pour les alertes de métrique :

- Les applets de commande PowerShell pour les alertes de métrique sont disponibles dans le [module Az.Monitor](/powershell/module/az.monitor/).
- Veillez à utiliser les cmdlets qui se terminent par « V2 » pour les nouvelles alertes de métrique (non classiques). Par exemple : [Add-AzMetricAlertRuleV2](/powershell/module/az.monitor/add-azmetricalertrulev2).

### <a name="azure-cli"></a>Azure CLI

Veillez à utiliser les commandes CLI appropriées pour les alertes de métrique :

- Les commandes CLI des alertes de métrique commencent par `az monitor metrics alert`. Consultez la [référence Azure CLI](/cli/azure/monitor/metrics/alert) pour en savoir plus sur la syntaxe.
- Vous pouvez voir un [exemple illustrant l’utilisation de l’interface CLI pour les alertes de métrique](./alerts-metric.md#with-azure-cli).
- Pour déclencher une alerte sur une métrique personnalisée, veillez à faire précéder le nom de la métrique par l’espace de noms de métrique approprié : NAMESPACE.METRIC

### <a name="general"></a>Général

- Si vous recevez une erreur `Metric not found` :

   - Pour une métrique de plateforme : vérifiez que vous utilisez le nom de la **métrique** mentionnée sur [la page des métriques prises en charge par Azure Monitor](../essentials/metrics-supported.md) et non le **nom d’affichage de la métrique**.

   - Pour une métrique personnalisée : vérifiez que la métrique est déjà émise (vous ne pouvez pas créer de règle d’alerte sur une métrique personnalisée qui n’existe pas encore) et que vous fournissez l’espace de noms de la métrique personnalisée (consultez un exemple de modèle Azure Resource Manager [ici](./alerts-metric-create-templates.md#template-for-a-static-threshold-metric-alert-that-monitors-a-custom-metric))

- Si vous créez des [alertes de métrique sur les journaux](./alerts-metric-logs.md), vérifiez que les dépendances appropriées sont incluses. Consultez [Exemple de modèle](./alerts-metric-logs.md#resource-template-for-metric-alerts-for-logs).

- Si vous créez une règle d’alerte qui contient plusieurs critères, notez les contraintes suivantes :

   - Vous ne pouvez sélectionner qu’une seule valeur par dimension au sein de chaque critère
   - Vous ne pouvez pas utiliser « \* » comme valeur de dimension
   - Lorsque des métriques configurées dans différents critères prennent en charge la même dimension, une valeur de dimension configurée doit être explicitement définie de la même façon pour toutes ces métriques (consultez un exemple de modèle Resource Manager [ici](./alerts-metric-create-templates.md#template-for-a-static-threshold-metric-alert-that-monitors-multiple-criteria)).


## <a name="no-permissions-to-create-metric-alert-rules"></a>Aucune autorisation pour créer des règles d'alerte de métrique

Pour créer une règle d'alerte de métrique, vous devez disposer des autorisations suivantes :

- Autorisation de lecture sur la ressource cible de la règle d'alerte
- Autorisation d’écriture sur le groupe de ressources dans lequel la règle d’alerte est créée (si vous créez la règle d’alerte à partir du portail Azure, elle est créée par défaut dans le groupe de ressources dans lequel réside la ressource cible)
- Autorisation de lecture sur tout groupe d'actions associé à la règle d'alerte (le cas échéant)


## <a name="naming-restrictions-for-metric-alert-rules"></a>Restrictions de nommage pour les règles d’alerte d’indicateurs de performance

Tenez compte des restrictions suivantes pour les noms de règle d’alerte d’indicateurs de performance :

- Les noms de règle d’alerte d’indicateurs de performance ne peuvent pas être modifiés (renommés) une fois créés
- Les noms de règles d’alerte des indicateurs de performance doivent être uniques au sein d’un groupe de ressources
- Les noms de règle d’alerte des indicateurs de performance ne peuvent pas contenir les caractères suivants : * # & + : < > ? @ % { } \ / 
- Les noms de règle d’alerte ne peuvent pas se terminer par un espace ni par un point
- Le nom du groupe de ressources et le nom de la règle d’alerte combinés ne peuvent pas dépasser 252 caractères

> [!NOTE] 
> Si le nom de la règle d’alerte contient des caractères qui ne sont ni alphabétiques ni numériques (par exemple des espaces, des signes de ponctuation ou des symboles), ces caractères peuvent être encodés dans l’URL quand ils sont récupérés par certains clients.

## <a name="restrictions-when-using-dimensions-in-a-metric-alert-rule-with-multiple-conditions"></a>Restrictions lors de l’utilisation de dimensions dans une règle d’alerte des indicateurs de performance avec plusieurs conditions

Les alertes de métrique plus récentes prennent en charge la génération d’alertes sur des métriques multidimensionnelles, ainsi que la définition de plusieurs conditions (jusqu’à 5 conditions par règle d’alerte).

Tenez compte des contraintes suivantes liées à l’utilisation de dimensions dans une règle d’alerte contenant plusieurs conditions :
- Vous ne pouvez sélectionner qu’une seule valeur par dimension au sein de chaque condition.
- Vous ne pouvez pas utiliser l’option « Sélectionner toutes les valeurs actuelles et futures » (sélectionnez \*).
- Quand des indicateurs de performance configurés dans différentes conditions prennent en charge la même dimension, une valeur de dimension configurée doit être explicitement définie de la même façon pour toutes ces métriques (dans les conditions appropriées).
Par exemple :
    - Prenons l’exemple d’une règle d’alerte des indicateurs de performance qui est définie sur un compte de stockage et qui analyse deux conditions :
        * Nombre total de **Transactions** > 5
        * Moyenne de **SuccessE2ELatency** > 250 ms
    - J’aimerais mettre à jour la première condition et surveiller uniquement les transactions où la dimension **ApiName** est égale à *« GetBlob »*
    - Étant donné que les **Transactions** et les indicateurs de performance **SuccessE2ELatency** prennent en charge une dimension **ApiName**, je dois mettre à jour les deux conditions, et les deux doivent spécifier la dimension **ApiName** avec une valeur *« GetBlob »* .

## <a name="setting-the-alert-rules-period-and-frequency"></a>Définition de la période et de la fréquence de la règle d’alerte

Nous vous recommandons de choisir une *granularité d’agrégation (période)* supérieure à la *fréquence d’évaluation* afin de réduire la probabilité de manquer la première évaluation de la série chronologique ajoutée dans les cas suivants :
-   Règle d’alerte métrique qui surveille plusieurs dimensions : quand une combinaison de valeurs de dimension est ajoutée
-   Règle d’alerte métrique qui surveille plusieurs ressources : quand une ressource est ajoutée à l’étendue
-   Règle d’alerte métrique qui surveille une métrique qui n’est pas émise en continu (métrique éparse) : lorsque la métrique est émise après une période de plus de 24 heures pendant laquelle elle n’a pas été émise

## <a name="the-dynamic-thresholds-borders-dont-seem-to-fit-the-data"></a>Les bordures de seuil dynamique ne semblent pas correspondre aux données

Les modifications apportées au comportement d’une métrique ne sont pas nécessairement reflétées immédiatement dans les bordures de seuil dynamique (limites supérieure et inférieure). Celles-ci sont en effet calculées en fonction des données de métriques des 10 derniers jours. Lorsque vous affichez les bordures de seuil dynamique d’une métrique donnée, veillez à examiner la tendance de la semaine précédente, et non seulement celle des dernières heures ou des derniers jours.

## <a name="why-is-weekly-seasonality-not-detected-by-dynamic-thresholds"></a>Pourquoi la saisonnalité hebdomadaire n’est-elle pas détectée par les seuils dynamiques ?

Pour identifier la saisonnalité hebdomadaire, le modèle de seuils dynamiques a besoin d’au moins trois semaines de données historiques. Dès que ces données sont disponibles, le caractère saisonnier hebdomadaire des données de métriques est identifié, et le modèle ajusté en conséquence. 

## <a name="dynamic-thresholds-shows-a-negative-lower-bound-for-a-metric-even-though-the-metric-always-has-positive-values"></a>Les seuils dynamiques affichent une limite inférieure négative pour une métrique, même si celle-ci présente toujours des valeurs positives

Lorsqu’une métrique présente des fluctuations importantes, les seuils dynamiques génèrent un modèle plus large autour de ses valeurs. Il peut en résulter une bordure inférieure négative, en particulier dans les cas suivants :
1. La sensibilité est définie sur faible. 
2. Les valeurs médianes sont proches de zéro.
3. La métrique présente un comportement irrégulier avec une forte variance (présence de pics ou de creux dans les données).

Lorsque la limite inférieure indique une valeur négative, cela signifie qu’il est plausible que la métrique atteigne une valeur nulle étant donné son comportement irrégulier. Vous pouvez choisir une sensibilité supérieure ou une *plus grande granularité d’agrégation (période)* pour rendre le modèle moins sensible. Vous avez également la possibilité d’utiliser l’option *Ignorer les données avant* pour exclure une irrégularité récente des données historiques utilisées pour générer le modèle.

## <a name="the-dynamic-thresholds-alert-rule-is-too-noisy-fires-too-much"></a>La règle d’alerte de seuils dynamiques est trop bruyante (se déclenche trop)
Pour réduire la sensibilité de votre règle d’alerte de seuils dynamiques, utilisez l’une des options suivantes :
1. Seuil de sensibilité - Réglez la sensibilité sur *Faible* afin d'être plus tolérant aux écarts.
2. Nombre de violations (sous *Paramètres avancés*) : configurez la règle d’alerte pour qu’elle se déclenche uniquement si un certain nombre d’écarts se produisent dans un laps de temps donné. Cela rend la règle moins vulnérable aux écarts temporaires.


## <a name="the-dynamic-thresholds-alert-rule-is-too-insensitive-doesnt-fire"></a>La règle d’alerte de seuils dynamiques n’est pas assez sensible (n’est pas déclenchée)
Parfois, une règle d’alerte ne se déclenche pas même quand une haute sensibilité est configurée. Cela se produit généralement lorsque la distribution de la métrique est très irrégulière.
Envisagez l’une des possibilités suivantes :
* Passez à la surveillance d’une mesure complémentaire adaptée à votre scénario (le cas échéant). Par exemple, recherchez les modifications du taux de réussite, plutôt que le taux d’échec.
* Essayez de sélectionner une granularité d’agrégation différente (période). 
* Vérifiez si le comportement de mesure a été radicalement modifié au cours des 10 derniers jours (une panne). Une modification soudaine peut avoir un impact sur les seuils supérieur et inférieur calculés pour la mesure et les rendre plus larges. Attendez quelques jours jusqu'à ce que la panne ne soit plus prise en compte dans le calcul des seuils, ou utilisez l'option *Ignorer les données avant* (sous *Paramètres avancés*).
* Si vos données comportent un caractère saisonnier hebdomadaire, mais que l’historique n’est pas suffisant pour la métrique, les seuils calculés peuvent entraîner des limites supérieures et inférieures. Par exemple, le calcul peut traiter les jours de semaine et les week-ends de la même façon, et créer des bordures larges qui ne correspondent pas toujours aux données. Cela devrait se résoudre une fois qu’un historique des métriques suffisant est disponible. À partir de là, le caractère saisonnier correct sera détecté et les seuils calculés seront mis à jour en conséquence.


## <a name="next-steps"></a>Étapes suivantes

- Pour obtenir des informations de dépannage générales sur les alertes et les notifications, consultez [Résolution des problèmes relatifs aux alertes Azure Monitor](alerts-troubleshoot.md).
