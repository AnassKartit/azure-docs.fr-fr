---
title: Gestion de l’agent des serveurs avec Azure Arc
description: Cet article décrit les différentes tâches de gestion à effectuer en règle générale pendant le cycle de vie de l’agent de machine connectée des serveurs avec Azure Arc.
ms.date: 10/14/2021
ms.topic: conceptual
ms.openlocfilehash: d0b4d32100a8063346d9a6f7bd2b4bd70156d868
ms.sourcegitcommit: 91915e57ee9b42a76659f6ab78916ccba517e0a5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/15/2021
ms.locfileid: "130041174"
---
# <a name="managing-and-maintaining-the-connected-machine-agent"></a>Gestion et maintenance de l’agent Connected Machine

Après le déploiement initial de l’agent de machine connectée de serveurs avec Azure Arc pour Windows ou Linux, vous devrez peut-être le reconfigurer, le mettre à niveau ou le supprimer de l’ordinateur. Vous pouvez facilement effectuer ces tâches de maintenance de routine manuellement ou automatiquement ce qui réduit les erreurs opérationnelles et les coûts.

## <a name="before-uninstalling-agent"></a>Avant de désinstaller l’agent

Avant de supprimer l’agent de machine connectée des serveurs avec Azure Arc, tenez compte des points suivants afin d’éviter des problèmes inattendus ou des coûts supplémentaires sur votre facture Azure :

* Si vous avez déployé des extensions de machine virtuelle Azure sur un serveur activé et que vous supprimez l’agent de machine connectée ou la ressource représentant le serveur avec Azure Arc dans le groupe de ressources, ces extensions continuent de s’exécuter et de fonctionner normalement.

* Si vous supprimez la ressource représentant le serveur avec Azure Arc dans le groupe de ressources, mais que vous ne désinstallez pas les extensions de machine virtuelle, lorsque vous réinscrirez la machine, vous ne pourrez pas gérer les extensions de machine virtuelle installées.

Pour les serveurs ou les machines que vous ne souhaitez plus gérer avec les serveurs avec Azure Arc, vous devez effectuer les étapes ci-dessous pour arrêter la gestion :

1. Supprimez les extensions de machine virtuelle qui sont installées sur la machine ou le serveur. Les étapes sont détaillées ci-dessous.

