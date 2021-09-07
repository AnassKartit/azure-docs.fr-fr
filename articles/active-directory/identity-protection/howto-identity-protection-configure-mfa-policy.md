---
title: Configurer la stratégie d’inscription de l’authentification multifacteur – Azure Active Directory Identity Protection
description: Découvrez comment configurer la stratégie d’inscription d’authentification multifacteur Azure AD Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 06/05/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d2bfaef8f00f6246dfef17145666c817ed26d9d
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122563143"
---
# <a name="how-to-configure-the-azure-ad-multi-factor-authentication-registration-policy"></a>Procédure : Configurer la stratégie d'inscription Azure AD Multi-Factor Authentication

Azure AD Identity Protection vous permet de gérer le déploiement de l'inscription Azure AD Multi-Factor Authentication (MFA) en configurant une stratégie d'accès conditionnel pour exiger l'inscription MFA, quelle que soit l'application d'authentification moderne à laquelle vous vous connectez.

## <a name="what-is-the-azure-ad-multi-factor-authentication-registration-policy"></a>Qu'est-ce qu'une stratégie d'inscription Azure AD Multi-Factor Authentication ?

Azure AD Multi-Factor Authentication permet de vérifier votre identité, à l'aide d'une méthode plus sécurisée que la simple fourniture d'un nom d'utilisateur et d'un mot de passe. Ce service fournit une deuxième couche de sécurité pour les connexions utilisateur. Pour que les utilisateurs puissent répondre aux invites d'authentification multifacteur, ils doivent d'abord s'inscrire à Azure AD Multi-Factor Authentication.

Voici les raisons pour lesquelles il est conseillé d'exiger une authentification Azure AD MFA pour les connexions utilisateur :

- Elle fournit une authentification renforcée avec un éventail d’options de vérification.
- Elle joue un rôle clé dans la correction automatique en cas de détections de risques dans Identity Protection.

Pour plus d'informations sur Azure AD Multi-Factor Authentication, consultez [Présentation d'Azure AD Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md).

## <a name="policy-configuration"></a>Configuration de la stratégie

1. Accédez au [portail Azure](https://portal.azure.com).
1. Accédez à **Azure Active Directory** > **Sécurité** > **Identity Protection** > **stratégie d’inscription MFA**.
   1. Sous **Affectations**
      1. **Utilisateurs** : choisissez **Tous les utilisateurs** ou **Sélectionner des personnes et des groupes** si vous limitez votre lancement.
         1. Si vous le souhaitez, vous pouvez exclure des utilisateurs de la stratégie.
   1. **Appliquer la stratégie** - **Activé**
   1. **Save**

## <a name="user-experience"></a>Expérience utilisateur

Azure Active Directory Identity Protection invite vos utilisateurs à s’inscrire la prochaine fois qu’ils se connectent de façon interactive, et ils disposent de 14 jours pour terminer l’inscription. Pendant cette période de 14 jours, les utilisateurs peuvent contourner l’inscription si l’authentification multifacteur n’est pas exigée. Toutefois, à la fin de la période, ils devront s’inscrire pour pouvoir aller au bout du processus de connexion.

Pour une obtenir une vue d’ensemble de l’expérience utilisateur, consultez :

- [Expériences de connexion avec Azure AD Identity Protection](concept-identity-protection-user-experience.md).  

## <a name="next-steps"></a>Étapes suivantes

- [Activer les stratégies de connexion et de risque utilisateur](howto-identity-protection-configure-risk-policies.md)

- [Activer la réinitialisation de mot de passe en libre-service Azure AD](../authentication/howto-sspr-deployment.md)

- [Activer l’authentification multifacteur Azure AD](../authentication/howto-mfa-getstarted.md)
