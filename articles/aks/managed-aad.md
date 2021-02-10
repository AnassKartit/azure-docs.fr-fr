---
title: Utiliser Azure AD dans Azure Kubernetes Service
description: Découvrez comment utiliser Azure AD dans Azure Kubernetes Service (AKS)
services: container-service
ms.topic: article
ms.date: 02/1/2021
ms.author: miwithro
ms.openlocfilehash: 7f6cf503a459175e3109a515b666bbeaa3a25b4d
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99549997"
---
# <a name="aks-managed-azure-active-directory-integration"></a>Intégration d’Azure Active Directory géré par AKS

L’intégration d’Azure AD géré par AKS est conçue pour simplifier l’expérience d’intégration d’Azure AD, où les utilisateurs devaient auparavant créer une application cliente et une application serveur et où le locataire Azure AD devait accorder des autorisations de lecture du répertoire. Dans la nouvelle version, le fournisseur de ressources AKS gère les applications cliente et serveur pour vous.

## <a name="azure-ad-authentication-overview"></a>Vue d’ensemble de l’authentification Azure AD

Les administrateurs de cluster peuvent configurer le contrôle d’accès en fonction du rôle Kubernetes (RBAC Kubernetes) en se basant sur l’identité ou l’appartenance à un groupe d’annuaires de l’utilisateur. L’authentification Azure AD est fournie aux clusters AKS à l’aide d’OpenID Connect. OpenID Connect est une couche d’identité basée sur le protocole OAuth 2.0. Pour plus d’informations sur OpenID Connect, voir la [documentation sur Open ID Connect][open-id-connect].

