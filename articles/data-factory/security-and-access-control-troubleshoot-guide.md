---
title: Résoudre les problèmes de sécurité et de contrôle d’accès
titleSuffix: Azure Data Factory & Azure Synapse
description: Découvrez comment résoudre les problèmes de sécurité et de contrôle d’accès dans Azure Data Factory.
author: lrtoyou1223
ms.service: data-factory
ms.subservice: integration-runtime
ms.custom: synapse
ms.topic: troubleshooting
ms.date: 07/28/2021
ms.author: lle
ms.openlocfilehash: d5824e4b7ffcdf8acab2ccfad9efdf4850f160b6
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122641343"
---
# <a name="troubleshoot-azure-data-factory-security-and-access-control-issues"></a>Résoudre les problèmes de sécurité et de contrôle d’accès dans Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Cet article présente des méthodes couramment employées pour résoudre les problèmes de sécurité et de contrôle d’accès dans Azure Data Factory.

## <a name="common-errors-and-messages"></a>Erreurs et messages courants

### <a name="connectivity-issue-in-the-copy-activity-of-the-cloud-datastore"></a>Problème de connectivité dans l’activité de copie du magasin de données cloud

#### <a name="symptoms"></a>Symptômes

Divers messages d’erreur peuvent être renvoyés en cas de problèmes de connectivité dans le magasin de données source ou récepteur.

#### <a name="cause"></a>Cause 

Le problème est-il généralement dû à l'un des facteurs suivants ?

* Paramètre de proxy dans le nœud Runtime d’intégration auto-hébergé (IR), si vous utilisez un IR auto-hébergé.

* Paramètre de pare-feu dans le nœud Runtime d’intégration auto-hébergé, si vous utilisez un IR auto-hébergé.

* Paramètre de pare-feu dans le magasin de données cloud.

#### <a name="resolution"></a>Résolution

* Pour vous assurer qu’il s’agit d’un problème de connectivité, vérifiez les points suivants :

   - L’erreur est générée à partir des connecteurs source ou récepteur.
   - L’échec se produit au début de l’activité de copie.
   - L’échec est cohérent pour Azure IR ou le runtime d’intégration auto-hébergé avec un nœud, car il peut s’agir d’un échec aléatoire dans un Runtime d’intégration auto-hébergé à plusieurs nœuds si seuls certains nœuds ont le problème.

