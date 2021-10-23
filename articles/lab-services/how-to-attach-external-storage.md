---
title: Utiliser le stockage de fichiers externes dans Azure Lab Services | Microsoft Docs
description: Découvrez comment configurer un labo qui utilise le stockage de fichiers externes dans Lab Services.
author: emaher
ms.topic: how-to
ms.date: 03/30/2021
ms.author: enewman
ms.openlocfilehash: f7c92e4369491a5b371c2f99fbcbdc4609b376f7
ms.sourcegitcommit: 92889674b93087ab7d573622e9587d0937233aa2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2021
ms.locfileid: "130180967"
---
# <a name="use-external-file-storage-in-lab-services"></a>Utiliser le stockage de fichiers externes dans Lab Services

Cet article aborde certaines des options de stockage de fichiers externes lorsque vous utilisez Azure Lab Services. [Azure Files](https://azure.microsoft.com/services/storage/files/) fournit des partages de fichiers entièrement managés dans le cloud, [accessibles via SMB 2.1 et SMB 3.0.](../storage/files/storage-how-to-use-files-windows.md) Un partage Azure Files peut être connecté publiquement ou en privé au sein d’un réseau virtuel. Vous pouvez également configurer le partage pour utiliser les informations d’identification à Active Directory d’un étudiant pour la connexion au partage de fichiers. Si vous êtes sur une machine Linux, vous pouvez également utiliser Azure NetApp Files avec des volumes NFS pour le stockage de fichiers externes avec Azure Lab Services.  

## <a name="which-solution-to-use"></a>Choix de la solution à utiliser

Chaque solution a des conditions requises et des capacités différentes. Le tableau suivant répertorie les points importants à prendre en compte pour chaque solution.  

|     Solution    |    À savoir    |
| -------------- | ------------------------ |
| [Partage Azure Files avec un point de terminaison public](#azure-files-share) | <ul><li>Tout le monde dispose d’un accès en lecture/écriture.</li><li>Aucun appairage de réseaux virtuels n’est requis.</li><li>Accessible à toutes les machines virtuelles, pas seulement aux machines virtuelles de labo.</li><li>Si vous utilisez Linux, les étudiants ont accès à la clé du compte de stockage.</li></ul> |
| [Partage Azure Files avec un point de terminaison privé](#azure-files-share) | <ul><li>Tout le monde dispose d’un accès en lecture/écriture.</li><li>L’appairage de réseaux virtuels est obligatoire.</li><li>Accessible uniquement aux machines virtuelles sur le même réseau (ou réseau homologue) que le compte de stockage.</li><li>Si vous utilisez Linux, les étudiants ont accès à la clé du compte de stockage.</li></ul> |
| [Azure Files avec l’autorisation basée sur l’identité](#azure-files-with-identity-based-authorization) | <ul><li>Les autorisations d’accès en lecture ou en lecture/écriture peuvent être définies pour un dossier ou un fichier.</li><li>L’appairage de réseaux virtuels est obligatoire.</li><li>Le compte de stockage doit être connecté à Active Directory.</li><li>Les machines virtuelles de labo doivent être jointes à un domaine.</li><li>Clé de compte de stockage inutilisée par les étudiants pour se connecter au partage de fichiers.</li></ul> |
| [NetApp Files avec volumes NFS](#netapp-files-with-nfs-volumes) | <ul><li>L’accès en lecture ou en lecture/écriture peut être défini pour les volumes.</li><li>Les autorisations sont définies à l’aide de l’adresse IP de la machine virtuelle d’un étudiant.</li><li>L’appairage de réseaux virtuels est obligatoire.</li><li>Vous devrez peut-être vous inscrire pour utiliser le service NetApp Files.</li><li>Linux uniquement.</li></ul>

Le coût d’utilisation du stockage externe n’est pas inclus dans le coût d’utilisation d’Azure Lab Services.  Pour plus d’informations sur la tarification, consultez [Tarifs Azure Files](https://azure.microsoft.com/pricing/details/storage/files/) et [Tarifs Azure NetApp Files](https://azure.microsoft.com/pricing/details/netapp/).

## <a name="azure-files-share"></a>Partage Azure Files

Les partages Azure Files sont accessibles à l’aide d’un point de terminaison public ou privé. Montez les partages à l’aide de la clé de compte de stockage comme mot de passe. Avec cette approche, tout le monde dispose d’un accès en lecture-écriture au partage de fichiers.

Si vous utilisez un point de terminaison public pour le partage Azure Files, il est important de se souvenir de ce qui suit :

- Le réseau virtuel pour le compte de stockage ne doit pas être homologué au compte de labo.  Le partage de fichiers peut être créé à tout moment avant la publication du modèle de machine virtuelle.
- Le partage de fichiers est accessible à partir de n’importe quelle machine si un utilisateur possède la clé de compte de stockage.  
- Les étudiants Linux peuvent voir la clé de compte de stockage. Les informations d’identification pour le montage d’un partage Azure Files sont stockées dans `{file-share-name}.cred` sur des machines virtuelles Linux et lisibles par sudo. Étant donné que les étudiants reçoivent un accès sudo par défaut dans les machines virtuelles Azure Lab Services, ils peuvent lire la clé du compte de stockage. Si le point de terminaison du compte de stockage est public, les étudiants peuvent accéder au partage de fichiers en dehors de leur machine virtuelle d’étudiant. Envisagez de faire pivoter la clé du compte de stockage après la fin de la classe et d’utiliser des partages de fichiers privés.

Si vous utilisez un point de terminaison privé pour le partage Azure Files, il est important de se souvenir de ce qui suit :

- L’accès est limité au trafic provenant du réseau privé et n’est pas accessible via l’Internet public. Seules les machines virtuelles du réseau virtuel privé, les machines virtuelles d’un réseau homologué au réseau virtuel privé ou les machines connectées à un VPN pour le réseau privé peuvent accéder au partage de fichiers.  
- Les étudiants Linux peuvent voir la clé de compte de stockage. Les informations d’identification pour le montage d’un partage Azure Files sont stockées dans `{file-share-name}.cred` sur des machines virtuelles Linux et lisibles par sudo. Étant donné que les étudiants reçoivent un accès sudo par défaut dans les machines virtuelles Azure Lab Services, ils peuvent lire la clé du compte de stockage. Envisagez de faire pivoter la clé de compte de stockage une fois la classe terminée.
- Cette approche nécessite que le réseau virtuel de partage de fichiers soit homologué au compte de labo. Le réseau virtuel pour le compte de stockage Azure doit être homologué au réseau virtuel pour le compte de labo avant la création du labo.

> [!NOTE]
> Par défaut, les partages de fichiers standard peuvent atteindre 5 Tio. Consultez [Créer un partage de fichiers Azure](../storage/files/storage-how-to-create-file-share.md) pour savoir comment créer des partages de fichiers pouvant atteindre 100 Tio.

Procédez comme suit pour créer une machine virtuelle connectée à un partage Azure Files.

1. Créez un [compte de stockage Azure](../storage/files/storage-how-to-create-file-share.md). Sur la page **Méthode de connectivité**, choisissez un **point de terminaison public** ou **privé**.
2. Si vous avez choisi la méthode privée, créez un [point de terminaison privé](../private-link/tutorial-private-endpoint-storage-portal.md) afin que les partages de fichiers soient accessibles à partir du réseau virtuel.
3. Créez un [partage de fichiers Azure](../storage/files/storage-how-to-create-file-share.md). Le partage de fichiers est accessible par le nom d’hôte public du compte de stockage en cas d’utilisation d’un point de terminaison public.  Le partage de fichiers est accessible par une adresse IP privée en cas d’utilisation d’un point de terminaison privé.  
4. Montez le partage de fichiers Azure dans le modèle de machine virtuelle :
    - [Windows](../storage/files/storage-how-to-use-files-windows.md)
    - [Linux](../storage/files/storage-how-to-use-files-linux.md). Pour éviter les problèmes de montage sur les machines virtuelles des étudiants, consultez la section [Utiliser Azure Files avec Linux](#use-azure-files-with-linux).
5. [Publiez](how-to-create-manage-template.md#publish-the-template-vm) le modèle de machine virtuelle.

> [!IMPORTANT]
> Assurez-vous que le pare-feu Windows Defender ne bloque pas la connexion SMB sortante via le port 445. Par défaut, SMB est autorisé pour les machines virtuelles Azure.

### <a name="use-azure-files-with-linux"></a>Utiliser Azure Files avec Linux

Si vous utilisez les instructions par défaut pour monter un partage de fichiers Azure, le partage de fichiers semble disparaître sur les machines virtuelles des étudiants une fois le modèle publié. Le script modifié suivant résout ce problème.  

Pour un partage de fichiers avec un point de terminaison public :
```bash
#!/bin/bash

# Assign variables values for your storage account and file share
storage_account_name=""
storage_account_key=""
fileshare_name=""

# Do not use 'mnt' for mount directory.
# Using ‘mnt’ will cause issues on student VMs.
mount_directory="prm-mnt" 

sudo mkdir /$mount_directory/$fileshare_name
if [ ! -d "/etc/smbcredentials" ]; then
    sudo mkdir /etc/smbcredentials
fi
if [ ! -f "/etc/smbcredentials/$storage_account_name.cred" ]; then
    sudo bash -c "echo ""username=$storage_account_name"" >> /etc/smbcredentials/$storage_account_name.cred"
    sudo bash -c "echo ""password=$storage_account_key"" >> /etc/smbcredentials/$storage_account_name.cred"
fi
sudo chmod 600 /etc/smbcredentials/$storage_account_name.cred

sudo bash -c "echo ""//$storage_account_name.file.core.windows.net/$fileshare_name /$mount_directory/$fileshare_name cifs nofail,vers=3.0,credentials=/etc/smbcredentials/$storage_account_name.cred,dir_mode=0777,file_mode=0777,serverino"" >> /etc/fstab"
sudo mount -t cifs //$storage_account_name.file.core.windows.net/$fileshare_name /$mount_directory/$fileshare_name -o vers=3.0,credentials=/etc/smbcredentials/$storage_account_name.cred,dir_mode=0777,file_mode=0777,serverino
```

Pour un partage de fichiers avec un point de terminaison privé :
```bash
#!/bin/bash

# Assign variables values for your storage account and file share
storage_account_name=""
storage_account_ip=""
storage_account_key=""
fileshare_name=""

# Do not use 'mnt' for mount directory.
# Using ‘mnt’ will cause issues on student VMs.
mount_directory="prm-mnt" 

sudo mkdir /$mount_directory/$fileshare_name
if [ ! -d "/etc/smbcredentials" ]; then
    sudo mkdir /etc/smbcredentials
fi
if [ ! -f "/etc/smbcredentials/$storage_account_name.cred" ]; then
    sudo bash -c "echo ""username=$storage_account_name"" >> /etc/smbcredentials/$storage_account_name.cred"
    sudo bash -c "echo ""password=$storage_account_key"" >> /etc/smbcredentials/$storage_account_name.cred"
fi
sudo chmod 600 /etc/smbcredentials/$storage_account_name.cred

sudo bash -c "echo ""//$storage_account_ip/$fileshare_name /$mount_directory/$fileshare_name cifs nofail,vers=3.0,credentials=/etc/smbcredentials/$storage_account_name.cred,dir_mode=0777,file_mode=0777,serverino"" >> /etc/fstab"
sudo mount -t cifs //$storage_account_name.file.core.windows.net/$fileshare_name /$mount_directory/$fileshare_name -o vers=3.0,credentials=/etc/smbcredentials/$storage_account_name.cred,dir_mode=0777,file_mode=0777,serverino
```

Si le modèle de machine virtuelle qui monte le partage Azure Files dans le répertoire `/mnt` est déjà publié, l’étudiant peut :

- Déplacez l’instruction à monter `/mnt` en haut du fichier `/etc/fstab`.  
- Modifier l’instruction de manière à monter `/mnt/{file-share-name}` dans un répertoire différent comme `/prm-mnt/{file-share-name}`.

Les étudiants doivent exécuter `mount -a` pour remonter les répertoires.

Pour plus d’informations générales, consultez [Utiliser Azure Files avec Linux](../storage/files/storage-how-to-use-files-linux.md).

## <a name="azure-files-with-identity-based-authorization"></a>Azure Files avec l’autorisation basée sur l’identité

Les partages Azure Files sont également accessibles à l’aide de l’authentification Active Directory si les deux conditions suivantes sont vraies :

- La machine virtuelle de l’étudiant est jointe au domaine.
- L’authentification Azure Directory est [activée sur le compte de stockage Azure](../storage/files/storage-files-active-directory-overview.md) qui héberge le partage de fichiers.  

Le lecteur réseau est monté sur la machine virtuelle à l’aide de l’identité de l’utilisateur et non de la clé du compte de stockage. Les points de terminaison publics ou privés fournissent l’accès au compte de stockage.

Gardez à l’esprit les points importants suivants :

- Vous pouvez définir des autorisations au niveau d’un répertoire ou d’un fichier.
- Vous pouvez utiliser les informations d’identification de l’utilisateur actuel pour l’authentification auprès du partage de fichiers.

Pour un point de terminaison public, le réseau virtuel pour le compte de stockage n’a pas besoin d’être homologué au compte de labo. Vous pouvez créer le partage de fichiers à tout moment avant la publication du modèle de machine virtuelle.

Pour un point de terminaison privé :

- L’accès est limité au trafic provenant du réseau privé et n’est pas accessible via l’Internet public. Seules les machines virtuelles du réseau virtuel privé, les machines virtuelles d’un réseau homologué au réseau virtuel privé ou les machines connectées à un VPN pour le réseau privé peuvent accéder au partage de fichiers.  
- Cette approche nécessite que le réseau virtuel de partage de fichiers soit homologué au compte de labo. Le réseau virtuel pour le compte de stockage Azure doit être homologué au réseau virtuel pour le compte de labo avant la création du labo.

Pour créer un partage Azure Files compatible avec l’authentification Active Directory et pour joindre des machines virtuelles de labo au domaine, procédez comme suit :

1. Créez un [compte de stockage Azure](../storage/files/storage-how-to-create-file-share.md).
2. Si vous avez choisi la méthode privée, créez un [point de terminaison privé](../private-link/tutorial-private-endpoint-storage-portal.md) afin que les partages de fichiers soient accessibles à partir du réseau virtuel. Créez une [zone DNS privée](../dns/private-dns-privatednszone.md) ou utilisez-en une existante. Les zones Azure DNS privées fournissent une résolution de noms au sein d’un réseau virtuel.
3. Créez un [partage de fichiers Azure](../storage/files/storage-how-to-create-file-share.md).
4. Suivez les étapes pour activer l’autorisation basée sur l’identité. Si vous utilisez Active Directory en local et le synchronisez avec Azure Active Directory (Azure AD), consultez [Authentification Active Directory Domain Services locale sur SMB pour les partages de fichiers Azure](../storage/files/storage-files-identity-auth-active-directory-enable.md). Si vous utilisez Azure AD uniquement, consultez [Activer l’authentification Azure Active Directory Domain Services sur Azure Files](../storage/files/storage-files-identity-auth-active-directory-domain-service-enable.md).
    >[!IMPORTANT]
    >Contactez l’équipe qui gère votre instance d’Active Directory pour vérifier que toutes les conditions préalables répertoriées dans les instructions sont remplies.
5. Affectez des rôles d’autorisation de partage SMB dans Azure. Pour plus d’informations sur les autorisations accordées à chaque rôle, consultez les [autorisations au niveau du partage](../storage/files/storage-files-identity-ad-ds-assign-permissions.md).
    - Le rôle **Contributeur avec élévation de privilèges du partage SMB de données de fichiers de stockage** doit être attribué à la personne ou au groupe qui configure des autorisations pour le contenu du partage de fichiers.
    - Le rôle **Contributeur de partage SMB de données de fichiers de stockage** doit être attribué aux étudiants qui doivent ajouter ou modifier des fichiers sur le partage de fichiers.
    - Le rôle **Lecteur de partage SMB des données de fichiers de stockage** doit être attribué aux étudiants qui doivent uniquement lire les fichiers à partir du partage de fichiers.
6. Configurer des autorisations au niveau du répertoire et/ou du fichier pour le partage de fichiers. Vous devez configurer les autorisations à partir d’un ordinateur joint à un domaine qui dispose d’un accès réseau au partage de fichiers. Pour modifier les autorisations au niveau du répertoire et/ou du fichier, montez le partage de fichiers à l’aide de la clé de stockage et non de vos informations d’identification Azure AD. Pour affecter des autorisations, utilisez la commande PowerShell [Set-Acl](/powershell/module/microsoft.powershell.security/set-acl) ou [icacls](/windows-server/administration/windows-commands/icacls) dans Windows.
7. [Homologuez le réseau virtuel](how-to-connect-peer-virtual-network.md) pour le compte de stockage sur le compte de labo.
8. [Créez le laboratoire de classe](how-to-manage-classroom-labs.md).
9. Enregistrez un script sur le modèle de machine virtuelle que les étudiants peuvent exécuter pour se connecter au lecteur réseau. Pour obtenir un exemple de script :
    1. Ouvrez le compte de stockage dans le portail Azure.
    1. Sous **Service de fichiers**, sélectionnez **Partages de fichiers**.
    1. Recherchez le partage auquel vous souhaitez vous connecter, sélectionnez le bouton des ellipses à l’extrême droite, puis choisissez **Se connecter**.
    1. Vous verrez des instructions pour Windows, Linux et macOS. Si vous utilisez Windows, définissez la **Méthode d’authentification** sur **Active Directory**.
    1. Copiez le code dans l’exemple et enregistrez-le sur le modèle de machine dans un fichier `.ps1` pour Windows ou un fichier `.sh` pour Linux.
10. Sur le modèle de machine, téléchargez et exécutez le script pour [joindre les machines des étudiants au domaine](https://github.com/Azure/azure-devtestlab/blob/master/samples/ClassroomLabs/Scripts/ActiveDirectoryJoin/README.md#usage). Le script `Join-AzLabADTemplate` [publie automatiquement le modèle de machine virtuelle](how-to-create-manage-template.md#publish-the-template-vm).  
    > [!NOTE]
    > Le modèle de machine n’est pas joint à un domaine. Pour afficher les fichiers sur le partage, les formateurs doivent utiliser une machine virtuelle d’étudiant pour eux-mêmes.
11. Les étudiants qui utilisent Windows peuvent se connecter au partage Azure Files à l’aide de l'[Explorateur de fichiers](../storage/files/storage-how-to-use-files-windows.md) avec leurs informations d’identification, une fois qu’ils ont reçu le chemin d’accès au partage de fichiers. Les étudiants peuvent également exécuter le script précédent pour se connecter au lecteur réseau. Pour les étudiants qui utilisent Linux, exécutez le script précédent.

## <a name="netapp-files-with-nfs-volumes"></a>NetApp Files avec volumes NFS

[Azure NetApp Files](https://azure.microsoft.com/services/netapp/) est un service de stockage de fichiers limité, hautes performances et de classe entreprise.

- Les stratégies d’accès peuvent être définies en fonction du volume.
- Les stratégies d’autorisation sont basées sur IP pour chaque volume.
- Si les étudiants ont besoin de leur propre volume auquel les autres étudiants n’ont pas accès, les stratégies d’autorisation doivent être attribuées après la publication du labo.
- Dans le contexte d’Azure Lab Services, seules les machines Linux sont prises en charge.
- Le réseau virtuel pour le pool de capacité Azure NetApp Files doit être homologué au réseau virtuel pour le compte de labo **avant** la création du labo.

Pour utiliser un partage Azure NetApp Files dans Azure Lab Services :

1. Pour créer un pool de capacité NetApp Files et au moins un volume NFS, consultez [Configurer Azure NetApp Files et un volume NFS](../azure-netapp-files/azure-netapp-files-quickstart-set-up-account-create-volumes.md). Pour plus d’informations sur les niveaux de service, consultez [Niveaux de service pour Azure NetApp Files](../azure-netapp-files/azure-netapp-files-service-levels.md).
2. [Homologuez le réseau virtuel](how-to-connect-peer-virtual-network.md) pour le pool de capacité NetApp Files sur le compte de labo.
3. [Créez le laboratoire de classe](how-to-manage-classroom-labs.md).
4. Sur le modèle de machine virtuelle, installez les composants nécessaires pour utiliser des partages de fichiers NFS.
    - Ubuntu :

        ```bash
        sudo apt update
        sudo apt install nfs-common
        ```

    - CentOS :

        ```bash
        sudo yum install nfs-utils
        ```

5. Sur le modèle de machine virtuelle, enregistrez le script suivant en tant que `mount_fileshare.sh` pour [monter le partage NetApp Files](../azure-netapp-files/azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md). Affectez la variable `capacity_pool_ipaddress` à l’adresse IP cible de montage pour le pool de capacité. Obtenez les instructions de montage pour le volume afin de trouver la valeur appropriée. Le script attend le chemin d’accès du volume NetApp Files. Pour vous assurer que les utilisateurs peuvent exécuter le script, exécutez `chmod u+x mount_fileshare.sh`.

    ```bash
    #!/bin/bash
    
    if [ $# -eq 0 ];  then
        echo "Must provide volume name."
        exit 1
    fi
    
    volume_name=$1
    capacity_pool_ipaddress=0.0.0.0 # IP address of capacity pool
    
    # Do not use 'mnt' for mount directory.
    # Using ‘mnt’ might cause issues on student VMs.
    mount_directory="prm-mnt" 
    
    sudo mkdir -p /$mount_directory
    sudo mkdir /$mount_directory/$folder_name
    
    sudo mount -t nfs -o rw,hard,rsize=65536,wsize=65536,vers=3,tcp $capacity_pool_ipaddress:/$volume_name /$mount_directory/$volume_name
    sudo bash -c "echo ""$capacity_pool_ipaddress:/$volume_name /$mount_directory/$volume_name nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0"" >> /etc/fstab"
    ```

6. Si tous les étudiants partagent l’accès au même volume NetApp Files, vous pouvez exécuter le script `mount_fileshare.sh` sur le modèle de machine avant la publication. Si les étudiants obtiennent chacun leur propre volume, enregistrez le script pour qu’il soit exécuté ultérieurement par l’étudiant.
7. [Publiez](how-to-create-manage-template.md#publish-the-template-vm) le modèle de machine virtuelle.
8. [Configurez la stratégie](../azure-netapp-files/azure-netapp-files-configure-export-policy.md) pour le partage de fichiers. La stratégie d’exportation peut permettre à une seule ou à plusieurs machines virtuelles d’accéder à un volume. Vous pouvez accorder un accès en lecture seule ou en lecture/écriture.
9. Les étudiants doivent démarrer leur machine virtuelle et exécuter le script pour monter le partage de fichiers. Ils n’ont à exécuter le script qu’une seule fois. La commande ressemble à ceci : `./mount_fileshare.sh myvolumename`.

## <a name="next-steps"></a>Étapes suivantes

Les étapes suivantes sont communes à la configuration de n’importe quel labo.

- [Créer et gérer un modèle](how-to-create-manage-template.md)
- [Ajout d'utilisateurs](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Définir un quota](how-to-configure-student-usage.md#set-quotas-for-users)
- [Définir un calendrier](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Envoyer par mail des liens d’inscription aux étudiants](how-to-configure-student-usage.md#send-invitations-to-users)
