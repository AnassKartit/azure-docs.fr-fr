---
title: Créer des abonnements Azure pour un Contrat Partenaire Microsoft programmatiquement avec les API les plus récentes
description: Découvrez comment créer des abonnements Azure pour un Contrat Partenaire Microsoft programmatiquement en utilisant les versions les plus récentes de l’API REST, d’Azure CLI, d’Azure PowerShell et des modèles Azure Resource Manager.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 06/22/2021
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 6db488449ad54957ae71f9c53c1a26bda25c6db1
ms.sourcegitcommit: 47fac4a88c6e23fb2aee8ebb093f15d8b19819ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2021
ms.locfileid: "122965957"
---
# <a name="programmatically-create-azure-subscriptions-for-a-microsoft-partner-agreement-with-the-latest-apis"></a>Créer des abonnements Azure pour un Contrat Partenaire Microsoft programmatiquement avec les API les plus récentes

Cet article vous aide à créer des abonnements Azure pour un Contrat Partenaire Microsoft programmatiquement à l’aide des versions les plus récentes des API. Si vous utilisez toujours la préversion, consultez [Créer des abonnements Azure programmatiquement avec des API héritées](programmatically-create-subscription-preview.md). 

Dans cet article, vous apprenez à créer des abonnements par programmation en utilisant Azure Resource Manager.

