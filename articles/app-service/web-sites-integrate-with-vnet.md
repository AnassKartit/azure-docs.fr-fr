---
title: Intégrer une application au réseau virtuel Microsoft Azure
description: Intégrez une application à Azure App Service avec des réseaux virtuels Azure.
author: ccompy
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.topic: article
ms.date: 08/04/2021
ms.author: ccompy
ms.custom: seodec18, devx-track-azurepowershell
ms.openlocfilehash: ac90dadc93ce09bc2ce0af6314e4bd2c48ab79f8
ms.sourcegitcommit: 2da83b54b4adce2f9aeeed9f485bb3dbec6b8023
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2021
ms.locfileid: "122768669"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Intégrer votre application à un réseau virtuel Azure

Cet article décrit la fonctionnalité d’intégration au réseau virtuel d’Azure App Service et explique comment la configurer avec des applications dans [Azure App Service](./overview.md). Les [réseaux virtuels Azure][VNETOverview] vous permettent de placer un grand nombre de vos ressources Azure dans un réseau routable non-Internet. La fonctionnalité d’intégration au réseau virtuel permet à vos applications d’accéder à des ressources dans ou via un réseau virtuel. Elle n’autorise pas l’accès privé à vos applications.

Azure App Service propose deux variantes :

[!INCLUDE [app-service-web-vnet-types](../../includes/app-service-web-vnet-types.md)]

## <a name="enable-vnet-integration"></a>Activer une intégration au réseau virtuel

1. Accédez à l’interface utilisateur **Réseau** dans le portail App Service. Sous **Intégration de réseau virtuel**, sélectionnez **Cliquez ici pour configurer**.

1. Sélectionnez **Ajouter un réseau virtuel**.

    :::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-app.png" alt-text="Sélectionner l’intégration au réseau virtuel":::

1. La liste déroulante contient tous les réseaux virtuels Azure Resource Manager de votre abonnement dans la même région. Sous cette liste se trouve une liste des réseaux virtuels Azure Resource Manager dans toutes les autres régions. Sélectionnez le réseau virtuel avec lequel vous souhaitez effectuer l’intégration.

    :::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-add-vnet.png" alt-text="Sélectionner le réseau virtuel":::

    * Si le réseau virtuel se trouve dans la même région, créez un sous-réseau ou sélectionnez un sous-réseau préexistant vide.
    * Pour sélectionner un réseau virtuel dans une autre région, vous devez disposer d’une passerelle de réseau virtuel provisionnée dont l’option Point à site est activée.
    * Pour une intégration à un réseau virtuel classique, au lieu de sélectionner la liste déroulante **Réseau virtuel**, sélectionnez **Cliquez ici pour vous connecter à un réseau virtuel classique**. Sélectionnez le réseau virtuel classique que vous souhaitez utiliser. Le réseau virtuel cible doit déjà être doté d’une passerelle de réseau virtuel provisionnée dont l’option Point à site est activée.

    :::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-classic.png" alt-text="Sélectionner un réseau virtuel classique":::

Lors de l’intégration, votre application est redémarrée. Une fois l’intégration terminée, vous verrez des informations sur le réseau virtuel auquel vous êtes intégré.

## <a name="regional-vnet-integration"></a>Intégration au réseau virtuel régional

L’intégration au réseau virtuel régional prend en charge la connexion à un réseau virtuel dans la même région et ne nécessite pas de passerelle. L’utilisation de l’intégration au réseau virtuel régional permet à votre application d’accéder aux :

* Ressources d’un réseau virtuel dans la même région que votre application.
* Ressources de réseaux virtuels appairés au réseau virtuel auquel votre application est intégrée.
* Services sécurisés des points de terminaison de service.
* Ressources sur des connexions Azure ExpressRoute.
* Ressources dans le réseau virtuel auquel vous êtes intégré.
* Ressources sur des connexions appairées, notamment des connexions Azure ExpressRoute.
* Services avec points de terminaison privés.

Lorsque vous utilisez l’intégration au réseau virtuel avec des réseaux virtuels d’une même région, vous pouvez utiliser les fonctionnalités de même en réseau Azure suivantes :

