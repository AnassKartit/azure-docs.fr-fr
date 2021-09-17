---
title: 'Tutoriel : prise en main d’un script Python'
titleSuffix: Azure Machine Learning
description: Commencez avec votre premier script Python dans Azure Machine Learning. Il s’agit de la première partie d’une série de prise en main qui en compte trois.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: aminsaied
ms.author: amsaied
ms.reviewer: sgilley
ms.date: 04/27/2021
ms.custom: devx-track-python, FY21Q4-aml-seo-hack, contperf-fy21q4
ms.openlocfilehash: 472fcf4c7a1cc486db5aded40c87ffec2a9e796a
ms.sourcegitcommit: 9f1a35d4b90d159235015200607917913afe2d1b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/21/2021
ms.locfileid: "122634691"
---
# <a name="tutorial-get-started-with-a-python-script-in-azure-machine-learning-part-1-of-3"></a>Tutoriel : prise en main d’un script Python dans Azure Machine Learning (partie 1 sur 3)

Dans ce tutoriel, vous allez exécuter votre premier script Python dans le cloud avec Azure Machine Learning. Ce tutoriel est la *première partie d’une série de tutoriels qui en compte trois*.

Ce tutoriel évite la complexité de l’apprentissage d’un modèle de Machine Learning. Vous allez exécuter un script Python « Hello World » dans le cloud. Vous allez apprendre comment un script de contrôle est utilisé pour configurer et créer une exécution dans Azure Machine Learning.

Ce didacticiel présente les procédures suivantes :

> [!div class="checklist"]
> * Créer et exécuter un script Python « Hello World! » Script Python.
> * Créer un script de contrôle Python pour soumettre « Hello World » à Azure Machine Learning.
> * Comprendre les concepts d’Azure Machine Learning en matière de script de contrôle.
> * Soumettre et exécuter le script « Hello world! ».
> * Afficher le résultat de votre code dans le cloud.

## <a name="prerequisites"></a>Prérequis

- Suivez les instructions contenues dans [Démarrage rapide : Configurer l’espace de travail pour bien démarrer avec Azure Machine Learning](quickstart-create-resources.md) afin de créer un espace de travail, une instance de calcul et un cluster de calcul à utiliser dans cette série de tutoriels.

## <a name="create-and-run-a-python-script"></a>Créer et exécuter un script Python

Ce tutoriel utilise l’instance de calcul sur votre ordinateur de développement.  Commencez par créer quelques dossiers ainsi que le script :