Pour en savoir plus sur le flux de l’intégration d’Azure AD, consultez la [documentation sur les concepts d’intégration d’Azure Active Directory](concepts-identity.md#azure-active-directory-integration).

## <a name="limitations"></a>Limites 

* L’intégration d’Azure AD géré par AKS ne peut pas être désactivée.
* Les clusters sans RBAC Kubernetes ne sont pas pris en charge pour l’intégration d’Azure AD managé par AKS
* La modification de l’abonné Azure AD associé à l’intégration d’Azure AD géré par AKS n’est pas prise en charge

## <a name="prerequisites"></a>Prérequis

* Azure CLI 2.11.0 ou une version ultérieure
* Kubectl avec une version minimale [1.18.1](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#v1181) ou [kubelogin](https://github.com/Azure/kubelogin)
* Si vous utilisez [helm](https://github.com/helm/helm), version minimale de helm 3.3.

> [!Important]
> Vous devez utiliser Kubectl avec une version minimale 1.18.1 ou kubelogin. Si vous n’utilisez pas la version appropriée, vous remarquerez des problèmes d’authentification.

Pour installer kubectl et kubelogin, utilisez les commandes suivantes :

```azurecli-interactive
sudo az aks install-cli
kubectl version --client
kubelogin --version
```

Pour d’autres systèmes d’exploitation, utilisez [ces instructions](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

## <a name="before-you-begin"></a>Avant de commencer

Pour votre cluster, vous avez besoin d’un groupe Azure AD. Le cluster a besoin de ce groupe comme groupe d’administration afin d’accorder des autorisations d’administrateur de cluster. Vous pouvez utiliser un groupe Azure AD existant ou en créer un. Enregistrez l’ID d’objet de votre groupe Azure AD.

```azurecli-interactive
# List existing groups in the directory
az ad group list --filter "displayname eq '<group-name>'" -o table
```

Pour créer un groupe Azure AD pour vos administrateurs de cluster, utilisez la commande suivante :

```azurecli-interactive
# Create an Azure AD group
az ad group create --display-name myAKSAdminGroup --mail-nickname myAKSAdminGroup
```

## <a name="create-an-aks-cluster-with-azure-ad-enabled"></a>Créer un cluster AKS compatible Azure AD

Créez un cluster AKS à l’aide des commandes CLI suivantes.

Créez un groupe de ressources Azure :

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location centralus
```

Créer un cluster AKS et activer l’accès administrateur pour votre groupe Azure AD

```azurecli-interactive
# Create an AKS-managed Azure AD cluster
az aks create -g myResourceGroup -n myManagedCluster --enable-aad --aad-admin-group-object-ids <id> [--aad-tenant-id <id>]
```

Lorsqu’un cluster Azure AD géré par AKS a bien été créé, vous voyez la section suivante dans le corps de la réponse.
```output
"AADProfile": {
    "adminGroupObjectIds": [
      "5d24****-****-****-****-****afa27aed"
    ],
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

Une fois le cluster créé, vous pouvez commencer à y accéder.

## <a name="access-an-azure-ad-enabled-cluster"></a>Accéder à un cluster compatible Azure AD

Pour effectuer les étapes ci-dessous, vous avez besoin du rôle intégré [Azure Kubernetes Service Cluster User](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-user-role) (Utilisateur de cluster Azure Kubernetes Service).

Récupérez les informations d’identification d’utilisateur permettant d’accéder au cluster :
 
```azurecli-interactive
 az aks get-credentials --resource-group myResourceGroup --name myManagedCluster
```
Suivez les instructions pour vous connecter.

Utilisez la commande kubectl get nodes pour voir les nœuds figurant dans le cluster :

```azurecli-interactive
kubectl get nodes

NAME                       STATUS   ROLES   AGE    VERSION
aks-nodepool1-15306047-0   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-1   Ready    agent   102m   v1.15.10
aks-nodepool1-15306047-2   Ready    agent   102m   v1.15.10
```
Configurez le [contrôle d’accès en fonction du rôle Azure (Azure RBAC)](./azure-ad-rbac.md) afin de configurer des groupes de sécurité supplémentaires pour vos clusters.

## <a name="troubleshooting-access-issues-with-azure-ad"></a>Résolution des problèmes d’accès avec Azure AD

> [!Important]
> Les étapes décrites ci-dessous contournent l’authentification normale des groupes Azure AD. Effectuez ces étapes uniquement en cas d’urgence.

Si vous restez bloqué sans pouvoir accéder à aucun groupe Azure AD valide ayant accès à votre cluster, vous pouvez obtenir les informations d’identification d’administrateur qui vous permettront d’accéder au cluster directement.

Pour effectuer ces étapes, vous devez avoir accès au rôle intégré [Azure Kubernetes Service Cluster Admin](../role-based-access-control/built-in-roles.md#azure-kubernetes-service-cluster-admin-role).

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myManagedCluster --admin
```

## <a name="enable-aks-managed-azure-ad-integration-on-your-existing-cluster"></a>Activer l’intégration de Azure AD géré par AKS sur votre cluster existant

Vous pouvez activer l’intégration d’Azure AD géré par AKS sur votre cluster avec RBAC Kubernetes existant. Veillez à définir votre groupe d’administration pour conserver l’accès à votre cluster.

```azurecli-interactive
az aks update -g MyResourceGroup -n MyManagedCluster --enable-aad --aad-admin-group-object-ids <id-1> [--aad-tenant-id <id>]
```

Lorsqu’un cluster Azure AD géré par AKS a bien été activé, vous voyez la section suivante dans le corps de la réponse

```output
"AADProfile": {
    "adminGroupObjectIds": [
      "5d24****-****-****-****-****afa27aed"
    ],
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

Téléchargez à nouveau les informations d’identification de l’utilisateur pour accéder à votre cluster en suivant les étapes [ici][access-cluster].

## <a name="upgrading-to-aks-managed-azure-ad-integration"></a>Mise à niveau vers l’intégration d’Azure AD géré par AKS

Si votre cluster utilise l’intégration d’Azure AD héritée, vous pouvez effectuer la mise à niveau vers l’intégration d’Azure AD géré par AKS.

```azurecli-interactive
az aks update -g myResourceGroup -n myManagedCluster --enable-aad --aad-admin-group-object-ids <id> [--aad-tenant-id <id>]
```

Lorsqu’un cluster Azure AD géré par AKS a bien été migré, vous voyez la section suivante dans le corps de la réponse.

```output
"AADProfile": {
    "adminGroupObjectIds": [
      "5d24****-****-****-****-****afa27aed"
    ],
    "clientAppId": null,
    "managed": true,
    "serverAppId": null,
    "serverAppSecret": null,
    "tenantId": "72f9****-****-****-****-****d011db47"
  }
```

Si vous souhaitez accéder au cluster, suivez les étapes décrites [ici][access-cluster].

## <a name="non-interactive-sign-in-with-kubelogin"></a>Connexion non interactive avec kubelogin

Certains scénarios non interactifs, tels que les pipelines d’intégration continue, ne sont pas possibles avec kubectl. Utilisez [`kubelogin`](https://github.com/Azure/kubelogin) pour accéder au cluster à l’aide d’une connexion non interactive avec le principal de service.

## <a name="use-conditional-access-with-azure-ad-and-aks"></a>Utiliser l’accès conditionnel avec Azure AD et AKS

Lorsque vous intégrez Azure AD à votre cluster AKS, vous pouvez également utiliser l’[accès conditionnel][aad-conditional-access] pour contrôler l’accès à votre cluster.

> [!NOTE]
> L’accès conditionnel Azure AD est une fonctionnalité Azure AD Premium.

Pour créer un exemple de stratégie d’accès conditionnel à utiliser avec AKS, procédez comme suit :

1. En haut du portail Azure, recherchez et sélectionnez Azure Active Directory.
1. Dans le menu Azure Active Directory sur le côté gauche, sélectionnez *Applications d’entreprise*.
1. Dans le menu Applications d’entreprise sur le côté gauche, sélectionnez *Accès conditionnel*.
1. Dans le menu Accès conditionnel sur le côté gauche, sélectionnez *Stratégies* puis *Nouvelle stratégie*.
    :::image type="content" source="./media/managed-aad/conditional-access-new-policy.png" alt-text="Ajout d’une stratégie d’accès conditionnel":::
1. Entrez le nom de la stratégie, par exemple *aks-policy*.
1. Sélectionnez *Utilisateurs et groupes*, puis sous *Inclure* sélectionnez *Sélectionner des utilisateurs et des groupes*. Choisissez les utilisateurs et les groupes auxquels vous souhaitez appliquer la stratégie. Pour cet exemple, choisissez le groupe Azure AD qui dispose déjà d’un accès d’administration à votre cluster.
    :::image type="content" source="./media/managed-aad/conditional-access-users-groups.png" alt-text="Sélection d’utilisateurs ou de groupes pour appliquer la stratégie d’accès conditionnel":::
1. Sélectionnez *Applications ou actions cloud*, puis sous *Inclure* sélectionnez *Sélectionner des applications*. Recherchez *Azure Kubernetes Service* et sélectionnez *Serveur AAD Azure Kubernetes Service*.
    :::image type="content" source="./media/managed-aad/conditional-access-apps.png" alt-text="Sélection du serveur AD Azure Kubernetes Service pour appliquer la stratégie d’accès conditionnel":::
1. Sous *Contrôles d’accès*, sélectionnez *Accorder*. Sélectionnez *Accorder l’accès* puis *Exiger que l’appareil soit marqué comme conforme*.
    :::image type="content" source="./media/managed-aad/conditional-access-grant-compliant.png" alt-text="Sélection de l’option Autoriser uniquement les appareils conformes pour la stratégie d’accès conditionnel":::
1. Sous *Activer une stratégie*, sélectionnez *Activé*, puis *Créer*.
    :::image type="content" source="./media/managed-aad/conditional-access-enable-policy.png" alt-text="Activation de la stratégie d’accès conditionnel":::

Récupérez les informations d’identification d’utilisateur permettant d’accéder au cluster, par exemple :

```azurecli-interactive
 az aks get-credentials --resource-group myResourceGroup --name myManagedCluster
```

Suivez les instructions pour vous connecter.

Exécutez la commande `kubectl get nodes` pour afficher les nœuds du cluster :

```azurecli-interactive
kubectl get nodes
```

Suivez les instructions pour vous reconnecter. Notez qu’un message d’erreur s’affiche, indiquant que vous êtes correctement connecté, mais que votre administrateur requiert que l’appareil demandant l’accès soit géré par votre instance Azure AD pour accéder à la ressource.

Dans le portail Azure, accédez à Azure Active Directory, sélectionnez *Applications d’entreprise*, puis sous *Activité*, sélectionnez *Connexions*. Notez une entrée en haut avec l’*état* *Échec* et l’*accès conditionnel* *Réussite*. Sélectionnez l’entrée, puis *Accès conditionnel* dans *Détails*. Notez que votre stratégie d’accès conditionnel est indiquée.

:::image type="content" source="./media/managed-aad/conditional-access-sign-in-activity.png" alt-text="Entrée de connexion ayant échoué en raison d’une stratégie d’accès conditionnel":::

## <a name="next-steps"></a>Étapes suivantes

* Apprenez-en davantage sur l’[intégration d’Azure RBAC pour l’autorisation Kubernetes][azure-rbac-integration].
* Apprenez-en davantage sur l’[intégration d’Azure AD avec Kubernetes RBAC][azure-ad-rbac].
* Utilisez [kubelogin](https://github.com/Azure/kubelogin) pour accéder aux fonctionnalités de l’authentification Azure non disponibles dans kubectl.
* Apprenez-en davantage sur les [concepts d’identité AKS et Kubernetes][aks-concepts-identity].
* Utilisez des [modèles ARM (Azure Resource Manager)][aks-arm-template] pour créer des clusters avec Azure AD géré par AKS.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[aks-arm-template]: /azure/templates/microsoft.containerservice/managedclusters

<!-- LINKS - Internal -->
[aad-conditional-access]: ../active-directory/conditional-access/overview.md
[azure-rbac-integration]: manage-azure-rbac.md
[aks-concepts-identity]: concepts-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v2-protocols-oidc.md
[az-ad-user-show]: /cli/azure/ad/user#az-ad-user-show
[rbac-authorization]: concepts-identity.md#role-based-access-controls-rbac
[operator-best-practices-identity]: operator-best-practices-identity.md
[azure-ad-rbac]: azure-ad-rbac.md
[azure-ad-cli]: azure-ad-integration-cli.md
[access-cluster]: #access-an-azure-ad-enabled-cluster
[aad-migrate]: #upgrading-to-aks-managed-azure-ad-integration
