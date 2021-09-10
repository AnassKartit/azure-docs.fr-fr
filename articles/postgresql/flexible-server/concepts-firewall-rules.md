---
title: Règles de pare-feu - Azure Database pour PostgreSQL - Serveur flexible
description: Cet article explique comment utiliser des règles de pare-feu pour se connecter à Azure Database pour PostgreSQL - Serveur flexible avec option de déploiement d’un réseau public.
author: gennadNY
ms.author: gennadyk
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/21/2021
ms.openlocfilehash: 4e442bcf33ba252105289d3dc506a05467b3bbf1
ms.sourcegitcommit: d43193fce3838215b19a54e06a4c0db3eda65d45
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/20/2021
ms.locfileid: "122566146"
---
# <a name="firewall-rules-in-azure-database-for-postgresql---flexible-server"></a>Règles de pare-feu dans Azure Database pour PostgreSQL - Serveur flexible
Vous pouvez choisir parmi deux options réseau principales lorsque vous exécutez Azure Database pour PostgreSQL - Serveur flexible. Il s’agit de l’accès privé (intégration au réseau virtuel) et de l’accès public (adresses IP autorisées) . Avec l’accès public, le serveur flexible est accessible par le biais d’un point de terminaison public.
Lorsque l’option d’accès public est sélectionnée, le serveur Azure Database pour PostgreSQL est sécurisé par défaut, ce qui empêche tout accès à votre serveur de base de données jusqu’à ce que vous spécifiiez les hôtes IP autorisés à y accéder. Le pare-feu octroie l’accès au serveur en fonction de l’adresse IP d’origine de chaque demande.
Pour configurer votre pare-feu, vous créez des règles de pare-feu qui spécifient les plages d’adresses IP acceptables. Vous pouvez créer des règles de pare-feu au niveau du serveur.
**Règles de pare-feu :** Ces règles permettent aux clients d’accéder à l’ensemble de votre serveur Azure Database pour PostgreSQL, c’est-à-dire à toutes les bases de données qui se trouvent sur le même serveur logique. Les règles de pare-feu au niveau du serveur peuvent être configurées en utilisant le Portail Azure ou des commandes d’Azure CLI. Pour créer des règles de pare-feu au niveau du serveur, vous devez être le propriétaire de l’abonnement ou collaborateur.

## <a name="firewall-overview"></a>Présentation du pare-feu
Par défaut, tous les accès au serveur Azure Database pour PostgreSQL sont bloqués par le pare-feu. Pour accéder à votre serveur à partir d’un autre ordinateur/client ou une autre application, vous devez spécifier une ou plusieurs règles de pare-feu au niveau du serveur afin de permettre l’accès à votre serveur. Utilisez les règles de pare-feu pour spécifier les plages d’adresses IP publiques autorisées. L’accès au site web du Portail Azure proprement dit n’est pas affecté par les règles de pare-feu.
Les tentatives de connexion à partir d’Internet et d’Azure doivent franchir le pare-feu pour pouvoir atteindre votre base de données PostgreSQL, comme l’illustre le diagramme suivant :

:::image type="content" source="../media/concepts-firewall-rules/1-firewall-concept.png" alt-text="Exemple de flux de fonctionnement du pare-feu":::

## <a name="connecting-from-the-internet"></a>Connexion à partir d’Internet
Les règles de pare-feu au niveau du serveur s’appliquent à toutes les bases de données qui se trouvent sur le même serveur Azure Database pour PostgreSQL. Si l’adresse IP source de la requête appartient à une des plages spécifiées dans les règles de pare-feu au niveau du serveur, la connexion est accordée ; dans le cas contraire, elle est rejetée. Par exemple, si votre application se connecte au pilote JDBC pour PostgreSQL, il se peut que vous rencontriez cette erreur au cours de la tentative de connexion, si le pare-feu la bloque.
> java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: no pg\_hba.conf entry for host "123.45.67.890", user "adminuser", database "postgresql", SSL

## <a name="connecting-from-azure"></a>Connexion à partir d’Azure
Il est recommandé de trouver l’adresse IP sortante d’une application ou d’un service et d’autoriser explicitement l’accès à ces plages ou adresses IP individuelles. Par exemple, vous pouvez trouver l’adresse IP sortante d’un service Azure App Service ou utiliser une adresse IP publique liée à une machine virtuelle ou à une autre ressource (voir ci-dessous pour plus d’informations sur la connexion avec l’adresse IP privée d’une machine virtuelle sur les points de terminaison de service). 