1. Connectez-vous au [studio Azure Machine Learning](https://ml.azure.com), puis sélectionnez votre espace de travail si vous y êtes invité.
1. Sur la gauche, sélectionnez **Notebooks**.
1. Dans la barre d’outils **Fichiers**, sélectionnez **+** , puis **Créer un dossier**.
  :::image type="content" source="media/tutorial-1st-experiment-hello-world/create-folder.png" alt-text="La capture d’écran montre la création d’un dossier à partir de la barre d’outils.":::
1. Nommez le dossier **get-started**.
1. À droite du nom du dossier, utilisez **...** pour créer un autre dossier sous **get-started**.
  :::image type="content" source="media/tutorial-1st-experiment-hello-world/create-sub-folder.png" alt-text="La capture d’écran montre le menu de création d’un sous-dossier.":::
1. Nommez le nouveau dossier **src**.  Utilisez le lien **Modifier l’emplacement** si l’emplacement du fichier n’est pas correct.
1. À droite du dossier **src**, utilisez **...** pour créer un fichier dans le dossier **src**. 
1. Nommez le fichier *hello.py*.  Changez le **Type de fichier** en *Python (* .py)*.

Copiez ce code dans votre fichier :

```python
# src/hello.py
print("Hello world!")
```

La structure de dossiers du projet ressemble désormais à : 

:::image type="content" source="media/tutorial-1st-experiment-hello-world/directory-structure.png" alt-text="La structure de dossiers montre hello.py dans le sous-dossier src.":::


### <a name="test-your-script"></a><a name="test"></a>Tester votre script

Vous pouvez exécuter votre code localement, ce qui signifie dans le cas présent sur l’instance de calcul. L’exécution locale du code présente l’avantage de pouvoir déboguer le code en mode interactif.  

Si vous avez arrêté l’instance de calcul, démarrez-la maintenant avec l’outil **​​Démarrer le calcul** situé à droite de la liste déroulante Calculer. Attendez environ une minute que l’état passe à *Exécution en cours*.

:::image type="content" source="media/tutorial-1st-experiment-hello-world/start-compute.png" alt-text="La capture d’écran montre le démarrage de l’instance de calcul, si elle est arrêtée":::

Sélectionnez **Enregistrer et exécuter le script dans le terminal** pour exécuter le script.

:::image type="content" source="media/tutorial-1st-experiment-hello-world/save-run-in-terminal.png" alt-text="La capture d’écran montre l’outil permettant d’enregistrer et d’exécuter le script dans le terminal à partir de la barre d’outils":::

Vous voyez la sortie du script dans la fenêtre du terminal au moment où elle s’ouvre. Fermez l’onglet, puis sélectionnez **Terminer** pour fermer la session.

## <a name="create-a-control-script"></a><a name="control-script"></a> Créer un script de contrôle

Un *script de contrôle* vous permet d’exécuter votre script `hello.py` sur différentes ressources de calcul. Vous utilisez le script de contrôle pour contrôler comment et où votre code de machine learning est exécuté.  

Sélectionnez **...** à la fin du dossier **get-started** pour créer un fichier.  Créez un fichier Python appelé *run-hello.py*, puis copiez/collez le code suivant dans ce fichier :

```python
# get-started/run-hello.py
from azureml.core import Workspace, Experiment, Environment, ScriptRunConfig

ws = Workspace.from_config()
experiment = Experiment(workspace=ws, name='day1-experiment-hello')

config = ScriptRunConfig(source_directory='./src', script='hello.py', compute_target='cpu-cluster')

run = experiment.submit(config)
aml_url = run.get_portal_url()
print(aml_url)
```

> [!TIP]
> Si vous avez utilisé un autre nom quand vous avez créé le cluster de calcul, veillez à modifier également ce nom dans le code `compute_target='cpu-cluster'`.

### <a name="understand-the-code"></a>Comprendre le code

Voici une description du fonctionnement du script de contrôle :

:::row:::
   :::column span="":::
      `ws = Workspace.from_config()`
   :::column-end:::
   :::column span="2":::
      [L’espace de travail](/python/api/azureml-core/azureml.core.workspace.workspace) se connecte à votre espace de travail Azure Machine Learning, afin que vous puissiez communiquer avec vos ressources Azure Machine Learning.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `experiment =  Experiment( ... )`
   :::column-end:::
   :::column span="2":::
      [L’expérience](/python/api/azureml-core/azureml.core.experiment.experiment) offre un moyen simple d’organiser plusieurs exécutions sous un nom unique. Plus tard, vous pourrez voir comment les expériences facilitent la comparaison des mesures entre des dizaines d’exécutions.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `config = ScriptRunConfig( ... )` 
   :::column-end:::
   :::column span="2":::
      [ScriptRunConfig](/python/api/azureml-core/azureml.core.scriptrunconfig) encapsule votre code `hello.py` et le transmet à votre espace de travail. Comme son nom l’indique, vous pouvez utiliser cette classe pour _configurer_ la manière dont vous souhaitez que votre _script_ _s’exécute_ dans Azure Machine Learning. Il spécifie également la cible de calcul sur laquelle le script s’exécutera. Dans ce code, la cible est le cluster de calcul que vous avez créé dans le [tutoriel de configuration](./quickstart-create-resources.md).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `run = experiment.submit(config)`
   :::column-end:::
   :::column span="2":::
       Soumet votre script. Cette soumission est appelée une [exécution](/python/api/azureml-core/azureml.core.run%28class%29). Un Run encapsule une seule exécution de votre code. Utilisez une exécution pour surveiller la progression du script, capturer la sortie, analyser les résultats, visualiser les métriques, etc.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="":::
      `aml_url = run.get_portal_url()` 
   :::column-end:::
   :::column span="2":::
        L’objet `run` fournit un handle sur l’exécution de votre code. Surveillez la progression depuis Azure Machine Learning Studio avec l’URL affichée par le script Python.  
   :::column-end:::
:::row-end:::


## <a name="submit-and-run-your-code-in-the-cloud"></a><a name="submit"></a> Soumettre et exécuter votre code dans le cloud

1. Sélectionnez **Enregistrer et exécuter le script dans le terminal** pour exécuter le script de contrôle, qui à son tour exécute `hello.py` sur le cluster de calcul que vous avez créé dans le [tutoriel de configuration](quickstart-create-resources.md).

1. Dans le terminal, vous pouvez être invité à vous connecter pour vous authentifier.  Copiez le code et suivez le lien pour effectuer cette étape.

1. Une fois que vous êtes authentifié, vous voyez un lien dans le terminal. Sélectionnez le lien pour afficher l’exécution.

    [!INCLUDE [amlinclude-info](../../includes/machine-learning-py38-ignore.md)]

## <a name="view-the-output"></a>Affichage du résultat

1. Dans la page qui s’ouvre, vous voyez l’état d’exécution.
1. Quand l’état de l’exécution est **Terminé**, sélectionnez **Sortie + journaux** en haut de la page.
1. Sélectionnez **70_driver_log.txt** pour afficher le résultat de votre exécution.

## <a name="monitor-your-code-in-the-cloud-in-the-studio"></a><a name="monitor"></a>Superviser votre code dans le cloud à partir du studio

La sortie de votre script va contenir un lien vers Studio qui se présente comme ceci : `https://ml.azure.com/experiments/hello-world/runs/<run-id>?wsid=/subscriptions/<subscription-id>/resourcegroups/<resource-group>/workspaces/<workspace-name>`.

Suivez le lien.  Dans un premier temps, vous verrez l’état **En file d’attente** ou **Préparation en cours**.  Comptez 5 à 10 minutes pour la première exécution. Ceci est dû aux événements suivants :

* Une image Docker est créée dans le cloud
* Le cluster de calcul repasse de 0 à 1 nœud
* L’image Docker est téléchargée dans le calcul. 

Les exécutions suivantes sont beaucoup plus rapides (environ 15 secondes), car l’image Docker est mise en cache sur le calcul. Vous pouvez le tester en renvoyant le code ci-dessous une fois la première exécution terminée.

Attendez environ 10 minutes.  Vous verrez un message indiquant que l’exécution s’est effectuée. Utilisez ensuite **Actualiser** pour voir l’état passer à *Effectué*.  Une fois le travail terminé, accédez à l’onglet **Sorties + journaux**. Vous pouvez y voir un fichier `70_driver_log.txt` qui se présente comme ceci :

```txt
 1: [2020-08-04T22:15:44.407305] Entering context manager injector.
 2: [context_manager_injector.py] Command line Options: Namespace(inject=['ProjectPythonPath:context_managers.ProjectPythonPath', 'RunHistory:context_managers.RunHistory', 'TrackUserError:context_managers.TrackUserError', 'UserExceptions:context_managers.UserExceptions'], invocation=['hello.py'])
 3: Starting the daemon thread to refresh tokens in background for process with pid = 31263
 4: Entering Run History Context Manager.
 5: Preparing to call script [ hello.py ] with arguments: []
 6: After variable expansion, calling script [ hello.py ] with arguments: []
 7:
 8: Hello world!
 9: Starting the daemon thread to refresh tokens in background for process with pid = 31263
10:
11:
12: The experiment completed successfully. Finalizing run...
13: Logging experiment finalizing status in history service.
14: [2020-08-04T22:15:46.541334] TimeoutHandler __init__
15: [2020-08-04T22:15:46.541396] TimeoutHandler __enter__
16: Cleaning up all outstanding Run operations, waiting 300.0 seconds
17: 1 items cleaning up...
18: Cleanup took 0.1812913417816162 seconds
19: [2020-08-04T22:15:47.040203] TimeoutHandler __exit__
```

À la ligne 8, vous voyez apparaître la sortie du script « Hello World ! » .

Le fichier `70_driver_log.txt` contient la sortie standard d’une exécution. Ce fichier peut être utile lors du débogage des exécutions distantes dans le cloud.


## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous avez pris un script « Hello World » simple et vous l’avez exécuté sur Azure. Vous avez vu comment vous connecter à votre espace de travail Azure Machine Learning, comment créer une expérience et comment soumettre votre code `hello.py` dans le cloud.

Dans le tutoriel suivant, vous allez développer ces apprentissages en exécutant un nom plus intéressant que `print("Hello world!")`.

> [!div class="nextstepaction"]
> [Tutoriel : entraîner un modèle](tutorial-1st-experiment-sdk-train.md)

>[!NOTE] 
> Si vous voulez arrêter la série de tutoriels ici sans passer à l’étape suivante, n’oubliez pas de [nettoyer vos ressources](tutorial-1st-experiment-bring-data.md#clean-up-resources).