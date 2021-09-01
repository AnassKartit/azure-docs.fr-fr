---
title: Gérer un compte Azure Video Analyzer for Media (anciennement Video Indexer)
titleSuffix: Azure Video Analyzer for Media
description: Découvrez comment gérer un compte Azure Video Analyzer for Media (anciennement Video Indexer) connecté à Azure.
services: azure-video-analyzer
author: Juliako
manager: femila
ms.topic: article
ms.subservice: azure-video-analyzer-media
ms.date: 01/14/2021
ms.author: juliako
ms.openlocfilehash: 836cff6a8b55378089a6215b9d0c25e3fcac76c4
ms.sourcegitcommit: 851b75d0936bc7c2f8ada72834cb2d15779aeb69
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2021
ms.locfileid: "123305172"
---
# <a name="manage-a-video-analyzer-for-media-account-connected-to-azure"></a>Gérer un compte Video Analyzer for Media connecté à Azure

Cet article explique comment gérer un compte Azure Video Analyzer for Media (anciennement Video Indexer) connecté à votre abonnement Azure et un compte Azure Media Services.

> [!NOTE]
> Vous devez être le propriétaire du compte Azure Video Analyzer for Media pour pouvoir effectuer les ajustements de configuration de compte décrits dans cette rubrique.

## <a name="prerequisites"></a>Prérequis

Connectez votre compte Azure Video Analyzer for Media, comme indiqué dans [Connecté à Azure](connect-to-azure.md).

Veillez à respecter les [prérequis](connect-to-azure.md#prerequisites-for-connecting-to-azure) et à examiner les [considérations](connect-to-azure.md#azure-media-services-considerations) exposées dans l’article.

## <a name="examine-account-settings"></a>Examiner les paramètres du compte

Cette section examine les paramètres de votre compte Azure Video Analyzer for Media.

Pour afficher les paramètres :

1. Cliquez sur l’icône utilisateur dans l’angle supérieur droit, puis sélectionnez **Paramètres**.

    ![Paramètres dans Video Analyzer for Media](./media/manage-account-connected-to-azure/select-settings.png)

2. Dans la page **Paramètres**, sélectionnez l’onglet **Compte**.

Si votre compte Video Indexer est connecté à Azure, voici ce que vous voyez :

* Le nom du compte Azure Media Services sous-jacent.
* Le nombre de travaux d’indexation en cours d’exécution et en file d’attente.
* Le nombre d’unités réservées allouées et leur type.

Si votre compte a besoin de quelques ajustements, les erreurs et les avertissements importants sur la configuration de votre compte figurent dans la page **Paramètres**. Les messages contiennent des liens vers les emplacements exacts dans le Portail Microsoft Azure où vous devez apporter les modifications. Pour plus d’informations, consultez la section [Erreurs et avertissements](#errors-and-warnings) ci-après.

## <a name="repair-the-connection-to-azure"></a>Corriger le problème de connexion à Azure

Dans la boîte de dialogue **Update connection to Azure Media Services** (Mettre à jour la connexion à Azure Media Services) de la page [Video Analyzer for Media](https://www.videoindexer.ai/), vous êtes invité à fournir des valeurs pour les paramètres suivants :

|Paramètre|Description|
|---|---|
|ID d’abonnement Azure|L’ID d’abonnement peut être récupéré à partir du portail Azure. Cliquez sur **Tous les services** dans le panneau gauche, puis recherchez « abonnements ». Sélectionnez **Abonnements** et choisissez l’ID voulu dans la liste de vos abonnements.|
|Nom du groupe de ressources Azure Media Services|Nom du groupe de ressources dans lequel vous avez créé le compte Media Services.|
|ID de l'application|ID d’application Azure AD (avec des autorisations pour le compte Media Services spécifié) que vous avez créé pour ce compte Video Analyzer for Media. <br/><br/>Pour obtenir l’ID de l’application, accédez au portail Azure. Sous le compte Media Services, choisissez votre compte et accédez à **Accès aux API**. Sélectionnez **Se connecter à l’API Media Services avec le principal de service** -> **Azure AD App**. Copiez les paramètres appropriés.|
|Clé de l'application|Clé d’application Azure AD associée au compte Media Services que vous avez spécifié précédemment. <br/><br/>Pour obtenir la clé d’application, accédez au portail Azure. Sous le compte Media Services, choisissez votre compte et accédez à **Accès aux API**. Sélectionnez **Se connecter à l’API Media Services avec le principal de service** -> **Gérer l’application** -> **Certificats et secrets**. Copiez les paramètres appropriés.|

## <a name="errors-and-warnings"></a>Erreurs et avertissements

Si votre compte a besoin de quelques ajustements, les erreurs et les avertissements importants sur la configuration de votre compte figurent dans la page **Paramètres**. Les messages contiennent des liens vers les emplacements exacts dans le Portail Microsoft Azure où vous devez apporter les modifications. Cette section fournit des détails supplémentaires sur les messages d’erreur et d’avertissement.

* EventGrid

    Vous devez inscrire le fournisseur de ressources EventGrid à partir du Portail Microsoft Azure. Dans le [Portail Microsoft Azure](https://portal.azure.com/), accédez à **Abonnements** > [abonnement] > **ResourceProviders** > **Microsoft.EventGrid**. S’il n’est pas à l’état **Inscrit**, sélectionnez **Inscrire**. Quelques minutes sont nécessaires pour effectuer l’inscription.

* point de terminaison de diffusion en continu

    Vérifiez que le **point de terminaison de streaming** par défaut du compte Media Services sous-jacent est à l’état Démarré. Si ce n’est pas le cas, vous ne pouvez pas regarder de vidéos à partir de ce compte Media Services, ni dans Video Analyzer for Media.

* Unités réservées Multimédia

    Vous devez allouer les unités réservées Multimédia dans votre ressource de service multimédia pour pouvoir indexer des vidéos. Pour des performances d’indexation optimales, il est recommandé d’allouer au moins 10 unités réservées S3. Pour en savoir plus sur les tarifs, consultez la section FAQ de la page [Tarification Media Services](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="next-steps"></a>Étapes suivantes

Vous pouvez interagir par programmation avec votre compte d’évaluation ou avec les comptes Video Analyzer for Media connectés à Azure en suivant les instructions dans : [Utiliser les API](video-indexer-use-apis.md).

Utilisez le même utilisateur Azure AD que lors de la connexion à Azure.
