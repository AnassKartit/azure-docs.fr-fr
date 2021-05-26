---
title: État d’orchestration personnalisé dans Fonctions durables - Azure
description: Découvrez comment configurer et utiliser l’état d’orchestration personnalisé pour Fonctions durables.
ms.topic: conceptual
ms.date: 05/10/2021
ms.author: azfuncdf
ms.openlocfilehash: de74c2d8c4e7abf5735dad0b1c04f2cce88aa2c1
ms.sourcegitcommit: 58e5d3f4a6cb44607e946f6b931345b6fe237e0e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2021
ms.locfileid: "110370923"
---
# <a name="custom-orchestration-status-in-durable-functions-azure-functions"></a>État d’orchestration personnalisé dans Fonctions durables (Azure Functions)

L’état d’orchestration personnalisé vous permet de définir une valeur d’état personnalisée pour votre fonction d’orchestrateur. Cet état est fourni via l’[API HTTP GetStatus ou l’API](durable-functions-http-api.md#get-instance-status) ou l’[API de SDK](durable-functions-instance-management.md#query-instances) équivalente sur l’objet client d’orchestration.

## <a name="sample-use-cases"></a>Exemples de cas d’utilisation

### <a name="visualize-progress"></a>Visualiser la progression

Les clients peuvent interroger le point de terminaison de l’état et afficher une interface utilisateur où l’étape d’exécution actuelle est visualisée. L’exemple suivant illustre le partage de la progression :

# <a name="c"></a>[C#](#tab/csharp)

> [!NOTE]
> Ces exemples C# sont écrits pour Durable Functions 2.x et ne sont pas compatibles avec Durable Functions 1.x. Pour en savoir plus sur les différences entre les versions, consultez l’article [Versions de Durable Functions](durable-functions-versions.md).

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var outputs = new List<string>();

    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
    context.SetCustomStatus("Tokyo");
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
    context.SetCustomStatus("Seattle");
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));
    context.SetCustomStatus("London");

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return outputs;
}

[FunctionName("E1_SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
    return $"Hello {name}!";
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Fonction d’orchestrateur `E1_HelloSequence` :

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    const outputs = [];

    outputs.push(yield context.df.callActivity("E1_SayHello", "Tokyo"));
    context.df.setCustomStatus("Tokyo");
    outputs.push(yield context.df.callActivity("E1_SayHello", "Seattle"));
    context.df.setCustomStatus("Seattle");
    outputs.push(yield context.df.callActivity("E1_SayHello", "London"));
    context.df.setCustomStatus("London");

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return outputs;
});
```

Fonction d'activité `E1_SayHello` :

```javascript
module.exports = async function(context, name) {
    return `Hello ${name}!`;
};
```
# <a name="python"></a>[Python](#tab/python)

### <a name="e1_hellosequence-orchestrator-function"></a>Fonction d’orchestrateur `E1_HelloSequence`
```python
import azure.functions as func
import azure.durable_functions as df


def orchestrator_function(context: df.DurableOrchestrationContext):
    
    output1 = yield context.call_activity('E1_SayHello', 'Tokyo')
    context.set_custom_status('Tokyo')
    output2 = yield context.call_activity('E1_SayHello', 'Seattle')
    context.set_custom_status('Seattle')
    output3 = yield context.call_activity('E1_SayHello', 'London')
    context.set_custom_status('London')
    
    return [output1, output2, output3]

main = df.Orchestrator.create(orchestrator_function)
```

### <a name="e1_sayhello-activity-function"></a>Fonction d’activité `E1_SayHello`
```python
def main(name: str) -> str:
    return f"Hello {name}!"

```
# <a name="powershell"></a>[PowerShell](#tab/powershell)

### <a name="e1_hellosequence-orchestrator-function"></a>Fonction d’orchestrateur `E1_HelloSequence`
```powershell
param($Context)

$output = @()

$output += Invoke-DurableActivity -FunctionName 'E1_SayHello' -Input 'Tokyo'
Set-DurableCustomStatus -CustomStatus 'Tokyo'

