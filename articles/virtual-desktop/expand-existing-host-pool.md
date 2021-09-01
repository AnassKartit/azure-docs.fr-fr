---
title: Développer un pool d’hôtes existant avec de nouveaux hôtes de session – Azure
description: Guide pratique pour développer un pool d’hôtes existant avec de nouveaux hôtes de session dans Azure Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 07/14/2021
ms.author: helohr
manager: femila
ms.openlocfilehash: f37e7e18fd32c3ad0b06f1189c57f44d72dced7a
ms.sourcegitcommit: 9339c4d47a4c7eb3621b5a31384bb0f504951712
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/14/2021
ms.locfileid: "113760860"
---
# <a name="expand-an-existing-host-pool-with-new-session-hosts"></a>Développer un pool d’hôtes existant avec de nouveaux hôtes de session

>[!IMPORTANT]
>Ce contenu s’applique à Azure Virtual Desktop avec des objets Azure Virtual Desktop pour Azure Resource Manager. Si vous utilisez Azure Virtual Desktop (classique) sans objets Azure Resource Manager, consultez [cet article](./virtual-desktop-fall-2019/expand-existing-host-pool-2019.md).

À mesure que vous intensifiez l’utilisation au sein de votre pool d’hôtes, il se peut que vous deviez développer votre pool d’hôtes existant avec de nouveaux hôtes de session pour gérer la nouvelle charge.

Cet article explique comment développer un pool d’hôtes existant avec de nouveaux hôtes de session.

## <a name="what-you-need-to-expand-the-host-pool"></a>Ce dont vous avez besoin pour développer le pool d’hôtes

Avant de commencer, assurez-vous que vous avez créé un pool d’hôtes et des machines virtuelles hôtes de session à l’aide de l’une des méthodes suivantes :

- [Azure portal](./create-host-pools-azure-marketplace.md)
- [Créer un pool d’hôtes avec PowerShell](./create-host-pools-powershell.md)

Vous aurez également besoin des informations suivantes recueillies lors de la création initiale du pool d’hôtes et des machines virtuelles hôtes de session :

- Taille de machine virtuelle, image et préfixe de nom
- Informations d’identification de l’administrateur de jonction de domaine
- Nom du réseau virtuel et nom du sous-réseau

## <a name="add-virtual-machines-with-the-azure-portal"></a>Ajouter des machines virtuelles avec le portail Azure

Pour développer votre pool d’hôtes en ajoutant des machines virtuelles :

1. Connectez-vous au portail Azure.

2. Recherchez et sélectionnez **Azure Virtual Desktop**.

3. Dans le menu sur le côté gauche de l’écran, sélectionnez **Pools d’hôtes**, puis sélectionnez le nom du pool d’hôtes auquel vous souhaitez ajouter des machines virtuelles.

4. Sélectionnez **Hôtes de session** dans le menu du côté gauche de l’écran.

5. Sélectionnez **+ Ajouter** pour commencer à créer votre pool d’hôtes.

6. Sélectionnez directement l’onglet **Détails de machine virtuelle**. Vous pouvez y voir et modifier les détails de la machine virtuelle que vous souhaitez ajouter au pool d’hôtes.

7. Sélectionnez le groupe de ressources sous lequel vous souhaitez créer les machines virtuelles, puis sélectionnez la région. Vous pouvez choisir la région en cours d’utilisation ou une nouvelle région.

8. Entrez le nombre d’hôtes de session que vous souhaitez ajouter à votre pool d’hôtes dans **Nombre de machines virtuelles**. Par exemple, si vous étendez votre pool d’hôte avec cinq hôtes, entrez **5**.

    >[!NOTE]
    >Bien qu’il soit possible de modifier l’image et le préfixe des machines virtuelles, nous vous déconseillons de les modifier si vous avez des machines virtuelles ayant des images différentes dans le même pool d’hôtes. Modifiez l’image et le préfixe uniquement si vous envisagez de supprimer des machines virtuelles ayant des images antérieures du pool d’hôtes concerné.

9. Pour les **informations sur le réseau virtuel**, sélectionnez le réseau virtuel et le sous-réseau auxquels vous souhaitez joindre les machines virtuelles. Vous pouvez sélectionner le même réseau virtuel que vos machines existantes, ou en choisir un autre qui est plus adapté à la région que vous avez sélectionnée à l’étape 7.

10. Pour le **Domaine à joindre**, choisissez de joindre les machines virtuelles à Active Directory ou [Azure Active Directory](deploy-azure-ad-joined-vm.md). La sélection **Inscrire la machine virtuelle avec Intune** inscrit automatiquement les machines virtuelles dans Intune. Toutes les machines virtuelles d’un pool d’hôtes doivent être jointes au même domaine ou locataire Azure AD.

11. Pour l’**UPN jonction de domaine AD**, entrez le nom d’utilisateur et le mot de passe du domaine Active Directory associés au domaine que vous avez sélectionné. Ces informations d’identification permettront de joindre les machines virtuelles au domaine Active Directory.

      >[!NOTE]
      >Vérifiez que les noms des administrateurs sont conformes aux informations fournies à cette étape, et qu’aucune authentification MFA n’est activée sur le compte.

12. Pour le **Compte administrateur de la machine virtuelle**, entrez les informations du compte d’administrateur local que vous souhaitez utiliser pour toutes les machines virtuelles.

13. Sélectionnez l’onglet **Étiquette** si vous souhaitez regrouper les machines virtuelles avec des étiquettes. Sinon, ignorez cet onglet.

14. Sélectionnez l’onglet **Vérifier + créer**. Passez en revue vos choix et, si tout semble parfait, sélectionnez **Créer**.

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous avez développé votre pool d’hôtes existant, vous pouvez vous connecter à un client Azure Virtual Desktop pour les tester dans le cadre d’une session utilisateur. Vous pouvez vous connecter à une session avec l’un des clients suivants :

- [Se connecter avec le client Windows Desktop](./user-documentation/connect-windows-7-10.md)
- [Se connecter avec le client web](./user-documentation/connect-web.md)
- [Se connecter avec le client Android](./user-documentation/connect-android.md)
- [Se connecter avec le client macOS](./user-documentation/connect-macos.md)
- [Se connecter avec le client iOS](./user-documentation/connect-ios.md)
