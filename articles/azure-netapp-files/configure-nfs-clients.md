---
title: Configurer un client NFS pour Azure NetApp Files | Microsoft Docs
description: Explique comment configurer des clients NFS à utiliser avec Azure NetApp Files.
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
ms.date: 06/17/2021
ms.author: b-juche
ms.openlocfilehash: 44a1dd3e5d95e8ab31c9a7716f5026ceb429e084
ms.sourcegitcommit: 91fdedcb190c0753180be8dc7db4b1d6da9854a1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2021
ms.locfileid: "112287890"
---
# <a name="configure-an-nfs-client-for-azure-netapp-files"></a>Configurer un client NFS pour Azure NetApp Files

La configuration du client NFS décrite dans cet article fait partie de l’installation lorsque vous [configurez le chiffrement NFSv4.1 Kerberos](configure-kerberos-encryption.md) ou [créez un volume à double protocole](create-volumes-dual-protocol.md). De nombreuses distributions Linux peuvent être utilisées avec Azure NetApp Files. Cet article décrit les configurations pour deux des environnements les plus couramment utilisés : RHEL 8 et Ubuntu 18.04. 

## <a name="requirements-and-considerations"></a>Conditions requises et éléments à prendre en compte  

Quelle que soit la version de Linux que vous utilisez, les configurations suivantes sont nécessaires :

* Configurez un client NTP pour éviter tout problème d’asymétrie temporelle.
* Configurez les entrées DNS du client Linux pour la résolution de noms.  
    Cette configuration doit inclure l’enregistrement « A » (direct) et l’enregistrement PTR (inversé). 
* Pour la jonction de domaine, créez un compte d’ordinateur pour le client Linux dans le domaine Active Directory cible (créé avec la commande de jonction de domaine). 
    > [!NOTE] 
    > La variable `$SERVICEACCOUNT` utilisée dans les commandes ci-dessous doit être un compte d’utilisateur disposant d’autorisations ou d’une délégation pour créer un compte d’ordinateur dans l’unité d’organisation ciblée.

## <a name="rhel-8-configuration"></a>Configuration pour RHEL 8 

Cette section décrit les configurations RHEL requises pour le chiffrement Kerberos NFSv4.1 et le protocole double.  

Les exemples de cette section utilisent le nom de domaine et l’adresse IP suivants :  

* Nom de domaine : `contoso.com`
* IP privée : `10.6.1.4`

### <a name="rhel-8-configuration-if-you-are-using-nfsv41-kerberos-encryption"></a><a name="rhel8_nfsv41_kerberos"></a>Configuration de RHEL 8 si vous utilisez le chiffrement Kerberos NFSv4.1

1. Configurez `/etc/resolv.conf` avec le serveur DNS approprié.  

    Par exemple :  

    `[root@reddoc cbs]# cat /etc/resolv.conf`   
    `search contoso.com`   
    `nameserver 10.6.1.4(private IP)`   

2. Ajoutez l’enregistrement client NFS dans le serveur DNS pour la zone de recherche directe et inversée DNS.

3. Pour vérifier DNS, utilisez les commandes suivantes à partir du client NFS :   

    `# nslookup [hostname/FQDN of NFS client(s)]`   
    `# nslookup [IP address of NFS client(s)]`

4. Installez les packages :   

    `yum update`   
    `sudo yum -y install realmd sssd adcli samba-common krb5-workstation chrony nfs-utils`

5.  Configurez le client NTP.  

    RHEL 8 utilise Chrony par défaut. Suivez les instructions de configuration de la page [Using the `Chrony` suite to configure NTP (Utilisation de la suite Chrony pour configurer NTP)](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/using-chrony-to-configure-ntp).

