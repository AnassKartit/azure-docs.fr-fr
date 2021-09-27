---
author: PatrickFarley
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: pafarley
ms.openlocfilehash: e4377b4c69f484f1e248f7aa74731b9fbe956d52
ms.sourcegitcommit: f2d0e1e91a6c345858d3c21b387b15e3b1fa8b4c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/07/2021
ms.locfileid: "123539951"
---
Trois SDK Speech sont disponibles pour les développements destinés à macOS.

- Le SDK Speech Objective-C est disponible en natif sous forme de package CocoaPod
- Le SDK Speech .NET peut être utilisé avec **Xamarin.Mac**, car il implémente .NET Standard 2.0
- Le SDK Speech Python est disponible sous forme de module PyPI

> [!TIP]
> Pour plus d’informations sur l’utilisation du Kit de développement logiciel (SDK) Speech Objective-C avec Swift, consultez <a href="https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/importing_objective-c_into_swift" target="_blank">Importation d’Objective-C dans Swift</a>.

### <a name="system-requirements"></a>Configuration système requise

- macOS version 10.13 ou ultérieure

# <a name="xcode"></a>[Xcode](#tab/mac-xcode)

:::row:::
    :::column span="3":::
        Le package CocoaPod macOS peut être téléchargé et utilisé avec l’IDE <a href="https://apps.apple.com/us/app/xcode/id497799835" target="_blank">Xcode 9.4.1 (ou ultérieur) </a>. Tout d’abord, <a href="https://aka.ms/csspeech/macosbinary" target="_blank">téléchargez le fichier binaire de CocoaPod</a>. Extrayez le pod dans le répertoire où vous prévoyez de l’utiliser, créez un *Podfile* et répertoriez le `pod` en tant que `target`.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Xcode" src="https://docs.microsoft.com/media/logos/logo_xcode.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

```
platform :ios, '9.3'
use_frameworks!

target 'MyApp' do
  pod 'MicrosoftCognitiveServicesSpeech', '~> 1.15.0'
end
```

# <a name="xamarinmac"></a>[Xamarin.Mac](#tab/mac-xamarin)

:::row:::
    :::column span="3":::
        Xamarin.Mac expose le SDK macOS complet pour permettre aux développeurs .NET de créer des applications Mac natives en utilisant C#. Pour plus d’informations, consultez <a href="/xamarin/mac/" target="_blank">Xamarin.Mac </a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Xamarin" src="https://docs.microsoft.com/media/logos/logo_xamarin.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

# <a name="python"></a>[Python](#tab/mac-python)

[!INCLUDE [Get Python Speech SDK](get-speech-sdk-python.md)]

---

#### <a name="additional-resources"></a>Ressources supplémentaires

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/objectivec/macos" target="_blank">Code source Objective-C du guide de démarrage rapide du SDK Speech pour macOS</a>
- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/swift/macos" target="_blank">Code source Swift du guide de démarrage rapide du SDK Speech pour macOS</a>