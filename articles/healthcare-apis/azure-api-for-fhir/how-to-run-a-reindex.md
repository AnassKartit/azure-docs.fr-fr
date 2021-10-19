---
title: Comment exécuter un travail de réindexation dans l’API Azure pour FHIR
description: Cet article explique comment exécuter un travail de réindexation pour indexer les paramètres de recherche ou de tri qui n’ont pas encore été indexés dans votre base de données.
author: ginalee-dotcom
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 8/23/2021
ms.author: cavoeg
ms.openlocfilehash: 3434a868ac11c90b3ad864f94adf6876e358a8aa
ms.sourcegitcommit: 2da83b54b4adce2f9aeeed9f485bb3dbec6b8023
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2021
ms.locfileid: "122772017"
---
# <a name="running-a-reindex-job-in-azure-api-for-fhir"></a>Exécution d’un travail de réindexation dans Azure API pour FHIR

Dans certains scénarios, vous pouvez avoir des paramètres de recherche ou de tri dans l’API Azure pour FHIR qui n’ont pas encore été indexés. Ce scénario est utile lorsque vous définissez vos propres paramètres de recherche. Tant que le paramètre de recherche n’est pas indexé, il ne peut pas être utilisé dans la recherche. Cet article présente une vue d’ensemble de l’exécution d’un travail de réindexation pour indexer les paramètres de recherche ou de tri qui n’ont pas encore été indexés dans votre base de données.

> [!Warning]
> Il est important de lire cet article dans son intégralité avant de commencer. Un travail de réindexement peut être très gourmand en performances. Cet article contient des options de limitation et de contrôle du travail de réindexation.

## <a name="how-to-run-a-reindex-job"></a>Guide pratique pour exécuter un travail de réindexation 

Pour démarrer un travail de réindexation, utilisez l’exemple de code suivant :

```json
POST {{FHIR URL}}/$reindex 

{ 

“resourceType”: “Parameters”,  

“parameter”: [] 

}
 ```

Si la demande est réussie, l’état **201 créé** est retourné. Le résultat de ce message ressemble à ce qui suit :

```json
HTTP/1.1 201 Created 
Content-Location: https://{{FHIR URL}}/_operations/reindex/560c7c61-2c70-4c54-b86d-c53a9d29495e 

{
  "resourceType": "Parameters",
  "id": "560c7c61-2c70-4c54-b86d-c53a9d29495e",
  "meta": {
    "versionId": "\"4c0049cd-0000-0100-0000-607dc5a90000\""
  },
  "parameter": [
    {
      "name": "id",
      "valueString": "560c7c61-2c70-4c54-b86d-c53a9d29495e"
    },
    {
      "name": "queuedTime",
      "valueDateTime": "2021-04-19T18:02:17.0118558+00:00"
    },
    {
      "name": "totalResourcesToReindex",
      "valueDecimal": 0.0
    },
    {
      "name": "resourcesSuccessfullyReindexed",
      "valueDecimal": 0.0
    },
    {
      "name": "progress",
      "valueDecimal": 0.0
    },
    {
      "name": "status",
      "valueString": "Queued"
    },
    {
      "name": "maximumConcurrency",
      "valueDecimal": 1.0
    },
    {
      "name": "resources",
      "valueString": ""
    },
    {
      "name": "searchParams",
      "valueString": ""
    }
  ]
}
```

> [!NOTE]
> Pour vérifier l’état de ou pour annuler un travail de réindexation, vous avez besoin de l’ID de réindexation. Il s’agit de l’ID de la ressource de paramètres résultante. Dans l’exemple ci-dessus, l’ID de la tâche de réindexation est `560c7c61-2c70-4c54-b86d-c53a9d29495e` .

 ## <a name="how-to-check-the-status-of-a-reindex-job"></a>Vérification de l’état d’un travail de réindexation

Une fois que vous avez démarré un travail de réindexation, vous pouvez vérifier l’état de la tâche à l’aide de l’appel suivant :

`GET {{FHIR URL}}/_operations/reindex/{{reindexJobId}`

L’état du résultat de la tâche de réindexage est indiqué ci-dessous :

```json
{

  "resourceType": "Parameters",
  "id": "b65fd841-1c62-47c6-898f-c9016ced8f77",
  "meta": {

    "versionId": "\"1800f05f-0000-0100-0000-607a1a7c0000\""
  },
  "parameter": [

    {

      "name": "id",
      "valueString": "b65fd841-1c62-47c6-898f-c9016ced8f77"
    },
    {

      "name": "startTime",
      "valueDateTime": "2021-04-16T23:11:35.4223217+00:00"
    },
    {

      "name": "queuedTime",
      "valueDateTime": "2021-04-16T23:11:29.0288163+00:00"
    },
    {

      "name": "totalResourcesToReindex",
      "valueDecimal": 262544.0
    },
    {

      "name": "resourcesSuccessfullyReindexed",
      "valueDecimal": 5754.0
    },
    {

      "name": "progress",
      "valueDecimal": 2.0
    },
    {

      "name": "status",
      "valueString": "Running"
    },
    {

      "name": "maximumConcurrency",
      "valueDecimal": 1.0
    },
    {

      "name": "resources",
      "valueString": 
      "{LIST OF IMPACTED RESOURCES}"
    },
    {
```

