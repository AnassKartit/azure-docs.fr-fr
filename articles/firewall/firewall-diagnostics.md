---
title: Superviser les journaux d’activité et les métriques du Pare-feu Azure
description: Dans cet article, vous allez apprendre à activer et gérer les journaux d'activité et les métriques du Pare-feu Azure.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 11/04/2020
ms.author: victorh
ms.openlocfilehash: 2899121db4b6a3f202be4860e2e4f43027cdef7c
ms.sourcegitcommit: 99955130348f9d2db7d4fb5032fad89dad3185e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93348762"
---
# <a name="monitor-azure-firewall-logs-and-metrics"></a>Superviser les journaux d’activité et les métriques du Pare-feu Azure

Vous pouvez surveiller le service Pare-feu Azure à l’aide des journaux d’activité de pare-feu. Vous pouvez également utiliser les journaux d’activité pour auditer les opérations sur les ressources de Pare-feu Azure. Grâce aux métriques, vous pouvez afficher des compteurs de performances dans le portail.

Vous pouvez accéder à certains de ces journaux d’activité via le portail. Les journaux d’activité peuvent être envoyés au service [Journaux d’activité Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md), au stockage et aux hubs d’événements, puis analysés dans les journaux d’activité Azure Monitor ou par différents outils comme Excel et Power BI.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Prérequis

