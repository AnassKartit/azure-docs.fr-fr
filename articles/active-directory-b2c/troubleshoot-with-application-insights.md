---
title: Résoudre les problèmes des stratégies personnalisées avec Application Insights
titleSuffix: Azure AD B2C
description: configuration d’Application Insights pour suivre l’exécution de vos stratégies personnalisées.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: troubleshooting
ms.date: 08/26/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: b8ceea26ed7a5e58e890c4e313b00f1f4f37f4e7
ms.sourcegitcommit: 47fac4a88c6e23fb2aee8ebb093f15d8b19819ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2021
ms.locfileid: "122967796"
---
# <a name="collect-azure-active-directory-b2c-logs-with-application-insights"></a>Collecter les journaux Azure Active Directory B2C avec Application Insights

Cet article explique comment collecter les journaux d’activité à partir d’Azure Active Directory B2C (Azure AD B2C) afin que vous puissiez diagnostiquer les problèmes liés à vos stratégies personnalisées. Application Insights permet de diagnostiquer les exceptions et de visualiser les problèmes de performances des applications. Azure AD B2C comprend une fonctionnalité d’envoi de données à Application Insights.

Les journaux d’activité détaillés décrits ici doivent être activés **UNIQUEMENT** lors du développement de vos stratégies personnalisées.

> [!WARNING]
> Ne définissez pas `DeploymentMode` sur `Development` dans les environnements de production. Les journaux d’activité recueillent toutes les revendications envoyées par et aux fournisseurs d’identité. En tant que développeur, vous assumez la responsabilité des données personnelles collectées dans vos journaux Application Insights. Ces journaux détaillés sont collectés uniquement lorsque la stratégie est placée en **MODE DÉVELOPPEUR**.

## <a name="set-up-application-insights"></a>Configurer Application Insights

Si vous n’en avez pas encore, créez une instance Application Insights dans votre abonnement.