$output += Invoke-DurableActivity -FunctionName 'E1_SayHello' -Input 'Seattle'
Set-DurableCustomStatus -CustomStatus 'Seattle'

$output += Invoke-DurableActivity -FunctionName 'E1_SayHello' -Input 'London'
Set-DurableCustomStatus -CustomStatus 'London'


return $output
```

### <a name="e1_sayhello-activity-function"></a>Fonction d’activité `E1_SayHello`
```powershell
param($name)

"Hello $name"
```
---

Le client reçoit ensuite la sortie de l’orchestration uniquement quand le champ `CustomStatus` est défini sur « London » :

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
    [DurableClient] IDurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, (string)eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    DurableOrchestrationStatus durableOrchestrationStatus = await starter.GetStatusAsync(instanceId);
    while (durableOrchestrationStatus.CustomStatus.ToString() != "London")
    {
        await Task.Delay(200);
        durableOrchestrationStatus = await starter.GetStatusAsync(instanceId);
    }

    HttpResponseMessage httpResponseMessage = new HttpResponseMessage(HttpStatusCode.OK)
    {
        Content = new StringContent(JsonConvert.SerializeObject(durableOrchestrationStatus))
    };

    return httpResponseMessage;
  }
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    // Function input comes from the request content.
    const eventData = req.body;
    const instanceId = await client.startNew(req.params.functionName, undefined, eventData);

    context.log(`Started orchestration with ID = '${instanceId}'.`);

    let durableOrchestrationStatus = await client.getStatus(instanceId);
    while (durableOrchestrationStatus.customStatus.toString() !== "London") {
        await new Promise((resolve) => setTimeout(resolve, 200));
        durableOrchestrationStatus = await client.getStatus(instanceId);
    }

    const httpResponseMessage = {
        status: 200,
        body: JSON.stringify(durableOrchestrationStatus),
    };

    return httpResponseMessage;
};
```

> [!NOTE]
> Dans JavaScript, le champ `customStatus` sera défini lorsque la prochaine action `yield` ou `return` est planifiée.

# <a name="python"></a>[Python](#tab/python)
```python
import json
import logging
import azure.functions as func
import azure.durable_functions as df
from time import sleep

async def main(req: func.HttpRequest, starter: str) -> func.HttpResponse:
    client = df.DurableOrchestrationClient(starter)    
    instance_id = await client.start_new(req.params.functionName, None, None)

    logging.info(f"Started orchestration with ID = '{instance_id}'.")

    durable_orchestration_status = await client.get_status(instance_id)
    while durable_orchestration_status.custom_status != 'London':
        sleep(0.2)
        durable_orchestration_status = await client.get_status(instance_id)

    return func.HttpResponse(body='Success', status_code=200, mimetype='application/json')
```

> [!NOTE]
> Dans Python, le champ `custom_status` sera défini lors de la planification de la prochaine action `yield` ou `return`.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

La fonctionnalité n’est pas implémentée actuellement dans PowerShell

---

### <a name="output-customization"></a>Personnalisation de la sortie

