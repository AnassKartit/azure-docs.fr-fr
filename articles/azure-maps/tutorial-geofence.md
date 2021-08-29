---
title: 'Tutoriel : Créer une limite géographique et suivre des appareils sur une carte Microsoft Azure'
description: Tutoriel sur la façon de configurer une limite géographique. Découvrir comment suivre des appareils par rapport à la limite géographique à l’aide du service spatial Azure Maps
author: anastasia-ms
ms.author: v-stharr
ms.date: 7/06/2021
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
ms.custom: mvc
ms.openlocfilehash: f9b2c74f25d5f27385b604d53530edbdb57a91fd
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "121750209"
---
# <a name="tutorial-set-up-a-geofence-by-using-azure-maps"></a>Tutoriel : Configurer une limite géographique à l’aide d’Azure Maps

Ce tutoriel présente les principes de base de la création et de l’utilisation des services de limite géographique Azure Maps. 

Examinez le cas suivant :

*Un responsable de site de construction doit suivre les entrées et les sorties des équipements dans la zone de construction. Chaque fois qu’un équipement entre dans ces périmètres ou en sort, une notification par e-mail est envoyée au chef d’exploitation.*

Azure Maps offre un certain nombre de services permettant de suivre les équipements qui entrent dans la zone de construction et en sortent. Ce didacticiel présente les procédures suivantes :

