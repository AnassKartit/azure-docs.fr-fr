---
title: Gérer les coûts Azure avec l’automatisation
description: Cet article explique comment gérer les coûts Azure avec l’automatisation.
author: bandersmsft
ms.author: banders
ms.date: 03/19/2021
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: cost-management
ms.reviewer: adwise
ms.openlocfilehash: 2a39f77e3e7409d23ab7506b525f65e01082e99e
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2021
ms.locfileid: "104720115"
---
# <a name="manage-costs-with-automation"></a>Gérer les coûts avec l’automatisation

Vous pouvez utiliser l’automatisation Cost Management pour créer un ensemble personnalisé de solutions pour récupérer et gérer les données de coût. Cet article présente des scénarios courants pour l’automatisation de Cost Management et les options disponibles en fonction de votre situation. Si vous souhaitez développer à l’aide d’API, des exemples de requêtes d’API communes sont présentés pour accélérer votre processus de développement.

## <a name="automate-cost-data-retrieval-for-offline-analysis"></a>Automatiser l’extraction de données de coût pour l’analyse hors connexion

Vous pouvez avoir besoin de télécharger vos données de coût Azure pour les fusionner avec d’autres jeux de données. Vous pouvez également vouloir intégrer les données de coût à vos propres systèmes. Différentes options sont disponibles en fonction de la quantité de données impliquées. Vous devez systématiquement disposer des autorisations Cost Management au niveau de l’étendue appropriée pour utiliser les API et les outils. Pour plus d’informations, consultez [Affecter une autorisation d’accès aux données](./assign-access-acm-data.md).

## <a name="suggestions-for-handling-large-datasets"></a>Suggestions pour la gestion des jeux de données volumineux

Si votre organisation dispose d’une grande présence Azure sur un grand nombre de ressources ou d’abonnements, vous disposez d’une grande quantité de données contenant des informations sur l’utilisation. Excel ne peut souvent pas charger de tels fichiers volumineux. Dans ce cas, nous vous recommandons d’utiliser les options suivantes :

**Power BI**

Power BI permet d’ingérer et de gérer de grandes quantités de données. Si vous êtes client d’un Contrat Entreprise, vous pouvez utiliser l’application de modèle Power BI pour analyser les coûts associés à votre compte de facturation. Le rapport contient les principales vues utilisées par les clients. Pour plus d’informations, consultez [Analyser les coûts Azure avec l’application de modèle Power BI](./analyze-cost-data-azure-cost-management-power-bi-template-app.md).

**Connecteur de données Power BI**

Si vous souhaitez analyser vos données quotidiennement, nous vous recommandons d’utiliser le [connecteur de données Power BI](/power-bi/connect-data/desktop-connect-azure-cost-management) pour obtenir des données en vue d’une analyse détaillée. Tous les rapports que vous créez sont mis à jour par le connecteur à mesure que des coûts supplémentaires s’accumulent.

**Exportations Cost Management**

Vous n’avez peut-être pas besoin d’analyser les données quotidiennement. Dans ce cas, envisagez d’utiliser la fonctionnalité [Exportations](./tutorial-export-acm-data.md) de Cost Management pour planifier des exportations de données vers un compte de stockage Azure. Vous pouvez ensuite charger les données dans Power BI en fonction des besoins, ou les analyser dans Excel si le fichier est suffisamment petit. Les exportations sont disponibles dans le portail Azure ou vous pouvez configurer des exportations avec l’[API Exportations](/rest/api/cost-management/exports).

**API Détails d’utilisation**

Envisagez d’utiliser l’[API Détails d’utilisation](/rest/api/consumption/usageDetails) si vous avez un petit jeu de données sur les coûts. Si vous disposez d’une grande quantité de données de coût, vous devez demander la plus petite quantité de données d’utilisation possible pour une période donnée. Pour ce faire, spécifiez un intervalle de temps réduit ou utilisez un filtre dans votre demande. Par exemple, dans un scénario où vous avez besoin de trois années de données de coût, l’API est plus performante lorsque vous effectuez plusieurs appels pour différents intervalles de temps plutôt qu’un seul appel. À partir de là, vous pouvez charger les données dans Excel pour une analyse plus poussée.

