---
title: Connexion sans mot de passe à Azure Active Directory
description: Découvrir les options de connexion sans mot de passe à Azure Active Directory à l’aide de clés de sécurité FIDO2 ou de l’application Microsoft Authenticator
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 06/28/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6cc7cfdf0c7cf578ea12bc1acf2c572445dd864c
ms.sourcegitcommit: bee590555f671df96179665ecf9380c624c3a072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/07/2021
ms.locfileid: "129668368"
---
# <a name="passwordless-authentication-options-for-azure-active-directory"></a>Options d’authentification sans mot de passe pour Azure Active Directory

Les fonctionnalités telles que l’authentification multifacteur (MFA) sont un excellent moyen de sécuriser votre organisation, mais les utilisateurs sont souvent frustrés par la couche de sécurité supplémentaire qui s’ajoute à la nécessité de se souvenir de leurs mots de passe. Les méthodes d’authentification sans mot de passe sont plus pratiques, car le mot de passe est supprimé et remplacé par quelque chose que vous avez, en plus de quelque chose que vous êtes ou que vous savez.

| Authentification  | Quelque chose que vous avez | Quelque chose que vous êtes ou savez |
| --- | --- | --- |
| Sans mot de passe | Appareil Windows 10, téléphone ou clé de sécurité | Biométrie ou code confidentiel |

Chaque organisation a des besoins différents en matière d’authentification. L’offre globale Microsoft Azure et Azure Government proposent les trois options d’authentification sans mot de passe suivantes qui s’intègrent à Azure Active Directory (Azure AD) :

- Windows Hello Entreprise
- Application Microsoft Authenticator
- Clés de sécurité FIDO2

![Authentification : Sécurité et commodité](./media/concept-authentication-passwordless/passwordless-convenience-security.png)

## <a name="windows-hello-for-business"></a>Windows Hello Entreprise

Windows Hello Entreprise est idéal pour les professionnels de l’information qui disposent de leur propre PC Windows. Les informations d’identification biométriques et de code secret sont directement liées au PC de l’utilisateur et, dès lors, personne d’autre ne peut y accéder. Avec l’intégration de l’infrastructure à clé publique (PKI)et la prise en charge intégrée de l’authentification unique (SSO), Windows Hello Entreprise propose une méthode d’accès pratique aux ressources d’entreprise localement et dans le cloud.

![Exemple de connexion d’un utilisateur à Windows Hello Entreprise](./media/concept-authentication-passwordless/windows-hellow-sign-in.jpeg)

Les étapes suivantes montrent la manière dont le processus de connexion fonctionne avec Azure AD :

![Diagramme décrivant les étapes nécessaires à la connexion des utilisateurs avec Windows Hello Entreprise](./media/concept-authentication-passwordless/windows-hello-flow.png)

1. Un utilisateur se connecte à Windows l’aide d'un geste biométrique ou d'un code confidentiel. Cette opération déverrouille la clé privée Windows Hello Entreprise et est transmise au fournisseur SSP (Security Support Provider) d'authentification cloud, appelé *fournisseur Cloud AP*.
1. Le fournisseur Cloud AP demande un nonce (un nombre arbitraire aléatoire qui ne peut être utilisé qu’une seule fois) à Azure AD.
1. Azure AD renvoie un nonce qui est valide pendant 5 minutes.
1. Le fournisseur Cloud AP signe le nonce à l’aide de la clé privée de l’utilisateur et le renvoie signé à Azure AD.
1. Azure AD vérifie le nonce signé à l’aide de la clé publique de l'utilisateur par rapport à la signature du nonce. Une fois la signature validée, Azure AD valide le nonce signé renvoyé. Après avoir validé le nonce, Azure AD crée un jeton d’actualisation principal (PRT) avec la clé de session chiffrée sur la clé de transport de l’appareil et le retourne au fournisseur Cloud AP.
1. Le fournisseur Cloud AP reçoit le PRT chiffré avec la clé de session. Le fournisseur Cloud AP utilise la clé de transport privée de l'appareil pour déchiffrer la clé de session et protège cette dernière à l’aide du module de plateforme sécurisée (TPM) de l’appareil.
1. Le fournisseur Cloud AP renvoie une réponse d’authentification réussie à Windows. L’utilisateur peut alors accéder aux applications Windows, ainsi qu’aux applications cloud et locales sans avoir à s’authentifier à nouveau (SSO).

