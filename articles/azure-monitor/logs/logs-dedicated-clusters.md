---
title: Clusters dédiés pour les journaux Azure Monitor
description: Les clients qui ingèrent plus de 1 To de données d’analyse peuvent utiliser des clusters dédiés plutôt que des clusters partagés.
ms.topic: conceptual
author: rboucher
ms.author: robb
ms.date: 07/29/2021
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: e9385787dd48e3354d3359cf7991492b8f813f0c
ms.sourcegitcommit: 8000045c09d3b091314b4a73db20e99ddc825d91
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/19/2021
ms.locfileid: "122535255"
---
# <a name="azure-monitor-logs-dedicated-clusters"></a>Clusters dédiés pour les journaux Azure Monitor

Les Clusters dédiés pour les journaux Azure Monitor sont une option de déploiement qui permet de bénéficier de fonctionnalités avancées pour les clients des journaux Azure Monitor. Les clients peuvent sélectionner les espaces de travail Log Analytics qui doivent être hébergés sur des clusters dédiés.

Les clusters dédiés exigent des clients qu’ils s’engagent à utiliser une capacité d’au moins 1 To d’ingestion des données par jour. Vous pouvez migrer un espace de travail existant vers un cluster dédié sans perte de données ni interruption de service. 

Les fonctionnalités qui nécessitent des clusters dédiés sont les suivantes :

