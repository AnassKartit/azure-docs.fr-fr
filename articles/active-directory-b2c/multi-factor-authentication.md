---
title: Multi-Factor Authentication dans Azure Active Directory B2C | Microsoft Docs
description: Activation de Multi-Factor Authentication dans des applications accessibles aux consommateurs sécurisées par Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/10/2021
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
zone_pivot_groups: b2c-policy-type
ms.openlocfilehash: 42feac542760bbebc703cabc4ecc114b0ab4259d
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122531536"
---
# <a name="enable-multi-factor-authentication-in-azure-active-directory-b2c"></a>Activer l'authentification multifacteur dans Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-choose-user-flow-or-custom-policy](../../includes/active-directory-b2c-choose-user-flow-or-custom-policy.md)]

Azure Active Directory B2C (Azure AD B2C) s’intègre directement à [Azure AD Multi-Factor Authentication](../active-directory/authentication/concept-mfa-howitworks.md) pour vous permettre d’ajouter une deuxième couche de sécurité aux expériences d’inscription et de connexion dans vos applications. Vous activez l’authentification multifacteur sans écrire la moindre ligne de code. Si vous déjà créé des flux d’utilisateur d’inscription et de connexion, vous pouvez toujours activer l’authentification multifacteur.

Cette fonctionnalité permet aux applications de gérer des scénarios tels que :

- Vous n’avez pas besoin de l'authentification multifacteur pour accéder à une application, mais en avez besoin pour une autre. Par exemple, le client peut se connecter à une application d’assurance automobile avec un compte local ou social, mais il doit confirmer le numéro de téléphone avant d’accéder à l’application d’assurance habitation inscrite dans le même annuaire.
- Vous n’avez pas besoin l'authentification multifacteur pour accéder à une application en général, mais en avez besoin pour accéder à des portions sensibles de celle-ci. Par exemple, le client peut se connecter à une application bancaire avec un compte local ou social pour consulter le solde de son compte, mais il doit confirmer le numéro de téléphone avant d’effectuer un virement.

### <a name="verification-methods"></a>Méthodes de vérification

Grâce à l’[accès conditionnel](conditional-access-identity-protection-overview.md), les utilisateurs peuvent être amenés à s’authentifier par l’intermédiaire de l’authentification multifacteur selon les choix de configuration que vous avez faits en tant qu’administrateur. Les méthodes de l’authentification multifacteur sont les suivantes :

- Courrier
- sms
- Appels téléphoniques

## <a name="set-multi-factor-authentication"></a>Définir l'authentification multifacteur

::: zone pivot="b2c-user-flow"

1. Connectez-vous au [portail Azure](https://portal.azure.com)
1. Utilisez le filtre **Annuaire + abonnement** dans le menu du haut pour sélectionner l’annuaire qui contient votre locataire Azure AD B2C.
1. Dans le menu de gauche, sélectionnez **Azure AD B2C**. Ou sélectionnez **Tous les services**, puis recherchez et sélectionnez **Azure AD B2C**.
1. Sélectionnez **Flux d’utilisateurs**.
1. Sélectionnez le flux d’utilisateurs pour lequel vous souhaitez activer l’authentification multifacteur. Par exemple, *B2C_1_signinsignup*.
1. Sélectionner **Propriétés**.
1. Dans la section **Authentification multifacteur**, sélectionnez le **type de méthode** souhaité. Puis, sous **Mise en application de l’authentification multifacteur**, sélectionnez une option :

   - **Désactivé** : L’authentification multifacteur n’est jamais appliquée pendant la connexion, et les utilisateurs ne sont pas invités à s’inscrire à l’authentification multifacteur dans le cadre de l’inscription ou de la connexion.
   - **Toujours activé** – MFA est toujours requise, quelle que soit votre configuration de l’accès conditionnel. Lors de l’inscription, les utilisateurs sont invités à s’inscrire dans MFA. Lors de la connexion, si les utilisateurs ne sont pas déjà inscrits à l’authentification multifacteur, ils sont invités à s’y inscrire.
   - **Conditionnel** – Lors de l’inscription et de la connexion, les utilisateurs sont invités à s’inscrire à l’authentification MFA (à la fois les nouveaux utilisateurs et les utilisateurs existants qui ne sont pas inscrits à l’authentification MFA). Lors de la connexion, l’authentification MFA est appliquée uniquement quand une évaluation de la stratégie d’accès conditionnel active l’exige :

    - Si le résultat est un défi d’authentification MFA sans risque, l’authentification MFA est appliquée. Si l’utilisateur n’est pas déjà inscrit à l’authentification multifacteur, il est invité à s’y inscrire.
    - Si le résultat est un défi d’authentification MFA en raison d’un risque *et* que l’utilisateur n’est pas inscrit dans MFA, la connexion est bloquée.

   > [!NOTE]
   >
   > - Avec la disponibilité générale de l’accès conditionnel dans Azure AD B2C, les utilisateurs sont désormais invités à s’inscrire à une méthode d’authentification multifacteur lors de l’inscription. Les flux d’utilisateur d’inscription que vous avez créés avant la mise à disposition générale ne reflètent pas automatiquement ce nouveau comportement, mais vous pouvez inclure le comportement en créant des flux d’utilisateurs.
   > - Si vous sélectionnez **Conditionnel**, vous devez également [ajouter l’accès conditionnel à des flux d’utilisateurs](conditional-access-user-flow.md) et spécifier les applications auxquelles la stratégie doit s’appliquer.
   > - L’authentification multifacteur (MFA) est désactivée par défaut pour les flux d’utilisateurs d’inscription. Vous pouvez activer la MFA dans les flux d’utilisateurs avec l’inscription par téléphone, mais, comme un numéro de téléphone est utilisé comme identificateur principal, l’envoi par e-mail d’un code secret à usage unique est la seule option disponible pour le deuxième facteur d’authentification.

1. Sélectionnez **Enregistrer**. La MFA est maintenant activée pour ce flux d’utilisateurs.

Vous pouvez utiliser **Exécuter maintenant** sur le flux d’utilisateur à des fins de vérification. Vérifiez le scénario suivant :

Un compte client est créé dans votre locataire avant l’étape d'authentification multifacteur. Lors de cette étape, le client est invité à fournir un numéro de téléphone et à le confirmer. Si la vérification aboutit, le numéro de téléphone est associé au compte en vue d’une utilisation ultérieure. Même si le client annule ou abandonne, il peut être invité à confirmer à nouveau un numéro de téléphone lors de la connexion suivante avec activation de l'authentification multifacteur.

::: zone-end

::: zone pivot="b2c-custom-policy"

Pour activer l’authentification multifacteur, récupérez les packs de démarrage de stratégies personnalisées sur GitHub, puis mettez à jour les fichiers XML du pack de démarrage **SocialAndLocalAccountsWithMFA** avec le nom de votre locataire Azure AD B2C. **SocialAndLocalAccountsWithMFA** permet d’activer les options d’authentification sociale, locale et multifacteur. Pour plus d’informations, consultez [Bien démarrer avec les stratégies personnalisées dans Active Directory B2C](tutorial-create-user-flows.md?pivots=b2c-custom-policy). 

::: zone-end