Quand vous créez un abonnement Azure par programmation, cet abonnement est régi par le contrat applicable à votre achat de services Azure auprès de Microsoft ou d’un revendeur agréé. Pour plus d’informations, consultez [Informations légales relatives à Microsoft Azure](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Prérequis

Vous devez avoir un rôle Administrateur général ou Agent d’administration sur le compte du fournisseur de solutions Cloud dans votre organisation pour créer un abonnement associé à votre compte de facturation. Pour plus d’informations, consultez [Espace partenaires - Affecter des rôles et des autorisations aux utilisateurs](/partner-center/permissions-overview).

Si vous ne savez pas si vous avez accès à un compte Contrat Partenaire Microsoft, consultez [Vérifier l’accès à un Contrat Partenaire Microsoft](mpa-request-ownership.md#check-access-to-a-microsoft-partner-agreement).

Les exemples suivants utilisent les API REST. Actuellement, PowerShell et Azure CLI ne sont pas pris en charge.

## <a name="find-the-billing-accounts-that-you-have-access-to"></a>Rechercher les comptes de facturation auxquels vous avez accès

Exécutez la requête suivante pour lister tous les comptes de facturation auxquels vous avez accès.

### <a name="rest"></a>[REST](#tab/rest)

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingaccounts/?api-version=2020-05-01
```

En réponse, l’API retourne la liste des comptes de facturation.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountStatus": "Active",
        "accountType": "Partner",
        "agreementType": "MicrosoftPartnerAgreement",
        "billingProfiles": {
          "hasMoreResults": false
        },
        "displayName": "Contoso",
        "hasReadAccess": true
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}
```

Utilisez la propriété `displayName` afin d’identifier le compte de facturation pour lequel vous souhaitez créer des abonnements. Vérifiez que le compte affiche *MicrosoftPartnerAgreement* comme agreementType. Copiez la valeur `name` du compte. Par exemple, si vous souhaitez créer un abonnement pour le compte de facturation `Contoso`, copiez `99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx`. Collez cette valeur quelque part, de façon à pouvoir l’utiliser à l’étape suivante.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Utilisez Azure CLI ou l’API REST pour obtenir cette valeur.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az billing account list
```
Vous obtiendrez une liste de tous les comptes de facturation auxquels vous avez accès.

```json
[
  {
    "accountStatus": "Active",
    "accountType": "Partner",
    "agreementType": "MicrosoftPartnerAgreement",
    "billingProfiles": {
      "hasMoreResults": false,
      "value": null
    },
    "departments": null,
    "displayName": "Contoso",
    "enrollmentAccounts": null,
    "enrollmentDetails": null,
    "hasReadAccess": true,
    "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
    "name": "99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
    "soldTo": null,
    "type": "Microsoft.Billing/billingAccounts"
  }
]
```

Utilisez la propriété displayName afin d’identifier le compte de facturation pour lequel vous souhaitez créer des abonnements. Vérifiez que le compte affiche MicrosoftPartnerAgreement comme agreementType. Copiez le nom du compte. Par exemple, pour créer un abonnement pour le compte de facturation Contoso, copiez 99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx. Collez cette valeur quelque part, de façon à pouvoir l’utiliser à l’étape suivante.

---

## <a name="find-customers-that-have-azure-plans"></a>Rechercher les clients qui ont des plans Azure

Exécutez la requête suivante avec la valeur `name`que vous avez copiée à la première étape (```99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx```) pour lister tous les clients associés au compte de facturation pour lequel vous pouvez créer des abonnements Azure.

### <a name="rest"></a>[REST](#tab/rest)

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers?api-version=2020-05-01
```

En réponse, l’API retourne la liste des clients du compte de facturation qui ont des plans Azure. Vous pouvez créer des abonnements pour ces clients.

```json
{
  "totalCount": 2,
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "properties": {
        "billingProfileDisplayName": "Fabrikam toys Billing Profile",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/YL4M-xxxx-xxx-xxx",
        "displayName": "Fabrikam toys"
      },
      "type": "Microsoft.Billing/billingAccounts/customers"
    },
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/acba85c9-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "acba85c9-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "properties": {
        "billingProfileDisplayName": "Contoso toys Billing Profile",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/YL4M-xxxx-xxx-xxx",
        "displayName": "Contoso toys"
      },
      "type": "Microsoft.Billing/billingAccounts/customers"
    }
  ]
}

```

Utilisez la propriété `displayName` afin d’identifier le client pour lequel vous souhaitez créer des abonnements. Copiez la valeur `id` du client. Par exemple, si vous souhaitez créer un abonnement pour `Fabrikam toys`, copiez `/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx`. Collez cette valeur quelque part de façon à pouvoir l’utiliser dans les étapes suivantes.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Utilisez Azure CLI ou l’API REST pour obtenir cette valeur.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az billing customer list --account-name 99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx
```

En réponse, l’API retourne la liste des clients du compte de facturation qui ont des plans Azure. Vous pouvez créer des abonnements pour ces clients.

```json
[
  {
    "billingProfileDisplayName": "Fabrikam toys Billing Profile",
    "billingProfileId": "providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "displayName": "Fabrikam toys",
    "id": "providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "acba85c9-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "resellers": null,
    "type": "Microsoft.Billing/billingAccounts/customers"
  },
  {
    "billingProfileDisplayName": "Contoso toys Billing Profile",
    "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/acba85c9-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "displayName": "Contoso toys",
    "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/acba85c9-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "d49c364c-f866-4cc2-a284-d89f369b7951",
    "resellers": null,
    "type": "Microsoft.Billing/billingAccounts/customers"
  }
]

```

Utilisez la propriété `displayName` afin d’identifier le client pour lequel vous souhaitez créer des abonnements. Copiez la valeur `id` du client. Par exemple, si vous souhaitez créer un abonnement pour `Fabrikam toys`, copiez `/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx`. Collez cette valeur quelque part de façon à pouvoir l’utiliser dans les étapes suivantes.

---

## <a name="get-the-resellers-for-a-customer"></a>Obtenir les revendeurs d’un client

Cette section est facultative pour les fournisseurs indirects uniquement.

Si vous avez été ajouté comme fournisseur indirect dans le modèle CSP de niveau deux, vous pouvez spécifier un revendeur au moment de la création des abonnements pour les clients.

### <a name="rest"></a>[REST](#tab/rest)

Exécutez la requête suivante avec la valeur `id` que vous avez copiée à la deuxième étape (```/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) afin de lister tous les revendeurs disponibles pour un client.

```json
GET "https://management.azure.com/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx?$expand=resellers&api-version=2020-05-01"
```
En réponse, l’API retourne la liste des revendeurs pour le client :

```json
{
  "value": [{
  "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.Billing/billingAccounts/customers",
  "properties": {
    "billingProfileDisplayName": "Fabrikam toys Billing Profile",
    "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/YL4M-xxxx-xxx-xxx",
    "displayName": "Fabrikam toys",
    "resellers": [
      {
        "resellerId": "3xxxxx",
        "description": "Wingtip"
      }
    ]
  }
}]
}
```

Utilisez la propriété `description` afin d’identifier le revendeur qui est associé à l’abonnement. Copiez la valeur `resellerId` du revendeur. Par exemple, pour associer `Wingtip`, copiez `3xxxxx`. Collez cette valeur quelque part, de façon à pouvoir l’utiliser à l’étape suivante.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Utilisez Azure CLI ou l’API REST pour obtenir cette valeur.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Effectuez la requête suivante, avec le paramètre `name` copié de la première étape (```99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx```) et le paramètre `name` du client copié à l’étape précédente (```acba85c9-xxxx-xxxx-xxxx-xxxxxxxxxxxx```).

```azurecli
 az billing customer show --expand "enabledAzurePlans,resellers" --account-name "99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx" --name "acba85c9-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

En réponse, l’API retourne la liste des revendeurs pour le client :

```json
{
  "billingProfileDisplayName": "Fabrikam toys Billing Profile",
  "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/YL4M-xxxx-xxx-xxx",
  "displayName": "Fabrikam toys",
  "enabledAzurePlans": [
    {
      "skuDescription": "Microsoft Azure Plan",
      "skuId": "0001"
    }
  ],
  "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "resellers": [
    {
      "description": "Wingtip",
      "resellerId": "3xxxxx"
    }
  ],
  "type": "Microsoft.Billing/billingAccounts/customers"
}