## <a name="automate-retrieval-with-usage-details-api"></a>Automatiser la récupération avec l’API Détails d’utilisation

L’[API Détails d’utilisation](/rest/api/consumption/usageDetails) offre un moyen simple d’accéder à des données de coût brutes et non agrégées qui correspondent à votre facture Azure. L’API est utile lorsque votre organisation a besoin d’une solution d’extraction de données par programme. Envisagez d’utiliser l’API si vous souhaitez analyser des jeux de données de coût plus petits. Toutefois, vous devez utiliser d’autres solutions identifiées précédemment si vous avez des jeux de données plus volumineux. Les données de l’API Détails d’utilisation sont fournies par compteur et par jour. Elles sont utilisées lors du calcul de votre facture mensuelle. La version en disponibilité générale de l’API est `2019-10-01`. Utilisez `2019-04-01-preview` pour accéder à la préversion de la réservation et aux achats sur la Place de marché Azure avec les API.

Si vous souhaitez obtenir régulièrement de grandes quantités de données exportées, consultez [Récupérer de grands jeux de données de coûts de façon récurrente avec des exportations](ingest-azure-usage-at-scale.md).

### <a name="usage-details-api-suggestions"></a>Suggestions concernant l’API Détails d’utilisation

**Planification des demandes**

Nous vous recommandons d’envoyer _une seule demande par jour_ à l’API Détails d’utilisation. Pour plus d’informations sur la fréquence à laquelle les données de coût sont actualisées et sur la façon dont l’arrondi est géré, consultez [Présentation des données de gestion des coûts](./understand-cost-mgt-data.md).

**Cibler des étendues de niveau supérieur sans filtrage**

Utilisez l’API pour récupérer toutes les données dont vous avez besoin à l’étendue ayant le niveau le plus élevé disponible. Attendez que toutes les données nécessaires soient ingérées avant de procéder à un filtrage, un regroupement ou une analyse agrégée. L’API est spécifiquement optimisée pour fournir de grandes quantités de données de coût brutes et non agrégées. Pour en savoir plus sur les étendues disponibles dans Cost Management, consultez [Comprendre et utiliser des étendues](./understand-work-scopes.md). Une fois que vous avez téléchargé les données nécessaires pour une étendue, utilisez Excel pour analyser les données plus en détail avec les filtres et les tableaux croisés dynamiques.

### <a name="notes-about-pricing"></a>Remarques concernant les prix

Si vous souhaitez rapprocher l’utilisation et les frais avec votre grille tarifaire ou votre facture, notez les informations suivantes.

Comportement des prix de la grille tarifaire : les prix indiqués dans la grille tarifaire sont ceux que vous recevez d’Azure. Ils sont ajustés selon une unité de mesure spécifique. Malheureusement, cette unité de mesure ne reflète pas toujours celle utilisée pour émettre l’utilisation réelle et les frais réels des ressources.

Comportement des prix des détails d’utilisation : les fichiers d’utilisation présentent des informations ajustées qui peuvent ne pas correspondre à celles de la grille tarifaire. Plus précisément :

- Prix unitaire : le prix est ajusté pour refléter l’unité de mesure dans laquelle les frais sont réellement émis par les ressources Azure. En cas d’ajustement, le prix ne correspond pas au prix indiqué dans la grille tarifaire.
- Unité de mesure : représente l’unité de mesure dans laquelle les frais sont réellement émis par les ressources Azure.
- Prix effectif/taux de ressources : le prix correspond au montant réel que vous payez par unité, une fois les remises prises en compte. Il s’agit du prix que vous devez utilisé avec Quantité dans les calculs Prix * Quantité pour rapprocher les frais. Le prix prend en compte les scénarios suivants et le prix unitaire ajusté qui est également présent dans les fichiers. Il peut donc être différent du prix unitaire ajusté.
  - Prix échelonnés : par exemple, 10 US$ pour les 100 premières unités et 8 US$ pour les 100 unités suivantes.
  - Quantité incluse : par exemple, 100 premières unités gratuites, puis 10 US$ par unité.
  - Réservations
  - Arrondi effectué pendant le calcul : l’arrondi prend en compte la quantité consommée, les prix échelonnés et la quantité incluse, ainsi que le prix unitaire ajusté.

