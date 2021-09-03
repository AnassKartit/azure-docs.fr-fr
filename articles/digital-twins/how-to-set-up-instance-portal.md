---
title: Configurer une instance et l’authentification (portail)
titleSuffix: Azure Digital Twins
description: Découvrez comment configurer une instance du service Azure Digital Twins à l’aide du portail Azure
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.custom: contperf-fy21q2, subject-rbac-steps
ms.openlocfilehash: f520ed1054c5d0d9dd6c053e22a984a813840131
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/22/2021
ms.locfileid: "114468773"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-portal"></a>Configurer une instance Azure Digital Twins et l’authentification (portail)

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Cet article explique comment **configurer une nouvelle instance Azure Digital Twins**, notamment la création de l’instance et la configuration de l’authentification. À l’issue de cet article, vous aurez une instance Azure Digital Twins prête pour la programmation.

Cette version de cet article suit ces étapes manuellement, une par une, à l’aide du portail Azure. Le portail Azure est une console web unifiée qui offre une alternative aux outils en ligne de commande.
* Pour suivre ces étapes manuellement à l’aide de CLI, consultez la version CLI de cet article : [Configurer une instance et l’authentification (CLI)](how-to-set-up-instance-cli.md).
* Pour exécuter une configuration automatisée à l’aide d’un exemple de script de déploiement, consultez la version avec script de cet article : [Configurer une instance et l’authentification (procédure scriptée)](how-to-set-up-instance-scripted.md).

[!INCLUDE [digital-twins-setup-steps.md](../../includes/digital-twins-setup-steps.md)]

## <a name="create-the-azure-digital-twins-instance"></a>Créer l’instance Azure Digital Twins

[!INCLUDE [digital-twins-setup-portal.md](../../includes/digital-twins-setup-portal.md)]

