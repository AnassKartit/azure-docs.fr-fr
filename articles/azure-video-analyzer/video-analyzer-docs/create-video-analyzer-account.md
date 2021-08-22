---
title: Créer un compte Azure Video Analyzer
description: Cette rubrique explique comment créer un compte pour Azure Video Analyzer.
ms.service: azure-video-analyzer
ms.topic: how-to
ms.date: 06/01/2021
ms.openlocfilehash: 1cb2f317ca712f2ad8ca911ecff0ac5e62cac0f5
ms.sourcegitcommit: 3941df51ce4fca760797fa4e09216fcfb5d2d8f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/23/2021
ms.locfileid: "114604005"
---
# <a name="create-a-video-analyzer-account"></a>Créer un compte Video Analyzer

Pour commencer à utiliser Azure Video Analyzer, vous devez créer un compte Video Analyzer. Le compte doit être associé à un compte de stockage et à une [identité managée affectée par l’utilisateur][docs-uami]. L’identité managée doit disposer des autorisations attribuées aux rôles [Contributeur aux données Blob du stockage][docs-storage-access] et [Lecteur][docs-role-reader] pour le compte de stockage. Cet article décrit les étapes à suivre pour créer un compte Video Analyzer.

 Vous pouvez utiliser le portail Azure ou un [modèle Azure Resource Manager (ARM)][docs-arm-template] pour créer un compte Video Analyzer. Choisissez l’onglet de la méthode que vous souhaitez utiliser.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="portal"></a>[Portail](#tab/portal/)

[!INCLUDE [the video analyzer account and storage account must be in the same subscription and region](./includes/note-account-storage-same-subscription.md)]

### <a name="create-a-video-analyzer-account-in-the-azure-portal"></a>Créer un compte Video Analyzer dans le portail Azure

1. Connectez-vous au [portail Azure](https://portal.azure.com/).
1. Dans la barre de recherche située en haut, entrez **Video Analyzer**.
1. Sous *Services*, cliquez sur *Analyseurs vidéo*.
1. Cliquez sur **Add**.
1. Dans la section **Créer un compte Video Analyzer**, entrez les valeurs requises.

    | Nom | Description |
    | ---|---|
    |**Abonnement**|Si vous disposez de plusieurs abonnements, sélectionnez-en un dans la liste des abonnements Azure auxquels vous avez accès.|
    |**Groupe de ressources**|Sélectionnez une ressource existante ou créez-en une. Un groupe de ressources désigne une collection de ressources qui partagent un cycle de vie, des autorisations et des stratégies. En savoir plus [ici](../../azure-resource-manager/management/overview.md#resource-groups).|
    |**Nom du compte Video Analyzer**|Entrez le nom du nouveau compte Video Analyzer. Un nom de compte Video Analyzer se compose de lettres en minuscules ou de chiffres, sans espaces. Sa longueur est comprise entre 3 et 24 caractères.|
    |**Lieu**|Sélectionnez la région géographique à utiliser pour stocker les enregistrements vidéo et les métadonnées de votre compte Video Analyzer. Seules les régions Video Analyzer disponibles s’affichent dans la liste déroulante. |
    |**Compte de stockage**|Sélectionnez le compte de stockage qui fournira le stockage d’objets blob du contenu vidéo pour votre compte Video Analyzer. Vous pouvez sélectionner un compte de stockage existant dans la même région géographique que votre compte Video Analyzer ou en créer un. Ce dernier sera créé dans la même région. Les règles de dénomination des comptes de stockage sont identiques à celles applicables aux comptes Video Analyzer.<br/>|
    |**Identité de l’utilisateur**|Sélectionnez une identité managée affectée par l’utilisateur, que le nouveau compte Video Analyzer utilisera pour accéder au compte de stockage. Vous pouvez sélectionner une identité managée affectée par l’utilisateur existante ou en créer une. L’identité managée affectée par l’utilisateur se verra attribuer les rôles [Contributeur aux données Blob du stockage][docs-storage-access] et [Lecteur][docs-role-reader] pour le compte de stockage.

1. Au bas du formulaire, cliquez sur **Examiner et créer**.

## <a name="template"></a>[Modèle](#tab/template/)

[!INCLUDE [the video analyzer account and storage account must be in the same subscription and region](./includes/note-account-storage-same-subscription.md)]

### <a name="create-a-video-analyzer-account-using-a-template"></a>Créer un compte Video Analyzer à l’aide d’un modèle

Les ressources suivantes sont définies dans le modèle :

- [**Microsoft.Media/videoAnalyzers**](/azure/templates/Microsoft.Media/videoAnalyzers) : ressource de compte pour Video Analyzer.
- [**Microsoft.Storage/storageAccounts**](/azure/templates/Microsoft.Storage/storageAccounts) : compte de stockage que Video Analyzer utilisera pour le stockage des vidéos et des métadonnées.
- [**Microsoft.ManagedIdentity/propriété userassignedidentities**](/azure/templates/Microsoft.ManagedIdentity/userAssignedIdentities) : identité managée affectée par l’utilisateur que Video Analyzer utilisera pour accéder au stockage.
- [**Microsoft.Storage/storageAccounts/Providers/roleAssignments**](/azure/templates/microsoft.authorization/roleassignments) : attributions de rôles qui permettent à Video Analyzer d’accéder au compte de stockage.

<!-- TODO replace with a reference like this:
:::code language="json" source="~/quickstart-templates/quickstarts/microsoft.compute/vm-simple-linux/azuredeploy.json":::
-->

```json

{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "namePrefix": {
            "metadata": {
                "description": "Used to qualify the names of all of the resources created in this template."
            },
            "defaultValue": "avasample",
            "type": "string",
            "minLength": 3,
            "maxLength": 13
        }
    },
    "variables": {
        "storageAccountName": "[concat(parameters('namePrefix'),uniqueString(resourceGroup().id))]",
        "accountName": "[concat(parameters('namePrefix'),uniqueString(resourceGroup().id))]",
        "managedIdentityName": "[concat(parameters('namePrefix'),'-',resourceGroup().name,'-storage-access-identity')]"
    },
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2020-10-01",
            "name": "deploy-storage-and-identity",
            "properties": {
                "mode": "Incremental",
                "expressionEvaluationOptions": {
                    "scope": "Inner"
                },
                "parameters": {
                    "namePrefix": {
                        "value": "[parameters('namePrefix')]"
                    },
                    "managedIdentityName": {
                        "value": "[variables('managedIdentityName')]"
                    }
                },
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "namePrefix": {
                            "type": "string"
                        },
                        "managedIdentityName": {
                            "type": "string"
                        }
                    },
                    "variables": {
                        "storageAccountName": "[concat(parameters('namePrefix'),uniqueString(resourceGroup().id))]",
                        "managedIdentityName": "[parameters('managedIdentityName')]",
                        "storageBlobDataContributorAssignment": "[guid('Storage Blob Data Contributor',variables('managedIdentityName'))]",
                        "storageBlobDataContributorDefinitionId": "[concat(resourceGroup().id, '/providers/Microsoft.Authorization/roleDefinitions/', 'ba92f5b4-2d11-453d-a403-e96b0029c9fe')]",
                        "readerAssignment": "[guid('Reader',variables('managedIdentityName'))]",
                        "readerDefinitionId": "[concat(resourceGroup().id, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
                    },
                    "resources": [
                        {
                            "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
                            "name": "[variables('managedIdentityName')]",
                            "apiVersion": "2015-08-31-preview",
                            "location": "[resourceGroup().location]"
                        },
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "apiVersion": "2019-04-01",
                            "name": "[variables('storageAccountName')]",
                            "location": "[resourceGroup().location]",
                            "sku": {
                                "name": "Standard_LRS"
                            },
                            "kind": "StorageV2",
                            "properties": {
                                "accessTier": "Hot"
                            }
                        },
                        {
                            "name": "[concat(variables('storageAccountName'), '/Microsoft.Authorization/', variables('storageBlobDataContributorAssignment'))]",
                            "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
                            "apiVersion": "2021-04-01-preview",
                            "dependsOn": [
                                "[variables('managedIdentityName')]",
                                "[variables('storageAccountName')]"
                            ],
                            "properties": {
                                "roleDefinitionId": "[variables('storageBlobDataContributorDefinitionId')]",
                                "principalId": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities',variables('managedIdentityName')), '2018-11-30').principalId]",
                                "principalType": "ServicePrincipal"
                            }
                        },
                        {
                            "name": "[concat(variables('storageAccountName'), '/Microsoft.Authorization/', variables('readerAssignment'))]",
                            "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
                            "apiVersion": "2021-04-01-preview",
                            "dependsOn": [
                                "[variables('managedIdentityName')]",
                                "[variables('storageAccountName')]"
                            ],
                            "properties": {
                                "roleDefinitionId": "[variables('readerDefinitionId')]",
                                "principalId": "[reference(resourceId('Microsoft.ManagedIdentity/userAssignedIdentities',variables('managedIdentityName')), '2018-11-30').principalId]",
                                "principalType": "ServicePrincipal"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        },
        {
            "type": "Microsoft.Media/videoAnalyzers",
            "comments": "The Azure Video Analyzer account",
            "apiVersion": "2021-05-01-preview",
            "name": "[variables('accountName')]",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "deploy-storage-and-identity"
            ],
            "properties": {
                "storageAccounts": [
                    {
                        "id": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
                        "identity": {
                            "userAssignedIdentity": "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities',variables('managedIdentityName'))]"
                        }
                    }
                ]
            },
            "identity": {
                "type": "UserAssigned",
                "userAssignedIdentities": {
                    "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities',variables('managedIdentityName'))]": {}
                }
            }
        }
    ],
    "outputs": { }
}
```

> [!NOTE]
> Le modèle utilise un déploiement imbriqué pour les attributions de rôle, afin de s’assurer qu’il est disponible avant le déploiement de la ressource de compte Video Analyzer.

### <a name="deploy-the-template"></a>Déployer le modèle

1. Pour vous connecter à Azure et ouvrir un modèle, cliquez sur *Déployer sur Azure*.

    [![Déployer sur Azure](https://aka.ms/deploytoazurebutton)](https://aka.ms/ava-click-to-deploy)

1. Sélectionnez ou entrez les valeurs suivantes. Utilisez les valeurs par défaut, le cas échéant.

    - **Abonnement** : sélectionnez un abonnement Azure.
    - **Groupe de ressources** : sélectionnez un groupe de ressources existant dans la liste déroulante ou sélectionnez **Créer**, entrez un nom unique pour le groupe de ressources, puis cliquez sur **OK**.
    - **Emplacement** : sélectionnez un emplacement.  Par exemple, **USA Ouest 2**.
    - **Préfixe de nom** : entrez une chaîne à utiliser pour préfixer le nom des ressources (les valeurs par défaut sont recommandées).

1. Sélectionnez **Revoir + créer**. Une fois la validation terminée, sélectionnez **Créer** pour créer et déployer la machine virtuelle.

Le portail Azure est utilisé pour déployer le modèle. Outre le portail Azure, vous pouvez utiliser Azure CLI, Azure PowerShell et l’API REST. Pour découvrir d’autres méthodes de déploiement, consultez [Déployer des modèles](../../azure-resource-manager/templates/deploy-cli.md).

### <a name="review-deployed-resources"></a>Vérifier les ressources déployées

Vous pouvez utiliser le portail Azure pour vérifier le compte et les autres ressources créées. Une fois le déploiement terminé, sélectionnez **Accéder au groupe de ressources** pour afficher le compte et les autres ressources.

### <a name="clean-up-resources"></a>Nettoyer les ressources

Quand vous n’en avez plus besoin, supprimez le groupe de ressources. Cela a pour effet de supprimer également le compte et toutes les ressources du groupe de ressources.

1. Sélectionnez le **groupe de ressources**.
1. Dans la page du groupe de ressources, sélectionnez **Supprimer**.
1. Lorsque vous y êtes invité, entrez le nom du groupe de ressources, puis sélectionnez **Supprimer**.

---

### <a name="next-steps"></a>Étapes suivantes

Découvrez comment [déployer Video Analyzer sur un appareil IoT Edge][docs-deploy-on-edge].

<!-- links -->
[docs-uami]: ../../active-directory/managed-identities-azure-resources/overview.md
[docs-storage-access]: ../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor
[docs-role-reader]: ../../role-based-access-control/built-in-roles.md#reader
[docs-arm-template]: ../../azure-resource-manager/templates/overview.md
[docs-deploy-on-edge]: deploy-iot-edge-device.md
[click-to-deploy]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fgist.githubusercontent.com%2Fbennage%2F58523b2e6a4d3bf213f16893d894dcaf%2Fraw%2Fazuredeploy.json
<!-- TODO update the link above! -->
