---
title: Exporter les métriques du connecteur Azure IoT pour FHIR (préversion) via les paramètres de diagnostic
description: Cet article explique comment exporter les métriques du connecteur Azure IoT pour FHIR (préversion) via les paramètres de diagnostic.
services: healthcare-apis
author: msjasteppe
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: how-to
ms.date: 11/13/2020
ms.author: jasteppe
ms.openlocfilehash: 9e0257ad4b4f5887adee724668b227ba87c55e29
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122433654"
---
# <a name="export-iot-connector-for-fhir-preview-metrics-through-diagnostic-settings"></a>exporter IoT Connector pour les métriques FHIR (version préliminaire) via les paramètres de Diagnostic

Dans cet article, vous allez découvrir comment exporter les journaux des indicateurs de performance du Connecteur Azure IoT pour FHIR&#174;* (Fast Healthcare Interoperability Resources). Ce sont les [**paramètres de diagnostic**](../../azure-monitor/essentials/diagnostic-settings.md) du portail Azure qui permettent d’activer la journalisation des métriques. 

> [!TIP]
> Suivez les instructions décrites dans [Activer la journalisation des diagnostics dans l’API Azure pour FHIR et le connecteur Azure IoT pour FHIR](enable-diagnostic-logging.md#enable-diagnostic-logging-in-azure-api-for-fhir) pour configurer la journalisation d’audit.

## <a name="enable-metrics-logging-for-the-azure-iot-connector-for-fhir-preview"></a>Activer la journalisation des métriques du connecteur Azure IoT pour FHIR (préversion)
1. Pour activer la journalisation des métriques du connecteur Azure IoT pour FHIR, sélectionnez votre service API Azure pour FHIR dans le portail Azure. 

2. Accédez à **Paramètres de diagnostic**. 

3. Sélectionnez **+ Ajouter un paramètre de diagnostic**.

   :::image type="content" source="media/iot-metrics-export/diagnostic-settings-main.png" alt-text="Connecteur1 IoT" lightbox="media/iot-metrics-export/diagnostic-settings-main.png"::: 

4. Entrez un nom dans la boîte de dialogue **Nom du paramètre de diagnostic**.

5. Sélectionnez la méthode que vous souhaitez utiliser pour accéder à vos journaux de diagnostic :

    1. **Archivez les journaux dans un compte de stockage** pour les auditer ou les inspecter manuellement. Le compte de stockage que vous souhaitez utiliser doit déjà être créé.
    2. **Envoyez les journaux à Event Hub** pour ingestion par un service tiers ou une solution d’analytique personnalisée. Vous devez créer un espace de noms et une stratégie Event Hub pour pouvoir configurer cette étape.
    3. **Envoyez les journaux vers l’espace de travail Log Analytics** dans Azure Monitor. Vous devrez créer votre espace de travail Logs Analytics avant de pouvoir sélectionner cette option.

6. Sélectionnez **Erreurs, Trafic et Latence** pour le connecteur Azure IoT pour FHIR.  Sélectionnez les catégories de métriques supplémentaires que vous souhaitez capturer pour l’API Azure pour FHIR.

7. Sélectionnez **Enregistrer**.

   :::image type="content" source="media/iot-metrics-export/diagnostic-setting-add.png" alt-text="Connecteur2 IoT" lightbox="media/iot-metrics-export/diagnostic-setting-add.png":::

> [!Note] 
> L’affichage des premiers journaux de métriques dans le référentiel de votre choix peut demander jusqu’à 15 minutes.  
 
Pour plus d’informations sur l’utilisation des journaux de diagnostic, consultez la [documentation relative aux journaux de ressources d’Azure](../../azure-monitor/essentials/platform-logs-overview.md).

## <a name="conclusion"></a>Conclusion 
L’accès aux journaux de métriques est essentiel pour la supervision et la résolution des problèmes.  Le connecteur Azure IoT pour FHIR vous permet d’effectuer ces actions par le biais des journaux de métriques. 

## <a name="next-steps"></a>Étapes suivantes

Consultez les questions fréquemment posées sur le Connecteur Azure IoT pour FHIR.

>[!div class="nextstepaction"]
>[FAQ sur le Connecteur Azure IoT pour FHIR](fhir-faq.yml)

*Sur le Portail Azure, le Connecteur Azure IoT pour FHIR est appelé Connecteur IoT (préversion). Le sigle FHIR est une marque déposée de HL7 et est utilisé avec l’autorisation de HL7.