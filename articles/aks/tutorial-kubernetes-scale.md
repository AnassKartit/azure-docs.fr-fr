---
title: Didacticiel Kubernetes sur Azure – Mise à l’échelle d’une application
description: Dans ce didacticiel Azure Kubernetes Service (AKS), vous découvrez comment mettre à l’échelle des nœuds et pods dans Kubernetes, et comment implémenter la mise à l’échelle automatique de pod horizontal.
services: container-service
ms.topic: tutorial
ms.date: 05/24/2021
ms.custom: mvc, devx-track-azurepowershell
ms.openlocfilehash: 0c577a316e5034e4a21599b0806be534c5f6888a
ms.sourcegitcommit: 20acb9ad4700559ca0d98c7c622770a0499dd7ba
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2021
ms.locfileid: "110697783"
---
# <a name="tutorial-scale-applications-in-azure-kubernetes-service-aks"></a>Tutoriel : Mettre à l’échelle des applications dans Azure Kubernetes Service (AKS)

Si vous avez suivi les tutoriels, vous disposez d’un cluster Kubernetes opérationnel dans AKS, et vous avez déployé l’exemple d’application Azure Voting. Dans ce tutoriel (cinquième d’une série de sept), vous allez effectuer un scale-out des pods dans l’application et essayer la mise à l’échelle automatique des pods. Vous allez également apprendre à mettre à l’échelle le nombre de nœuds de machine virtuelle Azure afin de modifier la capacité du cluster pour l’hébergement des charges de travail. Vous allez apprendre à effectuer les actions suivantes :

> [!div class="checklist"]
> * Mettre à l’échelle les nœuds Kubernetes
> * Mettre à l’échelle manuellement des pods Kubernetes qui exécutent votre application
> * Configurer la mise à l’échelle automatique des pods qui exécutent le serveur frontal d’applications

Dans les tutoriels ultérieurs, l’application Azure Vote est mise à jour vers une nouvelle version.

## <a name="before-you-begin"></a>Avant de commencer

Dans les tutoriels précédents, une application a été empaquetée dans une image conteneur. Cette image a été chargée dans Azure Container Registry et vous avez créé un cluster AKS. L’application a ensuite été déployée sur le cluster AKS. Si vous n’avez pas effectué ces étapes et si vous souhaitez suivre cette procédure, commencez par [Tutoriel 1 : Créer des images conteneur][aks-tutorial-prepare-app].

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Ce didacticiel nécessite l’exécution de l’interface de ligne de commande Azure CLI version 2.0.53 ou ultérieure. Exécutez `az --version` pour trouver la version. Si vous devez installer ou mettre à niveau, voir [Installer Azure CLI][azure-cli-install].

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Ce tutoriel nécessite que vous exécutiez Azure PowerShell version 5.9.0 ou ultérieure. Exécutez `Get-InstalledModule -Name Az` pour trouver la version. Si vous avez besoin de procéder à une installation ou à une mise à niveau, consultez [Installer Azure PowerShell][azure-powershell-install].

---

## <a name="manually-scale-pods"></a>Mettre à l’échelle des pods manuellement

Lorsque le serveur frontal Azure Vote et l’instance Redis ont été déployés dans des didacticiels précédents, un seul réplica a été créé. Pour voir le nombre et l’état des pods de votre cluster, utilisez la commande [kubectl get][kubectl-get] comme suit :

```console
kubectl get pods
```

L’exemple de sortie suivant montre un pod de serveur frontal et un pod de serveur principal :

```output
NAME                               READY     STATUS    RESTARTS   AGE
azure-vote-back-2549686872-4d2r5   1/1       Running   0          31m
azure-vote-front-848767080-tf34m   1/1       Running   0          31m
```

Pour changer manuellement le nombre de pods dans le déploiement *azure-vote-front*, utilisez la commande [kubectl scale][kubectl-scale]. L’exemple suivant augmente le nombre de pods de serveur frontal à *5* :

```console
kubectl scale --replicas=5 deployment/azure-vote-front
```

