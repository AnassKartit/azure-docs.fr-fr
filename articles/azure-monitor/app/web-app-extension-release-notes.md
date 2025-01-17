---
title: Notes de publication pour l’extension d’application web Azure - Application Insights
description: Publie des notes pour l’extension de Web Apps Azure pour l’instrumentation du runtime avec Application Insights.
ms.topic: conceptual
ms.date: 06/26/2020
ms.openlocfilehash: 84d01a2bcc4e371ca03610f2d002eaaf73159b2a
ms.sourcegitcommit: 677e8acc9a2e8b842e4aef4472599f9264e989e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2021
ms.locfileid: "132316030"
---
# <a name="release-notes-for-azure-web-app-extension-for-application-insights"></a>Notes de publication pour l’extension d’application web Azure pour Application Insights

Cet article contient les notes de publication de l’extension de Web Apps Azure pour l’instrumentation du runtime avec Application Insights. Cela s’applique uniquement aux extensions préinstallées.

Apprenez-en davantage sur l’[extension d’application web Azure pour Application Insights](azure-web-apps.md).

## <a name="frequently-asked-questions"></a>Forum aux questions

- Comment trouver la version de l’extension sur laquelle je suis actuellement ?
    - Atteindre `https://<yoursitename>.scm.azurewebsites.net/ApplicationInsights`. Pour plus d’informations, consultez le guide de résolution des problèmes pas à pas pour l’analyse basée sur l’extension/l’agent pour [ASP.NET Core](./azure-web-apps-net-core.md#troubleshooting), [ASP.NET](./azure-web-apps-net.md#troubleshooting), [Java](./azure-web-apps-java.md#troubleshooting) ou [Node.js](./azure-web-apps-nodejs.md#troubleshooting).

- Que se passe-t-il si j’utilise des extensions privées ?
    - Désinstallez les extensions de site privé, car elles ne sont plus prises en charge.

## <a name="release-notes"></a>Notes de publication

### <a name="2842"></a>2.8.42

- Extension JAVA : mise à niveau vers l’[agent Java 3.2.0](https://github.com/microsoft/ApplicationInsights-Java/releases/tag/3.2.0) à partir de la version 2.5.1.
- Extension Node.js : mise à jour du Kit de développement logiciel (SDK) IA vers la version [2.1.8](https://github.com/microsoft/ApplicationInsights-node.js/releases/tag/2.1.8) à partir de la version 2.1.7. Ajout de la prise en charge des identités managées AAD affectées par l’utilisateur et le système.
- .NET Core : ajout de déploiements autonomes et prise en charge de .NET 6.0 à l’aide d’un [crochet de démarrage .NET](https://github.com/dotnet/runtime/blob/main/docs/design/features/host-startup-hook.md).

### <a name="2841"></a>2.8.41

- Extension Node.js : mise à jour du Kit de développement logiciel (SDK) IA vers la version [2.1.7](https://github.com/microsoft/ApplicationInsights-node.js/releases/tag/2.1.7) à partir de la version 2.1.3.
- .NET Core : suppression des versions hors support (2.1). Les versions prises en charge sont les versions 3.1 et 5.0.

### <a name="2840"></a>2.8.40

- Extension JAVA : mise à niveau vers l’[agent Java 3.1.1 (GA)](https://github.com/microsoft/ApplicationInsights-Java/releases/tag/3.1.1) à partir de la version 3.0.2.
- Extension Node.js : mise à jour du Kit de développement logiciel (SDK) IA vers la version [2.1.3](https://github.com/microsoft/ApplicationInsights-node.js/releases/tag/2.1.3) à partir de la version 1.8.8.

### <a name="2839"></a>2.8.39

- .NET Core : ajout de la prise en charge de .NET Core 5.0.

### <a name="2838"></a>2.8.38

- Extension JAVA : mise à niveau vers l’[agent Java 3.0.2 (GA)](https://github.com/microsoft/ApplicationInsights-Java/releases/tag/3.0.2) à partir de la version 2.5.1.
- Extension Node.js : mise à jour du Kit de développement logiciel (SDK) IA vers la version [1.8.8](https://github.com/microsoft/ApplicationInsights-node.js/releases/tag/1.8.8) à partir de la version 1.8.7.
- .NET Core : suppression des versions hors support (2.0, 2.2, 3.0). Les versions prises en charge sont les versions 2.1 et 3.1.

### <a name="2837"></a>2.8.37

- Extension Windows AppSvc : .NET Core rendu opérationnel avec tout version de System.Diagnostics.DiagnosticSource.dll.

### <a name="2836"></a>2.8.36

- Extension Windows AppSvc : activation d’inter-op avec le Kit de développement logiciel (SDK) IA dans .NET Core.

### <a name="2835"></a>2.8.35

- Extension Windows AppSvc : Ajout de la prise en charge de .NET Core 3.1.

### <a name="2833"></a>2.8.33

- Les agents .NET, .NET core, Java et Node.js, ainsi que l’extension Windows : Prise en charge des clouds souverains. Les chaînes de connexion peuvent être utilisées pour envoyer des données à des clouds souverains.

### <a name="2831"></a>2.8.31

- Agent ASP.NET Core : Correction du problème lié à l’une des références du kit de développement logiciel (SDK) Application Insights mises à jour (voir les problèmes connus pour 2.8.26). Si la version incorrecte de `System.Diagnostics.DiagnosticSource.dll` est déjà chargée par le runtime, l’extension sans code ne bloquera pas l’application et la sauvegarde. Pour les clients concernés par ce problème, il est recommandé de supprimer le `System.Diagnostics.DiagnosticSource.dll` du dossier bin ou d’utiliser l’ancienne version de l’extension en définissant « ApplicationInsightsAgent_EXTENSIONVERSION = 2.8.24 ». Dans le cas contraire, l’analyse des applications n’est pas activée.

### <a name="2826"></a>2.8.26

- Agent ASP.NET Core : Correction du problème lié à la mise à jour du kit de développement logiciel Application Insights. L’agent ne tente pas de charger `AiHostingStartup` si le fichier ApplicationInsights.dll est déjà présent dans le dossier classe. Cela résout les problèmes liés à la réflexion viaAssembly\<AiHostingStartup\>.GetTypes().
- Problèmes connus : Une exception `System.IO.FileLoadException: Could not load file or assembly 'System.Diagnostics.DiagnosticSource, Version=4.0.4.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'` peut être levée si une autre version de `DiagnosticSource` dll est chargée. Cela peut se produire, par exemple, si `System.Diagnostics.DiagnosticSource.dll` est présent dans le dossier de publication. Pour atténuer les risques, utilisez la version précédente de l’extension en définissant les paramètres de l’application dans services d’application : ApplicationInsightsAgent_EXTENSIONVERSION=2.8.24.

### <a name="2824"></a>2.8.24

- Version repackagée de 2.8.21.

### <a name="2823"></a>2.8.23

- Ajout de la prise en charge de l’analyse du code ASP.NET Core 3.0.
- Mise à jour de ASP.NET Core SDK vers [2.8.0](https://github.com/microsoft/ApplicationInsights-aspnetcore/releases/tag/2.8.0) pour les versions du runtime 2.1, 2.2 et 3.0. Les applications ciblant .NET Core 2.0 continuent à utiliser le 2.1.1 du kit de développement logiciel (SDK).

### <a name="2814"></a>2.8.14

- Mise à jour d’ASP.NET Core de la version du kit de développement logiciel (SDK) de 2.3.0 vers la dernière version (2.6.1) pour les applications ciblant .NET Core 2.1, 2.2. Les applications ciblant .NET Core 2.0 continuent à utiliser le 2.1.1 du kit de développement logiciel (SDK).

### <a name="2812"></a>2.8.12

- Prise en charge des applications ASP.NET Core 2.2.
- Correction d’un bogue dans ASP.NET Core extension provoquant l’injection du SDK même lorsque l’application est déjà instrumentée avec le kit de développement logiciel (SDK). Pour les applications 2.1 et 2.2, la présence de ApplicationInsights.dll dans le dossier de l’application entraîne désormais l’interruption de l’extension. Pour les applications 2.0, l’extension est désactivée uniquement si ApplicationInsights est activé avec un appel de `UseApplicationInsights()`.

- Correction permanente de la réponse HTML incomplète pour les applications ASP.NET Core. Ce correctif est maintenant étendu pour fonctionner avec les applications .NET Core 2.2.

- Ajout de la prise en charge de la désactivation de l’injection JavaScript pour les applications ASP.NET Core (`APPINSIGHTS_JAVASCRIPT_ENABLED=false appsetting`). Pour ASP.NET Core, l’injection JavaScript est en mode « désactivation » par défaut, sauf si elle est explicitement désactivée. (Le paramètre par défaut est utilisé pour conserver le comportement actuel.)

- Correction du bogue de l’extension ASP.NET Core qui a provoqué l’injection même si ikey n’était pas présent.
- Correction d’un bogue dans la logique de préfixe de la version du SDK qui provoquait une version incorrecte du kit de développement logiciel (SDK).

- Ajout du préfixe de version du SDK pour les applications ASP.NET Core pour identifier la façon dont la télémétrie a été collectée.
- Correction de la page SCM-ApplicationInsights pour afficher correctement la version de l’extension pré-installée.

### <a name="2810"></a>2.8.10

- Correction de la réponse HTML incomplète pour les applications ASP.NET Core.

## <a name="next-steps"></a>Étapes suivantes

- Pour plus d’informations sur la configuration de l’analyse pour Azure App Service, consultez la [documentation Surveillance des applications pour Azure App Services](azure-web-apps.md). 
