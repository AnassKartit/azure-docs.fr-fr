---
title: Questions fréquentes (FAQ) sur le service Azure Web PubSub
description: Réponses aux questions fréquentes sur le service Azure Web PubSub.
author: yjin81
ms.author: yajin1
ms.service: azure-web-pubsub
ms.topic: overview
ms.date: 04/26/2021
ms.openlocfilehash: bcecb9b50c493ba85172fd56ccf3f2d286aa1839
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2021
ms.locfileid: "123431311"
---
# <a name="azure-web-pubsub-service-faq"></a>Questions fréquentes (FAQ) sur le service Azure Web PubSub

Voici les questions fréquemment posées sur le service Azure Web PubSub. 

## <a name="is-azure-web-pubsub-service-ready-for-production-use"></a>Le service Azure Web PubSub est-il prêt pour une utilisation en production ?

Le service Azure Web PubSub est en préversion publique et n’a pas de contrat SLA validé. 

## <a name="when-will-azure-web-pubsub-service-be-ready-for-production-use"></a>Quand le service Azure Web PubSub sera-t-il prêt pour une utilisation en production ?

L’équipe produit travaille activement à la mise en disponibilité générale du service Azure Web PubSub. Une fois en disponibilité générale, le service prendra en charge :
- Support Azure Active Directory
- Intégration du service Gestion des API
- Sous-protocole binaire Pub/Sub WebSocket
- Intégration des journaux de ressources Azure

Pour plus d’informations sur les fonctionnalités en disponibilité générale, contactez l’équipe produit.

## <a name="how-do-i-choose-between-azure-signalr-service-and-azure-web-pubsub-service"></a>Comment choisir entre Azure SignalR Service et le service Azure Web PubSub ?

[Azure SignalR Service](https://azure.microsoft.com/services/signalr-service) et le [service Azure Web PubSub](https://azure.microsoft.com/services/web-pubsub) aident tous deux les clients à créer facilement des applications web temps réel à grande échelle et avec une haute disponibilité, et permettent aux clients de se concentrer sur leur logique métier au lieu de gérer l’infrastructure de messagerie. En général, vous pouvez choisir Azure SignalR Service si vous utilisez déjà la bibliothèque SignalR pour créer une application temps réel. Par contre, si vous recherchez une solution générique pour créer une application temps réel basée sur le modèle WebSocket et publication-abonnement, vous pouvez choisir le service Azure Web PubSub. Le service Azure Web PubSub ne remplace **pas** Azure SignalR Service. Ils ciblent des scénarios différents.

Azure SignalR Service est plus adapté si :  

- Vous utilisez déjà ASP.NET ou ASP.NET Core SignalR, principalement avec .NET ou la nécessité de s’intégrer à l’écosystème .NET (comme Blazor).
- Un client SignalR est disponible pour votre plateforme. 
- Vous avez besoin d’un protocole établi qui prend en charge un large éventail de modèles d’appel (RPC et streaming) et de transports (WebSocket, événements envoyés par le serveur et interrogation longue), et avec un client qui gère la durée de vie de la connexion en votre nom. 

Le service Azure Web PubSub est plus adapté aux situations où :  

- Vous devez créer des applications temps réel basées sur la technologie WebSocket ou publication-abonnement sur WebSocket.
- Vous voulez créer votre propre sous-protocole ou utiliser des protocoles avancés existants sur WebSocket (par exemple MQTT, AMQP sur WebSocket). 
- Vous recherchez un serveur léger, par exemple pour envoyer des messages au client sans passer par le back-end configuré.  

##  <a name="where-does-my-data-reside"></a>Où mes données résident-elles ?

Le service Azure Web PubSub fonctionne comme un service de processeur de données. Il ne stocke pas de contenu client et la résidence des données est incluse par défaut. Si vous utilisez le service Azure Web PubSub avec d’autres services Azure, comme le Stockage Azure pour les diagnostics, consultez [ce livre blanc](https://azure.microsoft.com/resources/achieving-compliant-data-residency-and-security-with-azure/) pour obtenir des conseils sur la façon de maintenir la résidence des données dans les régions Azure.
