---
title: 'Tutoriel : Provisionner des appareils X.509 dans Azure IoT Hub avec un module de sécurité matériel (HSM) personnalisé'
description: Ce tutoriel utilise des groupes d’inscriptions. Dans ce tutoriel, vous allez apprendre à provisionner des appareils X.509 à l’aide d’un module de sécurité matériel (HSM) personnalisé et du SDK d’appareil C pour le service Azure IoT Hub Device Provisioning (DPS).
author: wesmc7777
ms.author: wesmc
ms.date: 05/24/2021
ms.topic: tutorial
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: d431e91eb71d5befe71f134e634fe5ad3c7b392d
ms.sourcegitcommit: 613789059b275cfae44f2a983906cca06a8706ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/29/2021
ms.locfileid: "129274466"
---
# <a name="tutorial-provision-multiple-x509-devices-using-enrollment-groups"></a>Tutoriel : Provisionner plusieurs appareils X.509 à l’aide de groupes d’inscriptions

Dans ce tutoriel, vous allez apprendre à provisionner des groupes d’appareils IoT qui utilisent des certificats X.509 à des fins d’authentification. L’exemple de code d’appareil issu du [SDK C Azure IoT](https://github.com/Azure/azure-iot-sdk-c) sera exécuté sur votre ordinateur de développement pour simuler le provisionnement des appareils X.509. Sur de vrais appareils, le code d’appareil serait déployé et exécuté à partir de l’appareil IoT.

Avant de poursuivre ce tutoriel, vérifiez que vous avez au moins effectué les étapes décrites dans [Configurer le service IoT Hub Device Provisioning avec le portail Azure](quick-setup-auto-provision.md). De plus, si vous ne connaissez pas le processus de provisionnement automatique, consultez la vue d’ensemble du [provisionnement](about-iot-dps.md#provisioning-process). 

Le service Azure IoT Device Provisioning prend en charge deux types d’inscriptions pour le provisionnement d’appareils :

* [Groupes d’inscription](concepts-service.md#enrollment-group) : utilisés pour inscrire plusieurs appareils connexes.
* [Inscriptions individuelles](concepts-service.md#individual-enrollment) : utilisées pour inscrire un seul appareil.

Ce tutoriel est similaire aux précédents qui montrent comment utiliser des groupes d’inscriptions pour provisionner des ensembles d’appareils. En revanche, des certificats X.509 vont être utilisés dans ce tutoriel à la place des clés symétriques. Passez en revue les tutoriels précédents de cette section pour découvrir une approche simple à l’aide de [clés symétriques](./concepts-symmetric-key-attestation.md).

Ce tutoriel présente l’[exemple de HSM personnalisé](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client/samples/custom_hsm_example) qui fournit une implémentation de stub pour créer une interface avec le stockage sécurisé basé sur le matériel. Un [module de sécurité matériel (HSM)](./concepts-service.md#hardware-security-module) est utilisé pour le stockage matériel sécurisé des secrets d’appareil. Un HSM peut être utilisé avec une clé symétrique, un certificat X.509 ou une attestation TPM pour assurer le stockage sécurisé des secrets. Le stockage matériel des secrets d’appareil n’est pas obligatoire, mais il est fortement recommandé pour renforcer la protection des informations sensibles telles que la clé privée de votre certificat d’appareil.


Dans ce tutoriel, vous allez atteindre les objectifs suivants :

> [!div class="checklist"]
> * Créer une chaîne de certificats de confiance pour organiser un ensemble d’appareils à l’aide de certificats X.509.
> * Réaliser la preuve de possession avec un certificat de signature utilisé avec la chaîne de certificats.
> * Créer une inscription de groupe qui utilise la chaîne de certificats.
> * Configurer l’environnement de développement pour le provisionnement d’un appareil à l’aide du code du [SDK C Azure IoT](https://github.com/Azure/azure-iot-sdk-c).
> * Provisionner un appareil en utilisant la chaîne de certificats avec l’exemple de module de sécurité matériel (HSM) personnalisé inclus dans le SDK.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prérequis

Les prérequis suivants s’appliquent à un environnement de développement Windows utilisé pour simuler les appareils. Pour Linux ou macOS, consultez la section appropriée de [Préparer votre environnement de développement](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) dans la documentation du kit de développement logiciel (SDK).

* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2019 avec la charge de travail [« Développement Desktop en C++ »](/cpp/ide/using-the-visual-studio-ide-for-cpp-desktop-development) activée. Visual Studio 2015 et Visual Studio 2017 sont également pris en charge. 

    Visual Studio est utilisé dans cet article pour générer l’exemple de code d’appareil qui serait déployé sur des appareils IoT.  Cela ne signifie pas que Visual Studio est nécessaire sur l’appareil lui-même.

* Dernière version de [Git](https://git-scm.com/download/) installée.

## <a name="prepare-the-azure-iot-c-sdk-development-environment"></a>Préparer l’environnement de développement nécessaire au SDK Azure IoT pour C

Dans cette section, vous allez préparer un environnement de développement pour générer le [SDK Azure IoT pour C](https://github.com/Azure/azure-iot-sdk-c). Le SDK comprend un exemple de code et des outils utilisés pour le provisionnement des appareils X.509 avec DPS.

1. Téléchargez le [système de génération CMake](https://cmake.org/download/).

    Il est important que les prérequis de Visual Studio ([Visual Studio](https://visualstudio.microsoft.com/vs/) et la charge de travail [« Développement Desktop en C++ »](/cpp/ide/using-the-visual-studio-ide-for-cpp-desktop-development)) soient installés sur votre machine **avant** de commencer l’installation de `CMake`. Une fois les composants requis en place et le téléchargement effectué, installez le système de génération de CMake.

2. Recherchez le nom d’étiquette de la [version la plus récente](https://github.com/Azure/azure-iot-sdk-c/releases/latest) du SDK C Azure IoT.

3. Ouvrez une invite de commandes ou l’interpréteur de commandes Git Bash. Exécutez les commandes suivantes pour cloner la dernière version du dépôt GitHub du [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c). Utilisez l’étiquette obtenue à l’étape précédente comme valeur pour le paramètre `-b` :

    ```cmd/sh
    git clone -b <release-tag> https://github.com/Azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c
    git submodule update --init
    ```

    Attendez-vous à ce que cette opération prenne plusieurs minutes.

4. Créez un sous-répertoire `cmake` dans le répertoire racine du référentiel Git et accédez à ce dossier. 

    ```cmd/sh
    mkdir cmake
    cd cmake
    ```

5. Le répertoire `cmake` que vous avez créé contient l’exemple de HSM personnalisé, ainsi que l’exemple de code de provisionnement des appareils qui utilise le HSM personnalisé pour assurer l’authentification X.509. 

    Exécutez la commande suivante dans votre répertoire `cmake` pour générer une version du SDK propre à votre plateforme de développement. La build va inclure une référence à l’exemple de HSM personnalisé. 

    Quand vous spécifiez le chemin utilisé avec `-Dhsm_custom_lib` ci-dessous, veillez à utiliser le chemin relatif du répertoire `cmake` que vous avez créé précédemment. Le chemin relatif indiqué ci-dessous n’est qu’un exemple.

    ```cmd
    $ cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=/d/azure-iot-sdk-c/cmake/provisioning_client/samples/custom_hsm_example/Debug/custom_hsm_example.lib ..
    ```

    Si `cmake` ne trouve pas votre compilateur C++, vous obtiendrez peut-être des erreurs de build lors de l’exécution de la commande ci-dessus. Si cela se produit, essayez d’exécuter cette commande dans [l’invite de commandes de Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs).

    Une fois la build réussie, une solution Visual Studio est générée dans votre répertoire `cmake`. Les dernières lignes de la sortie doivent ressembler à ceci :

    ```cmd/sh
    $ cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=/d/azure-iot-sdk-c/cmake/provisioning_client/samples/custom_hsm_example/Debug/custom_hsm_example.lib ..
    -- Building for: Visual Studio 16 2019
    -- The C compiler identification is MSVC 19.23.28107.0
    -- The CXX compiler identification is MSVC 19.23.28107.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: D:/azure-iot-sdk-c/cmake
    ```

## <a name="create-an-x509-certificate-chain"></a>Créer une chaîne de certificats X.509

Dans cette section, vous allez générer une chaîne de trois certificats X.509 pour le test de chaque appareil dans le cadre de ce tutoriel. Les certificats présenteront la hiérarchie suivante.

![Chaîne de certificat d’appareil du tutoriel](./media/tutorial-custom-hsm-enrollment-group-x509/example-device-cert-chain.png#lightbox)

[Certificat racine](concepts-x509-attestation.md#root-certificate) : Vous allez réaliser la [preuve de possession](how-to-verify-certificates.md) pour vérifier le certificat racine. Cette vérification permettra à DPS d’approuver ce certificat et de vérifier les certificats signés par celui-ci.

[Certificat intermédiaire](concepts-x509-attestation.md#intermediate-certificate) : Il est courant d’utiliser des certificats intermédiaires pour regrouper des appareils de manière logique par gammes de produits, divisions d’entreprise ou d’autres critères. Ce tutoriel utilise une chaîne de certificats composée d’un seul certificat intermédiaire. Le certificat intermédiaire est signé par le certificat racine. Ce certificat sert aussi sur le groupe d’inscriptions créé dans DPS pour regrouper un ensemble d’appareils de manière logique. Cette configuration permet de gérer un groupe entier d’appareils dont les certificats d’appareil sont signés par le même certificat intermédiaire. Vous pouvez créer des groupes d’inscriptions pour activer ou désactiver un groupe d’appareils. Pour plus d’informations sur la désactivation d’un groupe d’appareils, consultez [Bloquer un certificat d’autorité de certification racine ou intermédiaire X.509 à l’aide d’un groupe d’inscriptions](how-to-revoke-device-access-portal.md#disallow-an-x509-intermediate-or-root-ca-certificate-by-using-an-enrollment-group).

[Certificats d’appareil](concepts-x509-attestation.md#end-entity-leaf-certificate) : Le certificat (feuille) d’appareil est signé par le certificat intermédiaire et est stocké sur l’appareil avec sa clé privée. Idéalement, ces éléments sensibles seraient stockés de manière sécurisée avec un appareil HSM. Chaque appareil présente son certificat et sa clé privée, ainsi que la chaîne de certificats lors de la tentative de provisionnement. 

#### <a name="create-root-and-intermediate-certificates"></a>Créer des certificats racines et intermédiaires

Pour créer les parties racine et intermédiaire de la chaîne de certificats :

> [!IMPORTANT]
> Pour cet article, utilisez uniquement l'approche de l'interpréteur de commandes Bash. L'utilisation de PowerShell est possible, mais elle n'est pas abordée dans cet article.


1. Ouvrez une invite de commandes Git Bash. Effectuez les étapes 1 et 2 en suivant les instructions de l’interpréteur de commandes Bash qui se trouvent dans [Gestion des certificats d’autorité de certification de test pour les exemples et les tutoriels](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md#managing-test-ca-certificates-for-samples-and-tutorials).

    Cela crée un répertoire de travail pour les scripts de certificat et génère l’exemple de certificat racine et intermédiaire pour la chaîne de certificats avec OpenSSL. 
    
2. Remarquez dans la sortie l’indication de l’emplacement du certificat racine auto-signé. Ce certificat va passer par la phase de [preuve de possession](how-to-verify-certificates.md) pour en vérifier la propriété plus tard.

    ```output
    Creating the Root CA Certificate
    CA Root Certificate Generated At:
    ---------------------------------
        ./certs/azure-iot-test-only.root.ca.cert.pem
    
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number:
                fc:cc:6b:ab:3b:9a:3e:fe
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: CN=Azure IoT Hub CA Cert Test Only
            Validity
                Not Before: Oct 23 21:30:30 2020 GMT
                Not After : Nov 22 21:30:30 2020 GMT
            Subject: CN=Azure IoT Hub CA Cert Test Only
    ```        
    
3. Remarquez dans la sortie l’indication de l’emplacement du certificat intermédiaire signé/émis par le certificat racine. Ce certificat sera utilisé avec le groupe d’inscriptions que vous allez créer plus tard.

    ```output
    Intermediate CA Certificate Generated At:
    -----------------------------------------
        ./certs/azure-iot-test-only.intermediate.cert.pem
    
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number: 1 (0x1)
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: CN=Azure IoT Hub CA Cert Test Only
            Validity
                Not Before: Oct 23 21:30:33 2020 GMT
                Not After : Nov 22 21:30:33 2020 GMT
            Subject: CN=Azure IoT Hub Intermediate Cert Test Only
    ```    
    
#### <a name="create-device-certificates"></a>Créer des certificats d’appareil

Pour créer les certificats d’appareil signés par le certificat intermédiaire de la chaîne :

1. Exécutez la commande suivante pour créer un certificat d’appareil/feuille avec un nom d’objet que vous attribuez en tant que paramètre. Utilisez l’exemple de nom d’objet donné pour ce tutoriel, `custom-hsm-device-01`. Ce nom d’objet sera l’ID de votre appareil IoT. 

    > [!WARNING]
    > N’utilisez pas de nom d’objet comportant des espaces. Ce nom d’objet est l’ID de l’appareil IoT en cours de provisionnement. Les utilisateurs doivent se conformer aux règles en vigueur concernant l’ID d’appareil. Pour plus d’informations, consultez la rubrique [Propriétés d’identité des appareils](../iot-hub/iot-hub-devguide-identity-registry.md#device-identity-properties).

    ```cmd
    ./certGen.sh create_device_certificate_from_intermediate "custom-hsm-device-01"
    ```

    Remarquez la sortie suivante qui indique où se trouve le nouveau certificat d’appareil. Le certificat d’appareil est signé (émis) par le certificat intermédiaire.

    ```output
    -----------------------------------
    ./certs/new-device.cert.pem: OK
    Leaf Device Certificate Generated At:
    ----------------------------------------
        ./certs/new-device.cert.pem
    
    Certificate:
        Data:
            Version: 3 (0x2)
            Serial Number: 9 (0x9)
        Signature Algorithm: sha256WithRSAEncryption
            Issuer: CN=Azure IoT Hub Intermediate Cert Test Only
            Validity
                Not Before: Nov 10 09:20:33 2020 GMT
                Not After : Dec 10 09:20:33 2020 GMT
            Subject: CN=custom-hsm-device-01
    ```    
    
2. Exécutez la commande suivante pour créer un fichier .pem de chaîne de certificats complète qui comprend le nouveau certificat d’appareil pour `custom-hsm-device-01`.

    ```Bash
    cd ./certs && cat new-device.cert.pem azure-iot-test-only.intermediate.cert.pem azure-iot-test-only.root.ca.cert.pem > new-device-01-full-chain.cert.pem && cd ..
    ```

    Utilisez un éditeur de texte et ouvrez le fichier de chaîne de certificats, *./certs/new-device-01-full-chain.cert.pem*. Le texte de la chaîne de certificats contient la chaîne complète des trois certificats. Vous allez utiliser ce texte comme chaîne de certificats dans le code d’appareil HSM personnalisé plus loin dans ce tutoriel pour `custom-hsm-device-01`.

    Le texte de la chaîne complète est au format suivant :
 
    ```output 
    -----BEGIN CERTIFICATE-----
        <Text for the device certificate includes public key>
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
        <Text for the intermediate certificate includes public key>
    -----END CERTIFICATE-----
    -----BEGIN CERTIFICATE-----
        <Text for the root certificate includes public key>
    -----END CERTIFICATE-----
    ```

3. Remarquez que la clé privée du nouveau certificat d’appareil est écrite dans *./private/new-device.key.pem*. Renommez ce fichier de clé *./private/new-device-01.key.pem* pour l’appareil `custom-hsm-device-01`. Le texte de cette clé va être exigé par l’appareil pendant le provisionnement. Le texte sera ajouté plus tard à l’exemple de HSM personnalisé.

    ```bash
    $ mv private/new-device.key.pem private/new-device-01.key.pem
    ```

    > [!WARNING]
    > Le texte des certificats contient uniquement des informations de clé publique. 
    >
    > En revanche, l’appareil doit également avoir accès à la clé privée du certificat d’appareil. Cet accès est nécessaire, car l’appareil doit effectuer la vérification à l’aide de cette clé au moment de l’exécution pendant la tentative de provisionnement. La sensibilité de cette clé est l’une des principales raisons pour lesquelles il est recommandé d’utiliser un stockage matériel dans un réel HSM pour mieux sécuriser les clés privées.

4. Supprimez *./certs/new-device.cert.pem* et répétez les étapes 1 à 3 pour un deuxième appareil avec l'ID `custom-hsm-device-02`. Vous devez supprimer *./certs/new-device.cert.pem* ou la génération du certificat échouera pour le deuxième appareil. Seuls les fichiers de certificat de chaîne complète seront utilisés par cet article. Utilisez les valeurs suivantes pour le deuxième appareil :

    |   Description                 |  Valeur  |
    | :---------------------------- | :--------- |
    | Nom de sujet                  | `custom-hsm-device-02` |
    | Fichier de chaîne de certificats complète   | *./certs/new-device-02-full-chain.cert.pem* |
    | Nom de fichier de clé privée          | *private/new-device-02.key.pem* |
    

## <a name="verify-ownership-of-the-root-certificate"></a>Vérifier la propriété du certificat racine

> [!NOTE]
> À compter du 1er juillet 2021, vous pouvez effectuer une vérification automatique du certificat via la [vérification automatique](how-to-verify-certificates.md#automatic-verification-of-intermediate-or-root-ca-through-self-attestation).
>

1. À l’aide des instructions de la section [Inscrire la partie publique d’un certificat X.509 et obtenir un code de vérification](how-to-verify-certificates.md#register-the-public-part-of-an-x509-certificate-and-get-a-verification-code), chargez le certificat racine (`./certs/azure-iot-test-only.root.ca.cert.pem`) et obtenez un code de vérification à partir de DPS.

2. Une fois que vous avez un code de vérification fourni par DPS pour le certificat racine, exécutez la commande suivante à partir du répertoire de travail de votre script de certificat pour générer un certificat de vérification.
 
    Le code de vérification donné ici n’est qu’un exemple. Utilisez le code que vous avez généré à partir de DPS.    

    ```Bash
    ./certGen.sh create_verification_certificate 1B1F84DE79B9BD5F16D71E92709917C2A1CA19D5A156CB9F    
    ```    

    Ce script crée un certificat signé par le certificat racine avec le nom d’objet défini sur le code de vérification. Ce certificat permet à DPS de vérifier que vous avez accès à la clé privée du certificat racine. Remarquez l’emplacement du certificat de vérification dans la sortie du script. Ce certificat est généré au format `.pfx`.

    ```output
    Leaf Device PFX Certificate Generated At:
    --------------------------------------------
        ./certs/verification-code.cert.pfx
    ```

3. Comme indiqué dans [Charger le certificat de vérification signé](how-to-verify-certificates.md#upload-the-signed-verification-certificate), chargez le certificat de vérification, puis cliquez sur **Vérifier** dans DPS pour réaliser la preuve de possession du certificat racine.


## <a name="update-the-certificate-store-on-windows-based-devices"></a>Mettre à jour le magasin de certificats sur des appareils Windows

Sur des appareils non-Windows, vous pouvez transmettre la chaîne de certificats à partir du code en tant que magasin de certificats.

Sur des appareils Windows, vous devez ajouter les certificats de signature (racine et intermédiaire) à un [magasin de certificats](/windows/win32/secauthn/certificate-stores) Windows. Sinon, les certificats de signature ne sont pas transmis à DPS par un canal sécurisé par le protocole TLS (Transport Layer Security).

> [!TIP]
> Il est également possible d’utiliser OpenSSL au lieu d’un canal sécurisé (Schannel) avec le SDK C. Pour plus d’informations sur l’utilisation d’OpenSSL, consultez [Utilisation d’OpenSSL dans le SDK](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#using-openssl-in-the-sdk).

Pour ajouter les certificats de signature au magasin de certificats dans des appareils Windows :

1. Dans une invite Git Bash, accédez au sous-répertoire `certs` qui contient vos certificats de signature et convertissez-les en `.pfx` comme suit.

    Certificat racine :

    ```bash
    winpty openssl pkcs12 -inkey ../private/azure-iot-test-only.root.ca.key.pem -in ./azure-iot-test-only.root.ca.cert.pem -export -out ./root.pfx
    ```
    
    Certificat intermédiaire :   

    ```bash
    winpty openssl pkcs12 -inkey ../private/azure-iot-test-only.intermediate.key.pem -in ./azure-iot-test-only.intermediate.cert.pem -export -out ./intermediate.pfx
    ```

2. Cliquez avec le bouton droit sur le bouton **Démarrer** de Windows. Cliquez ensuite sur **Exécuter**. Entrez *certmgr.msc*, puis cliquez sur **OK** pour démarrer le composant logiciel enfichable MMC du gestionnaire de certificats.

3. Dans le gestionnaire de certificats, sous **Certificats - Utilisateur actuel**, cliquez sur **Autorités de certification racines de confiance**. Ensuite, dans le menu, cliquez sur **Action** > **Toutes les tâches** > **Importer** pour importer `root.pfx`.

    * Veillez à effectuer une recherche par **Échange d’informations personnelles (.pfx)** .
    * Utilisez `1234` comme mot de passe.
    * Placez le certificat dans le magasin de certificats **Autorités de certification racines de confiance**.

4. Dans le gestionnaire de certificats, sous **Certificats - Utilisateur actuel**, cliquez sur **Autorités de certification intermédiaires**. Ensuite, dans le menu, cliquez sur **Action** > **Toutes les tâches** > **Importer** pour importer `intermediate.pfx`.

    * Veillez à effectuer une recherche par **Échange d’informations personnelles (.pfx)** .
    * Utilisez `1234` comme mot de passe.
    * Placez le certificat dans le magasin de certificats **Autorités de certification intermédiaires**.

Vos certificats de signature sont désormais approuvés sur l’appareil Windows et la chaîne complète peut être transmise à DPS.



## <a name="create-an-enrollment-group"></a>Création d’un groupe d’inscription

1. Connectez-vous au portail Azure, sélectionnez le bouton **Toutes les ressources** dans le menu de gauche, puis ouvrez votre service Device Provisioning.

2. Sélectionnez l’onglet **Gérer les inscriptions**, puis le bouton **Ajouter un groupe d’inscriptions** en haut.

3. Dans le panneau **Ajouter un groupe d’inscriptions**, entrez les informations suivantes, puis appuyez sur le bouton **Enregistrer**.

      ![Ajouter un groupe d’inscriptions pour l’attestation X.509 dans le portail](./media/tutorial-custom-hsm-enrollment-group-x509/custom-hsm-enrollment-group-x509.png#lightbox)

    | Champ        | Valeur           |
    | :----------- | :-------------- |
    | **Nom du groupe** | Pour ce tutoriel, entrez **custom-hsm-x509-devices**. |
    | **Type d’attestation** | Sélectionnez **Certificat**. |
    | **Appareil IoT Edge** | Sélectionnez **Faux**. |
    | **Type de certificat** | Sélectionnez **Certificat intermédiaire**. |
    | **Fichier .pem ou .cer du certificat principal** | Accédez au certificat intermédiaire que vous avez créé précédemment ( *./certs/azure-iot-test-only.intermediate.cert.pem*). Ce certificat intermédiaire est signé par le certificat racine que vous avez déjà chargé et vérifié. DPS approuve cette racine une fois qu’elle a été vérifiée. DPS peut vérifier que le certificat intermédiaire fourni avec ce groupe d’inscriptions est véritablement signé par la racine approuvée. DPS va approuver chaque certificat intermédiaire réellement signé par ce certificat racine, et peut donc vérifier et approuver les certificats feuille signés par le certificat intermédiaire.  |


## <a name="configure-the-provisioning-device-code"></a>Configurer le code de l’appareil de provisionnement

Dans cette section, vous mettez à jour l’exemple de code avec les informations de votre instance du service Device Provisioning. Si l’appareil est authentifié, il est attribué à un hub IoT lié à l’instance du service Device Provisioning configurée dans cette section.

1. Dans le portail Azure, sélectionnez l’onglet **Vue d’ensemble** de votre service Device Provisioning et notez la valeur **_Étendue de l’ID_**.

    ![Extraction des informations de point de terminaison du service Device Provisioning à partir du panneau du Portail](./media/quick-create-simulated-device-x509/copy-id-scope.png) 

2. Lancez Visual Studio et ouvrez le nouveau fichier de solution créé dans le répertoire `cmake` que vous avez créé à la racine du dépôt git azure-iot-sdk-c. Le fichier de la solution est nommé `azure_iot_sdks.sln`.

3. Dans l’Explorateur de solutions pour Visual Studio, accédez à **Provisioning_Samples > prov_dev_client_sample > Source Files** et ouvrez *prov_dev_client_sample.c*.

4. Recherchez la constante `id_scope` et remplacez la valeur par la valeur **Étendue de l’ID** que vous avez copiée précédemment. 

    ```c
    static const char* id_scope = "0ne00000A0A";
    ```

5. Recherchez la définition de la fonction `main()` dans le même fichier. Vérifiez que la variable `hsm_type` a la valeur `SECURE_DEVICE_TYPE_X509`, comme indiqué ci-dessous.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    hsm_type = SECURE_DEVICE_TYPE_X509;
    //hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

6. Cliquez avec le bouton droit sur le projet **prov\_dev\_client\_sample** et sélectionnez **Définir comme projet de démarrage**.


## <a name="configure-the-custom-hsm-stub-code"></a>Configurer le code de stub HSM personnalisé

Les détails de l’interaction avec le stockage matériel sécurisé réel varient en fonction du matériel. Par conséquent, les chaînes de certificats utilisées par les appareils simulés dans ce tutoriel vont être codées en dur dans le code de stub HSM personnalisé. Dans un scénario réel, la chaîne de certificats est stockée dans le matériel HSM réel pour assurer une meilleure sécurité des informations sensibles. Des méthodes similaires aux méthodes stub utilisées dans cet exemple sont ensuite implémentées pour lire les secrets à partir de ce stockage matériel. 

Quand le matériel HSM n’est pas obligatoire, il est recommandé de protéger les informations sensibles, comme la clé privée du certificat. Si un vrai HSM était appelé par l’exemple, la clé privée ne serait pas présente dans le code source. La présence de la clé dans le code source expose la clé à toute personne qui consulte le code. Nous le faisons dans ce tutoriel uniquement pour faciliter l’apprentissage.

Pour mettre à jour le code de stub HSM personnalisé afin de simuler l’identité de l’appareil ayant l’ID `custom-hsm-device-01`, effectuez les étapes suivantes :

1. Dans l’Explorateur de solutions pour Visual Studio, accédez à **Provisioning_Samples > custom_hsm_example > Source Files** et ouvrez *custom_hsm_example.c*.

2. Mettez à jour la valeur de chaîne de la constante de chaîne `COMMON_NAME` à l’aide du nom commun que vous avez utilisé lors de la génération du certificat d’appareil.

    ```c
    static const char* const COMMON_NAME = "custom-hsm-device-01";
    ```

3. Dans le même fichier, vous devez mettre à jour la valeur de la chaîne constante `CERTIFICATE` à l’aide du texte de chaîne de certificats que vous avez enregistré dans *./certs/new-device-01-full-chain.cert.pem* après avoir généré vos certificats.

    La syntaxe du texte du certificat doit suivre le modèle ci-dessous, sans espace supplémentaire ni analyse effectuée par Visual Studio.

    ```c
    // <Device/leaf cert>
    // <intermediates>
    // <root>
    static const char* const CERTIFICATE = "-----BEGIN CERTIFICATE-----\n"
    "MIIFOjCCAyKgAwIBAgIJAPzMa6s7mj7+MA0GCSqGSIb3DQEBCwUAMCoxKDAmBgNV\n"
        ...
    "MDMwWhcNMjAxMTIyMjEzMDMwWjAqMSgwJgYDVQQDDB9BenVyZSBJb1QgSHViIENB\n"
    "-----END CERTIFICATE-----\n"
    "-----BEGIN CERTIFICATE-----\n"
    "MIIFPDCCAySgAwIBAgIBATANBgkqhkiG9w0BAQsFADAqMSgwJgYDVQQDDB9BenVy\n"
        ...
    "MTEyMjIxMzAzM1owNDEyMDAGA1UEAwwpQXp1cmUgSW9UIEh1YiBJbnRlcm1lZGlh\n"
    "-----END CERTIFICATE-----\n"
    "-----BEGIN CERTIFICATE-----\n"
    "MIIFOjCCAyKgAwIBAgIJAPzMa6s7mj7+MA0GCSqGSIb3DQEBCwUAMCoxKDAmBgNV\n"
        ...
    "MDMwWhcNMjAxMTIyMjEzMDMwWjAqMSgwJgYDVQQDDB9BenVyZSBJb1QgSHViIENB\n"
    "-----END CERTIFICATE-----";        
    ```

    La mise à jour de cette valeur de chaîne dans cette étape peut être très fastidieuse et sujette à erreur. Pour générer la syntaxe appropriée dans votre invite Git Bash, copiez et collez les commandes suivantes de l’interpréteur de commandes Bash dans votre invite de commandes Git Bash, puis appuyez sur **Entrée**. Ces commandes génèrent la syntaxe pour la valeur de chaîne de la constante `CERTIFICATE`.

    ```Bash
    input="./certs/new-device-01-full-chain.cert.pem"
    bContinue=true
    prev=
    while $bContinue; do
        if read -r next; then
          if [ -n "$prev" ]; then   
            echo "\"$prev\\n\""
          fi
          prev=$next  
        else
          echo "\"$prev\";"
          bContinue=false
        fi  
    done < "$input"
    ```

    Copiez et collez le texte du certificat de sortie pour la nouvelle valeur de constante. 


4. Dans le même fichier, la valeur de chaîne de la constante `PRIVATE_KEY` doit également être mise à jour avec la clé privée de votre certificat d’appareil.

    La syntaxe du texte de la clé privée doit suivre le modèle ci-dessous, sans espace supplémentaire ni analyse effectuée par Visual Studio.

    ```c
    static const char* const PRIVATE_KEY = "-----BEGIN RSA PRIVATE KEY-----\n"
    "MIIJJwIBAAKCAgEAtjvKQjIhp0EE1PoADL1rfF/W6v4vlAzOSifKSQsaPeebqg8U\n"
        ...
    "X7fi9OZ26QpnkS5QjjPTYI/wwn0J9YAwNfKSlNeXTJDfJ+KpjXBcvaLxeBQbQhij\n"
    "-----END RSA PRIVATE KEY-----";
    ```

    La mise à jour de cette valeur de chaîne dans cette étape peut également être très fastidieuse et sujette à erreur. Pour générer la syntaxe appropriée dans votre invite Git Bash, copiez et collez les commandes suivantes de l’interpréteur de commandes Bash, puis appuyez sur **Entrée**. Ces commandes génèrent la syntaxe pour la valeur de chaîne de la constante `PRIVATE_KEY` .

    ```Bash
    input="./private/new-device-01.key.pem"
    bContinue=true
    prev=
    while $bContinue; do
        if read -r next; then
          if [ -n "$prev" ]; then   
            echo "\"$prev\\n\""
          fi
          prev=$next  
        else
          echo "\"$prev\";"
          bContinue=false
        fi  
    done < "$input"
    ```

    Copiez et collez le texte de la clé privée de sortie pour la nouvelle valeur de constante. 

5. Enregistrez *custom_hsm_example.c*.

6. Dans le menu Visual Studio, sélectionnez **Déboguer** > **Exécuter sans débogage** pour exécuter la solution. Quand vous êtes invité à regénérer le projet, sélectionnez **Oui** pour le faire avant de l’exécuter.

    La sortie suivante illustre un exemple d’appareil simulé `custom-hsm-device-01` démarrant correctement et se connectant au service de provisionnement. L’appareil a été attribué à un hub IoT et inscrit :

    ```cmd
    Provisioning API Version: 1.3.9
    
    Registering Device
    
    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    
    Registration Information received from service: test-docs-hub.azure-devices.net, deviceId: custom-hsm-device-01
    Press enter key to exit:
    ```

7. Dans le portail, accédez au hub IoT lié à votre service de provisionnement, puis sélectionnez l’onglet **Appareils IoT**. En cas de réussite du provisionnement de l’appareil X.509 auprès du hub, son ID d’appareil s’affiche sur le panneau **Appareils IoT**, avec un *ÉTAT* **activé**. Vous devrez peut-être appuyer sur le bouton **Actualiser** dans la partie supérieure. 

    ![L’appareil HSM personnalisé est inscrit auprès du hub IoT.](./media/tutorial-custom-hsm-enrollment-group-x509/hub-provisioned-custom-hsm-x509-device.png) 

8. Répétez les étapes 1 à 7 pour un deuxième appareil ayant l’ID d’appareil `custom-hsm-device-02`. Utilisez les valeurs suivantes pour cet appareil :

    |   Description                 |  Valeur  |
    | :---------------------------- | :--------- |
    | `COMMON_NAME`                 | `"custom-hsm-device-02"` |
    | Chaîne de certificats complète        | Générer le texte à l’aide de `input="./certs/new-device-02-full-chain.cert.pem"` |
    | Clé privée                   | Générer le texte à l’aide de `input="./private/new-device-02.key.pem"` |

    La sortie suivante illustre un exemple d’appareil simulé `custom-hsm-device-02` démarrant correctement et se connectant au service de provisionnement. L’appareil a été attribué à un hub IoT et inscrit :

    ```cmd
    Provisioning API Version: 1.3.9
    
    Registering Device
    
    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    
    Registration Information received from service: test-docs-hub.azure-devices.net, deviceId: custom-hsm-device-02
    Press enter key to exit:
    ```


## <a name="clean-up-resources"></a>Nettoyer les ressources

Quand vous avez terminé de tester et explorer cet exemple de client d’appareil, effectuez les étapes suivantes pour supprimer toutes les ressources créées par ce tutoriel.

1. Fermez la fenêtre de sortie de l’exemple de client d’appareil sur votre machine.
1. Dans le menu de gauche du portail Azure, sélectionnez **Toutes les ressources**, puis votre service Device Provisioning. Ouvrez **Gérer les inscriptions** pour votre service, puis sélectionnez l’onglet **Groupes d’inscriptions**. Cochez la case située en regard de *Nom du groupe* pour le groupe d’appareils que vous avez créé dans ce tutoriel, puis appuyez sur le bouton **Supprimer** en haut du volet. 
1. Cliquez sur **Certificats** dans DPS. Pour chaque certificat que vous avez chargé et vérifié dans le cadre de ce tutoriel, cliquez sur le certificat, puis sur le bouton **Supprimer** pour le supprimer.
1. À partir du menu de gauche, dans le portail Azure, sélectionnez **Toutes les ressources**, puis votre hub IoT. Ouvrez **Appareils IoT** pour votre hub. Cochez la case située en regard de l’*ID d’appareil* d’un appareil que vous avez inscrit dans le cadre de ce tutoriel. Cliquez sur le bouton **Supprimer** au bas du volet.

## <a name="next-steps"></a>Étapes suivantes

Dans ce tutoriel, vous avez provisionné un appareil X.509 à l’aide d’un HSM personnalisé sur votre hub IoT. Pour apprendre à provisionner des appareils IoT sur plusieurs hubs, passez au tutoriel suivant. 

> [!div class="nextstepaction"]
> [Tutoriel : Provisionner des appareils sur des hubs IoT à charge équilibrée](tutorial-provision-multiple-hubs.md)