* **Groupes de sécurité réseau (NSG)**  : Vous pouvez bloquer le trafic sortant avec un groupe de sécurité réseau placé sur votre sous-réseau d’intégration. Les règles de trafic entrant ne s’appliquent pas, car vous ne pouvez pas utiliser l’intégration au réseau virtuel pour fournir un accès entrant à votre application.
* **Tables de routage (Routes définies par l’utilisateur)**  : Vous pouvez placer une table de routage sur le sous-réseau d’intégration pour envoyer le trafic sortant où vous voulez.

La fonctionnalité est entièrement prise en charge pour les applications Windows et Linux, notamment les [conteneurs personnalisés](./quickstart-custom-container.md). Tous les comportements sont identiques entre les applications Windows et les applications Linux.

### <a name="how-regional-vnet-integration-works"></a>Fonctionnement de l’intégration au réseau virtuel régional

Les applications contenues dans App Service sont hébergées dans des rôles de travail. Les plans tarifaires de base et supérieurs sont des plans d’hébergement dédiés dans lesquels aucune autre charge de travail de client ne s’exécute sur les mêmes Workers. L’intégration au réseau virtuel régional opère en montant des interfaces virtuelles avec des adresses du sous-réseau délégué. Comme l’adresse de départ se trouve dans votre réseau virtuel, elle peut accéder à la plupart des éléments contenus dans votre réseau virtuel ou accessibles par celui-ci, comme le ferait une machine virtuelle dans votre réseau virtuel. L’implémentation de la mise en réseau est différente de l’exécution d’une machine virtuelle dans votre réseau virtuel. C’est pourquoi certaines fonctionnalités de mise en réseau ne sont pas encore disponibles.

:::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-how-regional-works.png" alt-text="Fonctionnement de l’intégration au réseau virtuel régional":::

Lorsque l’intégration au réseau virtuel régional est activée, votre application envoie la sortie par l’intermédiaire de votre réseau virtuel. Les adresses sortantes figurant sur le portail des propriétés de l’application sont toujours les adresses qu’utilise votre application. Si le routage de tout le trafic est activé, tout le trafic sortant est envoyé dans votre réseau virtuel. Si le routage de tout le trafic n’est pas activé, seul le trafic privé (RFC1918) et les points de terminaison de service configurés sur le sous-réseau d’intégration seront envoyés dans le réseau virtuel, et le trafic sortant vers l’Internet passera par les mêmes canaux que d’habitude.

La fonctionnalité ne prend en charge qu’une seule interface virtuelle par Worker. Une interface virtuelle par Worker signifie une intégration au réseau virtuel régional par plan App Service. Toutes les applications d’un même plan App Service peuvent utiliser la même intégration de réseau virtuel. Si vous avez besoin d’une application pour vous connecter à un autre réseau virtuel, vous devez créer un autre plan App Service. L’interface virtuelle utilisée n’est pas une ressource à laquelle les clients peuvent accéder directement.

Compte tenu du mode de fonctionnement de cette technologie, le trafic utilisé avec l’intégration au réseau virtuel n’apparaît pas dans les journaux de flux du groupe de sécurité réseau (NSG) ou d’Azure Network Watcher.

### <a name="subnet-requirements"></a>Configuration requise du sous-réseau

L’intégration au réseau virtuel dépend d’un sous-réseau dédié. Lorsque vous approvisionnez un sous-réseau, le sous-réseau Azure perd cinq adresses IP dès le début. Une seule adresse du sous-réseau d’intégration est utilisée pour chaque instance de plan. Lorsque vous définissez l’échelle de votre application sur quatre instances, quatre adresses sont utilisées. 

Lorsque vous en augmentez ou diminuez la taille, l’espace d’adressage requis est doublé pendant une brève période de temps. Cela affecte les instances prises en charge réelles et disponibles pour une taille de sous-réseau donnée. Le tableau suivant indique à la fois le nombre maximal d’adresses disponibles par bloc CIDR et l’impact de cette mise à l’échelle horizontale :

| Taille de bloc CIDR | Nombre maximal d’adresses disponibles | Mise à l’échelle horizontale maximale (instances)<sup>*</sup> |
|-----------------|-------------------------|---------------------------------|
| /28             | 11                      | 5                               |
| /27             | 27                      | 13                              |
| /26             | 59                      | 29                              |

