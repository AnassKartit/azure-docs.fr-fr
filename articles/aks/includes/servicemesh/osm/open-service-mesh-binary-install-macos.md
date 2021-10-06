---
author: phillipgibson
ms.topic: include
ms.date: 03/15/2021
ms.author: phillipgibson
ms.openlocfilehash: e72c6536ab15246466e16e439997b6deb6547604
ms.sourcegitcommit: add71a1f7dd82303a1eb3b771af53172726f4144
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/03/2021
ms.locfileid: "123441008"
---
## <a name="download-and-install-the-open-service-mesh-osm-client-binary"></a>Télécharger et installer le fichier binaire client Open Service Mesh (OSM)

Dans l’interpréteur de commandes bash, utilisez `curl` pour télécharger la version d’OSM, puis extrayez-la avec `tar` de la façon suivante :

```bash
# Specify the OSM version that will be leveraged throughout these instructions
OSM_VERSION=v0.9.1

curl -sL "https://github.com/openservicemesh/osm/releases/download/$OSM_VERSION/osm-$OSM_VERSION-darwin-amd64.tar.gz" | tar -vxzf -
```

Le binaire client `osm` s’exécute sur votre machine cliente et vous permet de gérer OSM dans votre cluster AKS. Utilisez les commandes suivantes pour installer le binaire client OSM `osm` dans un interpréteur de commandes bash. Ces commandes copient le binaire client `osm` à l’emplacement du programme utilisateur standard dans votre `PATH`.

```bash
sudo mv ./darwin-amd64/osm /usr/local/bin/osm
sudo chmod +x /usr/local/bin/osm
```

Vous pouvez vérifier que la bibliothèque de client `osm` a été correctement ajoutée à votre chemin et son numéro de version à l’aide de la commande suivante.

```
osm version
```
