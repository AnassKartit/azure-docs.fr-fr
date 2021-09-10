---
title: Résoudre les problèmes liés aux flux de données de mappage
description: Découvrez comment résoudre les problèmes de flux de données dans Azure Data Factory.
ms.author: makromer
author: kromerm
ms.reviewer: daperlov
ms.service: data-factory
ms.subservice: data-flows
ms.topic: troubleshooting
ms.date: 08/18/2021
ms.openlocfilehash: 9925875f45f5715343ef50fff018b436966bb4c0
ms.sourcegitcommit: dcf1defb393104f8afc6b707fc748e0ff4c81830
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2021
ms.locfileid: "123099613"
---
# <a name="troubleshoot-mapping-data-flows-in-azure-data-factory"></a>Résoudre les problèmes liés aux flux de données de mappage dans Azure Data Factory

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Cet article présente des méthodes couramment employées pour résoudre les problèmes liés aux flux de données de mappage dans Azure Data Factory.

## <a name="common-error-codes-and-messages"></a>Messages et codes d’erreur courants 

### <a name="error-code-df-executor-sourceinvalidpayload"></a>Code d’erreur : DF-Executor-SourceInvalidPayload
- **Message** : L’exécution de l’aperçu des données, du débogage et du flux de données de pipeline a échoué car le conteneur n’existe pas
- **Cause** : Un jeu de données comprend un conteneur qui n’existe pas dans le stockage.
- **Recommandation** : Vérifiez que le conteneur référencé dans votre jeu de données existe et est accessible.

### <a name="error-code-df-executor-systeminvalidjson"></a>Code d’erreur : DF-Executor-SystemInvalidJson

- **Message** : Erreur d’analyse JSON, encodage ou multiligne non pris en charge.
- **Cause** : Problèmes possibles avec le fichier JSON (encodage non pris en charge, octets endommagés ou utilisation de la source JSON comme document unique sur de nombreuses lignes imbriquées).
- **Recommandation** : Vérifiez que l’encodage du fichier JSON est pris en charge. Sur la transformation source qui utilise un jeu de données JSON, développez **Paramètres JSON** et activez **Un seul document**.
 
### <a name="error-code-df-executor-broadcasttimeout"></a>Code d’erreur : DF-Executor-BroadcastTimeout

- **Message** : Erreur liée à l’expiration de la diffusion de la jointure. Vérifiez que la diffusion en streaming produit des données en moins de 60 s dans les exécutions de débogage et en moins de 300 s dans les exécutions de travaux.
- **Cause** : Le délai d’expiration par défaut de la diffusion est de 60 secondes pour les exécutions de débogage et de 300 secondes pour les exécutions de travaux. Le flux choisi pour la diffusion est trop volumineux pour produire des données respectant cette limite.
- **Recommandation** : Sous l’onglet **Optimiser**, vérifiez les transformations de jointure, d’existence et de recherche de votre flux de données. L’option par défaut pour la diffusion est **Auto**. Si l’option **Auto** est définie ou que vous indiquez manuellement la diffusion du côté gauche ou droit sous **Fixe**, vous pouvez soit définir une configuration de runtime d’intégration Azure plus importante, soit désactiver la diffusion. Pour obtenir des performances optimales dans les flux de données, nous vous recommandons d’autoriser Spark à diffuser à l’aide de l’option **Auto** et d’utiliser un runtime d’intégration Azure à mémoire optimisée. 
 
  Si vous exécutez le flux de données dans une exécution de test de débogage à partir d’une exécution de pipeline de débogage, vous pouvez rencontrer cette situation plus fréquemment. En effet, Azure Data Factory limite le délai d’expiration de diffusion à 60 secondes afin de maintenir une expérience de débogage plus rapide. Vous pouvez étendre ce délai d’expiration au délai de 300 secondes d’une exécution déclenchée. Pour ce faire, vous pouvez utiliser l’option **Déboguer** > **Utiliser le runtime d’activité** afin d’utiliser le runtime d’intégration Azure défini dans votre activité de pipeline Exécuter un flux de données.

- **Message** : Erreur de délai d’attente de la diffusion de la jonction. Pour éviter ce problème, vous pouvez choisir l’option de diffusion « Désactivé » dans la transformation de recherche/existence/jonction. Si vous envisagez de diffuser l’option de jonction pour améliorer les performances, assurez-vous que le flux de diffusion peut produire des données dans un délai de 60 secondes dans des exécutions de débogage et un délai de 300 secondes dans les exécutions de travaux.
- **Cause** : Le délai d’expiration par défaut de la diffusion est de 60 secondes dans les exécutions de débogage et de 300 secondes dans les exécutions de travaux. Dans la jointure de diffusion, le flux choisi pour la diffusion est trop volumineux pour produire des données respectant cette limite. Si une jointure de diffusion n’est pas utilisée, la diffusion par défaut effectuée par un flux de données peut atteindre la même limite.
- **Recommandation** : Désactivez l’option de diffusion ou évitez de diffuser des flux de données volumineux pour lesquels le traitement peut prendre plus de 60 secondes. Choisissez un flux plus petit à diffuser. Les fichiers sources et les tables Azure SQL Data Warehouse volumineux ne sont généralement pas de bons choix. En l’absence d’une jointure de diffusion, utilisez un cluster plus grand si cette erreur se produit.

### <a name="error-code-df-executor-conversion"></a>Code d’erreur : DF-Executor-Conversion

- **Message** : Échec de conversion en date ou heure en raison d’un caractère non valide
- **Cause** : Les données ne sont pas au format attendu.
- **Recommandation** : Utilisez le type de données correct.

### <a name="error-code-df-executor-invalidcolumn"></a>Code d’erreur : DF-Executor-InvalidColumn
- **Message** : Le nom de colonne doit être spécifié dans la requête, définissez un alias si vous utilisez une fonction SQL
- **Cause** : Aucun nom de colonne n’est spécifié.
- **Recommandation** : Définissez un alias si vous utilisez une fonction SQL telle que min() ou max().

### <a name="error-code-df-executor-drivererror"></a>Code d’erreur : DF-Executor-DriverError
- **Message** : INT96 est un type d’horodatage hérité qui n’est pas pris en charge par le flux de données ADF. Envisagez de mettre à niveau le type de colonne vers les types les plus récents.
- **Cause** : Erreur de pilote.
- **Recommandation** : INT96 est un type d’horodatage hérité qui n’est pas pris en charge par le flux de données Azure Data Factory. Envisagez de mettre à niveau le type de colonne vers le type le plus récent.

### <a name="error-code-df-executor-blockcountexceedslimiterror"></a>Code d’erreur : DF-Executor-BlockCountExceedsLimitError
- **Message** : Le nombre de blocs non validés ne peut pas dépasser la limite maximale de 100 000 blocs. Vérifiez la configuration de l’objet blob.
- **Cause** : Le nombre maximal de blocs non validés dans un objet blob est de 100 000.
- **Recommandation** : Pour plus d’informations sur ce problème, contactez l’équipe produit Microsoft.

### <a name="error-code-df-executor-partitiondirectoryerror"></a>Code d’erreur : DF-Executor-PartitionDirectoryError
- **Message** : Le chemin source spécifié comporte plusieurs répertoires partitionnés (par exemple, &lt;Chemin source&gt;/<Répertoire racine de partition 1>/a=10/b=20, &lt;Chemin source&gt;/&lt;Répertoire racine de partition 2&gt;/c=10/d=30) ou un répertoire partitionné avec un autre fichier ou un répertoire non partitionné (par exemple, &lt;Chemin source&gt;/&lt;Répertoire racine de partition 1&gt;/a=10/b=20, &lt;Chemin source&gt;/Répertoire 2/fichier1). Supprimez le répertoire racine de la partition du chemin source et lisez-le par le biais d’une transformation source distincte.
- **Cause** : Le chemin source comporte plusieurs répertoires partitionnés ou un répertoire partitionné avec un autre fichier ou un répertoire non partitionné.
- **Recommandation** : Supprimez le répertoire racine partitionné du chemin source et lisez-le par le biais d’une transformation source distincte.

### <a name="error-code-df-executor-invalidtype"></a>Code d’erreur : DF-Executor-InvalidType
- **Message** : Vérifiez que le type de paramètre correspond au type de valeur transmis. Le passage de paramètres flottants à partir de pipelines n’est pas pris en charge actuellement.
- **Cause** : Les types de données sont incompatibles entre le type déclaré et la valeur réelle du paramètre.
- **Recommandation** : Vérifiez que les valeurs de paramètre passées dans le flux de données correspondent au type déclaré.

