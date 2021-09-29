---
title: 'Démarrage rapide : Définir et récupérer un certificat dans Azure Key Vault avec Azure PowerShell'
description: Démarrage rapide montrant comment définir et récupérer un certificat dans Azure Key Vault à l’aide d’Azure PowerShell
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019, devx-track-azurepowershell
ms.date: 01/27/2021
ms.author: mbaldwin
ms.openlocfilehash: 170c115e56232b334527d92623c87bacd377f5aa
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128659839"
---
# <a name="quickstart-set-and-retrieve-a-certificate-from-azure-key-vault-using-azure-powershell"></a>Démarrage rapide : Définir et récupérer un certificat dans Azure Key Vault avec Azure PowerShell

Dans ce guide de démarrage rapide, vous créez un coffre de clés dans Azure Key Vault avec Azure PowerShell. Azure Key Vault est un service cloud qui fonctionne comme un magasin des secrets sécurisé. Vous pouvez stocker des clés, des mots de passe, des certificats et d’autres secrets en toute sécurité. Pour plus d’informations sur Key Vault, consultez la [présentation](../general/overview.md). Azure PowerShell vous permet de créer et gérer des ressources Azure à l’aide de commandes ou de scripts. Une fois que vous avez terminé, vous allez stocker un certificat.

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si vous choisissez d’installer et d’utiliser PowerShell en local, ce tutoriel nécessite le module Azure PowerShell version 1.0.0 ou ultérieure. Tapez `$PSVersionTable.PSVersion` pour connaître la version. Si vous devez effectuer une mise à niveau, consultez [Installer le module Azure PowerShell](/powershell/azure/install-az-ps). Si vous exécutez PowerShell en local, vous devez également lancer `Login-AzAccount` pour créer une connexion avec Azure.

```azurepowershell-interactive
Login-AzAccount
```

## <a name="create-a-resource-group"></a>Créer un groupe de ressources

[!INCLUDE [Create a resource group](../../../includes/key-vault-powershell-rg-creation.md)]

## <a name="create-a-key-vault"></a>Création d’un coffre de clés

[!INCLUDE [Create a key vault](../../../includes/key-vault-powershell-kv-creation.md)]

## <a name="add-a-certificate-to-key-vault"></a>Ajouter un certificat à Key Vault

Pour ajouter un certificat au coffre, vous devez effectuer deux autres opérations. Ce certificat peut être utilisé par une application. 

Tapez les commandes ci-dessous pour créer un certificat auto-signé avec une stratégie appelé **ExampleCertificate** :

```azurepowershell-interactive
$Policy = New-AzKeyVaultCertificatePolicy -SecretContentType "application/x-pkcs12" -SubjectName "CN=contoso.com" -IssuerName "Self" -ValidityInMonths 6 -ReuseKeyOnRenewal

Add-AzKeyVaultCertificate -VaultName "<your-unique-keyvault-name>" -Name "ExampleCertificate" -CertificatePolicy $Policy
```

Vous pouvez maintenant référencer ce certificat que vous avez ajouté à Azure Key Vault à l’aide de son URI. Utilisez **`https://<your-unique-keyvault-name>.vault.azure.net/certificates/ExampleCertificate`** pour obtenir la version actuelle. 

Pour voir le certificat stocké précédemment :

```azurepowershell-interactive
Get-AzKeyVaultCertificate -VaultName "<your-unique-keyvault-name>" -Name "ExampleCertificate"
```

Vous venez de créer un coffre de clés, d’y stocker un certificat et de récupérer ce dernier.

**Résolution des problèmes** :

L’opération a retourné un code d’état non valide : « Forbidden ».

Si vous recevez cette erreur, cela signifie que le compte qui accède à Azure Key Vault ne dispose pas des autorisations nécessaires pour créer des certificats.

Exécutez la commande Azure PowerShell suivante pour attribuer les autorisations nécessaires :

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy -VaultName <KeyVaultName> -ObjectId <AzureObjectID> -PermissionsToCertificates get,list,update,create
```

## <a name="clean-up-resources"></a>Nettoyer les ressources

[!INCLUDE [Create a key vault](../../../includes/key-vault-powershell-delete-resources.md)]

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez créé un coffre de clés et vous y avez stocké un certificat. Pour en savoir plus sur Key Vault et sur la manière de l’intégrer à vos applications, consultez les articles ci-dessous.

- Lire la [vue d’ensemble Azure Key Vault](../general/overview.md)
- Consulter la référence des [applets de commande Azure PowerShell Key Vault](/powershell/module/az.keyvault/)
- Passer en revue la [Vue d’ensemble de la sécurité de Key Vault](../general/security-features.md)
