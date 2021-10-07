---
title: Gérer des jumeaux numériques
titleSuffix: Azure Digital Twins
description: Consultez comment récupérer, mettre à jour et supprimer des jumeaux et des relations individuels.
author: baanders
ms.author: baanders
ms.date: 9/13/2021
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 4be8ef1085d6a940e7f2d95f43a75d1b4e7c11f8
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128611685"
---
# <a name="manage-digital-twins"></a>Gérer des jumeaux numériques

Les entités de votre environnement sont représentées par des [jumeaux numériques](concepts-twins-graph.md). La gestion de vos jumeaux numériques peut inclure la création, la modification et la suppression.

Cet article se concentre sur la gestion des jumeaux numériques. Pour utiliser des relations et le [graphe des jumeaux](concepts-twins-graph.md) dans leur ensemble, consultez [Gérer le graphe de jumeaux et les relations](how-to-manage-graph.md).

> [!TIP]
> Toutes les fonctions du Kit de développement logiciel (SDK) sont disponibles en versions synchrone et asynchrone.

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

[!INCLUDE [digital-twins-developer-interfaces.md](../../includes/digital-twins-developer-interfaces.md)]

[!INCLUDE [visualizing with Azure Digital Twins explorer](../../includes/digital-twins-visualization.md)]

:::image type="content" source="media/concepts-azure-digital-twins-explorer/azure-digital-twins-explorer-demo.png" alt-text="Capture d’écran d’Azure Digital Twins Explorer montrant des exemples de modèles et de jumeaux." lightbox="media/concepts-azure-digital-twins-explorer/azure-digital-twins-explorer-demo.png":::

## <a name="create-a-digital-twin"></a>Créer un jumeau numérique

Pour créer un jumeau, vous utilisez la méthode `CreateOrReplaceDigitalTwinAsync()` sur le client de service comme suit :

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="CreateTwinCall":::

