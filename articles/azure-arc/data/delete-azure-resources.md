---
title: Supprimer des ressources d’Azure
description: Supprimer des ressources d’Azure
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 07/30/2021
ms.topic: how-to
ms.openlocfilehash: 5a2b51573e4b639c80fd36b69cef667b9ea6eff5
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "122562963"
---
# <a name="delete-resources-from-azure"></a>Supprimer des ressources d’Azure

Cet article décrit comment supprimer des ressources d’Azure.

> [!WARNING]
> Lorsque vous supprimez des ressources comme décrit dans cet article, ces actions sont irréversibles.

## <a name="before"></a>Avant

Avant de supprimer une ressource telle l’instance gérée qu’Azure Arc SQL ou le contrôleur de données Azure Arc, vous devez exporter et charger les informations d’utilisation dans Azure pour obtenir un calcul de facturation précis en suivant les instructions décrites dans [Télécharger les données de facturation dans Azure](view-billing-data-in-azure.md#upload-billing-data-to-azure).

## <a name="direct-connectivity-mode"></a>Mode de connectivité directe

Lorsqu’un cluster est connecté à Azure en mode de connectivité directe, utilisez la Portail Azure pour gérer les ressources. Utilisez le portail pour toutes les opérations de création, lecture, mise à jour & suppression (CRUD) pour les groupes de contrôleur de données, Managed Instance et PostgreSQL.

Consultez [Gérer des ressources Azure à l’aide du Portail Azure](../../azure-resource-manager/management/manage-resources-portal.md).

## <a name="indirect-connectivity-mode"></a>Mode de connectivité indirecte

En mode de connexion indirecte, la suppression d’une instance de Kubernetes ne la supprime pas d’Azure, et la suppression d’une instance d’Azure ne la supprime pas de Kubernetes. En mode de connexion indirecte, la suppression d’une ressource est un processus en deux étapes qui sera amélioré dans le futur. Kubernetes sera la source fidèle et le portail sera mis à jour pour la refléter.

Dans certains cas, il se peut que vous deviez supprimer manuellement les ressources des services de données avec Azure Arc dans Azure.  Vous pouvez supprimer ces ressources à l’aide de l’une des options suivantes.

- [Supprimer des ressources d’Azure](#delete-resources-from-azure)
  - [Supprimer un groupe de ressources entier](#delete-an-entire-resource-group)
  - [Supprimer des ressources spécifiques dans le groupe de ressources](#delete-specific-resources-in-the-resource-group)
  - [Supprimer des ressources à l’aide d’Azure CLI](#delete-resources-using-the-azure-cli)
    - [Supprimer des ressources d’instance gérée SQL à l’aide d’Azure CLI](#delete-sql-managed-instance-resources-using-the-azure-cli)
    - [Supprimer des ressources de groupe de serveurs PostgreSQL Hyperscale à l’aide d’Azure CLI](#delete-postgresql-hyperscale-server-group-resources-using-the-azure-cli)
    - [Supprimer des ressources de contrôleur de données Azure Arc à l’aide d’Azure CLI](#delete-azure-arc-data-controller-resources-using-the-azure-cli)
    - [Supprimer un groupe de ressources à l’aide d’Azure CLI](#delete-a-resource-group-using-the-azure-cli)


## <a name="delete-an-entire-resource-group"></a>Supprimer un groupe de ressources entier

Si vous avez utilisé un groupe de ressources spécifique et dédié pour les services de données activés par Azure Arc et souhaitez supprimer *tout* le contenu du groupe de ressources, vous pouvez supprimer le groupe de ressources, ce qui a pour effet de supprimer tout son contenu.  

Vous pouvez supprimer un groupe de ressources dans le portail Azure en procédant comme suit :

- Accédez au groupe de ressources dans le portail Azure où les ressources des services de données avec Azure Arc ont été créées.
- Cliquez sur le bouton **Supprimer le groupe de ressources**.
- Validez la suppression en entrant le nom du groupe de ressources, puis cliquez sur **Supprimer**.

## <a name="delete-specific-resources-in-the-resource-group"></a>Supprimer des ressources spécifiques dans le groupe de ressources

Vous pouvez supprimer des ressources de services de données activées par Azure Arc dans un groupe de ressources sur le portail Azure en procédant comme suit :

- Accédez au groupe de ressources dans le portail Azure où les ressources des services de données avec Azure Arc ont été créées.
- Sélectionnez toutes les ressources à supprimer.
- Cliquez sur le bouton Supprimer.
- Pour confirmer la suppression, tapez Oui, puis cliquez sur **Supprimer**.

## <a name="delete-resources-using-the-azure-cli"></a>Supprimer des ressources à l’aide d’Azure CLI

Vous pouvez supprimer des ressources de services de données activées par Azure Arc à l’aide d’Azure CLI.

### <a name="delete-sql-managed-instance-resources-using-the-azure-cli"></a>Supprimer des ressources d’instance gérée SQL à l’aide d’Azure CLI

Pour supprimer des ressources d’instance gérée SQL d’Azure à l’aide d’Azure CLI remplacez les valeurs d’espace réservé dans la commande ci-dessous, puis exécutez celle-ci.

```azurecli
az resource delete --name <sql instance name> --resource-type Microsoft.AzureArcData/sqlManagedInstances --resource-group <resource group name>

#Example
#az resource delete --name sql1 --resource-type Microsoft.AzureArcData/sqlManagedInstances --resource-group rg1
```

### <a name="delete-postgresql-hyperscale-server-group-resources-using-the-azure-cli"></a>Supprimer des ressources de groupe de serveurs PostgreSQL Hyperscale à l’aide d’Azure CLI

Pour supprimer une ressource de groupe de serveurs PostgreSQL Hyperscale d’Azure à l’aide d’Azure CLI, remplacez les valeurs d’espace réservé dans la commande ci-dessous, puis exécutez-la.

```azurecli
az resource delete --name <postgresql instance name> --resource-type Microsoft.AzureArcData/postgresInstances --resource-group <resource group name>

#Example
#az resource delete --name pg1 --resource-type Microsoft.AzureArcData/postgresInstances --resource-group rg1
```

### <a name="delete-azure-arc-data-controller-resources-using-the-azure-cli"></a>Supprimer des ressources de contrôleur de données Azure Arc à l’aide d’Azure CLI

> [!NOTE]
> Avant de supprimer un contrôleur de données Azure Arc, vous devez supprimer toutes les ressources de l’instance de base de données qu’il gère.

Pour supprimer un contrôleur de données Azure Arc d’Azure à l’aide d’Azure CLI, remplacez les valeurs d’espace réservé dans la commande ci-dessous, puis exécutez-la.

```azurecli
az resource delete --name <data controller name> --resource-type Microsoft.AzureArcData/dataControllers --resource-group <resource group name>

#Example
#az resource delete --name dc1 --resource-type Microsoft.AzureArcData/dataControllers --resource-group rg1
```

### <a name="delete-a-resource-group-using-the-azure-cli"></a>Supprimer un groupe de ressources à l’aide Azure CLI

Pour [supprimer un groupe de ressources](../../azure-resource-manager/management/delete-resource-group.md), vous pouvez également utiliser Azure CLI.
