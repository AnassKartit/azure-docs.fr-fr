---
title: Notes de publication de la version 2103 d’Azure Stack Edge Pro
description: Décrit les problèmes majeurs existants et les résolutions possibles pour la version 2103 d’Azure Stack Edge Pro.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/23/2021
ms.author: alkohli
ms.openlocfilehash: d6909492d09cb751ba8206599470f776d1fa0aa0
ms.sourcegitcommit: 4a54c268400b4158b78bb1d37235b79409cb5816
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2021
ms.locfileid: "108133886"
---
# <a name="azure-stack-edge-2103-release-notes"></a>Notes de publication de la version 2103 d’Azure Stack Edge

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Les notes de publication suivantes identifient les problèmes critiques, résolus ou non, de la version 2103 pour les appareils Azure Stack Edge. Ces notes de publication s’appliquent aux appareils Azure Stack Edge Pro avec GPU, Azure Stack Edge Pro R et Azure Stack Edge Mini R. Les fonctionnalités et les problèmes qui correspondent à un modèle spécifique sont indiqués dans la mesure du possible.

Les notes de publication sont régulièrement mises à jour ; les problèmes critiques nécessitant une solution de contournement sont ajoutés au fur et à mesure de leur découverte. Avant de déployer votre appareil, lisez attentivement les informations contenues dans les notes de publication.

Cet article concerne la mise en production d’**Azure Stack Edge 2103**, qui correspond au numéro de version logicielle **2.2.1540.2890**. Vous pouvez appliquer ce logiciel à votre appareil si vous exécutez au moins un logiciel Azure Stack Edge 2010 (2.1.1377.2170).

## <a name="whats-new"></a>Nouveautés

Les nouvelles fonctionnalités suivantes sont disponibles dans la version 2103 d’Azure Stack Edge. 
 
- **Nouvelles fonctionnalités pour les machines virtuelles** : à partir de cette mise en production, vous pouvez effectuer via le portail Azure les opérations de gestion suivantes sur les machines virtuelles :
    - ajouter ou supprimer plusieurs interfaces réseau sur une machine virtuelle existante ;
    - ajouter ou supprimer plusieurs disques sur une machine virtuelle existante ;
    - redimensionner la machine virtuelle ;
    - ajouter des données personnalisées lors du déploiement d’une machine virtuelle Windows ou Linux.

  Vous pouvez également vous [connecter à la console de la machine virtuelle sur votre appareil](azure-stack-edge-gpu-connect-virtual-machine-console.md), et résoudre des problèmes de machine virtuelle.

- **Connexion à l’interface PowerShell via le protocole https** : à partir de cette mise en production, vous ne serez plus en mesure d’ouvrir une session PowerShell distante sur un appareil via le protocole *http*. Par défaut, le protocole *https* est utilisé pour toutes les sessions. Pour plus d’informations, découvrez comment [se connecter à l’interface PowerShell](azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface) de votre appareil.