6.  Joignez le domaine Active Directory :  

    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou="OU=$YOUROU"`

    Par exemple : 

    `sudo realm join CONTOSO.COM -U ad_admin --computer-ou="CN=Computers"`
    
    Assurez-vous que `default_realm` est défini sur le domaine indiqué dans `/etc/krb5.conf`.  Si ce n’est pas le cas, ajoutez-le sous la section `[libdefaults]` du fichier, comme indiqué dans l’exemple suivant :
    
    ```
    [libdefaults]
        default_realm = CONTOSO.COM
        default_tkt_enctypes = aes256-cts-hmac-sha1-96
        default_tgs_enctypes = aes256-cts-hmac-sha1-96
        permitted_enctypes = aes256-cts-hmac-sha1-96
    [realms]
        CONTOSO.COM = {
            kdc = dc01.contoso.com
            admin_server = dc01.contoso.com
            master_kdc = dc01.contoso.com
            default_domain = contoso.com
        }
    [domain_realm]
        .contoso.com = CONTOSO.COM
        contoso.com = CONTOSO.COM
    [logging]
        kdc = SYSLOG:INFO
        admin_server = FILE=/var/kadm5.log
    ```

7. Redémarrez tous les services NFS :  
 
    `systemctl start nfs-*`   
    `systemctl restart rpc-gssd.service`

    Le redémarrage empêche la condition d’erreur `“mount.nfs: an incorrect mount option was specified”` pendant le montage de Kerberos.

8. Exécutez la commande `kinit` avec le compte d’utilisateur pour recevoir des tickets : 
 
    `sudo kinit $SERVICEACCOUNT@DOMAIN`  

    Par exemple :   

    `sudo kinit ad_admin@CONTOSO.COM`


### <a name="rhel-8-configuration-if-you-are-using-dual-protocol"></a>Configuration de RHEL 8 si vous utilisez le protocole double

Les étapes suivantes sont facultatives. Vous devez effectuer les étapes uniquement si vous utilisez le mappage d’utilisateur au niveau du client NFS : 

1. Effectuez toutes les étapes décrites dans la section [Configuration de RHEL 8 si vous utilisez le chiffrement Kerberos NFSv4.1](#rhel8_nfsv41_kerberos).   

2. Ajoutez un enregistrement DNS statique dans votre fichier /etc/hosts pour utiliser le nom de domaine complet (FQDN) de votre AD au lieu d’utiliser l’adresse IP du fichier config SSSD :  

    `cat /etc/hosts`   
    `10.6.1.4 winad2016.contoso.com`

3. Ajoutez une section supplémentaire pour les domaines afin de résoudre les identificateurs à partir du serveur LDAP AD :    

    `[root@reddoc cbs]# cat /etc/sssd/sssd.conf`   
    `[sssd]`   
    `domains = contoso.com, contoso-ldap (new entry added for LDAP as id_provider)`   
    `config_file_version = 2`   
    `services = nss, pam, ssh, sudo (ensure nss is present in this list)`   
 
    `[domain/contoso-ldap] (Copy the following lines. Modify as per your domain name.)`   
    `auth_provider = krb5`   
    `chpass_provider = krb5`   
    `id_provider = ldap`   
    `ldap_search_base = dc=contoso,dc=com(your domain)`   
    `ldap_schema = rfc2307bis`   
    `ldap_sasl_mech = GSSAPI`   
    `ldap_user_object_class = user`   
    `ldap_group_object_class = group`   
    `ldap_user_home_directory = unixHomeDirectory`   
    `ldap_user_principal = userPrincipalName`   
    `ldap_account_expire_policy = ad`   
    `ldap_force_upper_case_realm = true`   
    `ldap_user_search_base = cn=Users,dc=contoso,dc=com (based on your domain)`   
    `ldap_group_search_base = cn=Users,dc=contoso,dc=com (based on your domain)`   
    `ldap_sasl_authid = REDDOC$ (ensure $ at the end you can get this from “klist -kte” command)`   
    `krb5_server = winad2016.contoso.com (same as AD address which is added in /etc/hosts)`   
    `krb5_realm = CONTOSO.COM (domain name in caps)`   
    `krb5_kpasswd = winad2016.contoso.com (same as AD address which is added in /etc/hosts)`   
    `use_fully_qualified_names = false`   
 
    `[domain/contoso.com]  (Do not edit or remove any of the following information. This information is automatically generated during the realm join process.)`   
    `ad_domain = contoso.com`   
    `krb5_realm = CONTOSO.COM`   
    `realmd_tags = manages-system joined-with-adcli`   
    `cache_credentials = True`   
    `id_provider = ad`   
    `krb5_store_password_if_offline = True`   
    `default_shell = /bin/bash`   
    `ldap_id_mapping = True`   
    `use_fully_qualified_names = True`   
    `fallback_homedir = /home/%u@%d`   
    `access_provider = ad`   