> [!TIP]
> Une même instance d’Application Insights peut être utilisée pour plusieurs locataires Azure AD B2C. Ensuite, dans votre requête, vous pouvez filtrer selon le locataire ou le nom de la stratégie. Pour plus d’informations, [consultez les journaux dans les exemples Application Insights](#see-the-logs-in-application-insights).

Pour utiliser une instance Application Insights de sortie dans votre abonnement, procédez comme suit :

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Sélectionnez le filtre **Annuaire et abonnement** dans le menu supérieur, puis l’annuaire qui contient votre abonnement Azure (et non votre annuaire Azure AD B2C).
1. Ouvrez la ressource Application Insights que vous avez créée précédemment.
1. Sur la page **Vue d’ensemble**, enregistrez la **Clé d’instrumentation**

Pour créer une instance Application Insights dans votre abonnement, procédez comme suit :

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Sélectionnez le filtre **Annuaire et abonnement** dans le menu supérieur, puis l’annuaire qui contient votre abonnement Azure (et non votre annuaire Azure AD B2C).
1. Sélectionnez **Créer une ressource** dans le menu de navigation de gauche.
1. Recherchez et sélectionnez **Application Insights**, puis sélectionnez **Créer**.
1. Remplissez le formulaire, sélectionnez **Vérifier + créer**, puis sélectionnez **Créer**.
1. Une fois le déploiement terminé, sélectionnez **Accéder à la ressource**.
1. Sous **Configurer** dans le menu Application Insights, sélectionnez **Propriétés**.
1. Enregistrez la **CLÉ D'INSTRUMENTATION**, que vous utiliserez dans une étape ultérieure.

## <a name="configure-the-custom-policy"></a>Configurer la stratégie personnalisée

1. Ouvrez le fichier de partie de confiance (RP), par exemple, *SignUpOrSignin.xml*.
1. Ajoutez les attributs suivants à l’élément `<TrustFrameworkPolicy>` :

   ```xml
   DeploymentMode="Development"
   UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
   ```

1. S’il n’existe pas déjà, ajoutez un nœud enfant `<UserJourneyBehaviors>` au nœud `<RelyingParty>`. Il doit être placé après `<DefaultUserJourney ReferenceId="UserJourney Id" from your extensions policy, or equivalent (for example:SignUpOrSigninWithAAD" />`.
1. Ajoutez le nœud suivant en tant qu’enfant de l’élément `<UserJourneyBehaviors>`. Veillez à remplacer `{Your Application Insights Key}` par la **clé d’instrumentation** Application Insights que vous avez enregistrée précédemment.

    ```xml
    <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
    ```

    * `DeveloperMode="true"` dit à Application Insights d’envoyer la télémétrie par le biais du pipeline de traitement. Adapté au développement, mais restreint à des volumes élevés. En production, définissez `DeveloperMode` sur `false`.
    * `ClientEnabled="true"` envoie le script côté client ApplicationInsights pour l’affichage de la page de suivi et les erreurs côté client. Vous pouvez les afficher dans la table **browserTimings** dans le portail Application Insights. En configurant `ClientEnabled= "true"`, vous ajoutez Application Insights à votre script de page et vous obtenez le minutage des chargements de page et des appels AJAX, le nombre d’exceptions du navigateur et d’échecs d’AJAX et leurs détails, ainsi que les nombres d’utilisateurs et de sessions. Ce champ est **facultatif** et est défini sur `false` par défaut.
    * `ServerEnabled="true"` envoie le JSON UserJourneyRecorder existant en tant qu’événement personnalisé à Application Insights.

    Par exemple :

    ```xml
    <TrustFrameworkPolicy
      ...
      TenantId="fabrikamb2c.onmicrosoft.com"
      PolicyId="SignUpOrSignInWithAAD"
      DeploymentMode="Development"
      UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
    >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="UserJourney ID from your extensions policy, or equivalent (for example: SignUpOrSigninWithAzureAD)" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
    </TrustFrameworkPolicy>
    ```

1. Téléchargez la stratégie.

## <a name="see-the-logs-in-application-insights"></a>Afficher des journaux d’activité dans Application Insights

Il y a un court délai, généralement moins de cinq minutes, avant que les nouveaux journaux d’activité s’affichent dans Application Insights.

1. Ouvrez la ressource Application Insights que vous avez créée sur le [portail Azure](https://portal.azure.com).
1. Dans la page **Vue d’ensemble**, sélectionnez **Journaux d’activité**.
1. Ouvrez un nouvel onglet dans Application Insights.

Voici une liste de requêtes que vous pouvez utiliser pour afficher les journaux d’activité :

| Requête | Description |
|---------------------|--------------------|
| `traces` | Obtenir tous les journaux d’activité générés par Azure AD B2C |
| `traces | where timestamp > ago(1d)` | Obtenez tous les journaux d’activité générés par Azure AD B2C pour le dernier jour.|
| `traces | where message contains "exception" | where timestamp > ago(2h)`|  Récupérez tous les journaux avec des erreurs au cours des deux dernières heures.|
| `traces | where customDimensions.Tenant == "contoso.onmicrosoft.com" and customDimensions.UserJourney  == "b2c_1a_signinandup"` | Obtenez tous les journaux générés par le client Azure AD B2C  *contoso.onmicrosoft.com* pour lesquels le parcours de l’utilisateur est *b2c_1a_signinandup*. |
| `traces | where customDimensions.CorrelationId == "00000000-0000-0000-0000-000000000000"`| Récupération de tous les journaux générés par Azure AD B2C pour un ID de corrélation. Remplacez l’ID de corrélation par votre ID de corrélation. | 

Les entrées peuvent être longues. Exporter au format CSV pour une étude plus approfondie.

Pour plus d’informations sur les requêtes, consultez [Vue d’ensemble des requêtes de journal dans Azure Monitor](../azure-monitor/logs/log-query-overview.md).

## <a name="see-the-logs-in-vs-code-extension"></a>Voir les journaux dans l’extension VS Code

Nous vous recommandons d’installer l’[extension Azure AD B2C](https://marketplace.visualstudio.com/items?itemName=AzureADB2CTools.aadb2c) pour [VS Code](https://code.visualstudio.com/). Grâce à l’extension Azure AD B2C, les journaux sont organisés pour vous par nom de stratégie, par ID de corrélation (Application Insights présente le premier chiffre de l’ID de corrélation) et par timestamp du journal. Cette fonctionnalité vous aide à trouver le journal pertinent en fonction du timestamp local et à voir le parcours utilisateur tel qu’il est exécuté par Azure AD B2C.

> [!NOTE]
> La communauté a développé l’extension VS Code pour Azure AD B2C pour aider les développeurs travaillant dans le domaine de l’identité. L’extension n’est pas prise en charge par Microsoft et est mise à disposition strictement telle quelle.

### <a name="set-application-insights-api-access"></a>Définir l’accès à l’API d’Application Insights

Après avoir configuré Application Insights et la stratégie personnalisée, vous devez récupérer votre **ID d’API** pour Application Insights et créer une **clé API**. L’ID d’API et la clé API sont utilisés par l’extension Azure AD B2C pour lire les événements Application Insights (télémétries). Vos clés API doivent être gérées comme des mots de passe. Ne le divulguez pas.

> [!NOTE]
> La clé d’instrumentation d’Application Insights que vous avez créée précédemment est utilisée par Azure AD B2C pour envoyer des télémétries à Application Insights. Vous utilisez la clé d’instrumentation uniquement dans votre stratégie Azure AD B2C, et non dans l’extension VS Code.

Pour récupérer l’ID et la clé d’Application Insights :

1. Dans Portail Azure, ouvrez la ressource Application Insights correspondant à votre application.
1. Sélectionnez **Paramètres**, puis **Accès à l’API**.
1. Copiez l’**ID de l’application**.
1. Sélectionnez **Créer une clé API**.
1. Cochez la case **Lire la télémétrie**.
1. Copiez la **clé** avant de fermer le panneau Créer une clé API et enregistrez-la dans un endroit sûr. Si vous perdez la clé, vous devez en créer une autre.

    ![Capture d’écran illustrant comment créer une clé d’accès à l’API.](./media/troubleshoot-with-application-insights/application-insights-api-access.png)

### <a name="set-up-azure-ad-b2c-vs-code-extension"></a>Configurer l’extension Azure AD B2C VS Code

Maintenant que vous avez l’ID et la clé de l’API Azure Application Insights, vous pouvez configurer l’extension VS Code pour lire les journaux. L’extension Azure AD B2C VS Code propose deux étendues pour les paramètres :

- **Paramètres globaux de l’utilisateur** : Paramètres qui s’appliquent globalement à toute instance de VS Code que vous ouvrez.
- **Paramètres de l’espace de travail** : Paramètres stockés dans votre espace de travail et uniquement applicables lorsque l’espace de travail est ouvert (en utilisant **Ouvrir un dossier** dans VS Code).

1. Dans l’explorateur **Azure AD B2C Trace**, cliquez sur l’icône **Paramètres**.

    ![Capture d’écran illustrant la sélection des paramètres Application Insights.](./media/troubleshoot-with-application-insights/app-insights-settings.png)

1. Fournissez l’**ID** et la **clé** d’Azure Application Insights.
1. Cliquez sur **Enregistrer**.

Une fois que vous avez enregistré les paramètres, les journaux d’Application Insights s’affichent dans la fenêtre **Azure AD B2C Trace (App Insights)** .

![Capture d’écran de l’extension Azure AD B2C pour VS Code, présentant la trace d’Azure Application Insights.](./media/troubleshoot-with-application-insights/vscode-extension-application-insights-trace.png)


## <a name="configure-application-insights-in-production"></a>Configurer Application Insights en production

Pour améliorer les performances de votre environnement de production et l'expérience des utilisateurs, il est important de configurer votre stratégie de manière à ignorer les messages sans importance. Utilisez la configuration suivante dans les environnements de production. 

1. Définissez l'attribut `DeploymentMode` de [TrustFrameworkPolicy](trustframeworkpolicy.md) sur `Production`. 

   ```xml
   <TrustFrameworkPolicy xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06" PolicySchemaVersion="0.3.0.0"
   TenantId="yourtenant.onmicrosoft.com"
   PolicyId="B2C_1A_signup_signin"
   PublicPolicyUri="http://yourtenant.onmicrosoft.com/B2C_1A_signup_signin"
   DeploymentMode="Production"
   UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights">
   ```

1. Définissez l'attribut `DeveloperMode` de [JourneyInsights](relyingparty.md#journeyinsights) sur `false`.

   ```xml
   <UserJourneyBehaviors>
     <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="false" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
   </UserJourneyBehaviors>
   ```
   
1. Chargez et testez votre stratégie.



## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur la [résolution des problèmes liés aux stratégies personnalisées Azure AD B2C](troubleshoot-custom-policies.md)
