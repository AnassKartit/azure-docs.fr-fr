---
title: Nom de domaine complet d’infrastructure pour le Pare-feu Azure
description: Le Pare-feu Azure comprend un regroupement de règles intégré pour les noms de domaine complets d’infrastructure qui sont autorisés par défaut.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: d4de81bad1ddda2f5b54f47c1be78925ad4cdbf3
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "74168977"
---
# <a name="infrastructure-fqdns"></a>Noms de domaine complets d’infrastructure

Le Pare-feu Azure comprend un regroupement de règles intégré pour les noms de domaine complets d’infrastructure qui sont autorisés par défaut. Ces noms de domaine complets sont spécifiques à la plateforme et ne peuvent pas être utilisés à d’autres fins. 

Les services suivants sont inclus dans le regroupement de règles intégré :

- Accès Compute au référentiel d’images de la plateforme (PIR) de stockage
- Accès de stockage de l’état des disques managés
- Diagnostics Azure et journalisation (MDS)

## <a name="overriding"></a>Remplacement 

Vous pouvez remplacer ce regroupement de règles d’infrastructure intégré en créant un regroupement de règles d’application Refuser tout qui sera traité en dernier. Il sera toujours traité avant le groupe de règles d’infrastructure. Tout ce qui ne se trouve pas dans le regroupement de règles d’infrastructure est refusé par défaut.

## <a name="next-steps"></a>Étapes suivantes

- Découvrez comment [déployer et configurer un Pare-feu Azure](tutorial-firewall-deploy-portal.md).