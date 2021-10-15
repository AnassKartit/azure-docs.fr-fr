---
title: Créer un volume NFS pour Azure NetApp Files | Microsoft Docs
description: Cet article explique comment créer un volume NFS dans Azure NetApp Files. En savoir plus sur les éléments à prendre en compte, notamment la version à utiliser, et les meilleures pratiques.
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
ms.topic: how-to
ms.date: 10/04/2021
ms.author: b-juche
ms.openlocfilehash: d1aafd863e35d8cb19f529928c22645496fff671
ms.sourcegitcommit: c27f71f890ecba96b42d58604c556505897a34f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2021
ms.locfileid: "129536270"
---
# <a name="create-an-nfs-volume-for-azure-netapp-files"></a>Créer un volume NFS pour Azure NetApp Files

Azure NetApp Files prend en charge la création de volumes avec NFS (NFSv3 ou NFSv4.1), SMB3 ou le double protocole (NFSv3 et SMB ou NFSv4.1 et SMB). La consommation de capacité d’un volume est comptée par rapport à la capacité configurée de son pool. 

Cet article explique comment créer un volume NFS. Pour les volumes SMB, consultez [Créer un volume SMB](azure-netapp-files-create-volumes-smb.md). Pour les volumes à deux protocoles, consultez [Créer un volume à deux protocoles](create-volumes-dual-protocol.md).

## <a name="before-you-begin"></a>Avant de commencer 
* Vous devez déjà avoir configuré un pool de capacité.  
    Consultez [Créer un pool de capacités](azure-netapp-files-set-up-capacity-pool.md).   
* Un sous-réseau doit être délégué à Azure NetApp Files.  
    Consultez [Déléguer un sous-réseau à Azure NetApp Files](azure-netapp-files-delegate-subnet.md).

## <a name="considerations"></a>Considérations 

