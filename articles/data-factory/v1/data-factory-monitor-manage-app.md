---
title: Monitorage et gestion de pipelines de données – Azure
description: Découvrez comment utiliser l’application de surveillance et gestion pour surveiller et gérer les fabriques de données et les pipelines Azure.
author: dcstwh
ms.author: weetok
ms.reviewer: jburchel
ms.service: data-factory
ms.subservice: v1
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: 76acbba66ccbeb18637ce0f181a702f8696148d6
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128591897"
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-monitoring-and-management-app"></a>Surveiller et gérer les pipelines Azure Data Factory à l’aide de l’application de surveillance et gestion
> [!div class="op_single_selector"]
> * [Utilisation du portail Azure/d’Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Utilisation de l’application de surveillance et gestion](data-factory-monitor-manage-app.md)
>
>

> [!NOTE]
> Cet article s’applique à la version 1 de Data Factory. Si vous utilisez la version actuelle du service Data Factory, consultez [Surveiller et gérer des pipelines Data Factory](../monitor-visually.md).

Cet article explique comment utiliser l’application de surveillance et gestion pour surveiller, gérer et déboguer vos pipelines Azure Data Factory. Vous pouvez commencer à utiliser l’application en regardant la vidéo suivante :

> [!NOTE]
> L’interface utilisateur présentée dans la vidéo ne correspondra peut-être pas exactement à ce que vous voyez dans le portail. Elle est un peu plus ancienne, mais les concepts restent les mêmes. 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-the-monitoring-and-management-app"></a>Lancement de l’application de surveillance et gestion
Pour lancer l’application de surveillance et gestion, cliquez sur la vignette **Surveiller et gérer** dans le panneau **Data Factory** de votre fabrique de données.

:::image type="content" source="./media/data-factory-monitor-manage-app/MonitoringAppTile.png" alt-text="Mosaïque Surveillance sur la page d’accueil de Data Factory":::

L’application de surveillance et gestion devrait s’ouvrir dans une nouvelle fenêtre.  

:::image type="content" source="./media/data-factory-monitor-manage-app/AppLaunched.png" alt-text="Application de surveillance et gestion":::

> [!NOTE]
> Si vous voyez que le navigateur web est bloqué au niveau « Autorisation ... », désactivez la case à cocher **Bloquer les cookies et les données de site tiers** ou laissez cette option sélectionnée, créez une exception pour **login.microsoftonline.com**, puis réessayez d’ouvrir l’application.


Dans la liste Activity Windows (Fenêtres d’activité), dans le volet central, vous pouvez voir une fenêtre d’activité pour chaque exécution d’une activité. Par exemple, si l’activité est planifiée pour s’exécuter toutes les heures pendant cinq heures, vous voyez cinq fenêtres d’activité associées à cinq tranches de données. Si vous ne voyez pas de fenêtre d’activité dans la liste du bas, procédez comme suit :
 
- Mettez à jour les filtres **Heure de départ** et **Heure de fin** en haut de la fenêtre pour adapter les heures de départ et de fin de votre pipeline, puis cliquez sur le bouton **Appliquer**.  
- La liste Activity Windows (Fenêtres d’activité) n’est pas actualisée automatiquement. Cliquez sur le bouton **Actualiser** dans la barre d’outils de la liste **Activity Windows** (Fenêtres d’activité).  

