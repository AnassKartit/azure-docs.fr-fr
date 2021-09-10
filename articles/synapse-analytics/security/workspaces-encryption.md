---
title: Chiffrement Azure Synapse Analytics
description: Article qui explique le chiffrement dans Azure Synapse Analytics
author: meenalsri
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: security
ms.date: 07/20/2021
ms.author: mesrivas
ms.reviewer: jrasnick, wiassaf
ms.openlocfilehash: 7e54c654428d86e77f3bad3a92ade0c33a278856
ms.sourcegitcommit: 40866facf800a09574f97cc486b5f64fced67eb2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/30/2021
ms.locfileid: "123225053"
---
# <a name="encryption-for-azure-synapse-analytics-workspaces"></a>Chiffrement pour les espaces de travail Azure Synapse Analytics

Cet article décrit ce qui suit :
* Chiffrement des données au repos dans les espaces de travail Synapse Analytics.
* Configuration des espaces de travail Synapse pour activer le chiffrement avec une clé gérée par le client.
* Gestion des clés utilisées pour chiffrer les données dans les espaces de travail.

## <a name="encryption-of-data-at-rest"></a>Chiffrement des données au repos

Une solution complète de chiffrement au repos fait en sorte que les données ne soient jamais stockées sous une forme non chiffrée. Le double chiffrement des données au repos atténue les menaces avec deux couches distinctes de chiffrement, ce qui offre une protection contre les risques liés à une couche unique. Azure Synapse Analytics offre une deuxième couche de chiffrement pour les données de votre espace de travail avec une clé gérée par le client. Cette clé est sauvegardée dans votre [Azure Key Vault](../../key-vault/general/overview.md), ce qui vous permet de prendre possession de la gestion et de la rotation des clés.

La première couche de chiffrement pour les services Azure est activée avec les clés gérées par la plateforme. Par défaut, les disques Azure et les données dans les comptes de stockage Azure sont automatiquement chiffrés au repos. Pour en savoir plus sur l’utilisation du chiffrement dans Microsoft Azure, accédez à la rubrique [Vue d’ensemble du chiffrement Azure](../../security/fundamentals/encryption-overview.md).

## <a name="azure-synapse-encryption"></a>Chiffrement Azure Synapse

Cette section vous permet de mieux comprendre la façon dont le chiffrement à clé gérée par le client est activé et appliqué dans les espaces de travail Synapse. Ce chiffrement utilise des clés existantes ou de nouvelles clés générées dans Azure Key Vault. Une clé unique est utilisée pour chiffrer toutes les données d’un espace de travail. Les espaces de travail Synapse prennent en charge les clés RSA de 2048 et 3072 octets, ainsi que les clés RSA-HSM.

> [!NOTE]
> Les espaces de travail Synapse ne prennent pas en charge l’utilisation de clés EC, EC-HSM, RSA-HSM et oct-HSM pour le chiffrement. 

Les données des composants Synapse suivants sont chiffrées à l’aide de la clé gérée par le client configurée au niveau de l’espace de travail :
* Pools SQL
 * Pools SQL dédiés
 * Pools SQL serverless
* Pools Apache Spark
* Runtimes d'intégration, pipelines et jeux de données Azure Data Factory.

## <a name="workspace-encryption-configuration"></a>Configuration du chiffrement de l’espace de travail

Les espaces de travail peuvent être configurés pour activer le double chiffrement avec une clé gérée par le client au moment de la création de l’espace de travail. Activez le chiffrement double à l’aide d’une clé gérée par le client sous l’onglet « Sécurité » lors de la création de votre nouvel espace de travail. Vous pouvez choisir d’entrer un URI d’identificateur de clé ou d’effectuer une sélection dans une liste de coffres de clés dans la **même région** que l’espace de travail. La **protection contre le vidage** doit être activée sur le Key Vault lui-même.

> [!IMPORTANT]
> Le paramètre de configuration pour le double chiffrement ne peut pas être modifié après la création de l’espace de travail.

