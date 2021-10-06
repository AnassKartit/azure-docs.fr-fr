---
title: Ajouter des réplicas à Azure Cache pour Redis
description: Découvrez comment ajouter des réplicas à vos instances Azure Cache pour Redis de niveau Premium
author: curib
ms.author: cauribeg
ms.service: cache
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: ca535f7f30db66bf527c00b737dc43ee1a7f1603
ms.sourcegitcommit: c27f71f890ecba96b42d58604c556505897a34f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2021
ms.locfileid: "129538230"
---
# <a name="add-replicas-to-azure-cache-for-redis"></a>Ajouter des réplicas à Azure Cache pour Redis

Dans cet article, vous allez apprendre à configurer une instance Azure Cache pour Redis avec des réplicas supplémentaires à l'aide du portail Azure.

Les niveaux Standard et Premium d’Azure Cache pour Redis offrent une redondance en hébergeant chaque cache sur deux machines virtuelles dédiées. Ces machines virtuelles sont configurées en tant que machine virtuelle principale et réplica. Lorsque la machine virtuelle principale devient indisponible, le réplica la détecte et prend le relais automatiquement comme nouveau réplica principal. Vous pouvez maintenant augmenter le nombre de réplicas dans un cache Premium jusqu’à trois, ce qui vous donne un total de quatre machines virtuelles pour assurer la sauvegarde d’un cache. Le fait de disposer de plusieurs réplicas entraîne une résilience supérieure à celle fournie par un seul réplica.

## <a name="prerequisites"></a>Prérequis

* Abonnement Azure : [créez-en un gratuitement](https://azure.microsoft.com/free/)

## <a name="create-a-cache"></a>Création d'un cache

Pour créer un cache, procédez comme suit :

1. Connectez-vous au [portail Azure](https://portal.azure.com) et sélectionnez **Créer une ressource**.
  
1. Dans la page **Nouvelle**, sélectionnez **Bases de données**, puis **Azure Cache pour Redis**.

    :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="Sélectionnez Azure Cache pour Redis.":::

1. Dans la page **Général**, configurez les paramètres du nouveau cache.

    | Paramètre      | Valeur suggérée  | Description |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Abonnement** | Sélectionnez votre abonnement. | Abonnement sous lequel créer cette nouvelle instance d’Azure Cache pour Redis. |
    | **Groupe de ressources** | Sélectionnez un groupe de ressources ou choisissez **Créer nouveau**, puis entrez un nouveau nom de groupe de ressources. | Nom du groupe de ressources dans lequel créer votre cache et d’autres ressources. En plaçant toutes les ressources de votre application dans un seul groupe de ressources, vous pouvez facilement les gérer ou les supprimer ensemble. |
    | **Nom DNS** | Entrez un nom globalement unique. | Le nom du cache doit être une chaîne de 1 à 63 caractères ne contenant que des chiffres, des lettres et des traits d’union. Le nom doit commencer et se terminer par un chiffre ou une lettre, et ne peut pas contenir de traits d’union consécutifs. Le *nom d’hôte* de votre instance de cache sera *\<DNS name>.redis.cache.windows.net*. |
    | **Lieu** | Sélectionnez un emplacement. | Choisissez une [Région](https://azure.microsoft.com/regions/) proche d’autres services qui utiliseront votre cache. |
    | **Type de cache** | Sélectionnez un cache de [niveau Premium](https://azure.microsoft.com/pricing/details/cache/). |  Le niveau tarifaire détermine la taille, les performances et les fonctionnalités disponibles pour le cache. Pour plus d’informations, consultez [Présentation du cache Azure pour Redis](cache-overview.md). |

1. Dans la page **Avancé**, choisissez **Nombre de réplicas**.

    :::image type="content" source="media/cache-how-to-multi-replicas/create-multi-replicas.png" alt-text="Nombre de réplicas.":::

    > [!NOTE]
    > Actuellement, vous ne pouvez pas utiliser la persistance AOF (append-only file) ou la géo-réplication avec plusieurs réplicas (plusieurs réplicas).
    >

1. Conservez les paramètres par défaut pour les autres options.

1. Sélectionnez **Create** (Créer).

    La création du cache prend un certain temps. Vous pouvez surveiller la progression dans la page **Vue d’ensemble** du Azure Cache pour Redis. Lorsque **État** indique **En cours d’exécution**, le cache est prêt pour utilisation.

    > [!NOTE]
    > Le nombre de réplicas d’un cache ne peut pas être modifié après la création de ce cache.
    >

## <a name="next-steps"></a>Étapes suivantes

En savoir plus sur les fonctionnalités d’Azure Cache pour Redis.

> [!div class="nextstepaction"]
> [Niveaux de service Premium Azure Cache pour Redis](cache-overview.md#service-tiers)
