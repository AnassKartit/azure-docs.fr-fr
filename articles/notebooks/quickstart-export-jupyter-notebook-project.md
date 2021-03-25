---
title: Utiliser un notebook Jupyter avec des offres Microsoft
description: Découvrez de quelle façon les notebooks Jupyter peuvent être utilisés avec les offres Microsoft.
ms.topic: quickstart
ms.date: 02/01/2021
ms.openlocfilehash: 5679c28d9cc8a4f1893ffad72002b66ad59861e6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99507375"
---
# <a name="use-a-jupyter-notebook-with-microsoft-offerings"></a>Utiliser un notebook Jupyter avec des offres Microsoft

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Dans ce guide de démarrage rapide, vous allez apprendre à importer un notebook Jupyter pour l’utiliser dans des offres Microsoft appropriées. 

Si vous avez déjà des notebooks Jupyter ou que vous souhaitez démarrer un nouveau projet, vous pouvez les utiliser avec de nombreuses offres Microsoft. Voici quelques options décrites dans les sections ci-dessous : 
- [Visual Studio Code](#use-notebooks-in-visual-studio-code)
- [GitHub Codespaces](#use-notebooks-in-github-codespaces)
- [Azure Machine Learning](#use-notebooks-with-azure-machine-learning)
- [Azure Lab Services](#use-azure-lab-services)
- [GitHub](#use-github)

## <a name="create-an-environment-for-notebooks"></a>Créer un environnement pour les notebooks

Si vous souhaitez créer un environnement qui correspond à la préversion mise hors service d’Azure Notebooks, utilisez le fichier de script fourni dans GitHub.

1. Accédez au [dépôt GitHub](https://github.com/microsoft/AzureNotebooks) Azure Notebooks ou [accédez directement au dossier environment](https://aka.ms/aznbrequirementstxt).
1. Depuis une invite de commandes, accédez au répertoire que vous souhaitez utiliser pour vos projets.
1. Téléchargez le contenu du dossier environment et suivez les instructions du fichier README pour installer les dépendances du package Azure Notebooks.


## <a name="use-notebooks-in-visual-studio-code"></a>Utiliser des notebooks dans Visual Studio Code

[VS Code](https://code.visualstudio.com/) est un éditeur de code gratuit que vous pouvez utiliser localement ou connecté à un calcul distant. Associé à l’extension Python, il offre un environnement complet pour le développement Python, y compris une expérience native riche pour utiliser des notebooks Jupyter. 

![Prise en charge des notebooks Jupyter dans VS Code](media/vs-code-jupyter-notebook.png)

Si vous avez des fichiers projet existants ou si vous souhaitez créer un autre notebook, vous pouvez utiliser VS Code ! Pour obtenir des conseils sur l’utilisation de VS Code avec des notebooks Jupyter, consultez les tutoriels [Utilisation de notebooks Jupyter dans Visual Studio Code](https://code.visualstudio.com/docs/python/jupyter-support) et [Science des données dans Visual Studio Code](https://code.visualstudio.com/docs/python/data-science-tutorial).

Vous pouvez également utiliser le [script d’environnement Azure Notebooks](#create-an-environment-for-notebooks) avec Visual Studio Code pour créer un environnement qui correspond à Azure Notebooks (préversion).

## <a name="use-notebooks-in-github-codespaces"></a>Utiliser des notebooks dans GitHub Codespaces

GitHub Codespaces fournit des environnements hébergés dans le cloud dans lesquels vous pouvez modifier vos notebooks à l’aide de Visual Studio Code ou dans votre navigateur web. Il procure la même magnifique expérience de Jupyter que VS Code, mais sans qu’il soit nécessaire d’installer quoi que ce soit sur votre appareil. Si vous ne souhaitez pas configurer un environnement local et préférez une solution cloud, la création d’un espace de code est une option intéressante. Pour commencer :
1. (Facultatif) Rassemblez les fichiers projet que vous souhaitez utiliser avec GitHub Codespaces.
1. [Créez un dépôt GitHub](https://help.github.com/github/getting-started-with-github/create-a-repo) pour y stocker vos notebooks.   
1. [Ajoutez vos fichiers](https://help.github.com/github/managing-files-in-a-repository/adding-a-file-to-a-repository) au dépôt.
1. [Demander l’accès à la préversion de GitHub Codespaces](https://github.com/features/codespaces)

## <a name="use-notebooks-with-azure-machine-learning"></a>Utiliser des notebooks avec Azure Machine Learning

Azure Machine Learning fournit une plateforme de machine learning de bout en bout qui permet de créer et de déployer des modèles plus rapidement sur Azure. Avec Azure Machine Learning, vous pouvez exécuter des notebooks Jupyter sur une machine virtuelle ou un environnement informatique de cluster partagé. Si vous avez besoin d’une solution basée sur le cloud pour votre charge de travail ML avec suivi d’expérimentation, gestion des jeux de données, etc., nous vous recommandons Azure Machine Learning. Pour bien démarrer avec Azure ML :

1. (Facultatif) Rassemblez tous les fichiers projet que vous souhaitez utiliser avec Azure Machine Learning.
1. [Créez un espace de travail](../machine-learning/how-to-manage-workspace.md) dans le portail Azure.

   ![Créer un espace de travail](../machine-learning/media/how-to-manage-workspace/create-workspace.gif)
 
1. Ouvrez [Azure Studio (préversion)](https://ml.azure.com/).
1. À l’aide de la barre de navigation de gauche, sélectionnez **Notebooks**.
1. Cliquez sur le bouton **Charger des fichiers** et chargez les fichiers projet.

Pour plus d’informations sur Azure ML et l’exécution de notebooks Jupyter, vous pouvez consulter la [documentation](../machine-learning/how-to-run-jupyter-notebooks.md) ou essayer le module [Présentation d’Azure Machine Learning](/learn/modules/intro-to-azure-machine-learning-service/) sur Microsoft Learn.


## <a name="use-azure-lab-services"></a>Utiliser Azure Lab Services

[Azure Lab Services](https://azure.microsoft.com/services/lab-services/) permet aux enseignants de configurer et de fournir facilement un accès à la demande aux machines virtuelles préconfigurées pour une classe entière. Si vous cherchez un moyen d’utiliser des notebooks Jupyter et le calcul cloud dans un environnement de type salle de classe personnalisé, Lab Services est une option intéressante.

![image](../lab-services/media/tutorial-setup-classroom-lab/new-lab-button.png)

Si vous avez des fichiers projet existants ou si vous souhaitez créer un autre notebook, vous pouvez utiliser Azure Lab Services. Pour obtenir de l’aide sur la configuration d’un labo, consultez [Configurer un laboratoire pour enseigner la science des données avec Python et Jupyter Notebooks](../lab-services/class-type-jupyter-notebook.md).

## <a name="use-github"></a>Utiliser GitHub

GitHub fournit un moyen gratuit et basé sur un contrôle de code source pour stocker les notebooks (et d’autres fichiers), partager vos notebooks et collaborer. Si vous recherchez un moyen de partager vos projets et de collaborer, GitHub est une option intéressante, que vous pouvez combiner avec [GitHub Codespaces](#use-notebooks-in-github-codespaces) pour bénéficier d’une expérience de développement exceptionnelle. Pour bien démarrer avec GitHub

1. (Facultatif) Rassemblez tous les fichiers projet que vous souhaitez utiliser avec GitHub.
1. [Créez un dépôt GitHub](https://help.github.com/github/getting-started-with-github/create-a-repo) pour y stocker vos notebooks. 
1. [Ajoutez vos fichiers](https://help.github.com/github/managing-files-in-a-repository/adding-a-file-to-a-repository) au dépôt.

## <a name="next-steps"></a>Étapes suivantes

- [Découvrir Python dans Visual Studio Code](https://code.visualstudio.com/docs/python/python-tutorial)
- [Découvrir Azure Machine Learning et les notebooks Jupyter](../machine-learning/how-to-run-jupyter-notebooks.md)
- [Découvrir GitHub Codespaces](https://github.com/features/codespaces)
- [Découvrir Azure Lab Services](https://azure.microsoft.com/services/lab-services/)
- [Découvrir GitHub](https://help.github.com/github/getting-started-with-github/)
