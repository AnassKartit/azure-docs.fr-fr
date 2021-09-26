---
title: Utiliser des points de terminaison privés
titleSuffix: Azure Storage
description: Vue d’ensemble des points de terminaison privés pour un accès sécurisé aux comptes de stockage à partir de réseaux virtuels.
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/16/2021
ms.author: normesta
ms.reviewer: santoshc
ms.subservice: common
ms.openlocfilehash: a52460db452d519c51fb7a1b191766b21da67f88
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128592277"
---
# <a name="use-private-endpoints-for-azure-storage"></a>Utiliser des points de terminaison privés pour Stockage Azure

Vous pouvez utiliser des [points de terminaison privés](../../private-link/private-endpoint-overview.md) pour vos comptes Stockage Azure afin de permettre aux clients d’un réseau virtuel (VNet) d’accéder en toute sécurité aux données via une liaison [Private Link](../../private-link/private-link-overview.md). Le point de terminaison privé utilise une adresse IP de l’espace d’adressage du réseau virtuel pour votre service de compte de stockage. Le trafic réseau entre les clients sur le réseau virtuel et le compte de stockage traverse le réseau virtuel et une liaison privée sur le réseau principal de Microsoft, ce qui élimine l’exposition sur l’Internet public.

L’utilisation de points de terminaison privés pour votre compte de stockage vous permet d’effectuer les opérations suivantes :

- Sécurisez votre compte de stockage en configurant le pare-feu de stockage pour bloquer toutes les connexions sur le point de terminaison public pour le service de stockage.
- Améliorez la sécurité du réseau virtuel en vous permettant de bloquer l’exfiltration des données à partir du réseau virtuel.
- Connectez-vous en toute sécurité aux comptes de stockage à partir de réseaux locaux qui se connectent au réseau virtuel à l’aide de [VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md) ou [d’ExpressRoutes](../../expressroute/expressroute-locations.md) avec le peering privé.

## <a name="conceptual-overview"></a>Vue d'ensemble conceptuelle

![Vue d’ensemble des points de terminaison privés pour Stockage Azure](media/storage-private-endpoints/storage-private-endpoints-overview.jpg)

Un point de terminaison privé est une interface réseau spéciale pour un service Azure dans votre [réseau virtuel](../../virtual-network/virtual-networks-overview.md). Lorsque vous créez un point de terminaison privé pour votre compte de stockage, il offre une connectivité sécurisée entre les clients sur votre réseau virtuel et votre stockage. Une adresse IP est attribuée au point de terminaison privé à partir de la plage d’adresses IP de votre réseau virtuel. La connexion entre le point de terminaison privé et le service de stockage utilise une liaison privée.

Les applications du réseau virtuel peuvent se connecter en toute transparence au service de stockage sur le point de terminaison privé, **à l’aide des mêmes chaînes de connexion et mécanismes d’autorisation qu’ils utiliseraient dans tous les cas**. Les points de terminaison privés peuvent être utilisés avec tous les protocoles pris en charge par le compte de stockage, notamment REST et SMB.

Vous pouvez créer des points de terminaison privés dans des sous-réseaux qui utilisent des [points de terminaison de service](../../virtual-network/virtual-network-service-endpoints-overview.md). Les clients d’un sous-réseau peuvent donc se connecter à un compte de stockage à l’aide d’un point de terminaison privé, tout en utilisant des points de terminaison de service pour accéder à d’autres.

Quand vous créez un point de terminaison privé pour un service de stockage dans votre réseau virtuel, une demande de consentement est envoyée pour approbation au propriétaire du compte de stockage. Si l’utilisateur qui demande la création du point de terminaison privé est également propriétaire du compte de stockage, cette demande de consentement est automatiquement approuvée.

