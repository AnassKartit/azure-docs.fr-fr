---
title: Fichier include
description: Fichier include
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 01/28/2020
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: bcc55156ca1d03614a4ff9767d6cf3f2603c06ca
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96028406"
---
Ajoutez la prise en charge dans votre environnement de développement préféré à l’aide des méthodes suivantes.

| Environnement de développement  | Type d'application      | Pour ajouter la prise en charge |
|--------------------------|-----------------------|----------------|
| Visual Studio            | Bibliothèque de classes C#      | [Installer le package NuGet](../articles/azure-functions/functions-bindings-register.md#vs) |
| Visual Studio Code       | Basé sur [Core Tools](../articles/azure-functions/functions-run-local.md) | [Inscrire le pack d’extension](../articles/azure-functions/functions-bindings-register.md#extension-bundles)<br><br>Il est recommandé d’installer l’[extension Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack). |
| Tout autre éditeur/IDE     | Basé sur [Core Tools](../articles/azure-functions/functions-run-local.md) | [Inscrire le pack d’extension](../articles/azure-functions/functions-bindings-register.md#extension-bundles) |
| Portail Azure             | En ligne uniquement dans le portail | S’installe lors de l’ajout d’une liaison<br /><br /> Pour mettre à jour des extensions de liaison existantes sans avoir à republier votre application de fonction, consultez [Mettre à jour vos extensions](../articles/azure-functions/functions-bindings-register.md). |