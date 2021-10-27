---
title: Comment modifier des ressources en bloc pour étiqueter des classifications, inscrire des termes au glossaire et modifier des contacts
description: Découvrez les ressources de modification en bloc dans Azure Purview.
author: nayenama
ms.author: nayenama
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 10/15/2021
ms.openlocfilehash: 511c104080acf27e233fecfa3fae7efa814b9bba
ms.sourcegitcommit: 147910fb817d93e0e53a36bb8d476207a2dd9e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2021
ms.locfileid: "130133395"
---
# <a name="how-to-bulk-edit-assets-to-annotate-classifications-glossary-terms-and-modify-contacts"></a>Comment modifier des ressources en bloc pour annoter des classifications, inscrire des termes au glossaire et modifier des contacts

Cet article explique comment étiqueter en une seule action plusieurs termes de glossaire, classifications, propriétaires et experts dans une liste de ressources données.

## <a name="add-assets-to-view-selected-list-using-search"></a>Ajouter des ressources pour afficher la liste sélectionnée à l’aide de la recherche

1. Recherchez la ressource de données que vous souhaitez ajouter à la liste pour la modifier en bloc.

1. Sur la page des résultats de la recherche, pointez sur la ressource que vous souhaitez ajouter à la liste **Afficher la sélection** de modification en bloc pour voir une case.

   :::image type="content" source="media/how-to-bulk-edit-assets/asset-checkbox.png" alt-text="Capture d'écran de la case.":::

1. Cochez la case pour l’ajouter à la liste **Affiche la sélection** de modification en bloc. Une fois l’ajout effectué, l’icône des éléments sélectionnés s’affiche au bas de la page.

   :::image type="content" source="media/how-to-bulk-edit-assets/selected-list.png" alt-text="Capture d'écran de la liste.":::

1. Répétez les étapes ci-dessus pour ajouter toutes les ressources de données à la liste.

## <a name="add-assets-to-view-selected-list-from-asset-detail-page"></a>Ajouter des ressources pour afficher la liste Afficher la sélection à partir de la page des détails de la ressource

Vous pouvez également ajouter une ressource à la liste de modifications en bloc dans la page Détails du bien. Cochez la case située dans le coin supérieur droit pour ajouter la ressource à la liste de modification en bloc.

   :::image type="content" source="media/how-to-bulk-edit-assets/asset-list.png" alt-text="Capture d'écran de la ressource.":::

## <a name="bulk-edit-assets-in-the-view-selected-list-to-add-replace-or-remove-glossary-terms"></a>Modifiez en bloc des ressources de la liste Afficher la sélection pour ajouter, remplacer ou supprimer des termes de glossaire.

1. Lorsque vous avez terminé l’identification de toutes les ressources de données qui doivent être modifiées en bloc, sélectionnez la liste **Afficher la sélection** à partir de la page des résultats de la recherche ou de la page des détails de la ressource.

    :::image type="content" source="media/how-to-bulk-edit-assets/view-list.png" alt-text="Capture d'écran de la vue.":::

1. Passez en revue la liste et sélectionnez **Désélectionner** si vous souhaitez supprimer des ressources dans la liste.

    :::image type="content" source="media/how-to-bulk-edit-assets/remove-list.png" alt-text="Capture d’écran avec le bouton Désélectionner mis en surbrillance.":::

1. Sélectionnez la **modification en bloc** pour ajouter, supprimer ou remplacer des termes de glossaire pour toutes les ressources sélectionnées.

    :::image type="content" source="media/how-to-bulk-edit-assets/bulk-edit.png" alt-text="Capture d’écran avec le bouton de modification en bloc mis en surbrillance.":::

1. Pour ajouter des termes de glossaire, sélectionnez l’opération **Ajouter**. Sélectionnez tous les termes de glossaire à ajouter dans la nouvelle valeur. Sélectionnez Appliquer lorsque vous avez terminé.
    - L’opération d’ajout ajoute la nouvelle valeur à la liste des termes de glossaire déjà étiquetés aux ressources de données.  
   
    :::image type="content" source="media/how-to-bulk-edit-assets/add-list.png" alt-text="Capture d'écran de l'ajout.":::

1. Pour remplacer les termes du glossaire, sélectionnez l’opération **Remplacer par**. Sélectionnez tous les termes de glossaire à remplacer dans la nouvelle valeur. Sélectionnez Appliquer lorsque vous avez terminé.
    - L’opération de remplacement remplace tous les termes de glossaire pour les ressources de données sélectionnées par les termes sélectionnés dans la nouvelle valeur.
   
1. Pour supprimer des termes de glossaire, sélectionnez l’opération **Supprimer**. Sélectionnez Appliquer lorsque vous avez terminé.
    - L’opération de suppression va supprimer tous les termes de glossaire pour les ressources de données sélectionnées.
   
    :::image type="content" source="media/how-to-bulk-edit-assets/replace-list.png" alt-text="Capture d'écran des termes à supprimer.":::

1. Répétez les étapes ci-dessus pour les classifications, les propriétaires et les experts.

    :::image type="content" source="media/how-to-bulk-edit-assets/all-list.png" alt-text="Capture d’écran des classifications et des contacts.":::

1. Une fois que vous avez terminé, fermez le panneau de modification en bloc en sélectionnant **Fermer** ou **Supprimer tout et fermer**. Fermer ne supprimera pas les ressources sélectionnées, tandis que Supprimer tout et fermer supprimera toutes les ressources sélectionnées.
    :::image type="content" source="media/how-to-bulk-edit-assets/close-list.png" alt-text="Capture d'écran de la fermeture.":::

   > [!Important]
   > Le nombre recommandé de ressources pour la modification en bloc est de 25. La sélection de plus de 25 éléments peut entraîner des problèmes de performances.
   > La case **Afficher la sélection** ne s’affiche que si au moins une ressource est sélectionnée.

## <a name="next-steps"></a>Étapes suivantes

Suivez le [Didacticiel : Créer et importer des termes de glossaire](how-to-create-import-export-glossary.md) pour en savoir plus.
