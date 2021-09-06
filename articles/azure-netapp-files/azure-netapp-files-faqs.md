---
title: Questions fréquentes (FAQ) sur NetApp Files | Microsoft Docs
description: Consultez les questions fréquemment posées sur Azure NetApp Files, telles que la mise en réseau, la sécurité, les performances, la gestion de la capacité et la migration/protection des données.
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
ms.date: 08/18/2021
ms.author: b-juche
ms.openlocfilehash: 954519f766c5c9f79fa0c8f61ed45f260743a0c2
ms.sourcegitcommit: 8000045c09d3b091314b4a73db20e99ddc825d91
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/19/2021
ms.locfileid: "122527942"
---
# <a name="faqs-about-azure-netapp-files"></a>Questions fréquentes (FAQ) sur Azure NetApp Files

Cet article contient les réponses à certaines questions fréquemment posées sur Azure NetApp Files. 

## <a name="networking-faqs"></a>FAQ sur la mise en réseau

### <a name="does-the-data-path-for-nfs-or-smb-go-over-the-internet"></a>Le chemin d’accès aux données pour NFS ou SMB passe-t-il par Internet ?  

Non. Le chemin d’accès aux données pour NFS ou SMB ne passe pas par Internet. Azure NetApp Files est un service natif Azure qui est déployé dans le réseau virtuel Azure (VNet) dans lequel le service est disponible. Azure NetApp Files utilise un sous-réseau délégué et configure une interface réseau directement sur le réseau virtuel. 

Voir [Consignes pour planifier un réseau Azure NetApp Files](./azure-netapp-files-network-topologies.md) pour plus de détails.  

### <a name="can-i-connect-a-vnet-that-i-already-created-to-the-azure-netapp-files-service"></a>Puis-je connecter un réseau virtuel que j’ai déjà créé pour le service Azure NetApp Files ?

Oui, vous pouvez connecter des réseaux virtuels que vous avez créés pour le service. 

Voir [Consignes pour planifier un réseau Azure NetApp Files](./azure-netapp-files-network-topologies.md) pour plus de détails.  

### <a name="can-i-mount-an-nfs-volume-of-azure-netapp-files-using-dns-fqdn-name"></a>Puis-je monter un volume NFS d’Azure NetApp Files avec le nom FQDN du DNS ?

Oui, vous pouvez le faire si vous créez les entrées DNS requises. Azure NetApp Files fournit l’adresse IP de service pour le volume configuré. 

> [!NOTE] 
> Azure NetApp Files peut déployer des adresses IP supplémentaires pour le service en fonction des besoins.  Les entrées DNS peuvent avoir à être mises à jour régulièrement.

### <a name="can-i-set-or-select-my-own-ip-address-for-an-azure-netapp-files-volume"></a>Puis-je définir ou sélectionner ma propre adresse IP pour un volume Azure NetApp Files ?  

Non. L’attribution d’adresses IP aux volumes Azure NetApp Files est dynamique. L’attribution d’adresses IP statiques n’est pas prise en charge. 

### <a name="does-azure-netapp-files-support-dual-stack-ipv4-and-ipv6-vnet"></a>Azure NetApp Files prend-il en charge le réseau virtuel à double pile (IPv4 et IPv6) ?

Non, actuellement Azure NetApp Files ne prend pas en charge le réseau virtuel à double pile (IPv4 et IPv6).  

### <a name="is-the-number-of-the-ip-addresses-using-azure-vmware-solutions-for-guest-os-mounts-limited-to-1000"></a>Le nombre d’adresses IP à l’aide de solutions VMware Azure pour les montages de système d’exploitation invités est-il [limité à 1000](azure-netapp-files-resource-limits.md#resource-limits) ?

Non. Les solutions VMware Azure se trouvent derrière une passerelle ER qui se comporte comme sur des systèmes locaux. Le nombre d’« hôtes » et d’« invités » AVS n’est pas visible pour Azure NetApp Files, et la limite d’adresse IP de 1000 ne s’applique pas.
 
## <a name="security-faqs"></a>Forum aux questions sur la sécurité

### <a name="can-the-network-traffic-between-the-azure-vm-and-the-storage-be-encrypted"></a>Le trafic réseau entre la machine virtuelle Azure et le stockage peut-il être chiffré ?

Le trafic des données Azure NetApp Files est sécurisé par conception parce qu’il ne fournit pas de point de terminaison public et que le trafic de données reste au sein du réseau virtuel appartenant au client. Les données à la volée ne sont pas chiffrées par défaut. Toutefois, le trafic des données d’une machine virtuelle Azure (exécutant un client SMB ou NFS) vers Azure NetApp Files est aussi sécurisé que tout autre trafic entre machines virtuelles Azure. 

