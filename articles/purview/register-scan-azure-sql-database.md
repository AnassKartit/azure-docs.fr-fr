---
title: Inscrire et analyser Azure SQL Database
description: Ce tutoriel explique comment analyser Azure SQL Database dans Azure Purview.
author: hophanms
ms.author: hophan
ms.service: purview
ms.subservice: purview-data-map
ms.topic: tutorial
ms.date: 09/27/2021
ms.openlocfilehash: a84de6dcdf3abebad1267382fa990fcc1cb0b3a4
ms.sourcegitcommit: e8c34354266d00e85364cf07e1e39600f7eb71cd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/29/2021
ms.locfileid: "129209735"
---
# <a name="register-and-scan-an-azure-sql-database"></a>Inscrire et analyser une base de données Azure SQL

Cet article explique comment inscrire une source de données Azure SQL Database dans Purview et configurer une analyse sur celle-ci.

## <a name="supported-capabilities"></a>Fonctionnalités prises en charge

La source de données Azure SQL Database prend en charge les fonctionnalités suivantes :

- **Analyses complètes et incrémentielles** pour capturer les métadonnées et la classification dans Azure SQL Database

- **Traçabilité** entre les ressources de données pour les activités de copie et de flux de données ADF

### <a name="known-limitations"></a>Limitations connues

> * Azure Purview prend en charge 300 colonnes au maximum sous l’onglet Schéma. Au-delà, il affiche « Additional-Columns-Truncated » (Colonnes-Supplémentaires-Tronquées). 

## <a name="prerequisites"></a>Prérequis

1. Créez un compte Purview si vous n’en avez pas déjà un.

1. Accès réseau entre le compte Purview et Azure SQL Database.

### <a name="set-up-authentication-for-a-scan"></a>Configurer l’authentification pour une analyse

Authentification pour analyser Azure SQL Database. Si vous avez besoin de créer une nouvelle authentification, vous devez [autoriser l’accès aux bases de données à SQL Database](../azure-sql/database/logins-create-manage.md). Il existe actuellement trois méthodes d’authentification prises en charge par Purview :

- Authentification SQL
- Principal de service
- Identité managée

#### <a name="sql-authentication"></a>Authentification SQL

> [!Note]
> Seule la connexion principale au niveau du serveur (créée par le processus de configuration) ou les membres du rôle de base de données `loginmanager` dans la base de données master peuvent créer des connexions. Cela nécessite environ **15 minutes** une fois l’autorisation accordée. Le compte Purview doit disposer des autorisations appropriées pour pouvoir analyser la ou les ressources.