Réexécutez [kubectl get pods][kubectl-get] pour vérifier qu’AKS crée correctement les pods supplémentaires. Au bout d’une minute environ, les pods sont disponibles dans votre cluster :

```console
kubectl get pods

                                    READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m
```

## <a name="autoscale-pods"></a>Mettre à l’échelle les pods automatiquement

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Kubernetes prend en charge la [mise à l’échelle automatique des pods horizontaux][kubernetes-hpa] pour ajuster le nombre de pods dans un déploiement en fonction de l’utilisation du processeur ou d’autres métriques. [Metrics Server][metrics-server] est utilisé pour indiquer l’utilisation des ressources à Kubernetes et est automatiquement déployé dans les clusters AKS versions 1.10 et ultérieures. Pour voir la version de votre cluster AKS, utilisez la commande [az aks show][az-aks-show], comme indiqué dans l’exemple suivant :

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --query kubernetesVersion --output table
```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

Kubernetes prend en charge la [mise à l’échelle automatique des pods horizontaux][kubernetes-hpa] pour ajuster le nombre de pods dans un déploiement en fonction de l’utilisation du processeur ou d’autres métriques. [Metrics Server][metrics-server] est utilisé pour indiquer l’utilisation des ressources à Kubernetes et est automatiquement déployé dans les clusters AKS versions 1.10 et ultérieures. Pour voir la version de votre cluster AKS, utilisez la cmdlet [Get-AzAksCluster][get-azakscluster], comme indiqué dans l’exemple suivant :

```azurepowershell
(Get-AzAksCluster -ResourceGroupName myResourceGroup -Name myAKSCluster).KubernetesVersion
```

---

> [!NOTE]
> Si votre cluster AKS est inférieur à *1.10*, Metrics Server n’est pas installé automatiquement. Les manifestes d’installation de Metrics Server sont disponibles en tant que ressource `components.yaml` dans les versions de Metrics Server, ce qui signifie que vous pouvez les installer via une URL. Pour en savoir plus sur ces définitions YAML, consultez la section [Déploiement][metrics-server-github] du fichier Lisez-moi.
>
> Exemple d’installation :
> ```console
> kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.3.6/components.yaml
> ```

Pour utiliser la mise à l’échelle automatique, tous les conteneurs de vos pods et vos pods doivent avoir des demandes et limites de processeur définies. Dans le déploiement `azure-vote-front`, le conteneur front-end demande déjà 0,25 processeur, avec une limite de 0,5 processeur. Ces demandes et limites de ressources sont définies comme indiqué dans l’exemple d’extrait suivant :

```yaml
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```

L’exemple suivant utilise la commande [kubectl autoscale][kubectl-autoscale] pour effectuer un scaling automatique du nombre de pods dans le déploiement *azure-vote-front*. Si l’utilisation moyenne du processeur sur tous les pods dépasse 50 % de l’utilisation demandée, l’outil de mise à l’échelle automatique (ou « autoscaler ») fait passer le nombre de pods à *10* instances, au maximum. Un minimum de *3* instances est ensuite défini pour le déploiement :

```console
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
```

Vous pouvez également créer un fichier manifeste pour définir le comportement de la mise à l’échelle automatique et les limites des ressources. Voici un exemple de fichier manifeste nommé `azure-vote-hpa.yaml`.

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: azure-vote-back-hpa
spec:
  maxReplicas: 10 # define max replica count
  minReplicas: 3  # define min replica count
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: azure-vote-back
  targetCPUUtilizationPercentage: 50 # target CPU utilization

---

apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: azure-vote-front-hpa
spec:
  maxReplicas: 10 # define max replica count
  minReplicas: 3  # define min replica count
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: azure-vote-front
  targetCPUUtilizationPercentage: 50 # target CPU utilization
```

Utilisez `kubectl apply` pour appliquer la mise à l’échelle automatique définie dans le fichier manifeste `azure-vote-hpa.yaml`.

```console
kubectl apply -f azure-vote-hpa.yaml
```

Pour voir l’état de la mise à l’échelle automatique, utilisez la commande `kubectl get hpa` comme suit :

