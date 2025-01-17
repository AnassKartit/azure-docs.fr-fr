---
title: 'Démarrage rapide : Bibliothèque de client de gestion Azure pour Python'
description: Dans ce guide de démarrage rapide, vous allez vous familiariser avec la bibliothèque de client de gestion Azure pour Python.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/04/2021
ms.author: pafarley
ms.openlocfilehash: 4a44e65a4cda79a07c3fb94231945aa1dd1654e3
ms.sourcegitcommit: 1deb51bc3de58afdd9871bc7d2558ee5916a3e89
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/19/2021
ms.locfileid: "122442295"
---
[Documentation de référence](/python/api/azure-mgmt-cognitiveservices/azure.mgmt.cognitiveservices) | [Code source de la bibliothèque](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-mgmt-cognitiveservices) | [Package (PyPi)](https://pypi.org/project/azure-mgmt-cognitiveservices/) | [Exemples](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-mgmt-cognitiveservices/tests)

## <a name="python-prerequisites"></a>Configuration Python requise

* Un abonnement Azure valide - [Créer un abonnement gratuitement](https://azure.microsoft.com/free/)
* [Python 3.x](https://www.python.org/)
* [!INCLUDE [contributor-requirement](./contributor-requirement.md)]
* [!INCLUDE [terms-azure-portal](./terms-azure-portal.md)]

[!INCLUDE [Create a service principal](./create-service-principal.md)]

[!INCLUDE [Create a resource group](./create-resource-group.md)]

## <a name="create-a-new-python-application"></a>Créer une application Python

Créez une application Python dans votre éditeur ou IDE préféré et accédez à votre projet dans une fenêtre de console.

### <a name="install-the-client-library"></a>Installer la bibliothèque de client

Vous pouvez installer la bibliothèque de client avec :

```console
pip install azure-mgmt-cognitiveservices
```

Si vous utilisez l’IDE Visual Studio, la bibliothèque de client est disponible sous forme de package NuGet téléchargeable.

### <a name="import-libraries"></a>Importer des bibliothèques

Ouvrez votre script Python et importez les bibliothèques suivantes.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_imports)]

## <a name="authenticate-the-client"></a>Authentifier le client

Ajoutez les champs suivants à la racine de votre script et complétez-les avec les valeurs appropriées à l'aide du principal de service que vous avez créé et des informations de votre compte Azure.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_constants)]

Ajoutez ensuite le code suivant pour générer un objet **CognitiveServicesManagementClient**. Cet objet est nécessaire pour toutes vos opérations de gestion Azure.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_auth)]

## <a name="create-a-cognitive-services-resource-python"></a>Créer une ressource Cognitive Services (Python)

Pour créer une ressource Cognitive Services et s'y abonner, utilisez la fonction **Create**. Cette fonction ajoute une nouvelle ressource facturable au groupe de ressources que vous transmettez. Lorsque vous créez votre nouvelle ressource, vous devez connaître le « type » du service que vous souhaitez utiliser, ainsi que son niveau tarifaire (ou référence SKU) et un emplacement Azure. La fonction suivante utilise tous ces arguments et crée une ressource.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_create)]

### <a name="choose-a-service-and-pricing-tier"></a>Choisir un service et son niveau tarifaire

Lorsque vous créez une nouvelle ressource, vous devez connaître le « type » du service que vous souhaitez utiliser, ainsi que le [niveau tarifaire](https://azure.microsoft.com/pricing/details/cognitive-services/) (ou référence SKU) souhaité. Ces informations et d'autres vous serviront de paramètres lors de la création de la ressource. La fonction suivante répertorie les « types » de ressources Cognitive Services disponibles.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_list_avail)]

[!INCLUDE [cognitive-services-subscription-types](../../../../includes/cognitive-services-subscription-types.md)]

[!INCLUDE [SKUs and pricing](./sku-pricing.md)]

## <a name="view-your-resources"></a>Afficher vos ressources

Pour afficher toutes les ressources disponibles sous votre compte Azure (dans tous les groupes de ressources), utilisez la fonction suivante :

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_list)]

## <a name="delete-a-resource"></a>Supprimer une ressource

La fonction suivante supprime la ressource spécifiée du groupe de ressources donné.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_delete)]

Si vous devez récupérer une ressource supprimée, consultez [Récupérer des ressources Cognitive Services supprimées](../../manage-resources.md).

## <a name="call-management-functions"></a>Appeler les fonctions de gestion

Ajoutez le code suivant en bas de votre script pour appeler les fonctions ci-dessus. Ce code répertorie les ressources disponibles, crée un exemple de ressource, répertorie les ressources que vous possédez, puis supprime l'exemple de ressource.

[!code-python[](~/cognitive-services-quickstart-code/python/azure_management_service/create_delete_resource.py?name=snippet_calls)]

## <a name="run-the-application"></a>Exécution de l'application

Exécutez votre application à partir de la ligne de commande avec la commande `python`.

```console
python <your-script-name>.py
```

## <a name="see-also"></a>Voir aussi

* Consultez **[Authentifier des requêtes auprès d’Azure Cognitive Services](../../authentication.md)** qui explique la façon de travailler en toute sécurité avec Cognitive Services.
* Consultez **[Présentation d’Azure Cognitive Services](../../what-are-cognitive-services.md)** pour obtenir la liste des différentes catégories présentes dans Cognitive Services.
* Consultez **[Prise en charge du langage naturel](../../language-support.md)** pour afficher la liste des langages naturels pris en charge par Cognitive Services.
* Consultez **[Utiliser Cognitive Services en tant que conteneurs](../../cognitive-services-container-support.md)** pour comprendre comment utiliser Cognitive Services en local.
* Consultez **[Planifier et gérer les coûts pour Cognitive Services](../../plan-manage-costs.md)** afin d’estimer le coût d’utilisation de Cognitive Services.
* Pour plus d’informations sur le kit de développement logiciel (SDK) de gestion, consultez la **[documentation de référence du kit de développement logiciel (SDK) de gestion Azure](/python/api/azure-mgmt-cognitiveservices/azure.mgmt.cognitiveservices)** .