Le protocole NFSv3 ne prend pas en charge le chiffrement. Ces données ne peuvent donc pas être chiffrées. Toutefois, le chiffrement des données à la volée NFSv4.1 et SMB3 peut être activé. Le trafic de données entre les clients NFSv4.1 et les volumes Azure NetApp Files peut être chiffré à l’aide de Kerberos avec le chiffrement AES-256. Pour plus d’informations, consultez [Configurer le chiffrement Kerberos NFSv4.1 pour Azure NetApp Files](configure-kerberos-encryption.md). Le trafic de données entre les clients SMB3 et les volumes Azure NetApp Files peut être chiffré à l’aide de l’algorithme AES-CCM sur SMB 3.0 et de l’algorithme AES-GCM sur les connexions SMB 3.1.1. Pour plus d’informations, consultez [Créer un volume SMB pour Azure NetApp Files](azure-netapp-files-create-volumes-smb.md). 

### <a name="can-the-storage-be-encrypted-at-rest"></a>Le stockage peut-il être chiffré au repos ?

Tous les volumes Azure NetApp Files sont chiffrés en suivant la norme FIPS 140-2. Toutes les clés sont gérées par le service Azure NetApp Files. 

### <a name="is-azure-netapp-files-cross-region-replication-traffic-encrypted"></a>Le trafic de réplication inter-région Azure NetApp Files est-il chiffré ?

La réplication inter-région Azure NetApp Files utilise le chiffrement TLS 1.2 AES-256 GCM pour chiffrer toutes les données transférées entre le volume source et le volume de destination. Ce chiffrement s’ajoute au [chiffrement Azure MACSec](../security/fundamentals/encryption-overview.md) qui est activé par défaut pour tout le trafic Azure, notamment la réplication inter-région Azure NetApp Files. 

### <a name="how-are-encryption-keys-managed"></a>Comment les clés de chiffrement sont-elles gérées ? 

La gestion des clés pour Azure NetApp Files est assurée par le service. Une clé de chiffrement de données XTS-AES-256 unique est générée pour chaque volume. Une hiérarchie de clés de chiffrement est utilisée pour chiffrer et protéger toutes les clés de volume. Ces clés de chiffrement ne sont jamais affichées ou indiquées dans un format non chiffré. Les clés de chiffrement sont supprimées en même temps qu'un volume.

La prise en charge des clés gérées par le client (Bring Your Own Key) à l’aide d’Azure Dedicated HSM est disponible de façon contrôlée dans les régions USA Est, USA Centre Sud, USA Ouest 2 et US Gov Virginie. Vous pouvez demander l’accès à [anffeedback@microsoft.com](mailto:anffeedback@microsoft.com) . Une fois la capacité disponible, les demandes sont approuvées.

### <a name="can-i-configure-the-nfs-export-policy-rules-to-control-access-to-the-azure-netapp-files-service-mount-target"></a>Puis-je configurer les règles de stratégie d’exportation NFS pour contrôler l’accès à la cible de montage du service Azure NetApp Files ?

Oui, vous pouvez configurer jusqu'à cinq règles dans une même stratégie d’exportation NFS.

### <a name="does-azure-netapp-files-support-network-security-groups"></a>Azure NetApp Files prend-il en charge les groupes de sécurité réseau ?

Non, actuellement vous ne pouvez pas appliquer de Groupes de sécurité réseau au sous-réseau délégué d’Azure NetApp Files ou aux interfaces réseau créées par le service.

### <a name="can-i-use-azure-rbac-with-azure-netapp-files"></a>Puis-je utiliser Azure RBAC avec Azure NetApp Files ?

Oui, Azure NetApp Files prend en charge les fonctionnalités Azure RBAC. Avec les rôles Azure intégrés, vous pouvez [créer des rôles personnalisés](../role-based-access-control/custom-roles.md) pour Azure NetApp files. 

