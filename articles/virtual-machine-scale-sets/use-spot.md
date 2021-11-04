---
title: Créer un groupe identique qui utilise des machines virtuelles Azure Spot
description: Découvrez comment créer des groupes identiques de machines virtuelles Azure qui utilisent des machines virtuelles Azure Spot pour réaliser des économies sur les coûts.
author: mimckitt
ms.author: mimckitt
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: spot
ms.date: 10/22/2021
ms.reviewer: cynthn
ms.custom: devx-track-azurecli, devx-track-azurepowershell
ms.openlocfilehash: 842394ed341da88fdb37ff6deb7ccdc519ac2f8e
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131060501"
---
# <a name="azure-spot-virtual-machines-for-virtual-machine-scale-sets"></a>Machines virtuelles Azure Spot et groupes de machines virtuelles identiques 

**S’applique à :** :heavy_check_mark: Machines virtuelles Linux :heavy_check_mark: Machines virtuelles Windows :heavy_check_mark: Groupes identiques flexibles :heavy_check_mark: Groupes identiques uniformes

L’utilisation de machines virtuelles Azure Spot sur des groupes identiques vous permet de disposer de notre capacité inutilisée en réalisant des économies significatives. Dès qu’Azure aura besoin de récupérer la capacité, l’infrastructure Azure supprimera les instances de machine virtuelle Azure Spot. Les instances de machine virtuelle Azure Spot sont donc appropriées pour les charges de travail capables de gérer les interruptions, comme les travaux de traitement par lots, les environnements de développement et de test, les charges de travail de calcul importantes, entre autres.

La capacité disponible dépend de divers facteurs, tels que la taille, la région, l’heure, etc. Lors du déploiement d’instances de machine virtuelle Azure Spot sur des groupes identiques, Azure alloue l’instance uniquement si la capacité est disponible, mais qu’il n’existe aucun contrat SLA pour ces instances. Un groupe de machines virtuelles identiques Azure Spot est déployé dans un domaine d’erreur unique. Il n’offre aucune garantie de haute disponibilité.

## <a name="limitations"></a>Limites

Les tailles suivantes ne sont pas prises en charge pour les machines virtuelles Azure Spot :
 - Série B
 - Versions promotionnelles de toutes les tailles (Dv2, NV, NC, H, etc.)

Une machine virtuelle Azure Spot peut être déployée dans n’importe quelle région, à l’exception de Microsoft Azure China 21Vianet.

