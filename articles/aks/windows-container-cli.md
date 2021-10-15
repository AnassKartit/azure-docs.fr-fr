---
title: Création d’un conteneur Windows Server sur un cluster AKS à l’aide d’Azure CLI
description: Découvrez comment créer rapidement un cluster Kubernetes et déployer une application dans un conteneur Windows Server dans Azure Kubernetes Service (AKS) à l’aide d’Azure CLI.
services: container-service
ms.topic: article
ms.date: 08/06/2021
ms.openlocfilehash: 284114ef1f9c8759eaec061b32282154fe72859c
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/29/2021
ms.locfileid: "129235304"
---
# <a name="create-a-windows-server-container-on-an-azure-kubernetes-service-aks-cluster-using-the-azure-cli"></a>Créer un conteneur Windows Server sur un cluster Azure Kubernetes Service (AKS) à l’aide d’Azure CLI

AKS (Azure Kubernetes Service) est un service Kubernetes managé qui vous permet de déployer et de gérer rapidement des clusters. Dans cet article, vous déployez un cluster AKS à l’aide d’Azure CLI. Vous déployez également un exemple d’application ASP.NET dans un conteneur Windows Server vers le cluster.

![Image de la navigation vers l’exemple d’application ASP.NET](media/windows-container/asp-net-sample-app.png)

Cet article suppose une compréhension élémentaire des concepts liés à Kubernetes. Pour plus d’informations, consultez [Concepts de base de Kubernetes pour AKS (Azure Kubernetes Service)][kubernetes-concepts].

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

### <a name="limitations"></a>Limites

Les limitations suivantes s’appliquent lorsque vous créez et gérez les clusters AKS prenant en charge plusieurs pools de nœuds :

* Vous ne pouvez pas supprimer le premier pool de nœuds.

Les limitations supplémentaires suivantes s’appliquent aux pools de nœuds Windows Server :

* Le cluster AKS peut comprendre au maximum 10 pools de nœuds.
* Le cluster AKS peut comprendre au maximum 100 nœuds dans chaque pool de nœuds.
* Le nom de pool de nœud Windows Server a une limite de 6 caractères.

## <a name="create-a-resource-group"></a>Créer un groupe de ressources

Un groupe de ressources Azure est un groupe logique dans lequel des ressources Azure sont déployées et gérées. Lorsque vous créez un groupe de ressources, vous devez spécifier un emplacement. Il s’agit de l’emplacement de stockage des métadonnées de groupe de ressources. C’est également là que vos ressources s’exécutent dans Azure si vous ne spécifiez pas une autre région lors de la création de ressources. Créez un groupe de ressources avec la commande [az group create][az-group-create].

L’exemple suivant crée un groupe de ressources nommé *myResourceGroup* à l’emplacement *eastus*.

> [!NOTE]
> Cet article utilise la syntaxe Bash pour les commandes dans ce didacticiel.
> Si vous utilisez Azure Cloud Shell, assurez-vous que la liste déroulante dans le coin supérieur gauche de la fenêtre de Cloud Shell est définie sur **Bash**.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

L’exemple de sortie suivant montre que le groupe de ressources a été créé correctement :

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

## <a name="create-an-aks-cluster"></a>Créer un cluster AKS

Pour pouvoir exécuter un cluster AKS qui prend en charge des pools de nœuds pour les conteneurs Windows Server, votre cluster doit appliquer une stratégie de réseau qui utilise un plug-in réseau [Azure CNI][azure-cni-about] (avancé). Pour plus d’informations sur la planification des plages de sous-réseau nécessaires et les considérations réseau, consultez [Configurer le réseau Azure CNI][use-advanced-networking]. Utilisez la commande [az aks create][az-aks-create] pour créer un cluster AKS nommé *myAKSCluster*. Cette commande crée les ressources réseau nécessaires si elles n’existent pas.

* Le cluster est configuré avec deux nœuds.
* Les paramètres `--windows-admin-password` et `--windows-admin-username` définissent les informations d’identification d’administrateur pour tous les nœuds Windows Server du cluster et doivent répondre aux [exigences relatives aux mots de passe de Windows Server][windows-server-password]. Si vous ne spécifiez pas le paramètre *windows-admin-password*, vous êtes invité à fournir une valeur.
* Le pool de nœuds utilise `VirtualMachineScaleSets`.

