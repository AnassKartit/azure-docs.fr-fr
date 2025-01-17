---
title: Utiliser un coffre de clés lors du déploiement d’une application gérée
description: Montre comment utiliser les secrets d’accès dans Azure Key Vault lors du déploiement d’applications managées
author: tfitzmac
ms.custom: subject-rbac-steps
ms.topic: conceptual
ms.date: 08/16/2021
ms.author: tomfitz
ms.openlocfilehash: d5a6dc1de2ee574a69b8dab746a24bcf82b44d64
ms.sourcegitcommit: 05dd6452632e00645ec0716a5943c7ac6c9bec7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2021
ms.locfileid: "122525948"
---
# <a name="access-key-vault-secret-when-deploying-azure-managed-applications"></a>Accéder à un secret dans le coffre de clés lors du déploiement d’applications managées Azure

Lorsque vous avez besoin de passer une valeur sécurisée (par exemple, un mot de passe) comme paramètre au cours du déploiement, vous pouvez récupérer la valeur à partir d’un coffre [Azure Key Vault](../../key-vault/general/overview.md). Pour accéder au coffre de clés lors du déploiement d’applications managées, vous devez accorder l’accès au principal de service du **fournisseur de ressources d’appliance**. Le service Applications managées utilise cette identité pour exécuter des opérations. Pour pouvoir récupérer une valeur dans un coffre de clés pendant le déploiement, le principal de service doit avoir accès à ce coffre.

Cet article décrit comment configurer le coffre de clés pour l’utiliser avec les applications managées.

## <a name="enable-template-deployment"></a>Activer un déploiement de modèle

1. Dans le portail, sélectionnez votre coffre de clés.

1. Sélectionnez **Stratégies d’accès**.   

   ![Sélection des stratégies d’accès](./media/key-vault-access/select-access-policies.png)

1. Sélectionnez **Cliquez ici pour afficher les stratégies d'accès avancé**.

   ![Affichage des stratégies d'accès avancé](./media/key-vault-access/advanced.png)

1. Sélectionnez **Activer l'accès à Azure Resource Manager pour le déploiement de modèles**. Sélectionnez ensuite **Enregistrer**.

   ![Activer un déploiement de modèle](./media/key-vault-access/enable-template.png)

## <a name="add-service-as-contributor"></a>Ajouter un service en tant que contributeur

Attribuez le rôle **Contributeur** à l'utilisateur du **fournisseur de ressources de l'appliance** au niveau de l'étendue du coffre de clés.

Pour connaître les étapes détaillées, consultez [Attribuer des rôles Azure à l’aide du portail Azure](../../role-based-access-control/role-assignments-portal.md).

## <a name="reference-key-vault-secret"></a>Référencer un secret de coffre de clés

Pour transmettre un secret d’un coffre de clés à un modèle dans votre application managée, vous devez utiliser un [modèle lié ou imbriqué](../templates/linked-templates.md) et référencer le coffre de clés dans les paramètres de ce modèle lié ou imbriqué. Fournissez l’ID de ressource du coffre de clés et le nom du secret.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "The location where the resources will be deployed."
      }
    },
    "vaultName": {
      "type": "string",
      "metadata": {
        "description": "The name of the keyvault that contains the secret."
      }
    },
    "secretName": {
      "type": "string",
      "metadata": {
        "description": "The name of the secret."
      }
    },
    "vaultResourceGroupName": {
      "type": "string",
      "metadata": {
        "description": "The name of the resource group that contains the keyvault."
      }
    },
    "vaultSubscription": {
      "type": "string",
      "defaultValue": "[subscription().subscriptionId]",
      "metadata": {
        "description": "The name of the subscription that contains the keyvault."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "name": "dynamicSecret",
      "properties": {
        "mode": "Incremental",
        "expressionEvaluationOptions": {
          "scope": "inner"
        },
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminLogin": {
              "type": "string"
            },
            "adminPassword": {
              "type": "securestring"
            },
            "location": {
              "type": "string"
            }
          },
          "variables": {
            "sqlServerName": "[concat('sql-', uniqueString(resourceGroup().id, 'sql'))]"
          },
          "resources": [
            {
              "type": "Microsoft.Sql/servers",
              "apiVersion": "2018-06-01-preview",
              "name": "[variables('sqlServerName')]",
              "location": "[parameters('location')]",
              "properties": {
                "administratorLogin": "[parameters('adminLogin')]",
                "administratorLoginPassword": "[parameters('adminPassword')]"
              }
            }
          ],
          "outputs": {
            "sqlFQDN": {
              "type": "string",
              "value": "[reference(variables('sqlServerName')).fullyQualifiedDomainName]"
            }
          }
        },
        "parameters": {
          "location": {
            "value": "[parameters('location')]"
          },
          "adminLogin": {
            "value": "ghuser"
          },
          "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
              },
              "secretName": "[parameters('secretName')]"
            }
          }
        }
      }
    }
  ],
  "outputs": {
  }
}
```

## <a name="next-steps"></a>Étapes suivantes

Vous avez configuré votre coffre de clés pour qu’il soit accessible au cours du déploiement d’une application managée.

* Pour obtenir des informations sur la transmission d’une valeur à partir d’un coffre de clés en tant que paramètre de modèle, consultez [Utiliser Azure Key Vault pour transmettre une valeur de paramètre sécurisée pendant le déploiement](../templates/key-vault-parameter.md).
* Pour obtenir des exemples d’applications managées, consultez [Exemples de projets pour des applications managées Azure](sample-projects.md).
* Pour en savoir plus sur la création d’un fichier de définition de l’interface utilisateur pour une application gérée, consultez [Prise en main de CreateUiDefinition](create-uidefinition-overview.md).
