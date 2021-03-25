---
title: Comportement des alertes SMS dans les groupes d’actions
description: Format de message SMS et réponse aux messages SMS pour annuler un abonnement, vous réinscrire ou demander de l’aide.
author: dkamstra
ms.author: dukek
services: monitoring
ms.topic: conceptual
ms.date: 02/16/2018
ms.openlocfilehash: e46517865ba01a0d4d113696fbadabf5ae7b0105
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102037929"
---
# <a name="sms-alert-behavior-in-action-groups"></a>Comportement des alertes SMS dans les groupes d’actions

## <a name="overview"></a>Vue d’ensemble 
Les groupes d’actions vous permettent de configurer une liste d’actions. Ces groupes sont utilisés lors de la définition d’alertes, ce qui permet de s’assurer qu’un groupe d’actions particulier est notifié au déclenchement de l’alerte. Une des actions prises en charge est le SMS ; les notifications par SMS prennent en charge la communication bidirectionnelle. Un utilisateur peut répondre à un SMS pour :

- **Se désabonner des alertes :** Un utilisateur peut annuler son abonnement à toutes les alertes par SMS pour tous les groupes d’actions ou un groupe d’actions particulier.
- **Se réabonner aux alertes :** Un utilisateur peut se réabonner à toutes les alertes par SMS pour tous les groupes d’actions ou un groupe d’actions particulier.  
- **Demander de l’aide :** Un utilisateur peut demander plus d’informations sur le SMS. Il est alors redirigé vers cet article.

Cet article traite du comportement des alertes SMS et des actions de réponse que l’utilisateur peut entreprendre en fonction des paramètres régionaux de l’utilisateur :

## <a name="receiving-an-sms-alert"></a>Réception d’une alerte SMS
Un destinataire de SMS, configuré dans le cadre d’un groupe d’actions, reçoit un SMS quand une alerte se déclenche. Ce SMS contient les informations suivantes :
* Le nom court du groupe d’actions auquel cette alerte a été envoyée
* Titre de l’alerte

| REPLY | Description |
| ----- | ----------- |
| DISABLE `<Action Group Short name>` | Désactive les futurs SMS en provenance du groupe d’actions |
| ENABLE `<Action Group Short name>` | Réactive les SMS en provenance du groupe d’actions |
| STOP | Désactive les futurs SMS en provenance de tous les groupes d’actions |
| START | Réactive les SMS en provenance de tous les groupes d’actions |
| HELP | Une réponse est envoyée à l’utilisateur avec un lien vers cet article. |

>[!NOTE]
>Si un utilisateur a annulé son abonnement aux alertes SMS, mais est ensuite ajouté à un nouveau groupe d’actions ; il RECEVRA les alertes SMS pour ce nouveau groupe d’actions, mais restera désabonné de tous les groupes d’actions précédents.

## <a name="next-steps"></a>Étapes suivantes
Obtenir une [vue d’ensemble des alertes de journal d’activité](./alerts-overview.md) et découvrir comment recevoir des alertes  
En savoir plus sur la [restriction des SMS](alerts-rate-limiting.md)  
En savoir plus sur les [groupes d’actions](./action-groups.md)