> [!div class="checklist"]
> * Charger des [données GeoJSON de geofencing](geofence-geojson.md) qui définissent les zones de site de construction que vous souhaitez superviser. Vous utiliserez l’[API de chargement de données](/rest/api/maps/data-v2/upload-preview) pour charger des limites géographiques sous forme de coordonnées de polygone sur votre compte Azure Maps.
> * Configurer deux [applications logiques](../event-grid/handler-webhooks.md#logic-apps) qui, quand elles sont déclenchées (quand un équipement entre dans la limite géographique ou en sort), envoient des notifications par e-mail au chef d’exploitation du site de construction.
> * Utiliser [Azure Event Grid](../event-grid/overview.md) afin de vous abonner aux événements d’entrée et de sortie pour votre limite géographique Azure Maps. Vous configurez deux abonnements aux événements webhook, qui appellent les points de terminaison HTTP définis dans vos deux applications logiques. Celles-ci envoient ensuite les notifications par e-mail appropriées, relatives à l’équipement qui entre dans la limite géographique ou en sort.
> * Utiliser l’[API Get Search Geofence](/rest/api/maps/spatial/getgeofence) pour recevoir des notifications quand un équipement entre dans les zones de limite géographique ou en sort.

## <a name="prerequisites"></a>Prérequis

1. [Créez un compte Azure Maps](quick-demo-map-app.md#create-an-azure-maps-account).
2. [Obtenir une clé d’abonnement principale](quick-demo-map-app.md#get-the-primary-key-for-your-account), également appelée clé primaire ou clé d’abonnement.

Ce tutoriel utilise l’application [Postman](https://www.postman.com/), mais vous pouvez utiliser un autre environnement de développement d’API.

## <a name="upload-geofencing-geojson-data"></a>Charger des données de geofencing GeoJSON

Dans ce tutoriel, vous allez charger les données de geofencing GeoJSON contenant une `FeatureCollection`. La `FeatureCollection` contient deux limites géographiques qui définissent des zones polygonales dans le site de construction. Aucune expiration ni restriction ne sont associées à la première limite géographique. La deuxième ne peut être interrogée que pendant les heures de bureau (de 9h00 à 17h00 dans le fuseau horaire Pacifique) et n’est plus valide après le 1er janvier 2022. Pour plus d’informations sur le format GeoJSON, consultez [Données Geofencing GeoJSON](geofence-geojson.md).

>[!TIP]
>Vous pouvez mettre à jour vos données de geofencing à tout moment. Pour plus d’informations, consultez la page sur l’[API de chargement de données](/rest/api/maps/data-v2/upload-preview).

Pour télécharger les données de geofencing de GeoJSON :

1. Dans l’application Postman, sélectionnez **New** (Nouveau).

2. Dans la fenêtre **Créer**, sélectionnez **Requête HTTP**.

3. Entrez un **Nom de requête** pour la requête, par exemple *Chargement données GeoJSON POST*.

4. Sélectionnez la méthode HTTP **POST**.

5. Entrez l’URL suivante. La requête doit ressembler à l’URL suivante (remplacez `{Azure-Maps-Primary-Subscription-key}` par votre clé d’abonnement principale) :

    ```HTTP
    https://us.atlas.microsoft.com/mapData?subscription-key={Azure-Maps-Primary-Subscription-key}&api-version=2.0&dataFormat=geojson
    ```

    Le paramètre `geojson` de l’URL représente le format des données en cours de chargement.

6. Sélectionnez l’onglet **Corps** .

7. Dans les listes déroulantes, sélectionnez **raw** (brut) et **JSON**.

8. Copiez les données GeoJSON suivantes, puis collez-les dans la fenêtre **Corps** :

   ```JSON
   {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13393688201903,
                  47.63829579223815
                ],
                [
                  -122.13389128446579,
                  47.63782047131512
                ],
                [
                  -122.13240802288054,
                  47.63783312249837
                ],
                [
                  -122.13238388299942,
                  47.63829037035086
                ],
                [
                  -122.13393688201903,
                  47.63829579223815
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "1"
          }
        },
        {
          "type": "Feature",
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -122.13374376296996,
                  47.63784758098976
                ],
                [
                  -122.13277012109755,
                  47.63784577367854
                ],
                [
                  -122.13314831256866,
                  47.6382813338708
                ],
                [
                  -122.1334782242775,
                  47.63827591198201
                ],
                [
                  -122.13374376296996,
                  47.63784758098976
                ]
              ]
            ]
          },
          "properties": {
            "geometryId": "2",
            "validityTime": {
            "expiredTime": "2022-01-01T00:00:00",
            "validityPeriod": [
                {
                  "startTime": "2020-07-15T16:00:00",
                  "endTime": "2020-07-15T24:00:00",
                  "recurrenceType": "Daily",
                  "recurrenceFrequency": 1,
                  "businessDayOnly": true
                }
              ]
            }
          }
        }
      ]
   }
   ```

9. Sélectionnez **Envoyer**.

10. Dans la fenêtre de réponse, sélectionnez l’onglet **En-têtes**.

11. Copiez la valeur de la clé **Operation-Location**, qui correspond à `status URL`. Nous allons utiliser `status URL` pour vérifier l’état du chargement des données GeoJSON.

    ```http
    https://us.atlas.microsoft.com/mapData/operations/<operationId>?api-version=2.0
    ```

### <a name="check-the-geojson-data-upload-status"></a>Vérifier l’état de chargement des données GeoJSON

Pour vérifier l’état des données GeoJSON et récupérer son ID unique (`udid`) :

1. Sélectionnez **Nouveau**.

2. Dans la fenêtre **Créer**, sélectionnez **Requête HTTP**.

3. Entrez un **Nom de requête** pour la requête, par exemple *État chargement données GET*.

4. Sélectionnez la méthode HTTP **GET**.

5. Entrez le `status URL` que vous avez copié dans [Chargement des données de geofencing GeoJSON](#upload-geofencing-geojson-data). La requête doit ressembler à l’URL suivante (remplacez `{Azure-Maps-Primary-Subscription-key}` par votre clé d’abonnement principale) :

   ```HTTP
   https://us.atlas.microsoft.com/mapData/<operationId>?api-version=2.0&subscription-key={Subscription-key}
   ```

6. Sélectionnez **Envoyer**.

7. Dans la fenêtre de réponse, sélectionnez l’onglet **En-têtes**.

8. Copiez la valeur de la clé **Resource-Location**, qui correspond à `resource location URL`. `resource location URL` contient l’identificateur unique (`udid`) des données chargées. Enregistrez `udid` pour pouvoir interroger l’API Get Geofence dans la dernière section de ce tutoriel.

    :::image type="content" source="./media/tutorial-geofence/resource-location-url.png" alt-text="Copiez l’URL de l’emplacement de la ressource.":::

### <a name="optional-retrieve-geojson-data-metadata"></a>(Facultatif) Récupérer les métadonnées des données GeoJSON

Vous pouvez récupérer des métadonnées à partir des données chargées. Les métadonnées contiennent des informations telles que l’URL de la localisation de la ressource, la date de création, la date de mise à jour, la taille et l’état du chargement.

Pour récupérer les métadonnées de contenu :

1. Sélectionnez **Nouveau**.

2. Dans la fenêtre **Créer**, sélectionnez **Requête HTTP**.

3. Entrez un **Nom de requête** pour la requête, par exemple *Chargement métadonnées GET*.

4. Sélectionnez la méthode HTTP **GET**.

5. Entrez le `resource Location URL` que vous avez copié dans [Vérifier l’état du chargement des données GeoJSON](#check-the-geojson-data-upload-status). La requête doit ressembler à l’URL suivante (remplacez `{Azure-Maps-Primary-Subscription-key}` par votre clé d’abonnement principale) :

    ```http
    https://us.atlas.microsoft.com/mapData/metadata/{udid}?api-version=2.0&subscription-key={Azure-Maps-Primary-Subscription-key}
    ```

6. Dans la fenêtre de réponse, sélectionnez l’onglet **Corps**. Les métadonnées doivent ressembler au fragment JSON suivant :

    ```json
    {
        "udid": "{udid}",
        "location": "https://us.atlas.microsoft.com/mapData/6ebf1ae1-2a66-760b-e28c-b9381fcff335?api-version=2.0",
        "created": "5/18/2021 8:10:32 PM +00:00",
        "updated": "5/18/2021 8:10:37 PM +00:00",
        "sizeInBytes": 946901,
        "uploadStatus": "Completed"
    }
    ```

## <a name="create-workflows-in-azure-logic-apps"></a>Créer des workflows dans Azure Logic Apps

Ensuite, nous allons créer deux points de terminaison d’[application logique](../event-grid/handler-webhooks.md#logic-apps) qui déclenchent une notification par e-mail. 

Pour créer les applications logiques :

1. Connectez-vous au [portail Azure](https://portal.azure.com).

2. En haut à gauche du portail Azure, sélectionnez **Créer une ressource**.

3. Dans la zone **Recherche dans la Place de marché**, tapez **Application logique**.

4. Sous Résultats, sélectionnez **Application logique**. Sélectionnez ensuite **Create** (Créer).

5. Dans la page **Application logique**, entrez les valeurs suivantes :
    * Sous **Abonnement**, l’abonnement à utiliser pour cette application logique.
    * Sous **Groupe de ressources**, le nom du groupe de ressources pour cette application logique. Vous pouvez choisir de **Créer** ou d’utiliser un groupe de ressources **Existant**.
    * Sous **Nom de l’application logique**, le nom de votre application logique. En l’occurrence, utilisez le nom `Equipment-Enter`.

    Pour les besoins de ce tutoriel, conservez toutes les autres valeurs par défaut.

    :::image type="content" source="./media/tutorial-geofence/logic-app-create.png" alt-text="Capture d’écran de la création d’une application logique.":::

6. Sélectionnez **Vérifier + créer**. Passez en revue vos paramètres, puis sélectionnez **Créer**.

7. Une fois le déploiement terminé, sélectionnez **Accéder à la ressource**.

8. Dans **Concepteur d’application logique**, faites défiler la page vers le bas pour faire apparaître la section **Démarrer avec un déclencheur courant**. Sélectionnez **Lors de la réception d’une demande HTTP**.

     :::image type="content" source="./media/tutorial-geofence/logic-app-trigger.png" alt-text="Capture d’écran de la création d’un déclencheur HTTP d’application logique.":::

9. Dans l’angle supérieur droit du concepteur d’application logique, sélectionnez **Enregistrer**. L’**URL HTTP POST** est générée automatiquement. Enregistrez l’URL. Vous en aurez besoin dans la section suivante pour créer un point de terminaison d’événement.

    :::image type="content" source="./media/tutorial-geofence/logic-app-httprequest.png" alt-text="Capture d’écran de l’URL et du JSON de la requête HTTP d’application logique":::

10. Sélectionnez **+ Nouvelle étape**.

11. Dans la zone de recherche, tapez `outlook.com email`. Faites défiler la liste **Actions** vers le bas et sélectionnez **Envoyer un e-mail (V2)** .
  
    :::image type="content" source="./media/tutorial-geofence/logic-app-designer.png" alt-text="Capture d’écran du concepteur d’application logique.":::

12. Connectez-vous à votre compte Outlook. Veillez à sélectionner **Oui** pour autoriser l’application logique à accéder au compte. Renseignez les champs pour l’envoi d’un e-mail.

    :::image type="content" source="./media/tutorial-geofence/logic-app-email.png" alt-text="Capture d’écran de l’étape d’envoi d’un e-mail de la création d’une application logique.":::

    >[!TIP]
    > Vous pouvez récupérer des données de réponse GeoJSON, telles que `geometryId` ou `deviceId`, dans vos notifications par e-mail. Vous pouvez configurer Logic Apps pour lire les données envoyées par Event Grid. Pour plus d’informations sur la configuration de Logic Apps pour consommer et transmettre des données d’événement dans des notifications par e-mail, consultez [Tutoriel : Envoyer des notifications par e-mail concernant des événements Azure IoT Hub à l’aide d’Event Grid et de Logic Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md).

13. Dans l’angle supérieur gauche du **Concepteur d’application logique**, sélectionnez **Enregistrer**.

14. Pour créer une deuxième application logique afin de notifier le chef d’exploitation quand un équipement sort du site de construction, répétez la même procédure. Nommez l’application logique `Equipment-Exit`.

## <a name="create-azure-maps-events-subscriptions"></a>Créer des abonnements aux événements Azure Maps

Azure Maps prend en charge [trois types d’événements](../event-grid/event-schema-azure-maps.md). Dans ce tutoriel, nous allons créer des abonnements aux deux événements suivants :

* Événements d’entrée de limite géographique
* Événements de sortie de limite géographique

Pour créer une sortie de limite géographique et entrer l’abonnement aux événements :

1. Dans votre compte Azure Maps, sélectionnez **Abonnements**.

2. Sélectionnez le nom de votre abonnement.

3. Dans le menu des paramètres, sélectionnez **Événements**.

    :::image type="content" source="./media/tutorial-geofence/events-tab.png" alt-text="Capture d’écran de l’accès aux événements de compte Azure Maps.":::

4. Dans la page des événements, sélectionnez **+ Abonnement à un événement**.

    :::image type="content" source="./media/tutorial-geofence/create-event-subscription.png" alt-text="Capture d’écran de la création d’un abonnement aux événements Azure Maps.":::

5. Dans la page **Créer un abonnement aux événements**, entrez les valeurs suivantes :
    * **Nom** de l’abonnement.
    * **Schéma d’événement** : *Schéma Event Grid*.
    * **Nom de la rubrique système** pour cet abonnement aux événements, en l’occurrence `Contoso-Construction`.
    * Pour **Filtrer sur les types d’événements**, choisissez le type d’événement `Geofence Entered`.
    * Pour **Type de point de terminaison**, choisissez `Web Hook`.
    * Pour **Point de terminaison**, copiez l’URL HTTP POST du point de terminaison d’entrée de l’application logique que vous avez créé dans la section précédente. Si vous avez oublié de l’enregistrer, vous pouvez simplement revenir au concepteur d’application logique et le copier (à partir de l’étape relative au déclencheur HTTP).

    :::image type="content" source="./media/tutorial-geofence/events-subscription.png" alt-text="Capture d’écran des détails de l’abonnement aux événements Azure Maps.":::

6. Sélectionnez **Create** (Créer).

7. Répétez le même processus pour l’événement de sortie de la limite géographique. Veillez à choisir le type d’événement `Geofence Exited`.

## <a name="use-spatial-geofence-get-api"></a>Utiliser l’API Get Spatial Geofence

Ensuite, nous allons utiliser l’[API Get Spatial Geofence](/rest/api/maps/spatial/getgeofence) pour envoyer des notifications par e-mail au gestionnaire des opérations quand un équipement entre dans les limites géographiques ou en sort.

Chaque équipement a un `deviceId`. Dans ce tutoriel, vous suivez un seul équipement dont l’ID unique est `device_1`.

Le schéma ci-dessous montre les cinq localisations de l’équipement au fil du temps en commençant par la localisation *Start*, située en dehors des limites géographiques. Pour les besoins de ce tutoriel, la localisation *Start* n’est pas définie, car vous n’interrogerez pas l’appareil à cette localisation.

Quand vous interrogez l’[API d’obtention de limite géographique spatiale](/rest/api/maps/spatial/getgeofence) avec une localisation d’équipement indiquant l’entrée ou la sortie initiale dans la limite géographique, Event Grid appelle le point de terminaison d’application logique approprié pour envoyer une notification par e-mail au chef d’exploitation.

Chacune des sections suivantes effectue des requêtes d’API avec chacune des cinq coordonnées de localisation de l’équipement.

![Schéma de carte de limite géographique dans Azure Maps](./media/tutorial-geofence/geofence.png)

### <a name="equipment-location-1-47638237-122132483"></a>Localisation de l’équipement 1 (47.638237,-122.132483)

1. Dans l’application Postman, sélectionnez **New** (Nouveau).

2. Dans la fenêtre **Créer**, sélectionnez **Requête HTTP**.

3. Entrez un **Nom de requête** pour la requête, par exemple *Emplacement 1*.

4. Sélectionnez la méthode HTTP **GET**.

5. Entrez l’URL suivante. La requête doit ressembler à l’URL suivante (remplacer `{Azure-Maps-Primary-Subscription-key}` par votre clé d’abonnement primaire et `{udid}` par l’`udid` que vous avez enregistré dans la section [Charger les données de geofencing GeoJSON](#upload-geofencing-geojson-data)).

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.638237&lon=-122.1324831&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

6. Sélectionnez **Envoyer**.

7. La réponse doit ressembler au fragment GeoJSON suivant :

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_1",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.638291,
          "nearestLon": -122.132483
        },
        {
          "deviceId": "device_1",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": 999.0,
          "nearestLat": 47.638053,
          "nearestLon": -122.13295
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ```

Dans la réponse GeoJSON précédente, la distance négative par rapport à la limite géographique du site principal signifie que l’équipement se trouve à l’intérieur de la limite géographique. La distance positive par rapport à la limite géographique de site secondaire signifie que le matériel se trouve en dehors de la limite géographique du site secondaire. Étant donné qu’il s’agit de la première fois que cet appareil a été localisé dans la limite géographique du site principal, le paramètre `isEventPublished` est défini sur `true`. Le chef d’exploitation reçoit une notification par e-mail indiquant que l’équipement est entré dans la limite géographique.

### <a name="location-2-4763800-122132531"></a>Localisation 2 (47.63800,-122.132531)

1. Dans l’application Postman, sélectionnez **New** (Nouveau).

2. Dans la fenêtre **Créer**, sélectionnez **Requête HTTP**.

3. Entrez un **Nom de requête** pour la requête, par exemple *Emplacement 2*.

4. Sélectionnez la méthode HTTP **GET**.

5. Entrez l’URL suivante. La requête doit ressembler à l’URL suivante (remplacer `{Azure-Maps-Primary-Subscription-key}` par votre clé d’abonnement primaire et `{udid}` par l’`udid` que vous avez enregistré dans la section [Charger les données de geofencing GeoJSON](#upload-geofencing-geojson-data)).

   ```HTTP
   https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udId={udId}&lat=47.63800&lon=-122.132531&searchBuffer=5&isAsync=True&mode=EnterAndExit
   ```

6. Sélectionnez **Envoyer**.

7. La réponse doit ressembler au fragment GeoJSON suivant :

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.637997,
          "nearestLon": -122.132399
        },
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": 999.0,
          "nearestLat": 47.63789,
          "nearestLon": -122.132809
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": false
    }
    ````

Dans la réponse GeoJSON précédente, l’équipement est resté dans la limite géographique du site principal et n’est pas entré dans la limite géographique du site secondaire. Le paramètre `isEventPublished` est donc défini sur `false`, et le chef d’exploitation ne reçoit aucune notification par e-mail.

### <a name="location-3-4763810783315048-12213336020708084"></a>Localisation 3 (47.63810783315048,-122.13336020708084)

1. Dans l’application Postman, sélectionnez **New** (Nouveau).

2. Dans la fenêtre **Créer**, sélectionnez **Requête HTTP**.

3. Entrez un **Nom de requête** pour la requête, par exemple *Emplacement 3*.

4. Sélectionnez la méthode HTTP **GET**.

5. Entrez l’URL suivante. La requête doit ressembler à l’URL suivante (remplacer `{Azure-Maps-Primary-Subscription-key}` par votre clé d’abonnement primaire et `{udid}` par l’`udid` que vous avez enregistré dans la section [Charger les données de geofencing GeoJSON](#upload-geofencing-geojson-data)).

    ```HTTP
      https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.63810783315048&lon=-122.13336020708084&searchBuffer=5&isAsync=True&mode=EnterAndExit
      ```

6. Sélectionnez **Envoyer**.

7. La réponse doit ressembler au fragment GeoJSON suivant :

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.638294,
          "nearestLon": -122.133359
        },
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "2",
          "distance": -999.0,
          "nearestLat": 47.638161,
          "nearestLon": -122.133549
        }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ````

Dans la réponse GeoJSON précédente, l’équipement est resté dans la limite géographique du site principal, mais il est entré dans la limite géographique du site secondaire. Par conséquent, le paramètre `isEventPublished` est défini sur `true`. Le chef d’exploitation reçoit une notification par e-mail indiquant que l’équipement est entré dans une limite géographique.

>[!NOTE]
>Si l’équipement est entré dans le site secondaire après les heures d’ouverture, aucun événement n’est publié et le chef d’exploitation ne reçoit aucune notification.  

### <a name="location-4-47637988-1221338344"></a>Localisation 4 (47.637988,-122.1338344)

1. Dans l’application Postman, sélectionnez **New** (Nouveau).

2. Dans la fenêtre **Créer**, sélectionnez **Requête HTTP**.

3. Entrez un **Nom de requête** pour la requête, par exemple *Emplacement 4*.

4. Sélectionnez la méthode HTTP **GET**.

5. Entrez l’URL suivante. La requête doit ressembler à l’URL suivante (remplacer `{Azure-Maps-Primary-Subscription-key}` par votre clé d’abonnement primaire et `{udid}` par l’`udid` que vous avez enregistré dans la section [Charger les données de geofencing GeoJSON](#upload-geofencing-geojson-data)).

    ```HTTP
    https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.637988&userTime=2023-01-16&lon=-122.1338344&searchBuffer=5&isAsync=True&mode=EnterAndExit
    ```

6. Sélectionnez **Envoyer**.

7. La réponse doit ressembler au fragment GeoJSON suivant :

    ```json
    {
      "geometries": [
        {
          "deviceId": "device_01",
          "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
          "geometryId": "1",
          "distance": -999.0,
          "nearestLat": 47.637985,
          "nearestLon": -122.133907
        }
      ],
      "expiredGeofenceGeometryId": [
        "2"
      ],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": false
    }
    ````

Dans la réponse GeoJSON précédente, l’équipement est resté dans la limite géographique du site principal, mais il est sorti de la limite géographique du site secondaire. Notez toutefois que la valeur `userTime` est postérieure à la valeur `expiredTime` définie dans les données de limite géographique. Le paramètre `isEventPublished` est donc défini sur `false`, et le chef d’exploitation ne reçoit aucune notification par e-mail.

### <a name="location-5-4763799--122134505"></a>Localisation 5 (47.63799, -122.134505)

1. Dans l’application Postman, sélectionnez **New** (Nouveau).

2. Dans la fenêtre **Créer**, sélectionnez **Requête HTTP**.

3. Entrez un **Nom de requête** pour la requête, par exemple *Emplacement 5*.

4. Sélectionnez la méthode HTTP **GET**.

5. Entrez l’URL suivante. La requête doit ressembler à l’URL suivante (remplacer `{Azure-Maps-Primary-Subscription-key}` par votre clé d’abonnement primaire et `{udid}` par l’`udid` que vous avez enregistré dans la section [Charger les données de geofencing GeoJSON](#upload-geofencing-geojson-data)).

    ```HTTP
    https://atlas.microsoft.com/spatial/geofence/json?subscription-key={subscription-key}&api-version=1.0&deviceId=device_01&udid={udid}&lat=47.63799&lon=-122.134505&searchBuffer=5&isAsync=True&mode=EnterAndExit
    ```

6. Sélectionnez **Envoyer**.

7. La réponse doit ressembler au fragment GeoJSON suivant :

    ```json
    {
      "geometries": [
      {
        "deviceId": "device_01",
        "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
        "geometryId": "1",
        "distance": -999.0,
        "nearestLat": 47.637985,
        "nearestLon": -122.133907
      },
      {
        "deviceId": "device_01",
        "udId": "64f71aa5-bbee-942d-e351-651a6679a7da",
        "geometryId": "2",
        "distance": 999.0,
        "nearestLat": 47.637945,
        "nearestLon": -122.133683
      }
      ],
      "expiredGeofenceGeometryId": [],
      "invalidPeriodGeofenceGeometryId": [],
      "isEventPublished": true
    }
    ````

Dans la réponse GeoJSON précédente, l’équipement est sorti de la limite géographique du site principal. Le paramètre `isEventPublished` est donc défini sur `true`, et le chef d’exploitation reçoit une notification par e-mail indiquant que l’équipement est sorti d’une limite géographique.

Vous pouvez également [envoyer des notifications par e-mail à l’aide d’Event Grid et de Logic Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md) et vérifier les [gestionnaires d’événements pris en charge dans Event Grid](../event-grid/event-handlers.md) à l’aide d’Azure Maps.

## <a name="clean-up-resources"></a>Nettoyer les ressources

Aucune ressource ne nécessite un nettoyage.

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Gérer les types de contenu dans Azure Logic Apps](../logic-apps/logic-apps-content-type.md)
