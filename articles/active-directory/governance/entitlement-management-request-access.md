---
title: Demander un package d’accès dans la gestion des droits d’utilisation Azure AD
description: Découvrez comment utiliser le portail Mon Accès pour demander l’accès à un package d’accès dans la gestion des droits d’utilisation Azure Active Directory.
services: active-directory
documentationCenter: ''
author: ajburnle
manager: daveba
editor: mamtakumar
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 08/31/2021
ms.author: ajburnle
ms.reviewer: mamkumar
ms.collection: M365-identity-device-management
ms.openlocfilehash: 945679db60f78e03d8f4385acdbc97d8155922bb
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2021
ms.locfileid: "123434481"
---
# <a name="request-access-to-an-access-package-in-azure-ad-entitlement-management"></a>Demander l’accès à un package d’accès dans la gestion des droits d’utilisation Azure AD

Avec la gestion des droits d'utilisation d’Azure AD, un package d’accès vous permet d’effectuer une configuration unique des ressources et des stratégies qui gèrent automatiquement l’accès pendant toute la durée de vie du package d’accès. 

Un gestionnaire de package d’accès peut configurer des stratégies pour exiger une approbation pour que les utilisateurs aient accès aux packages d’accès. Un utilisateur qui a besoin d’accéder à un package d’accès peut envoyer une demande d’accès. Cet article explique comment envoyer une demande d’accès.

## <a name="sign-in-to-the-my-access-portal"></a>Se connecter au portail Mon Accès

La première étape consiste à se connecter au portail Mon Accès à partir duquel vous pouvez demander l’accès à un package d’accès.

**Rôle prérequis :** Demandeur

1. Recherchez un e-mail ou un message de la part du chef de projet ou du directeur commercial avec lequel vous travaillez. L’e-mail doit inclure un lien vers le package d’accès auquel vous devrez accéder. Le lien commence par `myaccess`, comprend une indication de répertoire et se termine par un ID de package d’accès.  (pour US Government, le domaine peut être `https://myaccess.microsoft.us`)
 
    `https://myaccess.microsoft.com/@<directory_hint>#/access-packages/<access_package_id>`

1. Ouvrez le lien.

1. Connectez-vous au portail Mon Accès.

    Veillez à utiliser le compte de votre organisation (professionnel ou scolaire). Si vous n’êtes pas sûr, vérifiez auprès de votre chef de projet ou de votre directeur commercial.

## <a name="request-an-access-package"></a>Demander un package d’accès

Une fois que vous avez trouvé le package d’accès dans le portail Mon Accès, vous pouvez soumettre une demande.

**Rôle prérequis :** Demandeur

1. Recherchez le package d’accès dans la liste.  Si nécessaire, vous pouvez effectuer une recherche en tapant une chaîne de recherche, puis en sélectionnant le filtre **Nom**, **Catalogue** ou **Ressources**.

    ![Portail Mon Accès - Recherche de ressource](./media/entitlement-management-request-access/my-access-resource-search.png)

1. Cochez la case pour sélectionner le package d’accès.

1. Cliquez sur **Demander l’accès** pour ouvrir le volet Demander l’accès.

    ![Portail Mon Accès - Packages d’accès](./media/entitlement-management-request-access/my-access-request-access-button.png)

1. Si la case **Justification métier** s’affiche, fournissez une justification pour obtenir l’accès.

1. Si la case **Demander pendant une période spécifique ?** est activée, sélectionnez **Oui** ou **Non**.

1. Si nécessaire, spécifiez la date de début et la date de fin.

    ![Portail Mon Accès - Demander l’accès](./media/entitlement-management-shared/my-access-request-access.png)

1. Lorsque vous avez terminé, cliquez sur **Envoyer** pour envoyer votre demande.

1. Cliquez sur **Historique des demandes** pour afficher la liste de vos demandes et leur état.

    Si le package d’accès nécessite une approbation, l’état de la demande est désormais en attente d’approbation.

