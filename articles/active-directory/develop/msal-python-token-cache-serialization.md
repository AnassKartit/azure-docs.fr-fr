---
title: Sérialisation personnalisée du cache de jetons (MSAL Python) | Azure
titleSuffix: Microsoft identity platform
description: Découvrez comment sérialiser le cache de jetons pour MSAL pour Python
services: active-directory
author: rayluo
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 11/13/2019
ms.author: rayluo
ms.reviewer: nacanuma
ms.custom: aaddev, devx-track-python
ms.openlocfilehash: bbe91d7a25e516766939ba56a9d9cfafff4bf0b5
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/29/2021
ms.locfileid: "129229902"
---
# <a name="custom-token-cache-serialization-in-msal-for-python"></a>Sérialisation personnalisée du cache de jetons dans MSAL pour Python

Dans MSAL Python, un cache de jeton en mémoire qui persiste pendant la durée de la session d’application est fourni par défaut lorsque vous créez une instance [ClientApplication](https://msal-python.readthedocs.io/en/latest/#confidentialclientapplication).

La sérialisation du cache de jetons, afin que différentes sessions de votre application puissent y accéder, n’est pas fourni « prêt à l’emploi ». Cela est dû au fait que MSAL Python peut être utilisé dans les types d’applications qui n’ont pas accès au système de fichiers, tels que les applications web. Pour disposer d’un cache de jetons persistant dans une application MSAL Python, vous devez fournir une sérialisation personnalisée du cache de jetons.

Les stratégies de sérialisation du cache de jetons diffèrent selon que vous écrivez une application cliente publique (Bureau) ou une application cliente confidentielle (application web, API web ou application démon).

## <a name="token-cache-for-a-public-client-application"></a>Cache de jetons pour une application cliente publique

Les applications clientes publiques s’exécutent sur l’appareil d’un utilisateur et gèrent des jetons pour un seul utilisateur. Dans ce cas, vous pouvez sérialiser l’intégralité du cache dans un fichier. N’oubliez pas de fournir le verrouillage de fichier si votre application, ou une autre application, peut accéder simultanément au cache. Pour obtenir un exemple simple de sérialisation d’un cache de jetons dans un fichier sans verrouillage, consultez l’exemple dans la documentation de référence de la classe [SerializableTokenCache](https://msal-python.readthedocs.io/en/latest/#msal.SerializableTokenCache).

## <a name="token-cache-for-a-web-app-confidential-client-application"></a>Cache de jetons pour une application web (application cliente confidentielle)

Pour les applications ou API web, vous pouvez utiliser la session, un cache Redims ou une base de données pour stocker le cache de jetons. Il doit y avoir un cache de jetons par utilisateur (par compte). Veillez donc à sérialiser le cache de jetons par compte.

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir un exemple d’utilisation du cache de jetons pour une application web ou une API web Windows ou Linux, consultez [ms-identity-python-webapp](https://github.com/Azure-Samples/ms-identity-python-webapp/blob/0.3.0/app.py#L66-L74). L’exemple est destiné à une application web qui appelle l’API Microsoft Graph.