Un autre scénario intéressant consiste à segmenter les utilisateurs en renvoyant une sortie personnalisée basée sur des caractéristiques ou des interactions uniques. Grâce à l’état d’orchestration personnalisé, le code côté client reste générique. Toutes les modifications principales se produisent côté serveur, comme l’illustre l’exemple suivant :

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("CityRecommender")]
public static void Run(
  [OrchestrationTrigger] IDurableOrchestrationContext context)
{
  int userChoice = context.GetInput<int>();

  switch (userChoice)
  {
    case 1:
    context.SetCustomStatus(new
    {
      recommendedCities = new[] {"Tokyo", "Seattle"},
      recommendedSeasons = new[] {"Spring", "Summer"}
     });
      break;
    case 2:
      context.SetCustomStatus(new
      {
        recommendedCities = new[] {"Seattle, London"},
        recommendedSeasons = new[] {"Summer"}
      });
        break;
      case 3:
      context.SetCustomStatus(new
      {
        recommendedCities = new[] {"Tokyo, London"},
        recommendedSeasons = new[] {"Spring", "Summer"}
      });
        break;
  }

  // Wait for user selection and refine the recommendation
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

#### <a name="cityrecommender-orchestrator"></a>Orchestrateur `CityRecommender`

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const userChoice = context.df.getInput();

    switch (userChoice) {
        case 1:
            context.df.setCustomStatus({
                recommendedCities: [ "Tokyo", "Seattle" ],
                recommendedSeasons: [ "Spring", "Summer" ],
            });
            break;
        case 2:
            context.df.setCustomStatus({
                recommendedCities: [ "Seattle", "London" ],
                recommendedSeasons: [ "Summer" ],
            });
            break;
        case 3:
            context.df.setCustomStatus({
                recommendedCity: [ "Tokyo", "London" ],
                recommendedSeasons: [ "Spring", "Summer" ],
            });
            break;
    }

    // Wait for user selection and refine the recommendation
});
```

# <a name="python"></a>[Python](#tab/python)

#### <a name="cityrecommender-orchestrator"></a>Orchestrateur `CityRecommender`

```python
import azure.functions as func
import azure.durable_functions as df

def orchestrator_function(context: df.DurableOrchestrationContext):
    userChoice = int(context.get_input())

    if userChoice == 1:
        context.set_custom_status({
            'recommendedCities' : ['Tokyo', 'Seattle'], 
            'recommendedSeasons' : ['Spring', 'Summer']
        }))
    if userChoice == 2:
        context.set_custom_status({
            'recommendedCities' : ['Seattle', 'London']
            'recommendedSeasons' : ['Summer']
        }))
    if userChoice == 3:
        context.set_custom_status({
            'recommendedCities' : ['Tokyo', 'London'], 
            'recommendedSeasons' : ['Spring', 'Summer']
        }))



    # Wait for user selection and refine the recommendation

main = df.Orchestrator.create(orchestrator_function)
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

#### <a name="cityrecommender-orchestrator"></a>Orchestrateur `CityRecommender`

```powershell
param($Context)

$userChoice = $Context.Input -as [int]

if ($userChoice -eq 1) {
    Set-DurableCustomStatus -CustomStatus @{ recommendedCities = @('Tokyo', 'Seattle'); 
                                             recommendedSeasons = @('Spring', 'Summer') 
                                            }  
}

if ($userChoice -eq 2) {
    Set-DurableCustomStatus -CustomStatus @{ recommendedCities = @('Seattle', 'London'); 
                                             recommendedSeasons = @('Summer') 
                                            }  
}

if ($userChoice -eq 3) {
    Set-DurableCustomStatus -CustomStatus @{ recommendedCities = @('Tokyo', 'London'); 
                                             recommendedSeasons = @('Spring', 'Summer') 
                                            }  
}

# Wait for user selection and refine the recommendation
```
---

### <a name="instruction-specification"></a>Spécification d’instructions

L’orchestrateur peut fournir des instructions uniques aux clients via l’état personnalisé. Les instructions de l’état personnalisé sont mappées avec les étapes du code de l’orchestration :

# <a name="c"></a>[C#](#tab/csharp)

```csharp
[FunctionName("ReserveTicket")]
public static async Task<bool> Run(
  [OrchestrationTrigger] IDurableOrchestrationContext context)
{
  string userId = context.GetInput<string>();

  int discount = await context.CallActivityAsync<int>("CalculateDiscount", userId);

  context.SetCustomStatus(new
  {
    discount = discount,
    discountTimeout = 60,
    bookingUrl = "https://www.myawesomebookingweb.com",
  });

  bool isBookingConfirmed = await context.WaitForExternalEvent<bool>("BookingConfirmed");

  context.SetCustomStatus(isBookingConfirmed
    ? new {message = "Thank you for confirming your booking."}
    : new {message = "The booking was not confirmed on time. Please try again."});

  return isBookingConfirmed;
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const userId = context.df.getInput();

    const discount = yield context.df.callActivity("CalculateDiscount", userId);

    context.df.setCustomStatus({
        discount,
        discountTimeout = 60,
        bookingUrl = "https://www.myawesomebookingweb.com",
    });

    const isBookingConfirmed = yield context.df.waitForExternalEvent("bookingConfirmed");

    context.df.setCustomStatus(isBookingConfirmed
        ? { message: "Thank you for confirming your booking." }
        : { message: "The booking was not confirmed on time. Please try again." }
    );

    return isBookingConfirmed;
});
```
# <a name="python"></a>[Python](#tab/python)

