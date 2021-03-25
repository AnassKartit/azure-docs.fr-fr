---
title: Fichier include
description: Fichier include
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/14/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: a972b4738ce5646a1ee9eed6495bdc43a40826fd
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102219032"
---
Vous pouvez déployer le protocole FTP et Git local sur une application web Azure en faisant appel à un *utilisateur de déploiement*. Une fois que vous avez créé votre utilisateur de déploiement, vous pouvez l’utiliser pour tous vos déploiements Azure. Votre nom d’utilisateur et votre mot de passe de déploiement au niveau du compte sont différents de vos informations d’identification de l’abonnement Azure. 

Pour configurer l’utilisateur de déploiement, exécutez la commande [az webapp deployment user set](/cli/azure/webapp/deployment/user#az-webapp-deployment-user-set) dans Azure Cloud Shell. Remplacez \<username> et \<password> par un nom d’utilisateur et un mot de passe de déploiement. 

- Le nom d’utilisateur doit être unique dans Azure et, pour les push Git locaux, ne doit pas contenir le symbole « \@ ». 
- Le mot de passe doit comporter au moins huit caractères et inclure deux des trois éléments suivants : lettres, chiffres et symboles. 

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

La sortie JSON affiche le mot de passe comme étant `null`. Si vous obtenez une erreur `'Conflict'. Details: 409`, modifiez le nom d’utilisateur. Si vous obtenez une erreur `'Bad Request'. Details: 400`, utilisez un mot de passe plus fort. 

Enregistrez le nom d’utilisateur et le mot de passe à utiliser pour déployer vos applications web.
