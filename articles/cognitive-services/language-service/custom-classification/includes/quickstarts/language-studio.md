---
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-service
ms.topic: include
ms.date: 11/02/2021
ms.author: aahi
ms.custom: language-service-custom-classification, ignite-fall-2021
ms.openlocfilehash: 912aa02fadef0f950f5d89535bfe38133b61137b
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131097768"
---
## <a name="prerequisites"></a>Prérequis

* Abonnement Azure : [créez-en un gratuitement](https://azure.microsoft.com/free/cognitive-services).

## <a name="create-a-new-azure-resource-and-azure-blob-storage-account"></a>Créer une ressource Azure et un compte de stockage Blob Azure

Avant de pouvoir utiliser la classification de texte personnalisée, vous devez créer une ressource Azure Language qui vous permettra de disposer des informations d’identification nécessaires pour créer un projet et commencer à entraîner un modèle. Vous aurez également besoin d’un compte Stockage Azure, où vous pourrez charger le jeu de données à utiliser pour générer votre modèle.

> [!IMPORTANT]
> Pour bien démarrer rapidement, nous vous recommandons de créer une ressource Azure Language en suivant les étapes ci-dessous. Ainsi, vous pourrez créer la ressource et configurer en même temps un compte de stockage, ce qui est plus facile à faire maintenant que plus tard.
>
> Si vous disposez d’une ressource préexistante à utiliser, vous devez la configurer séparément du compte de stockage. Pour plus d’informations, consultez les [**exigences du projet**](../../how-to/create-project.md#using-a-pre-existing-azure-resource).

1. Accédez au [portail Azure](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) pour créer une ressource Azure Language. Si vous êtes invité à sélectionner des fonctionnalités supplémentaires, sélectionnez **Classification de texte personnalisée et NER personnalisée**. Quand vous créez la ressource, vérifiez qu’elle comporte les paramètres suivants.

    |Paramètres nécessaires à la ressource Azure  |Valeur requise  |
    |---------|---------|
    |Location | « USA Ouest 2 » ou « Europe Ouest »         |
    |Niveau tarifaire     | Niveau tarifaire standard (**S**)        |

2. Dans la section **Reconnaissance d’entité nommée (NER) personnalisée et Classification personnalisée (préversion)** , sélectionnez un compte de stockage existant, ou sélectionnez **Créer un compte de stockage**. Notez que ces valeurs concernent ce guide de démarrage rapide, et pas nécessairement les [valeurs du compte de stockage](/azure/storage/common/storage-account-overview) à utiliser dans les environnements de production.

    |Valeur du compte de stockage  |Valeur recommandée  |
    |---------|---------|
    | Nom | Nom quelconque |
    | Performances | Standard |
    | Type de compte| Stockage (v1 universel) |
    | Réplication | Stockage localement redondant (LRS)
    |Location | N’importe quel lieu le plus proche de vous, pour une meilleure latence.        |


## <a name="upload-sample-data-to-blob-container"></a>Charger les exemples de données dans un conteneur d’objets blob

Une fois que vous avez créé un compte Stockage Azure et que vous l’avez lié à votre ressource Language, vous devez charger les exemples de fichiers de ce guide de démarrage rapide. Ces fichiers sont utilisés par la suite pour entraîner votre modèle.

1. [Téléchargez les exemples de données](https://go.microsoft.com/fwlink/?linkid=2175083) pour ce démarrage rapide à partir de GitHub.

2. Accédez à votre compte de stockage Azure dans le [portail Azure](https://ms.portal.azure.com). Accédez à votre compte, puis chargez les exemples de données vers celui-ci.

L’exemple de jeu de données fourni contient environ 200 résumés de film qui appartiennent à une ou plusieurs des classes suivantes : « Mystery », « Drama », « Thriller », « Comedy », « Action ».

## <a name="create-a-custom-classification-project"></a>Créer un projet de classification personnalisée

[!INCLUDE [Language Studio project creation](../create-project.md)]

    
## <a name="train-your-model"></a>Entraîner votre modèle

En règle générale, après avoir créé un projet, vous importez les données et vous commencez à [étiqueter les entités](../../how-to/tag-data.md) contenues pour entraîner le modèle de classification. Pour ce guide de démarrage rapide, vous allez utiliser l’exemple de fichier de données étiqueté que vous avez téléchargé et stocké dans votre compte Stockage Azure.

Un modèle est un objet de machine learning, qui est entraîné pour classifier du texte. Votre modèle apprend à partir des exemples de données. Ainsi, il pourra ensuite classifier les tickets de support technique.

Pour commencer à entraîner votre modèle :

1. Dans le menu de gauche, sélectionnez **Entraîner**.

2. Sélectionnez **Entraîner un nouveau modèle**, puis tapez le nom du modèle dans la zone de texte ci-dessous.

    :::image type="content" source="../../media/train-model.png" alt-text="Capture d’écran montrant la page de sélection du modèle à entraîner" lightbox="../../media/train-model.png":::

3. Cliquez au bas de la page sur le bouton **Entraîner**.

    > [!NOTE]
    > * Durant l’entraînement, les données sont découpées en 2 ensembles : 80 % pour l’entraînement et 20 % pour les tests. Pour en savoir plus sur le découpage des données, cliquez [ici](../../how-to/train-model.md#data-splits)
    > * L’entraînement peut prendre quelques heures.

## <a name="deploy-your-model"></a>Déployer votre modèle


En règle générale, après l’entraînement d’un modèle, vous passez en revue les [détails de l’évaluation](../../how-to/view-model-evaluation.md) et vous [apportez des améliorations](../../how-to/improve-model.md) si nécessaire. Dans ce guide de démarrage rapide, vous allez simplement déployer votre modèle et le rendre disponible à des fins de test.

Une fois le modèle entraîné, vous pouvez le déployer. Le déploiement de votre modèle vous permet de commencer à l’utiliser pour classifier du texte avec l’[API d’analyse](https://aka.ms/ct-runtime-swagger).

1. Dans le menu de gauche, sélectionnez **Déployer le modèle**.

2. Sélectionnez le modèle à déployer, puis sélectionnez **Déployer le modèle**.

## <a name="test-your-model"></a>Tester votre modèle

Une fois votre modèle déployé, vous pouvez commencer à l’utiliser pour classifier du texte. Suivez les étapes ci-dessous pour envoyer votre première demande de classification de texte. 

1. Dans le menu de gauche, sélectionnez **Tester le modèle**.

2. Sélectionnez le modèle à tester.

3. Ajoutez votre texte à la zone de texte, vous pouvez également charger un fichier `.txt`. 

4. Cliquez sur **Run the test**.

5. Sous l’onglet **Result**, vous pouvez voir les classes prédites pour votre texte. Vous pouvez également voir la réponse JSON sous l’onglet **JSON**. 

    :::image type="content" source="../../media/test-model-results.png" alt-text="Capture d’écran montrant les résultats du test du modèle. L’exemple, tiré de CMU Movie Summary, CC BY-SA 3.0, a été modifié par Microsoft." lightbox="../../media/test-model-results.png":::

## <a name="clean-up-projects"></a>Nettoyer les projets

Quand vous n’avez plus besoin de votre projet, vous pouvez le supprimer de la [page des projets dans Language Studio](https://aka.ms/custom-classification
). Sélectionnez le projet à supprimer et cliquez sur **Supprimer**.
