---
title: Résoudre les problèmes de connexion à l’abonnement Azure
description: Aide à résoudre les problèmes de connexion au portail Azure.
services: cost-management-billing
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: troubleshooting
ms.date: 07/16/2021
ms.author: banders
ms.openlocfilehash: 7fad89f2ef4898e3fb790860abbb56593e9d06f1
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/03/2021
ms.locfileid: "131423125"
---
# <a name="troubleshoot-azure-subscription-sign-in-issues"></a>Résoudre les problèmes de connexion à l’abonnement Azure

Ce guide aide à résoudre les problèmes de connexion au portail Azure.

> [!NOTE]
> Si vous rencontrez des problèmes pour vous inscrire à un nouveau compte Azure, consultez [Résoudre les problèmes d’inscription aux abonnements Azure](./troubleshoot-azure-sign-up.md).

## <a name="page-hangs-in-the-loading-status"></a>La page se bloque à l’état de chargement

Si la page de votre navigateur Internet se bloque, essayez chacune des étapes suivantes jusqu’à ce que vous puissiez accéder au Portail Azure.

- Actualisez la page.
- Utilisez un autre navigateur Internet.
- Utilisez le mode de navigation privée de votre navigateur :

   - **Edge :** ouvrez **Paramètres** (les trois points en regard de l’image de votre profil), sélectionnez **Nouvelle fenêtre InPrivate**, puis accédez au [portail Azure](https://portal.azure.com/) et connectez-vous-y. 
   - **Chrome :** choisissez le mode **Navigation privée**.
   - **Safari :** choisissez **Fichier**, puis **Nouvelle fenêtre privée**.

- Effacez la mémoire cache et supprimez les cookies Internet :

   - **Edge :** ouvrez **Paramètres** et sélectionnez **Confidentialité et services**. Suivez les étapes sous **Effacer les données de navigation**. Vérifiez que les cases **Historique de navigation**, **Historique des téléchargements** et **Images et fichiers mis en cache** sont cochées, puis sélectionnez **Supprimer**.
   - **Chrome :** choisissez **Paramètres** et sélectionnez **Effacer les données de navigation** sous **Confidentialité et sécurité**.

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Vous êtes automatiquement connecté comme un autre utilisateur

Ce problème peut se produire si vous utilisez plusieurs comptes d’utilisateur dans un navigateur Internet.

Pour résoudre le problème, essayez l’une des méthodes suivantes :

- Effacez la mémoire cache et supprimez les cookies Internet.

   - **Edge :** ouvrez **Paramètres** et sélectionnez **Confidentialité et services**. Suivez les étapes sous **Effacer les données de navigation**. Vérifiez que les cases **Historique de navigation**, **Historique des téléchargements**, **Cookies** et **Images et fichiers mis en cache** sont cochées, puis sélectionnez **Supprimer**.
   - **Chrome :** choisissez **Paramètres** et sélectionnez **Effacer les données de navigation** sous **Confidentialité et sécurité**.
- Rétablissez les valeurs par défaut des paramètres de votre navigateur.
- Utilisez le mode de navigation privée de votre navigateur. 
   - **Edge :** ouvrez **Paramètres** (les trois points en regard de l’image de votre profil), sélectionnez **Nouvelle fenêtre InPrivate**, puis accédez au [portail Azure](https://portal.azure.com/) et connectez-vous-y. 
   - **Chrome :** choisissez le mode **Navigation privée**.
   - **Safari :** choisissez **Fichier**, puis **Nouvelle fenêtre privée**.

## <a name="i-can-sign-in-but-i-see-the-error-no-subscriptions-found"></a>Je peux me connecter mais l’erreur Aucun abonnement trouvé s’affiche

Ce problème se produit si vous avez sélectionné le mauvais annuaire ou si votre compte ne dispose pas des autorisations suffisantes.

**Scénario :** vous recevez l’erreur quand vous vous connectez au [portail Azure](https://portal.azure.com/)

Pour résoudre ce problème :

- Vérifiez que le répertoire Azure correct est sélectionné en sélectionnant votre compte dans l’angle supérieur droit.
- Si le répertoire Azure correct est sélectionné, mais que vous recevez néanmoins le messager d’erreur, [faites ajouter votre compte en tant que Propriétaire](./add-change-subscription-administrator.md).

## <a name="additional-help-resources"></a>Ressources d'aide supplémentaires

Autres articles sur la résolution des problèmes liés à la facturation et aux abonnements Azure

- [Carte refusée](./troubleshoot-declined-card.md)
- [Problèmes de souscription à l’abonnement](./troubleshoot-azure-sign-up.md)
- [Aucun abonnement trouvé](./no-subscriptions-found.md)
- [Désactivation de l’affichage des coûts d’entreprise](./enterprise-mgmt-grp-troubleshoot-cost-view.md)
- [Documentation sur la facturation Azure](../index.yml)

## <a name="contact-us-for-help"></a>Contactez-nous pour obtenir de l'aide

Si vous avez des questions ou si vous avez besoin d’aide mais que vous ne pouvez pas vous connecter au Portail Azure, [créez une demande de support](https://support.microsoft.com/oas/?prid=15470).