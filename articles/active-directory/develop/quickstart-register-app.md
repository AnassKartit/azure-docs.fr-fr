---
title: 'Démarrage rapide : Inscrire une application dans la plateforme d’identités Microsoft | Azure'
description: Dans ce guide de démarrage rapide, vous allez découvrir comment inscrire une application auprès de la plateforme d’identités Microsoft.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/03/2020
ms.author: marsma
ms.custom: aaddev, identityplatformtop40, contperf-fy21q1, contperf-fy21q2
ms.reviewer: aragra, lenalepa, sureshja
ms.openlocfilehash: 7b7adde8a6e60042bec7695e4553d6470e548316
ms.sourcegitcommit: 73fb48074c4c91c3511d5bcdffd6e40854fb46e5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106055682"
---
# <a name="quickstart-register-an-application-with-the-microsoft-identity-platform"></a>Démarrage rapide : Inscrire une application avec la plateforme d’identités Microsoft

Dans ce guide de démarrage rapide, vous allez inscrire une application dans le portail Azure afin que la plateforme d’identités Microsoft puisse fournir des services d’authentification et d’autorisation pour votre application et ses utilisateurs.

La plateforme d’identités Microsoft assure la gestion des identités et des accès (IAM) uniquement pour les applications inscrites. Qu’il s’agisse d’une application cliente telle qu’une application web ou mobile, ou d’une API web sur laquelle repose une application cliente, son inscription établit une relation d’approbation entre votre application et le fournisseur d’identité, à savoir la plateforme d’identités Microsoft.

## <a name="prerequisites"></a>Prérequis

