---
title: Tutoriel - Configurer le routage du trafic de sous-réseau avec Azure Traffic Manager
description: Ce tutoriel explique comment configurer Traffic Manager pour router le trafic des sous-réseaux des utilisateurs vers des points de terminaison spécifiques.
services: traffic-manager
documentationcenter: ''
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2021
ms.author: duau
ms.openlocfilehash: 5d97ebebe19235a54084ec551f3bbcb9e55e3022
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2021
ms.locfileid: "123425623"
---
# <a name="tutorial-direct-traffic-to-specific-endpoints-based-on-user-subnet-using-traffic-manager"></a>Tutoriel : Diriger le trafic vers des points de terminaison spécifiques en fonction du sous-réseau de l’utilisateur via Traffic Manager

Cet article explique comment configurer la méthode de routage du trafic de sous-réseau. La méthode de routage du trafic de **Sous-réseau** vous permet de mapper un ensemble de plages d’adresses IP à des points de terminaison spécifiques. Quand une requête est reçue par Traffic Manager, ce dernier inspecte l’IP source de la requête et retourne le point de terminaison associé.

Dans ce tutoriel, à l’aide du routage de sous-réseau et en fonction de l’adresse IP de la requête de l’utilisateur, le trafic est routé vers un site web interne ou un site web de production.

Dans ce tutoriel, vous allez apprendre à :

> [!div class="checklist"]
> * Créer deux machines virtuelles exécutant un site web de base sur IIS
> * Créer deux machines virtuelles de test pour afficher Traffic Manager en action
> * Configurer le nom DNS des machines virtuelles exécutant IIS
> * Créer un profil Traffic Manager pour router le trafic en fonction du sous-réseau de l’utilisateur
> * Ajouter des points de terminaison de machine virtuelle dans le profil Traffic Manager
> * Afficher Traffic Manager en action

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

## <a name="prerequisites"></a>Prérequis

Pour voir Traffic Manager en action dans ce tutoriel, vous devez déployer les éléments suivants :

- Deux sites web de base s’exécutant dans des régions Azure distinctes, **USA Est** (sert de site web interne) et **Europe Ouest** (sert de site web de production).
- Deux machines virtuelles de test pour tester Traffic Manager : une machine virtuelle dans la région **USA Est** et la seconde dans la région **Europe Ouest**.

Les machines virtuelles de test servent à illustrer la manière dont Traffic Manager route le trafic des utilisateurs vers le site web interne ou le site web de production en fonction du sous-réseau d’où provient la requête de l’utilisateur.

### <a name="sign-in-to-azure"></a>Connexion à Azure

