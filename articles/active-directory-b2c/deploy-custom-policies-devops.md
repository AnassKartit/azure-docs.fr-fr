---
title: Déployer des stratégies personnalisées avec Azure Pipelines
titleSuffix: Azure AD B2C
description: Découvrez comment déployer des stratégies personnalisées Azure AD B2C dans un pipeline CI/CD en utilisant Azure Pipelines.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/26/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: d24e1cf8394b697348492ffcddc4634646ebbbb6
ms.sourcegitcommit: 47fac4a88c6e23fb2aee8ebb093f15d8b19819ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2021
ms.locfileid: "122967085"
---
# <a name="deploy-custom-policies-with-azure-pipelines"></a>Déployer des stratégies personnalisées avec Azure Pipelines

[Azure Pipelines](/azure/devops/pipelines) prend en charge l’intégration continue (CI) et la livraison continue (CD) pour vous permettre de générer du code, le tester et le publier vers n’importe quelle cible, tout cela de manière constante et cohérente. Cet article explique comment automatiser le processus de déploiement des [stratégies personnalisées](user-flow-overview.md) Azure Active Directory B2C (Azure AD B2C) à l’aide d’Azure Pipelines.

> [!IMPORTANT]
> La gestion de stratégies personnalisées Azure AD B2C avec Azure Pipelines utilise actuellement des opérations en **préversion** disponibles sur le point de terminaison `/beta` de l’API Microsoft Graph. L’utilisation de ces API dans les applications de production n’est pas prise en charge. Pour plus d’informations, voir la [référence du point de terminaison beta de l’API REST Microsoft Graph](/graph/api/overview?toc=.%2fref%2ftoc.json&view=graph-rest-beta&preserve-view=true).

## <a name="prerequisites"></a>Prérequis

* Suivez les étapes de l’article [Prise en main des stratégies personnalisées dans Azure Active Directory B2C](tutorial-create-user-flows.md).
* Si vous n’avez pas encore créé d’organisation DevOps, créez-en une en suivant les instructions fournies dans [S’inscrire et se connecter à Azure DevOps](/azure/devops/user-guide/sign-up-invite-teammates).  

## <a name="register-an-application-for-management-tasks"></a>Inscrire une application pour des tâches de gestion

Vous utilisez le script PowerShell pour déployer les stratégies Azure AD B2C. Pour permettre l’interaction du script PowerShell avec l’[API Microsoft Graph](microsoft-graph-operations.md), créez une inscription d’application dans votre locataire Azure AD B2C. Si vous ne l’avez pas déjà fait, [inscrivez une application Microsoft Graph](microsoft-graph-get-started.md).

Pour permettre au script PowerShell d’accéder aux données dans MS Graph, accordez à l’application inscrite les [autorisations d’application](/graph/permissions-reference) qui conviennent. Accordez l’autorisation **Microsoft Graph** > **Stratégie** > **Policy.ReadWrite.TrustFramework** dans **Autorisations de l’API** de l’inscription d’application.

## <a name="configure-an-azure-repo"></a>Configurer un référentiel Azure

Une fois que vous avez une application Microsoft Graph inscrite, vous êtes prêt à configurer un référentiel pour vos fichiers de stratégie.

