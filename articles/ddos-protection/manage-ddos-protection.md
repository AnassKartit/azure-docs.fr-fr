---
title: Gérer le service Protection DDos Standard Azure à l’aide du portail Azure
description: Découvrez comment utiliser Azure DDoS Protection Standard pour atténuer une attaque.
services: ddos-protection
documentationcenter: na
author: aletheatoh
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: ddos-protection
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/17/2019
ms.author: yitoh
ms.openlocfilehash: 803d89dedc59c53001f078492d956e5e50d5411b
ms.sourcegitcommit: 851b75d0936bc7c2f8ada72834cb2d15779aeb69
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2021
ms.locfileid: "123305119"
---
# <a name="quickstart-create-and-configure-azure-ddos-protection-standard"></a>Démarrage rapide : Créer et configurer Azure DDoS Protection Standard

Prenez en main Azure DDoS Protection Standard à l’aide du portail Azure. 

Un plan de protection DDoS définit un ensemble de réseaux virtuels pour lesquels la protection DDoS standard est activée par le biais des abonnements. Vous pouvez configurer un plan de protection DDoS pour votre organisation et lier des réseaux virtuels à partir de plusieurs abonnements au même plan. 

Dans ce démarrage rapide, vous allez créer un plan de protection DDoS et le lier à un réseau virtuel. 

## <a name="prerequisites"></a>Prérequis

- Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.
- Connectez-vous au portail Azure sur https://portal.azure.com. Assurez-vous que votre compte dispose du rôle de [contributeur de réseau](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) ou d’un [rôle personnalisé](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) attribué aux actions appropriées répertoriées dans le guide pratique relatif aux [Autorisations](manage-permissions.md).

## <a name="create-a-ddos-protection-plan"></a>Créer un plan de protection DDoS

1. Sélectionnez **Créer une ressource** en haut à gauche du portail Azure.
2. Recherchez le terme *DDoS*. Lorsque **Plan de protection DDoS** apparaît dans les résultats de la recherche, sélectionnez cette entrée.
3. Sélectionnez **Create** (Créer).
4. Entrez ou sélectionnez les valeurs suivantes, puis sélectionnez **Créer** :

    |Paramètre        |Valeur                                              |
    |---------      |---------                                          |
    |Nom           | Entrez _MyDdosProtectionPlan_.                     |
    |Abonnement   | Sélectionnez votre abonnement.                         |
    |Resource group | Sélectionnez **Créer** et entrez _MyResourceGroup_.|
    |Emplacement       | Entrez _USA Est_.                                  |

## <a name="enable-ddos-protection-for-a-virtual-network"></a>Activer la protection DDoS pour un réseau virtuel

### <a name="enable-ddos-protection-for-a-new-virtual-network"></a>Activer la protection DDoS pour un nouveau réseau virtuel

1. Sélectionnez **Créer une ressource** en haut à gauche du portail Azure.
2. Sélectionnez **Mise en réseau**, puis **Réseau virtuel**.
3. Entrez ou sélectionnez les valeurs suivantes, acceptez les autres valeurs par défaut, puis sélectionnez **Créer** :

    | Paramètre         | Valeur                                           |
    | ---------       | ---------                                       |
    | Nom            | Entrez _MyVnet_.                                 |
    | Abonnement    | Sélectionnez votre abonnement.                                    |
    | Resource group  | Sélectionnez **Utiliser l’existant**, puis **MyResourceGroup**. |
    | Emplacement        | Entrez _USA Est_.                                                    |
    | DDoS Protection Standard | Sélectionnez **Activer**. Le plan que vous sélectionnez peut se trouver dans le même abonnement que le réseau virtuel, ou dans un abonnement différent, mais les deux abonnements doivent être associés au même locataire Azure Active Directory.|

Vous ne pouvez pas déplacer un réseau virtuel vers un autre groupe de ressources ou abonnement quand la protection DDoS Standard est activée pour le réseau virtuel. Si vous devez déplacer un réseau virtuel pour lequel la protection DDoS Standard est activée, désactivez d’abord celle-ci, déplacez le réseau virtuel, puis activez la protection DDoS Standard. Après le déplacement, les seuils de stratégie réglée automatiquement pour toutes les adresses IP publiques protégées dans le réseau virtuel sont réinitialisés.

### <a name="enable-ddos-protection-for-an-existing-virtual-network"></a>Activer la protection DDoS pour un réseau virtuel existant

