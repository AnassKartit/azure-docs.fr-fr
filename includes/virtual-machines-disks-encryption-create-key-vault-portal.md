---
title: Fichier Include
description: Fichier include
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/15/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3e05dd70ab0919ca4429cee15ecfc9d6ac9ee627
ms.sourcegitcommit: 6c6b8ba688a7cc699b68615c92adb550fbd0610f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122261707"
---
La configuration des clés gérées par le client pour vos disques vous oblige à créer des ressources dans un ordre particulier si vous les utilisez pour la première fois. Tout d’abord, vous devez créer et configurer un Azure Key Vault.

## <a name="set-up-your-azure-key-vault"></a>Configurer votre coffre de clés Azure

1. Connectez-vous au [portail Azure](https://aka.ms/diskencryptionupdates).
1. Recherchez et sélectionnez **Coffre de clés**.

    [![Capture d’écran du portail Azure, dans laquelle la boîte de dialogue de recherche est développée.](./media/virtual-machines-disk-encryption-portal/server-side-encryption-key-vault-portal-search.png)](./media/virtual-machines-disk-encryption-portal/sever-side-encryption-key-vault-portal-search-expanded.png#lightbox)

    > [!IMPORTANT]
    > Pour que le déploiement aboutisse, votre coffre de clés Azure, le jeu de chiffrement de disque, les machines virtuelles, les disques et les captures instantanées doivent tous se trouver dans la même région et le même abonnement.

1. Sélectionnez **+Créer** pour créer un nouveau Coffre de clés.
1. Créez un groupe de ressources.
1. Entrez un nom de coffre de clés, sélectionnez une région, puis sélectionnez un niveau tarifaire.

    > [!NOTE]
    > Lorsque vous créez l’instance Key Vault, vous devez activer la suppression réversible et la protection de purge. La suppression réversible permet de s’assurer que Key Vault contient une clé supprimée pour une période de rétention donnée (90 jours par défaut). La protection de purge garantit qu’une clé supprimée ne peut pas être supprimée définitivement tant que la période de rétention n’est pas écoulée. Ces paramètres vous protègent contre la perte de données en raison d’une suppression accidentelle. Ces paramètres sont obligatoires lors de l’utilisation d’un coffre de clés Key Vault pour le chiffrement des disques managés.

1. Sélectionnez **Vérifier + créer**, vérifiez vos choix, puis sélectionnez **Créer**.

    ![Capture d’écran de l’expérience de création Azure Key Vault. Indication des valeurs spécifiques que vous créez](./media/virtual-machines-disk-encryption-portal/server-side-encryption-create-a-key-vault.png)

1. Une fois que votre coffre de clés a terminé le déploiement, sélectionnez-le.
1. Sous **Paramètres**, sélectionnez **Clés**.
1. Sélectionnez **Générer/Importer**.

    ![Capture d’écran du volet Paramètres des ressources Key Vault. Affiche le bouton Générer/importer dans les paramètres.](./media/virtual-machines-disk-encryption-portal/sever-side-encryption-key-vault-generate-settings.png)

1. Laissez **Type de clé** défini sur **RSA** et **Taille de clé RSA** défini sur **2048**.
1. Renseignez les autres sélections comme vous le souhaitez, puis sélectionnez **Créer**.

    ![Capture d’écran du volet Créer une clé qui apparaît une fois que le bouton générer/importer est sélectionné](./media/virtual-machines-disk-encryption-portal/server-side-encryption-create-a-key-generate.png)

### <a name="add-an-azure-rbac-role"></a>Ajouter un rôle RBAC Azure

Maintenant que vous avez créé le coffre de clés Azure et une clé, vous devez ajouter un rôle RBAC Azure afin de pouvoir utiliser votre coffre de clés Azure avec votre jeu de chiffrement de disque.

1. Sélectionnez **Contrôle d’accès (IAM)** et ajoutez un rôle.
1. Ajoutez les rôles **Administrateur de coffre de clés**, **Propriétaire** ou **Contributeur**.

## <a name="set-up-your-disk-encryption-set"></a>Configuration du jeu de chiffrement de disque

1. Recherchez **Ensembles de chiffrement de disque** et sélectionnez-le.
1. Dans le volet **Jeux de chiffrement de disque** sélectionnez **+ Créer**.

    ![Capture d’écran de l’écran principal du portail de chiffrement de disque. Mise en surbrillance du bouton Ajouter](./media/virtual-machines-disk-encryption-portal/sever-side-encryption-create-disk-encryption-set.png)

1. Sélectionnez votre groupe de ressources, attribuez un nom à votre jeu de chiffrement et sélectionnez la même région que votre coffre de clés.
1. Pour **Type de chiffrement**, sélectionnez **Chiffrement au repos avec une clé gérée par le client**.

    > [!NOTE]
    > Une fois que vous avez créé un jeu de chiffrement de disque avec un type de chiffrement particulier, il n’est plus possible de le modifier. Si vous souhaitez utiliser un type de chiffrement différent, vous devez créer un nouveau jeu de chiffrement de disque.

1. Sélectionnez **Cliquer pour choisir une clé**.
1. Sélectionnez le coffre de clés et la clé que vous avez créés précédemment, ainsi que la version.
1. Appuyez sur **Sélectionner**.
1. Sélectionnez **Vérifier + créer**, puis **Créer**.

    ![Capture d’écran du volet de création du chiffrement de disque. Indique l’abonnement, le groupe de ressources, le nom du jeu de chiffrement de disque, la région et le coffre de clés + sélecteur de clé.](./media/virtual-machines-disk-encryption-portal/server-side-encryption-disk-set-blade.png)