### <a name="error-code-df-executor-parseerror"></a>Code d’erreur : DF-Executor-ParseError
- **Message** : Impossible d’analyser l’expression.
- **Cause** : Une expression a généré des erreurs d’analyse en raison d’une mise en forme incorrecte.
- **Recommandation** : Vérifiez la mise en forme dans l’expression.

### <a name="error-code-df-executor-systemimplicitcartesian"></a>Code d’erreur : DF-Executor-SystemImplicitCartesian
- **Message** : Le produit cartésien implicite pour la jointure INNER n’est pas pris en charge. Utilisez plutôt CROSS JOIN. Les colonnes utilisées dans la jointure doivent créer une clé unique pour les lignes.
- **Cause** : Les produits cartésiens implicites pour les jointures INNER entre les plans logiques ne sont pas pris en charge. Si vous utilisez des colonnes dans la jointure, créez une clé unique.
- **Recommandation** : Pour les jointures non basées sur l’égalité, utilisez CROSS JOIN.

### <a name="error-code-getcommand-outputasync-failed"></a>Code d’erreur : Échec de GetCommand OutputAsync
- **Message** : Lors du débogage du flux de données et de l’aperçu des données : Échec de GetCommand OutputAsync avec...
- **Cause** : Cette erreur est une erreur de service back-end. 
- **Recommandation** : Retentez l’opération et redémarrez votre session de débogage. Si la nouvelle tentative et le redémarrage ne résolvent pas le problème, contactez le support technique. 

### <a name="error-code-df-executor-outofmemoryerror"></a>Code d’erreur : DF-Executor-OutOfMemoryError
 
- **Message** : Le cluster a rencontré un problème de mémoire insuffisante pendant l’exécution, réessayez en utilisant un runtime d’intégration avec un nombre de cœurs plus élevé et/ou un type de calcul à mémoire optimisée.
- **Cause** : Le cluster manque de mémoire.
- **Recommandation** : Les clusters de débogage sont destinés au développement. Utilisez l’échantillonnage des données ainsi que le type et la taille de calcul appropriés pour exécuter la charge utile. Pour obtenir des conseils sur les performances, consultez le [guide des performances de flux de données de mappage](concepts-data-flow-performance.md).

### <a name="error-code-df-executor-illegalargument"></a>Code d’erreur : DF-Executor-illegalArgument

- **Message** : Vérifiez que la clé d’accès de votre service lié est correcte.
- **Cause** : Le nom du compte ou la clé d’accès est incorrect.
- **Recommandation** : Vérifiez que le nom du compte ou la clé d'accès spécifiés dans votre service lié sont corrects. 

### <a name="error-code-df-executor-columnunavailable"></a>Code d’erreur : DF-Executor-ColumnUnavailable
- **Message** : Le nom de colonne utilisé dans l’expression n’est pas disponible ou n’est pas valide.
- **Cause** : Un nom de colonne non valide ou non disponible est utilisé dans une expression.
- **Recommandation** : Vérifiez les noms de colonne utilisés dans les expressions.

 ### <a name="error-code-df-executor-outofdiskspaceerror"></a>Code d’erreur : DF-Executor-OutOfDiskSpaceError
- **Message** : Erreur interne du serveur
- **Cause** : Le cluster manque d’espace disque.
- **Recommandation** : Relancez le pipeline. Si cela ne résout pas le problème, contactez le support technique.


 ### <a name="error-code-df-executor-storeisnotdefined"></a>Code d’erreur : DF-Executor-StoreIsNotDefined
- **Message** : La configuration de magasin n’est pas définie. Cette erreur est peut-être provoquée par une attribution incorrecte de paramètre dans le pipeline.
- **Cause** : La configuration fournie pour le magasin n'est pas valide.
- **Recommandation** : Vérifiez l’attribution de la valeur du paramètre dans le pipeline. Une expression de paramètre peut contenir des caractères non valides.


