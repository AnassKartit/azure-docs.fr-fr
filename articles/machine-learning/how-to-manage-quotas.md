---
title: Gérer les ressources et les quotas
titleSuffix: Azure Machine Learning
description: Obtenez plus d’informations relatives aux quotas et aux limites sur les ressources pour Azure Machine Learning et sur la manière de demander des augmentations de quota.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: SimranArora904
ms.author: siarora
ms.date: 06/14/2021
ms.topic: how-to
ms.custom: troubleshooting,contperf-fy20q4, contperf-fy21q2
ms.openlocfilehash: eddf1f5a77ef67de3ab6e1861ae44a19b07d8027
ms.sourcegitcommit: f29615c9b16e46f5c7fdcd498c7f1b22f626c985
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2021
ms.locfileid: "129425977"
---
# <a name="manage-and-increase-quotas-for-resources-with-azure-machine-learning"></a>Gérer et augmenter les quotas pour les ressources avec Azure Machine Learning

Azure utilise des limites et des quotas pour empêcher les dépassements de budget dus à des fraudes et pour respecter les contraintes de capacité d’Azure. Tenez compte de ces limites lors de la mise à l’échelle des charges de travail de production. Cet article porte sur les points suivants :

> [!div class="checklist"]
> + Limites par défaut des ressources Azure relatives à [Azure Machine Learning](overview-what-is-azure-machine-learning.md).
> + Création de quotas au niveau de l’espace de travail.
> + Consultation de vos quotas et limites.
> + Demande d’augmentations de quota.

En plus de la gestion des quotas, vous pouvez également apprendre à [planifier et gérer les coûts pour Azure Machine Learning](concept-plan-manage-cost.md) ou découvrir les [limites de service dans Azure Machine Learning](resource-limits-quotas-capacity.md).

## <a name="special-considerations"></a>Considérations spéciales

