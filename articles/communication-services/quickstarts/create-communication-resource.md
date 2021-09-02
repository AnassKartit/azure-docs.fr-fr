---
title: 'Démarrage rapide : Créer et gérer des ressources dans Azure Communication Services'
titleSuffix: An Azure Communication Services quickstart
description: Dans ce guide de démarrage rapide, vous allez découvrir comment créer et gérer votre première ressource Azure Communication Services.
author: probableprime
manager: chpalm
services: azure-communication-services
ms.author: rifox
ms.date: 06/30/2021
ms.topic: quickstart
ms.service: azure-communication-services
zone_pivot_groups: acs-plat-azp-azcli-net-ps
ms.openlocfilehash: 4516e753248b133ef0a48ab12ffe032ef7a7809e
ms.sourcegitcommit: 2eac9bd319fb8b3a1080518c73ee337123286fa2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2021
ms.locfileid: "123253578"
---
# <a name="quickstart-create-and-manage-communication-services-resources"></a>Démarrage rapide : Créer et gérer des ressources Communication Services

Commencez avec Azure Communication Services en provisionnant votre première ressource Communication Services. Les ressources Communication Services peuvent être provisionnées dans le [portail Azure](https://portal.azure.com) ou avec le kit SDK Gestion .NET. Le kit SDK Gestion et le portail Azure vous permettent de créer, configurer, mettre à jour et supprimer vos ressources et votre interface avec [Azure Resource Manager](../../azure-resource-manager/management/overview.md), le service de gestion et de déploiement d’Azure. Toutes les fonctionnalités disponibles dans les kits SDK sont accessibles depuis le portail Azure. 


> [!WARNING]
> Notez que bien que Communication Services soit disponible dans plusieurs zones géographiques, afin d’obtenir un numéro de téléphone, l’emplacement des données de la ressource doit être défini sur « US ». Notez également qu’il n’est pas possible de créer un groupe de ressources en même temps qu’une ressource pour Azure Communication Services. Lors de la création d’une ressource, un groupe de ressources qui a déjà été créé doit être utilisé.

::: zone pivot="platform-azp"
[!INCLUDE [Azure portal](./includes/create-resource-azp.md)]
::: zone-end

::: zone pivot="platform-azcli"
[!INCLUDE [Azure CLI](./includes/create-resource-azcli.md)]
::: zone-end

::: zone pivot="platform-net"
[!INCLUDE [.NET](./includes/create-resource-net.md)]
::: zone-end

::: zone pivot="platform-powershell"
[!INCLUDE [PowerShell](./includes/create-resource-powershell.md)]
::: zone-end


## <a name="access-your-connection-strings-and-service-endpoints"></a>Accéder à vos chaînes de connexion et points de terminaison de service

Les chaînes de connexion sont utilisées par les kits SDK Communication Services pour se connecter et s’authentifier auprès d’Azure. Vous pouvez accéder à vos chaînes de connexion et points de terminaison de service Communication Services à partir du portail Azure, ou programmatiquement à l’aide des API Azure Resource Manager.

Une fois que vous avez accédé à votre ressource Communication Services, sélectionnez **Clés** dans le menu de navigation et copiez les valeurs de **Chaîne de connexion** ou de **Point de terminaison** à utiliser par les kits SDK Communication Services. Notez que vous avez accès aux clés primaires et secondaires. Cela peut être utile dans les scénarios où vous souhaitez accorder à un environnement tiers ou de préproduction un accès temporaire à vos ressources Communication Services.

:::image type="content" source="./media/key.png" alt-text="Capture d’écran de la page Clés dans Communication Services.":::

Vous pouvez également accéder aux informations de clé au moyen de l’interface Azure CLI, comme votre groupe de ressources ou les clés d’une ressource spécifique. 

Installez [Azure CLI](/cli/azure/install-azure-cli-windows?tabs=azure-cli) et utilisez la commande suivante pour vous connecter. Vous devrez fournir vos informations d’identification pour vous connecter à votre compte Azure.
```azurecli
az login
```

Vous pouvez désormais accéder à des informations importantes sur vos ressources.
```azurecli
az communication list --resource-group "<resourceGroup>"

az communication list-key --name "<communicationName>" --resource-group "<resourceGroup>"
```

Si vous souhaitez sélectionner un abonnement particulier, vous pouvez également indiquer l’indicateur ```--subscription``` et fournir l’ID d’abonnement.
```
az communication list --resource-group  "resourceGroup>"  --subscription "<subscriptionID>"

az communication list-key --name "<communicationName>" --resource-group "resourceGroup>" --subscription "<subscriptionID>"
```

## <a name="store-your-connection-string"></a>Stocker votre chaîne de connexion

Les kits SDK Communication Services utilisent des chaînes de connexion pour autoriser les requêtes adressées à Communication Services. Plusieurs options vous permettant de stocker votre chaîne de connexion s’offrent à vous :

* Une application s’exécutant sur le bureau ou sur un appareil peut stocker la chaîne de connexion dans un fichier **app.config** ou **web.config**. Ajoutez la chaîne de connexion dans la section **AppSettings** de ces fichiers.
* Une application qui s’exécute dans Azure App Service peut stocker la chaîne de connexion dans les [paramètres d’application d’App Service](../../app-service/configure-common.md). Ajoutez la chaîne de connexion dans la section **Chaînes de connexion** sous l’onglet Paramètres d’application dans le portail.
* Vous pouvez stocker votre chaîne de connexion dans [Azure Key Vault](../../data-factory/store-credentials-in-key-vault.md).
* Si vous exécutez votre application localement, vous voudrez peut-être stocker votre chaîne de connexion dans une variable d’environnement.

### <a name="store-your-connection-string-in-an-environment-variable"></a>Stocker votre chaîne de connexion dans une variable d’environnement

Pour configurer une variable d’environnement, ouvrez une fenêtre de console, puis sélectionnez votre système d’exploitation dans les onglets ci-dessous. Remplacez `<yourconnectionstring>` par votre chaîne de connexion.

#### <a name="windows"></a>[Windows](#tab/windows)

Ouvrez une fenêtre de console et entrez la commande suivante :

```console
setx COMMUNICATION_SERVICES_CONNECTION_STRING "<yourconnectionstring>"
```

Après avoir ajouté la variable d’environnement, vous devrez peut-être redémarrer tous les programmes en cours d’exécution qui devront la lire, y compris la fenêtre de console. Par exemple, si vous utilisez Visual Studio comme éditeur, redémarrez Visual Studio avant d’exécuter l’exemple.

#### <a name="macos"></a>[macOS](#tab/unix)

Modifiez le fichier **.zshrc** pour y ajouter la variable d’environnement :

```bash
export COMMUNICATION_SERVICES_CONNECTION_STRING="<yourconnectionstring>"
```

Après avoir ajouté la variable d’environnement, exécutez `source ~/.zshrc` depuis la fenêtre de console pour appliquer les changements. Si vous avez créé la variable d’environnement avec votre IDE ouvert, vous devrez peut-être fermer et rouvrir l’éditeur, l’IDE ou l’interpréteur de commandes pour accéder à la variable.

#### <a name="linux"></a>[Linux](#tab/linux)

Modifiez le fichier **.bash_profile** afin d’y ajouter la variable d’environnement :

```bash
export COMMUNICATION_SERVICES_CONNECTION_STRING="<yourconnectionstring>"
```

Après avoir ajouté la variable d’environnement, exécutez `source ~/.bash_profile` depuis la fenêtre de console pour appliquer les changements. Si vous avez créé la variable d’environnement avec votre IDE ouvert, vous devrez peut-être fermer et rouvrir l’éditeur, l’IDE ou l’interpréteur de commandes pour accéder à la variable.

---

## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous voulez nettoyer et supprimer un abonnement Communication Services, vous pouvez supprimer la ressource ou le groupe de ressources. La suppression du groupe de ressources efface également les autres ressources qui y sont associées. 

Si des numéros de téléphone sont encore associés à votre ressource au moment de la suppression de la ressource, ils sont automatiquement libérés de votre ressource en même temps. 

> [!Note]
> La suppression d’une ressource est **définitive** et aucune donnée, y compris les filtres d’événements, les numéros de téléphone ou autres informations liées à votre ressource, ne peut être récupérée si vous supprimez la ressource.

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez découvert comment :

> [!div class="checklist"]
> * Créer une ressource Communication Services
> * Configurer la zone géographique et des étiquettes pour la ressource
> * Accéder aux clés de cette ressource
> * Supprimer la ressource

> [!div class="nextstepaction"]
> [Créer vos premiers jetons d’accès utilisateur](access-tokens.md)