:::image type="content" source="./media/workspaces-encryption/workspaces-encryption.png" alt-text="Ce diagramme montre l’option qui doit être sélectionnée pour activer un espace de travail pour le double chiffrement avec une clé gérée par le client." lightbox="./media/workspaces-encryption/workspaces-encryption.png":::

### <a name="key-access-and-workspace-activation"></a>Accès aux clés et activation de l’espace de travail

Le modèle de chiffrement Azure Synapse avec des clés gérées par le client implique que l’espace de travail accède aux clés dans Azure Key Vault pour chiffrer et déchiffrer si nécessaire. Les clés sont rendues accessibles à l’espace de travail par le biais d’une stratégie d’accès ou d’un [accès RBAC Azure Key Vault](../../key-vault/general/rbac-guide.md). Lorsque vous accordez des autorisations via une stratégie d’accès Azure Key Vault, choisissez l’option [« Application uniquement »](../../key-vault/general/security-features.md#key-vault-authentication-options) lors de la création de la stratégie (sélectionnez l’identité managée de l’espace de travail et ne l’ajoutez pas comme application autorisée).

 L’identité managée par l’espace de travail doit avoir les autorisations nécessaires sur le coffre de clés pour que l’espace de travail puisse être activé. Cette approche progressive de l’activation de l’espace de travail garantit le chiffrement des données dans l’espace de travail avec la clé gérée par le client. Notez que le chiffrement peut être activé ou désactivé pour les Pools SQL dédiés. Par défaut, chaque pool n’est pas activé pour le chiffrement.

#### <a name="using-a-user-assigned-managed-identity"></a>Utilisation d’une identité managée affectée par l’utilisateur
Vous pouvez configurer des espaces de travail pour utiliser une [identité managée affectée par l’utilisateur](../../active-directory/managed-identities-azure-resources/overview.md) afin d’accéder à votre clé gérée par le client stockée dans Azure Key Vault. Configurez une identité managée affectée par l’utilisateur afin d’éviter l’activation par phases de votre espace de travail Azure Synapse lors de l’utilisation du chiffrement double avec des clés gérées par le client. Le rôle intégré Contributeur d’identité managée est requis pour attribuer une identité managée affectées par l’utilisateur à un espace de travail Azure Synapse.
> [!NOTE]
> Une identité managées attribuée par l’utilisateur ne peut pas être configurée pour accéder à une clé gérée par le client quand Azure Key Vault se trouve derrière un pare-feu.

:::image type="content" source="./media/workspaces-encryption/workspaces-encryption-uami.png" alt-text="Ce diagramme montre l’option à sélectionner pour activer un espace de travail afin d’utiliser une identité managée affectée par l’utilisateur pour le chiffrement double avec une clé gérée par le client." lightbox="./media/workspaces-encryption/workspaces-encryption-uami.png":::


#### <a name="permissions"></a>Autorisations

Pour chiffrer ou déchiffrer des données au repos, l’identité managée doit avoir les autorisations suivantes :
* WrapKey (pour insérer une clé dans Key Vault lors de la création d’une clé).
* UnwrapKey (pour obtenir la clé pour le déchiffrement).
* Get (pour lire la partie publique d’une clé)

#### <a name="workspace-activation"></a>Activation de l’espace de travail

Si vous ne configurez pas une identité managée affectée par l’utilisateur pour accéder aux clés gérées par le client lors de la création de l’espace de travail, votre espace de travail reste dans un État « en attente » jusqu’à ce que l’activation aboutisse. L’espace de travail doit être activé pour que vous puissiez utiliser pleinement toutes les fonctionnalités. Par exemple, vous ne pouvez créer un pool SQL dédié qu’une fois l’activation effectuée. Accordez à l’identité managée par l’espace de travail l’accès au coffre de clés, puis cliquez sur le lien d’activation dans la bannière du Portail Azure de l’espace de travail. Une fois l’activation terminée, votre espace de travail est prêt à être utilisé avec l’assurance que toutes les données qu’il contient sont protégées par votre clé gérée par le client. Comme indiqué précédemment, le coffre de clés doit avoir la protection contre le vidage activée pour que l’activation aboutisse.

:::image type="content" source="./media/workspaces-encryption/workspace-activation.png" alt-text="Ce diagramme montre la bannière avec le lien d’activation de l’espace de travail." lightbox="./media/workspaces-encryption/workspace-activation.png":::


### <a name="manage-the-workspace-customer-managed-key"></a>Gérer la clé gérée par le client de l’espace de travail 

Vous pouvez modifier la clé gérée par le client utilisée pour chiffrer les données à partir de la page **Chiffrement** dans le Portail Azure. Ici aussi, vous pouvez choisir une nouvelle clé à l’aide d’un identificateur de clé ou sélectionner des coffres de clés auxquels vous avez accès dans la même région que l’espace de travail. Si vous choisissez une clé dans un coffre de clés différent de ceux utilisés précédemment, accordez les autorisations « Get », « Wrap » et « Unwrap » de l’identité managée par l’espace de travail sur le nouveau coffre de clés. L’espace de travail validera son accès au nouveau coffre de clés et toutes les données de l’espace de travail seront à nouveau chiffrées avec la nouvelle clé.

:::image type="content" source="./media/workspaces-encryption/workspace-encryption-management.png" alt-text="Ce diagramme montre la section de chiffrement de l’espace de travail dans le Portail Azure." lightbox="./media/workspaces-encryption/workspace-encryption-management.png":::

>[!IMPORTANT]
>Lorsque vous modifiez la clé de chiffrement d’un espace de travail, conservez la clé jusqu’à ce que vous la remplaciez dans l’espace de travail par une nouvelle clé. Cela permet d’autoriser le déchiffrement des données avec l’ancienne clé avant qu’elle ne soit rechiffrée avec la nouvelle clé.

Les stratégies Azure Key Vault pour la rotation automatique, périodique des clés ou les actions sur les clés peuvent entraîner la création de nouvelles versions de clé. Vous pouvez choisir de chiffrer à nouveau toutes les données de l’espace de travail avec la dernière version de la clé active. Pour effectuer un nouveau chiffrement, remplacez la clé du Portail Azure par une clé temporaire, puis revenez à la clé que vous souhaitez utiliser pour le chiffrement. Par exemple, pour mettre à jour le chiffrement des données à l’aide de la version la plus récente de la clé active Key1, remplacez la clé gérée par le client de l’espace de travail par clé temporaire, Key2. Attendez la fin du chiffrement avec Key2. Ensuite, rebasculez la clé gérée par le client de l’espace de travail vers Key1. Les données de l’espace de travail seront à nouveau chiffrées avec la dernière version de Key1.

> [!NOTE]
> Azure Synapse Analytics travaille activement sur l’ajout de fonctionnalités permettant de rechiffrer automatiquement les données lors de la création de nouvelles versions de clés. Tant que cette fonctionnalité n’est pas disponible, pour garantir la cohérence dans votre espace de travail, forcez le rechiffrement des données à l’aide du processus détaillé ci-dessus.

#### <a name="sql-transparent-data-encryption-with-service-managed-keys"></a>SQL Transparent Data Encryption avec des clés gérées par le client

SQL Transparent Data Encryption (TDE) est disponible pour les Pools SQL dédiés dans les espaces de travail *non* activés pour le double chiffrement. Dans ce type d’espace de travail, une clé gérée par le service est utilisée pour fournir un double chiffrement pour les données dans les pools SQL dédiés. Le TDE avec la clé gérée par le service peut être activé ou désactivé pour des pools SQL dédiés individuels.

## <a name="next-steps"></a>Étapes suivantes

[Utiliser des stratégies Azure intégrées pour implémenter la protection du chiffrement pour les espaces de travail Synapse](../policy-reference.md)

