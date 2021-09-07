---
title: Configurer la collecte de données pour l’agent Azure Monitor (version préliminaire)
description: Explique comment créer une règle de collecte de données pour collecter des données à partir de machines virtuelles à l’aide de l’agent Azure Monitor.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/16/2021
ms.openlocfilehash: 749caf37ee09f9dc794dee60c6d4a5b93da43c6e
ms.sourcegitcommit: e2fa73b682a30048907e2acb5c890495ad397bd3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2021
ms.locfileid: "114386314"
---
# <a name="configure-data-collection-for-the-azure-monitor-agent"></a>Configurer la collecte de données pour l’agent Azure Monitor

Les règles de collecte de données (DCR) définissent les données entrantes dans Azure Monitor et spécifient l’emplacement où elles doivent être envoyées. Cet article explique comment créer une règle de collecte de données pour collecter des données à partir d’ordinateurs virtuels à l’aide de l’agent Azure Monitor.

Pour obtenir une description complète des règles de collecte des données, consultez [Règles de collecte de données dans Azure Monitor](data-collection-rule-overview.md).

> [!NOTE]
> Cet article explique comment configurer des données pour des machines virtuelles avec l’agent Azure Monitor uniquement.

## <a name="data-collection-rule-associations"></a>Associations de règles de collecte de données

Pour appliquer une DCR à une machine virtuelle, vous devez créer une association pour la machine virtuelle. Une machine virtuelle peut avoir une association à plusieurs DCR, et une DCR peut avoir plusieurs machines virtuelles associées. Cela vous permet de définir un ensemble de DCR, chacune correspondant à une exigence particulière, et de les appliquer uniquement aux machines virtuelles appropriées. 

Par exemple, prenons un environnement avec un ensemble de machines virtuelles exécutant une application métier et d’autres exécutant SQL Server. Vous pouvez avoir une règle de collecte de données par défaut qui s’applique à toutes les machines virtuelles et des règles de collecte de données distinctes qui collectent des données spécifiquement pour l’application métier et pour SQL Server. Les associations des machines virtuelles aux règles de collecte de données ressemblent au diagramme suivant.

![Le diagramme montre les machines virtuelles hébergeant des applications métier et les instances SQL Server associées aux règles de collecte des données nommées central-i t-default et lob-app pour les applications métier, et central-i t-default et s q l pour SQL Server.](media/data-collection-rule-azure-monitor-agent/associations.png)



## <a name="create-rule-and-association-in-azure-portal"></a>Créer une règle et une association dans le portail Azure

Vous pouvez utiliser le Portail Azure pour créer une règle de collecte de données et associer des machines virtuelles de votre abonnement à cette règle. L’agent Azure Monitor est automatiquement installé et une identité managée est créée pour toutes les machines virtuelles sur lesquelles il n’est pas déjà installé.