### <a name="error-code-4502"></a>Code d’erreur : 4502
- **Message** : Des exécutions de MappingDataflow simultanées importantes entraînent des échecs en raison de la limitation sous le runtime d’intégration.
- **Cause** : Un grand nombre d’exécutions d’activités de flux de données se produisent simultanément sur le runtime d’intégration. Pour plus d’informations, consultez [Limites d’Azure Data Factory](../azure-resource-manager/management/azure-subscription-service-limits.md#data-factory-limits).
- **Recommandation** : Si vous souhaitez exécuter davantage d’activités de flux de données en parallèle, répartissez-les sur plusieurs runtimes d’intégration.

### <a name="error-code-4510"></a>Code d’erreur : 4510
- **Message** : Défaillance inattendue pendant l’exécution. 
- **Cause** : Étant donné que les clusters de débogage fonctionnent différemment des clusters de travail, les exécutions de débogage excessives peuvent affecter le cluster dans le temps, ce qui peut provoquer des problèmes de mémoire et des redémarrages soudains.
- **Recommandation** : Redémarrez le cluster de débogage. Si vous exécutez plusieurs flux de données pendant la session de débogage, utilisez plutôt des exécutions d’activités, car l’exécution du niveau d’activité crée une session distincte sans imposer un cluster de débogage principal.

### <a name="error-code-invalidtemplate"></a>Code d’erreur : InvalidTemplate
- **Message** : L’expression de pipeline ne peut pas être évaluée.
- **Cause** : L’expression de pipeline passée dans l’activité de flux de données n’est pas traitée correctement en raison d’une erreur de syntaxe.
- **Recommandation** : Dans la supervision des activités, vérifiez l’expression liée à votre activité.

### <a name="error-code-2011"></a>Code d’erreur : 2011
- **Message** : L’activité était exécutée sur Azure Integration Runtime et n’a pas pu déchiffrer les informations d’identification du magasin de données ou du calcul connecté via un runtime d’intégration auto-hébergé. Vérifiez la configuration des services liés associés à cette activité et veillez à utiliser le type de runtime d’intégration approprié.
- **Cause** : Le flux de données ne prend pas en charge les services liés sur les runtimes d’intégration auto-hébergés.
- **Recommandation** : Configurez le flux de données pour qu’il s’exécute sur un runtime d’intégration de réseau virtuel géré.

### <a name="error-code-df-xml-invalidvalidationmode"></a>Code d’erreur : DF-XML-InvalidValidationMode
- **Message** : le mode de validation XML fourni n’est pas valide.
- **Cause** : Le mode de validation XML fourni n'est pas valide.
- **Recommandation** : vérifiez la valeur du paramètre et indiquez le mode de validation approprié.

### <a name="error-code-df-xml-invaliddatafield"></a>Code d’erreur : DF-XML-InvalidDataField
- **Message** : le champ pour les enregistrements endommagés doit être de type chaîne et pouvant accepter la valeur Null.
- **Cause** : Le type de données fourni pour la colonne `\"_corrupt_record\"` dans la source XML n'est pas valide.
- **Recommandation** : assurez-vous que la colonne `\"_corrupt_record\"` de la source XML est définie sur le type de données String et qu'elle peut accepter les valeurs Null.

### <a name="error-code-df-xml-malformedfile"></a>Code d’erreur : DF-XML-MalformedFile
- **Message** : Fichier XML malformé avec chemin en mode FAILFAST.
- **Cause** : Existence d'un fichier XML malformé avec chemin en mode FAILFAST.
- **Recommandation** : mettez à jour le contenu du fichier XML au format approprié.

### <a name="error-code-df-xml-invalidreferenceresource"></a>Code d’erreur : DF-XML-InvalidReferenceResource
- **Message** : La ressource de référence du fichier de données XML ne peut pas être résolue.
- **Cause** : La ressource de référence du fichier de données XML ne peut pas être résolue.
- **Recommandation** : vous devez vérifier la ressource de référence dans le fichier de données XML.

### <a name="error-code-df-xml-invalidschema"></a>Code d’erreur : DF-Xml-InvalidSchema
- **Message :** La validation du schéma a échoué.
- **Cause** : Le schéma fourni dans la source XML n'est pas valide.
- **Recommandation** : Vérifiez les paramètres du schéma au niveau de la source XML pour vous assurer qu'il s'agit du schéma de sous-ensemble des données source.

### <a name="error-code-df-xml-unsupportedexternalreferenceresource"></a>Code d’erreur : DF-XML-UnsupportedExternalReferenceResource
- **Message** : la ressource de référence externe dans le fichier de données XML n’est pas prise en charge.
- **Cause** : La ressource de référence externe du fichier de données XML n'est pas prise en charge.
- **Recommandation** : mettez à jour le contenu du fichier XML lorsque la ressource de référence externe n’est pas prise en charge.

### <a name="error-code-df-gen2-invalidaccountconfiguration"></a>Code d’erreur : DF-GEN2-InvalidAccountConfiguration
- **Message** : vous devez spécifier l’une des clés de compte ou client/SpnId/SpnCredential/SpnCredentialType ou MiServiceUri/miServiceToken.
- **Cause** : Les informations d'identification fournies dans le service lié ADLS Gen2 ne sont pas valides.
- **Recommandation** : Mettez à jour le service lié ADLS Gen2 afin de bénéficier de la configuration appropriée pour les informations d'identification.

### <a name="error-code-df-gen2-invalidauthconfiguration"></a>Code d’erreur : DF-GEN2-InvalidAuthConfiguration
- **Message** : seule une des trois méthodes d’authentification (Clé, ServicePrincipal et MI) peut être utilisée.
- **Cause** : La méthode d'authentification fournie dans le service lié ADLS Gen2 n'est pas valide.
- **Recommandation** : Mettez à jour le service lié ADLS Gen2 pour bénéficier d'une des trois méthodes d'authentification disponibles (par clé, ServicePrincipal et MI).

### <a name="error-code-df-gen2-invalidserviceprincipalcredentialtype"></a>Code d’erreur : DF-GEN2-InvalidServicePrincipalCredentialType
- **Message** : Type d'informations d'identification du principal de service non valide.
- **Cause** : Le type d'informations d'identification du principal de service n'est pas valide.
- **Recommandation** : Mettez à jour le service lié ADLS Gen2 afin de définir le type d'informations d'identification approprié pour le principal de service.

### <a name="error-code-df-blob-invalidaccountconfiguration"></a>Code d’erreur : DF-Blob-InvalidAccountConfiguration
- **Message** : Une clé de compte ou un jeton SAS doit être spécifié.
- **Cause** : Les informations d'identification fournies dans le service lié Azure Blob ne sont pas valides.
- **Recommandation** : Utilisez une clé de compte ou un jeton SAS pour le service lié Azure Blob.

### <a name="error-code-df-blob-invalidauthconfiguration"></a>Code d’erreur : DF-Blob-InvalidAuthConfiguration
- **Message** : vous pouvez spécifier une seule des deux méthodes d’authentification (Clé, SAS).
- **Cause** : La méthode d'authentification fournie dans le service lié n'est pas valide.
- **Recommandation** : Utilisez l'authentification par clé ou SAS pour le service lié Azure Blob.

### <a name="error-code-df-cosmos-partitionkeymissed"></a>Code d’erreur : DF-Cosmos-PartitionKeyMissed
- **Message** : le chemin d’accès de la clé de partition doit être indiqué pour les opérations de mise à jour et de suppression.
- **Cause** : Le chemin de la clé de partition est manquant dans le récepteur Azure Cosmos DB.
- **Recommandation** : Utilisez la clé de partition fournie dans les paramètres du récepteur Azure Cosmos DB.

### <a name="error-code-df-cosmos-invalidpartitionkey"></a>Code d’erreur : DF-Cosmos-InvalidPartitionKey
- **Message** : le chemin de la clé de partition ne peut pas être vide pour les opérations de mise à jour et de suppression.
- **Cause** : Le chemin de la clé de partition est vide pour les opérations de mise à jour et de suppression.
- **Recommandation** : Utilisez la clé de partition fournie dans les paramètres du récepteur Azure Cosmos DB.


### <a name="error-code-df-cosmos-idpropertymissed"></a>Code d’erreur : DF-Cosmos-IdPropertyMissed
- **Message** : la propriété « ID » doit être mappée pour les opérations de suppression et de mise à jour.
- **Cause** : La propriété `id` est manquante pour les opérations de mise à jour et de suppression.
- **Recommandation** : Assurez-vous que les données d'entrée comportent une colonne `id` dans les paramètres du récepteur Cosmos DB. Si ce n’est pas le cas, utilisez **sélectionner ou déduire la transformation** pour générer cette colonne avant le récepteur.

### <a name="error-code-df-cosmos-invalidpartitionkeycontent"></a>Code d’erreur : DF-Cosmos-InvalidPartitionKeyContent
- **Message** : la clé de partition doit commencer par /.
- **Cause** : La clé de partition fournie n'est pas valide.
- **Recommandation** : Veillez à ce que la clé de partition commence par `/` dans les paramètres du récepteur Cosmos DB (par exemple, `/movieId`).

### <a name="error-code-df-cosmos-invalidpartitionkey"></a>Code d’erreur : DF-Cosmos-InvalidPartitionKey
- **Message** : La clé de partition n'est pas mappée dans le récepteur pour les opérations de suppression et de mise à jour.
- **Cause** : La clé de partition fournie n'est pas valide.
- **Recommandation** : Dans les paramètres du récepteur Cosmos DB, utilisez la même clé de partition que celle de votre conteneur.

### <a name="error-code-df-cosmos-invalidconnectionmode"></a>Code d’erreur : DF-Cosmos-InvalidConnectionMode
- **Message** : Mode de connexion non valide.
- **Cause** : Le mode de connexion fourni n'est pas valide.
- **Recommandation** : Vérifiez que le mode pris en charge est **Gateway** et **DirectHttps** dans les paramètres Cosmos DB.

### <a name="error-code-df-cosmos-invalidaccountconfiguration"></a>Code d’erreur : DF-Cosmos-InvalidAccountConfiguration
- **Message** : Les éléments AccountName ou accountEndpoint doivent être spécifiés.
- **Cause** : Les informations de compte fournies ne sont pas valides.
- **Recommandation** : Dans le service lié Cosmos DB, spécifiez le nom ou le point de terminaison du compte.

### <a name="error-code-df-github-writenotsupported"></a>Code d’erreur : DF-GitHub-WriteNotSupported
- **Message** : le stockage GitHub n’autorise pas les écritures.
- **Cause** : Le magasin GitHub est en lecture seule.
- **Recommandation** : La définition de l'entité magasin se trouve à un autre emplacement.
  
### <a name="error-code-df-pgsql-invalidcredential"></a>Code d’erreur : DF-PGSQL-InvalidCredential
- **Message** : L’utilisateur et le mot de passe doivent être spécifiés.
- **Cause** : L'utilisateur/mot de passe est manquant.
- **Recommandation** : Assurez-vous que vous disposez des paramètres d'identification qui conviennent dans le service lié PostgreSQL associé.

### <a name="error-code-df-snowflake-invalidstageconfiguration"></a>Code d’erreur : DF-Snowflake-InvalidStageConfiguration
- **Message** : seul le type de stockage BLOB peut être utilisé en tant qu’étape de lecture/écriture en flocon.
- **Cause** : La configuration de la mise en lots fournie dans Snowflake n'est pas valide.
- **Recommandation** : Mettez à jour les paramètres de mise en lots de Snowflake pour veiller à ce que seul le service lié Azure Blob soit utilisé.

### <a name="error-code-df-snowflake-invalidstageconfiguration"></a>Code d’erreur : DF-Snowflake-InvalidStageConfiguration
- **Message** : Les propriétés de l’index Snowflake doivent être spécifiées avec l’authentification Azure Blob + SAS.
- **Cause** : La configuration de la mise en lots fournie dans Snowflake n'est pas valide.
- **Recommandation** : Veillez à ce que seule l'authentification Azure Blob + SAS soit spécifiée dans les paramètres de mise en lots de Snowflake.

### <a name="error-code-df-snowflake-invaliddatatype"></a>Code d’erreur : DF-Snowflake-InvalidDataType
- **Message** : le type Spark n’est pas pris en charge dans le flocon.
- **Cause** : Le type de données fourni dans Snowflake n'est pas valide.
- **Recommandation** : Utilisez la transformation de dérivation avant d'appliquer le récepteur Snowflake pour mettre à jour la colonne associée des données d'entrée dans le type String.

### <a name="error-code-df-hive-invalidblobstagingconfiguration"></a>Code d’erreur : DF-Hive-InvalidBlobStagingConfiguration
- **Message** : les propriétés de mise en lots du stockage d’objets BLOB doivent être spécifiées.
- **Cause** : La configuration de la mise en lots fournie dans Hive n'est pas valide.
- **Recommandation** : Vérifiez que la clé de compte, le nom du compte et le conteneur sont correctement définis dans le service lié Blob associé qui est utilisé pour la mise en lots.

### <a name="error-code-df-hive-invalidgen2stagingconfiguration"></a>Code d’erreur : DF-Hive-InvalidGen2StagingConfiguration
- **Message** : la mise en lots du stockage ADLS Gen2 ne prend en charge que les informations d’identification de la clé principale du service.
- **Cause** : La configuration de la mise en lots fournie dans Hive n'est pas valide.
- **Recommandation** : Mettez à jour le service lié ADLS Gen2 associé qui est utilisé pour la mise en lots. Actuellement, seules les informations d'identification de la clé du principal de service sont prises en charge.

### <a name="error-code-df-hive-invalidgen2stagingconfiguration"></a>Code d’erreur : DF-Hive-InvalidGen2StagingConfiguration
- **Message** : les propriétés de mise en lots du stockage d’objets ADLS Gen2 doivent être spécifiées. L’une des clés tenant/spnId/spnKey ou miServiceUri/miServiceToken est requise.
- **Cause** : La configuration de la mise en lots fournie dans Hive n'est pas valide.
- **Recommandation** : Mettez à jour le service lié ADLS Gen2 associé avec les informations d'identification appropriées utilisées pour la mise en lots dans Hive.

### <a name="error-code-df-hive-invaliddatatype"></a>Code d’erreur : DF-Hive-InvalidDataType
- **Message** : colonne(s) non prise(s) en charge.
- **Cause** : La ou les colonnes fournies ne sont pas prises en charge.
- **Recommandation** : Mettez à jour la colonne de données d'entrée pour qu'elle corresponde au type de données pris en charge par Hive.

### <a name="error-code-df-hive-invalidstoragetype"></a>Code d’erreur : DF-Hive-InvalidStorageType
- **Message** : le type de stockage peut être un objet BLOB ou un Gen2.
- **Cause** : Seul le type de stockage Azure Blob ou ADLS Gen2 est pris en charge.
- **Recommandation** : Choisissez le type de stockage qui convient à partir d'Azure Blob ou d'ADLS Gen2.

### <a name="error-code-df-delimited-invalidconfiguration"></a>Code d’erreur : DF-Delimited-InvalidConfiguration
- **Message** : vous devez spécifier l’une des lignes vides ou un en-tête personnalisé.
- **Cause** : La configuration délimitée fournie n'est pas valide.
- **Recommandation** : Mettez à jour les paramètres CSV pour spécifier une des lignes vides ou l'en-tête personnalisé.

### <a name="error-code-df-delimited-columndelimitermissed"></a>Code d’erreur : DF-Delimited-ColumnDelimiterMissed
- **Message** : Délimiteur de colonne requis pour l'analyse.
- **Cause** : Le délimiteur de colonne est manquant.
- **Recommandation** : Dans vos paramètres CSV, vérifiez que vous disposez du délimiteur de colonne requis pour l'analyse. 

### <a name="error-code-df-mssql-invalidcredential"></a>Code d’erreur : DF-MSSQL-InvalidCredential
- **Message**: l’un des uuser/pwd ou tenant/SpnId/SpnKey ou MiServiceUri/miServiceToken doit être spécifié.
- **Cause** : Les informations d'identification fournies dans le service lié MSSQL ne sont pas valides.
- **Recommandation** : Mettez à jour le service lié MSSQL associé avec les informations d'identification appropriées. Une des autorisations **user/pwd**, **tenant/spnId/spnKey** ou **miServiceUri/miServiceToken** doit aussi être spécifiée.

### <a name="error-code-df-mssql-invaliddatatype"></a>Code d’erreur : DF-MSSQL-InvalidDataType
- **Message** : champ(s) non pris en charge.
- **Cause** : Un ou plusieurs champs fournis ne sont pas pris en charge.
- **Recommandation** : modifiez la colonne de données d’entrée pour qu’elle corresponde au type de données pris en charge par MSSQL.

### <a name="error-code-df-mssql-invalidauthconfiguration"></a>Code d’erreur : DF-MSSQL-InvalidAuthConfiguration
- **Message** : seule une des trois méthodes d’authentification (Clé, ServicePrincipal et MI) peut être utilisée.
- **Cause** : La méthode d'authentification fournie dans le service lié MSSQL n'est pas valide.
- **Recommandation** : Vous ne pouvez spécifier qu'une des trois méthodes d'authentification disponibles (par clé, ServicePrincipal ou MI) dans le service lié MSSQL associé.

### <a name="error-code-df-mssql-invalidcloudtype"></a>Code d’erreur : DF-MSSQL-InvalidCloudType
- **Message** : le type de Cloud n’est pas valide.
- **Cause** : Le type de cloud fourni n'est pas valide.
- **Recommandation** : Vérifiez votre type de Cloud dans le service lié MSSQL associé.

### <a name="error-code-df-sqldw-invalidblobstagingconfiguration"></a>Code d’erreur : DF-SQLDW-InvalidBlobStagingConfiguration
- **Message** : les propriétés de mise en lots du stockage d’objets BLOB doivent être spécifiées.
- **Cause** : Les paramètres de mise en lots du stockage d'objets blob fournis ne sont pas valides.
- **Recommandation** : Vérifiez que les propriétés du service lié Blob utilisé pour la mise en lots sont correctes.

### <a name="error-code-df-sqldw-invalidstoragetype"></a>Code d’erreur : DF-SQLDW-InvalidStorageType
- **Message** : le type de stockage peut être un objet BLOB ou un Gen2.
- **Cause** : Le type de stockage fourni pour la mise en lots n'est pas valide.
- **Recommandation** : Vérifiez que le type de stockage du service lié utilisé pour la mise en lots est bien Blob ou Gen2.

### <a name="error-code-df-sqldw-invalidgen2stagingconfiguration"></a>Code d’erreur : DF-SQLDW-InvalidGen2StagingConfiguration
- **Message** : la mise en lots du stockage ADLS Gen2 ne prend en charge que les informations d’identification de la clé principale du service.
- **Cause** : Les informations d'identification fournies pour la mise en lots du stockage ADLS Gen2 ne sont pas valides.
- **Recommandation** : Utilisez les informations d'identification de la clé de principal de service du service lié Gen2 utilisé pour la mise en lots.
 

### <a name="error-code-df-sqldw-invalidconfiguration"></a>Code d’erreur : DF-SQLDW-InvalidConfiguration
- **Message** : les propriétés de mise en lots du stockage d’objets ADLS Gen2 doivent être spécifiées. L’une des clés ou tenant/spnId/spnCredential/spnCredentialType ou miServiceUri/miServiceToken est requise.
- **Cause** : Les propriétés de mise en lots ADLS Gen2 fournies ne sont pas valides.
- **Recommandation** : Mettez à jour les paramètres de mise en lots du stockage ADLS Gen2 pour bénéficier d'une des trois méthodes d'authentification disponibles (par **clé**, **tenant/spnId/spnCredential/spnCredentialType** ou **miServiceUri/miServiceToken**).

### <a name="error-code-df-delta-invalidconfiguration"></a>Code d’erreur : DF-DELTA-InvalidConfiguration
- **Message** : le timestamp et la version ne peuvent pas être définis en même temps.
- **Message** : Le timestamp et la version ne peuvent pas être définis en même temps.
- **Recommandation** : Définissez le timestamp ou la version dans les paramètres delta.

### <a name="error-code-df-delta-keycolumnmissed"></a>Code d’erreur : DF-DELTA-KeyColumnMissed
- **Message** : la ou les colonnes clés doivent être spécifiées pour les opérations qui ne peuvent pas être insérées.
- **Cause** : La ou les colonnes clés sont manquantes pour les opérations non insérables.
- **Recommandation** : Spécifiez une ou plusieurs colonnes clés sur le récepteur delta pour obtenir des opérations non insérables.

### <a name="error-code-df-delta-invalidtableoperationsettings"></a>Code d’erreur : DF-DELTA-InvalidTableOperationSettings
- **Message** : les options Recréer et Tronquer ne peuvent pas être indiquées en même temps.
- **Cause** : Les options Recréer et Tronquer ne peuvent pas être spécifiées simultanément.
- **Recommandation** : Mettez à jour les paramètres delta pour bénéficier d'une opération Recréer ou Tronquer.

### <a name="error-code-df-excel-worksheetconfigmissed"></a>Code d’erreur : DF-Excel-WorksheetConfigMissed
- **Message** : Le nom ou l’index de la feuille de calcul Excel est requis.
- **Cause** : La configuration fournie pour la feuille de calcul Excel n'est pas valide.
- **Recommandation** : Vérifiez la valeur du paramètre et spécifiez le nom ou l'index de la feuille pour lire les données Excel.

### <a name="error-code-df-excel-invalidworksheetconfiguration"></a>Code d’erreur : DF-Excel-InvalidWorksheetConfiguration
- **Message** : Le nom et l’index d’une feuille Excel ne peuvent pas exister en même temps.
- **Cause** : Le nom et l'index de la feuille Excel sont fournis en même temps.
- **Recommandation** : Vérifiez la valeur du paramètre et spécifiez le nom ou l'index de la feuille pour lire les données Excel.

### <a name="error-code-df-excel-invalidrange"></a>Code d’erreur : DF-Excel-InvalidRange
- **Message** : Une plage non valide est fournie.
- **Cause** : La plage fournie n'est pas valide.
- **Recommandation** : vérifiez la valeur du paramètre et indiquez la plage valide à l’aide de la référence suivante : [format Excel dans les propriétés d’Azure Data Factory-Dataset](./format-excel.md#dataset-properties).

### <a name="error-code-df-excel-worksheetnotexist"></a>Code d’erreur : DF-Excel-WorksheetNotExist
- **Message** : La feuille de calcul Excel n’existe pas.
- **Cause** : Le nom ou l'index fourni pour la feuille de calcul n'est pas valide.
- **Recommandation** : Vérifiez la valeur du paramètre et spécifiez un nom ou un index valide pour la feuille afin de lire les données Excel.

### <a name="error-code-df-excel-differentschemanotsupport"></a>Code d’erreur : DF-Excel-DifferentSchemaNotSupport
- **Message** : la lecture des fichiers Excel avec un schéma différent n’est pas prise en charge pour le moment.
- **Cause** : La lecture de fichiers Excel à l'aide d'autres schémas n'est pas prise en charge pour le moment.
- **Recommandation** : Appliquez une des options suivantes pour résoudre ce problème :
    1. Utilisez l'activité de flux de données **ForEach** + pour lire les feuilles de calcul Excel une par une. 
    1. Mettez à jour manuellement le schéma de chaque feuille de calcul pour avoir les mêmes colonnes avant de lire les données.

### <a name="error-code-df-excel-invaliddatatype"></a>Code d’erreur : DF-Excel-InvalidDataType
- **Message** : Type de données non pris en charge.
- **Cause** : Le type de données n'est pas pris en charge.
- **Recommandation** : Remplacez le type de données par **« string »** pour les colonnes de données d'entrée associées.

### <a name="error-code-df-excel-invalidfile"></a>Code d’erreur : DF-Excel-InvalidFile
- **Message** : un fichier Excel non valide est fourni alors que seuls les fichiers .xlsx et .xls sont pris en charge.
- **Cause** : Les fichiers Excel fournis ne sont pas valides.
- **Recommandation** : Utilisez le caractère générique pour filtrer et récupérer les fichiers Excel `.xls` et `.xlsx` avant de lire les données.

### <a name="error-code-df-executor-outofmemorysparkbroadcasterror"></a>Code d'erreur : DF-Executor-OutOfMemorySparkBroadcastError
- **Message** : Le jeu de données diffusé explicitement à l'aide de l’option gauche/droite doit être suffisamment petit pour tenir dans la mémoire du nœud. Pour contourner ce problème, vous pouvez choisir l'option de diffusion « Désactivé » dans la transformation de recherche/existence/jonction, ou utiliser un runtime d'intégration avec plus de mémoire.
- **Cause** : La taille de la table diffusée dépasse de loin la limite de mémoire du nœud.
- **Recommandation** : L'option de diffusion gauche/droite ne doit être utilisée que pour les jeux de données de petite taille qui peuvent tenir dans la mémoire du nœud. Veillez donc à configurer la taille du nœud de manière appropriée ou à désactiver l'option de diffusion.

### <a name="error-code-df-mssql-invalidfirewallsetting"></a>Code d'erreur : DF-MSSQL-InvalidFirewallSetting
- **Message** : La connexion TCP/IP à l'hôte a échoué. Assurez-vous qu’une instance de SQL Server est en cours d’exécution sur l’hôte et accepte les connexions TCP/IP au port. Vérifiez que les connexions TCP au port ne sont pas bloquées par un pare-feu.
- **Cause** : Le paramètre de pare-feu de la base de données SQL bloque l'accès au flux de données.
- **Recommandation** : Vérifiez le paramètre de pare-feu de votre base de données SQL, et autorisez les services et ressources Azure à accéder à ce serveur.

### <a name="error-code-df-executor-acquirestoragememoryfailed"></a>Code d'erreur : DF-Executor-AcquireStorageMemoryFailed
- **Message** : Le transfert de la mémoire de déroulement vers la mémoire de stockage a échoué. Le cluster a manqué de mémoire pendant l'exécution. Veuillez réessayer en utilisant un runtime d'intégration avec plus de cœurs et/ou un type de calcul à mémoire optimisée.
- **Cause** : Le cluster n'a pas assez de mémoire.
- **Recommandation** : Utilisez un runtime d'intégration avec plus de cœurs et/ou le type de calcul à mémoire optimisée.

### <a name="error-code-df-cosmos-deletedatafailed"></a>Code d'erreur : DF-Cosmos-DeleteDataFailed
- **Message** : Échec de la suppression des données de Cosmos après 3 tentatives.
- **Cause** : Le débit sur la collection Cosmos est faible, et entraîne une limitation ou mène à des données de ligne qui n'existent pas dans Cosmos.
- **Recommandation** : Procédez comme suit pour résoudre ce problème :
    1. En cas d'erreur 404, vérifiez que les données de la ligne associée existent dans la collection Cosmos. 
    1. En cas d'erreur de limitation, augmentez le débit de la collection Cosmos ou définissez-le sur Mise à l'échelle automatique.
    1. E cas d’erreur d’expiration de la requête, définissez « Taille du lot » dans le récepteur Cosmos sur une valeur plus petite, par exemple 1000.

### <a name="error-code-df-sqldw-errorrowsfound"></a>Code d'erreur : DF-SQLDW-ErrorRowsFound
- **Cause** : Des lignes d'erreur/non valides ont été trouvées lors de l'écriture dans le récepteur Azure Synapse Analytics.
- **Recommandation** : Recherchez les lignes d'erreur à l'emplacement de stockage des données rejetées, si un tel emplacement est configuré.

### <a name="error-code-df-sqldw-exporterrorrowfailed"></a>Code d'erreur : DF-SQLDW-ExportErrorRowFailed
- **Message** : Une exception s'est produite lors de l'écriture des lignes d'erreur dans le stockage.
- **Cause** : Une exception s'est produite lors de l'écriture des lignes d'erreur dans le stockage.
- **Recommandation** : Vérifiez la configuration du service lié des données rejetées.

### <a name="error-code-df-executor-fieldnotexist"></a>Code d'erreur : DF-Executor-FieldNotExist
- **Message** : Champ inexistant dans la structure.
- **Cause** : Des noms de champs non valides ou non disponibles sont utilisés dans les expressions.
- **Recommandation** : Vérifiez les noms de champs utilisés dans les expressions.

### <a name="error-code-df-xml-invalidelement"></a>Code d'erreur : DF-Xml-InvalidElement
- **Message** : L'élément XML comporte des sous-éléments ou des attributs qui ne peuvent pas être convertis.
- **Cause** : L'élément XML comporte des sous-éléments ou des attributs qui ne peuvent pas être convertis.
- **Recommandation** : Mettez à jour le fichier XML pour que l'élément XML bénéficie des sous-éléments ou des attributs appropriés.

### <a name="error-code-df-gen2-invalidcloudtype"></a>Code d'erreur : DF-GEN2-InvalidCloudType
- **Message** : le type de Cloud n’est pas valide.
- **Cause** : Le type de cloud fourni n'est pas valide.
- **Recommandation** : Vérifiez le type de cloud dans le service lié ADLS Gen2 associé.

### <a name="error-code-df-blob-invalidcloudtype"></a>Code d'erreur : DF-Blob-InvalidCloudType
- **Message** : le type de Cloud n’est pas valide.
- **Cause** : Le type de cloud fourni n'est pas valide.
- **Recommandation** : Vérifiez le type de cloud dans le service lié Azure Blob associé.

### <a name="error-code-df-cosmos-failtoresetthroughput"></a>Code d'erreur : DF-Cosmos-FailToResetThroughput
- **Message** : L'opération de mise à l'échelle du débit de Cosmos DB ne peut être effectuée car une autre opération de mise à l'échelle est en cours. Réessayez ultérieurement.
- **Cause** : L'opération de mise à l'échelle du débit de Cosmos DB ne peut pas être effectuée, car une autre opération de mise à l'échelle est en cours.
- **Recommandation** : Connectez-vous à votre compte Cosmos et modifiez manuellement le débit de son conteneur pour qu'il soit mis à l'échelle automatiquement, ou ajoutez des activités personnalisées après les flux de données pour réinitialiser le débit.

### <a name="error-code-df-executor-invalidpath"></a>Code d'erreur : DF-Executor-InvalidPath
- **Message** : Le chemin ne mène à aucun fichier. Assurez-vous que le fichier/dossier existe et qu'il n'est pas masqué.
- **Cause** : Un chemin d'accès à un fichier/dossier non valide est fourni. Celui-ci est introuvable ou inaccessible.
- **Recommandation** : Vérifiez le chemin d'accès au fichier/dossier, et assurez-vous qu'il existe et qu'il est accessible dans votre stockage.

### <a name="error-code-df-executor-invalidpartitionfilenames"></a>Code d'erreur : DF-Executor-InvalidPartitionFileNames
- **Message** : Les noms de fichiers ne peuvent pas comporter de valeur(s) vide(s) lorsque l'option de nom de fichier est définie selon la partition.
- **Cause** : Des noms de fichiers de partition non valides sont fournis.
- **Recommandation** : Vérifiez que les paramètres de votre récepteur sont définis sur les valeurs de noms de fichiers appropriées.

### <a name="error-code-df-executor-invalidoutputcolumns"></a>Code d'erreur : DF-Executor-InvalidOutputColumns
- **Message** : Le résultat ne contient aucune colonne de sortie. Veuillez vous assurer qu'au moins une colonne est mappée.
- **Cause** : Aucune colonne n'est mappée.
- **Recommandation** : Vérifiez le schéma du récepteur pour vous assurer qu'au moins une colonne est mappée.

### <a name="error-code-df-executor-invalidinputcolumns"></a>Code d'erreur : DF-Executor-InvalidInputColumns
- **Message** : La colonne de la configuration source est introuvable dans le schéma des données sources.
- **Cause** : Des colonnes non valides sont fournies sur la source.
- **Recommandation** : Vérifiez les colonnes dans la configuration source et assurez-vous qu'il s'agit du sous-ensemble des schémas des données sources.

### <a name="error-code-df-adobeintegration-invalidmaptofilter"></a>Code d’erreur : DF-AdobeIntegration-InvalidMapToFilter
- **Message** : la ressource personnalisée ne peut avoir qu’une clé/ID mappée au filtre.
- **Cause** : Les configurations fournies ne sont pas valides.
- **Recommandation** : Dans vos paramètres AdobeIntegration, assurez-vous que la ressource personnalisée ne peut avoir qu'une seule clé ou un seul ID mappé au filtre.

### <a name="error-code-df-adobeintegration-invalidpartitionconfiguration"></a>Code d’erreur : DF-AdobeIntegration-InvalidPartitionConfiguration
- **Message** : seule une partition unique est prise en charge. Le schéma de partition peut être RoundRobin ou Hash.
- **Cause** : Les configurations de partition fournies ne sont pas valides.
- **Recommandation** : Dans les paramètres d'AdobeIntegration, vérifiez que seule la partition unique est définie, et que les schémas de partition peuvent être RoundRobin ou Hash.

### <a name="error-code-df-adobeintegration-keycolumnmissed"></a>Code d’erreur : DF-AdobeIntegration-KeyColumnMissed
- **Message** : la clé doit être spécifiée pour les opérations qui ne peuvent pas être insérées.
- **Cause** : Des colonnes clés sont manquantes.
- **Recommandation** : Mettez à jour les paramètres d'AdobeIntegration afin de vous assurer que les colonnes clés sont spécifiées pour les opérations non insérables.

### <a name="error-code-df-adobeintegration-invalidpartitiontype"></a>Code d’erreur : DF-AdobeIntegration-InvalidPartitionType
- **Message** : le type de la partition doit être roundRobin.
- **Cause** : Les types de partition fournis ne sont pas valides.
- **Recommandation** : Mettez à jour les paramètres d'AdobeIntegration pour définir votre type de partition sur RoundRobin.

### <a name="error-code-df-adobeintegration-invalidprivacyregulation"></a>Code d’erreur : DF-AdobeIntegration-InvalidPrivacyRegulation
- **Message** : Le seul règlement sur la confidentialité qui est actuellement pris en charge est le « RGPD ».
- **Cause** : Les configurations fournies en matière de confidentialité ne sont pas valides.
- **Recommandation** : Mettez à jour les paramètres d'AdobeIntegration. Seul le « RGPD » est pris en charge.

### <a name="error-code-df-executor-remoterpcclientdisassociated"></a>Code d’erreur : DF-Executor-RemoteRPCClientDisassociated
- **Message** : Client RPC dissocié. Cette erreur est probablement provoquée par des conteneurs dépassant des seuils ou des problèmes réseau.
- **Cause** : L’exécution de l’activité de flux de données a échoué en raison du problème réseau temporaire ou parce que la mémoire d’un nœud du cluster Spark est insuffisante.
- **Recommandation** : Utilisez les options suivantes pour résoudre ce problème :
  - Option 1 : utilisez un cluster puissant (la mémoire des nœuds lecteur et exécuteur est suffisante pour gérer les Big Data) afin d’exécuter des pipelines de flux de données avec le paramètre « Type de calcul » sur « Mémoire optimisée ». Les paramètres sont illustrés dans l’image ci-dessous.
        
      :::image type="content" source="media/data-flow-troubleshoot-guide/configure-compute-type.png" alt-text="Capture d’écran montrant la configuration de Type de calcul.":::   

  - Option 2 : utilisez une plus grande taille de cluster (par exemple, 48 cœurs) pour exécuter vos pipelines de flux de données. Vous pouvez en savoir plus sur la taille du cluster dans ce document : [Taille du cluster](./concepts-integration-runtime-performance.md#cluster-size).
  
  - Option 3 : répartissez vos données d’entrée. Pour la tâche en cours d’exécution sur le cluster Spark de flux de données, une partition est une tâche et s’exécute sur un nœud. Si les données d’une partition sont trop volumineuses, la tâche associée en cours d’exécution sur le nœud doit consommer plus de mémoire que le nœud lui-même, ce qui entraîne une défaillance. Par conséquent, vous pouvez utiliser la répartition pour éviter une asymétrie des données, vous assurer que la taille des données dans chaque partition est moyenne et que la consommation de mémoire n’est pas trop importante.
    
      :::image type="content" source="media/data-flow-troubleshoot-guide/configure-partition.png" alt-text="Capture d’écran montrant la configuration de partitions.":::

    > [!NOTE]
    >  Vous devez évaluer la taille des données ou le nombre de partitions de données d’entrée, puis définir un numéro de partition raisonnable sous « Optimiser ». Par exemple, le cluster que vous utilisez dans l’exécution du pipeline de flux de données comporte 8 cœurs et la mémoire de chaque cœur est de 20 Go, mais les données d’entrée sont de 1000 Go avec 10 partitions. Si vous exécutez le flux de données directement, il rencontrera le problème OOM, car 1000 Go/10 > 20 Go. Il est donc préférable de définir le nombre de répartitions sur 100 (1000 Go/100 < 20 Go).
    
  - Option 4 : Réglez et optimisez les paramètres de la source/du récepteur/de la transformation. Par exemple, essayez de copier tous les fichiers dans un conteneur et n’utilisez pas le modèle de caractère générique. Pour plus d’informations, reportez-vous au [Guide des performances et réglages des flux de données de mappage](./concepts-data-flow-performance.md).

### <a name="error-code-df-mssql-errorrowsfound"></a>Code d’erreur : DF-MSSQL-ErrorRowsFound
- **Cause** : Des lignes d’erreur/non valides ont été trouvées lors de l’écriture dans le récepteur Azure SQL Database.
- **Recommandation** : Recherchez les lignes d'erreur à l'emplacement de stockage des données rejetées (s’il est configuré).

### <a name="error-code-df-mssql-exporterrorrowfailed"></a>Code d'erreur : DF-SQLDW-ExportErrorRowFailed
- **Message** : Une exception s'est produite lors de l'écriture des lignes d'erreur dans le stockage.
- **Cause** : Une exception s'est produite lors de l'écriture des lignes d'erreur dans le stockage.
- **Recommandation** : Vérifiez la configuration du service lié des données rejetées.

### <a name="error-code-df-synapse-invaliddatabasetype"></a>Code d’erreur : DF-Synapse-InvalidDatabaseType
- **Message** : Le type de base de données n’est pas pris en charge.
- **Cause** : Le type de base de données n’est pas pris en charge.
- **Recommandation** : Vérifiez le type de base de données et remplacez-le par le type approprié.

### <a name="error-code-df-synapse-invalidformat"></a>Code d’erreur : DF-Synapse-InvalidFormat
- **Message** : Le format n'est pas pris en charge.
- **Cause** : Le format n'est pas pris en charge. 
- **Recommandation** : Vérifiez le format et remplacez-le par le format approprié.

### <a name="error-code-df-synapse-invalidtabledbname"></a>Code d’erreur : DF-Synapse-InvalidTableDBName
- **Cause** : Le nom de table/base de données n'est pas valide.
- **Recommandation** : Donnez un nom valide à la table/base de données. Les noms valides ne contiennent que des caractères alphabétiques, des nombres et `_`.

### <a name="error-code-df-synapse-invalidoperation"></a>Code d’erreur : DF-Synapse-InvalidOperation
- **Cause** : L'opération n'est pas prise en charge.
- **Recommandation** : Changez l’opération non valide.

### <a name="error-code-df-synapse-dbnotexist"></a>Code d’erreur : DF-Synapse-DBNotExist
- **Cause** : La base de données n’existe pas.
- **Recommandation** : Vérifiez si la base de données existe.

### <a name="error-code-df-synapse-storedprocedurenotsupported"></a>Code d’erreur : DF-Synapse-StoredProcedureNotSupported
- **Message** : L’utilisation de « Procédure stockée » en tant que source n’est pas prise en charge pour le pool serverless (à la demande).
- **Cause** : Le pool serverless présente des limitations.
- **Recommandation** : Réessayez en utilisant « requête » en tant que source ou en enregistrant la procédure stockée en tant que vue, puis utilisez « table » en tant que source pour lire directement à partir de la vue.

### <a name="error-code-df-executor-broadcastfailure"></a>Code d’erreur : DF-Executor-BroadcastFailure
- **Message** : L’exécution du flux de données a échoué pendant l’échange de diffusion. Les causes potentielles incluent des connexions à des sources mal configurées ou une erreur de délai d’expiration de la jointure de diffusion. Pour vous assurer que les sources sont correctement configurées, testez la connexion ou exécutez un aperçu des données sources dans une session de débogage de flux de données. Pour éviter le délai d’expiration de la jointure de diffusion, vous pouvez choisir l’option de diffusion « Désactivé » dans les transformations de jointure/d’existence/de recherche. Si vous envisagez d’utiliser l’option de diffusion pour améliorer les performances, assurez-vous que les flux de diffusion peuvent produire des données dans un délai de 60 secondes pour les exécutions de débogage et un délai de 300 secondes pour les exécutions de travaux. Si le problème persiste, contactez le support.

- **Cause** :  
    1. L’erreur de connexion/configuration source peut entraîner une défaillance de diffusion dans les transformations de jointure/d’existence/de recherche.
    2. Le délai d’expiration par défaut de la diffusion est de 60 secondes dans les exécutions de débogage et de 300 secondes dans les exécutions de travaux. Dans la jointure de diffusion, le flux choisi pour la diffusion semble être trop volumineux pour produire des données respectant cette limite. Si une jointure de diffusion n’est pas utilisée, la diffusion par défaut effectuée par un flux de données peut atteindre la même limite.

- **Recommandation** :
    1. Affichez l’aperçu des données au niveau des sources pour vérifier que les sources sont bien configurées. 
    1. Désactivez l’option de diffusion ou évitez de diffuser des flux de données volumineux dont le traitement peut prendre plus de 60 secondes. Choisissez plutôt un flux plus petit à diffuser. 
    1. Les fichiers sources et les tables SQL/Data Warehouse volumineux sont généralement de mauvais candidats. 
    1. En l’absence d’une diffusion de la jonction, utilisez un cluster plus grand si l’erreur se produit. 
    1. Si le problème persiste, contactez le support client.

### <a name="error-code-df-cosmos-shorttypenotsupport"></a>Code d’erreur : DF-Cosmos-ShortTypeNotSupport
- **Message** : Le type de données courtes n’est pas pris en charge dans Cosmos DB.
- **Cause** : Le type de données courtes n’est pas pris en charge dans Azure Cosmos DB.
- **Recommandation** : Ajoutez une transformation dérivée pour convertir les colonnes associées de « short » en « integer » avant de les utiliser dans le récepteur Cosmos.

### <a name="error-code-df-blob-functionnotsupport"></a>Code d’erreur : DF-BLOB-FunctionNotSupport
- **Message** : Ce point de terminaison ne prend pas en charge BlobStorageEvents, SoftDelete ou AutomaticSnapshot. Désactivez ces fonctionnalités de compte si vous souhaitez utiliser ce point de terminaison.
- **Cause** : Les événements de Stockage Blob Azure, la suppression réversible ou la capture instantanée automatique ne sont pas pris en charge dans les flux de données si le service lié Stockage Blob Azure est créé avec l’authentification du principal de service ou de l’identité managée.
- **Recommandation** : Désactivez les événements de Stockage Blob Azure, la suppression réversible ou la fonctionnalité de capture instantanée automatique sur le compte Azure Blob, ou utilisez l’authentification par clé pour créer le service lié.

### <a name="error-code-df-cosmos-invalidaccountkey"></a>Code d’erreur : DF-Cosmos-InvalidAccountKey
- **Message** : Le jeton d’autorisation d’entrée ne peut pas traiter la requête. Vérifiez que la charge utile attendue est générée conformément au protocole et vérifiez la clé utilisée.
- **Cause** : L’autorisation de lecture/d’écriture de données Azure Cosmos DB est insuffisante.
- **Recommandation** : Utilisez la clé de lecture/écriture pour accéder à Azure Cosmos DB.

## <a name="miscellaneous-troubleshooting-tips"></a>Conseils pour la résolution de problèmes divers
- **Problème** : Une exception inattendue s’est produite et l’exécution a échoué.
    - **Message** : Lors de l’exécution de l’activité de flux de données : Exception inattendue et échec de l’exécution.
    - **Cause** : Cette erreur est une erreur de service back-end. Retentez l’opération et redémarrez votre session de débogage.
    - **Recommandation** : Si la nouvelle tentative et le redémarrage ne résolvent pas le problème, contactez le support technique.

-  **Problème** : Aucune donnée de sortie lors de la jointure pendant l’aperçu des données de débogage.
    - **Message** : Il existe un nombre élevé de valeurs Null ou manquantes qui peuvent être dues à l’échantillonnage d’un trop petit nombre de lignes. Essayez de mettre à jour la limite de lignes de débogage et d’actualiser les données.
    - **Cause** : La condition de jointure ne correspond à aucune ligne ou a généré un nombre élevé de valeurs Null au cours de l’aperçu des données.
    - **Recommandation** : Dans **Paramètres de débogage**, augmentez le nombre de lignes dans la limite de lignes sources. Veillez à sélectionner un runtime d’intégration Azure disposant d’un cluster de flux de données suffisamment grand pour gérer davantage de données.
    
- **Problème** : Erreur de validation à la source avec des fichiers CSV multilignes. 
    - **Message** : Vous pourriez voir l’un des messages d’erreur suivants :
        - La dernière colonne est null ou manquante.
        - Échec de la validation de schéma à la source.
        - L’importation de schéma ne s’affiche pas correctement dans l’interface utilisateur et la dernière colonne contient un caractère de nouvelle ligne dans le nom.
    - **Cause** : Dans le flux de données de mappage, les fichiers sources CSV multilignes ne fonctionnent pas quand \r\n est utilisé comme séparateur de lignes. Parfois, des lignes supplémentaires au niveau des retours chariot peuvent provoquer des erreurs. 
    - **Recommandation** : Générez le fichier à la source avec \n comme séparateur de lignes plutôt que \r\n. Vous pouvez aussi utiliser l’activité de copie pour convertir le fichier CSV afin d’utiliser \n comme séparateur de lignes.

## <a name="general-troubleshooting-guidance"></a>Instructions générales pour la résolution des problèmes
1. Vérifiez l’état de vos connexions de jeu de données. Dans chaque transformation de la source et du récepteur, accédez au service lié pour chaque jeu de données que vous utilisez et testez les connexions.
2. Vérifiez l’état de vos connexions aux fichiers et aux tables dans le concepteur de flux de données. En mode de débogage, sélectionnez **Aperçu des données** dans vos transformations sources pour vérifier que vous pouvez accéder à vos données.
3. Si tout semble correct dans l’aperçu des données, accédez au concepteur de pipeline et mettez votre flux de données dans une activité de pipeline. Déboguez le pipeline pour un test de bout en bout.

### <a name="improvement-on-csvcdm-format-in-data-flow"></a>Amélioration du format CSV/CDM dans le Data Flow 

Si vous utilisez le **format Texte délimité ou CDM pour le flux de données de mappage dans Azure Data Factory v2**, vous pourrez être confronté à des changements de comportement de vos pipelines existants en raison de l’amélioration du format Texte délimité/CDM dans le flux de données à partir du **1er mai 2021**. 

Vous pouvez rencontrer les problèmes suivants avant l’amélioration, mais après l’amélioration, les problèmes seront corrigés. Lisez le contenu suivant pour déterminer si cette amélioration vous concerne. 

#### <a name="scenario-1-encounter-the-unexpected-row-delimiter-issue"></a>Scénario 1 : Vous rencontrez un problème de délimiteur de ligne inattendu

 Vous êtes concerné dans les conditions suivantes :
 - Vous utilisez le format Texte délimité avec le paramètre Multiline défini sur True ou CDM comme source.
 - La première ligne contient plus de 128 caractères. 
 - Le délimiteur de ligne dans les fichiers de données n’est pas `\n`.

 Avant l’amélioration, le délimiteur de ligne par défaut `\n` peut être utilisé de manière inattendue pour analyser les fichiers texte délimité, car, lorsque le paramètre Multiline est défini sur True, il invalide le paramètre de délimiteur de ligne et le délimiteur de ligne est automatiquement détecté en fonction des 128 premiers caractères. Si vous ne parvenez pas à détecter le délimiteur de ligne réel, le système revient à `\n`.  

 Après l’amélioration, l’un des trois délimiteurs de ligne, `\r`, `\n` ou `\r\n`, devrait fonctionner.
 
 L’exemple suivant montre un changement de comportement du pipeline après l’amélioration :

 **Exemple** :<br/>
   Pour la colonne suivante :<br/>
    `C1, C2, {long first row}, C128\r\n `<br/>
    `V1, V2, {values………………….}, V128\r\n `<br/>
 
   Avant l’amélioration, `\r` est conservé dans la valeur de la colonne. Le résultat de la colonne analysée est le suivant :<br/>
   `C1 C2 {long first row} C128`**`\r`**<br/>
   `V1 V2 {values………………….} V128`**`\r`**<br/> 

   Après l’amélioration, le résultat de la colonne analysée doit être :<br/>
   `C1 C2 {long first row} C128`<br/>
   `V1 V2 {values………………….} V128`<br/>
  
#### <a name="scenario-2-encounter-an-issue-of-incorrectly-reading-column-values-containing-rn"></a>Scénario 2 : Vous rencontrez un problème de lecture incorrecte des valeurs de colonne contenant « \r\n »

 Vous êtes concerné dans les conditions suivantes :
 - Vous utilisez le format Texte délimité avec le paramètre Multiline défini sur True ou CDM comme source. 
 - Le délimiteur de ligne est `\r\n`.

 Avant l’amélioration, lors de la lecture de la valeur de colonne, le `\r\n` de celle-ci peut être incorrectement remplacé par `\n`. 

 Après l’amélioration, `\r\n` dans la valeur de colonne ne sera pas remplacé par `\n`.

 L’exemple suivant montre un changement de comportement du pipeline après l’amélioration :
 
 **Exemple** :<br/>
  
 Pour la colonne suivante :<br/>
  **`"A\r\n"`**`, B, C\r\n`<br/>

 Avant l’amélioration, le résultat de la colonne analysée est le suivant :<br/>
  **`A\n`**` B C`<br/>

 Après l’amélioration, le résultat de la colonne analysée doit être :<br/>
  **`A\r\n`**` B C`<br/>  

#### <a name="scenario-3-encounter-an-issue-of-incorrectly-writing-column-values-containing-n"></a>Scénario 3 : Vous rencontrez un problème d’écriture incorrecte des valeurs de colonne contenant « \r\n »

 Vous êtes concerné dans les conditions suivantes :
 - Vous utilisez le format Texte délimité comme récepteur.
 - La valeur de colonne contient `\n`.
 - L’option Séparateur de lignes est définie sur `\r\n`.
 
 Avant l’amélioration, lors de l’écriture de la valeur de colonne, le `\n` de celle-ci peut être incorrectement remplacé par `\r\n`. 

 Après l’amélioration, `\n` dans la valeur de colonne ne sera pas remplacé par `\r\n`.
 
 L’exemple suivant montre un changement de comportement du pipeline après l’amélioration :

 **Exemple** :<br/>

 Pour la colonne suivante :<br/>
 **`A\n`**` B C`<br/>

 Avant l’amélioration, le récepteur CSV est :<br/>
  **`"A\r\n"`**`, B, C\r\n` <br/>

 Après l’amélioration, le récepteur CSV doit être :<br/>
  **`"A\n"`**`, B, C\r\n`<br/>

#### <a name="scenario-4-encounter-an-issue-of-incorrectly-reading-empty-string-as-null"></a>Scénario 4 : Vous rencontrez un problème de lecture incorrecte d’une chaîne vide comme NULL
 
 Vous êtes concerné dans les conditions suivantes :
 - Vous utilisez le format Texte délimité comme source. 
 - La valeur NULL est définie comme une valeur non vide. 
 - La valeur de colonne est une chaîne vide et est sans guillemets. 
 
 Avant l’amélioration, la valeur de colonne de la chaîne vide sans guillemets est lue comme NULL. 

 Après l’amélioration, la chaîne vide ne sera pas interprétée comme une valeur NULL. 
 
 L’exemple suivant montre un changement de comportement du pipeline après l’amélioration :

 **Exemple** :<br/>

 Pour la colonne suivante :<br/>
  `A, ,B, `<br/>

 Avant l’amélioration, le résultat de la colonne analysée est le suivant :<br/>
  `A null B null`<br/>

 Après l’amélioration, le résultat de la colonne analysée doit être :<br/>
  `A "" (empty string) B "" (empty string)`<br/>

###  <a name="internal-server-errors"></a>Erreurs de serveur internes

Des scénarios spécifiques qui peuvent provoquer des erreurs internes de serveurs sont présentés ci-dessous.

#### <a name="scenario-1-not-choosing-the-appropriate-compute-sizetype-and-other-factors"></a>Scénario 1 : Ne pas choisir la taille/le type de calcul approprié et d’autres facteurs

  La réussite de l’exécution des flux de données dépend de nombreux facteurs, parmi lesquels la taille/le type de calcul, le nombre de sources/récepteurs à traiter, la spécification de partition, les transformations impliquées, la taille des jeux de données, l’asymétrie des données, etc.<br/>
  
  Pour plus d’aide, consultez [Performance du Runtime d’intégration](concepts-integration-runtime-performance.md).

#### <a name="scenario-2-using-debug-sessions-with-parallel-activities"></a>Scénario 2 : Utilisation de sessions de débogage avec des activités parallèles

  Lors du déclenchement d’une exécution en utilisant une session de débogage de flux de données avec des constructions telles que ForEach dans le pipeline, plusieurs exécutions parallèles peuvent être soumises au même cluster. Cette situation peut entraîner des problèmes de défaillance du cluster au moment de l’exécution en raison de problèmes de ressources, tels qu’une mémoire insuffisante.<br/>
  
  Pour soumettre une exécution avec la configuration de runtime d’intégration appropriée définie dans l’activité de pipeline et après la publication des modifications, sélectionnez **Déclencher maintenant** ou **Déboguer** > **Utiliser le runtime de l'activité**.

#### <a name="scenario-3-transient-issues"></a>Scénario 3 : Problèmes temporaires

  Des problèmes temporaires avec des microservices impliqués dans l’exécution peuvent entraîner l’échec de l’exécution.<br/>
  
  La configuration de nouvelles tentatives dans l’activité de pipeline peut résoudre les problèmes dus à des problèmes temporaires. Pour plus d’informations, consultez la section [Stratégie d’activité](concepts-pipelines-activities.md#activity-json) de notre documentation.

## <a name="next-steps"></a>Étapes suivantes

Pour obtenir une aide supplémentaire sur la résolution des problèmes, consultez les ressources suivantes :

*  [Blog Data Factory](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Demandes de fonctionnalités Data Factory](/answers/topics/azure-data-factory.html)
*  [Vidéos Azure](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Forum Stack Overflow pour Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Informations Twitter sur Data Factory](https://twitter.com/hashtag/DataFactory)