Vous pouvez suivre les instructions fournies dans [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-current&preserve-view=true#examples-1) pour créer une connexion pour Azure SQL Database si vous n’en avez pas. Vous aurez besoin d’un **nom d’utilisateur** et d’un **mot de passe** pour les étapes suivantes.

1. Accédez à votre coffre de clés dans le portail Azure.
1. Sélectionnez **Paramètres > Secrets**.
1. Sélectionnez **+ Générer/importer** et entrez le **nom** et la **valeur** comme *mot de passe* de votre base de données Azure SQL Database.
1. Sélectionnez **Créer** pour terminer.
1. Si votre coffre de clés n’est pas encore connecté à Purview, vous devrez [créer une connexion de coffre de clés](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account).
1. Enfin, [créez de nouvelles informations d’identification](manage-credentials.md#create-a-new-credential) à l’aide du **nom d’utilisateur** et du **mot de passe** pour configurer votre analyse.

#### <a name="service-principal-and-managed-identity"></a>Principal de service et identité managée

Il existe plusieurs étapes pour autoriser Purview à utiliser le principal de service ou l’**identité managée** de Purview pour analyser votre base de données Azure SQL Database.

   > [!Note]
   > Purview a besoin de l’**ID d’application (client)** et du **secret client** pour effectuer l’analyse.

##### <a name="create-or-use-an-existing-service-principal"></a>Créer ou utiliser un principal de service existant

> [!Note]
> Ignorez cette étape si vous utilisez l’**identité managée** de Purview.

Vous pouvez utiliser un principal de service existant ou en créer un nouveau. 

> [!Note]
> Si vous devez créer un principal de service, procédez comme suit :
> 1. Accédez au [portail Azure](https://portal.azure.com).
> 1. Dans le menu de gauche, sélectionnez **Azure Active Directory**.
> 1. Sélectionnez **Inscriptions d’applications**.
> 1. Sélectionnez **+ Nouvelle inscription d’application**.
> 1. Entrez un nom pour l’**application** (nom du principal de service).
> 1. Sélectionnez **Comptes dans ce répertoire organisationnel uniquement**.
> 1. Pour l’URI de redirection, sélectionnez **Web** et entrez l’URL de votre choix. Il n’est pas nécessaire qu’elle soit réelle ni qu’elle fonctionne.
> 1. Sélectionnez ensuite **Inscription**.

##### <a name="configure-azure-ad-authentication-in-the-database-account"></a>Configurer l’authentification Azure AD dans le compte de base de données

Le principal de service ou l’identité managée doivent avoir l’autorisation d’obtenir des métadonnées pour la base de données, les schémas et les tables. Ils doivent également être en mesure d’interroger les tables à échantillonner pour la classification.

- [Configurer et gérer l’authentification Azure AD avec Azure SQL](../azure-sql/database/authentication-aad-configure.md)
- Si vous utilisez une identité managée, votre compte Purview possède sa propre identité managée, qui est fondamentalement votre nom Purview lorsque vous l’avez créé. Vous devez créer un utilisateur Azure AD dans Azure SQL Database avec l’identité managée exacte de Purview ou votre propre principal de service en suivant le tutoriel [Créer l’utilisateur de principal de service dans Azure SQL Database](../azure-sql/database/authentication-aad-service-principal-tutorial.md#create-the-service-principal-user-in-azure-sql-database). Vous devez affecter l’autorisation appropriée (par exemple `db_datareader`) à l’identité. Exemple de syntaxe SQL pour créer l’utilisateur et accorder l’autorisation :

    ```sql
    CREATE USER [Username] FROM EXTERNAL PROVIDER
    GO
    
    EXEC sp_addrolemember 'db_datareader', [Username]
    GO
    ```

    > [!Note]
    > `Username` est soit votre propre principal de service, soit l’identité managée de Purview. En savoir plus sur les [rôles de base de données fixes et leurs fonctionnalités](/sql/relational-databases/security/authentication-access/database-level-roles#fixed-database-roles).

##### <a name="add-service-principal-to-key-vault-and-purviews-credential"></a>Ajouter le principal de service aux informations d’identification de Purview et du coffre de clés

> [!Note]
> Si vous envisagez d’utiliser l’**identité managée** de Purview, vous pouvez ignorer cette étape, car l’identité managée par défaut de Purview figure déjà dans les informations d’identification de **Purview-MSI**.

Il est nécessaire d’obtenir l’ID d’application et le secret du principal de service :

1. Accédez à votre principal de service dans le [portail Azure](https://portal.azure.com).
1. Copiez les valeurs **ID d’application (client)** dans **Vue d’ensemble** et **Secret client** dans **Certificats et secrets**.
1. Accédez à votre coffre de clés.
1. Sélectionnez **Paramètres > Secrets**.
1. Sélectionnez **+ Générer/importer** et entrez le **nom** de votre choix et la **valeur** comme **secret client** de votre principal de service.
1. Sélectionnez **Créer** pour terminer.
1. Si votre coffre de clés n’est pas encore connecté à Purview, vous devrez [créer une connexion de coffre de clés](manage-credentials.md#create-azure-key-vaults-connections-in-your-azure-purview-account).
1. Enfin, [créez de nouvelles informations d’identification](manage-credentials.md#create-a-new-credential) à l’aide du principal de service pour configurer votre analyse.

### <a name="firewall-settings"></a>Paramètres du pare-feu

Si un pare-feu est activé sur votre serveur de base de données, vous devez mettre à jour le pare-feu pour autoriser l’accès de l’une des deux manières suivantes :

1. Autorisez les connexions Azure via le pare-feu.
1. Installez un runtime d'intégration auto-hébergé et donnez-lui accès par le biais du pare-feu.

#### <a name="allow-azure-connections"></a>Autoriser les connexions Azure

L’activation des connexions Azure permettra à Azure Purview d’atteindre et de connecter le serveur sans mettre à jour le pare-feu lui-même. Vous pouvez suivre le guide pratique pour des [connexions à partir d’Azure](../azure-sql/database/firewall-configure.md#connections-from-inside-azure).

1. Accédez à votre compte de base de données.
1. Dans la page **Vue d’ensemble**, sélectionnez le nom du serveur.
1. Sélectionnez **Sécurité > Pare-feux et réseaux virtuels**.
1. Sélectionnez **Oui** pour **Autoriser les services et les ressources Azure à accéder à ce serveur**.

    :::image type="content" source="media/register-scan-azure-sql-database/sql-firewall.png" alt-text="Autoriser les services et les ressources Azure à accéder à ce serveur" border="true":::

#### <a name="self-hosted-integration-runtime"></a>Runtime d’intégration auto-hébergé

Un runtime d’intégration auto-hébergé (SHIR) peut être installé sur un ordinateur pour se connecter à une ressource d’un réseau privé.

1. [Créez et installez un runtime d’intégration auto-hébergé](./manage-integration-runtimes.md) sur un ordinateur personnel ou sur un ordinateur situé dans le même réseau virtuel que votre serveur de base de données.
1. Vérifiez le pare-feu de votre serveur de base de données pour vérifier que l’ordinateur SHIR a accès par le biais du pare-feu. Ajoutez l’adresse IP de la machine si elle ne dispose pas déjà d’un accès.
1. Si votre SQL Server Azure se trouve derrière un point de terminaison privé ou dans un réseau virtuel, vous pouvez utiliser un [point de terminaison privé](catalog-private-link-ingestion.md#deploy-self-hosted-integration-runtime-ir-and-scan-your-data-sources) d’ingestion pour garantir l’isolement réseau de bout en bout.

## <a name="register-an-azure-sql-database-data-source"></a>Inscrire une source de données Azure SQL Database

Pour inscrire une nouvelle base de données Azure SQL Database dans votre catalogue de données, effectuez les actions suivantes :

1. Accédez à votre compte Purview.

1. Sélectionnez **Data Map** dans le volet de navigation de gauche.

1. Sélectionnez **Inscrire**.

1. Sous **Inscrire des sources**, sélectionnez **Azure SQL Database**. Sélectionnez **Continuer**.

:::image type="content" source="media/register-scan-azure-sql-database/register-new-data-source.png" alt-text="Inscrire une nouvelle source de données" border="true":::

Dans l’écran **Inscrire des sources (Azure SQL Database)** , procédez comme suit :

1. Entrez le **nom** avec lequel la source de données sera listée dans le catalogue.
1. Sélectionnez **À partir de l’abonnement Azure**, puis sélectionnez l’abonnement approprié dans la zone de liste déroulante **Abonnement Azure** et le serveur approprié dans la zone de liste déroulante **Nom du serveur**.
1. Sélectionnez **Inscrire** pour inscrire la source de données.

:::image type="content" source="media/register-scan-azure-sql-database/add-azure-sql-database.png" alt-text="Options d’inscription de sources" border="true":::

[!INCLUDE [create and manage scans](includes/manage-scans.md)]

> [!NOTE]
> * La suppression de votre analyse ne supprime pas vos ressources des analyses Azure SQL Database précédentes.
> * La ressource n’est plus mise à jour avec les modifications de schéma si votre table source est modifiée et que vous relancez l’analyse de la table source après avoir modifié la description sous l’onglet Schéma de Purview.

## <a name="next-steps"></a>Étapes suivantes

- [Navigation dans le catalogue de données Azure Purview](how-to-browse-catalog.md)
- [Recherche dans le catalogue de données Azure Purview](how-to-search-catalog.md)