> [!NOTE]
> Pour garantir un fonctionnement fiable de votre cluster, vous devez exécuter au moins 2 (deux) nœuds dans le pool de nœuds par défaut.

Créez un nom d’utilisateur à utiliser en tant qu’informations d’identification d’administrateur pour les nœuds Windows Server sur votre cluster. Les commandes suivantes vous invitent à entrer un nom d’utilisateur et à lui affecter la valeur WINDOWS_USERNAME pour une utilisation dans une commande ultérieure (n’oubliez pas que les commandes de cet article sont entrées dans un interpréteur de commandes BASH).

```azurecli-interactive
echo "Please enter the username to use as administrator credentials for Windows Server nodes on your cluster: " && read WINDOWS_USERNAME
```

Créez votre cluster en vous assurant que vous spécifiez le paramètre `--windows-admin-username`. L’exemple de commande suivant crée un cluster à l’aide de la valeur de *WINDOWS_USERNAME* que vous avez définie dans la commande précédente. Vous pouvez également fournir un nom d’utilisateur différent directement dans le paramètre au lieu d’utiliser *WINDOWS_USERNAME*. La commande suivante vous invite également à créer un mot de passe pour les informations d’identification d’administrateur des nœuds Windows Server sur votre cluster. Vous pouvez également utiliser le paramètre *windows-admin-password* et y spécifier votre propre valeur.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 2 \
    --enable-addons monitoring \
    --generate-ssh-keys \
    --windows-admin-username $WINDOWS_USERNAME \
    --vm-set-type VirtualMachineScaleSets \
    --kubernetes-version 1.20.7 \
    --network-plugin azure
```

> [!NOTE]
> Si vous recevez une erreur de validation du mot de passe, vérifiez que le mot de passe que vous avez défini respecte les [exigences en matière de mot de passe de Windows Server][windows-server-password]. Si votre mot de passe répond aux conditions, essayez de créer votre groupe de ressources dans une autre région. Puis essayez de créer le cluster avec le nouveau groupe de ressources.
>
> Si vous ne spécifiez pas un nom d’utilisateur et mot de passe d’administrateur lorsque vous définissez `--vm-set-type VirtualMachineScaleSets` et `--network-plugin azure`, le nom d’utilisateur est défini sur *azureuser* et le mot de passe sur une valeur aléatoire.
> 
> Le nom d’utilisateur de l’administrateur ne peut pas être modifié, mais vous pouvez modifier le mot de passe d’administrateur utilisé par votre cluster AKS pour les nœuds Windows Server à l’aide de la commande `az aks update`. Pour plus d’informations, consultez [FAQ sur les pools de nœuds Windows Server][win-faq-change-admin-creds].

Au bout de quelques minutes, la commande se termine et retourne des informations au format JSON sur le cluster. Parfois, le provisionnement du cluster peut prendre plus de quelques minutes. Autorisez jusqu’à 10 minutes dans ces cas de figure.

## <a name="add-a-windows-server-node-pool"></a>Ajouter un pool de nœuds Windows Server

Par défaut, un cluster AKS est créé avec un pool de nœuds qui peut exécuter des conteneurs Linux. Utilisez la commande `az aks nodepool add` pour ajouter un pool de nœuds supplémentaire qui peut exécuter des conteneurs Windows Server en même temps que le pool de nœuds Linux.

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --os-type Windows \
    --name npwin \
    --node-count 1
```

La commande ci-dessus crée un pool de nœuds nommé *npwin* et l’ajoute à *myAKSCluster*. La commande ci-dessus utilise également le sous-réseau par défaut dans le réseau virtuel par défaut créé lors de l’exécution de `az aks create`.

## <a name="optional-using-containerd-with-windows-server-node-pools-preview"></a>Facultatif : utilisation de `containerd` avec des pools de nœuds Windows Server (préversion)

Depuis Kubernetes version 1.20, vous pouvez spécifier `containerd` comme runtime de conteneur pour des pools de nœuds Windows Server 2019.

[!INCLUDE [preview features callout](./includes/preview/preview-callout.md)]

Vous aurez besoin de la version 0.5.24 ou d’une version ultérieure de l’extension Azure CLI *aks-preview*. Installez l’extension d’Azure CLI *aks-preview* à l’aide de la commande [az extension add][az-extension-add]. Ou installez toutes les mises à jour disponibles à l’aide de la commande [az extension update][az-extension-update].

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

