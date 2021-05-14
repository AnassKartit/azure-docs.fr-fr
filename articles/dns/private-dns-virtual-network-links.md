---
title: Qu’est-ce qu’une sous-ressource de liaison de réseau virtuel dans les zones Azure DNS privées ?
description: Vue d’ensemble d’une sous-ressource de liaison de réseau virtuel dans une zone Azure DNS privée
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 04/26/2021
ms.author: rohink
ms.openlocfilehash: 9e8ed0e21f1a6a98915dd3db1d5b8c2dcc1d6103
ms.sourcegitcommit: 02d443532c4d2e9e449025908a05fb9c84eba039
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/06/2021
ms.locfileid: "108763286"
---
# <a name="what-is-a-virtual-network-link"></a>Qu’est-ce qu’une liaison de réseau virtuel ?

Après avoir créé une zone DNS privée dans Azure, vous devez lier un réseau virtuel à celle-ci. Après liaison, les machines virtuelles hébergées dans ce réseau virtuel peuvent accéder à la zone DNS privée. Chaque zone DNS privée a une collection de ressources enfants de liaisons de réseau virtuel. Chacune de ces ressources représente une connexion à un réseau virtuel. Un réseau virtuel ne peut être lié qu’à une seule zone DNS privée en tant que réseau virtuel d’inscription ou de résolution.

## <a name="registration-virtual-network"></a>Réseau virtuel d’inscription

Lors de la [création d’une liaison](./private-dns-getstarted-portal.md#link-the-virtual-network) entre une zone DNS privée et un réseau virtuel, vous avez la possibilité d’activer l’[inscription automatique](./private-dns-autoregistration.md). Si vous activez ce paramètre, le réseau virtuel devient un réseau virtuel d’inscription pour la zone DNS privée. Un enregistrement DNS est automatiquement créé pour les machines virtuelles que vous déployez dans le réseau virtuel. Des enregistrements DNS sont créés pour les machines virtuelles que vous avez déjà déployées dans le réseau virtuel.

Du côté du réseau virtuel, la zone DNS privée devient la zone d’inscription pour ce réseau virtuel. Une zone DNS privée peut avoir plusieurs réseaux virtuels d’inscription. Toutefois, chaque réseau virtuel ne peut être associé qu’à une seule zone d’inscription.

## <a name="resolution-virtual-network"></a>Réseau virtuel de résolution

Si vous choisissez de lier votre réseau virtuel à la zone DNS privée sans inscription automatique, le réseau virtuel est traité comme un réseau virtuel de résolution uniquement. Les enregistrements DNS pour les machines virtuelles déployées sur ce réseau virtuel ne sont pas créées automatiquement dans la zone privée. Toutefois, ces machines virtuelles peuvent interroger correctement les enregistrements DNS à partir de la zone privée. Ces enregistrements incluent les enregistrements créés manuellement et ceux inscrits automatiquement à partir d’autres réseaux virtuels liés à la zone DNS privée.

Une zone DNS privée peut avoir plusieurs réseaux virtuels de résolution et un réseau virtuel peut être associé à plusieurs zones de résolution.

## <a name="limits"></a>limites

Pour savoir combien de réseaux d’inscription et de résolution vous pouvez lier à des zones DNS privées, consultez [Limites d’Azure DNS](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-dns-limits).

## <a name="other-considerations"></a>Autres éléments à prendre en compte

* Les réseaux virtuels déployés selon le modèle de déploiement Classic ne sont pas pris en charge.

* Vous ne pouvez créer qu’une seule liaison entre une zone DNS privée et un réseau virtuel.

* Chaque liaison de réseau virtuel sous une zone DNS privée doit avoir un nom unique dans le contexte de la zone DNS privée. Il peut y avoir des liaisons du même nom dans des zones DNS privées différentes.

* Après la création d’une liaison de réseau virtuel, vérifiez le champ « État du lien » de la ressource de liaison de réseau virtuel. Selon la taille du réseau virtuel, un délai de quelques minutes peut s’écouler avant que la liaison soit opérationnelle et que l’état de la liaison passe à *Terminé*.

* Quand vous supprimez un réseau virtuel, toutes les liaisons de réseau virtuel et les enregistrements DNS inscrits automatiquement qui y sont associés dans les différentes zones DNS privées sont automatiquement supprimés.

## <a name="next-steps"></a>Étapes suivantes

* Découvrez comment lier un réseau virtuel à une zone DNS privée à l’aide du [portail Azure](./private-dns-getstarted-portal.md#link-the-virtual-network).

* Découvrez comment créer une zone privée dans Azure DNS à l’aide [d’Azure PowerShell](./private-dns-getstarted-powershell.md) ou [d’Azure CLI](./private-dns-getstarted-cli.md).

* Passez en revue certains [scénarios de zones privées](./private-dns-scenarios.md) courants qui peuvent être réalisés avec des zones privées dans Azure DNS.

* Pour trouver des réponses aux questions fréquemment posées concernant les zones privées dans Azure DNS, notamment le comportement spécifique auquel vous attendre pour certains types d’opérations, voir le [FAQ sur les DNS privés](./dns-faq-private.yml).