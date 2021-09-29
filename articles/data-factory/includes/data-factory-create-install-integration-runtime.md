---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 6bf40202908aa68345fabd2b6fe55d501e8325ff
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124733027"
---
## <a name="create-a-self-hosted-integration-runtime"></a>Créer un runtime d’intégration auto-hébergé

Dans cette section, vous allez créer un runtime d’intégration auto-hébergé et l’associer à un ordinateur local avec la base de données SQL Server. Le runtime d’intégration auto-hébergé est le composant qui copie les données de SQL Server sur votre machine vers Azure SQL Database. 

1. Créez une variable pour le nom du runtime d’intégration. Utilisez un nom unique et notez-le. Vous l’utiliserez ultérieurement dans ce tutoriel. 

    ```powershell
   $integrationRuntimeName = "ADFTutorialIR"
    ```
2. Créez un runtime d’intégration auto-hébergé. 

   ```powershell
   Set-AzDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Voici l'exemple de sortie :

   ```console
    Name              : <Integration Runtime name>
    Type              : SelfHosted
    ResourceGroupName : <ResourceGroupName>
    DataFactoryName   : <DataFactoryName>
    Description       : 
    Id                : /subscriptions/<subscription ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DataFactory/factories/<DataFactoryName>/integrationruntimes/ADFTutorialIR
    ```
  
3. Exécutez la commande suivante pour récupérer l’état du runtime d’intégration créé : Vérifiez que la valeur de la propriété **État** est définie sur **NeedRegistration**. 

   ```powershell
   Get-AzDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Voici l'exemple de sortie :

   ```console  
   State                     : NeedRegistration
   Version                   : 
   CreateTime                : 9/24/2019 6:00:00 AM
   AutoUpdate                : On
   ScheduledUpdateDate       : 
   UpdateDelayOffset         : 
   LocalTimeZoneOffset       : 
   InternalChannelEncryption : 
   Capabilities              : {}
   ServiceUrls               : {eu.frontend.clouddatahub.net}
   Nodes                     : {}
   Links                     : {}
   Name                      : ADFTutorialIR
   Type                      : SelfHosted
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Description               : 
   Id                        : /subscriptions/<subscription ID>/resourceGroups/<ResourceGroup name>/providers/Microsoft.DataFactory/factories/<DataFactory name>/integrationruntimes/<Integration Runtime name>
   ```

4. Exécutez la commande suivante pour récupérer les clés d’authentification permettant d’enregistrer le runtime d’intégration auto-hébergé auprès du service Azure Data Factory dans le cloud : 

   ```powershell
   Get-AzDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Voici l'exemple de sortie :

   ```json
   {
    "AuthKey1": "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=",
    "AuthKey2":  "IR@0000000000-0000-0000-0000-000000000000@xy0@xy@yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy="
   }
   ```    

5. Copiez l’une des clés (sans les guillemets) pour enregistrer le runtime d’intégration auto-hébergé que vous allez installer sur votre ordinateur à l’étape suivante.  

## <a name="install-the-integration-runtime-tool"></a>Installer l’outil Integration Runtime

1. Si le runtime d’intégration est déjà installé sur votre machine, désinstallez-le à l’aide de **Ajouter ou supprimer des programmes**. 

2. [Téléchargez](https://www.microsoft.com/download/details.aspx?id=39717) le runtime d’intégration auto-hébergé sur un ordinateur Windows local. Exécutez l’installation.

3. Sur la page **Bienvenue dans l’assistant d’installation de Microsoft Integration Runtime**, cliquez sur **Suivant**.

4. Sur la page **Contrat de Licence utilisateur final**, acceptez les conditions et le contrat de licence, puis cliquez sur **Suivant**.

5. Sur la page **Dossier de destination**, cliquez sur **Suivant**.

6. Sur la page **Prêt à installer Microsoft Integration Runtime**, cliquez sur **Installer**.

7. Sur la page **Assistant d’installation de Microsoft Integration Runtime terminé**, cliquez sur **Terminer**.

8. Sur la page **Inscrire le Integration Runtime (auto-hébergé)** , collez la clé que vous avez enregistrée dans la section précédente, puis cliquez sur **Inscrire**. 

    :::image type="content" source="media/data-factory-create-install-integration-runtime/register-integration-runtime.png" alt-text="Inscrire le runtime d’intégration":::

9. Dans la page **Nouveau runtime d’intégration (auto-hébergé)** , sélectionnez **Terminer**. 

10. Le message suivant s’affiche une fois que le runtime d’intégration auto-hébergé est bien inscrit :

    :::image type="content" source="media/data-factory-create-install-integration-runtime/registered-successfully.png" alt-text="Inscription réussie":::

14. Sur la page **Inscrire le runtime d’intégration (auto-hébergé)** , cliquez sur **Lancer le Gestionnaire de configuration**.

15. La page suivante apparaît une fois que le nœud est connecté au service cloud :

    :::image type="content" source="media/data-factory-create-install-integration-runtime/node-is-connected.png" alt-text="Page Le nœud est connecté":::

16. Maintenant, testez la connectivité à votre base de données SQL Server.

    :::image type="content" source="media/data-factory-create-install-integration-runtime/config-manager-diagnostics-tab.png" alt-text="Onglet Diagnostic":::   

    a. Sur la page **Gestionnaire de configuration**, accédez à l’onglet **Diagnostics**.

    b. Sélectionnez **SqlServer** comme type de source de données.

    c. Saisissez le nom du serveur.

    d. Saisissez le nom de la base de données.

    e. Sélectionnez le mode d’authentification.

    f. Entrer le nom d'utilisateur.

    g. Entrez le mot de passe associé au nom d’utilisateur.

    h. Cliquez sur **Tester** pour vérifier que le runtime d’intégration se connecte à SQL Server. Une coche verte apparaît si la connexion est établie. Un message d’erreur apparaît si la connexion échoue. Corrigez les problèmes et assurez-vous que le runtime d’intégration peut se connecter au serveur SQL Server.    

    > [!NOTE]
    > Notez les valeurs pour le type d’authentification, le serveur, la base de données, l’utilisateur et le mot de passe. Vous les utiliserez ultérieurement dans ce didacticiel.
