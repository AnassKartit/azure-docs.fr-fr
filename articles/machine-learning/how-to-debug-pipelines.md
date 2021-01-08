---
title: Résolution des problèmes de pipelines ML
titleSuffix: Azure Machine Learning
description: Comment résoudre les problèmes lorsque vous recevez des erreurs lors de l’exécution d’un pipeline Machine Learning. Écueils habituels et des conseils pour vous aider à déboguer les scripts avant et pendant l’exécution à distance.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
author: lobrien
ms.author: laobri
ms.date: 10/22/2020
ms.topic: troubleshooting
ms.custom: troubleshooting, devx-track-python, contperf-fy21q2
ms.openlocfilehash: 9baf305ab72354c150cb06e594ed8909f2fa1dda
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97739312"
---
# <a name="troubleshooting-machine-learning-pipelines"></a>Résolution des problèmes de pipeline Machine Learning

Dans cet article, vous allez apprendre à résoudre les problèmes lorsque vous obtenez des erreurs lors de l’exécution d’un [pipeline Machine Learning](concept-ml-pipelines.md) dans [Azure Machine Learning SDK](/python/api/overview/azure/ml/intro?preserve-view=true&view=azure-ml-py) et [Azure machine learning designer](./concept-designer.md). 

## <a name="troubleshooting-tips"></a>Conseils de dépannage

Le tableau suivant présente les problèmes courants qui se produisent pendant le développement de pipelines ainsi que les solutions possibles.

