---
title: 'Azure AD Connect : Authentification unique transparente| Microsoft Docs'
description: Cette rubrique décrit l’authentification unique transparente Azure Active Directory (Azure AD) et explique comment cette fonction vous permet de fournir une véritable authentification unique aux utilisateurs du réseau d’entreprise.
services: active-directory
keywords: Qu’est-ce qu’Azure AD Connect, Installation d’Active Directory, Composants requis pour Azure AD, SSO, Authentification unique
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 08/13/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0cf360196b3fee73dc91ea7936f2a8faad2dedc5
ms.sourcegitcommit: 1f29603291b885dc2812ef45aed026fbf9dedba0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/29/2021
ms.locfileid: "129231631"
---
# <a name="azure-active-directory-seamless-single-sign-on"></a>Authentification unique transparente Azure Active Directory

## <a name="what-is-azure-active-directory-seamless-single-sign-on"></a>Qu’est-ce que l’authentification unique transparente Azure Active Directory ?

L’authentification unique transparente Azure Active Directory connecte automatiquement les utilisateurs lorsque leurs appareils d’entreprise sont connectés au réseau de l’entreprise. Lorsque cette fonctionnalité est activée, les utilisateurs n’ont plus besoin de taper leur mot de passe pour se connecter à Azure AD ni même, dans la plupart des cas, leur nom d’utilisateur. Cette fonctionnalité offre à vos utilisateurs un accès facilité à vos applications cloud sans nécessiter de composants locaux supplémentaires.