Avant de commencer, consultez [Journaux d'activité et métriques du Pare-feu Azure](logs-and-metrics.md) pour obtenir une vue d'ensemble des journaux de diagnostic et des métriques disponibles pour le Pare-feu Azure.

## <a name="enable-diagnostic-logging-through-the-azure-portal"></a>Activer la journalisation des diagnostics via le portail Azure

L’affichage des données dans vos journaux d’activité peut prendre quelques minutes après que vous avez effectué cette procédure d’activation de la journalisation des diagnostics. Si aucune donnée n’apparaît dans un premier temps, patientez quelques minutes supplémentaires, puis vérifiez de nouveau.

1. Sur le portail Azure, ouvrez votre groupe de ressources de pare-feu et sélectionnez le pare-feu.
2. Sous **Supervision** , sélectionnez **Paramètres de diagnostic**.

   Pour Pare-feu Azure, des journaux spécifiques à quatre services sont disponibles :

   * AzureFirewallApplicationRule
   * AzureFirewallNetworkRule
   * AzureFirewallThreatIntelLog
   * AzureFirewallDnsProxy


3. Sélectionnez **Ajouter le paramètre de diagnostic**. La page **Paramètres de diagnostic** contient les paramètres des journaux de diagnostic.
5. Dans cet exemple, les journaux d’activité Azure Monitor stockent les journaux d’activité. Par conséquent, tapez **analytique des journaux d’activité de pare-feu** comme nom.
6. Sous **Journal** , sélectionnez **AzureFirewallApplicationRule** , **AzureFirewallNetworkRule** , **AzureFirewallThreatIntelLog** et **AzureFirewallDnsProxy** pour collecter les journaux.
7. Sélectionnez **Envoyer à Log Analytics** pour configurer votre espace de travail.
8. Sélectionnez votre abonnement.
9. Sélectionnez **Enregistrer**.

## <a name="enable-logging-with-powershell"></a>Activation de la journalisation avec PowerShell

La journalisation d’activité est automatiquement activée pour chaque ressource Resource Manager. Vous devez activer la journalisation des diagnostics pour commencer à collecter les données disponibles dans ces journaux d’activité.

Pour activer la journalisation des diagnostics, procédez comme suit :

1. Notez l’ID de ressource de votre compte de stockage, où les données de journalisation sont stockées. Cette valeur se présente sous la forme : */subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Storage/storageAccounts/\<storage account name\>* .

   Vous pouvez utiliser n’importe quel compte de stockage dans votre abonnement. Vous pouvez utiliser le portail Azure pour rechercher ces informations. Les informations se trouvent dans la page **Propriété** de la ressource.

2. Notez l’ID de ressource de votre pare-feu pour lequel la journalisation est activée. Cette valeur se présente sous la forme : */subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Network/azureFirewalls/\<Firewall name\>* .

   Vous pouvez utiliser le portail pour rechercher ces informations.

3. Activez la journalisation des diagnostics à l’aide de l’applet de commande PowerShell suivante :

    ```powershell
    Set-AzDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/azureFirewalls/<Firewall name> `
   -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> `
   -Enabled $true     
    ```

> [!TIP]
>Les journaux de diagnostic ne nécessitent pas de compte de stockage distinct. L’utilisation du stockage pour la journalisation de l’accès et des performances occasionne des frais de service.

## <a name="enable-diagnostic-logging-by-using-azure-cli"></a>Activer la journalisation des diagnostics à l’aide d’Azure CLI

La journalisation d’activité est automatiquement activée pour chaque ressource Resource Manager. Vous devez activer la journalisation des diagnostics pour commencer à collecter les données disponibles dans ces journaux d’activité.

[!INCLUDE [azure-cli-prepare-your-environment-h3.md](../../includes/azure-cli-prepare-your-environment-h3.md)]

### <a name="enable-diagnostic-logging"></a>Activer la journalisation des diagnostics

Utilisez les commandes suivantes pour activer la journalisation des diagnostics.

1. Exécutez la commande [az monitor diagnostic-settings create](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_create) pour activer la journalisation des diagnostics :

   ```azurecli
   az monitor diagnostic-settings create –name AzureFirewallApplicationRule \
     --resource Firewall07 --storage-account MyStorageAccount
   ```

   Exécutez la commande [az monitor diagnostic-settings list](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_list) pour afficher les paramètres de diagnostic d’une ressource :

   ```azurecli
   az monitor diagnostic-settings list --resource Firewall07
   ```

   Utilisez la commande [az monitor diagnostic-settings show](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_show) pour afficher les paramètres de diagnostic actifs d’une ressource :

   ```azurecli
   az monitor diagnostic-settings show --name AzureFirewallApplicationRule --resource Firewall07
   ```

1. Exécutez la commande [az monitor diagnostic-settings update](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_update) pour mettre à jour les paramètres.

   ```azurecli
   az monitor diagnostic-settings update --name AzureFirewallApplicationRule --resource Firewall07 --set retentionPolicy.days=365
   ```

   Utilisez la commande [az monitor diagnostic-settings delete](/cli/azure/monitor/diagnostic-settings#az_monitor_diagnostic_settings_delete) pour supprimer un paramètre de diagnostic.

   ```azurecli
   az monitor diagnostic-settings delete --name AzureFirewallApplicationRule --resource Firewall07
   ```

> [!TIP]
>Les journaux de diagnostic ne nécessitent pas de compte de stockage distinct. L’utilisation du stockage pour la journalisation de l’accès et des performances occasionne des frais de service.

## <a name="view-and-analyze-the-activity-log"></a>Afficher et analyser le journal d’activité

Vous pouvez afficher et analyser les données du journal d’activité en utilisant l’une des méthodes suivantes :

* **Outils Azure**  : récupérez les informations du journal d’activité en utilisant Azure PowerShell, Azure CLI, l’API REST Azure ou le portail Azure. Des instructions pas à pas pour chaque méthode sont détaillées dans l’article [Opérations d’activité avec Resource Manager](../azure-resource-manager/management/view-activity-logs.md).
* **Power BI** : si vous n’avez pas encore de compte [Power BI](https://powerbi.microsoft.com/pricing), vous pouvez l’essayer gratuitement. À l’aide du [pack de contenus des journaux d’activité Azure pour Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), vous pouvez analyser vos données avec des tableaux de bord préconfigurés à utiliser en l’état ou à personnaliser.
* **Azure Sentinel**  : Vous pouvez connecter des journaux Pare-feu Azure à Azure Sentinel, ce qui vous permet d’afficher les données des journaux dans des classeurs, de les utiliser pour créer des alertes personnalisées et de les incorporer pour améliorer votre investigation. Le connecteur de données Pare-feu Azure dans Azure Sentinel est en préversion publique. Pour plus d’informations, consultez [Connecter des données à partir du Pare-feu Azure](../sentinel/connect-azure-firewall.md).

## <a name="view-and-analyze-the-network-and-application-rule-logs"></a>Afficher et analyser les journaux d’activité de règles et d’application et de réseau

Les [journaux d’activité Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md) collectent les compteurs et les fichiers journaux des événements. Il inclut des visualisations et des fonctionnalités puissantes de recherche pour analyser vos journaux d’activité.

Pour accéder aux exemples de requêtes d’analytique des journaux d’activité de Pare-feu Azure, consultez [Exemples d’analytique des journaux d’activité de pare-feu](log-analytics-samples.md).

Vous pouvez également vous connecter à votre compte de stockage et récupérer les entrées de journal d’activité JSON pour les journaux d’activité d’accès et des performances. Après avoir téléchargé les fichiers JSON, vous pouvez les convertir en CSV et les afficher dans Excel, PowerBI ou tout autre outil de visualisation de données.

> [!TIP]
> Si vous savez utiliser Visual Studio et les concepts de base de la modification des valeurs de constantes et variables en C#, vous pouvez utiliser les [outils de convertisseur de journaux](https://github.com/Azure-Samples/networking-dotnet-log-converter) disponibles dans GitHub.

## <a name="view-metrics"></a>Afficher les mesures
Accédez à un Pare-feu Azure. Sous **Supervision** , sélectionnez **Métriques**. Pour afficher les valeurs disponibles, sélectionnez la liste déroulante **MÉTRIQUE**.

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous avez configuré votre pare-feu pour collecter des journaux d’activité, vous pouvez explorer les journaux d’activité Azure Monitor pour voir vos données.

[Solutions de supervision réseau dans les journaux d’activité Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md)
