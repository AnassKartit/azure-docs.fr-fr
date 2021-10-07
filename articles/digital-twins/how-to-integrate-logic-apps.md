---
title: Intégrer à Logic Apps
titleSuffix: Azure Digital Twins
description: Découvrez comment connecter Logic Apps à Azure Digital Twins à l’aide d’un connecteur personnalisé
author: baanders
ms.author: baanders
ms.date: 9/1/2021
ms.topic: how-to
ms.service: digital-twins
ms.reviewer: baanders
ms.openlocfilehash: aa8ea41233cad42bcad9fc387b8f984503c6e75e
ms.sourcegitcommit: 0770a7d91278043a83ccc597af25934854605e8b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2021
ms.locfileid: "124797736"
---
# <a name="integrate-with-logic-apps-using-a-custom-connector"></a>Intégrer à Logic Apps à l’aide d’un connecteur personnalisé

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) est un service cloud qui vous permet d’automatiser les flux de travail entre les applications et les services. En connectant Logic Apps aux API Azure Digital Twins, vous pouvez créer de tels flux automatisés autour d’Azure Digital Twins et des données connexes.

Azure Digital Twins n’a pas de connecteur certifié (préintégré) pour Logic Apps. À la place, le processus actuel d’utilisation de Logic Apps avec Azure Digital Twins consiste à créer un [connecteur Logic Apps personnalisé](../logic-apps/custom-connector-overview.md) à l’aide d’un [fichier de définition Swagger Azure Digital Twins personnalisé](/samples/azure-samples/digital-twins-custom-swaggers/azure-digital-twins-custom-swaggers/) ayant été modifié pour fonctionner avec Logic Apps.

> [!NOTE]
> Il existe plusieurs versions du fichier de définition Swagger dans l’exemple de Swagger personnalisé lié ci-dessus. La version la plus récente se trouve dans le sous-dossier avec la date la plus récente, mais les versions antérieures contenues dans l’exemple continuent également d’être prises en charge.

