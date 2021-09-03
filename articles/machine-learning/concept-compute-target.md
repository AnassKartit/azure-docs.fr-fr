---
title: Qu’est-ce qu’une cible de calcul ?
titleSuffix: Azure Machine Learning
description: Découvrez comment désigner une ressource ou un environnement de calcul pour former ou déployer votre modèle avec Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 07/27/2021
ms.openlocfilehash: bb7baa20b5bc7e47e231e3e15937dde941ac0e03
ms.sourcegitcommit: 0ede6bcb140fe805daa75d4b5bdd2c0ee040ef4d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2021
ms.locfileid: "122608257"
---
# <a name="what-are-compute-targets-in-azure-machine-learning"></a>Qu’est-ce qu’une cible de calcul dans Azure Machine Learning ?

Une *cible de calcul* est un environnement ou une ressource de calcul où vous exécutez votre script d’entraînement ou hébergez votre déploiement de service. Cet emplacement peut être votre machine locale ou une ressource de calcul informatique. Les cibles de calcul facilitent la modification de votre environnement de calcul sans modifier votre code.

Dans un cycle de vie de développement de modèle type, vous pouvez effectuer les opérations suivantes :

1. Commencez par développer et expérimenter une petite quantité de données. À ce stade, utilisez votre environnement local, tel qu’un ordinateur local ou une machine virtuelle informatique, comme cible de calcul.
1. Effectuez un scale-up vers des données plus volumineuses ou effectuez une [formation distribuée](how-to-train-distributed-gpu.md) à l’aide d’une de ces [cibles de calcul de formation](#train).
1. Une fois que votre modèle est prêt, déployez-le sur un environnement d’hébergement web avec l’une de ces [cibles de calcul de déploiement](#deploy).

Les ressources de calcul utilisées pour vos cibles de calcul sont associées à un [espace de travail](concept-workspace.md). Les cibles de calcul autres que l’ordinateur local sont partagées par les utilisateurs de l’espace de travail.

## <a name="training-compute-targets"></a><a name="train"></a> Cibles de calcul d’entraînement

La prise en charge d’Azure Machine Learning varie selon les cibles de calcul. Un cycle de vie typique du développement d’un modèle commence par le déploiement ou l’expérimentation sur une petite quantité de données. À ce stade, utilisez un environnement local tel que votre ordinateur local ou une machine virtuelle informatique. Quand vous effectuez un scale-up de votre formation sur des jeux de données plus volumineux ou que vous effectuez une [formation distribuée](how-to-train-distributed-gpu.md), utilisez la capacité de calcul Azure Machine Learning pour créer un cluster avec un ou plusieurs nœuds qui se met à l’échelle automatiquement chaque fois que vous lancez une exécution. Vous pouvez également attacher votre propre ressource de calcul, bien que la prise en charge de différents scénarios puisse varier.

[!INCLUDE [aml-compute-target-train](../../includes/aml-compute-target-train.md)]

En savoir plus sur la façon de [soumettre une exécution de formation à une cible de calcul](how-to-set-up-training-targets.md).

## <a name="compute-targets-for-inference"></a><a name="deploy"></a> Cibles de calcul pour l’inférence

Pour effectuer l’inférence, Azure Machine Learning crée un conteneur Docker qui héberge le modèle et les ressources associées nécessaires pour l’utiliser. Ce conteneur est ensuite utilisé dans une cible de calcul.

[!INCLUDE [aml-deploy-target](../../includes/aml-compute-target-deploy.md)]

Découvrez [où et comment déployer votre modèle sur une cible de calcul](how-to-deploy-and-where.md).

<a name="amlcompute"></a>
## <a name="azure-machine-learning-compute-managed"></a>Calcul Azure Machine Learning (managé)

Une ressource de calcul managée est créée et managée par Azure Machine Learning. Ce calcul est optimisé pour les charges de travail Machine Learning. Les clusters de calcul Azure Machine Learning et les [instances de calcul](concept-compute-instance.md) sont les seuls calculs managés.

Vous pouvez créer des instances de calcul Azure Machine Learning ou des clusters de calcul depuis :

* [Azure Machine Learning Studio](how-to-create-attach-compute-studio.md).
* Le SDK Python et Azure CLI :
    * [Instance de calcul](how-to-create-manage-compute-instance.md).
    * [Cluster de calcul](how-to-create-attach-compute-cluster.md).
* Un modèle Azure Resource Manager. Pour obtenir un exemple de modèle, consultez [Créer un cluster de calcul Azure Machine Learning](https://github.com/Azure/azure-quickstart-templates/tree/master/quickstarts/microsoft.machinelearningservices/machine-learning-compute-create-amlcompute).
* Une [extension de Machine Learning pour l’interface de ligne de commande Azure](reference-azure-machine-learning-cli.md#resource-management).

Une fois créées, ces ressources de calcul font automatiquement partie de votre espace de travail, contrairement à d’autres types de cibles de calcul.


|Fonctionnalité  |Cluster de calcul  |Instance de calcul  |
|---------|---------|---------|
|Cluster unique ou à plusieurs nœuds     |    **&check;**       |    Cluster mononœud     |
|Mises à l’échelle automatique chaque fois que vous envoyez une exécution     |     **&check;**      |         |
|Gestion des clusters et planification automatiques des travaux     |   **&check;**        |     **&check;**      |
|Prise en charge des ressources UC et GPU     |  **&check;**         |    **&check;**       |


> [!NOTE]
> Quand un *cluster* de calcul est inactif, il adapte son échelle automatiquement à zéro nœud, ce qui vous évite de payer quand il n’est pas utilisé. Une *instance* de calcul est toujours activée et n’adapte pas son échelle automatiquement. Vous devez [arrêter l’instance de calcul](how-to-create-manage-compute-instance.md#manage) quand vous ne l’utilisez pas pour éviter des frais supplémentaires.

### <a name="supported-vm-series-and-sizes"></a>Tailles et séries de machine virtuelle prises en charge

Lorsque vous sélectionnez une taille de nœud pour une ressource de calcul managée dans Azure Machine Learning, vous pouvez choisir parmi les tailles de machines virtuelles disponibles dans Azure. Azure propose une gamme de tailles de machines virtuelles Windows et Linux pour différentes charges de travail. Pour plus d’informations, consultez [Types et tailles des machines virtuelles](../virtual-machines/sizes.md).

Quelques exceptions et limites s’appliquent quant au choix d’une taille de machine virtuelle :

* Certaines séries de machines virtuelles ne sont pas prises en charge dans Azure Machine Learning.
* Certaines séries de machines virtuelles sont restreintes. Pour utiliser une série restreinte, contactez le support technique et demandez une augmentation du quota pour la série. Pour plus d’informations sur la manière de contacter le support, consultez [Options de support Azure](https://azure.microsoft.com/support/options/).

Pour en savoir plus sur les séries prises en charge et les restrictions, consultez le tableau suivant.

| **Série de machines virtuelles prises en charge**  | **Restrictions** | **Catégorie** | **Pris en charge par** |
|------------|------------|------------|------------|
| [DDSv4](../virtual-machines/ddv4-ddsv4-series.md#ddsv4-series) | Aucun. | Usage général | Clusters et instance de calcul |
| [Dv2](../virtual-machines/dv2-dsv2-series.md#dv2-series) | Aucun. | Usage général | Clusters et instance de calcul |
| [Dv3](../virtual-machines/dv3-dsv3-series.md#dv3-series) | Aucun.| Usage général | Clusters et instance de calcul |
| [DSv2](../virtual-machines/dv2-dsv2-series.md#dsv2-series) | Aucun. | Usage général | Clusters et instance de calcul |
| [DSv3](../virtual-machines/dv3-dsv3-series.md#dsv3-series) | Aucun.| Usage général | Clusters et instance de calcul |
| [EAv4](../virtual-machines/eav4-easv4-series.md) | Aucun. | Mémoire optimisée | Clusters et instance de calcul |
| [Ev3](../virtual-machines/ev3-esv3-series.md) | Aucun. | Mémoire optimisée | Clusters et instance de calcul |
| [FSv2](../virtual-machines/fsv2-series.md) | Aucun. | Optimisé pour le calcul | Clusters et instance de calcul |
| [H](../virtual-machines/h-series.md) | Aucun. | Calcul haute performance | Clusters et instance de calcul |
| [HB](../virtual-machines/hb-series.md) | Nécessite une approbation. | Calcul haute performance | Clusters et instance de calcul |
| [HBv2](../virtual-machines/hbv2-series.md) | Nécessite une approbation. |  Calcul haute performance | Clusters et instance de calcul |
| [HC](../virtual-machines/hc-series.md) | Nécessite une approbation. |  Calcul haute performance | Clusters et instance de calcul |
| [M](../virtual-machines/m-series.md) | Nécessite une approbation. | Mémoire optimisée | Clusters et instance de calcul |
| [NC](../virtual-machines/nc-series.md) | Aucun. |  GPU | Clusters et instance de calcul |
| [NC Promo](../virtual-machines/nc-series.md) | Aucun. | GPU | Clusters et instance de calcul |
| [NCv2](../virtual-machines/ncv2-series.md) | Nécessite une approbation. | GPU | Clusters et instance de calcul |
| [NCv3](../virtual-machines/ncv3-series.md) | Nécessite une approbation. | GPU | Clusters et instance de calcul |
| [ND](../virtual-machines/nd-series.md) | Nécessite une approbation. | GPU | Clusters et instance de calcul |
| [NDv2](../virtual-machines/ndv2-series.md) | Nécessite une approbation. | GPU | Clusters et instance de calcul |
| [NV](../virtual-machines/nv-series.md) | Aucun. | GPU | Clusters et instance de calcul |
| [NVv3](../virtual-machines/nvv3-series.md) | Nécessite une approbation. | GPU | Clusters et instance de calcul |
| [NCasT4_v3](../virtual-machines/nct4-v3-series.md) | Nécessite une approbation. | GPU | Clusters et instance de calcul |
| [NDasrA100_v4](../virtual-machines/nda100-v4-series.md) | Nécessite une approbation. | GPU | Clusters et instance de calcul |


Même si Azure Machine Learning prend en charge ces séries de machines virtuelles, elles peuvent ne pas être disponibles dans toutes les régions Azure. Pour vérifier si les séries de machines virtuelles sont disponibles, consultez [Disponibilité des produits par région](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines).

> [!NOTE]
> Azure Machine Learning ne prend pas en charge toutes les tailles de machines virtuelles prises en charge par Azure Compute. Pour lister les tailles de machine virtuelle disponibles, utilisez l’une des méthodes suivantes :
> * [REST API](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/machinelearningservices/resource-manager/Microsoft.MachineLearningServices/stable/2020-08-01/examples/ListVMSizesResult.json)
> * [Kit de développement logiciel (SDK) Python](/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute#supported-vmsizes-workspace--location-none-)
>

Si vous utilisez les cibles de calcul avec GPU, il est important de s’assurer que les pilotes CUDA corrects sont installés dans l’environnement d’apprentissage. Pour déterminer la version de CUDA correcte à utiliser, reportez-vous au tableau suivant :

| **Architecture GPU**  | **Séries de machines virtuelles Azure** | **Versions de CUDA prises en charge** |
|------------|------------|------------|
| Ampere | NDA100_v4 | 11.0+ |
| Turing | NCT4_v3 | 10.0+ |
| Volta | NCv3, NDv2 | 9.0+ |
| Pascal | NCv2, ND | 9.0+ |
| Maxwell | NV, NVv3 | 9.0+ |
| Kepler | NC, NC Promo| 9.0+ |

En plus de vérifier la compatibilité des versions de CUDA avec le matériel, assurez-vous que la version de CUDA est compatible avec la version de l’infrastructure d’apprentissage automatique que vous utilisez : 

- Pour PyTorch, vous pouvez vérifier la compatibilité [ici](https://pytorch.org/get-started/previous-versions/). 
- Pour Tensorflow, vous pouvez vérifier la compatibilité [ici](https://www.tensorflow.org/install/source#gpu).

### <a name="compute-isolation"></a>Isolation du calcul

Le calcul Azure Machine Learning offre des tailles de machines virtuelles qui sont isolées dans un type de matériel spécifique et qui sont dédiées à un client unique. Les tailles de machines virtuelles isolées sont les mieux adaptées aux charges de travail qui nécessitent un niveau élevé d’isolement par rapport aux charges de travail des autres clients pour des raisons de conformité et d’exigences réglementaires, entre autres. L’utilisation d’une taille isolée garantit que votre machine virtuelle sera la seule en cours d’exécution sur cette instance de serveur spécifique.

Les offres actuelles de machines virtuelles isolées sont les suivantes :

* Standard_M128ms
* Standard_F72s_v2
* Standard_NC24s_v3
* Standard_NC24rs_v3*

*Prenant en charge RDMA

Pour en savoir plus sur l’isolement, consultez [Isolation dans le cloud public Azure](../security/fundamentals/isolation-choices.md).

## <a name="unmanaged-compute"></a>Calcul non managé

Une cible de calcul non managée n’est *pas* managée par Azure Machine Learning. Vous créez ce type de cible de calcul en dehors d’Azure Machine Learning, puis l’attachez à votre espace de travail. Il se peut que des étapes supplémentaires soient requises pour ces ressources de calcul non managées afin de maintenir ou d’améliorer les performances des charges de travail Machine Learning. 

Azure Machine Learning prend en charge les types de calcul non managés suivants :

* Votre ordinateur local
* Machines virtuelles distantes
* Azure HDInsight
* Azure Batch
* Azure Databricks
* Service Analytique Azure Data Lake
* Azure Container Instance
* Azure Kubernetes Service et Kubernetes compatible avec Azure Arc (préversion)

Pour plus d’informations, consultez [Configurer des cibles de calcul pour le déploiement et la formation de modèles](how-to-attach-compute-targets.md).

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment :
* [Utiliser une cible de calcul pour entraîner votre modèle](how-to-set-up-training-targets.md)
* [Déployer votre modèle sur une cible de calcul](how-to-deploy-and-where.md)