* Un compte Azure disposant d’un abonnement actif. [Créez un compte gratuitement](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Suivi du guide de démarrage rapide [Configurer un locataire](quickstart-create-new-tenant.md).

## <a name="register-an-application"></a>Inscrire une application

L’inscription de votre application établit une relation d’approbation entre votre application et la plateforme d’identités Microsoft. L’approbation est unidirectionnelle : votre application approuve la plateforme d’identités Microsoft, et non le contraire.

Effectuez les étapes suivantes pour créer l’inscription d’application :

1. Connectez-vous au <a href="https://portal.azure.com/" target="_blank">portail Azure</a>.
1. Si vous avez accès à plusieurs locataires, dans le menu supérieur, utilisez le filtre **Annuaire + abonnement** :::image type="icon" source="./media/common/portal-directory-subscription-filter.png" border="false"::: pour sélectionner le locataire dans lequel vous voulez inscrire une application.
1. Recherchez et sélectionnez **Azure Active Directory**.
1. Sous **Gérer**, sélectionnez **Inscriptions d’applications** > **Nouvelle inscription**.
1. Entrez un **nom** d’affichage pour votre application. Les utilisateurs de votre application peuvent voir le nom d’affichage lorsqu’ils l’utilisent, par exemple lors de la connexion.
    Vous pouvez modifier le nom d’affichage à tout moment, et plusieurs inscriptions d’applications peuvent partager le même nom. L’ID d’application (client) généré automatiquement par l’inscription de l’application, et non son nom d’affichage, identifie de manière unique votre application au sein de la plateforme d’identités.
1. Spécifiez les personnes qui peuvent utiliser l’application ; elles sont parfois appelées *audience de connexion*.

    | Types de comptes pris en charge | Description |
    |-------------------------|-------------|
    | **Comptes dans cet annuaire organisationnel uniquement** | Sélectionnez cette option si vous générez une application destinée à être utilisée uniquement par des utilisateurs (ou des invités) dans *votre* locataire.<br><br>Souvent appelée application *métier* (LOB), cette application est une application *monolocataire* dans la plateforme d’identités Microsoft. |
    | **Comptes dans un annuaire organisationnel** | Sélectionnez cette option si vous voulez que les utilisateurs de *n’importe quel* locataire Azure Active Directory (Azure AD) puisse utiliser votre application. Cette option est appropriée si, par exemple, vous générez une application Logiciel en tant que service (SaaS) que vous envisagez de fournir à plusieurs organisations.<br><br>Ce type d’application est dit *multilocataire* dans la plateforme d’identités Microsoft. |
    | **Comptes dans un annuaire organisationnel et comptes personnels Microsoft** | Sélectionnez cette option pour cibler l’ensemble le plus large de clients.<br><br>En sélectionnant cette option, vous inscrivez une application *multilocataire* qui peut également prendre en charge les utilisateurs disposant de *comptes Microsoft* personnels. |
    | **Comptes Microsoft personnels** | Sélectionnez cette option si vous générez une application destinée uniquement aux utilisateurs disposant de comptes Microsoft personnels. Les comptes Microsoft personnels incluent les comptes Skype, Xbox, Live et Hotmail. |

1. N’entrez rien pour **URI de redirection (facultatif)** . Vous allez configurer un URI de redirection à la section suivante.
1. Sélectionnez **Inscrire** pour procéder à l’inscription d’application initiale.

    :::image type="content" source="media/quickstart-register-app/portal-02-app-reg-01.png" alt-text="Capture d’écran du portail Azure dans un navigateur web : le volet Inscrire une application.":::

Une fois l’inscription terminée, le portail Azure affiche le volet **Vue d’ensemble** de l’inscription de l’application. Vous voyez l’**ID d’application (client)** . Aussi appelée *ID client*, cette valeur identifie de manière unique votre application dans la plateforme d’identités Microsoft. 

> [!IMPORTANT]
> Par défaut, les inscriptions de nouvelles applications sont masquées aux utilisateurs. Quand vous êtes prêt pour rendre l’application visible aux utilisateurs dans la [page Mes applications](../user-help/my-apps-portal-end-user-access.md), vous pouvez l’activer. Pour activer l’application, dans le portail Azure, accédez à **Azure Active Directory** > **Applications d’entreprise**, puis sélectionnez l’application. Puis, dans la page **Propriétés**, basculez **Visible pour les utilisateurs ?** sur Oui.

Le code de votre application, ou plus généralement une bibliothèque d’authentification servant dans votre application, utilise également l’ID client. L’ID est utilisé dans le cadre de la validation des jetons de sécurité reçus de la plateforme d’identités.

:::image type="content" source="media/quickstart-register-app/portal-03-app-reg-02.png" alt-text="Capture d’écran du portail Azure dans un navigateur web : le volet Vue d’ensemble d’une inscription d’application.":::

## <a name="add-a-redirect-uri"></a>Ajouter un URI de redirection

Un *URI de redirection* est l’emplacement où la plateforme d’identités Microsoft redirige le client d’un utilisateur et envoie des jetons de sécurité après authentification.

Dans une application web de production, par exemple, l’URI de redirection est souvent un point de terminaison public sur lequel votre application s’exécute, comme `https://contoso.com/auth-response`. Pendant le développement, il est courant d’ajouter également le point de terminaison où vous exécutez votre application localement, comme `https://127.0.0.1/auth-response` ou `http://localhost/auth-response`.

Pour ajouter et modifier des URI de redirection pour vos applications inscrites, vous devez configurer leurs [paramètres de plateforme](#configure-platform-settings).

### <a name="configure-platform-settings"></a>Configurer des paramètres de plateforme

Les paramètres de chaque type d’application, dont les URI de redirection, sont configurés dans **Configurations de plateforme** dans le portail Azure. Certaines plateformes, comme le **web** et les **applications monopages**, nécessitent de spécifier manuellement un URI de redirection. Pour les autres plateformes, comme les plateformes mobiles et de bureau, vous pouvez sélectionner des URI de redirection générés automatiquement quand vous configurez leurs autres paramètres.

Pour configurer des paramètres d’application en fonction de la plateforme ou de l’appareil ciblé :

1. Dans **Inscriptions d’applications** du portail Azure, sélectionnez votre application.
1. Sous **Gérer**, sélectionnez **Authentification**.
1. Sous **Configurations de plateformes**, sélectionnez **Ajouter une plateforme**.
1. Sous **Configurer des plateformes**, sélectionnez la vignette correspondant à votre type d’application (plateforme) pour configurer ses paramètres.

    :::image type="content" source="media/quickstart-register-app/portal-04-app-reg-03-platform-config.png" alt-text="Capture d’écran du volet de configuration de la plateforme dans le portail Azure." border="false":::

    | Plateforme | Paramètres de configuration |
    | -------- | ---------------------- |
    | **Web** | Entrez un **URI de redirection** pour votre application. Cet URI est l’emplacement où la plateforme d’identités Microsoft redirige le client d’un utilisateur et envoie des jetons de sécurité après authentification.<br/><br/>Sélectionnez cette plateforme pour les applications web standard qui s’exécutent sur un serveur. |
    | **Application monopage** | Entrez un **URI de redirection** pour votre application. Cet URI est l’emplacement où la plateforme d’identités Microsoft redirige le client d’un utilisateur et envoie des jetons de sécurité après authentification.<br/><br/>Sélectionnez cette plateforme si vous générez une application web côté client en utilisant JavaScript ou un framework comme Angular, Vue.js, React.js ou Blazor WebAssembly. |
    | **iOS / macOS** | Entrez l’**ID de bundle** d’applications. Recherchez-le dans **Paramètres de build** ou dans Xcode, dans *info.plist*.<br/><br/>Un URI de redirection est automatiquement généré quand vous spécifiez un **ID de bundle**. |
    | **Android** | Entrez le **Nom du package** d’applications. Recherchez-le dans le fichier *AndroidManifest.xml*. Générez et entrez également le **Hachage de signature**.<br/><br/>Un URI de redirection est automatiquement généré quand vous spécifiez ces paramètres. |
    | **Applications de bureau et mobiles** | Sélectionnez l’un des **URI de redirection suggérés**. Ou spécifiez un **URI de redirection personnalisé**.<br/><br/>Pour les applications de bureau utilisant un navigateur incorporé, nous vous recommandons<br/>`https://login.microsoftonline.com/common/oauth2/nativeclient`<br/><br/>Pour les applications de bureau utilisant un navigateur système, nous vous recommandons<br/>`http://localhost`<br/><br/>Sélectionnez cette plateforme pour les applications mobiles qui n’utilisent pas la toute dernière bibliothèque d’authentification Microsoft (MSAL), ou qui n’utilisent pas de répartiteur. Sélectionnez également cette plateforme pour les applications de bureau. |
1. Sélectionnez **Configurer** pour effectuer la configuration de la plateforme.

### <a name="redirect-uri-restrictions"></a>Restrictions applicables aux URI de redirection

Plusieurs restrictions s’appliquent au format des URI de redirection que vous ajoutez à une inscription d’application. Pour plus d’informations sur ces restrictions, consultez [Limitations et restrictions des URI de redirection (URL de réponse)](reply-url.md).

## <a name="add-credentials"></a>Ajouter les informations d’identification

Les informations d’identification sont utilisées par les [applications clientes confidentielles](msal-client-applications.md) qui accèdent à une API web. Les [applications web](scenario-web-app-call-api-overview.md), les autres [API web](scenario-protected-web-api-overview.md) ou les [applications de type service et démon](scenario-daemon-overview.md) sont des exemples de clients confidentiels. Les informations d’identification permettent à votre application de s’authentifier de façon autonome, sans qu’aucune interaction utilisateur ne soit nécessaire au moment de l’exécution. 

Vous pouvez ajouter des certificats et des secrets clients (une chaîne) en tant qu’informations d’identification à votre inscription d’application cliente confidentielle.

:::image type="content" source="media/quickstart-register-app/portal-05-app-reg-04-credentials.png" alt-text="Capture d’écran du portail Azure : le volet Certificats et secrets dans une inscription d’application.":::

### <a name="add-a-certificate"></a>Ajouter un certificat

Parfois appelé *clé publique*, un certificat représente le type d’informations d’identification recommandé. Il offre une meilleure assurance qu’un secret client. Pour plus d’informations sur l’utilisation d’un certificat comme méthode d’authentification dans votre application, consultez [Informations d’identification de certificat d’authentification d’application de la plateforme d’identités Microsoft](active-directory-certificate-credentials.md).

1. Dans **Inscriptions d’applications** du portail Azure, sélectionnez votre application.
1. Sélectionnez **Certificats et secrets** > **Charger le certificat**.
1. Sélectionnez les fichiers à charger. Il doit s’agir d’un fichier de type *.cer*, *.pem* ou *.crt*.
1. Sélectionnez **Ajouter**.

### <a name="add-a-client-secret"></a>Ajouter un secret client

Le secret client est également appelé *mot de passe d’application*. Il s’agit d’une valeur de chaîne que votre application peut utiliser à la place d’un certificat pour s’identifier. Le secret client est le plus facile des deux types d’informations d’identification à utiliser. Il est souvent utilisé pendant le développement, mais il est considéré comme moins sécurisé qu’un certificat. Utilisez des certificats dans vos applications qui s’exécutent en production. 

Pour plus d’informations sur les recommandations de sécurité des applications, consultez [Bonnes pratiques et recommandations relatives à la plateforme d’identités Microsoft](identity-platform-integration-checklist.md#security).

1. Dans **Inscriptions d’applications** du portail Azure, sélectionnez votre application.
1. Sélectionnez **Certificats et secrets** >  **Nouveau secret client**.
1. Ajoutez une description pour votre clé secrète client.
1. Sélectionnez une durée.
1. Sélectionnez **Ajouter**.
1. *Enregistrez la valeur du secret* en prévision d’une utilisation dans le code de votre application cliente. Cette valeur secrète ne sera *plus jamais affichée* lorsque vous aurez quitté cette page.


## <a name="next-steps"></a>Étapes suivantes

Les applications clientes doivent généralement accéder aux ressources d’une API web. Vous pouvez protéger votre application cliente en utilisant la plateforme d’identités Microsoft. Vous pouvez également utiliser la plateforme pour autoriser un accès délimité, basé sur les autorisations, à votre API web.

Passez au guide de démarrage rapide suivant de la série, afin de créer une autre inscription d’application pour votre API web et d’exposer ses étendues.

> [!div class="nextstepaction"]
> [Configurer une application pour exposer une API web](quickstart-configure-app-expose-web-apis.md)
