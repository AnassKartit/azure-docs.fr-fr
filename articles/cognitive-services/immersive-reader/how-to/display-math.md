---
title: Afficher des éléments mathématiques dans le lecteur immersif
titleSuffix: Azure Applied AI Services
description: Cet article vous explique comment afficher des éléments mathématiques dans le lecteur immersif.
author: nitinme
manager: guillasi
ms.service: applied-ai-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: nitinme
ms.custom: devx-track-js
ms.openlocfilehash: 0d4a0237d502cb353c262a40c92f99dfdbb8e7d6
ms.sourcegitcommit: 34feb2a5bdba1351d9fc375c46e62aa40bbd5a1f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/10/2021
ms.locfileid: "111888173"
---
# <a name="how-to-display-math-in-the-immersive-reader"></a>Guide pratique pour afficher des éléments mathématiques dans le lecteur immersif

Le lecteur immersif peut afficher des éléments mathématiques quand ils sont fournis sous forme de langage [MathML](https://developer.mozilla.org/docs/Web/MathML).
Le type MIME peut être défini via le [bloc](../reference.md#chunk) du lecteur immersif. Pour plus d’informations, consultez [Types MIME pris en charge](../reference.md#supported-mime-types).

## <a name="send-math-to-the-immersive-reader"></a>Envoyer des éléments mathématiques au lecteur immersif
Pour envoyer des éléments mathématiques au lecteur immersif, fournissez un bloc contenant du langage MathML et définissez le type MIME sur ```application/mathml+xml```.

Par exemple, si votre contenu est le suivant :

```html
<div id='ir-content'>
    <math xmlns='http://www.w3.org/1998/Math/MathML'>
        <mfrac>
            <mrow>
                <msup>
                    <mi>x</mi>
                    <mn>2</mn>
                </msup>
                <mo>+</mo>
                <mn>3</mn>
                <mi>x</mi>
                <mo>+</mo>
                <mn>2</mn>
            </mrow>
            <mrow>
                <mi>x</mi>
                <mo>−</mo>
                <mn>3</mn>
            </mrow>
        </mfrac>
        <mo>=</mo>
        <mn>4</mn>
    </math>
</div>
```

Vous pouvez afficher votre contenu avec le code JavaScript suivant.

```javascript
const data = {
    title: 'My Math',
    chunks: [{
        content: document.getElementById('ir-content').innerHTML.trim(),
        mimeType: 'application/mathml+xml'
    }]
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, data, YOUR_OPTIONS);
```

Quand vous lancez le lecteur immersif, vous devez voir :

![Éléments mathématiques dans le lecteur immersif](../media/how-tos/1-math.png)

## <a name="next-steps"></a>Étapes suivantes

* Explorer le [SDK Lecteur Immersif](https://github.com/microsoft/immersive-reader-sdk) et la [référence du SDK Lecteur immersif](../reference.md)