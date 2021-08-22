---
title: Guide d’intégration de Microsoft Security Code Analysis
description: Découvrez comment intégrer et installer l’extension Microsoft Security Code Analysis. Découvrez les conditions préalables et des ressources supplémentaires.
author: sukhans
manager: sukhans
ms.author: terrylan
ms.date: 03/22/2021
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: e5d9f30f2c5fad33f597ea3b977996ee75d4d1a1
ms.sourcegitcommit: 0af634af87404d6970d82fcf1e75598c8da7a044
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/15/2021
ms.locfileid: "112115946"
---
# <a name="onboarding-and-installing"></a>Intégration et installation

> [!Note]
> Le 1er mars 2022, l’extension Microsoft Security Code Analysis (MSCA) sera supprimée. Les clients MSCA existants conserveront leur accès à MSCA jusqu’au 1er mars 2022. Pour découvrir d’autres options dans Azure DevOps, consultez [Outils d’analyse du code source OWASP](https://owasp.org/www-community/Source_Code_Analysis_Tools). Pour les clients qui prévoient de migrer vers GitHub, vous pouvez consulter [GitHub Advanced Security](https://docs.github.com/github/getting-started-with-github/about-github-advanced-security).

Conditions préalables pour commencer à utiliser Microsoft Security Code Analysis :

- Une offre de Support unifié Microsoft éligible, telle que mentionnée dans la section suivante.
- Une organisation Azure DevOps.
- L’autorisation d’installer des extensions à l’organisation Azure DevOps.
- Un code source qui peut être synchronisé avec un pipeline Azure DevOps hébergé dans le cloud.

## <a name="onboarding-the-microsoft-security-code-analysis-extension"></a>Intégration de l’extension Microsoft Security Code Analysis

### <a name="interested-in-purchasing-the-microsoft-security-code-analysis-extension"></a>Vous souhaitez acheter l’extension Microsoft Security Code Analysis ?

Si vous disposez de l’une des offres de support suivantes, contactez votre responsable technique de compte pour acheter ou échanger des heures existantes afin d’accéder à l’extension :

- Niveau avancé de Support unifié
- Niveau de performance de Support unifié
- Support Premier pour les développeurs
- Support Premier pour les partenaires
- Support Premier pour l’entreprise

Si vous n’avez pas l’un des contrats de support susmentionnés, vous pouvez acheter l’extension auprès de l’un de nos partenaires.

**Étapes suivantes :**

Si vous disposez des qualifications ci-dessus, contactez un partenaire de la liste ci-dessous pour acheter l’extension Microsoft Security Code Analysis. Sinon, contactez le [support de Microsoft Security Code Analysis](mailto:mscahelp@microsoft.com?Subject=Microsoft%20Security%20Code%20Analysis%20Support%20Request).

>**Partenaires :**

- Zones – Coordonnées : cloudsupport@zones.com
- Wortell – Coordonnées : info@wortell.nl
- Logique - Coordonnées : logicalisleads@us.logicalis.com

### <a name="become-a-partner"></a>Devenir partenaire

L’équipe Microsoft Security Code Analysis cherche à intégrer des partenaires à un contrat Support Premier pour les partenaires. Les partenaires aideront les clients Azure DevOps à développer de façon plus sécurisée en vendant l’extension aux clients qui souhaitent l’acheter, mais qui n’ont pas de contrat Support Entreprise avec Microsoft. Les partenaires intéressés peuvent s’inscrire [ici](http://www.microsoftpartnersupport.com/msrd/opin).

## <a name="installing-the-microsoft-security-code-analysis-extension"></a>Installation de l’extension Microsoft Security Code Analysis

1. Une fois l’extension partagée avec votre organisation Azure DevOps, accédez à la page de votre organisation Azure DevOps. `https://dev.azure.com/contoso` est un exemple d’URL pour une telle page.
1. Sélectionnez l’icône de panier d’achat en haut à droite à côté de votre nom, puis **Gérer les extensions**.
1. Sélectionnez **Partagé**.
1. Sélectionnez l’extension Microsoft Security Code Analysis, puis **Installer**.
1. Dans la liste déroulante, choisissez l’organisation Azure DevOps sur laquelle installer l’extension.
1. Sélectionnez **Installer**. Une fois l’installation terminée, vous pouvez commencer à l’utiliser.

>[!NOTE]
> Même si vous n’avez pas accès à l’installation de l’extension, poursuivez avec les étapes d’installation. Vous pouvez demander l’accès à l’administrateur de votre organisation Azure DevOps lors du processus d’installation.

Une fois l’extension installée, les tâches de build de développement sécurisé sont visibles et disponibles pour l’ajout à vos Azure Pipelines.

## <a name="adding-specific-build-tasks-to-your-azure-devops-pipeline"></a>Ajout de tâches de build spécifiques à votre pipeline Azure DevOps

1. À partir de votre organisation Azure DevOps, ouvrez votre projet d’équipe.
1. Sélectionnez **Pipelines** > **Builds**.
1. Sélectionnez le pipeline auquel vous voulez ajouter les tâches de build de l’extension :
   - Nouveau pipeline : Sélectionnez **Nouveau**, puis suivez les étapes détaillées pour créer un pipeline.
   - Modifier le pipeline : Sélectionnez un pipeline, puis sélectionner **Modifier** pour commencer à le modifier.
1. Sélectionnez **+** , puis accédez au volet **Ajouter des tâches**.
1. Dans la liste ou via la zone de recherche, recherchez la tâche de build à ajouter. Sélectionnez **Ajouter**.
1. Spécifiez les paramètres nécessaires pour la tâche.
1. Placez une nouvelle build dans la file d'attente.
   >[!NOTE]
   >Les chemins d’accès de fichier et de dossier sont relatifs à la racine de votre référentiel source. Si vous spécifiez les fichiers et les dossiers de sortie en tant que paramètres, ils sont remplacés par l’emplacement commun que nous avons défini sur l’agent de build.

> [!TIP]
>
> - Pour exécuter une analyse après votre build, placez les tâches de build de Microsoft Security Code Analysis après l’étape Publier les artefacts de build de votre build. De cette façon, votre build peut se terminer et publier les résultats avant d’exécuter les outils d’analyse statique.
> - Activez toujours l’option **Continuer en cas d’erreur** pour les tâches de build de développement sécurisé. Même si l’un des outils échoue, les autres peuvent s’exécuter. Il n’y a aucune interdépendance entre les outils.
> - Les tâches de build de Microsoft Security Code Analysis échouent uniquement en cas d’échec d’exécution d’un outil. Mais elles fonctionnent même si un outil identifie des problèmes dans le code. Vous pouvez configurer votre build pour qu’elle échoue quand un outil identifie des problèmes dans le code en utilisant la tâche de build Post-analyse.
> - Certaines tâches de build d’Azure DevOps ne sont pas prises en charge quand elles sont exécutées via un pipeline de mise en production. Plus précisément, Azure DevOps ne prend pas en charge les tâches qui publient des artefacts à partir d’un pipeline de mise en production.
> - Pour obtenir la liste des variables prédéfinies dans Azure DevOps Team Build que vous pouvez spécifier en tant que paramètres, voir [Variables de build d’Azure DevOps](/azure/devops/pipelines/build/variables?tabs=batch).

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la configuration des tâches de build, consultez notre [Guide de configuration](security-code-analysis-customize.md) ou notre [Guide de configuration YAML](yaml-configuration.md).

Si vous avez d’autres questions sur l’extension et les outils proposés, voir notre [page FAS](security-code-analysis-faq.yml).