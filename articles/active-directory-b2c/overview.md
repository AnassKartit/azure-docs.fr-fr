---
title: Qu’est-ce qu’Azure Active Directory B2C ?
description: Découvrez comment utiliser Azure Active Directory B2C pour prendre en charge les identités externes dans vos applications, notamment l’inscription via les réseaux sociaux avec Facebook, Google et d’autres fournisseurs d’identité.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 10/01/2021
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 54af9f4b8584500faa9c3134f350a436da54c24b
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2021
ms.locfileid: "129709971"
---
# <a name="what-is-azure-active-directory-b2c"></a>Qu’est-ce qu’Azure Active Directory B2C ?

Azure Active Directory B2C fournit une identité entreprise- client en tant que service. Vos clients utilisent leurs identités de compte local, d’entreprise ou de réseau social par défaut pour bénéficier d’un accès par authentification unique à vos applications et API.

![Infographie des fournisseurs d’identité Azure AD B2C et d’applications en aval](./media/overview/azureadb2c-overview.png)

Azure AD B2C est une solution de gestion des identités et des accès clients, capable de prendre en charge des millions d’utilisateurs et des milliards d’authentifications par jour. Elle prend en charge la mise à l’échelle et la sécurité de la plateforme d’authentification, surveillant et gérant automatiquement les menaces, telles que les attaques par déni de service, pulvérisation de mot de passe ou force brute.

Azure AD B2C est un service distinct d’[Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md). Il repose sur la même technologie qu’Azure AD, mais à des fins différentes. Il permet aux entreprises de créer des applications orientées client, puis d’autoriser quiconque à s’y inscrire sans aucune restriction sur le compte d’utilisateur.
   
## <a name="who-uses-azure-ad-b2c"></a>Qui utilise Azure AD B2C ?
Toute entreprise ou personne qui souhaite authentifier les utilisateurs finaux sur leurs applications web/mobiles à l’aide d’une solution d’authentification de marque blanche. Outre l’authentification, le service Azure AD B2C est utilisé pour l’autorisation telle que l’accès aux ressources d’API par les utilisateurs authentifiés. Azure AD B2C est destiné à être utilisé par des **administrateurs informatiques** et des **développeurs**.

## <a name="custom-branded-identity-solution"></a>Solution d’identité personnalisée respectant la marque

Azure AD B2C est une solution d’authentification de marque blanche. Vous pouvez personnaliser la totalité de l’expérience utilisateur avec votre marque, de manière à la fondre parfaitement dans vos applications web et mobiles.

Personnalisez chaque page affichée par Azure AD B2C lorsque vos utilisateurs s’inscrivent, se connectent et modifient leurs informations de profil. Personnalisez aussi le code HTML, CSS et JavaScript dans vos parcours utilisateur pour que l’expérience Azure AD B2C ressemble à une partie native de votre application.

![Pages d’inscription et de connexion personnalisées avec image d’arrière-plan](./media/overview/sign-in-small.png)

## <a name="single-sign-on-access-with-a-user-provided-identity"></a>Accès avec authentification unique et identité fournie par l’utilisateur

Azure AD B2C utilise des protocoles d’authentification reposant sur des normes, dont OpenID Connect, OAuth 2.0 et SAML (Security Assertion Markup Language). Il s’intègre à la plupart des applications modernes et logiciels du commerce.

:::image type="content" source="./media/overview/scenario-singlesignon.png" alt-text="Diagramme d’une fédération d’identités tierces avec Azure AD B2C.":::

En jouant le rôle d’autorité d’authentification centrale pour vos applications web, applications mobiles et API, Azure AD B2C vous permet de créer une solution d’authentification unique (SSO) pour celles-ci. Centralisez la collecte des informations de profil utilisateur et de préférence, et capturez une analytique détaillée sur le comportement de connexion et la conversion d’inscription.

## <a name="integrate-with-external-user-stores"></a>Intégration aux magasins d’utilisateurs externes

Azure AD B2C fournit un annuaire pouvant contenir 100 attributs personnalisés par utilisateur. Cependant, vous pouvez également intégrer des systèmes externes. Par exemple, vous utilisez Azure AD B2C pour l’authentification, mais déléguez les données client à une base de données externe de gestion de la relation client (CRM) ou de fidélisation des clients en tant que source de confiance.

