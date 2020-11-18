---
title: Guide de configuration accélérée de laboratoire pour Azure Lab Services
description: Ce guide aide les créateurs de laboratoires à configurer rapidement un compte de laboratoire pour une utilisation au sein de leur école.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: f7423a76fd3ceb238c8c5c1a4ea794ff83b28b4a
ms.sourcegitcommit: b4880683d23f5c91e9901eac22ea31f50a0f116f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/11/2020
ms.locfileid: "94491662"
---
# <a name="lab-setup-guide"></a>Guide de configuration d’un laboratoire

Le processus de publication d’un labo pour vos étudiants peut prendre plusieurs heures.  Ce délai dépend du nombre de machines virtuelles créées dans votre labo. Accordez-vous au moins une journée pour configurer un labo afin de vous assurer qu’il fonctionne correctement et pour disposer de suffisamment de temps pour publier les machines virtuelles des étudiants.

## <a name="understand-the-lab-requirements-of-your-class"></a>Comprendre les conditions requises du labo de votre classe

Avant de configurer un nouveau labo, vous devez vous poser les questions suivantes.

### <a name="what-software-requirements-does-the-class-have"></a>Quelle est la configuration logicielle requise par la classe ?

En fonction des objectifs d’apprentissage de votre classe, déterminez le système d’exploitation, les applications et les outils qui doivent être installés sur les machines virtuelles du labo. Pour configurer des machines virtuelles de laboratoire, vous avez trois options :

- **Utiliser une image de la Place de marché Azure** : la Place de marché Azure fournit des centaines d’images que vous pouvez utiliser lors de la création d’un labo. Pour certaines classes, l’une de ces images peut déjà contenir tout ce dont vous avez besoin pour votre classe.

- **Créer une image personnalisée** : vous pouvez créer votre propre image personnalisée en utilisant une image de la Place de marché Azure comme point de départ et en la personnalisant en installant des logiciels supplémentaires et en apportant des modifications de configuration.

- **Utiliser une image personnalisée existante** : vous pouvez réutiliser des images personnalisées existantes que vous avez créées ou qui ont été créées par d’autres administrateurs ou professeurs de votre établissement scolaire. Pour utiliser des images personnalisées, vos administrateurs doivent configurer une galerie d’images partagées.  Une galerie d’images partagées est un référentiel que vous pouvez utiliser pour enregistrer des images personnalisées.

> [!NOTE]
> Les administrateurs sont responsables de l’activation des images de la Place de marché Azure et des images personnalisées afin que vous puissiez les utiliser. Collaborez avec votre service informatique pour être sûr que les images dont vous avez besoin sont activées. Les images personnalisées que vous créez sont automatiquement activées pour une utilisation dans les laboratoires dont vous êtes propriétaire.

### <a name="what-hardware-requirements-does-the-class-have"></a>Quelle est la configuration matérielle requise pour la classe ?

Vous pouvez choisir parmi différentes tailles de calcul :

- Des tailles de virtualisation imbriquées, afin que vous puissiez permettre aux étudiants d’accéder à une machine virtuelle capable d’héberger plusieurs machines virtuelles imbriquées. Par exemple, vous pourriez utiliser cette taille de calcul pour les cours sur les réseaux ou le piratage éthique.

- Des tailles de GPU, afin que vos étudiants puissent utiliser des types d’applications nécessitant beaucoup de ressources de calcul. Ce choix convient par exemple souvent à l’intelligence artificielle et au Machine Learning.

