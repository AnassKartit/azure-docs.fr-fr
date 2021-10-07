---
title: Métriques pour Azure NetApp Files | Microsoft Docs
description: Azure NetApp Files fournit des métriques sur le stockage alloué, l’utilisation réelle du stockage, les IOPS du volume et la latence. Utilisez ces métriques pour comprendre l’utilisation et les performances.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/29/2021
ms.author: b-juche
ms.openlocfilehash: cc034689e2c3cd6846986680225ca7ca21ac41c8
ms.sourcegitcommit: f3f2ec7793ebeee19bd9ffc3004725fb33eb4b3f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2021
ms.locfileid: "129407529"
---
# <a name="metrics-for-azure-netapp-files"></a>Métriques pour Azure NetApp Files

Azure NetApp Files fournit des métriques sur le stockage alloué, l’utilisation réelle du stockage, les IOPS du volume et la latence. En analysant ces métriques, vous en saurez plus sur le mode d'utilisation et les performances des volumes de vos comptes NetApp.  

Vous pouvez rechercher des métriques pour un pool de capacité ou un volume en sélectionnant le **pool de capacité** ou le **volume**.  Cliquez ensuite sur **Métrique** pour afficher les métriques disponibles : 

[ ![Instantané qui montre comment accéder à la liste déroulante de métriques.](../media/azure-netapp-files/metrics-navigate-volume.png)](../media/azure-netapp-files/metrics-navigate-volume.png#lightbox)

## <a name="usage-metrics-for-capacity-pools"></a><a name="capacity_pools"></a>Métriques d'utilisation des pools de capacités

- *Taille allouée au pool*   
    Taille provisionnée du pool.

- *Pool alloué à la taille du volume*  
    Quota total des volumes (en Gio) d’un pool de capacités donné (autrement dit, total des tailles provisionnées pour les volumes du pool de capacités).  
    Il s’agit de la taille que vous avez sélectionnée lors de la création des volumes.  

- *Taille du pool consommée*  
    Espace logique total (en Gio) utilisé sur l’ensemble des volumes d’un pool de capacités.  

- *Taille totale des instantanés du pool*    
    Somme de la taille des instantanés de tous les volumes du pool.

## <a name="usage-metrics-for-volumes"></a><a name="volumes"></a>Métriques d'utilisation des volumes

- *Taille du volume consommée en pourcentage*    
    Pourcentage du volume consommé, y compris les instantanés.  
- *Taille allouée aux volumes*   
    Taille provisionnée d’un volume
- *Taille de quota du volume*    
    Taille de quota (Gio) avec laquelle le volume est approvisionné.   
-  *Taille du volume consommée*  
    Taille logique du volume (octets utilisés).  
    Cette taille inclut l'espace logique utilisé par les captures instantanées et les systèmes de fichiers actifs.  
- *Taille du cliché instantané de volume*   
   Taille de tous les instantanés dans un volume.  

## <a name="performance-metrics-for-volumes"></a>Métriques de performances des volumes

> [!NOTE] 
> La latence de volume de type *Latence de lecture moyenne* et *Latence d’écriture moyenne* est mesurée au sein du service de stockage sans tenir compte du temps de réponse du réseau.

- *Latence de lecture moyenne*   
    Temps moyen des lectures du volume (en millisecondes).
- *Latence d’écriture moyenne*   
    Temps moyen des écritures du volume (en millisecondes).
- *E/S par seconde en lecture*   
    Nombre de lectures sur le volume par seconde.
- *E/S par seconde en écriture*   
    Nombre d’écritures sur le volume par seconde.

## <a name="volume-replication-metrics"></a><a name="replication"></a>Métriques de réplication de volume

> [!NOTE] 
> * La taille de transfert réseau (par exemple, les métriques *Transfert total de la réplication de volume*) peut différer des volumes source ou de destination d’une réplication entre régions. Ce comportement est dû à l’utilisation d’un moteur de réplication efficace pour réduire au maximum le coût du transfert réseau.
> * Les métriques de réplication de volume sont actuellement renseignées pour les volumes de destination de réplication et non la source de la relation de réplication.

- *L’état de la réplication de volume est-il sain*   
    Condition de la relation de réplication. Un état sain est indiqué par `1`. Un état non sain est indiqué par `0`.

- *La réplication de volume est-elle en cours de transfert*    
    L’état de la réplication de volume est-il « En cours de transfert ». 

- *Durée du dernier transfert de réplication de volume*   
    Durée, en secondes, nécessaire au dernier transfert 

- *Taille du dernier transfert de réplication de volume*    
    Nombre total d’octets transférés dans le cadre du dernier transfert 

- *Progression de la réplication de volume*    
    Le volume total de données transférées pour l’opération de transfert en cours. 

- *Transfert total de la réplication de volume*   
    Les octets cumulés transférés pour la relation. 

## <a name="throughput-metrics-for-capacity-pools"></a>Mesures de débit pour les pools de capacité   

* *Débit alloué du pool*    
    Somme du débit de tous les volumes appartenant au pool.
    
* *Débit provisionné pour le pool*   
    Débit approvisionné de ce pool.


## <a name="throughput-metrics-for-volumes"></a>Métriques de débit pour les volumes   

* *Débit de lecture*   
    Débit de lecture en octets par seconde.
    
* *Débit total*   
    Somme de tous les débits en octets par seconde.

* *Débit d’écriture*    
    Débit d’écriture en octets par seconde.

* *Autre débit*   
    Autre débit (qui n’est pas en lecture ou en écriture) en octets par seconde.

## <a name="volume-backup-metrics"></a>Métriques de sauvegarde d’un volume  

* *La sauvegarde des volumes est-elle activée*   
    Indique si la sauvegarde est activée pour le volume. `1` est activé. `0` indique qu'ils sont désactivés.

* *L'opération de sauvegarde du volume est-elle terminée*   
    Indique si la dernière opération de sauvegarde ou de restauration de volume s'est déroulée avec succès.  `1` a réussi. `0` n’a pas réussi.

* *La sauvegarde des volumes est-elle suspendue*   
    Indique si la stratégie de sauvegarde est suspendue pour le volume.  `1` n’est pas suspendu. `0` est suspendu.

* *Octets de sauvegarde du volume*   
    Nombre total d’octets sauvegardés pour ce volume.

* *Sauvegarde du volume Derniers octets transférés*   
    Nombre total d’octets transférés pour la dernière opération de sauvegarde ou de restauration  

## <a name="next-steps"></a>Étapes suivantes

* [Comprendre la hiérarchie de stockage d’Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Créer un pool de capacités](azure-netapp-files-set-up-capacity-pool.md)
* [Créer un volume pour Azure NetApp Files](azure-netapp-files-create-volumes.md)