### <a name="a-single-resource-might-have-multiple-records-for-a-single-day"></a>Une ressource unique peut avoir plusieurs enregistrements pour une seule journée.

Les fournisseurs de ressources Azure envoient les données d’utilisation et les frais dans le système de facturation et renseignent le champ `Additional Info` des enregistrements d’utilisation. Parfois, les fournisseurs de ressources peuvent envoyer les données d’utilisation pour un jour donné et marquer les enregistrements avec différents centres de données dans le champ `Additional Info` des enregistrements d’utilisation. Cela peut entraîner la présence de plusieurs enregistrements pour un compteur/une ressource dans votre fichier d’utilisation pour un même jour. Dans ce cas, vous n’êtes pas surfacturé. Les enregistrements multiples représentent le coût total du compteur pour la ressource ce jour.

## <a name="example-usage-details-api-requests"></a>Exemples de demandes envoyées à l’API Détails d’utilisation

Les exemples de demande suivants sont utilisés par les clients Microsoft pour résoudre les scénarios courants que vous pouvez rencontrer.

### <a name="get-usage-details-for-a-scope-during-specific-date-range"></a>Obtenir les détails d’utilisation d’une étendue pendant une plage de dates spécifique

Les données retournées par la requête correspondent à la date à laquelle l’utilisation a été reçue par le système de facturation. Elles peuvent inclure des coûts issus de plusieurs factures. L’appel à utiliser varie en fonction de votre type d’abonnement.

Pour les clients hérités disposant d’un Accord Entreprise (EA) ou d’un abonnement avec paiement à l’utilisation, utilisez l’appel suivant :

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?$filter=properties%2FusageStart%20ge%20'2020-02-01'%20and%20properties%2FusageEnd%20le%20'2020-02-29'&$top=1000&api-version=2019-10-01
```

Pour les clients modernes disposant d’un Contrat client Microsoft, utilisez l’appel suivant :

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?startDate=2020-08-01&endDate=&2020-08-05$top=1000&api-version=2019-10-01
```

### <a name="get-amortized-cost-details"></a>Recevoir des détails sur les coûts amortis

Si vous avez besoin de coûts réels pour afficher les achats au fur et à mesure qu’ils sont cumulés, remplacez la *métrique* par `ActualCost` dans la requête suivante. Pour utiliser les coûts amortis et réels, vous devez utiliser la version `2019-04-01-preview`. La version actuelle de l’API fonctionne de la même façon que la version `2019-10-01`, à l’exception du nouvel attribut type/metric et des noms de propriété modifiés. Si vous avez un Contrat client Microsoft, vos filtres sont `startDate` et `endDate` dans l’exemple suivant.

```http
GET https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?metric=AmortizedCost&$filter=properties/usageStart+ge+'2019-04-01'+AND+properties/usageEnd+le+'2019-04-30'&api-version=2019-04-01-preview
```

## <a name="automate-alerts-and-actions-with-budgets"></a>Automatiser les alertes et les actions à l’aide de budgets

Deux composants essentiels interviennent dans l’optimisation de la valeur de votre investissement dans le cloud. L’un d’eux est la création d’un budget automatique. L’autre consiste à configurer l’orchestration basée sur les coûts, en réponse aux alertes budgétaires. Il existe différentes façons d’automatiser la création d’un budget Azure. Diverses réponses d’alerte se produisent lorsque les seuils d’alerte configurés sont dépassés.

Les sections suivantes traitent des options disponibles et fournissent des exemples de requêtes d’API pour vous aider à vous familiariser avec l’automatisation de budget.

### <a name="how-costs-are-evaluated-against-your-budget-threshold"></a>Évaluation des coûts par rapport à votre seuil budgétaire

Vos coûts sont évalués par rapport à votre seuil budgétaire une fois par jour. Lorsque vous créez un budget ou à la date de réinitialisation de votre budget, les coûts comparés au seuil sont zéro/nuls, car l’évaluation n’a peut-être pas été effectuée.

Quand Azure détecte que vos coûts dépassent le seuil, une notification est déclenchée dans l’heure suivant la détection.

### <a name="view-your-current-cost"></a>Afficher votre coût actuel

Pour afficher vos coûts actuels, vous devez effectuer un appel GET au moyen de l’[API de requête](/rest/api/cost-management/query).

