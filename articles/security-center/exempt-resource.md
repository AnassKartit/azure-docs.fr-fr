---
title: Exempter une recommandation Azure Security Center d’une ressource, d’un abonnement, d’un groupe d’administration et d’un score sécurisé
description: Découvrez comment créer des règles pour exempter les recommandations de sécurité des abonnements ou des groupes d’administration et les empêcher d’avoir un impact sur votre score sécurisé
author: memildin
ms.author: memildin
ms.date: 03/11/2021
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 13abb35d0fa9ad3ee949b6edf5205de601a02956
ms.sourcegitcommit: 79c9c95e8a267abc677c8f3272cb9d7f9673a3d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/19/2021
ms.locfileid: "107718554"
---
# <a name="exempting-resources-and-recommendations-from-your-secure-score"></a>Exempter une ressource des recommandations et du degré de sécurisation 

Une priorité de base de chaque équipe de sécurité s’assure que les analystes peuvent se concentrer sur les tâches et les incidents importants pour l’entreprise. Security Center dispose de nombreuses fonctionnalités permettant de personnaliser l’expérience et de s’assurer que votre score sécurisé reflète les priorités de sécurité de votre organisation. L’option **exempt** est une fonctionnalité de ce type.

Quand vous examinez vos recommandations de sécurité dans Azure Security Center, l’une des premières informations que vous examinez est la liste des ressources affectées.

Parfois, une ressource sera répertoriée et ne devrait pas être incluse. D’autres fois, il arrive qu’une recommandation s’affiche dans une étendue à laquelle elle n’est pas censée appartenir. La ressource a peut-être été corrigée par un processus non suivi par Security Center. La recommandation peut ne pas convenir à un abonnement. Ou encore, il est possible que votre organisation ait simplement décidé d’accepter les risques liés à une ressource ou à une recommandation.

Dans ce cas, vous pouvez créer une exemption pour une recommandation pour :

- **Exempter une ressource** pour qu’elle ne soit plus listée parmi les ressources non saines et n’affecte pas votre degré de sécurisation. La ressource sera listée comme non applicable, avec le motif « exemptée » et la justification de votre choix.

- **Exempter un abonnement ou un groupe d’administration** pour que la recommandation n’impacte pas votre degré de sécurisation et ne s’affiche plus pour l’abonnement ou le groupe d’administration. Cela concerne les ressources existantes et futures. La recommandation sera signalée avec la justification que vous avez choisie pour l’étendue sélectionnée.

## <a name="availability"></a>Disponibilité

