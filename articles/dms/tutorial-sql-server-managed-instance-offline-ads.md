---
title: 'Tutoriel : Migrer SQL Server vers Azure SQL Managed Instance hors connexion avec Azure Data Studio'
titleSuffix: Azure Database Migration Service
description: Migrer SQL Server vers Azure SQL Managed Instance hors connexion à l’aide d’Azure Data Studio et d’Azure Database Migration Service (préversion)
services: dms
author: mokabiru
ms.author: mokabiru
manager: ''
ms.reviewer: ''
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: tutorial
ms.date: 10/05/2021
ms.openlocfilehash: a94587fc0ccaa2683ee00e2da01efad4cad77184
ms.sourcegitcommit: 57b7356981803f933cbf75e2d5285db73383947f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2021
ms.locfileid: "129546416"
---
# <a name="tutorial-migrate-sql-server-to-an-azure-sql-managed-instance-offline-using-azure-data-studio-with-dms-preview"></a>Tutoriel : Migrer SQL Server vers Azure SQL Managed Instance hors connexion avec Azure Data Studio et DMS (préversion)

Vous pouvez utiliser l’extension de migration Azure SQL dans Azure Data Studio pour migrer la ou les bases de données d’une instance de SQL Server vers Azure SQL Managed Instance. Pour connaître les méthodes qui peuvent nécessiter un effort manuel, consultez l’article [Migration d’une instance de SQL Server vers Azure SQL Managed Instance](../azure-sql/migration-guides/managed-instance/sql-server-to-managed-instance-guide.md).

Dans ce tutoriel, vous allez migrer la base de données **Adventureworks** d’une instance locale de SQL Server vers Azure SQL Managed Instance à l’aide d’Azure Data Studio et d’Azure Database Migration Service (DMS). Ce tutoriel se concentre sur le mode de migration hors connexion, qui prend en compte un temps d’arrêt acceptable durant le processus de migration.

Dans ce tutoriel, vous apprenez à effectuer les opérations suivantes :
> [!div class="checklist"]
>
> * Lancer l’Assistant Migration vers Azure SQL dans Azure Data Studio.
> * Exécuter une évaluation de vos bases de données SQL Server sources
> * Spécifier les détails de votre serveur SQL Server source, de l’emplacement de sauvegarde et de votre instance cible d’Azure SQL Managed Instance
> * Créer une instance d’Azure Database Migration Service et installer le runtime d’intégration autohébergé pour accéder au serveur source et aux sauvegardes.
> * Démarrer et superviser la progression de votre migration jusqu’à son achèvement

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Cet article décrit une migration hors connexion de SQL Server vers une instance managée SQL. Pour une migration en ligne, consultez [Migrer SQL Server vers Azure SQL Managed Instance en ligne à l’aide d’Azure Data Studio](tutorial-sql-server-managed-instance-online-ads.md).

## <a name="prerequisites"></a>Prérequis

Pour suivre ce didacticiel, vous devez effectuer les opérations suivantes :

* [Télécharger et installer Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio)
* [Installer l’extension de migration Azure SQL](/sql/azure-data-studio/extensions/azure-sql-migration-extension) à partir de la place de marché Azure Data Studio
* Disposer d’un compte Azure affecté à l’un des rôles intégrés listés ci-dessous :
    - Contributeur pour Azure SQL Managed Instance cible (et compte Stockage pour charger vos fichiers de sauvegarde de base de données à partir d’un partage réseau SMB).
    - Rôle de propriétaire ou de contributeur pour les groupes de ressources Azure contenant Azure SQL Managed Instance cible ou le compte de stockage Azure.
    - Rôle de propriétaire ou de contributeur pour l’abonnement Azure.
