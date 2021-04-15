---
title: Fichier include
description: Fichier include
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/26/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: aa9a715fdafc143a116458691965087b016dec1f
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "83343352"
---
Appliquez la procédure suivante pour afficher et mettre à jour la version du runtime utilisée actuellement par une application de fonction.

1. Dans le [portail Azure](https://portal.azure.com), accédez à une application de fonction.

1. Sous **Paramètres**, sélectionnez **Configuration**. Dans l’onglet **Paramètres du runtime de fonction**, trouvez la **Version du runtime**. Notez la version du runtime spécifique. Dans l’exemple ci-dessous, la version est définie sur `~3`.

    :::image type="content" source="./media/functions-view-update-version-portal/functions-view-runtime-version.png" alt-text="Afficher la version du runtime." border="true":::

1. Pour conserver votre application de fonction sur la version 1.x du runtime, choisissez  **~1** sous **Version du runtime**. Ce commutateur est désactivé lorsque votre application comporte des fonctions.

1. Quand vous modifiez la version du runtime, revenez à l’onglet **Vue d’ensemble** et choisissez l’option **Redémarrer** pour redémarrer l’application.  L’application de fonction redémarre avec la version 1.x du runtime, et les modèles de la version 1.x sont utilisés lorsque vous créez des fonctions.

    :::image type="content" source="./media/functions-view-update-version-portal/functions-restart-function-app.png" alt-text="Redémarrer votre application de fonction." border="true":::
