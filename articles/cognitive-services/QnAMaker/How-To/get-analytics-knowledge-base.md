---
title: Analytique sur la base de connaissances – QnA Maker
titleSuffix: Azure Cognitive Services
description: QnA Maker stocke tous les journaux d’activité de conversation et d’autres données de télémétrie si vous avez activé App Insights pendant la création de votre service QnA Maker. Exécutez les exemples de requêtes pour obtenir vos journaux d’activité de conversation à partir d’App Insights.
services: cognitive-services
manager: nitinme
displayName: chat history, history, chat logs, logs
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/25/2021
ms.custom: ignite-fall-2021
ms.openlocfilehash: d9b596ad4766848e460d534be42930f39c5b6043
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131069180"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Obtenir des analyses sur votre base de connaissances

QnA Maker stocke tous les journaux d’activité de conversation et d’autres données de télémétrie si vous avez activé App Insights pendant la [création de votre service QnA Maker](./set-up-qnamaker-service-azure.md). Exécutez les exemples de requêtes pour obtenir vos journaux de conversation d’Application Insights.

[!INCLUDE [Custom question answering](../includes/new-version.md)]

1. Accédez à votre ressource Application Insights.

    ![Sélectionnez votre ressource Application Insights](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Sélectionnez **Log Analytics**. Une nouvelle fenêtre dans laquelle vous pouvez interroger les données de télémétrie QnA Maker s’ouvre.

3. Collez la requête suivante et exécutez-la.

    ```kusto
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, url, resultCode, duration, performanceBucket
    | parse kind = regex url with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId
    ) on id
    | extend question = tostring(customDimensions['Question'])
    | extend answer = tostring(customDimensions['Answer'])
    | extend score = tostring(customDimensions['Score'])
    | project timestamp, resultCode, duration, id, question, answer, score, performanceBucket,KbId
    ```

    Sélectionnez **Exécuter** pour exécuter la requête.

    [![Exécuter la requête pour déterminer les questions, les réponses et le score des utilisateurs](../media/qnamaker-how-to-analytics-kb/run-query.png)](../media/qnamaker-how-to-analytics-kb/run-query.png#lightbox)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Exécuter des requêtes pour d’autres analyses sur votre base de connaissances QnA Maker

### <a name="total-90-day-traffic"></a>Trafic total pendant 90 jours

```kusto
//Total Traffic
requests
| where url endswith "generateAnswer" and name startswith "POST"
| parse kind = regex url with *"(?i)knowledgebases/"KbId"/generateAnswer"
| summarize ChatCount=count() by bin(timestamp, 1d), KbId
```

### <a name="total-question-traffic-in-a-given-time-period"></a>Question sur le trafic total dans un laps de temps donné

```kusto
//Total Question Traffic in a given time period
let startDate = todatetime('2019-01-01');
let endDate = todatetime('2020-12-31');
requests
| where timestamp <= endDate and timestamp >=startDate
| where url endswith "generateAnswer" and name startswith "POST"
| parse kind = regex url with *"(?i)knowledgebases/"KbId"/generateAnswer"
| summarize ChatCount=count() by KbId
```

### <a name="user-traffic"></a>Trafic utilisateur

```kusto
//User Traffic
requests
| where url endswith "generateAnswer"
| project timestamp, id, url, resultCode, duration
| parse kind = regex url with *"(?i)knowledgebases/"KbId"/generateAnswer"
| join kind= inner (
traces | extend id = operation_ParentId
) on id
| extend UserId = tostring(customDimensions['UserId'])
| summarize ChatCount=count() by bin(timestamp, 1d), UserId, KbId
```

### <a name="latency-distribution-of-questions"></a>Distribution de la latence de questions

```kusto
//Latency distribution of questions
requests
| where url endswith "generateAnswer" and name startswith "POST"
| parse kind = regex url with *"(?i)knowledgebases/"KbId"/generateAnswer"
| project timestamp, id, name, resultCode, performanceBucket, KbId
| summarize count() by performanceBucket, KbId
```

### <a name="unanswered-questions"></a>Questions sans réponse

```kusto
// Unanswered questions
requests
| where url endswith "generateAnswer"
| project timestamp, id, url
| parse kind = regex url with *"(?i)knowledgebases/"KbId"/generateAnswer"
| join kind= inner (
traces | extend id = operation_ParentId
) on id
| extend question = tostring(customDimensions['Question'])
| extend answer = tostring(customDimensions['Answer'])
| extend score = tostring(customDimensions['Score'])
| where  score  == "0" and message == "QnAMaker GenerateAnswer"
| project timestamp, KbId, question, answer, score
| order  by timestamp  desc
```

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Choisir la capacité](./improve-knowledge-base.md)
