---
title: Résoudre les problèmes de connectivité Azure Private Endpoint
description: Guide pas à pas pour diagnostiquer la connectivité de point de terminaison privé
services: private-endpoint
documentationcenter: na
author: rdhillon
manager: narayan
editor: ''
ms.service: private-link
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2020
ms.author: rdhillon
ms.openlocfilehash: 0df95d90d0119f8bc513fe2a26ed731d87401b3d
ms.sourcegitcommit: df2a8281cfdec8e042959339ebe314a0714cdd5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2021
ms.locfileid: "129154258"
---
# <a name="troubleshoot-azure-private-endpoint-connectivity-problems"></a>Résoudre les problèmes de connectivité d’Azure Private Endpoint

Cet article fournit des instructions pas à pas pour valider et diagnostiquer votre configuration de connectivité de point de terminaison privé Azure.

Azure Private Endpoint est une interface réseau qui vous permet de vous connecter de façon privée et sécurisée à un service de liaisons privées. Cette solution vous aide à sécuriser vos charges de travail dans Azure en fournissant une connectivité privée à vos ressources de service Azure à partir de votre réseau virtuel, ce qui revient à placer ces services sur votre réseau virtuel.

Voici les scénarios de connectivité disponibles avec les points de terminaison privés :

- Réseau virtuel de la même région
- réseaux virtuels avec peering régional ;
- réseaux virtuels avec peering mondial ;
- Circuits Azure ExpressRoute ou client local via VPN

## <a name="diagnose-connectivity-problems"></a>Diagnostiquer les problèmes de connectivité 

Passez en revue ces étapes pour vous assurer que toutes les configurations habituelles sont telles que prévu, afin de résoudre les problèmes de connectivité liés à votre configuration de point de terminaison privé.

1. Passez en revue la configuration du point de terminaison privé en parcourant la ressource.

    a. Accédez au [Centre Private Link](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/PrivateLinkCenterBlade/overview).

      ![Centre de liaisons privées](./media/private-endpoint-tsg/private-link-center.png)

    b. Dans le volet gauche, sélectionnez **Points de terminaison privés**.
    
      ![Instances Private Endpoint](./media/private-endpoint-tsg/private-endpoints.png)

    c. Filtrez et sélectionnez le point de terminaison privé que vous souhaitez diagnostiquer.

    d. Passez en revue les informations relatives au réseau virtuel et au système DNS.
     - Vérifiez que l’état de la connexion indique **Approuvée**.
     - Vérifiez que la machine virtuelle dispose d’une connectivité au réseau virtuel qui héberge les points de terminaison privés.
     - Vérifiez que les informations de nom de domaine complet (copie) et d’adresse IP privée sont affectées.
    
       ![Configuration du réseau virtuel et du DNS](./media/private-endpoint-tsg/vnet-dns-configuration.png)
    
1. Utilisez [Azure Monitor](../azure-monitor/overview.md) pour vérifier que les données sont transmises.

    a. Sur la ressource du point de terminaison privé, sélectionnez **Superviser**.
     - Sélectionnez **Octets entrants** ou **Octets sortants**. 
     - Vérifiez que les données sont transmises lors de la tentative de connexion au point de terminaison privé. Prévoyez un retard d’environ 10 minutes.
    
       ![Vérifier la télémétrie du point de terminaison privé](./media/private-endpoint-tsg/private-endpoint-monitor.png)

1.  Utilisez la **résolution des problèmes de connexion de machine virtuelle** à partir d’Azure Network Watcher.

    a. Sélectionnez la machine virtuelle cliente.

    b. Sélectionnez **Résoudre les problèmes de connexion**, puis l’onglet **Connexions sortantes**.
    
      ![Network Watcher - Tester les connexions sortantes](./media/private-endpoint-tsg/network-watcher-outbound-connection.png)
    
    c. Sélectionnez **Utiliser Network Watcher pour le suivi détaillé des connexions**.
    
      ![Network Watcher - Résolution des problèmes de connexion](./media/private-endpoint-tsg/network-watcher-connection-troubleshoot.png)

    d. Sélectionnez **Tester par nom de domaine complet**.
     - Collez le nom de domaine complet à partir de la ressource Private Endpoint.
     - Spécifiez un port. En général, utilisez 443 pour Azure Storage ou Azure Cosmos DB, et 1336 pour SQL.

    e. Sélectionnez **Test** et validez les résultats des tests.
    
      ![Network Watcher - Résultats des tests](./media/private-endpoint-tsg/network-watcher-test-results.png)
    
        
