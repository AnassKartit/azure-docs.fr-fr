---
title: Gérer du contenu personnalisé pour Azure Sentinel dans votre propre référentiel | Microsoft Docs
description: Cet article explique comment créer des connexions avec un référentiel GitHub ou Azure DevOps dans lequel vous pouvez enregistrer votre contenu personnalisé.
services: sentinel
cloud: na
documentationcenter: na
author: batamig
manager: rkarlin
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 10/20/2021
ms.author: bagol
ms.custom: ignite-fall-2021
ms.openlocfilehash: 7d1a20fd1d74c3b5057201cac38a9353737f98dd
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131096483"
---
# <a name="deploy-custom-content-from-your-repository-public-preview"></a>Déployer du contenu personnalisé à partir de votre référentiel (préversion publique)

> [!IMPORTANT]
>
> La page **Référentiels** d’Azure Sentinel est actuellement en **PRÉVERSION**. Consultez l’[Avenant aux conditions d’utilisation pour les préversions de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) pour connaître les conditions juridiques supplémentaires s’appliquant aux fonctionnalités Azure sont en version bêta, en préversion ou non encore en disponibilité générale.

Le *contenu* Azure Sentinel est un ensemble de technologies SIEM (informations de sécurité et gestion d’événements) qui aide les clients en lien avec les opérations d’ingestion, de surveillance, de génération d’alertes, de chasse et bien plus encore dans Azure Sentinel. Par exemple, le contenu Azure Sentinel comprend des connecteurs de données, des analyseurs, des classeurs et des règles d’analyse. Pour plus d’informations, consultez [À propos du contenu et des solutions Azure Sentinel](sentinel-solutions.md).

Vous pouvez utiliser le contenu prêt à l’emploi (intégré) fourni dans le hub de contenu Azure Sentinel, et le personnaliser selon vos besoins, ou créer votre propre contenu personnalisé à partir de rien.

Lorsque vous créez du contenu personnalisé, vous pouvez le stocker et le gérer dans vos propres espaces de travail Azure Sentinel ou dans un référentiel de contrôle de code source externe tel que les référentiels GitHub et Azure DevOps. Cet article explique comment créer et gérer les connexions entre Azure Sentinel et des référentiels de contrôle de code source externe. La gestion de votre contenu dans un référentiel externe vous permet de mettre à jour ce contenu en dehors d’Azure Sentinel et de faire en sorte qu’il soit déployé automatiquement dans vos espaces de travail.

