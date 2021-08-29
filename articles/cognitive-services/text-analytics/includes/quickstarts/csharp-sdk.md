---
title: 'Démarrage rapide : Bibliothèque de client Analyse de texte pour C# | Microsoft Docs'
description: Découvrir comment utiliser la bibliothèque de client Analyse de texte pour C#
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 07/15/2021
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: 817f282f167277c90ecfc19e11dd9312640c8bbf
ms.sourcegitcommit: 1deb51bc3de58afdd9871bc7d2558ee5916a3e89
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/19/2021
ms.locfileid: "122442363"
---
<a name="HOLTop"></a>

# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

[Documentation de référence v3.1](/dotnet/api/azure.ai.textanalytics?preserve-view=true&view=azure-dotnet-preview) | [Code source de la bibliothèque v3.1](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics) | [Package v3.1 (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics/5.1.0) | [Exemples v3.1](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

# <a name="version-30"></a>[Version 3.0](#tab/version-3)

[Documentation de référence v3](/dotnet/api/azure.ai.textanalytics) | [Code source de la bibliothèque v3](https://github.com/Azure/azure-sdk-for-net/tree/Azure.AI.TextAnalytics_5.0.0/sdk/textanalytics/Azure.AI.TextAnalytics) | [Package v3 (NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [Exemples v3](https://github.com/Azure/azure-sdk-for-net/tree/Azure.AI.TextAnalytics_5.0.0/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

---

## <a name="prerequisites"></a>Prérequis

* Abonnement Azure - [En créer un gratuitement](https://azure.microsoft.com/free/cognitive-services)
* [IDE Visual Studio](https://visualstudio.microsoft.com/vs/)
* [!INCLUDE [contributor-requirement](../../../includes/quickstarts/contributor-requirement.md)]
* Une fois que vous avez votre abonnement Azure, <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="créez une ressource Analyse de texte"  target="_blank">Créer une ressource Analyse de texte </a> dans le portail Azure pour obtenir votre clé et votre point de terminaison.  Une fois le déploiement effectué, cliquez sur **Accéder à la ressource**.
    * Vous aurez besoin de la clé et du point de terminaison de la ressource que vous créez pour connecter votre application à l’API Analyse de texte. Vous collerez votre clé et votre point de terminaison dans le code ci-dessous plus loin dans le guide de démarrage rapide.
    * Vous pouvez utiliser le niveau tarifaire Gratuit (`F0`) pour tester le service, puis passer par la suite à un niveau payant pour la production.
* Pour utiliser la fonctionnalité d’analyse, vous aurez besoin d’une ressource Analyse de texte avec le niveau tarifaire Standard (S).

## <a name="setting-up"></a>Configuration

### <a name="create-a-new-net-core-application"></a>Créez une application .NET Core

En utilisant l’IDE Visual Studio, créez une application console .NET Core. Cette action va créer un projet nommé « Hello World » avec un seul fichier source C# : *program.cs*.

# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

Installez la bibliothèque cliente en cliquant avec le bouton droit sur la solution dans l’**Explorateur de solutions** et en sélectionnant **Gérer les packages NuGet**. Dans le gestionnaire de package qui s’ouvre, sélectionnez **Parcourir** et recherchez `Azure.AI.TextAnalytics`. Sélectionnez la version `5.1.0`, puis **Installer**. Vous pouvez aussi utiliser la [Console du Gestionnaire de package](/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package).

# <a name="version-30"></a>[Version 3.0](#tab/version-3)

Installez la bibliothèque cliente en cliquant avec le bouton droit sur la solution dans l’**Explorateur de solutions** et en sélectionnant **Gérer les packages NuGet**. Dans le gestionnaire de package qui s’ouvre, sélectionnez **Parcourir** et recherchez `Azure.AI.TextAnalytics`. Sélectionnez la version `5.0.0`, puis **Installer**. Vous pouvez aussi utiliser la [Console du Gestionnaire de package](/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package).


> [!TIP]
> Vous voulez voir l’intégralité du fichier de code de démarrage rapide à la fois ? Vous le trouverez [sur GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/TextAnalytics/program.cs), qui contient les exemples de code dans ce guide de démarrage rapide. 

---

# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

Ouvrez le fichier *program.cs* et ajoutez les directives `using` suivantes :

```csharp
using Azure;
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

Dans la classe `Program` de l’application, créez des variables pour la clé et le point de terminaison de votre ressource.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Remplacez la méthode `Main` de l’application. Vous définirez les méthodes appelées ici ultérieurement.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    SentimentAnalysisWithOpinionMiningExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityLinkingExample(client);
    RecognizePIIExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

# <a name="version-30"></a>[Version 3.0](#tab/version-3)

Ouvrez le fichier *program.cs* et ajoutez les directives `using` suivantes :

```csharp
using Azure;
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

Dans la classe `Program` de l’application, créez des variables pour la clé et le point de terminaison de votre ressource.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

Remplacez la méthode `Main` de l’application. Vous définirez les méthodes appelées ici ultérieurement.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityLinkingExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

---

## <a name="object-model"></a>Modèle objet

Le client Analyse de texte est un objet `TextAnalyticsClient` qui s’authentifie auprès d’Azure avec votre clé et fournit des fonctions permettant d’accepter du texte sous forme de chaînes individuelles ou de lots. Vous pouvez envoyer du texte à l’API de façon synchrone ou de façon asynchrone. L’objet Response contient les informations d’analyse de chaque document que vous envoyez. 

Si vous utilisez la version `3.x` du service, vous pouvez vous servir d’une instance `TextAnalyticsClientOptions` facultative pour initialiser le client avec différents paramètres par défaut (par exemple, la langue par défaut ou l’indicateur de pays/région). Vous pouvez également vous authentifier à l’aide d’un jeton Azure Active Directory. 

[!INCLUDE [text-analytics-character-limits](../character-limits.md)]

## <a name="code-examples"></a>Exemples de code

* [Analyse des sentiments](#sentiment-analysis)
* [Exploration des opinions](#opinion-mining)
* [Détection de la langue](#language-detection)
* [Reconnaissance d’entité nommée](#named-entity-recognition-ner)
* [Liaison d’entités](#entity-linking)
* [Extraction de phrases clés](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>Authentifier le client

# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

Vérifiez que votre méthode principale précédemment utilisée crée un objet client avec votre point de terminaison et vos informations d’identification.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

# <a name="version-30"></a>[Version 3.0](#tab/version-3)

Vérifiez que votre méthode principale précédemment utilisée crée un objet client avec votre point de terminaison et vos informations d’identification.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

---

## <a name="sentiment-analysis"></a>analyse de sentiments

# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

Créez une fonction appelée `SentimentAnalysisExample()` qui accepte le client que vous avez créé, puis appelez sa fonction `AnalyzeSentiment()`. L’objet `Response<DocumentSentiment>` retourné contient l’étiquette de sentiment et le score de l’intégralité du document d’entrée ainsi qu’une analyse des sentiments pour chaque phrase si l’opération réussit. En cas d’erreur, une exception `RequestFailedException`est levée.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tText: \"{sentence.Text}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>Output

```console
Document sentiment: Positive

        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

## <a name="opinion-mining"></a>Exploration des opinions

Créez une fonction appelée `SentimentAnalysisWithOpinionMiningExample()` qui accepte le client que vous avez créé, puis appelez sa fonction `AnalyzeSentimentBatch()` avec l’option `IncludeOpinionMining` dans le conteneur `AnalyzeSentimentOptions`. L’objet `AnalyzeSentimentResultCollection` retourné contient la collection de `AnalyzeSentimentResult` dans laquelle `Response<DocumentSentiment>` est représenté. La différence entre `SentimentAnalysis()` et `SentimentAnalysisWithOpinionMiningExample()` réside dans le fait que ce dernier contiendra des `SentenceOpinion` dans chaque phrase, ce qui montre une cible analysée et les évaluations associées. En cas d’erreur, une exception `RequestFailedException` est levée.

```csharp
static void SentimentAnalysisWithOpinionMiningExample(TextAnalyticsClient client)
{
    var documents = new List<string>
    {
        "The food and service were unacceptable, but the concierge were nice."
    };

    AnalyzeSentimentResultCollection reviews = client.AnalyzeSentimentBatch(documents, options: new AnalyzeSentimentOptions()
    {
        IncludeOpinionMining = true
    });

    foreach (AnalyzeSentimentResult review in reviews)
    {
        Console.WriteLine($"Document sentiment: {review.DocumentSentiment.Sentiment}\n");
        Console.WriteLine($"\tPositive score: {review.DocumentSentiment.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {review.DocumentSentiment.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {review.DocumentSentiment.ConfidenceScores.Neutral:0.00}\n");
        foreach (SentenceSentiment sentence in review.DocumentSentiment.Sentences)
        {
            Console.WriteLine($"\tText: \"{sentence.Text}\"");
            Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
            Console.WriteLine($"\tSentence positive score: {sentence.ConfidenceScores.Positive:0.00}");
            Console.WriteLine($"\tSentence negative score: {sentence.ConfidenceScores.Negative:0.00}");
            Console.WriteLine($"\tSentence neutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");

            foreach (SentenceOpinion sentenceOpinion in sentence.Opinions)
            {
                Console.WriteLine($"\tTarget: {sentenceOpinion.Target.Text}, Value: {sentenceOpinion.Target.Sentiment}");
                Console.WriteLine($"\tTarget positive score: {sentenceOpinion.Target.ConfidenceScores.Positive:0.00}");
                Console.WriteLine($"\tTarget negative score: {sentenceOpinion.Target.ConfidenceScores.Negative:0.00}");
                foreach (AssessmentSentiment assessment in sentenceOpinion.Assessments)
                {
                    Console.WriteLine($"\t\tRelated Assessment: {assessment.Text}, Value: {assessment.Sentiment}");
                    Console.WriteLine($"\t\tRelated Assessment positive score: {assessment.ConfidenceScores.Positive:0.00}");
                    Console.WriteLine($"\t\tRelated Assessment negative score: {assessment.ConfidenceScores.Negative:0.00}");
                }
            }
        }
        Console.WriteLine($"\n");
    }
}
```

### <a name="output"></a>Output

```console
Document sentiment: Positive

        Positive score: 0.84
        Negative score: 0.16
        Neutral score: 0.00

        Text: "The food and service were unacceptable, but the concierge were nice."
        Sentence sentiment: Positive
        Sentence positive score: 0.84
        Sentence negative score: 0.16
        Sentence neutral score: 0.00

        Target: food, Value: Negative
        Target positive score: 0.01
        Target negative score: 0.99
                Related Assessment: unacceptable, Value: Negative
                Related Assessment positive score: 0.01
                Related Assessment negative score: 0.99
        Target: service, Value: Negative
        Target positive score: 0.01
        Target negative score: 0.99
                Related Assessment: unacceptable, Value: Negative
                Related Assessment positive score: 0.01
                Related Assessment negative score: 0.99
        Target: concierge, Value: Positive
        Target positive score: 1.00
        Target negative score: 0.00
                Related Assessment: nice, Value: Positive
                Related Assessment positive score: 1.00
                Related Assessment negative score: 0.00

Press any key to exit.
```

# <a name="version-30"></a>[Version 3.0](#tab/version-3)

Créez une fonction appelée `SentimentAnalysisExample()` qui accepte le client que vous avez créé, puis appelez sa fonction `AnalyzeSentiment()`. L’objet `Response<DocumentSentiment>` retourné contient l’étiquette de sentiment et le score de l’intégralité du document d’entrée ainsi qu’une analyse des sentiments pour chaque phrase si l’opération réussit. En cas d’erreur, une exception `RequestFailedException`est levée.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tText: \"{sentence.Text}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>Output

```console
Document sentiment: Positive

        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

---

## <a name="language-detection"></a>Détection de la langue

# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)


Créez une fonction appelée `LanguageDetectionExample()` qui accepte le client que vous avez créé, puis appelez sa fonction `DetectLanguage()`. L’objet `Response<DetectedLanguage>` retourné contient la langue détectée ainsi que son nom et le code ISO-6391. En cas d’erreur, une exception `RequestFailedException` est levée.

> [!Tip]
> Dans certains cas, il peut être difficile de lever toute ambiguïté sur les langues en fonction de l’entrée. Vous pouvez utiliser le paramètre `countryHint` pour spécifier un code de pays/région à deux lettres. L’API utilise « US » comme countryHint par défaut. Pour modifier ce comportement, vous pouvez réinitialiser ce paramètre en définissant cette valeur sur une chaîne vide `countryHint = ""`. Pour définir une autre valeur par défaut, définissez la propriété `TextAnalyticsClientOptions.DefaultCountryHint` et transmettez-la pendant l’initialisation du client.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>Output

```console
Language:
        French, ISO-6391: fr
```

# <a name="version-30"></a>[Version 3.0](#tab/version-3)


Créez une fonction appelée `LanguageDetectionExample()` qui accepte le client que vous avez créé, puis appelez sa fonction `DetectLanguage()`. L’objet `Response<DetectedLanguage>` retourné contient la langue détectée ainsi que son nom et le code ISO-6391. En cas d’erreur, une exception `RequestFailedException` est levée.

> [!Tip]
> Dans certains cas, il peut être difficile de lever toute ambiguïté sur les langues en fonction de l’entrée. Vous pouvez utiliser le paramètre `countryHint` pour spécifier un code de pays/région à deux lettres. L’API utilise « US » comme countryHint par défaut. Pour modifier ce comportement, vous pouvez réinitialiser ce paramètre en définissant cette valeur sur une chaîne vide `countryHint = ""`. Pour définir une autre valeur par défaut, définissez la propriété `TextAnalyticsClientOptions.DefaultCountryHint` et transmettez-la pendant l’initialisation du client.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>Output

```console
Language:
        French, ISO-6391: fr
```


---

## <a name="named-entity-recognition-ner"></a>Reconnaissance d’entité nommée (NER)

# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)


Créez une fonction appelée `EntityRecognitionExample()` qui accepte le client que vous avez créé, appelez sa fonction `RecognizeEntities()`, puis effectuez une itération dans les résultats. L’objet `Response<CategorizedEntityCollection>` retourné contient la collection des entités détectées `CategorizedEntity`. En cas d’erreur, une exception `RequestFailedException` est levée.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tScore: {entity.ConfidenceScore:F2},\tLength: {entity.Length},\tOffset: {entity.Offset}\n");
    }
}
```

### <a name="output"></a>Output

```console
Named Entities:
        Text: trip,     Category: Event,        Sub-Category:
                Score: 0.61,    Length: 4,      Offset: 18

        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Score: 0.82,    Length: 7,      Offset: 26

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Score: 0.80,    Length: 9,      Offset: 34
```

## <a name="personally-identifiable-information-pii-recognition"></a>Reconnaissance des informations d’identification personnelle

Créez une fonction appelée `RecognizePIIExample()` qui accepte le client que vous avez créé, appelez sa fonction `RecognizePiiEntities()`, puis effectuez une itération dans les résultats. L’objet `PiiEntityCollection` retourné représente la liste des entités d’informations d’identification personnelle détectées. En cas d’erreur, une exception `RequestFailedException` est levée.

```csharp
static void RecognizePIIExample(TextAnalyticsClient client)
{
    string document = "A developer with SSN 859-98-0987 whose phone number is 800-102-1100 is building tools with our APIs.";

    PiiEntityCollection entities = client.RecognizePiiEntities(document).Value;

    Console.WriteLine($"Redacted Text: {entities.RedactedText}");
    if (entities.Count > 0)
    {
        Console.WriteLine($"Recognized {entities.Count} PII entit{(entities.Count > 1 ? "ies" : "y")}:");
        foreach (PiiEntity entity in entities)
        {
            Console.WriteLine($"Text: {entity.Text}, Category: {entity.Category}, SubCategory: {entity.SubCategory}, Confidence score: {entity.ConfidenceScore}");
        }
    }
    else
    {
        Console.WriteLine("No entities were found.");
    }
}
```

### <a name="output"></a>Output

```console
Redacted Text: A developer with SSN *********** whose phone number is ************ is building tools with our APIs.
Recognized 2 PII entities:
Text: 859-98-0987, Category: U.S. Social Security Number (SSN), SubCategory: , Confidence score: 0.65
Text: 800-102-1100, Category: Phone Number, SubCategory: , Confidence score: 0.8
```

# <a name="version-30"></a>[Version 3.0](#tab/version-3)

Créez une fonction appelée `EntityRecognitionExample()` qui accepte le client que vous avez créé, appelez sa fonction `RecognizeEntities()`, puis effectuez une itération dans les résultats. L’objet `Response<IReadOnlyCollection<CategorizedEntity>>` retourné contient la liste des entités détectées. En cas d’erreur, une exception `RequestFailedException` est levée.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tScore: {entity.ConfidenceScore:F2}\n");
    }
}
```

### <a name="output"></a>Output

```console
Named Entities:
        Text: trip,     Category: Event,        Sub-Category:
                Score: 0.61

        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Score: 0.82

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Score: 0.80
```

--- 

## <a name="entity-linking"></a>Liaison d’entités

# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

Créez une fonction appelée `EntityLinkingExample()` qui accepte le client que vous avez créé, appelez sa fonction `RecognizeLinkedEntities()`, puis effectuez une itération dans les résultats. L’objet `Response<LinkedEntityCollection>` retourné contient la collection des entités détectées `LinkedEntity`. En cas d’erreur, une exception `RequestFailedException` est levée. Les entités liées étant identifiées de manière unique, les occurrences d’une même entité sont regroupées sous un objet `LinkedEntity` sous la forme d’une liste d’objets `LinkedEntityMatch`.

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.DataSourceEntityId},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tScore: {match.ConfidenceScore:F2}");
            Console.WriteLine($"\t\tLength: {match.Length}");
            Console.WriteLine($"\t\tOffset: {match.Offset}\n");
        }
    }
}
```

### <a name="output"></a>Output

```console
Linked Entities:
        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Score: 0.55
                Length: 9
                Offset: 0

                Text: Microsoft
                Score: 0.55
                Length: 9
                Offset: 150

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Score: 0.63
                Length: 10
                Offset: 25

                Text: Gates
                Score: 0.63
                Length: 5
                Offset: 161

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Score: 0.60
                Length: 10
                Offset: 40

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Score: 0.32
                Length: 7
                Offset: 54

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Score: 0.33
                Length: 5
                Offset: 89

        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Score: 0.88
                Length: 11
                Offset: 116
```

# <a name="version-30"></a>[Version 3.0](#tab/version-3)

Créez une fonction appelée `EntityLinkingExample()` qui accepte le client que vous avez créé, appelez sa fonction `RecognizeLinkedEntities()`, puis effectuez une itération dans les résultats. L’objet `Response<IReadOnlyCollection<LinkedEntity>>` retourné représente la liste des entités détectées. En cas d’erreur, une exception `RequestFailedException` est levée. Les entités liées étant identifiées de manière unique, les occurrences d’une même entité sont regroupées sous un objet `LinkedEntity` sous la forme d’une liste d’objets `LinkedEntityMatch`.

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.DataSourceEntityId},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tScore: {match.ConfidenceScore:F2}\n");
        }
    }
}
```

### <a name="output"></a>Output

```console
Linked Entities:
        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Score: 0.88

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Score: 0.63

                Text: Gates
                Score: 0.63

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Score: 0.60

        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Score: 0.55

                Text: Microsoft
                Score: 0.55

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Score: 0.32

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Score: 0.33
```

---

## <a name="key-phrase-extraction"></a>Extraction d’expressions clés

# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

Créez une fonction appelée `KeyPhraseExtractionExample()` qui accepte le client que vous avez créé, puis appelez sa fonction `ExtractKeyPhrases()`. L’objet `<Response<KeyPhraseCollection>` retourné contient la liste des phrases clés détectées. En cas d’erreur, une exception `RequestFailedException` est levée.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```

# <a name="version-30"></a>[Version 3.0](#tab/version-3)

Créez une fonction appelée `KeyPhraseExtractionExample()` qui accepte le client que vous avez créé, puis appelez sa fonction `ExtractKeyPhrases()`. L’objet `<Response<IReadOnlyCollection<string>>` retourné contient la liste des phrases clés détectées. En cas d’erreur, une exception `RequestFailedException` est levée.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```

---


## <a name="extract-health-entities"></a>Extraire les entités de santé

[!INCLUDE [health operation pricing](../health-operation-pricing-caution.md)]

Vous pouvez utiliser Analyse de texte pour effectuer une demande asynchrone afin d’extraire les entités de santé du texte. Voici un exemple de base. et [sur GitHub](https://github.com/Azure/azure-sdk-for-net/blob/main/sdk/textanalytics/Azure.AI.TextAnalytics/samples/Sample7_AnalyzeHealthcareEntities.md) un exemple plus avancé.

# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

```csharp
static async Task healthExample(TextAnalyticsClient client)
{
    string document = "Prescribed 100mg ibuprofen, taken twice daily.";

    List<string> batchInput = new List<string>()
    {
        document
    };
    AnalyzeHealthcareEntitiesOperation healthOperation = await client.StartAnalyzeHealthcareEntitiesAsync(batchInput);
    await healthOperation.WaitForCompletionAsync();

    await foreach (AnalyzeHealthcareEntitiesResultCollection documentsInPage in healthOperation.Value)
    {
        Console.WriteLine($"Results of Azure Text Analytics \"Healthcare Async\" Model, version: \"{documentsInPage.ModelVersion}\"");
        Console.WriteLine("");

        foreach (AnalyzeHealthcareEntitiesResult entitiesInDoc in documentsInPage)
        {
            if (!entitiesInDoc.HasError)
            {
                foreach (var entity in entitiesInDoc.Entities)
                {
                    // view recognized healthcare entities
                    Console.WriteLine($"  Entity: {entity.Text}");
                    Console.WriteLine($"  Category: {entity.Category}");
                    Console.WriteLine($"  Offset: {entity.Offset}");
                    Console.WriteLine($"  Length: {entity.Length}");
                    Console.WriteLine($"  NormalizedText: {entity.NormalizedText}");
                }
                Console.WriteLine($"  Found {entitiesInDoc.EntityRelations.Count} relations in the current document:");
                Console.WriteLine("");

                // view recognized healthcare relations
                foreach (HealthcareEntityRelation relations in entitiesInDoc.EntityRelations)
                {
                    Console.WriteLine($"    Relation: {relations.RelationType}");
                    Console.WriteLine($"    For this relation there are {relations.Roles.Count} roles");

                    // view relation roles
                    foreach (HealthcareEntityRelationRole role in relations.Roles)
                    {
                        Console.WriteLine($"      Role Name: {role.Name}");

                        Console.WriteLine($"      Associated Entity Text: {role.Entity.Text}");
                        Console.WriteLine($"      Associated Entity Category: {role.Entity.Category}");
                        Console.WriteLine("");
                    }
                    Console.WriteLine("");
                }
            }
            else
            {
                Console.WriteLine("  Error!");
                Console.WriteLine($"  Document error code: {entitiesInDoc.Error.ErrorCode}.");
                Console.WriteLine($"  Message: {entitiesInDoc.Error.Message}");
            }
            Console.WriteLine("");
        }
    }
}
```

```console
Results of Azure Text Analytics "Healthcare Async" Model, version: "2021-05-15"

  Entity: 100mg
  Category: Dosage
  Offset: 11
  Length: 5
  NormalizedText:
  Entity: ibuprofen
  Category: MedicationName
  Offset: 17
  Length: 9
  NormalizedText: ibuprofen
  Entity: twice daily
  Category: Frequency
  Offset: 34
  Length: 11
  NormalizedText:
  Found 2 relations in the current document:

    Relation: DosageOfMedication
    For this relation there are 2 roles
      Role Name: Dosage
      Associated Entity Text: 100mg
      Associated Entity Category: Dosage

      Role Name: Medication
      Associated Entity Text: ibuprofen
      Associated Entity Category: MedicationName


    Relation: FrequencyOfMedication
    For this relation there are 2 roles
      Role Name: Medication
      Associated Entity Text: ibuprofen
      Associated Entity Category: MedicationName

      Role Name: Frequency
      Associated Entity Text: twice daily
      Associated Entity Category: Frequency
```

# <a name="version-30"></a>[Version 3.0](#tab/version-3)

Cette fonctionnalité n’est pas disponible dans la version 3.0.

---

## <a name="use-the-api-asynchronously-with-the-analyze-operation"></a>Utiliser l’API de manière asynchrone avec l’opération d’analyse

# <a name="version-31"></a>[Version 3.1](#tab/version-3-1)

L’opération d’analyse peut servir à effectuer différentes demandes de lot asynchrones : la reconnaissance d’entité nommée, l’extraction de phrases clés, l’analyse des sentiments et la détection d’informations d’identification personnelle. Vous trouverez ci-dessous un exemple de base sur une opération, et [sur GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples/Sample_AnalyzeActions.md) un exemple plus avancé.

[!INCLUDE [Analyze operation pricing](../analyze-operation-pricing-caution.md)]

Ajoutez les déclarations d'utilisation suivantes à votre fichier C#.

```csharp
using System.Threading.Tasks;
using System.Collections.Generic;
using System.Linq;
```

Créez une fonction appelée `AnalyzeOperationExample()` qui accepte le client que vous avez créé, puis appelez sa fonction `StartAnalyzeBatchActionsAsync()`. L'opération retournée contiendra un objet `AnalyzeBatchActionsResult`. Comme il s’agit d’une opération durable, attendez (`await`) que la valeur soit mise à jour par `operation.WaitForCompletionAsync()`. Une fois `WaitForCompletionAsync()` terminé, le regroupement doit avoir été mis à jour dans `operation.Value`. En cas d’erreur, une exception `RequestFailedException` est levée.


```csharp
static async Task AnalyzeOperationExample(TextAnalyticsClient client)
    {
        string inputText = "Microsoft was founded by Bill Gates and Paul Allen.";

        var batchDocuments = new List<string> { inputText };


        TextAnalyticsActions actions = new TextAnalyticsActions()
        {
            RecognizeEntitiesActions = new List<RecognizeEntitiesAction>() { new RecognizeEntitiesAction() },
            ExtractKeyPhrasesActions = new List<ExtractKeyPhrasesAction>() { new ExtractKeyPhrasesAction() },
            DisplayName = "Analyze Operation Quick Start Example"
        };

        AnalyzeActionsOperation operation = await client.StartAnalyzeActionsAsync(batchDocuments, actions);

        await operation.WaitForCompletionAsync();

        Console.WriteLine($"Status: {operation.Status}");
        Console.WriteLine($"Created On: {operation.CreatedOn}");
        Console.WriteLine($"Expires On: {operation.ExpiresOn}");
        Console.WriteLine($"Last modified: {operation.LastModified}");
        if (!string.IsNullOrEmpty(operation.DisplayName))
            Console.WriteLine($"Display name: {operation.DisplayName}");
        //Console.WriteLine($"Total actions: {operation.TotalActions}");
        Console.WriteLine($"  Succeeded actions: {operation.ActionsSucceeded}");
        Console.WriteLine($"  Failed actions: {operation.ActionsFailed}");
        Console.WriteLine($"  In progress actions: {operation.ActionsInProgress}");

        await foreach (AnalyzeActionsResult documentsInPage in operation.Value)
        {
            RecognizeEntitiesResultCollection entitiesResult = documentsInPage.RecognizeEntitiesResults.FirstOrDefault().DocumentsResults;
            ExtractKeyPhrasesResultCollection keyPhrasesResults = documentsInPage.ExtractKeyPhrasesResults.FirstOrDefault().DocumentsResults;

            Console.WriteLine("Recognized Entities");

            foreach (RecognizeEntitiesResult result in entitiesResult)
            {
                Console.WriteLine($"  Recognized the following {result.Entities.Count} entities:");

                foreach (CategorizedEntity entity in result.Entities)
                {
                    Console.WriteLine($"  Entity: {entity.Text}");
                    Console.WriteLine($"  Category: {entity.Category}");
                    Console.WriteLine($"  Offset: {entity.Offset}");
                    Console.WriteLine($"  Length: {entity.Length}");
                    Console.WriteLine($"  ConfidenceScore: {entity.ConfidenceScore}");
                    Console.WriteLine($"  SubCategory: {entity.SubCategory}");
                }
                Console.WriteLine("");
            }

            Console.WriteLine("Key Phrases");
            
            foreach (ExtractKeyPhrasesResult documentResults in keyPhrasesResults)
            {
                Console.WriteLine($"  Recognized the following {documentResults.KeyPhrases.Count} Keyphrases:");

                foreach (string keyphrase in documentResults.KeyPhrases)
                {
                    Console.WriteLine($"  {keyphrase}");
                }
                Console.WriteLine("");
            }
            
        }
    }
```

Après avoir ajouté cet exemple à votre application, appelez-le dans votre méthode `main()` avec `await`. Comme l'opération Analyser est asynchrone, vous devrez mettre à jour votre méthode `Main()` en fonction du type `async Task`.

```csharp
static async Task Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    await AnalyzeOperationExample(client).ConfigureAwait(false);
}
```

### <a name="output"></a>Output

```console
Status: succeeded
Created On: 3/10/2021 2:25:01 AM +00:00
Expires On: 3/11/2021 2:25:01 AM +00:00
Last modified: 3/10/2021 2:25:05 AM +00:00
Display name: Analyze Operation Quick Start Example
Total actions: 1
  Succeeded actions: 1
  Failed actions: 0
  In progress actions: 0
Recognized Entities
    Recognized the following 3 entities:
    Entity: Microsoft
    Category: Organization
    Offset: 0
    ConfidenceScore: 0.83
    SubCategory: 
    Entity: Bill Gates
    Category: Person
    Offset: 25
    ConfidenceScore: 0.85
    SubCategory: 
    Entity: Paul Allen
    Category: Person
    Offset: 40
    ConfidenceScore: 0.9
    SubCategory: 
```

# <a name="version-30"></a>[Version 3.0](#tab/version-3)

Cette fonctionnalité n’est pas disponible dans la version 3.0.

---