Si vous ne disposez pas d’une application de fabrique de données pour tester ces étapes, suivez ce didacticiel : [Copie de données de Stockage Blob vers SQL Database à l’aide de Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="understand-the-monitoring-and-management-app"></a>Présentation de l’application de surveillance et gestion
Trois onglets sont disponibles sur le côté gauche : **Explorateur de ressources**, **Vues de surveillance** et **Alertes**. Le premier onglet (**Explorateur de ressources**) est sélectionné par défaut.

### <a name="resource-explorer"></a>Explorateur de ressources
Vous voyez l'affichage suivant :

* L’**arborescence** de l’Explorateur de ressources dans le volet gauche.
* Le **vue schématique** en haut du volet central.
* La liste d’**activités Windows** en bas du volet central.
* Les onglets **Propriétés**, **Explorateur de fenêtres d’activités** et **Script** dans le volet de droite.

Dans l’explorateur de ressources, vous pouvez voir toutes les ressources (pipelines, groupes de données, services liés) organisées en arborescence dans la fabrique de données. Lorsque vous sélectionnez un objet dans l’Explorateur de ressources :

* L’entité Data Factory associée est mise en surbrillance dans la vue schématique.
* Les [fenêtres d’activité associées](data-factory-scheduling-and-execution.md) sont mises en évidence dans la liste des fenêtres d’activité en bas de la page.  
* Les propriétés de l’objet sélectionné s’affichent dans la fenêtre Propriétés du volet droit.
* La définition JSON de l’objet sélectionné apparaît, le cas échéant. Par exemple : un service lié, un jeu de données ou un pipeline.

:::image type="content" source="./media/data-factory-monitor-manage-app/ResourceExplorer.png" alt-text="Explorateur de ressources":::

Consultez l’article [Planification et exécution](data-factory-scheduling-and-execution.md) pour obtenir des informations conceptuelles détaillées sur les fenêtres d’activité.

### <a name="diagram-view"></a>Vue du diagramme
La vue schématique d'une fabrique de données est un point unique de surveillance et de gestion d’une fabrique de données et de ses ressources. Lorsque vous sélectionnez une entité Data Factory (jeu de données/pipeline) dans la vue schématique :

* L’entité Data Factory est sélectionnée dans l’arborescence.
* Les fenêtres d’activité associées sont mises en surbrillance dans la liste des fenêtres d’activité.
* Les propriétés de l’objet sélectionné s’affichent dans la fenêtre Propriétés.

Lorsque le pipeline est activé (il n’est alors pas en pause), la barre de sa mosaïque est verte :

:::image type="content" source="./media/data-factory-monitor-manage-app/PipelineRunning.png" alt-text="Pipeline en cours d’exécution":::

Vous pouvez suspendre, reprendre ou arrêter un pipeline en le sélectionnant dans la vue schématique et en utilisant les boutons de la barre de commandes.

:::image type="content" source="./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png" alt-text="Mettre en pause/relancer depuis la barre de commandes":::
 
La vue schématique comporte trois boutons de barre de commandes dédiés au pipeline. Vous pouvez utiliser le deuxième bouton pour mettre en pause le pipeline. La mise en pause n’interrompt pas les activités en cours et les laisse se terminer. Le troisième bouton permet de mettre en pause le pipeline et d’interrompre ses activités en cours. Le premier bouton permet de relancer le pipeline. Lorsque votre pipeline est suspendu, il change de couleur. Par exemple, un pipeline suspendu se présente comme illustré ci-après : 

:::image type="content" source="./media/data-factory-monitor-manage-app/PipelinePaused.png" alt-text="Pipeline suspendu":::

Vous pouvez sélectionner deux pipelines ou plus en maintenant la touche Ctrl enfoncée. Pour mettre en pause/relancer plusieurs pipelines simultanément, utilisez les boutons de la barre de commandes.

Vous pouvez également cliquer sur un pipeline avec le bouton droit et sélectionner les options appropriées pour le suspendre, le reprendre ou l’arrêter. 

:::image type="content" source="./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png" alt-text="Menu contextuel pour un pipeline":::

Cliquez sur l’option **Ouvrir le pipeline** pour afficher toutes les activités dans le pipeline. 

:::image type="content" source="./media/data-factory-monitor-manage-app/OpenPipelineMenu.png" alt-text="Menu Ouvrir un pipeline":::

Dans la vue du pipeline ouvert, vous voyez toutes les activités dans le pipeline. Dans cet exemple, le pipeline ne contient qu’une seule activité : une activité de copie. 

:::image type="content" source="./media/data-factory-monitor-manage-app/OpenedPipeline.png" alt-text="Pipeline ouvert":::

Pour revenir à la vue précédente, cliquez sur le nom de la fabrique de données dans le menu de navigation en haut de la page.

Dans la vue du pipeline, lorsque vous sélectionnez un jeu de données de sortie ou que vous le survolez avec la souris, la fenêtre contextuelle d’activité correspondante s’affiche.

:::image type="content" source="./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png" alt-text="Fenêtre contextuelle d’activité Windows":::

Vous pouvez cliquer sur une fenêtre d’activité pour afficher ses détails dans la fenêtre **Propriétés** du volet droit.

:::image type="content" source="./media/data-factory-monitor-manage-app/ActivityWindowProperties.png" alt-text="Propriétés des fenêtres d’activité":::

Dans le volet droit, affichez l’onglet **Explorateur de fenêtres d’activité** pour afficher plus de détails.

:::image type="content" source="./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png" alt-text="Capture d’écran montrant comment accéder à l’onglet Explorateur de la fenêtre d’activité.":::

Vous voyez également les **variables résolues** pour chaque tentative d’exécution d’une activité dans la section **Tentatives**.

:::image type="content" source="./media/data-factory-monitor-manage-app/ResolvedVariables.PNG" alt-text="Variables résolues":::

Affichez l’onglet **Script** pour afficher la définition du script JSON pour l’objet sélectionné.   

:::image type="content" source="./media/data-factory-monitor-manage-app/ScriptTab.png" alt-text="Onglet Script":::

Les fenêtres d’activité s’affichent à trois emplacements différents :

* La fenêtre contextuelle d’activité dans la vue schématique (volet central).
* L’Explorateur de fenêtres d’activité dans le volet droit.
* La liste des fenêtres d’activité dans le volet inférieur.

Dans la fenêtre contextuelle et l’Explorateur de fenêtres d’activité, vous pouvez passer à la semaine précédente ou suivante à l’aide des flèches gauche et droite.

:::image type="content" source="./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png" alt-text="Flèches gauche/droite de l’Explorateur de fenêtres d’activité":::

En bas de la vue schématique, vous trouverez des boutons pour effectuer un zoom avant ou arrière, zoomer pour ajuster, effectuer un Zoom à 100 % et verrouiller la disposition. Le bouton **Verrouiller la disposition** vous empêche de déplacer accidentellement des tables et des pipelines dans la vue schématique. L’option est activée par défaut. Vous pouvez la désactiver et déplacer des entités dans la vue schématique. Lorsque vous la désactivez, vous pouvez utiliser le dernier bouton pour positionner automatiquement les tables et les pipelines. Vous pouvez également effectuer des zooms avant et arrière à l’aide de la roulette de la souris.

:::image type="content" source="./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png" alt-text="Commandes de zoom de la vue schématique":::

### <a name="activity-windows-list"></a>Liste des fenêtres d’activité
La liste des fenêtres d’activité dans la partie inférieure du volet central affiche toutes les fenêtres d’activité du jeu de données que vous avez sélectionné dans l’Explorateur de ressources ou la vue schématique. Par défaut, la liste est en ordre décroissant, ce qui signifie que la fenêtre d’activité la plus récente s’affiche en premier.

:::image type="content" source="./media/data-factory-monitor-manage-app/ActivityWindowsList.png" alt-text="Liste des fenêtres d’activité":::

Cette liste ne s’actualise pas automatiquement. Pour l’actualiser manuellement, utilisez le bouton d’actualisation de la barre d’outils.  

Les fenêtres d’activité peuvent avoir l’un des statuts suivants :

<table>
<tr>
    <th align="left">Statut</th><th align="left">Sous-état</th><th align="left">Description</th>
</tr>
<tr>
    <td rowspan="8">En attente</td><td>ScheduleTime</td><td>L’heure d’exécution de la fenêtre d’activité n’est pas encore venue.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>Les dépendances en amont ne sont pas prêtes.</td>
</tr>
<tr>
<td>ComputeResources</td><td>Les ressources de calcul ne sont pas disponibles.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Toutes les instances d’activité sont occupées par l’exécution d’autres fenêtres d’activité.</td>
</tr>
<tr>
<td>ActivityResume</td><td>L’activité est mise en pause et ne peut pas exécuter les fenêtres d’activité jusqu’à sa reprise.</td>
</tr>
<tr>
<td>Recommencer</td><td>L’exécution de l’activité est retentée.</td>
</tr>
<tr>
<td>Validation</td><td>La validation n’a pas encore démarré.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Une nouvelle tentative de validation va avoir lieu.</td>
</tr>
<tr>
<tr>
<td rowspan="2">InProgress</td><td>Validation</td><td>La validation est en cours.</td>
</tr>
<td>-</td>
<td>La fenêtre d’activité est en cours de traitement.</td>
</tr>
<tr>
<td rowspan="4">Échec</td><td>TimedOut</td><td>L'exécution de l’activité a pris plus de temps que l’activité ne l’autorise.</td>
</tr>
<tr>
<td>Opération annulée</td><td>La fenêtre d’activité a été annulée par l’utilisateur.</td>
</tr>
<tr>
<td>Validation</td><td>La validation a échoué.</td>
</tr>
<tr>
<td>-</td><td>La génération ou la validation de la fenêtre d’activité ont échoué.</td>
</tr>
<td>Ready</td><td>-</td><td>La fenêtre d’activité est prête à être consommée.</td>
</tr>
<tr>
<td>Ignoré</td><td>-</td><td>La fenêtre d’activité n’a pas été traitée.</td>
</tr>
<tr>
<td>None</td><td>-</td><td>Fenêtre d’activité qui a été réinitialisée alors qu’elle existait avec un statut différent.</td>
</tr>
</table>


Quand vous cliquez sur une fenêtre d’activité dans la liste, les détails la concernant s’affichent dans **l’Explorateur de fenêtres d’activité** ou dans la fenêtre **Propriétés** sur la droite.

:::image type="content" source="./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png" alt-text="Capture d’écran montrant comment afficher des détails à propos d’une fenêtre d’activité.":::

### <a name="refresh-activity-windows"></a>Actualiser les fenêtres d’activité
Les détails ne sont pas automatiquement actualisés. Pour actualiser manuellement la liste des fenêtres d’activité, utilisez le bouton Actualiser (le deuxième) dans la barre de commandes.  

### <a name="properties-window"></a>Fenêtre Propriétés
La fenêtre Propriétés se trouve dans le volet situé tout à droite de l’application de surveillance et gestion.

:::image type="content" source="./media/data-factory-monitor-manage-app/PropertiesWindow.png" alt-text="Fenêtre Propriétés":::

Elle affiche les propriétés de l’élément que vous avez sélectionné dans l’Explorateur de ressources (arborescence), la vue schématique ou la liste des fenêtres d’activité.

### <a name="activity-window-explorer"></a>Explorateur de fenêtres d’activité
L **’Explorateur de fenêtres d’activité** se trouve dans le volet situé tout à droite de l’application de surveillance et gestion. Il affiche des détails sur la fenêtre d’activité que vous avez sélectionnée dans la fenêtre contextuelle ou la liste des fenêtres d’activité.

:::image type="content" source="./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png" alt-text="Explorateur de fenêtres d’activité":::

Vous pouvez passer à une autre fenêtre d’activité en cliquant dessus dans la vue calendaire en haut de l’écran. Vous pouvez également utiliser les boutons flèche gauche/flèche droite en haut de l’écran pour afficher les fenêtres d’activité de la semaine précédente ou suivante.

Vous pouvez utiliser les boutons de la barre d’outils dans le volet inférieur pour réexécuter la fenêtre d’activité ou actualiser les détails affichés dans le volet.

### <a name="script"></a>Script
Vous pouvez utiliser l’onglet **Script** pour afficher la définition JSON de l’entité Data Factory sélectionnée (service lié, jeu de données et pipeline).

:::image type="content" source="./media/data-factory-monitor-manage-app/ScriptTab.png" alt-text="Onglet Script":::

## <a name="use-system-views"></a>Utiliser les vues système
L’application de surveillance et gestion inclut des vues système intégrées (**Fenêtres d’activité récentes**, **Fenêtres d’activité ayant échoué**, **Fenêtres d’activité en cours**), qui vous permettent d’afficher les fenêtres d’activité (récentes, ayant échoué et en cours) de votre fabrique de données.

Affichez l’onglet **Vues de surveillance** sur la gauche en cliquant dessus.

:::image type="content" source="./media/data-factory-monitor-manage-app/MonitoringViewsTab.png" alt-text="Onglet Vues de surveillance":::

Actuellement, trois vues système sont prises en charge. Sélectionnez une option pour afficher les fenêtres d’activité récentes, ayant échoué, ou en cours dans la liste des fenêtres d’activité (en bas du volet central).

Quand vous sélectionnez l’option **Fenêtres d’activité récentes**, toutes les fenêtres d’activité récentes s’affichent dans l’ordre décroissant en fonction de **l’heure de la dernière tentative**.

Vous pouvez utiliser la vue **Fenêtres d’activité ayant échoué** pour afficher toutes les fenêtres d’activité de la liste qui ont échoué. Dans la liste, sélectionnez une fenêtre d’activité ayant échoué pour afficher les détails la concernant dans la fenêtre **Propriétés** ou dans **l’Explorateur de fenêtres d’activité**. Vous pouvez également télécharger tous les journaux d’activité d’une fenêtre d’activité ayant échoué.

## <a name="sort-and-filter-activity-windows"></a>Trier et filtrer les fenêtres d’activité
Pour filtrer les fenêtres d’activité, modifiez les paramètres **d’heure de début** et **d’heure de fin** dans la barre de commandes. Après avoir modifié les heures de début et de fin, cliquez sur le bouton en regard de l’heure de fin pour actualiser la liste des fenêtres d’activité.

:::image type="content" source="./media/data-factory-monitor-manage-app/StartAndEndTimes.png" alt-text="Heures de début et de fin":::

> [!NOTE]
> Pour l’instant, toutes les heures de l’application de surveillance et gestion sont au format UTC.
>
>

Dans la **liste des fenêtres d’activité**, cliquez sur le nom d’une colonne (par exemple, Statut).

:::image type="content" source="./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png" alt-text="Menu déroulant de la liste des fenêtres d’activité":::

Vous pouvez effectuer les opérations suivantes :

* Trier dans l’ordre croissant.
* Trier dans l’ordre décroissant.
* Filtrer selon une ou plusieurs valeurs (Prêt, En attente, etc.).

Lorsque vous spécifiez un filtre sur une colonne, vous voyez que le bouton de filtrage est activé pour cette colonne afin d’indiquer que les valeurs filtrées sont celles de cette colonne.

:::image type="content" source="./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png" alt-text="Filtrer sur une colonne de la liste des fenêtres d’activité":::

Vous pouvez utiliser la même fenêtre contextuelle pour effacer les filtres. Pour effacer tous les filtres de la liste des fenêtres d’activité, cliquez sur le bouton Effacer le filtre dans la barre de commandes.

:::image type="content" source="./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png" alt-text="Effacer tous les filtres de la liste des fenêtres d’activité":::

## <a name="perform-batch-actions"></a>Exécuter des opérations de traitement par lot
### <a name="rerun-selected-activity-windows"></a>Réexécuter les fenêtres d’activité sélectionnées
Sélectionnez une fenêtre d’activité, cliquez sur la flèche vers le bas du premier bouton de la barre de commandes et sélectionnez **Réexécuter** / **Réexécuter avec les fenêtres en amont dans le pipeline**. L’option **Réexécuter avec les fenêtres en amont dans le pipeline** permet de réexécuter également toutes les fenêtres d’activité en amont.
    :::image type="content" source="./media/data-factory-monitor-manage-app/ReRunSlice.png" alt-text="Réexécuter une fenêtre d’activité":::

Vous pouvez également sélectionner plusieurs fenêtres d’activité dans la liste et les réexécuter simultanément. Vous pouvez filtrer les fenêtres d’activité en fonction de leur statut (par exemple, **Échec**), puis réexécuter celles qui ont échoué après avoir corrigé le problème à l’origine de cet échec. Pour en savoir plus sur le filtrage des fenêtres d’activité dans la liste, consultez la section suivante.  

### <a name="pauseresume-multiple-pipelines"></a>Mettre en pause/relancer plusieurs pipelines
Vous pouvez sélectionner 2 pipelines ou plus en maintenant la touche Ctrl enfoncée. Pour les mettre en pause/les relancer, utilisez les boutons de la barre de commandes (mis en évidence dans le rectangle rouge de l’image suivante).

:::image type="content" source="./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png" alt-text="Mettre en pause/relancer depuis la barre de commandes":::
