---
title: Réinitialiser des mots de passe pour des machines virtuelles dans Azure Lab Services | Microsoft Docs
description: Découvrez comment réinitialiser les mots de passe de machines virtuelles (VM) dans les labos d’Azure Lab Services.
ms.topic: how-to
ms.date: 06/26/2020
ms.openlocfilehash: ca311c08419ff54fdfe72b20a2a2bffbc436d1dd
ms.sourcegitcommit: 92889674b93087ab7d573622e9587d0937233aa2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2021
ms.locfileid: "130180131"
---
# <a name="set-or-reset-password-for-virtual-machines-in-labs-students"></a>Définition ou réinitialisation du mot de passe de machines virtuelles dans des labos (étudiants)
Cet article explique comment les étudiants peuvent définir/réinitialiser le mot de passe de leurs machines virtuelles. 

## <a name="enable-resetting-of-passwords"></a>Activer la réinitialisation des mots de passe
Au moment de la création du labo, son propriétaire peut activer ou désactiver l’option **Use same password for all virtual machines** (Utiliser le même mot de passe pour toutes les machines virtuelles). Si cette option est activée, les étudiants ne peuvent pas réinitialiser le mot de passe. Toutes les machines virtuelles des labos ont le même mot de passe, défini par le formateur. 

Si cette option est désactivée, les utilisateurs doivent définir un mot de passe lorsqu’ils tentent de se connecter à la machine virtuelle pour la première fois. Les étudiants peuvent également réinitialiser le mot de passe ultérieurement, comme expliqué dans la dernière section de cet article. 

## <a name="reset-password-for-the-first-time"></a>Réinitialisation du mot de passe pour la première fois
Si l’option **Utiliser le même mot de passe pour toutes les machines virtuelles** est désactivée, lorsque les utilisateurs (étudiants) sélectionnent le bouton **Connexion** sur la vignette du labo de la page **Mes machines virtuelles**, l’utilisateur voit apparaître la boîte de dialogue suivante, qui permet de définir le mot de passe d’accès à la machine virtuelle : 

![Réinitialisation du mot de passe par l’étudiant](./media/how-to-set-virtual-machine-passwords/student-set-password.png)

## <a name="reset-password-later"></a>Réinitialiser le mot de passe plus tard
L’étudiant peut également définir le mot de passe en cliquant sur le menu de dépassement de capacité (représenté par **trois points verticaux**) sur la mosaïque du labo, et en sélectionnant **Réinitialiser le mot de passe**. 

![Réinitialiser le mot de passe plus tard](./media/how-to-set-virtual-machine-passwords/student-set-password-2.png)


## <a name="next-steps"></a>Étapes suivantes
Pour en savoir plus sur les autres options d’utilisation que le propriétaire du labo peut configurer pour les étudiants, consultez l’article suivant : [Configure student usage](how-to-configure-student-usage.md) (Configurer l’utilisation pour les étudiants).