4. Assurez-vous que votre propriété `/etc/nsswitch.conf` possède l’entrée `sss` :   

    `cat /etc/nsswitch.conf`   
    `passwd: sss files systemd`   
    `group: sss files systemd`   
    `netgroup: sss files`   

5. Redémarrez le service `sssd` et effacez le cache :   

    `service sssd stop`   
    `rm -f /var/lib/sss/db/*`   
    `service sssd start`   
 
6. Effectuez un test pour vous assurer que votre client est intégré au serveur LDAP :   

    `[root@red81 cbs]# id ldapuser1`   
    `uid=1234(ldapuser1) gid=1111(ldapgroup1) groups=1111(ldapgroup1)`   

## <a name="ubuntu-configuration"></a>Configuration pour Ubuntu   

Cette section décrit les configurations Ubuntu requises pour le chiffrement Kerberos NFSv4.1 et le protocole double. 

Les exemples de cette section utilisent le nom de domaine et l’adresse IP suivants :  

* Nom de domaine : `contoso.com`
* IP privée : `10.6.1.4`

1. Configurez `/etc/resolv.conf` avec le serveur DNS approprié :

    `root@ubuntu-rak:/home/cbs# cat /etc/resolv.conf`   
    `search contoso.com`   
    `nameserver <private IP address of DNS server>`   

2. Ajoutez l’enregistrement client NFS dans le serveur DNS pour la zone de recherche directe et inversée DNS.
 
    Pour vérifier DNS, utilisez les commandes suivantes à partir du client NFS :

    `# nslookup [hostname/FQDN of NFS client(s)]`   
    `# nslookup [IP address of NFS client(s)]`   

3. Installez les packages :
 
    `apt-get update`   
    `apt-get install -y realmd packagekit sssd adcli samba-common chrony krb5-user nfs-common`
    
    Quand vous y êtes invité, entrez `$DOMAIN.NAME` (en utilisant des majuscules, par exemple, `CONTOSO.COM`) comme domaine Kerberos par défaut.

4. Redémarrez le service `rpc-gssd.service` : 

    `sudo systemctl start rpc-gssd.service`