> [!TIP]
> Cet article ne décrit *pas* comment créer des types spécifiques de contenu à partir de rien. Pour plus d’informations, consultez le [wiki GitHub Azure Sentinel](https://github.com/Azure/Azure-Sentinel/wiki#get-started) approprié pour chaque type de contenu.
>

## <a name="prerequisites-and-scope"></a>Prérequis et étendue

Avant de connecter votre espace de travail Azure Sentinel à un référentiel de contrôle de code source externe, assurez-vous de disposer de ce qui suit :

- Accès à un référentiel GitHub ou Azure DevOps, avec tous les fichiers de contenu personnalisé que vous souhaitez déployer dans vos espaces de travail, dans des [modèles Azure Resource Manager (ARM)](/azure/azure-resource-manager/templates/) pertinents.

    Azure Sentinel prend actuellement en charge les connexions uniquement avec des référentiels GitHub et Azure DevOps.

- Un rôle **Propriétaire** dans le groupe de ressources contenant votre espace de travail Azure Sentinel. Le rôle **Propriétaire** est requis pour créer la connexion entre Azure Sentinel et votre référentiel de contrôle de code source.

### <a name="maximum-connections-and-deployments"></a>Nombre maximal de connexions et de déploiements

- Chaque espace de travail Azure Sentinel est actuellement limité à **cinq connexions**.

- L’historique de déploiement de chaque groupe de ressources Azure est limité à **800 déploiements**. Si vous avez un grand nombre de déploiements de modèles ARM dans vos groupes de ressources, il se peut qu’une erreur `Deployment QuotaExceeded` s’affiche. Pour plus d’informations, consultez [DeploymentQuotaExceeded](/azure/azure-resource-manager/templates/deployment-quota-exceeded) dans la documentation relative aux modèles Azure Resource Manager.

### <a name="validate-your-content"></a>Valider votre contenu

Le déploiement de contenu sur Azure Sentinel via une connexion de référentiel valide ce contenu uniquement en vérifiant que les données sont au format de modèle ARM approprié.

Nous vous recommandons de valider vos modèles de contenu à l’aide de votre processus de validation normal. Vous pouvez tirer profit des outils et du [processus de validation GitHub d’Azure Sentinel](https://github.com/Azure/Azure-Sentinel/wiki#test-your-contribution) pour configurer votre propre processus de validation.

## <a name="connect-a-repository"></a>Connecter un référentiel

Cette procédure décrit comment connecter un référentiel GitHub ou Azure DevOps à votre espace de travail Azure Sentinel, où vous pouvez enregistrer et gérer votre contenu personnalisé plutôt que dans Azure Sentinel.

Chaque connexion peut prendre en charge plusieurs types de contenus personnalisés, dont des règles d’analyse, des règles d’automatisation, des requêtes de chasse, des analyseurs, des playbooks et des classeurs. Pour plus d’informations, consultez [À propos du contenu et des solutions Azure Sentinel](sentinel-solutions.md).


**Pour créer votre connexion** :

1. Assurez-vous que vous êtes connecté à votre application de contrôle de code source avec les informations d’identification que vous souhaitez utiliser pour votre connexion.  Si vous êtes actuellement connecté à l’aide d’autres informations d’identification, commencez par vous déconnecter.

1. Dans Azure Sentinel, sur la gauche, sous **Gestion du contenu**, sélectionnez **Référentiels**.

1. Sélectionnez **Ajouter nouveau**, puis, dans la page **Créer une connexion**, entrez un nom et une description explicites pour votre connexion.

1. Dans la liste déroulante **Contrôle de code source**, sélectionnez le type de référentiel auquel vous souhaitez vous connecter, puis choisissez **Autoriser**.

1. Sélectionnez l’un des onglets suivants en fonction de votre type de connexion :
    # <a name="github"></a>[GitHub](#tab/github)

    1. Lorsque vous y êtes invité, entrez vos informations d’identification GitHub.

        La première fois que vous ajoutez une connexion, une nouvelle fenêtre ou un nouvel onglet de navigateur s’affichent, vous invitant à autoriser la connexion à Azure Sentinel. Si vous êtes déjà connecté à votre compte GitHub sur le même navigateur, vos informations d’identification GitHub sont remplies automatiquement.

    1. Une zone **Référentiel** s’affiche à présent dans la page **Créer une connexion**, dans laquelle vous pouvez sélectionner un référentiel existant auquel vous connecter. Sélectionnez votre référentiel dans la liste, puis choisissez **Ajouter un référentiel**.

        La première fois que vous vous connectez à un référentiel spécifique, une nouvelle fenêtre ou un nouvel onglet de navigateur s’affichent, vous invitant à installer l’application **Azure-Sentinel** sur votre référentiel. Si vous avez plusieurs référentiels, sélectionnez ceux sur lesquels vous souhaitez installer l’application **Azure-Sentinel**, puis installez-la.

        Vous êtes redirigé vers GitHub pour continuer l’installation de l’application.

    1. Une fois l’application **Azure-Sentinel** installée dans votre référentiel, la liste déroulante **Branche** dans la page **Créer une connexion** est remplie avec vos branches. Sélectionnez la branche que vous souhaitez connecter à votre espace de travail Azure Sentinel.

    1. Dans la liste déroulante **Types de contenus**, sélectionnez le type de contenu que vous allez déployer.

        - Les analyseurs et les requêtes de chasse utilisent l’API **Recherches enregistrées** pour déployer du contenu sur Azure Sentinel. Si vous sélectionnez l’un de ces types de contenus et disposez d’un contenu de l’autre type dans votre branche, les deux types de contenus sont déployés.

        - Pour tous les autres types de contenus, la sélection d’un type de contenu dans le volet **Créer une connexion** a pour effet de déployer uniquement ce contenu sur Azure Sentinel. Le contenu des autres types n’est pas déployé.

    1. Sélectionnez **Créer** pour créer votre connexion. Par exemple :

        :::image type="content" source="media/ci-cd/create-new-connection-github.png" alt-text="Capture d’écran d’une nouvelle connexion de référentiel GitHub.":::


    # <a name="azure-devops"></a>[Azure DevOps](#tab/azure-devops)

    Vous êtes automatiquement connecté à Azure DevOps à l’aide de vos informations d’identification Azure actuelles. Si vous n’êtes pas actuellement connecté à Azure DevOps avec les mêmes informations d’identification que celles que vous utilisez dans Azure Sentinel, changez votre compte dans Azure DevOps pour qu’il corresponde à celui d’Azure Sentinel.

    1.  Dans Azure Sentinel, dans les listes déroulantes qui s’affichent, sélectionnez vos **Organisation**, **Projet**, **Référentiel**, **Branche** et **Types de contenus**.

        - Les analyseurs et les requêtes de chasse utilisent l’API **Recherches enregistrées** pour déployer du contenu sur Azure Sentinel. Si vous sélectionnez l’un de ces types de contenus et disposez d’un contenu de l’autre type dans votre branche, les deux types de contenus sont déployés.

        - Pour tous les autres types de contenus, la sélection d’un type de contenu dans le volet **Créer une connexion** a pour effet de déployer uniquement ce contenu sur Azure Sentinel. Le contenu des autres types n’est pas déployé.

    1. Sélectionnez **Créer** pour créer votre connexion. Par exemple :

        :::image type="content" source="media/ci-cd/create-new-connection-devops.png" alt-text="Capture d’écran d’une nouvelle connexion de référentiel GitHub.":::

    ---

    > [!NOTE]
    > Vous ne pouvez pas créer de connexions en double avec les mêmes référentiel et branche dans un espace de travail Azure Sentinel unique.
    >

Une fois la connexion créée, un nouveau flux de travail ou pipeline est généré dans votre référentiel, et le contenu stocké dans votre référentiel est déployé dans votre espace de travail Azure Sentinel.

La durée du déploiement peut varier en fonction de la quantité de contenu que vous déployez. Affichez l’état du déploiement :

- **Dans GitHub**: sous l’onglet **Actions** du référentiel. Sélectionnez le fichier **.yaml** de flux de travail présenté pour accéder aux journaux de déploiement détaillés et à tous les messages d’erreur spécifiques, le cas échéant.
- **Dans Azure DevOps** : sous l’onglet **Pipelines** du référentiel.

Une fois le déploiement terminé :

- Le contenu stocké dans votre référentiel s’affiche dans votre espace de travail Azure Sentinel, dans la page Azure Sentinel appropriée.

- Les détails de connexion dans la page **Référentiels** sont mis à jour avec le lien vers les journaux de déploiement de la connexion. Par exemple :

    :::image type="content" source="media/ci-cd/deployment-logs-link.png" alt-text="Capture d’écran des journaux de déploiement de la connexion au référentiel GitHub.":::

### <a name="customize-the-deployment-workflow"></a>Personnaliser le flux de travail de déploiement

Le déploiement de contenu par défaut déploie l’ensemble du contenu personnalisé approprié à partir de la branche de référentiel connectée lors de chaque envoi (push) à une partie quelconque de cette branche.

Si la configuration par défaut de votre déploiement de contenu à partir de GitHub ou d’Azure DevOps ne répond pas à tous vos besoins, vous pouvez modifier l’expérience en conséquence.

Par exemple, vous pouvez configurer différents déclencheurs de déploiement, ou déployer du contenu uniquement à partir d’un dossier racine spécifique.

Sélectionnez l’un des onglets suivants en fonction de votre type de connexion :

# <a name="github"></a>[GitHub](#tab/github)

**Pour personnaliser votre flux de travail de déploiement GitHub** :

1. Dans GitHub, accédez à votre référentiel et recherchez votre flux de travail dans le répertoire `.github/workflows`.

    Le nom du flux de travail figure dans la première ligne du fichier de flux de travail et respecte la convention d’affectation de noms par défaut suivante : `Deploy Content to <workspace-name> [<deployment-id>]`.

    Par exemple : `name: Deploy Content to repositories-demo [xxxxx-dk5d-3s94-4829-9xvnc7391v83a]`

1. Sélectionnez le bouton crayon en haut à droite de la page pour ouvrir le fichier à modifier, puis modifiez le déploiement comme suit :

    - **Pour modifier le déclencheur de déploiement**, mettez à jour la section `on` dans le code, qui décrit l’événement déclenchant l’exécution du flux de travail.

        Par défaut, cette configuration est définie sur `on: push`, ce qui signifie que le flux de travail est déclenché lors de tout envoi (push) à la branche connectée, y compris les modifications de contenu et les ajouts de nouveau contenu au référentiel. Par exemple :

        ```yml
        on:
            push:
                branches: [ main ]
                paths:
                - `**`
                - `!.github/workflows/**` # this filter prevents other workflow changes from triggering this workflow
                - `.github/workflows/sentinel-deploy-<deployment-id>.yml`
        ```

        Vous pouvez modifier ces paramètres, par exemple, pour planifier l’exécution périodique du flux de travail ou pour combiner différents événements de flux de travail.

        Pour plus d’informations, consultez la [documentation GitHub](https://docs.github.com/en/actions/learn-github-actions/events-that-trigger-workflows#configuring-workflow-events) sur la configuration des événements de flux de travail.

    - **Pour modifier le chemin d’accès du déploiement** :

        Dans la configuration par défaut présentée ci-dessus pour la section `on`, les caractères génériques (`**`) dans la première ligne de la section `paths` indiquent que la branche entière se trouve dans le chemin d’accès pour les déclencheurs de déploiement.

        Cette configuration par défaut signifie qu’un flux de travail de déploiement est déclenché à chaque envoi (push) de contenu à une partie quelconque de la branche.

        Plus loin dans le fichier, la section `jobs` contient la configuration par défaut suivante : `directory: '${{ github.workspace }}'`. Cette ligne indique que la branche GitHub entière se trouve dans le chemin d’accès pour le déploiement de contenu, sans filtrage des chemins d’accès de dossiers.

        Pour déployer du contenu uniquement à partir d’un chemin d’accès de dossier spécifique, ajoutez-le à la configuration de `paths` et de `directory`. Par exemple, pour déployer du contenu uniquement à partir d’un dossier racine nommé `SentinelContent`, mettez à jour votre code comme suit :

        ```yml
        paths:
        - `SentinelContent/**`
        - `!.github/workflows/**` # this filter prevents other workflow changes from triggering this workflow
        - `.github/workflows/sentinel-deploy-<deployment-id>.yml`

        ...
            directory: '${{ github.workspace }}/SentinelContent'
        ```

Pour plus d’informations, consultez la [documentation GitHub](https://docs.github.com/en/actions/learn-github-actions/workflow-syntax-for-github-actions#onpushpull_requestpaths) sur GitHub Actions et la modification de flux de travail GitHub.

# <a name="azure-devops"></a>[Azure DevOps](#tab/azure-devops)

**Pour personnaliser votre pipeline de déploiement Azure DevOps** :

1. Dans Azure DevOps, accédez à votre référentiel et recherchez votre fichier de définition de pipeline dans le répertoire `.sentinel`.

    Le nom du pipeline figure dans la première ligne du fichier de pipeline et respecte la convention d’affectation de noms par défaut suivante : `Deploy Content to <workspace-name> [<deployment-id>]`.

    Par exemple : `name: Deploy Content to repositories-demo [xxxxx-dk5d-3s94-4829-9xvnc7391v83a]`

1. Sélectionnez le bouton crayon en haut à droite de la page pour ouvrir le fichier à modifier, puis modifiez le déploiement comme suit :

    - **Pour modifier le déclencheur de déploiement**, mettez à jour la section `trigger` dans le code, qui décrit l’événement déclenchant l’exécution du flux de travail.

        Par défaut, cette configuration est définie pour détecter tout envoi (push) à la branche connectée, y compris les modifications de contenu et les ajouts de nouveau contenu au référentiel.

        Modifiez ce déclencheur en n’importe quel déclencheur Azure DevOps disponible, tel un déclencheur de planification ou de demande de tirage (pull request). Pour plus d’informations, consultez la [documentation sur les déclencheurs Azure DevOps](/azure/devops/pipelines/yaml-schema).


    - **Pour modifier le chemin d’accès du déploiement** :

        La configuration par défaut de la section `trigger` contient le code suivant qui indique que la branche `main` se trouve dans le chemin d’accès pour les déclencheurs de déploiement :

        ```yml
        trigger:
            branches:
                include:
                - main
        ```

        Cette configuration par défaut signifie qu’un pipeline de déploiement est déclenché à chaque envoi (push) de contenu à une partie quelconque de la branche `main`.

        Pour déployer du contenu uniquement à partir d’un chemin d’accès de dossier spécifique, ajoutez le nom du dossier à la section `include` pour le déclencheur, et à la section `steps` pour le chemin d’accès de déploiement, ci-dessous si nécessaire.

        Par exemple, pour déployer du contenu uniquement à partir d’un dossier racine nommé `SentinelContent` dans votre branche `main`, ajoutez les paramètres `include` et `workingDirectory` à votre code comme suit :

        ```yml
        paths:
            exclude:
            - .sentinel/*
            include:
            - .sentinel/sentinel-deploy-39d8ekc8-397-5963-49g8-5k63k5953829.yml
            - SentinelContent
        ....
        steps:
        - task: AzurePowerShell@5
          inputs:
            azureSubscription: `Sentinel_Deploy_ServiceConnection_0000000000000000`
            workingDirectory: `SentinelContent`
        ```

Pour plus d’informations, consultez la [documentation Azure DevOps](/azure/devops/pipelines/yaml-schema) sur le schéma YAML Azure DevOps.

---

> [!IMPORTANT]
> Dans GitHub et Azure DevOps, veillez à ce que les répertoires des chemins de déclenchement et de déploiement soient cohérents.
>
## <a name="edit-or-delete-content-in-your-repository"></a>Modifier ou supprimer du contenu dans votre référentiel

Une fois que vous avez créé une connexion à votre référentiel de contrôle de code source, chaque fois que du contenu est modifié ou ajouté dans ce référentiel, le flux de travail de déploiement s’exécute à nouveau et déploie tout le contenu du référentiel sur tous les espaces de travail Azure Sentinel connectés.

Nous vous recommandons de modifier tout contenu stocké dans un référentiel connecté *uniquement* dans le référentiel, pas dans Azure Sentinel. Par exemple, pour apporter des modifications à vos règles d’analyse, faites-le directement dans GitHub ou Azure DevOps.

Si vous avez modifié le contenu dans Azure Sentinel, veillez à l’exporter vers votre référentiel de contrôle de code source pour éviter que vos modifications soient remplacées lors du prochain déploiement du contenu du référentiel sur votre espace de travail.

Si vous supprimez du contenu, veillez à le supprimer tant de votre référentiel que du Portail Azure. La suppression de contenu de votre référentiel n’a pas pour effet de le supprimer de votre espace de travail Azure Sentinel.

## <a name="remove-a-repository-connection"></a>Supprimer une connexion de référentiel

Cette procédure décrit comment supprimer la connexion à un référentiel de contrôle de code source à partir d’Azure Sentinel.

**Pour supprimer votre connexion** :

1. Dans Azure Sentinel, sur la gauche, sous **Gestion du contenu**, sélectionnez **Référentiels**.
1. Dans la grille, sélectionnez la connexion que vous souhaitez supprimer, puis choisissez **Supprimer**.
1. Sélectionnez **Oui** pour confirmer la suppression.

Une fois que vous avez supprimé votre connexion, le contenu précédemment déployé via la connexion reste dans votre espace de travail Azure Sentinel. Le contenu ajouté au référentiel après suppression de la connexion n’est pas déployé.

> [!TIP]
> Si vous rencontrez des problèmes ou un message d’erreur lors de la suppression de votre connexion, nous vous recommandons de vérifier votre contrôle de code source pour vous assurer que le flux de travail GitHub ou le pipeline Azure DevOps associés à la connexion ont été supprimés.
>

### <a name="removing-the-azure-sentinel-app-from-your-github-repository"></a>Suppression de l’application Azure Sentinel de votre référentiel GitHub

Si vous envisagez de supprimer l’application Azure Sentinel d’un référentiel GitHub, nous vous recommandons de supprimer *d’abord* toutes les connexions associées de la page **Référentiels** d’Azure Sentinel.

Chaque installation d’application Azure Sentinel a un ID unique qui est utilisé lors de l’ajout et de la suppression de la connexion. Si l’ID est manquant ou a été modifié, vous devez supprimer la connexion de la page **Référentiels** d’Azure Sentinel, et supprimer manuellement le flux de travail de votre référentiel GitHub pour empêcher tout déploiement de contenu ultérieur.

## <a name="next-steps"></a>Étapes suivantes

Utilisez votre contenu personnalisé dans Azure Sentinel de la même façon que vous utilisez du contenu prêt à l’emploi.

Pour plus d'informations, consultez les pages suivantes :

- [Découvrir et déployer des solutions Azure Sentinel (préversion publique)](sentinel-solutions-deploy.md)
- [Connecteurs de données Azure Sentinel](connect-data-sources.md)
- [Analyseurs du modèle Azure Sentinel Information Model (ASIM) | Microsoft Docs (préversion publique)](normalization-about-parsers.md)
- [Visualiser les données collectées](get-visibility.md)
- [Créer des règles d’analytique personnalisées pour détecter des menaces](detect-threats-custom.md)
- [Repérer les menaces avec Azure Sentinel](hunting.md)
- [Utiliser les Watchlists Azure Sentinel](watchlists.md)
- [Automatiser la réponse aux menaces à l’aide de playbooks dans Azure Sentinel](automate-responses-with-playbooks.md)
