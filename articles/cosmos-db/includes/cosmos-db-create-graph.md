---
title: Fichier Include
description: Fichier include
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 4f11826c5f4a39a6df8c1b9823c54620cb66f3ee
ms.sourcegitcommit: f3b930eeacdaebe5a5f25471bc10014a36e52e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/16/2021
ms.locfileid: "113223443"
---
Vous pouvez désormais utiliser l’outil Explorateur de données dans le portail Azure pour créer une base de données de graphiques. 

1. Sélectionnez **Data Explorer** > **Nouveau graphique**.

    La zone **Ajouter un graphique** est affichée à l’extrême droite. Il peut donc être nécessaire de faire défiler à droite pour l’afficher.

    ![Explorateur de données du portail Azure, page Ajouter un graphique](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer-graph.png)

2. Dans la page **Ajouter un graphique**, entrez les paramètres relatifs au nouveau graphique.

    Paramètre|Valeur suggérée|Description
    ---|---|---
    ID de base de données|sample-database|Entrez le nom *sample-database* pour la nouvelle base de données. Les noms de base de données doivent inclure entre 1 et 255 caractères et ne peuvent pas contenir `/ \ # ?` ni d’espace de fin.
    Débit|400 unités de requête|Changez le débit en indiquant 400 unités de requête par seconde (RU/s). Si vous souhaitez réduire la latence, vous pourrez augmenter le débit par la suite.
    ID du graphique|sample-graph|Entrez le nom *sample-graph* pour votre nouvelle collection. Les noms de graphes sont soumis aux mêmes exigences pour les caractères que les ID de bases de données.
    Clé de partition| /pk |Tous les comptes Cosmos DB ont besoin d’une clé de partition se mettre à l’échelle horizontalement. Découvrez comment sélectionner une clé de partition appropriée dans l'article sur le [partitionnement des données graphiques](../graph-partitioning.md).

3. Une fois le formulaire rempli, sélectionnez **OK**.