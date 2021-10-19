---
title: Prise en charge de plusieurs origines par point de terminaison Azure CDN
description: Prise en main de plusieurs origines par point de terminaison Azure CDN.
services: cdn
author: asudbring
manager: KumudD
ms.service: azure-cdn
ms.topic: how-to
ms.date: 08/18/2021
ms.author: allensu
ms.openlocfilehash: 1e17c747c87a5abb184ae4b7b263a8b5bd1a3a49
ms.sourcegitcommit: d2875bdbcf1bbd7c06834f0e71d9b98cea7c6652
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2021
ms.locfileid: "129855989"
---
# <a name="azure-cdn-endpoint-multi-origin"></a>Prise en charge de plusieurs origines par point de terminaison Azure CDN

La prise en charge de plusieurs origines élimine les temps d’arrêt et établit une redondance globale. 

En choisissant plusieurs origines au sein d’un point de terminaison Azure CDN, la redondance fournie réagit au risque en sondant l’intégrité de chaque origine et en basculant si nécessaire.

Configurez un ou plusieurs groupes d’origines et choisissez un groupe d’origines par défaut. Chaque groupe d'origines est un ensemble d'une ou plusieurs origines qui peuvent prendre des charges de travail similaires.

Le premier groupe d’origines est défini comme groupe d’origines par défaut. La fonctionnalité multiorigine est activée lorsqu’un groupe d’origines par défaut pour le point de terminaison du CDN est sélectionné. Une fois la fonctionnalité multiorigine activée, elle ne peut pas être désactivée et le groupe d’origines par défaut ne peut pas être supprimé. Le groupe d’origines par défaut est utilisé pour acheminer les demandes à l’origine. Vous êtes autorisé à mettre à jour la configuration du groupe d’origines et à basculer vers une configuration d’origine unique. Vous êtes également autorisé à modifier la désignation du groupe d’origines par défaut afin d’utiliser un autre groupe d’origines.

> [!NOTE]
> Actuellement, cette fonctionnalité est uniquement disponible à partir d'Azure CDN de Microsoft. 

## <a name="create-the-origin-group"></a>Créer le groupe d'origines