>[!VIDEO https://www.youtube.com/embed/PyeAC85Gm7w]

L’authentification unique transparente peut être combinée avec la [synchronisation de hachage de mot de passe](how-to-connect-password-hash-synchronization.md) et [l’authentification directe](how-to-connect-pta.md). L’authentification unique transparente n’est _pas_ applicable aux services de fédération Active Directory (AD FS).

![Authentification unique transparente](./media/how-to-connect-sso/sso1.png)

## <a name="sso-via-primary-refresh-token-vs-seamless-sso"></a>Authentification unique à l’aide d’un jeton d’actualisation principal ou authentification unique fluide

Pour Windows 10, Windows Server 2016 et les versions ultérieures, il est recommandé d’utiliser l’authentification unique à l’aide d’un jeton d’actualisation principal (PRT). Pour Windows 7 et Windows 8.1, il est recommandé d’utiliser l’authentification unique transparente.
L’authentification unique fluide a besoin que l’appareil de l’utilisateur soit joint à un domaine, mais cette propriété n’est pas utilisée sur les appareils Windows 10 avec [jointure Azure AD](../devices/concept-azure-ad-join.md) ou [jointure hybride Azure AD](../devices/concept-azure-ad-join-hybrid.md). L’authentification unique sur des appareils avec jointure Azure AD, avec jointure hybride Azure AD et inscrits dans Azure AD fonctionne selon le [jeton d’actualisation principal (PRT)](../devices/concept-primary-refresh-token.md)

L’authentification unique via PRT fonctionne une fois que les appareils sont inscrits auprès d’Azure AD pour les appareils Azure AD avec jointure hybride, les appareils avec jointure Azure AD ou les appareils personnels inscrits via Ajouter un compte professionnel ou scolaire. Pour plus d’informations sur le fonctionnement de l’authentification unique avec Windows 10 à l’aide de PRT, consultez : [Jeton d’actualisation principal (PRT) et Azure AD](../devices/concept-primary-refresh-token.md)


## <a name="key-benefits"></a>Principaux avantages

- *Une meilleure expérience utilisateur*
  - Les utilisateurs sont automatiquement connectés aux applications cloud et locales.
  - Les utilisateurs n’ont pas à saisir leur mot de passe plusieurs fois.
- *Facile à déployer et à gérer*
  - Aucun composant local supplémentaire n’est nécessaire pour que cela fonctionne.
  - Fonctionne avec n’importe quelle méthode d’authentification cloud - [Synchronisation de hachage de mot de passe](how-to-connect-password-hash-synchronization.md) ou [Authentification directe](how-to-connect-pta.md).
  - Peut être déployée pour certains utilisateurs ou pour l’ensemble de vos utilisateurs à l’aide d’une stratégie de groupe.
  - Inscrivez des appareils non-Windows 10 auprès d’Azure AD sans avoir besoin d’une infrastructure AD FS. Cette fonctionnalité exige que vous utilisiez la version 2.1 ou supérieure du [client workplace-join](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="feature-highlights"></a>Présentation des fonctionnalités

- Le nom d’utilisateur peut être soit le nom d’utilisateur local par défaut (`userPrincipalName`), soit un autre attribut configuré dans Azure AD Connect (`Alternate ID`). Les deux cas d’utilisation fonctionnent car l’authentification unique transparente utilise la revendication `securityIdentifier` dans le ticket Kerberos pour rechercher l’objet utilisateur correspondant dans Azure AD.
- L’authentification unique transparente est une fonctionnalité opportuniste. Si elle échoue pour une raison quelconque, l’expérience de connexion utilisateur retrouve son comportement normal (l’utilisateur doit alors entrer son mot de passe dans la page de connexion).
- Si une application (par exemple, `https://myapps.microsoft.com/contoso.com`) transfère un paramètre `domain_hint` (OpenID Connect) ou `whr` (SAML), correspondant à votre locataire, ou encore un paramètre `login_hint`, correspondant à l’utilisateur, dans sa requête de connexion Azure AD, les utilisateurs sont automatiquement connectés, sans qu’ils n’aient à entrer leurs nom d’utilisateur et mot de passe.
- Les utilisateurs obtiennent également une expérience de connexion silencieuse si une application (par exemple, `https://contoso.sharepoint.com`) envoie des demandes de connexion aux points de terminaison d'Azure AD définis en tant que locataires ; c'est-à-dire, `https://login.microsoftonline.com/contoso.com/<..>` ou `https://login.microsoftonline.com/<tenant_ID>/<..>` au lieu du point de terminaison commun d’Azure AD ; c'est-à-dire, `https://login.microsoftonline.com/common/<...>`.
- La déconnexion est prise en charge. Cela permet aux utilisateurs de choisir le compte Azure AD auquel se connecter, au lieu d’être connecté automatiquement à l’aide de l’authentification unique transparente.
- Les clients Win32 Microsoft 365 (Outlook, Word, Excel, etc.) dotés des versions 16.0.8730.xxxx et ultérieures sont pris en charge au moyen d’un flux non interactif. En ce qui concerne OneDrive, vous devez activer la [fonctionnalité de configuration silencieuse OneDrive](https://techcommunity.microsoft.com/t5/Microsoft-OneDrive-Blog/Previews-for-Silent-Sync-Account-Configuration-and-Bandwidth/ba-p/120894) pour une utilisation de l’authentification sans assistance.
- L’authentification unique transparente peut être activée par le biais d’Azure AD Connect.
- Cette fonctionnalité est gratuite et il est inutile de disposer des éditions payantes d’Azure AD pour l’utiliser.
- L’authentification unique est prise en charge par les clients basés sur le navigateur web et les clients Office qui prennent en charge [l’authentification moderne](/office365/enterprise/modern-auth-for-office-2013-and-2016) sur les plateformes et navigateurs compatibles avec l’authentification Kerberos :

| Système d’exploitation\Navigateur |Internet Explorer|Microsoft Edge\*\*\*\*|Google Chrome|Mozilla Firefox|Safari|
| --- | --- |--- | --- | --- | -- 
|Windows 10|Oui\*|Oui|Oui|Oui\*\*\*|N/A
|Windows 8.1|Oui\*|Oui*\*\*\*|Oui|Oui\*\*\*|N/A
|Windows 8|Oui\*|N/A|Oui|Oui\*\*\*|N/A
|Windows Server 2012 R2 ou version ultérieure|Oui\*\*|N/A|Oui|Oui\*\*\*|N/A
|Mac OS X|N/A|N/A|Oui\*\*\*|Oui\*\*\*|Oui\*\*\*

 > [!NOTE]
 >Microsoft Edge (hérité) n’est plus pris en charge.


\*Requiert Internet Explorer version 11 ou ultérieure. ([À compter du 17 août 2021, les applications et services Microsoft 365 ne prendront pas en charge IE 11](https://techcommunity.microsoft.com/t5/microsoft-365-blog/microsoft-365-apps-say-farewell-to-internet-explorer-11-and/ba-p/1591666).)

\*\*Requiert Internet Explorer version 11 ou ultérieure. Désactivez le mode protégé amélioré.

\*\*\*Nécessite une [configuration supplémentaire](how-to-connect-sso-quick-start.md#browser-considerations).

\*\*\*\*Microsoft Edge basé sur Chromium

## <a name="next-steps"></a>Étapes suivantes

- [**Démarrage rapide**](how-to-connect-sso-quick-start.md) : découvrez l’authentification unique transparente Azure AD.
- [**Plan de déploiement**](../manage-apps/plan-sso-deployment.md) : plan de déploiement étape par étape.
- [**Immersion technique**](how-to-connect-sso-how-it-works.md) : découvrez comment fonctionne cette fonctionnalité.
- [**Questions fréquentes (FAQ)**](how-to-connect-sso-faq.yml) : réponses aux questions fréquentes.
- [**Résolution des problèmes**](tshoot-connect-sso.md) : découvrez comment résoudre les problèmes courants susceptibles de survenir avec cette fonctionnalité.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) : pour le dépôt de nouvelles demandes de fonctionnalités.