Pour obtenir des conseils sur la sélection de la taille de machine virtuelle appropriée, lisez les articles suivants :
- [Dimensionnement des machines virtuelles](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#vm-sizing)
- [Moving from a Physical Lab to Azure Lab Services](https://techcommunity.microsoft.com/t5/azure-lab-services/moving-from-a-physical-lab-to-azure-lab-services/ba-p/1654931) (Passer d’un laboratoire physique à Azure Lab Services)

> [!NOTE]
> En fonction de la région de votre labo, vous pourriez voir moins de tailles de calcul disponibles, car cela varie selon la région. En règle générale, vous devez sélectionner la plus petite taille de calcul la plus proche de vos besoins. Avec Azure Lab Services, vous pouvez configurer un nouveau labo avec une capacité de calcul différente ultérieurement, si nécessaire.

### <a name="what-dependencies-does-the-class-have-on-external-azure-or-network-resources"></a>Quelles sont les dépendances de la classe envers les ressources réseau ou Azure externes ?
Vos machines virtuelles de laboratoire peuvent avoir besoin d’accéder à des ressources externes, par exemple à une base de données, un partage de fichiers ou un serveur de licences.  Pour permettre aux machines virtuelles de votre laboratoire d’utiliser des ressources externes, contactez vos administrateurs informatiques.

> [!NOTE]
> Vous devez déterminer si vous pouvez réduire les dépendances de votre laboratoire aux ressources externes en fournissant la ressource directement sur la machine virtuelle. Par exemple, pour éviter d’avoir à lire les données d’une base de données externe, vous pouvez installer la base de données directement sur la machine virtuelle.  

### <a name="how-will-costs-be-controlled"></a>Comment les coûts seront-ils contrôlés ?
Les services Lab utilisent un modèle tarifaire avec paiement à l’utilisation, ce qui signifie que vous payez uniquement pour la durée d’exécution d’une machine virtuelle de labo. Pour contrôler les coûts, vous avez trois options qui sont généralement utilisées conjointement :

- **Planification** : une planification vous permet de contrôler automatiquement le moment où les machines virtuelles de vos labos sont démarrées et arrêtées.
- **Quota** : le quota contrôle le nombre d’heures pendant lesquelles les étudiants auront accès à une machine virtuelle en dehors des heures planifiées.  Lorsqu’un étudiant utilise sa machine virtuelle et que son quota est atteint, la machine virtuelle s’arrête automatiquement.  L’étudiant ne pourra redémarrer la machine virtuelle que si le quota est augmenté.
- **Arrêt automatique** : quand cette option est activée, le paramètre d’arrêt automatique entraîne l’arrêt automatique des machines virtuelles Windows après qu’un étudiant s’est déconnecté d’une session RDP (Remote Desktop Protocol). Ce paramètre est désactivé par défaut.

Pour plus d'informations, consultez les articles suivants :
- [Estimer les coûts](https://docs.microsoft.com/azure/lab-services/cost-management-guide#estimate-the-lab-costs)
- [Gérer les coûts](https://docs.microsoft.com/azure/lab-services/cost-management-guide#manage-costs)

### <a name="how-will-students-save-their-work"></a>Comment les étudiants vont-ils enregistrer leur travail ?
Chaque machine virtuelle affectée à un étudiant l’est pour la durée du labo. Les élèves peuvent choisir de procéder de différentes façons :

- Enregistrer directement sur la machine virtuelle.
- Enregistrer sur un emplacement externe, tel que OneDrive ou GitHub.

Il est possible de configurer OneDrive automatiquement pour les étudiants sur leurs machines virtuelles de labo.

> [!NOTE]
> Pour être certain de disposer d’un accès continu à leur travail enregistré en dehors du labo, et après les cours, nous recommandons aux étudiants d’enregistrer leur travail dans un référentiel externe.

### <a name="how-will-students-connect-to-their-vm"></a>Comment les étudiants vont-ils se connecter à leur machine virtuelle ?
Pour les connexions RDP aux machines virtuelles Windows, nous recommandons aux étudiants d’utiliser le [client Bureau à distance Microsoft](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients). Le client Bureau à distance prend en charge Mac, Chromebooks et Windows.

Pour les machines virtuelles Linux, les étudiants peuvent utiliser SSH ou RDP. Pour que les étudiants puissent se connecter à l’aide du protocole RDP, vous devez installer et configurer les packages RDP et GUI nécessaires.

### <a name="will-students-also-be-using-microsoft-teams"></a>Les étudiants utiliseront-ils aussi Microsoft Teams ?
Azure Lab Services s’intègre à Microsoft Teams afin que les enseignants puissent créer et gérer leurs laboratoires au sein de Teams.  De même, les étudiants peuvent accéder au laboratoire au sein de Teams.

Pour plus d’informations, consultez l’article suivant :
- [Azure Lab Services dans Microsoft Teams](https://docs.microsoft.com/azure/lab-services/lab-services-within-teams-overview)

## <a name="set-up-your-lab"></a>Configurer votre lab

Une fois que vous avez compris les conditions requises pour le labo de votre classe, vous êtes prêt à le configurer. Suivez les liens de cette section pour savoir comment configurer votre laboratoire.  Notez que différentes étapes sont fournies selon que vous utilisez des laboratoires au sein de Teams.

1. **Créez un labo**. Reportez-vous aux tutoriels sur la création d’un laboratoire :
    - [Créer un laboratoire de classe](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#create-a-classroom-lab).
    - [Créer un labo depuis Teams](https://docs.microsoft.com/azure/lab-services/how-to-get-started-create-lab-within-teams)

    > [!NOTE]
    > Si votre classe nécessite une virtualisation imbriquée, consultez les étapes décrites dans l’article sur l’[activation de la virtualisation imbriquée](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-nested-virtualization-template-vm).

1. **Personnalisez des images et publiez des machines virtuelles de labo.** Connectez-vous à une machine virtuelle spéciale appelée « modèle de machine virtuelle ». Consultez les étapes décrites dans les guides suivants :
    - [Créer et gérer un modèle de machine virtuelle](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#publish-the-template-vm)
    - [Utiliser une galerie d’images partagées](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-shared-image-gallery)

    > [!NOTE]
    > Si vous utilisez Windows, vous devez également consulter les instructions fournies dans l’article sur la [préparation d’un modèle de machine virtuelle Windows](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-prepare-windows-template). Ces instructions incluent les étapes de configuration pour OneDrive et Office afin que vos élèves puissent les utiliser.

1. **Gérez le pool et la capacité des machines virtuelles.** Vous pouvez facilement effectuer un scale-up ou un scale-down des machines virtuelles en fonction des besoins de votre classe. N’oubliez pas que l’augmentation de la capacité des machines virtuelles peut prendre plusieurs heures, car de nouvelles machines virtuelles sont alors configurées. Consultez les étapes décrites dans les articles suivants :
    - [Configurer et gérer un pool de machines virtuelles](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-set-virtual-machine-passwords)
    - [Gérer un pool de machines virtuelles dans Lab Services à partir de Teams](https://docs.microsoft.com/azure/lab-services/how-to-manage-vm-pool-within-teams)

1. **Ajoutez et gérez des utilisateurs de labo.** Pour ajouter des utilisateurs à votre laboratoire, reportez-vous aux étapes décrites dans les didacticiels suivants :
   - [Ajouter des utilisateurs au labo](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#add-users-to-the-lab)
   - [Envoyer des invitations aux utilisateurs](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#send-invitation-emails-to-users)
   - [Gérer les listes d’utilisateurs de Lab Services à partir de Teams](https://docs.microsoft.com/azure/lab-services/how-to-manage-user-lists-within-teams)

    Pour plus d’informations sur les types de comptes que les étudiants peuvent utiliser, consultez [Comptes étudiants](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#student-accounts).
  
1. **Maîtriser les coûts.** Pour maîtriser les coûts de votre labo, définissez des planifications, des quotas et un arrêt automatique. Consultez les didacticiels suivants :

   - [Définir un calendrier](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#set-a-schedule-for-the-lab)

        > [!NOTE]
        > En fonction du type de système d’exploitation que vous avez installé, le démarrage d’une machine virtuelle peut prendre plusieurs minutes. Pour vous assurer qu’une machine virtuelle de labo est prête à l’emploi pendant vos heures planifiées, nous vous recommandons de démarrer les machines virtuelles 30 minutes à l’avance.

   - [Définir des quotas pour les utilisateurs](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-quotas-for-users) et [définir un quota supplémentaire pour un utilisateur spécifique](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-additional-quotas-for-specific-users)
  
   - [Activer l’arrêt automatique lors de la déconnexion](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

        > [!NOTE]
        > Les planifications et les quotas ne s’appliquent pas à la machine virtuelle du modèle, mais les paramètres d’arrêt automatique s’appliquent. 
        > 
        > Quand vous créez un labo, le modèle de machine virtuelle est créé, mais il ne démarre pas. Vous pouvez le démarrer, vous y connecter et installer les logiciels prérequis pour le labo, puis le publier. Quand vous publiez le modèle de machine virtuelle, il s’arrête automatiquement si vous ne l’avez pas arrêté vous-même. 
        > 
        > Étant donné que les modèles de machines virtuelles engendrent des **coûts** lors de leur exécution, prenez soin de vérifier que le modèle de machine virtuelle est arrêté lorsque vous n’avez pas besoin de l’exécuter.

    - [Créer et gérer des planifications Azure Lab Services dans Teams](https://docs.microsoft.com/azure/lab-services/how-to-create-schedules-within-teams) 

1. **Utilisez le tableau de bord.** Pour obtenir des instructions, consultez l’article sur l’[utilisation du tableau de bord du labo](https://docs.microsoft.com/azure/lab-services/classroom-labs/use-dashboard).

    > [!NOTE]
    > Le coût estimé montré dans le tableau de bord est le coût maximal auquel vous pouvez vous attendre pour l’utilisation du labo par les étudiants. Par exemple, les heures de quota inutilisées par vos élèves ne sont *pas* facturées. Les coûts estimés *ne reflètent pas* les frais d’utilisation de la machine virtuelle modèle ou de la galerie d’images partagées ni ceux encourus quand le créateur du labo démarre la machine d’un utilisateur.

## <a name="next-steps"></a>Étapes suivantes

- [Suivre l’utilisation d’un labo de classe](tutorial-track-usage.md)
  
- [Accéder à un laboratoire de salle de classe](tutorial-connect-virtual-machine-classroom-lab.md)