> [!IMPORTANT]
> Lors de l’utilisation `containerd` de avec des pools de nœuds Windows Server 2019 :
> - Le plan de contrôle et les pools de nœuds Windows Server 2019 doivent utiliser Kubernetes version 1.20 ou ultérieure.
> - Lors de la création ou de la mise à jour d’un pool de nœuds pour exécuter des conteneurs Windows Server, la valeur par défaut de *node-vm-size* est *Standard_D2s_v3*, soit la taille minimale recommandée pour des pools de nœuds Windows Server 2019 antérieurs à Kubernetes version 1.20. La taille minimale recommandée pour des pools de nœuds Windows Server 2019 utilisant `containerd` est *Standard_D4s_v3*. Si vous définissez le paramètre *node-vm-size*, consultez la liste des [tailles de machines virtuelles limitées][restricted-vm-sizes].
> - Il est fortement recommandé d’utiliser [des teintes ou des étiquettes][aks-taints] avec vos pools de nœuds Windows Server 2019 exécutant `containerd`, et des tolérances ou des sélecteurs de nœud avec vos déploiements pour garantir que vos charges de travail sont correctement planifiées.

Inscrivez l’indicateur de fonctionnalité `UseCustomizedWindowsContainerRuntime` à l’aide de la commande [az feature register][az-feature-register], comme indiqué dans l’exemple suivant :

```azurecli
az feature register --namespace "Microsoft.ContainerService" --name "UseCustomizedWindowsContainerRuntime"
```

Vous pouvez vérifier l’état de l’enregistrement à l’aide de la commande [az feature list][az-feature-list] :

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedWindowsContainerRuntime')].{Name:name,State:properties.state}"
```

Quand vous êtes prêt, actualisez l’inscription du fournisseur de ressources Microsoft.ContainerService à l’aide de la commande [az provider register][az-provider-register] :

```azurecli
az provider register --namespace Microsoft.ContainerService
```

### <a name="add-a-windows-server-node-pool-with-containerd-preview"></a>Ajouter un pool de nœuds Windows Server avec `containerd` (préversion)

Utilisez la commande `az aks nodepool add` pour ajouter un pool de nœuds supplémentaire pouvant exécuter des conteneurs Windows Server avec le runtime `containerd`.

> [!NOTE]
> Si vous ne spécifiez pas l’en-tête personnalisé *WindowsContainerRuntime=containerd*, le pool de nœuds utilise Docker comme runtime de conteneur.

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --os-type Windows \
    --name npwcd \
    --node-vm-size Standard_D4s_v3 \
    --kubernetes-version 1.20.5 \
    --aks-custom-headers WindowsContainerRuntime=containerd \
    --node-count 1
```

La commande ci-dessus crée un pool de nœuds Windows Server en utilisant `containerd` en tant que runtime nommé *npwcd*, et l’ajoute à *myAKSCluster*. La commande ci-dessus utilise également le sous-réseau par défaut dans le réseau virtuel par défaut créé lors de l’exécution de `az aks create`.

### <a name="upgrade-an-existing-windows-server-node-pool-to-containerd-preview"></a>Mettre à niveau un pool de nœuds Windows Server existant vers `containerd` (préversion)

Utilisez la commande `az aks nodepool upgrade` pour mettre à niveau un pool de nœuds spécifique de Docker vers `containerd`.

```azurecli
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name npwd \
    --kubernetes-version 1.20.7 \
    --aks-custom-headers WindowsContainerRuntime=containerd
```

La commande ci-dessus met à niveau un pool de nœuds nommé *npwd* vers le runtime `containerd`.

Pour mettre à niveau tous les pools de nœuds existants dans un cluster afin d’utiliser le runtime `containerd` pour tous les pools de nœuds Windows Server :

```azurecli
az aks upgrade \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --kubernetes-version 1.20.7 \
    --aks-custom-headers WindowsContainerRuntime=containerd
```

La commande ci-dessus met à niveau tous les pools de nœuds Windows Server dans *myAKSCluster* pour utiliser le runtime `containerd`.

> [!NOTE]
> Après la mise à niveau de tous les pools de nœuds Windows Server existants pour utiliser le runtime `containerd`, Docker sera toujours le runtime par défaut lors de l’ajout de nouveaux pools de nœuds Windows Server. 

## <a name="connect-to-the-cluster"></a>Se connecter au cluster

