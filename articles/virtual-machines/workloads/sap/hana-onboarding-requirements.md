---
title: Conditions d’intégration pour SAP HANA sur Azure (grandes instances) | Microsoft Docs
description: Découvrez les conditions d’intégration pour SAP HANA sur Azure (grandes instances).
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-sap
ms.subservice: baremetal-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/14/2021
ms.author: madhukan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 899fd32a60f4938f42dc03b5ac836ff809c5c61f
ms.sourcegitcommit: e1d5abd7b8ded7ff649a7e9a2c1a7b70fdc72440
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/27/2021
ms.locfileid: "110579274"
---
# <a name="onboarding-requirements"></a>Conditions d’intégration

Cet article répertorie les conditions requises pour l’exécution de SAP HANA sur Azure (grandes instances) (également appelées instances BareMetal Infrastructure).

## <a name="microsoft-azure"></a>Microsoft Azure

- Un abonnement Azure qui peut être lié à SAP HANA sur Azure (grandes instances).
- Contrat de support Premier Microsoft. Pour plus d’informations sur l’exécution de SAP dans Azure, consultez [Remarque sur la prise en charge SAP n° 2015553 – SAP sur Microsoft Azure : composants requis de la prise en charge](https://launchpad.support.sap.com/#/notes/2015553). Si vous utilisez des unités de grande instance HANA incluant 384 UC et plus, vous devez également étendre le contrat de support Premier pour inclure Azure Rapid Response.
- Connaissance des [références (SKU) de grande instance HANA](hana-available-skus.md) dont vous aurez besoin après avoir effectué un [exercice de dimensionnement](hana-sizing.md) avec SAP.

## <a name="network-connectivity"></a>Connectivité réseau

- ExpressRoute entre le système local et Azure : pour connecter votre centre de données local à Azure, assurez-vous de commander au moins une connexion de 1 Gbit/s à votre ISP. La connexion entre les grandes instances HANA et Azure utilise également la technologie ExpressRoute. Cette connexion ExpressRoute entre les grandes instances HANA et Azure est incluse dans le prix des unités des grandes instances HANA. Ce prix comprend également tous les frais d’entrée et de sortie des données pour ce circuit ExpressRoute spécifique. Vous n’aurez donc pas de coûts ajoutés au-delà de votre liaison ExpressRoute entre l’environnement local et Azure.

## <a name="operating-system"></a>Système d'exploitation

- Licences pour SUSE Linux Enterprise Server 12 pour les applications SAP.

   > [!NOTE] 
   > Le système d’exploitation livré par Microsoft n’est pas enregistré avec SUSE. Il n’est pas connecté à une instance de l’outil de gestion des abonnements.

- L’outil de gestion des abonnements SUSE Linux déployé dans Azure sur une machine virtuelle. Cet outil offre la capacité pour SAP HANA sur Azure (grandes instances) d’être inscrit et respectivement mis à jour par SUSE. (Il n’existe aucun accès internet au sein du centre de données de la grande instance HANA.) 
- Licences pour Red Hat Enterprise Linux 6.7 ou 7.x pour SAP HANA.

   > [!NOTE]
   > Le système d’exploitation livré par Microsoft n’est pas enregistré avec Red Hat. Il n’est pas connecté à une instance de l’outil de gestion des abonnements Red Hat.

- Gestionnaire des abonnements Red Hat déployé dans Azure sur une machine virtuelle. Le gestionnaire des abonnements Red Hat permet à SAP HANA sur Azure (grandes instances) d’être inscrit et respectivement mis à jour par Red Hat. (Il n’existe aucun accès direct à internet à partir de l’abonné déployé sur le tampon de grande instance Azure.)
- Le système SAP requiert également un contrat de support avec votre fournisseur Linux. Cette exigence n’est pas supprimée par la solution de la grande instance HANA ni par le fait de l’exécution de Linux dans Azure. Contrairement à certaines images de la galerie Linux Azure, les frais de service ne sont *pas* inclus dans l’offre de solution de la grande instance HANA. Il vous incombe de respecter les exigences de SAP en ce qui concerne les contrats de support avec le distributeur Linux. 
   - Pour connaître les exigences en matière de contrat de support avec SUSE Linux, consultez [Remarque SAP n° 1984787 - SUSE LINUX Enterprise Server 12 : remarques d’installation](https://launchpad.support.sap.com/#/notes/1984787) et [Remarque SAP n ° 1056161 - Support prioritaire SUSE pour les applications SAP](https://launchpad.support.sap.com/#/notes/1056161).
   - Pour Red Hat Linux, vous devez disposer des bons niveaux d’abonnement, y compris la prise en charge et les mises à jour du service des systèmes d’exploitation de la grande instance HANA. Red Hat recommande la solution d’abonnement Red Hat Enterprise Linux pour SAP. Consultez la page https://access.redhat.com/solutions/3082481. 

Pour consulter la matrice de prise en charge des différentes versions SAP HANA avec les différentes versions Linux, reportez-vous à [Remarque SAP n° 2235581](https://launchpad.support.sap.com/#/notes/2235581).

Pour la matrice de compatibilité des versions de système d’exploitation et de microprogramme/pilote HLI, consultez [Mise à niveau du système d’exploitation pour HLI](os-upgrade-hana-large-instance.md).


> [!IMPORTANT] 
> Pour les unités de Type II, seule la version SLES 12 SP2 du système d’exploitation est actuellement prise en charge. 


## <a name="database"></a>Base de données

- Licences et composants d’installation logiciels pour SAP HANA (édition Enterprise ou Platform).

## <a name="applications"></a>Applications

- Licences et composants d’installation logiciels pour toutes les applications SAP se connectant à SAP HANA et associées aux contrats de support SAP.
- Licences et composants d’installation logiciels pour toutes les applications autres que SAP utilisées avec SAP HANA sur Azure (grandes instances) et associées aux contrats de support.

## <a name="skills"></a>Compétences

- Expérience et connaissances relatives à Azure IaaS et ses composants.
- Expérience et connaissances du mode de déploiement d’une charge de travail SAP dans Azure.
- Personnel certifié pour l’installation de SAP HANA.
- Compétences en architecture SAP pour concevoir la haute disponibilité et la récupération d’urgence autour de SAP HANA.

## <a name="sap"></a>SAP

- Vous devez être un client SAP et disposer d’un contrat de support auprès de SAP.
- En particulier pour les implémentations de références SKU de grande instance HANA de classe Type II, consultez SAP pour connaître les versions de SAP HANA et les configurations éventuelles sur du matériel monté en puissance de grande taille.

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment utiliser des nœuds d’extension et de hiérarchisation des données SAP HANA.

> [!div class="nextstepaction"]
> [Utiliser une hiérarchisation des données et des nœuds d’extension SAP HANA](hana-data-tiering-extension-nodes.md)