Pour obtenir la liste complète des autorisations Azure NetApp Files, consultez les opérations de fournisseur de ressources Azure pour [`Microsoft.NetApp`](../role-based-access-control/resource-provider-operations.md#microsoftnetapp).

### <a name="are-azure-activity-logs-supported-on-azure-netapp-files"></a>Les journaux d’activité Azure sont-ils pris en charge sur Azure NetApp Files ?

Azure NetApp Files est un service natif Azure. Toutes les API PUT, POST et DELETE opérant sur Azure NetApp Files sont journalisées. Par exemple, les journaux présentent des activités indiquant, par exemple, qui a créé la capture instantanée, qui a modifié le volume, et ainsi de suite.

Pour obtenir la liste complète des opérations API, consultez [API REST Azure NetApp Files](/rest/api/netapp/).

### <a name="can-i-use-azure-policies-with-azure-netapp-files"></a>Puis-je utiliser des stratégies Azure avec Azure NetApp Files ?

Oui, vous pouvez créer des [stratégies Azure personnalisées](../governance/policy/tutorials/create-custom-policy-definition.md). 

Toutefois, vous ne pouvez pas créer de stratégies Azure (stratégies de nommage personnalisées) sur l’interface Azure NetApp Files. Consultez [Consignes pour planifier un réseau Azure NetApp Files](azure-netapp-files-network-topologies.md#considerations).

### <a name="when-i-delete-an-azure-netapp-files-volume-is-the-data-deleted-safely"></a>Lorsque je supprime un volume Azure NetApp Files, les données sont-elles supprimées en toute sécurité ? 

La suppression d’un volume Azure NetApp Files est effectuée par programmation dans le serveur principal (couche d’infrastructure physique) avec effet immédiat. L’opération de suppression comprend la suppression des clés utilisées pour chiffrer les données au repos. Il n’existe aucune option pour un scénario de récupération d’un volume supprimé une fois l’opération de suppression exécutée avec succès (via des interfaces telles que le portail Azure et l’API).

## <a name="performance-faqs"></a>Questions fréquentes relatives aux performances

### <a name="what-should-i-do-to-optimize-or-tune-azure-netapp-files-performance"></a>Que puis-je faire pour optimiser ou ajuster les performances d’Azure NetApp Files ?

Vous pouvez effectuer les actions suivantes en fonction de vos exigences en matière de performances : 
- Assurez-vous que la taille de la machine virtuelle est adaptée.
- Activez la mise en réseau accélérée pour la machine virtuelle.
- Sélectionnez un niveau de service et une taille adaptés pour le pool de capacité.
- Créez un volume avec la taille de quota de votre choix pour la capacité et les performances.

### <a name="how-do-i-convert-throughput-based-service-levels-of-azure-netapp-files-to-iops"></a>Comment convertir les niveaux de service basés sur le débit d’Azure NetApp Files en IOPS ?

Vous pouvez convertir les Mo/s en IOPS à l’aide de la formule suivante :  

`IOPS = (MBps Throughput / KB per IO) * 1024`

### <a name="how-do-i-change-the-service-level-of-a-volume"></a>Comment modifier le niveau de service d’un volume ?

Vous pouvez modifier le niveau de service d’un volume existant en déplaçant le volume vers un autre pool de capacité qui utilise le [niveau de service](azure-netapp-files-service-levels.md) souhaité pour le volume. Consultez [Changer dynamiquement le niveau de service d’un volume](dynamic-change-volume-service-level.md). 

### <a name="how-do-i-monitor-azure-netapp-files-performance"></a>Comment surveiller les performances d’Azure NetApp Files ?

Azure NetApp Files fournit des métriques de performance des volumes. Vous pouvez également utiliser Azure Monitor pour surveiller les métriques d’utilisation pour Azure NetApp Files.  Consultez [Métriques pour Azure NetApp Files](azure-netapp-files-metrics.md) pour obtenir la liste des métriques de performance pour Azure NetApp Files.

### <a name="whats-the-performance-impact-of-kerberos-on-nfsv41"></a>Quel est l’impact de Kerberos sur les performances de NFSv4.1 ?

Pour plus d’informations sur les options de sécurité pour NFSv4.1, les vecteurs de performances testés et l’impact sur les performances attendu, consultez [Impact de Kerberos sur les performances des volumes NFSv4.1](performance-impact-kerberos.md). 

### <a name="does-azure-netapp-files-support-smb-direct"></a>Azure NetApp Files prend-il en charge SMB Direct ?

Non, Azure NetApp Files ne prend pas en charge SMB Direct. 

### <a name="is-nic-teaming-supported-in-azure"></a>L’association de cartes réseau est-elle prise en charge dans Azure ?

L’association de cartes réseau n’est pas prise en charge dans Azure. Bien que plusieurs interfaces réseau soient prises en charge sur les machines virtuelles Azure, elles représentent une construction logique plutôt qu’une construction physique. Ainsi, elles ne fournissent aucune tolérance de panne.  En outre, la bande passante disponible pour une machine virtuelle Azure est calculée pour la machine elle-même, et non pour une interface réseau individuelle.

### <a name="are-jumbo-frames-supported"></a>Les trames Jumbo sont-elles prises en charge ?

Les trames Jumbo ne sont pas prises en charge avec les machines virtuelles Azure.

## <a name="nfs-faqs"></a>Questions fréquentes sur NFS

### <a name="i-want-to-have-a-volume-mounted-automatically-when-an-azure-vm-is-started-or-rebooted--how-do-i-configure-my-host-for-persistent-nfs-volumes"></a>Je veux monter un volume automatiquement lorsqu’une machine virtuelle Azure est démarrée ou redémarrée.  Comment configurer mon hôte pour les volumes NFS persistants ?

Pour qu’un volume NFS soit monté automatiquement au moment du démarrage ou du redémarrage de la machine virtuelle, ajoutez une entrée au fichier `/etc/fstab` sur l’hôte. 

Consultez [Monter ou démonter un volume pour des machines virtuelles Windows ou Linux](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md) pour plus d’informations.  

### <a name="what-nfs-version-does-azure-netapp-files-support"></a>Quelle version de NFS Azure NetApp Files prend-il en charge ?

Azure NetApp Files prend en charge NFSv3 et NFSv4.1. Vous pouvez [créer un volume](azure-netapp-files-create-volumes.md) à l’aide de l’une de ces versions NFS. 

### <a name="how-do-i-enable-root-squashing"></a>Comment activer le root squash ?

Vous pouvez spécifier si le compte racine peut accéder ou non au volume à l’aide de la stratégie d’exportation du volume. Pour plus d’informations, consultez [Configurer une stratégie d’exportation pour un volume NFS](azure-netapp-files-configure-export-policy.md).

### <a name="can-i-use-the-same-file-path-volume-creation-token-for-multiple-volumes"></a>Puis-je utiliser le même chemin d’accès de fichier (jeton de création de volume) pour plusieurs volumes ?

Oui, vous pouvez. Toutefois, le chemin d’accès du fichier doit être utilisé dans un autre abonnement ou dans une autre région.   

Par exemple, vous créez un volume appelé `vol1`. Puis, vous créez un autre volume également appelé `vol1` dans un autre pool de capacité, mais dans le même abonnement et la même région. Dans ce cas, l’utilisation du même nom de volume `vol1` génère une erreur. Pour utiliser le même chemin d’accès du fichier, l’autre nom doit se trouver dans une région ou un abonnement différent.

### <a name="when-i-try-to-access-nfs-volumes-through-a-windows-client-why-does-the-client-take-a-long-time-to-search-folders-and-subfolders"></a>Lorsque j’essaie d’accéder à des volumes NFS via un client Windows, pourquoi le client met-il beaucoup de temps à rechercher des dossiers et des sous-dossiers ?

Assurez-vous que `CaseSensitiveLookup` est activé sur le client Windows pour accélérer la recherche de dossiers et de sous-dossiers :

1. Utilisez la commande PowerShell suivante pour activer CaseSensitiveLookup :   
    `Set-NfsClientConfiguration -CaseSensitiveLookup 1`    
2. Montez le volume sur le serveur Windows.   
    Exemple :   
    `Mount -o rsize=1024 -o wsize=1024 -o mtype=hard \\10.x.x.x\testvol X:*`

### <a name="how-does-azure-netapp-files-support-nfsv41-file-locking"></a>Comment Azure NetApp Files prend-il en charge le verrouillage de fichiers NFSv 4.1 ? 

Pour les clients NFSv 4.1, Azure NetApp Files prend en charge le mécanisme de verrouillage de fichiers NFSv 4.1 qui maintient l’état de tous les verrous de fichier sous un modèle basé sur un bail. 

Selon la norme RFC 3530, Azure NetApp Files définit une période de bail unique pour l’ensemble des états conservés par un client NFS. Si le client ne renouvelle pas son bail au cours de la période définie, tous les états associés à son bail sont libérés par le serveur.  

Par exemple, si un client montant un volume cesse de répondre ou se bloque au-delà des délais d’attente, les verrous sont libérés. Le client peut renouveler son bail de manière explicite ou implicite en effectuant des opérations telles que la lecture d’un fichier.   

Une période de grâce définit une période de traitement spécial durant laquelle les clients peuvent tenter de récupérer leur état de verrouillage lors d’une récupération de serveur. Le délai d’expiration par défaut des baux est de 30 secondes, avec une période de grâce de 45 secondes. Passé ce délai, le bail du client est libéré.   

## <a name="smb-faqs"></a>Questions fréquentes sur SMB

### <a name="which-smb-versions-are-supported-by-azure-netapp-files"></a>Quelles sont les versions de SMB prises en charge par Azure NetApp Files ?

Azure NetApp Files prend en charge les versions SMB 2.1 et SMB 3.1 (qui inclut la prise en charge de SMB 3.0).    

### <a name="is-an-active-directory-connection-required-for-smb-access"></a>Est-ce qu’une connexion Active Directory est requise pour l’accès à SMB ? 

Oui, vous devez créer une connexion Active Directory avant de déployer un volume SMB. Les contrôleurs de domaine spécifiés doivent être accessibles par le sous-réseau délégué d’Azure NetApp Files pour une connexion réussie.  Consultez [Créer un volume SMB](./azure-netapp-files-create-volumes-smb.md) pour plus d’informations. 

### <a name="how-many-active-directory-connections-are-supported"></a>Combien de connexions Active Directory sont prises en charge ?

Vous ne pouvez configurer qu’une seule connexion Active Directory (AD) par abonnement et par région. Pour plus d’informations, consultez [Configuration requise pour les connexions Active Directory](create-active-directory-connections.md#requirements-for-active-directory-connections). 

Toutefois, vous pouvez mapper plusieurs comptes NetApp d’un même abonnement et d’une même région à un serveur AD commun créé dans l’un des comptes NetApp. Consultez [Mapper plusieurs comptes NetApp d’un même abonnement et d’une même région à une connexion AD](create-active-directory-connections.md#shared_ad). 

### <a name="does-azure-netapp-files-support-azure-active-directory"></a>Est-ce qu’Azure NetApp Files prend en charge Azure Active Directory ? 

[Azure Active Directory (AD) Domain Services](../active-directory-domain-services/overview.md) et [Active Directory Domain Services (AD DS)](/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview) sont tous deux pris en charge. Vous pouvez utiliser des contrôleurs de domaine Active Directory existants avec Azure NetApp Files. Les contrôleurs de domaine peuvent résider dans Azure en tant que machines virtuelles ou en local via ExpressRoute ou S2S VPN. Azure NetApp Files ne prend pas en charge la jonction AD pour [Azure Active Directory](https://azure.microsoft.com/resources/videos/azure-active-directory-overview/) pour l’instant.

Si vous utilisez Azure NetApp Files avec Azure Active Directory Domain Services, le chemin d’accès de l’unité d’organisation est `OU=AADDC Computers` lorsque vous configurez Active Directory pour votre compte NetApp.

### <a name="what-versions-of-windows-server-active-directory-are-supported"></a>Quelles versions de Windows Server Active Directory sont prises en charge ?

Azure NetApp Files prend en charge les versions Windows Server 2008r2SP1-2019 d’Active Directory Domain Services.

### <a name="im-having-issues-connecting-to-my-smb-share-what-should-i-do"></a>Je n’arrive pas à me connecter à mon partage SMB. Que dois-je faire ?

Il est recommandé de définir la tolérance maximale de synchronisation de l’horloge de l’ordinateur sur cinq minutes. Pour plus d’informations, consultez [Tolérance maximale de synchronisation de l’horloge de l’ordinateur](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj852172(v=ws.11)). 

### <a name="can-i-manage-smb-shares-sessions-and-open-files-through-computer-management-console-mmc"></a>Puis-je gérer `SMB Shares`, `Sessions` et `Open Files` via la console de gestion de l’ordinateur (MMC) ?

La gestion de `SMB Shares`, `Sessions` et `Open Files` via la console de gestion de l’ordinateur (MMC) n’est pas prise en charge pour le moment.

### <a name="how-can-i-obtain-the-ip-address-of-an-smb-volume-via-the-portal"></a>Comment puis-je obtenir l’adresse IP d’un volume SMB via le portail ?

Utilisez le lien **Affichage JSON** dans le volet de vue d’ensemble du volume et recherchez l’identificateur **startIp** sous **Propriétés** -> **mountTargets**.

### <a name="can-an-azure-netapp-files-smb-share-act-as-an-dfs-namespace-dfs-n-root"></a>Un partage SMB d’Azure NetApp Files peut-il faire office de racine pour un espace de noms DFS (DFS-N) ?

Non. Toutefois, les partages SMB d’Azure NetApp Files peuvent servir de cible de dossier pour un espace de noms DFS (DFS-N).   
Pour utiliser un partage SMB d’Azure NetApp Files comme cible de dossier DFS-N, indiquez le chemin d’accès de montage UNC (Convention d’affectation des noms) du partage SMB d’Azure NetApp Files à l’aide de la procédure [Ajouter un dossier cible de DFS](/windows-server/storage/dfs-namespaces/add-folder-targets#to-add-a-folder-target).  

### <a name="can-the-smb-share-permissions-be-changed"></a>Les autorisations de partage SMB peuvent-elles être modifiées ?   

Non, les autorisations de partage ne peuvent pas être modifiées. Toutefois, les autorisations NTFS du volume `root` peuvent être modifiées à l’aide de la procédure [Autorisations de fichier et de dossier NTFS](azure-netapp-files-create-volumes-smb.md#ntfs-file-and-folder-permissions). 

## <a name="capacity-management-faqs"></a>Questions fréquentes (FAQ) sur la gestion de la capacité

### <a name="how-do-i-monitor-usage-for-capacity-pool-and-volume-of-azure-netapp-files"></a>Comment surveiller l’utilisation du pool de capacité et du volume Azure NetApp Files ? 

Azure NetApp Files fournit des métriques de pool de capacité et de volume d’utilisation. Vous pouvez également utiliser Azure Monitor pour surveiller l’utilisation d’Azure NetApp Files. Consultez [Métriques pour Azure NetApp Files](azure-netapp-files-metrics.md) pour plus d’informations. 

### <a name="how-do-i-determine-if-a-directory-is-approaching-the-limit-size"></a>Comment déterminer si un répertoire approche de la taille limite ?

Vous pouvez utiliser la commande `stat` à partir d’un client pour voir si un répertoire approche de la [limite de taille maximale](azure-netapp-files-resource-limits.md#resource-limits) pour des métadonnées de répertoire (320 Mo).
Consultez [Limites des ressources pour Azure NetApp Files](azure-netapp-files-resource-limits.md#directory-limit) pour connaître la limite et le calcul. 

<!-- 
You can use the `stat` command from a client to see whether a directory is approaching the maximum size limit for directory metadata (320 MB).   

For a 320-MB directory, the number of blocks is 655360, with each block size being 512 bytes.  (That is, 320x1024x1024/512.)  This number translates to approximately 4 million files maximum for a 320-MB directory. However, the actual number of maximum files might be lower, depending on factors such as the number of files containing non-ASCII characters in the directory. As such, you should use the `stat` command as follows to determine whether your directory is approaching its limit.  

Examples:

```console
[makam@cycrh6rtp07 ~]$ stat bin
File: 'bin'
Size: 4096            Blocks: 8          IO Block: 65536  directory

[makam@cycrh6rtp07 ~]$ stat tmp
File: 'tmp'
Size: 12288           Blocks: 24         IO Block: 65536  directory
 
[makam@cycrh6rtp07 ~]$ stat tmp1
File: 'tmp1'
Size: 4096            Blocks: 8          IO Block: 65536  directory
```
--> 

### <a name="does-snapshot-space-count-towards-the-usable--provisioned-capacity-of-a-volume"></a>L’espace de capture instantanée est-il pris en compte dans la capacité utilisable ou approvisionnée d’un volume ?

Oui, la [capacité de capture instantanée consommée](azure-netapp-files-cost-model.md#capacity-consumption-of-snapshots) est comptabilisée dans l’espace approvisionné du volume. En cas de saturation du volume, envisagez d’effectuer les actions suivantes :

* [Redimensionnez le volume](azure-netapp-files-resize-capacity-pools-or-volumes.md).
* [Supprimez les captures instantanées plus anciennes](azure-netapp-files-manage-snapshots.md#delete-snapshots) pour libérer de l’espace dans le volume d’hébergement. 

### <a name="does-azure-netapp-files-support-auto-grow-for-volumes-or-capacity-pools"></a>Azure NetApp Files prend-il en charge la croissance automatique pour des volumes ou des pools de capacité ?

Non, les volumes et le pool de capacité d’Azure NetApp Files ne connaissent pas de croissance automatique lors de la saturation. Consultez [Modèle de coût pour Azure NetApp Files](azure-netapp-files-cost-model.md).   

Vous pouvez utiliser l’[outil pris en charge par la communauté ANFCapacityManager Logic Apps](https://github.com/ANFTechTeam/ANFCapacityManager) pour gérer les règles d’alerte basées sur la capacité. L’outil peut augmenter automatiquement la taille des volumes pour les empêcher de manquer d’espace.

### <a name="does-the-destination-volume-of-a-replication-count-towards-hard-volume-quota"></a>Le volume de destination d’une réplication est-il pris en compte pour le quota de volume en dur ?  

Non, le volume de destination d’une réplication n’est pas pris en compte pour le quota de volume en dur.

### <a name="can-i-manage-azure-netapp-files-through-azure-storage-explorer"></a>Puis-je gérer Azure NetApp Files via l’Explorateur Stockage Azure ?

Non. Azure NetApp Files n’est pas pris en charge par l’Explorateur Stockage Azure.

## <a name="data-migration-and-protection-faqs"></a>Questions fréquentes sur la migration et la protection des données

### <a name="how-do-i-migrate-data-to-azure-netapp-files"></a>Comment migrer des données vers Azure NetApp Files ?
Azure NetApp Files fournit les volumes NFS et SMB.  Vous pouvez utiliser n’importe quel outil de copie basé sur le fichier pour migrer des données vers le service. 

NetApp offre une solution SaaS, [NetApp Cloud Sync](https://cloud.netapp.com/cloud-sync-service).  La solution vous permet de répliquer les données NFS ou SMB dans des exportations NFS ou partages SMB d’Azure NetApp Files. 

Vous pouvez également utiliser un large éventail d’outils gratuits pour copier les données. Pour NFS, vous pouvez utiliser des outils de charge de travail, comme [rsync](https://rsync.samba.org/examples.html) pour copier et synchroniser les données source dans un volume Azure NetApp Files. Pour SMB, vous pouvez utiliser des charges de travail [robocopy](/windows-server/administration/windows-commands/robocopy) de la même manière.  Ces outils peuvent également répliquer les autorisations de fichier ou de dossier. 

La configuration requise pour la migration de données en local vers Azure NetApp Files est la suivante : 

- Vérifiez qu’Azure NetApp Files est disponible dans la région Azure cible.
- Validez la connectivité réseau entre la source et l’adresse IP du volume Azure NetApp Files cible. Le transfert de données entre la machine locale et le service Azure NetApp Files est pris en charge via ExpressRoute.
- Créez le volume Azure NetApp Files cible.
- Transférez les données de la source vers le volume cible à l’aide de votre outil de copie de fichiers par défaut.

### <a name="where-does-azure-netapp-files-store-customer-data"></a>Où Azure NetApp Files stocke-t-il les données client ?   

Par défaut, vos données restent dans la région dans laquelle vous déployez vos volumes Azure NetApp Files. Toutefois, vous pouvez choisir de répliquer vos données volume par volume vers les régions de destination disponibles à l’aide de la [réplication entre les régions](cross-region-replication-introduction.md).

### <a name="how-do-i-create-a-copy-of-an-azure-netapp-files-volume-in-another-azure-region"></a>Comment créer une copie d’un volume Azure NetApp Files dans une autre région Azure ?
    
Azure NetApp Files fournit les volumes NFS et SMB.  N’importe quel outil de copie basé sur le fichier peut être utilisé pour répliquer des données entre régions Azure. 

La fonctionnalité de [réplication inter-région](cross-region-replication-introduction.md) vous permet de répliquer de façon asynchrone des données à partir d’un volume Azure NetApp Files (source) d’une région vers un autre volume Azure NetApp Files (destination) d’une autre région.  En outre, vous pouvez [créer un nouveau volume à l’aide d’une capture instantanée d’un volume existant](azure-netapp-files-manage-snapshots.md#restore-a-snapshot-to-a-new-volume).

NetApp offre une solution SaaS, [NetApp Cloud Sync](https://cloud.netapp.com/cloud-sync-service).  La solution vous permet de répliquer les données NFS ou SMB dans des exportations NFS ou partages SMB d’Azure NetApp Files. 

Vous pouvez également utiliser un large éventail d’outils gratuits pour copier les données. Pour NFS, vous pouvez utiliser des outils de charge de travail, comme [rsync](https://rsync.samba.org/examples.html) pour copier et synchroniser les données source dans un volume Azure NetApp Files. Pour SMB, vous pouvez utiliser des charges de travail [robocopy](/windows-server/administration/windows-commands/robocopy) de la même manière.  Ces outils peuvent également répliquer les autorisations de fichier ou de dossier. 

La configuration requise pour la réplication d’un volume Azure NetApp Files vers une autre région Azure est la suivante : 
- Vérifiez qu’Azure NetApp Files est disponible dans la région Azure cible.
- Validez la connectivité réseau entre les réseaux virtuels dans chaque région. Actuellement, le peering global entre réseaux virtuels n’est pas pris en charge.  Vous pouvez établir la connectivité entre réseaux virtuels en les liant à un circuit ExpressRoute ou à l’aide d’une connexion S2S VPN. 
- Créez le volume Azure NetApp Files cible.
- Transférez les données de la source vers le volume cible à l’aide de votre outil de copie de fichiers par défaut.

### <a name="is-migration-with-azure-data-box-supported"></a>Est-ce que la migration avec Azure Data Box est prise en charge ?

Non. Azure Data Box ne prend pas en charge Azure NetApp Files actuellement. 

### <a name="is-migration-with-azure-importexport-service-supported"></a>Est-ce que la migration avec le service Azure Import/Export est prise en charge ?

Non. Le service Azure Import/Export ne prend pas en charge Azure NetApp Files actuellement.

## <a name="product-faqs"></a>FAQ relative au produit

### <a name="can-i-use-azure-netapp-files-nfs-or-smb-volumes-with-azure-vmware-solution-avs"></a>Puis-je utiliser des volumes NFS ou SMB d’Azure NetApp Files avec Azure VMware Solution (AVS) ?

Vous pouvez monter des volumes NFS d’Azure NetApp Files sur des machines virtuelles Linux ou des machines virtuelles Windows AVS. Vous pouvez mapper des partages SMB d’Azure NetApp Files à des machines virtuelles Windows AVS. Pour plus d’informations, consultez [Azure NetApp Files avec Azure VMware Solution]( ../azure-vmware/netapp-files-with-azure-vmware-solution.md).  

### <a name="what-regions-are-supported-for-using-azure-netapp-files-nfs-or-smb-volumes-with-azure-vmware-solution-avs"></a>Quelles sont les régions prises en charge pour l’utilisation de volumes NFS ou SMB d’Azure NetApp Files avec Azure VMware Solution (AVS) ?

L’utilisation de volumes NFS ou SMB d’Azure NetApp Files avec AVS pour les *montages de systèmes d’exploitation invités* est prise en charge dans [toutes les régions où AVS et ANF sont activés](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware,netapp).

### <a name="does-azure-netapp-files-work-with-azure-policy"></a>Azure NetApp Files fonctionne-t-il avec Azure Policy ?

Oui. Azure NetApp Files est un service interne. Il adhère pleinement aux normes du fournisseur de ressources Azure. Par conséquent, Azure NetApp Files peut être intégré à Azure Policy par le biais de *définitions de stratégie personnalisées*. Pour plus d’informations sur la façon d’implémenter des stratégies personnalisées pour Azure NetApp Files, consultez [Azure Policy désormais disponible pour Azure NetApp Files](https://techcommunity.microsoft.com/t5/azure/azure-policy-now-available-for-azure-netapp-files/m-p/2282258) sur Microsoft Tech Community. 

### <a name="which-unicode-character-encoding-is-supported-by-azure-netapp-files-for-the-creation-and-display-of-file-and-directory-names"></a>Quel codage de caractères Unicode est pris en charge par Azure NetApp Files pour la création et l’affichage des noms de fichiers et de répertoires ?   

Azure NetApp Files prend uniquement en charge les noms de fichiers et de répertoires qui sont encodés avec le format de codage de caractères Unicode UTF-8 pour les volumes NFS et SMB.

Si vous essayez de créer des fichiers ou des répertoires dont les noms utilisent des caractères supplémentaires ou des paires de substitutions, tels que des caractères non standard et des Emoji qui ne sont pas pris en charge par UTF-8, l’opération échoue. Dans ce cas, une erreur d’un client Windows peut indiquer « Le nom de fichier que vous avez spécifié n’est pas valide ou est trop long. Spécifiez un autre nom de fichier. » 

## <a name="next-steps"></a>Étapes suivantes  

- [Questions fréquentes (FAQ) sur Microsoft Azure ExpressRoute](../expressroute/expressroute-faqs.md)
- [Questions fréquentes (FAQ) sur les réseaux virtuels Microsoft Azure](../virtual-network/virtual-networks-faq.md)
- [Création d’une demande de support Azure](../azure-portal/supportability/how-to-create-azure-support-request.md)
- [Azure Data Box](../databox/index.yml)
- [Questions fréquentes (FAQ) sur les performances SMB pour Azure NetApp Files](azure-netapp-files-smb-performance.md)