- **Améliorations pour Compute** : plusieurs améliorations ont été apportées, notamment pour les points suivants :

    - **Qualité globale de la plateforme de calcul**. Cette mise en production inclut des correctifs de bogues pour améliorer la qualité globale de la plateforme de calcul. Découvrez les [Problèmes corrigés dans la version 2103](#issues-fixed-in-2103-release). 
    - **Composants de la plateforme de calcul**. Des mises à jour de sécurité ont été appliquées à l’image de machine virtuelle Compute. IoT Edge et Azure Arc pour les versions de Kubernetes ont également été mis à jour.
    - **Diagnostics**. Une nouvelle API est mise en production pour vérifier les conditions de ressource et de réseau. Vous pouvez vous connecter à l’interface PowerShell de l’appareil et utiliser la commande `Test-HcsKubernetesStatus` pour vérifier la préparation au réseau de l’appareil.
    - **Collecte des journaux** qui devrait aboutir à une amélioration du débogage. 
    - **Infrastructure d’alerte** qui vous permettra de détecter les conflits d’adresses IP pour les adresses IP de calcul. 
    - **Mélange de la charge de travail** de Kubernetes et d’Azure Resource Manager local. 

- **Journalisation proactive par défaut** : à partir de cette mise en production, la collecte proactive de journaux est activée par défaut sur votre appareil. Cette fonctionnalité permet à Microsoft de collecter des journaux de manière proactive en fonction des indicateurs d’intégrité du système pour résoudre efficacement des problèmes liés aux appareils. Pour plus d’informations, consultez [Collecte de journaux proactive sur votre appareil](azure-stack-edge-gpu-proactive-log-collection.md).

## <a name="issues-fixed-in-2103-release"></a>Problèmes résolus dans la version 2013

Le tableau suivant répertorie les problèmes signalés dans les mises en production précédentes et résolus dans la mise en production actuelle.

| Non. | Fonctionnalité | Problème | 
| --- | --- | --- |
|**1.**|Kubernetes |Le registre de conteneurs Edge ne fonctionne pas lorsque le proxy web est activé.|
|**2.**|Kubernetes |Le registre de conteneurs Edge ne fonctionne pas avec les modules IoT Edge.| 

## <a name="known-issues-in-2103-release"></a>Problèmes connus dans la version 2103

Le tableau suivant récapitule les problèmes connus de la version 2103.

| Non. | Fonctionnalité | Problème | Solution de contournement/commentaires |
| --- | --- | --- | --- |
|**1.**|Fonctionnalités de préversion |Pour cette mise en production, toutes les fonctionnalités suivantes sont en préversion : Azure Resource Manager en local, machines virtuelles, gestion cloud des machines virtuelles, gestion de cloud Kubernetes, Kubernetes avec Azure Arc, VPN pour Azure Stack Edge Pro R et Azure Stack Edge Mini R, service multiprocessus (MPS) pour appareil Azure Stack Edge Pro avec GPU.  |Ces fonctionnalités seront généralement disponibles dans une version ultérieure. |
|**2.**|Machines virtuelles de GPU |Avant cette mise en production, le cycle de vie des machines virtuelles GPU n’était pas géré dans le flux de mise à jour. Par conséquent, lors de la mise à jour vers la version 2103, les machines virtuelles GPU ne sont pas arrêtées automatiquement pendant la mise à jour. Vous devez arrêter manuellement les machines virtuelles GPU à l’aide d’un indicateur `stop-stayProvisioned` avant de mettre à jour votre appareil. Pour plus d’informations, consultez [Interrompre ou arrêter la machine virtuelle](azure-stack-edge-gpu-deploy-virtual-machine-powershell.md#suspend-or-shut-down-the-vm).<br> Toutes les machines virtuelles GPU en cours d’exécution avant la mise à jour sont démarrées après celle-ci. Dans ces cas, les charges de travail s’exécutant sur les machines virtuelles ne sont pas arrêtées correctement. Et les machines virtuelles pourraient se retrouver dans un état indésirable après la mise à jour. <br>Toutes les machines virtuelles GPU arrêtées via `stop-stayProvisioned` avant la mise à jour sont démarrées automatiquement après celle-ci. <br>Si vous arrêtez les machines virtuelles GPU via le portail Azure, vous devez démarrer manuellement la machine virtuelle après la mise à jour de l’appareil.| Si vous exécutez des machines virtuelles GPU avec Kubernetes, arrêtez les machines virtuelles GPU juste avant la mise à jour. <br>Une fois les machines virtuelles GPU arrêtées, Kubernetes prend en charge les GPU qui étaient utilisés à l’origine par les machines virtuelles. <br>Plus longtemps les machines virtuelles GPU sont à l’état arrêté, plus les chances sont élevées que Kubernetes prenne en charge les GPU. |
|**3.**|Extension de script personnalisé de machine virtuelle |Il existe un problème connu dans les machines virtuelles Windows créées dans une version antérieure, et l’appareil a été mis à jour vers la version 2103. <br> Si vous ajoutez une extension de script personnalisé sur ces machines virtuelles, l’agent invité de machine virtuelle Windows (version 2.7.41491.901 uniquement) est bloqué dans la mise à jour entraînant une expiration du délai de déploiement de l’extension. | Pour contourner ce problème : <ul><li> Connectez-vous à la machine virtuelle Windows à l’aide du protocole RDP. </li><li> Vérifiez que `waappagent.exe` est en cours d’exécution sur la machine : `Get-Process WaAppAgent` . </li><li> Si `waappagent.exe` n’est pas en cours d’exécution, redémarrez le service `rdagent` : `Get-Service RdAgent` \| `Restart-Service`. Patientez 5 minutes.</li><li> Pendant l’exécution de `waappagent.exe`, arrêtez le processus `WindowsAzureGuest.exe`. </li><li>Une fois le processus arrêté, il recommence à s’exécuter avec la version plus récente.</li><li>Vérifiez que la version de l’agent invité de machine virtuelle Windows est 2.7.41491.971 à l’aide de la commande suivante : `Get-Process WindowsAzureGuestAgent` \| `fl ProductVersion`.</li><li>[Configurer l’extension de script personnalisé sur une machine virtuelle Windows](azure-stack-edge-gpu-deploy-virtual-machine-custom-script-extension.md) </li><ul> | 
|**4.**|Service multiprocessus (MPS) |Lors de la mise à jour du logiciel de l’appareil et du cluster Kubernetes, le paramètre MPS n’est pas conservé pour les charges de travail.   |[Réactivez MPS](azure-stack-edge-gpu-connect-powershell-interface.md#connect-to-the-powershell-interface) et redéployez les charges de travail qui utilisaient MPS. |


## <a name="known-issues-from-previous-releases"></a>Problèmes connus issus des versions précédentes

Le tableau suivant fournit un résumé des problèmes connus déjà présents dans les versions précédentes.

| Non. | Fonctionnalité | Problème | Solution de contournement/commentaires |
| --- | --- | --- | --- |
| **1.** |Azure Stack Edge Pro + SQL Azure | La création d’une base de données SQL nécessite un accès administrateur.   |Suivez les étapes suivantes au lieu des étapes 1-2 dans [https://docs.microsoft.com/azure/iot-edge/tutorial-store-data-sql-server#create-the-sql-database](../iot-edge/tutorial-store-data-sql-server.md#create-the-sql-database). <ul><li>Dans l’interface utilisateur locale de votre appareil, activez l’interface de calcul. Sélectionnez **Compute > N° de port > Activer pour le calcul > Appliquer.**</li><li>Téléchargez `sqlcmd` sur votre ordinateur client à partir de https://docs.microsoft.com/sql/tools/sqlcmd-utility. </li><li>Connectez-vous à l’adresse IP de votre interface de calcul (port activé), en ajoutant un « 1401 » à la fin de l’adresse.</li><li>La commande finale ressemble à ceci : sqlcmd -S {Interface IP},1401 -U SA -P "Strong!Passw0rd".</li>Ensuite, les étapes 3-4 de la documentation actuelle devraient être identiques. </li></ul> |
| **2.** |Actualiser| Les modifications incrémentielles d’objets blob restaurés via la fonctionnalité **Actualiser** ne sont pas prises en charge. |Pour les points de terminaison d’objet blob, des mises à jour partielles d’objets blob après une actualisation peuvent empêcher le chargement des mises à jour dans le cloud. Prenons l’exemple de la séquence d’actions suivante :<ul><li>Créer un objet blob dans le cloud. Ou supprimer un objet blob précédemment chargé à partir de l’appareil.</li><li>Actualiser l’objet blob à partir du cloud dans l’appliance à l’aide de la fonctionnalité d’actualisation.</li><li>Mettre à jour uniquement une partie de l’objet blob à l’aide des API REST du Kit de développement logiciel (SDK) Azure.</li></ul>Ces actions peuvent avoir pour effet que des sections mises à jour de l’objet blob ne sont pas mises à jour dans le cloud. <br>**Solution de contournement** : servez-vous d’outils tels que Robocopy ou d’une copie de fichiers normale via l’Explorateur ou la ligne de commande pour remplacer des objets blob entiers.|
|**3.**|Limitation|Lors d’une limitation, si de nouvelles écritures sur l’appareil ne sont pas autorisées, les écritures effectuées par le client NFS échouent avec l’erreur « Autorisation refusée ».| L’erreur s’affiche comme ci-dessous :<br>`hcsuser@ubuntu-vm:~/nfstest$ mkdir test`<br>mkdir : impossible de créer le répertoire ’test’ : Autorisation refusée|
|**4.**|Ingestion du Stockage Blob|Lors de l’utilisation d’AzCopy version 10 pour l’ingestion du stockage d’objets blob, exécutez AzCopy avec l’argument suivant : `Azcopy <other arguments> --cap-mbps 2000`.| Si ces limites ne sont pas fournies pour AzCopy, cela risque d’entraîner l’envoi d’un grand nombre de demandes à l’appareil et d’occasionner des problèmes avec le service.|
|**5.**|Comptes de stockage hiérarchisés|Voici ce qu’il se produit lors de l’utilisation de comptes de stockage hiérarchisés :<ul><li> Seuls les objets blob de blocs sont pris en charge. Les objets blob de pages ne sont pas pris en charge.</li><li>Il n’existe aucune prise en charge de l’API de copie ou de capture instantanée.</li><li> L’ingestion de charge de travail Hadoop via `distcp` n’est pas prise en charge, car elle sollicite abondamment l’opération de copie.</li></ul>||
|**6.**|Connexion de partage NFS|Si plusieurs processus copient vers le même partage et que l’attribut `nolock` n’est pas utilisé, des erreurs peuvent apparaître en cours de copie.|L’attribut `nolock` doit être passé à la commande mount pour copier des fichiers vers le partage NFS. Par exemple : `C:\Users\aseuser mount -o anon \\10.1.1.211\mnt\vms Z:`.|
|**7.**|Cluster Kubernetes|Lorsque vous appliquez une mise à jour sur votre appareil exécutant un cluster Kubernetes, les machines virtuelles Kubernetes redémarrent. Dans ce cas, seuls les pods déployés avec des réplicas spécifiés sont automatiquement restaurés après une mise à jour.  |Si vous avez créé des pods individuels en dehors d’un contrôleur de réplication sans spécifier de jeu de réplicas, ces pods ne seront pas restaurés automatiquement après la mise à jour de l’appareil. Vous devrez les restaurer.<br>Un jeu de réplicas remplace des pods supprimés ou arrêtés pour une raison quelconque, par exemple un échec de nœud ou une mise à niveau de nœud perturbatrice. Pour cette raison, nous vous recommandons d’utiliser un jeu de réplicas, même si votre application ne requiert qu’un seul pod.|
|**8.**|Cluster Kubernetes|Kubernetes sur Azure Stack Edge Pro est pris en charge uniquement avec Helm v3 ou ultérieur. Pour plus d’informations, consultez le [Forum Aux Questions : Suppression de Tiller](https://v3.helm.sh/docs/faq/).|
|**9.**|Kubernetes avec Azure Arc |Pour la disponibilité générale, Kubernetes activé par Azure Arc est mis à jour de la version 0.1.18 vers la version 0.2.9. Étant donné que la mise à jour de Kubernetes activé par Azure Arc n’est pas pris en charge sur un appareil Azure Stack Edge, vous devez redéployer Kubernetes activé par Azure Arc.|Effectuez les étapes suivantes :<ol><li>[Appliquez les mises à jour logicielles de l’appareil et de Kubernetes](azure-stack-edge-gpu-install-update.md).</li><li>Connectez-vous à l’[interface PowerShell de l’appareil](azure-stack-edge-gpu-connect-powershell-interface.md).</li><li>Supprimez l’agent Azure Arc existant. Entrez : `Remove-HcsKubernetesAzureArcAgent`.</li><li>Déployez [Azure Arc sur une nouvelle ressource](azure-stack-edge-gpu-deploy-arc-kubernetes-cluster.md). N’utilisez pas de ressource Azure Arc existante.</li></ol>|
|**10.**|Kubernetes avec Azure Arc|Les déploiements d’Azure Arc ne sont pas pris en charge si un proxy web est configuré sur votre appareil Azure Stack Edge Pro.||
|**11.**|Kubernetes |Le port 31000 est réservé au tableau de bord Kubernetes. Le port 31001 est réservé au registre de conteneurs Edge. De même, dans la configuration par défaut, les adresses IP 172.28.0.1 et 172.28.0.10 sont réservées respectivement pour le service Kubernetes et le service DNS de base.|N’utilisez pas d’adresses IP réservées.|
|**12.**|Kubernetes |Kubernetes n’autorise pas actuellement les services LoadBalancer multiprotocoles. Par exemple, un service DNS devant écouter à la fois les protocoles TCP et UDP. |Pour contourner cette limitation de Kubernetes avec MetalLB, deux services (un pour TCP, un pour UDP) peuvent être créés sur le même sélecteur de pod. Ces services utilisent la même clé de partage et spec.loadBalancerIP pour partager la même adresse IP. Des adresses IP peuvent également être partagées si vous avez plus de services qu’il n’y d’adresses IP disponibles. <br> Pour plus d’informations, voir [Partage d’adresse IP](https://metallb.universe.tf/usage/#ip-address-sharing).|
|**13.**|Cluster Kubernetes|Des modules existants de la Place de marché Azure IoT Edge peuvent nécessiter des modifications pour s’exécuter sur IoT Edge sur un appareil Azure Stack Edge.|Pour plus d’informations, consultez Modifier des modules Azure IoT Edge de la place de marché pour les exécuter sur des appareils Azure Stack Edge.<!-- insert link-->|
|**14.**|Kubernetes |Les montages de liaison basés sur des fichiers ne sont pas pris en charge avec Azure IoT Edge sur Kubernetes sur un appareil Azure Stack Edge.|IoT Edge utilise une couche de traduction pour convertir des options de `ContainerCreate` en constructions Kubernetes. La création de `Binds` mappe au répertoire `hostpath`. Par conséquent, des montages de liaison basés sur des fichiers ne peuvent pas être liés à des chemins d’accès dans des conteneurs IoT Edge. Si possible, mappez le répertoire parent.|
|**15.**|Kubernetes |Si vous apportez vos propres certificats pour IoT Edge et que vous les ajoutez sur votre appareil Azure Stack Edge une fois le calcul configuré sur celui-ci, les nouveaux certificats ne sont pas sélectionnés.|Pour contourner ce problème, vous devez charger les certificats avant de configurer le calcul sur l’appareil. Si le calcul est déjà configuré, [connectez-vous à l’interface PowerShell de l’appareil et exécutez des commandes IoT Edge](azure-stack-edge-gpu-connect-powershell-interface.md#use-iotedge-commands). Redémarrez les pods `iotedged` et `edgehub`.|
|**16.**|Certificats |Dans certains cas, la mise à jour de l’état des certificats dans l’interface utilisateur locale peut prendre plusieurs secondes. |Les scénarios suivants dans l’interface utilisateur locale peuvent être affectés.<ul><li>Colonne **État** dans la page **Certificats**.</li><li>Vignette **Sécurité** dans la page **Démarrer**.</li><li>Vignette **Configuration** dans la page **Vue d’ensemble**.</li></ul>  |
|**17.**|IoT Edge |Les modules déployés via IoT Edge ne peuvent pas utiliser le réseau hôte. | |
|**18.**|Calcul + Kubernetes |Le calcul/Kubernetes ne prend pas en charge le proxy web NTLM. ||
|**19.**|Kubernetes + mise à jour |Les versions logicielles antérieures, telles que les versions 2008, présentent un problème de mise à jour de condition de concurrence qui provoque l’échec de la mise à jour avec ClusterConnectionException. |L’utilisation des builds plus récentes devrait permettre d’éviter ce problème. Si ce problème persiste, la solution de contournement consiste à retenter la mise à niveau, qui devrait fonctionner.|
|**20**|Internet Explorer|Si les fonctionnalités de sécurité améliorées sont activées, vous ne pourrez peut-être pas accéder aux pages de l’interface utilisateur web locale. | Désactivez la sécurité renforcée, puis redémarrez votre navigateur.|
|**21.**|Tableau de bord Kubernetes | Le point de terminaison *HTTPS* pour le tableau de bord Kubernetes avec le certificat SSL n’est pas pris en charge. | |
|**22.**|Kubernetes |Kubernetes ne prend pas en charge « : » dans les noms de variables d’environnement utilisés par les applications .NET. Cela est également nécessaire pour que le module Event Grid IoT Edge fonctionne sur l’appareil Azure Stack Edge et d’autres applications. Pour plus d’informations, consultez la [documentation sur ASP.NET Core](/aspnet/core/fundamentals/configuration/?tabs=basicconfiguration#environment-variables).|Remplacez « : » par un trait de soulignement double. Pour plus d’informations, consultez la section [Problème Kubernetes](https://github.com/kubernetes/kubernetes/issues/53201).|
|**23.** |Cluster Azure Arc + Kubernetes |Par défaut, lorsque les ressources `yamls` sont supprimées du référentiel Git, les ressources correspondantes ne sont pas supprimées du cluster Kubernetes.  |Pour autoriser la suppression des ressources quand elles sont supprimées du référentiel git, définissez `--sync-garbage-collection` dans OperatorParams, dans Arc. Pour plus d’informations, consultez [Supprimer une configuration](../azure-arc/kubernetes/tutorial-use-gitops-connected-cluster.md#additional-parameters). |
|**24.**|NFS |Les applications qui utilisent des montages de partage NFS sur votre appareil pour écrire des données doivent utiliser l’écriture exclusive. Ceci garantit que les écritures sont écrites sur le disque.| |
|**25.**|Configuration du calcul |La configuration du calcul échoue dans les configurations réseau où les passerelles, commutateurs ou routeurs répondent aux demandes ARP (Address Resolution Protocol) pour des systèmes qui n’existent pas sur le réseau.| |
|**26.**|Calcul et Kubernetes |Si Kubernetes est configuré en premier sur votre appareil, il réclame tous les GPU disponibles. Par conséquent, il n’est pas possible de créer des machines virtuelles Azure Resource Manager à l’aide de GPU après avoir configuré Kubernetes. |Si votre appareil a 2 GPU, vous pouvez créer 1 machine virtuelle qui utilise le GPU, puis configurer Kubernetes. Dans ce cas, Kubernetes utilise le GPU disponible restant. |


<!--|**18.**|Azure Private Edge Zone (Preview) |There is a known issue with Virtual Network Function VM if the VM was created on Azure Stack Edge device running earlier preview builds such as 2006/2007b and then the device was updated to 2009 GA release. The issue is that the VNF information can't be retrieved or any new VNFs can't be created unless the VNF VMs are deleted before the device is updated.  |Before you update Azure Stack Edge device to 2009 release, use the PowerShell command `get-mecvnf` followed by `remove-mecvnf <VNF guid>` to remove all Virtual Network Function VMs one at a time. After the upgrade, you will need to redeploy the same VNFs.|-->


## <a name="next-steps"></a>Étapes suivantes

- [Mettre à jour votre appareil](azure-stack-edge-gpu-install-update.md)