Les informations suivantes sont affichées dans le résultat du travail de réindexation :

* **totalResourcesToReindex**: indique le nombre total de ressources en cours de réindexation dans le cadre du travail.

* **resourcesSuccessfullyReindexed**: total qui a déjà été correctement réindexé.

* **progression**: pourcentage d’achèvement du travail de réindexion. Est égal à resourcesSuccessfullyReindexed/totalResourcesToReindex x 100.

* **État**: indique si le travail de réindexation est mis en file d’attente, en cours d’exécution, terminé, en échec ou annulé.

* **ressources**: répertorie tous les types de ressources affectés par le travail de réindexation.

## <a name="delete-a-reindex-job"></a>Supprimer un travail de réindexation

Si vous devez annuler un travail de réindexation, utilisez un appel Delete et spécifiez l’ID de travail de réindexation :

`Delete {{FHIR URL}}/_operations/reindex/{{reindexJobId}`

## <a name="performance-considerations"></a>Considérations relatives aux performances

Un travail de réindexement peut être très gourmand en performances. Nous avons implémenté des contrôles de limitation pour vous aider à gérer la façon dont un travail de réindexation s’exécutera sur votre base de données.

> [!NOTE]
> Il n’est pas rare que des jeux de données volumineux soient exécutés pour un travail de réindexation pendant plusieurs jours. Pour une base de données avec 30 millions millions de ressources, nous avons remarqué 4-5 jours à 100 000 unités de base pour réindexer la totalité de la base de données.

Vous trouverez ci-dessous un tableau détaillant les paramètres disponibles, les valeurs par défaut et les plages recommandées. Vous pouvez utiliser ces paramètres pour accélérer le processus (utiliser davantage de calcul) ou ralentir le processus (utilisez moins de calcul). Par exemple, vous pouvez exécuter le travail de réindexation sur un faible temps de trafic et augmenter votre calcul pour qu’il soit terminé plus rapidement. Au lieu de cela, vous pouvez utiliser les paramètres pour garantir une utilisation faible de Compute et l’exécuter pendant des jours en arrière-plan. 

| **Paramètre**                     | **Description**              | **Par défaut**        | **Plage disponible**           |
| --------------------------------- | ---------------------------- | ------------------ | ------------------------------- |
| QueryDelayIntervalInMilliseconds  | Délai entre chaque lot de ressources en cours de lancement pendant le travail de réindexation. Un nombre plus petit accélère le travail, alors qu’un nombre plus élevé le ralentit. | 500 MS (. 5 secondes) | 50-500000 |
| MaximumResourcesPerQuery  | Nombre maximal de ressources incluses dans le lot de ressources à réindexer.  | 100 | 1-5000 |
| MaximumConcurrency  | Nombre de lots effectués à la fois.  | 1 | 1-10 |
| targetDataStoreUsagePercentage | Vous permet de spécifier le pourcentage de votre magasin de données à utiliser pour le travail de réindexation. par exemple, vous pouvez spécifier 50% et vous assurer qu’au plus la tâche de réindexation utilise 50% des unités de demande disponibles sur Cosmos DB.  | Non présent, ce qui signifie que jusqu’à 100% peuvent être utilisés. | 0-100 |

Si vous souhaitez utiliser l’un des paramètres ci-dessus, vous pouvez les transmettre à la ressource de paramètres lorsque vous démarrez le travail de réindexation.

```json
{
  "resourceType": "Parameters",
  "parameter": [
    {
      "name": "maximumConcurrency",
      "valueInteger": "3"
    },
    {
      "name": "targetDataStoreUsagePercentage",
      "valueInteger": "20"
    },
    {
      "name": "queryDelayIntervalInMilliseconds",
      "valueInteger": "1000"
    },
    {
      "name": "maximumNumberOfResourcesPerQuery",
      "valueInteger": "1"
    }
  ]
}
```

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous avez appris à démarrer un travail de réindexation. Pour savoir comment définir de nouveaux paramètres de recherche qui nécessitent le travail de réindexation, consultez. 

>[!div class="nextstepaction"]
>[Définition des paramètres de recherche personnalisés](how-to-do-custom-search.md)
