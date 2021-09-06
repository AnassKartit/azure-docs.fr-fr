---
title: Gérer les anomalies de facturation mesurée dans l’Espace partenaires | Place de marché Azure
description: Découvrez comment la détection d’anomalie automatique pour la facturation mesurée vous aide à vérifier que vos clients sont facturés conformément à l’utilisation mesurée de vos offres commerciales de la place de marché.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 5/03/2021
author: sayantanroy83
ms.author: sroy
ms.openlocfilehash: 1226a66a68c9ee8163e1a786cba8f1107c84c2b1
ms.sourcegitcommit: 5163ebd8257281e7e724c072f169d4165441c326
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2021
ms.locfileid: "112414626"
---
# <a name="manage-metered-billing-anomalies-in-partner-center"></a>Gérer les anomalies de facturation mesurée dans l’Espace partenaires

L’option de facturation mesurée personnalisée est disponible pour les offres[SaaS (Software as a service)](plan-saas-offer.md) et les [applications Azure](plan-azure-application-offer.md#types-of-plans) avec un plan d’application managée.

Si vous utilisez l’option de facturation mesurée pour créer des offres dans le programme de la place de marché commerciale qui vous permet de facturer l’utilisation en fonction d’unités non standard, vous devez savoir quand votre client a utilisé un service plus que prévu.

## <a name="use-the-anomaly-detection-feature"></a>Utiliser la fonctionnalité Détection d’anomalie

Microsoft fait appel au partenaire (vous) pour signaler le dépassement d’utilisation par votre client de ses offres SaaS ou d’applications managées par Azure avant que Microsoft ne facture le client. En cas de signalement d’utilisation incorrecte, le client peut recevoir une facture incorrecte, mettant à mal la crédibilité de Microsoft et du partenaire.

Pour vous assurer que vos clients sont facturés correctement, utilisez la fonctionnalité **Détection d’anomalie** pour les applications SaaS et les plans d’application managée Azure. Cette fonctionnalité supervise l’utilisation selon la facturation mesurée et prédit la valeur attendue de l’utilisation dans la plage prévue. Si l’utilisation dépasse la plage attendue, cela est considéré comme inattendu (une anomalie) et vous recevez une notification d’alerte sur votre page Vue d’ensemble de l’offre dans le programme commercial de la place de marché de l’Espace partenaires. Vous pouvez suivre l’utilisation quotidienne de vos clients pour chaque dimension de compteur personnalisée que vous avez définie.

## <a name="view-and-manage-metered-usage-anomalies"></a>Afficher et gérer les anomalies liées à l’utilisation facturée à l’usage

1. Connectez-vous à l’[Espace partenaires](https://partner.microsoft.com/dashboard/home).
1. Dans le menu de navigation de gauche, sélectionnez **Place de marché commerciale** > **Analyser** > **Utilisation**.
1. Sélectionnez l’onglet **Anomalies liées à l’utilisation facturée à l’usage**.

    [![Illustre l’onglet Anomalies liées à l’utilisation facturée à l’usage sur la page Utilisation.](./media/anomaly-detection/metered-usage-anomalies.png)](./media/anomaly-detection/metered-usage-anomalies.png#lightbox)<br>
    ***Figure 1 : Onglet Anomalies liées à l’utilisation facturée à l’usage***

1. Pour toute anomalie d’utilisation détectée par rapport à la facturation à l’usage, en tant qu’éditeur, vous êtes invité à examiner et à confirmer s’il s’agit ou non d’une anomalie. Sélectionnez **Marquer comme anomalie** pour confirmer le diagnostic.

     [![Illustre la boîte de dialogue Marquer comme anomalie.](./media/anomaly-detection/mark-as-anomaly.png)](./media/anomaly-detection/mark-as-anomaly.png#lightbox)<br>
    ***Figure 2 : Boîte de dialogue Marquer comme anomalie***

1. Si vous pensez que l’anomalie liée au dépassement de l’utilisation que nous avons détectée n’est pas avérée, vous pouvez le faire savoir en sélectionnant **Pas d’anomalie** au niveau de l’anomalie signalée par l’Espace partenaires pour le dépassement d’utilisation spécifique.

    [![Illustre la boîte de dialogue Pourquoi ne s’agit-il pas d’une anomalie ?](./media/anomaly-detection/why-is-it-not-an-anomaly.png)](./media/anomaly-detection/why-is-it-not-an-anomaly.png#lightbox)
    ***Figure 3 : Boîte de dialogue Pourquoi ne s’agit-il pas d’une anomalie ?***

1. Vous pouvez faire défiler la page pour afficher une liste des anomalies non confirmées. La liste dresse un inventaire des anomalies que vous n’avez pas confirmées. Vous pouvez choisir de marquer les anomalies signalées par l’Espace partenaires comme étant avérées ou non.

   [![Illustre la liste des anomalies signalées par l’Espace partenaires encore non confirmées sur la page Utilisation.](./media/anomaly-detection/unacknowledged-anomalies.png)](./media/anomaly-detection/unacknowledged-anomalies.png#lightbox)<br>
    ***Figure 4 : Liste des anomalies signalées par l’Espace partenaires encore non confirmées***

    Par défaut, les anomalies signalées qui ont un impact financier estimé supérieur à 100 USD sont affichées dans l’Espace partenaires. Toutefois, vous pouvez sélectionner **Tout** dans la liste **Impact financier estimé des anomalies** pour voir toutes les anomalies signalées.

    :::image type="content" source="./media/anomaly-detection/all-anomalies.png" alt-text="Capture d’écran de toutes les anomalies d’utilisation limitées pour l’offre sélectionnée.":::

1. Vous pouvez également consulter un journal d’actions liées aux anomalies qui reprend les actions que vous avez effectuées concernant les dépassements d’utilisation. Dans le journal des actions, vous pourrez voir quels événements de dépassement d’utilisation ont été marqués comme étant avérés ou non.

   [![Illustre le journal des actions liées aux anomalies sur la page Utilisation.](./media/anomaly-detection/anomaly-action-log.png)](./media/anomaly-detection/anomaly-action-log.png#lightbox)<br>
   ***Figure 5 : Journal des actions liées aux anomalies***

1. L’analyse de l’Espace partenaires ne prend pas en charge la reformulation des événements de dépassement d’utilisation dans les rapports d’exportation. L’Espace partenaires vous permet d’entrer la correction du dépassement d’utilisation pour une anomalie et les détails sont transmis aux équipes Microsoft pour investigation. En fonction de l’investigation, Microsoft émet des remboursements de crédit au client surfacturé, le cas échéant. Lorsque vous sélectionnez l’une des anomalies marquées, vous pouvez sélectionner **Marquer comme anomalie** pour marquer l’anomalie de dépassement d’utilisation comme avérée.

   [![Illustre la boîte de dialogue Marquer comme anomalie.](./media/anomaly-detection/new-reported-usage.png)](./media/anomaly-detection/new-reported-usage.png#lightbox)<br>
   ***Figure 6 : Boîte de dialogue Marquer comme anomalie***

La première fois qu’un dépassement d’utilisation est marqué comme anormal dans l’Espace partenaires, vous avez un délai de 30 jours à partir de cette instance pour marquer l’anomalie comme avérée ou fausse. Après la période de 30 jours, en tant qu’éditeur, vous ne pouvez plus réagir aux anomalies.

> [!IMPORTANT]
> Les unités de dépassement d’utilisation non signalées ne sont pas éligibles à la reformulation ni à l’ajustement financier.

Vous pouvez voir toutes les anomalies (reconnues ou non) pour la période de calcul sélectionnée. Les différentes périodes de calcul sont les 30 derniers jours, les 60 derniers jours et les 90 derniers jours.

> [!IMPORTANT]
> Vous êtes invité à réagir aux anomalies signalées dans les 30 jours qui suivent le premier signalement des anomalies dans l’Espace partenaires.

Le mode de filtre dans le widget vous permet de sélectionner des offres individuelles et des compteurs personnalisés individuels.

Après avoir marqué un dépassement d’utilisation comme une anomalie ou reconnu un modèle qui signalait une anomalie comme étant avérée, vous ne pouvez pas modifier la sélection en « Pas d’anomalie ».

> [!IMPORTANT]
> Vous pouvez soumettre à nouveau des dépassements d’utilisation en cas de surfacturation.

## <a name="see-also"></a>Voir également
- [Facturation à l’usage pour SaaS à l’aide du service de mesure de la consommation de la Place de marché commerciale](./partner-center-portal/saas-metered-billing.md)
- [Facturation des applications managées basée sur des mesures](marketplace-metering-service-apis.md)
- [Service de détection des anomalies pour la facturation à l’usage](./partner-center-portal/anomaly-detection-service-for-metered-billing.md)