Les [types d’offres](https://azure.microsoft.com/support/legal/offer-details/) suivants sont pris en charge :

-   Contrat Entreprise
-   Code de l’offre de paiement à l’utilisation (003P)
-   Sponsorisé (0036P et 0136P)
- Pour le fournisseur de services cloud (CSP), consultez l’[Espace partenaires](/partner-center/azure-plan-get-started) ou contactez directement votre partenaire.

## <a name="pricing"></a>Tarifs

Les tarifs des instances de machine virtuelle Azure Spot sont variables, en fonction de la région et de la référence SKU. Pour plus d’informations, consultez les prix pour [Linux](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) et [Windows](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/windows/). 


En raison de la variabilité des tarifs, vous avez la possibilité de définir un prix maximal en dollars américains (USD) ayant jusqu’à cinq décimales. Par exemple, la valeur `0.98765` correspond à un prix maximal de 0,98765 $ USD par heure. Si vous définissez `-1` comme prix maximal, l’instance n’est pas supprimée en fonction du prix. Le prix de l’instance sera le prix actuel de la machine virtuelle Azure Spot ou le prix d’une instance standard, la valeur la plus faible étant retenue, à condition que la capacité et le quota soient disponibles.



## <a name="eviction-policy"></a>Stratégie d’éviction

Lorsque vous créez un groupe identique à l’aide de machines virtuelles spot Azure, vous pouvez affecter à la stratégie d’éviction la valeur *Libérer* (par défaut) ou *Supprimer*. 

La stratégie *Libérer* affecte à vos instances écartées l’état « arrêté-libéré », ce qui vous permet de redéployer les instances écartées. Toutefois, la réussite de l’allocation n’est pas garantie. Les machines virtuelles libérées sont comptabilisées dans votre quota d’instances de groupe identique, et vos disques sous-jacents vous sont facturés. 

Si vous souhaitez que les instances soient supprimées après avoir été écartées, affectez à la stratégie d’éviction la valeur *Supprimer*. Avec cette configuration, vous pouvez créer d’autres machines virtuelles en définissant la propriété du nombre d’instances du groupe identique à une valeur plus grande. Les machines virtuelles évincées sont supprimées en même temps que leurs disques sous-jacents. Vous n’êtes donc pas facturé pour le stockage. Vous pouvez également utiliser la fonctionnalité de mise à l’échelle automatique des groupes identiques pour essayer et compenser automatiquement les machines virtuelles évincées, mais sans garantie de réussite de l’allocation. Nous vous recommandons d’utiliser uniquement la mise à l’échelle automatique des groupes de machines virtuelles identiques Azure Spot lorsque vous définissez la stratégie d’éviction avec suppression pour éviter la facturation de vos disques et le dépassement de vos limites de quota. 

Les utilisateurs peuvent s’abonner pour recevoir des notifications dans la machine virtuelle via [Azure Scheduled Events](../virtual-machines/linux/scheduled-events.md). Vous serez ainsi informé si vos machines virtuelles sont en cours d’éviction, et vous aurez 30 secondes pour terminer vos tâches et arrêter la machine virtuelle avant que ne commence l’éviction. 

## <a name="eviction-history"></a>Historique d’éviction
Vous pouvez voir l’historique des tarifs et des taux d’éviction par taille dans une région du portail. Sélectionnez **Voir l'historique des prix et comparer les prix dans les régions proches** pour afficher une table ou un graphique de tarification pour une taille spécifique.  Les tarifs et les taux d’éviction des images suivantes sont uniquement des exemples. 

**Graphique**:

:::image type="content" source="../virtual-machines/media/spot-chart.png" alt-text="Capture d’écran des options de région avec la différence de tarification et les taux d’éviction sous forme de graphique.":::

**Table**:

:::image type="content" source="../virtual-machines/media/spot-table.png" alt-text="Capture d’écran des options de région avec la différence de tarification et les taux d’éviction sous forme de table.":::

## <a name="try--restore"></a>Essayer & restaurer 

Cette fonctionnalité au niveau de la plateforme utilise l’intelligence artificielle pour essayer automatiquement de restaurer les instances de machine virtuelle spot Azure écartées à l’intérieur d’un groupe identique afin de conserver le nombre d’instances cibles. 

Avantages de la fonctionnalité Essayer et restaurer :
- Tentatives de restauration des machines virtuelles spot Azure écartées en raison de la capacité.
- Les machines virtuelles spot Azure restaurées sont censées s’exécuter pendant une durée plus longue, avec une probabilité inférieure d’une capacité d’éviction déclenchée.
- Améliore la durée de vie d’une machine virtuelle spot Azure, de sorte que les charges de travail s’exécutent pendant une durée plus longue.
- Aide les groupes de machines virtuelles identiques à maintenir le nombre de cibles pour les machines virtuelles spot Azure, similaire à la fonctionnalité de maintien du nombre de cibles qui existe déjà pour les machines virtuelles avec paiement à l’utilisation.

La fonctionnalité Essayer et restaurer est désactivée dans les groupes identiques qui utilisent la [mise à l’échelle automatique](virtual-machine-scale-sets-autoscale-overview.md). Le nombre de machines virtuelles dans le groupe identique est piloté par les règles de mise à l’échelle automatique.


## <a name="placement-groups"></a>Groupes de placement

Un groupe de placement est une construction similaire à un groupe à haute disponibilité Azure, avec ses propres domaines d’erreur et domaines de mise à niveau. Par défaut, un groupe identique se compose d’un seul groupe de placement contenant au maximum 100 machines virtuelles. Si la propriété de groupe identique appelée `singlePlacementGroup` est définie sur *false*, le groupe identique peut se composer de plusieurs groupes de placement et présente une plage de 0 à 1 000 machines virtuelles. 

> [!IMPORTANT]
> À moins que vous n’utilisiez InfiniBand avec HPC, il est fortement recommandé de définir la propriété de groupe identique `singlePlacementGroup` sur *false* pour activer plusieurs groupes de placement et améliorer la mise à l’échelle dans la région ou la zone. 

## <a name="deploying-azure-spot-virtual-machines-in-scale-sets"></a>Déploiement de machines virtuelles Azure spot dans des groupes identiques

Pour déployer des machines virtuelles Azure Spot dans des groupes identiques, définissez le nouvel indicateur *Priority* sur *Spot*. Toutes les machines virtuelles dans votre groupe identique sont alors configurées sur Spot. Pour créer un groupe identique avec des machines virtuelles Azure Spot, utilisez l’une des méthodes suivantes :
- [Azure portal](#portal)
- [Azure CLI](#azure-cli)
- [Azure PowerShell](#powershell)
- [Modèles Microsoft Azure Resource Manager](#resource-manager-templates)

## <a name="portal"></a>Portail

Le processus de création d’un groupe identique qui utilise des machines virtuelles Azure Spot est le même que celui décrit dans l’[article Bien démarrer](quick-create-portal.md). Lorsque vous déployez un groupe identique, vous pouvez choisir de définir l’indicateur Spot, le type d’éviction, la stratégie d’éviction et si vous souhaitez activer la tentative de restauration des instances : ![Créer un groupe identique avec Azure Spot Virtual Machines](media/virtual-machine-scale-sets-use-spot/vmss-spot-portal-1.png)


## <a name="azure-cli"></a>Azure CLI

Le processus de création d’un groupe identique avec des machines virtuelles Azure Spot est le même que celui décrit dans l’[article Bien démarrer](quick-create-cli.md). Ajoutez simplement « --Priority Spot » et `--max-price`. Dans cet exemple, nous utilisons `-1` pour `--max-price` afin que l’instance ne soit pas supprimée en fonction du prix.

```azurecli
az vmss create \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --single-placement-group false \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --eviction-policy Deallocate \
    --max-price -1 \
    --enable-spot-restore True \
    --spot-restore-timeout PT1H
```

## <a name="powershell"></a>PowerShell

Le processus de création d’un groupe identique avec des machines virtuelles Azure Spot est le même que celui décrit dans l’[article Bien démarrer](quick-create-powershell.md).
Ajoutez simplement « -Priority Spot » et fournissez une valeur `-max-price` pour [New-AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig).

```powershell
$vmssConfig = New-AzVmssConfig `
    -Location "East US 2" `
    -SkuCapacity 2 `
    -SkuName "Standard_DS2" `
    -UpgradePolicyMode Automatic `
    -Priority "Spot" `
    -max-price -1 `
    -EnableSpotRestore `
    -SpotRestoreTimeout 60 `
    -EvictionPolicy delete
```

## <a name="resource-manager-templates"></a>Modèles Resource Manager

Le processus de création d’un groupe identique qui utilise des machines virtuelles Azure Spot est le même que celui décrit dans l’article Bien démarrer pour [Linux](quick-create-template-linux.md) ou [Windows](quick-create-template-windows.md). 

Pour le déploiement de modèles de machine virtuelle Azure Spot, utilisez`"apiVersion": "2019-03-01"` ou une version ultérieure. 

Ajoutez les propriétés `priority`, `evictionPolicy`, `billingProfile` et `spotRestoryPolicy` à la section `"virtualMachineProfile":` et la propriété `"singlePlacementGroup": false,` à la section `"Microsoft.Compute/virtualMachineScaleSets"` de votre modèle :

```json

{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  },
  "properties": {
    "singlePlacementGroup": false,
    }

        "virtualMachineProfile": {
              "priority": "Spot",
                "evictionPolicy": "Deallocate",
                "billingProfile": {
                    "maxPrice": -1
                },
                "spotRestorePolicy": {
                  "enabled": "bool",
                  "restoreTimeout": "string"
    },
            },
```

Pour supprimer l’instance après son exclusion, remplacez le paramètre `evictionPolicy` par `Delete`.


## <a name="simulate-an-eviction"></a>Simuler une éviction

Vous pouvez [simuler l’éviction](/rest/api/compute/virtualmachines/simulateeviction) d’une machine virtuelle spot Azure afin de tester l’efficacité de la réponse de votre application à une éviction soudaine. 

Remplacez les éléments suivants par vos informations : 

- `subscriptionId`
- `resourceGroupName`
- `vmName`


```rest
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/simulateEviction?api-version=2020-06-01
```

`Response Code: 204` signifie que l’éviction simulée a réussi. 

Pour plus d’informations, consultez [Test d’une notification d’éviction simulée](../virtual-machines/windows/spot-powershell.md#simulate-an-eviction).

## <a name="faq"></a>Questions fréquentes (FAQ)

**Q :** Une fois créée, l’instance de machine virtuelle Azure Spot est-elle identique à l’instance standard ?

**R :** Oui, sauf qu’il n’existe aucun contrat SLA pour les machines virtuelles Azure Spot et qu’elles peuvent être supprimées à tout moment.


**Q :** Que faire quand votre machine virtuelle est supprimée alors que vous avez encore besoin de capacité ?

**R :** Nous vous recommandons d’utiliser des machines virtuelles standard au lieu de machines virtuelles Azure Spot si vous avez besoin de capacité immédiatement.


**Q :** Comment est géré le quota pour les machines virtuelles Azure Spot ?

**R :** Les instances de machine virtuelle Azure Spot et les instances standard auront des pools de quotas distincts. Le quota de machine virtuelle Azure Spot est partagé entre les machines virtuelles et les instances de groupe identique. Pour plus d’informations, consultez [Abonnement Azure et limites, quotas et contraintes de service](../azure-resource-manager/management/azure-subscription-service-limits.md).


**Q :** Puis-je demander un quota supplémentaire pour une machine virtuelle Azure spot ?

**R :** Oui, vous pouvez demander une augmentation de votre quota pour les machines virtuelles Azure Spot via la [procédure de demande de quota standard](../azure-portal/supportability/per-vm-quota-requests.md).


**Q :** Puis-je convertir des groupes identiques existants en groupes de machines virtuelles identiques Azure Spot ?

**R :** Non. La définition de l’indicateur `Spot` n’est prise en charge qu’au moment de la création.


**Q :** Si j’utilisais `low` pour les groupes identiques basse priorité, dois-je commencer à utiliser `Spot` à la place ?

**R :** Pour le moment, `low` et `Spot` fonctionnent, mais vous devez commencer à passer à l’utilisation de `Spot`.


**Q :** Puis-je créer un groupe identique avec des machines virtuelles normales et des machines virtuelles Azure Spot ?

**R :** Non. Un groupe identique ne prend en charge qu’un seul type de priorité.


**Q :**  Puis-je utiliser la mise à l’échelle automatique avec des groupes de machines virtuelles identiques Azure Spot ?

**R :** Oui. Vous pouvez définir des règles de mise à l’échelle automatique sur votre groupe de machines virtuelles identiques Azure Spot. Si vos machines virtuelles sont supprimées, la mise à l’échelle automatique peut essayer de créer des machines virtuelles Azure Spot. N’oubliez pas que cette fonctionnalité n’est pas garantie. 


**Q :**  La mise à l’échelle automatique fonctionne-t-elle avec les deux stratégies d’éviction (Libérer et Supprimer) ?

**R :** Oui. Toutefois, il est recommandé de choisir la stratégie d’éviction Supprimer avec la mise à l’échelle automatique. En effet, le nombre d’instances libérées est soustrait de la capacité sur le groupe identique. Quand vous utilisez la mise à l’échelle automatique, le nombre d’instances cibles est souvent rapidement atteint en raison des instances libérées et écartées. En outre, vos opérations de mise à l’échelle peuvent être affectées par les évictions Spot. Par exemple, les instances de groupe de machines virtuelles identiques peuvent tomber sous le nombre minimal défini en raison de plusieurs évictions Spot au cours des opérations de mise à l’échelle. 


**Q :** Où puis-je poster des questions ?

**R :** Vous pouvez poster et étiqueter vos questions avec `azure-spot` sur [Questions et réponses](/answers/topics/azure-spot.html). 

## <a name="next-steps"></a>Étapes suivantes

Consultez la page [Tarification des groupes identiques de machines virtuelles](https://azure.microsoft.com/pricing/details/virtual-machine-scale-sets/linux/) pour connaître les tarifs appliqués.
