---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 0198bcdd34cd92822c779329affeb1a405264bae
ms.sourcegitcommit: 16e25fb3a5fa8fc054e16f30dc925a7276f2a4cb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2021
ms.locfileid: "122830509"
---
## <a name="configure-your-local-environment"></a>Configurer votre environnement local

Avant de commencer la lecture cet article, vous devez disposer des éléments suivants :

::: zone pivot="programming-language-csharp"
[!INCLUDE [functions-cli-dotnet-prerequisites](functions-cli-dotnet-prerequisites.md)]
::: zone-end  
::: zone pivot="programming-language-java,programming-language-javascript,programming-language-typescript,programming-language-powershell,programming-language-python,programming-language-other" 
+ [Azure Functions Core Tools](../articles/azure-functions/functions-run-local.md#v2).

+ [Azure CLI](/cli/azure/install-azure-cli), version 2.4 ou ultérieure. 
:::zone-end  
::: zone pivot="programming-language-javascript,programming-language-typescript"
+ [Node.js](https://nodejs.org/), Active LTS et Maintenance LTS (versions 8.11.1 et 10.14.1 recommandées).
::: zone-end

::: zone pivot="programming-language-python"
+ [Python 3.8 (64 bits)](https://www.python.org/downloads/release/python-382/), [Python 3.7 (64 bits)](https://www.python.org/downloads/release/python-375/), [Python 3.6 (64 bits)](https://www.python.org/downloads/release/python-368/), qui sont pris en charge par Azure Functions. 
::: zone-end
::: zone pivot="programming-language-powershell"
+ [SDK .NET Core 3.1](https://dotnet.microsoft.com/download)
::: zone-end
::: zone pivot="programming-language-java"  
+ [Java Developer Kit (JDK)](/azure/developer/java/fundamentals/java-jdk-long-term-support) version 8 ou 11. 

+ [Apache Maven](https://maven.apache.org) version 3.0 ou ultérieure.

::: zone-end
::: zone pivot="programming-language-other"
+ Outils de développement pour le langage que vous utilisez. Ce tutoriel utilise le [langage de programmation R](https://www.r-project.org/) à titre d’exemple.
::: zone-end

Vous avez aussi besoin d’un compte Azure avec un abonnement actif. [Créez un compte gratuitement](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).