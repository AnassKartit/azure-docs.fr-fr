---
title: Gérer des règles de pare-feu - Azure CLI - Azure Database pour PostgreSQL - Serveur flexible
description: Créez et gérez des règles de pare-feu pour Azure Database pour PostgreSQL – Serveur flexible à l’aide d’une ligne de commande Azure CLI.
author: sunilagarwal
ms.author: sunila
ms.service: postgresql
ms.devlang: azurecli
ms.topic: how-to
ms.date: 09/22/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: e571d29dc9bade1069f6c67257bf04552ff894ef
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2021
ms.locfileid: "130241607"
---
# <a name="create-and-manage-azure-database-for-postgresql---flexible-server-firewall-rules-using-the-azure-cli"></a>Créer et gérer des règles de pare-feu Azure Database pour PostgreSQL – Serveur flexible à l’aide d’Azure CLI

> [!IMPORTANT]
> Azure Database pour PostgreSQL - Serveur flexible est en préversion

Azure Database pour PostgreSQL - Serveur flexible prend en charge deux types de méthodes de connectivité réseau mutuellement exclusives pour se connecter à votre serveur flexible. Les deux options sont les suivantes :

* Accès public (adresses IP autorisées)
* Accès privé (intégration au réseau virtuel)

Cet article est axé sur la création d’un serveur PostgreSQL avec **accès public (adresses IP autorisées)** à l’aide d’Azure CLI, et fournit une vue d’ensemble des commandes Azure CLI que vous pouvez utiliser pour créer, mettre à jour, supprimer, lister et afficher les règles de pare-feu après la création du serveur. Avec un *accès public (adresses IP autorisées)* , les connexions au serveur PostgreSQL sont limitées aux adresses IP autorisées. Les adresses IP des clients doivent être autorisées dans les règles de pare-feu. Pour en savoir plus à ce sujet, consultez [Accès public (adresses IP autorisées)](./concepts-networking.md#public-access-allowed-ip-addresses). Vous pouvez définir les règles de pare-feu au moment de la création du serveur (recommandé), mais aussi les ajouter ultérieurement.

## <a name="launch-azure-cloud-shell"></a>Lancement d’Azure Cloud Shell

[Azure Cloud Shell](../../cloud-shell/overview.md) est un interpréteur de commandes interactif et gratuit que vous pouvez utiliser pour exécuter les étapes de cet article. Il contient des outils Azure courants préinstallés et configurés pour être utilisés avec votre compte.

Pour ouvrir Cloud Shell, sélectionnez simplement **Essayer** en haut à droite d’un bloc de code. Vous pouvez également ouvrir Cloud Shell dans un onglet distinct du navigateur en accédant à [https://shell.azure.com/bash](https://shell.azure.com/bash). Sélectionnez **Copier** pour copier les blocs de code, collez-les dans Cloud Shell et sélectionnez **Entrée** pour les exécuter.

Si vous préférez installer et utiliser l’interface de ligne de commande en local, ce guide de démarrage rapide nécessite au minimum la version 2.0 d’Azure CLI. Exécutez `az --version` pour trouver la version. Si vous devez installer ou mettre à niveau, voir [Installer Azure CLI](/cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Prérequis

Vous devrez vous connecter à votre compte avec la commande [az login](/cli/azure/reference-index#az_login). Notez la propriété **ID**, qui fait référence à l’**ID d’abonnement** de votre compte Azure.

```azurecli-interactive
az login
```

Sélectionnez l’abonnement spécifique sous votre compte à l’aide de la commande [az account set](/cli/azure/account#az_account_set). Notez la valeur **ID** issue de la commande **az login** ; vous devez vous en servir comme valeur de l’argument **subscription** dans la commande. Si vous avez plusieurs abonnements, sélectionnez l’abonnement approprié dans lequel la ressource doit être facturée. Pour accéder à l’ensemble de votre abonnement, utilisez [az account list](/cli/azure/account#az_account_list).

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-firewall-rule-during-flexible-server-create-using-azure-cli"></a>Créer une règle de pare-feu pendant la création d’un serveur flexible à l’aide d’Azure CLI

Vous pouvez utiliser la commande `az postgres flexible-server --public access` pour créer le serveur flexible avec un *accès public (adresses IP autorisées)* et configurer les règles de pare-feu lors de la création d’un serveur flexible. Vous pouvez utiliser le commutateur **--public-access** pour fournir les adresses IP autorisées qui pourront se connecter au serveur. Vous pouvez fournir une adresse IP ou une plage d’adresses IP à inclure dans la liste des adresses IP autorisées. Les adresses IP de la plage doivent être séparées par des tirets et ne doivent pas contenir d’espaces. Il existe différentes options pour créer un serveur flexible à l’aide de l’interface CLI, comme indiqué dans les exemples ci-dessous.

Consultez la documentation de référence sur Azure CLI <!--FIXME --> pour obtenir la liste complète de ses paramètres configurables. Par exemple, dans les commandes ci-dessous, vous pouvez spécifier le groupe de ressources.

- Créer un serveur flexible avec accès public et ajouter l’adresse IP du client pour avoir accès au serveur
    ```azurecli-interactive
    az postgres flexible-server create --public-access <my_client_ip>
    ```
- Créer un serveur flexible avec accès public et ajouter la plage d’adresses IP pour avoir accès au serveur

    ```azurecli-interactive
    az postgres flexible-server create --public-access <start_ip_address-end_ip_address>
    ```
- Créer un serveur flexible avec accès public, et autoriser les applications en provenance d’adresses IP Azure à se connecter à votre serveur flexible
    ```azurecli-interactive
    az postgres flexible-server create --public-access 0.0.0.0
    ```
    > [!IMPORTANT]
    > Cette option configure le pare-feu pour autoriser l’accès public à partir de services Azure et de ressources Azure à ce serveur, y compris les connexions à partir d’abonnements d’autres clients. Lorsque vous sélectionnez cette option, vérifiez que votre connexion et vos autorisations utilisateur limitent l’accès aux seuls utilisateurs autorisés.
    >
- - Créer un serveur flexible avec accès public et autoriser toutes les adresses IP
    ```azurecli-interactive
    az postgres flexible-server create --public-access all
    ```
    >[!Note]
    > La commande ci-dessus crée une règle de pare-feu avec une adresse IP de début=0.0.0.0, une adresse IP de fin=255.255.255.255 et aucune adresse IP bloquée. N’importe quel hôte sur Internet peut accéder à ce serveur. Il est fortement recommandé d’utiliser cette règle uniquement de manière temporaire et uniquement sur les serveurs de test ne contenant pas de données sensibles.
- Créer un serveur flexible avec accès public et sans adresse IP
    ```azurecli-interactive
    az postgres flexible-server create --public-access none
    ```
    >[!Note]
    > Nous déconseillons de créer un serveur sans règle de pare-feu. Si vous n’ajoutez pas de règle de pare-feu, aucun client ne pourra se connecter au serveur.
## <a name="create-and-manage-firewall-rule-after-server-create"></a>Créer et gérer une règle de pare-feu après la création du serveur
La commande Azure CLI **az postgres flexible-server firewall-rule** permet de créer, supprimer, lister, afficher et mettre à jour des règles de pare-feu.

Commandes :
- **create** : créer une règle de pare-feu de serveur flexible.
- **list** : lister les règles de pare-feu de serveur flexible.
- **update** : mettre à jour une règle de pare-feu de serveur flexible.
- **show** : afficher les détails d’une règle de pare-feu de serveur flexible.
- **delete** : supprimer une règle de pare-feu de serveur flexible.

Consultez la documentation de référence sur Azure CLI <!--FIXME --> pour obtenir la liste complète de ses paramètres configurables. Par exemple, dans les commandes ci-dessous, vous pouvez spécifier le groupe de ressources.

### <a name="create-a-firewall-rule"></a>Créer une règle de pare-feu
Utilisez la commande `az postgres flexible-server firewall-rule create` pour créer une règle de pare-feu sur le serveur.
Pour accorder l’accès à une plage d’adresses IP, indiquez l’adresse IP de début et l’adresse IP de fin, comme dans cet exemple. Cette commande attend également un nom de groupe de ressources Azure où se trouve le serveur en tant que paramètre. 
```azurecli-interactive
az postgres flexible-server firewall-rule create --name mydemoserver --resource-group testGroup --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```

Pour accorder l’accès à une adresse IP, il vous suffit de fournir une adresse IP, comme dans cet exemple.
```azurecli-interactive
az postgres flexible-server firewall-rule create --name mydemoserver  --resource-group testGroup  --start-ip-address 1.1.1.1
```

Pour autoriser les applications en provenance d’adresses IP Azure à se connecter à votre serveur flexible, spécifiez l’adresse IP 0.0.0.0 comme adresse IP de début, comme dans cet exemple.
```azurecli-interactive
az postgres flexible-server firewall-rule create --name mydemoserver --resource-group testGroup --start-ip-address 0.0.0.0
```

> [!IMPORTANT]
> Cette option configure le pare-feu pour autoriser l’accès public à partir de services Azure et de ressources Azure à ce serveur, y compris les connexions à partir d’abonnements d’autres clients. Lorsque vous sélectionnez cette option, vérifiez que votre connexion et vos autorisations utilisateur limitent l’accès aux seuls utilisateurs autorisés.
> 
En cas de réussite, la sortie de chaque commande de création affiche les détails de la règle de pare-feu que vous avez créée au format JSON (par défaut). En cas d’échec, la sortie affiche un texte de message d’erreur.

### <a name="list-firewall-rules"></a>Répertorier les règles de pare-feu 
Utilisez la commande `az postgres flexible-server firewall-rule list` pour lister les règles de pare-feu de serveur existantes sur le serveur. Notez que l’attribut de nom de serveur est spécifié dans le commutateur **---name**. 
```azurecli-interactive
az postgres flexible-server firewall-rule list --name mydemoserver --resource-group testGroup
```
La sortie répertorie les règles éventuelles au format JSON (par défaut). Vous pouvez utiliser le commutateur --output table** pour générer les résultats dans un format tabulaire plus lisible.
```azurecli-interactive
az postgres flexible-server firewall-rule list --name mydemoserver --resource-group testGroup --output table
```

### <a name="update-a-firewall-rule"></a>Mettre à jour une règle de pare-feu
Utilisez la commande `az postgres flexible-server firewall-rule update` pour mettre à jour une règle de pare-feu sur le serveur. Indiquez le nom de la règle de pare-feu existante en entrée, ainsi que les attributs d’adresse IP de début et de fin à mettre à jour.
```azurecli-interactive
az postgres flexible-server firewall-rule update --name mydemoserver --rule-name FirewallRule1 --resource-group testGroup --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
En cas de réussite, la sortie de la commande affiche les détails de la règle de pare-feu que vous avez mise à jour au format JSON (par défaut). En cas d’échec, la sortie affiche un texte de message d’erreur.

> [!NOTE]
> Si la règle de pare-feu n’existe pas, elle est créée par la commande de mise à jour.
### <a name="show-firewall-rule-details"></a>Afficher les détails d’une règle de pare-feu
Utilisez la commande `az postgres flexible-server firewall-rule show` pour afficher les détails de la règle de pare-feu existante à partir du serveur. Indiquez le nom de la règle de pare-feu existante comme entrée.
```azurecli-interactive
az postgres flexible-server firewall-rule show --name mydemoserver --rule-name FirewallRule1 --resource-group testGroup
```
En cas de réussite, la sortie de la commande affiche les détails de la règle de pare-feu que vous avez spécifiée au format JSON (par défaut). En cas d’échec, la sortie affiche un texte de message d’erreur.

### <a name="delete-a-firewall-rule"></a>Supprimer une règle de pare-feu
Utilisez la commande `az postgres flexible-server firewall-rule delete` pour supprimer une règle de pare-feu existante du serveur. Indiquez le nom de la règle de pare-feu existante.
```azurecli-interactive
az postgres flexible-server firewall-rule delete --name mydemoserver --rule-name FirewallRule1 --resource-group testGroup
```
En cas de réussite, aucun résultat ne s’affiche. En cas d’échec, le texte du message d’erreur s’affiche.

## <a name="next-steps"></a>Étapes suivantes
- En savoir plus sur les [réseaux dans Azure Database pour PostgreSQL - Serveur flexible](./concepts-networking.md)
- En savoir plus sur les [règles de pare-feu pour Azure Database pour PostgreSQL - Serveur flexible](./concepts-networking.md#public-access-allowed-ip-addresses)
- [Créer et gérer des règles de pare-feu Azure Database pour PostgreSQL - Serveur flexible à l’aide du portail Azure](./how-to-manage-firewall-portal.md)