Pour créer un jumeau numérique, vous devez fournir les éléments suivants :
* Une valeur d’identifiant que vous souhaitez attribuer au jumeau numérique (vous définissez cet identifiant lors de la création du jumeau)
* Le [modèle](concepts-models.md) à utiliser
* Toute initialisation souhaitée des données de jumeau, y compris…
    - Les propriétés (facultatives pour l’initialisation) : si vous le souhaitez, vous pouvez définir les valeurs initiales des propriétés du jumeau numérique. Les propriétés sont considérées comme facultatives et peuvent être définies ultérieurement, mais notez qu’**elles ne s’affichent pas dans le cadre d’un jumeau tant qu’elles n’ont pas été définies**.
    - La télémétrie (recommandée pour l’initialisation) : vous pouvez également définir les valeurs initiales des champs de télémétrie sur le jumeau. Bien que l’initialisation de la télémétrie ne soit pas obligatoire, les champs de télémétrie ne s’affichent pas non plus dans le cadre d’un jumeau tant qu’ils n’ont pas été définis. Cela signifie que **vous ne pourrez pas modifier les valeurs de télémétrie pour un jumeau, sauf si elles ont d’abord été initialisées**.
    - Les composants (obligatoires pour l’initialisation s’ils sont présents sur le jumeau) : si votre jumeau contient des [composants](concepts-models.md#elements-of-a-model), ceux-ci doivent être initialisés lors de la création du jumeau. Il peut s’agir d’objets vides, mais les composants proprement dits doivent exister.
    
Le modèle et les éventuelles valeurs des propriétés initiales sont fournis par le biais du paramètre `initData`, qui est une chaîne JSON contenant les données pertinentes. Pour plus d’informations sur la structuration de cet objet, passez à la section suivante.

> [!TIP]
> Après la création ou la mise à jour d’un jumeau, il peut y avoir une latence allant jusqu’à 10 secondes avant que les modifications soient reflétées dans les [requêtes](how-to-query-graph.md). L’API `GetDigitalTwin` (décrite [plus loin dans cet article](#get-data-for-a-digital-twin)) ne subit pas ce délai. Utilisez l’appel d’API au lieu d’une interrogation pour voir vos nouveaux jumeaux créés si vous avez besoin d’une réponse instantanée. 

### <a name="initialize-model-and-properties"></a>Initialiser le modèle et les propriétés

Vous pouvez initialiser les propriétés d’un jumeau au moment de sa création. 

L’API de création de jumeau accepte un objet qui est sérialisé dans une description JSON valide des propriétés du jumeau. Consultez [Jumeaux numériques et graphe des jumeaux](concepts-twins-graph.md) pour obtenir une description du format JSON d’un jumeau. 

Tout d’abord, vous pouvez créer un objet de données pour représenter le jumeau et ses données de propriété. Vous pouvez créer un objet de paramètre manuellement ou à l’aide d’une classe d’assistance fournie. Voici un exemple de chaque méthode.

#### <a name="create-twins-using-manually-created-data"></a>Créer des jumeaux à l’aide de données créées manuellement

Sans l’utilisation de classes d’assistance personnalisées, vous pouvez représenter les propriétés d’un jumeau dans un `Dictionary<string, object>`, où `string` est le nom de la propriété et `object` est un objet représentant la propriété et sa valeur.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="CreateTwin_noHelper":::

#### <a name="create-twins-with-the-helper-class"></a>Créer des jumeaux avec la classe d’assistance

La classe d’assistance de `BasicDigitalTwin` vous permet de stocker directement les champs de propriété dans un objet « jumeau ». Vous pouvez toujours créer la liste de propriétés à l’aide d’un `Dictionary<string, object>`, qui peut ensuite être ajouté à l’objet jumeau comme `CustomProperties` directement.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="CreateTwin_withHelper":::

>[!NOTE]
> Les objets `BasicDigitalTwin` sont fournis avec un champ `Id`. Vous pouvez conserver ce champ vide, mais, si vous ajoutez une valeur d’ID, celle-ci doit correspondre au paramètre d’ID transmis à l’appel de `CreateOrReplaceDigitalTwinAsync()`. Par exemple :
>
>```csharp
>twin.Id = "myRoomId";
>```

## <a name="get-data-for-a-digital-twin"></a>Obtenir des données pour un jumeau numérique

Vous pouvez accéder aux détails de n’importe quel jumeau numérique en appelant la méthode `GetDigitalTwin()` comme suit :

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="GetTwinCall":::

Cet appel retourne des données de jumeau sous forme de type d’objet fortement typé tel que `BasicDigitalTwin`. `BasicDigitalTwin` est une classe d’assistance de sérialisation incluse avec le Kit de développement logiciel (SDK), qui retourne les propriétés et les métadonnées de base du jumeau dans un format préalablement analysé. Vous pouvez toujours désérialiser des données de jumeau à l’aide de la bibliothèque JSON de votre choix, comme `System.Text.Json` ou `Newtonsoft.Json`. Toutefois, si vous avez besoin d’un accès de base à un jumeau, les classes d’assistance peuvent vous faciliter la tâche.

> [!NOTE]
> `BasicDigitalTwin` utilise des attributs `System.Text.Json`. Pour pouvoir utiliser `BasicDigitalTwin` avec votre [DigitalTwinsClient](/dotnet/api/azure.digitaltwins.core.digitaltwinsclient?view=azure-dotnet&preserve-view=true), vous devez initialiser le client avec le constructeur par défaut. Si vous voulez personnaliser l’option du sérialiseur, utilisez [JsonObjectSerializer](/dotnet/api/azure.core.serialization.jsonobjectserializer?view=azure-dotnet&preserve-view=true).

La classe d’assistance `BasicDigitalTwin` vous donne également accès aux propriétés définies dans le jumeau, par le biais d’un `Dictionary<string, object>`. Pour lister les propriétés du jumeau, vous pouvez utiliser ceci :

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="GetTwin" highlight="2":::

Seules les propriétés qui ont été définies au moins une fois sont retournées lorsque vous récupérez un jumeau avec la méthode `GetDigitalTwin()`.

>[!TIP]
>La `displayName` pour un jumeau est une partie de ses métadonnées de modèle, donc elle ne s’affichera pas lors de l’obtention de données pour l’instance jumelle. Pour afficher cette valeur, vous pouvez [la récupérer à partir du modèle](how-to-manage-model.md#retrieve-models).

Pour récupérer plusieurs jumeaux avec un seul appel d’API, consultez les exemples d’API de requête dans [Interroger le graphe de jumeaux](how-to-query-graph.md).

Prenez pour exemple le modèle suivant (écrit en [DTDL (Digital Twins Definition Language)](https://github.com/Azure/opendigitaltwins-dtdl/tree/master/DTDL)) qui définit une Lune :

:::code language="json" source="~/digital-twins-docs-samples/models/Moon.json":::

Le résultat de l’appel de `object result = await client.GetDigitalTwinAsync("my-moon");` sur un jumeau de type Lune peut se présenter comme suit :

```json
{
  "$dtId": "myMoon-001",
  "$etag": "W/\"e59ce8f5-03c0-4356-aea9-249ecbdc07f9\"",
  "radius": 1737.1,
  "mass": 0.0734,
  "$metadata": {
    "$model": "dtmi:example:Moon;1",
    "radius": {
      "desiredValue": 1737.1,
      "desiredVersion": 5,
      "ackVersion": 4,
      "ackCode": 200,
      "ackDescription": "OK"
    },
    "mass": {
      "desiredValue": 0.0734,
      "desiredVersion": 8,
      "ackVersion": 8,
      "ackCode": 200,
      "ackDescription": "OK"
    }
  }
}
```

Les propriétés définies du jumeau numérique sont retournées en tant que propriétés de niveau supérieur sur le jumeau numérique. Les métadonnées ou les informations système qui ne font pas partie de la définition DTDL sont retournées avec un préfixe `$`. Les propriétés de métadonnées comprennent les valeurs suivantes :
* `$dtId` : L’ID du jumeau numérique dans cette instance d’Azure Digital Twins
* `$etag` : Un champ HTTP standard attribué par le serveur web. Sa valeur est mise à jour chaque fois que le jumeau est mis à jour, ce qui peut être utile pour déterminer si les données du jumeau ont été mises à jour sur le serveur depuis la vérification précédente. Vous pouvez utiliser `If-Match` pour effectuer des mises à jour et des suppressions qui se terminent uniquement si l’Etag de l’entité correspond à celui fourni. Pour plus d’informations sur ces opérations, consultez la documentation relative à [DigitalTwins Update](/rest/api/digital-twins/dataplane/twins/digitaltwins_update) et à [DigitalTwins Delete](/rest/api/digital-twins/dataplane/twins/digitaltwins_delete).
* `$metadata` : Un ensemble d’autres propriétés, notamment :
  - DTMI du modèle du jumeau numérique.
  - État de synchronisation de chaque propriété accessible en écriture. Cela est très utile pour les appareils, où il est possible que le service et l’appareil aient des états différents (par exemple, lorsqu’un appareil est hors connexion). Actuellement, cette propriété s’applique uniquement aux appareils physiques connectés à IoT Hub. Avec les données dans la section des métadonnées, il est possible de comprendre l’état complet d’une propriété, ainsi que les horodatages des dernières modifications. Pour plus d’informations sur l’état de la synchronisation, consultez [ce tutoriel IoT Hub](../iot-hub/tutorial-device-twins.md) sur la synchronisation de l’état de l’appareil.
  - Les métadonnées spécifiques au service, par exemple à partir d’IoT Hub ou d’Azure Digital Twins. 

Pour plus d’informations sur les classes d’assistance de sérialisation comme `BasicDigitalTwin`, consultez [API et SDK Azure Digital Twins](concepts-apis-sdks.md#serialization-helpers).

## <a name="view-all-digital-twins"></a>Supprimer tous les jumeaux numériques

Pour afficher tous les représentations numériques présents dans votre instance, utilisez une [requête](how-to-query-graph.md). Vous pouvez exécuter une requête avec les [API de requête](/rest/api/digital-twins/dataplane/query) ou les [commandes CLI](/cli/azure/dt/twin?view=azure-cli-latest&preserve-view=true#az_dt_twin_query).

Voici le corps de la requête de base qui retourne la liste de tous les jumeaux numériques dans l’instance :

:::code language="sql" source="~/digital-twins-docs-samples/queries/examples.sql" id="GetAllTwins":::

## <a name="update-a-digital-twin"></a>Mettre à jour un jumeau numérique

Pour mettre à jour les propriétés d’un jumeau numérique, vous écrivez les informations que vous souhaitez remplacer au format de [correctif JSON](http://jsonpatch.com/). De cette façon, vous pouvez remplacer plusieurs propriétés à la fois. Vous transmettez ensuite le document de correctif JSON dans une méthode `UpdateDigitalTwin()` :

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="UpdateTwinCall":::

Un appel de correctif peut mettre à jour autant de propriétés que vous le souhaitez (même toutes) sur un seul jumeau. Si vous devez mettre à jour des propriétés sur plusieurs jumeaux, vous aurez besoin d’un appel de mise à jour distinct pour chaque jumeau.

> [!TIP]
> Après la création ou la mise à jour d’un jumeau, il peut y avoir une latence allant jusqu’à 10 secondes avant que les modifications soient reflétées dans les [requêtes](how-to-query-graph.md). L’API `GetDigitalTwin` (décrite [plus loin dans cet article](#get-data-for-a-digital-twin)) ne subit pas ce délai. Utilisez l’appel d’API au lieu d’une interrogation pour voir vos jumeaux mis à jour si vous avez besoin d’une réponse instantanée. 

Voici un exemple de code de correctif JSON. Ce document remplace les valeurs des propriétés *masse* et *rayon* du jumeau numérique auquel il est appliqué.

:::code language="json" source="~/digital-twins-docs-samples/models/patch.json":::

>[!NOTE]
> Cet exemple montre l'opération `replace` JSON Patch, qui remplace la valeur d'une propriété existante. Pour obtenir la liste complète des opérations de correctif JSON qui peuvent être utilisées, y compris `add` et `remove` , consultez [Opérations de JSON Patch](http://jsonpatch.com/#operations). 

Lors de la mise à jour d'un jumeau à partir d'un projet de code utilisant le SDK .NET, vous pouvez créer des correctifs JSON à l'aide du [JsonPatchDocument](/dotnet/api/azure.jsonpatchdocument?view=azure-dotnet&preserve-view=true) du SDK Azure .NET. Voici un exemple.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_other.cs" id="UpdateTwin":::

### <a name="update-sub-properties-in-digital-twin-components"></a>Mettre à jour les sous-propriétés dans les composants des jumeaux numériques

Rappelez-vous qu’un modèle peut contenir des composants, ce qui lui permet d’être constitué d’autres modèles. 

Pour corriger les propriétés dans les composants d’un jumeau numérique, vous pouvez utiliser la syntaxe du chemin dans le correctif JSON :

:::code language="json" source="~/digital-twins-docs-samples/models/patch-component.json":::

### <a name="update-sub-properties-in-object-type-properties"></a>Mettre à jour les sous-propriétés dans les propriétés de type Object

Les modèles peuvent contenir des propriétés qui sont d’un type Object. Ces objets peuvent avoir leurs propres propriétés, et vous pouvez mettre à jour l’une de ces sous-propriétés appartenant à la propriété de type Object. Ce processus est similaire au processus de [mise à jour des sous-propriétés dans les composants](#update-sub-properties-in-digital-twin-components), mais peut nécessiter des étapes supplémentaires. 

Prenons l’exemple d’un modèle avec une propriété de type Object, `ObjectProperty`. `ObjectProperty` a une propriété de chaîne nommée `StringSubProperty`.

Quand un jumeau est créé à l’aide de ce modèle, il n’est pas nécessaire d’instancier l’`ObjectProperty` à ce moment-là. Si la propriété d’objet n’est pas instanciée lors de la création d’un jumeau, aucun chemin par défaut n’est créé pour accéder à `ObjectProperty` et à sa `StringSubProperty` pour une opération de correctif. Vous devez ajouter le chemin à `ObjectProperty` vous-même avant de pouvoir mettre à jour ses propriétés.

Pour ce faire, vous pouvez utiliser une opération de correctif JSON `add`, comme suit :

:::code language="json" source="~/digital-twins-docs-samples/models/patch-object-sub-property-1.json":::

>[!NOTE]
> Si `ObjectProperty` a plusieurs propriétés, vous devez toutes les inclure dans le champ `value` de cette opération, même si vous n’en mettez qu’une seule à jour :
> ```json
>... "value": {"StringSubProperty":"<string-value>", "Property2":"<property2-value>", ...}
>```


Une fois cette opération effectuée, il existe un chemin à `StringSubProperty` et il peut désormais être mis à jour directement avec une opération `replace` ordinaire :

:::code language="json" source="~/digital-twins-docs-samples/models/patch-object-sub-property-2.json":::

Bien que la première étape ne soit pas nécessaire dans les cas où `ObjectProperty` a été instanciée lors de la création du jumeau, nous vous recommandons de l’utiliser chaque fois que vous mettez à jour une sous-propriété pour la première fois, car vous ne serez pas toujours certain que la propriété de l’objet a été instanciée initialement.

### <a name="update-a-digital-twins-model"></a>Mettre à jour le modèle d’un jumeau numérique

La fonction `UpdateDigitalTwin()` peut également être utilisée pour migrer un jumeau numérique vers un modèle différent. 

Par exemple, examinez le document de correctif JSON suivant qui remplace le champ `$model` des métadonnées du jumeau numérique :

:::code language="json" source="~/digital-twins-docs-samples/models/patch-model-1.json":::

Cette opération réussit uniquement si le jumeau numérique modifié par le correctif est conforme au nouveau modèle. 

Prenons l’exemple suivant :
1. Imaginez un jumeau numérique avec un modèle foo_old. foo_old définit une propriété obligatoire : *masse*.
2. Le nouveau modèle foo_new définit une propriété de masse et ajoute une nouvelle propriété obligatoire : *température*.
3. Après le correctif, le jumeau numérique doit avoir une propriété de masse et de température. 

Le correctif pour cette situation doit mettre à jour le modèle et la propriété de température du jumeau, comme suit :

:::code language="json" source="~/digital-twins-docs-samples/models/patch-model-2.json":::

### <a name="handle-conflicting-update-calls"></a>Traiter les appels de mise à jour en conflit

Azure Digital Twins garantit que toutes les requêtes entrantes sont traitées l’une après l’autre. Cela signifie que même si plusieurs fonctions essaient de mettre à jour la même propriété sur un jumeau en même temps, il est **inutile** d’écrire un code de verrouillage explicite pour gérer le conflit.

Ce comportement se fait par jumeau. 

Par exemple, imaginez un scénario dans lequel ces trois appels arrivent en même temps : 
*   Écrire la propriété A sur Twin1
*   Écrire la propriété B sur Twin1
*   Écrire la propriété A sur Twin2

Les deux appels qui modifient Twin1 sont exécutés l’un après l’autre, et les messages de modification sont générés pour chaque modification. L’appel à modifier Twin2 peut être exécuté simultanément sans conflit, dès qu’il arrive.

## <a name="delete-a-digital-twin"></a>Supprimer un jumeau numérique

Vous pouvez supprimer des jumeaux à l’aide de la méthode `DeleteDigitalTwin()`. Toutefois, vous ne pouvez supprimer un jumeau que s’il n’a plus de relations. Ainsi, supprimez d’abord les relations entrantes et sortantes du jumeau.

Voici un exemple de code permettant de supprimer des jumeaux et leurs relations. L’appel du kit SDK à `DeleteDigitalTwin` est mis en surbrillance pour clarifier son emplacement par rapport au contexte de l’exemple.

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs" id="DeleteTwin" highlight="7":::

### <a name="delete-all-digital-twins"></a>Supprimer tous les jumeaux numériques

Pour obtenir un exemple de suppression simultanée de tous les jumeaux numériques, téléchargez l’exemple d’application utilisé dans [Explorer les bases avec un exemple d’application cliente](tutorial-command-line-app.md). Le fichier *CommandLoop.cs* le fait dans une fonction `CommandDeleteAllTwins()`.

## <a name="runnable-digital-twin-code-sample"></a>Exemple de code de jumeau numérique exécutable

Vous pouvez utiliser l’exemple de code exécutable ci-dessous pour créer un jumeau, mettre à jour ses détails et supprimer le jumeau. 

### <a name="set-up-sample-project-files"></a>Configurer des exemples de fichiers de projet

L’extrait de code utilise l’exemple de définition de modèle [Room.json](https://raw.githubusercontent.com/Azure-Samples/digital-twins-samples/master/AdtSampleApp/SampleClientApp/Models/Room.json). Pour **télécharger le fichier de modèle** afin de l’utiliser dans votre code, utilisez ce lien pour accéder directement au fichier dans GitHub. Ensuite, cliquez avec le bouton droit n’importe où sur l’écran, sélectionnez **Enregistrer sous** dans le menu contextuel du navigateur, puis utilisez la fenêtre Enregistrer sous pour enregistrer le fichier sous le nom **Room.json**.

Ensuite, créez un **projet d’application console** dans Visual Studio ou l’éditeur de votre choix.

Puis, **copiez le code suivant** de l’exemple exécutable dans votre projet :

:::code language="csharp" source="~/digital-twins-docs-samples/sdks/csharp/twin_operations_sample.cs":::

### <a name="configure-project"></a>Configurer un projet

Ensuite, effectuez les étapes ci-après pour configurer votre code de projet :
1. Ajoutez le fichier **Room.json** que vous avez téléchargé précédemment à votre projet et remplacez l’espace réservé `<path-to>` dans le code pour indiquer à votre programme où le trouver.
2. Remplacez l’espace réservé `<your-instance-hostname>` par le nom d’hôte de votre instance Azure Digital Twins.
3. Ajoutez deux dépendances à votre projet. Elles seront nécessaires pour travailler avec Azure Digital Twins. La première correspond au package pour le [SDK Azure Digital Twins pour .NET](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true), et la seconde fournit des outils facilitant l’authentification auprès d’Azure.

      ```cmd/sh
      dotnet add package Azure.DigitalTwins.Core
      dotnet add package Azure.Identity
      ```

Vous devez également configurer des informations d’identification locales si vous souhaitez exécuter l’exemple directement. La section suivante décrit cette procédure.
[!INCLUDE [Azure Digital Twins: local credentials prereq (outer)](../../includes/digital-twins-local-credentials-outer.md)]

### <a name="run-the-sample"></a>Exécution de l'exemple

Vous avez terminé la configuration. Vous pouvez à présent exécuter l’exemple de projet de code.

Voici la sortie de la console du programme ci-dessus : 

:::image type="content" source="./media/how-to-manage-twin/console-output-manage-twins.png" alt-text="Capture d’écran de la sortie de la console indiquant que le jumeau est créé, mis à jour et supprimé." lightbox="./media/how-to-manage-twin/console-output-manage-twins.png":::

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment créer et gérer des relations entre vos jumeaux numériques :
* [Gérer le graphe de jumeaux et les relations](how-to-manage-graph.md)