- **[Clés gérées par le client](../logs/customer-managed-keys.md)**  : chiffrez les données du cluster à l’aide de clés fournies et contrôlées par le client.
- **[Lockbox](../logs/customer-managed-keys.md#customer-lockbox-preview)**  : contrôlez les demandes d’accès à vos données par les ingénieurs du support technique Microsoft.
- **[Double chiffrement](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption)**  : permet d’éviter un scénario impliquant une possible compromission d’un algorithme ou d’une clé de chiffrement. Dans ce cas, la couche de chiffrement supplémentaire continue de protéger vos données.
- **[Zones de disponibilité](../../availability-zones/az-overview.md)**  : protégez vos données contre les défaillances de centre de données avec des zones de disponibilité sur un cluster dédié. Les zones de disponibilité sont des centres de données situés dans des emplacements physiques distincts. Elles sont équipées d’une alimentation, d’un refroidissement et d’une mise en réseau indépendants. Cette infrastructure indépendante et la séparation physique des zones réduisent la probabilité de survenue d’un incident puisque l’espace de travail peut reposer sur les ressources de l’une des zones.
- **[Espaces de travail multiples](../logs/cross-workspace-query.md)**  : si un client utilise plusieurs espaces de travail pour la production, il peut être judicieux d’utiliser un cluster dédié. Les requêtes entre espaces de travail s’exécuteront plus rapidement si tous les espaces de travail se trouvent sur le même cluster. Il peut s’avérer plus rentable d’utiliser un cluster dédié, car le niveau d’engagement affecté prend en compte toutes les ingestions de cluster et s’appliquent à tous ses espaces de travail, même si certains d’entre eux sont réduits et ne peuvent bénéficier de la remise de niveau d'engagement.



## <a name="management"></a>Gestion 

Les clusters dédiés sont gérés avec une ressource Azure qui représente les clusters de journaux Azure Monitor. Toutes les opérations sont effectuées sur cette ressource à l’aide de PowerShell ou de l’API REST.

Une fois le cluster créé, vous pouvez le configurer et des espaces de travail peuvent lui être associés. Lorsqu’un espace de travail est lié à un cluster, les nouvelles données envoyées à l’espace de travail résident sur le cluster. Seuls les espaces de travail qui se trouvent dans la même région que le cluster peuvent être liés à celui-ci. Les espaces de travail peuvent être dissociés d’un cluster avec certaines limitations. Cet article contient plus de détails sur ces limitations. 

Les données ingérées dans les clusters dédiés sont chiffrées deux fois : une fois au niveau du service à l’aide de clés gérées par Microsoft ou d’[une clé gérée par le client](../logs/customer-managed-keys.md), et une fois au niveau de l’infrastructure à l’aide de deux algorithmes de chiffrement différents et de deux clés différentes. Le [double chiffrement](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) permet d’éviter un scénario impliquant une possible compromission d’un algorithme ou d’une clé de chiffrement. Dans ce cas, la couche de chiffrement supplémentaire continue de protéger vos données. Le cluster dédié vous permet également de protéger vos données à l’aide du contrôle [Lockbox](../logs/customer-managed-keys.md#customer-lockbox-preview).

Toutes les opérations au niveau du cluster requièrent l’autorisation de l’action `Microsoft.OperationalInsights/clusters/write` sur le cluster. Cette autorisation peut être accordée via le propriétaire ou le contributeur qui contient l’action `*/write` ou via le rôle Contributeur Log Analytics qui contient l’action `Microsoft.OperationalInsights/*`. Pour plus d’informations sur les autorisations de Log Analytics, consultez [Gérer l’accès aux données de journal et aux espaces de travail dans Azure Monitor](./manage-access.md). 


## <a name="cluster-pricing-model"></a>Modèle de tarification des clusters

Les clusters dédiés Log Analytics utilisent un modèle de tarification de niveau d’engagement d’au moins 500 Go/jour. Toute utilisation au-delà du niveau de la couche sera facturée à un taux effectif par Go de ce niveau d’engagement.  Les informations sur la tarification par niveau d’engagement sont disponibles sur la [page de tarification Azure Monitor]( https://azure.microsoft.com/pricing/details/monitor/).  

Le niveau d’engagement du cluster est configuré par programmation avec Azure Resource Manager à l’aide du paramètre `Capacity` sous `Sku`. La valeur de `Capacity` est spécifiée en unités de Go et peut avoir les valeurs suivantes : 500, 1 000, 2 000 ou 5 000 Go/jour.

Il existe deux modes de facturation pour l’utilisation sur un cluster. Ils peuvent être spécifiés par le paramètre `billingType` lors de la configuration de votre cluster. 

1. **Cluster (par défaut)**  : la facturation des données ingérées est effectuée au niveau du cluster. Les quantités de données ingérées de chaque espace de travail associé à un cluster sont agrégées pour calculer la facture quotidienne du cluster. 

2. **Espaces de travail** : les coûts de niveau d’engagement pour votre cluster sont attribués proportionnellement aux espaces de travail du cluster, par volume d’ingestion des données (après prise en compte des allocations par nœud depuis [Azure Security Center](../../security-center/index.yml) pour chaque espace de travail). Les détails complets de ce modèle de tarification sont expliqués [ici](./manage-cost-storage.md#log-analytics-dedicated-clusters). 

Si votre espace de travail utilise le niveau tarifaire Par nœud hérité, lorsqu’il est lié à un cluster, il sera facturé en fonction des données ingérées par rapport au niveau d’engagement du cluster, et non plus Par nœud. Les allocations de données par nœud d’Azure Security Center continuent à être appliquées.

Vous trouverez les détails complets sur la facturation des clusters dédiés Log Analytics [ici](./manage-cost-storage.md#log-analytics-dedicated-clusters).

## <a name="asynchronous-operations-and-status-check"></a>Opérations asynchrones et vérification de l’état

Certaines étapes de configuration s’exécutent de façon asynchrone, car elles ne peuvent pas être effectuées rapidement. L’état de la réponse peut avoir l’une des valeurs suivantes : *InProgress*, *Updating*, *Deleting*, *Succeeded*, ou *Failed* dans le code d’erreur. Lors de l’utilisation de REST, la réponse retourne initialement un code d’état HTTP 202 (Accepté) et un en-tête avec la propriété Azure-AsyncOperation :

```JSON
"Azure-AsyncOperation": "https://management.azure.com/subscriptions/subscription-id/providers/Microsoft.OperationalInsights/locations/region-name/operationStatuses/operation-id?api-version=2021-06-01"
```

Pour vérifier l’état de l’opération asynchrone, envoyez une requête GET dans la valeur d’en-tête Azure-AsyncOperation :

```rest
GET https://management.azure.com/subscriptions/subscription-id/providers/microsoft.operationalInsights/locations/region-name/operationstatuses/operation-id?api-version=2021-06-01
Authorization: Bearer <token>
```

## <a name="create-a-dedicated-cluster"></a>Créer un cluster dédié

Lorsque vous créez un cluster dédié, vous devez spécifier les propriétés suivantes :

- **ClusterName** : Utilisée à des fins d’administration. Les utilisateurs ne sont pas exposés à ce nom.
- **ResourceGroupName** : groupe de ressources pour le cluster dédié. Vous devez utiliser un groupe central de ressources informatiques, car les clusters sont généralement partagés par de nombreuses équipes dans l’organisation. Pour plus d’informations sur les considérations de conception, consultez [Conception de votre déploiement de journaux Azure Monitor](../logs/design-logs-deployment.md).
- **Emplacement** : Un cluster se trouve dans une région Azure spécifique. Seuls les espaces de travail situés dans cette région peuvent être liés à ce cluster.
- **SkuCapacity** : vous devez spécifier le niveau d’engagement (SKU) lors de la création d’une ressource de cluster. Le niveau d’engagement peut être défini sur 500, 1 000, 2 000 ou 5 000 Go/jour. Pour plus d’informations sur les coûts des clusters, consultez [Gérer les coûts des clusters Log Analytics](./manage-cost-storage.md#log-analytics-dedicated-clusters). 
 

> [!NOTE]
> Les niveaux d’engagement étaient précédemment appelés réservations de capacité. 

Après avoir créé votre ressource de cluster, vous pouvez modifier des propriétés supplémentaires telles que *SKU*, *keyVaultProperties ou *billingType*. Pour plus d’informations, voir ci-dessous.

Vous pouvez disposer de 2 clusters actifs par abonnement et par région. Si le cluster est supprimé, il reste réservé pendant 14 jours. Vous pouvez disposer de 4 clusters réservés (actifs ou récemment supprimés) par abonnement et par région.

> [!WARNING]
> La création du cluster déclenche l’allocation et l’approvisionnement de la ressource. L’exécution de cette opération peut prendre quelques heures. Il est recommandé de l’exécuter de manière asynchrone.

Le compte d'utilisateur qui crée les clusters doit disposer de l'autorisation standard de création de ressources Azure `Microsoft.Resources/deployments/*` et de l'autorisation d'écriture de cluster `Microsoft.OperationalInsights/clusters/write` en ayant dans ses attributions de rôle une action spécifique, `Microsoft.OperationalInsights/*` ou `*/write`.

**PowerShell**

```powershell
New-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name} -Location {region-name} -SkuCapacity {daily-ingestion-gigabyte} -AsJob

# Check when the job is done
Get-Job -Command "New-AzOperationalInsightsCluster*" | Format-List -Property *
```

**REST API**

*Appeler* 

```rest
PUT https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2021-06-01
Authorization: Bearer <token>
Content-type: application/json

{
  "identity": {
    "type": "systemAssigned"
    },
  "sku": {
    "name": "capacityReservation",
    "Capacity": 500
    },
  "properties": {
    "billingType": "Cluster",
    },
  "location": "<region>",
}
```

*Réponse*

Doit être 202(Accepté) et un en-tête.



### <a name="check-cluster-provisioning-status"></a>Vérifier l’état de l’approvisionnement du cluster

L’approvisionnement du cluster Log Analytics prend un certain temps. Utilisez l’une des méthodes suivantes pour vérifier l’état d’approvisionnement :

**PowerShell**

Exécutez la commande *Get-AzOperationalInsightsCluster* avec le nom du groupe de ressources et vérifiez la propriété *ProvisioningState*. La valeur indique *ProvisioningAccount* pendant l’approvisionnement et *Succeeded* une fois celui-ci terminé. Copiez la valeur de l’URL Azure-AsyncOperation à partir de la réponse et suivez les instructions decontrôle de l’état des opérations asynchrones.


  ```powershell
  Get-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} 
  ```

 
**REST API**

Envoyez une requête GET sur la ressource de cluster et examinez la valeur *provisioningState*. La valeur indique *ProvisioningAccount* pendant l’approvisionnement et *Succeeded* une fois celui-ci terminé.

  ```rest
  GET https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2021-06-01
  Authorization: Bearer <token>
  ```

  **Réponse**

  ```json
  {
    "identity": {
      "type": "SystemAssigned",
      "tenantId": "tenant-id",
      "principalId": "principal-id"
    },
    "sku": {
      "name": "capacityreservation",
      "capacity": 500
    },
    "properties": {
      "provisioningState": "ProvisioningAccount",
      "clusterId": "cluster-id",
      "billingType": "Cluster",
      "lastModifiedDate": "last-modified-date",
      "createdDate": "created-date",
      "isDoubleEncryptionEnabled": false,
      "isAvailabilityZonesEnabled": false,
      "capacityReservationProperties": {
        "lastSkuUpdate": "last-sku-modified-date",
        "minCapacity": 500
      }
    },
    "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
    "name": "cluster-name",
    "type": "Microsoft.OperationalInsights/clusters",
    "location": "cluster-region"
  }
  ```

Le GUID *principalId* est généré par le service d’identité managée lors de la création du cluster.

---



## <a name="link-a-workspace-to-a-cluster"></a>Lier un espace de travail à un cluster

Lorsqu’un espace de travail Log Analytics est lié à un cluster dédié, les nouvelles données qui sont ingérées dans l’espace de travail sont acheminées vers le nouveau cluster alors que les données actuelles restent sur le cluster existant. Si le cluster dédié est chiffré à l’aide de clés gérées par le client (CMK), seules les nouvelles données sont chiffrées avec la clé. Le système abstrait cette différence. Vous pouvez ainsi interroger l’espace de travail comme d’habitude pendant que le système exécute des requêtes inter-clusters en arrière-plan.

Un cluster peut être lié à jusqu’à 1 000 espaces de travail. Les espaces de travail liés se trouvent dans la même région que le cluster. Pour protéger le serveur principal système et éviter la fragmentation des données, un espace de travail ne peut pas être lié à un cluster plus de deux fois par mois.

Pour effectuer l’opération de liaison, vous devez disposer des autorisations « d’écriture » pour l’espace de travail et la ressource de cluster :

- Dans l’espace de travail : *Microsoft.OperationalInsights/workspaces/write*
- Dans la ressource de cluster : *Microsoft.OperationalInsights/clusters/write*

Outre les aspects de facturation, l’espace de travail lié conserve ses propres paramètres, tels que la durée de conservation des données.

L’espace de travail et le cluster peuvent se trouver dans des abonnements différents. L’espace de travail et le cluster peuvent se trouver dans différents locataires si Azure Lighthouse est utilisé pour les mapper à un seul locataire.

La liaison d’un espace de travail ne peut être accomplie qu’une fois l’approvisionnement du cluster Log Analytics terminé.

> [!WARNING]
> La liaison d’un espace de travail à un cluster requiert la synchronisation de plusieurs composants principaux et la l’alimentation du cache. Dans la mesure où cette opération peut prendre jusqu’à deux heures, vous devez l’exécuter de manière asynchrone.


**PowerShell**

Utilisez la commande PowerShell suivante pour le relier à un cluster :

```powershell
# Find cluster resource ID
$clusterResourceId = (Get-AzOperationalInsightsCluster -ResourceGroupName {resource-group-name} -ClusterName {cluster-name}).id

# Link the workspace to the cluster
Set-AzOperationalInsightsLinkedService -ResourceGroupName {resource-group-name} -WorkspaceName {workspace-name} -LinkedServiceName cluster -WriteAccessResourceId $clusterResourceId -AsJob

# Check when the job is done
Get-Job -Command "Set-AzOperationalInsightsLinkedService" | Format-List -Property *
```

**REST API**

Utilisez l’appel REST suivant pour le relier à un cluster :

*Envoi*

```rest
PUT https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>/linkedservices/cluster?api-version=2021-06-01 
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "WriteAccessResourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/clusters/<cluster-name>"
    }
}
```

*Réponse*

202 (Accepté) et en-tête.

---

### <a name="check-workspace-link-status"></a>Vérifier l’état d’association de l’espace de travail
  
Lorsqu’un cluster est configuré avec des clés gérées par le client, les données ingérées dans les espaces de travail après la fin de l’opération de liaison sont stockées en étant chiffrées avec votre clé gérée. L’opération de liaison de l’espace de travail peut prendre jusqu’à 90 minutes et vous pouvez vérifier l’état de deux manières :

- Copiez la valeur de l’URL Azure-AsyncOperation à partir de la réponse et suivez les instructions decontrôle de l’état des opérations asynchrones.
- Effectuez une opération Get sur l’espace de travail et observez si la propriété *clusterResourceId* est présente dans la réponse sous *Fonctionnalités*.

**INTERFACE DE LIGNE DE COMMANDE**
```azurecli
az monitor log-analytics workspace show --resource-group "resource-group-name" --workspace-name "workspace-name"
```
**PowerShell**

```powershell
Get-AzOperationalInsightsWorkspace -ResourceGroupName "resource-group-name" -Name "workspace-name"
```

**REST API**

*Appeler*

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/microsoft.operationalinsights/workspaces/<workspace-name>?api-version=2021-06-01
Authorization: Bearer <token>
```

*Réponse*

```json
{
  "properties": {
    "source": "Azure",
    "customerId": "workspace-name",
    "provisioningState": "Succeeded",
    "sku": {
      "name": "pricing-tier-name",
      "lastSkuUpdate": "Tue, 28 Jan 2020 12:26:30 GMT"
    },
    "retentionInDays": 31,
    "features": {
      "legacy": 0,
      "searchVersion": 1,
      "enableLogAccessUsingOnlyResourcePermissions": true,
      "clusterResourceId": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name"
    },
    "workspaceCapping": {
      "dailyQuotaGb": -1.0,
      "quotaNextResetTime": "Tue, 28 Jan 2020 14:00:00 GMT",
      "dataIngestionStatus": "RespectQuota"
    }
  },
  "id": "/subscriptions/subscription-id/resourcegroups/resource-group-name/providers/microsoft.operationalinsights/workspaces/workspace-name",
  "name": "workspace-name",
  "type": "Microsoft.OperationalInsights/workspaces",
  "location": "region"
}
```

---


## <a name="change-cluster-properties"></a>Modifier les propriétés du cluster

Une fois que vous avez créé votre ressource de cluster et qu’elle est entièrement approvisionnée, vous pouvez modifier des propriétés supplémentaires à l’aide de PowerShell ou de l’API REST. Les propriétés supplémentaires qui peuvent être définies après l’approvisionnement du cluster sont les suivantes :

- **keyVaultProperties** : contient la clé dans Azure Key Vault avec les paramètres suivants : *KeyVaultUri*, *KeyName*, *KeyVersion*. Consultez [Mettre à jour le cluster avec les détails de l’identificateur de clé](../logs/customer-managed-keys.md#update-cluster-with-key-identifier-details).
- **Identité** : identité servant pour l’authentification auprès de votre coffre de clés. Elle peut avoir été attribuée par le système ou par l’utilisateur.
- **billingType** : attribution de facturation pour la ressource de cluster et ses données. Inclut les valeurs suivantes :
  - **Cluster (par défaut)**  : les coûts de votre cluster sont attribués à la ressource de cluster.
  - **Espaces de travail** : les coûts de votre cluster sont attribués proportionnellement aux espaces de travail du cluster. Une partie de l’utilisation est facturée à la ressource de cluster si le total des données ingérées pour la journée est inférieur au niveau d’engagement. Pour en savoir plus sur le modèle de tarification du cluster, consultez [Clusters dédiés Log Analytics](./manage-cost-storage.md#log-analytics-dedicated-clusters).


>[!IMPORTANT]
>La mise à jour du cluster ne doit pas inclure les détails relatifs à l’identité et à l’identificateur de clé dans la même opération. S’il vous faut les mettre à jour, cette mise à jour doit faire l’objet de deux opérations consécutives.

> [!NOTE]
> La propriété *billingType* n’est pas prise en charge dans PowerShell.

## <a name="get-all-clusters-in-resource-group"></a>Obtenir tous les clusters dans un groupe de ressources

**INTERFACE DE LIGNE DE COMMANDE**

```azurecli
az monitor log-analytics cluster list --resource-group "resource-group-name"
```
**PowerShell**

```powershell
Get-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name"
```

**REST API**

*Appeler*

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters?api-version=2021-06-01
Authorization: Bearer <token>
```

*Réponse*
  
```json
{
  "value": [
    {
      "identity": {
        "type": "SystemAssigned",
        "tenantId": "tenant-id",
        "principalId": "principal-id"
      },
      "sku": {
        "name": "capacityreservation",
        "capacity": 500
      },
      "properties": {
        "provisioningState": "Succeeded",
        "clusterId": "cluster-id",
        "billingType": "Cluster",
        "lastModifiedDate": "last-modified-date",
        "createdDate": "created-date",
        "isDoubleEncryptionEnabled": false,
        "isAvailabilityZonesEnabled": false,
        "capacityReservationProperties": {
          "lastSkuUpdate": "last-sku-modified-date",
          "minCapacity": 500
        }
      },
      "id": "/subscriptions/subscription-id/resourceGroups/resource-group-name/providers/Microsoft.OperationalInsights/clusters/cluster-name",
      "name": "cluster-name",
      "type": "Microsoft.OperationalInsights/clusters",
      "location": "cluster-region"
    }
  ]
}
```

---



## <a name="get-all-clusters-in-subscription"></a>Obtenir tous les clusters dans un abonnement

**INTERFACE DE LIGNE DE COMMANDE**

```azurecli
az monitor log-analytics cluster list
```

**PowerShell**

```powershell
Get-AzOperationalInsightsCluster
```
**REST API**

*Appeler*

```rest
GET https://management.azure.com/subscriptions/<subscription-id>/providers/Microsoft.OperationalInsights/clusters?api-version=2021-06-01
Authorization: Bearer <token>
```
    
*Réponse*
    
La même que pour « clusters dans un groupe de ressources », mais dans l’étendue d’un abonnement.

---


## <a name="update-commitment-tier-in-cluster"></a>Mettre à jour le niveau d’engagement dans le cluster

À mesure que le volume de données de vos espaces de travail liés évolue, vous souhaitez mettre à jour le niveau d’engagement en conséquence. Le niveau est spécifié en unités de Go et peut présenter des valeurs de 500, 1 000, 2 000 ou 5 000 Go/jour. Notez que vous n’avez pas besoin de fournir le corps entier de la requête REST et que vous devez inclure la référence SKU.

**INTERFACE DE LIGNE DE COMMANDE**


```azurecli
az monitor log-analytics cluster update --name "cluster-name" --resource-group "resource-group-name" --sku-capacity 500
```

### <a name="powershell"></a>PowerShell

```powershell
Update-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name" -SkuCapacity 500
```

### <a name="rest-api"></a>API REST

*Appeler*

```rest
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2021-06-01
Authorization: Bearer <token>
Content-type: application/json

{
  "sku": {
    "name": "capacityReservation",
    "Capacity": 2000
  }
}
```

---



### <a name="update-billingtype-in-cluster"></a>Mettre à jour la propriété billingType dans le cluster

La propriété *billingType* détermine l’attribution de facturation pour le cluster et ses données :
- *Cluster* (par défaut) : la facturation est attribuée à la ressource de cluster.
- *Espaces de travail* : la facturation est attribuée de façon proportionnée aux espaces de travail liés. Quand le volume de données de tous les espaces de travail est inférieur au niveau d’engagement, le volume restant est attribué au cluster.

**REST**

*Appeler*

```rest
PATCH https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2021-06-01
Authorization: Bearer <token>
Content-type: application/json

{
  "properties": {
    "billingType": "Workspaces",
    }  
}
```

### <a name="unlink-a-workspace-from-cluster"></a>Dissocier un espace de travail d’un cluster

Vous pouvez dissocier un espace de travail d’un cluster. Après la dissociation d’un espace de travail du cluster, les nouvelles données associées à cet espace de travail ne sont pas envoyées au cluster dédié. En outre, la facturation de l’espace de travail ne se fait plus via le cluster. Les anciennes données de l’espace de travail dissocié peuvent être conservées sur le cluster. Si ces données sont chiffrées à l’aide de clés gérées par le client (CMK), les secrets Key Vault sont conservés. Le système extrait cette modification des utilisateurs de Log Analytics. Les utilisateurs peuvent simplement interroger l’espace de travail comme d’habitude. Le système exécute des requêtes entre clusters sur le serveur principal le cas échéant, sans aucune indication aux utilisateurs.  

> [!WARNING] 
> Il existe une limite de deux opérations de liaison par espace de travail spécifique dans un délai d’un mois. Prenez le temps de prendre en compte et de planifier les actions de dissociation en conséquence.

**INTERFACE DE LIGNE DE COMMANDE**

```azurecli
az monitor log-analytics workspace linked-service delete --resource-group "resource-group-name" --workspace-name "workspace-name" --name cluster
```

**PowerShell**

Utilisez la commande PowerShell suivante pour dissocier un espace de travail d’un cluster :

```powershell
# Unlink a workspace from cluster
Remove-AzOperationalInsightsLinkedService -ResourceGroupName {resource-group-name} -WorkspaceName {workspace-name} -LinkedServiceName cluster
```

---




## <a name="delete-cluster"></a>Supprimer un cluster

Vous devez dissocier tous les espaces de travail d’un cluster dédié avant de le supprimer. Pour cela, vous devez disposer des autorisations d’*écriture* sur la ressource de cluster. 

Une fois la ressource de cluster supprimée, le cluster physique entre dans un processus de vidage et de suppression. La suppression d’un cluster entraîne la suppression de toutes les données qui ont été stockées sur le cluster. Les données peuvent provenir d’espaces de travail qui ont été reliés au cluster par le passé.

Une ressource de cluster supprimée au cours des 14 derniers jours est dans un état de suppression réversible. Il est possible de la récupérer avec ses données. Étant donné que tous les espaces de travail ont été dissociés de la ressource de cluster suite à la suppression de celle-ci, vous devez réassocier vos espaces de travail après la récupération. L’opération de récupération ne peut pas être effectuée par l’utilisateur. Contactez votre canal Microsoft ou le support technique pour toute demande de récupération.

Dans les 14 jours qui suivent la suppression, le nom de la ressource de cluster est réservé et ne peut pas être utilisé par d’autres ressources.

> [!WARNING] 
> Le nombre de clusters par abonnement est limité à trois. Les clusters actifs et supprimés de manière réversible sont comptabilisés dans ce cadre. Les clients ne doivent pas créer de procédures réactives qui créent et suppriment des clusters. Cela a un impact significatif sur les systèmes principaux de Log Analytics.

**PowerShell**

Utilisez la commande PowerShell suivante pour supprimer un cluster :

```powershell
Remove-AzOperationalInsightsCluster -ResourceGroupName "resource-group-name" -ClusterName "cluster-name"
```

**REST API**

Utilisez l’appel REST suivant pour supprimer un cluster :

```rest
DELETE https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.OperationalInsights/clusters/<cluster-name>?api-version=2021-06-01
Authorization: Bearer <token>
```

  **Réponse**

  200 OK

---



## <a name="limits-and-constraints"></a>Limites et contraintes

- Un maximum de deux clusters actifs peut être créé dans chaque région et abonnement.

- Un nombre maximal de quatre clusters réservés (actifs ou récemment supprimés) peut être créé dans chaque région et abonnement.

- Un maximum de 1 000 espaces de travail Log Analytics peuvent être liés à un cluster.

- Un maximum de deux opérations de liaison d’espace de travail sur un espace de travail particulier est autorisé sur une période de 30 jours.

- Le déplacement d’un cluster vers un autre groupe de ressources ou un autre abonnement n’est pas pris en charge actuellement.

- La mise à jour du cluster ne doit pas inclure les détails relatifs à l’identité et à l’identificateur de clé dans la même opération. S’il vous faut les mettre à jour, cette mise à jour doit faire l’objet de deux opérations consécutives.

- Actuellement, Lockbox n’est pas disponible en Chine. 

- Le [double chiffrement](../../storage/common/storage-service-encryption.md#doubly-encrypt-data-with-infrastructure-encryption) est automatiquement configuré pour les clusters créés depuis octobre 2020 dans les régions prises en charge. Pour vérifier si votre cluster est configuré pour le chiffrement double, envoyez une requête GET sur le cluster et observez la valeur de `isDoubleEncryptionEnabled`. Une valeur `true` correspond aux clusters pour lesquels le chiffrement double est activé. 
  - Si vous créez un cluster et recevez une erreur « region-name ne prend pas en charge le chiffrement double pour les clusters », vous pouvez toujours créer le cluster sans le double chiffrement en ajoutant `"properties": {"isDoubleEncryptionEnabled": false}` dans le corps de la demande REST.
  - Le paramètre de double chiffrement ne peut pas être modifié une fois le cluster créé.

## <a name="troubleshooting"></a>Dépannage

- Si vous recevez une erreur de conflit lors de la création d’un cluster, il est possible que vous ayez supprimé votre cluster au cours des 14 derniers jours et qu’il se trouve dans un état de suppression réversible. Le nom du cluster reste réservé pendant la période de suppression réversible et vous ne pouvez pas l’utiliser pour un autre cluster. Le nom est libéré après la période de suppression réversible, lorsque le cluster est définitivement supprimé.

- Si vous mettez à jour votre cluster alors que le cluster est en cours d’approvisionnement ou de mise à jour, la mise à jour échoue.

- Certains opérations sont longues et peuvent prendre du temps. Il s’agit des opérations de *création de cluster*, de *mise à jour de la clé du cluster* et de *suppression du cluster*. Vous pouvez vérifier l’état de l’opération de deux manières :
  - Lorsque vous utilisez REST, copiez la valeur de l’URL Azure-AsyncOperation à partir de la réponse et suivez les instructions de [vérification de l’état des opérations asynchrones](#asynchronous-operations-and-status-check).
  - Envoyez une requête GET au cluster ou à l’espace de travail du cluster et observez la réponse. Par exemple, l’espace de travail dissocié n’a pas de *clusterResourceId* sous *features*.

- La liaison d’un espace de travail à un cluster échoue s’il est lié à un autre cluster.

## <a name="error-messages"></a>Messages d’erreur
  
### <a name="cluster-create"></a>Création de cluster

-  400 -- Le nom du cluster n’est pas valide. Le nom du cluster peut contenir les caractères a-z, A-Z, 0-9 et doit compter entre 3 et 63 caractères.
-  400 -- Le corps de la demande a la valeur null ou est dans un format incorrect.
-  400 -- Nom de SKU non valide. Définissez le nom de la SKU sur capacityReservation.
-  400 -- La capacité a été fournie, mais la référence SKU n’est pas capacityReservation. Définissez le nom de la SKU sur capacityReservation.
-  400 -- Xapacité manquante dans la SKU. Définissez la valeur de capacité sur 500, 1 000, 2 000 ou 5 000 Go/jour.
-  400 -- La capacité est verrouillée pendant 30 jours. La réduction de la capacité est autorisée 30 jours après la mise à jour.
-  400 -- Aucune SKU n’a été définie. Définissez le nom du niveau tarifaire sur capacityReservation et la valeur de capacité sur 500, 1 000, 2 000 ou 5 000 Go/jour.
-  400 -- L’identité a la valeur nul ou est vide. Définissez l’identité avec le type systemAssigned.
-  400 -- Les KeyVaultProperties sont définies à la création. Mettez à jour les KeyVaultProperties après la création du cluster.
-  400 -- Impossible d’exécuter une opération pour le moment. L’opération asynchrone est dans un état autre que réussi. Le cluster doit effectuer cette opération avant l’exécution d’une opération de mise à jour.

### <a name="cluster-update"></a>Mise à jour d’un cluster

-  400 -- Le cluster est en cours de suppression. L’opération asynchrone est en cours. Le cluster doit effectuer cette opération avant l’exécution d’une opération de mise à jour.
-  400 -- Les KeyVaultProperties ne sont pas vides, mais leur format est incorrect. Consultez [mise à jour de l’identificateur de la clé](../logs/customer-managed-keys.md#update-cluster-with-key-identifier-details).
-  400 -- Échec de validation de la clé dans Key Vault. Peut être dû à un manque d’autorisations ou à l’inexistence de la clé. Vérifiez que vous [avez défini la clé et la stratégie d’accès](../logs/customer-managed-keys.md#grant-key-vault-permissions) dans Key Vault.
-  400 -- La clé n’est pas récupérable. La suppression réversible et la protection contre le vidage doivent être définis pour Key Vault. Consulter la [documentation sur Key Vault](../../key-vault/general/soft-delete-overview.md)
-  400 -- Impossible d’exécuter une opération pour le moment. Attendez que l’opération asynchrone se termine et réessayez.
-  400 -- Le cluster est en cours de suppression. Attendez que l’opération asynchrone se termine et réessayez.

### <a name="cluster-get"></a>Obtention de cluster

 -  404 -- Cluster introuvable, le cluster a peut-être été supprimé. Si vous essayez de créer un cluster portant ce nom et que cela génère un conflit, le cluster est supprimé de manière réversible pendant 14 jours. Vous pouvez contacter le support technique pour le récupérer ou utiliser un autre nom pour créer un nouveau cluster. 

### <a name="cluster-delete"></a>Suppression de cluster

 -  409 -- Impossible de supprimer un cluster en état d’approvisionnement. Attendez que l’opération asynchrone se termine et réessayez.

### <a name="workspace-link"></a>Liaison d’un espace de travail

-  404 -- Espace de travail introuvable. L’espace de travail que vous avez spécifié n’existe pas ou a été supprimé.
-  409 -- Opération de liaison ou de dissociation d’espace de travail en cours.
-  400 -- Cluster introuvable, le cluster que vous avez spécifié n’existe pas ou a été supprimé. Si vous essayez de créer un cluster portant ce nom et que cela génère un conflit, le cluster est supprimé de manière réversible pendant 14 jours. Contactez le support technique pour le récupérer.

### <a name="workspace-unlink"></a>Dissociation d’un espace de travail
-  404 -- Espace de travail introuvable. L’espace de travail que vous avez spécifié n’existe pas ou a été supprimé.
-  409 -- Opération de liaison ou de dissociation d’espace de travail en cours.

## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur la [facturation des clusters dédiés Log Analytics](./manage-cost-storage.md#log-analytics-dedicated-clusters)
- En savoir plus sur la [conception appropriée des espaces de travail Log Analytics](../logs/design-logs-deployment.md)
