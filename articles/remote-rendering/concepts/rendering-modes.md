---
title: Modes de rendu
description: Décrit les différents modes de rendu côté serveur
author: florianborn71
ms.author: flborn
ms.date: 02/03/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: 2cf1872bcdd7b1bda74046198f5fc32be1069913
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99594499"
---
# <a name="rendering-modes"></a>Modes de rendu

Azure Remote Rendering offre deux modes principaux de fonctionnement, le mode **TileBasedComposition** et le mode **DepthBasedComposition**. Ces modes déterminent la façon dont la charge de travail est répartie sur plusieurs GPU sur le serveur. Le mode doit être spécifié au moment de la connexion et ne peut pas être modifié pendant l’exécution.

Les deux modes présentent des avantages, mais également avec des limitations de fonctionnalités. Par conséquent, le choix du mode le plus approprié est spécifique du cas d’usage.

## <a name="modes"></a>Modes

Les deux modes sont désormais décrits plus en détail.

### <a name="tilebasedcomposition-mode"></a>Mode « TileBasedComposition »

En mode **TileBasedComposition**, chaque GPU impliqué affiche des sous-rectangles spécifiques (vignettes). Le GPU principal compose l’image finale à partir des vignettes avant son envoi en tant que trame vidéo au client. Ainsi, tous les GPU requièrent le même ensemble de ressources pour le rendu, de sorte que les ressources chargées doivent tenir dans la mémoire d’un seul GPU.

La qualité de rendu dans ce mode est légèrement meilleure que dans le mode **DepthBasedComposition**, car la technologie MSAA peut opérer sur l’ensemble de la géométrie pour chaque GPU. La capture d’écran suivante montre que l’anticrénelage fonctionne correctement pour les deux bords de la même manière :

![MSAA dans TileBasedComposition](./media/service-render-mode-quality.png)

En outre, dans ce mode, chaque composant peut être basculé vers un matériau transparent ou en mode **transparence** via le composant [HierarchicalStateOverrideComponent](../overview/features/override-hierarchical-state.md).

### <a name="depthbasedcomposition-mode"></a>Mode « DepthBasedComposition »

En mode **DepthBasedComposition**, chaque GPU impliqué affiche en résolution plein écran, mais uniquement un sous-ensemble de mailles. La composition finale de l’image sur le GPU principal veille à ce que les éléments soient correctement fusionnés en fonction de leurs informations de profondeur. Naturellement, la charge utile de mémoire est répartie entre les GPU, ce qui permet de rendre les modèles qui ne tiennent pas dans la mémoire d’un seul GPU.

Chaque GPU utilise la technologie MSAA pour l’anticrénelage du contenu local. Toutefois, il peut exister un crénelage intrinsèque entre des bords produits par des GPU distincts. Cet effet est atténué par post-traitement de l’image finale, mais la qualité de MSAA reste pire qu’en mode **TileBasedComposition**.

Les artefacts de la technologie MSAA sont illustrés dans l’image suivante : ![MSAA en mode DepthBasedComposition](./media/service-render-mode-balanced.png)

L’anticrénelage fonctionne correctement entre la sculpture et le rideau, car les deux parties sont rendues sur le même GPU. Par ailleurs, le bord entre le rideau et le mur présente un crénelage, car ces deux éléments sont composés par des GPU distincts.

La principale limitation de ce mode est que les composants de la géométrie ne peuvent pas être basculés vers des matériaux transparents de manière dynamique, et que le mode **transparence** ne fonctionne pas non plus pour le composant [HierarchicalStateOverrideComponent](../overview/features/override-hierarchical-state.md). En revanche, les autres fonctionnalités de remplacement d’état (contour, teinte, etc.) fonctionnent bel et bien. Par ailleurs, les matériaux marqués comme transparents au moment de la conversion fonctionnent correctement dans ce mode.

### <a name="performance"></a>Performances

Les caractéristiques de performances des deux modes variant en fonction du cas d’usage, il est difficile de conclure ou de formuler des recommandations générales. Si vous n’êtes pas concerné par les limitations mentionnées ci-dessus (mémoire ou transparence), nous vous recommandons de tester les deux modes et d’observer les performances en utilisant différentes positions de caméra.

## <a name="setting-the-render-mode"></a>Définition du mode de rendu

Le mode de rendu utilisé sur un serveur Remote Rendering est spécifié pendant l’exécution de `RenderingSession.ConnectAsync` via les paramètres `RendererInitOptions`.

```cs
async void ExampleConnect(RenderingSession session)
{
    RendererInitOptions parameters = new RendererInitOptions();

    // Connect with one rendering mode
    parameters.RenderMode = ServiceRenderMode.TileBasedComposition;
    await session.ConnectAsync(parameters);

    session.Disconnect();

    // Wait until session.IsConnected == false

    // Reconnect with a different rendering mode
    parameters.RenderMode = ServiceRenderMode.DepthBasedComposition;
    await session.ConnectAsync(parameters);
}
```

## <a name="api-documentation"></a>Documentation de l’API

* [C# RenderingSession.ConnectAsync()](/dotnet/api/microsoft.azure.remoterendering.renderingsession.connectasync)
* [RendererInitOptions, struct C#](/dotnet/api/microsoft.azure.remoterendering.rendererinitoptions)
* [C++ RenderingSession::ConnectToConnectAsyncRuntime()](/cpp/api/remote-rendering/renderingsession#connectasync)
* [RendererInitOptions, struct C++](/cpp/api/remote-rendering/rendererinitoptions)

## <a name="next-steps"></a>Étapes suivantes

* [Sessions](../concepts/sessions.md)
* [Composant de remplacement d’état hiérarchique](../overview/features/override-hierarchical-state.md)