* Si vous utilisez un **runtime d'intégration auto-hébergé**, vérifiez vos paramètres de proxy, de pare-feu et réseau, car la connexion au même magasin de données peut être réussie si vous utilisez un Azure IR. Pour résoudre les problèmes de ce scénario, consultez :

   * [Ports et pare-feu pour le runtime d’intégration auto-hébergé](./create-self-hosted-integration-runtime.md#ports-and-firewalls)
   * [Connecteur Azure Data Lake Storage](./connector-azure-data-lake-store.md)
  
* Si vous utilisez un **Azure IR**, essayez de désactiver le paramètre de pare-feu du magasin de données. Cette approche peut résoudre les problèmes dans les deux situations suivantes :
  
   * Les [adresses IP Azure IR ](./azure-integration-runtime-ip-addresses.md) ne figurent pas dans la liste verte.
   * La fonctionnalité *Autoriser les services Microsoft approuvés à accéder à ce compte de stockage* est désactivée pour [Stockage Blob Azure](./connector-azure-blob-storage.md#supported-capabilities) et [Azure Data Lake Storage Gen 2](./connector-azure-data-lake-storage.md#supported-capabilities).
   * Le paramètre *Autoriser l’accès aux services Azure* n’est pas activé pour Azure Data Lake Storage Gen1.

Si aucune des méthodes précédentes ne fonctionne, contactez Microsoft pour obtenir de l’aide.


### <a name="invalid-or-empty-authentication-key-issue-after-public-network-access-is-disabled"></a>Problème de clé d’authentification vide ou non valide après la désactivation de l’accès au réseau public

#### <a name="symptoms"></a>Symptômes

Une fois que vous avez désactivé l’accès au réseau public pour Data Factory, le runtime d’intégration auto-hébergé génère l’erreur suivante : « La clé d'authentification n'est pas valide. »

#### <a name="cause"></a>Cause

Le problème est probablement dû à une erreur de résolution DNS (Domain Name System), car la désactivation de la connectivité publique et l’établissement d’un point de terminaison privé ne permettent pas la reconnexion.

Pour vérifier si le nom de domaine complet (FQDN) Data Factory est résolu en adresse IP publique, procédez comme suit :

1. Vérifiez que vous avez créé la machine virtuelle Azure dans le même réseau virtuel que le point de terminaison privé Data Factory.

2. Exécutez PsPing et Ping à partir de la machine virtuelle Azure dans le nom de domaine complet Data Factory :

   `psping.exe <dataFactoryName>.<region>.datafactory.azure.net:443`
   `ping <dataFactoryName>.<region>.datafactory.azure.net`

   > [!Note]
   > Vous devez spécifier un port pour la commande PsPing. Le port 443 est illustré ici, mais n’est pas obligatoire.

3. Vérifiez si les deux commandes correspondent à une adresse IP publique Azure Data Factory basée sur une région spécifiée. L’adresse IP doit respecter le format suivant : `xxx.xxx.xxx.0`

#### <a name="resolution"></a>Résolution

Pour résoudre ce problème, procédez comme suit :

- En option, nous vous proposons d’ajouter manuellement une « liaison de réseau virtuel » sous la « zone DNS de liaison privée » de Data Factory. Pour plus d’informations, consultez l’article [Azure Private Link pour Azure Data Factory](./data-factory-private-link.md#dns-changes-for-private-endpoints). L’instruction consiste à configurer la zone DNS privée ou le serveur DNS privé pour résoudre le nom de domaine complet Data Factory en adresse IP privée. 

- Toutefois, si vous ne voulez pas configurer la zone DNS privée ou le serveur DNS privé, essayez la solution temporaire suivante :

  1. Modifiez le fichier hôte dans Windows et mappez l’adresse IP privée (le point de terminaison privé Azure Data Factory) au nom de domaine complet Azure Data Factory.
  
     Dans la machine virtuelle Azure, accédez à `C:\Windows\System32\drivers\etc`, puis ouvrez le fichier *hôte* dans le Bloc-notes. Ajoutez la ligne qui mappe l’adresse IP privée au nom de domaine complet à la fin du fichier, puis enregistrez la modification.
     
     ![Capture d’écran du mappage de l’adresse IP privée à l’hôte.](media/self-hosted-integration-runtime-troubleshoot-guide/add-mapping-to-host.png)

  1. Réexécutez les mêmes commandes que dans les étapes de vérification précédentes pour vérifier la réponse, qui doit contenir l’adresse IP privée.

  1. Réinscrivez le runtime d’intégration auto-hébergé et le problème devrait être résolu.

### <a name="unable-to-register-ir-authentication-key-on-self-hosted-vms-due-to-private-link"></a>Impossible d’inscrire la clé d’authentification de l’IR sur les machines virtuelles auto-hébergées en raison d’une liaison privée

#### <a name="symptoms"></a>Symptômes

Vous ne pouvez pas inscrire la clé d’authentification IR sur la machine virtuelle auto-hébergée, car le lien privé est activé. Vous recevez le message d’erreur suivant :

« Échec de l’obtention du jeton de service du service ADF avec la clé *************** et le coût du temps est : 0,1250079 seconde, le code d’erreur est : InvalidGatewayKey, activityId est : XXXXXXX et le message d’erreur détaillé est L’adresse IP du client n’est pas une adresse IP privée valide, car la fabrique de données n’a pas pu accéder au réseau public et ne peut donc pas atteindre le cloud pour établir la connexion. »

#### <a name="cause"></a>Cause

Le problème peut être dû à la machine virtuelle dans laquelle vous tentez d’installer le runtime d’intégration auto-hébergé. Pour vous connecter au cloud, assurez-vous que l’accès au réseau public est activé.

#### <a name="resolution"></a>Résolution

**Solution 1**
 
Pour résoudre ce problème, procédez comme suit :

1. Accédez à la page [Fabriques - Mettre à jour](/rest/api/datafactory/Factories/Update).

1. En haut à droite, sélectionnez le bouton **Essayer**.
1. Sous **Paramètres**, renseignez les informations requises. 
1. Sous **Corps**, collez la propriété suivante :

    ```
    { "tags": { "publicNetworkAccess":"Enabled" } }
    ```
1. Sélectionnez **Exécuter** pour exécuter la fonction. 

1. Sous **Paramètres**, renseignez les informations requises. 

1. Sous **Corps**, collez la propriété suivante :
    ```
    { "tags": { "publicNetworkAccess":"Enabled" } }
    ``` 

1. Sélectionnez **Exécuter** pour exécuter la fonction. 
1. Vérifiez que **Code de réponse : 200** s'affiche. La propriété que vous avez collée doit également s’afficher dans la définition JSON.

1. Ajoutez de nouveau la clé d’authentification IR dans le runtime d’intégration.

**Solution 2**

Pour résoudre le problème, accédez à [Azure Private Link pour Azure Data Factory](./data-factory-private-link.md).

Essayez d’activer l’accès au réseau public sur l’interface utilisateur, comme illustré dans la capture d’écran suivante :

![Capture d’écran du contrôle « Activé » pour « Autoriser l’accès au réseau public » dans le volet Mise en réseau.](media/self-hosted-integration-runtime-troubleshoot-guide/enable-public-network-access.png)

### <a name="adf-private-dns-zone-overrides-azure-resource-manager-dns-resolution-causing-not-found-error"></a>La zone DNS privée ADF remplace la résolution DNS d’Azure Resource Manager, entraînant une erreur « introuvable »

#### <a name="cause"></a>Cause
Azure Resource Manager et ADF utilisent la même zone privée, ce qui crée un conflit potentiel sur le DNS privé du client avec un scénario dans lequel les enregistrements Azure Resource Manager sont introuvables.

#### <a name="solution"></a>Solution
1. Recherchez des zones DNS privées **privatelink.Azure.com** dans le portail Azure.
![Capture d’écran de la recherche de zones DNS privées.](media/security-access-control-troubleshoot-guide/private-dns-zones.png)
2. Vérifiez s’il existe un enregistrement A **adf**.
![Capture d’écran d’un enregistrement A.](media/security-access-control-troubleshoot-guide/a-record.png)
3.  Accédez à **Liens de réseau virtuel**, puis supprimez tous les enregistrements.
![Capture d’écran de lien de réseau virtuel.](media/security-access-control-troubleshoot-guide/virtual-network-link.png)
4.  Accédez à votre fabrique de données dans le portail Azure et recréez le point de terminaison privé pour le portail Azure Data Factory.
![Capture d’écran de la recréation d’un point de terminaison privé.](media/security-access-control-troubleshoot-guide/create-private-endpoint.png)
5.  Revenez à Zones DNS privées et vérifiez s’il existe une nouvelle zone DNS privée **privatelink.adf.azure.com**.
![Capture d’écran de nouvel enregistrement DNS.](media/security-access-control-troubleshoot-guide/check-dns-record.png)

### <a name="connection-error-in-public-endpoint"></a>Erreur de connexion dans un point de terminaison public

#### <a name="symptoms"></a>Symptômes

Lorsque vous copiez des données avec un accès public au compte de Stockage Blob Azure, les exécutions du pipeline échouent de façon aléatoire avec l’erreur suivante.

Par exemple : le récepteur de Stockage Blob Azure utilisait Azure IR (réseau virtuel public non géré) et, la source Azure SQL Database utilisait le runtime d’intégration de réseau virtuel managé. Ou bien la source ou le récepteur utilisent le runtime d’intégration de réseau virtuel managé uniquement avec un accès public au stockage.

`
<LogProperties><Text>Invoke callback url with req:
"ErrorCode=UserErrorFailedToCreateAzureBlobContainer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Unable to create Azure Blob container. Endpoint: XXXXXXX/, Container Name: test.,Source=Microsoft.DataTransfer.ClientLibrary,''Type=Microsoft.WindowsAzure.Storage.StorageException,Message=Unable to connect to the remote server,Source=Microsoft.WindowsAzure.Storage,''Type=System.Net.WebException,Message=Unable to connect to the remote server,Source=System,''Type=System.Net.Sockets.SocketException,Message=A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond public ip:443,Source=System,'","Details":null}}</Text></LogProperties>.
`

#### <a name="cause"></a>Cause

ADF peut toujours utiliser le runtime d’intégration de réseau virtuel managé, mais vous pouvez rencontrer une telle erreur parce que le point de terminaison public pour le Stockage Blob Azure dans un réseau virtuel managé n’est pas fiable compte tenu du résultat du test, et le Stockage Blob Azure et Azure Data Lake Gen2 ne sont pas pris en charge pour être connectés via un point de terminaison public à partir du réseau virtuel managé ADF comme expliqué dans [Réseau virtuel managé et points de terminaison privés managés](./managed-virtual-network-private-endpoint.md#outbound-communications-through-public-endpoint-from-adf-managed-virtual-network).

#### <a name="solution"></a>Solution

- Le point de terminaison privé étant activé sur la source et également côté récepteur lors de l’utilisation du runtime d’intégration de réseau virtuel managé.
- Si vous souhaitez toujours utiliser le point de terminaison public, vous pouvez basculer vers le runtime d’intégration public uniquement au lieu d’utiliser le runtime d’intégration de réseau virtuel managé pour la source et le récepteur. Même si vous revenez au runtime d’intégration public, ADF peut continuer à utiliser le runtime d’intégration de réseau virtuel managé si le runtime d’intégration de réseau virtuel managé est toujours présent.

### <a name="internal-error-while-trying-to-delete-adf-with-customer-managed-key-cmk-and-user-assigned-managed-identity-ua-mi"></a>Erreur interne lors de la tentative de suppression de ADF avec une clé gérée par le client (CMK) et une identité managée attribuée par l’utilisateur (UA-MI)

#### <a name="symptoms"></a>Symptômes
`{\"error\":{\"code\":\"InternalError\",\"message\":\"Internal error has occurred.\"}}`

#### <a name="cause"></a>Cause

Si vous effectuez une opération liée à CMK, vous devez d’abord effectuer toutes les opérations ADF associées, puis les opérations externes (telles que les identités managées ou les opérations Key Vault). Par exemple, si vous souhaitez supprimer toutes les ressources, vous devez d’abord supprimer la fabrique, puis le coffre de clés, si vous le faites dans un ordre différent, l’appel ADF échouera, car il ne pourra plus lire les objets associés et ne sera pas en mesure de vérifier si la suppression est possible ou non. 

#### <a name="solution"></a>Solution

Il existe trois moyens possibles de résoudre ce problème. Les voici :

* Vous avez révoqué l’accès ADF au coffre de clés dans lequel la clé CMK a été stockée. 
Vous pouvez réattribuer l’accès à Data Factory avec les autorisations suivantes : **Obtenir, Ne pas inclure la clé et Inclure la clé**. Ces autorisations sont requises pour activer des clés gérées par le client dans Data Factory. Reportez-vous à [Octroyer l’accès à ADF](enable-customer-managed-key.md#grant-data-factory-access-to-azure-key-vault). Une fois l’autorisation fournie, vous devriez pouvoir supprimer ADF.
 
* Le client a supprimé Key Vault/CMK avant de supprimer ADF. CMK dans ADF doit disposer des options « Suppression réversible » et « Protection contre la suppression définitive contre la purge » activées présentant par défaut la stratégie de rétention de 90 jours. Vous pouvez restaurer la clé supprimée.  
 Consultez [Récupérer une clé supprimée](../key-vault/general/key-vault-recovery.md?tabs=azure-portal#list-recover-or-purge-soft-deleted-secrets-keys-and-certificates) et [Valeur de la clé supprimée](../key-vault/general/key-vault-recovery.md?tabs=azure-portal#list-recover-or-purge-a-soft-deleted-key-vault).

* L’identité managée attribuée par l’utilisateur (UA-MI) a été supprimée avant ADF. Vous pouvez effectuer une récupération à l’aide d’appels d’API REST. Pour ce faire, vous pouvez utiliser le client http de votre choix dans n’importe quel langage de programmation. Si vous n’avez pas encore configuré d’appels d’API REST avec l’authentification Azure, le plus simple consiste à utiliser POSTMAN/Fiddler. Procédez comme suit.

   1.  Effectuez un appel GET vers la fabrique à l’aide de la méthode : GET Url like   `https://management.azure.com/subscriptions/YourSubscription/resourcegroups/YourResourceGroup/providers/Microsoft.DataFactory/factories/YourFactoryName?api-version=2018-06-01`

   2. Vous devez créer une identité managée par l’utilisateur avec un nom différent (le même nom peut fonctionner, mais il est plus sûr d’opter pour un autre nom que celui de la réponse GET).

   3. Modifiez les propriétés encryption.identity et identity.userassignedidentities de manière à ce qu’elles pointent vers l’identité managée nouvellement créée. Supprimez clientId et principalId de l’objet userAssignedIdentity. 

   4.  Effectuez un appel PUT vers la même URL de fabrique transmettant le nouveau corps. Il est très important que vous transmettiez ce que vous avez obtenu dans la réponse GET et ne modifiiez que l’identité. Dans le cas contraire, ces éléments remplaceront d’autres paramètres. 

   5.  Lorsque l’appel aboutit, vous pouvez voir les entités à nouveau et retenter la suppression. 

## <a name="sharing-self-hosted-integration-runtime"></a>Utilisation du runtime d’intégration auto-hébergé

### <a name="sharing-a-self-hosted-ir-from-a-different-tenant-is-not-supported"></a>Le partage de l’IR auto-hébergé à partir d’un autre locataire n’est pas pris en charge 

#### <a name="symptoms"></a>Symptômes

Vous pouvez remarquer d’autres fabriques de données (sur différents locataires) lors de la tentative de partage de l’IR auto-hébergé à partir de l’interface utilisateur Azure Data Factory, mais vous ne pouvez pas le partager entre les fabriques de données qui se trouvent sur des locataires différents.

#### <a name="cause"></a>Cause

L’IR auto-hébergé ne peut pas être partagé entre plusieurs locataires.


## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la résolution des problèmes, essayez les ressources suivantes :

*  [Private Link pour Data Factory](data-factory-private-link.md)
*  [Blog Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Demandes de fonctionnalités Data Factory](https://feedback.azure.com/forums/270578-data-factory)
*  [Vidéos Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Page Microsoft Q&A](/answers/topics/azure-data-factory.html)
*  [Forum Stack Overflow pour Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Informations Twitter sur Data Factory](https://twitter.com/hashtag/DataFactory)
