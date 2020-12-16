---
title: Points de terminaison et règles de réseau virtuel pour les bases de données dans Azure SQL Database
description: Marquez un sous-réseau en tant que point de terminaison de service de réseau virtuel, puis le point de terminaison en tant que règle de réseau virtuel dans la liste ACL de votre base de données. La base de données accepte alors les communications provenant de toutes les machines virtuelles et d’autres nœuds sur le sous-réseau.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto, genemi
ms.date: 11/14/2019
ms.openlocfilehash: c5839589c35ea5a9c52303801a8767fc598434fc
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96905874"
---
# <a name="use-virtual-network-service-endpoints-and-rules-for-servers-in-azure-sql-database"></a>Utiliser des points de terminaison de service de réseau virtuel et des règles pour serveurs dans Azure SQL Database
[!INCLUDE[appliesto-sqldb-asa](../includes/appliesto-sqldb-asa.md)]

Les *règles de réseau virtuel* constituent une fonctionnalité de sécurité du pare-feu. Elles permettent de déterminer si le serveur de vos bases de données et pools élastiques Azure [SQL Database](sql-database-paas-overview.md), ou de vos bases de données [Azure Synapse](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md), doit accepter les communications provenant de sous-réseaux spécifiques de réseaux virtuels. Cet article explique pourquoi la fonctionnalité de règle de réseau virtuel est parfois la meilleure solution qui s’offre à vous pour autoriser en toute sécurité les communications à destination de votre base de données dans Azure SQL Database et Azure Synapse Analytics.

> [!NOTE]
> Cet article s’applique à la fois à Azure SQL Database et à Azure Synapse Analytics. Pour simplifier, le terme « base de données » fait référence aux bases de données dans Azure SQL Database et Azure Synapse Analytics. De même, toutes les références à « serveur » désignent le [serveur SQL logique](logical-servers.md) qui héberge Azure SQL Database et Azure Synapse Analytics.

Pour créer une règle de réseau virtuel, il doit d’abord exister un [point de terminaison de service de réseau virtuel][vm-virtual-network-service-endpoints-overview-649d] pour la règle à référencer.

## <a name="how-to-create-a-virtual-network-rule"></a>Création d’une règle de réseau virtuel

