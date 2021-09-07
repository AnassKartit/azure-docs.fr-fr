---
title: Remise d’événements à l’aide du service de liaison privée
description: Cet article explique la procédure de contournement de l’impossibilité de remettre des événements à l’aide du service de liaison privée.
ms.topic: how-to
ms.date: 07/01/2021
ms.openlocfilehash: 0672b2b93cf7413ac9a3d46e8d824354276ba89f
ms.sourcegitcommit: 8b7d16fefcf3d024a72119b233733cb3e962d6d9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2021
ms.locfileid: "114286114"
---
# <a name="deliver-events-using-private-link-service"></a>Remise d’événements à l’aide du service de liaison privée
Actuellement, il n’est pas possible de remettre des événements à l’aide de [points de terminaison privés](../private-link/private-endpoint-overview.md). Autrement dit, il n’y a pas de prise en charge si vous avez des exigences strictes en matière d’isolement réseau lorsque le trafic des événements remis ne doit pas sortir de l’espace IP privé. 

## <a name="use-managed-identity"></a>Utiliser l’identité managée
Toutefois, si vos exigences comprennent une méthode sécurisée pour envoyer des événements à l’aide d’un canal chiffré et une identité connue de l’expéditeur (dans ce cas, Event Grid) utilisant un espace d’adressage IP public, vous pouvez remettre des événements au service Event Hubs, Service Bus ou Stockage Azure à l’aide d’une rubrique ou d’un domaine personnalisés Azure Event Grid avec une identité managée par le système. Pour plus d’informations sur la remise d’événements à l’aide d’une identité managée, consultez [Remise d’événement à l’aide d’une identité managée](managed-service-identity.md). 

Ensuite, vous pouvez utiliser une liaison privée configurée dans Azure Functions ou votre webhook déployé sur votre réseau virtuel pour extraire des événements. Voir l’exemple : [Connectez-vous à des points de terminaison privés avec Azure Functions](/samples/azure-samples/azure-functions-private-endpoints/connect-to-private-endpoints-with-azure-functions/).


:::image type="content" source="./media/consume-private-endpoints/deliver-private-link-service.svg" alt-text="Remise via un service de liaison privée":::


Dans cette configuration, le trafic transite par l’adresse IP publique ou Internet d’Event Grid vers le service Event Hubs, Service Bus ou Stockage Azure, mais le canal peut être chiffré et une identité managée d’Event Grid est utilisée. Si vous configurez votre Azure Functions ou webhook déployé sur votre réseau virtuel pour utiliser un stockage Event Hubs, Service Bus ou stockage Azure via une liaison privée, cette section du trafic restera évidemment dans Azure.

## <a name="deliver-events-to-event-hubs-using-managed-identity"></a>Remettre des événements à Event Hubs à l’aide d’une identité managée
Pour remettre des événements à Event Hubs dans votre espace de noms Event Hubs à l’aide d’une identité managée, procédez comme suit :

1. Activez l'identité attribuée par le système : [rubriques système](enable-identity-system-topics.md), [rubriques personnalisées et domaines](enable-identity-custom-topics-domains.md).  
1. [Ajoutez l’identité au rôle **Expéditeur de données Azure Event Hubs** dans l’espace de noms Event Hubs](../event-hubs/authenticate-managed-identity.md#to-assign-azure-roles-using-the-azure-portal).
1. [Activez le paramètre **Autoriser les services Microsoft approuvés à contourner ce pare-feu** dans votre espace de noms Event hubs](../event-hubs/event-hubs-service-endpoints.md#trusted-microsoft-services). 
1. [Configurez l’abonnement aux événements](managed-service-identity.md#create-event-subscriptions-that-use-an-identity) qui utilise un hub d’événements comme point de terminaison pour utiliser l’identité attribuée au système.

## <a name="deliver-events-to-service-bus-using-managed-identity"></a>Remettre des événements à Service Bus à l’aide d’une identité managée
Pour remettre des événements à des files d’attente ou des rubriques Service Bus dans votre espace de noms Service Bus à l’aide d’une identité managée, procédez comme suit :

1. Activez l'identité attribuée par le système : [rubriques système](enable-identity-system-topics.md), [rubriques personnalisées et domaines](enable-identity-custom-topics-domains.md). 
1. [Ajoutez l'identité au rôle **Expéditeur de données Azure Service Bus**](../service-bus-messaging/service-bus-managed-service-identity.md#azure-built-in-roles-for-azure-service-bus) dans l'espace de noms Service Bus.
1. [Activez le paramètre **Autoriser les services Microsoft approuvés à contourner ce pare-feu** dans votre espace de noms Service Bus](../service-bus-messaging/service-bus-service-endpoints.md#trusted-microsoft-services). 
1. [Configurez l’abonnement aux événements](managed-service-identity.md) qui utilise une file d’attente ou une rubrique Service Bus comme point de terminaison pour utiliser l’identité affectée au système.

## <a name="deliver-events-to-storage"></a>Remettre des événements au stockage 
Pour remettre des événements dans les files d’attente de stockage à l’aide d’une identité managée, procédez comme suit :

1. Activez l'identité attribuée par le système : [rubriques système](enable-identity-system-topics.md), [rubriques personnalisées et domaines](enable-identity-custom-topics-domains.md). 
1. [Ajoutez l'identité au rôle **Expéditeur de messages de données en file d'attente du stockage**](../storage/blobs/assign-azure-role-data-access.md) dans la file d'attente de Stockage Azure.
1. [Configurez l’abonnement aux événements](managed-service-identity.md#create-event-subscriptions-that-use-an-identity) qui utilise une file d’attente de stockage comme point de terminaison pour utiliser l’identité affectée par le système.


## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur la remise d’événements à l’aide d’une identité managée, consultez [Remise d’événement à l’aide d’une identité managée](managed-service-identity.md).