---
title: Configuration système exigée Microsoft Azure Data Box Disk | Microsoft Docs
description: En savoir plus sur les conditions logicielles et réseau requises pour votre solution Azure Data Box Disk
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 10/07/2021
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 171297be3e0e8e5215c5c551e984a45d0516507e
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2021
ms.locfileid: "130003153"
---
::: zone target="docs"

# <a name="azure-data-box-disk-system-requirements"></a>Configuration système requise pour Azure Data Box Disk

Cet article décrit la configuration système importante qui est demandée pour la solution Microsoft Azure Data Box Disk et pour les clients accédant au disque Data Box. Nous vous recommandons de lire attentivement les informations suivantes avant de déployer votre disque Data Box, puis d’y revenir si nécessaire pendant le déploiement, et après pour son fonctionnement.

La configuration système demandée comprend les plateformes prises en charge pour les clients accédant aux disques, les comptes de stockage pris en charge et les types de stockage.

::: zone-end

::: zone target="chromeless"

## <a name="review-prerequisites"></a>Vérifier les conditions préalables

1. Vous devez avoir commandé Data Box Disk à l’aide du [Tutoriel : Commander Azure Data Box Disk](data-box-disk-deploy-ordered.md). Vous avez reçu vos disques et un câble de connexion par disque.
2. Vous avez un ordinateur client disponible à partir duquel vous pouvez copier les données. Votre ordinateur client doit :

    - Exécuter un système d’exploitation pris en charge.
    - Avoir installé les autres logiciels requis.

::: zone-end

## <a name="supported-operating-systems-for-clients"></a>Systèmes d’exploitation pris en charge pour les clients

Voici une liste des systèmes d’exploitation pris en charge pour le déverrouillage de disque et l’opération de copie des données par le biais des clients connectés au disque Data Box.

| **Système d’exploitation** | **Versions testées** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows (64 bit) |7, 8, 10 |
|Linux <br> <li> Ubuntu </li><li> Debian </li><li> Red Hat Enterprise Linux (RHEL) </li><li> CentOS| <br>14.04, 16.04, 18.04 <br> 8.11, 9 <br> 7.0 <br> 6.5, 6.9, 7.0, 7.5 |  

## <a name="other-required-software-for-windows-clients"></a>Autres logiciels requis pour les clients Windows

Pour un client Windows, les éléments suivants doivent également être installés.

| **Logiciels**| **Version** |
| --- | --- |
| Windows PowerShell |5.0 |
| .NET Framework |4.5.1 |
| Windows Management Framework |5,1|
| BitLocker| - |

## <a name="other-required-software-for-linux-clients"></a>Autres logiciels requis pour les clients Linux

Pour les clients Linux, l’ensemble d’outils Data Box Disk installe les logiciels requis suivants :

- dislocker
- OpenSSL

## <a name="supported-connection"></a>Connexion prise en charge

L’ordinateur client qui contient les données doit être doté d’un port USB 3.0 ou version ultérieure. Les disques se connectent à ce client à l’aide du câble fourni.

## <a name="supported-storage-accounts"></a>Comptes de stockage pris en charge

Voici une liste des types de stockage pris en charge pour le disque Data Box.

| **Compte de stockage** | **Niveaux d’accès pris en charge** |
| --- | --- |
| Standard classique | |
| Édition Standard v1 à usage général  | Chaud, froid |
| Comptes de stockage à usage général v1 Premium   |  |
| Standard v2 universel<sup>*</sup> | Chaud, froid |
| Comptes de stockage à usage général v2 Premium   |  |
| Compte de stockage d’objets blob | |

<sup>*</sup> *Azure Data Lake Storage Gen2 (ADLS Gen2) est pris en charge.*

> [!IMPORTANT]
> Le protocole NFS (Network File System) 3.0 dans Stockage Blob Azure n’est pas pris en charge avec Data Box Disk.

## <a name="supported-storage-types-for-upload"></a>Types de stockage pris en charge pour le chargement

Voici la liste des types de stockages pris en charge pour téléchargement sur Azure à l’aide de Data Box Disk.

| **Format de fichier** | **Remarques** |
| --- | --- |
| Objet blob de blocs Azure | |
| Objet blob de pages Azure  | |
| Azure Files  | |
| Disques managés | |

::: zone target="docs"

## <a name="next-step"></a>Étape suivante

* [Déployer votre disque Azure Data Box](data-box-disk-deploy-ordered.md)

::: zone-end