* Choix de la version de NFS à utiliser  
  NFSv3 peut gérer de nombreux cas d’usage et est couramment déployé dans la plupart des applications d’entreprise. Vous devez valider la version (NFSv3 ou NFSv 4.1) requise par votre application et créer votre volume à l’aide de la version appropriée. Par exemple, si vous utilisez [Apache ActiveMQ](https://activemq.apache.org/shared-file-system-master-slave), le verrouillage de fichiers avec NFSv 4.1 est recommandé par rapport à NFSv3. 

* Sécurité  
  La prise en charge des bits en mode UNIX (lecture, écriture et exécution) est disponible pour NFSv3 et NFSv 4.1. Un accès de niveau racine est requis sur le client NFS pour monter des volumes NFS.

* Prise en charge des utilisateurs/groupes locaux et de LDAP pour NFSv 4.1  
  Actuellement, NFSv 4.1 prend en charge l’accès racine aux volumes uniquement. Consultez [Configurer le domaine par défaut NFSv4.1 pour Azure NetApp Files](azure-netapp-files-configure-nfsv41-domain.md). 

## <a name="best-practice"></a>Bonne pratique

* Assurez-vous d’utiliser les instructions de montage appropriées pour le volume.  Voir [Monter ou démonter un volume pour des machines virtuelles Windows ou Linux](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md).

* Le client NFS doit se trouver sur le même réseau virtuel ou réseau virtuel avec peering que le volume Azure NetApp Files. La connexion depuis l’extérieur du réseau virtuel est prise en charge ; toutefois, cela introduira une latence supplémentaire et réduira les performances globales.

* Vérifiez que le client NFS est à jour et qu’il exécute les mises à jour les plus récentes du système d’exploitation.

## <a name="create-an-nfs-volume"></a>Créer un volume NFS

1.  Cliquez sur le panneau **Volumes** à partir du panneau Pools de capacités. Cliquez sur **+ Ajouter un volume** pour créer un volume. 

    ![Accédez à Volumes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png) 

2.  Dans la fenêtre Créer un volume, cliquez sur **Créer** et fournissez les informations pour les champs suivants sous l’onglet De base :   
    * **Nom du volume**      
        Spécifiez le nom du volume que vous créez.   

        Un nom de volume doit être unique au sein de chaque pool de capacité. Il doit comprendre au moins trois caractères. Le nom doit commencer par une lettre. Peut contenir des lettres, des chiffres, des traits de soulignement (_) et des traits d’union (-) uniquement.

        Vous ne pouvez pas utiliser `default` ni `bin` comme nom de volume.

    * **Pool de capacités**  
        Spécifiez le pool de capacité dans lequel vous souhaitez que le volume soit créé.

    * **Quota**  
        Spécifiez la quantité de stockage logique allouée au volume.  

        Le champ **Quota disponible** indique la quantité d’espace inutilisé dans le pool de capacités choisi, que vous pouvez utiliser pour créer un volume. La taille du nouveau volume ne doit pas dépasser le quota disponible.  

    * **Débit (Mio/s)**    
        Si le volume est créé dans un pool de capacité avec Qualité de service manuelle, spécifiez le débit souhaité pour le volume.   

        Si le volume est créé dans un pool de capacité avec Qualité de service automatique, la valeur affichée dans ce champ est (quota x débit du niveau de service).   

    * **Réseau virtuel**  
        Spécifiez le réseau virtuel Azure (VNet) à partir duquel vous voulez accéder au volume.  

        Le réseau virtuel que vous spécifiez doit avoir un sous-réseau délégué à Azure NetApp Files. Le service Azure NetApp Files est accessible seulement à partir du même réseau virtuel ou d’un sous-réseau qui se trouve dans la même région que le volume via le peering de réseau virtuel. Vous pouvez également accéder au volume à partir de votre réseau local via Express Route.   

    * **Sous-réseau**  
        Spécifiez le sous-réseau que vous souhaitez utiliser pour le volume.  
        Le sous-réseau que vous spécifiez doit être délégué à Azure NetApp Files. 
        
        Si vous n’avez pas délégué un sous-réseau, vous pouvez cliquer sur **Créer** sur la page Créer un volume. Ensuite, dans la page Créer un sous-réseau, fournissez les informations sur le sous-réseau, puis sélectionnez **Microsoft.NetApp/volumes** pour déléguer le sous-réseau à Azure NetApp Files. Dans chaque réseau virtuel, un seul sous-réseau peut être délégué à Azure NetApp Files.   
 
        ![Créer un volume](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Créer un sous-réseau](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

    * **Fonctionnalités réseau**  
        Dans les régions prises en charge, vous pouvez préciser si vous souhaitez utiliser les fonctionnalités réseau **De base** ou **Standard** pour le volume. Consultez [Configurer les fonctionnalités réseau d’un volume](configure-network-features.md) et [Consignes pour planifier un réseau Azure NetApp Files](azure-netapp-files-network-topologies.md) pour plus de détails.

    * Si vous souhaitez appliquer une stratégie d’instantané existante au volume, cliquez sur **Afficher la section avancée** pour la développer, indiquez si vous souhaitez masquer le chemin d'accès de l’instantané, puis sélectionnez une stratégie d’instantané dans le menu déroulant. 

        Pour plus d’informations sur la création d’une stratégie d’instantané, consultez [Gérer les stratégies d’instantané](snapshots-manage-policy.md).

        ![Afficher la sélection avancée](../media/azure-netapp-files/volume-create-advanced-selection.png)

3. Cliquez sur **Protocole**, puis effectuez les actions suivantes :  
    * Sélectionnez **NFS** comme type de protocole pour le volume.   

    * Spécifiez un **chemin de fichier** unique pour le volume. Ce chemin est utilisé lorsque vous créez des cibles de montage. Les exigences concernant ce chemin sont les suivantes :   
        - Il doit être unique au sein de chaque sous-réseau de la région. 
        - Il doit commencer par un caractère alphabétique.
        - Il doit contenir uniquement des lettres, des chiffres ou des tirets (`-`). 
        - La longueur totale ne doit pas dépasser 80 caractères.

    * Sélectionnez la **Version** (**NFSv3** ou **NFSv4.1**) du volume.  

    * Si vous utilisez NFSv4.1, indiquez si vous souhaitez activer le chiffrement **Kerberos** pour le volume.  

        Des configurations supplémentaires sont requises si vous utilisez Kerberos avec NFSv4.1. Suivez les instructions fournies dans [Configurer le chiffrement Kerberos NFSv4.1](configure-kerberos-encryption.md).

    * Si vous voulez activer les utilisateurs et les groupes étendus (jusqu’à 1 024 groupes) Active Directory LDAP pour accéder au volume, sélectionnez l’option **LDAP**. Suivez les instructions dans [Configurer ADDS LDAP avec des groupes étendus pour l’accès au volume NFS](configure-ldap-extended-groups.md) afin d’effectuer les configurations requises. 
 
    *  Personnalisez les **autorisations Unix** si nécessaire pour spécifier les autorisations du chemin d’accès de montage. Ce paramètre ne s’applique pas aux fichiers sous le chemin d’accès de montage. La valeur par défaut est `0770`. Ce paramètre par défaut accorde des autorisations de lecture, d’écriture et d’exécution au propriétaire et au groupe, mais aucune autorisation n’est accordée aux autres utilisateurs.     
        La spécification et les considérations relatives à l’inscription s’appliquent à la définition des **autorisations UNIX**. Suivez les instructions dans [Configurer des autorisations Unix et modifier le mode de propriété](configure-unix-permissions-change-ownership-mode.md).   

    * Le cas échéant, [configurez une stratégie d’exportation pour le volume NFS](azure-netapp-files-configure-export-policy.md).

    ![Spécifier le protocole NFS](../media/azure-netapp-files/azure-netapp-files-protocol-nfs.png)

4. Cliquez sur **Vérifier et créer** pour passer en revue les informations du volume.  Cliquez ensuite sur **Créer** pour créer le volume.

    Le volume créé s’affiche dans la page Volumes. 
 
    Un volume hérite de l’abonnement, du groupe de ressources et des attributs d’emplacement de son pool de capacité. Vous pouvez suivre l’état du déploiement du volume dans le volet des notifications.

## <a name="next-steps"></a>Étapes suivantes  

* [Configurer le domaine par défaut NFSv4.1 pour Azure NetApp Files](azure-netapp-files-configure-nfsv41-domain.md)
* [Configurer le chiffrement Kerberos NFSv4.1](configure-kerberos-encryption.md)
* [Configurer ADDS LDAP sur TLS pour Azure NetApp Files](configure-ldap-over-tls.md)
* [Configurer ADDS LDAP avec des groupes étendus pour l’accès au volume NFS](configure-ldap-extended-groups.md)
* [Monter ou démonter un volume pour des machines virtuelles Windows ou Linux](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Configurer une stratégie d’exportation pour un volume NFS](azure-netapp-files-configure-export-policy.md)
* [Configurer des autorisations Unix et modifier le mode de propriété](configure-unix-permissions-change-ownership-mode.md). 
* [Limites des ressources pour Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [En savoir plus sur l’intégration d’un réseau virtuel pour les services Azure](../virtual-network/virtual-network-for-azure-services.md)