```

Utilisez la propriété `description` afin d’identifier le revendeur qui est associé à l’abonnement. Copiez la valeur `resellerId` du revendeur. Par exemple, pour associer `Wingtip`, copiez `3xxxxx`. Collez cette valeur quelque part, de façon à pouvoir l’utiliser à l’étape suivante.

---

## <a name="create-a-subscription-for-a-customer"></a>Créer un abonnement pour un client

L’exemple suivant crée un abonnement nommé *Dev Team subscription* pour *Fabrikam toys* et associe le revendeur *Wingtip* à l’abonnement. Utilisez l’étendue de facturation copiée à l’étape précédente : `/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx`. 

### <a name="rest"></a>[REST](#tab/rest)

```json
PUT  https://management.azure.com/providers/Microsoft.Subscription/aliases/sampleAlias?api-version=2020-09-01
```

### <a name="request-body"></a>Corps de la demande

```json
{
  "properties":
    {
        "billingScope": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "DisplayName": "Dev Team subscription",
        "Workload": "Production"
    }
}
```
### <a name="response"></a>response

```json
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "type": "Microsoft.Subscription/aliases",
  "properties": {
    "subscriptionId": "b5bab918-e8a9-4c34-a2e2-ebc1b75b9d74",
    "provisioningState": "Accepted"
  }
}
```

Vous pouvez exécuter une opération GET sur la même URL pour obtenir l’état de la requête.

### <a name="request"></a>Requête

```json
GET https://management.azure.com/providers/Microsoft.Subscription/aliases/sampleAlias?api-version=2020-09-01
```

### <a name="response"></a>response

```json
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "type": "Microsoft.Subscription/aliases",
  "properties": {
    "subscriptionId": "b5bab918-e8a9-4c34-a2e2-ebc1b75b9d74",
    "provisioningState": "Succeeded"
  }
}
```

L’état En cours est retourné comme un état `Accepted` sous `provisioningState`. 

Passez la valeur *resellerId* facultative, que vous avez copiée à la deuxième étape dans le corps de requête de l’API.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Pour installer la version la plus récente du module contenant la cmdlet `New-AzSubscriptionAlias`, exécutez `Install-Module Az.Subscription`. Pour installer une version récente de PowerShellGet, consultez [Obtenir le module PowerShellGet](/powershell/scripting/gallery/installing-psget).

Exécutez la commande New-AzSubscriptionAlias suivante à l’aide de l’étendue de facturation `"/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx"`. 

```azurepowershell
New-AzSubscriptionAlias -AliasName "sampleAlias" -SubscriptionName "Dev Team Subscription" -BillingScope "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx" -Workload 'Production"
```

Vous recevez le subscriptionId dans la réponse à la commande.

```json
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "properties": {
    "provisioningState": "Succeeded",
    "subscriptionId": "4921139b-ef1e-4370-a331-dd2229f4f510"
  },
  "type": "Microsoft.Subscription/aliases"
}
```

Passez la valeur *resellerId* facultative, que vous avez copiée à la deuxième étape dans l’appel `New-AzSubscriptionAlias`.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Commencez par installer cette extension en exécutant `az extension add --name account` et `az extension add --name alias`.

Exécutez la commande [az account alias create](/cli/azure/account/alias#az_account_alias_create) suivante. 

```azurecli
az account alias create --name "sampleAlias" --billing-scope "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx" --display-name "Dev Team Subscription" --workload "Production"
```

Vous recevez le subscriptionId dans la réponse à la commande.

```json
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "properties": {
    "provisioningState": "Succeeded",
    "subscriptionId": "4921139b-ef1e-4370-a331-dd2229f4f510"
  },
  "type": "Microsoft.Subscription/aliases"
}
```

Passez la valeur *resellerId* facultative, que vous avez copiée à la deuxième étape dans l’appel `az account alias create`.

---

## <a name="use-arm-template"></a>Utiliser un modèle ARM

La section précédente vous a montré comment créer un abonnement avec PowerShell, Azure CLI ou l’API REST. Si vous devez automatiser la création des abonnements, utilisez plutôt un modèle ARM (Azure Resource Manager).

Le modèle suivant permet de créer un abonnement. Pour `billingScope`, indiquez l’ID client. L’abonnement est créé dans le groupe d’administration racine. Après avoir créé l’abonnement, vous pouvez le déplacer vers un autre groupe d’administration.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "subscriptionAliasName": {
            "type": "string",
            "metadata": {
                "description": "Provide a name for the alias. This name will also be the display name of the subscription."
            }
        },
        "billingScope": {
            "type": "string",
            "metadata": {
                "description": "Provide the full resource ID of billing scope to use for subscription creation."
            }
        }
    },
    "resources": [
        {
            "scope": "/", 
            "name": "[parameters('subscriptionAliasName')]",
            "type": "Microsoft.Subscription/aliases",
            "apiVersion": "2020-09-01",
            "properties": {
                "workLoad": "Production",
                "displayName": "[parameters('subscriptionAliasName')]",
                "billingScope": "[parameters('billingScope')]"
            }
        }
    ],
    "outputs": {}
}
```

