---
title: Utilisation du portail Azure pour configurer le téléchargement du fichier | Microsoft Docs
description: Utilisation du portail Azure pour configurer votre IoT Hub en vue d’activer les transferts de fichiers à partir d’appareils connectés. Comprend des informations sur la configuration du compte de stockage Azure de destination.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/20/2021
ms.author: robinsh
ms.openlocfilehash: bf8bf9d1b472ff1986596f6cae7e1f7b415d3bc6
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122524350"
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>Configurer les chargements de fichiers IoT Hub à l’aide du portail Azure

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Cet article explique comment configurer des chargements de fichiers sur votre hub IoT à l’aide du portail Azure. 

Pour utiliser la [fonctionnalité de chargement de fichiers dans IoT Hub](iot-hub-devguide-file-upload.md), vous devez d’abord associer un compte de stockage Azure et un conteneur blob à votre hub IoT. IoT Hub génère automatiquement des URI SAS avec des autorisations d’écriture pour ce conteneur d’objets blob pour les appareils à utiliser lorsqu’ils chargent des fichiers. En plus du compte de stockage et du conteneur blob, vous pouvez définir la durée de vie de l’URI SAS et le type d’authentification que IoT Hub utilise avec le stockage Azure. Vous pouvez également configurer les paramètres des notifications de chargement de fichiers facultatives que IoT Hub peut remettre aux services principaux.

## <a name="prerequisites"></a>Prérequis

* Un compte Azure actif. Si vous ne possédez pas de compte, vous pouvez créer un [compte gratuit](https://azure.microsoft.com/pricing/free-trial/) en quelques minutes.

* Un IoT Hub Azure. Si vous n’avez pas de hub IoT, consultez [Créer un hub IoT en utilisant le portail](iot-hub-create-through-portal.md).

## <a name="configure-your-iot-hub"></a>Configuration de votre IoT Hub

1. Dans le [portail Azure](https://portal.azure.com), accédez à votre hub IoT et sélectionnez **Chargement de fichiers** pour afficher les propriétés de chargement de fichiers. Puis sélectionnez **Conteneur de stockage Azure** sous **paramètres du Conteneur de stockage**.

    :::image type="content" source="./media/iot-hub-configure-file-upload/file-upload-settings.png" alt-text="Afficher les paramètres de chargement de fichier IoT Hub dans le portail":::

1. Sélectionnez un compte de Stockage et un conteneur d’objets blob Azure dans votre abonnement actuel à associer à votre hub IoT. Si nécessaire, vous pouvez créer un compte de Stockage Azure dans le volet **Comptes de stockage** et un conteneur d’objets blob dans le volet **Conteneurs**. 

   :::image type="content" source="./media/iot-hub-configure-file-upload/file-upload-container-selection.png" alt-text="Afficher les conteneurs de stockage pour le chargement de fichiers dans le portail":::

1. Après avoir sélectionné un compte Stockage Azure et un conteneur d’objets blob, configurez le reste des propriétés de chargement de fichiers.    

    :::image type="content" source="./media/iot-hub-configure-file-upload/file-upload-selected-container.png" alt-text="Configurer le chargement de fichier IoT Hub dans le portail":::

    * **Recevoir des notifications pour les fichiers chargés** : Activez ou désactivez les notifications de chargement de fichiers via le bouton bascule.

    * **Durée de vie de SAS** : ce paramètre est la durée de vie des URI de signature d’accès partagé retournés à l’appareil par IoT Hub. Il est défini par défaut sur une heure, mais il peut être personnalisé avec d’autres valeurs à l’aide du curseur.

    * **Durée de vie par défaut des paramètres de notification de fichiers**: la durée de vie d’une notification de chargement avant son expiration. Il est défini par défaut sur un jour, mais il peut être personnalisé avec d’autres valeurs à l’aide du curseur.

    * **Nombre maximal de remises de notifications de fichier** : nombre de tentatives de remise d’une notification de chargement de fichier par le hub IoT. Il est défini par défaut sur 10, mais il peut être personnalisé avec d’autres valeurs à l’aide du curseur.

    * **Type d’authentification** : par défaut, Azure IoT Hub utilise l’authentification basée sur les clés pour la connexion et l’autorisation avec le Stockage Azure. Vous pouvez également configurer des identités managées affectées par l’utilisateur ou affectées par le système pour authentifier Azure IoT Hub avec le Stockage Azure. Les identités managées fournissent aux services Azure une identité managée automatiquement dans Azure AD, de manière sécurisée. Pour savoir comment configurer des identités managées, consultez [Prise en charge IoT Hub pour les identités managées](./iot-hub-managed-identity.md). Une fois que vous avez configuré une ou plusieurs identités managées sur votre compte de Stockage et votre hub IoT Azure, vous pouvez en sélectionner une pour l’authentification avec le Stockage Azure à l’aide des boutons **Affectée par le système** ou **Affectée par l’utilisateur** buttons.

        > [!NOTE]
        > Le paramètre du type d’authentification configure la manière dont votre hub IoT s’authentifie avec votre compte de stockage Azure. Les appareils s’authentifient toujours avec le stockage Azure à l’aide de l’URI SAS qu’ils obtiennent du hub IoT. 

1. Sélectionnez **Save** (Enregistrer) pour enregistrer vos paramètres. Veillez à vérifier la confirmation pour une opération réussie. Certaines sélections, comme le **Type d’authentification**, sont uniquement disponibles après la sauvegarde de vos paramètres. 

## <a name="next-steps"></a>Étapes suivantes

* [Charger des fichiers à partir de l’aperçu d’un appareil](iot-hub-devguide-file-upload.md)
* [Prise en charge des identités managées par IoT Hub](./iot-hub-managed-identity.md)
* [Guides de procédures pour le chargement de fichiers](./iot-hub-csharp-csharp-file-upload.md)