Pour gérer un cluster Kubernetes, vous utilisez [kubectl][kubectl], le client de ligne de commande Kubernetes. Si vous utilisez Azure Cloud Shell, `kubectl` est déjà installé. Pour installer `kubectl` en local, utilisez la commande [az aks install-cli][az-aks-install-cli] :

```azurecli
az aks install-cli
```

Pour configurer `kubectl` afin de vous connecter à votre cluster Kubernetes, exécutez la commande [az aks get-credentials][az-aks-get-credentials]. Cette commande télécharge les informations d’identification et configure l’interface CLI Kubernetes pour les utiliser.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Pour vérifier la connexion à votre cluster, utilisez la commande [kubectl get][kubectl-get] pour retourner une liste des nœuds du cluster.

```console
kubectl get nodes -o wide
```

L’exemple de sortie suivant montre tous les nœuds du cluster. Vérifiez que l’état de tous les nœuds est *Prêt* :

```output
NAME                                STATUS   ROLES   AGE    VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE                         KERNEL-VERSION     CONTAINER-RUNTIME
aks-nodepool1-12345678-vmss000000   Ready    agent   34m    v1.20.7   10.240.0.4    <none>        Ubuntu 18.04.5 LTS               5.4.0-1046-azure   containerd://1.4.4+azure
aks-nodepool1-12345678-vmss000001   Ready    agent   34m    v1.20.7   10.240.0.35   <none>        Ubuntu 18.04.5 LTS               5.4.0-1046-azure   containerd://1.4.4+azure
aksnpwcd123456                      Ready    agent   9m6s   v1.20.7   10.240.0.97   <none>        Windows Server 2019 Datacenter   10.0.17763.1879    containerd://1.4.4+unknown
aksnpwin987654                      Ready    agent   25m    v1.20.7   10.240.0.66   <none>        Windows Server 2019 Datacenter   10.0.17763.1879    docker://19.3.14
```

> [!NOTE]
> Le runtime de conteneur pour chaque pool de nœuds est affiché sous *CONTAINER-RUNTIME*. Notez que *aksnpwin987654* commence par `docker://`, ce qui signifie qu’il utilise Docker pour le runtime de conteneur. Notez que *aksnpwcd123456* commence par `containerd://`, ce qui signifie qu’il utilise `containerd` pour le runtime de conteneur.

## <a name="run-the-application"></a>Exécution de l'application

Un fichier manifeste Kubernetes définit un état souhaité pour le cluster, notamment les images conteneur à exécuter. Dans cet article, un manifeste est utilisé pour créer tous les objets nécessaires pour exécuter l’exemple d’application ASP.NET dans un conteneur Windows Server. Ce manifeste comprend un [déploiement Kubernetes][kubernetes-deployment] pour l’exemple d’application ASP.NET et un [service Kubernetes][kubernetes-service] externe pour accéder à l’application à partir d’Internet.

L’exemple d’application ASP.NET est fourni dans le cadre des [Exemples .NET Framework][dotnet-samples] et s’exécute dans un conteneur Windows Server. AKS exige que les conteneurs Windows Server soient basés sur des images de *Windows Server 2019* ou versions supérieures. Le fichier manifeste Kubernetes doit également définir un [sélecteur de nœud][node-selector] pour indiquer à votre cluster AKS d’exécuter le pod de votre exemple d’application ASP.NET sur un nœud qui peut exécuter des conteneurs Windows Server.

Créez un fichier nommé `sample.yaml`, et copiez-y la définition YAML suivante. Si vous utilisez Azure Cloud Shell, vous pouvez créer ce fichier à l’aide de `vi` ou de `nano` comme si vous travailliez sur un système virtuel ou physique :

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample
  labels:
    app: sample
spec:
  replicas: 1
  template:
    metadata:
      name: sample
      labels:
        app: sample
    spec:
      nodeSelector:
        "kubernetes.io/os": windows
      containers:
      - name: sample
        image: mcr.microsoft.com/dotnet/framework/samples:aspnetapp
        resources:
          limits:
            cpu: 1
            memory: 800M
          requests:
            cpu: .1
            memory: 300M
        ports:
          - containerPort: 80
  selector:
    matchLabels:
      app: sample
---
apiVersion: v1
kind: Service
metadata:
  name: sample
spec:
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
  selector:
    app: sample
