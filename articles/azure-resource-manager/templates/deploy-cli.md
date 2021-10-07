---
title: Modèles de déploiement Azure avec Azure CLI – Azure Resource Manager | Microsoft Docs
description: Utilisez Azure Resource Manager et Azure CLI pour créer et déployer des groupes de ressources sur Azure. Les ressources sont définies dans un modèle de déploiement Azure.
ms.topic: conceptual
ms.date: 09/17/2021
ms.custom: devx-track-azurecli, seo-azure-cli
keywords: azure cli deploy arm template, create resource group azure, azure deployment template, deployment resources, arm template, azure arm template
ms.openlocfilehash: 5b7734e3b91f7e842f17888f3f3c67d05655fc97
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128585011"
---
# <a name="how-to-use-azure-resource-manager-arm-deployment-templates-with-azure-cli"></a>Utilisation des modèles de déploiement Azure Resource Manager (ARM) avec Azure CLI

Cet article explique comment utiliser Azure CLI avec des modèles Resource Manager pour déployer vos ressources dans Azure.  Si vous n’avez pas une bonne connaissance des concepts de déploiement et de gestion des solutions Azure, consultez [Vue d’ensemble du déploiement de modèles](overview.md).

Les commandes de déploiement ont changé dans la version 2.2.0 d’Azure CLI. Les exemples de cet article nécessitent la [Azure CLI version 2.20.0 ou une version ultérieure](/cli/azure/install-azure-cli).

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

Si Azure CLI n’est pas installé, vous pouvez utiliser Azure Cloud Shell. Pour plus d’informations, consultez [Déployer des modèles Azure Resource Manager à partir d’Azure Cloud Shell](deploy-cloud-shell.md).

## <a name="deployment-scope"></a>Étendue du déploiement

Vous pouvez cibler votre modèle de déploiement Azure au niveau d’un groupe de ressources, d’un abonnement, d’un groupe d’administration ou d’un locataire. Les commandes à utiliser diffèrent en fonction de l’étendue du déploiement.

