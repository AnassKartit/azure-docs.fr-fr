---
title: Supprimer une instance managée SQL avec Azure Arc
description: Supprimez une instance managée SQL avec Azure Arc.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: dnethi
ms.author: dinethi
ms.reviewer: mikeray
ms.date: 07/30/2021
ms.topic: how-to
ms.openlocfilehash: 19f5befde22ed7b16302b7da5df313c476b47194
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122524498"
---
# <a name="delete-azure-arc-enabled-sql-managed-instance"></a>Supprimer une instance managée SQL avec Azure Arc
Cet article explique comment supprimer une instance managée SQL avec Azure Arc.


## <a name="view-existing-azure-arc-enabled-sql-managed-instances"></a>Afficher les instances managées SQL avec Azure Arc existantes
Pour afficher les instances SQL Managed Instance, exécutez la commande suivante :

```azurecli
az sql mi-arc list --k8s-namespace <namespace> --use-k8s
```

Le résultat suivant doit ressembler à ce qui suit :

```console
Name    Replicas    ServerEndpoint    State
------  ----------  ----------------  -------
demo-mi 1/1         10.240.0.4:32023  Ready
```

## <a name="delete-a-azure-arc-enabled-sql-managed-instance"></a>Supprimer une instance managée SQL avec Azure Arc
Pour supprimer un service SQL Managed Instance, exécutez la commande suivante :

```azurecli
az sql mi-arc delete -n <NAME_OF_INSTANCE> --k8s-namespace <namespace> --use-k8s
```

Le résultat suivant doit ressembler à ce qui suit :

```azurecli
# az sql mi-arc delete -n demo-mi --k8s-namespace <namespace> --use-k8s
Deleted demo-mi from namespace arc
```

## <a name="reclaim-the-kubernetes-persistent-volume-claims-pvcs"></a>Récupérer les revendications de volume persistant (PVC) Kubernetes

La suppression d’un service SQL Managed Instance n’a pas pour effet de supprimer ses [PVC](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) associés. C'est la procédure normale. L’objectif est d’aider l’utilisateur à accéder aux fichiers de base de données en cas de suppression accidentelle de l’instance. La suppression des PVC n’est pas obligatoire. Elle est cependant recommandée. Si vous ne récupérez pas ces PVC, vous finirai par obtenir des erreurs, car votre cluster Kubernetes estimera que l’espace disque est insuffisant. Pour récupérer les PVC, procédez comme suit :

### <a name="1-list-the-pvcs-for-the-server-group-you-deleted"></a>1. Répertoriez les PVC du groupe de serveurs que vous avez supprimés
Pour répertorier les PVC, exécutez la commande suivante :
```console
kubectl get pvc
```

Dans l’exemple ci-dessous, notez les PVC pour les instances gérées SQL que vous avez supprimées.
```console
# kubectl get pvc -n arc

NAME                    STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS      AGE
data-demo-mi-0        Bound     pvc-1030df34-4b0d-4148-8986-4e4c20660cc4   5Gi        RWO            managed-premium   13h
logs-demo-mi-0        Bound     pvc-11836e5e-63e5-4620-a6ba-d74f7a916db4   5Gi        RWO            managed-premium   13h
```

### <a name="2-delete-each-of-the-pvcs"></a>2. Supprimez chacun des PVC
Supprimez les PVC de données et de journal pour chacune des instances gérées SQL que vous avez supprimées.
Le format général de cette commande est le suivant : 
```console
kubectl delete pvc <name of pvc>
```

Exemple :
```console
kubectl delete pvc data-demo-mi-0 -n arc
kubectl delete pvc logs-demo-mi-0 -n arc
```

Chacune de ces commandes kubectl confirme la réussite de la suppression du PVC. Exemple :
```console
persistentvolumeclaim "data-demo-mi-0" deleted
persistentvolumeclaim "logs-demo-mi-0" deleted
```
  

> [!NOTE]
> Comme indiqué, le fait de ne pas supprimer les PVC peut finalement conduire votre cluster Kubernetes à une situation où il lèvera des erreurs. Certaines de ces erreurs peuvent inclure la possibilité de créer, de lire, de mettre à jour ou de supprimer des ressources de l’API Kubernetes, ou de pouvoir exécuter des commandes comme `az arcdata dc export` car les pods de contrôleur peuvent être éliminés des nœuds Kubernetes en raison de ce problème de stockage (comportement Kubernetes normal).
>
> Par exemple, vous pouvez voir des messages dans les journaux, similaires à ce qui suit :  
> - Annotations:    microsoft.com/ignore-pod-health: true  
> - État :         Échec  
> - Motif :         Supprimé  
> - Message :        La nœud manquait de ressource : éphémère-storage. Le contrôleur de conteneurs utilisait 16372 Ki, ce qui dépasse sa demande de 0.

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur les [fonctionnalités et les capacités de SQL Managed Instance avec Azure Arc](managed-instance-features.md)

[Commencer en créant un contrôleur de données](create-data-controller.md)

Vous avez déjà créé un contrôleur de données ? [Créer une instance SQL Managed Instance avec Azure Arc](create-sql-managed-instance.md)