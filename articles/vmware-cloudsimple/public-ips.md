---
title: Azure VMware Solution by CloudSimple - Allouer des adresses IP publiques
description: Explique comment allouer des adresses IP publiques à des machines virtuelles dans un environnement de cloud privé
author: Ajayan1008
ms.author: v-hborys
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: bb46ad726cd3b99324e9bb96b998ed1b4da885de
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97899182"
---
# <a name="allocate-public-ip-addresses-for-private-cloud-environment"></a>Allouer des adresses IP publiques pour un environnement de cloud privé

Ouvrez l’onglet Public IPs de la page Network (Réseau) pour allouer des adresses IP publiques aux machines virtuelles de votre environnement de cloud privé.

1. [Accédez au portail CloudSimple](access-cloudsimple-portal.md), puis sélectionnez **Network** dans le menu latéral.
2. Sélectionnez **Public IPs**.
3. Cliquez sur **New Public IP**.

    ![Page d’IP publiques](media/public-ips-page.png)

4. Entrez un nom pour identifier l’entrée de l’adresse IP.
5. Conservez l’emplacement par défaut.
6. Utilisez le curseur pour changer le délai d’inactivité, si nécessaire.
7. Entrez l'adresse IP locale à laquelle vous voulez attribuer une adresse IP publique.
8. Entrez un nom DNS associé.
9. Cliquez sur **Envoyer**.

![Allouer des adresses IP publiques](media/network-public-ip-allocate.png)

La tâche d'attribution de l'adresse IP publique commence. Vous pouvez vérifier l'état de la tâche sur la page **Activity > Tasks** (Activité > Tâches). Une fois l'attribution terminée, la nouvelle entrée s’affiche sur la page des adresses IP publiques.