1. Créez un plan de protection DDoS en effectuant les étapes décrites dans [Créer un plan de protection DDoS](#create-a-ddos-protection-plan), si vous n’avez pas de plan de protection DDoS.
2. Entrez le nom du réseau virtuel pour lequel vous souhaitez activer le service DDoS Protection Standard dans la zone **Rechercher dans les ressources, les services et les documents** en haut du Portail Azure. Quand le nom du réseau virtuel apparaît dans les résultats de la recherche, sélectionnez-le.
3. Sélectionnez **Protection DDoS**, sous **PARAMÈTRES**.
4. Sélectionnez **Standard**. Sous **Plan de protection DDoS**, sélectionnez un plan de protection DDoS existant, ou le plan que vous avez créé à l’étape 1, puis sélectionnez **Enregistrer**. Le plan que vous sélectionnez peut se trouver dans le même abonnement que le réseau virtuel, ou dans un abonnement différent, mais les deux abonnements doivent être associés au même locataire Azure Active Directory.

### <a name="enable-ddos-protection-for-all-virtual-networks"></a>Activer la protection DDoS pour tous les réseaux virtuels

Cette [stratégie intégrée](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F94de2ad3-e0c1-4caf-ad78-5d47bbc83d3d) détecte tous les réseaux virtuels configurés dans une étendue définie pour lesquels DDoS Protection Standard n’est pas activé, puis crée éventuellement une tâche de correction qui établit l’association pour protéger le réseau virtuel. Pour connaître la liste complète des stratégies intégrées, consultez [Définitions intégrées Azure Policy pour Azure DDoS Protection Standard](policy-reference.md). 

## <a name="validate-and-test"></a>Valider et tester

Tout d’abord, vérifiez les détails de votre plan de protection DDoS :

1. Sélectionnez **Tous les services** dans la partie supérieure gauche du portail.
2. Entrez *DDoS* dans la zone **Filtre**. Quand **Plans de protection DDoS** apparaît dans les résultats, sélectionnez cette entrée.
3. Sélectionnez votre plan de protection DDoS dans la liste.

Le réseau virtuel _MyVnet_ doit être répertorié. 

### <a name="view-protected-resources"></a>Afficher les ressources protégées
Sous **Ressources protégées**, vous pouvez afficher vos réseaux virtuels protégés et vos adresses IP publiques, ou ajouter d’autres réseaux virtuels à votre plan de protection DDoS :

![Afficher les ressources protégées](./media/manage-ddos-protection/ddos-protected-resources.png)

## <a name="clean-up-resources"></a>Nettoyer les ressources

Vous pouvez conserver vos ressources pour le tutoriel suivant. Si vous n’en avez plus besoin, supprimez le groupe de ressources _MyResourceGroup_. Lorsque vous supprimez le groupe de ressources, vous supprimez aussi le plan de protection DDoS et toutes ses ressources associées. Si vous n’envisagez pas d’utiliser ce plan de protection DDoS, vous devez supprimer les ressources pour éviter des frais inutiles.

   >[!WARNING]
   >Cette action est irréversible.

1. Dans le portail Azure, sélectionnez et recherchez **Groupes de ressources** ou sélectionnez **Groupes de ressources** dans le menu du portail Azure.

2. Filtrez ou faites défiler vers le bas pour rechercher le groupe de ressources _MyResourceGroup_.

3. Sélectionnez le groupe de ressources, puis **Supprimer le groupe de ressources**.

4. Tapez le nom du groupe de ressources pour vérifier, puis sélectionnez **Supprimer**.

Pour désactiver la protection DDoS pour un réseau virtuel : 

1. Entrez le nom du réseau virtuel pour lequel vous souhaitez désactiver le service DDoS Protection Standard dans la zone **Rechercher dans les ressources, les services et les documents** en haut du portail. Quand le nom du réseau virtuel apparaît dans les résultats de la recherche, sélectionnez-le.
2. Sous **DDoS Protection Standard**, sélectionnez **Désactiver**.

Si vous souhaitez supprimer un plan de protection DDoS, vous devez d’abord dissocier tous les réseaux virtuels de celui-ci. 

## <a name="next-steps"></a>Étapes suivantes

Pour savoir comment consulter et configurer la télémétrie pour votre plan de protection DDoS, passez aux tutoriels.

> [!div class="nextstepaction"]
> [Consulter et configurer la télémétrie de la protection DDoS](telemetry.md)
