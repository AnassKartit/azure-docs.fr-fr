---
title: 'Démarrage rapide : Bibliothèque de client QnA Maker pour Node.js'
description: Ce guide de démarrage rapide montre comment commencer avec la bibliothèque cliente QnA Maker pour Node.js.
ms.topic: quickstart
ms.date: 06/18/2020
ms.custom: devx-track-js
ms.openlocfilehash: d394928df4abae926922e075ae00e7547a17b08e
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2021
ms.locfileid: "130287415"
---
# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

Utilisez la bibliothèque de client QnA Maker pour Node.js afin de :

* Créer une base de connaissances
* Mettre à jour une base de connaissances
* Publier une base de connaissances
* Obtenir la clé de point de terminaison du runtime de prédiction
* Attendre l’exécution d’une tâche de longue durée
* Télécharger une base de connaissances
* Obtenir une réponse d’une base de connaissances
* Supprimer une base de connaissances

[Documentation de référence](/javascript/api/@azure/cognitiveservices-qnamaker/) | [Code source de la bibliothèque](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-qnamaker) | [Package (npm)](https://www.npmjs.com/package/@azure/cognitiveservices-qnamaker) | [Exemples Node.js](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js)

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

Utilisez la bibliothèque de client QnA Maker pour Node.js afin de :

* Créer une base de connaissances
* Mettre à jour une base de connaissances
* Publier une base de connaissances
* Attendre l’exécution d’une tâche de longue durée
* Télécharger une base de connaissances
* Obtenir une réponse d’une base de connaissances
* Supprimer une base de connaissances

[Documentation de référence](/javascript/api/@azure/cognitiveservices-qnamaker/) | [Code source de la bibliothèque](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-qnamaker) | [Package (npm)](https://www.npmjs.com/package/@azure/cognitiveservices-qnamaker) | [Exemples Node.js](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/preview-sdk/quickstart.js)

---

[!INCLUDE [Custom subdomains notice](../../../../includes/cognitive-services-custom-subdomains-note.md)]

## <a name="prerequisites"></a>Prérequis

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

* Abonnement Azure - [En créer un gratuitement](https://azure.microsoft.com/free/cognitive-services)
* Version actuelle de [Node.js](https://nodejs.org).
* Une fois en possession de votre abonnement Azure, créez une [ressource QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) sur le portail Azure pour obtenir vos clé et ressource de création. À la fin du déploiement, sélectionnez **Accéder à la ressource**.
    * Vous aurez besoin de la clé et du nom de la ressource que vous créez pour connecter votre application à l’API QnA Maker. Vous collerez la clé et le nom de la ressource dans le code ci-dessous plus tard dans le guide de démarrage rapide.
    * Vous pouvez utiliser le niveau tarifaire Gratuit (`F0`) pour tester le service, puis passer par la suite à un niveau payant pour la production.

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

* Abonnement Azure - [En créer un gratuitement](https://azure.microsoft.com/free/cognitive-services)
* Version actuelle de [Node.js](https://nodejs.org).
* Les questions et réponses personnalisées nécessitent une [ressource Analyse de texte](https://ms.portal.azure.com/?quickstart=true#create/Microsoft.CognitiveServicesTextAnalytics) avec la fonctionnalité de réponses aux questions personnalisées activée pour générer une clé d’API et un point de terminaison.
     * Après le déploiement de votre ressource Analyse de texte, sélectionnez **Accéder à la ressource**. Vous aurez besoin de la clé et du point de terminaison de la ressource que vous créez pour connecter votre application à l’API QnA Maker. Vous collerez votre clé et votre point de terminaison dans le code ci-dessous plus loin dans le guide de démarrage rapide.

---

## <a name="setting-up"></a>Configuration

### <a name="create-a-new-nodejs-application"></a>Création d’une application Node.js

Dans une fenêtre de console (telle que cmd, PowerShell ou bash), créez un répertoire pour votre application et accédez-y.

```console
mkdir qnamaker_quickstart && cd qnamaker_quickstart
```

Exécutez la commande `npm init -y` pour créer une application de nœud avec un fichier `package.json`.

```console
npm init -y
```

### <a name="install-the-client-library"></a>Installer la bibliothèque de client

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

Installez les packages NPM suivants :

```console
npm install @azure/cognitiveservices-qnamaker
npm install @azure/cognitiveservices-qnamaker-runtime
npm install @azure/ms-rest-js
```

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

Installez les packages NPM suivants :

```console
npm install @azure/cognitiveservices-qnamaker
npm install @azure/ms-rest-js
```

---

Le fichier `package.json` de votre application est mis à jour avec les dépendances.

Créez un fichier nommé index.js et importez les bibliothèques suivantes :

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

[!code-javascript[Dependencies](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=Dependencies)]

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

[!code-javascript[Dependencies](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=Dependencies)]

---

Créez une variable pour le nom de ressource et la clé Azure de votre ressource.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

- Nous utilisons une clé d’abonnement et une clé de création de manière interchangeable. Pour plus d’informations sur la clé de création, consultez [Clés dans QnA Maker](../concepts/azure-resources.md?tabs=v1#keys-in-qna-maker).

- Le format de la valeur de QNA_MAKER_ENDPOINT est `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`. Accédez au portail Azure, puis recherchez la ressource QnA Maker que vous avez créée à l’étape des prérequis. Cliquez sur la page **Clés et point de terminaison**, sous **Gestion des ressources** pour localiser la clé de création (abonnement) et le point de terminaison QnA Maker.

 ![Point de terminaison de création QnA Maker](../media/keys-endpoint.png)

- Le format de la valeur de QNA_MAKER_RUNTIME_ENDPOINT est `https://YOUR-RESOURCE-NAME.azurewebsites.net`. Accédez au portail Azure, puis recherchez la ressource QnA Maker que vous avez créée à l’étape des prérequis. Cliquez sur la page **Exporter le modèle**, sous **Automation** pour localiser le point de terminaison du Runtime.

 ![Point de terminaison du runtine QnA Maker](../media/runtime-endpoint.png)
   
- Pour la production, envisagez d’utiliser une méthode de stockage et d’accès sécurisée pour vos informations d’identification. Par exemple, [Azure Key Vault](../../../key-vault/general/overview.md) fournit un stockage de clé sécurisé.

[!code-javascript[Set the resource key and resource name](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=Resourcevariables)]

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

- Nous utilisons une clé d’abonnement et une clé de création de manière interchangeable. Pour plus d’informations sur la clé de création, consultez [Clés](../concepts/azure-resources.md?tabs=v2#keys-in-qna-maker).

- Le format de la valeur de QNA_MAKER_ENDPOINT est `https://YOUR-RESOURCE-NAME.cognitiveservices.azure.com`. Accédez au portail Azure, puis recherchez la ressource Analyse de texte que vous avez créée à l’étape des prérequis. Cliquez sur la page **Clés et point de terminaison**, sous **Gestion des ressources** pour trouver la clé de création (abonnement) et le point de terminaison.

> [!div class="mx-imgBorder"]
> ![Point de terminaison de création de questions-réponses personnalisées](../media/qnamaker-how-to-key-management/custom-qna-keys-and-endpoint.png)

- Pour la production, envisagez d’utiliser une méthode de stockage et d’accès sécurisée pour vos informations d’identification. Par exemple, [Azure Key Vault](../../../key-vault/general/overview.md) fournit un stockage de clé sécurisé.

[!code-javascript[Set the resource key and resource name](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=Resourcevariables)]

---

## <a name="object-models"></a>Modèles objet

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

[QnA Maker](/javascript/api/@azure/cognitiveservices-qnamaker/) utilise deux modèles objet différents :
* **[QnAMakerClient](#qnamakerclient-object-model)** est l’objet utilisé pour créer, gérer, publier et télécharger la base de connaissances.
* **[QnAMakerRuntime](#qnamakerruntimeclient-object-model)** est l’objet utilisé pour interroger la base de connaissances avec l’API GenerateAnswer et envoyer de nouvelles questions suggérées avec l’API Train (dans le cadre de l’[apprentissage actif](../how-to/use-active-learning.md)).

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

[QnA Maker](/javascript/api/@azure/cognitiveservices-qnamaker/) utilise le modèle objet suivant :
* **[QnAMakerClient](#qnamakerclient-object-model)** est l’objet utilisé pour créer, gérer, publier, télécharger et interroger la base de connaissances.

---

### <a name="qnamakerclient-object-model"></a>Modèle objet QnAMakerClient

Le client QnA Maker de création est un objet [QnAMakerClient](/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient) qui s’authentifie auprès d’Azure à l’aide de vos informations d’identification, qui contiennent votre clé.

Une fois le client créé, utilisez la propriété [knowledgebase](/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient#knowledgebase) pour créer, gérer et publier votre base de connaissances.

Gérez votre base de connaissances en envoyant un objet JSON. Pour les opérations immédiates, une méthode retourne généralement un objet JSON indiquant l’état. Pour les opérations de longue durée, la réponse est l’ID d’opération. Appelez la méthode [client.operations.getDetails](/javascript/api/@azure/cognitiveservices-qnamaker/operations#getdetails-string--msrest-requestoptionsbase-) avec l’ID d’opération pour déterminer l’[état de la requête](/javascript/api/@azure/cognitiveservices-qnamaker/operation).

### <a name="qnamakerruntimeclient-object-model"></a>Modèle objet QnAMakerRuntimeClient

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

Le client QnA Maker de prédiction est un objet QnAMakerRuntimeClient qui s’authentifie auprès d’Azure à l’aide de Microsoft.Rest.ServiceClientCredentials, qui contient la clé du runtime de prédiction retournée par l’appel du client de création, [client.EndpointKeys.getKeys](/javascript/api/@azure/cognitiveservices-qnamaker/endpointkeys#getkeys-msrest-requestoptionsbase-), après la publication de la base de connaissances.

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

La réponse à une question personnalisée ne nécessite pas l’utilisation de l’objet QnAMakerRuntimeClient. Vous appelez plutôt [generateAnswer](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#generateAnswer_string__QueryDTO__msRest_RequestOptionsBase_) directement sur l’objet [QnAMakerClient](/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient).

---

## <a name="code-examples"></a>Exemples de code

Ces extraits de code montrent comment effectuer les opérations suivantes avec la bibliothèque de client QnA Maker pour .NET :

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

* [Authentifier le client de création](#authenticate-the-client-for-authoring-the-knowledge-base)
* [Créer une base de connaissances](#create-a-knowledge-base)
* [Mettre à jour une base de connaissances](#update-a-knowledge-base)
* [Télécharger une base de connaissances](#download-a-knowledge-base)
* [Publier une base de connaissances](#publish-a-knowledge-base)
* [Supprimer une base de connaissances](#delete-a-knowledge-base)
* [Obtenir la clé du runtime de requête](#get-query-runtime-key)
* [Obtenir l’état d’une opération](#get-status-of-an-operation)
* [Authentifier le client du runtime de requête](#authenticate-the-runtime-for-generating-an-answer)
* [Générer une réponse à partir de la base de connaissances](#generate-an-answer-from-the-knowledge-base)

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

* [Authentifier le client de création](#authenticate-the-client-for-authoring-the-knowledge-base)
* [Créer une base de connaissances](#create-a-knowledge-base)
* [Mettre à jour une base de connaissances](#update-a-knowledge-base)
* [Télécharger une base de connaissances](#download-a-knowledge-base)
* [Publier une base de connaissances](#publish-a-knowledge-base)
* [Supprimer une base de connaissances](#delete-a-knowledge-base)
* [Obtenir l’état d’une opération](#get-status-of-an-operation)
* [Générer une réponse à partir de la base de connaissances](#generate-an-answer-from-the-knowledge-base)

---

## <a name="authenticate-the-client-for-authoring-the-knowledge-base"></a>Authentifier le client pour la création de la base de connaissances

Instanciez un client avec votre point de terminaison et la clé. Créez un objet ServiceClientCredentials avec votre clé et utilisez-le avec votre point de terminaison pour créer un objet [QnAMakerClient](/javascript/api/@azure/cognitiveservices-qnamaker/qnamakerclient).

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

[!code-javascript[Create QnAMakerClient object with key and endpoint](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=AuthorizationAuthor)]

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

[!code-javascript[Create QnAMakerClient object with key and endpoint](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=AuthorizationAuthor)]

---

## <a name="create-a-knowledge-base"></a>Créer une base de connaissances

Une base de connaissances stocke des paires de questions et réponses pour l’objet [CreateKbDTO](/javascript/api/@azure/cognitiveservices-qnamaker/createkbdto) à partir de trois sources :

* Pour le **contenu éditorial**, utilisez l'objet [QnADTO](/javascript/api/@azure/cognitiveservices-qnamaker/qnadto).
    * Pour utiliser les métadonnées et les invites de suivi, utilisez le contexte éditorial, car cette donnée est ajoutée au niveau de chaque paire question/réponse.
* Pour les **fichiers**, utilisez l'objet [FileDTO](/javascript/api/@azure/cognitiveservices-qnamaker/filedto). FileDTO contient le nom de fichier ainsi que l’URL publique d’accès au fichier.
* Pour les **URL**, utilisez une liste de chaînes pour représenter les URL disponibles publiquement.

L’étape de création comprend également les propriétés de la base de connaissances :
* `defaultAnswerUsedForExtraction` : données retournées quand aucune réponse n’est trouvée
* `enableHierarchicalExtraction` : créez automatiquement des relations d’invite entre les paires question/réponse extraites
* `language` : quand vous créez la première base de connaissances d’une ressource, définissez le langage à utiliser dans l’index Recherche Azure.

Appelez la méthode [create](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#create-createkbdto--servicecallback-operation--) avec les informations de la base de connaissances. Ces informations sont essentiellement un objet JSON.

Au retour de la méthode create, passez l’ID d’opération retourné à la méthode [wait_for_operation](#get-status-of-an-operation) pour interroger l’état. La méthode wait_for_operation est retournée à la fin de l’opération. Analysez la valeur d’en-tête `resourceLocation` de l’opération retournée pour obtenir le nouvel ID de la base de connaissances.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

[!code-javascript[Create knowledgebase](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=CreateKBMethod)]

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

[!code-javascript[Create knowledgebase](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=CreateKBMethod)]

---

Veillez à inclure la fonction [`wait_for_operation`](#get-status-of-an-operation), référencée dans le code ci-dessus, afin de créer une base de connaissances.

## <a name="update-a-knowledge-base"></a>Mettre à jour une base de connaissances

Vous pouvez mettre à jour une base de connaissances en passant l’ID de celle-ci et un [UpdateKbOperationDTO](/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto) contenant des objets DTO [add](/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto#add), [update](/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto#update) et [delete](/javascript/api/@azure/cognitiveservices-qnamaker/updatekboperationdto#deleteproperty) à la méthode [update](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#update-string--updatekboperationdto--msrest-requestoptionsbase-). Les objets DTO sont également essentiellement des objets JSON. Utilisez la méthode [wait_for_operation](#get-status-of-an-operation) pour déterminer si la mise à jour a réussi.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

[!code-javascript[Update a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=UpdateKBMethod)]

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

[!code-javascript[Update a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=UpdateKBMethod)]

---

Veillez à inclure la fonction [`wait_for_operation`](#get-status-of-an-operation), référencée dans le code ci-dessus, afin de mettre à jour une base de connaissances.

## <a name="download-a-knowledge-base"></a>Télécharger une base de connaissances

Utilisez la méthode [download](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#download-string--models-environmenttype--msrest-requestoptionsbase-) pour télécharger la base de données sous la forme d’une liste de [QnADocumentsDTO](/javascript/api/@azure/cognitiveservices-qnamaker/qnadocumentsdto). Ceci n'est _pas_ équivalent à l’exportation à partir de la page **Paramètres** du portail QnA Maker, car le résultat de cette méthode n’est pas un fichier TSV.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

[!code-javascript[Download a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=DownloadKB)]

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

[!code-javascript[Download a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=DownloadKB)]

---

## <a name="publish-a-knowledge-base"></a>Publier une base de connaissances

Publiez la base de connaissances à l’aide de la méthode [publish](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#publish-string--msrest-requestoptionsbase-). Celle-ci prend le modèle actuel enregistré et entraîné, référencé par l’ID de base de connaissances, et le publie sur un point de terminaison. Examinez le code de réponse HTTP pour vérifier que la publication a abouti.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

[!code-javascript[Publish a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=PublishKB)]

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

[!code-javascript[Publish a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=PublishKB)]

---

## <a name="query-a-knowledge-base"></a>Interroger une base de connaissances

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

### <a name="get-query-runtime-key"></a>Obtenir la clé du runtime de requête

Une fois que la base de connaissances a été publiée, vous avez besoin de la clé du runtime de requête pour interroger le runtime. Il ne s’agit pas de la même clé que celle utilisée pour créer l’objet client d’origine.

Utilisez la méthode [EndpointKeys.getKeys](/javascript/api/@azure/cognitiveservices-qnamaker/endpointkeys) pour obtenir la classe [EndpointKeysDTO](/javascript/api/@azure/cognitiveservices-qnamaker/endpointkeysdto).

Utilisez l’une des propriétés de clé retournées dans l’objet pour interroger la base de connaissances.

[!code-javascript[Get query runtime key](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=GetQueryEndpointKey)]

### <a name="authenticate-the-runtime-for-generating-an-answer"></a>Authentifier le runtime pour générer une réponse

Créez un QnAMakerRuntimeClient pour interroger la base de connaissances afin de générer une réponse ou d’effectuer un entraînement par le biais de l’apprentissage actif.

[!code-javascript[Authenticate the runtime](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=AuthorizationQuery)]

Utilisez QnAMakerRuntimeClient pour obtenir une réponse de la base de connaissances ou pour envoyer de nouvelles questions suggérées à la base de connaissances en vue de l’[apprentissage actif](../index.yml).

### <a name="generate-an-answer-from-the-knowledge-base"></a>Générer une réponse à partir de la base de connaissances

Générez une réponse à partir d’une base de connaissances publiée à l’aide de la méthode RuntimeClient.runtime.generateAnswer. Cette méthode accepte l’ID de la base de connaissances et [QueryDTO](/javascript/api/@azure/cognitiveservices-qnamaker/querydto). Accédez à des propriétés supplémentaires de QueryDTO comme Top et Context, que vous pouvez utiliser dans votre chat bot.

[!code-javascript[Generate an answer from a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=GenerateAnswer)]

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

### <a name="generate-an-answer-from-the-knowledge-base"></a>Générer une réponse à partir de la base de connaissances

Générez une réponse à partir d’une base de connaissances publiée à l’aide de la méthode QnAMakerClient.knowledgebase.generateAnswer. Cette méthode accepte l’ID de la base de connaissances et [QueryDTO](/javascript/api/@azure/cognitiveservices-qnamaker/querydto). Accédez à des propriétés supplémentaires de QueryDTO comme Top et Context, que vous pouvez utiliser dans votre chat bot.

[!code-javascript[Generate an answer from a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=GenerateAnswer)]

---

Il s’agit d’un exemple simple d’interrogation de la base de connaissances. Pour comprendre les scénarios d’interrogation avancés, consultez ces [autres exemples de requêtes](../quickstarts/get-answer-from-knowledge-base-using-url-tool.md?pivots=url-test-tool-curl#use-curl-to-query-for-a-chit-chat-answer).

## <a name="delete-a-knowledge-base"></a>Supprimer une base de connaissances

Supprimez la base de connaissances à l’aide de la méthode [delete](/javascript/api/@azure/cognitiveservices-qnamaker/knowledgebase#deletemethod-string--msrest-requestoptionsbase-) avec comme paramètre l’ID de la base de connaissances.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

[!code-javascript[Delete a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=DeleteKB)]

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

[!code-javascript[Delete a knowledge base](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=DeleteKB)]

---

## <a name="get-status-of-an-operation"></a>Obtenir l’état d’une opération

Certaines méthodes, telles que Create et Update, peuvent prendre tellement de temps qu’au lieu d’attendre que le processus se termine, une [opération](/javascript/api/@azure/cognitiveservices-qnamaker/operations) est retournée. Utilisez l'[ID de l’opération](/javascript/api/@azure/cognitiveservices-qnamaker/operation#operationid) pour interroger (avec une logique de nouvelle tentative) afin de déterminer l’état de la méthode d’origine.

L’appel _delayTimer_ dans le bloc de code suivant est utilisé pour simuler la logique de nouvelle tentative. Remplacez cette logique par votre propre logique de nouvelle tentative.

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

[!code-javascript[Monitor an operation](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/qnamaker_quickstart.js?name=MonitorOperation)]

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

[!code-javascript[Monitor an operation](~/cognitive-services-quickstart-code/javascript/QnAMaker/sdk/preview-sdk/quickstart.js?name=MonitorOperation)]

---

## <a name="run-the-application"></a>Exécution de l'application

Exécutez l’application avec la commande `node index.js` à partir de votre répertoire d’application.

```console
node index.js
```

# <a name="qna-maker-ga-stable-release"></a>[QnA Maker GA (version stable)](#tab/version-1)

Le code source de cet exemple est disponible sur [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/qnamaker_quickstart.js).

# <a name="custom-question-answering-preview-release"></a>[Réponses aux questions personnalisées (préversion)](#tab/version-2)

Le code source de cet exemple est disponible sur [GitHub](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/QnAMaker/sdk/preview-sdk/quickstart.js).

---