1. La résolution DNS des résultats des tests doit avoir la même adresse IP privée assignée au point de terminaison privé.

    a. Si les paramètres DNS sont incorrects, procédez comme suit :
     - Si vous utilisez une zone privée : 
       - Vérifiez que le réseau virtuel de machine virtuelle client est associé à la zone privée.
       - Vérifiez que l'enregistrement de la zone DNS privée existe. Si ce n’est pas le cas, créez-le.
     - Si vous utilisez un DNS personnalisé :
       - vérifiez que vos paramètres DNS personnalisés et la configuration DNS sont corrects.
       Pour obtenir de l’aide, voir [Présentation du point de terminaison privé : configuration DNS](./private-endpoint-overview.md#dns-configuration).

    b. Si la connectivité est défaillante en raison de groupes de sécurité du réseau (NSG) ou d’itinéraires définis par l'utilisateur :
     - Passez en revue les règles de trafic sortant du groupe de sécurité réseau et créez des règles de trafic sortant appropriées pour autoriser le trafic.
    
       ![Règles de trafic sortant du groupe de sécurité réseau](./media/private-endpoint-tsg/nsg-outbound-rules.png)

1. L’itinéraire du tronçon suivant de l’adresse IP du point de terminaison privé de la machine virtuelle source doit être InterfaceEndpoints dans les itinéraires effectifs de la carte réseau. 

    a. Si vous ne voyez pas l’itinéraire du point de terminaison privé sur la machine virtuelle source, vérifiez les points suivants : 
     - Si la machine virtuelle source et le point de terminaison privé appartiennent au même réseau virtuel, vous devez contacter le support. 
     - Si la machine virtuelle source et le point de terminaison privé font partie de différents réseaux virtuels, vérifiez la connectivité IP entre les réseaux virtuels. S’il existe une connectivité IP et que vous ne voyez toujours pas l’itinéraire, contactez le support. 

1. Si la connexion a des résultats validés, le problème de connectivité est peut-être lié à d’autres aspects tels que les secrets, les jetons et les mots de passe au niveau de la couche application.
   - Dans ce cas, passez en revue la configuration de la ressource Private Link associée au point de terminaison privé. Pour plus d’informations, consultez le [Guide de résolution des problèmes Azure Private Link](troubleshoot-private-link-connectivity.md).
   
1. Il est toujours judicieux d’affiner le problème avant de déclencher le ticket de support. 

    a. En cas de problème de connexion de la source locale à un point de terminaison privé dans Azure, effectuez les tests suivants : 
      - Connectez-vous à une autre machine virtuelle locale et vérifiez si vous disposez d’une connectivité IP au réseau virtuel à partir d’un emplacement local. 
      - Connectez-vous au point de terminaison privé à partir d’une machine virtuelle du réseau virtuel.
      
    b. Si la source est Azure et que le point de terminaison privé se trouve dans un autre réseau virtuel, effectuez les tests suivants : 
      - Connectez-vous au point de terminaison privé à partir d’une autre source. Vous pourrez ainsi isoler les problèmes propres aux machines virtuelles. 
      - Connectez-vous à n’importe quelle machine virtuelle faisant partie du même réseau virtuel que le point de terminaison privé.  

1. Si le point d'extrémité privé est lié à un [service de liaison privé](./troubleshoot-private-link-connectivity.md) qui est lié à un équilibreur de charge, vérifiez si le pool backend est sain. La correction de l'état de santé de l'équilibreur de charge résoudra le problème de la connexion au point d'extrémité privé.

    - Vous pouvez voir un diagramme visuel ou une [vue des dépendances](../azure-monitor/insights/network-insights-overview.md#dependency-view) des ressources, métriques et Insights associés en accédant à :
        - Azure Monitor
        - Réseaux
        - Instances Private Endpoint
        - Affichage des dépendances 

![Superviser les réseaux](https://user-images.githubusercontent.com/20302679/134994620-0660b9e2-e2a3-4233-8953-d3e49b93e2f2.png)

![DependencyView](https://user-images.githubusercontent.com/20302679/134994637-fb8b4a1a-81d5-4723-b1c3-d7bdc72162f3.png)

9. Si votre problème n’est toujours pas résolu et que les difficultés de connectivité persistent, contactez l’équipe [Support Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="next-steps"></a>Étapes suivantes

 * [Créer un point de terminaison privé sur le sous-réseau mis à jour (portail Azure)](./create-private-endpoint-portal.md)
 * [Guide de résolution des problèmes de liaison privée Azure](troubleshoot-private-link-connectivity.md)
