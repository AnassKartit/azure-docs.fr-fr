---
title: Utilisation de la licence Microsoft Defender for Endpoint incluse dans Azure Security Center
description: Découvrez Microsoft Defender for Endpoint et déployez-le à partir d’Azure Security Center.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: how-to
ms.date: 08/16/2021
ms.author: memildin
ms.openlocfilehash: a57a94b8ee0bb91deb0b15a3da0265af15dbc3a3
ms.sourcegitcommit: c27f71f890ecba96b42d58604c556505897a34f3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2021
ms.locfileid: "129536303"
---
# <a name="protect-your-endpoints-with-security-centers-integrated-edr-solution-microsoft-defender-for-endpoint"></a>Protéger vos points de terminaison avec la solution EDR intégrée de Security Center : Microsoft Defender for Endpoint

Microsoft Defender for Endpoint est une solution holistique de sécurité des points de terminaison dans le cloud. Ses principales fonctionnalités sont les suivantes :

- Gestion et évaluation des vulnérabilités basées sur les risques 
- Réduction de la surface d’attaque
- Protection basée sur le comportement et gérée par le cloud
- Détection de point de terminaison et réponse (EDR)
- Investigation et correction automatiques
- Services de chasse gérés

> [!TIP]
> Lancé à l’origine sous le nom **Windows Defender ATP**, ce produit de détection de point de terminaison et réponse (EDR) a été renommé en 2019 en **Microsoft Defender ATP**.
>
> À l’occasion d’Ignite 2020, nous avons lancé la [suite Microsoft Defender XDR](https://www.microsoft.com/security/business/threat-protection) et ce composant EDR a été renommé **Microsoft Defender for Endpoint**.


## <a name="availability"></a>Disponibilité

| Aspect                                       | Détails                                                                                                                                                                                                                                                                               |
|----------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| État de sortie :                               | • Intégration à Defender pour point de terminaison pour Windows - Disponibilité générale<br> • Intégration à Defender pour point de terminaison pour Linux - Préversion                                                                                                                                     |
| Prix :                                     | Nécessite [Azure Defender pour les serveurs](defender-for-servers-introduction.md)                                                                                                                                                                                                           |
| Environnements pris en charge :                      | :::image type="icon" source="./media/icons/yes-icon.png"::: Machines Azure Arc exécutant Windows/Linux<br>:::image type="icon" source="./media/icons/yes-icon.png":::Machines virtuelles Azure exécutant Linux ([versions prises en charge](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux))<br>:::image type="icon" source="./media/icons/yes-icon.png"::: Machines virtuelles Azure exécutant Windows Server 2022, 2019, 2016, 2012 R2, 2008 R2 SP1, [ Windows Virtual Desktop (WVD)](../virtual-desktop/overview.md) et [Windows 10 Entreprise multisession](../virtual-desktop/windows-10-multisession-faq.yml) (anciennement Entreprise pour bureaux virtuels [EVD])<br>:::image type="icon" source="./media/icons/no-icon.png"::: Machines virtuelles Azure exécutant Windows 10 (autres que EVD ou WVD)           |
| Rôles et autorisations obligatoires :              | • Pour activer/désactiver l’intégration : **Administrateur de sécurité** ou **Propriétaire**<br>• Pour voir les alertes Defender pour point de terminaison dans Security Center : **Lecteur de sécurité**, **Lecteur**, **Contributeur du groupe de ressources**, **Propriétaire du groupe de ressources**, **Administrateur de la sécurité**, **Propriétaire de l’abonnement** ou **Contributeur de l’abonnement** |
| Clouds :                                      | :::image type="icon" source="./media/icons/yes-icon.png"::: Clouds commerciaux<br>:::image type="icon" source="./media/icons/yes-icon.png":::Azure Government<br>:::image type="icon" source="./media/icons/no-icon.png"::: Azure China 21Vianet                                                         |
|                                              |                                                                                                                                                                                                                                                                                       |

## <a name="benefits-of-integrating-microsoft-defender-for-endpoint-with-security-center"></a>Avantages de l’intégration de Microsoft Defender pour point de terminaison à Security Center

Microsoft Defender for Endpoint fournit les éléments suivants :

- **Capteurs de détection des violations avancés**. Les capteurs de Defender pour point de terminaison collectent un large éventail de signaux comportementaux sur vos machines.

- **Détection des violations basée sur des analyses dans le cloud**. Defender for Endpoint s’adapte rapidement aux menaces changeantes. Elle utilise l’analytique avancée et le Big Data. Il est amplifié par la puissance d’Intelligent Security Graph avec des signaux à travers Windows, Azure et Office pour détecter les menaces inconnues. Il fournit des alertes actionnables et vous permet de réagir rapidement.

- **Informations sur les menaces**. Defender for Endpoint génère des alertes quand il identifie les outils, les techniques et les procédures de l’attaquant. Il utilise les données générées par les chasseurs de menaces de Microsoft et les équipes de sécurité, complétées par les renseignements fournis par les partenaires.

En intégrant Defender pour point de terminaison à Security Center, vous bénéficierez des capacités supplémentaires suivantes :

- **Intégration automatisée**. Security Center active automatiquement le capteur Defender pour point de terminaison sur toutes les machines prises en charge connectées à Security Center.

- **Volet unique**. Les pages du portail Security Center affichent les alertes Defender pour point de terminaison. Pour approfondir vos recherches, utilisez les pages du portail de Microsoft Defender for Endpoint, où vous verrez des informations supplémentaires telles que l’arborescence du processus d’alerte et le graphique d’incident. Vous pouvez également voir une chronologie détaillée de la machine, qui indique tous les comportements pour un historique pouvant s’étendre sur six mois.

    :::image type="content" source="./media/security-center-wdatp/microsoft-defender-security-center.png" alt-text="Centre de sécurité de Microsoft Defender for Endpoint" lightbox="./media/security-center-wdatp/microsoft-defender-security-center.png":::

## <a name="what-are-the-requirements-for-the-microsoft-defender-for-endpoint-tenant"></a>Quelles sont les exigences de locataire pour Microsoft Defender pour point de terminaison ?

Quand vous utilisez Security Center pour superviser vos machines, un locataire Defender pour point de terminaison est créé automatiquement.

- **Emplacement :** Les données collectées par Defender for Endpoint sont stockées dans la zone géographique du locataire tel qu’il est identifié lors de l’approvisionnement. Les données des clients, sous forme pseudonymisée, peuvent également être stockées dans des systèmes de stockage et de traitement centralisés aux États-Unis. Une fois que vous avez configuré l’emplacement, vous ne pouvez plus le modifier. Si vous disposez de votre propre licence Microsoft Defender pour point de terminaison et qu’il vous faut déplacer vos données vers un autre emplacement, [contactez le support Microsoft](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) pour réinitialiser le locataire.
- **Déplacement des abonnements :** Si vous avez déplacé votre abonnement Azure entre locataires Azure, certaines étapes préparatoires manuelles sont requises avant que Security Center déploie Defender pour point de terminaison. Pour plus d’informations, [contactez le support technique Microsoft](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).


## <a name="enable-the-microsoft-defender-for-endpoint-integration"></a>Activation de l’intégration de Microsoft Defender for Endpoint

### <a name="prerequisites"></a>Prérequis

Vérifiez que votre machine est conforme aux conditions requises pour Defender pour point de terminaison :

1. Assurez-vous que la machine est connectée à Azure et à Internet comme requis :

    - **Machines virtuelles Azure (Windows ou Linux)**  : configurez les paramètres réseau décrits dans les paramètres de configuration du proxy et de connectivité Internet : [Windows](/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet) ou [Linux](/microsoft-365/security/defender-endpoint/linux-static-proxy-configuration).

    - **Machines locales** : connectez vos machines cibles à Azure Arc comme expliqué dans [Connecter des machines hybrides à des serveurs avec Azure Arc](../azure-arc/servers/learn/quick-enable-hybrid-vm.md).

1. Activez **Azure Defender pour les serveurs**. Consultez [Démarrage rapide : Activer Azure Defender](enable-azure-defender.md).

1. Si vous avez déplacé votre abonnement entre locataires Azure, certaines étapes préparatoires manuelles sont également requises. Pour plus d’informations, [contactez le support technique Microsoft](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).




### <a name="enable-the-integration"></a>Activer l’intégration

### <a name="windows"></a>[**Windows**](#tab/windows)

1. Dans le menu de Security Center, sélectionnez **Tarification et paramètres**, puis sélectionnez l’abonnement avec les machines Windows dont vous voulez qu’elles reçoivent Defender pour point de terminaison.

1. Sélectionnez **Intégrations**.

1. Sélectionnez **Autoriser Microsoft Defender for Endpoint à accéder à mes données**, puis **Enregistrer**.

    :::image type="content" source="./media/security-center-wdatp/enable-integration-with-edr.png" alt-text="Activer l’intégration entre Azure Security Center et la solution EDR de Microsoft, Microsoft Defender for Endpoint":::

    Azure Security Center intégrera automatiquement vos machines à Microsoft Defender pour point de terminaison. L’intégration peut prendre jusqu’à 24 heures.

### <a name="linux-preview"></a>[**Linux** (préversion)](#tab/linux)

Pendant la période de préversion, vous allez déployer Defender pour point de terminaison sur vos machines Linux de deux manières, selon que vous les avez déjà déployées sur vos machines Windows :

- [Utilisateurs existants d’Azure Defender et Microsoft Defender pour point de terminaison pour Windows](#existing-users-of-azure-defender-and-microsoft-defender-for-endpoint-for-windows)
- [Nouveaux utilisateurs qui n’ont jamais activé l’intégration avec Microsoft Defender pour le point de terminaison Windows](#new-users-whove-never-enabled-the-integration-with-microsoft-defender-for-endpoint-for-windows)


### <a name="existing-users-of-azure-defender-and-microsoft-defender-for-endpoint-for-windows"></a>Utilisateurs existants d’Azure Defender et Microsoft Defender pour point de terminaison pour Windows

si vous avez déjà effectué l’intégration avec **Defender pour point de terminaison pour Windows**, vous disposez du contrôle total sur quand et comment déployer Defender pour point de terminaison sur vos machines **Linux**.

1. Dans le menu de Security Center, sélectionnez **Tarification et paramètres**, puis sélectionnez l’abonnement avec les machines Linux dont vous voulez qu’elles reçoivent Defender pour point de terminaison.

1. Sélectionnez **Intégrations**. Vous savez que l’intégration est activée, si la case à cocher **Autoriser Microsoft Defender pour le point de terminaison à accéder à mes données** est sélectionnée, comme indiqué ci-dessous :

    :::image type="content" source="./media/security-center-wdatp/integration-enabled.png" alt-text="Activer l’intégration entre Azure Security Center et la solution EDR de Microsoft, Microsoft Defender pour point de terminaison":::

    > [!NOTE]
    > Si ce n’est pas le cas, suivez les instructions fournies dans [Nouveaux utilisateurs qui n’ont jamais activé l’intégration avec Microsoft Defender pour point de terminaison pour Windows](#new-users-whove-never-enabled-the-integration-with-microsoft-defender-for-endpoint-for-windows).

    Avec cette préversion, un nouveau bouton **Activer pour les machines Linux** a été ajouté sous cette case à cocher :

    :::image type="content" source="./media/security-center-wdatp/deploy-to-linux.png" alt-text="L’aperçu présente un bouton qui permet de contrôler manuellement le moment du déploiement de Defender pour point de terminaison pour Linux":::

1. Pour ajouter vos machines Linux à l’intégration

    1. Sélectionnez **Activer pour les machines Linux**.
    1. Sélectionnez **Enregistrer**.
    1. Dans l’invite de confirmation, vérifiez les informations et sélectionnez **Activer** si vous voulez continuer. 

    :::image type="content" source="./media/security-center-wdatp/enable-for-linux-result.png" alt-text="Conformation de l’intégration entre Security Center et la solution EDR de Microsoft, Microsoft Defender pour point de terminaison pour Linux":::

    Azure Security Center va effectuer les opérations suivantes :

    - Intégrer automatiquement vos machines Linux à Defender pour point de terminaison
    - Ignorer les machines qui exécutent d’autres solutions basées sur fanotify (voir les détails de l’option de noyau `fanotify` requise dans la [Configuration système requise pour Linux](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux#system-requirements))
    - Détecter toutes les installations précédentes de Defender pour point de terminaison et les reconfigurer pour qu’elles s’intègrent à Security Center

    L’intégration peut prendre jusqu’à 24 heures.

    > [!NOTE]
    > La prochaine fois que vous reviendrez sur cette page du portail Azure, le bouton **Activer pour les machines Linux** ne s’affichera pas. Pour désactiver l’intégration pour Linux, vous devez la désactiver pour Windows également en désactivant la case à cocher **Autoriser Microsoft Defender pour point de terminaison à accéder à mes données**, puis en sélectionnant **Enregistrer**.


1. Pour vérifier l’installation de Defender pour point de terminaison sur une machine Linux, exécutez la commande Shell suivante sur vos machines :

    `mdatp health`

    Si Microsoft Defender pour point de terminaison est installé, son état d’intégrité s’affiche :

    `healthy : true`

    `licensed: true`

    En outre, dans le Portail Azure vous verrez une nouvelle extension Azure sur vos machines, appelée `MDE.Linux`.

### <a name="new-users-whove-never-enabled-the-integration-with-microsoft-defender-for-endpoint-for-windows"></a>Nouveaux utilisateurs qui n’ont jamais activé l’intégration avec Microsoft Defender pour le point de terminaison Windows

Si vous n’avez jamais activé l’intégration pour Windows, l’option **Autoriser Microsoft Defender pour point de terminaison à accéder à mes données** permet à Security Center de déployer Defender pour point de terminaison *aussi bien* sur vos machines Windows que sur vos machines Linux.

1. Dans le menu de Security Center, sélectionnez **Tarification et paramètres**, puis sélectionnez l’abonnement avec les machines Linux dont vous voulez qu’elles reçoivent Defender pour point de terminaison.

1. Sélectionnez **Intégrations**.

1. Sélectionnez **Autoriser Microsoft Defender for Endpoint à accéder à mes données**, puis **Enregistrer**.

    Azure Security Center va effectuer les opérations suivantes :

    - Intégrer automatiquement vos machines Windows et Linux à Defender pour point de terminaison
    - Ignorer les machines Linux qui exécutent d’autres solutions basées sur fanotify (voir les détails de l’option de noyau `fanotify` requise dans la [Configuration système requise pour Linux](/microsoft-365/security/defender-endpoint/microsoft-defender-endpoint-linux#system-requirements))
    - Détecter toutes les installations précédentes de Defender pour point de terminaison et les reconfigurer pour qu’elles s’intègrent à Security Center

    L’intégration peut prendre jusqu’à 24 heures.

1. Pour vérifier l’installation de Defender pour point de terminaison sur une machine Linux, exécutez la commande Shell suivante sur vos machines :

    `mdatp health`

    Si Microsoft Defender pour point de terminaison est installé, son état d’intégrité s’affiche :

    `healthy : true`

    `licensed: true`

    En outre, dans le Portail Azure vous verrez une nouvelle extension Azure sur vos machines, appelée `MDE.Linux`.
--- 

## <a name="access-the-microsoft-defender-for-endpoint-portal"></a>Accéder au portail Microsoft Defender for Endpoint

1. Vérifiez que le compte d’utilisateur dispose des autorisations nécessaires. Pour plus d’informations, consultez [Attribuer l'accès utilisateur au Centre de sécurité Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/assign-portal-access).

1. Vérifiez si vous avez un proxy ou un pare-feu qui bloque le trafic anonyme. Le capteur Defender for Endpoint se connecte à partir du contexte système, de sorte que le trafic anonyme doit être autorisé. Pour garantir un accès sans entraves au portail Defender for Endpoint, suivez les instructions mentionnées dans [Activer l’accès aux URL du service dans le serveur proxy](/windows/security/threat-protection/microsoft-defender-atp/configure-proxy-internet#enable-access-to-microsoft-defender-atp-service-urls-in-the-proxy-server).

1. Ouvrez le [portail Centre de sécurité Microsoft Defender](https://securitycenter.windows.com/). En savoir plus sur les fonctionnalités et les icônes du portail dans [Vue d’ensemble du portail Centre de sécurité Microsoft Defender](/windows/security/threat-protection/microsoft-defender-atp/portal-overview). 






## <a name="send-a-test-alert"></a>Envoyer une alerte de test

Pour générer une alerte de test bénigne de Defender pour point de terminaison, sélectionnez l’onglet correspondant au système d’exploitation de votre point de terminaison :

### <a name="windows"></a>[**Windows**](#tab/windows)

Pour les points de terminaison exécutant Windows :

1. Créez un dossier « C:\test-MDATP-test ».
1. Utilisez Bureau à distance pour accéder à votre ordinateur.
1. Ouvrez une fenêtre de ligne de commande.
1. A l'invite, copiez et exécutez la commande suivante. La fenêtre d’invite de commandes se ferme automatiquement.

    ```powershell
    powershell.exe -NoExit -ExecutionPolicy Bypass -WindowStyle Hidden (New-Object System.Net.WebClient).DownloadFile('http://127.0.0.1/1.exe', 'C:\\test-MDATP-test\\invoice.exe'); Start-Process 'C:\\test-MDATP-test\\invoice.exe'
    ```
    :::image type="content" source="./media/security-center-wdatp/generate-edr-alert.png" alt-text="Une fenêtre d’invite de commandes avec la commande pour générer une alerte de test.":::

    Si la commande réussit, une nouvelle alerte s’affiche sur le tableau de bord Azure Security Center et sur le portail Microsoft Defender for Endpoint. L’alerte peut mettre quelques minutes à s’afficher.
1. Pour examiner l’alerte dans Security Center, accédez à **Alertes de sécurité** > **Ligne de commande PowerShell suspecte**.
1. Dans la fenêtre d’enquête, sélectionnez le lien d’accès au portail Microsoft Defender for Endpoint.

    > [!TIP]
    > L’alerte est déclenchée avec la gravité **Informations**.


### <a name="linux"></a>[**Linux**](#tab/linux)

Pour les points de terminaison exécutant Linux :

1. Télécharger l’outil d’alerte de test sur https://aka.ms/LinuxDIY
1. Extrayez le contenu du fichier zip et exécutez ce script d’interpréteur de commandes :

    `./mde_linux_edr_diy`

    Si la commande réussit, une nouvelle alerte s’affiche sur le tableau de bord Azure Security Center et sur le portail Microsoft Defender for Endpoint. L’alerte peut mettre quelques minutes à s’afficher.

1. Pour voir l’alerte dans Security Center, accédez à **Alertes de sécurité** > **Énumération des fichiers contenant des données sensibles**.
1. Dans la fenêtre d’enquête, sélectionnez le lien d’accès au portail Microsoft Defender for Endpoint.

    > [!TIP]
    > L’alerte est déclenchée avec la gravité **Basse**.

--- 



## <a name="faq---security-centers-integration-with-microsoft-defender-for-endpoint"></a>FAQ : Intégration de Security Center à Microsoft Defender pour point de terminaison

- [Qu’est-ce que l’extension « MDE.Windows »/« MDE.Linux » qui s’exécute sur ma machine ?](#whats-this-mdewindows--mdelinux-extension-running-on-my-machine)
- [Quelles sont les conditions de licence pour Microsoft Defender for Endpoint ?](#what-are-the-licensing-requirements-for-microsoft-defender-for-endpoint)
- [Si j’ai déjà une licence Microsoft Defender pour point de terminaison, puis-je bénéficier d’une remise pour Azure Defender ?](#if-i-already-have-a-license-for-microsoft-defender-for-endpoint-can-i-get-a-discount-for-azure-defender)
- [Comment basculer dessus à partir d’un outil EDR tiers ?](#how-do-i-switch-from-a-third-party-edr-tool)

### <a name="whats-this-mdewindows--mdelinux-extension-running-on-my-machine"></a>Qu’est-ce que l’extension « MDE.Windows »/« MDE.Linux » qui s’exécute sur ma machine ?

Auparavant, Microsoft Defender pour point de terminaison était approvisionné par l’agent Log Analytics. Lorsque [nous avons étendu la prise en charge de manière à inclure Windows Server 2019](release-notes-archive.md#microsoft-defender-for-endpoint-integration-with-azure-defender-now-supports-windows-server-2019-and-windows-10-virtual-desktop-wvd-released-for-general-availability-ga) et Linux, nous avons également ajouté une extension pour effectuer l’intégration automatique. 

Security Center déploie automatiquement l’extension sur les machines exécutant :

- Windows Server 2019.
- Windows 10 Virtual Desktop (WVD).
- D’autres versions de Windows Server si Security Center ne reconnaît pas la version du système d’exploitation (par exemple, lorsqu’une image de machine virtuelle personnalisée est utilisée). Dans ce cas, Microsoft Defender pour point de terminaison continue d’être approvisionné par l’agent Log Analytics.
- Linux.

> [!IMPORTANT]
> Si vous supprimez l’extension MDE.Windows, elle ne supprime pas Microsoft Defender pour point de terminaison. Pour plus d’informations, consultez [Retirer des serveurs Windows](/microsoft-365/security/defender-endpoint/configure-server-endpoints).

### <a name="what-are-the-licensing-requirements-for-microsoft-defender-for-endpoint"></a>Quelles sont les conditions de licence pour Microsoft Defender for Endpoint ?
Defender pour point de terminaison est inclus sans coût supplémentaire avec **Azure Defender pour les serveurs**. Vous pouvez également l’acheter séparément pour 50 machines ou plus.

### <a name="if-i-already-have-a-license-for-microsoft-defender-for-endpoint-can-i-get-a-discount-for-azure-defender"></a>Si j’ai déjà une licence Microsoft Defender pour point de terminaison, puis-je bénéficier d’une remise pour Azure Defender ?
Si vous disposez déjà d’une licence pour Microsoft Defender for Endpoint, vous n’aurez pas à payer pour cette partie de votre licence Azure Defender.

Pour demander votre remise, [Contactez l’équipe de support technique de Security Center](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). Fournissez l’ID d’espace de travail, la région et le nombre de licences Microsoft Defender pour point de terminaison qui sont appliquées sur les ordinateurs de l’espace de travail donné.

La remise est effective à compter de la date d’approbation et ne sera pas rétroactive.

### <a name="how-do-i-switch-from-a-third-party-edr-tool"></a>Comment basculer dessus à partir d’un outil EDR tiers ?
Pour plus d’informations sur le basculement à partir d’une solution de point de terminaison non Microsoft, accédez à la documentation Microsoft Defender for Endpoint : [Vue d’ensemble de la migration](/windows/security/threat-protection/microsoft-defender-atp/switch-to-microsoft-defender-migration).


## <a name="next-steps"></a>Étapes suivantes

- [Plateformes et fonctionnalités prises en charge par Azure Security Center](security-center-os-coverage.md)
- [Découvrez la façon dont les suggestions vous aident à protéger vos ressources Azure](security-center-recommendations.md)
