---
title: Fichier include
description: Fichier include
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 07/02/2021
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: ffc9f653541acd74d1b3fb9b9106c9647f626b65
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "121735423"
---
1. Dans une nouvelle fenêtre du navigateur, connectez-vous au [portail Azure](https://portal.azure.com/).

2. Dans le menu de gauche, sélectionnez **Créer une ressource**.
   
   :::image type="content" source="./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-0.png" alt-text="Créer une ressource dans le Portail Azure":::
   
3. Dans la page **Nouveau**, sélectionnez **Bases de données** > **Azure Cosmos DB**.
   
   :::image type="content" source="./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png" alt-text="Volet Bases de données du Portail Azure":::
   
3. Dans la page **Créer un compte Azure Cosmos DB**, entrez les paramètres du nouveau compte Azure Cosmos DB. 
 
    Paramètre|Valeur|Description
    ---|---|---
    Abonnement|Votre abonnement|Sélectionnez l’abonnement Azure que vous souhaitez utiliser pour ce compte Azure Cosmos DB. 
    Groupe de ressources|**Créer nouveau**, puis Nom du compte|Sélectionnez **Créer nouveau**. Entrez ensuite le nom du nouveau groupe de ressources pour votre compte. Pour rester simple, utilisez le nom de votre compte Azure Cosmos DB. 
    Nom du compte|Un nom unique|Entrez un nom unique pour identifier votre compte Azure Cosmos DB.<br><br>Le nom peut contenir uniquement des lettres minuscules, des chiffres et des traits d’union (-), et doit comporter entre 3 et 44 caractères.
    API|Table de charge de travail|L’API détermine le type de compte à créer. Azure Cosmos DB fournit cinq API : Core (SQL) pour les bases de données de documents, Gremlin pour les bases de données de graphes, MongoDB pour les bases de données de documents, Table Azure et Cassandra. Vous devez créer un compte distinct pour chaque API. <br><br>Sélectionnez **Table Azure**, car ce guide de démarrage rapide vous permet de créer une table qui fonctionne avec l’API Table. <br><br>[Découvrez plus d’informations sur l’API Table](../table/introduction.md).|
    Emplacement|La région la plus proche de vos utilisateurs|Sélectionnez la zone géographique dans laquelle héberger votre compte Azure Cosmos DB. Utilisez l’emplacement le plus proche de vos utilisateurs pour leur donner l’accès le plus rapide possible aux données.
    Mode de capacité|Débit approvisionné ou serverless|Sélectionnez **Débit approvisionné** pour créer un compte dans mode de [débit approvisionné](../set-throughput.md). Sélectionnez **serverless** pour créer un compte en mode [serverless](../serverless.md).

    Vous pouvez laisser les options **Géoredondance** et **Écritures multirégions** définies sur **Désactiver** pour éviter d’avoir des frais supplémentaires, et ignorer les sections **Réseau** et **Étiquettes**.

5. Sélectionnez **Vérifier + créer**. Une fois la validation terminée, sélectionnez **Créer** pour créer le compte. 
 
   :::image type="content" source="./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-create-new-account.png" alt-text="Page de nouveau compte pour Azure Cosmos DB":::

6. La création du compte prend quelques minutes. Un message vous indique que **Votre déploiement est en cours**. Attendez la fin du déploiement, puis sélectionnez **Accéder à la ressource**.

    :::image type="content" source="./media/cosmos-db-create-dbaccount-table/azure-cosmos-db-account-created.png" alt-text="Volet Notifications du Portail Azure":::