+ Un quota est une limite de crédit, pas une garantie de capacité. Si vous avez des besoins de capacité à grande échelle, [contactez le support Azure pour augmenter votre quota](#request-quota-increases).

+ Un quota est partagé entre tous les services de vos abonnements, y compris Azure Machine Learning. Calculez l’utilisation sur tous les services lorsque vous évaluez la capacité.
 
  Le calcul Azure Machine Learning est une exception. Il dispose d’un quota distinct du quota de calcul principal. 

+ Les limites par défaut varient selon le type de catégorie d’offre, comme Essai gratuit ou Paiement à l’utilisation, et selon la gamme de machine virtuelle (Dv2, F et G).

## <a name="default-resource-quotas"></a>Quotas de ressources par défaut

Cette section porte sur les limites de quota par défaut et maximale pour les ressources suivantes :

+ Ressources Azure Machine Learning
  + Capacité de calcul Azure Machine Learning
  + Pipelines Azure Machine Learning
+ Machines virtuelles
+ Azure Container Instances
+ Stockage Azure

> [!IMPORTANT]
> Les limites sont susceptibles d’être modifiées. Pour obtenir les informations les plus récentes, consultez [Limites de service dans Azure Machine Learning](resource-limits-quotas-capacity.md).



### <a name="azure-machine-learning-assets"></a>Ressources Azure Machine Learning
Les limites suivantes s’appliquent aux ressources pour chaque espace de travail. 

| **Ressource** | **Limite maximale** |
| --- | --- |
| Groupes de données | 10 millions |
| Exécutions | 10 millions |
| Modèles | 10 millions|
| Artifacts | 10 millions |

En outre, la **durée d’exécution** maximale est de 30 jours, et le nombre de **métriques journalisées par exécution** maximal est de 1 million.

### <a name="azure-machine-learning-compute"></a>Capacité de calcul Azure Machine Learning
La [Capacité de calcul Azure Machine Learning](concept-compute-target.md#azure-machine-learning-compute-managed) a une limite de quota par défaut sur le nombre de cœurs (par famille de machines virtuelles et en nombre total cumulé de cœurs) et le nombre de ressources de calcul autorisées par région dans un abonnement. Ce quota est distinct du quota de cœurs de machines virtuelles mentionné dans la section précédente, car il s’applique uniquement aux ressources de calcul managées d’Azure Machine Learning.

[Demandez une augmentation du quota](#request-quota-increases) pour augmenter les limites des différents quotas de cœurs par famille de machines virtuelles, total des quotas de cœurs de l’abonnement et ressources dans cette section.

Ressources disponibles :
+ Les **cœurs dédiés par région** ont une limite par défaut comprise entre 24 et 300 ressources en fonction du type de votre offre d’abonnement. Vous pouvez augmenter le nombre de cœurs dédiés par abonnement pour chaque famille de machines virtuelles. Les familles de machines virtuelles spécialisées comme NCv2, NCv3 ou ND ont une valeur initiale par défaut de zéro cœur.

+ Les **cœurs de faible priorité par région** ont une limite par défaut comprise entre 100 et 3 000 ressources en fonction du type de votre offre d’abonnement. Le nombre de cœurs basse priorité par abonnement peut être augmenté et représente une valeur unique entre les familles de machines virtuelles.

+ Les **clusters par région** ont une limite par défaut de 200. Elles sont partagées entre un cluster de formation et une instance de calcul. (Une instance de calcul est considérée comme un cluster à nœud unique à des fins de quota.)

> [!TIP]
> Pour en savoir plus sur la famille de machines virtuelles pour laquelle demander une augmentation de quota, consultez [Tailles de machines virtuelles dans Azure](../virtual-machines/sizes.md). Par exemple, le nom des familles de machines virtuelles GPU commence par un « N » (exemple : série NCv3).

Le tableau suivant présente des limites supplémentaires dans la plateforme. Pour demander une exception, contactez l’équipe produit AzureML par le biais d’un ticket de support **technique**.

| **Ressource ou action** | **Limite maximale** |
| --- | --- |
| Espaces de travail par groupe de ressources | 800 |
| Nœuds d’un **cluster** de Capacité de calcul Azure Machine Learning (AmlCompute) unique configurés en tant que pool qui n’est pas compatible avec la communication (c’est-à-dire qui ne peut pas exécuter de travaux MPI) | 100 nœuds, mais configurable jusqu’à 65 000 nœuds |
| Nœuds dans une **exécution** d’étape d’exécution parallèle unique sur un cluster de Capacité de calcul Azure Machine Learning (AmlCompute) | 100 nœuds, mais configurable jusqu’à 65 000 nœuds si votre cluster est configuré pour une mise à l’échelle conformément aux indications ci-dessus |
| Nœuds d’un **cluster** de Capacité de calcul Azure Machine Learning (AmlCompute) unique configurés en tant que pool compatible avec la communication | 300 nœuds, mais configurable jusqu’à 4000 nœuds |
| Nœuds d’un **cluster** de Capacité de calcul Azure Machine Learning (AmlCompute) unique configurés en tant que pool compatible avec la communication sur une famille de machines virtuelles compatible RDMA | 100 nœuds |
| Nœuds dans une **exécution** MPI unique sur un cluster de Capacité de calcul Azure Machine Learning (AmlCompute) | 100 nœuds, mais peut être augmenté jusqu’à 300 nœuds |
| Processus MPI GPU par nœud | 1-4 |
| Workers GPU par nœud | 1-4 |
| Durée de vie du travail | 21 jours<sup>1</sup> |
| Durée de vie du travail sur un nœud basse priorité | 7 jours<sup>2</sup> |
| Serveurs de paramètres par nœud | 1 |

<sup>1</sup> La durée de vie maximale est la durée entre le début et la fin d’une exécution. Les exécutions terminées sont conservées indéfiniment. Les données des exécutions non terminées pendant la durée de vie maximale ne sont pas accessibles.
<sup>2</sup> Les travaux sur un nœud basse priorité doivent être vidés au préalable chaque fois qu’il existe une contrainte de capacité. Nous vous recommandons d’implémenter des points de contrôle dans votre travail.

### <a name="azure-machine-learning-managed-online-endpoints-preview"></a>Points de terminaison en ligne managés Azure Machine Learning (préversion)
[!INCLUDE [preview disclaimer](../../includes/machine-learning-preview-generic-disclaimer.md)]

Les points de terminaison en ligne managés Azure Machine Learning présentent les limites suivantes.

| **Ressource** | **Limite** |
| --- | --- |
| Nom du point de terminaison| Les noms des points de terminaison doivent <li> Commencer par une lettre <li> Comporter entre 3 et 32 caractères  <li> Comprendre uniquement des lettres et des chiffres <sup>1</sup> |
| Nom du déploiement| Les noms des déploiements doivent <li> Commencer par une lettre <li> Comporter entre 3 et 32 caractères  <li>  Comprendre uniquement des lettres et des chiffres <sup>1</sup> |
| Nombre de points de terminaison par abonnement | 50 |
| Nombre de déploiements par abonnement | 200 |
| Nombre de déploiements par point de terminaison | 20 |
| Nombre d’instances par déploiement | 20 |
| Taille maximale de la charge utile au niveau du point de terminaison |1,5 Mo |
| Délai d’expiration maximal des demandes au niveau du point de terminaison  | 60 secondes |
| Nombre total de requêtes par seconde (RPS) au niveau du point de terminaison pour tous les déploiements  | 100 |

<sup>1</sup> Les tirets simples, comme `my-endpoint-name`, sont acceptés dans les noms des points de terminaison et des déploiements

#### <a name="azure-machine-learning-pipelines"></a>Pipelines Azure Machine Learning
Les [pipelines Azure Machine Learning](concept-ml-pipelines.md) ont les limites suivantes.

| **Ressource** | **Limite** |
| --- | --- |
| Étapes d’un pipeline | 30,000 |
| Espaces de travail par groupe de ressources | 800 |

### <a name="virtual-machines"></a>Machines virtuelles
Chaque abonnement Azure est limité quant au nombre de machines virtuelles entre tous les services. Une limite totale régionale et une limite régionale par gamme de taille s’appliquent aux cœurs de machine virtuelle. Les deux limites sont appliquées séparément.

Par exemple, considérons un abonnement dont le nombre total limite de cœurs de machine virtuelle est de 30 pour la région USA Est, de 30 pour la gamme A et de 30 pour la gamme D. Cet abonnement serait autorisé à déployer 30 machines virtuelles A1, ou 30 machines virtuelles D1, ou encore une combinaison de ces deux types de machines dans la limite de 30 cœurs au total.

Vous ne pouvez pas augmenter les limites des machines virtuelles au-delà des valeurs indiquées dans le tableau suivant.

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="container-instances"></a>Container Instances

Pour plus d’informations, consultez [Limites de Container Instances](../azure-resource-manager/management/azure-subscription-service-limits.md#container-instances-limits).

### <a name="storage"></a>Stockage
Stockage Azure a une limite de 250 comptes de stockage par région et par abonnement. Cette limite comprend à la fois les comptes de stockage Standard et Premium.

Pour augmenter la limite, effectuez une demande via le [support Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/). L’équipe Stockage Azure examinera votre cas et peut approuver jusqu’à 250 comptes de stockage pour une région.


## <a name="workspace-level-quotas"></a>Quotas au niveau de l’espace de travail

Utilisez des quotas au niveau de l’espace de travail pour l’allocation cible de capacité de calcul Azure Machine Learning entre plusieurs [espaces de travail](concept-workspace.md) dans le même abonnement.

Par défaut, tous les espaces de travail partagent le même quota que le quota au niveau de l’abonnement pour toutes les familles de machines virtuelles. Toutefois, vous pouvez définir un quota maximal pour les familles de machines virtuelles individuelles dans les espaces de travail d’un abonnement. Cela vous permet de partager la capacité et d’éviter les problèmes de conflit entre les ressources.

1. Accédez à n’importe quel espace de travail dans votre abonnement.
1. Dans le volet de gauche, sélectionnez **Utilisation + quotas**.
1. Sélectionnez l’onglet **Configurer les quotas** pour afficher les quotas.
1. Développez une famille de machines virtuelles.
1. Définissez une limite de quota sur un espace de travail répertorié sous cette famille de machines virtuelles.

Vous ne pouvez pas définir une valeur négative ou une valeur supérieure au quota au niveau de l’abonnement.

[![Capture d’écran montrant un quota au niveau de l’espace de travail Azure Machine Learning.](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)](./media/how-to-manage-quotas/azure-machine-learning-workspace-quota.png)

> [!NOTE]
> Vous avez besoin d’autorisations de niveau abonnement pour définir un quota au niveau de l’espace de travail.

## <a name="view-your-usage-and-quotas"></a>Voir votre utilisation et les quotas

Pour afficher votre quota pour différentes ressources Azure telles que les machines virtuelles, le stockage ou le réseau, utilisez le portail Azure :

1. Dans le volet gauche, sélectionnez **Tous les services**, puis **Abonnements** sous la catégorie **Général**.

2. Dans la liste des abonnements, sélectionnez celui dont vous recherchez le quota.

3. Sélectionnez **Utilisation + quotas** pour afficher l’utilisation et les limites de quota actuelles. Utilisez les filtres pour sélectionner le fournisseur et les emplacements. 

Vous gérez le quota de capacité de calcul Azure Machine Learning sur votre abonnement séparément des autres quotas Azure : 

1. Dans le portail Azure, accédez à votre espace de travail **Azure Machine Learning**.

2. Dans le volet de gauche, sous la section **Prise en charge + détection des problèmes**, sélectionnez **Utilisation + quotas** pour afficher vos limites de quota et votre utilisation actuelles.

3. Sélectionnez un abonnement pour afficher les limites de quota. Filtrez sur la région qui vous intéresse.

4. Vous pouvez basculer entre une vue au niveau de l’abonnement et une vue au niveau de l’espace de travail.

## <a name="request-quota-increases"></a>Demander des augmentations de quota

Pour augmenter la limite ou le quota au-delà de la limite par défaut, [ouvrez une demande de support client en ligne](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest/) gratuitement.

Vous ne pouvez pas augmenter les limites au-dessus des valeurs maximales indiquées dans les tableaux précédents. S’il n’y a pas de limite maximale, vous ne pouvez pas ajuster la limite pour la ressource.

Lorsque vous demandez une augmentation du quota, sélectionnez le service auquel vous pensez. Par exemple, sélectionnez Azure Machine Learning, Container Instances ou Stockage. En ce qui concerne la capacité de calcul Azure Machine Learning, vous pouvez sélectionner le bouton **Demander un quota** tout en visualisant le quota dans les étapes précédentes.

> [!NOTE]
> Les [abonnements d’essai gratuit](https://azure.microsoft.com/offers/ms-azr-0044p) ne permettent pas de bénéficier d’augmentations de la limite ou du quota. Si vous disposez d’un abonnement d’essai gratuit, vous pouvez le mettre à niveau vers un abonnement avec [paiement à l’utilisation](https://azure.microsoft.com/offers/ms-azr-0003p/). Pour en savoir plus, consultez la rubrique [Mise à niveau de la version d’évaluation gratuite d’Azure vers le paiement à l’utilisation](../cost-management-billing/manage/upgrade-azure-subscription.md) et la [FAQ sur le compte gratuit Azure](https://azure.microsoft.com/free/free-account-faq).

## <a name="next-steps"></a>Étapes suivantes

+ [Planifier et gérer les coûts d’Azure Machine Learning](concept-plan-manage-cost.md)
+ [Limites de service dans Azure Machine Learning](resource-limits-quotas-capacity.md)
+ [Résolution des problèmes de déploiement et de scoring de points de terminaison en ligne managés (préversion)](how-to-troubleshoot-managed-online-endpoints.md)