3. Sur la page **Créer une ressource**, renseignez les valeurs ci-dessous :
    * **Abonnement**: Abonnement Azure que vous utilisez.
      - **Groupe de ressources** : Groupe de ressources dans lequel déployer l’instance. Si vous n’avez pas de groupe de ressources existant à l’esprit, vous pouvez en créer un ici en sélectionnant le lien *Créer* et en entrant un nom pour le nouveau groupe de ressources.
    * **Emplacement** : Région prenant en charge Azure Digital Twins pour le déploiement. Pour plus d’informations sur la prise en charge régionale, visitez [Produits Azure disponibles par région (Azure Digital Twins)](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins) .
    * **Nom de la ressource** : Nom de votre instance Azure Digital Twins. Si votre abonnement a une autre instance Azure Digital Twins dans la région qui utilise déjà le nom spécifié, vous êtes invité à choisir un autre nom.
    * **Accorder l’accès à la ressource** : Cochez la case de cette section pour autoriser votre compte Azure à accéder aux données de l’instance et à les gérer. Si vous êtes la personne qui gérera l’instance, vous devez cocher cette case maintenant. Si elle est grisée parce que vous n’avez pas l’autorisation dans l’abonnement, vous pouvez continuer à créer la ressource et demander ultérieurement à une personne ayant les autorisations requises de vous accorder le rôle. Pour plus d’informations sur ce rôle et l’attribution de rôles à votre instance, consultez la section suivante : [Configurer les autorisations d’accès utilisateur](#set-up-user-access-permissions).

    :::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins-2.png" alt-text="Capture d’écran du processus de création de ressources pour Azure Digital Twins dans le portail Azure. Les valeurs décrites sont renseignées.":::

4. Lorsque vous avez terminé, vous pouvez sélectionner **Vérifier + créer** si vous ne souhaitez pas configurer d’autres paramètres pour votre instance. Une page de résumé s’affiche, dans laquelle vous pouvez consulter les détails de l’instance que vous avez entrés, avant d’appuyer sur **Créer**. 

    Si vous ne souhaitez pas configurer d’autres détails pour votre instance, la section suivante décrit les autres onglets d’installation.

### <a name="additional-setup-options"></a>Options d’installation supplémentaires

Voici les options supplémentaires que vous pouvez configurer lors de l’installation en utilisant les autres onglets du processus **Créer une ressource**.

* **Réseau** : Dans cet onglet, vous pouvez activer des points de terminaison privés avec [Azure Private Link](../private-link/private-link-overview.md) afin d’éliminer l’exposition au réseau public pour votre instance. Pour obtenir des instructions, consultez [Activer l’accès privé avec Private Link (préversion)](./how-to-enable-private-link.md?tabs=portal#add-a-private-endpoint-during-instance-creation).
* **Avancé** : Dans cet onglet, vous pouvez activer une identité managée par le système pour votre instance, qui peut être utilisée lors du transfert d’événements vers des [points de terminaison](concepts-route-events.md). Pour plus d’informations sur l’utilisation d’identités gérées par le système avec Azure Digital Twins, consultez [Sécurité pour les solutions Azure Digital Twins](concepts-security.md#managed-identity-for-accessing-other-resources-preview).
* **Étiquettes** : Dans cet onglet, vous pouvez ajouter des balises à votre instance pour vous aider à les organiser parmi vos ressources Azure. Pour plus d’informations sur les balises de ressources Azure, consultez [Baliser les ressources, les groupes de ressources et les abonnements pour l’organisation logique](../azure-resource-manager/management/tag-resources.md).

### <a name="verify-success-and-collect-important-values"></a>Vérifier la réussite de l’exécution et collecter les valeurs importantes

Après avoir terminé la configuration de votre instance et appuyé sur **Créer**, vous pouvez afficher l’état du déploiement de votre instance dans vos notifications Azure le long de la barre d’icônes du portail. La notification indique quand le déploiement a réussi, et vous pourrez sélectionner le bouton _Accéder à la ressource_ pour afficher votre instance créée.

:::image type="content" source="media/how-to-set-up-instance/portal/notifications-deployment.png" alt-text="Capture d’écran des notifications Azure montrant un déploiement réussi et mettant en évidence le bouton « Accéder à la ressource » dans le portail Azure":::

Autrement, si le déploiement échoue, la notification indique pourquoi. Lisez le conseil du message d’erreur, puis réessayez de créer l’instance.

>[!TIP]
>Une fois votre instance créée, vous pouvez revenir à sa page à tout moment en recherchant son nom dans la barre de recherche du portail Azure.

À partir de la page *Vue d’ensemble* de l’instance, notez son *Nom*, son *Groupe de ressources* et son *Nom d’hôte*. Il s’agit de toutes les valeurs importantes dont vous pouvez avoir besoin quand vous continuez à travailler avec votre instance Azure Digital Twins. Si d’autres utilisateurs doivent programmer pour l’instance, vous devez partager ces valeurs avec eux.

:::image type="content" source="media/how-to-set-up-instance/portal/instance-important-values.png" alt-text="Capture d’écran du portail Azure, mettant en évidence les valeurs importantes de la page de présentation de l’instance Azure Digital Twins":::

Vous disposez maintenant d’une instance Azure Digital Twins opérationnelle. Ensuite, vous allez accorder les autorisations utilisateur Azure appropriées pour la gérer.

## <a name="set-up-user-access-permissions"></a>Configurer les autorisations d’accès utilisateur

[!INCLUDE [digital-twins-setup-role-assignment.md](../../includes/digital-twins-setup-role-assignment.md)]

Il existe deux façons de créer une attribution de rôle pour un utilisateur dans Azure Digital Twins :
* [Lors de la création d’une instance Azure Digital Twins](#assign-the-role-during-instance-creation)
* [En utilisant la gestion des identités et des accès Azure (IAM)](#assign-the-role-using-azure-identity-management-iam)

Ces deux méthodes requièrent les mêmes autorisations.

### <a name="prerequisites-permission-requirements"></a>Configuration requise : Spécifications relatives aux autorisations

[!INCLUDE [digital-twins-setup-permissions.md](../../includes/digital-twins-setup-permissions.md)]

### <a name="assign-the-role-during-instance-creation"></a>Attribuer le rôle lors de la création de l’instance

Lors de la création de votre ressource Azure Digital Twins via le processus décrit [plus haut dans cet article](#create-the-azure-digital-twins-instance), sélectionnez l’option **Attribuer le rôle Propriétaire des données Azure Digital Twins** sous **Accorder l’accès à la ressource**. Vous obtiendrez ainsi un accès complet aux API du plan de données.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins-2-role.png" alt-text="Capture d’écran du processus de création de ressources pour Azure Digital Twins dans le portail Azure. La case à cocher sous Accorder l’accès à la ressource est en surbrillance.":::

Si vous n’avez pas l’autorisation d’attribuer un rôle à une identité, la case apparaît grisée.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins-2-role-greyed.png" alt-text="Capture d’écran du processus de création de ressources pour Azure Digital Twins dans le portail Azure. La case à cocher sous Accorder l’accès à la ressource est désactivée.":::

Dans ce cas, vous pouvez toujours continuer à créer la ressource Azure Digital Twins, mais une personne ayant les autorisations appropriées devra vous attribuer ce rôle ou l’attribuer à la personne qui gérera les données de l’instance.

### <a name="assign-the-role-using-azure-identity-management-iam"></a>Attribuer le rôle en utilisant la gestion des identités et des accès Azure (IAM)

Vous pouvez également attribuer le rôle **Propriétaire des données Azure Digital Twins** à l’aide des options de contrôle d’accès dans la gestion des identités et des accès Azure (IAM).

1. Commencez par ouvrir la page de votre instance Azure Digital Twins dans le portail Azure. 

1. Sélectionnez **Contrôle d’accès (IAM)** .

1. Sélectionnez **Ajouter** > **Ajouter une attribution de rôle** pour ouvrir la page Ajouter une attribution de rôle.

1. Attribuez le rôle **Propriétaires des données Azure Digital Twins**. Pour connaître les étapes détaillées, consultez [Attribuer des rôles Azure à l’aide du portail Azure](../role-based-access-control/role-assignments-portal.md).
    
    | Paramètre | Valeur |
    | --- | --- |
    | Role | [Propriétaire des données Azure Digital Twins](../role-based-access-control/built-in-roles.md#azure-digital-twins-data-owner) |
    | Attribuer l’accès à | Utilisateur, groupe ou principal de service |
    | Membres | Recherchez le nom ou l’adresse e-mail de l’utilisateur à attribuer. |
    
    ![Page Ajouter une attribution de rôle](../../includes/role-based-access-control/media/add-role-assignment-page.png)

### <a name="verify-success"></a>Vérifier la réussite de l’exécution

Vous pouvez afficher l’attribution de rôle que vous avez configurée sous *Contrôle d’accès (IAM) > Attributions de rôle*. L’utilisateur doit s’afficher dans la liste avec le rôle *Propriétaire des données Azure Digital Twins*. 

:::image type="content" source="media/how-to-set-up-instance/portal/verify-role-assignment.png" alt-text="Capture d’écran des attributions de rôles pour une instance Azure Digital Twins dans le portail Azure":::

Vous disposez maintenant d’une instance Azure Digital Twins prête à l’emploi et des autorisations pour la gérer.

## <a name="next-steps"></a>Étapes suivantes

Testez les appels d’API REST individuels sur votre instance à l’aide des commandes CLI d’Azure Digital Twins : 
* [az dt reference](/cli/azure/dt?view=azure-cli-latest&preserve-view=true)
* [Ensemble de commandes CLI Azure Digital Twins](concepts-cli.md)

Vous pouvez également découvrir comment connecter une application cliente à votre instance avec un code d’authentification :
* [Écrire le code d’authentification de l’application](how-to-authenticate-client.md)