| Aspect                          | Détails                                                                                                                                                                                                                                                                                                                            |
|---------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| État de sortie :                  | PRÉVERSION<br>[!INCLUDE [Legalese](../../includes/security-center-preview-legal-text.md)]                                                                                                                                                                                                                                             |
| Prix :                        | Il s’agit d’une capacité premium d’Azure Policy proposée sans surcoût aux clients d’Azure Defender. Pour les autres utilisateurs, des frais pourront s’appliquer.                                                                                                                                                                 |
| Rôles et autorisations obligatoires : | **Propriétaire** ou **Contributeur de stratégie de ressource** pour créer une exemption<br>Pour créer une règle, vous devez disposer de l’autorisation de modifier des stratégies dans Azure Policy.<br>Pour plus d’informations, consultez [Autorisations Azure RBAC dans Azure Policy](../governance/policy/overview.md#azure-rbac-permissions-in-azure-policy).                                            |
| Limites :                    | Les exemptions ne peuvent être créées que pour les recommandations incluses dans l’initiative par défaut de Security Center, [Azure Security Benchmark](https://docs.microsoft.com/security/benchmark/azure/introduction), ou l’une des initiatives de normes réglementaires fournies. Les recommandations générées à partir d’initiatives personnalisées ne peuvent pas être exemptées. En savoir plus sur les relations entre les [stratégies, les initiatives et les recommandations](security-policy-concept.md). |
| Clouds :                         | ![Oui](./media/icons/yes-icon.png) Clouds commerciaux<br>![Non](./media/icons/no-icon.png) National/souverain (US Gov, Chine Gov, autres Gov)                                                                                                                                                                                         |
|                                 |                                                                                                                                                                                                                                                                                                                                    |

## <a name="define-an-exemption"></a>Définir une exemption

Pour affiner les recommandations de sécurité que Security Center fournit concernant vos abonnements, vos groupes d’administration ou vos ressources, vous pouvez créer une règle d’exemption afin de :

- Marquer une **recommandation** spécifique ou en tant que « atténué » ou « risque accepté ». Vous pouvez créer des exemptions de recommandation pour un abonnement, plusieurs abonnements ou un groupe d’administration entier.
- Marquer **une ou plusieurs ressources** comme « atténué » ou « risque accepté » pour une recommandation spécifique.

> [!NOTE]
> Les exemptions ne peuvent être créées que pour les recommandations incluses dans l’initiative par défaut de Security Center, Azure Security Benchmark, ou l’une des initiatives de normes réglementaires fournies. Les recommandations générées à partir d’initiatives personnalisées assignées à vos abonnements ne peuvent pas être exemptées. En savoir plus sur les relations entre les [stratégies, les initiatives et les recommandations](security-policy-concept.md).

> [!TIP]
> Vous pouvez également créer des exemptions à l’aide de l’API. Pour obtenir un exemple de code JSON et une explication des structures pertinentes, consultez [Structure d’exemption Azure Policy](../governance/policy/concepts/exemption-structure.md).

Pour créer une règle d’exemption :

1. Ouvrez la page de détails des recommandations pour obtenir une recommandation spécifique.

1. Dans la barre d’outils en haut de la page, sélectionnez **Exemption**.

    :::image type="content" source="media/exempt-resource/exempting-recommendation.png" alt-text="Créez une règle d’exemption pour qu’une recommandation soit exemptée d’un abonnement ou d’un groupe d’administration.":::

1. Dans le volet **Exemption** :
    1. Sélectionnez l’étendue de cette règle d’exemption :
        - Si vous sélectionnez un groupe d’administration, la recommandation est exemptée de tous les abonnements au sein de ce groupe
        - Si vous créez cette règle pour exempter une ou plusieurs ressources de la recommandation, choisissez « Ressources sélectionnées » et sélectionnez les ressources appropriées dans la liste.

    1. Entrez un nom pour cette règle d’exemption.
    1. Éventuellement, définissez une date d’expiration.
    1. Sélectionnez la catégorie de l’exemption :
        - **Résolu par le biais du tiers (atténué)**  : si vous utilisez un service tiers que Security Center n’a pas identifié. 

            > [!NOTE]
            > Lorsque vous exemptez une recommandation comme étant atténuée, vous ne recevez pas de points pour votre score de sécurité. Toutefois, étant donné que les points ne sont pas *supprimés* pour les ressources défectueuses, le résultat est que votre score augmente.

        - **Risque accepté (renonciation)**  : si vous avez décidé d’accepter le risque de ne pas atténuer cette recommandation
    1. Si vous le souhaitez, entrez une description.
    1. Sélectionnez **Create** (Créer).

    :::image type="content" source="media/exempt-resource/defining-recommendation-exemption.png" alt-text="Étapes de création d’une règle d’exemption pour exempter une recommandation de votre abonnement ou groupe d’administration":::

    Lorsque l’exemption prend effet (cela peut prendre jusqu’à 30 minutes) :
    - La recommandation ou les ressources n’ont pas d’impact sur votre score sécurisé.
    - Si vous avez exempté des ressources spécifiques, celles-ci sont listées dans l’onglet **Non applicable** de la page Détails de la recommandation.
    - Si vous avez exempté une recommandation, elle est masquée par défaut sur la page Recommandations de Security Center. Cela est dû au fait que les options par défaut du filtre de l’**état Recommandation** de cette page doivent exclure les recommandations **Non applicable**. Il en va de même si vous n’avez pas exempté toutes les recommandations dans un contrôle de sécurité.

        :::image type="content" source="media/exempt-resource/recommendations-filters-hiding-not-applicable.png" alt-text="Filtres par défaut sur la page recommandations de Azure Security Center masque les recommandations et les contrôles de sécurité non applicables":::

    - La bande d’informations en haut de la page des détails de la recommandation met à jour le nombre de ressources exemptées :
        
        :::image type="content" source="./media/exempt-resource/info-banner.png" alt-text="Nombre de ressources exemptées":::

1. Pour passer en revue vos ressources exemptées, ouvrez l’onglet **Non applicable** :

    :::image type="content" source="./media/exempt-resource/modifying-exemption.png" alt-text="Modification d’une exemption":::

    La raison de chaque exemption est spécifiée dans le tableau (1).

    Pour modifier ou supprimer une exemption, sélectionnez le menu de points de suspension (« ... ») comme indiqué (2).

1. Pour passer en revue toutes les règles d’exemption de votre abonnement, sélectionnez **Afficher les exemptions** à partir de la bande d’informations :

    > [!IMPORTANT]
    > Pour voir les exemptions spécifiques relatives à une recommandation, filtrez la liste en fonction de l’étendue et du nom de la recommandation appropriés.

    :::image type="content" source="./media/exempt-resource/policy-page-exemption.png" alt-text="Page d’exemption d’Azure Policy":::

    > [!TIP]
    > Vous pouvez également [utiliser Azure Resource Graph pour rechercher des recommandations avec des exemptions](#find-recommendations-with-exemptions-using-azure-resource-graph).

## <a name="monitor-exemptions-created-in-your-subscriptions"></a>Surveiller les exemptions créées dans vos abonnements

Comme expliqué plus haut sur cette page, les règles d'exemption constituent un outil puissant permettant un contrôle précis des recommandations affectant les ressources de vos abonnements et groupes d’administration. 

Pour garder une trace de la façon dont vos utilisateurs exercent cette fonctionnalité, nous avons créé un modèle Azure Resource Manager (ARM) qui déploie un guide opérationnel d'application logique et toutes les connexions API nécessaires pour vous avertir lorsqu'une exemption a été créée.

- Pour en savoir plus sur le playbook, consultez le billet de blog de la communauté technique, intitulé [How to keep track of Resource Exemptions in Azure Security Center](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-keep-track-of-resource-exemptions-in-azure-security/ba-p/1770580) (Suivi des exemptions de ressources dans Azure Security Center)
- Vous trouverez le modèle ARM dans le [référentiel GitHub Azure Security Center](https://github.com/Azure/Azure-Security-Center/tree/master/Workflow%20automation/Notify-ResourceExemption)
- Pour déployer tous les composants nécessaires, [utilisez ce processus automatisé](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FAzure-Security-Center%2Fmaster%2FWorkflow%2520automation%2FNotify-ResourceExemption%2Fazuredeploy.json)


## <a name="find-recommendations-with-exemptions-using-azure-resource-graph"></a>Rechercher des recommandations avec des exemptions à l’aide d’Azure Resource Graph

Azure Resource Graph (ARG) fournit un accès instantané aux informations relatives aux ressources de vos environnements cloud avec des fonctionnalités robustes de filtrage, de regroupement et de tri. Il s’agit d’un moyen rapide et efficace de demander des informations dans les abonnements Azure par programmation ou depuis le Portail Azure.

Pour afficher toutes les recommandations qui ont des règles d’exemption :

1. Ouvrez l’**Explorateur Azure Resource Graph**.

    :::image type="content" source="./media/security-center-identity-access/opening-resource-graph-explorer.png" alt-text="Lancement de la page de recommandations de l’Explorateur Azure Resource Graph**" :::

1. Entrez la requête suivante et sélectionnez **Exécuter la requête**.

    ```kusto
    securityresources
    | where type == "microsoft.security/assessments"
    // Get recommendations in useful format
    | project
    ['TenantID'] = tenantId,
    ['SubscriptionID'] = subscriptionId,
    ['AssessmentID'] = name,
    ['DisplayName'] = properties.displayName,
    ['ResourceType'] = tolower(split(properties.resourceDetails.Id,"/").[7]),
    ['ResourceName'] = tolower(split(properties.resourceDetails.Id,"/").[8]),
    ['ResourceGroup'] = resourceGroup,
    ['ContainsNestedRecom'] = tostring(properties.additionalData.subAssessmentsLink),
    ['StatusCode'] = properties.status.code,
    ['StatusDescription'] = properties.status.description,
    ['PolicyDefID'] = properties.metadata.policyDefinitionId,
    ['Description'] = properties.metadata.description,
    ['RecomType'] = properties.metadata.assessmentType,
    ['Remediation'] = properties.metadata.remediationDescription,
    ['Severity'] = properties.metadata.severity,
    ['Link'] = properties.links.azurePortal
    | where StatusDescription contains "Exempt"    
    ```


Pour en savoir plus, consultez les pages suivantes :
- [Apprenez-en davantage sur Azure Resource Graph](../governance/resource-graph/index.yml).
- [Procédure pour créer des requêtes avec l’Explorateur Azure Resource Graph](../governance/resource-graph/first-query-portal.md)
- [Langage de requête Kusto (KQL)](/azure/data-explorer/kusto/query/)





## <a name="faq---exemption-rules"></a>FAQ - Règles d’exemption

### <a name="what-happens-when-one-recommendation-is-in-multiple-policy-initiatives"></a>Que se passe-t-il quand une recommandation figure dans plusieurs initiatives de stratégie ?

Parfois, une recommandation de sécurité apparaît dans plusieurs initiatives de stratégie. Si vous disposez de plusieurs instances de la même recommandation affectées au même abonnement et que vous créez une exemption pour la recommandation, cela affecte toutes les initiatives que vous êtes autorisé à modifier. 

Par exemple, la recommandation **** fait partie de l’initiative de stratégie par défaut affectée à tous les abonnements Azure par Azure Security Center. Elle figure également dans XXXXX.

Si vous essayez de créer une exemption pour cette recommandation, vous verrez l’un des deux messages suivants :

- Si vous disposez des autorisations nécessaires pour modifier les deux initiatives, vous verrez :

    *Cette recommandation est incluse dans plusieurs initiatives de stratégie : [noms d’initiative séparés par une virgule]. Des exemptions seront créées sur chacune d’elles.*  

- Si vous ne disposez pas des autorisations suffisantes pour les deux initiatives, vous verrez ce message à la place :

    *Vous avez des autorisations limitées pour appliquer l’exemption à toutes les initiatives de stratégie, les exemptions sont créées uniquement sur les initiatives avec des autorisations suffisantes.*


## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous avez appris à exempter une ressource d’une recommandation afin qu’elle n’affecte pas votre degré de sécurisation. Pour plus d’informations sur le degré de sécurisation, voir :

- [Degré de sécurisation dans Azure Security Center](secure-score-security-controls.md)
