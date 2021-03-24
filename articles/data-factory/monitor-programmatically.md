---
title: Surveiller par programmation une fabrique de données Azure
description: Découvrez comment surveiller un pipeline dans une fabrique de données à l’aide de différents kits de développement logiciels (SDK).
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/16/2018
author: dcstwh
ms.author: weetok
ms.custom: devx-track-python
ms.openlocfilehash: 6c913c7c623c77baea0c575d06d2c44709af43fa
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101740435"
---
# <a name="programmatically-monitor-an-azure-data-factory"></a>Surveiller par programmation une fabrique de données Azure

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Cet article explique comment surveiller un pipeline dans une fabrique de données à l’aide de différents kits de développement logiciels (SDK). 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="data-range"></a>Plage de données

La fabrique de données stocke uniquement les données d’exécution du pipeline pendant 45 jours. Lorsque vous interrogez par programme les données sur les exécutions du pipeline de la fabrique de données, par exemple, avec la commande PowerShell `Get-AzDataFactoryV2PipelineRun`, il n’existe aucune date maximale pour les paramètres `LastUpdatedAfter` et `LastUpdatedBefore` facultatifs. Cependant, si vous interrogez les données de l’année précédente, par exemple, vous n’obtiendrez pas d’erreur, mais seulement les données d’exécution de pipeline des 45 derniers jours.

Si vous souhaitez conserver les données d’exécution de pipeline pendant plus de 45 jours, configurez votre propre journalisation des diagnostics avec [Azure Monitor](monitor-using-azure-monitor.md).

## <a name="pipeline-run-information"></a>Informations sur l’exécution de pipeline

Pour les propriétés d’exécution de pipeline, consultez la [référence de l’API PipelineRun](/rest/api/datafactory/pipelineruns/get#pipelinerun). Une exécution de pipeline a différents états pendant son cycle de vie. Les valeurs possibles de l’état d’exécution sont répertoriées ci-dessous :

* Mis en file d'attente.
* InProgress
* Opération réussie
* Échec
* Canceling
* Opération annulée

## <a name="net"></a>.NET
Pour obtenir une description complète de la création et de la surveillance d’un pipeline à l’aide du Kit de développement logiciel (SDK) .NET, consultez [Créer une fabrique de données et un pipeline avec .NET](quickstart-create-data-factory-dot-net.md).

1. Ajoutez le code suivant afin de vérifier en permanence l’état de l’exécution du pipeline jusqu’à la fin de la copie des données.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress" || pipelineRun.Status == "Queued")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Ajoutez le code suivant qui récupère les détails de l’exécution de l’activité de copie, par exemple la taille des données lues/écrites.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");
   
    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
    if (pipelineRun.Status == "Succeeded")
        Console.WriteLine(activityRuns.First().Output);
    else
        Console.WriteLine(activityRuns.First().Error);
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

Pour une documentation complète sur le SDK .NET, consultez la [référence au SDK .NET de Data Factory](/dotnet/api/microsoft.azure.management.datafactory).

## <a name="python"></a>Python
Pour obtenir une description complète de la création et de la surveillance d’un pipeline à l’aide du Kit de développement logiciel (SDK) Python, consultez [Créer une fabrique de données et un pipeline à l’aide de Python](quickstart-create-data-factory-python.md).

Pour surveiller l’exécution du pipeline, ajoutez le code suivant :

```python
# Monitor the pipeline run
time.sleep(30)
pipeline_run = adf_client.pipeline_runs.get(
    rg_name, df_name, run_response.run_id)
print("\n\tPipeline run status: {}".format(pipeline_run.status))
activity_runs_paged = list(adf_client.activity_runs.list_by_pipeline_run(
    rg_name, df_name, pipeline_run.run_id, datetime.now() - timedelta(1),  datetime.now() + timedelta(1)))
print_activity_run_details(activity_runs_paged[0])
```

Pour une documentation complète sur le SDK Python, consultez la [référence au SDK Python de Data Factory](/python/api/overview/azure/datafactory).

## <a name="rest-api"></a>API REST
Pour obtenir une description complète de la création et de la surveillance d’un pipeline à l’aide d’une API REST, consultez [Créer une fabrique de données et un pipeline à l’aide de l’API REST](quickstart-create-data-factory-rest-api.md).
 
1. Exécutez le script suivant afin de vérifier en permanence l’état de l’exécution du pipeline jusqu’à la fin de la copie des données.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}?api-version=${apiVersion}"
    while ($True) {
        $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
        Write-Host  "Pipeline run status: " $response.Status -foregroundcolor "Yellow"

        if ( ($response.Status -eq "InProgress") -or ($response.Status -eq "Queued") ) {
            Start-Sleep -Seconds 15
        }
        else {
            $response | ConvertTo-Json
            break
        }
    }
    ```
2. Exécutez le script suivant pour récupérer les détails de l’exécution de l’activité de copie, par exemple la taille des données lues/écrites.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}/activityruns?api-version=${apiVersion}&startTime="+(Get-Date).ToString('yyyy-MM-dd')+"&endTime="+(Get-Date).AddDays(1).ToString('yyyy-MM-dd')+"&pipelineName=Adfv2QuickStartPipeline"
    $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
    $response | ConvertTo-Json
    ```

Pour obtenir une documentation complète sur les API REST, consultez la [référence à l’API REST Data Factory](/rest/api/datafactory/).

## <a name="powershell"></a>PowerShell
Pour obtenir une description complète de la création et de la surveillance d’un pipeline à l’aide de PowerShell, consultez [Créer une fabrique de données et un pipeline à l’aide de PowerShell](quickstart-create-data-factory-powershell.md).

1. Exécutez le script suivant afin de vérifier en permanence l’état de l’exécution du pipeline jusqu’à la fin de la copie des données.

    ```powershell
    while ($True) {
        $run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ( ($run.Status -ne "InProgress") -and ($run.Status -ne "Queued") ) {
                Write-Output ("Pipeline run finished. The status is: " +  $run.Status)
                $run
                break
            }
            Write-Output ("Pipeline is running...status: " + $run.Status)
        }

        Start-Sleep -Seconds 30
    }
    ```
2. Exécutez le script suivant pour récupérer les détails de l’exécution de l’activité de copie, par exemple la taille des données lues/écrites.

    ```powershell
    Write-Host "Activity run details:" -foregroundcolor "Yellow"
    $result = Get-AzDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
    ```

Pour obtenir une documentation complète sur les applets de commande PowerShell, consultez la [référence aux applets de commande PowerShell de Data Factory](/powershell/module/az.datafactory).

## <a name="next-steps"></a>Étapes suivantes
Consultez l’article [Surveiller les pipelines à l’aide d’Azure Monitor](monitor-using-azure-monitor.md) pour découvrir comment utiliser Azure Monitor dans le cadre de la surveillance des pipelines Data Factory.