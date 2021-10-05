---
title: Validation fonctionnelle d’une offre Dynamics 365 Operations dans Microsoft AppSource (Place de marché Azure)
description: Effectuez la validation fonctionnelle d’une offre Dynamics 365 Operations dans Microsoft AppSource (Place de marché Azure).
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: vamahtan
ms.author: vamahtan
ms.date: 09/27/2021
ms.openlocfilehash: d2e6316485b1ad3676092e6e19dcc4c6b55d5d2b
ms.sourcegitcommit: 10029520c69258ad4be29146ffc139ae62ccddc7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2021
ms.locfileid: "129080886"
---
# <a name="dynamics-365-for-operations-functional-validation"></a>Validation fonctionnelle de Dynamics 365 for Operations

La publication d’une offre Dynamics 365 for Operations dans [l’Espace partenaires](https://go.microsoft.com/fwlink/?linkid=2166002) nécessite deux validations fonctionnelles :

- Charger une vidéo de démonstration de l’environnement Dynamics 365 qui présente les fonctionnalités de base
- Présenter des captures d’écran montrant l’environnement LCS ([Lifecycle Services](https://lcs.dynamics.com/)) de la solution

> [!NOTE]
> Les publications de recertification ultérieures ne nécessitent pas de démonstration. Pour plus d’informations, consultez le [document sur les stratégies AppSource](/legal/marketplace/certification-policies#1440-dynamics-365-finance-ops).

## <a name="how-to-validate"></a>Processus de validation

Deux options sont disponibles pour la validation fonctionnelle :

- Participer à une conférence téléphonique de 30 minutes pendant les heures de bureau (Heure standard du Pacifique, PST), afin de nous montrer et d’enregistrer l’environnement et la solution [LCS](https://lcs.dynamics.com/), ou
- Dans l’Espace partenaires, accéder à [Place de marché commerciale](https://go.microsoft.com/fwlink/?linkid=2165290), puis charger l’URL d’une vidéo de démonstration et des captures d’écran LCS sous l’onglet Contenu supplémentaire de l’offre.

L’équipe de certification Microsoft examinera la vidéo et les fichiers, puis elle approuvera la solution ou vous enverra des e-mails concernant les étapes à suivre.

> [!NOTE]
> Si la solution ou l’offre que vous créez est un connecteur uniquement, montrez le connecteur fonctionnant pendant l’appel, ou utilisez l’une des options de chargement vidéo répertoriées ci-dessous.  

### <a name="option-1-30-minute-conference-call"></a>Option 1 : Conférence téléphonique de 30 minutes

Pour planifier un appel de révision finale, envoyez un e-mail à l’adresse [appsourceCRM@microsoft.com](mailto:appsourceCRM@microsoft.com) en mentionnant le nom de votre offre et vos disponibilités entre 8 h 00 et 17 h heure standard du Pacifique.

### <a name="option-2-upload-a-demo-video-and-lcs-screenshots"></a>Option n°2 : Charger une vidéo de démonstration et des captures d’écran LCS

#### <a name="workspaces-view"></a>[Vue des espaces de travail](#tab/workspaces-view)

1. Enregistrez une vidéo et chargez son adresse dans le site d’hébergement de votre choix. Voici les conditions à respecter concernant la vidéo :

    - Elle doit pouvoir être visionnée par l’équipe de certification Microsoft.
    - Sa durée doit être inférieure à 20 minutes
    - Elle doit comprendre au maximum trois des fonctionnalités principales de votre solution dans l’environnement Dynamics 365.

    > [!NOTE]
    > Vous pouvez utiliser une vidéo marketing existante si celle-ci respecte ces conditions.

2. Prenez les captures d’écran suivantes de l’environnement [LCS](https://lcs.dynamics.com/), qui correspondent à l’offre ou à la solution que vous souhaitez publier. Elles doivent être suffisamment nettes pour que l’équipe de certification puisse en lire le texte. Enregistrez les captures d’écran au format JPG. Plutôt que de fournir des captures d’écran, vous pouvez autoriser [appSourceCRM@microsoft.com](mailto:appSourceCRM@microsoft.com) à accéder à votre environnement LCS pour qu’ils puissent vérifier la configuration.

    1. Accédez à **LCS** > **Concepteur de processus d’entreprise** > **Bibliothèque de projets**. Prenez des captures d’écran de toutes les étapes du processus. Incluez les colonnes **Diagrammes** et **Révisé**, comme illustré ici :

       :::image type="content" source="media/dynamics-365-operations/project-library.png" alt-text="Affiche la fenêtre de la bibliothèque de projets.":::

    2. Accédez à **LCS** > **Gestion des solutions** > **Tester le package de solution**. Prenez des captures d’écran qui incluent une vue d’ensemble du package et de son contenu, comme dans les exemples suivants :

    | Champ | Image |
    | --- | --- |
    | Vue d’ensemble du package | [![Screenshot that shows the "Package overview" window.](media/dynamics-365-operations/package-overview-45.png)](media/dynamics-365-operations/package-overview.png#lightbox) |
    | <ul><li>Approbateurs de solution</li></ul> | [![Vue d’ensemble du package](media/dynamics-365-operations/solution-approvers-45.png)](media/dynamics-365-operations/solution-approvers.png#lightbox) |
    | Contenu d'un package<ul><li>Modèle</li><li>Package logiciel déployable</li></ul> | [![Contenu du package - Capture 1](media/dynamics-365-operations/package-contents-1-45.png)](media/dynamics-365-operations/package-contents-1.png#lightbox) |
    | <ul><li>Configuration GER</li><li>Sauvegarde de base de données</li></ul><br>Les artefacts ne sont pas nécessaires dans la section **Configuration GER**. | [![Contenu du package - Capture 2](media/dynamics-365-operations/package-contents-2-45.png)](media/dynamics-365-operations/package-contents-2.png#lightbox) |
    | <ul><li>Modèle de rapport Power BI</li><li>Artefact BPM</li></ul><br>Les artefacts ne sont pas nécessaires dans la section **Power BI**. | [![Contenu du package - Capture 3](media/dynamics-365-operations/package-contents-3-45.png)](media/dynamics-365-operations/package-contents-3.png#lightbox) |
    | <ul><li>Traiter le package de données</li><li>Contrat de licence et politique de confidentialité de la solution</li></ul><br>Dans les offres Operations, les sections **Configuration GER** et **Modèle de rapport Power BI** sont facultatives. | [![Contenu du package - Capture 4](media/dynamics-365-operations/package-contents-4-45.png)](media/dynamics-365-operations/package-contents-4.png#lightbox) |

    Pour en savoir plus sur chaque section du portail LCS, consultez le [Guide de l’utilisateur LCS](/dynamics365/fin-ops-core/dev-itpro/lifecycle-services/lcs-user-guide).

3. Chargez le package dans l’Espace partenaires.

    1. Créez un document texte contenant l’adresse de la vidéo de démonstration et des captures d’écran, ou enregistrez chaque capture d’écran au format JPG.
    2. Ajoutez le texte et les images à un fichier .zip dans [l’Espace partenaires](https://go.microsoft.com/fwlink/?linkid=2165290) sur l’onglet **Contenu supplémentaire** de l’offre.

    [![Illustre un fichier zip chargé dans la page de contenu supplémentaire.](./media//dynamics-365-operations/supplemental-content-workspaces.png) ](./media//dynamics-365-operations/supplemental-content-workspaces.png#lightbox)

#### <a name="current-view"></a>[Affichage actuel](#tab/current-view)

1. Enregistrez une vidéo et chargez son adresse dans le site d’hébergement de votre choix. Voici les conditions à respecter concernant la vidéo :

    - Elle doit pouvoir être visionnée par l’équipe de certification Microsoft.
    - Sa durée doit être inférieure à 20 minutes
    - Elle doit comprendre au maximum trois des fonctionnalités principales de votre solution dans l’environnement Dynamics 365.

    > [!NOTE]
    > Vous pouvez utiliser une vidéo marketing existante si celle-ci respecte ces conditions.

2. Prenez les captures d’écran suivantes de l’environnement [LCS](https://lcs.dynamics.com/), qui correspondent à l’offre ou à la solution que vous souhaitez publier. Elles doivent être suffisamment nettes pour que l’équipe de certification puisse en lire le texte. Enregistrez les captures d’écran au format JPG.

    1. Accédez à **LCS** > **Concepteur de processus d’entreprise** > **Bibliothèque de projets**. Prenez des captures d’écran de toutes les étapes du processus. Incluez les colonnes **Diagrammes** et **Révisé**, comme illustré ici :

       :::image type="content" source="media/dynamics-365-operations/project-library.png" alt-text="Affiche la fenêtre de la bibliothèque de projets.":::

      2. Accédez à **LCS** > **Gestion des solutions** > **Tester le package de solution**. Prenez des captures d’écran qui incluent une vue d’ensemble du package et de son contenu, comme dans les exemples suivants :

    | Champ | Image |
    | --- | --- |
    | Vue d’ensemble du package | [![Screenshot that shows the "Package overview" window.](media/dynamics-365-operations/package-overview-45.png)](media/dynamics-365-operations/package-overview.png#lightbox) |
    | <ul><li>Approbateurs de solution</li></ul> | [![Vue d’ensemble du package](media/dynamics-365-operations/solution-approvers-45.png)](media/dynamics-365-operations/solution-approvers.png#lightbox) |
    | Contenu d'un package<ul><li>Modèle</li><li>Package logiciel déployable</li></ul> | [![Contenu du package - Capture 1](media/dynamics-365-operations/package-contents-1-45.png)](media/dynamics-365-operations/package-contents-1.png#lightbox) |
    | <ul><li>Configuration GER</li><li>Sauvegarde de base de données</li></ul><br>Les artefacts ne sont pas nécessaires dans la section **Configuration GER**. | [![Contenu du package - Capture 2](media/dynamics-365-operations/package-contents-2-45.png)](media/dynamics-365-operations/package-contents-2.png#lightbox) |
    | <ul><li>Modèle de rapport Power BI</li><li>Artefact BPM</li></ul><br>Les artefacts ne sont pas nécessaires dans la section **Power BI**. | [![Contenu du package - Capture 3](media/dynamics-365-operations/package-contents-3-45.png)](media/dynamics-365-operations/package-contents-3.png#lightbox) |
    | <ul><li>Traiter le package de données</li><li>Contrat de licence et politique de confidentialité de la solution</li></ul><br>Dans les offres Operations, les sections **Configuration GER** et **Modèle de rapport Power BI** sont facultatives. | [![Contenu du package - Capture 4](media/dynamics-365-operations/package-contents-4-45.png)](media/dynamics-365-operations/package-contents-4.png#lightbox) |

    Pour en savoir plus sur chaque section du portail LCS, consultez le [Guide de l’utilisateur LCS](/dynamics365/fin-ops-core/dev-itpro/lifecycle-services/lcs-user-guide).

3. Chargez le package dans l’Espace partenaires.

    1. Créez un document texte contenant l’adresse de la vidéo de démonstration et des captures d’écran, ou enregistrez chaque capture d’écran au format JPG.
    2. Ajoutez le texte et les images à un fichier .zip dans [l’Espace partenaires](https://go.microsoft.com/fwlink/?linkid=2165290) sur l’onglet **Contenu supplémentaire** de l’offre.

    [![Affiche la fenêtre de la bibliothèque de projets](media/dynamics-365-operations/supplemental-content.png)](media/dynamics-365-operations/supplemental-content.png#lightbox)

---

## <a name="next-steps"></a>Étapes suivantes

- Pour commencer à créer une offre, consultez [Planification d’une offre Microsoft Dynamics 365](marketplace-dynamics-365.md)
- Si vous avez terminé la création de votre offre, il est temps de [Vérifier et publier](dynamics-365-review-publish.md)