Déployez le modèle au [niveau du groupe d’administration](../../azure-resource-manager/templates/deploy-to-management-group.md).

### <a name="rest"></a>[REST](#tab/rest)

```json
PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/mg1/providers/Microsoft.Resources/deployments/exampledeployment?api-version=2020-06-01
```

Avec le corps de la demande :

```json
{
  "location": "eastus",
  "properties": {
    "templateLink": {
      "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json"
    },
    "parameters": {
      "subscriptionAliasName": {
        "value": "sampleAlias"
      },
      "billingScope": {
        "value": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      }
    },
    "mode": "Incremental"
  }
}
```

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
New-AzManagementGroupDeployment `
  -Name exampledeployment `
  -Location eastus `
  -ManagementGroupId mg1 `
  -TemplateFile azuredeploy.json `
  -subscriptionAliasName sampleAlias `
  -billingScope "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli
az deployment mg create \
  --name exampledeployment \
  --location eastus \
  --management-group-id mg1 \
  --template-file azuredeploy.json \
  --parameters subscriptionAliasName='sampleAlias' billingScope='/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx'
```

---

Pour déplacer un abonnement vers un nouveau groupe d’administration, utilisez le modèle suivant.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-08-01/managementGroupDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "targetMgId": {
            "type": "string",
            "metadata": {
                "description": "Provide the ID of the management group that you want to move the subscription to."
            }
        },
        "subscriptionId": {
            "type": "string",
            "metadata": {
                "description": "Provide the ID of the existing subscription to move."
            }
        }
    },
    "resources": [
        {
            "scope": "/",
            "type": "Microsoft.Management/managementGroups/subscriptions",
            "apiVersion": "2020-05-01",
            "name": "[concat(parameters('targetMgId'), '/', parameters('subscriptionId'))]",
            "properties": {
            }
        }
    ],
    "outputs": {}
}
```

## <a name="next-steps"></a>Étapes suivantes

* Maintenant que vous avez créé un abonnement, vous pouvez accorder cette capacité à d’autres utilisateurs et principaux de service. Pour plus d’informations, consultez [Accorder l’accès pour créer des abonnements Azure Enterprise (préversion)](grant-access-to-create-subscription.md).
* Pour plus d’informations sur la gestion d’un grand nombre d’abonnements à l’aide de groupes d’administration, consultez [Organiser vos ressources avec des groupes d’administration Azure](../../governance/management-groups/overview.md).