Les propriétaires de comptes de stockage peuvent gérer les demandes de consentement et les points de terminaison privés, via l’onglet « *Points de terminaison privés* » du compte de stockage dans le [portail Azure](https://portal.azure.com).

> [!TIP]
> Si vous souhaitez restreindre l’accès à votre compte de stockage uniquement par le biais du point de terminaison privé, configurez le pare-feu de stockage pour refuser ou contrôler l’accès via le point de terminaison public.

Vous pouvez sécuriser votre compte de stockage pour accepter uniquement les connexions à partir de votre réseau virtuel, en [configurant le pare-feu de stockage](storage-network-security.md#change-the-default-network-access-rule) afin de refuser l’accès via son point de terminaison public par défaut. Vous n’avez pas besoin d’une règle de pare-feu pour autoriser le trafic à partir d’un réseau virtuel doté d’un point de terminaison privé, puisque le pare-feu de stockage contrôle uniquement l’accès via le point de terminaison public. Les points de terminaison privés reposent plutôt sur le flux de consentement pour accorder l’accès aux sous-réseaux au service de stockage.

> [!NOTE]
> Lors de la copie d’objets blob entre des comptes de stockage, votre client doit disposer d’un accès réseau aux deux comptes. Par conséquent, si vous choisissez d’utiliser une liaison privée pour un seul compte (la source ou la destination), assurez-vous que votre client dispose d’un accès réseau à l’autre compte. Pour en savoir plus sur les autres méthodes de configuration de l’accès réseau, consultez [Configurer des réseaux virtuels et des pare-feu de stockage Azure](storage-network-security.md?toc=/azure/storage/blobs/toc.json).

<a id="private-endpoints-for-azure-storage"></a>

## <a name="creating-a-private-endpoint"></a>Création d’un point de terminaison privé

Pour créer un point de terminaison privé à l’aide du portail Azure, consultez [Se connecter en privé à un compte de stockage à partir de l’expérience Compte de stockage dans le portail Azure](../../private-link/tutorial-private-endpoint-storage-portal.md).

Pour créer un point de terminaison privé à l’aide de PowerShell ou d’Azure CLI, consultez l’un de ces articles. Tous deux présentent une application web Azure en tant que service cible, mais les étapes de création d’une liaison privée sont les mêmes que pour un compte de stockage Azure.

- [Créer un point de terminaison privé à l’aide d’Azure CLI](../../private-link/create-private-endpoint-cli.md)

- [Créer un point de terminaison privé à l’aide d’Azure PowerShell](../../private-link/create-private-endpoint-powershell.md)

Lorsque vous créez un point de terminaison privé, vous devez spécifier le compte de stockage et le service de stockage auxquels il se connecte.

Vous avez besoin d’un point de terminaison privé distinct pour chacune des ressources de stockage auxquelles vous devez accéder, à savoir [Objets Blob](../blobs/storage-blobs-overview.md), [Data Lake Storage Gen2](../blobs/data-lake-storage-introduction.md), [Fichiers](../files/storage-files-introduction.md), [Files d’attente](../queues/storage-queues-introduction.md), [Tables](../tables/table-storage-overview.md) et [Sites web statiques](../blobs/storage-blob-static-website.md). Sur le point de terminaison privé, ces services de stockage sont définis en tant que **sous-ressource cible** du compte de stockage associé.

Si vous créez un point de terminaison privé pour la ressource de stockage Data Lake Storage Gen2, vous devez également en créer un pour la ressource de stockage Objets Blob. En effet, les opérations qui ciblent le point de terminaison Data Lake Storage Gen2 peuvent être redirigées vers le point de terminaison d’objet blob. En créant un point de terminaison privé pour les deux ressources, vous vous assurez que les opérations réussissent.

> [!TIP]
> Créez un point de terminaison privé distinct pour l’instance secondaire du service de stockage afin d’améliorer les performances de lecture sur les comptes RA-GRS.
> Veillez à créer un compte de stockage v2 (Standard ou Premium) universel.

Pour accéder en lecture à la région secondaire avec un compte de stockage configuré pour le stockage géoredondant, vous devez disposer de points de terminaison privés distincts pour les instances principale et secondaire du service. Vous n’avez pas besoin de créer un point de terminaison privé pour l’instance secondaire pour le **basculement**. Le point de terminaison privé se connecte automatiquement à la nouvelle instance principale après le basculement. Pour plus d'informations sur les options de redondance du stockage, consultez [Redondance du Stockage Azure](storage-redundancy.md).

<a id="connecting-to-private-endpoints"></a>

## <a name="connecting-to-a-private-endpoint"></a>Connexion à un point de terminaison privé

Les clients sur un réseau virtuel utilisant le point de terminaison privé doivent utiliser la même chaîne de connexion pour le compte de stockage que les clients se connectant au point de terminaison public. Nous nous appuyons sur la résolution DNS pour acheminer automatiquement les connexions entre le réseau virtuel et le compte de stockage via une liaison privée.

> [!IMPORTANT]
> Utilisez la même chaîne de connexion pour vous connecter au compte de stockage avec des points de terminaison privés, comme vous le feriez dans le cas contraire. Veuillez ne pas vous connecter au compte de stockage à l’aide de son URL de sous-domaine `privatelink`.

Nous créons une [zone DNS privée](../../dns/private-dns-overview.md) attachée au réseau virtuel avec les mises à jour nécessaires pour les points de terminaison privés, par défaut. Toutefois, si vous utilisez votre propre serveur DNS, vous devrez peut-être apporter des modifications supplémentaires à votre configuration DNS. La section sur les [modifications DNS](#dns-changes-for-private-endpoints) ci-dessous décrit les mises à jour requises pour les points de terminaison privés.

## <a name="dns-changes-for-private-endpoints"></a>Modifications DNS pour les points de terminaison privés

Quand vous créez un point de terminaison privé, l’enregistrement de la ressource CNAME DNS pour le compte de stockage est mis à jour en un alias dans un sous-domaine avec le préfixe `privatelink`. Par défaut, nous créons également une [zone DNS privée](../../dns/private-dns-overview.md) correspondant au sous-domaine `privatelink`, avec les enregistrements de ressource DNS A pour les points de terminaison privés.

Lorsque vous résolvez l’URL du point de terminaison de stockage à l’extérieur du réseau virtuel avec le point de terminaison privé, elle correspond au point de terminaison public du service de stockage. En cas de résolution à partir du réseau virtuel hébergeant le point de terminaison privé, l’URL du point de terminaison de stockage correspond à l’adresse IP du point de terminaison privé.

Pour l’exemple illustré ci-dessus, les enregistrements de ressources DNS pour le compte de stockage « StorageAccountA », lorsqu’ils sont résolus à l’extérieur du réseau virtuel hébergeant le point de terminaison privé, sont les suivants :

| Nom                                                  | Type  | Valeur                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| `StorageAccountA.blob.core.windows.net`             | CNAME | `StorageAccountA.privatelink.blob.core.windows.net` |
| `StorageAccountA.privatelink.blob.core.windows.net` | CNAME | \<storage service public endpoint\>                   |
| \<storage service public endpoint\>                   | Un     | \<storage service public IP address\>                 |

Comme mentionné précédemment, vous pouvez refuser ou contrôler l’accès pour les clients en dehors du réseau virtuel via le point de terminaison public à l’aide du pare-feu de stockage.

Les enregistrements de ressources DNS pour StorageAccountA, lorsqu’ils sont résolus par un client dans le réseau virtuel hébergeant le point de terminaison privé, sont les suivants :

| Nom  | Type | Valeur |
| :--- | :---: | :--- |
| `StorageAccountA.blob.core.windows.net` | CNAME | `StorageAccountA.privatelink.blob.core.windows.net` |
| `StorageAccountA.privatelink.blob.core.windows.net` | Un | `10.1.1.5` |

Cette approche permet d’accéder au compte de stockage **avec la même chaîne de connexion** pour les clients sur le réseau virtuel hébergeant les points de terminaison privés, ainsi que des clients en dehors du réseau virtuel.

Si vous utilisez un serveur DNS personnalisé sur votre réseau, les clients doivent pouvoir résoudre le nom de domaine complet du point de terminaison du compte de stockage vers l’adresse IP du point de terminaison privé. Vous devez configurer votre serveur DNS de manière à déléguer votre sous-domaine de liaison privée à la zone DNS privée du réseau virtuel, ou configurer les enregistrements A pour `StorageAccountA.privatelink.blob.core.windows.net` avec l'adresse IP du point de terminaison privé.

> [!TIP]
> Lorsque vous utilisez un serveur DNS personnalisé ou local, configurez votre serveur DNS de façon à résoudre le nom du compte de stockage dans le sous-domaine `privatelink` vers l’adresse IP du point de terminaison privé. Pour ce faire, vous pouvez déléguer le sous-domaine `privatelink` à la zone DNS privée du réseau virtuel, ou configurer la zone DNS sur votre serveur DNS et ajouter les enregistrements A DNS.

Les noms de zone DNS recommandés pour les points de terminaison privés des services de stockage et les sous-ressources cibles de point de terminaison associées sont les suivants :

| Service de stockage        | Sous-ressource cible | Nom de la zone                            |
| :--------------------- | :------------------ | :----------------------------------- |
| Service d'objets blob           | objet BLOB                | `privatelink.blob.core.windows.net`  |
| Data Lake Storage Gen2 | dfs                 | `privatelink.dfs.core.windows.net`   |
| Service Fichier           | fichier                | `privatelink.file.core.windows.net`  |
| Service File d’attente          | queue               | `privatelink.queue.core.windows.net` |
| Service Table          | table               | `privatelink.table.core.windows.net` |
| Sites web statiques        | web                 | `privatelink.web.core.windows.net`   |

Pour plus d’informations sur la configuration de votre propre serveur DNS pour la prise en charge des points de terminaison privés, reportez-vous aux articles suivants :

- [Résolution de noms pour des ressources dans les réseaux virtuels Azure](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)
- [Configuration DNS pour les points de terminaison privés](../../private-link/private-endpoint-overview.md#dns-configuration)

## <a name="pricing"></a>Tarifs

Pour plus d’informations sur les tarifs, consultez [Tarification Liaison privée Azure](https://azure.microsoft.com/pricing/details/private-link).

## <a name="known-issues"></a>Problèmes connus

Gardez à l’esprit les problèmes connus suivants concernant les points de terminaison privés pour le Stockage Azure.

### <a name="storage-access-constraints-for-clients-in-vnets-with-private-endpoints"></a>Contraintes d’accès au stockage pour les clients dans des réseaux virtuels avec des points de terminaison privés

Les clients dans réseaux virtuels avec des points de terminaison privés existants sont soumis à des contraintes lors de l’accès à d’autres comptes de stockage qui ont des points de terminaison privés. Par exemple, supposons qu’un réseau virtuel N1 possède un point de terminaison privé pour un compte de stockage A1 pour le service Stockage Blob. Si le compte de stockage A2 possède un point de terminaison privé dans un réseau virtuel N2 pour le stockage Blob, les clients dans le réseau virtuel N1 doivent également accéder au stockage Blob du compte A2 à l’aide d’un point de terminaison privé. Si le compte de stockage A2 ne possède pas de points de terminaison privés pour le stockage Blob, les clients dans le réseau virtuel N1 peuvent accéder au stockage Blob de ce compte sans point de terminaison privé.

Cette contrainte résulte des modifications DNS effectuées lorsque le compte A2 crée un point de terminaison privé.

### <a name="network-security-group-rules-for-subnets-with-private-endpoints"></a>Règles de groupe de sécurité réseau pour les sous-réseaux avec des points de terminaison privés

Actuellement, vous ne pouvez pas configurer de règles de [groupe de sécurité réseau](../../virtual-network/network-security-groups-overview.md) (NSG, Network Security Group) ni de routes définies par l’utilisateur pour des points de terminaison privés. Les règles NSG appliquées au sous-réseau qui héberge le point de terminaison privé ne sont pas appliquées au point de terminaison privé. Elles s’appliquent uniquement à d’autres points de terminaison (par exemple les contrôleurs d’interface réseau). Une solution de contournement limitée pour ce problème consiste à implémenter vos règles d’accès pour les points de terminaison privés sur les sous-réseaux sources, bien que cette approche puisse nécessiter une charge de gestion supérieure.

### <a name="copying-blobs-between-storage-accounts"></a>Copie des objets blob entre des comptes de stockage

Vous pouvez copier des objets blob entre des comptes de stockage à l’aide de points de terminaison privés uniquement si vous utilisez l’API REST Azure ou des outils utilisant l’API REST. Ces outils incluent AzCopy, Explorateur Stockage, Azure PowerShell, Azure CLI et les kits de développement logiciel (SDK) Stockage Blob Azure.

Seuls les points de terminaison privés ciblant la ressource de stockage Blob sont pris en charge. Les points de terminaison privés ciblant Data Lake Storage Gen2 ou la ressource de fichier ne sont pas pris en charge pour le moment. De même, la copie entre comptes de stockage à l’aide du protocole NFS (Network File System) n’est pas encore prise en charge.

## <a name="next-steps"></a>Étapes suivantes

- [Configurer des pare-feu et des réseaux virtuels dans Stockage Azure](storage-network-security.md)
- [Recommandations de sécurité pour Stockage Blob](../blobs/security-recommendations.md)
