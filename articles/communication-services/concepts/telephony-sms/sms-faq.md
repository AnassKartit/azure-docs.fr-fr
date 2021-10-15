---
title: FAQ relative aux SMS
titleSuffix: An Azure Communication Services concept document
description: FAQ relative aux SMS
author: prakulka
manager: nmurav
services: azure-communication-services
ms.author: prakulka
ms.date: 06/30/2021
ms.topic: conceptual
ms.service: azure-communication-services
ms.subservice: sms
ms.openlocfilehash: 658e13aa888e448de723a97bf9cc89c162a774b5
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2021
ms.locfileid: "129356317"
---
# <a name="sms-faq"></a>FAQ relative aux SMS

[!INCLUDE [Public Preview Notice](../../includes/public-preview-include.md)]
## <a name="can-a-customer-use-azure-communication-services-for-emergency-purposes"></a>Un client peut-il utiliser Azure Communication Services à des fins d’urgence ?

Azure Communication Services ne prend pas en charge l’envoi de SMS au 911 aux États-Unis, mais il est possible que vous soyez obligé de le faire en vertu des règles de la Federal Communications Commission (FCC).  Vous devez déterminer si les règles de la FCC en matière de SMS au 911 s’appliquent à votre service ou à votre application. Dans la mesure où vous êtes soumis à ces règles, vous êtes responsable de l’acheminement des SMS adressés au 911 aux centres d’appels d’urgence qui en font la demande. Vous êtes libre de déterminer votre propre modèle de transmission de SMS au 911, mais une approche acceptée par la FCC consiste à lancer automatiquement le clavier natif de l’appareil mobile de l’utilisateur pour transmettre les SMS au 911 par l’intermédiaire de l’opérateur mobile sous-jacent.

## <a name="are-there-any-limits-on-sending-messages"></a>L’envoi de messages est-il limité ?

Pour garantir que nous continuons à offrir une qualité de service élevée conforme à nos SLA, Azure Communication Services applique des limites de taux (différentes pour chaque primitive). Les développeurs qui appellent nos API au-delà de cette limite recevront une réponse avec un code d’état HTTP 429. Si les exigences de votre entreprise excèdent les limites de débit, envoyez-nous un e-mail à l’adresse phone@microsoft.com.

Limites de débit pour SMS :

|Opération|Étendue|Délai(s) d’exécution| Limite (nbre de demandes) | Unités de message par minute|
|---------|-----|-------------|-------------------|-------------------------|
|Envoyer un message|Par nombre|60|200|200|

## <a name="how-does-azure-communication-services-handle-opt-outs-for-toll-free-numbers"></a>Comment Azure Communication Services gère-t-il les exclusions pour les numéros gratuits ?

Les désinscriptions des numéros gratuits aux États-Unis sont régies et appliquées par les opérateurs nationaux.
- STOP : si le destinataire d’un SMS souhaite se désinscrire, il peut envoyer « STOP » au numéro gratuit. L’opérateur envoie la réponse par défaut suivante pour STOP : « MESSAGE RÉSEAU : Vous avez répondu avec le mot “stop” qui bloque tout SMS envoyé depuis ce numéro. Répondez “unstop” par SMS pour recevoir les messages de nouveau. »
- START/UNSTOP : si le destinataire souhaite se réinscrire aux SMS envoyés depuis un numéro gratuit, il peut envoyer « START » ou « UNSTOP » à ce numéro. L’opérateur envoie la réponse par défaut suivante pour START/UNSTOP : «MESSAGE RÉSEAU : Vous avez répondu « unstop » et allez recommencer à recevoir les messages envoyés depuis ce numéro. »
- Azure Communication Services détecte le message STOP et bloque tous les futurs messages adressés au destinataire. Le rapport de remise indique un échec de la remise avec un message d’état selon lequel l’expéditeur est bloqué pour le destinataire concerné.
- Les messages STOP, UNSTOP et START sont relayés jusqu’à vous. Azure Communication Services vous encourage à superviser et à implémenter ces désinscriptions pour garantir qu’aucune autre tentative d’envoi de message n’est effectuée en direction des destinataires qui ont refusé vos communications.

## <a name="how-can-i-receive-messages-using-azure-communication-services"></a>Comment puis-je recevoir des messages à l’aide d’Azure Communication Services ?

Les clients d’Azure Communication Services peuvent utiliser Azure Event Grid pour recevoir des messages entrants. Suivez ce [démarrage rapide](../../quickstarts/telephony-sms/handle-sms-events.md) pour configurer votre grille d’événements et recevoir des messages.

## <a name="what-is-the-sms-character-limit"></a>Quelle est la limite de caractères pour les SMS ?
La limite de caractères pour un seul SMS est de 160 caractères. Lors de l’envoi de messages, tous les messages de plus de 160 caractères sont divisés en segments et livrés individuellement, qui sont ensuite concaténés par l’appareil du destinataire. De même, pour la réception de longs messages, plusieurs segments d’un long message sont automatiquement concaténés et livrés en un seul message sur le point de terminaison spécifié dans Azure Event Grid. 

## <a name="can-i-sendreceive-long-messages-2048-chars"></a>Puis-je envoyer ou recevoir des messages longs (plus de 2048 caractères) ?

Azure Communication Services prend en charge l’envoi et la réception de messages longs par SMS. Toutefois, certains opérateurs ou appareils sans fil peuvent agir différemment lors de la réception de messages longs.

## <a name="how-are-messages-sent-to-landline-numbers-treated"></a>Comment sont traités les messages envoyés à des numéros de téléphone fixe ?

Aux États-Unis, Azure Communication Services ne vérifie pas les numéros de téléphone fixe et tente de les envoyer aux opérateurs pour les livrer. Les clients seront facturés pour les messages envoyés à des numéros de téléphone fixe. 

## <a name="can-i-send-messages-to-multiple-recipients"></a>Puis-je envoyer des messages à plusieurs destinataires ?

Oui, vous pouvez faire une demande avec plusieurs destinataires. Suivez ce [démarrage rapide](../../quickstarts/telephony-sms/send.md?pivots=programming-language-csharp) pour envoyer des messages à plusieurs destinataires.

##  <a name="i-received-a-http-status-202-from-the-send-sms-api-but-the-sms-didnt-reach-my-phone-what-do-i-do-now"></a>J’ai reçu un état HTTP 202 de l’API d’envoi de SMS, mais le SMS n’a pas été reçu par mon téléphone. Que dois-je faire maintenant ?

L’état 202 retourné par le service signifie que votre message a été mis en file d’attente pour être envoyé et n’a pas été remis. Utilisez ce [guide de démarrage rapide](../../quickstarts/telephony-sms/handle-sms-events.md) pour vous abonner aux événements de rapport de remise et résoudre le problème. Une fois les événements configurés, examinez le champ « deliveryStatus » de votre rapport de remise pour vérifier si la remise a réussi ou échoué.
