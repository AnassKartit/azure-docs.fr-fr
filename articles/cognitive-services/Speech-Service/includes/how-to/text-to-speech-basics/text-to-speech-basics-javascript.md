---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 02/10/2021
ms.author: trbye
ms.custom: devx-track-js
ms.openlocfilehash: 3fa47935721ccfccdfe18d60a66d5cc480582e7d
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102428233"
---
Dans ce guide de démarrage rapide, vous découvrez les modèles de conception courants qui permettent d’utiliser la synthèse vocale au moyen du kit SDK Speech. Vous commencez par créer une configuration et une synthèse de base, puis passez à des exemples plus poussés en matière de développement d’applications personnalisées, notamment :

* Obtention de réponses en tant que flux en mémoire
* Personnalisation du taux d’échantillonnage de sortie et de la vitesse de transmission
* Envoi de demandes de synthèse à l’aide de SSML (Speech Synthesis Markup Language)
* Utilisation de voix neurales

## <a name="skip-to-samples-on-github"></a>Passer aux exemples sur GitHub

Si vous souhaitez passer directement à l’exemple de code, consultez les [exemples de démarrage rapide JavaScript](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/javascript/node/text-to-speech) sur GitHub.

## <a name="prerequisites"></a>Prérequis

Cet article part du principe que vous disposez d’un compte Azure et d’une ressource de service Speech. Si vous n’avez ni compte ni ressource, [essayez le service Speech gratuitement](../../../overview.md#try-the-speech-service-for-free).

## <a name="install-the-speech-sdk"></a>Installer le Kit de développement logiciel (SDK) Speech

Avant de pouvoir faire quoi que ce soit, vous devez installer le <a href="https://www.npmjs.com/package/microsoft-cognitiveservices-speech-sdk" target="_blank">SDK Speech pour JavaScript</a>. Suivez les instructions ci-dessous, en fonction de votre plateforme :
- <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=nodejs#get-the-speech-sdk" target="_blank">Node.js <span 
class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk?tabs=browser#get-the-speech-sdk" target="_blank">Navigateur web </a>

De plus, selon l’environnement cible, utilisez l’un des éléments suivants :

# <a name="script"></a>[script](#tab/script)

Téléchargez et extrayez le fichier *microsoft.cognitiveservices.speech.sdk.bundle.js* du <a href="https://aka.ms/csspeech/jsbrowserpackage" target="_blank">SDK Speech pour JavaScript</a>  , puis placez-le dans un dossier accessible à votre fichier HTML.

```html
<script src="microsoft.cognitiveservices.speech.sdk.bundle.js"></script>;
```

> [!TIP]
> Si vous ciblez un navigateur web et que vous utilisez l’étiquette `<script>`, le préfixe `sdk` n’est pas nécessaire. Le préfixe `sdk` est un alias utilisé pour nommer le module `require`.

# <a name="import"></a>[import](#tab/import)

```javascript
import * as sdk from "microsoft-cognitiveservices-speech-sdk";
```

Pour plus d’informations sur `import`, consultez la page consacrée aux directives <a href="https://javascript.info/import-export" target="_blank">export et import </a>.

# <a name="require"></a>[require](#tab/require)

```javascript
const sdk = require("microsoft-cognitiveservices-speech-sdk");
```

Pour plus d’informations sur `require`, consultez la <a href="https://nodejs.org/en/knowledge/getting-started/what-is-require/" target="_blank">présentation de require </a>.

---


## <a name="create-a-speech-configuration"></a>Créer une configuration Speech

Pour appeler le service Speech à l’aide du SDK Speech, vous devez créer une classe [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig). Cette classe comprend des informations sur votre ressource, par exemple votre clé ainsi que la région, le point de terminaison, l’hôte ou le jeton d’autorisation associés.

> [!NOTE]
> Quand vous procédez à une reconnaissance vocale, une synthèse vocale, une traduction ou une reconnaissance intentionnelle, vous devez toujours créer une configuration.

Vous pouvez initialiser une [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig) de plusieurs façons :

* Avec une ressource : transmettez une clé et la région associée.
* Avec un point de terminaison : transmettez un point de terminaison de service Speech. Une clé ou un jeton d’autorisation est facultatif.
* Avec un hôte : transmettez une adresse d’hôte. Une clé ou un jeton d’autorisation est facultatif.
* Avec un jeton d’autorisation : transmettez un jeton d’autorisation et la région associée.

Dans cet exemple, vous créez un [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig) à l’aide d’une clé de ressource et d’une région. Pour obtenir ces informations d’identification, suivez les étapes indiquées dans [Essayer le service Speech gratuitement](../../../overview.md#try-the-speech-service-for-free). Vous pouvez aussi créer un code réutilisable de base à utiliser pour le reste de cet article, que vous modifiez pour différentes personnalisations.

```javascript
function synthesizeSpeech() {
    const speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
}

synthesizeSpeech();
```

## <a name="synthesize-speech-to-a-file"></a>Synthétiser la voix dans un fichier

Ensuite, vous allez créer un objet [`SpeechSynthesizer`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesizer), qui exécute des conversions de synthèse vocale et des sorties vers des haut-parleurs, des fichiers ou d’autres flux de sortie. [`SpeechSynthesizer`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesizer) accepte en tant que paramètres l’objet [`SpeechConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechconfig) créé à l’étape précédente et un objet [`AudioConfig`](/javascript/api/microsoft-cognitiveservices-speech-sdk/audioconfig) qui spécifie la façon dont les résultats de sortie doivent être gérés.

Pour commencer, créez un `AudioConfig` pour écrire automatiquement la sortie dans un fichier `.wav` à l’aide de la fonction statique `fromAudioFileOutput()`.

```javascript
function synthesizeSpeech() {
    const speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const audioConfig = AudioConfig.fromAudioFileOutput("path/to/file.wav");
}
```

Ensuite, instanciez un `SpeechSynthesizer` transmettant votre objet `speechConfig` et l’objet `audioConfig` en tant que paramètres. Dès lors, exécuter la synthèse vocale et écrire dans un fichier sont aussi simples que d’exécuter `speakTextAsync()` avec une chaîne de texte. Le rappel de résultat est l’endroit idéal pour appeler `synthesizer.close()`. En fait, cet appel est nécessaire pour que la synthèse fonctionne correctement.

```javascript
function synthesizeSpeech() {
    const speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const audioConfig = AudioConfig.fromAudioFileOutput("path-to-file.wav");

    const synthesizer = new SpeechSynthesizer(speechConfig, audioConfig);
    synthesizer.speakTextAsync(
        "A simple test to write to a file.",
        result => {
            synthesizer.close();
            if (result) {
                // return result as stream
                return fs.createReadStream("path-to-file.wav");
            }
        },
        error => {
            console.log(error);
            synthesizer.close();
        });
}
```

Exécutez le programme ; un fichier `.wav` synthétisé est alors écrit à l’emplacement que vous avez spécifié. Il s’agit d’un bon exemple de l’utilisation la plus simple, mais vous allez maintenant vous pencher sur la personnalisation de la sortie et sur la gestion de la réponse de sortie sous forme de flux en mémoire pour travailler sur des scénarios personnalisés.

## <a name="synthesize-to-speaker-output"></a>Synthétiser vers la sortie du haut-parleur

Dans certains cas, vous pouvez souhaiter sortir directement la synthèse vocale vers un haut-parleur. Pour cela, instanciez `AudioConfig` à l’aide de la fonction statique `fromDefaultSpeakerOutput()`. La sortie est alors dirigée vers l’appareil de sortie actuellement actif.

```javascript
function synthesizeSpeech() {
    const speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const audioConfig = AudioConfig.fromDefaultSpeakerOutput();

    const synthesizer = new SpeechSynthesizer(speechConfig, audioConfig);
    synthesizer.speakTextAsync(
        "Synthesizing directly to speaker output.",
        result => {
            if (result) {
                synthesizer.close();
                return result.audioData;
            }
        },
        error => {
            console.log(error);
            synthesizer.close();
        });
}
```

## <a name="get-result-as-an-in-memory-stream"></a>Obtenir le résultat sous forme de flux en mémoire

Dans de nombreux scénarios de développement d’application vocale, vous aurez probablement besoin des données audio obtenues sous forme de flux en mémoire plutôt que directement écrites dans un fichier. Cela vous permettra de générer un comportement personnalisé, notamment :

* Résumer le tableau d’octets obtenu sous forme de flux pouvant faire l’objet de recherches pour des services en aval personnalisés.
* Intégrer le résultat à d’autres API ou services.
* Modifier les données audio, écrire des en-têtes `.wav` personnalisés, etc.

Il est facile d’apporter cette modification à partir de l’exemple précédent. Tout d’abord, supprimez le bloc `AudioConfig`, car vous allez gérer le comportement de sortie manuellement à partir de ce point pour un contrôle accru. Transmettez ensuite `undefined` pour `AudioConfig` dans le constructeur `SpeechSynthesizer`. 

> [!NOTE]
> Le fait de transmettre `undefined` pour `AudioConfig`, au lieu de l’omettre comme dans l’exemple de sortie de haut-parleur précédent, n’aura pas pour effet de lire par défaut l’audio sur l’appareil de sortie actuellement actif.

Cette fois, vous allez enregistrer le résultat dans une variable [`SpeechSynthesisResult`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesisresult). La propriété `SpeechSynthesisResult.audioData` retourne un `ArrayBuffer` des données de sortie, le type de flux de navigateur par défaut. Pour le code serveur, convertissez arrayBuffer en un flux de mémoire tampon. 

Le code suivant fonctionne pour le code côté client. 

```javascript
function synthesizeSpeech() {
    const speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const synthesizer = new sdk.SpeechSynthesizer(speechConfig);

    synthesizer.speakTextAsync(
        "Getting the response as an in-memory stream.",
        result => {
            synthesizer.close();
            return result.audioData;
        },
        error => {
            console.log(error);
            synthesizer.close();
        });
}
```

À partir de là, vous pouvez implémenter un comportement personnalisé à partir de l’objet `ArrayBuffer` obtenu. ArrayBuffer est un type qu’il est courant de recevoir dans un navigateur et de lire dans ce format. 

Pour le code serveur, si vous devez utiliser les données sous forme de flux à la place d’ArrayBuffer, convertissez l’objet en flux. 

```javascript
function synthesizeSpeech() {
    const speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const synthesizer = new sdk.SpeechSynthesizer(speechConfig);

    synthesizer.speakTextAsync(
        "Getting the response as an in-memory stream.",
        result => {
            const { audioData } = result;

            synthesizer.close();

            // convert arrayBuffer to stream
            // return stream
            const bufferStream = new PassThrough();
            bufferStream.end(Buffer.from(audioData));
            return bufferStream;
        },
        error => {
            console.log(error);
            synthesizer.close();
        });
}
```

## <a name="customize-audio-format"></a>Personnaliser le format audio

La section suivante montre comment personnaliser les attributs de sortie audio, notamment :

* Le type de fichier audio
* Le taux d’échantillonnage
* La profondeur de bits

Pour changer de format audio, utilisez la propriété `speechSynthesisOutputFormat` sur l’objet `SpeechConfig`. Cette propriété attend un `enum` de type [`SpeechSynthesisOutputFormat`](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesisoutputformat), dont vous allez vous servir pour sélectionner le format de sortie. Consultez les documents de référence pour obtenir la [liste des formats audio](/javascript/api/microsoft-cognitiveservices-speech-sdk/speechsynthesisoutputformat) disponibles.

Il existe diverses options pour les différents types de fichiers, ce qui permet de répondre à vos besoins. Notez que par définition, les formats bruts comme `Raw24Khz16BitMonoPcm` n’incluent pas d’en-têtes audio. N’utilisez les formats bruts que si vous savez que votre implémentation en aval pourra décoder un flux binaire brut ou bien si vous envisagez de créer manuellement des en-têtes en fonction de la profondeur de bits, du taux d’échantillonnage, du nombre de canaux, etc.

> [!NOTE]
> Les voix **en-US-AriaRUS** et **en-US-GuyRUS** sont créées à partir d’exemples encodés au taux d’échantillonnage `Riff24Khz16BitMonoPcm`.

Dans cet exemple, vous allez spécifier un format RIFF haute fidélité `Riff24Khz16BitMonoPcm` en définissant `speechSynthesisOutputFormat` sur l’objet `SpeechConfig`. Comme dans l’exemple de la section précédente, récupérez les données `ArrayBuffer` audio et interagissez avec elles.

```javascript
function synthesizeSpeech() {
    const speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");

    // Set the output format
    speechConfig.speechSynthesisOutputFormat = SpeechSynthesisOutputFormat.Riff24Khz16BitMonoPcm;

    const synthesizer = new sdk.SpeechSynthesizer(speechConfig, undefined);
    synthesizer.speakTextAsync(
        "Customizing audio output format.",
        result => {
            // Interact with the audio ArrayBuffer data
            const audioData = result.audioData;
            console.log(`Audio data byte size: ${audioData.byteLength}.`)

            synthesizer.close();
        },
        error => {
            console.log(error);
            synthesizer.close();
        });
}
```

Si vous exécutez à nouveau votre programme, un fichier `.wav` est écrit dans le chemin spécifié.

## <a name="use-ssml-to-customize-speech-characteristics"></a>Utiliser SSML pour personnaliser les caractéristiques vocales

Le langage SSML (Speech Synthesis Markup Language) vous permet d’ajuster la tonalité, la prononciation, la vitesse d’élocution, le volume et d’autres éléments de sortie de la synthèse vocale en soumettant vos demandes à partir d’un schéma XML. Cette section présente quelques exemples d’utilisation pratiques, mais pour des indications plus détaillées, consultez l’[article de procédures SSML](../../../speech-synthesis-markup.md).

Pour commencer à utiliser SSML pour la personnalisation, vous allez apporter une modification simple qui change la voix.
Tout d’abord, créez un fichier XML pour la configuration SSML dans le répertoire racine de votre projet, à savoir `ssml.xml`. L’élément racine est toujours `<speak>`, et le fait d’intégrer le texte dans un élément `<voice>` vous permet de changer la voix à l’aide du paramètre `name`. Cet exemple change la voix en voix masculine de langue anglaise (Royaume-Uni). Notez que cette voix est une voix **standard**, dont le coût et la disponibilité sont différents de ceux des voix **neuronales**. Consultez la [liste complète](../../../language-support.md#standard-voices) des voix **standard** prises en charge.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    When you're on the motorway, it's a good idea to use a sat-nav.
  </voice>
</speak>
```

Ensuite, vous devez changer la demande de synthèse vocale de sorte qu’elle fasse référence à votre fichier XML. La demande est presque totalement identique, sauf qu’au lieu d’utiliser la fonction `speakTextAsync()`, vous allez utiliser `speakSsmlAsync()`. Sachant que cette fonction attend une chaîne XML, vous devez d’abord créer une fonction pour charger un fichier XML et le retourner sous forme de chaîne.

```javascript
function xmlToString(filePath) {
    const xml = readFileSync(filePath, "utf8");
    return xml;
}
```

Pour plus d’informations sur `readFileSync`, consultez <a href="https://nodejs.org/api/fs.html#fs_fs_readlinksync_path_options" target="_blank">Système de fichiers Node.js</a>. À partir de là, l’objet obtenu est exactement le même que dans les exemples précédents.

```javascript
function synthesizeSpeech() {
    const speechConfig = sdk.SpeechConfig.fromSubscription("YourSubscriptionKey", "YourServiceRegion");
    const synthesizer = new sdk.SpeechSynthesizer(speechConfig, undefined);

    const ssml = xmlToString("ssml.xml");
    synthesizer.speakSsmlAsync(
        ssml,
        result => {
            if (result.errorDetails) {
                console.error(result.errorDetails);
            } else {
                console.log(JSON.stringify(result));
            }

            synthesizer.close();
        },
        error => {
            console.log(error);
            synthesizer.close();
        });
}
```

Bien que la sortie fonctionne, vous pouvez y apporter d’autres modifications simples pour obtenir un ton plus naturel. La vitesse d’élocution générale étant un peu trop rapide, nous allons ajouter une balise `<prosody>` et réduire la vitesse à **90 %** de la vitesse par défaut. De plus, la pause après la virgule dans la phrase est un peu trop courte et n’est pas naturelle à l’oreille. Pour corriger ce problème, ajoutez une balise `<break>` pour ralentir le débit de parole, puis définissez le paramètre de temps sur **200ms**. Réexécutez la synthèse pour observer l’incidence de ces personnalisations sur la sortie.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    <prosody rate="0.9">
      When you're on the motorway,<break time="200ms"/> it's a good idea to use a sat-nav.
    </prosody>
  </voice>
</speak>
```

## <a name="neural-voices"></a>Voix neurales

Les voix neuronales sont des algorithmes de synthèse vocale qui reposent sur des réseaux neuronaux profonds. Lorsque vous utilisez une voix neuronale, il est presque impossible de distinguer la voix synthétisée des enregistrements humains. Grâce à la prosodie naturelle quasi humaine et à la bonne articulation des mots, les voix neuronales réduisent considérablement la fatigue d’écoute des utilisateurs qui interagissent avec les systèmes d’intelligence artificielle.

Pour passer à une voix neuronale, remplacez `name` par une des [options de voix neuronale](../../../language-support.md#neural-voices). Ensuite, ajoutez un espace de noms XML pour `mstts` et intégrez votre texte dans la balise `<mstts:express-as>`. Utilisez le paramètre `style` pour personnaliser le style vocal. Cet exemple utilise le style vocal `cheerful`, mais essayez de le remplacer par `customerservice` ou `chat` pour voir la différence.

> [!IMPORTANT]
> Les voix neuronales sont **uniquement** prises en charge pour les ressources vocales créées dans les régions *USA Est*, *Asie Sud-Est* et *Europe Ouest*.

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis"
    xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
  <voice name="en-US-AriaNeural">
    <mstts:express-as style="cheerful">
      This is awesome!
    </mstts:express-as>
  </voice>
</speak>
```