Si aucune adresse IP sortante fixe n’est disponible pour votre service Azure, vous pouvez envisager d’activer les connexions à partir de toutes les adresses IP du centre de données Azure. Ce paramètre peut être activé à partir du portail Azure en affectant à l’option **Autoriser l’accès public dans Azure à ce serveur depuis n’importe quel service Azure** la valeur **ACTIVÉ** à partir du volet **Sécurité de la connexion**, puis en cliquant sur **Enregistrer**. À partir de l’interface de ligne de commande Azure, un paramètre de règle de pare-feu avec une adresse de début et de fin égale à 0.0.0.0 effectue l’opération équivalente. Si la tentative de connexion est rejetée par les règles de pare-feu, elle n’atteint pas le serveur Azure Database pour PostgreSQL.

> [!IMPORTANT]
> L’option **Autoriser l’accès public dans Azure à ce serveur à partir de n’importe quel service Azure** permet de configurer le pare-feu afin d’autoriser toutes les connexions en provenance d’Azure, notamment les connexions à partir des abonnements d’autres clients. Lorsque vous sélectionnez cette option, vérifiez que votre connexion et vos autorisations utilisateur limitent l’accès aux seuls utilisateurs autorisés.
> 

:::image type="content" source="./media/concepts-firewall-rules/allow-public-access.png" alt-text="Configurer Autoriser l’accès aux services Azure dans le portail":::
## <a name="programmatically-managing-firewall-rules"></a>Gestion par programmation des règles de pare-feu
En dehors du Portail Azure, les règles de pare-feu peuvent être gérées par programmation à l’aide d’Azure CLI.


## <a name="troubleshooting-firewall-issues"></a>Résolution des problèmes de pare-feu
Considérez les points suivants quand l’accès au service de serveur de base de données Microsoft Azure pour PostgreSQL présente un comportement anormal :

* **Les modifications apportées à la liste verte n’ont pas encore pris effet :** Jusqu’à cinq minutes peuvent s’écouler avant que les changements apportés à la configuration du pare-feu du serveur Azure Database pour PostgreSQL ne soient effectives.

* **La connexion n’est pas autorisée ou un mot de passe incorrect a été utilisé :** Si une connexion n’a pas d’autorisations sur le serveur Azure Database pour PostgreSQL ou que le mot de passe est incorrect, la connexion au serveur Azure Database pour PostgreSQL est refusée. Créer un paramètre de pare-feu permet uniquement aux clients de tenter de se connecter à votre serveur ; chaque client doit tout de même fournir les informations d’identification de sécurité nécessaires.

   Par exemple, avec un client JDBC, l’erreur suivante est susceptible d’apparaître.
   > java.util.concurrent.ExecutionException: java.lang.RuntimeException: org.postgresql.util.PSQLException: FATAL: password authentication failed for user "yourusername"

* **Adresse IP dynamique :** Si vous avez une connexion Internet avec un adressage IP dynamique et que le pare-feu demeure infranchissable, vous pouvez essayer une des solutions suivantes :

   * Demandez à votre fournisseur de services Internet (ISP) la plage d’adresses IP affectée à vos ordinateurs clients qui accèdent au serveur de base de données Azure pour PostgreSQL, puis ajoutez cette plage dans une règle de pare-feu.

   * Obtenez un adressage IP statique à la place pour vos ordinateurs clients, puis ajoutez l’adresse IP statique en tant que règle de pare-feu.

  


* **La règle de pare-feu n’est pas disponible pour le format IPv6 :** Les règles de pare-feu doivent être au format IPv4. Si vous spécifiez des règles de pare-feu au format IPv6, l’erreur de validation s’affiche.


## <a name="next-steps"></a>Étapes suivantes

* [Créer et gérer des règles de pare-feu de la base de données Azure pour PostgreSQL à l’aide du Portail Azure](how-to-manage-firewall-portal.md)
* [Créer et gérer des règles de pare-feu de la base de données Azure pour PostgreSQL à l’aide d’Azure CLI](how-to-manage-firewall-cli.md)