1. Connectez-vous à votre [organisation Azure DevOps](https://azure.microsoft.com/services/devops/).
1. [Créez un projet][devops-create-project], ou sélectionnez un projet existant.
1. Dans votre projet, accédez à **Référentiels**, puis sélectionnez **Fichiers**. 
1. Sélectionnez un référentiel existant ou créez-en un.
1. Dans le répertoire racine de votre référentiel, créez un dossier nommé `B2CAssets`. Ajoutez les fichiers de vos stratégies personnalisées Azure AD B2C dans le dossier *B2CAssets*.
1. Dans le répertoire racine de votre référentiel, créez un dossier nommé `Scripts`. Créez un fichier PowerShell appelé *DeployToB2C.ps1*. Collez le script PowerShell suivant dans le fichier *DeployToB2C.ps1*. 
1. **Commitez** et **envoyez (push)** les changements.

Le script suivant acquiert un jeton d’accès à partir d’Azure AD. Avec le jeton, le script appelle l’API MS Graph pour charger les stratégies dans le dossier *B2CAssets*. Vous pouvez également modifier le contenu d’une stratégie avant de la charger. Par exemple, remplacez `tenant-name.onmicrosoft.com` par le nom de votre locataire.

```PowerShell
[Cmdletbinding()]
Param(
    [Parameter(Mandatory = $true)][string]$ClientID,
    [Parameter(Mandatory = $true)][string]$ClientSecret,
    [Parameter(Mandatory = $true)][string]$TenantId,
    [Parameter(Mandatory = $true)][string]$Folder,
    [Parameter(Mandatory = $true)][string]$Files
)

try {
    $body = @{grant_type = "client_credentials"; scope = "https://graph.microsoft.com/.default"; client_id = $ClientID; client_secret = $ClientSecret }

    $response = Invoke-RestMethod -Uri https://login.microsoftonline.com/$TenantId/oauth2/v2.0/token -Method Post -Body $body
    $token = $response.access_token

    $headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
    $headers.Add("Content-Type", 'application/xml')
    $headers.Add("Authorization", 'Bearer ' + $token)

    # Get the list of files to upload
    $filesArray = $Files.Split(",")

    Foreach ($file in $filesArray) {

        $filePath = $Folder + $file.Trim()

        # Check if file exists
        $FileExists = Test-Path -Path $filePath -PathType Leaf

        if ($FileExists) {
            $policycontent = Get-Content $filePath

            # Optional: Change the content of the policy. For example, replace the tenant-name with your tenant name.
            # $policycontent = $policycontent.Replace("your-tenant.onmicrosoft.com", "contoso.onmicrosoft.com")     
    
    
            # Get the policy name from the XML document
            $match = Select-String -InputObject $policycontent  -Pattern '(?<=\bPolicyId=")[^"]*'
    
            If ($match.matches.groups.count -ge 1) {
                $PolicyId = $match.matches.groups[0].value
    
                Write-Host "Uploading the" $PolicyId "policy..."
    
                $graphuri = 'https://graph.microsoft.com/beta/trustframework/policies/' + $PolicyId + '/$value'
                $response = Invoke-RestMethod -Uri $graphuri -Method Put -Body $policycontent -Headers $headers
    
                Write-Host "Policy" $PolicyId "uploaded successfully."
            }
        }
        else {
            $warning = "File " + $filePath + " couldn't be not found."
            Write-Warning -Message $warning
        }
    }
}
catch {
    Write-Host "StatusCode:" $_.Exception.Response.StatusCode.value__

    $_

    $streamReader = [System.IO.StreamReader]::new($_.Exception.Response.GetResponseStream())
    $streamReader.BaseStream.Position = 0
    $streamReader.DiscardBufferedData()
    $errResp = $streamReader.ReadToEnd()
    $streamReader.Close()

    $ErrResp

    exit 1
}

exit 0
```

## <a name="configure-azure-pipelines"></a>Configure Azure Pipelines

Une fois votre référentiel initialisé et rempli avec vos fichiers de stratégie personnalisés, vous êtes prêt à configurer le pipeline de mise en production. Pour créer un pipeline, procédez comme suit :

1. Dans votre projet, sélectionnez **Pipelines** > **Versions** > **Nouveau pipeline**.
1. Sous **Sélectionner un modèle**, sélectionnez **Projet vide**, puis sélectionnez **Appliquer**.
1. Entrez le **Nom de l’étape**, par exemple, *DeployCustomPolicies*, puis fermez le volet.
1. Sélectionnez **Ajouter un artefact**, puis, sous **Type de source**, choisissez **Référentiel Azure**.
    1. Dans **Projet**, sélectionnez votre projet.
    1. Sélectionnez la **Source (dépôt)** qui contient le dossier *Scripts*.
    1. Sélectionnez une **Branche par défaut**, par exemple *maître*.
    1. Conservez le paramètre **Version par défaut** de *La dernière de la branche par défaut*.
    1. Entrez un **Alias source** pour le référentiel. Par exemple, *policyRepo*. 
1. Sélectionnez **Ajouter**
1. Renommez le pipeline afin de refléter son intention. Par exemple, *Pipeline de déploiement de stratégie personnalisée*.
1. Sélectionnez **Enregistrer** pour enregistrer la configuration du pipeline.

### <a name="configure-pipeline-variables"></a>Configurer des variables de pipeline

Les variables de pipeline sont pratiques, car elles vous permettent d’injecter des bits de données clés dans différentes parties du pipeline. Les variables suivantes fournissent des informations relatives à votre environnement Azure AD B2C.

| Nom | Valeur |
| ---- | ----- |
| `clientId` | **ID d’application (client)** pour l’application que vous avez inscrite. |
| `clientSecret` | Valeur de la **clé secrète client** que vous avez créée précédemment. <br /> Modifiez le type de variable en **secret** (sélectionnez l’icône de verrou). |
| `tenantId` | `your-b2c-tenant.onmicrosoft.com`, où *your-b2c-tenant* est le nom de votre locataire Azure AD B2C. |

Pour ajouter des variables de pipeline, effectuez les étapes suivantes :

1. Dans votre pipeline, sélectionnez l’onglet **Variables**.
1. Sous **Variables de pipeline**, ajoutez les variables ci-dessus avec leurs valeurs.
1. Sélectionnez **Enregistrer** pour enregistrer les variables.

### <a name="add-pipeline-tasks"></a>Ajouter des tâches de pipeline

Une tâche de pipeline est un script prédéfini qui effectue une action. Ajoutez une tâche qui appelle le script PowerShell *DeployToB2C.ps1*.

1. Dans le pipeline que vous avez créé, sélectionnez l’onglet **Tâches**.
1. Sélectionnez **Travail de l’agent**, puis le signe plus ( **+** ) pour ajouter une tâche au travail de l’agent.
1. Recherchez et sélectionnez **PowerShell**. Ne sélectionnez pas « Azure PowerShell », « PowerShell sur des machines cibles » ou une autre entrée PowerShell.
1. Sélectionnez la tâche de **Script PowerShell** que vous venez d’ajouter.
1. Entrez les valeurs suivantes pour la tâche de Script PowerShell :
    * **Version de la tâche** : 2.*
    * **Nom d'affichage** : Nom de la stratégie que cette tâche doit charger. Par exemple, *B2C_1A_TrustFrameworkBase*.
    * **Type** : Chemin d'accès au fichier
    * **Chemin d’accès au script** : Sélectionnez les points de suspension (**_..._* _), accédez au dossier _Scripts*, puis sélectionnez le fichier *DeployToB2C.ps1*.
    * **Arguments** : entrez le script PowerShell suivant. 


        ```PowerShell
        -ClientID $(clientId) -ClientSecret $(clientSecret) -TenantId $(tenantId) -Folder $(System.DefaultWorkingDirectory)/policyRepo/B2CAssets/ -Files "TrustFrameworkBase.xml,TrustFrameworkExtensions.xml,SignUpOrSignin.xml,ProfileEdit.xml,PasswordReset.xml"
        ```
        
        Le paramètre `-Files` est une liste délimitée par des virgules des fichiers de stratégie à déployer. Mettez à jour la liste avec vos fichiers de stratégie.
        
        > [!IMPORTANT]
        >  Vérifiez que les stratégies sont chargées dans le bon ordre. Tout d’abord, la stratégie de base, la stratégie d’extensions, puis les stratégies de la partie de confiance. Par exemple : `TrustFrameworkBase.xml,TrustFrameworkExtensions.xml,SignUpOrSignin.xml`.
        
1. Sélectionnez **Enregistrer** pour enregistrer le Travail de l’agent.

## <a name="test-your-pipeline"></a>Tester votre pipeline

Pour tester votre pipeline de mise en production :

1. Sélectionnez **Pipelines**, puis **Mises en production**.
1. Sélectionnez le pipeline précédemment, par exemple *DeployCustomPolicies*.
1. Sélectionnez **Créer une mise en production**, puis **Créer** pour mettre en file d’attente la mise en production.

Vous devez voir une bannière de notification indiquant qu’une mise en production a été mise en file d’attente. Pour afficher son état, sélectionnez le lien dans la bannière de notification ou dans la liste sous l’onglet **Mises en production**.


## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus :

* [Appels de service à service utilisant des informations d’identification de client](../active-directory/develop/v2-oauth2-client-creds-grant-flow.md)
* [Azure DevOps Services](/azure/devops/user-guide/)

<!-- LINKS - External -->
[devops]: /azure/devops/
[devops-create-project]:  /azure/devops/organizations/projects/create-project
[devops-pipelines]: /azure/devops/pipelines
