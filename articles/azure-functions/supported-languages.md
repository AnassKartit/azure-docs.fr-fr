---
title: Langages pris en charge dans Azure Functions
description: Découvrez les langues prises en charge (GA) et celles qui sont en préversion, ainsi que les méthodes d’extension du développement des fonctions dans d’autres langages.
ms.topic: conceptual
ms.date: 11/27/2019
ms.openlocfilehash: 91dcd26895ad41d79606458287f6aabab9594f79
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2021
ms.locfileid: "130224302"
---
# <a name="supported-languages-in-azure-functions"></a>Langages pris en charge dans Azure Functions

Cet article explique les niveaux de prise en charge offerts pour les langages que vous pouvez utiliser avec Azure Functions. Il décrit également les stratégies de création de fonctions à l’aide de langages non pris en charge en mode natif.

[!INCLUDE [functions-support-levels](../../includes/functions-support-levels.md)]

## <a name="languages-by-runtime-version"></a>Langues par version du runtime 

[Plusieurs versions du runtime Azure Functions](functions-versions.md) sont disponibles. Le tableau suivant montre les langages qui sont pris en charge dans chaque version du runtime.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

[!INCLUDE [functions-portal-language-support](../../includes/functions-portal-language-support.md)]

### <a name="language-major-version-support"></a>Prise en charge de la version principale de langage

Azure Functions offre une garantie de prise en charge des versions principales des langages de programmation pris en charge. Pour la plupart des langages, des versions mineures ou correctives sont publiées pour mettre à jour une version principale prise en charge. Exemples de versions mineures ou correctives : Python 3.9.1 et Node 14.17. Une fois les nouvelles versions mineures des langages pris en charge disponibles, les versions mineures utilisées par vos applications de fonction sont automatiquement mises à niveau vers ces versions mineures ou correctives plus récentes. 

> [!NOTE]
>Étant donné qu’Azure Functions peut supprimer la prise en charge des anciennes versions mineures à tout moment après lorsqu’une nouvelle version mineure devient disponible, vous ne devez pas lier vos applications de fonction à une version mineure/corrective spécifique d’un langage de programmation.  
>

## <a name="custom-handlers"></a>Gestionnaires personnalisés

Les gestionnaires personnalisés sont des serveurs web légers qui reçoivent des événements de l’hôte Azure Functions. Tout langage qui prend en charge les primitives HTTP peut implémenter un gestionnaire personnalisé. Cela signifie que les gestionnaires personnalisés peuvent être utilisés pour créer des fonctions dans des langages qui ne sont pas officiellement prises en charge. Pour en savoir plus, consultez [Gestionnaires personnalisés Azure Functions](functions-custom-handlers.md).

## <a name="language-extensibility"></a>Extensibilité de langage

À compter de la version 2.x, le runtime est conçu pour offrir une [extensibilité de langage](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility). Les langages JavaScript et Java dans le runtime 2.x sont générés avec cette extensibilité.

## <a name="next-steps"></a>Étapes suivantes

Pour savoir plus en détails comment développer des fonctions dans les langages pris en charge, voir les ressources suivantes :

+ [Informations de référence pour les développeurs de bibliothèques de classes C#](functions-dotnet-class-library.md)
+ [Informations de référence pour les développeurs de scripts C#](functions-reference-csharp.md)
+ [Informations de référence pour les développeurs Java](functions-reference-java.md)
+ [Informations de référence pour les développeurs JavaScript](functions-reference-node.md)
+ [Informations de référence pour les développeurs PowerShell](functions-reference-powershell.md)
+ [Informations de référence pour les développeurs Python](functions-reference-python.md)
+ [Informations de référence pour les développeurs TypeScript](functions-reference-node.md#typescript)
