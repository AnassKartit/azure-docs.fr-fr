---
title: Créer un nom de domaine complet pour une machine virtuelle sur le Portail Azure
description: Apprenez à créer un nom de domaine complet (FQDN) pour une machine virtuelle dans le portail Azure.
author: cynthn
ms.service: virtual-machines
ms.subservice: networking
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 05/07/2021
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ad48a8d4c2f10bab26e04bcb105747e7a7c474f9
ms.sourcegitcommit: 58d82486531472268c5ff70b1e012fc008226753
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/23/2021
ms.locfileid: "122696511"
---
# <a name="create-a-fully-qualified-domain-name-for-a-vm-in-the-azure-portal"></a>Créer un nom de domaine complet pour une machine virtuelle dans le portail Azure

**S’applique à :** **S’applique à :** :heavy_check_mark: machines virtuelles Linux :heavy_check_mark: machines virtuelles Windows

Lorsque vous créez une machine virtuelle dans le [portail Azure](https://portal.azure.com), une ressource d’adresse IP publique est créée automatiquement pour la machine virtuelle. Vous utilisez cette adresse IP publique pour accéder à distance à la machine virtuelle. Bien que le portail ne crée pas de [nom de domaine complet](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN), vous pouvez en ajouter un après la création de la machine virtuelle. Cet article explique les étapes pour créer un nom DNS ou un nom de domaine complet. Si vous créez une machine virtuelle sans adresse IP publique, vous ne pouvez pas créer de nom de domaine complet.

## <a name="create-a-fqdn"></a>Créer un nom de domaine complet
Cet article suppose que vous avez déjà créé une machine virtuelle. Si nécessaire, vous pouvez créer une machine virtuelle [Linux](./linux/quick-create-portal.md) ou [Windows](./windows/quick-create-portal.md) dans le portail. Une fois que votre machine virtuelle est en cours d’exécution, procédez comme suit :


1. Sélectionnez votre machine virtuelle dans le portail. 
1. Dans le menu de gauche, sélectionnez **Propriétés**.
1. Sous **Adresse IP publique\Étiquette du nom DNS**, sélectionnez votre adresse IP.
2. Sous **Étiquette du nom DNS**, entrez le préfixe à utiliser.
3. En haut de la page, sélectionnez **Enregistrer**.
4. Sélectionnez **Vue d’ensemble** dans le menu de gauche pour revenir à la vue d’ensemble de la machine virtuelle.
5. Vérifiez que le **nom DNS** s’affiche correctement. 

## <a name="next-steps"></a>Étapes suivantes

Vous pouvez aussi gérer DNS avec des [zones DNS Azure](../dns/dns-getstarted-portal.md).