Un appel GET à l’API Budgets ne retourne pas les coûts actuels affichés dans l’analyse des coûts. Au lieu de cela, l’appel retourne votre dernier coût évalué.

### <a name="automate-budget-creation"></a>Automatiser la création d’un budget

Vous pouvez automatiser la création d’un budget au moyen de l’API [Budgets](/rest/api/consumption/budgets). Vous pouvez également créer un budget avec un [modèle de budget](quick-create-budget-template.md). Les modèles constituent un moyen simple de standardiser les déploiements Azure tout en veillant à ce que le contrôle des coûts soit correctement configuré et appliqué.

### <a name="supported-locales-for-budget-alert-emails"></a>Paramètres régionaux pris en charge pour les e-mails d’alerte de budget

Avec Budgets, vous êtes alerté lorsque les coûts franchissent un seuil défini. Vous pouvez définir jusqu’à cinq destinataires par budget. Les destinataires reçoivent des alertes par e-mail dans les 24 heures suivant le dépassement du seuil budgétaire. Toutefois, votre destinataire peut avoir besoin de recevoir un e-mail dans une autre langue. Vous pouvez utiliser les codes de culture de langue suivants avec l’API Budgets. Définissez le code de culture avec le paramètre `locale` similaire à l’exemple suivant.

```json
{
  "eTag": "\"1d681a8fc67f77a\"",
  "properties": {
    "timePeriod": {
      "startDate": "2020-07-24T00:00:00Z",
      "endDate": "2022-07-23T00:00:00Z"
    },
    "timeGrain": "BillingMonth",
    "amount": 1,
    "currentSpend": {
      "amount": 0,
      "unit": "USD"
    },
    "category": "Cost",
    "notifications": {
      "actual_GreaterThan_10_Percent": {
        "enabled": true,
        "operator": "GreaterThan",
        "threshold": 20,
        "locale": "en-us",
        "contactEmails": [
          "user@contoso.com"
        ],
        "contactRoles": [],
        "contactGroups": [],
        "thresholdType": "Actual"
      }
    }
  }
}

```

Langues prises en charge par un code de culture :

| Code de culture| Langage |
| --- | --- |
| fr-FR | Anglais (États-Unis) |
| ja-jp | Japonais (Japon) |
| zh-cn | Chinois (simplifié, Chine) |
| de-de | Allemand (Allemagne) |
| es-es | Espagnol (Espagne, International) |
| fr-fr | Français (France) |
| it-it | Italien (Italie) |
| ko-kr | Coréen (Corée) |
| pt-br | Portugais (Brésil) |
| ru-ru | Russe (Russie) |
| zh-tw | Chinois (traditionnel, Taïwan) |
| cs-cz | Tchèque (République tchèque) |
| pl-pl | Polonais (Pologne) |
| tr-tr | Turc (Turquie) |
| da-dk | Danois (Danemark) |
| en-gb | Anglais (Royaume-Uni) |
| hu-hu | Hongrois (Hongrie) |
| nb-no | Norvégien Bokmal (Norvège) |
| nl-nl | Néerlandais (Pays-Bas) |
| pt-pt | Portugais (Portugal) |
| sv-se | Suédois (Suède) |

### <a name="common-budgets-api-configurations"></a>Configurations courantes de l’API Budgets

Il existe de nombreuses manières de configurer un budget dans votre environnement Azure. Examinez d’abord votre scénario, puis identifiez les options de configuration qui l’activent. Passez en revue les options suivantes :

- **Fragment de temps**  : représente la période récurrente utilisée par votre budget pour comptabiliser et évaluer les coûts. Les options les plus courantes sont Mensuel, Trimestriel et Annuel.
- **Durée** : représente la durée de validité de votre budget. Le budget supervise activement et vous alerte uniquement tant qu’il est en cours de validité.
- **Notifications**
  - E-mails de contact : les adresses e-mail reçoivent des alertes lorsqu’un budget comptabilise des coûts et dépasse les seuils définis.
  - Rôles de contact : tous les utilisateurs qui détiennent un rôle Azure correspondant sur l’étendue donnée reçoivent des alertes par e-mail avec cette option. Par exemple, les propriétaires d’abonnements peuvent recevoir une alerte pour un budget créé dans l’étendue de l’abonnement.
  - Groupes de contacts : le budget appelle les groupes d’actions configurés lorsqu’un seuil d’alerte est dépassé.
