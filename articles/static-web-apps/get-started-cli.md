---
title: 'Démarrage rapide : Création de votre premier site statique avec le service Azure Static Web Apps à l’aide de l’interface CLI'
description: Apprenez à déployer un site statique sur Azure Static Web Apps avec l’interface Azure CLI.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: quickstart
ms.date: 08/13/2020
ms.author: cshoe
ms.openlocfilehash: ef3072d5c30a8e07f9073539e877ca37837a68a4
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2021
ms.locfileid: "123428323"
---
# <a name="quickstart-building-your-first-static-site-using-the-azure-cli"></a>Démarrage rapide : Création de votre premier site statique à l’aide de l’interface Azure CLI

Azure Static Web Apps publie un site web dans un environnement de production en compilant les applications à partir d’un dépôt GitHub. Dans ce guide de démarrage rapide, vous déployez une application web dans Azure Static Web Apps à l’aide de l’interface Azure CLI.

Si vous n’avez pas d’abonnement Azure, [créez un compte d’essai gratuit](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Prérequis

- [GitHub](https://github.com)
- [Jeton d’accès personnel GitHub](https://docs.github.com/github/authenticating-to-github/creating-a-personal-access-token)
- Compte [Azure](https://portal.azure.com)
- Interface [Azure CLI](/cli/azure/install-azure-cli) installée (version 2.8.0 ou ultérieure)

[!INCLUDE [create repository from template](../../includes/static-web-apps-get-started-create-repo.md)]

[!INCLUDE [clone the repository](../../includes/static-web-apps-get-started-clone-repo.md)]

Ensuite, basculez vers le nouveau dossier à l’aide de la commande suivante.

```bash
cd my-first-static-web-app
```

## <a name="create-a-static-web-app"></a>Créer une application web statique

Maintenant que le dépôt est constitué, vous pouvez créer une application web statique à partir de l’interface Azure CLI.

> [!IMPORTANT]
> Assurez-vous que vous êtes dans le dossier _my-first-static-web-app_ de votre terminal.

1. Connectez-vous à l’interface Azure CLI en utilisant la commande suivante.

    ```azurecli
    az login
    ```

1. Créez une application web statique à partir de votre dépôt.

    # <a name="no-framework"></a>[Pas de framework](#tab/vanilla-javascript)

    ```azurecli
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b main \
        --app-artifact-location "src" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="angular"></a>[Angular](#tab/angular)

    ```azurecli
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b main \
        --app-artifact-location "dist/angular-basic" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="react"></a>[React](#tab/react)

    ```azurecli
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b main \
        --app-artifact-location "build" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="vue"></a>[Vue](#tab/vue)

    ```azurecli
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b main \
        --app-artifact-location "dist" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    ---
    
    > [!IMPORTANT]
    > L’URL transmise au paramètre `s` ne doit pas inclure le suffixe `.git`.

    - `<RESOURCE_GROUP_NAME>`Remplacez cette valeur par un [nom de groupe de ressources Azure](../azure-resource-manager/management/manage-resources-cli.md) existant.

      - Consultez la documentation [az group](/cli/azure/group#az_group_list) pour plus d’informations sur l’énumération des groupes de ressources.

    - `<YOUR_GITHUB_ACCOUNT_NAME>`: Remplacez cette valeur par votre nom d’utilisateur GitHub.

    - `<LOCATION>`: Remplacez cette valeur par l’emplacement le plus proche de vous. Options disponibles : _CentralUS_, _EastAsia_, _EastUS2_, _WestEurope_ et _WestUS2_.

    - `<YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>`: Remplacez cette valeur par le [jeton d’accès personnel GitHub](https://docs.github.com/github/authenticating-to-github/creating-a-personal-access-token) que vous avez généré précédemment. Les autorisations minimales sont l’étendue `workflow` pour les dépôts publics et l’étendue `repo` pour les dépôts privés.

    Vous pouvez désormais afficher l’application créée dans Azure.

1. Ouvrez le [Portail Azure](https://portal.azure.com).

1. Recherchez **my-first-web-static-app** dans la barre de recherche supérieure.

1. Sélectionnez **my-first-web-static-app**.

[!INCLUDE [view website](../../includes/static-web-apps-get-started-view-website.md)]

## <a name="clean-up-resources"></a>Nettoyer les ressources

Si vous ne prévoyez pas de continuer à utiliser cette application, vous pouvez supprimer l’instance Azure Static Web Apps en exécutant la commande suivante :

```azurecli
az staticwebapp delete \
    --name my-first-static-web-app \
    --resource-group my-first-static-web-app
```

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Ajouter une API](add-api.md)
