---
title: Installer l’agent de provisionnement cloud Azure AD Connect
description: Cet article explique comment installer l’agent d’approvisionnement Azure AD Connect.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 05/19/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: dcb322805ac3368dd6ed8e193875e083b27195e1
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94695280"
---
# <a name="install-the-azure-ad-connect-cloud-provisioning-agent"></a>Installer l’agent de provisionnement cloud Azure AD Connect
Ce document vous guide tout au long du processus d’installation de l’agent d’approvisionnement Azure Active Directory (Azure AD) Connecte, et explique comment le configurer au départ dans le portail Azure.

>[!IMPORTANT]
>Les instructions d’installation suivantes supposent que tous les [prérequis](how-to-prerequisites.md) ont été réunis.

L’installation et la configuration du provisionnement Azure AD Connect sont accomplies dans les étapes suivantes :
    
- [Installer l’agent](#install-the-agent)
- [Vérifier l’installation de l’agent](#verify-agent-installation)


## <a name="install-the-agent"></a>Installer l’agent
Pour installer l’agent, procédez comme suit.

1. Connectez-vous au serveur que vous allez utiliser à l’aide des autorisations d’administrateur d’entreprise.
1. Connectez-vous au portail Azure, puis accédez à **Azure Active Directory**.
1. Dans le menu de gauche, sélectionnez **Azure AD Connect**.
1. Sélectionnez **Gérer le provisionnement (préversion)**  > **Passer en revue tous les agents**.
1. Téléchargez l’agent de provisionnement Azure AD Connect à partir du portail Azure.

   ![Télécharger un agent local](media/how-to-install/install-9.png)</br>
1. Exécutez le programme d’installation d’approvisionnement Azure AD Connect (AADConnectProvisioningAgent.Installer).
1. Dans l’écran **Package Agent d’approvisionnement Microsoft Azure AD Connect**, acceptez les termes du contrat de licence et sélectionnez **Installer**.

   ![Écran Package Agent d’approvisionnement Microsoft Azure AD Connect](media/how-to-install/install-1.png)</br>

1. Une fois cette opération terminée, l’Assistant Configuration démarre. Connectez-vous avec votre compte d’administrateur général Azure AD.
1. Dans l’écran **Connecter Active Directory**, sélectionnez **Ajouter un répertoire**. Ensuite, connectez-vous avec votre compte Administrateur Active Directory. Cette opération ajoute votre répertoire local. Sélectionnez **Suivant**.

   ![Écran Connecter Active Directory](media/how-to-install/install-3.png)</br>

1. Dans l’écran **Configuration terminée**, cliquez sur **Confirmer**. Cette opération inscrit et redémarre l’agent.

   ![Écran Configuration terminée](media/how-to-install/install-4a.png)</br>

1. Une fois cette opération terminée, vous devriez voir s’afficher l’avis **La configuration de votre agent a été vérifiée avec succès**. Sélectionnez **Quitter**.

   ![Bouton Quitter](media/how-to-install/install-5.png)</br>
1. Si vous l’écran initial **Package Agent d’approvisionnement Microsoft Azure AD Connect** est toujours affiché, sélectionnez **Fermer**.

## <a name="verify-agent-installation"></a>Vérifier l’installation de l’agent
La vérification de l’agent se produit dans le portail Azure et sur le serveur local qui exécute l’agent.

### <a name="azure-portal-agent-verification"></a>Vérification de l’agent dans le portail Azure
Pour vérifier que l’agent est visible par Azure, procédez comme suit.

1. Connectez-vous au portail Azure.
1. À gauche, sélectionnez **Azure Active Directory** > **Azure AD Connect** Au centre, sélectionnez **Gérer le provisionnement (préversion)** .

   ![Portail Azure](media/how-to-install/install-6.png)</br>

1.  Dans l’écran **Provisionnement Azure AD (préversion)** , sélectionnez **Passer en revue tous les agents**.

    ![Option Passer en revue tous les agents](media/how-to-install/install-7.png)</br>
 
1. L’écran **Agents de provisionnement locaux** affiche les agents que vous avez installés. Vérifiez que l’agent en question est présent et qu’il est marqué comme étant *actif*.

   ![Écran Agents de provisionnement locaux](media/how-to-install/verify-1.png)</br>



### <a name="on-the-local-server"></a>Sur le serveur local
Pour vérifier que l’agent est en cours d’exécution, procédez comme suit.

1.  Connectez-vous au serveur avec un compte Administrateur.
1.  Ouvrez **Services** en y accédant ou en accédant à **Démarrer** > **Exécuter** > **Services.msc**.
1.  Sous **Services**, assurez-vous que le **Programme de mise à jour de l’agent Microsoft Azure AD Connect** et l’**agent d’approvisionnement Microsoft Azure AD Connect** sont présents, et que leur état est *En cours d’exécution*.

    ![Écran Services](media/how-to-install/troubleshoot-1.png)

>[!IMPORTANT]
>L’agent a été installé mais il faut le configurer et l’activer pour qu’il commence à synchroniser les utilisateurs. Pour configurer un nouvel agent, consultez [Créer une configuration pour le provisionnement cloud Azure AD Connect](how-to-configure.md).



## <a name="next-steps"></a>Étapes suivantes 

- [Présentation du provisionnement](what-is-provisioning.md)
- [Présentation du provisionnement cloud Azure AD Connect](what-is-cloud-provisioning.md)
 
