---
title: Créer un contrôleur de données
description: Créez un contrôleur de données Azure Arc sur un cluster Kubernetes à nœuds multiples standard que vous avez déjà déployé.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 07/30/2021
ms.topic: how-to
ms.openlocfilehash: 74c41ed89a3980967532955e46d4dade9eaacdb1
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122531630"
---
# <a name="create-the-azure-arc-data-controller"></a>Créer le contrôleur de données Azure Arc


## <a name="overview-of-creating-the-azure-arc-data-controller"></a>Vue d'ensemble de la création du contrôleur de données Azure Arc

Les services de données avec Azure Arc peuvent être créés sur différents types de clusters Kubernetes et de services Kubernetes managés par le biais d’approches différentes.

Actuellement, la liste des services Kubernetes et distributions pris en charge est la suivante :

- Azure Kubernetes Service (AKS)
- Service Azure Kubernetes sur Azure Stack HCI
- Azure RedHat OpenShift (ARO)
- OpenShift Container Platform (OCP)
- AWS Elastic Kubernetes Service (EKS)
- Moteur Kubernetes Google Cloud (GKE)
- Kubernetes open source en amont, généralement déployé à l’aide de kubeadm

> [!IMPORTANT]
> * La version minimale prise en charge de Kubernetes est la version v1.17. Consultez [Problèmes connus](./release-notes.md#known-issues) pour plus d’informations. 
> * La version minimale prise en charge d’OCP est 4.5.
> * pour comprendre la connectivité requise entre votre environnement et Azure, consultez les [exigences de connectivité](connectivity.md).
> * pour comprendre les détails de la configuration de votre stockage persistant, consultez les [Instructions de configuration du stockage](storage-configuration.md).
> * si vous utilisez Azure Container Service, la taille de machine virtuelle du nœud Worker de votre cluster doit être au moins **Standard_D8s_v3** et utiliser des **disques Premium**. Le cluster ne doit pas s’étendre sur plusieurs zones de disponibilité. 
> * Si vous utilisez une autre distribution ou un autre service Kubernetes, vous devez vous assurer de disposer d’une taille de nœud minimale de 8 Go de RAM et de 4 cœurs, ainsi que d’une capacité totale de 32 Go de RAM disponibles pour l’ensemble de vos nœuds Kubernetes. Par exemple, vous pouvez avoir 1 nœud de 32 Go de RAM et 4 cœurs, ou 2 nœuds de 16 Go de RAM et 4 cœurs chacun.

> [!NOTE]
> Si vous utilisez Red Hat OpenShift Container Platform sur Azure, nous vous recommandons d'utiliser la dernière version disponible.

Selon l'option choisie, certains outils seront _requis_, mais nous vous recommandons d'[installer tous les outils clients](./install-client-tools.md) avant de commencer à créer le contrôleur de données Azure Arc.

Quelle que soit l'option choisie, pendant le processus de création, vous devez fournir les informations suivantes :

- **Nom du contrôleur de données** : nom descriptif de votre contrôleur de données, par exemple, « Contrôleur de données de production », « contrôleur de données Seattle ».
- **Nom d’utilisateur du contrôleur de données** : tout nom d’utilisateur pour l’administrateur du contrôleur de données.
- **Mot de passe du contrôleur de données** : mot de passe pour l’utilisateur administrateur du contrôleur de données.
- **Nom de votre espace de noms Kubernetes** : nom de l'espace de noms Kubernetes dans lequel vous souhaitez créer le contrôleur de données.
- **Mode de connectivité** : le mode de connectivité détermine le degré de connectivité de votre environnement de services de données Azure Arc vers Azure. Le mode de connectivité indirecte est en disponibilité générale. Le mode de connectivité directe est en préversion.  Pour plus d’informations, consultez [mode de connectivité](./connectivity.md). 
- **ID d'abonnement Azure** : GUID de l'abonnement Azure dans lequel vous souhaitez que soit créée la ressource de contrôleur de données dans Azure.
- **Nom du groupe de ressources Azure** : nom du groupe de ressources dans lequel vous souhaitez que soit créée la ressource de contrôleur de données dans Azure.
- **Emplacement Azure** : emplacement Azure où les métadonnées de la ressource de contrôleur de données seront stockées. Pour obtenir la liste des régions disponibles, consultez [Infrastructure globale Azure / Produits par région](https://azure.microsoft.com/global-infrastructure/services/?products=azure-arc). Les métadonnées et les informations de facturation relatives aux ressources Azure gérées par le contrôleur de données que vous déployez sont stockées uniquement à l’emplacement dans Azure que vous spécifiez comme paramètre d’emplacement. Si vous déployez en mode connecté directement, le paramètre d’emplacement du contrôleur de données sera le même que l’emplacement de la ressource d’emplacement personnalisée que vous ciblez.

## <a name="next-steps"></a>Étapes suivantes

Plusieurs options sont disponibles pour créer le contrôleur de données Azure Arc :

> **Vous voulez juste l’essayer ?**  
> Démarrez rapidement avec [Démarrage rapide d'Azure Arc](https://azurearcjumpstart.io/azure_arc_jumpstart/azure_arc_data/) sur Azure Kubernetes Service (AKS), AWS Elastic Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) ou sur une machine virtuelle Azure.
> 
- [Créez un contrôleur de données en mode de connexion indirecte avec l’interface CLI](create-data-controller-indirect-cli.md)
- [Créez un contrôleur de données en mode de connexion indirecte avec Azure Data Studio](create-data-controller-indirect-azure-data-studio.md)
- [Créez un contrôleur de données en mode de connexion indirecte à partir du portail Azure via un notebook Jupyter dans Azure Data Studio](create-data-controller-indirect-azure-portal.md)
- [Créez un contrôleur de données en mode de connexion indirecte avec des outils Kubernetes tels que kubectl ou oc](create-data-controller-using-kubernetes-native-tools.md)
- [Créez un contrôleur de données en mode de connexion directe](create-data-controller-direct-prerequisites.md)
- [Créer un contrôleur de données avec Démarrage rapide d'Azure Arc pour un déploiement test accéléré](https://azurearcjumpstart.io/azure_arc_jumpstart/azure_arc_data/)