<sup>*</sup>Part du principe que vous devrez effectuer un scale-up ou un scale-down en taille ou en SKU à un moment donné. 

Comme la taille du sous-réseau ne peut pas être modifiée après l’affectation, utilisez un sous-réseau suffisamment grand pour s’adapter à l’échelle que votre application est susceptible d’atteindre. Pour éviter tout problème de capacité du sous-réseau, vous devez utiliser un /26 avec 64 adresses.

Lorsque vous souhaitez que les applications de votre plan atteignent un réseau virtuel auquel sont déjà connectées des applications d’un autre plan, sélectionnez un sous-réseau différent de celui utilisé par l’intégration au réseau virtuel préexistante.

### <a name="routes"></a>Itinéraires

Il existe deux types de routage à prendre en compte lors de la configuration d’une intégration au réseau virtuel régional. Le routage des applications définit quel trafic est acheminé de votre application vers le réseau virtuel. Le routage du réseau permet de contrôler la manière dont le trafic est acheminé depuis votre réseau virtuel vers l’extérieur.

#### <a name="application-routing"></a>Routage d’applications

Lorsque vous configurez le routage des applications, vous pouvez acheminer tout le trafic ou seulement le trafic privé (également appelé trafic [RFC1918](https://datatracker.ietf.org/doc/html/rfc1918#section-3)) dans votre réseau virtuel. Cette configuration s’effectue par le biais du paramètre Tout acheminer. Si Tout acheminer est désactivé, votre application achemine uniquement le trafic privé vers votre réseau virtuel. Si vous souhaitez acheminer l’ensemble de votre trafic sortant vers votre réseau virtuel, assurez-vous que le paramètre Tout acheminer est activé.

> [!NOTE]
> * Lorsque Tout acheminer est activé, tout le trafic est soumis aux NSG et UDR appliqués à votre sous-réseau d’intégration. Lorsque l’acheminement de tout le trafic est activé, le trafic sortant est toujours envoyé à partir des adresses répertoriées dans les propriétés de votre application, sauf si vous fournissez des itinéraires qui dirigent le trafic ailleurs.
> 
> * Tout acheminer n’est actuellement pas pris en charge dans les conteneurs Windows.
>
> * L’intégration au réseau virtuel régional n’est pas en mesure d’utiliser le port 25.

Vous pouvez suivre les étapes suivantes pour désactiver Tout acheminer dans votre application via le portail : 

:::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-route-all-enabled.png" alt-text="Tout acheminer activé":::

1. Accédez à l’interface utilisateur **Intégration au réseau virtuel** dans votre portail d’application.
1. Définissez **Tout acheminer** sur Désactivé.
    
    :::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-route-all-disabling.png" alt-text="Désactiver Tout acheminer":::

1. Sélectionnez **Oui**.

Vous pouvez également configurer Tout acheminer à l’aide de CLI (*Remarque* : `az version` minimale requise est 2.27.0) :

```azurecli-interactive
az webapp config set --resource-group myRG --name myWebApp --vnet-route-all-enabled [true|false]
```

Le paramètre de configuration Tout acheminer est le moyen recommandé d’activer le routage de tout le trafic. L’utilisation du paramètre de configuration vous permet d’auditer le comportement avec une [stratégie intégrée](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F33228571-70a4-4fa1-8ca1-26d0aba8d6ef). Le paramètre d’application `WEBSITE_VNET_ROUTE_ALL` existant peut toujours être utilisé et vous pouvez activer tout le routage du trafic avec l’un ou l’autre des paramètres.

#### <a name="network-routing"></a>Routage réseau

Vous pouvez utiliser des tables de routage pour router le trafic sortant depuis votre application vers où vous voulez. Les tables de routage ont un impact sur le trafic de destination. Lorsque Tout acheminer est désactivé dans le [routage des applications](#application-routing), seul le trafic privé (RFC1918) est concerné par vos tables de routage. Les destinations courantes peuvent inclure des pare-feu ou des passerelles. Les routes définies sur votre sous-réseau d’intégration n’affectent pas les réponses aux requêtes d’application entrantes. 

Si vous souhaitez router tout le trafic sortant localement, vous pouvez utiliser une table de route pour envoyer l’intégralité du trafic sortant vers votre passerelle ExpressRoute. Si vous choisissez de router le trafic vers une passerelle, veillez à définir des routes dans le réseau externe pour renvoyer des réponses.

Les routes BGP (Border Gateway Protocol) affectent également le trafic de votre application. Si vous avez des routes BGP provenant par exemple d’une passerelle ExpressRoute, le trafic sortant de votre application est affecté. Semblable aux itinéraires définis par l’utilisateur, les itinéraires BGP ont un impact sur le trafic en fonction des paramètres d’étendue du routage.

### <a name="network-security-groups"></a>Groupes de sécurité réseau

Une application utilisant l’intégration au réseau virtuel régional peut utiliser un [groupe de sécurité réseau][VNETnsg] pour bloquer le trafic sortant vers des ressources dans votre réseau virtuel ou sur Internet. Pour bloquer le trafic vers des adresses publiques, vous devez vous assurer d’activer [Tout acheminer](#application-routing) vers le réseau virtuel. Lorsque Tout acheminer n’est pas activé, les NSG sont uniquement appliqués au trafic RFC1918.

Un NSG appliqué à votre sous-réseau d’intégration est en vigueur indépendamment des tables de routage appliquées à votre sous-réseau d’intégration. 

Les règles de trafic entrant dans un NSG ne s’appliquent pas à votre application, car l’intégration au réseau virtuel n’a d’incidence que sur le trafic sortant de votre application. Pour contrôler le trafic entrant vers votre application, utilisez la fonctionnalité Restrictions d’accès.

### <a name="service-endpoints"></a>Points de terminaison de service

L’intégration au réseau virtuel régional vous permet d’atteindre les services Azure sécurisés avec les points de terminaison de service. Pour accéder à un service sécurisé par point de terminaison de service, vous devez effectuer les opérations suivantes :

* Configurez l’intégration au réseau virtuel régional avec votre application web pour vous connecter à un sous-réseau spécifique pour l’intégration.
* Accédez au service de destination et configurez des points de terminaison de service sur le sous-réseau d’intégration.

### <a name="private-endpoints"></a>Instances Private Endpoint

Si vous souhaitez effectuer des appels vers des [points de terminaison privés][privateendpoints], vous devez vous assurer que vos recherches DNS seront résolues sur le point de terminaison privé. Vous pouvez appliquer ce comportement de l’une des façons suivantes : 

* Intégrer à des zones privées Azure DNS Si votre réseau virtuel n’a pas de serveur DNS personnalisé, cette opération est effectuée automatiquement lorsque les zones sont liées au réseau virtuel.
* Gérer le point de terminaison privé dans le serveur DNS utilisé par votre application. Pour ce faire, vous devez connaître l’adresse du point de terminaison privé, puis pointer le point de terminaison que vous essayez d’atteindre vers cette adresse à l’aide d’un enregistrement A.
* Configurez votre propre serveur DNS pour le transfert vers des zones privées Azure DNS.

### <a name="azure-dns-private-zones"></a>Zones privées Azure DNS 

Une fois votre application intégrée à votre réseau virtuel, elle utilise le même serveur DNS que celui avec lequel votre réseau virtuel est configuré. Si aucun DNS personnalisé n’est spécifié, elle utilise le DNS par défaut d’Azure et toute zone privée liée au réseau virtuel.

> [!NOTE]
> Pour les applications Linux, les zones privées Azure DNS fonctionnent uniquement si Tout acheminer est activé.

### <a name="limitations"></a>Limites

Il existe certaines limitations concernant l’utilisation de l’intégration au réseau virtuel avec les réseaux virtuels d’une même région :

* Vous ne pouvez pas accéder à des ressources via des connexions d’appairage mondiales.
* Vous ne pouvez pas atteindre des ressources sur des connexions de Peering avec des réseaux virtuels classiques.
* Cette fonctionnalité est disponible à partir de toutes les unités d’échelle App Service dans Premium v2 et Premium v3. Elle est également disponible dans Standard, mais uniquement à partir d’unités d’échelle App Service plus récentes. Si vous utilisez une unité d’échelle plus ancienne, vous ne pouvez utiliser la fonctionnalité qu’à partir d’un plan App Service Premium v2. Si vous souhaitez vous assurer de pouvoir utiliser la fonctionnalité dans un plan App Service Standard, créez votre application dans un plan App Service Premium v3. Ces plans ne sont pris en charge que sur les unités d’échelle les plus récentes. Vous pouvez effectuer un scale-down par la suite si vous le souhaitez.  
* Le sous-réseau d’intégration peut être utilisé par un seul plan App Service.
* La fonctionnalité ne peut pas être utilisée par des applications de plan Isolé qui se trouvent dans un environnement App Service.
* La fonctionnalité nécessite un sous-réseau inutilisé /28 ou d’une taille supérieure dans un réseau virtuel Azure Resource Manager.
* L’application et le réseau virtuel doivent être dans la même région.
* Vous ne pouvez pas supprimer un réseau virtuel avec une application intégrée. Supprimez l’intégration avant de supprimer le réseau virtuel.
* Vous ne pouvez disposer que d’une seule intégration au réseau virtuel régional par plan App Service. Plusieurs applications d’un même plan App Service peuvent utiliser le même réseau virtuel.
* Vous ne pouvez pas changer l’abonnement d’une application ou d’un plan quand une application utilise l’intégration au réseau virtuel régional.
* Votre application ne peut pas résoudre les adresses dans Azure DNS Private Zones sur les plans Linux sans modification de la configuration.

## <a name="gateway-required-vnet-integration"></a>Intégration au réseau virtuel avec passerelle obligatoire

L’intégration au réseau virtuel avec passerelle obligatoire prend en charge la connexion à un réseau virtuel d’une autre région ou à un réseau virtuel classique. Intégration au réseau virtuel avec passerelle obligatoire :

* Permet à une application de se connecter à un seul réseau virtuel à la fois.
* Permet d’intégrer jusqu’à cinq réseaux virtuels dans un plan App Service.
* Permet à un même réseau virtuel d’être utilisé par plusieurs applications dans le cadre d’un plan App Service sans impact sur le nombre total d’applications pouvant être utilisées par un plan App Service. Si vous avez six applications qui utilisent le même réseau virtuel dans le cadre d’un plan App Service, cela compte pour un réseau virtuel utilisé.
* Prend en charge un contrat SLA de 99,9 % du fait du contrat SLA de la passerelle.
* Permet à vos applications d’utiliser le DNS avec lequel le réseau virtuel est configuré.
* Nécessite une passerelle de réseau virtuel basée sur le routage et configurée avec un VPN de point à site SSTP avant de pouvoir être connecté à une application.

Vous ne pouvez pas utiliser l’intégration au réseau virtuel avec passerelle obligatoire :

* Avec un réseau virtuel connecté au moyen d’Azure ExpressRoute.
* À partir d’une application Linux.
* À partir d’un [conteneur Windows](quickstart-custom-container.md).
* Pour accéder à des ressources sécurisées de points de terminaison de service.
* Avec une passerelle de coexistence qui prend en charge à la fois les connexions ExpressRoute et les VPN de point à site ou site à site.

### <a name="set-up-a-gateway-in-your-azure-virtual-network"></a>Configurer une passerelle dans votre réseau virtuel Azure

Pour créer une passerelle :

1. [Créez un sous-réseau de passerelle][creategatewaysubnet] dans votre réseau virtuel.

1. [Créez la passerelle VPN][creategateway]. Sélectionnez un type de VPN basé sur les routes.

1. [Définissez les adresses de point à site][setp2saddresses]. Si la passerelle n’est pas dans la référence (SKU) de base, c’est que IKEV2 doit être désactivé dans la configuration de point à site et que SSTP doit être sélectionné. L’espace d’adressage de point à site doit se situer dans les blocs d’adresses RFC 1918, à savoir 10.0.0.0/8, 172.16.0.0/12 et 192.168.0.0/16.

Si vous créez la passerelle pour l’utiliser avec l’intégration au réseau virtuel App Service, il n’est pas nécessaire de charger un certificat. La création de la passerelle peut prendre 30 minutes. Vous ne serez pas en mesure d’intégrer votre application à votre réseau virtuel tant que la passerelle n’est pas provisionnée.

### <a name="how-gateway-required-vnet-integration-works"></a>Fonctionnement de l’intégration au réseau virtuel avec passerelle obligatoire

L’intégration au réseau virtuel avec passerelle obligatoire s’appuie sur la technologie VPN de point à site. Les VPN de point à site limitent l’accès réseau à la seule machine virtuelle qui héberge l’application. Les applications sont limitées au seul envoi du trafic vers Internet, via des connexions hybrides ou via l’intégration au réseau virtuel. Lorsque votre application est configurée avec le portail pour utiliser l’intégration au réseau virtuel avec passerelle obligatoire, une négociation complexe est managée en votre nom pour créer et attribuer des certificats sur la passerelle et du côté de l’application. Au final, les Workers utilisés pour héberger vos applications sont en mesure de se connecter directement à la passerelle de réseau virtuel, dans le réseau virtuel sélectionné.

:::image type="content" source="./media/web-sites-integrate-with-vnet/vnetint-how-gateway-works.png" alt-text="Fonctionnement de l’intégration au réseau virtuel avec passerelle obligatoire":::

### <a name="access-on-premises-resources"></a>Accès aux ressources locales

Les applications peuvent accéder aux ressources locales en s’intégrant à des réseaux virtuels qui ont des connexions site à site. Si vous utilisez l’intégration au réseau virtuel avec passerelle obligatoire, mettez à jour les routes de votre passerelle VPN locale avec vos blocs d’adresses de point à site. Lors de la configuration initiale de ce VPN site à site, les scripts utilisés pour le configurer doivent définir les routes correctement. Si vous ajoutez des adresses de point à site après avoir créé votre VPN de site à site, vous devez mettre à jour les routes manuellement. La procédure détaillée dépend de la passerelle et n’est pas décrite ici. Vous ne pouvez pas configurer BGP avec une connexion VPN de site à site.

Aucune configuration supplémentaire n’est nécessaire pour permettre à la fonctionnalité d’intégration au réseau virtuel régional d’accéder à vos ressources locales via votre réseau virtuel. Vous devez simplement connecter votre réseau virtuel à vos ressources locales à l’aide d’ExpressRoute ou d’un VPN de site à site.

> [!NOTE]
> La fonctionnalité d’intégration au réseau virtuel avec passerelle obligatoire n’intègre pas une application à un réseau virtuel doté d’une passerelle ExpressRoute. Même si la passerelle ExpressRoute est configurée en [mode de coexistence][VPNERCoex], l’intégration au réseau virtuel ne fonctionne pas. Si vous avez besoin d’accéder à des ressources via une connexion ExpressRoute, utilisez la fonctionnalité d’intégration au réseau virtuel régional ou un [environnement ASE (App Service Environment)][ASE], qui s’exécute dans votre réseau virtuel.
>
>

### <a name="peering"></a>Peering

Si vous utilisez le peering avec l’intégration au réseau virtuel régional, aucune configuration supplémentaire n’est nécessaire.

Si vous utilisez l’intégration au réseau virtuel avec passerelle obligatoire ainsi que le peering, vous devez configurer quelques éléments supplémentaires. Pour configurer le peering afin qu’il fonctionne avec votre application :

1. Ajoutez une connexion de peering sur le réseau virtuel auquel votre application se connecte. Lors de l’ajout de la connexion de peering, activez **Autoriser l’accès au réseau virtuel** et sélectionnez **Autoriser le trafic transféré** et **Autoriser le transit par passerelle**.
1. Ajoutez une connexion de peering sur le réseau virtuel à appairer au réseau virtuel auquel vous êtes connecté. Lors de l’ajout de la connexion de peering sur le réseau virtuel de destination, activez **Autoriser l’accès au réseau virtuel** et sélectionnez **Autoriser le trafic transféré** et **Autoriser les passerelles distantes**.
1. Accédez à l’interface utilisateur **Plan App Service** > **Mise en réseau** > **Intégration du réseau virtuel** dans le portail. Sélectionnez le réseau virtuel auquel votre application se connecte. Dans la section Routage, ajoutez la plage d’adresses du réseau virtuel qui est appairé au réseau virtuel auquel votre application est connectée.

## <a name="manage-vnet-integration"></a>Gérer l’intégration au réseau virtuel

Les connexions et déconnexions avec un réseau virtuel se font au niveau de l’application. Les opérations qui peuvent affecter l’intégration au réseau virtuel entre plusieurs applications s’effectuent au niveau du plan App Service. Sur le portail, sous application > **Mise en réseau** > **Intégration de réseau virtuel**, vous pouvez obtenir des détails sur votre réseau virtuel. Vous pouvez consulter des informations similaires au niveau du plan App Service dans le portail **Plan App Service** > **Mise en réseau** > **Intégration du réseau virtuel**.

La seule opération que vous pouvez effectuer dans la vue d’application de votre instance d’intégration au réseau virtuel consiste à déconnecter votre application du réseau virtuel auquel elle est actuellement connectée. Pour déconnecter votre application d’un réseau virtuel, sélectionnez **Déconnecter**. Votre application est redémarrée quand vous vous déconnectez d’un réseau virtuel. La déconnexion ne change pas votre réseau virtuel. Le sous-réseau ou la passerelle ne sont pas supprimés. Donc, si vous souhaitez supprimer votre réseau virtuel, déconnectez d’abord votre application du réseau virtuel puis supprimez les ressources qu’il contient comme les passerelles.

L’interface utilisateur de l’intégration au réseau virtuel du plan App Service vous montre toutes les intégrations au réseau virtuel utilisées par les applications de votre plan App Service. Pour voir des détails supplémentaires sur chaque réseau virtuel, sélectionnez le réseau virtuel qui vous intéresse. Vous pouvez effectuer deux actions ici pour l’intégration au réseau virtuel avec passerelle obligatoire :

* **Synchroniser le réseau** : L’opération de synchronisation du réseau s’adresse uniquement à la fonctionnalité d’intégration au réseau virtuel dépendante de la passerelle. L’exécution d’une opération de synchronisation du réseau est l’assurance que vos certificats et informations réseau sont synchronisés. Si vous ajoutez ou changez le DNS de votre réseau virtuel, effectuez une opération de synchronisation du réseau. Cette opération redémarre toutes les applications qui utilisent ce réseau virtuel. Cette opération ne fonctionnera pas si l’application et le réseau virtuel utilisés appartiennent à différents abonnements.
* **Ajouter des routes** : L’ajout de routes achemine le trafic sortant vers votre réseau virtuel.

L’adresse IP privée assignée à l’instance est exposée via la variable d’environnement **WEBSITE_PRIVATE_IP**. L’interface utilisateur de la console Kudu affiche également la liste des variables d’environnement disponibles pour l’application web. Cette adresse IP est attribuée à partir de la plage d’adresses du sous-réseau intégré. Pour l’intégration au réseau virtuel régional, la valeur de WEBSITE_PRIVATE_IP est une adresse IP de la plage d’adresses du sous-réseau délégué et, pour l’intégration au réseau virtuel avec passerelle obligatoire, la valeur est une adresse IP de la plage d’adresses du pool d’adresses point à site configuré sur la passerelle de réseau virtuel. Il s’agit de l’adresse IP qui sera utilisée par l’application web pour se connecter aux ressources via le réseau virtuel. 

> [!NOTE]
> La valeur de WEBSITE_PRIVATE_IP est appelée à changer. Toutefois, il s’agit d’une adresse IP de la plage d’adresses du sous-réseau d’intégration ou de la plage d’adresses de point à site. De ce fait, vous devrez autoriser l’accès depuis toutes les adresses de la plage.
>

### <a name="gateway-required-vnet-integration-routing"></a>Routage dans une intégration au réseau virtuel avec passerelle obligatoire
Les routes définies dans votre réseau virtuel sont utilisées pour diriger le trafic dans votre réseau virtuel à partir de votre application. Pour envoyer du trafic sortant supplémentaire dans le réseau virtuel, ajoutez ces blocs d’adresses ici. Cette fonctionnalité n’opère qu’avec l’intégration au réseau virtuel avec passerelle obligatoire. Les tables de route n’ont pas d’incidence sur le trafic de votre application lors de l’utilisation de l’intégration au réseau virtuel avec passerelle obligatoire, comme c’est le cas pour l’intégration au réseau virtuel régional.

### <a name="gateway-required-vnet-integration-certificates"></a>Certificats dans une intégration au réseau virtuel avec passerelle obligatoire
Quand l’intégration au réseau virtuel avec passerelle obligatoire est activée, un échange de certificats est nécessaire pour garantir la sécurité de la connexion. En plus des certificats, la configuration DNS, les routes et d’autres éléments similaires décrivent le réseau.

Si des certificats ou des informations du réseau sont modifiés, sélectionnez **Synchroniser le réseau**. Quand vous sélectionnez **Synchroniser le réseau**, la connectivité entre votre application et votre réseau virtuel est brièvement interrompue. Votre application n’est pas redémarrée, et la perte de connectivité peut altérer le fonctionnement correct de votre site.

## <a name="pricing-details"></a>Détails de la tarification
L’utilisation de la fonctionnalité d’intégration au réseau virtuel régional ne s’accompagne d’aucuns frais supplémentaires au-delà des frais liés au niveau tarifaire du plan App Service.

Trois types de frais sont appliqués en cas d’utilisation de la fonctionnalité d’intégration au réseau virtuel avec passerelle obligatoire :

* **Frais liés au niveau tarifaire du plan App Service** : Vos applications doivent être dans un plan App Service Standard, Premium, PremiumV2 ou PremiumV3. Pour plus d’informations sur ces frais, consultez [Tarification d’App Service][ASPricing].
* **Coût de transfert des données** : Les sorties de données engendrent des coûts, même si le réseau virtuel est dans le même centre de données. Ces coûts sont décrits dans [Détails de tarification des transferts de données][DataPricing].
* **Coûts de la passerelle VPN** : La passerelle de réseau virtuel nécessaire pour la connexion VPN de point à site engendre un coût. Pour plus d’informations, consultez [Tarification Passerelle VPN][VNETPricing].

## <a name="troubleshooting"></a>Dépannage

> [!NOTE]
> L’intégration au réseau virtuel n’est pas prise en charge pour les scénarios Docker Compose dans App Service.
> Les restrictions d’accès Azure Functions sont ignorées en présence d’un point de terminaison privé.
>

[!INCLUDE [app-service-web-vnet-troubleshooting](../../includes/app-service-web-vnet-troubleshooting.md)]

## <a name="automation"></a>Automatisation

Une prise en charge de l’interface CLI est disponible pour l’intégration au réseau virtuel régional. Pour accéder aux commandes suivantes, [installez l’interface de ligne de commande Azure][installCLI].

```azurecli
az webapp vnet-integration --help

Group
    az webapp vnet-integration : Methods that list, add, and remove virtual network
    integrations from a webapp.
        This command group is in preview. It may be changed/removed in a future release.
Commands:
    add    : Add a regional virtual network integration to a webapp.
    list   : List the virtual network integrations on a webapp.
    remove : Remove a regional virtual network integration from webapp.
```

La prise en charge de l’intégration au réseau virtuel régional par PowerShell est également disponible, mais vous devez créer une ressource générique avec un tableau de propriétés du sous-réseau resourceID.

```azurepowershell
# Parameters
$sitename = 'myWebApp'
$resourcegroupname = 'myRG'
$VNetname = 'myVNet'
$location = 'myRegion'
$integrationsubnetname = 'myIntegrationSubnet'
$subscriptionID = 'aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee'

# Property array with the SubnetID
$properties = @{
  subnetResourceId = "/subscriptions/$subscriptionID/resourceGroups/$resourcegroupname/providers/Microsoft.Network/virtualNetworks/$VNetname/subnets/$integrationsubnetname"
}

# Creation of the VNet Integration
$vNetParams = @{
  ResourceName = "$sitename/VirtualNetwork"
  Location = $location
  ResourceGroupName = $resourcegroupname
  ResourceType = 'Microsoft.Web/sites/networkConfig'
  PropertyObject = $properties
}
New-AzResource @vNetParams
```

<!--Links-->
[VNETOverview]: ../virtual-network/virtual-networks-overview.md
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: ../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw
[creategateway]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#creategw
[setp2saddresses]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#addresspool
[VNETRouteTables]: ../virtual-network/manage-route-table.md
[installCLI]: /cli/azure/install-azure-cli
[privateendpoints]: networking/private-endpoint.md
[VNETnsg]: /azure/virtual-network/security-overview/