```

Déployez l’application à l’aide de la commande [kubectl apply][kubectl-apply] et spécifiez le nom de votre manifeste YAML :

```console
kubectl apply -f sample.yaml
```

L’exemple de sortie suivant montre que le déploiement et le service ont été créés correctement :

```output
deployment.apps/sample created
service/sample created
```

## <a name="test-the-application"></a>Test de l’application

Quand l’application s’exécute, un service Kubernetes expose le front-end de l’application sur Internet. L’exécution de ce processus peut prendre plusieurs minutes. Parfois, le provisionnement du service peut prendre plus de quelques minutes. Autorisez jusqu’à 10 minutes dans ces cas de figure.

Pour surveiller la progression, utilisez la commande [kubectl get service][kubectl-get] avec l’argument `--watch`.

```console
kubectl get service sample --watch
```

Dans un premier temps, la valeur *EXTERNAL-IP* pour *l’exemple* de service apparaît comme étant *en attente*.

```output
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
sample             LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Quand l’adresse *EXTERNAL-IP* passe de l’état *pending* à une adresse IP publique réelle, utilisez `CTRL-C` pour arrêter le processus de surveillance `kubectl`. L’exemple de sortie suivant montre une adresse IP publique valide affectée au service :

```output
sample  LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Pour voir l’exemple d’application en action, ouvrez un navigateur web en utilisant l’adresse IP externe de votre service.

![Image de la navigation vers l’exemple d’application ASP.NET](media/windows-container/asp-net-sample-app.png)

> [!Note]
> Si vous dépassez le délai d’expiration en tentant de charger la page, vous devez vérifier que l’exemple d’application est prêt avec la commande [kubectl get pods --watch] suivante. Parfois, le conteneur Windows n’est pas démarré au moment où votre adresse IP externe est disponible.

## <a name="delete-cluster"></a>Supprimer un cluster

Lorsque vous n’avez plus besoin du cluster, utilisez la commande [az group delete][az-group-delete] pour supprimer le groupe de ressources, le service conteneur et toutes les ressources associées.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Lorsque vous supprimez le cluster, le principal de service Azure Active Directory utilisé par le cluster AKS n’est pas supprimé. Pour obtenir des instructions sur la façon de supprimer le principal de service, consultez [Considérations et suppression du principal de service AKS][sp-delete]. Si vous avez utilisé une identité managée, l’identité est managée par la plateforme et n’a pas besoin d’être supprimée.

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous avez déployé un cluster Kubernetes et déployé un exemple d’application ASP.NET dans un conteneur Windows Server vers ce cluster. [Accédez au tableau de bord web Kubernetes][kubernetes-dashboard] pour le cluster que vous venez de créer.

Pour en savoir plus sur AKS et parcourir le code complet de l’exemple de déploiement, passez au tutoriel sur le cluster Kubernetes.

> [!div class="nextstepaction"]
> [Didacticiel AKS][aks-tutorial]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[dotnet-samples]: https://hub.docker.com/_/microsoft-dotnet-framework-samples/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[aks-monitor]: ../azure-monitor/containers/container-insights-onboard.md
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[aks-taints]:  use-multiple-node-pools.md#specify-a-taint-label-or-tag-for-a-node-pool
[az-aks-browse]: /cli/azure/aks#az_aks_browse
[az-aks-create]: /cli/azure/aks#az_aks_create
[az-aks-get-credentials]: /cli/azure/aks#az_aks_get_credentials
[az-aks-install-cli]: /cli/azure/aks#az_aks_install_cli
[az-extension-add]: /cli/azure/extension#az_extension_add
[az-feature-list]: /cli/azure/feature#az_feature_list
[az-feature-register]: /cli/azure/feature#az_feature_register
[az-group-create]: /cli/azure/group#az_group_create
[az-group-delete]: /cli/azure/group#az_group_delete
[az-provider-register]: /cli/azure/provider#az_provider_register
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cni-about]: concepts-network.md#azure-cni-advanced-networking
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[kubernetes-dashboard]: kubernetes-dashboard.md
[restricted-vm-sizes]: quotas-skus-regions.md#restricted-vm-sizes
[use-advanced-networking]: configure-azure-cni.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[windows-server-password]: /windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements#reference
[win-faq-change-admin-creds]: windows-faq.md#how-do-i-change-the-administrator-password-for-windows-server-nodes-on-my-cluster
