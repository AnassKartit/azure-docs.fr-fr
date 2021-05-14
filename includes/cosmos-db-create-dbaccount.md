---
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: include
ms.date: 04/21/2021
ms.custom: include file
ms.openlocfilehash: 23b9385f715993446ebd5392547b51ac06b9ddb1
ms.sourcegitcommit: 5f785599310d77a4edcf653d7d3d22466f7e05e1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/27/2021
ms.locfileid: "108065637"
---
1. Dans le menu du portail Azure ou dans la **page d’accueil**, sélectionnez **Créer une ressource**.

1. Dans la page **Nouveau**, recherchez et sélectionnez **Azure Cosmos DB**.

1. Dans la page **Azure Cosmos DB**, sélectionnez **Créer**.

1. Dans la page **Créer un compte Azure Cosmos DB**, entrez les paramètres de base du nouveau compte Azure Cosmos. 

    |Paramètre|Valeur|Description |
    |---|---|---|
    |Abonnement|Nom d’abonnement|Sélectionnez l’abonnement Azure à utiliser pour ce compte Azure Cosmos. |
    |Groupe de ressources|Nom de groupe ressources|Sélectionnez un groupe de ressources ou sélectionnez **Créer**, puis entrez un nom unique pour le nouveau groupe de ressources. |
    |Nom du compte|Un nom unique|Entrez un nom pour identifier votre compte Azure Cosmos. Étant donné que *documents.azure.com* est ajouté au nom que vous fournissez pour créer votre URI, utilisez un nom unique.<br><br>Le nom peut uniquement contenir des lettres minuscules, des chiffres et le caractère de trait d’union (-). Sa longueur doit être comprise entre 3 et 44 caractères.|
    |API|Type de compte à créer|Sélectionnez **Core (SQL)** pour créer une base de données orientée document et effectuez des requêtes à l’aide de la syntaxe SQL. <br><br>L’API détermine le type de compte à créer. Azure Cosmos DB fournit cinq API : Core (SQL) et MongoDB pour les données de document, Gremlin pour les données de graphe, Table Azure et Cassandra. Actuellement, vous devez créer un compte distinct pour chaque API. |
    |Mode de capacité|Débit approvisionné ou serverless|Sélectionnez **Débit approvisionné** pour créer un compte dans mode de [débit approvisionné](../articles/cosmos-db/set-throughput.md). Sélectionnez **serverless** pour créer un compte en mode [serverless](../articles/cosmos-db/serverless.md).|
    |Appliquer la remise de niveau gratuit|Appliquer ou ne pas appliquer|Avec Azure Cosmos DB niveau gratuit, vous recevrez 400 RU/s et 5 Go de stockage dans un compte, sans frais. Découvrez-en plus sur le [niveau gratuit](https://azure.microsoft.com/pricing/details/cosmos-db/).|
    |Emplacement|La région la plus proche de vos utilisateurs|Sélectionnez la zone géographique dans laquelle héberger votre compte Azure Cosmos DB. Utilisez l’emplacement le plus proche de vos utilisateurs pour leur donner l’accès le plus rapide possible aux données.|
    |Type de compte|Production ou non-production|Sélectionnez **Production** si le compte sera utilisé pour une charge de travail de production. Sélectionnez **Hors production** si le compte sera utilisé hors production, par exemple pour le développement, les tests, l’assurance qualité ou la préproduction. Il s’agit d’un paramètre d’étiquette de ressource Azure qui permet d’ajuster l’expérience du portail sans affecter le compte Azure Cosmos DB sous-jacent. Vous pouvez modifier cette valeur à tout moment.|

    > [!NOTE]
    > Vous pouvez avoir un seul compte Azure Cosmos DB de niveau gratuit par abonnement Azure et vous devez vous inscrire lors de la création du compte. Si vous ne voyez pas l’option permettant d’appliquer la remise de niveau gratuit, cela signifie qu’un autre compte dans l’abonnement a déjà été activé avec le niveau gratuit.
   
    > [!NOTE]
    > Les options suivantes ne sont pas disponibles si vous sélectionnez **Serverless** comme **Mode de capacité** :
    > - Appliquer la remise de niveau gratuit
    > - Géo-redondance
    > - Écritures multirégions
    
    :::image type="content" source="./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account-detail.png" alt-text="Page de nouveau compte pour Azure Cosmos DB":::

1. Sélectionnez **Revoir + créer**. Vous pouvez ignorer les sections **Réseau** et **Balises**.

1. Passez en revue les paramètres du compte, puis sélectionnez **Créer**. La création du compte prend quelques minutes. Attendez que la page du portail affiche **Votre déploiement est terminé**. 

    :::image type="content" source="./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png" alt-text="Volet Notifications du portail Azure":::

1. Sélectionnez **Accéder à la ressource** pour accéder à la page du compte Azure Cosmos DB. 

    :::image type="content" source="./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png" alt-text="Page du compte Azure Cosmos DB":::
