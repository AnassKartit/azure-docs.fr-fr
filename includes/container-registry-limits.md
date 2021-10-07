---
title: fichier descriptif
description: Fichier include
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 08/12/2021
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 43baf554bda4c2b3ae9bda9d79b46035e073cead
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128582329"
---
| Ressource | De base | standard | Premium |
|---|---|---|---|
| Stockage inclus<sup>1</sup> (Gio) | 10 | 100 | 500 |
| Limite de stockage (Tio) | 20| 20 | 20 |
| Taille maximale du calque d’image (Gio) | 200 | 200 | 200 |
| Taille maximale du manifeste (Mio) | 4 | 4 | 4 |
| ReadOps par minute<sup>2, 3</sup> | 1 000 | 3 000 | 10 000 |
| WriteOps par minute<sup>2, 4</sup> | 100 | 500 | 2 000 |
| Bande passante de téléchargement<sup>2</sup> (Mbits/s) | 30 | 60 | 100 |
| Bande passante de chargement <sup>2</sup> (Mbits/s) | 10 | 20 | 50 |
| Webhooks | 2 | 10 | 500 |
| Géoréplication | N/A | N/A | [Pris en charge][geo-replication] |
| Zones de disponibilité | N/A | N/A | [Préversion][zones] |
| Approbation du contenu | N/A | N/A | [Pris en charge][content-trust] |
| Liaison privée avec points de terminaison privés | N/A | N/A | [Pris en charge][plink] |
| &bull; Points de terminaison privés | N/A | N/A | 10 |
| Règles de réseau d’adresses IP publiques | N/A | N/A | 100 |
| Accès au réseau virtuel du point de terminaison de service | N/A | N/A | [Préversion][vnet] |
| &bull; Règles de réseau virtuel | N/A | N/A | 100 |
| Clés gérées par le client | N/A | N/A | [Pris en charge][cmk] |
| Autorisations délimitées par le référentiel | N/A | N/A | [Préversion][token]|
| &bull; Jetons | N/A | N/A | 20 000 |
| &bull; Mappages d’étendue | N/A | N/A | 20 000 |
| &bull; Référentiels par mappage d’étendue | N/A | N/A | 500 |


<sup>1</sup> Stockage compris dans le tarif journalier pour chaque niveau. Vous pouvez utiliser du stockage supplémentaire (jusqu’à la limite de stockage du registre), auquel un tarif journalier supplémentaire par Gio vous sera facturé. Pour obtenir des informations sur les tarifs, consultez [Tarification d’Azure Container Registry][pricing]. Si vous avez besoin de davantage de stockage, contactez le Support Azure.

<sup>2</sup>Les valeurs *ReadOps*, *WriteOps* et *Bandwidth* sont des estimations minimales. Azure Container Registry s’efforce d’améliorer les performances en fonction de l’utilisation requise.

<sup>3</sup>[docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) se traduit par plusieurs opérations de lecture en fonction du nombre de couches dans l’image, plus la récupération du manifeste.

<sup>4</sup>[docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) se traduit par plusieurs opérations d’écriture, en fonction du nombre de couches qui doivent être envoyées. Un `docker push` inclut des *ReadOps* pour récupérer un manifeste pour une image existante.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
[vnet]: ../articles/container-registry/container-registry-vnet.md
[plink]: ../articles/container-registry/container-registry-private-link.md
[cmk]: ../articles/container-registry/container-registry-customer-managed-keys.md
[token]: ../articles/container-registry/container-registry-repository-scoped-permissions.md
[zones]: ../articles/container-registry/zone-redundancy.md