Le [guide de planification](/windows/security/identity-protection/hello-for-business/hello-planning-guide) Windows Hello Entreprise peut vous aider à déterminer le type de déploiement de Windows Hello Entreprise dont vous avez besoin, ainsi que les options à prendre en compte.

## <a name="microsoft-authenticator-app"></a>Application Microsoft Authenticator

Vous pouvez également autoriser le téléphone de votre employé à devenir une méthode d’authentification sans mot de passe. Vous utilisez peut-être déjà l’application Microsoft Authenticator comme une option pratique d’authentification multifacteur en plus d’un mot de passe. Vous pouvez également utiliser l’application Authenticator comme option sans mot de passe.

![Connectez-vous à Microsoft Edge avec l’application Microsoft Authenticator](./media/concept-authentication-passwordless/concept-web-sign-in-microsoft-authenticator-app.png)

L’application Authenticator transforme n’importe quel téléphone iOS ou Android en informations d’identification fortes et sans mot de passe. Les utilisateurs peuvent se connecter à toute plateforme ou tout navigateur en obtenant une notification sur leur téléphone, en faisant correspondre un numéro affiché sur l’écran à celui affiché sur leur téléphone, puis en utilisant leur leurs données biométriques (toucher ou visage) ou un code confidentiel pour confirmer. Pour plus d’informations sur l’installation, consultez [Télécharger et installer l’application Microsoft Authenticator](https://support.microsoft.com/account-billing/download-and-install-the-microsoft-authenticator-app-351498fc-850a-45da-b7b6-27e523b8702a).

L’authentification sans mot de passe à l’aide de l’application Authenticator suit le même modèle de base que Windows Hello Entreprise. C’est un peu plus compliqué lorsque l’utilisateur a besoin d’être identifié afin qu’Azure AD puisse trouver la version de l’application Microsoft Authenticator en cours d’utilisation :

![Diagramme décrivant les étapes nécessaires à la connexion de l’utilisateur avec l’application Microsoft Authenticator](./media/concept-authentication-passwordless/authenticator-app-flow.png)

1. L'utilisateur entre son nom d’utilisateur.
1. Azure AD détecte que l'utilisateur dispose d'informations d'identification fortes et démarre le flux correspondant.
1. Une notification est envoyée à l’application via Apple Push Notification Service (APNS) sur les appareils iOS ou Firebase Cloud Messaging (FCM) sur les appareils Android.
1. L'utilisateur reçoit la notification Push et ouvre l'application.
1. L’application appelle Azure AD et reçoit un test de preuve de présence et un nonce.
1. L’utilisateur répond au test à l'aide en entrant un code biométrique ou un code confidentiel pour déverrouiller la clé privée.
1. Le nonce est signé avec la clé privée, puis renvoyé à Azure AD.
1. Azure AD procède à la validation des clés publique/privée et renvoie un jeton.

Pour prendre en main la connexion sans mot de passe, procédez comme suit :

> [!div class="nextstepaction"]
> [Activer la connexion sans mot de passe à l’aide de l’application Authenticator](howto-authentication-passwordless-phone.md)

## <a name="fido2-security-keys"></a>Clés de sécurité FIDO2

La FIDO (Fast Identity Online) Alliance permet de promouvoir les normes d’authentification ouverte et de réduire l’utilisation des mots de passe en tant qu’authentification. FIDO2 est la dernière norme qui incorpore la norme d’authentification Web (WebAuthn).

Les clés de sécurité FIDO2 sont une méthode d’authentification sans mot de passe basée sur une norme. Elles ne peuvent pas être usurpées et peuvent s’afficher dans n’importe quel facteur de forme. Fast Identity Online (FIDO) est une norme ouverte d’authentification sans mot de passe. Elle permet aux utilisateurs et aux organisations de tirer parti de la norme pour se connecter à leurs ressources sans nom d’utilisateur ou mot de passe, en utilisant une clé de sécurité externe ou une clé de plateforme intégrée à un appareil.

Les utilisateurs peuvent enregistrer, puis sélectionner une clé de sécurité FIDO2 dans l’interface de connexion en tant que principal moyen d’authentification. Ces clés de sécurité FIDO2 sont généralement des périphériques USB, mais elles peuvent également utiliser les technologies Bluetooth ou NFC. Avec un périphérique matériel gérant l’authentification, la sécurité d’un compte augmente car il n’existe aucun mot de passe qui pourrait être exposé ou deviné.

Les clés de sécurité FIDO2 peuvent être utilisées pour se connecter à leurs appareils Windows 10 joints à Azure AD ou à Azure AD hybride et bénéficier de l’authentification unique sur leurs ressources cloud et locales. Les utilisateurs peuvent également se connecter aux navigateurs pris en charge. Les clés de sécurité FIDO2 constituent une excellente solution pour les entreprises qui sont très sensibles à la sécurité ou ayant des scénarios ou des employés qui ne sont pas prêts à ou capables d’utiliser leur téléphone comme deuxième facteur.

Nous proposons un document de référence sur [les navigateurs qui prennent en charge l’authentification FIDO2 avec Azure AD](fido2-compatibility.md) ainsi que les meilleures pratiques pour les développeurs qui souhaitent [prendre en charge l’authentification FIDO2 dans les applications qu’ils développent](../develop/support-fido2-authentication.md).

![Connectez-vous à Microsoft Edge avec une clé de sécurité](./media/concept-authentication-passwordless/concept-web-sign-in-security-key.png)

Le processus suivant est utilisé lorsqu’un utilisateur se connecte avec une clé de sécurité FIDO2 :

![Diagramme décrivant les étapes nécessaires à la connexion de l’utilisateur avec une clé de sécurité FIDO2](./media/concept-authentication-passwordless/fido2-security-key-flow.png)

1. L’utilisateur connecte la clé de sécurité FIDO2 à son ordinateur.
2. Windows détecte la clé de sécurité FIDO2.
3. Windows envoie une requête d’authentification.
4. Azure AD renvoie un nonce.
5. L’utilisateur fait le geste qui convient pour déverrouiller la clé privée stockée dans l’enclave sécurisée de la clé de sécurité FIDO2.
6. La clé de sécurité FIDO2 signe le nonce avec la clé privée.
7. La demande de jeton d'actualisation principal (PRT) contenant le nonce signé est envoyée à Azure AD.
8. Azure AD vérifie le nonce signé à l’aide de la clé publique FIDO2.
9. Azure AD renvoie le PRT pour activer l’accès aux ressources locales.

Bien qu’il existe de nombreuses clés FIDO2 certifiées par la FIDO Alliance, Microsoft exige que certaines des extensions facultatives de la spécification FIDO2 CTAP (Client-to-Authenticator Protocol) soient implémentées par le fournisseur, afin de garantir une sécurité maximale et la meilleure expérience possible.

Une clé de sécurité DOIT implémenter les fonctionnalités et extensions du protocole FIDO2 CTAP suivantes pour être compatible avec Microsoft. Le fournisseur de l’authentificateur doit implémenter les versions FIDO_2_0 et FIDO_2_1 de la spécification. Pour plus d’informations, voir [Protocole client à authentificateur](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html).

| # | Fonctionnalité/Extension de confiance | Pourquoi cette fonctionnalité ou extension est-elle nécessaire ? |
| --- | --- | --- |
| 1 | Clé résidente/détectable | Cette fonctionnalité permet à la clé de sécurité d’être portable. Vos informations d’identification sont alors stockées sur la clé de sécurité et sont détectables, ce qui rend possible les flux sans nom d’utilisateur. |
| 2 | Code confidentiel client | Cette fonctionnalité vous permet de protéger vos informations d’identification avec un deuxième facteur et s’applique aux clés de sécurité qui n’ont pas d’interface utilisateur.<br>[PIN protocol 1](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#pinProto1) et [PIN protocol 2](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#pinProto2) DOIVENT être implémentés. |
| 3 | hmac-secret | Cette extension garantit que vous pouvez vous connecter à votre appareil lorsqu’il est hors connexion ou en mode avion. |
| 4 | Plusieurs comptes par fournisseur de ressources | Cette fonctionnalité garantit que vous pouvez utiliser la même clé de sécurité dans plusieurs services, comme un compte Microsoft et Azure Active Directory. |
| 5 | Gestion des informations d’identification    | Cette fonctionnalité permet aux utilisateurs de gérer leurs informations d’identification sur les clés de sécurité sur les plateformes et s’applique aux clés de sécurité qui n’ont pas cette fonctionnalité intégrée.<br>L’authentificateur DOIT implémenter les commandes [authenticatorCredentialManagement](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#authenticatorCredentialManagement) et [credentialMgmtPreview](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#prototypeAuthenticatorCredentialManagement) pour cette fonctionnalité  |
| 6 | Inscription biométrique           | Cette fonctionnalité permet aux utilisateurs d’inscrire leur biométrie sur leurs authentificateurs et s’applique aux clés de sécurité pour lesquelles cette fonctionnalité n’est pas intégrée.<br> L’authentificateur DOIT implémenter les commandes [authenicatorBioEnrollment](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#authenticatorBioEnrollment) et [userVerificationMgmtPreview](https://fidoalliance.org/specs/fido-v2.1-ps-20210615/fido-client-to-authenticator-protocol-v2.1-ps-20210615.html#prototypeAuthenticatorBioEnrollment) pour cette fonctionnalité |
| 7 | pinUvAuthToken           | Cette fonctionnalité permet à la plateforme d’avoir des jetons d’authentification à l’aide d’un code confidentiel ou d’une recherche biométrique, ce qui permet une meilleure expérience utilisateur lorsque plusieurs informations d’identification sont présentes sur l’authentificateur.  |
| 8 | forcePinChange           | Cette fonctionnalité permet aux entreprises de demander aux utilisateurs de modifier leur code PIN dans les déploiements à distance.  |
| 9 | setMinPINLength          | Cette fonctionnalité permet aux entreprises d’avoir une longueur minimale personnalisée pour leurs utilisateurs. L’authentificateur DOIT implémenter l’extension minPinLength et avoir une valeur d’au moins 1 pour maxRPIDsForSetMinPINLength.  |
| 10 | alwaysUV                | Cette fonctionnalité permet aux entreprises ou aux utilisateurs de toujours demander à l’utilisateur de vérifier l’utilisation de cette clé de sécurité. Authenticator DOIT implémenter la sous-commande toggleAlwaysUv. Il revient au fournisseur de décider de la valeur par défaut d’alwaysUV. À ce stade, en raison de la nature des différentes versions de système d’exploitation et d’adoption de RP, la valeur recommandée pour les authentificateurs à base de biométrie est vrai et celle pour les authentificateurs non basés sur la biométrie est faux.  |
| 11 | credBlob                | Cette extension permet aux sites web de stocker des informations de petite taille dans la clé de sécurité. maxCredBlobLength doit être au moins de 32 octets.  |
| 12 | largeBlob               | Cette extension permet aux sites web de stocker des informations plus volumineuses, comme des certificats, dans la clé de sécurité. maxSerializedLargeBlobArray doit être d’au moins 1024 octets.  |


### <a name="fido2-security-key-providers"></a>Fournisseurs de clés de sécurité FIDO2

Les fournisseurs suivants offrent des clés de sécurité FIDO2 de différents facteurs de forme, qui sont connues pour être compatibles avec l’expérience sans mot de passe. Nous vous encourageons à évaluer les propriétés de sécurité de ces clés en contactant le fournisseur, ainsi que la FIDO Alliance.

| Fournisseur                  |     Biométrie     | USB | NFC | BLE | Certifié FIPS | Contact                                                                                             |
|---------------------------|:-----------------:|:---:|:---:|:---:|:--------------:|-----------------------------------------------------------------------------------------------------|
| AuthenTrend               | ![y]              | ![y]| ![y]| ![y]| ![n]           | https://authentrend.com/about-us/#pg-35-3                                                           |
| Ensurity                  | ![y]              | ![y]| ![n]| ![n]| ![n]           | https://www.ensurity.com/contact                                                                    |
| Excelsecu                 | ![y]              | ![y]| ![y]| ![y]| ![n]           | https://www.excelsecu.com/productdetail/esecufido2secu.html                                         |
| Feitian                   | ![y]              | ![y]| ![y]| ![y]| ![y]           | https://shop.ftsafe.us/pages/microsoft                                                              |
| Fortinet                  | ![n]              | ![y]| ![n]| ![n]| ![n]           | https://www.fortinet.com/                                                                           |
| GoTrustID Inc.            | ![n]              | ![y]| ![y]| ![y]| ![n]           | https://www.gotrustid.com/idem-key                                                                  |
| HID                       | ![n]              | ![y]| ![y]| ![n]| ![n]           | https://www.hidglobal.com/contact-us                                                                |
| Hypersecu                 | ![n]              | ![y]| ![n]| ![n]| ![n]           | https://www.hypersecu.com/hyperfido                                                                 |
| IDmelon Technologies Inc. | ![y]              | ![y]| ![y]| ![y]| ![n]           | https://www.idmelon.com/#idmelon                                                                    |
| Kensington                | ![y]              | ![y]| ![n]| ![n]| ![n]           | https://www.kensington.com/solutions/product-category/why-biometrics/                               |
| KONA I                    | ![y]              | ![n]| ![y]| ![y]| ![n]           | https://konai.com/business/security/fido                                                            |
| Nymi                      | ![y]              | ![n]| ![y]| ![n]| ![n]           | https://www.nymi.com/nymi-band                                                                      | 
| Octatco                   | ![y]              | ![y]| ![n]| ![n]| ![n]           | https://octatco.com/                                                                                |
| OneSpan Inc.              | ![n]              | ![y]| ![n]| ![y]| ![n]           | https://www.onespan.com/products/fido                                                               |
| Thales Group              | ![n]              | ![y]| ![y]| ![n]| ![n]           | https://cpl.thalesgroup.com/access-management/authenticators/fido-devices                           |
| Thetis                    | ![y]              | ![y]| ![y]| ![y]| ![n]           | https://thetis.io/collections/fido2                                                                 |
| Token2 Switzerland        | ![y]              | ![y]| ![y]| ![n]| ![n]           | https://www.token2.swiss/shop/product/token2-t2f2-alu-fido2-u2f-and-totp-security-key               |
| Solutions TrustKey        | ![y]              | ![y]| ![n]| ![n]| ![n]           | https://www.trustkeysolutions.com/security-keys/                                                    |
| VinCSS                    | ![n]              | ![y]| ![n]| ![n]| ![n]           | https://passwordless.vincss.net                                                                     |
| Yubico                    | ![y]              | ![y]| ![y]| ![n]| ![y]           | https://www.yubico.com/solutions/passwordless/                                                      |



<!--Image references-->
[y]: ./media/fido2-compatibility/yes.png
[n]: ./media/fido2-compatibility/no.png

> [!NOTE]
> Si vous achetez des clés de sécurité NFC et que vous prévoyez de les utiliser, vous devez disposer d’un lecteur NFC pris en charge pour la clé de sécurité. Le lecteur NFC n’est pas une exigence ou une limitation Azure. Pour obtenir la liste des lecteurs NFC pris en charge, contactez le fournisseur de votre clé de sécurité NFC.

Si vous êtes fournisseur et que vous souhaitez que votre appareil figure dans cette liste des appareils pris en charge, consultez nos conseils sur la façon de [devenir fournisseur de clés de sécurité FIDO2 compatibles Microsoft](concept-fido2-hardware-vendor.md).

Pour prendre en main les clés de sécurité FIDO2, procédez comme suit :

> [!div class="nextstepaction"]
> [Activer la connexion sans mot de passe à l’aide des clés de sécurité FIDO2](howto-authentication-passwordless-security-key.md)

## <a name="supported-scenarios"></a>Scénarios pris en charge

Les considérations suivantes s'appliquent :

- Les administrateurs peuvent activer des méthodes d’authentification sans mot de passe pour leur locataire.

- Les administrateurs peuvent cibler tous les utilisateurs ou sélectionner des utilisateurs/groupes au sein de leur locataire pour chaque méthode.

- Les utilisateurs peuvent inscrire et gérer ces méthodes d’authentification sans mot de passe sur le portail de leur compte.

- Les utilisateurs peuvent se connecter avec les méthodes d’authentification sans mot de passe suivantes :
   - Application Microsoft Authenticator : fonctionne dans les scénarios où l’authentification Azure AD est utilisée, notamment sur tous les navigateurs, pendant la configuration de Windows 10 et avec les applications mobiles intégrées sur n’importe quel système d’exploitation.
   - Clés de sécurité : Fonctionnent sur l’écran de verrouillage Windows 10 et les navigateurs web pris en charge, comme Microsoft Edge (ancienne et nouvelle version de Edge).

- Les utilisateurs peuvent utiliser des informations d’identification sans mot de passe pour accéder à des ressources dans des locataires dans lequel ils sont invités, mais il se peut qu’ils soient tenus d’effectuer une authentification multifacteur dans ce locataire de ressources. Pour plus d’informations, consultez [Risque de redondance de l’authentification multifacteur](../external-identities/current-limitations.md#possible-double-multi-factor-authentication).  

- Les utilisateurs ne peuvent pas enregistrer les informations d’identification sans mot de passe dans un locataire dans lequel ils sont invités, de la même façon qu’ils n’ont pas de mot de passe géré dans ce locataire.  


## <a name="choose-a-passwordless-method"></a>Choix d’une méthode sans mot de passe

Le choix entre ces trois options sans mot de passe dépend des exigences en matière de sécurité, de plateforme et d'applications de votre entreprise.

Voici quelques facteurs à prendre en compte au moment de choisir une technologie Microsoft sans mot de passe :

||**Windows Hello Entreprise**|**Connexion avec l’application Microsoft Authenticator**|**Clés de sécurité FIDO2**|
|:-|:-|:-|:-|
|**Conditions préalables**| Windows 10 version 1809 ou ultérieure<br>Azure Active Directory| Application Microsoft Authenticator<br>Téléphone (appareils iOS et Android exécutant Android version 6.0 ou ultérieure).|Windows 10 version 1903 ou ultérieure<br>Azure Active Directory|
|**Mode**|Plateforme|Logiciel|Matériel|
|**Systèmes et appareils**|PC avec module de plateforme sécurisée (TPM) intégré<br>Code confidentiel et reconnaissance biométrique |Code confidentiel et reconnaissance biométrique sur téléphone|Périphériques de sécurité FIDO2 compatibles Microsoft|
|**Expérience utilisateur**|Connectez-vous à l’aide d’un code confidentiel ou d'une reconnaissance biométrique (faciale, iris ou empreinte digitale) aux appareils Windows.<br>L'authentification Windows Hello est liée à l'appareil ; l'utilisateur a besoin de l'appareil et d'un composant de connexion tel qu'un code confidentiel ou un facteur biométrique pour accéder aux ressources de l'entreprise.|Connectez-vous à l’aide d’un téléphone mobile en utilisant une empreinte digitale, une reconnaissance faciale ou de l'iris, ou un code confidentiel.<br>Les utilisateurs se connectent à un compte professionnel ou personnel à partir de leur PC ou téléphone mobile.|Connectez-vous à l’aide du périphérique de sécurité FIDO2 (biométrie, code confidentiel et carte NFC)<br>L'utilisateur peut accéder à l'appareil selon les contrôles de l’organisation et s’authentifier par code confidentiel ou biométrie à l'aide de périphériques tels que des clés de sécurité USB et des cartes à puce NFC, des clés ou autres objets connectés portables.|
|**Scénarios possibles**| Expérience sans mot de passe avec appareil Windows.<br>Applicable pour le PC de travail dédié avec possibilité d'authentification unique à l'appareil et aux applications.|Solution sans mot de passe à l'aide d'un téléphone mobile.<br>Applicable pour accéder aux applications professionnelles et personnelles sur le web à partir de n’importe quel appareil.|Expérience sans mot de passe pour les employés avec données biométriques, code confidentiel et carte à puce NFC.<br>Applicable pour les PC partagés et lorsqu'un téléphone mobile n’est pas une option viable (personnel du support technique, borne publique ou équipe hospitalière)|

Utilisez le tableau suivant pour choisir la méthode répondant à vos besoins et à ceux de vos utilisateurs.

|Utilisateur|Scénario|Environnement|Technologie sans mot de passe|
|:-|:-|:-|:-|
|**Administrateur**|Sécuriser l’accès à un appareil pour les tâches de gestion|Appareil Windows 10 attribué|Windows Hello Entreprise et/ou clé de sécurité FIDO2|
|**Administrateur**|Tâches de gestion sur des appareils non Windows| Appareil mobile ou non Windows|Connexion sans mot de passe avec l'application Microsoft Authenticator|
|**Professionnel de l'information**|Productivité|Appareil Windows 10 attribué|Windows Hello Entreprise et/ou clé de sécurité FIDO2|
|**Professionnel de l'information**|Productivité| Appareil mobile ou non Windows|Connexion sans mot de passe avec l'application Microsoft Authenticator|
|**Employé de terrain**|Borne dans une usine, un magasin de détail ou un point d'entrée de données|Appareils Windows 10 partagés|Clés de sécurité FIDO2|

## <a name="next-steps"></a>Étapes suivantes

Pour prendre en main la connexion sans mot de passe dans Azure AD, effectuez l’une des procédures suivantes :

* [Activer la connexion sans mot de passe par clé de sécurité FIDO2](howto-authentication-passwordless-security-key.md)
* [Activer la connexion sans mot de passe par téléphone avec l’application Authenticator](howto-authentication-passwordless-phone.md)

### <a name="external-links"></a>Liens externes

* [FIDO Alliance](https://fidoalliance.org/)
* [Spécification de FIDO2 CTAP](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