Dans cet article, vous allez utiliser le [portail Azure](https://portal.azure.com) pour **créer un connecteur personnalisé** qui permet de connecter Logic Apps à une instance d’Azure Digital Twins. Vous allez ensuite **créer une application logique** qui utilise cette connexion pour obtenir un exemple de scénario, dans lequel les événements déclenchés par un minuteur mettent automatiquement à jour un jumeau dans votre instance Azure Digital Twins. 

## <a name="prerequisites"></a>Prérequis

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.
Connectez-vous au [portail Azure](https://portal.azure.com) avec ce compte. 

Vous devez également accomplir les étapes suivantes dans le cadre de la configuration requise. Le reste de cette section vous guidera dans les étapes suivantes :
- Configurer une instance Azure Digital Twins
- Ajouter un jumeau numérique
- Configurer une inscription d’application Azure Active Directory (Azure AD)

### <a name="set-up-azure-digital-twins-instance"></a>Configurer l’instance Azure Digital Twins

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

### <a name="add-a-digital-twin"></a>Ajouter un jumeau numérique

Cet article utilise Logic Apps pour mettre à jour un jumeau dans votre instance Azure Digital Twins. Pour continuer, vous devez ajouter au moins un jumeau à votre instance. 

Vous pouvez ajouter des jumeaux à l’aide des [API DigitalTwins](/rest/api/digital-twins/dataplane/twins), du [SDK .NET (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true) ou de la [CLI Azure Digital Twins](/cli/azure/dt?view=azure-cli-latest&preserve-view=true). Pour obtenir des instructions détaillées sur la façon de créer des jumeaux à l’aide de ces méthodes, consultez [Gérer des jumeaux numériques](how-to-manage-twin.md).

Vous allez avoir besoin d’un **ID de jumeau** dans l’instance que vous avez créée.

### <a name="set-up-app-registration"></a>Configurer l’inscription d’application

[!INCLUDE [digital-twins-prereq-registration.md](../../includes/digital-twins-prereq-registration.md)]

## <a name="create-custom-logic-apps-connector"></a>Créer un connecteur Logic Apps personnalisé

Vous être maintenant prêt à créer un [connecteur Logic Apps personnalisé](../logic-apps/custom-connector-overview.md) pour les API Azure Digital Twins. Cela vous permettra de connecter Azure Digital Twins au moment de la création d’une application logique dans la prochaine section.

Accédez à la page [Connecteur personnalisé Logic Apps](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Web%2FcustomApis) dans le portail Azure (vous pouvez utiliser ce lien ou rechercher la page dans la barre de recherche du portail). Sélectionnez *+ Créer*.

:::image type="content" source="media/how-to-integrate-logic-apps/logic-apps-custom-connector.png" alt-text="Capture d’écran de la page « Connecteur personnalisé Logic Apps » dans le portail Azure. Le bouton « Ajouter » est mis en surbrillance.":::

Dans la page **Créer un connecteur personnalisé Logic Apps** qui suit, sélectionnez votre abonnement et votre groupe de ressources ainsi qu’un nom et un emplacement de déploiement pour votre nouveau connecteur. Sélectionnez **Revoir + créer**. 

:::image type="content" source="media/how-to-integrate-logic-apps/create-logic-apps-custom-connector.png" alt-text="Capture d’écran de la page « Créer un connecteur personnalisé Logic Apps » dans le portail Azure.":::

Cela vous permet d’accéder à l’onglet **Vérifier + créer**, où vous pouvez sélectionner **Créer**, en bas, pour créer votre ressource.

:::image type="content" source="media/how-to-integrate-logic-apps/review-logic-apps-custom-connector.png" alt-text="Capture d’écran de l’onglet « Vérifier + créer » de la page « Vérifier un connecteur personnalisé Logic Apps » dans le portail Azure.":::

Vous serez dirigé vers la page de déploiement pour le connecteur. Une fois le déploiement terminé, sélectionnez le bouton **Accéder à la ressource** pour voir les détails du connecteur dans le portail.

### <a name="configure-connector-for-azure-digital-twins"></a>Configurer le connecteur pour Azure Digital Twins

Ensuite, vous allez configurer le connecteur que vous avez créé pour atteindre Azure Digital Twins.

Tout d’abord, téléchargez un fichier Swagger Azure Digital Twins personnalisé qui a été modifié pour fonctionner avec Logic Apps. Téléchargez les [exemples de Swaggers personnalisés Azure Digital Twins (connecteur Logic Apps)](/samples/azure-samples/digital-twins-custom-swaggers/azure-digital-twins-custom-swaggers/) en sélectionnant le bouton **Télécharger le ZIP**. Accédez au dossier téléchargé *Azure_Digital_Twins_custom_Swaggers__Logic_Apps_connector_.zip* et décompressez-le. 

L’exemple de Swagger personnalisé pour ce tutoriel se trouve dans le dossier *digital-twins-custom-swaggers-main\LogicApps*. Ce dossier contient des sous-dossiers appelés *stable* et *preview*, qui contiennent des versions différentes de Swagger organisées par date. Le dossier dont la date est la plus récente contient la dernière copie du fichier de définition Swagger. Quelle que soit la version que vous sélectionnez, le fichier Swagger est nommé _digitaltwins.json_.

> [!NOTE]
> À moins d’utiliser une fonctionnalité en préversion, il est généralement recommandé d’employer la version *stable* la plus récente du fichier Swagger. Toutefois, les versions antérieures et les préversions du fichier Swagger continuent également d’être prises en charge. 

Ensuite, accédez à la page Vue d’ensemble de votre connecteur dans le [portail Azure](https://portal.azure.com), puis sélectionnez **Modifier**.

:::image type="content" source="media/how-to-integrate-logic-apps/edit-connector.png" alt-text="Capture d’écran de la page « Vue d’ensemble » pour le connecteur créé à l’étape précédente. Bouton « Modifier » mis en surbrillance.":::

Dans la page **Modifier le connecteur personnalisé Logic Apps** qui suit, configurez les informations suivantes :
* **Connecteurs personnalisés**
    - Point de terminaison d’API : REST (conservez la valeur par défaut)
    - Mode d’importation : Fichier OpenAPI (conservez la valeur par défaut)
    - Fichier : cette configuration correspond au fichier Swagger personnalisé que vous avez téléchargé. Sélectionnez **Import**, recherchez le fichier sur votre ordinateur (*Azure_Digital_Twins_custom_Swaggers__Logic_Apps_connector_\LogicApps\...\digitaltwins.json*), puis sélectionnez **Ouvrir**.
* **Informations générales**
    - Icône : chargez une icône qui vous plaît.
    - Couleur d’arrière-plan de l’icône : Entrez le code hexadécimal au format « #xxxxxx » pour votre couleur.
    - Description : Remplissez les valeurs de votre choix.
    - Se connecter via une passerelle de données locale : option désactivée (conserver la valeur par défaut)
    - Schéma : HTTPS (conservez la valeur par défaut)
    - Hôte : Le **nom d’hôte** de votre instance Azure Digital Twins.
    - URL de base : / (conservez la valeur par défaut)

Ensuite, sélectionnez le bouton **Sécurité** au bas de la fenêtre pour passer à l’étape de configuration suivante.

:::image type="content" source="media/how-to-integrate-logic-apps/configure-next.png" alt-text="Capture d’écran du bas de la page « Modifier le connecteur personnalisé Logic Apps ». Mise en surbrillance du bouton permettant d’accéder à Sécurité.":::

Dans l’étape Sécurité, sélectionnez **Modifier** et configurez ces informations :
* **Type d'authentification** : OAuth 2.0
* **OAuth 2.0** :
    - Fournisseur d’identité : Azure Active Directory
    - ID client : **ID d’application (client)** pour l’inscription d’application Azure AD que vous avez créée dans [Prérequis](#prerequisites)
    - Clé secrète client : **clé secrète client** de l’inscription d’application 
    - URL de connexion : https://login.windows.net (conservez la valeur par défaut)
    - ID de locataire : **ID d’annuaire (locataire)** pour l’inscription de votre application Azure AD
    - URL de la ressource : 0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    - Étendue : Directory.AccessAsUser.All
    - URL de redirection : (conservez la valeur par défaut pour l’instant)

Le champ URL de redirection indique qu’il est nécessaire d’*enregistrer le connecteur personnalisé pour générer l’URL de redirection*. Générez-le maintenant en sélectionnant **Mettre à jour le connecteur** en haut du volet pour confirmer les paramètres du connecteur.

:::image type="content" source="media/how-to-integrate-logic-apps/update-connector.png" alt-text="Capture d’écran du haut de la page « Modifier le connecteur personnalisé Logic Apps ». Bouton « Mettre à jour le connecteur » mis en surbrillance.":::

Revenez au champ URL de redirection et copiez la valeur qui a été générée. Vous l’utiliserez à l’étape suivante.

:::image type="content" source="media/how-to-integrate-logic-apps/copy-redirect-url.png" alt-text="Capture d’écran du champ URL de redirection de la page « Modifier le connecteur personnalisé Logic Apps ». Le bouton permettant de copier la valeur est mis en surbrillance.":::

Vous avez entré à présent toutes les informations nécessaires à la création de votre connecteur (inutile de passer de l’étape Sécurité à l’étape Définition). Vous pouvez fermer le volet **Modifier le connecteur personnalisé Logic Apps**.

>[!NOTE]
>De retour dans la page Vue d’ensemble de votre connecteur, où vous avez sélectionné **Modifier**, notez que le fait de sélectionner à nouveau sur Modifier redémarre tout le processus de saisie de vos choix de configuration. Cette opération ne renseigne pas vos valeurs entrées la dernière fois. Par conséquent, si vous souhaitez enregistrer une configuration mise à jour avec des valeurs modifiées, vous devez entrer à nouveau toutes les autres valeurs également pour éviter qu’elles ne soient remplacées par les valeurs par défaut.

### <a name="grant-connector-permissions-in-the-azure-ad-app"></a>Accorder des autorisations de connecteur dans l’application Azure AD

Ensuite, utilisez la valeur **URL de redirection** du connecteur personnalisé que vous avez copiée à la dernière étape pour accorder les autorisations de connecteur dans l’inscription de votre application Azure AD.

Accédez à la page [Inscriptions d’applications](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) du portail Azure et sélectionnez votre inscription dans la liste. 

Sous **Authentification** dans le menu de l’inscription, ajoutez un URI.

:::image type="content" source="media/how-to-integrate-logic-apps/add-uri.png" alt-text="Capture d’écran de la page d’authentification pour l’inscription d’application dans le portail Azure. Le bouton « Ajouter un URI » et le menu « Authentification » sont mis en surbrillance."::: 

Entrez **l’URL de redirection** du connecteur personnalisé dans le nouveau champ, puis sélectionnez l’icône **Enregistrer**.

:::image type="content" source="media/how-to-integrate-logic-apps/save-uri.png" alt-text="Capture d’écran de la page Authentification pour l’inscription d’application dans le portail Azure. La nouvelle URL de redirection et le bouton « Enregistrer » sont mis en surbrillance.":::

Vous avez à présent fini de configurer un connecteur personnalisé pouvant accéder aux API Azure Digital Twins. 

## <a name="create-logic-app"></a>Créer une application logique

Ensuite, vous allez créer une application logique qui utilisera votre nouveau connecteur pour automatiser les mises à jour Azure Digital Twins.

Dans le [portail Azure](https://portal.azure.com), recherchez **Logic Apps** dans la barre de recherche. La sélection de cette option a pour effet de vous diriger vers la page **Logic Apps**. Sélectionnez **+ Ajouter** pour créer une application logique.

:::image type="content" source="media/how-to-integrate-logic-apps/create-logic-app.png" alt-text="Capture d’écran de la page « Logic Apps » dans le portail Azure. Le bouton « Créer une application logique » est mis en surbrillance.":::

Dans la page **Application logique** qui suit, entrez votre abonnement et votre groupe de ressources. Sous **Détails de l’instance**, sélectionnez le type d’instance **Consumption**, choisissez un nom pour votre application logique, puis sélectionnez l’emplacement de déploiement. Indiquez si vous souhaitez activer ou désactiver Log Analytics.

Sélectionnez le bouton _Vérifier + Créer_.

Cela vous permet d’accéder à l’onglet **Vérifier + créer**, où vous pouvez vérifier vos informations et sélectionner **Créer**, en bas, pour créer votre ressource.

Vous serez dirigé vers la page de déploiement pour l’application logique. Une fois le déploiement terminé, sélectionnez le bouton **Accéder à la ressource** pour accéder au **Concepteur Logic Apps**, où vous allez indiquer la logique du workflow.

### <a name="design-workflow"></a>Flux de travail de conception

Dans le concepteur Logic Apps, sous **Démarrer avec un déclencheur courant**, sélectionnez **Périodicité**.

:::image type="content" source="media/how-to-integrate-logic-apps/logic-apps-designer-recurrence.png" alt-text="Capture d’écran de la page « Concepteur Logic Apps » dans le portail Azure. Le déclencheur courant « Récurrence » est mis en surbrillance.":::

Dans la page Concepteur Logic Apps qui suit, définissez la fréquence de **Récurrence** sur **Seconde**, afin que l’événement soit déclenché toutes les 3 secondes. La sélection de cette fréquence vous permet de voir facilement les résultats plus tard, sans avoir à attendre très longtemps.

Sélectionnez **+ Nouvelle étape**.

Cela entraîne l’ouverture d’une boîte de dialogue Choisir une action. Basculez vers l’onglet **Personnalisé**. Votre connecteur personnalisé précédemment doit s’afficher dans la zone supérieure.

:::image type="content" source="media/how-to-integrate-logic-apps/custom-action.png" alt-text="Capture d’écran de la création d’un flux dans le Concepteur Logic Apps dans le portail Azure. Le connecteur personnalisé est mis en surbrillance.":::

Sélectionnez-le pour afficher la liste des API contenues dans ce connecteur. Utilisez la barre de recherche ou faites défiler la liste pour sélectionner **DigitalTwins_Add**. (L’action **DigitalTwins_Add** est l’appel d’API utilisé dans cet article, mais vous pouvez également sélectionner n’importe quelle autre API en tant que choix valide pour une connexion Logic Apps).

Vous serez peut-être invité à entrer vos informations d’identification Azure pour vous connecter au connecteur. Si une boîte de dialogue **Autorisations demandées** s’affiche, suivez les invites afin d’accorder le consentement pour votre application et d’accepter.

Dans la nouvelle zone **DigitalTwinsAdd**, renseignez les champs comme suit :
* id : renseignez l’**ID de jumeau** du jumeau numérique dans l’instance que l’application logique doit mettre à jour.
* jumeau : Ce champ vous permet d’entrer le corps requis par la demande d’API choisie. Pour **DigitalTwinsUpdate**, ce corps est sous la forme d’un code de correctif JSON. Pour plus d’informations sur la structuration d’un correctif JSON afin de mettre à jour votre jumeau, consultez la section [Mettre à jour un jumeau numérique](how-to-manage-twin.md#update-a-digital-twin) dans *Procédure : Gestion des jumeaux numériques*.
* api-version : Version la plus récente de l’API. Actuellement, cette valeur est *2020-10-31*.

Sélectionnez **Enregistrer** dans le Concepteur Logic Apps.

Vous pouvez choisir d’autres opérations en sélectionnant _+ Nouvelle étape_ dans la même fenêtre.

:::image type="content" source="media/how-to-integrate-logic-apps/save-logic-app.png" alt-text="Capture d’écran de la vue finale de l’application dans le connecteur Logic App. La zone DigitalTwinsAdd est renseignée avec les valeurs décrites ci-dessus.":::

## <a name="query-twin-to-see-the-update"></a>Interroger le jumeau pour voir la mise à jour

Maintenant que votre application logique a été créée, l’événement de mise à jour du jumeau que vous avez défini dans le Concepteur Logic Apps doit se produire avec une récurrence toutes les trois secondes. La configuration de cette fréquence signifie qu’au bout de trois secondes, vous devez pouvoir interroger votre jumeau et voir les nouvelles valeurs corrigées.

Vous pouvez interroger votre jumeau par le biais de la méthode de votre choix (par exemple une [application cliente personnalisée](tutorial-command-line-app.md), [Azure Digital Twins Explorer](concepts-azure-digital-twins-explorer.md), les [SDK et les API](concepts-apis-sdks.md) ou l’[interface CLI](concepts-cli.md)). 

Pour plus d’informations sur les requêtes de votre instance Azure Digital Twins, consultez [Interroger le graphique de jumeaux](how-to-query-graph.md).

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous avez créé une application logique qui met régulièrement à jour un jumeau dans votre instance Azure Digital Twins avec un correctif que vous avez fourni. Vous pouvez essayer de sélectionner d’autres API dans le connecteur personnalisé afin de créer des applications Logic Apps pour effectuer diverses actions sur votre instance.

Pour en savoir plus sur les opérations d’API disponibles et les informations requises, consultez [Utiliser les API et les kits de développement logiciel (SDK) Azure Digital Twins](concepts-apis-sdks.md).
