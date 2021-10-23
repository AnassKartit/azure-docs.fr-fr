---
title: Formateur accédant à des machines virtuelles dans Azure Lab Services
description: Cet article explique comment les formateurs peuvent accéder aux machines virtuelles des étudiants à partir de l’affichage formateur. Par exemple, un assistant peut être formateur pour une classe, mais étudiant pour d’autres.
ms.topic: how-to
ms.date: 06/26/2020
ms.openlocfilehash: d20d95f7f86e3116d9daf42417a3a0aae5855bbb
ms.sourcegitcommit: 92889674b93087ab7d573622e9587d0937233aa2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2021
ms.locfileid: "130179637"
---
# <a name="access-virtual-machines-as-a-student-from-the-educator-view"></a>Accéder aux machines virtuelles en tant qu’étudiant depuis l’affichage formateur
Cet article explique aux formateurs comment accéder aux machines virtuelles pour les classes auxquelles ils participent en tant qu’étudiants. 

Voici un scénario illustrant l'utilité de cette fonctionnalité. Un assistant est formateur pour un classe, mais étudiant pour d’autres. Et le formateur souhaite afficher les machines virtuelles des étudiants et y accéder à partir de l’affichage formateur montrant les laboratoires dont il est propriétaire. 

## <a name="access-vms-from-educator-view"></a>Accéder aux machines virtuelles à partir de l’affichage formateur

1. Connectez-vous au site web d’[Azure Lab Services](https://labs.azure.com). Vous voyez les labs dont vous êtes propriétaire. Ces labs peuvent être des labs que vous avez créés vous-même ou des labs que l’administrateur vous a affectés en tant que propriétaire. Pour plus d'informations, consultez [Guide pratique pour ajouter des propriétaires à un labo existant](how-to-add-user-lab-owner.md).
2. Pour accéder aux machines virtuelles pour les classes auxquelles vous participez en tant qu’étudiant, sélectionnez l’icône d'ordinateur dans l’angle supérieur droit. Assurez-vous de voir les machines virtuelles auxquelles vous pouvez accéder en tant qu’étudiant. Dans l’exemple suivant, l’utilisateur est assistant d'enseignement du lab Python, mais étudiant du lab Java. L’utilisateur voit donc la machine virtuelle du lab Java dans la liste déroulante. L’utilisateur peut démarrer la machine virtuelle et s’y connecter. 
    
    ![Accéder aux machines virtuelles des étudiants](./media/instructors-access-virtual-machines/access-student-virtual-machines.png)

## <a name="next-steps"></a>Étapes suivantes
Voir les articles suivants :

- [Se connecter à une machine virtuelle](how-to-use-classroom-lab.md#connect-to-the-vm)
- [Se connecter à une machine virtuelle avec RDP sur un Mac](connect-virtual-machine-mac-remote-desktop.md)
- [Se connecter à une machine virtuelle à l’aide de RDP sur un Chromebook](connect-virtual-machine-chromebook-remote-desktop.md)
- [Utiliser le Bureau à distance pour les machines virtuelles Linux](how-to-use-remote-desktop-linux-student.md)
