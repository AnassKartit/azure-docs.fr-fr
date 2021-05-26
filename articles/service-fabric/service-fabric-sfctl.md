---
title: Interface CLI Azure Service Fabric - sfctl
description: Apprenez-en plus sur sfctl, l’interface de ligne de commande d’Azure Service Fabric. Comprend une liste de commandes et de sous-groupes.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 4721598961ae912e8f0a9ef2f61022e5feb39e6c
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/26/2021
ms.locfileid: "110462247"
---
# <a name="sfctl"></a>sfctl
Commandes permettant de gérer les clusters et entités Service Fabric. Cette version est compatible avec le runtime Service Fabric 7.0.

Les commandes suivent le modèle nom-verbe. Pour plus d’informations, consultez la section sur les sous-groupes.

## <a name="subgroups"></a>Sous-groupes
|Sous-groupe|Description|
| --- | --- |
| [application](service-fabric-sfctl-application.md) | Permet de créer, de supprimer et de gérer les applications et les types d’application. |
| [chaos](service-fabric-sfctl-chaos.md) | Permet de démarrer, d’arrêter et de créer des rapports sur le service de test chaos. |
| [cluster](service-fabric-sfctl-cluster.md) | Permet de sélectionner, de gérer et d’utiliser les clusters Service Fabric. |
| [compose](service-fabric-sfctl-compose.md) | Permet de créer, de supprimer et de gérer les applications Docker Compose. |
| [container](service-fabric-sfctl-container.md) | Exécutez des commandes relatives à un conteneur sur un nœud de cluster. |
| [events](service-fabric-sfctl-events.md) | Récupérez les événements du magasin d’événements (si le service EventStore est déjà installé). |
| [is](service-fabric-sfctl-is.md) | Interroge et envoie des commandes vers le service d’infrastructure. |
| [node](service-fabric-sfctl-node.md) | Permet de gérer les nœuds qui forment un cluster. |
| [partition](service-fabric-sfctl-partition.md) | Interroge et gère des partitions pour tout service. |
| [property](service-fabric-sfctl-property.md) | Stocke et interroge des propriétés avec des noms Service Fabric. |
| [replica](service-fabric-sfctl-replica.md) | Permet de gérer les réplicas qui font partie des partitions de service. |
| [rpm](service-fabric-sfctl-rpm.md) | Interroge et envoie des commandes vers le service gestionnaire de réparation. |
| [sa-cluster](service-fabric-sfctl-sa-cluster.md) | Permet de gérer les clusters Service Fabric autonomes. |
| [service](service-fabric-sfctl-service.md) | Permet de créer, de supprimer et de gérer le service, les types de service et les packages de services. |
| [settings](service-fabric-sfctl-settings.md) | Permet de configurer les paramètres locaux pour cette instance de sfctl. |
| [store](service-fabric-sfctl-store.md) | Effectue des opérations élémentaires au niveau des fichiers dans le magasin d’images de cluster. |

## <a name="next-steps"></a>Étapes suivantes
- [Configurez](service-fabric-cli.md) l’interface de ligne de commande Service Fabric.
- Découvrez comment utiliser l’interface de ligne de commande (CLI) Service Fabric à l’aide d’[exemples de scripts](./scripts/sfctl-upgrade-application.md).