- **Filtres de dimension de coût** : le filtrage que vous pouvez effectuer dans l’analyse des coûts ou l’API de requête peut également être effectué sur votre budget. Utilisez ce filtre pour réduire la plage des coûts que vous supervisez avec le budget.

Après avoir identifié les options de création de budget qui répondent à vos besoins, créez le budget à l’aide de l’API. L’exemple ci-dessous vous aide à vous familiariser avec une configuration de budget courante.

**Créer un budget filtré sur plusieurs ressources et étiquettes**

URL de requête : `PUT https://management.azure.com/subscriptions/{SubscriptionId} /providers/Microsoft.Consumption/budgets/{BudgetName}/?api-version=2019-10-01`

```json
{
  "eTag": "\"1d34d016a593709\"",
  "properties": {
    "category": "Cost",
    "amount": 100.65,
    "timeGrain": "Monthly",
    "timePeriod": {
      "startDate": "2017-10-01T00:00:00Z",
      "endDate": "2018-10-31T00:00:00Z"
    },
    "filter": {
      "and": [
        {
          "dimensions": {
            "name": "ResourceId",
            "operator": "In",
            "values": [
              "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{meterName}",
              "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{meterName}"
            ]
          }
        },
        {
          "tags": {
            "name": "category",
            "operator": "In",
            "values": [
              "Dev",
              "Prod"
            ]
          }
        },
        {
          "tags": {
            "name": "department",
            "operator": "In",
            "values": [
              "engineering",
              "sales"
            ]
          }
        }
      ]
    },
    "notifications": {
      "Actual_GreaterThan_80_Percent": {
        "enabled": true,
        "operator": "GreaterThan",
        "threshold": 80,
        "contactEmails": [
          "user1@contoso.com",
          "user2@contoso.com"
        ],
        "contactRoles": [
          "Contributor",
          "Reader"
        ],
        "contactGroups": [
          "/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/microsoft.insights/actionGroups/{actionGroupName}
        ],
        "thresholdType": "Actual"
      }
    }
  }
}
```

### <a name="configure-cost-based-orchestration-for-budget-alerts"></a>Configurer une orchestration basée sur les coûts pour les alertes budgétaires

Vous pouvez configurer des budgets pour démarrer des actions automatisées à l’aide des groupes d’actions Azure. Pour en savoir plus sur l’automatisation d’actions à l’aide de budgets, consultez [Automatisation avec les budgets Azure](../manage/cost-management-budget-scenario.md).

## <a name="data-latency-and-rate-limits"></a>Latence des données et limites du taux de transfert

Nous vous recommandons d’appeler les API au maximum une fois par jour. Les données Cost Management sont actualisées toutes les quatre heures au fur et à mesure que de nouvelles données d’utilisation sont reçues de la part des fournisseurs de ressources Azure. Des appels plus fréquents ne produisent pas de données supplémentaires. Au lieu de cela, la charge augmente. Pour en savoir plus sur la fréquence de modification des données et sur la façon dont la latence des données est gérée, consultez [Présentation des données de gestion des coûts](understand-cost-mgt-data.md).

### <a name="error-code-429---call-count-has-exceeded-rate-limits"></a>Code d’erreur 429 - Le nombre d’appels a dépassé les limites de débit

Pour garantir une expérience cohérente pour tous les abonnés Cost Management, les API Cost Management ont un débit limité. Lorsque vous atteignez la limite, vous recevez le code d’état HTTP `429: Too many requests`. Les limites de débit actuelles pour nos API sont les suivantes :

- 30 appels par minute : cette opération est effectuée par étendue, par utilisateur ou par application.
- 200 appels par minute : cette opération est effectuée par locataire, par utilisateur ou par application.

## <a name="next-steps"></a>Étapes suivantes

- [Analyser les coûts Azure avec l’application de modèle Power BI](./analyze-cost-data-azure-cost-management-power-bi-template-app.md)
- [Créer et gérer des données exportées](./tutorial-export-acm-data.md) avec la fonctionnalité Exportations
- En savoir plus sur l’[API Détails d’utilisation](/rest/api/consumption/usageDetails)