Un autre scénario de magasin d’utilisateurs externes consiste à laisser Azure AD B2C gérer l’authentification pour votre application tout en intégrant un système externe qui stocke les données des profils utilisateur ou les données personnelles. Par exemple, pour répondre aux exigences de résidence des données, telles que les stratégies de stockage de données régionales ou locales. Toutefois, le service Azure Active Directory B2C lui-même est disponible dans le monde entier via le cloud public Azure. 

:::image type="content" source="./media/overview/scenario-remoteprofile.png" alt-text="Diagramme logique de la communication d’Azure AD B2C avec un magasin d’utilisateurs externes.":::

Azure AD B2C peut faciliter la collecte des informations au niveau de l’utilisateur lors de l’inscription ou de la modification de profil, puis transmettre ces données au système externe via une API. Par la suite, lors d’authentifications ultérieures, Azure AD B2C peut récupérer les données du système externe et, si nécessaire, les inclure dans le cadre de la réponse du jeton d’authentification qu’il envoie à votre application.

## <a name="progressive-profiling"></a>Profilage progressif

Une autre option de parcours utilisateur comprend le profilage progressif. Le profilage progressif permet à vos clients d’effectuer rapidement leur première transaction en recueillant une quantité minime d’informations. Ensuite, il collecte petit à petit plus de données de profil auprès du client lors de connexions futures.

:::image type="content" source="./media/overview/scenario-progressive.png" alt-text="Représentation visuelle d’un profilage progressif.":::

## <a name="third-party-identity-verification-and-proofing"></a>Vérification et confirmation d’identités tierces

Utilisez Azure AD B2C pour faciliter la vérification et la confirmation d’identités en recueillant des données utilisateur, puis en les passant à un système tiers afin de procéder à la validation, au scoring de confiance et à l’approbation autorisant la création de comptes d’utilisateur.


:::image type="content" source="./media/overview/scenario-idproofing.png" alt-text="Diagramme montrant le flux d’utilisateur pour la confirmation d’identités tierces.":::

Vous avez appris quelques-unes des actions qu’il vous est possible d’accomplir avec Azure AD B2C en tant que plateforme d’identités entreprise-à-client. Les sections suivantes de cette présentation vous guident tout au long d’une application de démonstration qui utilise Azure AD B2C. Vous êtes également invité à passer directement à une [vue d’ensemble technique plus détaillée d’Azure AD B2C](technical-overview.md).

## <a name="example-woodgrove-groceries"></a>Exemple : WoodGrove Groceries

[WoodGrove Groceries][woodgrove] est une application web en ligne créée par Microsoft pour illustrer quelques fonctionnalités Azure AD B2C. Les sections suivantes passent en revue plusieurs options d’authentification fournies par Azure AD B2C sur le site web de WoodGrove.

### <a name="business-overview"></a>Présentation de l’entreprise

WoodGrove est une épicerie en ligne qui vend de la nourriture aux particuliers et aux professionnels. Ses clients professionnels achètent de la nourriture pour le compte de leur société, ou pour les différents commerces qu’ils dirigent.

### <a name="sign-in-options"></a>Options de connexion

WoodGrove Groceries propose différentes options de connexion, en fonction de la relation que ses clients établissent avec le magasin :

* Les clients **particuliers** peuvent s’inscrire ou se connecter avec des comptes individuels, par exemple à l’aide d’un fournisseur d’identité de réseau social, ou d’une adresse e-mail et d’un mot de passe.
* Les clients **professionnels** peuvent s’inscrire ou se connecter avec leurs informations d’identification d’entreprise.
* Les **partenaires** et les fournisseurs sont des personnes qui approvisionnent l’épicerie en denrées à vendre. L’identité du partenaire est fournie par [Azure Active Directory B2B](../active-directory/external-identities/what-is-b2b.md).

![Pages de connexion pour les particuliers (B2C), les professionnels (B2C) et les partenaires (B2B)](./media/overview/woodgrove-overview.png)

### <a name="authenticate-individual-customers"></a>Authentifier les clients particuliers

Lorsqu’un client sélectionne **Sign in with your personal account** (Connectez-vous avec votre compte personnel), il est redirigé vers une page de connexion personnalisée, hébergée par Azure AD B2C. Vous pouvez remarquer dans l’image suivante que nous avons personnalisé l’interface utilisateur (IU) pour qu’elle ressemble à celle du site web de WoodGrove Groceries. Les clients de WoodGrove ignorent que l’expérience d’authentification est hébergée et sécurisée par Azure AD B2C.