5. Ubuntu 18.04 utilise Chrony par défaut. Suivez les instructions de configuration de la page [Ubuntu Bionic : Using chrony to configure NTP](https://ubuntu.com/blog/ubuntu-bionic-using-chrony-to-configure-ntp) (Utilisation de Chrony pour configurer NTP).

6. Joignez le domaine Active Directory :   
 
    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou="OU=$YOUROU"`
 
    Par exemple :    
    `sudo realm join CONTOSO.COM -U ad_admin --computer-ou="CN=Computers"`

7. Exécutez la commande `kinit` avec l’utilisateur pour recevoir des tickets : 
 
    `sudo kinit $SERVICEACCOUNT`   
 
    Par exemple :    
    `sudo kinit ad_admin`  

### <a name="ubuntu-configuration-if-you-are-using-dual-protocol"></a>Configuration d’Ubuntu si vous utilisez le protocole double  

Les étapes suivantes sont facultatives.  Vous devez effectuer les étapes uniquement si vous souhaitez utiliser le mappage d’utilisateur au niveau du client NFS :  

1. Pour mettre à niveau les packages installés, exécutez la commande suivante :   
    `sudo apt update && sudo apt install libnss-ldap libpam-ldap ldap-utils nscd`

    L’exemple suivant utilise des valeurs d’exemple. Lorsque la commande vous invite à entrer des données, vous devez fournir une entrée en fonction de votre environnement. 

    `base dc=contoso,dc=com uri ldap://10.20.0.4:389/ ldap_version 3 rootbinddn cn=admin,cn=Users,dc=contoso,dc=com pam_password ad`   

2. Vérifiez que votre fichier `/etc/nsswitch.conf` a les entrées `ldap` suivantes :   
    `passwd:    compat systemd ldap`   
    `group:     compat systemd ldap`

3. Pour redémarrer et activer le service, exécutez la commande suivante :

    `sudo systemctl restart nscd && sudo systemctl enable nscd`   

L'exemple suivant interroge le serveur LDAP AD à partir du client LDAP Ubuntu pour un utilisateur LDAP `‘hari1’` : 

`root@cbs-k8s-varun4-04:/home/cbs# getent passwd hari1`   
`hari1:*:1237:1237:hari1:/home/hari1:/bin/bash`   

## <a name="configure-two-vms-with-the-same-hostname-to-access-nfsv41-volumes"></a>Configurer deux machines virtuelles avec le même nom d’hôte pour accéder aux volumes NFSv4.1 

Cette section explique comment configurer deux machines virtuelles qui ont le même nom d’hôte pour accéder aux volumes Azure NetApp Files NFSv4.1. Cette procédure peut être utile lorsque vous exécutez un test de récupération d’urgence (DR) et que vous avez besoin d’un système de test avec le même nom d’hôte que le système de récupération d’urgence principal. Cette procédure n’est requise que si vous avez le même nom d’hôte sur deux machines virtuelles qui accèdent aux mêmes volumes Azure NetApp Files.  

NFSv4.x requiert que chaque client s’identifie auprès des serveurs avec une chaîne *unique*. L’état d’ouverture et de verrouillage d’un fichier partagé entre un client et un serveur est associé à cette identité. Pour prendre en charge la récupération d’état NFSv4.x et la migration d’état transparents, cette chaîne d’identité ne doit pas changer au cours des redémarrages client.

1. Affichez la chaîne `nfs4_unique_id` sur les clients de machines virtuelles à l’aide de la commande suivante :
    
    `# systool -v -m nfs | grep -i nfs4_unique`     
    `    nfs4_unique_id      = ""`

    Pour monter le même volume sur une machine virtuelle supplémentaire avec le même nom d’hôte, par exemple le système de récupération d’urgence, créez un `nfs4_unique_id` de sorte qu’il puisse s’identifier de façon unique auprès du service NFS Azure NetApp Files.  Cette étape permet au service de faire la distinction entre les deux machines virtuelles avec le même nom d’hôte et d’activer le montage des volumes NFSv4.1 sur les deux machines virtuelles.  

    Vous devez effectuer cette étape uniquement sur le système de récupération d’urgence de test. Pour des besoins de cohérence, vous pouvez envisager d’appliquer un paramètre unique sur chaque machine virtuelle impliquée.

2. Sur le système de récupération d’urgence de test, ajoutez la ligne suivante au fichier `nfsclient.conf`, généralement situé dans `/etc/modprobe.d/` :

    `options nfs nfs4_unique_id=uniquenfs4-1`  

    La chaîne `uniquenfs4-1` peut être n’importe quelle chaîne alphanumérique, à condition qu’elle soit unique sur les machines virtuelles à connecter au service.

    Consultez la documentation de votre distribution relative à la configuration des paramètres client NFS.

    Redémarrez la machine virtuelle pour que la modification prenne effet.

3. Sur le système de récupération d’urgence de test, vérifiez que `nfs4_unique_id` a été défini après le redémarrage de la machine virtuelle :       

    `# systool -v -m nfs | grep -i nfs4_unique`   
    `   nfs4_unique_id      = "uniquenfs4-1"`   

4. [Montez le volume NFSv4.1](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md) sur les deux machines virtuelles comme d’habitude.

    Les deux machines virtuelles avec le même nom d’hôte peuvent désormais monter et accéder au volume NFSv4.1.  

## <a name="next-steps"></a>Étapes suivantes  

* [Créer un volume NFS pour Azure NetApp Files](azure-netapp-files-create-volumes.md)
* [Créer un volume double protocole pour Azure NetApp Files](create-volumes-dual-protocol.md)
* [Monter ou démonter un volume pour des machines virtuelles Windows ou Linux](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md) 