> [!IMPORTANT]
> La création d’une règle de collecte de données à l’aide du portail active également l’identité managée affectée par le système sur les ressources cibles, en plus des identités affectées par l’utilisateur existantes (le cas échéant). Pour les applications existantes, à moins qu’elles ne spécifient l’identité affectée par l’utilisateur dans la demande, la machine utilise par défaut l’identité affectée par le système à la place. [En savoir plus](../../active-directory/managed-identities-azure-resources/managed-identities-faq.md#what-identity-will-imds-default-to-if-dont-specify-the-identity-in-the-request)

                    

> [!NOTE]
> Si vous souhaitez envoyer des données à Log Analytics, vous devez créer la règle de collecte de données dans la **même région** que celle où se trouve votre espace de travail Log Analytics. La règle peut être associée à des machines dans d’autres régions prises en charge.

Dans le menu **Azure Monitor** du Portail Azure, sélectionnez **Règles de collecte des données** dans la section **Paramètres**. Cliquez sur **Créer** pour créer une nouvelle règle de collecte de données et une attribution.

[![Règles de collecte des données](media/data-collection-rule-azure-monitor-agent/data-collection-rules-updated.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rules-updated.png#lightbox)

Cliquez sur **Ajouter** pour créer une règle et un ensemble d’associations. Fournissez un **nom de règle** et spécifiez un **abonnement**, un **groupe de ressources** et une **région**. Spécifie l’emplacement où la DCR sera créée. Les machines virtuelles et leurs associations peuvent se trouver dans n’importe quel abonnement ou groupe de ressources dans le locataire.
En outre, choisissez le **type de plateforme** approprié qui spécifie le type de ressources auquel cette règle peut s’appliquer. La règle personnalisée permet d’appliquer les types Windows et Linux. Cela permet des expériences de création préorganisées avec des options adaptées au type de plateforme sélectionné.

[![Notions de base des règles de collecte des données](media/data-collection-rule-azure-monitor-agent/data-collection-rule-basics-updated.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-basics-updated.png#lightbox)

Dans l’onglet **Ressources**, ajoutez les ressources (machines virtuelles, groupes de machines virtuelles identiques, Azure Arc pour serveurs) auxquelles la règle de collecte de données doit être appliquée. L’agent Azure Monitor sera installé sur les ressources sur lesquelles il n’est pas déjà installé et activera également Azure Managed Identity.

[![Règles de collecte des données - Machines virtuelles](media/data-collection-rule-azure-monitor-agent/data-collection-rule-virtual-machines-updated.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-virtual-machines-updated.png#lightbox)

Sous l’onglet **Collecter et livrer**, cliquez sur **Ajouter une source de données** pour ajouter une source de données et un ensemble de destinations. Sélectionnez un **Type de source de données**, et les détails correspondants à sélectionner s’affichent. Pour les compteurs de performances, vous pouvez sélectionner un jeu d’objets prédéfini et leur taux d’échantillonnage. Pour les événements, vous pouvez sélectionner parmi un ensemble de journaux ou d’installations ainsi que le niveau de gravité. 

[![Source de données - De base](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-basic-updated.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-basic-updated.png#lightbox)


Pour spécifier d’autres journaux et compteurs de performances que les [sources de données actuellement prises en charge](azure-monitor-agent-overview.md#data-sources-and-destinations) ou pour filtrer les événements à l’aide de requêtes XPath, sélectionnez **Personnalisée**. Vous pouvez ensuite spécifier un [XPath](https://www.w3schools.com/xml/xpath_syntax.asp) pour toutes les valeurs spécifiques à collecter. Pour des exemples, consultez [Exemple de DCR](data-collection-rule-overview.md#sample-data-collection-rule).

[![Source de données - Personnalisé](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-custom-updated.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-data-source-custom-updated.png#lightbox)

Sous l’onglet **Destination**, ajoutez une ou plusieurs destinations pour la source de données. Les sources de données d’événement Windows et Syslog peuvent uniquement envoyer aux Journaux Azure Monitor. Les compteurs de performance peuvent envoyer à la fois aux Métriques Azure Monitor et aux Journaux Azure Monitor.

[![Destination](media/data-collection-rule-azure-monitor-agent/data-collection-rule-destination.png)](media/data-collection-rule-azure-monitor-agent/data-collection-rule-destination.png#lightbox)

Cliquez sur **Ajouter une source de données**, puis sur **Vérifier + créer** pour passer en revue les détails de la règle de collecte des données et l’association avec l’ensemble de machines virtuelles. Cliquez sur **Créer** pour la créer.

> [!NOTE]
> Une fois que la règle de collecte des données et les associations ont été créées, il peut falloir jusqu’à 5 minutes pour que les données soient envoyées aux destinations.

## <a name="limit-data-collection-with-custom-xpath-queries"></a>Limiter la collecte de données avec des requêtes XPath personnalisées
Étant donné que vous êtes facturé pour toutes les données collectées dans un espace de travail Log Analytics, vous devez veiller à collecter uniquement les données dont vous avez besoin. En utilisant la configuration de base dans le portail Azure, vous ne disposez que d’une capacité limitée pour filtrer les événements à collecter. Pour les journaux système et d’application, il s’agit de tous les journaux avec une gravité particulière. Pour les journaux de sécurité, il s’agit de tous les journaux de réussite ou d’échec d’audit.

Pour spécifier des filtres supplémentaires, vous devez utiliser la configuration personnalisée et spécifier un XPath qui filtre les événements dont vous n’avez pas besoin. Les entrées XPath sont écrites sous la forme `LogName!XPathQuery`. Par exemple, vous souhaiterez peut-être retourner uniquement les événements du journal des événements d’application avec l’ID d’événement 1035. Le XPathQuery pour ces événements serait `*[System[EventID=1035]]`. Étant donné que vous souhaitez récupérer les événements du journal des événements de l’application, le XPath est `Application!*[System[EventID=1035]]`

Consultez les [Limitations de XPath 1.0](/windows/win32/wes/consuming-events#xpath-10-limitations) pour obtenir la liste des limitations dans le XPath pris en charge par le journal des événements Windows.

> [!TIP]
> Utilisez l’applet de commande PowerShell `Get-WinEvent` avec le paramètre `FilterXPath` pour tester la validité d’une XPathQuery. Le script suivant donne un exemple.
> 
> ```powershell
> $XPath = '*[System[EventID=1035]]'
> Get-WinEvent -LogName 'Application' -FilterXPath $XPath
> ```
>
> - Si des événements sont retournés, la requête est valide.
> - Si vous recevez le message *Aucun événement correspondant aux critères de sélection spécifiés n’a été trouvé*, il se peut que la requête soit valide, mais qu’il n’existe aucun événement correspondant sur l’ordinateur local.
> - Si vous recevez le message *La requête spécifiée n’est pas valide*, la syntaxe de la requête n’est pas valide. 

Le tableau suivant présente des exemples de filtrage d’événements à l’aide d’un XPath personnalisé.

| Description |  XPath |
|:---|:---|
| Collecter uniquement les événements système avec l’ID d’événement = 4648 |  `System!*[System[EventID=4648]]`
| Collecter uniquement les événements système avec l’ID d’événement = 4648 et le nom de processus consent.exe | `Security!*[System[(EventID=4648)]] and *[EventData[Data[@Name='ProcessName']='C:\Windows\System32\consent.exe']]` |
| Collecter tous les événements critiques, d’erreur, d’avertissement et d’informations à partir du journal des événements système, à l’exception de l’ID d’événement = 6 (pilote chargé) |  `System!*[System[(Level=1 or Level=2 or Level=3) and (EventID != 6)]]` |
| Collecter tous les événements de sécurité de réussite et d’échec, à l’exception de l’ID d’événement 4624 (connexion réussie) |  `Security!*[System[(band(Keywords,13510798882111488)) and (EventID != 4624)]]` |


## <a name="create-rule-and-association-using-rest-api"></a>Créer une règle et une association à l’aide de l’API REST

Suivez les étapes ci-dessous pour créer une règle de collecte de données et des associations à l’aide de l’API REST.

> [!NOTE]
> Si vous souhaitez envoyer des données à Log Analytics, vous devez créer la règle de collecte de données dans la **même région** que celle où se trouve votre espace de travail Log Analytics. La règle peut être associée à des machines dans d’autres régions prises en charge.

1. Créez manuellement le fichier de règle de collecte de données à l’aide du format JSON présenté dans [Exemple de règle de collecte de données](data-collection-rule-overview.md#sample-data-collection-rule).

2. Créez la règle à l’aide de l’[API REST](/rest/api/monitor/datacollectionrules/create#examples).

3. Créez une association pour chaque machine virtuelle à la règle de collecte de données à l’aide de l’[API REST](/rest/api/monitor/datacollectionruleassociations/create#examples).


## <a name="create-rule-and-association-using-resource-manager-template"></a>Créer une règle et une association à l’aide d’un modèle Resource Manager

> [!NOTE]
> Si vous souhaitez envoyer des données à Log Analytics, vous devez créer la règle de collecte de données dans la **même région** que celle où se trouve votre espace de travail Log Analytics. La règle peut être associée à des machines dans d’autres régions prises en charge.

Vous pouvez créer une règle et une association pour une machine virtuelle Azure ou un serveur avec Azure Arc à l’aide de modèles Resource Manager. Pour des exemples de modèles, consultez [Exemples de modèle Resource Manager pour les règles de collecte de données dans Azure Monitor](./resource-manager-data-collection-rules.md).


## <a name="manage-rules-and-association-using-powershell"></a>Gérer les règles et l’association à l’aide de PowerShell

> [!NOTE]
> Si vous souhaitez envoyer des données à Log Analytics, vous devez créer la règle de collecte de données dans la **même région** que celle où se trouve votre espace de travail Log Analytics. La règle peut être associée à des machines dans d’autres régions prises en charge.

**Règles de collecte de données**

| Action | Commande |
|:---|:---|
| Obtenir des règles | [Get-AzDataCollectionRule](/powershell/module/az.monitor/get-azdatacollectionrule?view=azps-5.4.0&preserve-view=true) |
| Créer une règle | [New-AzDataCollectionRule](/powershell/module/az.monitor/new-azdatacollectionrule?view=azps-6.0.0&viewFallbackFrom=azps-5.4.0&preserve-view=true) |
| Mettre à jour une règle | [Set-AzDataCollectionRule](/powershell/module/az.monitor/set-azdatacollectionrule?view=azps-6.0.0&viewFallbackFrom=azps-5.4.0&preserve-view=true) |
| Supprimer une règle | [Remove-AzDataCollectionRule](/powershell/module/az.monitor/remove-azdatacollectionrule?view=azps-6.0.0&viewFallbackFrom=azps-5.4.0&preserve-view=true) |
| Mettre à jour les balises pour une règle | [Update-AzDataCollectionRule](/powershell/module/az.monitor/update-azdatacollectionrule?view=azps-6.0.0&viewFallbackFrom=azps-5.4.0&preserve-view=true) |

**Associations de règles de collecte de données**

| Action | Commande |
|:---|:---|
| Obtenir des associations | [Get-AzDataCollectionRuleAssociation](/powershell/module/az.monitor/get-azdatacollectionruleassociation?view=azps-6.0.0&viewFallbackFrom=azps-5.4.0&preserve-view=true) |
| Créer une association | [New-AzDataCollectionRuleAssociation](/powershell/module/az.monitor/new-azdatacollectionruleassociation?view=azps-6.0.0&viewFallbackFrom=azps-5.4.0&preserve-view=true) |
| Supprimer une association | [Remove-AzDataCollectionRuleAssociation](/powershell/module/az.monitor/remove-azdatacollectionruleassociation?view=azps-6.0.0&viewFallbackFrom=azps-5.4.0&preserve-view=true) |



## <a name="manage-rules-and-association-using-azure-cli"></a>Gérer les règles et l’association à l’aide d’Azure CLI

> [!NOTE]
> Si vous souhaitez envoyer des données à Log Analytics, vous devez créer la règle de collecte de données dans la **même région** que celle où se trouve votre espace de travail Log Analytics. La règle peut être associée à des machines dans d’autres régions prises en charge.

Cette option est activée dans le cadre de l’extension **monitor-control-service** d’Azure CLI. [Voir toutes les commandes](/cli/azure/monitor/data-collection/rule?view=azure-cli-latest&preserve-view=true)


## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur l’[agent Azure Monitor](azure-monitor-agent-overview.md).
- En savoir plus sur les [règles de collecte de données](data-collection-rule-overview.md).
