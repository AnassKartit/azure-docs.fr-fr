---
title: Demander des données de transport public en temps réel avec le service Mobility (préversion) de Microsoft Azure Maps
description: Apprenez à demander des données de transport public en temps réel, telles que des arrivées à un arrêt spécifique. Pour ce faire, découvrez comment utiliser le service Mobility (préversion) d’Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/23/2021
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
ms.custom: mvc
ms.openlocfilehash: 6e7358e76cdcf07f39a9b5a5fcab7ab0151ebc32
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122524909"
---
# <a name="request-real-time-public-transit-data-using-the-azure-maps-mobility-services-preview"></a>Demander des données de transport public en temps réel avec le service Mobility (préversion) d’Azure Maps 

> [!IMPORTANT]
> La version préliminaire des Services Mobility Azure Maps a été retirée et ne sera plus disponible ni prise en charge après le 5 octobre 2021. Tous les autres Services et API Azure Maps ne sont pas affectés par cette annonce de déclassement.
> Pour plus d’informations, consultez [Déclassement des Services Mobility Azure Maps](https://azure.microsoft.com/updates/azure-maps-mobility-services-preview-retirement/).


Cet article vous montre comment utiliser le [service Mobility](/rest/api/maps/mobility) d’Azure Maps pour demander des données de transport public en temps réel.

Dans cet article, découvrez comment demander les horaires d’arrivée en temps réel de toutes les lignes de bus à un arrêt donné

## <a name="prerequisites"></a>Prérequis

Vous devez d’abord disposer d’un compte Azure Maps et d’une clé d’abonnement pour effectuer des appels vers les API de transports publics Azure Maps. Pour plus d'informations sur la création d'un compte Azure Maps, suivez les instructions de la section [Créer un compte](quick-demo-map-app.md#create-an-azure-maps-account). Suivez les étapes de la section [Obtenir la clé primaire](quick-demo-map-app.md#get-the-primary-key-for-your-account) afin de vous procurer la clé primaire de votre compte. Pour plus d’informations sur l’authentification dans Azure Maps, voir [Gérer l’authentification dans Azure Maps](./how-to-manage-authentication.md).

Cet article utilise [l’application Postman](https://www.getpostman.com/apps) pour générer des appels REST. Vous pouvez utiliser l’environnement de développement d’API que vous préférez.

## <a name="request-real-time-arrivals-for-a-stop"></a>Demander les horaires d’arrivée en temps réel à un arrêt donné

Pour demander les horaires d’arrivée en temps réel à un arrêt de transport public particulier, vous devez envoyer une demande à l’[API Real-time Arrivals](/rest/api/maps/mobility/getrealtimearrivalspreview) du [service Mobility](/rest/api/maps/mobility) (préversion) d’Azure Maps. Vous devez indiquer les paramètres **metroID** et **stopID** dans la demande. Pour en savoir plus sur l’obtention de ces paramètres, consultez notre guide sur la façon de [demander des itinéraires des transports publics](./how-to-request-transit-data.md).

Utilisons « 522 » comme numéro de zone urbaine, qui est le numéro de la zone « Seattle–Tacoma–Bellevue, WA ». Utilisons « 522---2060603 » comme numéro d’arrêt ; cet arrêt de bus se trouve à « Ne 24th St & 162nd Ave Ne, Bellevue WA ». Pour demander les horaires d’arrivée en temps réel des cinq prochains passages de bus à cet arrêt, effectuez les étapes suivantes :

1. Ouvrez l’application Postman. Pour créer la requête, sélectionnez **Nouveau**. Dans la fenêtre **Create New** (Créer), sélectionnez **HTTP Request** (Requête HTTP). Entrez un **Request name** (Nom de demande) pour la demande.

2. Sélectionnez la méthode HTTP **GET** sous l'onglet Builder (Générateur), puis entrez l'URL suivante pour créer une requête GET. Remplacez `{subscription-key}` par votre clé primaire Azure Maps.

    ```HTTP
    https://atlas.microsoft.com/mobility/realtime/arrivals/json?subscription-key={subscription-key}&api-version=1.0&metroId=522&query=522---2060603&transitType=bus
    ```

3. Après une demande réussie, vous recevrez la réponse suivante.  Notez que le paramètre scheduleType définit si l’horaire d’arrivée est estimé d’après des données en temps réel ou statiques.

    ```JSON
    {
        "results": [
            {
                "arrivalMinutes": 8,
                "scheduleType": "realTime",
                "patternId": "522---4143196",
                "line": {
                    "lineId": "522---3760143",
                    "lineGroupId": "522---666077",
                    "direction": "backward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "249",
                    "lineDestination": "South Bellevue S Kirkland P&R",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            },
            {
                "arrivalMinutes": 25,
                "scheduleType": "realTime",
                "patternId": "522---3510227",
                "line": {
                    "lineId": "522---2756599",
                    "lineGroupId": "522---666063",
                    "direction": "forward",
                    "agencyId": "522---5872",
                    "agencyName": "Metro Transit",
                    "lineNumber": "226",
                    "lineDestination": "Bellevue Transit Center Crossroads",
                    "transitType": "Bus"
                },
                "stop": {
                    "stopId": "522---2060603",
                    "stopKey": "71300",
                    "stopName": "NE 24th St & 162nd Ave NE",
                    "stopCode": "71300",
                    "position": {
                        "latitude": 47.631504,
                        "longitude": -122.125275
                    },
                    "mainTransitType": "Bus",
                    "mainAgencyId": "522---5872",
                    "mainAgencyName": "Metro Transit"
                }
            }
        ]
    }
    ```

## <a name="next-steps"></a>Étapes suivantes

Découvrez comment demander des données de transport public à l’aide du service Mobility (préversion) :

> [!div class="nextstepaction"]
> [Comment demander des données de transit](how-to-request-transit-data.md)

Explorez la documentation relative à l’API du service Mobility Azure Maps (préversion) :

> [!div class="nextstepaction"]
> [Documentation sur l’API du service Mobility](/rest/api/maps/mobility)