1. Connectez-vous au [portail Azure](https://portal.azure.com)

2. Sélectionnez votre profil Azure CDN, puis choisissez le point de terminaison à configurer avec plusieurs origines.

3. Sélectionnez **Origine** sous **Paramètres** dans la configuration du point de terminaison :

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-1.png" alt-text="Point de terminaison CDN" border="true":::

4. Pour activer plusieurs origines, au moins un groupe d'origines est nécessaire. Sélectionnez **Créer un groupe d'origines** :

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-2.png" alt-text="Paramètres de l'origine" border="true":::

5. Dans la configuration **Ajouter un groupe d'origines**, entrez ou sélectionnez les informations suivantes :

   | Paramètre           | Valeur                                                                 |
   |-------------------|-----------------------------------------------------------------------|
   | Nom du groupe d'origines | Entrez un nom pour votre groupe d'origines.                                   |
   | État de la sonde d’intégrité      | Sélectionnez **Enabled**. </br> Azure CDN exécutera des sondes d’intégrité à partir de différents points dans le monde pour déterminer l’intégrité de l’origine. Ne pas activer si le groupe d’origines actuel n’est pas actif pour éviter des coûts supplémentaires.
   | Chemin d’analyse        | Chemin d'accès à l'origine utilisé pour déterminer l'intégrité. |
   | Intervalle de sondage    | Sélectionnez un intervalle de sondage de 1, 2 ou 4 minutes.                        |
   | Protocole de sondage    | Sélectionnez **HTTP** ou **HTTPS**.                                         |
   | Méthode de sondage      | Sélectionnez **Head** ou **Get**.                                           |
   | Groupe d'origines par défaut | Cochez la case pour définir comme groupe d’origines par défaut.
    
   :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-3.png" alt-text="Ajouter un groupe d'origines" border="true":::

6. Sélectionnez **Ajouter**.

## <a name="add-multiple-origins"></a>Ajouter plusieurs origines

1. Dans les paramètres d'origine de votre point de terminaison, sélectionnez **+ Créer une origine** :

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-5.png" alt-text="Créer une origine" border="true":::

2. Entrez ou sélectionnez les informations suivantes dans la configuration **Ajouter une origine** :

   | Paramètre           | Valeur                                                                 |
   |-------------------|-----------------------------------------------------------------------|
   | Nom        | Entrez le nom de l’origine.        |
   | Type d'origine | Sélectionnez **Stockage**, **Service cloud**, **Application web** ou **Origine personnalisée**.                                   |
   | Nom d’hôte de l’origine        | Sélectionnez ou entrez le nom d'hôte de votre origine.  La liste déroulante répertorie toutes les origines disponibles pour le type que vous avez spécifié au paramètre précédent. Si vous avez sélectionné **Origine personnalisée** comme type d'origine, entrez le domaine du serveur d'origine de votre client. |
   | En-tête de l’hôte d’origine    | Entrez l'en-tête de l'hôte qu'Azure CDN doit envoyer avec chaque requête, ou conservez la valeur par défaut.                        |
   | Port HTTP   | Entrez le port HTTP.                                         |
   | Port HTTPS     | Entrez le port HTTPS.                                           |
   | Priorité    | Entrez un chiffre compris entre 1 et 5.       |
   | Poids      | Entrez un nombre compris entre 1 et 1000.   |

    > [!NOTE]
    > Lorsqu'une origine est créée au sein d'un groupe d'origines, une priorité et un poids doivent lui être attribués. Si un groupe ne comprend qu'une seule origine, la priorité et le poids par défaut sont définis sur 1. Le trafic est acheminé vers l'origine à la priorité la plus élevée si elle est saine. Si une origine n'est pas jugée saine, les connexions sont détournées vers une autre origine, en respectant l'ordre de priorité. Si deux origines ont la même priorité, le trafic est distribué selon le poids spécifié pour l'origine. 

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-6.png" alt-text="Ajouter une origine supplémentaire" border="true":::

3. Sélectionnez **Ajouter**.

4. Sélectionnez **Configurer l'origine** pour définir le chemin d'accès de toutes les origines :

    Le chemin Origin est utilisé pour réécrire l’URL que Microsoft CDN utilise lors de la construction de la requête redirigée vers l’origin. Il transportera également toutes les autres parties de la requête entrante. Par défaut, ce chemin n'est pas fourni. Par conséquent, Microsoft CDN utilisera le chemin URL entrant dans la requête transmise à l’origin.

    Chemin de l'origine : `/fwd/`

    Chemin URL entrant : `/foo/a/b/image1.jpg` </br> URL de Microsoft CDN à l’origin : `fwd/foo/a/b/image1.jpg.`

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-7.png" alt-text="Configurer un chemin d’accès à l’origine" border="true":::

5. Sélectionnez **OK**.

## <a name="configure-origins-and-origin-group-settings"></a>Configurer les paramètres des origines et groupes d'origines

Lorsque vous disposez de plusieurs origines et d'un groupe d'origines, vous pouvez ajouter ou supprimer des origines dans différents groupes. Les origines d'un même groupe doivent servir des charges de travail similaires. Le trafic sera réparti entre ces origines en fonction de leur état d'intégrité, de leur priorité et de leur poids. 

1. Dans les paramètres de l'origine du point de terminaison Azure CDN, sélectionnez le nom du groupe d’origines que vous souhaitez configurer :

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-8.png" alt-text="Configurer les paramètres des origines et du groupe d'origines" border="true":::

2. Dans **Mettre à jour le groupe d'origines**, sélectionnez **+ Sélectionner l’origine** :

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-9.png" alt-text="Mettre à jour le groupe d'origines" border="true":::

4. Sélectionnez l'origine à ajouter au groupe dans la liste déroulante, puis sélectionnez **OK**.

5. Vérifiez que l'origine a été ajoutée au groupe, puis sélectionnez **Enregistrer** :

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-10.png" alt-text="Vérifier l’origine supplémentaire ajoutée au groupe" border="true":::

## <a name="remove-origin-from-origin-group"></a>Supprimer une origine du groupe d'origines

1. Dans les paramètres de l'origine du point de terminaison Azure CDN, sélectionnez le nom du groupe d'origines :

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-8.png" alt-text="Supprimer une origine du groupe" border="true":::

2. Pour supprimer une origine du groupe d'origines, sélectionnez l'icône Corbeille en regard de l'origine, puis sélectionnez **Enregistrer** :

    :::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-11.png" alt-text="Mettre à jour le groupe d’origines supprimer l’origine" border="true":::

## <a name="override-origin-group-with-rules-engine"></a>Remplacer le groupe d’origines par le moteur de règles

Personnaliser la façon dont le trafic est distribué à différents groupes d’origines à l’aide du moteur de règles standard.

Répartissez le trafic vers un autre groupe en fonction de l’URL de la requête.

1. Dans votre point de terminaison CDN, sélectionnez **Moteur de règles** sous **Paramètres** :

:::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-12.png" alt-text="Moteur de règles" border="true":::

2. Sélectionnez **+ Ajouter une règle**.

3. Entrez le nom de la règle dans **Nom**.

4. Sélectionnez **+ Condition**, puis sélectionnez **Chemin de l’URL**.

5. Dans la liste déroulante **opérateur**, sélectionnez **Contient**.

6. Dans **Valeur**, entrez **/images**.

7. Sélectionnez **+ Ajouter une action**, puis sélectionnez **Remplacement du groupe d’origines**.

8. Dans **Groupe d’origines**, sélectionnez le groupe d'origines dans la liste déroulante.

:::image type="content" source="./media/endpoint-multiorigin/endpoint-multiorigin-13.png" alt-text="Conditions du moteur de règles" border="true":::

Pour toutes les requêtes entrantes si le chemin d’accès de l’URL contient **/images**, la requête est alors attribuée au groupe d’origines dans la section d’action **(myorigingroup)** . 

## <a name="next-steps"></a>Étapes suivantes
Dans cet article, vous avez activé plusieurs origines par point de terminaison Azure CDN.

Pour plus d’informations sur Azure CDN et les autres services Azure mentionnés dans cet article, consultez :

* [Azure CDN](./cdn-overview.md)
* [Comparer les caractéristiques du produit CDN Azure](./cdn-features.md)