* Créez une instance managée SQL en suivant les indications de l’article [Créer une instance managée SQL dans le portail Azure](../azure-sql/managed-instance/instance-create-quickstart.md).
* Vérifiez que les comptes de connexion utilisés pour connecter l’instance SQL Server source sont membres du rôle serveur *sysadmin* ou ont l’autorisation `CONTROL SERVER`. 
* Fournissez un partage réseau SMB, un partage de fichiers de compte Stockage Azure ou un conteneur d’objets blob de compte Stockage Azure contenant vos fichiers de sauvegarde complète de base de données et de sauvegarde des journaux de transactions suivants, car Azure Database Migration Service s’en sert pour la migration de base de données.
    > [!IMPORTANT]
    > - Si vos fichiers de sauvegarde de base de données sont fournis sur un partage réseau SMB, vous devez [créer un compte Stockage Azure](../storage/common/storage-account-create.md) qui permet au service DMS de charger les fichiers de sauvegarde de base de données et de les utiliser pour la migration.  Veillez à créer le compte de stockage Azure dans la même région que celle où l’instance d’Azure Database Migration Service est créée.
    > - Azure Database Migration Service ne lance pas les sauvegardes. Il utilise les sauvegardes existantes (qui peuvent déjà faire partie de votre plan de récupération d'urgence) pour la migration.
    > - Vous devez effectuer des [sauvegardes à l’aide de l' `WITH CHECKSUM` option](/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server?preserve-view=true&view=sql-server-2017). 
    > - Chaque sauvegarde peut être enregistrée dans un fichier de sauvegarde distinct ou dans plusieurs fichiers de sauvegarde. Toutefois, l’ajout de plusieurs sauvegardes (par exemple, full et t-log) sur un seul support de sauvegarde n’est pas pris en charge. 
    > - Vous pouvez fournir des sauvegardes compressées pour réduire le risque de rencontrer des problèmes de migration de sauvegardes volumineuses.
* Assurez-vous que le compte de service exécutant l’instance source SQL Server dispose des autorisations en lecture et en écriture sur le partage réseau SMB qui contient les fichiers de sauvegarde de base de données.

* Si vous migrez une base de données protégée par un chiffrement TDE (Transparent Data Encryption), le certificat de l’instance source de SQL Server doit être migré vers l’instance cible d’Azure SQL Managed Instance avant la restauration de la base de données. Pour en savoir plus, consultez [Migrer vers Azure SQL Managed Instance un certificat d’une base de données protégée par TDE](../azure-sql/managed-instance/tde-certificate-migrate.md).
    > [!TIP]
    > Si votre base de données contient des données sensibles protégées par la fonctionnalité [Always Encrypted](/sql/relational-databases/security/encryption/configure-always-encrypted-using-sql-server-management-studio), le processus de migration à l’aide d’Azure Data Studio et de DMS effectue automatiquement la migration de vos clés Always Encrypted vers votre instance cible d’Azure SQL Managed Instance.

* Fournissez une machine sur laquelle installer le [runtime d’intégration autohébergé](../data-factory/create-self-hosted-integration-runtime.md) pour accéder aux sauvegardes de base de données et les migrer **si vos sauvegardes de base de données se trouvent sur un partage de fichiers réseau**. L’Assistant Migration vous fournit le lien de téléchargement et les clés d’authentification nécessaires pour télécharger et installer votre runtime d’intégration autohébergé. En prévision de la migration, vérifiez que les règles de pare-feu sortantes et les noms de domaine suivants sont activés sur la machine où vous devez installer le runtime d’intégration autohébergé :

    | Noms de domaine                                          | Ports sortants | Description                |
    | ----------------------------------------------------- | -------------- | ---------------------------|
    | Cloud public : `{datafactory}.{region}.datafactory.azure.net`<br> ou `*.frontend.clouddatahub.net` <br> Azure Government : `{datafactory}.{region}.datafactory.azure.us` <br> Chine : `{datafactory}.{region}.datafactory.azure.cn` | 443            | Requis par l’IR auto-hébergé pour se connecter au service de migration des données. <br>Pour les fabriques de données Data Factory créées dans un cloud public, recherchez le FQDN dans votre clé de runtime d’intégration autohébergé au format {datafactory}.{region}.datafactory.azure.net. Pour une ancienne fabrique de données, si vous ne voyez pas le nom de domaine complet dans votre clé d’intégration auto-hébergée, utilisez *.frontend.clouddatahub.net à la place. |
    | `download.microsoft.com`    | 443            | Exigé par le runtime d’intégration auto-hébergé pour télécharger les mises à jour. Si vous avez désactivé la mise à jour automatique, vous pouvez ignorer la configuration de ce domaine. |
    | `*.core.windows.net`          | 443            | Utilisé par le runtime d’intégration autohébergé pour se connecter au compte Stockage Azure afin de charger les sauvegardes de base de données à partir de votre partage réseau |

    > [!TIP]
    > Si vos fichiers de sauvegarde de base de données sont déjà fournis dans un compte de stockage Azure, l’IR auto-hébergé n’est pas requis pendant le processus de migration.

* Lorsque vous utilisez l’IR auto-hébergé, assurez-vous que l’ordinateur sur lequel le runtime est installé peut se connecter à l’instance SQL Server et au partage de fichiers réseau source où se trouvent les fichiers de sauvegarde. Le port de sortie 445 doit être activé pour autoriser l’accès au partage de fichiers réseau. Consultez également les [recommandations d’utilisation du runtime d’intégration autohébergé](migration-using-azure-data-studio.md#recommendations-for-using-self-hosted-integration-runtime-for-database-migrations)
* Si vous utilisez l’Azure Database Migration Service pour la première fois, assurez-vous que le fournisseur de ressources Microsoft.DataMigration est inscrit dans votre abonnement. Vous pouvez suivre les étapes pour [inscrire le fournisseur de ressources](quickstart-create-data-migration-service-portal.md#register-the-resource-provider)

## <a name="launch-the-migrate-to-azure-sql-wizard-in-azure-data-studio"></a>Lancer l’Assistant Migration vers Azure SQL dans Azure Data Studio

1. Ouvrez Azure Data Studio, puis sélectionnez l’icône de serveur pour vous connecter à votre serveur SQL Server local (ou à SQL Server sur les machines virtuelles Azure).
1. Cliquez avec le bouton droit sur la connexion au serveur, puis sélectionnez **Gérer**.
1. Dans la page d’accueil du serveur, sélectionnez l’extension **Migration Azure SQL**.
1. Dans le tableau de bord de l’extension de migration Azure SQL, sélectionnez **Migrer vers Azure SQL** pour lancer l’Assistant Migration.
    :::image type="content" source="media/tutorial-sql-server-to-managed-instance-offline-ads/launch-migrate-to-azure-sql-wizard.png" alt-text="Lancer l’Assistant Migration vers Azure SQL":::
1. Dans la première étape de l’Assistant Migration, liez votre compte Azure si vous êtes déjà connecté à Azure Data Studio, ou liez un nouveau compte Azure.

## <a name="run-database-assessment-and-select-target"></a>Exécuter une évaluation de base de données et sélectionner la cible

1. Sélectionnez la ou les bases de données à évaluer, puis sélectionnez **Suivant**.
1. Sélectionnez Azure SQL Managed Instance en tant que cible.
    :::image type="content" source="media/tutorial-sql-server-to-managed-instance-offline-ads/assessment-complete-target-selection.png" alt-text="Confirmation de l’évaluation":::
1. Sélectionnez le bouton **Afficher/Sélectionner** pour voir les détails des résultats de l’évaluation de vos bases de données, sélectionnez les bases de données à migrer, puis sélectionnez **OK**.
    :::image type="content" source="media/tutorial-sql-server-to-managed-instance-offline-ads/assessment-issues-details.png" alt-text="Détails de l’évaluation de base de données":::
1. Spécifiez votre instance cible d’**Azure SQL Managed Instance** en sélectionnant l’abonnement, l’emplacement et le groupe de ressources appropriés dans les listes déroulantes correspondantes, puis sélectionnez **Suivant**.

## <a name="configure-migration-settings"></a>Configurer les paramètres de migration

1. Sélectionnez le mode de migration **Migration en ligne**.
    > [!NOTE]
    > En mode de migration hors connexion, la base de données SQL Server source n’est pas disponible pour les activités de lecture et d’écriture pendant que les sauvegardes de base de données sont restaurées sur l’instance cible d’Azure SQL Managed Instance. Le temps d’arrêt des applications doit être pris en compte jusqu’à la fin de la migration.
1. Sélectionnez l’emplacement de vos sauvegardes de base de données. Vos sauvegardes de base de données peuvent se trouver sur un partage réseau local ou dans un conteneur Azure Storage Blob.

    > [!NOTE]
    > Si vos sauvegardes de base de données se trouvent sur un partage réseau local, DMS vous demande de configurer le runtime d’intégration autohébergé à l’étape suivante de l’Assistant. Le runtime d’intégration autohébergé est nécessaire pour accéder aux sauvegardes de votre base de données source, vérifier la validité du jeu de sauvegarde et charger ce dernier vers le compte Stockage Azure.<br/> Si vos sauvegardes de base de données se trouvent déjà dans un conteneur Azure Storage Blob, vous n’avez pas besoin de configurer le runtime d’intégration autohébergé.

1. Après avoir sélectionné l’emplacement de sauvegarde, fournissez les détails relatifs à votre serveur SQL Server source et à l’emplacement de sauvegarde de la source.

    |Champ    |Description  |
    |---------|-------------|
    |**Informations d’identification de la source - Nom d’utilisateur**    |Informations d’identification (authentification Windows/SQL) permettant de se connecter à l’instance source de SQL Server et de valider les fichiers de sauvegarde.         |
    |**Informations d’identification de la source - Mot de passe utilisateur**    |Informations d’identification (authentification Windows/SQL) permettant de se connecter à l’instance source de SQL Server et de valider les fichiers de sauvegarde.         |
    |**Emplacement de partage réseau qui contient les sauvegardes**     |Emplacement de partage réseau qui contient les fichiers de sauvegarde complète et de sauvegarde des journaux de transactions. Les fichiers non valides ou les fichiers de sauvegarde du partage réseau qui n’appartiennent pas au jeu de sauvegarde valide sont automatiquement ignorés durant le processus de migration.        |
    |**Compte d’utilisateur Windows avec accès en lecture à l’emplacement du partage réseau**     |Informations d’identification Windows (nom d’utilisateur) du compte ayant accès en lecture au partage réseau pour récupérer les fichiers de sauvegarde.       |
    |**Mot de passe**     |Informations d’identification Windows (Mot de passe) du compte ayant accès en lecture au partage réseau pour récupérer les fichiers de sauvegarde.         |
    |**Nom de la base de données cible** |Vous pouvez modifier le nom de la base de données cible si vous souhaitez changer le nom de la base de données sur la cible durant le processus de migration.            |

1. Spécifiez le **compte Stockage Azure** en sélectionnant l’**abonnement**, l’**emplacement** et le **groupe de ressources** appropriés dans les listes déroulantes correspondantes. Ce compte Stockage Azure est utilisé par DMS pour charger les sauvegardes de base de données à partir du partage réseau. Vous n’avez pas besoin de créer de conteneur, car DMS crée automatiquement un conteneur d’objets blob dans le compte de stockage spécifié durant le processus de chargement.
> [!IMPORTANT]
> Si la fonctionnalité du contrôle de bouclage est activée et que le SQL Server source et le partage de fichiers se trouvent sur le même ordinateur, la source ne peut pas accéder au partage de fichiers à l’aide du FQDN. Pour résoudre ce problème, désactivez la fonctionnalité du contrôle de bouclage à l’aide des instructions indiquées [ici](https://support.microsoft.com/help/926642/error-message-when-you-try-to-access-a-server-locally-by-using-its-fqd)

## <a name="create-azure-database-migration-service"></a>Créer une instance d’Azure Database Migration Service

1. Créez une instance d’Azure Database Migration Service, ou réutilisez un service existant que vous avez créé.
    > [!NOTE]
    > Si vous avez créé une instance de DMS à l’aide du portail Azure, vous ne pouvez pas la réutiliser dans l’Assistant Migration d’Azure Data Studio. Seule l’instance de DMS créée à l’aide d’Azure Data Studio peut être réutilisée.
1. Sélectionnez le **groupe de ressources** où vous avez déjà une instance de DMS, ou dans lequel vous devez en créer une. La liste déroulante **Azure Database Migration Service** liste les instances existantes de DMS dans le groupe de ressources sélectionné.
1. Pour réutiliser une instance existante de DMS, sélectionnez-la dans la liste déroulante. L’état du runtime d’intégration autohébergé s’affiche ensuite au bas de la page.
1. Pour créer une instance de DMS, sélectionnez **Créer**. Dans l’écran **Créer une instance d’Azure Database Migration Service**, indiquez le nom de l’instance de DMS, puis sélectionnez **Créer**.
1. Une fois la création de l’instance de DMS réussie, vous recevez les détails nécessaires pour configurer le **runtime d’intégration**.
1. Sélectionnez **Télécharger et installer le runtime d’intégration** pour ouvrir le lien de téléchargement dans un navigateur web. Effectuez le téléchargement. Installez le runtime d’intégration sur une machine qui répond aux prérequis de connexion au serveur SQL Server source et à l’emplacement contenant la sauvegarde de la source.
1. Une fois l’installation effectuée, le **Gestionnaire de configuration de Microsoft Integration Runtime** se lance automatiquement pour débuter le processus d’inscription.
1. Copiez et collez l’une des clés d’authentification fournies dans l’écran de l’Assistant au sein d’Azure Data Studio. Si la clé d’authentification est valide, une icône représentant une coche verte s’affiche dans le Gestionnaire de configuration d’Integration Runtime pour indiquer que vous pouvez passer à l’**inscription**.
1. Une fois l’inscription du runtime d’intégration autohébergé réussie, fermez le **Gestionnaire de configuration de Microsoft Integration Runtime**, puis revenez à l’Assistant Migration dans Azure Data Studio.
1. Sélectionnez **Tester la connexion** dans l’écran **Créer une instance d’Azure Database Migration Service** au sein d’Azure Data Studio pour vérifier que l’instance de DMS créée est connectée au runtime d’intégration autohébergé récemment inscrit.
    :::image type="content" source="media/tutorial-sql-server-to-managed-instance-offline-ads/test-connection-integration-runtime-complete.png" alt-text="Tester le runtime d’intégration de la connexion":::
1. Passez en revue le récapitulatif, puis sélectionnez **Terminé** pour démarrer la migration de base de données.

## <a name="monitor-your-migration"></a>Superviser la migration

1. Dans l’**État de la migration de base de données**, vous pouvez suivre les migrations en cours, les migrations effectuées et les migrations non réussies (le cas échéant).

    :::image type="content" source="media/tutorial-sql-server-to-managed-instance-offline-ads/monitor-migration-dashboard.png" alt-text="tableau de bord de supervision de la migration":::
1. Sélectionnez **Migrations de base de données en cours** pour voir les migrations en cours et obtenir plus de détails en sélectionnant le nom de la base de données.
1. La page des détails de la migration affiche les fichiers de sauvegarde et leur état correspondant :

    | Statut | Description |
    |--------|-------------|
    | Arrivé | Le fichier de sauvegarde est arrivé à l’emplacement de sauvegarde de la source et a été validé |
    | Chargement | Le runtime d’intégration charge le fichier de sauvegarde vers le service Stockage Azure|
    | Téléchargé | Le fichier de sauvegarde est chargé vers le service Stockage Azure |
    | Restoring | Azure Database Migration Service restaure le fichier de sauvegarde sur Azure SQL Managed Instance|
    | Restaurée | Le fichier de sauvegarde a été correctement restauré sur Azure SQL Managed Instance |
    | Opération annulée | Le processus de migration a été annulé |
    | Ignoré | Le fichier de sauvegarde a été ignoré, car il n’appartient à aucune chaîne de sauvegarde de base de données valide |

    :::image type="content" source="media/tutorial-sql-server-to-managed-instance-offline-ads/offline-to-mi-migration-details-inprogress-backups-restored.png" alt-text="détails d’une restauration de sauvegarde hors connexion":::

Une fois que toutes les sauvegardes de base de données ont été restaurées sur Azure SQL Managed Instance, un basculement automatique de la migration est lancé par Azure DMS pour garantir que la base de données migrée dans Azure SQL Managed Instance est prête à l’emploi. De plus, l’état de la migration passe de *en cours* à *opération réussie*.

> [!IMPORTANT]
> Après la migration, la disponibilité de SQL Managed Instance avec le niveau de service Critique pour l’entreprise peut prendre beaucoup plus de temps à atteindre que la disponibilité du niveau de service Universel, car trois réplicas secondaires doivent être amorcés pour le groupe de haute disponibilité AlwaysOn. La durée de cette opération dépend de la taille des données. Pour plus d’informations, consultez [Durée des opérations de gestion](../azure-sql/managed-instance/management-operations-overview.md#duration).

## <a name="next-steps"></a>Étapes suivantes

* Pour accéder à un tutoriel vous expliquant comment migrer une base de données vers SQL Managed Instance à l’aide de la commande T-SQL RESTORE, consultez [Restaurer une sauvegarde vers SQL Managed Instance à l’aide de la commande restore](../azure-sql/managed-instance/restore-sample-database-quickstart.md).
* Pour obtenir des informations sur SQL Managed Instance, consultez [Présentation de SQL Managed Instance](../azure-sql/managed-instance/sql-managed-instance-paas-overview.md).
* Pour plus d’informations sur la connexion d’applications à SQL Managed Instance , consultez [Connecter des applications](../azure-sql/managed-instance/connect-application-instance.md).
