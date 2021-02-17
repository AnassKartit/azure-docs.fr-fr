---
title: Application d’accès Azure Spring Cloud dans un réseau virtuel
description: Application d’accès dans Azure Spring Cloud dans un réseau virtuel.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 11/11/2020
ms.custom: devx-track-java
ms.openlocfilehash: 37c8b4bc186c217ecb27638f5f50297102345de7
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99576529"
---
# <a name="access-your-application-in-a-private-network"></a>Accéder à votre application sur un réseau privé

Ce document explique comment accéder à un point de terminaison pour votre application dans un réseau privé.  Pour obtenir l’accès, vous devez créer une **zone DNS privée Azure** dans votre abonnement afin de traduire/résoudre le nom de domaine complet privé en son adresse IP.

Lorsque **Affecter un point de terminaison** pour les applications dans une instance de service Azure Spring Cloud est déployé dans votre réseau virtuel, le point de terminaison est un nom de domaine complet privé. Le domaine est uniquement accessible dans le réseau privé. Les applications et les services utilisent le point de terminaison d’application. Ils comprennent le **point de terminaison de test** décrit dans [Affichage des applications et des déploiements](spring-cloud-howto-staging-environment.md#view-apps-and-deployments). La **diffusion en continu des journaux**, décrite dans [Envoyer en streaming les journaux d’application Azure Spring Cloud en temps réel](spring-cloud-howto-log-streaming.md), ne fonctionne que dans le réseau privé.

## <a name="create-a-private-dns-zone"></a>Créer une zone DNS privée

La procédure suivante crée une zone DNS privée pour une application dans le réseau privé.

1. Ouvrez le portail Azure. Dans la zone de recherche supérieure, recherchez **Zone DNS privée**, puis sélectionnez **Zones DNS privées** dans les résultats.

2. Sur la page **Zones DNS privées**, sélectionnez **+ Ajouter**.

3. Remplissez le formulaire sur la page **Créer une zone DNS privée**. Entrez **<span>private.azuremicroservices.io</span>** comme **Nom** de la zone.

4. Sélectionnez **Vérifier + créer**.

5. Sélectionnez **Create** (Créer).

La création de la zone peut prendre plusieurs minutes.

## <a name="link-the-virtual-network"></a>Lier le réseau virtuel

Pour lier la zone DNS privée au réseau virtuel, vous devez créer un lien de réseau virtuel.

1. Sélectionnez la ressource de zone DNS privée créée ci-dessus : **<span>private.azuremicroservices.io</span>** 

2. Dans le volet gauche, sélectionnez **Liens de réseau virtuel**.

3. Sélectionnez **Ajouter**.

4. Entrez **azure-spring-cloud-dns-link** pour le **Nom du lien**.

5. Pour **Réseau virtuel**, sélectionnez le réseau virtuel que vous avez créé en suivant la procédure décrite dans [Déployer Azure Spring Cloud dans votre réseau virtuel Azure (injection VNet)](spring-cloud-tutorial-deploy-in-azure-virtual-network.md).

    ![Ajouter un lien de réseau virtuel](media/spring-cloud-access-app-vnet/add-virtual-network-link.png)

6. Cliquez sur **OK**.

## <a name="create-dns-record"></a>Créer un enregistrement DNS

Pour utiliser la zone DNS privée pour traduire/résoudre le DNS, vous devez créer un enregistrement de type « A » dans la zone.

1. Sélectionnez la ressource de réseau virtuel que vous avez créée en suivant la procédure décrite dans [Déployer Azure Spring Cloud dans votre réseau virtuel Azure (injection VNet)](spring-cloud-tutorial-deploy-in-azure-virtual-network.md).

2. Dans la zone de recherche **Appareils connectés**, entrez *kubernetes-internal*.

3. Dans les résultats filtrés, recherchez l’**appareil** connecté au **sous-réseau** de runtime de service de l’instance de service, puis copiez son **Adresse IP**. Dans cet exemple, l’adresse IP est *10.1.0.7*.

    [![Créer un enregistrement DNS](media/spring-cloud-access-app-vnet/create-dns-record.png) ](media/spring-cloud-access-app-vnet/create-dns-record.png)

Vous pouvez également récupérer l’adresse IP à l’aide de la commande CLI az suivante :

```azurecli
SPRING_CLOUD_RG= # Resource group name of your Azure Spring Cloud service instance
SPRING_CLOUD= # Name of your Azure Spring Cloud service instance

SERVICE_RUNTIME_RG=`az spring-cloud show -g $SPRING_CLOUD_RG -n $SPRING_CLOUD --query \
"properties.networkProfile.serviceRuntimeNetworkResourceGroup" -o tsv`

IP_ADDRESS=`az network lb frontend-ip list --lb-name kubernetes-internal -g \
$SERVICE_RUNTIME_RG --query "[0].privateIpAddress" -o tsv`
```

4. Sélectionnez la ressource de zone DNS privée créée ci-dessus : **<span>private.azuremicroservices.io</span>** .

5. Sélectionnez **+ Jeu d’enregistrements**.

6. Dans **Ajouter un jeu d’enregistrements**, entrez ou sélectionnez ces informations :

    |Paramètre     |Valeur                                                                      |
    |------------|---------------------------------------------------------------------------|
    |Nom        |Entrez *\**                                                                 |
    |Type        |Sélectionnez **A**                                                               |
    |TTL         |Entrez *1*                                                                  |
    |Unité de durée de vie    |Sélectionnez **Heures**                                                           |
    |Adresse IP  |Entrez l’adresse IP copiée à l’étape 3. Dans l’exemple, entrez *10.1.0.7*.    |

    Sélectionnez ensuite **OK**.

    ![Ajouter un enregistrement de zone DNS privée](media/spring-cloud-access-app-vnet/private-dns-zone-add-record.png)

## <a name="assign-private-fqdn-for-your-application"></a>Attribuer un nom de domaine complet privé pour votre application

Après avoir effectué la procédure décrite dans [Générer et déployer des applications de microservices](spring-cloud-tutorial-deploy-in-azure-virtual-network.md), vous pouvez affecter un nom de domaine complet privé pour votre application.

1. Sélectionnez l’instance de service Azure Spring Cloud déployée dans votre réseau virtuel, puis ouvrez l’onglet **Applications** dans le menu de gauche.

2. Sélectionnez l’application pour afficher la page **Vue d’ensemble**.

3. Sélectionnez **Attribuer un point de terminaison** pour attribuer un nom de domaine complet privé à votre application. ce qui peut prendre quelques minutes.

    ![Attribuer un point de terminaison privé](media/spring-cloud-access-app-vnet/assign-private-endpoint.png)

4. Le nom de domaine complet privé (étiqueté **URL**) attribué est désormais disponible. Il n’est accessible qu’au sein du réseau privé, et pas sur Internet.

## <a name="access-application-private-fqdn"></a>Accéder au nom de domaine complet privé de l’application

Une fois le nom de domaine complet privé attribué à votre application, vous pouvez y accéder dans le réseau privé. Par exemple, vous pouvez créer un ordinateur jumpbox dans le même réseau virtuel, ou un réseau virtuel associé, et le nom de domaine complet privé est accessible sur cet ordinateur jumpbox.

![Accéder au point de terminaison privé dans le réseau virtuel](media/spring-cloud-access-app-vnet/access-private-endpoint.png)

## <a name="next-steps"></a>Étapes suivantes

- [Exposer des applications à Internet à l’aide d’Application Gateway et du Pare-feu Azure](spring-cloud-expose-apps-gateway-azure-firewall.md)

## <a name="see-also"></a>Voir aussi

- [Résolution des problèmes Azure Spring Cloud dans un réseau virtuel](spring-cloud-troubleshooting-vnet.md)
- [Responsabilités du client pour l’exécution d’Azure Spring Cloud dans un réseau virtuel](spring-cloud-vnet-customer-responsibilities.md)