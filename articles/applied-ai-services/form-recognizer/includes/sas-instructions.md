---
author: laujan
ms.service: applied-ai-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 11/02/2021
ms.author: lajanuar
ms.custom: ignite-fall-2021
ms.openlocfilehash: 5b9bf09957f70ed08a6d0c7a4988f4c0fde6fab9
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131021654"
---
Si vous souhaitez récupérer l’URL SAS pour vos données d’entraînement de modèle personnalisé, accédez à votre ressource de stockage dans le portail Azure, puis sélectionnez l’onglet **Explorateur Stockage**. Accédez à votre conteneur, cliquez avec le bouton droit, puis sélectionnez **Obtenir une signature d’accès partagé**. Il est important d’obtenir la signature d’accès partagé de votre conteneur, et non celle du compte de stockage. Vérifiez que les autorisations de **lecture**, d’**écriture**, de **suppression** et de **liste** sont cochées, puis cliquez sur **Créer**. Copiez ensuite la valeur de la section **URL** dans un emplacement temporaire. Il doit avoir le format : `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.
