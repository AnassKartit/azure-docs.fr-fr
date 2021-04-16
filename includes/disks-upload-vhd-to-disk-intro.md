---
title: Fichier include
description: Fichier include
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/27/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e96d205ef1a8f94baa3a0cfe6c5127b6cf570e5a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "80420950"
---
Cet article explique comment charger un disque dur virtuel depuis votre machine locale vers un disque managé Azure ou copier un disque managé dans une autre région avec AzCopy. Ce processus, le chargement direct, vous permet aussi de charger un disque dur virtuel d’une taille maximale de 32 Tio directement dans un disque managé. Actuellement, le chargement direct est pris en charge pour les disques managés de type HDD Standard, SSD Standard et SSD Premium. Il n’est pas encore pris en charge pour les disques Ultra.

Si vous fournissez une solution de sauvegarde pour les machines virtuelles IaaS dans Azure, nous vous recommandons d’utiliser le chargement direct afin de restaurer les sauvegardes des clients sur des disques managés. Si vous chargez un disque dur virtuel dans Azure depuis une machine externe, les vitesses dépendront de votre bande passante locale. Lors du chargement ou de la copie depuis une machine virtuelle Azure, votre bande passante est identique à celle des disques durs standard.