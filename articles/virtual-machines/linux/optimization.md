---
title: Optimiser votre machine virtuelle Linux sur Azure
description: Découvrez différents conseils d’optimisation qui vous permettront de configurer votre machine virtuelle Linux de manière à en optimiser les performances sur Azure.
author: rickstercdn
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 09/06/2016
ms.author: rclaus
ms.subservice: disks
ms.openlocfilehash: 1e3551834e7664d5036fa8a5e0497e5a37f61c2f
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96498504"
---
# <a name="optimize-your-linux-vm-on-azure"></a>Optimiser votre machine virtuelle Linux sur Azure
Il est simple de créer une machine virtuelle Linux à partir de la ligne de commande ou du portail. Ce didacticiel vous explique comment configurer votre machine virtuelle de manière à en optimiser les performances sur la plateforme Microsoft Azure. Dans cette rubrique, une machine virtuelle de serveur Ubuntu est utilisée, mais vous pouvez également créer des machines virtuelles Linux en utilisant vos [propres images en tant que modèles](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="prerequisites"></a>Prérequis
Cet article repose sur l’hypothèse que vous disposez déjà d’un abonnement Azure actif ([s’inscrire pour un essai gratuit](https://azure.microsoft.com/pricing/free-trial/)) et que vous avez déjà approvisionné une machine virtuelle dans votre abonnement Azure. Vérifiez que vous avez installé la dernière version [d’Azure CLI](/cli/azure/install-az-cli2) et que vous vous êtes connecté à votre abonnement Azure avec la commande [az login](/cli/azure/reference-index) avant de [créer une machine virtuelle](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-os-disk"></a>Disque de système d’exploitation Azure
Lorsque vous créez une machine virtuelle Linux dans Azure, deux disques lui sont associés. **/dev/sda** est le disque de votre système d’exploitation, et **/dev/sdb** est votre disque temporaire.  Le disque de système d’exploitation principal ( **/dev/sda**) est exclusivement destiné au système d’exploitation, car il est optimisé pour le démarrage rapide des machines virtuelles et ne délivre pas de performances adéquates pour vos charges de travail. Vous pouvez attacher un ou plusieurs disques à votre machine virtuelle afin de bénéficier d’un stockage persistant et optimisé pour vos données. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Ajout de disques selon vos objectifs de taille et de performances
Selon la taille de machine virtuelle, vous pouvez attacher jusqu’à 16 disques supplémentaires dans une machine virtuelle série A, 32 disques dans une machine série D et 64 disques dans une machine série G, chacun de ces disques pouvant avoir une taille maximale de 32 To. Ajoutez des disques en fonction de vos besoins en matière d’espace et d’E/S par seconde. Les performances ciblées de chaque disque sont 500 E/S par seconde pour un stockage Standard et jusqu’à 20 000 E/S par seconde maximum pour un stockage Premium.

Pour bénéficier des plus hauts niveaux d’E/S par seconde sur les disques de Stockage Premium dont le paramètre de cache est défini sur **ReadOnly** ou sur **None**, vous devez désactiver les **barrières** lors du montage du système de fichiers dans Linux. Ces barrières sont inutiles, car les écritures sur les disques de stockage Premium sont pérennes avec ces paramètres de cache.

* Si vous utilisez **reiserFS**, désactivez les barrières à l’aide de l’option de montage `barrier=none`. (Pour activer les barrières, utilisez `barrier=flush`.)
* Si vous utilisez **ext3/ext4**, désactivez les barrières à l’aide de l’option de montage `barrier=0`. (Pour activer les barrières, utilisez `barrier=1`.)
* Si vous utilisez **XFS**, désactivez les barrières à l’aide de l’option de montage `nobarrier`. (Pour activer les barrières, utilisez l’option `barrier`.)

## <a name="unmanaged-storage-account-considerations"></a>Considérations relatives aux comptes de stockage non gérés
L’action par défaut quand vous créez une machine virtuelle avec l’interface Azure CLI est d’utiliser Azure Disques managés.  Ces disques sont gérés par la plateforme Azure et ne nécessitent pas de préparation ou d’emplacement pour les stocker.  Les disques non managés requièrent un compte de stockage et sont associés à certaines autres considérations en matière de performances.  Pour plus d’informations sur les disques managés, consultez [Vue d’ensemble d’Azure Disques managés](../managed-disks-overview.md).  La section qui suit décrit les considérations relatives aux performances dont vous devez tenir compte uniquement si vous utilisez des disques non managés.  Une fois de plus, la solution de stockage par défaut et recommandée consiste à utiliser des disques managés.

Si vous créez une machine virtuelle avec des disques non managés, veillez à attacher les disques à partir de comptes de stockage résidant dans la même région que votre machine virtuelle afin d’assurer une proximité étroite et de minimiser la latence du réseau.  Chaque compte de stockage Standard présente une limite de 20 000 E/S par seconde et une taille maximale de 500 To.  Cette limite correspond à environ 40 disques utilisés de manière intensive, incluant le disque de système d’exploitation et tous les disques de données que vous créez. Pour les comptes de stockage Premium, il n’existe aucun nombre maximal d’E/S par seconde, mais une limite de taille de 32 To. 

Lorsque vous devez gérer des charges de travail à E/S par seconde élevées et que vous avez choisi le stockage Standard pour vos disques, vous pouvez avoir besoin de fractionner les disques entre plusieurs comptes de stockage pour ne pas risquer d’atteindre la limite de 20 000 E/S par seconde propre aux comptes de stockage Standard. Votre machine virtuelle peut contenir une combinaison de disques associés à différents comptes de stockage et types de comptes de stockage pour vous offrir la configuration optimale souhaitée.
 

## <a name="your-vm-temporary-drive"></a>Votre lecteur temporaire de machine virtuelle
Lorsque vous créez une machine virtuelle, Azure vous fournit par défaut un disque de système d’exploitation ( **/dev/sda**) et un disque temporaire ( **/dev/sdb**).  Tous les disques que vous ajoutez apparaissent sous la forme **/dev/sdc**, **/dev/sdd**, **/dev/sde**, etc. Les données figurant sur votre disque temporaire ( **/dev/sdb**) ne sont pas pérennes et risquent d’être perdues si des événements spécifiques, tels qu’un redimensionnement, un redéploiement ou une maintenance de machine virtuelle, imposent un redémarrage de votre machine virtuelle.  La taille et le type de votre disque temporaire sont liés à la taille de machine virtuelle que vous avez choisie au moment du déploiement. Pour toutes les machines virtuelles de taille Premium (séries DS, G et DS_V2), le lecteur temporaire est secondé par un disque SSD local offrant un surcroît de performances qui peut atteindre 48 000 E/S par seconde. 

## <a name="linux-swap-partition"></a>Partition d’échange Linux
Si votre machine virtuelle Azure est issue d’une image Ubuntu ou CoreOS, vous pouvez utiliser le fichier CustomData afin d’envoyer un fichier cloud-config à Cloud-init. Si vous [avez téléchargé une image Linux personnalisée](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) qui utilise cloud-init, vous configurez également les partitions d’échange à l’aide de cloud-init.

Vous ne pouvez pas utiliser le fichier **/etc/waagent.conf** pour gérer l’échange de toutes les images qui sont approvisionnées et prises en charge par cloud-init. Pour obtenir la liste complète des images, consultez [Utiliser cloud-init](using-cloud-init.md). 

Le moyen le plus simple de gérer l’échange pour ces images consiste à suivre les étapes suivantes :

1. Dans le dossier **/var/lib/cloud/scripts/per-boot**, créez un fichier nommé **create_swapfile.sh** :

   **$ sudo touch /var/lib/cloud/scripts/per-boot/create_swapfile.sh**

1. Ajoutez les lignes suivantes au fichier :

   **$ sudo vi /var/lib/cloud/scripts/per-boot/create_swapfile.sh**

   ```
   #!/bin/sh
   if [ ! -f '/mnt/swapfile' ]; then
   fallocate --length 2GiB /mnt/swapfile
   chmod 600 /mnt/swapfile
   mkswap /mnt/swapfile
   swapon /mnt/swapfile
   swapon -a ; fi
   ```

   > [!NOTE]
   > Vous pouvez modifier la valeur en fonction de vos besoins et en fonction de l’espace disponible sur votre disque de ressources, qui varie en fonction de la taille de la machine virtuelle utilisée.

1. Rendre le fichier exécutable :

   **$ sudo chmod +x /var/lib/cloud/scripts/per-boot/create_swapfile.sh**

1. Pour créer le fichier d’échange, exécutez le script juste après la dernière étape :

   **$ sudo /var/lib/cloud/scripts/per-boot/./create_swapfile.sh**

Pour les images sans prise en charge cloud-init, les images déployées à partir d’Azure Marketplace présentent un agent Linux de machine virtuelle intégré au système d’exploitation. Cet agent permet à la machine virtuelle d’interagir avec divers services Azure. Si vous ayez déployé une image standard à partir d’Azure Marketplace, vous devez suivre la procédure ci-après pour configurer les paramètres de votre fichier d’échange Linux de manière adéquate :

Vous devez rechercher et modifier deux entrées spécifiques dans le fichier **/etc/waagent.conf** . Ces entrées contrôlent l’existence d’un fichier d’échange dédié et la taille de ce fichier. Les paramètres à vérifier sont `ResourceDisk.EnableSwap` et `ResourceDisk.SwapSizeMB`. 

Pour activer un disque correctement activé et un fichier d'échange monté, vérifiez que les paramètres sont définis comme suit :

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={taille en Mo en fonction de vos besoins} 

Une fois que vous avez modifié ces paramètres, vous devez redémarrer waagent ou votre machine virtuelle Linux pour que ces modifications soient prises en compte.  Vous pouvez vérifier que ces modifications ont été implémentées et qu’un fichier d’échange a été créé en utilisant la commande `free` pour visualiser l’espace disponible. L’exemple ci-après correspond à la création d’un fichier d’échange de 512 Mo suite à la modification du fichier **waagent.conf** :

```bash
azuseruser@myVM:~$ free
            total       used       free     shared    buffers     cached
Mem:       3525156     804168    2720988        408       8428     633192
-/+ buffers/cache:     162548    3362608
Swap:       524284          0     524284
```

## <a name="io-scheduling-algorithm-for-premium-storage"></a>Algorithme de planification d’E/S pour le stockage Premium
Avec le noyau Linux 2.6.18, l’algorithme de planification d’E/S par défaut est passé de Deadline à CFQ (algorithme de file d’attente complètement équitable). Pour les modèles d’E/S à accès aléatoire, les différences de performances entre CFQ et Deadline sont minimes.  Pour les disques SSD qui présentent un modèle d’E/S de disque principalement séquentiel, le retour à l’algorithme NOOP ou Deadline peut contribuer à améliorer les performances d’E/S.

### <a name="view-the-current-io-scheduler"></a>Visualiser le planificateur d’E/S actuel
Utilisez la commande suivante :  

```bash
cat /sys/block/sda/queue/scheduler
```

Vous obtenez la sortie ci-après, indiquant le planificateur actuel.  

```bash
noop [deadline] cfq
```

### <a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Changement du dispositif actuel (/dev/sda) de l’algorithme de planification d’E/S
Utilisez les commandes suivantes :  

```bash
azureuser@myVM:~$ sudo su -
root@myVM:~# echo "noop" >/sys/block/sda/queue/scheduler
root@myVM:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
root@myVM:~# update-grub
```

> [!NOTE]
> L’application de ce paramètre uniquement pour le disque **/dev/sda** n’a pas d’utilité. Définissez-le sur tous les disques de données présentant un modèle d’E/S majoritairement séquentiel.  

Vous devriez obtenir la sortie ci-après, qui indique que **grub.cfg** a été régénéré avec succès et que le planificateur par défaut a été mis à jour vers NOOP.  

```bash
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.13.0-34-generic
Found initrd image: /boot/initrd.img-3.13.0-34-generic
Found linux image: /boot/vmlinuz-3.13.0-32-generic
Found initrd image: /boot/initrd.img-3.13.0-32-generic
Found memtest86+ image: /memtest86+.elf
Found memtest86+ image: /memtest86+.bin
done
```

Pour la famille de distribution Red Hat, la commande suivante suffit :   

```bash
echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local
```

Ubuntu 18.04 avec le noyau optimisé pour Azure utilise des planificateurs d’E/S à plusieurs files d’attente. Dans ce scénario, `none` est la sélection appropriée au lieu de `noop`. Pour plus d’informations, consultez [Planificateurs d’E/S Ubuntu](https://wiki.ubuntu.com/Kernel/Reference/IOSchedulers).

## <a name="using-software-raid-to-achieve-higher-iops"></a>Utilisation d’un RAID logiciel pour optimiser le nombre d’E/S par seconde
Si vos charges de travail nécessitent un niveau d’E/S par seconde supérieur à celui fourni par un seul disque, vous devez utiliser une configuration de RAID logiciel composée de plusieurs disques. Étant donné qu’Azure assure déjà la résilience de disque au niveau de la couche de structure locale, une configuration d’entrelacement RAID-0 vous offre un niveau de performances optimal.  Approvisionnez et créez des disques dans l’environnement Azure et attachez-les à votre machine virtuelle Linux avant de procéder au partitionnement, au formatage et au montage des lecteurs.  Pour plus d’informations sur la configuration d’un RAID logiciel sur votre machine virtuelle Linux dans Azure, consultez le document **[Configuration d’un RAID logiciel sur Linux](/previous-versions/azure/virtual-machines/linux/configure-raid?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)** .

En guise d’alternative à une configuration RAID traditionnelle, vous pouvez également choisir d’installer Logical Volume Manager (LVM) afin de configurer un certain nombre de disques physiques en un seul volume de stockage logique agrégé par bandes. Dans cette configuration, les lectures et les écritures sont réparties sur plusieurs disques contenus dans le groupe de volumes (similaire à RAID 0). Pour obtenir des performances optimales, il est recommandé d’agréger les volumes logiques afin que les lectures et écritures utilisent tous les disques de données joints.  Pour plus d’informations sur la configuration d’un volume logique agrégé par bandes sur votre machine virtuelle Linux dans Azure, consultez le document **[Configurer LVM sur une machine virtuelle Linux dans Azure](/previous-versions/azure/virtual-machines/linux/configure-lvm?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)** .

## <a name="next-steps"></a>Étapes suivantes
Comme dans toute procédure d’optimisation, n’oubliez pas que vous devez effectuer des tests avant et après chaque modification afin d’en évaluer l’impact.  L’optimisation est un processus graduel dont les résultats varient d’une machine à l’autre dans votre environnement.  Ce qui fonctionne bien pour une configuration donnée n’offre pas nécessairement de bons résultats pour d’autres.

Voici quelques liens utiles vous permettant d’accéder à des ressources supplémentaires :

* [Guide d’utilisateur de l’agent Linux Azure](../extensions/agent-linux.md)
* [Configuration d’un RAID logiciel sur Linux](/previous-versions/azure/virtual-machines/linux/configure-raid)