### <a name="select-a-policy"></a>Sélectionner une stratégie

Si vous demandez l’accès à un package d’accès auquel plusieurs stratégies s’appliquent, il pourra vous être demandé de sélectionner une stratégie. Par exemple, un gestionnaire de package d’accès peut configurer un package d’accès avec deux stratégies pour deux groupes d’employés internes. La première stratégie peut autoriser l’accès pendant 60 jours et nécessiter une approbation. La deuxième stratégie peut autoriser l’accès pendant 2 jours et ne nécessiter aucune approbation. Si vous rencontrez ce scénario, vous devez sélectionner la stratégie que vous souhaitez utiliser.

![Portail Mon Accès – Demander l’accès – plusieurs stratégies](./media/entitlement-management-request-access/my-access-multiple-policies.png)

### <a name="fill-out-requestor-information"></a>Renseigner les informations relatives au demandeur

Vous pouvez demander à accéder à un package d’accès qui requiert une justification métier et des informations supplémentaires sur le demandeur avant de vous autoriser à accéder au package d’accès. Renseignez toutes les informations relatives au demander pour accéder au package d’accès.

![Portail Mon accès - Demander l’accès - Renseigner les informations relatives au demandeur](./media/entitlement-management-request-access/my-access-requestor-information.png)

> [!NOTE]
> Vous remarquerez peut-être que certaines informations sur le demandeur ont des valeurs préremplies. Cela se produit généralement si votre compte a déjà des informations d’attribut définies à partir d’une demande ou d’un autre processus antérieurs. Ces valeurs peuvent être modifiables ou non en fonction des paramètres de la stratégie sélectionnée.

## <a name="resubmit-a-request"></a>Resoumettre une demande

Lorsque vous demandez l’accès à un package d’accès, votre demande peut être refusée ou expirer si les approbateurs ne répondent pas dans le délai imparti. Si vous avez besoin de cet accès, vous pouvez réessayer de soumettre votre demande. La procédure suivante explique comment resoumettre une demande d’accès :

**Rôle prérequis :** Demandeur

1. Connectez-vous au portail **Mon Accès**.

1. Cliquez sur **Historique des demandes** dans le menu de navigation à gauche.

1. Recherchez le package d’accès pour lequel vous resoumettre une demande.

1. Cliquez sur la coche pour sélectionner le package d’accès.

1. Cliquez sur le lien **Afficher** en bleu à droite du package d’accès sélectionné.
    
    ![Sélectionner un package d’accès et afficher le lien](./media/entitlement-management-request-access/resubmit-request-select-request-and-view.png)

    Un volet s’ouvre à droite, affichant l’Historique des demandes pour le package d’accès.
    
    ![Sélectionner le bouton Soumettre à nouveau](./media/entitlement-management-request-access/resubmit-request-select-resubmit.png)

1. Cliquez sur le bouton **Soumettre à nouveau** en bas du volet.

## <a name="cancel-a-request"></a>Annuler une demande

Si vous envoyez une demande d’accès et que l’état de la demande est toujours **En attente d’approbation**, vous pouvez annuler la demande.

**Rôle prérequis :** Demandeur

1. Dans le portail Mon Accès, cliquez sur **Historique des demandes**, sur la gauche, pour afficher la liste de vos demandes et leur état.

1. Cliquez sur le lien **Affichage** pour la demande que vous souhaitez annuler.

1. Si l’état de la demande est toujours **En attente d’approbation**, vous pouvez cliquer sur **Annuler la demande** pour annuler la demande.

    ![Portail Mon Accès - Annuler la demande](./media/entitlement-management-request-access/my-access-cancel-request.png)

1. Cliquez sur **Historique des demandes** pour confirmer que la demande a bien été annulée.

## <a name="next-steps"></a>Étapes suivantes

- [Approuver ou refuser les demandes d’accès](entitlement-management-request-approve.md)
- [Processus de requête et notifications par e-mail](entitlement-management-process.md)