| Problème | Solution possible |
|--|--|
| Impossible de transmettre les données au répertoire `PipelineData` | Vérifiez que vous avez créé un répertoire dans le script qui correspond à l’emplacement où votre pipeline attend les données de sortie de l’étape. Dans la plupart des cas, un argument d’entrée définit le répertoire de sortie, puis crée le répertoire explicitement. Utilisez `os.makedirs(args.output_dir, exist_ok=True)` pour créer le répertoire de sortie. Pour obtenir un exemple de script de scoring qui illustre ce modèle de conception, consultez ce [tutoriel](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script). |
| Bogues de dépendance | Si vous constatez dans votre pipeline à distance des erreurs de dépendance qui ne se sont pas produites lors des tests en local, vérifiez que vos dépendances d’environnement distant et les versions correspondent à celles de votre environnement de test. (Voir [Création, mise en cache et réutilisation d’environnement](./concept-environments.md#environment-building-caching-and-reuse))|
| Erreurs ambiguës liées aux cibles de calcul | Essayez de supprimer et recréer les cibles de calcul. La recréation de cibles de calcul est rapide et peut résoudre certains problèmes temporaires. |
| Le pipeline ne réutilise pas les étapes | La réutilisation d’étape est activée par défaut, mais vérifiez que vous ne l’avez pas désactivée dans une étape du pipeline. Si la réutilisation est désactivée, le paramètre `allow_reuse` de l’étape est défini sur `False`. |
| Le pipeline se réexécute inutilement | Pour faire en sorte que les étapes ne se réexécutent que lorsque leurs données ou scripts sous-jacents changent, découplez les répertoires de votre code source pour chaque étape. Si vous utilisez le même répertoire source pour plusieurs étapes, des réexécutions inutiles peuvent se produire. Utilisez le paramètre `source_directory` sur un objet d’étape de pipeline pour pointer vers votre répertoire isolé pour cette étape, et vérifiez que vous n’utilisez pas le même chemin `source_directory` pour plusieurs étapes. |
| Étape ralentissant les époques d’apprentissage ou tout autre comportement de bouclage | Essayez de faire basculer les écritures de fichier, notamment a journalisation, de `as_mount()` à `as_upload()`. Le mode **montage** utilise un système de fichiers virtualisé distant et charge l’intégralité du fichier chaque fois qu’il y est ajouté. |

### <a name="authentication-errors"></a>Erreurs d’authentification

Si vous effectuez une opération de gestion sur une cible de calcul à partir d'un travail distant, vous recevrez l'une des erreurs suivantes : 

```json
{"code":"Unauthorized","statusCode":401,"message":"Unauthorized","details":[{"code":"InvalidOrExpiredToken","message":"The request token was either invalid or expired. Please try again with a valid token."}]}
```

```json
{"error":{"code":"AuthenticationFailed","message":"Authentication failed."}}
```

Par exemple, vous recevrez une erreur si vous essayez de créer ou de joindre une cible de calcul à partir d'un pipeline Machine Learning soumis en vue d'une exécution à distance.

## <a name="debugging-techniques"></a>Techniques de débogage

Il existe trois techniques principales pour déboguer les pipelines : 

* Déboguer des étapes de pipeline individuelles sur votre ordinateur local
* Utiliser une journalisation et Application Insights pour isoler et diagnostiquer la source du problème
* Attacher un débogueur distant à un pipeline en cours d’exécution dans Azure

### <a name="debug-scripts-locally"></a>Déboguer les scripts localement

L’une des erreurs les plus courantes dans un pipeline est que le script de domaine ne s’exécute pas comme prévu, ou contient des erreurs de runtime dans le contexte de calcul distant, qui sont difficiles à déboguer.

Les pipelines eux-mêmes ne peuvent pas être exécutés localement, mais l’exécution de scripts en isolation sur votre ordinateur local vous permet de déboguer plus rapidement dans la mesure où vous n’avez pas besoin d’attendre le processus de génération de calcul et d’environnement. Cela demande un peu de travail de développement :

* Si vos données se trouvent dans un magasin de données cloud, vous devez les télécharger et les rendre accessibles à votre script. Utiliser un petit échantillon de données est un bon moyen de réduire la durée d’exécution et d’être rapidement renseigné sur le comportement du script.
* Si vous tentez de simuler une étape de pipeline intermédiaire, vous devrez peut-être créer manuellement les types d’objet que le script attend de l’étape précédente.
* Vous devrez aussi définir votre propre environnement et répliquer les dépendances définies dans votre environnement de calcul distant.

Une fois que vous avez configuré un script pour qu’il s’exécute dans un environnement local, il est beaucoup plus facile d’effectuer certaines tâches de débogage, notamment :

* Attacher une configuration de débogage personnalisée
* Suspendre l’exécution et inspecter l’état des objets
* Intercepter les erreurs de type ou les erreurs logiques qui n’apparaîtront pas avant l’exécution

> [!TIP] 
> Une fois que vous pouvez vérifier que votre script s’exécute comme prévu, nous vous recommandons d’exécuter le script dans un pipeline à une seule étape avant de tenter de l’exécuter dans un pipeline à plusieurs étapes.

## <a name="configure-write-to-and-review-pipeline-logs"></a>Configurer, écrire et consulter des journaux de pipeline

Tester les scripts localement est un excellent moyen de déboguer des fragments de code importants et une logique complexe avant de commencer à créer un pipeline. Mais à un moment donné, vous devrez probablement déboguer des scripts pendant l’exécution effective du pipeline, surtout quand il s’agira de diagnostiquer le comportement que les étapes du pipeline interagiront. Nous vous recommandons d’utiliser à loisir les instructions `print()` dans les scripts d’étapes pour voir l’état des objets et les valeurs attendues pendant l’exécution distante, comme vous le feriez pour déboguer du code JavaScript.

### <a name="logging-options-and-behavior"></a>Options et comportement de journalisation

Le tableau ci-dessous fournit des informations sur les différentes options de débogage pour les pipelines. Cette liste n’est pas exhaustive, car il existe d’autres options que les solutions zure Machine Learning, Python ou OpenCensus présentées ici.

| Bibliothèque                    | Type   | Exemple                                                          | Destination                                  | Ressources                                                                                                                                                                                                                                                                                                                    |
|----------------------------|--------|------------------------------------------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kit de développement logiciel (SDK) Azure Machine Learning | Métrique | `run.log(name, val)`                                             | Interface utilisateur du portail Azure Machine Learning             | [Comment suivre les expériences](how-to-track-experiments.md)<br>[azureml.core.Run class](/python/api/azureml-core/azureml.core.run%28class%29?preserve-view=true&view=azure-ml-py)                                                                                                                                                 |
| Impression/Journalisation pour Python    | Journal    | `print(val)`<br>`logging.info(message)`                          | Journaux du pilote, concepteur Azure Machine Learning | [Comment suivre les expériences](how-to-track-experiments.md)<br><br>[Journalisation pour Python](https://docs.python.org/2/library/logging.html)                                                                                                                                                                       |
| OpenCensus pour Python          | Journal    | `logger.addHandler(AzureLogHandler())`<br>`logging.log(message)` | Application Insights – Traces                | [Déboguer des pipelines dans Application Insights](./how-to-log-pipelines-application-insights.md)<br><br>[Exportateurs OpenCensus Azure Monitor](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-azure)<br>[Fiches pratiques concernant la journalisation pour Python](https://docs.python.org/3/howto/logging-cookbook.html) |

#### <a name="logging-options-example"></a>Exemple d’options du journalisation

```python
import logging

from azureml.core.run import Run
from opencensus.ext.azure.log_exporter import AzureLogHandler

run = Run.get_context()

# Azure ML Scalar value logging
run.log("scalar_value", 0.95)

# Python print statement
print("I am a python print statement, I will be sent to the driver logs.")

# Initialize python logger
logger = logging.getLogger(__name__)
logger.setLevel(args.log_level)

# Plain python logging statements
logger.debug("I am a plain debug statement, I will be sent to the driver logs.")
logger.info("I am a plain info statement, I will be sent to the driver logs.")

handler = AzureLogHandler(connection_string='<connection string>')
logger.addHandler(handler)

# Python logging with OpenCensus AzureLogHandler
logger.warning("I am an OpenCensus warning statement, find me in Application Insights!")
logger.error("I am an OpenCensus error statement with custom dimensions", {'step_id': run.id})
``` 

## <a name="azure-machine-learning-designer"></a>Concepteur Azure Machine Learning

Pour les pipelines créés dans le concepteur, vous trouverez le fichier **70_driver_log** dans la page de création ou dans la page des détails d’exécutions de pipeline.

### <a name="enable-logging-for-real-time-endpoints"></a>Activer la journalisation pour les points de terminaison en temps réel

Pour dépanner et déboguer des points de terminaison en temps réel dans le concepteur, vous devez activer la journalisation Application Insights à l’aide du Kit de développement logiciel (SDK). La journalisation vous permet de déboguer et dépanner les problèmes de déploiement et d’utilisation du modèle. Pour plus d’informations, consultez [Journalisation pour les modèles déployés](./how-to-enable-app-insights.md). 

### <a name="get-logs-from-the-authoring-page"></a>Obtenir des journaux à partir de la page de création

Quand vous publiez une exécution de pipeline en restant dans la page de création, vous voyez les fichiers journaux générés pour chaque module à mesure que chaque module termine son exécution.

1. Sélectionnez un module dont l’exécution est terminée dans le canevas de création.
1. Dans le volet droit du module, accédez à l’onglet **Sorties + journaux**.
1. Développez le volet droit, puis sélectionnez le fichier **70_driver_log.txt** pour afficher le fichier dans le navigateur. Vous pouvez également télécharger les journaux localement.

    ![Volet de sortie développé dans le concepteur](./media/how-to-debug-pipelines/designer-logs.png)

### <a name="get-logs-from-pipeline-runs"></a>Obtenir des journaux à partir des exécutions de pipeline

Vous pouvez également trouver les fichiers journaux d’exécutions spécifiques dans la page des détails d’exécutions de pipeline, qui est disponible dans la section **Pipelines** ou la section **Expériences** du studio.

1. Sélectionnez une exécution de pipeline créée dans le concepteur.

    ![Page d’exécutions de pipeline](./media/how-to-debug-pipelines/designer-pipelines.png)

1. Sélectionnez un module dans le volet d’aperçu.
1. Dans le volet droit du module, accédez à l’onglet **Sorties + journaux**.
1. Développez le volet droit pour afficher le fichier **70_driver_log.txt** dans le navigateur, ou sélectionnez le fichier pour télécharger les journaux localement.

> [!IMPORTANT]
> Pour mettre à jour un pipeline à partir de la page des détails d’exécution du pipeline, vous devez **cloner** l’exécution du pipeline dans un nouveau brouillon de pipeline. Une exécution de pipeline est un instantané du pipeline. Elle est similaire à un fichier journal et ne peut pas être modifiée. 

## <a name="application-insights"></a>Application Insights
Pour en savoir plus sur l’utilisation de la bibliothèque Python OpenCensus de cette manière, consultez ce guide : [Déboguer et résoudre les problèmes de pipelines de Machine Learning dans Application Insights](./how-to-log-pipelines-application-insights.md)

## <a name="interactive-debugging-with-visual-studio-code"></a>Débogage interactif avec Visual Studio Code

Dans certains cas, vous devrez peut-être déboguer interactivement le code Python utilisé dans votre pipeline ML. À l’aide de Visual Studio Code (VS Code) et de debugpy, vous pouvez attacher le code au fur et à mesure de son exécution dans l’environnement d’apprentissage. Pour plus d’informations, consultez le [guide de débogage interactif dans VS Code](how-to-debug-visual-studio-code.md#debug-and-troubleshoot-machine-learning-pipelines).

## <a name="next-steps"></a>Étapes suivantes

* [Déboguer et détecter un problème ParallelRunStep](how-to-debug-parallel-run-step.md)

* Pour accéder à un tutoriel complet sur l'utilisation de `ParallelRunStep`, consultez [Tutoriel : Créer un pipeline Azure Machine Learning pour le scoring par lots](tutorial-pipeline-batch-scoring-classification.md).

* Pour obtenir un exemple complet de Machine Learning automatisé dans des pipelines ML, consultez [Utiliser le ML automatisé dans un pipeline Azure Machine Learning en Python](how-to-use-automlstep-in-pipelines.md).

* Consultez les informations de référence sur le SDK pour obtenir de l’aide sur les packages [azureml-pipelines-core](/python/api/azureml-pipeline-core/?preserve-view=true&view=azure-ml-py) et [azureml-pipelines-steps](/python/api/azureml-pipeline-steps/?preserve-view=true&view=azure-ml-py).

* Consultez la liste des [exceptions et codes d’erreur du concepteur](algorithm-module-reference/designer-error-codes.md).