2. Déconnectez la machine d’Azure Arc en choisissant l’une des méthodes suivantes :

    * Exécutez la commande `azcmagent disconnect` sur la machine ou le serveur.

    * Sur le serveur Azure Arc inscrit que vous avez sélectionné dans le Portail Azure, sélectionnez **Supprimer** dans la barre supérieure.

    * Utilisez [Azure CLI](../../azure-resource-manager/management/delete-resource-group.md?tabs=azure-cli#delete-resource) ou [Azure PowerShell](../../azure-resource-manager/management/delete-resource-group.md?tabs=azure-powershell#delete-resource). Pour le paramètre`ResourceType`, utilisez `Microsoft.HybridCompute/machines`.

3. [Désinstallez l’agent](#remove-the-agent) de l’ordinateur ou du serveur en suivant les étapes décrites ci-dessous.

## <a name="renaming-a-machine"></a>Modification du nom d’une machine

Lorsque vous modifiez le nom de la machine Linux ou Windows connectée à des serveurs avec Azure Arc, le nouveau nom n’est pas reconnu automatiquement parce que le nom de la ressource dans Azure est immuable. Comme pour d’autres ressources Azure, vous devez supprimer la ressource et la recréer afin d’utiliser le nouveau nom.

Pour des serveurs avec Azure Arc, avant de renommer la machine, il est nécessaire de supprimer les extensions de machine virtuelle avant de continuer.

> [!NOTE]
> Si les extensions installées continuent de s’exécuter et d’effectuer leurs opérations normales une fois cette procédure terminée, vous ne pourrez pas les gérer. Si vous tentez de redéployer les extensions sur la machine, il se peut que vous rencontriez un comportement imprévisible.

> [!WARNING]
> Nous vous recommandons d’éviter de renommer la machine et de n’effectuer cette procédure que si cela est absolument nécessaire.

1. Vérifiez les extensions de machine virtuelle installées sur la machine et notez leur configuration à l’aide d’[Azure CLI](manage-vm-extensions-cli.md#list-extensions-installed) ou d’[Azure PowerShell](manage-vm-extensions-powershell.md#list-extensions-installed).

2. Supprimez les extensions de machine virtuelle installées du [portail Azure](manage-vm-extensions-portal.md#uninstall-extensions) à l’aide d’[Azure CLI](manage-vm-extensions-cli.md#remove-an-installed-extension) ou d’[Azure PowerShell](manage-vm-extensions-powershell.md#remove-an-installed-extension).

3. Utilisez l’outil **azcmagent** avec le paramètre [Disconnect](manage-agent.md#disconnect) pour déconnecter la machine d’Azure Arc et supprimer la ressource machine d’Azure. La déconnexion de la machine des serveurs avec Azure Arc n’a pas pour effet de supprimer l’agent de la machine connectée et vous n’avez pas besoin de supprimer l’agent dans le cadre de ce processus. Vous pouvez exécuter azcmagent manuellement quand vous êtes connecté de manière interactive, l’automatiser à l’aide du principal de service utilisé pour intégrer plusieurs agents ou utiliser un [jeton d’accès](../../active-directory/develop/access-tokens.md) de la plateforme d’identités Microsoft. Si vous n’avez pas utilisé de principal de service pour inscrire la machine auprès des serveurs avec Azure Arc, consultez l’[article](onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) suivant pour créer un principal de service.

4. Modifiez le nom d'ordinateur de la machine.

5. Réinscrivez l'agent de machine connectée auprès des serveurs avec Azure Arc. Exécutez l’outil `azcmagent` avec le paramètre [Connect](manage-agent.md#connect) pour effectuer cette étape.

6. Redéployez les extensions de machine virtuelle déployées à l’origine sur la machine à partir de serveurs avec Azure Arc. Si vous avez déployé l’agent Azure Monitor pour machines virtuelles (insights) ou l’agent Log Analytics à l’aide d’une définition de stratégie Azure, les agents sont redéployés après le [cycle d’évaluation](../../governance/policy/how-to/get-compliance-data.md#evaluation-triggers) suivant.

## <a name="upgrading-agent"></a>Mise à niveau de l’agent

Azure Connected Machine Agent est régulièrement mis à jour pour appliquer des correctifs de bogues, des améliorations de la stabilité et de nouvelles fonctionnalités. [Azure Advisor](../../advisor/advisor-overview.md) identifie les ressources qui n’utilisent pas la dernière version de l’agent de machine et vous recommande d’effectuer une mise à niveau vers la dernière version. Il vous avertit quand vous sélectionnez le serveur avec Azure Arc en présentant une bannière dans la page **Vue d’ensemble** ou quand vous accédez à Advisor par le biais du Portail Azure.

L’agent Azure Connected Machine pour Windows et Linux peut être mis à niveau vers la dernière version de façon manuelle ou automatique selon vos besoins.

Le tableau suivant décrit les méthodes prises en charge pour effectuer la mise à niveau de l’agent.

| Système d’exploitation | Méthode de mise à niveau |
|------------------|----------------|
| Windows | Manuellement<br> Windows Update |
| Ubuntu | [Apt](https://help.ubuntu.com/lts/serverguide/apt.html) |
| SUSE Linux Enterprise Server | [zypper](https://en.opensuse.org/SDB:Zypper_usage_11.3) |
| RedHat Enterprise, Amazon, CentOS Linux | [yum](https://wiki.centos.org/PackageManagement/Yum) |

### <a name="windows-agent"></a>Agent Windows

Le package de mise à jour de l’agent Connected Machine pour Windows est disponible sur :

* Microsoft Update

* [Catalogue Microsoft Update](https://www.catalog.update.microsoft.com/Home.aspx)

* [Package Windows Installer de l’agent pour Windows](https://aka.ms/AzureConnectedMachineAgent) à partir du centre de téléchargement Microsoft.

L’agent peut être mis à jour suivant différentes méthodes, ce qui permet de prendre en charge votre processus de gestion des mises à jour de logiciels. À part l’obtention depuis Microsoft Update, vous pouvez également procéder au téléchargement et à l’exécution manuels à partir de l’invite de commandes, d’un script ou d’une autre solution d’automatisation, ou à partir de l’Assistant interface utilisateur en exécutant `AzureConnectedMachine.msi`.

> [!NOTE]
> * Pour mettre à niveau l’agent, vous devez disposer d’autorisations *Administrateur*.
> * Pour mettre à niveau manuellement, vous devez tout d’abord télécharger et copier le package Installer dans un dossier sur le serveur cible, ou à partir d’un dossier réseau partagé. 

Si vous n’êtes pas familiarisé avec les options de ligne de commande pour les packages Windows Installer, passez en revue [Options de ligne de commande standard Msiexec](/windows/win32/msi/standard-installer-command-line-options) et [Options de ligne de commande Msiexec](/windows/win32/msi/command-line-options).

#### <a name="to-upgrade-using-the-setup-wizard"></a>Pour effectuer la mise à niveau à l’aide de l’Assistant d’installation

1. Connectez-vous à la machine avec un compte disposant des droits d’administration.

2. Exécutez **AzureConnectedMachineAgent.msi** pour démarrer l’Assistant Installation.

L’Assistant Installation détecte si une version précédente existe, puis effectue automatiquement une mise à niveau de l’agent. Une fois la mise à niveau terminée, l’Assistant Installation se ferme automatiquement.

#### <a name="to-upgrade-from-the-command-line"></a>Pour effectuer la mise à niveau à l’aide de la ligne de commande

1. Connectez-vous à la machine avec un compte disposant des droits d’administration.

2. Pour mettre à niveau l’agent en mode silencieux et créer un fichier journal d’installation dans le dossier `C:\Support\Logs`, exécutez la commande suivante.

    ```dos
    msiexec.exe /i AzureConnectedMachineAgent.msi /qn /l*v "C:\Support\Logs\Azcmagentupgradesetup.log"
    ```

### <a name="linux-agent"></a>Agent Linux

La mise à jour de l’agent vers la dernière version sur un ordinateur Linux implique deux commandes. Une commande pour mettre à jour l’index de package local avec la liste des derniers packages disponibles dans les référentiels, et une commande pour mettre à niveau le package local.

Vous pouvez télécharger le package d’agent le plus récent à partir du [référentiel de packages](https://packages.microsoft.com/) de Microsoft.

> [!NOTE]
> Pour mettre à niveau l’agent, vous devez disposer d’autorisations d’accès *racine* ou d’un compte avec des droits élevés utilisant Sudo.

#### <a name="upgrade-ubuntu"></a>Mettre à niveau Ubuntu

1. Pour mettre à jour l’index de package local avec les dernières modifications apportées aux référentiels, exécutez la commande suivante :

    ```bash
    sudo apt update
    ```

2. Pour mettre à niveau votre système, exécutez la commande suivante :

    ```bash
    sudo apt upgrade azcmagent
    ```

Les actions de la commande [apt](https://help.ubuntu.com/lts/serverguide/apt.html), telles que l’installation et la suppression de packages, sont consignées dans le fichier journal `/var/log/dpkg.log`.

#### <a name="upgrade-red-hatcentosamazon-linux"></a>Mettre à niveau Red Hat/CentOS/Amazon Linux

1. Pour mettre à jour l’index de package local avec les dernières modifications apportées aux référentiels, exécutez la commande suivante :

    ```bash
    sudo yum check-update
    ```

2. Pour mettre à niveau votre système, exécutez la commande suivante :

    ```bash
    sudo yum update azcmagent
    ```

Les actions de la commande [yum](https://access.redhat.com/articles/yum-cheat-sheet), telles que l’installation et la suppression de packages, sont consignées dans le fichier journal `/var/log/yum.log`. 

#### <a name="upgrade-suse-linux-enterprise"></a>Mettre à niveau SUSE Linux Enterprise

1. Pour mettre à jour l’index de package local avec les dernières modifications apportées aux référentiels, exécutez la commande suivante :

    ```bash
    sudo zypper refresh
    ```

2. Pour mettre à niveau votre système, exécutez la commande suivante :

    ```bash
    sudo zypper update azcmagent
    ```

Les actions de la commande [zypper](https://en.opensuse.org/Portal:Zypper), telles que l’installation et la suppression de packages, sont consignées dans le fichier journal `/var/log/zypper.log`.

## <a name="about-the-azcmagent-tool"></a>À propos de l’outil Azcmagent

L’outil Azcmagent (Azcmagent.exe) sert à configurer l’agent de machine connectée des serveurs avec Azure Arc pendant l’installation ou à modifier la configuration initiale de l’agent après installation. Azcmagent.exe fournit des paramètres de ligne de commande pour personnaliser l’agent et afficher son état :

* **Connect** : pour connecter la machine à Azure Arc.

* **Disconnect** : pour déconnecter la machine d’Azure Arc.

* **Show** : afficher l’état et les propriétés de configuration de l’agent (nom du groupe de ressources, ID d’abonnement, version, etc.), qui peuvent être utiles lors de la résolution d’un problème avec l’agent. Incluez le paramètre `-j` pour générer les résultats au format JSON.

* **Logs**  : crée un fichier. zip dans le répertoire actif contenant les journaux pour vous aider lors de la résolution des problèmes.

* **Version** : montre la version de l’agent Connected Machine.

* **-useStderr** : dirige l’erreur et la sortie détaillée vers stderr. Incluez le paramètre `-json` pour générer les résultats au format JSON.

* **-h ou--Help** : afficher les paramètres de ligne de commande disponibles.

    Par exemple, pour obtenir une aide détaillée sur le paramètre **Connect**, tapez `azcmagent connect -h`. 

* **-v ou--verbose** : activer la journalisation détaillée.

Vous pouvez effectuer une opération **Connect** et **Disconnect** manuellement quand vous êtes connecté de manière interactive, l’automatiser en utilisant le principal de service utilisé pour intégrer plusieurs agents ou avec un [jeton d’accès](../../active-directory/develop/access-tokens.md) de plateforme d’identité Microsoft. Si vous n’avez pas utilisé de principal de service pour inscrire la machine auprès des serveurs avec Azure Arc, consultez l’[article](onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) suivant pour créer un principal de service.

>[!NOTE]
>Vous devez disposer des autorisations d’accès *racine* sur les ordinateurs Linux pour exécuter **azcmagent**.

### <a name="connect"></a>Se connecter

Ce paramètre spécifie une ressource dans Azure Resource Manager représentant la machine créée dans Azure. La ressource se trouve dans l’abonnement et le groupe de ressources spécifiés, et les données relatives à la machine sont stockées dans la région Azure spécifiée par le paramètre `--location`. Le nom de ressource par défaut est le nom d’hôte de la machine s’il n’est pas spécifié.

Un certificat correspondant à l’identité attribuée par le système de cet machine est ensuite téléchargé, puis stocké localement. Après cette étape, Azure Connected Machine Metadata Service et le service d’agent de configuration d’invités commencent la synchronisation avec les serveurs avec Azure Arc.

Pour vous connecter à un principal de service, exécutez la commande suivante :

`azcmagent connect --service-principal-id <serviceprincipalAppID> --service-principal-secret <serviceprincipalPassword> --tenant-id <tenantID> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

Pour vous connecter à l’aide d’un jeton d’accès, exécutez la commande suivante :

`azcmagent connect --access-token <> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

Pour vous connecter à l’aide de vos informations d’identification d’ouverture de session avec privilèges élevés (interactives), exécutez la commande suivante :

`azcmagent connect --tenant-id <TenantID> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

### <a name="disconnect"></a>Déconnecter

Ce paramètre spécifie une ressource dans Azure Resource Manager, indiquant que la machine est supprimée dans Azure. Il ne supprime pas l’agent de la machine. L’agent est désinstallé séparément. Une fois la machine déconnectée, si vous souhaitez la réinscrire auprès des serveurs avec Azure Arc, utilisez la commande `azcmagent connect` afin qu’une nouvelle ressource soit créée pour elle dans Azure.

> [!NOTE]
> Si vous avez déployé une ou plusieurs extensions de machine virtuelle Azure sur votre serveur avec Azure Arc et que vous supprimez son inscription dans Azure, les extensions restent installées. Il est important de comprendre que, en fonction de l’extension installée, elle est exécutée activement. Les extensions des machines qui doivent être retirées ou qui ne sont plus gérées par des serveurs avec Azure Arc doivent d’abord être supprimées avant de supprimer leur inscription d’Azure.

Pour vous déconnecter à l’aide d’un principal du service, exécutez la commande suivante :

`azcmagent disconnect --service-principal-id <serviceprincipalAppID> --service-principal-secret <serviceprincipalPassword> --tenant-id <tenantID>`

Pour vous déconnecter à l’aide d’un jeton d’accès, exécutez la commande suivante :

`azcmagent disconnect --access-token <accessToken>`

Pour vous déconnecter à l’aide de vos informations d’identification d’ouverture de session avec privilèges élevés (interactives), exécutez la commande suivante :

`azcmagent disconnect`

## <a name="remove-the-agent"></a>Supprimer l’agent

Utilisez l’une des méthodes suivantes pour désinstaller l’agent Connected Machine Windows ou Linux de l’ordinateur. La suppression de l’agent n’entraîne pas la désinscription de la machine auprès des serveurs avec Azure Arc ni la suppression des extensions de machine virtuelle Azure installées. Pour les serveurs ou les machines que vous ne souhaitez plus gérer avec les serveurs avec Azure Arc, vous devez effectuer les étapes ci-dessous pour arrêter la gestion : 

1. Supprimez les extensions de machine virtuelle installées à partir du [portail Azure](manage-vm-extensions-portal.md#uninstall-extensions) et que vous ne souhaitez pas conserver sur l’ordinateur à l’aide [d’Azure CLI](manage-vm-extensions-cli.md#remove-an-installed-extension) ou [d’Azure PowerShell](manage-vm-extensions-powershell.md#remove-an-installed-extension).
1. Annulez l’inscription de la machine en exécutant `azcmagent disconnect` pour supprimer la ressource de serveurs avec Azure Arc dans Azure. En cas d’échec, vous pouvez supprimer manuellement la ressource dans Azure. Dans le cas contraire, si la ressource a été supprimée dans Azure, vous devez exécuter `azcmagent disconnect --force-local-only` sur le serveur pour supprimer la configuration locale.

### <a name="windows-agent"></a>Agent Windows

Les deux méthodes suivantes suppriment l’agent, mais pas le dossier *C:\Program Files\AzureConnectedMachineAgent* sur l’ordinateur.

#### <a name="uninstall-from-control-panel"></a>Désinstaller à partir du Panneau de configuration

1. Pour désinstaller l’agent Windows de la machine, procédez comme suit :

    a. Connectez-vous à la machine avec un compte disposant des autorisations d’administration.  
    b. Dans **Panneau de configuration**, sélectionnez **Programmes et fonctionnalités**.  
    c. Dans **Programmes et fonctionnalités**, sélectionnez **Agent Azure Connected Machine Agent**, **Désinstaller**, puis **Oui**.  

    >[!NOTE]
    > Vous pouvez également exécuter l’Assistant Installation de l’agent en double-cliquant sur le package d’installation **AzureConnectedMachineAgent.msi**.

#### <a name="uninstall-from-the-command-line"></a>Désinstaller à partir de la ligne de commande

Pour désinstaller l’agent manuellement à partir de l’invite de commandes ou pour utiliser une méthode automatisée, telle qu’un script, vous pouvez utiliser l’exemple suivant. Tout d’abord, vous devez récupérer le code du produit, c’est-à-dire un GUID qui est l’identificateur principal du package d’application, à partir du système d’exploitation. La désinstallation est effectuée à l’aide de la ligne de commande Msiexec.exe - `msiexec /x {Product Code}`.

1. Ouvrez l’Éditeur du Registre.

2. Sous la clé de Registre `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall`, recherchez et copiez le GUID du code du produit.

3. Vous pouvez alors désinstaller l’agent à l’aide de Msiexec en utilisant les exemples suivants :

   * À partir du type de la ligne de commande :

       ```dos
       msiexec.exe /x {product code GUID} /qn
       ```

   * Vous pouvez effectuer les mêmes étapes en utilisant PowerShell :

       ```powershell
       Get-ChildItem -Path HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall | `
       Get-ItemProperty | `
       Where-Object {$_.DisplayName -eq "Azure Connected Machine Agent"} | `
       ForEach-Object {MsiExec.exe /x "$($_.PsChildName)" /qn}
       ```

### <a name="linux-agent"></a>Agent Linux

> [!NOTE]
> Pour désinstaller l’agent, vous devez disposer d’autorisations d’accès *racine* ou d’un compte avec des droits élevés utilisant Sudo.

Pour désinstaller l’agent Linux, la commande à utiliser dépend du système d’exploitation Linux.

- Pour Ubuntu, exécutez la commande suivante :

    ```bash
    sudo apt purge azcmagent
    ```

- Pour RHEL, CentOS et Amazon Linux, exécutez la commande suivante :

    ```bash
    sudo yum remove azcmagent
    ```

- Pour SLES, exécutez la commande suivante :

    ```bash
    sudo zypper remove azcmagent
    ```

## <a name="unregister-machine"></a>Désinscrire l’ordinateur

Si vous prévoyez d’arrêter la gestion de la machine avec les services de prise en charge dans Azure, effectuez les étapes suivantes pour désinscrire la machine auprès des serveurs avec Azure Arc. Vous pouvez suivre ces étapes avant ou après la suppression de l’agent Connected Machine de la machine.

1. Ouvrez des serveurs avec Azure Arc en accédant au [Portail Azure](https://aka.ms/hybridmachineportal).

2. Sélectionnez la machine dans la liste, sélectionnez les points de suspension ( **...** ), puis sélectionnez **Supprimer**.

## <a name="update-or-remove-proxy-settings"></a>Mettre à jour ou supprimer des paramètres de proxy

Pour configurer l’agent afin qu’il communique avec le service via un serveur proxy ou pour supprimer cette configuration après le déploiement, utilisez l’une des méthodes suivantes. L’agent communique en sortie à l’aide du protocole HTTP dans le cadre de ce scénario.

> [!NOTE]
> Les serveurs avec Azure Arc ne prennent pas en charge l’utilisation d’une [passerelle Log Analytics](../../azure-monitor/agents/gateway.md) comme proxy pour l’agent Connected Machine.
>

### <a name="windows"></a>Windows

Pour définir la variable d’environnement du serveur proxy, exécutez la commande suivante :

```powershell
# If a proxy server is needed, execute these commands with the proxy URL and port.
[Environment]::SetEnvironmentVariable("https_proxy","http://{proxy-url}:{proxy-port}","Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# For the changes to take effect, the agent service needs to be restarted after the proxy environment variable is set.
Restart-Service -Name himds
```

Pour configurer l’agent afin qu’il cesse de communiquer via un serveur proxy, exécutez la commande suivante pour supprimer la variable d’environnement du serveur proxy et redémarrer le service de l’agent :

```powershell
[Environment]::SetEnvironmentVariable("https_proxy",$null,"Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# For the changes to take effect, the agent service needs to be restarted after the proxy environment variable removed.
Restart-Service -Name himds
```

### <a name="linux"></a>Linux

Pour définir le serveur proxy, exécutez la commande suivante à partir du répertoire dans lequel vous avez téléchargé le package d’installation de l’agent :

```bash
# Reconfigure the connected machine agent and set the proxy server.
bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
```

Pour configurer l’agent afin qu’il cesse de communiquer via un serveur proxy, exécutez la commande suivante pour supprimer la configuration du proxy :

```bash
sudo azcmagent_proxy remove
```

## <a name="next-steps"></a>Étapes suivantes

* Pour plus d’informations sur la résolution des problèmes, consultez le [guide Résoudre les problèmes de l’agent Connected Machine](troubleshoot-agent-onboard.md).

* Consultez le [Guide de planification et de déploiement](plan-at-scale-deployment.md) pour planifier le déploiement de serveurs avec Azure Arc à n’importe quelle échelle et implémenter la gestion et la supervision centralisées.

* Découvrez comment gérer votre machine à l’aide de [Azure Policy](../../governance/policy/overview.md), par exemple pour la [configuration invité](../../governance/policy/concepts/guest-configuration.md) des machines virtuelles, en vérifiant que la machine crée des rapports sur l’espace de travail Log Analytics prévu, activez l’analyse avec [Insights de machines virtuelles](../../azure-monitor/vm/vminsights-enable-policy.md) et bien plus encore.