![Page de connexion WoodGrove personnalisée, hébergée par Azure AD B2C](./media/overview/sign-in.png)

WoodGrove permet à ses clients de s’inscrire et de se connecter en utilisant leurs comptes Google, Facebook ou Microsoft comme fournisseur d’identité. Ils peuvent, sinon, s’inscrire au moyen de leur adresse e-mail et d’un mot de passe pour créer ce que l’on appelle un *compte local*.

Quand un client sélectionne **Sign up with your personal account** (Inscrivez-vous avec votre compte personnel), puis **Sign up now** (Inscrivez-vous maintenant), une page d’inscription personnalisée s’affiche.

![Page d’inscription WoodGrove personnalisée, hébergée par Azure AD B2C](./media/overview/sign-up.png)

Après avoir indiqué une adresse e-mail et sélectionné **Send verification code** (Envoyer le code de vérification), il reçoit le code envoyé par Azure AD B2C. Une fois qu’il a entré son code, sélectionné **Verify code**, puis indiquez les autres informations demandées dans le formulaire, il doit aussi accepter les conditions d’utilisation du service.

Le fait de cliquer sur le bouton **Create** déclenche la redirection de l’utilisateur par Azure AD B2C vers le site web de WoodGrove Groceries. Lors de la redirection, Azure AD B2C transmet un jeton d’authentification OpenID Connect à l’application web WoodGrove. L’utilisateur est maintenant connecté et prêt à utiliser le site, son nom d’affichage apparaît dans l’angle supérieur droit pour indiquer qu’il est connecté.

![L’en-tête du site web WoodGrove Groceries indiquant que l’utilisateur est connecté](./media/overview/signed-in-individual.png)

### <a name="authenticate-business-customers"></a>Authentifier les clients professionnels

Lorsqu’un client sélectionne une des options sous **Business customers**, le site web de WoodGrove Groceries appelle une autre *stratégie Azure AD B2C* que celle utilisée pour les particuliers. Apprenez ce qu’est une *stratégie B2C* dans [Présentation technique d’Azure AD B2C](technical-overview.md).

Cette stratégie propose à l’utilisateur de se servir de ses informations d’identification d’entreprise pour l’inscription et la connexion. Dans l’exemple de WoodGrove, les utilisateurs sont invités à se connecter avec un compte professionnel ou scolaire. Cette stratégie utilise une [application Azure AD multilocataire](../active-directory/develop/howto-convert-app-to-be-multi-tenant.md) et le point de terminaison Azure AD `/common` pour fédérer Azure AD B2C avec n’importe quel client Microsoft 365 dans le monde.

### <a name="authenticate-partners"></a>Authentifier les partenaires

Le lien **Sign in with your supplier account** (Connectez-vous avec votre compte fournisseur) utilise la fonctionnalité de collaboration d’Azure Active Directory B2B. Azure AD B2B constitue un groupe de fonctionnalités au sein d’Azure Active Directory pour gérer les identités des partenaires. Ces identités peuvent être fédérées à partir d’Azure Active Directory afin d’accéder aux applications protégées par Azure AD B2C.

Apprenez-en davantage sur Azure AD B2B avec la [Présentation de l’accès utilisateur invité dans Azure Active Directory B2B](../active-directory/external-identities/what-is-b2b.md).

<!-- UNCOMMENT WHEN REPO IS UPDATED WITH LATEST DEMO CODE
### Sample code

If you'd like to jump right into the code to see how the WoodGrove Groceries application is built, you can find the repository on GitHub:

[Azure-Samples/active-directory-external-identities-woodgrove-demo][woodgrove-repo] (GitHub)
-->

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous avez une idée de ce qu’est Azure AD B2C, et de son utilité dans quelques-uns des scénarios qui ont été évoqués, découvrez-en un peu plus sur ses fonctionnalités et aspects techniques.

> [!div class="nextstepaction"]
> [Vue d’ensemble technique d’Azure AD B2C >](technical-overview.md)

<!-- LINKS - External -->
[woodgrove]: https://aka.ms/ciamdemo
[woodgrove-repo]: https://github.com/Azure-Samples/active-directory-external-identities-woodgrove-demo