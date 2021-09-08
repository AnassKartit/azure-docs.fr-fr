---
title: Services IoT Azure Allemagne | Microsoft Docs
description: Cet article fournit un point de départ pour Azure IoT Suite pour Azure Allemagne.
ms.topic: article
ms.date: 10/16/2020
author: gitralf
ms.author: ralfwi
ms.service: germany
ms.custom: bfdocs
ms.openlocfilehash: 16259a816413bcf7f57475b12f46c9410cb3ca44
ms.sourcegitcommit: a055089dd6195fde2555b27a84ae052b668a18c7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "122562093"
---
# <a name="azure-germany-iot-services"></a>Services IoT Azure Allemagne

[!INCLUDE [closureinfo](../../includes/germany-closure-info.md)]

## <a name="iot-solution-accelerators"></a>Accélérateurs de solution IoT
Tous les services requis pour Azure IoT Suite sont disponibles dans Azure Allemagne. 

### <a name="variations"></a>Variantes
La page d’accueil d’Azure IoT Suite dans Azure Allemagne est différente de la page dans Azure global.

## <a name="solution-accelerators"></a>Accélérateurs de solution
Vous souhaiterez peut-être commencer par un des accélérateurs de solution suivants. 

### <a name="remote-monitoring"></a>Surveillance à distance
L’accélérateur de solution de surveillance à distance est une implémentation d’une solution de supervision de bout en bout destinée à plusieurs ordinateurs distants. Combinant les principaux services Azure pour fournir une implémentation générique du scénario d’entreprise. Vous pouvez utiliser la solution comme point de départ pour votre propre implémentation et la personnaliser pour répondre à vos propres exigences professionnelles.

### <a name="predictive-maintenance"></a>Maintenance prédictive
L’accélérateur de solution Maintenance prédictive est une solution de bout en bout pour un scénario d’entreprise qui prévoit le moment auquel une défaillance est susceptible de se produire. Vous pouvez utiliser cette solution de manière proactive pour des activités telles que l’optimisation de la maintenance. La solution combine des services Azure IoT Suite clés, comme Azure IoT Hub, Stream Analytics et un espace de travail de Machine Learning. Cet espace de travail contient un modèle basé sur un échantillon de données publiques, permettant de prédire la durée de vie utile restante (VUR) d’un moteur d’avion. La solution implémente complètement le scénario professionnel IoT comme point de départ, pour vous permettre de planifier et de mettre en œuvre une solution qui réponde à vos besoins professionnels.


## <a name="deploying-the-solution-accelerator"></a>Déployer l’accélérateur de solution

Les deux solutions peuvent être déployées de deux manières, via le site Web ou via PowerShell.

### <a name="deploy-via-website"></a>Déployer via le site Web

Suivez les instructions du [tutoriel pour les solutions préconfigurées](/previous-versions/azure/iot-accelerators/about-iot-accelerators) à l’aide de la page d’accueil mentionnée précédemment.

### <a name="deploy-via-powershell"></a>Déployer avec PowerShell

Il existe une version complète (utilisant des modèles Azure Resource Manager et Visual Studio) pour la solution de *surveillance à distance*. Effectuez le téléchargement à partir du [référentiel Azure-IoT-Remote-Monitoring sur GitHub](https://github.com/Azure/azure-iot-remote-monitoring). Le déploiement de PowerShell est prêt pour d’autres environnements, tels que Azure Allemagne. Indiquez le paramètre d’*environnement* « AzureGermanCloud » afin qu’il ressemble à ce qui suit :

```powershell
build.cmd cloud debug AzureGermanCloud
```

Bing Maps n’est actuellement pas disponible dans Azure Allemagne. Par conséquent, il est impossible de s’y abonner automatiquement. Vous pouvez résoudre ce problème en vous abonnant au service dans Azure global et en utilisant le service à cet endroit. 

> [!NOTE]
> Lorsque vous utilisez Bing Maps de la façon décrite ici, vous quittez l’environnement Azure Allemagne.

Voici la marche à suivre :

1. Créez une API Bings Maps dans le portail Azure global en cliquant sur **+ Nouveau**, en recherchant **Bing Maps API pour Entreprise** et en suivant la procédure.
2. Obtenez votre clé Bing Maps API pour Entreprise sur le portail Azure global : 
    1. Accédez au groupe de ressources contenant Bing Maps API pour Entreprise dans le portail Azure global.
    2. Cliquez sur **Tous les paramètres** > **Gestion des clés**. 
    3. Vous voyez deux clés : MasterKey et QueryKey. Copiez la valeur de QueryKey.
3. Déroulez le code le plus récent dans le [référentiel Azure-IoT-Remote-Monitoring sur GitHub](https://github.com/Azure/azure-iot-remote-monitoring).
4. Effectuez un déploiement dans le cloud dans votre environnement selon les instructions de déploiement par ligne de commande dans le dossier `/docs/` du référentiel. 
5. Une fois le déploiement effectué, recherchez dans votre dossier racine le fichier **.user.config** créé. Ouvrez ce fichier dans un éditeur de texte. 
6. Modifiez la ligne suivante en y incluant la valeur que vous avez copiée pour QueryKey : `<setting name="MapApiQueryKey" value="" />`
7. Redéployez la solution en répétant l’étape 4.
 


## <a name="next-steps"></a>Étapes suivantes
Pour obtenir des informations supplémentaires et des mises à jour, abonnez-vous au [blog Azure Allemagne](/archive/blogs/azuregermany/).