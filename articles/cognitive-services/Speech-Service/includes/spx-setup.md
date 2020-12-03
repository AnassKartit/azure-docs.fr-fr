---
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/15/2020
ms.author: v-demjoh
ms.openlocfilehash: 6f80d41001d11c52a00454ea2a593f3f1fce32db
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96027540"
---
## <a name="download-and-install"></a>Télécharger et installer

#### <a name="windows-install"></a>[Installation Windows](#tab/windowsinstall)

Procédez ainsi pour installer l’interface CLI Speech sur Windows :

1. Sur Windows, vous avez besoin de [Microsoft Redistributable Visual C++ pour Visual Studio 2019](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) pour votre plateforme. La première installation peut demander i, redémarrage.
2. Téléchargez [l’archive zip](https://aka.ms/speech/spx-zips.zip) de l’interface CLI Speech, puis extrayez-la.
3. Accédez au répertoire où vous avez extrait `spx-zips`. Ce dossier contient les fichiers programme de l’interface CLI de Speech sur diverses plateformes. 
4. Extrayez les fichiers de votre plateforme (`spx-net471` pour .NET Framework 4.7 ou `spx-netcore-win-x64` pour .NET Core 3.0 sur un processeur x64). Gardez à l’esprit que vous allez exécuter `spx` à partir de ce répertoire.

### <a name="run-the-speech-cli"></a>Exécuter l’interface CLI Speech

1. Ouvrez l’invite de commandes ou PowerShell, puis accédez au répertoire dans lequel vous avez extrait l’interface CLI de Speech.  
2. Tapez `spx` pour voir les commandes d’aide de l’interface CLI de Speech.

> [!NOTE]
> PowerShell ne vérifie pas le répertoire local lors de la recherche d’une commande. Dans PowerShell, changez de répertoire en accédant à l’emplacement de `spx` et appelez l’outil en entrant `.\spx`.
> Si vous ajoutez ce répertoire à votre chemin, PowerShell et l’invite de commandes Windows recherchent `spx` à partir de n’importe quel répertoire sans inclure le préfixe `.\`.

### <a name="font-limitations"></a>Limitations des polices

Sur Windows, l’interface CLI de Speech peut afficher uniquement les polices disponibles à l’invite de commandes sur l’ordinateur local.
Le [Terminal Windows](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701) prend en charge toutes les polices produites de manière interactive par l’interface CLI de Speech.

Si la sortie est effectuée dans un fichier, un éditeur de texte tel que le Bloc-notes ou un navigateur web tel que Microsoft Edge peut également afficher toutes les polices.

#### <a name="linux-install"></a>[Installation Linux](#tab/linuxinstall)

Procédez ainsi pour installer l’interface CLI Speech sur Linux, sur un processeur x64 :

1. Installez [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0).
2. Téléchargez [l’archive zip](https://aka.ms/speech/spx-zips.zip) de l’interface CLI Speech, puis extrayez-la.
3. Accédez au répertoire racine `spx-zips` que vous avez extrait du téléchargement, puis extrayez `spx-netcore-30-linux-x64` dans un nouveau répertoire `~/spx`.
4. Dans un terminal, tapez les commandes suivantes :
   1. `cd ~/spx`
   2. `sudo chmod +r+x spx`
   3. `PATH=~/spx:$PATH`

Tapez `spx` pour afficher l’aide de l’interface CLI Speech.

#### <a name="docker-install"></a>[Installation de Docker](#tab/dockerinstall)

> [!NOTE]
> <a href="https://www.docker.com/get-started" target="_blank">Docker Desktop pour votre plateforme <span class="docon docon-navigate-external x-hidden-focus"></span></a> doit être installé.

Suivez la procédure ci-dessous pour installer l’interface CLI Speech dans un conteneur Docker :

1. Dans une nouvelle invite de commandes ou un terminal, entrez cette commande : `docker pull msftspeech/spx`
2. Entrez la commande suivante : Vous devriez voir des informations d’aide sur l’interface CLI Speech : `docker run -it --rm msftspeech/spx help`

### <a name="mount-a-directory-in-the-container"></a>Monter un répertoire dans le conteneur

L’outil d’interface CLI Speech enregistre les paramètres de configuration sous forme de fichiers et charge ces fichiers lors de l’exécution des commandes (à l’exception des commandes d’aide).
Lors de l’utilisation de l’interface CLI Speech dans un conteneur Docker, vous devez monter un répertoire local à partir du conteneur pour permettre à l’outil de stocker ou de rechercher les paramètres de configuration, ainsi que de lire ou d’écrire les fichiers requis par la commande, tels que les fichiers audio.

Sur Windows, entrez la commande suivante pour créer un répertoire local que l’interface CLI Speech peut utiliser au sein du conteneur :

`mkdir c:\spx-data`

Sur Linux ou Mac, entrez cette commande dans un terminal pour créer un répertoire et voir son chemin d’accès absolu :

```bash
mkdir ~/spx-data
cd ~/spx-data
pwd
```

Vous utilisez le chemin d’accès absolu lorsque vous appelez l’interface CLI Speech.

### <a name="run-speech-cli-in-the-container"></a>Exécuter l’interface CLI Speech dans le conteneur

Cette documentation porte sur la commande `spx` CLI Speech utilisée dans les installations autres que Docker.
Lors de l’appel de la commande `spx` dans un conteneur Docker, vous devez monter un répertoire du conteneur dans le système de fichiers pour permettre à l’interface CLI Speech de stocker et de rechercher des valeurs de configuration, et de lire et d’écrire des fichiers.
Sur Windows, vos commandes commencent comme suit :

`docker run -it -v c:\spx-data:/data --rm msftspeech/spx`

Sur Linux ou Mac, vos commandes commencent comme suit :

`sudo docker run -it -v /ABSOLUTE_PATH:/data --rm msftspeech/spx`

> [!NOTE]
> Remplacez `/ABSOLUTE_PATH` par le chemin d’accès absolu indiqué par la commande `pwd` dans la section ci-dessus.

Pour utiliser la commande `spx` installée dans un conteneur, entrez toujours la commande complète présentée ci-dessus, suivie des paramètres de votre requête.
Par exemple, sur Windows, cette commande définit votre clé :

`docker run -it -v c:\spx-data:/data --rm msftspeech/spx config @key --set SUBSCRIPTION-KEY`

> [!NOTE]
> Vous ne pouvez utiliser ni le microphone ni le haut-parleur de votre ordinateur lorsque vous exécutez l’interface CLI Speech dans un conteneur Docker.
> Pour utiliser ces périphériques, transmettez les fichiers audio vers et depuis l’interface CLI Speech à des fins d’enregistrement/de lecture en dehors du conteneur Docker.
> L’outil de l’interface CLI Speech peut accéder au répertoire local que vous avez configuré lors des étapes ci-dessus.

***

## <a name="create-subscription-config"></a>Création d’une configuration d’abonnement

Pour pouvoir utiliser l’interface CLI Speech, vous devez entrer votre clé d’abonnement et votre identificateur de région Speech. Pour obtenir ces informations d’identification, suivez les étapes indiquées dans [Essayer le service Speech gratuitement](../overview.md#try-the-speech-service-for-free).
Une fois que vous avez récupéré votre clé d’abonnement et votre identificateur de la région (par exemple, `eastus`, `westus`), exécutez les commandes suivantes.

```shell
spx config @key --set SUBSCRIPTION-KEY
spx config @region --set REGION
```

L’authentification de votre abonnement est maintenant stockée pour les futures demandes SPX. Si vous devez supprimer l’une de ces valeurs stockées, exécutez `spx config @region --clear` ou `spx config @key --clear`.
