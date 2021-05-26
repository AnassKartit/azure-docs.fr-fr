---
title: Configurer une authentification d’abonné webhook - Azure Event Grid IoT Edge | Microsoft Docs
description: Configurer une authentification d’abonné webhook
author: VidyaKukke
manager: rajarv
ms.author: vkukke
ms.reviewer: spelluru
ms.subservice: iot-edge
ms.date: 05/10/2021
ms.topic: article
ms.openlocfilehash: 8855369516067c0cf05bab8be7128b628d61ce6b
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2021
ms.locfileid: "110370494"
---
# <a name="configure-webhook-subscriber-authentication"></a>Configurer une authentification d’abonné webhook

Ce guide fournit des exemples de configurations possibles d’abonnés webhook pour un module Event Grid. Par défaut, seuls les points de terminaison HTTPs sont acceptés pour les abonnés webhook. Le module Event Grid est rejeté si l’abonné présente un certificat auto-signé.

## <a name="allow-only-https-subscriber"></a>Autoriser uniquement l’abonné HTTPS

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=false"
  ]
}
 ```

## <a name="allow-https-subscriber-with-self-signed-certificate"></a>Autoriser l’abonné HTTPS avec un certificat auto-signé

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Affectez à la propriété `outbound__webhook__allowUnknownCA` la valeur `true` uniquement dans les environnements de test, car vous pouvez généralement utiliser des certificats auto-signés. Pour les charges de travail de production, nous vous recommandons de les définir sur **false**.

## <a name="allow-https-subscriber-but-skip-certificate-validation"></a>Autoriser l’abonné HTTPS mais ignorer la validation du certificat

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=true",
    "outbound__webhook__skipServerCertValidation=true",
    "outbound__webhook__allowUnknownCA=false"
  ]
}
 ```

>[!NOTE]
>Affectez à la propriété `outbound__webhook__skipServerCertValidation` la valeur `true` uniquement dans les environnements de test, car vous ne présentez peut-être pas un certificat qui doit être authentifié. Pour les charges de travail de production, nous vous recommandons de les définir sur **false**

## <a name="allow-both-http-and-https-with-self-signed-certificates"></a>Autoriser à la fois HTTP et HTTPs avec des certificats auto-signés

```json
 {
  "Env": [
    "outbound__webhook__httpsOnly=false",
    "outbound__webhook__skipServerCertValidation=false",
    "outbound__webhook__allowUnknownCA=true"
  ]
}
 ```

>[!NOTE]
>Affectez à la propriété `outbound__webhook__httpsOnly` la valeur `false` uniquement dans les environnements de test, car vous souhaiterez peut-être afficher d’abord un abonné HTTP. Pour les charges de travail de production, nous vous recommandons de les définir sur **true**