Si vous créez uniquement une règle de réseau virtuel, vous pouvez passer directement à la procédure et à l’explication donnée [plus loin dans cet article](#anchor-how-to-by-using-firewall-portal-59j).

## <a name="details-about-virtual-network-rules"></a>Informations sur les règles de réseau virtuel

Cette section fournit des informations sur les règles de réseau virtuel.

### <a name="only-one-geographic-region"></a>Une seule région géographique

Chaque point de terminaison de service de réseau virtuel s’applique à une seule région Azure. Le point de terminaison ne permet pas à d’autres régions d’accepter les communications provenant du sous-réseau.

Une règle de réseau virtuel est limitée à la région à laquelle s’applique son point de terminaison sous-jacent.

### <a name="server-level-not-database-level"></a>Niveau serveur, et non niveau base de données

Chaque règle de réseau virtuel s’applique à tout le serveur, et pas seulement à une base de données particulière sur le serveur. En d’autres termes, la règle de réseau virtuel s’applique au niveau du serveur, et non au niveau de la base de données.

- En revanche, les règles IP peuvent être appliquées à tout niveau.

### <a name="security-administration-roles"></a>Rôles d’administration de la sécurité

Il existe une séparation des rôles de sécurité dans l’administration des points de terminaison de service de réseau virtuel. Chacun des rôles suivants doit réaliser une action :

- **Administrateur réseau :** &nbsp; Activez le point de terminaison.
- **Administrateur de base de données :** &nbsp; mettre à jour la liste de contrôle d’accès (ACL) pour ajouter le sous-réseau donné au serveur.

*Alternative Azure RBAC :*

Les rôles d’administrateur de réseau et d’administrateur de base de données disposent de plus de fonctionnalités que nécessaires pour gérer les règles de réseau virtuel. Seule une partie de ces fonctionnalités est réellement nécessaire.

Vous avez la possibilité d’utiliser le [contrôle d’accès en fonction du rôle Azure (Azure RBAC)][rbac-what-is-813s] dans Azure pour créer un rôle personnalisé unique disposant uniquement des fonctionnalités nécessaires. Le rôle personnalisé peut être utilisé au lieu d’impliquer l’administrateur de réseau ou l’administrateur de base de données. Votre surface d’exposition de sécurité est inférieure si vous assignez un rôle personnalisé à un utilisateur au lieu de lui assigner les deux principaux rôles d’administrateur.

> [!NOTE]
> Il peut arriver que la base de données Azure SQL et le sous-réseau de réseau virtuel se trouvent dans des abonnements différents. Dans ce cas, vous devez vérifier les configurations suivantes :
>
> - Les deux abonnements doivent se trouver dans le même locataire Azure Active Directory.
> - L’utilisateur dispose des autorisations requises pour lancer des opérations, telles que l’activation des points de terminaison de service et l’ajout d’un sous-réseau de réseau virtuel sur le serveur donné.
> - Le fournisseur Microsoft.Sql doit être inscrit pour les deux abonnements.

## <a name="limitations"></a>Limites

Pour Azure SQL Database, la fonctionnalité de règle de réseau virtuel présente les limitations suivantes :

- Dans le pare-feu pour votre base de données Azure SQL Database, chaque règle de réseau virtuel fait référence à un sous-réseau. Tous ces sous-réseaux référencés doivent être hébergés dans la même région géographique que celle qui héberge la base de données.

- Chaque serveur peut avoir jusqu’à 128 entrées ACL pour un réseau virtuel donné.

- Les règles de réseau virtuel s’appliquent uniquement à des réseaux virtuels Azure Resource Manager, et non à des réseaux avec un [modèle de déploiement classique][arm-deployment-model-568f].

- L’activation des points de terminaison de service de réseau virtuel pour Azure SQL Database active également les points de terminaison des services Azure MySQL et PostgreSQL. Toutefois, avec les points de terminaison activés, les tentatives de connexion à partir des points de terminaison pour vos instances de MySQL ou PostgreSQL peuvent échouer.
  - Il est fort probable qu'une règle de réseau virtuel n'ait pas été configurée pour MySQL et PostgreSQL. Vous devez configurer une règle de réseau virtuel pour Azure Database pour MySQL et PostgreSQL afin de permettre la connexion.
  - Pour définir des règles de pare-feu de réseau virtuel sur un serveur logique SQL qui est déjà configuré avec des points de terminaison privés, définissez **Deny public network access** (Refuser l’accès au réseau public) sur **No** (Non).

- Sur le pare-feu, les plages d’adresses IP s’appliquent aux éléments de mise en réseau suivants, contrairement aux règles de réseau virtuel :
  - [Réseau privé virtuel (VPN) site à site (S2S)][vpn-gateway-indexmd-608y]
  - Localement via [ExpressRoute](../../expressroute/index.yml)

### <a name="considerations-when-using-service-endpoints"></a>Considérations en cas d’utilisation des points de terminaison de service

Lorsque vous utilisez des points de terminaison de service pour Azure SQL Database, passez en revue les considérations suivantes :

- **Une sortie à destination d’adresses IP publiques Azure SQL Database est nécessaire** : Des groupes de sécurité réseau (NSG) doivent être ouverts aux adresses IP Azure SQL Database pour autoriser la connectivité. Vous pouvez pour ce faire utiliser des [balises de service](../../virtual-network/network-security-groups-overview.md#service-tags) NSG pour Azure SQL Database.

### <a name="expressroute"></a>ExpressRoute

Si vous utilisez [ExpressRoute](../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) localement, pour le Peering public ou Microsoft, vous devez identifier les adresses IP NAT (traduction d’adresses réseau) utilisées. Pour le peering public, chaque circuit ExpressRoute utilise par défaut deux adresses IP NAT qui sont appliquées au trafic de service Azure lorsque le trafic entre dans le réseau principal de Microsoft Azure. Pour le peering Microsoft, les adresses IP NAT qui sont utilisées sont fournies par le client ou par le fournisseur de services. Pour autoriser l’accès à vos ressources de votre service, vous devez autoriser ces adresses IP publiques dans le paramètre de pare-feu IP de ressource. Pour trouver les adresses IP de votre circuit ExpressRoute de peering public, [ouvrez un ticket de support avec ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) via le portail Azure. Découvrez d’autres informations sur le [peering public et Microsoft NAT pour ExpressRoute.](../../expressroute/expressroute-nat.md?toc=%2fazure%2fvirtual-network%2ftoc.json#nat-requirements-for-azure-public-peering)

Pour permettre la communication entre votre circuit et Azure SQL Database, vous devez créer des règles de réseau IP pour les adresses IP publiques de votre processus NAT.

<!--
FYI: Re ARM, 'Azure Service Management (ASM)' was the old name of 'classic deployment model'.
When searching for blogs about ASM, you probably need to use this old and now-forbidden name.
-->

## <a name="impact-of-using-vnet-service-endpoints-with-azure-storage"></a>Impact de l’utilisation des points de terminaison de service de réseau virtuel avec le stockage Azure

Stockage Azure a implémenté la même fonctionnalité qui vous permet de limiter la connectivité à votre compte Stockage Azure. Si vous choisissez d’utiliser cette fonctionnalité avec un compte Stockage Azure qui est utilisé par Azure SQL Database, vous risquez de rencontrer des problèmes. Vous trouverez ci-dessous une liste et une présentation des fonctionnalités Azure SQL Database et Azure Synapse Analytics qui sont concernées par ce problème.

### <a name="azure-synapse-polybase-and-copy-statement"></a>Azure Synapse Analytics et instruction COPY

La technologie PolyBase et l’instruction COPY sont couramment utilisées pour charger des données dans Azure Synapse Analytics à partir de comptes Stockage Azure pour l’ingestion des données à haut débit. Si le compte Stockage Azure à partir duquel vous chargez des données limite l’accès à un ensemble de sous-réseaux de réseau virtuel, lors de l’utilisation de la technologie PolyBase et de l’instruction COPY, la connexion au compte de stockage est interrompue. Pour activer les scénarios d’importation et d’exportation à l’aide de l’instruction COPY et de la technologie PolyBase dans lesquels Azure Synapse Analytics se connecte au service Stockage Azure sécurisé sur le réseau virtuel, procédez de la manière décrite ci-dessous :

#### <a name="prerequisites"></a>Prérequis

- Installez Azure PowerShell en vous aidant de ce [guide](/powershell/azure/install-az-ps).
- Si vous disposez d’un compte de stockage d’objets blob ou v1 universel, vous devez commencer par le mettre à niveau avec un compte v2 universel en vous aidant de ce [guide](../../storage/common/storage-account-upgrade.md).
- Vous devez avoir activé **Autoriser les services Microsoft approuvés à accéder à ce compte de stockage** sous le menu de paramètres **Pare-feux et réseaux virtuels** du compte Stockage Azure. L’activation de cette configuration permettra à la technologie Polybase et à l’instruction COPY de se connecter au compte de stockage en utilisant une authentification forte où le trafic réseau reste sur le réseau principal Azure. Pour plus d’informations, consultez ce [guide](../../storage/common/storage-network-security.md#exceptions).

> [!IMPORTANT]
> Le module PowerShell Azure Resource Manager est toujours pris en charge par Azure SQL Database, mais tous les développements futurs sont destinés au module Az.Sql. Le module AzureRM continue à recevoir des résolutions de bogues jusqu’à au moins décembre 2020.  Les arguments des commandes dans le module Az sont sensiblement identiques à ceux des modules AzureRm. Pour en savoir plus sur leur compatibilité, consultez [Présentation du nouveau module Az Azure PowerShell](/powershell/azure/new-azureps-module-az).

#### <a name="steps"></a>Étapes

1. Si vous disposez d’un pool SQL dédié autonome, inscrivez votre serveur SQL Server auprès d’Azure Active Directory (AAD) à l’aide de PowerShell : 

   ```powershell
   Connect-AzAccount
   Select-AzSubscription -SubscriptionId <subscriptionId>
   Set-AzSqlServer -ResourceGroupName your-database-server-resourceGroup -ServerName your-SQL-servername -AssignIdentity
   ```

   Cette étape n’est pas nécessaire pour les pools SQL dédiés qui se trouvent dans un espace de travail Synapse.

1. Si vous disposez d’un espace de travail Synapse, inscrivez l’identité gérée par le système de votre espace de travail :

   1. Accédez à votre espace de travail Synapse sur le portail Azure
   2. Accédez au panneau Identités managées 
   3. Vérifiez que l’option « Autoriser les pipelines » est activée
   
1. Créez un **compte de stockage v2 universel** en vous aidant de ce [guide](../../storage/common/storage-account-create.md).

   > [!NOTE]
   >
   > - Si vous disposez d’un compte de stockage d’objets blob ou v1 universel, vous devez **d’abord le mettre à niveau avec v2** en vous aidant de ce [guide](../../storage/common/storage-account-upgrade.md).
   > - Pour examiner les problèmes connus liés à Azure Data Lake Storage Gen2, consultez ce [guide](../../storage/blobs/data-lake-storage-known-issues.md).

1. Sous votre compte de stockage, accédez à **Contrôle d’accès (IAM)** , puis sélectionnez **Ajouter une attribution de rôle**. Attribuez le rôle Azure de **Contributeur aux données blob du stockage** au serveur ou à l’espace de travail hébergeant votre pool SQL dédié que vous avez inscrit auprès d’Azure Active Directory (AAD).

   > [!NOTE]
   > Seuls les membres dotés du privilège Propriétaire sur le compte de stockage peuvent effectuer cette étape. Pour découvrir les divers rôles intégrés Azure, consultez ce [guide](../../role-based-access-control/built-in-roles.md).
  
1. **Connectivité PolyBase au compte Stockage Azure :**

   1. Créez une **[clé principale](/sql/t-sql/statements/create-master-key-transact-sql)** de base de données si vous n’en avez pas déjà créée une :

       ```sql
       CREATE MASTER KEY [ENCRYPTION BY PASSWORD = 'somepassword'];
       ```

   1. Créez des informations d’identification incluses dans l’étendue de la base de données avec **IDENTITY = 'Managed Service Identity'**  :

       ```sql
       CREATE DATABASE SCOPED CREDENTIAL msi_cred WITH IDENTITY = 'Managed Service Identity';
       ```

       > [!NOTE]
       >
       > - Il est inutile de spécifier SECRET avec la clé d’accès Stockage Azure, car ce mécanisme utilise l’[identité managée](../../active-directory/managed-identities-azure-resources/overview.md) en arrière-plan.
       > - Le nom d’IDENTITY doit être **'Managed Service Identity'** pour que la connectivité PolyBase entre le compte Stockage Azure et le réseau virtuel fonctionne de manière sécurisée.

   1. Créez la source de données externe avec le schéma `abfss://` pour vous connecter à votre compte de stockage v2 universel en utilisant PolyBase :

       ```SQL
       CREATE EXTERNAL DATA SOURCE ext_datasource_with_abfss WITH (TYPE = hadoop, LOCATION = 'abfss://myfile@mystorageaccount.dfs.core.windows.net', CREDENTIAL = msi_cred);
       ```

       > [!NOTE]
       >
       > - Si des tables externes sont déjà associées au compte de stockage d’objets blob ou v1 universel, vous devez dans un premier temps supprimer ces tables externes et dans un deuxième temps supprimer la source de données externe correspondante. Créez ensuite la source de données externe en faisant en sorte que le schéma `abfss://` se connecte au compte de stockage v2 universel comme indiqué ci-dessus, puis recréez toutes les tables externes en utilisant cette nouvelle source de données externe. Vous pouvez utiliser l’[Assistant Générer et publier des scripts](/sql/ssms/scripting/generate-and-publish-scripts-wizard) pour générer des scripts de création pour toutes les tables externes.
       > - Pour plus d’informations sur le schéma `abfss://`, consultez ce [guide](../../storage/blobs/data-lake-storage-introduction-abfs-uri.md).
       > - Pour plus d’informations sur CREATE EXTERNAL DATA SOURCE, consultez ce [guide](/sql/t-sql/statements/create-external-data-source-transact-sql).

   1. Exécutez des requêtes comme vous le faites habituellement en utilisant des [tables externes](/sql/t-sql/statements/create-external-table-transact-sql).

### <a name="azure-sql-database-blob-auditing"></a>Audit d’objets blob Azure SQL Database

L’audit d’objets blob transfère des journaux d’audit à votre propre compte de stockage. Si ce compte de stockage utilise la fonctionnalité de points de terminaison de service de réseau virtuel, la connectivité entre Azure SQL Database et le compte de stockage est arrêtée.

## <a name="adding-a-vnet-firewall-rule-to-your-server-without-turning-on-vnet-service-endpoints"></a>Ajout d’une règle de pare-feu de réseau virtuel à votre serveur sans activer les points de terminaison de service de réseau virtuel

Il y a longtemps, avant que cette fonctionnalité ne soit améliorée, vous deviez activer les points de terminaison de service VNet avant de pouvoir implémenter une règle de réseau virtuel dynamique dans le pare-feu. Les points de terminaison associaient un sous-réseau/réseau virtuel donné à une base de données dans Azure SQL Database. Depuis janvier 2018, vous pouvez éviter cette opération en définissant l’indicateur **IgnoreMissingVNetServiceEndpoint**.

La simple définition d’une règle de pare-feu ne permet pas de sécuriser le serveur. Vous devez également activer les points de terminaison de service Vnet pour que la sécurité entre en vigueur. Quand vous activez les points de terminaison de service, votre sous-réseau/réseau virtuel est arrêté pendant qu’il passe de l’état désactivé à l’état activé. Cela est particulièrement vrai dans le contexte de grands réseaux virtuels. Vous pouvez utiliser l’indicateur **IgnoreMissingVNetServiceEndpoint** pour réduire ou éliminer le temps d’arrêt pendant la transition.

Vous pouvez définir l’indicateur **IgnoreMissingVNetServiceEndpoint** en utilisant PowerShell. Pour plus d’informations, consultez [Utiliser PowerShell pour créer un point de terminaison de service de réseau virtuel et une règle pour Azure SQL Database][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

## <a name="errors-40914-and-40615"></a>Erreurs 40914 et 40615

L’erreur de connexion 40914 est liée aux *règles de réseau virtuel*, comme spécifié dans le volet Pare-feu du portail Azure. L’erreur 40615 est similaire, à ceci près qu’elle est liée aux *règles des adresses IP* sur le pare-feu.

### <a name="error-40914"></a>Erreur 40914

*Texte du message :* Impossible d’ouvrir le serveur « *[nom-serveur]* » demandé par la connexion. Le client n’est pas autorisé à accéder au serveur.

*Description de l’erreur :* le client se trouve dans un sous-réseau qui dispose de points de terminaison de serveur de réseau virtuel. Mais le serveur n’a pas de règle de réseau virtuel qui accorde au sous-réseau le droit de communiquer avec la base de données.

*Résolution de l’erreur :* dans le volet Pare-feu du portail Azure, utilisez le contrôle des règles de réseau virtuel pour [ajouter une règle de réseau virtuel](#anchor-how-to-by-using-firewall-portal-59j) pour le sous-réseau.

### <a name="error-40615"></a>Erreur 40615

*Texte du message :* Impossible d'ouvrir le serveur '{0}' demandé par la connexion. Le client avec l'adresse IP '{1}' n'est pas autorisé à accéder au serveur.

*Description de l’erreur :* Le client tente de se connecter à partir d’une adresse IP qui n’est pas autorisée à se connecter au serveur. Le pare-feu du serveur ne dispose d’aucune règle d’adresse IP qui autorise un client à communiquer à partir de l’adresse IP donnée vers la base de données.

*Résolution de l’erreur :* entrez l’adresse IP du client en tant que règle IP. Effectuez cette opération via le volet Pare-feu du portail Azure.

<a name="anchor-how-to-by-using-firewall-portal-59j"></a>

## <a name="portal-can-create-a-virtual-network-rule"></a>Le portail peut créer une règle de réseau virtuel

Cette section montre comment utiliser le [portail Azure][http-azure-portal-link-ref-477t] pour créer une *règle de réseau virtuel* dans votre base de données Azure SQL Database. La règle donne l’instruction à votre base de données d’accepter les communications provenant d’un sous-réseau spécifique qui a été marqué comme étant un *point de terminaison de service de réseau virtuel*.

> [!NOTE]
> Si vous envisagez d’ajouter un point de terminaison de service aux règles de pare-feu de réseau virtuel de votre serveur, commencez par vérifier que les points de terminaison de service sont activés pour le sous-réseau.
>
> Si les points de terminaison de service ne sont pas activés pour le sous-réseau, le portail vous invite à les activer. Cliquez sur le bouton **Activer** sur le même panneau que celui où vous ajoutez la règle.

## <a name="powershell-alternative"></a>Alternative PowerShell

Un script peut également créer des règles de réseau virtuel à l’aide de la cmdlet PowerShell **New-AzSqlServerVirtualNetworkRule** ou [az network vnet create](/cli/azure/network/vnet#az-network-vnet-create). Si cette option vous intéresse, consultez [Utiliser PowerShell pour créer un point de terminaison de service de réseau virtuel et une règle pour Azure SQL Database][sql-db-vnet-service-endpoint-rule-powershell-md-52d].

## <a name="rest-api-alternative"></a>API REST de remplacement

En interne, les applets de commande PowerShell pour les actions de réseau virtuel SQL appellent des API REST. Vous pouvez appeler les API REST directement.

- [Règles de réseau virtuel : Opérations][rest-api-virtual-network-rules-operations-862r]

## <a name="prerequisites"></a>Prérequis

Vous devez déjà disposer d’un sous-réseau étiqueté avec le *nom de type* de point de terminaison de service de réseau virtuel particulier approprié pour Azure SQL Database.

- Le nom de type de point de terminaison approprié est **Microsoft.Sql**.
- Si votre sous-réseau n’est pas étiqueté avec le nom de type, consultez [Vérifier que votre sous-réseau est un point de terminaison][sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100].

<a name="a-portal-steps-for-vnet-rule-200"></a>

## <a name="azure-portal-steps"></a>Étapes dans le portail Azure

1. Connectez-vous au [portail Azure][http-azure-portal-link-ref-477t].

2. Recherchez et sélectionnez **Serveurs SQL**, puis sélectionnez votre serveur. Sous **Sécurité**, sélectionnez **Pare-feux et réseaux virtuels**.

3. Définissez le contrôle **Autoriser l’accès aux services Azure** sur DÉSACTIVÉ.

    > [!IMPORTANT]
    > Si vous laissez le contrôle défini sur ON (Activé), votre serveur accepte les communications en provenance de n’importe quel sous-réseau à l’intérieur de la limite Azure, c’est-à-dire en provenance de l’une des adresses IP reconnues comme celles comprises dans les plages définies pour les centres de données Azure. En termes de sécurité, le fait de laisser le contrôle ACTIVÉ peut avoir pour effet de multiplier excessivement les accès. La fonctionnalité de points de terminaison de service de réseau virtuel Microsoft Azure, associée à la fonctionnalité de règle de réseau virtuel de SQL Database, peuvent ensemble réduire votre surface d’exposition de sécurité.

4. Cliquez sur le contrôle **+ Ajouter existant** dans la section **Réseaux virtuels**.

    ![Cliquez sur Ajouter existant (point de terminaison de sous-réseau, en tant que règle SQL).][image-portal-firewall-vnet-add-existing-10-png]

5. Dans le nouveau volet **Créer/mettre à jour**, renseignez les contrôles avec les noms de vos ressources Azure.

    > [!TIP]
    > Vous devez inclure le **préfixe d’adresse** correct pour votre sous-réseau. Vous pouvez trouver la valeur correspondante dans le portail.
    > Accédez à **Toutes les ressources** &gt; **Tous les types** &gt; **Réseaux virtuels**. Le filtre affiche vos réseaux virtuels. Cliquez sur votre réseau virtuel, puis sur **Sous-réseaux**. La colonne **PLAGE D’ADRESSES** contient le préfixe d’adresse nécessaire.

    ![Renseignez les champs de la nouvelle règle.][image-portal-firewall-create-update-vnet-rule-20-png]

6. Cliquez sur le bouton **OK** en bas du volet.

7. Le volet du pare-feu affiche la règle de réseau virtuel obtenue.

    ![Le volet du pare-feu affiche la nouvelle règle.][image-portal-firewall-vnet-result-rule-30-png]

> [!NOTE]
> Les états suivants s’appliquent aux règles :
>
> - **Prêt :** indique que l’opération que vous avez lancée a abouti.
> - **Échec :** indique que l’opération que vous avez lancée a échoué.
> - **Supprimé :** s’applique uniquement à l’opération de suppression et indique que la règle a été supprimée et qu’elle ne s’applique plus.
> - **En cours :** indique que l’opération est en cours d’exécution. L’ancienne règle s’applique lorsque l’opération est à cet état.

<a name="anchor-how-to-links-60h"></a>

## <a name="related-articles"></a>Articles connexes

- [Points de terminaison de service de réseau virtuel Azure][vm-virtual-network-service-endpoints-overview-649d]
- [Règles de pare-feu au niveau du serveur et de la base de données][sql-db-firewall-rules-config-715d]

## <a name="next-steps"></a>Étapes suivantes

- [Utiliser PowerShell pour créer un point de terminaison de service de réseau virtuel, puis une règle de réseau virtuel pour Azure SQL Database.][sql-db-vnet-service-endpoint-rule-powershell-md-52d]
- [Règles de réseau virtuel : opérations][rest-api-virtual-network-rules-operations-862r] avec les API REST

<!-- Link references, to images. -->
[image-portal-firewall-vnet-add-existing-10-png]: ../../sql-database/media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-add-existing-10.png
[image-portal-firewall-create-update-vnet-rule-20-png]: ../../sql-database/media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-create-update-vnet-rule-20.png
[image-portal-firewall-vnet-result-rule-30-png]: ../../sql-database/media/sql-database-vnet-service-endpoint-rule-overview/portal-firewall-vnet-result-rule-30.png

<!-- Link references, to text, Within this same GitHub repo. -->
[arm-deployment-model-568f]: ../../azure-resource-manager/management/deployment-models.md
[expressroute-indexmd-744v]:../index.yml
[rbac-what-is-813s]:../../role-based-access-control/overview.md
[sql-db-firewall-rules-config-715d]:firewall-configure.md
[sql-db-vnet-service-endpoint-rule-powershell-md-52d]:scripts/vnet-service-endpoint-rule-powershell-create.md
[sql-db-vnet-service-endpoint-rule-powershell-md-a-verify-subnet-is-endpoint-ps-100]:scripts/vnet-service-endpoint-rule-powershell-create.md#a-verify-subnet-is-endpoint-ps-100
[vm-configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal-321w]: ../virtual-network/virtual-networks-static-private-ip-arm-pportal.md
[vm-virtual-network-service-endpoints-overview-649d]: ../../virtual-network/virtual-network-service-endpoints-overview.md
[vpn-gateway-indexmd-608y]: ../../vpn-gateway/index.yml

<!-- Link references, to text, Outside this GitHub repo (HTTP). -->
[http-azure-portal-link-ref-477t]: https://portal.azure.com/
[rest-api-virtual-network-rules-operations-862r]: /rest/api/sql/virtualnetworkrules

<!-- ??2
#### Syntax related articles
- REST API Reference, including JSON
- Azure CLI
- ARM templates
-->