```python
import azure.functions as func
import azure.durable_functions as df

def orchestrator_function(context: df.DurableOrchestrationContext):
    userId = int(context.get_input())

    discount = yield context.call_activity('CalculateDiscount', userId)

    status = { 'discount' : discount,
        'discountTimeout' : 60,
        'bookingUrl' : "https://www.myawesomebookingweb.com",
    }
    context.set_custom_status(status)

    is_booking_confirmed = yield context.wait_for_external_event('BookingConfirmed')
    context.set_custom_status({'message': 'Thank you for confirming your booking.'} if is_booking_confirmed 
        else {'message': 'The booking was not confirmed on time. Please try again.'})
    return is_booking_confirmed

main = df.Orchestrator.create(orchestrator_function)
```
# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
param($Context)

$userId = $Context.Input -as [int]

$discount = Invoke-DurableActivity -FunctionName 'CalculateDiscount' -Input $userId

$status = @{
            discount = $discount;
            discountTimeout = 60;
            bookingUrl = "https://www.myawesomebookingweb.com"
            }

Set-DurableCustomStatus -CustomStatus $status

$isBookingConfirmed = Invoke-DurableActivity -FunctionName 'BookingConfirmed'

if ($isBookingConfirmed) {
    Set-DurableCustomStatus -CustomStatus @{message = 'Thank you for confirming your booking.'}
} else {
    Set-DurableCustomStatus -CustomStatus @{message = 'The booking was not confirmed on time. Please try again.'}
}

return $isBookingConfirmed
```
---

## <a name="sample"></a>Exemple

Dans l’exemple suivant, l’état personnalisé est défini en premier.

# <a name="c"></a>[C#](#tab/csharp)

```csharp
public static async Task SetStatusTest([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    var customStatus = new { nextActions = new [] {"A", "B", "C"}, foo = 2, };
    context.SetCustomStatus(customStatus);

    // ...do more work...
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    const customStatus = { nextActions: [ "A", "B", "C" ], foo: 2, };
    context.df.setCustomStatus(customStatus);

    // ...do more work...
});
```

# <a name="python"></a>[Python](#tab/python)

```python
import azure.functions as func
import azure.durable_functions as df

def orchestrator_function(context: df.DurableOrchestrationContext):
    # ...do work...

    custom_status = {'nextActions': ['A','B','C'], 'foo':2}
    context.set_custom_status(custom_status)

    # ...do more work...

main = df.Orchestrator.create(orchestrator_function)
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
param($Context)

# ...do work...

Set-DurableCustomStatus -CustomStatus @{ nextActions = @('A', 'B', 'C'); 
                                         foo = 2 
                                        }  

# ...do more work...
```
---

Pendant l’exécution de l’orchestration, les clients externes peuvent récupérer cet état personnalisé :

```http
GET /runtime/webhooks/durabletask/instances/instance123
```

Les clients obtiennent la réponse suivante :

```json
{
  "runtimeStatus": "Running",
  "input": null,
  "customStatus": { "nextActions": ["A", "B", "C"], "foo": 2 },
  "output": null,
  "createdTime": "2019-10-06T18:30:24Z",
  "lastUpdatedTime": "2019-10-06T19:40:30Z"
}
```

> [!WARNING]
> La charge utile d’état personnalisé est limitée à 16 ko de texte JSON UTF-16. Nous vous recommandons d’utiliser un stockage externe si vous avez besoin d’une charge utile plus importante.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [En savoir plus sur les minuteurs durables](durable-functions-timers.md)