```
kubectl get hpa

NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

Au bout de quelques minutes, avec une charge minimale sur l’application Azure Vote, le nombre de réplicas de pods descend automatiquement à trois. Vous pouvez utiliser à nouveau `kubectl get pods` pour voir les pods inutiles en cours de suppression.

## <a name="manually-scale-aks-nodes"></a>Mettre manuellement à l’échelle les nœuds AKS

Si vous avez créé votre cluster Kubernetes à l’aide des commandes dans le tutoriel précédent, le cluster comporte deux nœuds. Vous pouvez ajuster le nombre de nœuds manuellement si vous prévoyez davantage ou moins de charges de travail de conteneur sur votre cluster.

L’exemple suivant permet d’augmenter le nombre de nœuds à trois dans le cluster Kubernetes nommé *myAKSCluster*. Quelques minutes sont nécessaires pour exécuter la commande.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 3
```

Quand le cluster a été mis à l’échelle correctement, la sortie ressemble à l’exemple suivant :

```output
"agentPoolProfiles": [
  {
    "count": 3,
    "dnsPrefix": null,
    "fqdn": null,
    "name": "myAKSCluster",
    "osDiskSizeGb": null,
    "osType": "Linux",
    "ports": null,
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_D2_v2",
    "vnetSubnetId": null
  }
```

### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

```azurepowershell
Get-AzAksCluster -ResourceGroupName myResourceGroup -Name myAKSCluster | Set-AzAksCluster -NodeCount 3
```

Quand le cluster a été mis à l’échelle correctement, la sortie ressemble à l’exemple suivant :

```output
ProvisioningState       : Succeeded
MaxAgentPools           : 100
KubernetesVersion       : 1.19.9
DnsPrefix               : myAKSCluster
Fqdn                    : myakscluster-000a0aa0.hcp.eastus.azmk8s.io
PrivateFQDN             :
AgentPoolProfiles       : {default}
WindowsProfile          : Microsoft.Azure.Commands.Aks.Models.PSManagedClusterWindowsProfile
AddonProfiles           : {}
NodeResourceGroup       : MC_myresourcegroup_myAKSCluster_eastus
EnableRBAC              : True
EnablePodSecurityPolicy :
NetworkProfile          : Microsoft.Azure.Commands.Aks.Models.PSContainerServiceNetworkProfile
AadProfile              :
ApiServerAccessProfile  :
Identity                :
LinuxProfile            : Microsoft.Azure.Commands.Aks.Models.PSContainerServiceLinuxProfile
ServicePrincipalProfile : Microsoft.Azure.Commands.Aks.Models.PSContainerServiceServicePrincipalProfile
Id                      : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myresourcegroup/providers/Micros
                          oft.ContainerService/managedClusters/myAKSCluster
Name                    : myAKSCluster
Type                    : Microsoft.ContainerService/ManagedClusters
Location                : eastus
Tags                    : {}
```

---

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez utilisé différentes fonctionnalités de mise à l’échelle dans votre cluster Kubernetes. Vous avez appris à :

> [!div class="checklist"]
> * Mettre à l’échelle manuellement des pods Kubernetes qui exécutent votre application
> * Configurer la mise à l’échelle automatique des pods qui exécutent le serveur frontal d’applications
> * Mettre à l’échelle manuellement les nœuds Kubernetes

Passez au didacticiel suivant pour savoir comment mettre à jour une application dans Kubernetes.

> [!div class="nextstepaction"]
> [Mettre à jour une application dans Kubernetes][aks-tutorial-update-app]

<!-- LINKS - external -->
[kubectl-autoscale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#autoscale
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-scale]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
[kubernetes-hpa]: https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
[metrics-server-github]: https://github.com/kubernetes-sigs/metrics-server/blob/master/README.md#deployment
[metrics-server]: https://kubernetes.io/docs/tasks/debug-application-cluster/resource-metrics-pipeline/#metrics-server

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-update-app]: ./tutorial-kubernetes-app-update.md
[az-aks-scale]: /cli/azure/aks#az_aks_scale
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az_aks_show
[azure-powershell-install]: /powershell/azure/install-az-ps
[get-azakscluster]: /powershell/module/az.aks/get-azakscluster
