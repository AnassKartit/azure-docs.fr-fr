---
title: Tutoriel - Sauvegarder des bases de données SAP HANA dans des machines virtuelles Azure
description: Dans ce tutoriel, découvrez comment sauvegarder des bases de données SAP HANA s’exécutant sur une machine virtuelle Azure dans un coffre Recovery Services de Sauvegarde Azure.
ms.topic: tutorial
ms.date: 09/27/2021
ms.openlocfilehash: 5469c10bb62164e7feea33a1b56cef3457d46efb
ms.sourcegitcommit: 87de14fe9fdee75ea64f30ebb516cf7edad0cf87
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/01/2021
ms.locfileid: "129349679"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm"></a>Tutoriel : Sauvegarder des bases de données SAP HANA dans une machine virtuelle Azure

Ce tutoriel vous explique comment sauvegarder des bases de données SAP HANA s’exécutant sur des machines virtuelles Azure dans un coffre Recovery Services de Sauvegarde Azure. Cet article porte sur les points suivants :

> [!div class="checklist"]
>
> * Créer et configurer un coffre
> * Découvrir des bases de données
> * Configurer des sauvegardes

[Voici](sap-hana-backup-support-matrix.md#scenario-support) tous les scénarios actuellement pris en charge.

## <a name="prerequisites"></a>Prérequis

Avant de configurer les sauvegardes, prenez soin d’effectuer les opérations suivantes :

* Identifiez ou créez un [coffre Recovery Services](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault) dans la même région et avec le même abonnement que la machine virtuelle qui exécute SAP HANA.
* Autorisez la connectivité de la machine virtuelle à Internet pour lui permettre d’atteindre Azure comme décrit dans la procédure [Configurer la connectivité réseau](#set-up-network-connectivity) ci-dessous.
* Vérifiez que la longueur combinée du nom de la machine virtuelle SAP HANA Server et du nom du groupe de ressources ne dépasse pas 84 caractères pour Azure Resource Manager (machines virtuelles ARM_) (et 77 caractères pour les machines virtuelles classiques). Cette limitation est due au fait que certains caractères sont réservés par le service.
* Le **hdbuserstore** doit inclure une clé qui respecte les critères suivants :
  * Elle doit être présente dans le **hdbuserstore** par défaut. Par défaut, il s’agit du compte `<sid>adm` sous lequel SAP HANA est installé.
  * Pour MDC, la clé doit pointer vers le port SQL de **NAMESERVER**. Pour SDC, elle doit pointer vers le port SQL de **INDEXSERVER**.
  * Elle doit disposer des informations d’identification nécessaires pour ajouter et supprimer des utilisateurs.
  * Notez que cette clé peut être supprimée après l’exécution du script de préinscription
* Vous pouvez également choisir de créer une clé pour l’utilisateur HANA SYSTSEM existant dans **hdbuserstore** au lieu de créer une clé personnalisée comme indiqué dans l’étape ci-dessus.
* Exécutez le script de configuration de sauvegarde SAP HANA (script de préinscription) dans la machine virtuelle où HANA est installé en tant qu’utilisateur racine. [Ce script](https://go.microsoft.com/fwlink/?linkid=2173610) prépare le système HANA pour la sauvegarde et nécessite que la clé créée dans les étapes ci-dessus soit passée en entrée. Pour comprendre comment passer cette entrée au script en tant que paramètre, consultez la section [Ce que fait le script de préinscription](#what-the-pre-registration-script-does). Vous verrez également ce que fait le script de préinscription.
* Si votre configuration HANA utilise des points de terminaison privés, exécutez le [script de préinscription](https://go.microsoft.com/fwlink/?linkid=2173610) avec le paramètre *-sn* ou *--skip-network-checks*.

>[!NOTE]
>Le script de préinscription installe **compat-unixODBC234** pour les charges de travail SAP HANA s’exécutant sur RHEL (7.4, 7.6 et 7.7) et **unixODBC** pour RHEL 8.1. [Ce package se trouve dans le dépôt des services de mise à jour de RHEL for SAP HANA (pour RHEL 7 Server) pour les solutions SAP (RPM)](https://access.redhat.com/solutions/5094721).  Pour une image RHEL de la Place de marché Azure, le dépôt est **rhui-rhel-sap-hana-for-rhel-7-server-rhui-e4s-rpms**.

## <a name="set-up-network-connectivity"></a>Configurer la connectivité réseau

Pour toutes les opérations, une base de données SAP HANA s’exécutant sur une machine virtuelle Azure nécessite une connectivité avec le service Sauvegarde Azure, Stockage Azure et Azure Active Directory. Pour ce faire, vous pouvez utiliser des points de terminaison privés ou autoriser l’accès aux IP publiques ou aux noms de domaine complets (FQDN) requis. Le fait de ne pas permettre une connectivité appropriée aux services Azure requis peut entraîner l’échec d’opérations telles que la détection de base de données, la configuration de la sauvegarde, l’exécution de sauvegardes et la restauration de données.

Le tableau suivant répertorie les différentes alternatives que vous pouvez utiliser pour établir la connectivité :

| **Option**                        | **Avantages**                                               | **Inconvénients**                                            |
| --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Instances Private Endpoint                 | Autorisent les sauvegardes sur des adresses IP privées au sein du réseau virtuel  <br><br>   Fournissent un contrôle approfondi du côté du réseau et du coffre | Engendre des [coûts](https://azure.microsoft.com/pricing/details/private-link/) de point de terminaison privé standard |
| Balises de service NSG                  | Plus faciles à gérer car les modifications apportées à la plage sont fusionnées automatiquement   <br><br>   Aucun coût supplémentaire | Peut être utilisé uniquement avec les groupes de sécurité réseau  <br><br>    Fournit l’accès à l’ensemble du service |
| Balises FQDN de Pare-feu Azure          | Plus faciles à gérer, car les FQDN requis sont gérés automatiquement | Utilisabes avec Pare-feu Azure uniquement                         |
| Autoriser l’accès aux FQDN/adresses IP du service | Aucun coût supplémentaire   <br><br>  Fonctionne avec toutes les appliances de sécurité réseau et tous les pare-feu | Il peut être nécessaire d’accéder à un large éventail d’adresses IP ou de FQDN   |
| Utiliser un proxy HTTP                 | Un seul point d’accès Internet aux machines virtuelles                       | Frais supplémentaires d’exécution de machine virtuelle avec le logiciel de serveur proxy         |
| [Point de terminaison de service de réseau virtuel](/azure/virtual-network/virtual-network-service-endpoints-overview)    |     Peut être utilisé pour le stockage Azure (= coffre Recovery Services).     <br><br>     Offre un avantage important pour optimiser les performances du trafic du plan de données.          |         Ne peut pas être utilisé pour Azure AD, le service de sauvegarde Azure.    |
| Appliance virtuelle réseau      |      Peut être utilisé pour le stockage Azure, Azure AD, le service de sauvegarde Azure. <br><br> **Plan de données**   <ul><li>      Stockage Azure : `*.blob.core.windows.net`, `*.queue.core.windows.net`  </li></ul>   <br><br>     **Plan de gestion**  <ul><li>  Azure AD : autorisez l’accès aux noms de domaine complets mentionnés dans les sections 56 et 59 de [Services communs Microsoft 365 et Office Online](/microsoft-365/enterprise/urls-and-ip-address-ranges?view=o365-worldwide&preserve-view=true#microsoft-365-common-and-office-online). </li><li>   Service de sauvegarde Azure : `.backup.windowsazure.com` </li></ul> <br>Apprenez-en davantage sur les [Étiquettes de service du Pare-feu Azure](/azure/firewall/fqdn-tags#:~:text=An%20FQDN%20tag%20represents%20a%20group%20of%20fully,the%20required%20outbound%20network%20traffic%20through%20your%20firewall.).       |  Ajoute une surcharge au trafic du plan de données et réduit le débit/les performances.  |

De plus amples informations sur l’utilisation de ces options sont disponibles ci-dessous :

### <a name="private-endpoints"></a>Instances Private Endpoint

Les points de terminaison privés vous permettent de vous connecter en toute sécurité à votre coffre Recovery Services à partir de serveurs situés dans un réseau virtuel. Le point de terminaison privé utilise une adresse IP privée de l’espace d’adressage du réseau virtuel pour votre coffre. Le trafic réseau entre vos ressources dans le réseau virtuel et le coffre transite via votre réseau virtuel et une liaison privée sur le réseau principal de Microsoft. Cela élimine l’exposition de l’Internet public. Un point de terminaison privé est attribué à un sous-réseau spécifique d’un réseau virtuel et ne peut pas être utilisé pour Azure Active Directory. Pour en savoir plus sur les points de terminaison privés pour Sauvegarde Azure, cliquez [ici](./private-endpoints.md).

### <a name="nsg-tags"></a>Balises NSG

Si vous utilisez des groupes de sécurité réseau (NSG), utilisez la balise de service *AzureBackup* pour autoriser l’accès sortant vers Sauvegarde Azure. En plus de l’étiquette pour Sauvegarde Azure, vous devez également autoriser la connectivité pour l’authentification et le transfert de données en créant des [règles NSG](../virtual-network/network-security-groups-overview.md#service-tags) similaires pour Azure AD (*AzureActiveDirectory*) et Stockage Azure (*Storage*). Les étapes suivantes décrivent le processus de création d’une règle pour la balise de Sauvegarde Azure :

1. Dans **Tous les services**, accédez à **Groupes de sécurité réseau** et sélectionnez le groupe de sécurité réseau.

1. Sous **PARAMÈTRES**, sélectionnez **Règles de sécurité de trafic sortant**.

1. Sélectionnez **Ajouter**. Entrez toutes les informations nécessaires à la création d’une nouvelle règle, comme décrit dans [paramètres de règle de sécurité](../virtual-network/manage-network-security-group.md#security-rule-settings). Vérifiez que l’option **Destination** est définie sur *Balise de service* et l’option **Balise de service de destination** sur *AzureBackup*.

1. Sélectionnez **Ajouter** pour enregistrer la règle de sécurité de trafic sortant que vous venez de créer.

De même, vous pouvez créer des [règles de sécurité de trafic sortant NSG](../virtual-network/network-security-groups-overview.md#service-tags) pour Stockage Azure et Azure AD. Pour plus d’informations sur les balises de service, consultez [cet article](../virtual-network/service-tags-overview.md).

### <a name="azure-firewall-tags"></a>Balises Pare-feu Azure

Si vous utilisez Pare-feu Azure, créez une règle d’application en utilisant la [balise FQDN de Pare-feu Azure](../firewall/fqdn-tags.md) *AzureBackup*. Cela autorise tout accès sortant vers Sauvegarde Azure.

### <a name="allow-access-to-service-ip-ranges"></a>Autoriser l’accès aux plages d’adresses IP du service

Si vous choisissez d’autoriser l’accès aux adresses IP du service, reportez-vous aux plages d’adresses IP répertoriées dans le fichier JSON accessible [ici](https://www.microsoft.com/download/confirmation.aspx?id=56519). Vous devez autoriser l’accès aux adresses IP correspondant à Sauvegarde Azure, Stockage Azure et Azure Active Directory.

### <a name="allow-access-to-service-fqdns"></a>Autoriser l’accès aux FQDN du service

Vous pouvez également utiliser les FQDN suivants pour autoriser l’accès aux services requis à partir de vos serveurs :

| Service    | Noms de domaine auxquels accéder                             |
| -------------- | ------------------------------------------------------------ |
| Sauvegarde Azure  | `*.backup.windowsazure.com`                             |
| Stockage Azure | `*.blob.core.windows.net` <br><br> `*.queue.core.windows.net` |
| Azure AD      | Autoriser l’accès aux FQDN en vertu des sections 56 et 59 conformément à [cet article](/office365/enterprise/urls-and-ip-address-ranges#microsoft-365-common-and-office-online) |

### <a name="use-an-http-proxy-server-to-route-traffic"></a>Utiliser un serveur proxy HTTP pour acheminer le trafic

Lorsque vous sauvegardez une base de données SAP HANA qui s’exécute sur une machine virtuelle Azure, l’extension de sauvegarde sur la machine virtuelle utilise les API HTTPS pour envoyer des commandes de gestion à Sauvegarde Azure et des données à Stockage Azure. L’extension de sauvegarde utilise également Azure AD pour l’authentification. Acheminez le trafic de l’extension de sauvegarde pour ces trois services via le proxy HTTP. Utilisez la liste des adresses IP et des FQDN ci-dessus pour autoriser l’accès aux services requis. Les serveurs proxy authentifiés ne sont pas pris en charge.

## <a name="understanding-backup-and-restore-throughput-performance"></a>Présentation de la performance de débit des sauvegardes et des restaurations

Les sauvegardes (de fichiers journaux et autres) dans les machines virtuelles Azure avec SAP HANA, fournies par le biais de Backint, sont des flux vers les coffres Azure Recovery Services (qui en interne utilisent Azure Storage Blob). Il est donc important de comprendre la méthodologie de streaming employée.

Le composant Backint de HANA fournit les « canaux » (un pour la lecture et un pour l’écriture), connectés aux disques sous-jacents où résident les fichiers de base de données, qui sont alors lus par le service de sauvegarde Azure et transportés jusqu’au coffre Azure Recovery Services, un compte de stockage Azure distant. En plus des contrôles de validation natifs de Backint, le service Sauvegarde Azure effectue une somme de contrôle pour valider les flux. Ces validations permettent de vérifier que les données présentes dans le coffre Azure Recovery Services sont bien fiables et récupérables.

À partir du moment où les flux gèrent principalement des disques, vous devez comprendre les performances de disque, en termes de performances de lecture et de réseau pour transférer les données de sauvegarde, pour évaluer aussi les performances de sauvegarde et de restauration. Référez-vous à [cet article](../virtual-machines/disks-performance.md) pour approfondir votre connaissance du débit et des performances de disque ou de réseau dans les machines virtuelles Azure. Ces informations s’appliquent également aux performances de sauvegarde et de restauration.

**Le service Sauvegarde Azure tente d’atteindre environ 420 Mbits/s pour les sauvegardes sans fichiers journaux (complètes, différentielles, incrémentielles, etc.) et jusqu’à 100 Mbits/s pour les sauvegardes de fichiers journaux pour HANA**. Comme indiqué ci-dessus, ces vitesses ne sont pas garanties et dépendent des facteurs suivants :

- Débit de disque maximal sans mise en cache, de la machine virtuelle : lecture à partir de données ou d’une zone de journaux.
- Type de disque sous-jacent et son débit : lecture à partir de données ou d’une zone de journaux.
- Débit maximal du réseau de la machine virtuelle : écriture dans le coffre Recovery Services.
- Si le réseau virtuel est équipé d’une appliance virtuelle réseau ou d’un pare-feu, il s’agit du débit réseau.
- Si les données/les journaux sont sur Azure NetApp Files : lecture à partir de ANF, écriture dans le coffre, et consommation du réseau de la machine virtuelle.

> [!IMPORTANT]
> Dans les machines virtuelles plus petites où le débit de disque non mis en cache est très proche de 400 Mbits/s (ou inférieur à cette valeur), une préoccupation est que le service de sauvegarde consomme la totalité des IOPS du disque, ce qui pourrait affecter les opérations de SAP HANA liées à la lecture/écriture des disques. Dans ce cas, pour limiter la consommation du service de sauvegarde à la valeur maximale, reportez-vous à la section suivante.

### <a name="limiting-backup-throughput-performance"></a>Limitation des performances de débit de sauvegarde

Si vous souhaitez limiter la consommation des IOPS du disque de service de sauvegarde à une valeur maximale, effectuez les étapes suivantes.

1. Accédez au dossier « opt/msawb/bin ».
2. Créez un fichier JSON nommé « ExtensionSettingOverrides.JSON ».
3. Ajoutez une paire clé-valeur au fichier JSON comme suit :

    ```json
    {
    "MaxUsableVMThroughputInMBPS": 200
    }
    ```

4. Modifiez les autorisations et la propriété du fichier comme suit :
    
    ```bash
    chmod 750 ExtensionSettingsOverrides.json
    chown root:msawb ExtensionSettingsOverrides.json
    ```

5. Aucun service ne doit être redémarré. Le service Sauvegarde Azure tente de limiter les performances de débit, comme indiqué dans ce fichier.

## <a name="what-the-pre-registration-script-does"></a>Ce que fait le script de préinscription

Le script de préinscription assure les fonctions suivantes :

* En fonction de votre distribution Linux, le script installe ou met à jour tous les packages nécessaires à l’agent de Sauvegarde Azure sur votre distribution.
* Il effectue les vérifications de connectivité réseau sortante avec les serveurs de Sauvegarde Azure et les services dépendants comme Azure Active Directory et Stockage Azure.
* Il se connecte à votre système HANA en utilisant la clé utilisateur personnalisée ou la clé utilisateur SYSTEM mentionnée dans le cadre des [prérequis](#prerequisites). Il permet de créer un utilisateur de sauvegarde (AZUREWLBACKUPHANAUSER) dans le système HANA, et la clé utilisateur peut être supprimée dès lors que le script de préinscription a été correctement exécuté. _Notez que la clé utilisateur SYSTEM ne doit pas être supprimée_.
* AZUREWLBACKUPHANAUSER reçoit les rôles et autorisations nécessaires suivants :
  * Pour MDC : DATABASE ADMIN et BACKUP ADMIN (depuis HANA 2.0 SPS05 et versions ultérieures) : pour créer des bases de données lors de la restauration.
  * Pour SDC : BACKUP ADMIN : pour créer des bases de données lors de la restauration.
  * CATALOG READ : permet de lire le catalogue de sauvegarde.
  * SAP_INTERNAL_HANA_SUPPORT : permet d’accéder à certaines tables privées. Obligatoire uniquement pour les versions SDC et MDC antérieures à HANA 2.0 SPS04 rév. 46. Cela n’est pas obligatoire pour HANA 2.0 SPS04 rév. 46 et versions ultérieures, car nous obtenons maintenant les informations requises des tables publiques avec le correctif de l’équipe HANA.
* Le script ajoute une clé à **hdbuserstore** pour AZUREWLBACKUPHANAUSER afin que le plug-in de sauvegarde HANA gère toutes les opérations (requêtes de base de données, opérations de restauration, configuration et exécution de la sauvegarde).
* Vous pouvez également choisir de créer votre propre utilisateur de sauvegarde. Vérifiez que les autorisations et rôles requis suivants sont attribués à cet utilisateur :
  * Pour MDC : DATABASE ADMIN et BACKUP ADMIN (depuis HANA 2.0 SPS05 et versions ultérieures) : pour créer des bases de données lors de la restauration.
  * Pour SDC : BACKUP ADMIN : pour créer des bases de données lors de la restauration.
  * CATALOG READ : permet de lire le catalogue de sauvegarde.
  * SAP_INTERNAL_HANA_SUPPORT : permet d’accéder à certaines tables privées. Obligatoire uniquement pour les versions SDC et MDC antérieures à HANA 2.0 SPS04 rév. 46. Cela n’est pas obligatoire pour HANA 2.0 SPS04 rév. 46 et ultérieur, car nous obtenons maintenant les informations requises des tables publiques avec le correctif de l’équipe HANA.
* Ajoutez ensuite une clé à hdbuserstore pour votre utilisateur de sauvegarde personnalisé afin que le plug-in de sauvegarde HANA gère toutes les opérations (requêtes de base de données, opérations de restauration, configuration et exécution de la sauvegarde). Passez cette clé d’utilisateur de sauvegarde personnalisée au script en tant que paramètre : `-bk CUSTOM_BACKUP_KEY_NAME` ou `-backup-key CUSTOM_BACKUP_KEY_NAME`.  _Notez que l’expiration du mot de passe de cette clé de sauvegarde personnalisée peut entraîner l’échec des opérations de sauvegarde et de restauration._

>[!NOTE]
> Pour connaître les autres paramètres acceptés par le script, utilisez la commande `bash msawb-plugin-config-com-sap-hana.sh --help`

Pour confirmer la création de la clé, exécutez la commande HDBSQL sur la machine HANA avec les informations d’identification SIDADM :

```hdbsql
hdbuserstore list
```

La sortie de commande doit afficher la clé {SID} {DBNAME} avec l’utilisateur en tant que AZUREWLBACKUPHANAUSER.

>[!NOTE]
> Vérifiez que vous disposez d’un ensemble unique de fichiers SSFS sous `/usr/sap/{SID}/home/.hdb/`. Vous ne devez trouver qu’un seul dossier dans ce chemin d’accès.

Voici un récapitulatif des étapes requises pour effectuer l’exécution du script de préinscription. Notez que dans ce flux, nous fournissons la clé utilisateur SYSTEM en tant que paramètre d’entrée au script de préinscription.

| Qui     |    Du    |    À exécuter    |    Commentaires    |
| --- | --- | --- | --- |
| `<sid>`adm (OS) |    Système d’exploitation HANA   | Lisez le tutoriel et téléchargez le script de pré-inscription.  |    Tutoriel : [Sauvegarder des bases de données HANA dans une machine virtuelle Azure](/azure/backup/tutorial-backup-sap-hana-db)   <br><br>    Téléchargez le [script de préinscription](https://go.microsoft.com/fwlink/?linkid=2173610). |
| `<sid>`adm (OS)    |    Système d’exploitation HANA    |   Démarrez HANA (HDB start)    |   Avant de configurer, assurez-vous que HANA est opérationnel.   |
| `<sid>`adm (OS)   |   Système d’exploitation HANA  |    Exécutez la commande suivante : <br>  `hdbuserstore Set`   |  `hdbuserstore Set SYSTEM <hostname>:3<Instance#>13 SYSTEM <password>`  <br><br>   **Remarque** <br>  Veillez à utiliser le nom d’hôte au lieu de l’adresse IP ou du nom de domaine complet.   |
| `<sid>`adm (OS)   |  Système d’exploitation HANA    |   Exécutez la commande suivante :<br> `hdbuserstore List`   |  Vérifiez si le résultat comprend le magasin par défaut, comme ci-dessous : <br><br> `KEY SYSTEM`  <br> `ENV : <hostname>:3<Instance#>13`    <br>  `USER : SYSTEM`   |
| Root (OS)   |   Système d’exploitation HANA    |    Exécutez le [script de pré-inscription HANA de la sauvegarde Azure](https://go.microsoft.com/fwlink/?linkid=2173610).     | `./msawb-plugin-config-com-sap-hana.sh -a --sid <SID> -n <Instance#> --system-key SYSTEM`    |
| `<sid>`adm (OS)   |   Système d’exploitation HANA   |    Exécutez la commande suivante : <br> `hdbuserstore List`   |   Vérifiez si le résultat comprend de nouvelles lignes, comme indiqué ci-dessous : <br><br>  `KEY AZUREWLBACKUPHANAUSER` <br>  `ENV : localhost: 3<Instance#>13`   <br> `USER: AZUREWLBACKUPHANAUSER`    |
| Contributeur Azure     |    Portail Azure    |   Configurez NSG, l’appliance virtuelle réseau, le pare-feu Azure, etc. pour autoriser le trafic sortant vers le service de sauvegarde Azure, Azure AD et le stockage Azure.     |    [Configurer la connectivité réseau](/azure/backup/tutorial-backup-sap-hana-db#set-up-network-connectivity)    |
| Contributeur Azure |   Portail Azure    |   Créez ou ouvrez un coffre Recovery Services, puis sélectionnez Sauvegarde HANA.   |   Recherchez toutes les machines virtuelles HANA cibles à sauvegarder.   |
| Contributeur Azure    |   Portail Azure    |   Découvrez les bases de données HANA et configurez la stratégie de sauvegarde.   |  Par exemple : <br><br>  Sauvegarde hebdomadaire : tous les dimanches à 2:00 ; conservation des données, hebdomadaire 12 semaines, mensuelle 12 mois, annuelle 3 ans   <br>   Différentielle ou incrémentielle : tous les jours, à l’exception du dimanche    <br>   Journal : toutes les 15 minutes, conservé pendant 35 jours    |
| Contributeur Azure  |   Portail Azure    |    Coffre Recovery Service – Éléments de sauvegarde – SAP HANA     |   Vérifiez les tâches de sauvegarde (charge de travail Azure).    |
| Administrateur HANA    | HANA Studio   | Vérifiez la console de sauvegarde, le catalogue de sauvegarde, backup.log, backint.log et globa.ini   |    SYSTEMDB et base de données locataire.   |

Après avoir exécuté le script de pré-inscription et procédé à la vérification, vous pouvez vérifier [les exigences de connectivité](#set-up-network-connectivity), puis [configurer la sauvegarde](#discover-the-databases) à partir du coffre Recovery Services.

## <a name="create-a-recovery-services-vault"></a>Créer un coffre Recovery Services

Un coffre Recovery Services est une entité qui stocke les sauvegardes et les points de récupération créés au fil du temps. Le coffre Recovery Services contient également les stratégies de sauvegarde associées aux machines virtuelles protégées.

Pour créer un archivage de Recovery Services :

1. Connectez-vous à votre abonnement sur le [portail Azure](https://portal.azure.com/).

2. Dans le menu de gauche, sélectionnez **Tous les services**

   ![Sélectionner Tous les services](./media/tutorial-backup-sap-hana-db/all-services.png)

3. Dans la boîte de dialogue **Tous les services**, entrez **Recovery Services**. Liste des filtres de ressources variant en fonction de votre entrée. Dans la liste des ressources, sélectionnez **Coffres Recovery Services**.

   ![Sélectionner Coffres Recovery Services](./media/tutorial-backup-sap-hana-db/recovery-services-vaults.png)

4. Dans le tableau de bord **Coffres Recovery Services**, sélectionnez **Ajouter**.

   ![Ajouter un coffre Recovery Services](./media/tutorial-backup-sap-hana-db/add-vault.png)

   La boîte de dialogue **Coffre Recovery Services** s’ouvre. Attribuez des valeurs aux champs **Nom, Abonnement, Groupe de ressources** et **Emplacement**

   ![Créer un coffre Recovery Services](./media/tutorial-backup-sap-hana-db/create-vault.png)

   * **Name** : Le nom est utilisé pour identifier le coffre Recovery Services et doit être propre à l’abonnement Azure. Spécifiez un nom composé d’au moins deux caractères, mais sans dépasser 50 caractères. Il doit commencer par une lettre et ne peut être constitué que de lettres, chiffres et traits d’union. Pour ce tutoriel, nous avons utilisé le nom **SAPHanaVault**.
   * **Abonnement**: choisissez l’abonnement à utiliser. Si vous êtes membre d’un seul abonnement, son nom s’affiche. Si vous ne savez pas quel abonnement utiliser, utilisez l’abonnement par défaut (suggéré). Vous ne disposez de plusieurs choix que si votre compte professionnel ou scolaire est associé à plusieurs abonnements Azure. Ici, nous avons utilisé l’abonnement de **laboratoire de solution SAP HANA**.
   * **Groupe de ressources** : Utilisez un groupe de ressources existant ou créez-en un. Ici, nous avons utilisé **SAPHANADemo**.<br>
   Pour afficher la liste des groupes de ressources disponibles dans votre abonnement, sélectionnez **Utiliser existant**, puis sélectionnez une ressource dans la zone de liste déroulante. Pour créer un groupe de ressources, sélectionnez **Créer** et entrez le nom. Pour obtenir des informations complètes sur les groupes de ressources, consultez [Vue d’ensemble d’Azure Resource Manager](../azure-resource-manager/management/overview.md).
   * **Emplacement** : sélectionnez la région géographique du coffre. Le coffre doit se trouver dans la même région que la machine virtuelle exécutant SAP HANA. Nous avons utilisé **USA Est 2**.

5. Sélectionnez **Vérifier + créer**.

   ![Sélectionner Vérifier + créer](./media/tutorial-backup-sap-hana-db/review-create.png)

Le coffre Recovery Services est maintenant créé.

## <a name="enable-cross-region-restore"></a>Activer la restauration inter-région

Dans le coffre Recovery Services, vous pouvez activer la restauration inter-région. Vous devez activer la restauration inter-région avant de configurer et de protéger les sauvegardes sur vos bases de données HANA. Découvrez l’[activation de la restauration inter-région](/azure/backup/backup-create-rs-vault#set-cross-region-restore).

[Apprenez-en davantage](/azure/backup/backup-azure-recovery-services-vault-overview) sur la restauration inter-région.

## <a name="discover-the-databases"></a>Détecter les bases de données

1. Dans le coffre, dans **Démarrer**, sélectionnez **Sauvegarde**. Dans la zone **Où s’exécute votre charge de travail ?** , sélectionnez **SAP HANA dans les machines virtuelles Azure**.
2. Sélectionnez **Démarrer la découverte**. Cette opération lance la détection des machines virtuelles Linux non protégées dans la région du coffre. Vous verrez la machine virtuelle Azure que vous souhaitez protéger.
3. Dans **Sélectionner les machines virtuelles**, sélectionnez le lien pour télécharger le script qui donne les autorisations au service Sauvegarde Azure d’accéder aux machines virtuelles SAP HANA pour la découverte des bases de données.
4. Exécutez le script sur la machine virtuelle hébergeant les bases de données SAP HANA que vous souhaitez sauvegarder.
5. Après avoir exécuté le script sur la machine virtuelle, dans **Sélectionner les machines virtuelles**, sélectionnez la machine virtuelle. Sélectionnez ensuite **Découvrir les bases de données**.
6. Le service Sauvegarde Azure détecte toutes les bases de données SAP HANA résidant sur la machine virtuelle. Lors de la détection, le service Sauvegarde Azure inscrit la machine virtuelle auprès du coffre et y installe une extension. Aucun agent n’est installé sur la base de données.

   ![Détecter les bases de données](./media/tutorial-backup-sap-hana-db/database-discovery.png)

## <a name="configure-backup"></a>Configurer une sauvegarde

Maintenant que les bases de données à sauvegarder sont découvertes, nous allons activer la sauvegarde.

1. Sélectionnez **Configurer la sauvegarde**.

   ![Configurer une sauvegarde](./media/tutorial-backup-sap-hana-db/configure-backup.png)

2. Dans **Sélectionner les éléments à sauvegarder**, sélectionnez une ou plusieurs bases de données à protéger, puis sélectionnez **OK**.

   ![Sélectionner les éléments à sauvegarder](./media/tutorial-backup-sap-hana-db/select-items-to-backup.png)

3. Dans **Stratégie de sauvegarde > Choisir une stratégie de sauvegarde**, créez une stratégie de sauvegarde pour les bases de données en suivant les instructions de la section suivante.

   ![Choisir une stratégie de sauvegarde](./media/tutorial-backup-sap-hana-db/backup-policy.png)

4. Après avoir créé la stratégie, dans le menu **Sauvegarde**, sélectionnez **Activer la sauvegarde**.

   ![Sélectionner Activer la sauvegarde](./media/tutorial-backup-sap-hana-db/enable-backup.png)

5. Vous pouvez suivre la progression de la configuration de la sauvegarde dans la zone **Notifications** du portail.

## <a name="creating-a-backup-policy"></a>Création d’une stratégie de sauvegarde

Une stratégie de sauvegarde définit le moment auquel les sauvegardes sont effectuées, ainsi que leur durée de rétention.

* Une stratégie est créée au niveau du coffre.
* Plusieurs coffres peuvent utiliser la même stratégie de sauvegarde, mais vous devez appliquer la stratégie de sauvegarde à chaque coffre.

Spécifiez les paramètres de stratégie comme suit :

1. Dans **Nom de la stratégie**, entrez le nom de la nouvelle stratégie. Dans le cas présent, entrez **SAPHANA**.

   ![Entrer le nom de la nouvelle stratégie](./media/tutorial-backup-sap-hana-db/new-policy.png)

2. Dans **Stratégie de sauvegarde complète**, sélectionnez une **Fréquence de sauvegarde**. Vous pouvez choisir **Quotidienne** ou **Hebdomadaire**. Pour ce tutoriel, nous avons choisi la sauvegarde **Quotidienne**.

   ![Sélectionner une fréquence de sauvegarde](./media/tutorial-backup-sap-hana-db/backup-frequency.png)

3. Dans **Durée de rétention**, configurez les paramètres de rétention pour la sauvegarde complète.
   * Par défaut, toutes les options sont sélectionnées. Désactivez les limites de durée de rétention que vous ne souhaitez pas utiliser et définissez celles qui vous intéressent.
   * La période de rétention minimale est de sept jours pour tous les types de sauvegardes (complète/différentielle/fichier journal).
   * Des points de récupération sont marqués pour la rétention et varient selon la durée de rétention. Par exemple, si vous sélectionnez une sauvegarde complète quotidienne, seule une sauvegarde complète est déclenchée chaque jour.
   * La sauvegarde d’un jour spécifique est marquée et conservée conformément à la durée de rétention hebdomadaire et aux paramètres.
   * Les durées de rétention mensuelle et annuelle ont le même comportement.
4. Dans le menu de **stratégie Sauvegarde complète**, cliquez sur **OK** pour accepter les paramètres.
5. Sélectionnez ensuite **Sauvegarde différentielle** pour ajouter une stratégie différentielle.
6. Dans la stratégie **Sauvegarde différentielle**, sélectionnez **Activer** pour ouvrir les contrôles de fréquence et de rétention. Nous avons activé une sauvegarde différentielle chaque **dimanche** à **2:00**, qui est conservée pendant **30 jours**.

   ![Stratégie de sauvegarde différentielle](./media/tutorial-backup-sap-hana-db/differential-backup-policy.png)

   >[!NOTE]
   >Vous pouvez choisir une sauvegarde différentielle ou incrémentielle comme sauvegarde quotidienne, mais pas les deux.

7. Dans la stratégie **Sauvegarde incrémentielle**, sélectionnez **Activer** pour ouvrir les contrôles de fréquence et de rétention.
    * Vous pouvez déclencher au plus une sauvegarde incrémentielle par jour.
    * Les sauvegardes incrémentielles peuvent être conservées jusqu’à 180 jours. Si vous avez besoin d’une durée de rétention supérieure, vous devez utiliser des sauvegardes complètes.

    ![Stratégie de sauvegarde incrémentielle](./media/backup-azure-sap-hana-database/incremental-backup-policy.png)

8. Sélectionnez **OK** pour enregistrer la stratégie et revenir au menu principal **Stratégie de sauvegarde**.
9. Sélectionnez **Sauvegarde de fichier journal** pour ajouter une stratégie de sauvegarde de fichier journal.
   * L’option **Sauvegarde de fichier journal** est définie par défaut sur **Activer**. Cette option ne peut pas être désactivée, car SAP HANA gère toutes les sauvegardes de fichiers journaux.
   * Nous avons défini **2 heures** comme planification de sauvegarde et **15 jours** de période de rétention.

    ![Stratégie de sauvegarde de fichier journal](./media/tutorial-backup-sap-hana-db/log-backup-policy.png)

   >[!NOTE]
   > Les sauvegardes de fichiers journaux ne commencent à se produire qu’en cas de réussite d’une sauvegarde complète.
   >

10. Sélectionnez **OK** pour enregistrer la stratégie et revenir au menu principal **Stratégie de sauvegarde**.
11. Après avoir défini la stratégie de sauvegarde, sélectionnez **OK**.

Vous avez maintenant configuré les sauvegardes de vos bases de données SAP HANA.

## <a name="next-steps"></a>Étapes suivantes

* Découvrez comment [exécuter des sauvegardes à la demande sur des bases de données SAP HANA qui s’exécutent sur des machines virtuelles Azure](backup-azure-sap-hana-database.md#run-an-on-demand-backup)
* Découvrez comment [restaurer des bases de données SAP HANA qui s’exécutent sur des machines virtuelles Azure](sap-hana-db-restore.md)
* Découvrez comment [gérer des bases de données SAP HANA sauvegardées à l’aide de Sauvegarde Azure](sap-hana-db-manage.md)
* Découvrez comment [résoudre les problèmes courants lors de la sauvegarde de bases de données SAP HANA](backup-azure-sap-hana-database-troubleshoot.md)
