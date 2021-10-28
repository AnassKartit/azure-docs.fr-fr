---
title: À propos de la sauvegarde de base de données SAP HANA dans les machines virtuelles Azure
description: Cet article explique comment sauvegarder des bases de données SAP HANA qui s’exécutent sur des machines virtuelles Azure.
ms.topic: conceptual
ms.date: 09/27/2021
ms.openlocfilehash: 470635afee7e6b483ec9ae6e79071cd4ef9975dc
ms.sourcegitcommit: 692382974e1ac868a2672b67af2d33e593c91d60
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/22/2021
ms.locfileid: "130264576"
---
# <a name="about-sap-hana-database-backup-in-azure-vms"></a>À propos de la sauvegarde de base de données SAP HANA dans les machines virtuelles Azure

Les bases de données SAP HANA sont des charges de travail critiques qui nécessitent un objectif de point de récupération (RPO) faible et un objectif de temps de récupération (RTO) rapide. Vous pouvez désormais [sauvegarder des bases de données SAP HANA s’exécutant sur les machines virtuelles Azure](./tutorial-backup-sap-hana-db.md) à l’aide du service [Sauvegarde Azure](./backup-overview.md).

Le service Sauvegarde Azure est [certifié Backint](https://www.sap.com/dmc/exp/2013_09_adpd/enEN/#/d/solutions?id=8f3fd455-a2d7-4086-aa28-51d8870acaa5) par SAP pour la prise en charge de sauvegarde native à l’aide d’API natives de SAP HANA. Cette offre du service Sauvegarde Azure s’aligne sur le mantra de Sauvegarde Azure des sauvegardes d’**infrastructure zéro**, ce qui élimine le besoin de déployer et de gérer l’infrastructure de sauvegarde. Vous pouvez désormais effectuer en toute transparence une sauvegarde et une restauration de bases de données SAP HANA s’exécutant sur des machines virtuelles Azure (les [machines virtuelles de la série M](../virtual-machines/m-series.md) sont désormais également prises en charge) et tirer parti des fonctionnalités de gestion d’entreprise du service Sauvegarde Azure.

## <a name="added-value"></a>Valeur ajoutée

L’utilisation du service Sauvegarde Azure pour sauvegarder et restaurer des bases de données SAP HANA offre les avantages suivants :

* **Objectif de point de récupération (RPO) de 15 minutes** : La récupération des données critiques jusqu’à 15 minutes est désormais possible.
* **Restaurations dans le temps en un seul clic** : La restauration de données de production sur d’autres serveurs HANA est facilitée. Le chaînage des sauvegardes et des catalogues pour effectuer des restaurations est entièrement géré par Azure en arrière-plan.
* **Conservation à long terme** : Pour des besoins stricts de conformité et d’audit. Conservez vos sauvegardes pendant des années, en fonction de la durée de conservation au-delà de laquelle les points de récupération sont nettoyés automatiquement par la fonctionnalité intégrée de gestion du cycle de vie.
* **Gestion des sauvegardes à partir d’Azure** : Utilisez les fonctionnalités de gestion et de surveillance du service Sauvegarde Azure pour améliorer l’expérience de gestion. Azure CLI est également pris en charge.

Pour afficher les scénarios de sauvegarde et de restauration que nous prenons en charge aujourd’hui, reportez-vous à la [matrice de prise en charge du scénario SAP HANA](./sap-hana-backup-support-matrix.md#scenario-support).

## <a name="backup-architecture"></a>Architecture de sauvegarde

Vous pouvez sauvegarder des bases de données SAP HANA s’exécutant à l’intérieur d’une machine virtuelle Azure et diffuser en continu des données de sauvegarde directement dans le coffre Recovery Services Azure.

![Diagramme de l’architecture de sauvegarde](./media/sap-hana-db-about/backup-architecture.png)

* Le processus de sauvegarde commence par la [création d’un coffre Recovery Services](./tutorial-backup-sap-hana-db.md#create-a-recovery-services-vault) dans Azure. Ce coffre sera utilisé pour stocker les sauvegardes et les points de récupération créés au fil du temps.
* La machine virtuelle Azure exécutant le serveur SAP HANA est inscrite auprès du coffre, et les bases de données à sauvegarder sont [découvertes](./tutorial-backup-sap-hana-db.md#discover-the-databases). Pour permettre au service Sauvegarde Azure de découvrir les bases de données, un [script de préinscription](https://go.microsoft.com/fwlink/?linkid=2173610) doit être exécuté sur le serveur HANA en tant qu’utilisateur racine.
* Ce script crée l’utilisateur de base de données et/ou utilise l’utilisateur de sauvegarde personnalisé **AZUREWLBACKUPHANAUSER** que vous avez déjà créé, puis crée une clé correspondante portant le même nom dans **hdbuserstore**. [En savoir plus](./tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) sur la fonctionnalité du script.
* Le service Sauvegarde Azure installe à présent le **plug-in Sauvegarde Azure pour HANA** sur le serveur SAP HANA inscrit.
* L’utilisateur de base de données **AZUREWLBACKUPHANAUSER** créé par le script de pré-inscription/l’utilisateur de sauvegarde personnalisé que vous avez créé (et ajouté comme entrée au script de pré-inscription) est utilisé par le **plug-in Sauvegarde Azure pour Hana** pour effectuer toutes les opérations de sauvegarde et de restauration. Si vous tentez de configurer une sauvegarde pour des bases de données SAP HANA sans exécuter ce script, il se peut que vous receviez l’erreur **UserErrorHanaScriptNotRun**.
* Pour [configurer une sauvegarde](./tutorial-backup-sap-hana-db.md#configure-backup) sur les bases de données découvertes, choisissez la stratégie de sauvegarde requise et activez les sauvegardes.

* Une fois la sauvegarde configurée, le service Sauvegarde Azure configure les paramètres Backint suivants au niveau de la BASE DE DONNÉES sur le serveur de SAP HANA protégé :
  * [catalog_backup_using_backint:true]
  * [enable_accumulated_catalog_backup:false]
  * [parallel_data_backup_backint_channels:1]
  * [log_backup_timeout_s:900)]
  * [backint_response_timeout:7200]

>[!NOTE]
>Assurez-vous que ces paramètres ne sont *pas* présents au niveau de HÔTE. Les paramètres au niveau de l’hôte remplacent ces paramètres et peuvent entraîner un comportement inattendu.
>

* Le **plug-in Sauvegarde Azure pour HANA** conserve l’ensemble des planifications de sauvegarde et des détails de la stratégie. Il déclenche les sauvegardes planifiées et communique avec le **Moteur de sauvegarde HANA** via les API Backint.
* Le **Moteur de sauvegarde HANA** retourne un flux Backint avec les données à sauvegarder.
* Les sauvegardes planifiées et à la demande (déclenchées à partir du portail Azure) complètes ou différentielles sont toutes initiées par le **plug-in Sauvegarde Azure pour HANA**. Toutefois, les sauvegardes de fichier journal sont gérées et déclenchées par le **Moteur de sauvegarde HANA**.
* Le service Sauvegarde Azure pour SAP HANA, qui est une solution certifiée par BackInt, ne dépend pas des types de disque ou de machine virtuelle sous-jacents. La sauvegarde est effectuée par des flux générés par HANA.

## <a name="using-azure-vm-backup-with-azure-sap-hana-backup"></a>Utilisation de la sauvegarde de machine virtuelle Azure avec la sauvegarde Azure SAP HANA

En plus d’utiliser la sauvegarde SAP HANA dans Azure qui assure une sauvegarde et une récupération de niveau base de données, vous pouvez utiliser la solution de sauvegarde de machine virtuelle Azure pour sauvegarder les disques de système d’exploitation et les disques qui ne comportent pas de base de données.

La [solution de sauvegarde Azure SAP HANA certifiée par Backint](#backup-architecture) peut être utilisée pour la sauvegarde et la récupération de base de données.

La [sauvegarde de machine virtuelle Azure ](backup-azure-vms-introduction.md) peut être utilisée pour sauvegarder les disques de système d’exploitation et d’autres disques qui ne comportent pas de base de données. La sauvegarde de machine virtuelle a lieu une fois par jour et porte sur tous les disques (à l’exception des **disques de système d’exploitation de type WA (Accélérateur d’écriture)** et des **disques Ultra**). Sachant que la base de données est sauvegardée à l’aide de la solution de sauvegarde Azure SAP HANA, vous pouvez effectuer une sauvegarde cohérente au niveau fichier uniquement pour les disques de système d’exploitation et les disques qui ne comportent pas de base de données en utilisant la fonctionnalité [Sauvegarde et restauration de disque sélective pour les machines virtuelles Azure](selective-disk-backup-restore.md).

Pour restaurer une machine virtuelle exécutant SAP HANA, effectuez ces étapes :

* [Restaurez une nouvelle machine virtuelle à partir de la sauvegarde de machine virtuelle Azure](backup-azure-arm-restore-vms.md) et en partant du dernier point de récupération. Sinon, créez une machine virtuelle vide et attachez les disques du dernier point de récupération.
* Si les disques WA sont exclus, ils ne sont pas restaurés. Dans ce cas, créez des disques WA vides et une zone de journal.
* Une fois que toutes les autres configurations (adresse IP, nom du système, etc.) ont été effectuées, la machine virtuelle est définie pour recevoir les données de base de données de Sauvegarde Azure.
* Restaurez maintenant la base de données dans la machine virtuelle à partir de la [sauvegarde de base de données Azure SAP HANA](sap-hana-db-restore.md#restore-to-a-point-in-time-or-to-a-recovery-point) jusqu’aux date et heure souhaitées.

## <a name="next-steps"></a>Étapes suivantes

* Découvrez comment [Restaurer une base de données SAP HANA s’exécutant sur une machine virtuelle Azure](./sap-hana-db-restore.md)
* Découvrez comment [gérer des bases de données SAP HANA sauvegardées à l’aide de Sauvegarde Azure](./sap-hana-db-manage.md)