Connectez-vous au portail Azure sur [https://portal.azure.com](https://portal.azure.com).

### <a name="create-websites"></a>Créer des sites web

Dans cette section, vous allez créer deux instances de site web qui fournissent les deux points de terminaison de service pour le profil Traffic Manager dans deux régions Azure. La création des deux sites web comprend les étapes suivantes :

1. La création de deux machines virtuelles pour l’exécution d’un site web de base, l’une située dans la région **USA Est** et l’autre dans la région **Europe Ouest**.
2. L’installation d’un serveur IIS sur chaque machine virtuelle, et la mise à jour de la page de site web par défaut qui décrit le nom de la machine virtuelle à laquelle un utilisateur est connecté lorsqu’il visite le site web.

#### <a name="create-vms-for-running-websites"></a>Créer des machines virtuelles pour exécuter des sites web

Dans cette section, vous allez créer deux machines virtuelles (*myIISVMEastUS* et *myIISVMWestEurope*) dans les régions Azure **USA Est** et **Europe Ouest**.

1. En haut à gauche du portail Azure, sélectionnez **Créer une ressource** > **Calculer** > **Windows Server 2019 Datacenter**.
2. Dans **Créer une machine virtuelle**, tapez ou sélectionnez les valeurs suivantes sous l’onglet **De base** :

   - **Abonnement** > **Groupe de ressources** : Sélectionnez **Créer un nouveau**, puis tapez **myResourceGroupTM1**.
   - **Détails de l’instance** > **Nom de la machine virtuelle** : Tapez *myIISVMEastUS*.
   - **Détails de l’instance** > **Région** :  Sélectionnez **USA Est**.
   - **Compte d’administrateur** > **Nom d’utilisateur** :  Entrez un nom d’utilisateur de votre choix.
   - **Compte d’administrateur** > **Mot de passe** :  Entrez un mot de passe de votre choix. Le mot de passe doit contenir au moins 12 caractères et satisfaire aux [exigences de complexité définies](../virtual-machines/windows/faq.yml?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm-).
   - **Règles des ports d’entrée** > **Ports d’entrée publics** : Sélectionnez **Autoriser les ports sélectionnés**.
   - **Règles des ports d’entrée** > **Sélectionner des ports d’entrée** virtual-machines/windows/faq.yml : Sélectionnez **RDP** et **HTTP** dans la zone de liste déroulante.

3. Sélectionnez l’onglet **Gestion** ou sélectionnez **Suivant : Disques**, puis **Suivant : Mise en réseau**, puis **Suivant : Gestion**. Sous **Supervision**, définissez **Diagnostics de démarrage** sur **Désactivé**.
4. Sélectionnez **Revoir + créer**.
5. Passez en revue les paramètres, puis sélectionnez **Créer**.  
6. Suivez les étapes pour créer une seconde machine virtuelle nommée *myIISVMWestEurope*, avec un **groupe de ressources** nommé *myResourceGroupTM2*, une **localisation** dans la région *Europe Ouest* et tous les autres paramètres identiques à ceux de *myIISVMEastUS*.
7. La création des machines virtuelles peut prendre plusieurs minutes. Attendez que les deux machines virtuelles aient été créées avant de passer aux étapes restantes.

#### <a name="install-iis-and-customize-the-default-web-page"></a>Installer IIS et personnaliser la page web par défaut

Dans cette section, vous allez installer le serveur IIS sur les deux machines virtuelles, *myIISVMEastUS* & *myIISVMWestEurope*, puis mettre à jour la page de site web par défaut. La page de site web personnalisée affiche le nom de la machine virtuelle à laquelle vous vous connectez quand vous visitez le site web à partir d’un navigateur web.

1. Sélectionnez **Toutes les ressources** dans le menu de gauche, puis, dans la liste de ressources, sélectionnez *myIISVMEastUS*, qui se trouve dans le groupe de ressources *myResourceGroupTM1*.
2. Dans la page **Vue d’ensemble**, sélectionnez **Se connecter**, puis dans **Se connecter à la machine virtuelle**, sélectionnez **Télécharger le fichier RDP**.
3. Ouvrez le fichier .rdp téléchargé. Si vous y êtes invité, sélectionnez **Connexion**. Entrez le nom d’utilisateur et le mot de passe spécifiés lors de la création de la machine virtuelle. Vous devrez peut-être sélectionner **Plus de choix**, puis **Utiliser un autre compte**, pour spécifier les informations d’identification que vous avez entrées lorsque vous avez créé la machine virtuelle.
4. Sélectionnez **OK**.
5. Un avertissement de certificat peut s’afficher pendant le processus de connexion. Si vous recevez l’avertissement, sélectionnez **Oui** ou **Continuer** pour poursuivre le processus de connexion.
6. Sur le bureau du serveur, accédez à **Outils d’administration Windows**>**Gestionnaire de serveur**.
7. Lancez Windows PowerShell sur la machine virtuelle *myIISVMEastUS*, puis utilisez les commandes suivantes pour installer le serveur IIS et mettre à jour le fichier htm par défaut.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # Remove default htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm

    #Add custom htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my " + $env:computername)
    ```

8. Fermez la connexion RDP établie avec la machine virtuelle *myIISVMEastUS*.
9. Répétez les étapes 1 à 6 en créant une connexion RDP avec la machine virtuelle *myIISVMWestEurope* au sein du groupe de ressources *myResourceGroupTM2* pour installer IIS et personnaliser sa page web par défaut.
10. Lancez Windows PowerShell sur la machine virtuelle *myIISVMWestEurope*, puis utilisez les commandes suivantes pour installer le serveur IIS et mettre à jour le fichier htm par défaut.

    ```powershell-interactive
    # Install IIS
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # Remove default htm file
    remove-item C:\inetpub\wwwroot\iisstart.htm

    #Add custom htm file
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from my " + $env:computername)
    ```

#### <a name="configure-dns-names-for-the-vms-running-iis"></a>Configurer les noms DNS des machines virtuelles exécutant IIS

Traffic Manager achemine le trafic utilisateur en fonction du nom DNS des points de terminaison de service. Dans cette section, vous configurez les noms DNS des serveurs IIS *myIISVMEastUS* et *myIISVMWestEurope*.

1. Sélectionnez **Toutes les ressources** dans le menu de gauche, puis, dans la liste de ressources, sélectionnez *myIISVMEastUS*, qui se trouve dans le groupe de ressources *myResourceGroupTM1*.
2. Dans la page **Vue d’ensemble**, sous **Nom DNS**, sélectionnez **Configurer**.
3. Dans la page **Configuration**, sous l’étiquette du nom DNS, ajoutez un nom unique, puis sélectionnez **Enregistrer**.
4. Répétez les étapes 1 à 3 pour la machine virtuelle nommée *myIISVMWestEurope* qui se trouve dans le groupe de ressources *myResourceGroupTM2*.

### <a name="create-test-vms"></a>Créer des machines virtuelles de test

Dans cette section, vous allez créer une machine virtuelle (*myVMEastUS* et *myVMWestEurope*) dans chaque région Azure (**USA Est** et **Europe Ouest**). Vous utiliserez ces machines virtuelles pour tester la façon dont Traffic Manager achemine le trafic utilisateur en fonction du sous-réseau de la requête d’un utilisateur.

1. En haut à gauche du portail Azure, sélectionnez **Créer une ressource** > **Calculer** > **Windows Server 2019 Datacenter**.
2. Dans **Créer une machine virtuelle**, tapez ou sélectionnez les valeurs suivantes sous l’onglet **De base** :

   - **Abonnement** > **Groupe de ressources** : Sélectionnez **myResourceGroupTM1**.
   - **Détails de l’instance** > **Nom de la machine virtuelle** : Tapez *myVMEastUS*.
   - **Détails de l’instance** > **Région** :  Sélectionnez **USA Est**.
   - **Compte d’administrateur** > **Nom d’utilisateur** :  Entrez un nom d’utilisateur de votre choix.
   - **Compte d’administrateur** > **Mot de passe** :  Entrez un mot de passe de votre choix. Le mot de passe doit contenir au moins 12 caractères et satisfaire aux [exigences de complexité définies](../virtual-machines/windows/faq.yml?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm-).
   - **Règles des ports d’entrée** > **Ports d’entrée publics** : Sélectionnez **Autoriser les ports sélectionnés**.
   - **Règles des ports d’entrée** > **Sélectionner des ports d’entrée** : Sélectionnez **RDP** dans la zone de liste déroulante.

3. Sélectionnez l’onglet **Gestion** ou sélectionnez **Suivant : Disques**, puis **Suivant : Mise en réseau**, puis **Suivant : Gestion**. Sous **Supervision**, définissez **Diagnostics de démarrage** sur **Désactivé**.
4. Sélectionnez **Revoir + créer**.
5. Passez en revue les paramètres, puis sélectionnez **Créer**.  
6. Suivez les étapes pour créer une seconde machine virtuelle nommée *myVMWestEurope*, avec un **groupe de ressources** nommé *myResourceGroupTM2*, une **localisation** dans la région *Europe Ouest* et tous les autres paramètres identiques à ceux de *myVMEastUS*.
7. La création des machines virtuelles peut prendre plusieurs minutes. Attendez que les deux machines virtuelles aient été créées avant de passer aux étapes restantes.

## <a name="create-a-traffic-manager-profile"></a>Créer un profil Traffic Manager

Créez un profil Traffic Manager qui vous permet de retourner des points de terminaison spécifiques en fonction de l’adresse IP source de la requête.

1. En haut à gauche de l’écran, sélectionnez **Créer une ressource**. Recherchez *Profil Traffic Manager*, puis sélectionnez **Créer**.
2. Dans **Créer un profil Traffic Manager**, entrez ou sélectionnez les informations suivantes. Acceptez les valeurs par défaut pour les paramètres restants, puis sélectionnez **Créer**.

    ![Créer un profil Traffic Manager](./media/tutorial-traffic-manager-subnet-routing/create-traffic-manager-profile.png)

    | Paramètre                 | Valeur                                              |
    | ---                     | ---                                                |
    | Nom                   | Ce nom doit être unique au sein de la zone trafficmanager.net et affiche le nom DNS, trafficmanager.net, qui est utilisé pour accéder à votre profil Traffic Manager.                                   |
    | Méthode de routage          | Sélectionnez la méthode de routage de **Sous-réseau**.                                       |
    | Abonnement            | Sélectionnez votre abonnement.                          |
    | Resource group          | Sélectionnez **Existant**, puis entrez *myResourceGroupTM1*. |

## <a name="add-traffic-manager-endpoints"></a>Ajouter des points de terminaison Traffic Manager

Ajoutez les deux machines virtuelles qui exécutent les serveurs IIS : *myIISVMEastUS* & *myIISVMWestEurope* pour router le trafic utilisateur en fonction du sous-réseau de la requête de l’utilisateur.

1. Dans la barre de recherche du portail, recherchez le nom du profil Traffic Manager que vous avez créé dans la section précédente et sélectionnez le profil dans les résultats affichés.
2. Dans **Profil Traffic Manager**, dans la section **Paramètres**, sélectionnez **Points de terminaison**, puis sélectionnez **Ajouter**.
3. Entrez ou sélectionnez les informations suivantes. Acceptez les valeurs par défaut pour les paramètres restants, puis sélectionnez **OK** :

    | Paramètre                 | Valeur                                              |
    | ---                     | ---                                                |
    | Type                    | Point de terminaison Azure                                   |
    | Nom           | myInternalWebSiteEndpoint                                        |
    | Type de ressource cible           | Adresse IP publique                          |
    | Ressource cible          | Sélectionnez **Choisir une adresse IP publique** pour afficher la liste des ressources pourvues d’adresses IP publiques dans le même abonnement. Dans **Ressource**, sélectionnez l’adresse IP publique nommée *myIISVMEastUS-ip*. Il s’agit de l’adresse IP publique de la machine virtuelle serveur IIS qui se trouve dans la région USA Est.|
    |  Paramètres de routage de sous-réseau    |   Ajoutez l’adresse IP de l’outil de résolution DNS récursif utilisé par la machine virtuelle de test *myVMEastUS*. Toute requête utilisateur provenant de cette machine virtuelle est dirigée vers *myInternalWebSiteEndpoint*.    |

4. Répétez les étapes 2 et 3 pour ajouter un autre point de terminaison nommé *myProdWebsiteEndpoint* pour l’adresse IP publique *myIISVMWestEurope-ip* qui est associée à la machine virtuelle serveur IIS nommée *myIISVMWestEurope*. Pour les **paramètres de routage de sous-réseau**, ajoutez l’adresse IP de l’outil de résolution DNS récursif utilisé par la machine virtuelle de test *myVMWestEurope*. Les requêtes utilisateur qui proviennent de cette machine virtuelle de test en passant par son outil de résolution DNS sont routées vers le point de terminaison *myProdWebsiteEndpoint*.
5. Une fois l’ajout des deux points de terminaison terminé, ceux-ci s’affichent dans **Profil Traffic Manager** avec leur état de surveillance défini sur **En ligne**.

## <a name="test-traffic-manager-profile"></a>Tester le profil Traffic Manager

Dans cette section, vous testez la manière dont Traffic Manager route le trafic utilisateur d’un sous-réseau donné vers un point de terminaison spécifique. Pour afficher Traffic Manager en action, procédez comme suit :

1. Déterminez le nom DNS de votre profil Traffic Manager.
2. Affichez Traffic Manager en action comme suit :
    - À partir de la machine virtuelle de test (*myVMEastUS*) qui se trouve dans la région **USA Est**, dans un navigateur web, accédez au nom DNS de votre profil Traffic Manager.
    - À partir de la machine virtuelle de test (*myVMWestEurope*) située dans la région **Europe Ouest**, dans un navigateur web, accédez au nom DNS de votre profil Traffic Manager.

### <a name="determine-dns-name-of-traffic-manager-profile"></a>Déterminer le nom DNS du profil Traffic Manager

Dans ce didacticiel, par souci de simplicité, vous utilisez le nom DNS du profil Traffic Manager pour visiter les sites web.

Vous pouvez déterminer le nom DNS du profil Traffic Manager en procédant comme suit :

1. Dans la barre de recherche du portail, recherchez le nom du **profil Traffic Manager** que vous avez créé dans la section précédente. Dans les résultats affichés, sélectionnez le profil Traffic Manager.
2. Sélectionnez **Vue d’ensemble**.
3. Le **profil Traffic Manager** affiche le nom DNS de votre profil Traffic Manager nouvellement créé. Dans les déploiements en environnements de production, vous configurez un nom de domaine personnalisé pour qu’il pointe vers le nom de domaine Traffic Manager, à l’aide d’un enregistrement DNS CNAME.

### <a name="view-traffic-manager-in-action"></a>Afficher Traffic Manager en action

Dans cette section, vous pouvez voir Traffic Manager en action.

1. Sélectionnez **Toutes les ressources** dans le menu de gauche, puis, dans la liste de ressources, sélectionnez *myVMEastUS*, qui se trouve dans le groupe de ressources *myResourceGroupTM1*.
2. Dans la page **Vue d’ensemble**, sélectionnez **Se connecter**, puis dans **Se connecter à la machine virtuelle**, sélectionnez **Télécharger le fichier RDP**.
3. Ouvrez le fichier .rdp téléchargé. Si vous y êtes invité, sélectionnez **Connexion**. Entrez le nom d’utilisateur et le mot de passe spécifiés lors de la création de la machine virtuelle. Vous devrez peut-être sélectionner **Plus de choix**, puis **Utiliser un autre compte**, pour spécifier les informations d’identification que vous avez entrées lorsque vous avez créé la machine virtuelle.
4. Sélectionnez **OK**.
5. Un avertissement de certificat peut s’afficher pendant le processus de connexion. Si vous recevez l’avertissement, sélectionnez **Oui** ou **Continuer** pour poursuivre le processus de connexion.
6. Dans un navigateur web sur la machine virtuelle *myVMEastUS*, tapez le nom DNS de votre profil Traffic Manager pour afficher votre site web. Dans la mesure où l’adresse IP de la machine virtuelle *myVMEastUS* est associée au point de terminaison *myInternalWebsiteEndpoint*, le navigateur web lance le serveur de site web de test - *myIISVMEastUS*.

7. Connectez-vous à la machine virtuelle *myVMWestEurope* située dans la région **Europe Ouest** en suivant les étapes 1 à 5, puis accédez au nom de domaine du profil Traffic Manager à partir de cette machine virtuelle. Dans la mesure où l’adresse IP de la machine virtuelle *myVMWestEurope* est associée au point de terminaison *myProductionWebsiteEndpoint*, le navigateur web lance le serveur de site web de test - *myIISVMWestEurope*.

## <a name="clean-up-resources"></a>Nettoyer les ressources

Lorsque vous n’en avez plus besoin, supprimez les groupes de ressources (**ResourceGroupTM1** et **ResourceGroupTM2**). Pour ce faire, sélectionnez le groupe de ressources (**ResourceGroupTM1** ou **ResourceGroupTM2**), puis **Supprimer**.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la méthode de routage de sous-réseau, consultez :

> [!div class="nextstepaction"]
> [Méthode de routage de sous-réseau](traffic-manager-routing-methods.md#subnet)
