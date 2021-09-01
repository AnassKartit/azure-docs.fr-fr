---
title: Matrice de prise en charge du Centre de sauvegarde
description: Cet article résume les scénarios pris en charge par le Centre de sauvegarde pour chaque type de charge de travail.
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: b922f2e320f9b41186f5a6397af5cd5f4b3f7750
ms.sourcegitcommit: 2d412ea97cad0a2f66c434794429ea80da9d65aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/14/2021
ms.locfileid: "122563578"
---
# <a name="support-matrix-for-backup-center"></a>Matrice de prise en charge du Centre de sauvegarde

Le Centre de sauvegarde offre aux entreprises une interface unique pour [gérer, surveiller, exploiter et analyser les sauvegardes à grande échelle](backup-center-overview.md). Cet article résume les scénarios pris en charge par le Centre de sauvegarde pour chaque type de charge de travail.

## <a name="supported-scenarios"></a>Scénarios pris en charge

| **Catégorie** | **Scénario**  | **Charges de travail prises en charge**  | **Limites** |
| -------------| ------------- | ----------------------- |------------|
| Surveillance   | Afficher tous les travaux | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure <br><br> <li> Azure Files<br/><br/> <li>Objets blob Azure<br/><br/> <li>Azure Disques managés | <li> 7 jours de travaux prêts à l'emploi. <br> <li> Chaque filtre/liste déroulante prend en charge un maximum de 1 000 éléments. Le Centre de sauvegarde peut donc être utilisé pour surveiller un maximum de 1 000 abonnements et de 1 000 coffres sur les différents locataires. |
| Surveillance | Afficher toutes les instances de sauvegarde | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure <br><br> <li> Azure Files<br/><br/> <li>Objets blob Azure<br/><br/> <li>Azure Disques managés | Identique à ce qui précède |
| Surveillance | Afficher toutes les stratégies de sauvegarde | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure <br><br> <li> Azure Files<br/><br/> <li>Objets blob Azure<br/><br/> <li>Azure Disques managés | Identique à ce qui précède |
| Surveillance | Afficher tous les coffres | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure <br><br> <li> Azure Files<br/><br/> <li>Objets blob Azure<br/><br/> <li>Azure Disques managés | Identique à ce qui précède |
| Actions | Configurer une sauvegarde | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure <br><br> <li> Azure Files<br/><br/> <li>Objets blob Azure<br/><br/> <li>Azure Disques managés | Reportez-vous aux matrices de prise en charge pour la [Sauvegarde de machines virtuelles Azure](./backup-support-matrix-iaas.md) et la [Sauvegarde d'un serveur Azure Database pour PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Actions | Restaurer une instance de sauvegarde | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure <br><br> <li> Azure Files<br/><br/> <li>Objets blob Azure<br/><br/> <li>Azure Disques managés | Reportez-vous aux matrices de prise en charge pour la [Sauvegarde de machines virtuelles Azure](./backup-support-matrix-iaas.md) et la [Sauvegarde d'un serveur Azure Database pour PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Actions | Create vault | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure <br><br> <li> Azure Files<br/><br/> <li>Objets blob Azure<br/><br/> <li>Azure Disques managés | Reportez-vous aux matrices de prise en charge pour le [coffre Recovery Services](./backup-support-matrix.md#vault-support) |
| Actions | Créer la stratégie de sauvegarde | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure <br><br> <li> Azure Files<br/><br/> <li>Objets blob Azure<br/><br/> <li>Azure Disques managés | Reportez-vous aux matrices de prise en charge pour le [coffre Recovery Services](./backup-support-matrix.md#vault-support) |
| Actions | Exécuter une sauvegarde à la demande pour une instance de sauvegarde | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure <br><br> <li> Azure Files<br/><br/> <li>Objets blob Azure<br/><br/> <li>Azure Disques managés | Reportez-vous aux matrices de prise en charge pour la [Sauvegarde de machines virtuelles Azure](./backup-support-matrix-iaas.md) et la [Sauvegarde d'un serveur Azure Database pour PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Actions | Arrêter la sauvegarde pour une instance de sauvegarde | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure <br><br> <li> Azure Files<br/><br/> <li>Objets blob Azure<br/><br/> <li>Azure Disques managés | Reportez-vous aux matrices de prise en charge pour la [Sauvegarde de machines virtuelles Azure](./backup-support-matrix-iaas.md) et la [Sauvegarde d'un serveur Azure Database pour PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Insights | Afficher les rapports de sauvegarde | <li> Machine virtuelle Azure <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA dans une machine virtuelle Azure <br><br> <li> Azure Files <br><br> <li> System Center Data Protection Manager <br><br> <li> Agent Azure Backup (MARS) <br><br> <li> Serveur Azure Backup (MABS) | Reportez-vous aux [scénarios pris en charge pour les Rapports de sauvegarde](./configure-reports.md#supported-scenarios) |
| Gouvernance | Afficher et attribuer des stratégies Azure intégrées et personnalisées sous la catégorie « Sauvegarde » | N/A | N/A |
| Gouvernance | Afficher les sources de données non configurées pour la sauvegarde | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL | N/A |
| Surveillance | Afficher les alertes Azure Monitor à grande échelle | <li> Machine virtuelle Azure <br><br> <li> Serveur Azure Database pour PostgreSQL <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure <br><br> <li> Azure Files<br/><br/> <li>Objets blob Azure<br/><br/> <li>Azure Disques managés | Consultez la documentation sur les [alertes](./backup-azure-monitoring-built-in-monitor.md#azure-monitor-alerts-for-azure-backup-preview) |
| Actions | Exécuter un travail de restauration inter-région à partir du Centre de sauvegarde | <li> Machine virtuelle Azure <br><br> <li> SQL dans une machine virtuelle Azure <br><br> <li> SAP HANA sur machine virtuelle Azure | Consultez la document sur la [restauration inter-régions](./backup-create-rs-vault.md#set-cross-region-restore) |

## <a name="unsupported-scenarios"></a>Scénarios non pris en charge

| **Catégorie** | **Scénario**  |
|--------------|---------------|
| Actions | La configuration des paramètres de coffre à grande échelle n’est pas prise en charge actuellement par le centre de sauvegarde |
| Disponibilité | Le centre de sauvegarde n’est actuellement pas disponible dans les clouds nationaux | 

## <a name="next-steps"></a>Étapes suivantes

* [Passer en revue la matrice de prise en charge du service Sauvegarde Azure](./backup-support-matrix.md)
* [Passer en revue la matrice de prise en charge de la sauvegarde des machines virtuelles Azure](./backup-support-matrix-iaas.md)
* [Passer en revue la matrice de prise en charge de la sauvegarde d'un serveur Azure Database pour PostgreSQL](backup-azure-database-postgresql.md#support-matrix)