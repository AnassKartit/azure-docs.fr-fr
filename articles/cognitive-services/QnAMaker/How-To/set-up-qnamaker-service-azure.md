---
title: Configurer un service QnA Maker - QnA Maker
description: Avant de pouvoir créer des bases de connaissances QnA Maker, vous devez tout d’abord configurer un service QnA Maker dans Azure. Toute personne disposant d’autorisations pour créer des ressources dans un abonnement peut configurer un service QnA Maker.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/14/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: ce152375289cd48681a87775ef534f5b9e6932de
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131071360"
---
# <a name="manage-qna-maker-resources"></a>Gérer les ressources QnA Maker

Avant de pouvoir créer des bases de connaissances QnA Maker, vous devez tout d’abord configurer un service QnA Maker dans Azure. Toute personne disposant d’autorisations pour créer des ressources dans un abonnement peut configurer un service QnA Maker. Si vous essayez la fonctionnalité Réponses aux questions personnalisées, vous devez créer la ressource Analyse de texte, puis ajouter la fonctionnalité.

[!INCLUDE [Custom question answering](../includes/new-version.md)]

Veillez à bien assimiler les concepts suivants avant de créer votre ressource :

* [Ressources QnA Maker](../Concepts/azure-resources.md)
* [Création et publication de clés](../Concepts/azure-resources.md#keys-in-qna-maker)

## <a name="create-a-new-qna-maker-service"></a>Créer un nouveau service QnA Maker

Cette procédure permet de créer les ressources Azure nécessaires pour gérer le contenu de la base de connaissances. Une fois ces étapes terminées, vous trouverez les clés _d’abonnement_ dans la page **Clés** de la ressource dans la portail Azure.

1. Connectez-vous au portail Azure et [créez une ressource QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker).

1. Sélectionnez **Créer** après avoir lu les conditions générales :

    ![Créer un nouveau service QnA Maker](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

1. Dans **QnA Maker**, sélectionnez les niveaux et les régions appropriés :

    ![Créer un service QnA Maker - Niveau tarifaire et régions](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Dans le champ **Nom**, renseignez un nom unique pour identifier ce service QnA Maker. Ce nom identifie également le point de terminaison QnA Maker auquel vos bases de connaissances seront associées.
    * Choisissez **l’abonnement** dans lequel la ressource QnA Maker sera déployée.
    * Sélectionnez le **Niveau tarifaire** pour les services d’administration de QnA Maker (portail et API de gestion). Voir [plus d’informations sur la tarification des références SKU](https://aka.ms/qnamaker-pricing).
    * Créez un nouveau **Groupe de ressources** (recommandé) ou utilisez un groupe de ressources existant dans lequel déployer cette ressource QnA Maker. QnA Maker crée plusieurs ressources Azure. Lorsque vous créez un groupe de ressources pour conserver ces ressources, vous pouvez facilement les rechercher, les gérer et les supprimer par le biais du nom du groupe de ressources.
    * Sélectionnez un **Emplacement du groupe de ressources**.
    * Choisissez le **Niveau tarifaire de recherche** du service Recherche cognitive Azure. Si l’option de niveau Gratuit est indisponible (semble grisée), cela signifie que vous disposez déjà d’un service gratuit déployé dans votre abonnement. Dans ce cas, vous devrez commencer par le niveau De base. Consultez les [Détails de la tarification du service Recherche cognitive Azure](https://azure.microsoft.com/pricing/details/search/).
    * Choisissez **l’emplacement de recherche** où vous souhaitez déployer les index du service Recherche cognitive Azure. Les restrictions relatives à l’emplacement de stockage des données client vous aideront à déterminer l’emplacement que vous choisissez pour la Recherche cognitive Azure.
    * Dans le champ **Nom de l’application**, entrez un nom pour votre instance Azure App Service.
    * Par défaut, App Service utilise le niveau Standard (S1). Vous pouvez modifier le plan après sa création. Consultez des informations supplémentaires sur [la tarification d’App Service](https://azure.microsoft.com/pricing/details/app-service/).
    * Choisissez **l’emplacement du site web** où App Service sera déployé.

        > [!NOTE]
        > **Emplacement de recherche** peut être différent de **Emplacement du site web**.

    * Choisissez si vous souhaitez activer **Application Insights** ou non. Si **Application Insights** est activé, QnA Maker collecte les données de télémétrie sur le trafic, les journaux d’activité de conversation et les erreurs.
    * Choisissez **l’emplacement d’Application Insights** où la ressource Application Insights sera déployée.
    * Pour réduire vos coûts, vous pouvez [partager](configure-QnA-Maker-resources.md#configure-qna-maker-to-use-different-cognitive-search-resource) certaines des ressources Azure créées pour QnA Maker, mais pas toutes.

1. Une fois tous les champs validés, sélectionnez **Créer**. L’exécution de ce processus peut prendre plusieurs minutes.

1. Une fois le déploiement terminé, vous verrez les ressources suivantes créées dans votre abonnement :

   ![Service QnA Maker créé par une ressource](../media/qnamaker-how-to-setup-service/resources-created.png)

    La ressource avec le type _Cognitive Services_ a vos clés _d’abonnement_.

## <a name="upgrade-azure-resources"></a>Mettre à niveau les ressources Azure

### <a name="upgrade-qna-maker-sku"></a>Mettre à niveau la référence SKU de QnA Maker

Si vous souhaitez avoir plus de questions et de réponses dans votre base de connaissances, au-delà de votre niveau actuel, mettez à niveau votre niveau tarifaire du service QnA Maker.

Pour mettre à niveau la référence SKU de gestion de QnA Maker :

1. Accédez à votre ressource QnA Maker dans le portail Azure et sélectionnez **Niveau tarifaire**.

    ![Ressource QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource.png)

1. Choisissez la référence SKU appropriée et appuyez sur **Sélectionner**.

    ![Tarification de QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-pricing-page.png)
    
### <a name="upgrade-app-service"></a>Mise à niveau du service d’application

Lorsque votre base de connaissances doit répondre à un plus grand nombre de demandes de votre application client, mettez à niveau le niveau tarifaire d’App Service.

Vous pouvez effectuer un [scale-up](../../../app-service/manage-scale-up.md) ou un scale-out du service d’application.

Accédez à la ressource App Service dans le portail Azure et sélectionnez l’option **Scale-up** ou **Scale-out** en fonction des besoins.

![Mise à l'échelle du service d’application QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-scale.png)

### <a name="upgrade-the-azure-cognitive-search-service"></a>Mettre à niveau le service Recherche cognitive Azure

Si vous prévoyez de disposer de plusieurs bases de connaissances, mettez à niveau votre niveau tarifaire du service Recherche cognitive Azure.

Il n’est actuellement pas possible d’effectuer une mise à niveau sur place de la référence SKU de la recherche Azure. Toutefois, vous pouvez créer une ressource de recherche Azure avec la référence SKU souhaitée, restaurer les données vers la nouvelle ressource, puis la lier à la pile QnA Maker. Pour ce faire, procédez comme suit :

1. Créez une ressource de recherche Azure dans le portail Azure et sélectionnez la référence SKU souhaitée.

    ![Ressources de recherche QnA Maker Azure](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-new.png)

1. Restaurez les index de votre ressource de recherche Azure d’origine vers la nouvelle. Consultez [l’exemple de code de restauration de sauvegarde](https://github.com/pchoudhari/QnAMakerBackupRestore).

1. Une fois que les données sont restaurées, accédez à votre nouvelle ressource de recherche Azure, sélectionnez **Clés**, puis notez le **Nom** et la **Clé d’administration** :

    ![Clés de recherche QnA Maker Azure](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-azuresearch-keys.png)

1. Pour lier la nouvelle ressource de recherche Azure à la pile QnA Maker, accédez à l’instance App Service de QnA Maker.

    ![Instance du service d’application QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-resource-list-appservice.png)

1. Sélectionnez **Paramètres d'application** et modifiez les paramètres dans les champs **AzureSearchName** et **AzureSearchAdminKey** à partir de l’étape 3.

    ![Paramètre de service d’application QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-settings.png)

1. Redémarrez l’instance App Service.

    ![Redémarrage de l’instance d’App Service de QnA Maker](../media/qnamaker-how-to-upgrade-qnamaker/qnamaker-appservice-restart.png)
    
### <a name="inactivity-policy-for-free-search-resources"></a>Stratégie d’inactivité pour les ressources de recherche gratuites

Si vous n’utilisez pas de ressource QnA Maker, vous devez supprimer toutes les ressources. Si vous ne supprimez pas les ressources inutilisées, votre base de connaissances cessera de fonctionner si vous avez créé une ressource de recherche gratuite.

Les ressources de recherche gratuites sont supprimées après 90 jours sans recevoir d’appel d’API.

## <a name="delete-azure-resources"></a>Supprimer les ressources Azure

Si vous supprimez l’une des ressources Azure utilisées pour vos bases de connaissances, celles-ci ne fonctionneront plus. Avant de supprimer des ressources, veillez à exporter vos bases de connaissances à partir de la page **Paramètres**.

## <a name="next-steps"></a>Étapes suivantes

Découvrez-en plus sur [App Service](../../../app-service/index.yml) et le [service de recherche](../../../search/index.yml).

> [!div class="nextstepaction"]
> [Découvrir comment créer en équipe](../index.yml)
