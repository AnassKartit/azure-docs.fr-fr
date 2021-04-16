---
title: Migration de la série StorSimple 8000 vers Azure File Sync
description: Découvrez comment effectuer une migration d’une appliance StorSimple 8100 ou 8600 vers Azure File Sync.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 10/16/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 1d2de439e661ef5b1d1669187355621f25400bc4
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106075570"
---
# <a name="storsimple-8100-and-8600-migration-to-azure-file-sync"></a>Migration de StorSimple 8100 et 8600 vers Azure File Sync

La série StorSimple 8000 est représentée par les appliances physiques locales 8100 ou 8600 et leurs composants de service cloud. Il est possible d’effectuer la migration des données de l’une ou l’autre de ces appliances vers un environnement Azure File Sync. Azure File Sync est le service Azure à long terme stratégique et par défaut vers lequel les appliances StorSimple peuvent être migrées.

StorSimple série 8000 atteindra sa [fin de vie](https://support.microsoft.com/en-us/lifecycle/search?alpha=StorSimple%208000%20Series) en décembre 2022. Il est important de planifier votre migration le plus tôt possible. Cet article donne les étapes à suivre pour effectuer correctement une migration vers Azure File Sync, en fournissant toutes les connaissances générales nécessaires.

## <a name="phase-1-prepare-for-migration"></a>Phase 1 : Préparation de la migration

Cette section contient les étapes à suivre au début de la migration des volumes StorSimple vers des partages de fichiers Azure.

### <a name="inventory"></a>Inventaire

Lorsque vous commencez à planifier votre migration, identifiez tout d’abord toutes les appliances et tous les volumes StorSimple à migrer. Une fois que vous avez terminé, vous pouvez choisir le chemin de migration qui vous convient le mieux.

* Pour les appliances physiques StorSimple (série 8000) : utilisez ce guide de migration.
* Pour les appliances virtuelles, [série StorSimple 1200 : utilisez un autre guide de migration](storage-files-migration-storsimple-1200.md).

### <a name="migration-cost-summary"></a>Résumé des coûts de migration

Les migrations vers des partages de fichiers Azure à partir de volumes StorSimple et via des tâches de migration dans une ressource StorSimple Data Manager sont gratuites. Toutefois, d’autres coûts peuvent être appliqués pendant et après une migration :

* **Sortie réseau :** Vos fichiers StorSimple résident dans un compte de stockage au sein d’une région Azure spécifique. Si vous provisionnez les partages de fichiers Azure que vous déplacez vers un compte de stockage qui se trouve dans la même région Azure, il n’y aura aucun coût de sortie. Dans le cadre de cette migration, vous pouvez déplacer vos fichiers vers un compte de stockage situé dans une autre région. Dans ce cas, des coûts de sortie vous seront facturés.
* **Transactions de partage de fichiers Azure :** Lorsque des fichiers sont copiés dans un partage de fichiers Azure (dans le cadre d’une migration ou autre cas de figure), des coûts de transaction s’appliquent à l’écriture des fichiers et des métadonnées. La meilleure pratique consiste à démarrer votre partage de fichiers Azure sur le niveau Transaction optimisée pendant la migration. Basculez sur le niveau souhaité une fois la migration terminée. Les phases suivantes vous le signaleront au moment opportun.
* **Modification d’un niveau de partage de fichiers Azure :** La modification du niveau d’un partage de fichiers Azure entraîne des coûts de transaction. Dans la plupart des cas, il est plus rentable de suivre les conseils du point précédent.
* **Coût de stockage :** Lorsque cette migration commence à copier des fichiers dans un partage de fichiers Azure, le stockage Azure Files est consommé et facturé. Les sauvegardes migrées deviendront des [instantanés de partage de fichiers Azure](storage-snapshots-files.md). Les instantanés de partage de fichiers ne consomment de la capacité de stockage que pour les différences qu'ils contiennent.
* **StorSimple :** Tant que vous n’avez pas déprovisionné les appareils StorSimple et les comptes de stockage, les coûts de StorSimple pour le stockage, les sauvegardes et les appliances continueront à être facturés.

### <a name="direct-share-access-vs-azure-file-sync"></a>Accès direct au partage par rapport à Azure File Sync

Les partages de fichiers Azure ouvrent un tout nouveau monde de possibilités pour structurer le déploiement de vos services de fichiers. Un partage de fichiers Azure désigne simplement un partage SMB dans le cloud, que vous pouvez configurer pour que les utilisateurs y accèdent directement via le protocole SMB avec l’authentification Kerberos habituelle et les autorisations NTFS existantes (listes de contrôle d’accès de fichiers et de dossiers) fonctionnant en mode natif. En savoir plus sur l’[accès aux partages de fichiers Azure en fonction de l’identité](storage-files-active-directory-overview.md).

Une alternative à l’accès direct est [Azure File Sync](./storage-sync-files-planning.md). Azure File Sync est un équivalent direct de la mise en cache locale des fichiers fréquemment utilisés proposée par StorSimple.

Azure File Sync est un service cloud Microsoft basé sur deux composants principaux :

* Synchronisation des fichiers et hiérarchisation cloud pour créer un cache d'accès/performances sur n'importe quel serveur Windows.
* Partages de fichiers comme stockage natif dans Azure, accessibles par le biais de différents protocoles comme SMB et File REST.

Les partages de fichiers Azure conservent d’importants aspects de fidélité sur les fichiers stockés comme les attributs, les autorisations et les horodatages. Grâce aux partages de fichiers Azure, il n’est plus nécessaire qu’une application ou un service interprète les fichiers et dossiers stockés dans le cloud. Vous pouvez y accéder en mode natif via des protocoles et des clients familiers, tels que l’Explorateur de fichiers Windows. Les partages de fichiers Azure vous permettent de stocker dans le cloud des données d’applications et des données de serveurs de fichiers à usage général. La sauvegarde d’un partage de fichiers Azure est une fonctionnalité intégrée qui peut être améliorée par Sauvegarde Azure.

Cet article est consacré aux étapes de migration. Si vous souhaitez en savoir plus sur Azure File Sync avant d’effectuer la migration, consultez les articles suivants :

* [Vue d’ensemble d’Azure File Sync](./storage-sync-files-planning.md "Vue d’ensemble")
* [Guide de déploiement d’Azure File Sync](storage-sync-files-deployment-guide.md)

### <a name="storsimple-service-data-encryption-key"></a>Clé de chiffrement des données de service StorSimple

Lorsque vous avez configuré votre appliance StorSimple pour la première fois, celle-ci a généré une « clé de chiffrement des données de service » et vous a demandé de stocker cette clé de façon sécurisée. Cette clé est utilisée pour chiffrer toutes les données du compte de stockage Azure associé dans lequel l’appliance StorSimple stocke vos fichiers.

La « clé de chiffrement des données de service » est nécessaire pour une migration réussie. Le moment est venu de récupérer cette clé dans vos dossiers, pour chacune des appliances de votre inventaire.

Si vous ne trouvez pas les clés dans vos dossiers, vous pouvez générer une nouvelle clé à partir de l’appliance. Chaque appliance a une clé de chiffrement unique.

#### <a name="change-the-service-data-encryption-key"></a>Modifier la clé de chiffrement des données de service

[!INCLUDE [storage-files-migration-generate-key](../../../includes/storage-files-migration-generate-key.md)]

> [!CAUTION]
> Lorsque vous décidez de la façon de vous connecter à votre appliance StorSimple, prenez en compte les points suivants :
>
> * La connexion via une session HTTPS est l’option la plus sûre et celle qui est recommandée.
> * Une connexion directe à la console série de l’appareil est sécurisée, mais une connexion à la console via des commutateurs réseau ne l’est pas.
> * Les connexions de session HTTP sont possibles, mais elles ne sont *pas chiffrées*. Elles ne sont donc pas recommandées, sauf si vous les utilisez au sein d’un réseau approuvé et fermé.

### <a name="storsimple-volume-backups"></a>Sauvegardes de volume StorSimple

StorSimple propose des sauvegardes différentielles au niveau du volume. Les partages de fichiers Azure ont également cette capacité, appelée « instantanés de partage ».
Vos tâches de migration ne peuvent déplacer que les sauvegardes, pas les données du volume actif. Par conséquent, la sauvegarde la plus récente doit toujours figurer dans la liste des sauvegardes déplacées lors d'une migration.

Déterminez si vous devez déplacer les anciennes sauvegardes au cours de la migration.
La meilleure pratique consiste à réduire le plus possible cette liste, afin que vos tâches de migration soient traitées plus rapidement.

Pour identifier les sauvegardes critiques qui doivent être migrées, établissez une liste de vérification de vos stratégies de sauvegarde. Exemple :
* la sauvegarde la plus récente. (Remarque : la sauvegarde la plus récente doit toujours figurer dans cette liste).
* Une sauvegarde par mois pendant 12 mois.
* Une sauvegarde par an pendant trois ans. 

Plus tard, lors de la création de vos tâches de migration, vous pourrez utiliser cette liste pour identifier précisément les sauvegardes de volume StorSimple qui doivent être migrées afin de répondre aux exigences de votre liste.

> [!CAUTION]
> Il est impossible de sélectionner plus de **50** sauvegardes de volume StorSimple.
> Vos tâches de migration ne peuvent déplacer que les sauvegardes, jamais les données du volume actif. Par conséquent, la sauvegarde la plus récente est la plus proche des données actives et doit donc toujours figurer sur la liste des sauvegardes à déplacer dans le cadre d'une migration.

### <a name="map-your-existing-storsimple-volumes-to-azure-file-shares"></a>Mapper vos volumes StorSimple existants à des partages de fichiers Azure

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

### <a name="number-of-storage-accounts"></a>Nombre de comptes de stockage

Votre migration est susceptible de tirer parti d’un déploiement de plusieurs comptes de stockage, qui contiennent chacun un plus petit nombre de partages de fichiers Azure.

Si vos partages de fichiers sont très actifs (utilisés par de nombreux utilisateurs ou applications), seuls deux partages de fichiers Azure peuvent atteindre la limite de performances de votre compte de stockage. Pour conséquent, la meilleure pratique consiste à effectuer une migration vers plusieurs comptes de stockage, chacun disposant de ses propres partages de fichiers individuels, et généralement pas plus de deux ou trois partages par compte de stockage.

Une meilleure pratique consiste à déployer les comptes de stockage avec un partage de fichiers pour chaque. Vous pouvez regrouper plusieurs partages de fichiers Azure dans le même compte de stockage s’ils sont destinés à l’archivage.

Ces considérations s’appliquent davantage à l’[accès direct au cloud](#direct-share-access-vs-azure-file-sync) (par le biais d’une machine virtuelle ou d’un service Azure) qu’à Azure File Sync. Si vous envisagez d'utiliser exclusivement Azure File Sync sur ces partages, vous pouvez regrouper plusieurs partages sur le même compte de stockage Azure. À l'avenir, vous souhaiterez peut-être effectuer une migration lift-and-shift d'une application vers le cloud, avec accès direct à un partage de fichiers. Ce scénario pourrait permettre de bénéficier d'un nombre d'IOPS et d'un débit plus élevés. Ou vous pourriez commencer à utiliser un service Azure qui permettrait également de bénéficier d'un nombre d'IOPS et d'un débit plus élevés.

Si vous avez établi la liste de vos partages, mappez chaque partage au compte de stockage dans lequel il résidera.

> [!IMPORTANT]
> Choisissez une région Azure, puis vérifiez que chaque compte de stockage et chaque ressource Azure File Sync correspondent à la région que vous avez sélectionnée.
> Ne configurez pas les paramètres réseau et les paramètres de pare-feu des comptes de stockage pour le moment. À ce stade, ces configurations rendraient la migration impossible. Vous configurerez ces paramètres de stockage Azure une fois la migration terminée.

### <a name="phase-1-summary"></a>Récapitulatif de la phase 1

À la fin de la phase 1 :

* Vous avez une bonne vue d’ensemble de vos appareils et volumes StorSimple.
* Le service Data Manager est prêt à accéder à vos volumes StorSimple dans le cloud, car vous avez récupéré votre « clé de chiffrement des données de service » pour chaque appareil StorSimple.
* Vous disposez d'un plan pour les volumes et les sauvegardes (s'il y en a au-delà de la plus récente) qui doivent être migrés.
* Vous savez comment mapper vos volumes avec le nombre approprié de partages de fichiers et de comptes de stockage Azure.

## <a name="phase-2-deploy-azure-storage-and-migration-resources"></a>Phase 2 : Déployer des ressources de stockage et de migration Azure

Cette section décrit les considérations relatives au déploiement des différents types de ressources nécessaires dans Azure. Certaines conserveront vos données après la migration et d’autres sont nécessaires uniquement pour la migration. Ne commencez pas à déployer les ressources avant d’avoir finalisé votre plan de déploiement. Il est difficile, parfois impossible, de modifier certains aspects de vos ressources Azure une fois qu’elles ont été déployées.

### <a name="deploy-storage-accounts"></a>Déployer des comptes de stockage

Vous aurez probablement besoin de déployer plusieurs comptes de stockage Azure. Chacun d’eux contiendra un plus petit nombre de partages de fichiers Azure, conformément au plan de déploiement vous avez terminé dans la section précédente de cet article. Rendez-vous sur le portail Azure pour [déployer les comptes de stockage que vous avez planifiés](../common/storage-account-create.md#create-a-storage-account). Vous pouvez utiliser les paramètres de base suivants pour tout nouveau compte de stockage.

> [!IMPORTANT]
> Ne configurez pas les paramètres réseau et les paramètres de pare-feu de vos comptes de stockage pour le moment. À ce stade, ces configurations rendraient la migration impossible. Vous configurerez ces paramètres de stockage Azure une fois la migration terminée.

#### <a name="subscription"></a>Abonnement

Vous pouvez utiliser le même abonnement que celui utilisé pour votre déploiement StorSimple ou en utiliser un autre. La seule limitation est que votre abonnement doit se trouver dans le même locataire Azure Active Directory que l’abonnement StorSimple. Avant toute migration, pensez à déplacer l'abonnement StorSimple vers le locataire approprié. Vous êtes obligé de déplacer l'intégralité de l'abonnement ; il est en effet impossible de déplacer des ressources StorSimple individuelles vers un autre locataire ou abonnement.

#### <a name="resource-group"></a>Resource group

Les groupes de ressources aident à l’organisation des ressources et à la gestion administrative des autorisations. En savoir plus sur les [groupes de ressources dans Azure](../../azure-resource-manager/management/manage-resource-groups-portal.md#what-is-a-resource-group).

#### <a name="storage-account-name"></a>Nom du compte de stockage

Le nom de votre compte de stockage devient partie intégrante d’une URL et présente certaines limitations de caractère. Dans votre convention de nommage, considérez que les noms de compte de stockage doivent être uniques au monde. N’autorisez que des lettres minuscules et des chiffres, exigez entre 3 et 24 caractères, et interdisez les caractères spéciaux tels que les tirets ou les traits de soulignement. Pour plus d’informations, consultez [Règles de nommage des ressources de stockage Azure](../../azure-resource-manager/management/resource-name-rules.md#microsoftstorage).

#### <a name="location"></a>Emplacement

La localisation, ou région Azure, d’un compte de stockage est très important. Si vous utilisez Azure File Sync, tous vos comptes de stockage doivent se trouver dans la même région que votre ressource de service de synchronisation de stockage. La région Azure que vous sélectionnez doit être proche ou centrale par rapport à vos serveurs et utilisateurs locaux. Une fois que votre ressource a été déployée, vous ne pouvez plus changer sa région.

Vous pouvez choisir une région différente de celle où résident actuellement vos données StorSimple (compte de stockage).

> [!IMPORTANT]
> Si vous choisissez une région différente de celle où se trouve votre compte de stockage StorSimple, des [frais de sortie s’appliqueront](https://azure.microsoft.com/pricing/details/bandwidth) pendant la migration. Les données quitteront la région StorSimple et entreront dans la région de votre compte de stockage. Aucuns frais de bande passante ne s’appliquent si vous restez dans la même région Azure.

#### <a name="performance"></a>Performances

Vous avez la possibilité de choisir entre le stockage Premium (SSD) et le stockage Standard pour les partages de fichiers Azure. Le stockage Standard comprend [plusieurs niveaux pour un partage de fichiers](storage-how-to-create-file-share.md#changing-the-tier-of-an-azure-file-share). Le stockage Standard est l’option qui convient à la plupart des clients qui migrent depuis StorSimple.

Vous ne savez toujours pas ?

* Choisissez le stockage Premium si vous avez besoin des [performances d’un partage de fichiers Azure Premium](understanding-billing.md#provisioned-model).
* Choisissez le stockage Standard pour les charges de travail du serveur de fichiers à usage général, y compris les données chaudes et les données d’archivage. Choisissez également le stockage Standard si la seule charge de travail sur le partage dans le cloud sera Azure File Sync.

#### <a name="account-kind"></a>Type de compte

* Pour le stockage standard, choisissez *StorageV2 (universel v2)* .
* Pour les partages de fichiers Premium, choisissez *FileStorage*.

#### <a name="replication"></a>Réplication

Plusieurs paramètres de réplication sont disponibles. En savoir plus sur les différents types de réplication.

Choisissez l’une des deux options suivantes :

* *Stockage localement redondant (LRS)* .
* *Stockage redondant interzone (ZRS)* , qui n’est pas disponible dans toutes les régions Azure.

> [!NOTE]
> Seuls les types de redondance LRS et ZRS sont compatibles avec les grands partages de fichiers Azure d’une capacité de 100 Tio.

Le stockage globalement redondant, toutes variantes confondues, n’est pas pris en charge pour le moment. Vous pouvez changer de type de redondance plus tard et passer à GRS lorsqu’Azure le prendra en charge.

#### <a name="enable-100-tib-capacity-file-shares"></a>Activer les partages de fichiers d’une capacité de 100 Tio

:::row:::
    :::column:::
        :::image type="content" source="media/storage-files-how-to-create-large-file-share/large-file-shares-advanced-enable.png" alt-text="Image illustrant l’onglet Avancé dans le portail Azure pour la création d’un compte de stockage.":::
    :::column-end:::
    :::column:::
        Dans la section **Avancé** de l’Assistant Nouveau compte de stockage du portail Azure, vous pouvez activer la prise en charge des **partages de fichiers volumineux** dans ce compte de stockage. Si cette option n’est pas disponible, vous avez probablement sélectionné le mauvais type de redondance. Veillez à sélectionner LRS ou ZRS pour que cette option soit disponible.
    :::column-end:::
:::row-end:::

Opter pour les grands partages de fichiers d’une capacité de 100 Tio présente plusieurs avantages :

* Vos performances sont considérablement accrues par rapport aux partages de fichiers plus petits d’une capacité de 5 Tio (par exemple, 10 fois plus d’E/S).
* La migration se terminera beaucoup plus rapidement.
* Le partage de fichiers disposera d'une capacité suffisante pour contenir toutes les données que vous migrerez vers celui-ci, y compris la capacité de stockage requise par les sauvegardes différentielles.
* La croissance future est couverte.

### <a name="azure-file-shares"></a>Partages de fichiers Azure

Une fois les comptes de stockage créés, accédez à la section **Partage de fichiers** du compte de stockage, puis déployez le nombre approprié de partages de fichiers Azure selon votre plan de migration (voir phase 1). Envisagez d’utiliser les paramètres de base suivants pour tout nouveau partage de fichiers dans Azure.

:::row:::
    :::column:::
        :::image type="content" source="media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-new-share.png" alt-text="Capture d’écran du portail Azure montrant l’interface utilisateur du nouveau partage de fichiers.":::
    :::column-end:::
    :::column:::
        </br>**Nom**</br>Les lettres minuscules, les chiffres et les traits d’union sont pris en charge.</br></br>**Quota**</br>Ici, le quota est comparable au quota inconditionnel SMB d’une instance Windows Server. Selon les bonnes pratiques, vous ne devez pas définir de quota ici, car votre migration et vos autres services échoueront lorsque ce quota sera atteint.</br></br>**Niveaux**</br>Sélectionnez **Transaction optimisée** pour votre nouveau partage de fichiers. Au cours de la migration, de nombreuses transactions seront exécutées. Il sera plus économique de remplacer votre niveau actuel plus tard par un niveau mieux adapté à votre charge de travail.
    :::column-end:::
:::row-end:::

### <a name="storsimple-data-manager"></a>StorSimple Data Manager

La ressource Azure qui contiendra vos tâches de migration s’appelle **StorSimple Data Manager**. Sélectionnez **Nouvelle ressource**, puis recherchez-la. Sélectionnez ensuite **Créer**.

Cette ressource temporaire est utilisée pour l’orchestration. Vous la déprovisionnerez une fois la migration terminée. Elle doit être déployée dans le même abonnement, le même groupe de ressources et la même région que votre compte de stockage StorSimple.

### <a name="azure-file-sync"></a>Azure File Sync

Azure File Sync vous permet d’ajouter la mise en cache locale des fichiers les plus fréquemment consultés. À l’instar des capacités de mise en cache de StorSimple, la fonctionnalité de hiérarchisation cloud d’Azure File Sync offre une latence d’accès local combinée à un meilleur contrôle de la capacité de cache disponible sur l’instance Windows Server et à une synchronisation multisite. Si votre objectif est de disposer d’un cache local, préparez dans votre réseau local une machine virtuelle Windows Server (les serveurs physiques et les clusters de basculement sont également pris en charge) avec une capacité suffisante de stockage en attachement direct.

> [!IMPORTANT]
> Ne configurez pas encore Azure File Sync. Il est préférable de configurer Azure File Sync une fois la migration de votre partage terminée. Le déploiement d’Azure File Sync ne doit pas démarrer avant la phase 4 d’une migration.

### <a name="phase-2-summary"></a>Récapitulatif de la phase 2

À la fin de la phase 2, vous aurez déployé vos comptes de stockage et tous les partages de fichiers Azure sur lesdits comptes. Vous aurez également une ressource StorSimple Data Manager. Vous utiliserez cette dernière dans la phase 3, lorsque vous configurerez vos tâches de migration.

## <a name="phase-3-create-and-run-a-migration-job"></a>Phase 3 : Création et exécution d’une tâche de migration

Cette section décrit comment configurer une tâche de migration et mapper avec soin les répertoires sur un volume StorSimple qui doit être copié dans le partage de fichiers Azure cible que vous sélectionnez. Pour commencer, accédez à StorSimple Data Manager, recherchez **Définitions de tâches** dans le menu, puis sélectionnez **+ Définition de tâche**. Le type de stockage cible qui convient est le type par défaut : **Partage de fichiers Azure**.

![Types de tâches de migration de StorSimple série 8000.](media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-new-job-type.png "Capture d’écran de Définitions de tâches dans le portail Azure avec une nouvelle boîte de dialogue Définition de tâche ouverte qui demande le type de tâche : Copier vers un partage de fichiers ou un conteneur de blobs.")

:::row:::
    :::column:::
        ![Tâche de migration de StorSimple série 8000.](media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-new-job.png "Capture d'écran du nouveau formulaire de création d'une tâche de migration.")
    :::column-end:::
    :::column:::
        **Nom de la définition de travail**</br>Ce nom doit indiquer l’ensemble des fichiers que vous déplacez. Donnez-lui un nom similaire à celui de votre partage de fichiers Azure. </br></br>**Emplacement d’exécution de la tâche**</br>Lorsque vous sélectionnez une région, vous devez sélectionner celle de votre compte de stockage StorSimple ou, si cela n’est pas possible, une région proche de celui-ci. </br></br><h3>Source</h3>**Abonnement source**</br>Sélectionnez l’abonnement dans lequel vous stockez votre ressource StorSimple Device Manager. </br></br>**Ressource StorSimple**</br>Sélectionnez la ressource StorSimple Device Manager auprès de laquelle votre appliance est inscrite. </br></br>**Clé de chiffrement de données du service**</br>Consultez la [section précédente de cet article](#storsimple-service-data-encryption-key) si vous ne trouvez pas la clé dans vos dossiers. </br></br>**Appareil**</br>Sélectionnez l’appareil StorSimple contenant le volume vers lequel vous souhaitez effectuer la migration. </br></br>**Volume**</br>Sélectionnez le volume source. Plus tard, vous déciderez si vous souhaitez migrer l’ensemble du volume ou des sous-répertoires vers le partage de fichiers Azure cible.</br></br> **Sauvegardes de volume**</br>Vous pouvez sélectionner *Sélectionner des sauvegardes de volume* pour choisir les sauvegardes à déplacer dans le cadre de cette tâche. [Une section dédiée de cet article](#selecting-volume-backups-to-migrate) sera prochainement disponible. Celle-ci couvrira le processus en détail.</br></br><h3>Cible</h3>Sélectionnez l’abonnement, le compte de stockage et le partage de fichiers Azure comme cible de cette tâche de migration.</br></br><h3>Mappage de répertoires</h3>[Une section dédiée de cet article](#directory-mapping) traite de tous les détails pertinents.
    :::column-end:::
:::row-end:::

### <a name="selecting-volume-backups-to-migrate"></a>Sélectionner les sauvegardes de volume à migrer

Le choix des sauvegardes à migrer repose sur différents aspects importants :

- Vos tâches de migration ne peuvent déplacer que des sauvegardes, pas les données d'un volume actif. La sauvegarde la plus récente est donc la plus proche des données actives et doit toujours figurer dans la liste des sauvegardes déplacées lors d'une migration.
- Vérifiez que votre dernière sauvegarde est récente afin de réduire au maximum le delta sur le partage actif. Il peut être intéressant de procéder manuellement au déclenchement et à la sauvegarde d'un autre volume avant de créer une tâche de migration. Un delta réduit sur le partage actif améliorera votre expérience de migration. Si ce delta peut être égal à zéro = aucune autre modification du volume StorSimple n'a eu lieu après la dernière sauvegarde de votre liste - alors en phase 5 : Le basculement des utilisateurs sera considérablement simplifié et accéléré.
- Sur le partage de fichiers Azure, les sauvegardes doivent être lues dans l'ordre suivant : **de la plus ancienne à la plus récente**. Une sauvegarde ancienne ne peut pas être « triée » dans la liste des sauvegardes du partage de fichiers Azure après l'exécution d'une tâche de migration. Par conséquent, vous devez vous assurer que votre liste de sauvegardes est terminée *avant* de créer une tâche. 
- Cette liste de sauvegardes ne peut pas être modifiée une fois la tâche créée, même si celle-ci n'a jamais été exécutée. 

:::row:::
    :::column:::        
        :::image type="content" source="media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-job-select-backups.png" alt-text="Capture d'écran du nouveau formulaire de création de tâche détaillant la partie où les sauvegardes StorSimple sont sélectionnées pour la migration." lightbox="media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-job-select-backups-expanded.png":::
    :::column-end:::
    :::column:::
        Pour sélectionner des sauvegardes de votre volume StorSimple dans le cadre de votre tâche de migration, sélectionnez le bouton *Sélectionner des sauvegardes de volume* sur le formulaire de création de tâche.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        :::image type="content" source="media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-job-select-backups-annotated.png" alt-text="Image montrant que la moitié supérieure du panneau de sélection des sauvegardes répertorie toutes les sauvegardes disponibles. Une sauvegarde sélectionnée sera grisée dans cette liste et ajoutée à une deuxième liste dans la moitié inférieure du panneau. Là, elle pourra également être de nouveau supprimée." lightbox="media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-job-select-backups-annotated.png":::
    :::column-end:::
    :::column:::
        Lorsque le panneau de sélection des sauvegardes s'ouvre, il est divisé en deux listes. La première liste répertorie toutes les sauvegardes disponibles. Vous pouvez développer et réduire le jeu de résultats en filtrant la liste selon un intervalle de temps spécifique. (voir la section suivante) </br></br>Une sauvegarde sélectionnée sera grisée et ajoutée à une deuxième liste dans la moitié inférieure du panneau. La deuxième liste répertorie toutes les sauvegardes sélectionnées pour la migration. Une sauvegarde sélectionnée par erreur peut également être retirée.
        > [!CAUTION]
        > Vous devez sélectionner **toutes** les sauvegardes que vous souhaitez migrer. Il sera ensuite impossible d'ajouter des sauvegardes plus anciennes. Une fois la tâche créée, vous ne pouvez pas la modifier pour changer votre sélection.
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        :::image type="content" source="media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-job-select-backups-time.png" alt-text="Capture d'écran illustrant la sélection d'un intervalle de temps dans le panneau de sélection des sauvegardes." lightbox="media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-job-select-backups-time-expanded.png":::
    :::column-end:::
    :::column:::
        Par défaut, la liste est filtrée pour afficher les sauvegardes de volume StorSimple datant des sept derniers jours afin de faciliter la sélection de la sauvegarde la plus récente. Pour les sauvegardes plus anciennes, utilisez le filtre d'intervalle de temps disponible en haut du panneau. Vous pouvez effectuer une sélection à partir d'un filtre existant ou définir un intervalle de temps personnalisé pour filtrer uniquement les sauvegardes effectuées au cours de cette période.
    :::column-end:::
:::row-end:::

> [!CAUTION]
> Il est impossible de sélectionner plus de 50 sauvegardes de volume StorSimple. Les tâches comportant un grand nombre de sauvegardes peuvent échouer.

### <a name="directory-mapping"></a>Mappage de répertoires

Le mappage de répertoires est facultatif pour votre tâche de migration. Si vous laissez la section vide, *tous* les fichiers et dossiers à la racine de votre volume StorSimple seront déplacés vers la racine de votre partage de fichiers Azure cible. Dans la plupart des cas, le stockage du contenu d’un volume entier dans un partage de fichiers Azure n’est pas la meilleure approche. Il est souvent préférable de fractionner le contenu d’un volume sur plusieurs partages de fichiers dans Azure. Si vous n’avez pas encore créé de plan, consultez d’abord [Mapper votre volume StorSimple à des partages de fichiers Azure](#map-your-existing-storsimple-volumes-to-azure-file-shares).

Dans le cadre de votre plan de migration, vous avez peut-être décidé que les dossiers présents sur un volume StorSimple doivent être répartis entre plusieurs partages de fichiers Azure. Si tel est le cas, vous pouvez les répartir en :

1. Définissant plusieurs tâches afin d’effectuer la migration des dossiers d’un volume. Chacune aura la même source de volume StorSimple, mais un partage de fichiers Azure différent comme cible.
1. Spécifiant précisément les dossiers du volume StorSimple qui doivent être déplacés vers le partage de fichiers spécifié, à l’aide de la section **Mappage des répertoires** du formulaire de création de tâche, et en respectant la [sémantique de mappage](#semantic-elements).

> [!IMPORTANT]
> Les chemins et les expressions de mappage de ce formulaire ne peuvent pas être validés lorsque le formulaire est envoyé. Si les mappages ne sont pas spécifiés correctement, une tâche peut échouer intégralement ou produire un résultat indésirable. Dans ce cas, il est généralement préférable de supprimer le partage de fichiers Azure, de le recréer, puis de corriger les instructions de mappage dans une nouvelle tâche de migration pour le partage. L’exécution d’une nouvelle tâche avec des instructions de mappage corrigées peut résoudre les dossiers omis et les placer dans le partage existant. Toutefois, seuls les dossiers qui ont été omis en raison d’un chemin d’accès mal orthographié peuvent être résolus de cette façon.

#### <a name="semantic-elements"></a>Éléments sémantiques

Un mappage est exprimé de gauche à droite : [\source path] \> [\target path].

|Caractère sémantique          | Signification  |
|:---------------------------|:---------|
| **\\**                     | Indicateur de niveau racine.       |
| **\>**                     | Opérateur de mappage [source] et [cible].     |
|**\|** ou RETOUR (nouvelle ligne) | Séparateur de deux instructions de mappage de dossiers. </br>Vous pouvez également omettre ce caractère et sélectionner **Entrée** pour afficher l’expression de mappage suivante sur sa propre ligne.        |

### <a name="examples"></a>Exemples
Déplace le contenu du dossier *Données utilisateur* à la racine du partage de fichiers cible :
``` console
\User data > \
```
Déplace l’intégralité du contenu du volume vers un nouveau chemin d’accès sur le partage de fichiers cible :
``` console
\ > \Apps\HR tracker
```
Déplace le contenu du dossier source vers un nouveau chemin sur le partage de fichiers cible :
``` console
\HR resumes-Backup > \Backups\HR\resumes
```
Trie plusieurs emplacements sources dans une nouvelle structure de répertoires :
``` console
\HR\Candidate Tracker\v1.0 > \Apps\Candidate tracker
\HR\Candidates\Resumes > \HR\Candidates\New
\Archive\HR\Old Resumes > \HR\Candidates\Archived
```

### <a name="semantic-rules"></a>Règles sémantiques

* Spécifiez toujours des chemins de dossier relatifs au niveau racine.
* Commencez chaque chemin de dossier par l’indicateur de niveau racine « \\ ».
* N’incluez pas la lettre du lecteur.
* Si vous spécifiez plusieurs chemins, notez que les chemins source ou cible ne peuvent pas se chevaucher :</br>
   Exemple de chevauchement du chemin d’accès source non valide :</br>
    *\\dossier\1 > \\dossier*</br>
    *\\dossier\\1\\2 > \\dossier2*</br>
   Exemple de chevauchement du chemin d’accès cible non valide :</br>
   *\\dossier > \\*</br>
   *\\dossier2 > \\*</br>
* Les dossiers sources qui n’existent pas seront ignorés.
* Les structures de dossiers qui n’existent pas sur la cible seront créées.
* Tout comme Windows, les noms de dossiers ne respectent pas la casse, mais la conservent.

> [!NOTE]
> Le contenu du dossier *\System Volume Information* et de *$Recycle.Bin* sur votre volume StorSimple ne sera pas copié par la tâche de migration.

### <a name="run-a-migration-job"></a>Exécuter une tâche de migration

Vos tâches de migration sont répertoriées sous *Définitions des tâches* dans la ressource Data Manager que vous avez déployée dans un groupe de ressources.
Dans la liste des définitions de tâches, sélectionnez la tâche que vous souhaitez exécuter.

Dans le panneau qui s'ouvre, vous pouvez voir que votre tâche est exécutée dans la liste inférieure. Initialement, cette liste est vide. En haut du panneau se trouve une commande appelée *Exécuter la tâche*. Cette commande n'exécute pas immédiatement la tâche, elle ouvre le panneau **Exécution de la tâche** :

:::row:::
    :::column:::
        :::image type="content" source="media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-run-job.png" alt-text="Image du panneau d'exécution de la tâche dans lequel une liste déroulante est ouverte, avec la sélection des sauvegardes à migrer. La sauvegarde la plus ancienne est en surbrillance. Elle doit être sélectionnée en premier." lightbox="media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-run-job-expanded.png":::
    :::column-end:::
    :::column:::
        Dans cette version, chaque tâche doit être exécutée plusieurs fois. </br></br>**Vous devez commencer par la sauvegarde la plus ancienne de la liste des sauvegardes que vous souhaitez migrer.** (mis en surbrillance dans l'image)</br></br>Vous ré-exécutez la tâche, autant de fois que vous avez de sauvegardes sélectionnées, chaque fois sur une sauvegarde plus récente.
        </br></br>
        > [!CAUTION]
        > Il est impératif d'exécuter la tâche de migration en commençant par la sauvegarde la plus ancienne sélectionnée, puis en passant à une sauvegarde plus récente. Vous devez toujours maintenir l'ordre de vos sauvegardes manuellement, de la plus ancienne à la plus récente.
    :::column-end:::
:::row-end:::

### <a name="phase-3-summary"></a>Récapitulatif de la phase 3

À la fin de la phase 3, vous aurez exécuté au moins une de vos tâches de migration entre les volumes StorSimple et le(s) partage(s) de fichiers Azure. Vous aurez exécuté la même tâche de migration plusieurs fois, des sauvegardes les plus anciennes aux plus récentes qui doivent être migrées. À présent, vous pouvez soit configurer Azure File Sync pour le partage (une fois que les tâches de migration d'un partage seront terminées), soit diriger l'accès de vos applications et de vos travailleurs de l'information vers le partage de fichiers Azure.

## <a name="phase-4-access-your-azure-file-shares"></a>Phase 4 : Accéder aux partages de fichiers Azure

Il existe deux stratégies principales pour accéder à vos partages de fichiers Azure :

* **Azure File Sync** : [Déployez Azure File Sync](#deploy-azure-file-sync) sur une instance locale de Windows Server. Azure File Sync offre tous les avantages d’un cache local, tout comme StorSimple.
* **Accès direct au partage** : [Déployez l’accès direct au partage](#deploy-direct-share-access). Utilisez cette stratégie si votre scénario d’accès pour un partage de fichiers Azure donné ne tirera pas parti de la mise en cache locale, ou si vous n’avez plus la possibilité d’héberger une instance locale de Windows Server. Ici, vos utilisateurs et vos applications continuent d’accéder aux partages SMB via le protocole SMB. Ces partages ne se trouvent plus sur un serveur local, mais directement dans le cloud.

Vous devez déjà avoir choisi l’option qui vous convient le mieux dans la [phase 1](#phase-1-prepare-for-migration) de ce guide.

Le reste de cette section se concentre sur les instructions de déploiement.

### <a name="deploy-azure-file-sync"></a>Déployer Azure File Sync

Il est temps de déployer une partie d’Azure File Sync.

1. Créez la ressource cloud Azure File Sync.
1. Déployez l’agent Azure File Sync sur votre serveur local.
1. Inscrivez le serveur auprès de la ressource cloud.

Ne créez pas encore de groupes de synchronisation. La configuration de la synchronisation avec un partage de fichiers Azure doit se produire uniquement lorsque vos tâches de migration vers un partage de fichiers Azure sont terminées. Si vous avez commencé à utiliser Azure File Sync avant la fin de votre migration, cela rendra votre migration inutilement difficile, car vous ne pourrez pas facilement dire quand il sera temps de lancer une opération de basculement.

#### <a name="deploy-the-azure-file-sync-cloud-resource"></a>Déployer la ressource cloud Azure File Sync

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

> [!TIP]
> Si vous souhaitez modifier la région Azure dans laquelle vos données résident une fois que la migration est terminée, déployez le service de synchronisation de stockage dans la même région que les comptes de stockage cibles pour cette migration.

#### <a name="deploy-an-on-premises-windows-server-instance"></a>Déployer une instance locale de Windows Server

* Créez une instance Windows Server 2019 (au minimum 2012 R2) en tant que machine virtuelle ou serveur physique. Un cluster de basculement Windows Server est également pris en charge. Ne réutilisez pas le serveur qui précède l’appliance StorSimple 8100 ou 8600.
* Provisionnez ou ajoutez un stockage en attachement direct. Le stockage attaché au réseau n’est pas pris en charge.

La meilleure méthode consiste à attribuer à votre nouvelle instance Windows Server une quantité de stockage égale ou supérieure à celle dont votre appliance StorSimple 8100 ou 8600 dispose localement pour la mise en cache. Vous utiliserez l’instance Windows Server de la même façon que vous avez utilisé l’appliance StorSimple. Si elle dispose de la même quantité de stockage que l’appliance, l’expérience de mise en cache sera similaire, voire identique. Vous pouvez ajouter ou supprimer autant d’espace de stockage que vous le souhaitez sur l’instance Windows Server. Cela vous permet de mettre à l’échelle la taille de votre volume local, ainsi que la quantité de stockage local disponible pour la mise en cache.

#### <a name="prepare-the-windows-server-instance-for-file-sync"></a>Préparer l’instance Windows Server pour la synchronisation de fichiers

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

#### <a name="configure-azure-file-sync-on-the-windows-server-instance"></a>Configurer Azure File Sync sur l’instance Windows Server

Votre instance Windows Server locale inscrite doit être prête et connectée à Internet pour ce processus.

> [!IMPORTANT]
> Votre migration StorSimple de fichiers et de dossiers vers le partage de fichiers Azure doit être terminée avant de continuer. Assurez-vous qu’aucune autre modification n’est effectuée sur le partage de fichiers.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!IMPORTANT]
> Veillez à activer la hiérarchisation cloud. La hiérarchisation cloud désigne la fonctionnalité Azure File Sync qui permet au serveur local d’avoir moins de capacité de stockage que ce qui est stocké dans le cloud, tout en ayant l’espace de noms complet disponible. Les données intéressantes localement sont également mises en cache localement pour des performances d’accès local rapides. Une autre raison d’activer la hiérarchisation cloud à cette étape est que nous ne souhaitons pas synchroniser le contenu des fichiers à ce stade, mais seulement l’espace de noms.

### <a name="deploy-direct-share-access"></a>Déployer l’accès direct au partage

:::row:::
    :::column:::
        <iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/jd49W33DxkQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    :::column-end:::
    :::column:::
        Cette vidéo montre comment exposer directement et de façon sécurisée les partages de fichiers Azure aux travailleurs de l’information et aux applications, en cinq étapes simples.</br>
        La vidéo fait référence à une documentation dédiée pour certains sujets :

* [Vue d’ensemble de l’identité](storage-files-active-directory-overview.md)
* [Comment joindre un domaine à un compte de stockage](storage-files-identity-auth-active-directory-enable.md)
* [Présentation de la mise en réseau pour les partages de fichiers Azure](storage-files-networking-overview.md)
* [Comment configurer des points de terminaison publics et privés](storage-files-networking-endpoints.md)
* [Comment configurer un VPN site à site](storage-files-configure-s2s-vpn.md)
* [Comment configurer un VPN point à site Windows](storage-files-configure-p2s-vpn-windows.md)
* [Comment configurer un VPN point à site Linux](storage-files-configure-p2s-vpn-linux.md)
* [Procédure de configuration du transfert DNS](storage-files-networking-dns.md)
* [Configurer DFS-N](/windows-server/storage/dfs-namespaces/dfs-overview)
   :::column-end:::
:::row-end:::

### <a name="phase-4-summary"></a>Récapitulatif de la phase 4

Au cours de cette phase, vous avez créé et exécuté plusieurs tâches de migration dans StorSimple Data Manager. Ces tâches ont migré vos fichiers et dossiers vers des partages de fichiers Azure. Vous avez également déployé Azure File Sync ou préparé votre réseau et vos comptes de stockage pour un accès direct au partage.

## <a name="phase-5-user-cut-over"></a>Phase 5 : Transfert de l’utilisateur

Cette phase est consacrée à la finalisation de votre migration :

* Planifiez votre temps d’arrêt.
* Découvrez les modifications qui ont été apportées par vos utilisateurs et vos applications côté StorSimple pendant que les tâches de migration de la phase 3 étaient en cours d'exécution.
* Faites basculer vos utilisateurs vers la nouvelle instance Windows Server avec Azure File Sync ou vers les partages de fichiers Azure via un accès direct au partage.

### <a name="plan-your-downtime"></a>Planifier votre temps d’arrêt

Cette approche de migration nécessite un temps d’arrêt pour vos utilisateurs et vos applications. L’objectif est de réduire au maximum le temps d’arrêt. Voici certains points qui pourront vous aider :

* Gardez vos volumes StorSimple disponibles pendant l'exécution de vos tâches de migration.
* Lorsque vous avez terminé d’exécuter vos tâches de migration des données pour un partage, vous devez supprimer l’accès utilisateur (au moins l’accès en écriture) des volumes ou partages StorSimple. Une RoboCopy finale récupèrera les modifications de votre partage de fichiers Azure. Vous pourrez alors faire basculer vos utilisateurs. L’emplacement où vous exécutez RoboCopy varie selon que vous avez choisi d’utiliser Azure File Sync ou l’accès direct au partage. La prochaine section sur RoboCopy aborde ce sujet.
* Une fois que vous avez terminé la récupération RoboCopy, vous êtes prêt à exposer le nouvel emplacement à vos utilisateurs, soit directement par le biais du partage de fichiers Azure, soit par le biais d’un partage SMB présent sur une instance Windows Server avec Azure File Sync. Souvent, un déploiement DFS-N permet d’effectuer une opération de basculement rapide et efficace. Il conserve la cohérence de vos adresses de partage existantes et les redirigent vers un nouvel emplacement où se trouvent vos fichiers et vos dossiers déplacés.

### <a name="determine-when-your-namespace-has-fully-synced-to-your-server"></a>Déterminer le moment où votre espace de noms est entièrement synchronisé à votre serveur

Lorsque vous utilisez Azure File Sync pour un partage de fichiers Azure, il est important d’attendre que l’espace de noms entier ait été entièrement téléchargé sur le serveur *avant* de démarrer une RoboCopy locale. Le temps nécessaire au téléchargement de votre espace de noms dépend du nombre d’éléments dans votre partage de fichiers Azure. Il existe deux méthodes permettant de déterminer si votre espace de noms a été entièrement téléchargé sur le serveur.

#### <a name="azure-portal"></a>Portail Azure

Vous pouvez utiliser le portail Azure pour voir à quel moment votre espace de noms est entièrement arrivé.

* Connectez-vous au portail Azure et accédez à votre groupe de synchronisation. Vérifiez l’état de synchronisation de votre groupe de synchronisation et de votre point de terminaison de serveur.
* Le sens le plus important est celui du téléchargement. Si le point de terminaison de serveur vient d’être provisionné, il affiche **Synchronisation initiale**, ce qui indique que l’espace de noms est toujours en cours de téléchargement.
Lorsqu’il n’affiche plus **Synchronisation initiale**, cela signifie que votre espace de noms a été entièrement copié sur le serveur. Vous pouvez maintenant procéder à une copie RoboCopy locale.

#### <a name="windows-server-event-viewer"></a>Observateur d’événements Windows Server

Vous pouvez également utiliser l’observateur d’événements sur l’instance Windows Server pour savoir quand l’espace de noms est entièrement téléchargé.

1. Ouvrez l’**observateur d’événements** et accédez à **Applications et services**.
1. Ouvrez **Microsoft\FileSync\Agent\Telemetry**.
1. Recherchez l’**événement 9102** le plus récent, qui correspond à une session de synchronisation terminée.
1. Sélectionnez **Détails** et vérifiez que vous consultez bien un événement dans lequel la valeur **SyncDirection** est **Télécharger**.
1. Lorsque votre espace de noms aura été téléchargé sur le serveur, il y aura un événement unique avec **Scenario**, la valeur **FullGhostedSync** et **HResult** = **0**.
1. S’il vous manque cet événement, vous pouvez également rechercher d’autres **événements 9102** où **SyncDirection** = **Télécharger** et **Scenario** =  **« RegularSync »** . Le fait de trouver l’un de ces événements indique aussi que l’espace de noms a terminé son téléchargement et que la synchronisation est passée à des sessions de synchronisation régulières, qu’il y ait quelque chose à synchroniser ou non, pour le moment.

### <a name="a-final-robocopy"></a>Une RoboCopy finale

À ce stade, il existe des différences entre votre instance locale de Windows Server et l’appliance StorSimple 8100 ou 8600.

1. Vous devez récupérer les modifications apportées par les utilisateurs ou les applications côté StorSimple pendant la migration.
1. Pour les cas où vous utilisez Azure File Sync : L’appliance StorSimple dispose d’un cache rempli, alors que l’instance Windows Server n’a qu’un espace de noms sans contenu de fichier stocké localement pour l’instant. Par conséquent, la RoboCopy finale peut vous aider à démarrer votre cache Azure File Sync local en récupérant le contenu du fichier mis en cache localement dans la mesure où il est disponible et peut tenir sur le serveur Azure File Sync.
1. Certains fichiers peuvent avoir été ignorés par la tâche de migration en raison de caractères non valides. Si c’est le cas, copiez-les sur l’instance Windows Server avec Azure File Sync. Plus tard, vous pourrez les ajuster pour qu’ils se synchronisent. Si vous n’utilisez pas Azure File Sync pour un partage en particulier, il est préférable de renommer les fichiers avec des caractères non valides sur le volume StorSimple. Ensuite, vous pourrez exécuter la RoboCopy directement sur le partage de fichiers Azure.

> [!WARNING]
> Robocopy dans Windows Server 2019 rencontre actuellement un problème qui entraîne la recopie des fichiers hiérarchisés par Azure File Sync sur le serveur cible à partir de la source, puis le rechargement vers Azure lors de l’utilisation de la fonction /MIR de Robocopy. Il est impératif d’utiliser Robocopy sur un serveur Windows Server autre que 2019. Le choix par défaut est Windows Server 2016. Cette note sera mise à jour en cas de résolution du problème via Windows Update.

> [!WARNING]
> Vous *ne devez pas* démarrer la RoboCopy avant que le serveur ait téléchargé complètement l’espace de noms pour un partage de fichiers Azure. Pour plus d’informations, consultez [Déterminer le moment où votre espace de noms est entièrement synchronisé à votre serveur](#determine-when-your-namespace-has-fully-synced-to-your-server).

 Vous souhaitez uniquement copier les fichiers qui ont été modifiés après la dernière exécution de la tâche de migration et les fichiers qui n’ont pas été déplacés par lesdites tâches auparavant. Vous pouvez comprendre pourquoi ils n’ont pas été déplacés plus tard sur le serveur, une fois la migration terminée. Pour plus d’informations, consultez [Résolution des problèmes Azure File Sync](storage-sync-files-troubleshoot.md#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing).

RoboCopy a plusieurs paramètres. L’exemple suivant montre une commande terminée, ainsi que la liste des raisons pour lesquelles vous devez choisir ces paramètres.

```console
Robocopy /MT:16 /UNILOG:<file name> /TEE /NP /B /MIR /IT /COPYALL /DCOPY:DAT <SourcePath> <Dest.Path>
```

Arrière-plan :

:::row:::
   :::column span="1":::
      /MT
   :::column-end:::
   :::column span="1":::
      Permet à RoboCopy de s’exécuter en multithread. La valeur par défaut est 8 et la valeur maximale est 128.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /UNILOG:<file name>
   :::column-end:::
   :::column span="1":::
      Renvoie l’état au fichier LOG au format UNICODE (remplace le journal existant).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /TEE
   :::column-end:::
   :::column span="1":::
      Génère les sorties dans la fenêtre de la console. Utilisé conjointement avec la sortie dans un fichier journal.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /NP
   :::column-end:::
   :::column span="1":::
      Omet la journalisation de la progression pour garder le journal lisible.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /B
   :::column-end:::
   :::column span="1":::
      Exécute RoboCopy dans le même mode qu’une application de sauvegarde. Cela permet à RoboCopy de déplacer des fichiers pour lesquels l’utilisateur actuel n’a pas d’autorisations.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /MIR
   :::column-end:::
   :::column span="1":::
      Permet à RoboCopy de prendre uniquement en compte les écarts entre la source (appliance StorSimple) et la cible (répertoire Windows Server).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /IT
   :::column-end:::
   :::column span="1":::
      Garantit la fidélité dans certains scénarios de mise en miroir.</br>Exemple : Entre deux exécutions Robocopy, un fichier fait l’objet d’une modification de liste de contrôle d’accès et d’une mise à jour d’attribut, par exemple, il est également marqué *masqué*. Sans /IT, la modification de la liste de contrôle d’accès peut être ignorée par Robocopy et donc pas transférée vers l’emplacement cible.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPY:copyflag[s]
   :::column-end:::
   :::column span="1":::
      Fidélité de la copie de fichier (la valeur par défaut est /COPY:DAT), indicateurs de copie : D=Données, A=Attributs, T=Horodatages, S=Sécurité=ACL NTFS, O=Informations propriétaire, U=Informations audit.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPYALL
   :::column-end:::
   :::column span="1":::
      Informations de fichier COPY ALL (équivalent de /COPY:DATSOU).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /DCOPY:copyflag[s]
   :::column-end:::
   :::column span="1":::
      Fidélité de la copie des répertoires (la valeur par défaut est /DCOPY:DA), indicateurs de copie : D=Données, A=Attributs, T=Horodatages.
   :::column-end:::
:::row-end:::

Quand vous configurez les emplacements source et cible de la commande RoboCopy, veillez à examiner la structure de la source et de la cible pour être sûr qu’elles correspondent. Si vous avez utilisé la fonctionnalité de mappage de répertoires de la tâche de migration, la structure de votre répertoire racine peut être différente de celle de votre volume StorSimple. Si c’est le cas, vous pouvez avoir besoin de plusieurs tâches RoboCopy, une pour chaque sous-répertoire. Si vous ne savez pas si la commande s’exécute comme prévu, vous pouvez utiliser le paramètre */L*, qui simulera la commande sans apporter de modification.

Cette commande RoboCopy utilise /MIR. Elle ne déplace donc pas les fichiers qui sont identiques (fichiers hiérarchisés, par exemple). Toutefois, si le chemin source et le chemin cible sont incorrects, /MIR vide également les structures de répertoires qui sont présentes sur votre instance Windows Server ou sur le partage de fichiers Azure, mais pas sur le chemin source StorSimple. Elles doivent donc correspondre exactement pour que la tâche RoboCopy atteigne son objectif, à savoir la mise à jour du contenu déplacé avec les dernières modifications apportées pendant la migration.

Consultez le fichier journal RoboCopy pour voir si des fichiers ont été ignorés. Si des problèmes existent, corrigez-les, puis réexécutez la commande RoboCopy. Ne déprovisionnez pas les ressources StorSimple avant d’avoir corrigé les problèmes en suspens pour les fichiers ou dossiers qui vous intéressent.

Si vous n’utilisez pas Azure File Sync pour mettre en cache le partage de fichiers Azure en question, mais que vous avez plutôt opté pour un accès direct au partage :

1. [Montez votre partage de fichiers Azure](storage-how-to-use-files-windows.md#mount-the-azure-file-share) en tant que lecteur réseau sur un ordinateur Windows local.
1. Exécutez RoboCopy entre votre volume StorSimple et le partage de fichiers Azure monté. Si les fichiers ne sont pas copiés, corrigez leurs noms côté StorSimple pour supprimer les caractères non valides. Ensuite, réessayez RoboCopy. La commande RoboCopy mentionnée précédemment peut être exécutée plusieurs fois sans provoquer de rappel inutile vers StorSimple.

### <a name="user-cut-over"></a>Transfert de l’utilisateur

Si vous utilisez Azure File Sync, vous devrez probablement créer des partages SMB sur cette instance Windows Server avec Azure File Sync correspondant aux partages que vous aviez sur les volumes StorSimple. Vous pouvez commencer par cette étape, et le plus tôt possible pour ne pas perdre de temps. Toutefois, vous devez garantir que personne ne pourra apporter des modifications à l’instance Windows Server avant la fin de cette étape.

Si vous avez un déploiement DFS-N, vous pouvez faire pointer les espaces de noms DFN vers les emplacements des nouveaux dossiers du serveur. Si vous ne disposez pas d’un déploiement DFS-N et que vous avez déjà relié votre appliance 8100 ou 8600 localement à une instance Windows Server, vous pouvez retirer ce serveur du domaine. Ensuite, joignez le domaine à votre nouvelle instance Windows Server avec Azure File Sync. Au cours de ce processus, donnez au serveur le même nom de serveur et les mêmes noms de partage que l’ancien serveur, afin que le basculement reste transparent pour vos utilisateurs, votre stratégie de groupe et vos scripts.

En savoir plus sur [DFS-N](/windows-server/storage/dfs-namespaces/dfs-overview).

## <a name="deprovision"></a>annulation du déploiement

Lorsque vous déprovisionnez une ressource, vous perdez l’accès à la configuration de cette ressource, ainsi qu’à ses données. Le déprovisionnement ne peut pas être annulé. Ne poursuivez pas tant que vous n’avez pas vérifié les points suivants :

* Votre migration est terminée.
* Il n’existe aucune dépendance vis-à-vis des fichiers, dossiers ou sauvegardes de volume StorSimple que vous allez déprovisionner.

Avant de commencer, il est recommandé d’observer quelque temps votre nouveau déploiement Azure File Sync en production. Cette période d’observation vous donne l’occasion de corriger tout problème que vous pourriez rencontrer. Une fois que vous avez observé votre déploiement Azure File Sync pendant au moins quelques jours, vous pouvez commencer à déprovisionner les ressources dans cet ordre :

1. Déprovisionnez votre ressource StorSimple Data Manager via le portail Azure. Toutes vos tâches DTS seront supprimées avec elle. Vous ne pourrez pas récupérer facilement les journaux de copie. S’ils sont importants pour vos dossiers, récupérez-les avant de déprovisionner la ressource.
1. Vérifiez que vos appliances physiques StorSimple ont été déplacées, puis désinscrivez-les. Si vous n’êtes pas tout à fait sûr qu’elles ont été déplacées, ne poursuivez pas. Si vous déprovisionnez ces ressources alors qu’elles sont encore nécessaires, vous ne pourrez pas récupérer les données ni leur configuration.<br>Si vous le souhaitez, vous pouvez d’abord déprovisionner la ressource de volume StorSimple, ce qui nettoiera les données de l’appliance. Ce processus peut prendre plusieurs jours et **n’aura pas comme effet** de supprimer l’intégralité des données de l’appliance. Si cela est important pour vous, supprimez l’intégralité des données d’un disque dans un cadre autre que celui du déprovisionnement des ressources, et conformément à vos stratégies.
1. S’il ne reste plus d’appareils inscrits dans une ressource StorSimple Device Manager, vous pouvez supprimer cette dernière.
1. Il est maintenant temps de supprimer le compte de stockage StorSimple dans Azure. Là encore, avant de poursuivre, vérifiez que la migration est terminée, et que rien ni personne ne dépend de ces données.
1. Débranchez l’appliance physique StorSimple de votre centre de données.
1. Si vous êtes propriétaire de l’appliance StorSimple, vous êtes libre de la recycler. Si votre appareil est loué, informez le bailleur et retournez l’appareil comme il se doit.

Votre migration est terminée.

> [!NOTE]
> Vous avez des questions ou rencontrez des problèmes ?</br>
> Nous sommes là pour vous aider : AzureFilesMigration@microsoft.com.

## <a name="next-steps"></a>Étapes suivantes

* Familiarisez-vous avec [Azure File Sync](./storage-sync-files-planning.md).
* Comprenez la flexibilité des stratégies de [hiérarchisation cloud](storage-sync-cloud-tiering-overview.md).
* [Activez Sauvegarde Azure](../../backup/backup-afs.md#configure-backup-from-the-file-share-pane) sur vos partages de fichiers Azure pour planifier des instantanés et définir des planifications de conservation de sauvegardes.
* Si vous constatez dans le portail Azure que certains fichiers ne se synchronisent jamais, consultez le [guide de résolution des problèmes](storage-sync-files-troubleshoot.md) pour connaître les mesures à prendre pour les résoudre.
