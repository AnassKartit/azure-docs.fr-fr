---
title: Télécharger une liste de groupes dans le portail Azure Active Directory | Microsoft Docs
description: Téléchargez des propriétés de groupe en bloc dans le centre d’administration Azure dans Azure Active Directory.
services: active-directory
author: curtand
ms.author: curtand
manager: daveba
ms.date: 09/01/2021
ms.topic: how-to
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.custom: it-pro
ms.reviewer: jeffsta
ms.collection: M365-identity-device-management
ms.openlocfilehash: 32000f69719b957c9543180bbd118f93ed0fda04
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2021
ms.locfileid: "123430681"
---
# <a name="bulk-download-a-list-of-groups-in-azure-active-directory"></a>Télécharger une liste de groupes par lot dans Azure Active Directory

À l’aide du portail Azure Active Directory (Azure AD), vous pouvez télécharger en bloc la liste de tous les groupes de votre organisation dans un fichier de valeurs séparées par des virgules (CSV).

## <a name="to-download-a-list-of-groups"></a>Pour télécharger une liste de groupes

1. Connectez-vous [au portail Azure](https://portal.azure.com) avec un compte administrateur dans l’organisation.
1. Dans Azure AD, sélectionnez **Groupes** > **Télécharger les groupes**.
1. Dans la page de **Téléchargement de groupes**, sélectionnez **Démarrer** pour recevoir un fichier CSV qui répertorie vos groupes.

   ![La commande Télécharger les groupes se trouve sur la page Tous les groupes](./media/groups-bulk-download/bulk-download.png)

## <a name="check-download-status"></a>Vérifier l’état du téléchargement

Vous pouvez voir l’état de toutes vos demandes d’opération en bloc en attente dans la page **Résultats de l’opération en bloc**.

[![Vérifier l'état sur la page Résultats des opérations en bloc.](./media/groups-bulk-download/bulk-center.png)](./media/groups-bulk-download/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>Limites du service de téléchargement en bloc

Chaque opération en bloc de téléchargement de liste de groupes peut prendre jusqu’à une heure. Cela vous permet de télécharger une liste d’au moins 300 000 groupes.

## <a name="next-steps"></a>Étapes suivantes

- [Supprimer en bloc les membres d’un groupe](groups-bulk-remove-members.md)
- [Télécharger les membres d’un groupe](groups-bulk-download-members.md)
