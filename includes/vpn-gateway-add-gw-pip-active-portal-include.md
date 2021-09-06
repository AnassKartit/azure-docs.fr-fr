---
title: Fichier Include
description: Fichier include
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 07/16/2021
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: ff30bf22f0f435db8ae0888440bb0ea26948bef3
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/22/2021
ms.locfileid: "114481216"
---
4. Spécifiez les valeurs pour **Adresse IP publique**. Ces paramètres spécifient l’objet d’adresse IP publique associé à la passerelle VPN. L’adresse IP publique est attribuée dynamiquement à cet objet pendant la création de la passerelle VPN. Pour les passerelles qui ne sont pas [redondantes interzones](../articles/vpn-gateway/about-zone-redundant-vnet-gateways.md), l’adresse IP publique change uniquement lorsque la passerelle est supprimée, puis recréée. Elle n’est pas modifiée lors du redimensionnement, de la réinitialisation ou des autres opérations de maintenance/mise à niveau internes de votre passerelle VPN.

   :::image type="content" source="./media/vpn-gateway-add-gw-pip-active-portal/pip-details.png" alt-text="Capture d’écran du champ Adresse IP publique.":::

     * **Adresse IP publique** : Laissez l’option **Créer** sélectionnée.
     * **Nom de l’adresse IP publique** : Dans la zone de texte, tapez un nom pour votre instance d’adresse IP publique.
     * **Attribution** : La passerelle VPN prend en charge seulement le mode dynamique.
     * **Activer le mode Actif-Actif** : sélectionnez **Activé**.
     * **Deuxième adresse IP publique** : sélectionnez **Créer**.
     * **Nom de l’adresse IP publique :** nom de la deuxième adresse IP publique.
     * Laissez l’option **Configurer BGP** définie sur **Désactivé**, sauf si votre configuration exige spécifiquement ce paramètre. Si vous avez besoin de ce paramètre, la valeur par défaut ASN est 65515, bien que vous puissiez la modifier.
5. Sélectionnez **Vérifier + créer** pour exécuter la validation.
6. Une fois la validation réussie, sélectionnez **Créer** pour déployer la passerelle VPN.