* Pour un déploiement dans un **groupe de ressources**, utilisez [az deployment group create](/cli/azure/deployment/group#az_deployment_group_create) :

  ```azurecli-interactive
  az deployment group create --resource-group <resource-group-name> --template-file <path-to-template>
  ```

* Pour un déploiement dans un **abonnement**, utilisez [az deployment sub create](/cli/azure/deployment/sub#az_deployment_sub_create) :

  ```azurecli-interactive
  az deployment sub create --location <location> --template-file <path-to-template>
  ```

  Pour plus d’informations sur les déploiements au niveau de l’abonnement, consultez [Créer des groupes de ressources et des ressources au niveau de l’abonnement](deploy-to-subscription.md).

* Pour un déploiement dans un **groupe de d’administration**, utilisez [az deployment mg create](/cli/azure/deployment/mg#az_deployment_mg_create) :

  ```azurecli-interactive
  az deployment mg create --location <location> --template-file <path-to-template>
  ```

  Pour plus d’informations sur les déploiements au niveau du groupe d’administration, consultez [Créer des ressources au niveau du groupe d’administration](deploy-to-management-group.md).

* Pour un déploiement dans un **locataire**, utilisez [az deployment tenant create](/cli/azure/deployment/tenant#az_deployment_tenant_create) :

  ```azurecli-interactive
  az deployment tenant create --location <location> --template-file <path-to-template>
  ```

  Pour plus d’informations sur les déploiements au niveau du locataire, consultez [Créer des ressources au niveau du locataire](deploy-to-tenant.md).

Pour chaque étendue, l’utilisateur qui déploie le modèle doit disposer des autorisations nécessaires pour créer des ressources.

## <a name="deploy-local-template"></a>Déployer un modèle local

Vous pouvez déployer un modèle ARM à partir de votre ordinateur local ou d’un modèle stocké en externe. Cette section décrit le déploiement d’un modèle local.

Si vous effectuez un déploiement vers un groupe de ressources qui n’existe pas, vous devez commencer par créer ce dernier. Le nom du groupe de ressources ne peut contenir que des caractères alphanumériques, des points, des traits de soulignement, des traits d'union et des parenthèses. Il peut comprendre jusqu’à 90 caractères. Le nom ne peut pas se terminer par un point.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
```

Pour déployer un modèle local, utilisez le paramètre `--template-file` dans la commande de déploiement. L’exemple suivant montre également comment définir une valeur de paramètre.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file <path-to-template> \
  --parameters storageAccountType=Standard_GRS
```

Le déploiement d’un modèle Azure peut prendre plusieurs minutes. Au terme, vous voyez un message qui inclut le résultat :

```output
"provisioningState": "Succeeded",
```

## <a name="deploy-remote-template"></a>Déployer un modèle distant

Au lieu de stocker les modèles Resource Manager sur votre ordinateur local, vous pouvez les stocker dans un emplacement externe. Vous pouvez stocker des modèles dans un dépôt de contrôle de code source (par exemple, GitHub). Vous pouvez aussi les stocker dans un compte de stockage Azure pour mettre en place un accès partagé dans votre organisation.

[!INCLUDE [Deploy templates in private GitHub repo](../../../includes/resource-manager-private-github-repo-templates.md)]

Si vous effectuez un déploiement vers un groupe de ressources qui n’existe pas, vous devez commencer par créer ce dernier. Le nom du groupe de ressources ne peut contenir que des caractères alphanumériques, des points, des traits de soulignement, des traits d'union et des parenthèses. Il peut comprendre jusqu’à 90 caractères. Le nom ne peut pas se terminer par un point.

```azurecli-interactive
az group create --name ExampleGroup --location "Central US"
```

Pour déployer un modèle externe, utilisez le paramètre `template-uri`.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/quickstarts/microsoft.storage/storage-account-create/azuredeploy.json" \
  --parameters storageAccountType=Standard_GRS
```

L’exemple précédent nécessite un URI accessible publiquement pour le modèle, ce qui convient pour la plupart des scénarios, sachant que votre modèle ne doit pas inclure de données sensibles. Si vous avez besoin de spécifier des données sensibles (par exemple, un mot de passe d’administrateur), passez cette valeur en tant que paramètre sécurisé. Toutefois, si vous souhaitez gérer l’accès au modèle, envisagez d’utiliser des [spécifications de modèle](#deploy-template-spec).

Pour déployer des modèles liés à distance avec un chemin d’accès relatif et qui sont stockés dans un compte de stockage, utilisez `query-string` pour spécifier le jeton SAP :

```azurecli-interactive
az deployment group create \
  --name linkedTemplateWithRelativePath \
  --resource-group myResourceGroup \
  --template-uri "https://stage20210126.blob.core.windows.net/template-staging/mainTemplate.json" \
  --query-string $sasToken
```

Pour plus d’informations, consultez [Utiliser le chemin d’accès relatif pour les modèles liés](./linked-templates.md#linked-template).

## <a name="azure-deployment-template-name"></a>Nom du modèle de déploiement Azure

Lors du déploiement d’un modèle ARM, vous pouvez attribuer un nom au modèle de déploiement Azure. Ce nom peut vous aider à récupérer le déploiement à partir de l’historique de déploiement. Si vous n’attribuez pas de nom au déploiement, le nom du fichier de modèle est utilisé. Par exemple, si vous déployez un modèle nommé _azuredeploy.json_ et que vous ne spécifiez pas de nom de déploiement, le déploiement est nommé `azuredeploy`.

Chaque fois que vous exécutez un déploiement, une entrée est ajoutée à l’historique de déploiement du groupe de ressources avec le nom du déploiement. Si vous exécutez un autre déploiement et que vous lui attribuez le même nom, l’entrée précédente est remplacée par le déploiement actuel. Si vous souhaitez conserver des entrées uniques dans l’historique de déploiement, attribuez un nom unique à chaque déploiement.

Pour créer un nom unique, vous pouvez assigner un numéro aléatoire.

```azurecli-interactive
deploymentName='ExampleDeployment'$RANDOM
```

Vous pouvez aussi ajouter une valeur de date.

```azurecli-interactive
deploymentName='ExampleDeployment'$(date +"%d-%b-%Y")
```

Si vous exécutez des déploiements simultanés dans le même groupe de ressources avec le même nom de déploiement, seul le dernier déploiement aboutit. Les déploiements de même nom qui n’arrivent pas à terme sont remplacés par le dernier déploiement. Par exemple, si vous exécutez un déploiement nommé `newStorage` qui déploie un compte de stockage nommé `storage1` et que, dans le même temps, vous exécutez un autre déploiement nommé `newStorage` qui déploie un compte de stockage nommé `storage2`, vous ne déployez qu’un seul compte de stockage. Le compte de stockage qui en résulte est nommé `storage2`.

En revanche, si vous exécutez un déploiement nommé `newStorage` qui déploie un compte de stockage nommé `storage1` et que, aussitôt terminé, vous exécutez un autre déploiement nommé `newStorage` qui déploie un compte de stockage nommé `storage2`, vous disposez de deux comptes de stockage : un nommé `storage1` et l’autre nommé `storage2`. Cependant, l’historique de déploiement ne présente qu’une seule entrée.

Quand vous spécifiez un nom unique pour chaque déploiement, vous pouvez les exécuter simultanément sans conflit. Si vous exécutez un déploiement nommé `newStorage1` qui déploie un compte de stockage nommé `storage1` et que, dans le même temps, vous exécutez un autre déploiement nommé `newStorage2` qui déploie un compte de stockage nommé `storage2`, vous disposez de deux comptes de stockage et l’historique de déploiement présente deux entrées.

Pour éviter les conflits lors de déploiements simultanés et faire en sorte que l’historique de déploiement présente des entrées uniques, attribuez un nom unique à chaque déploiement.

## <a name="deploy-template-spec"></a>Déployer une spec de modèle

Au lieu de déployer un modèle local ou distant, vous pouvez créer une [spécification de modèle](template-specs.md). La spécification de modèle est une ressource de votre abonnement Azure qui contient un modèle ARM. Elle facilite le partage sécurisé du modèle avec les utilisateurs de votre organisation. Vous utilisez le contrôle d’accès Azure en fonction du rôle (Azure RBAC) pour accorder l’accès à la spécification de modèle. Actuellement, cette fonctionnalité est uniquement disponible en tant que version préliminaire.

Les exemples suivants montrent comment créer et déployer une spécification de modèle.

Tout d’abord, créez la spécification de modèle en fournissant le modèle ARM.

```azurecli
az ts create \
  --name storageSpec \
  --version "1.0" \
  --resource-group templateSpecRG \
  --location "westus2" \
  --template-file "./mainTemplate.json"
```

Ensuite, recevez l’ID de la spécification de modèle et déployez-le.

```azurecli
id = $(az ts show --name storageSpec --resource-group templateSpecRG --version "1.0" --query "id")

az deployment group create \
  --resource-group demoRG \
  --template-spec $id
```

Pour plus d’informations, consultez [Specs de modèle Azure Resource Manager](template-specs.md).

## <a name="preview-changes"></a>Prévisualiser les modifications

Avant de déployer votre modèle ARM, vous pouvez prévisualiser les modifications que le modèle apportera à votre environnement. Utilisez l’[opération de simulation](./deploy-what-if.md) pour vérifier que le modèle apporte les changements prévus. Cette opération vérifie aussi que le modèle est exempt d’erreurs.

## <a name="parameters"></a>Paramètres

Pour passer les valeurs de paramètre, vous pouvez utiliser des paramètres inline ou un fichier de paramètres.

### <a name="inline-parameters"></a>Paramètres inline

Pour passer des paramètres inline, indiquez les valeurs dans `parameters`. Par exemple, pour transmettre une chaîne et un tableau à un modèle dans un interpréteur de commandes Bash, utilisez :

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file <path-to-template> \
  --parameters exampleString='inline string' exampleArray='("value1", "value2")'
```

Si vous utilisez l’interface de ligne de commande Azure avec l’invite de commandes Windows (CMD) ou PowerShell, transmettez le tableau au format : `exampleArray="['value1','value2']"`.

Vous pouvez également récupérer le contenu d’un fichier et fournir ce contenu en tant que paramètre inline.

```azurecli-interactive
az deployment group create \
  --resource-group testgroup \
  --template-file <path-to-template> \
  --parameters exampleString=@stringContent.txt exampleArray=@arrayContent.json
```

Obtenir une valeur de paramètre à partir d’un fichier est utile lorsque vous devez fournir des valeurs de configuration. Par exemple, vous pouvez fournir des [valeurs cloud-init pour une machine virtuelle Linux](../../virtual-machines/linux/using-cloud-init.md).

Le format _arrayContent.json_ est :

```json
[
    "value1",
    "value2"
]
```

Pour passer un objet en entrée, par exemple pour définir des balises, utilisez JSON. Par exemple, votre modèle peut inclure un paramètre comme celui-ci :

```json
    "resourceTags": {
      "type": "object",
      "defaultValue": {
        "Cost Center": "IT Department"
      }
    }
```

Dans ce cas, vous pouvez passer une chaîne JSON pour définir le paramètre comme indiqué dans le script bash suivant :

```bash
tags='{"Owner":"Contoso","Cost Center":"2345-324"}'
az deployment group create --name addstorage  --resource-group myResourceGroup \
--template-file $templateFile \
--parameters resourceName=abcdef4556 resourceTags="$tags"
```

Utilisez des guillemets doubles autour du JSON que vous voulez passer à l’objet.

Vous pouvez utiliser une variable pour contenir les valeurs de paramètre. Dans Bash, définissez la variable sur toutes les valeurs de paramètre et ajoutez-la à la commande de déploiement.

```azurecli-interactive
params="prefix=start suffix=end"

az deployment group create \
  --resource-group testgroup \
  --template-file <path-to-template> \
  --parameters $params
``` 

Toutefois, si vous utilisez Azure CLI avec l’invite de commandes (CMD) Windows ou PowerShell, définissez la variable sur une chaîne JSON. Échappez les guillemets : `$params = '{ \"prefix\": {\"value\":\"start\"}, \"suffix\": {\"value\":\"end\"} }'`.

### <a name="parameter-files"></a>Fichiers de paramètres

Au lieu de passer des paramètres en tant que valeurs inline dans votre script, il peut s’avérer plus facile d’utiliser un fichier JSON qui contient les valeurs des paramètres. Le fichier de paramètres doit être un fichier local. Les fichiers de paramètres externes ne sont pas pris en charge avec Azure CLI.

Pour plus d’informations sur le fichier de paramètres, consultez [Créer un fichier de paramètres Resource Manager](parameter-files.md).

Pour transmettre un fichier de paramètres local, utilisez `@` pour spécifier un fichier local nommé _storage.parameters.json_.

```azurecli-interactive
az deployment group create \
  --name ExampleDeployment \
  --resource-group ExampleGroup \
  --template-file storage.json \
  --parameters '@storage.parameters.json'
```

## <a name="handle-extended-json-format"></a>Gérer le format JSON étendu

Pour déployer un modèle avec des commentaires ou à plusieurs chaînes de ligne à l’aide d’Azure CLI version 2.3.0 ou antérieure, vous devez utiliser le commutateur `--handle-extended-json-format`.  Par exemple :

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2018-10-01",
  "name": "[variables('vmName')]", // to customize name, change it in variables
  "location": "[
    parameters('location')
    ]", //defaults to resource group location
  /*
    storage account and network interface
    must be deployed first
  */
  "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
  ],
```

## <a name="next-steps"></a>Étapes suivantes

* Pour restaurer un déploiement réussi lorsque vous obtenez une erreur, consultez [Restaurer en cas d’erreur vers un déploiement réussi](rollback-on-error.md).
* Pour spécifier comment gérer les ressources présentes dans le groupe de ressources, mais non définies dans le modèle, consultez [Modes de déploiement Azure Resource Manager](deployment-modes.md).
* Pour comprendre comment définir des paramètres dans votre modèle, consultez [Comprendre la structure et la syntaxe des modèles Azure Resource Manager](./syntax.md).
* Pour obtenir des conseils sur la résolution des erreurs courantes de déploiement, consultez la page [Résolution des erreurs courantes de déploiement Azure avec Azure Resource Manager](common-deployment-errors.md).