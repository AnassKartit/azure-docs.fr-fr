---
title: Services de sécurité et d’identité Azure Germany | Microsoft Docs
description: Cet article fournit une comparaison des services de sécurité et d’identité pour Azure Allemagne.
ms.topic: article
ms.date: 10/16/2020
author: gitralf
ms.author: ralfwi
ms.service: germany
ms.custom: bfdocs
ms.openlocfilehash: 1d66a1f9a1fd2aea17d28d52cbcaa102ec3cb6cc
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/21/2020
ms.locfileid: "122523960"
---
# <a name="azure-germany-security-and-identity-services"></a>Services de sécurité et d’identité Azure Germany

[!INCLUDE [closureinfo](../../includes/germany-closure-info.md)]

## <a name="key-vault"></a>Key Vault
Pour plus d’informations sur le service Azure Key Vault et son utilisation, consultez la [documentation générale Key Vault](../key-vault/index.yml).

Azure Key Vault est mis à la disposition générale dans Azure Germany. Comme Azure global, il n’y a pas d’extension. Key Vault est donc uniquement disponible via PowerShell et l’interface CLI.

## <a name="azure-active-directory"></a>Azure Active Directory
Azure Active Directory offre des fonctionnalités d’identité et d’accès pour les systèmes informatiques exécutés dans Microsoft Azure. Grâce à l’utilisation des services de répertoire, les groupes de sécurité et la stratégie de groupe, vous pouvez aider à contrôler les stratégies d’accès et de sécurité des ordinateurs qui utilisent Azure Active Directory. Vous pouvez utiliser des comptes et des groupes de sécurité, ainsi que le contrôle d’accès en fonction du rôle Azure (Azure RBAC) afin de mieux gérer l’accès aux systèmes d’informations. 

Généralement, Azure Active Directory est disponible dans Azure Germany.

### <a name="variations"></a>Variantes

* Azure Active Directory dans Azure Germany est complètement différent d’Azure Active Directory dans Azure global. 
* Les clients ne peuvent pas utiliser un compte Microsoft pour se connecter à Azure Germany.
* Le suffixe de connexion pour Azure Germany est *onmicrosoft.de* (et non *onmicrosoft.com* comme pour Azure global).
* Les clients doivent disposer d’un abonnement distinct pour travailler dans Azure Germany.
* Les clients dans Azure Germany ne peuevnt pas accéder aux ressources qui nécessitent un abonnement ou une identité dans Azure global.
* Les clients dans Azure global ne peuevnt pas accéder aux ressources qui nécessitent un abonnement ou une identité dans Azure Germany.
* Des domaines supplémentaires peuvent être ajoutés/vérifiés uniquement dans l’un des environnements cloud.
 
> [!NOTE]
> L’attribution de droits aux utilisateurs d’autres abonnés avec *les deux abonnés présents dans Azure Germany* n’est pas encore disponible.


## <a name="next-steps"></a>Étapes suivantes
Pour obtenir des informations supplémentaires et des mises à jour, abonnez-vous au [blog Azure Allemagne](/archive/blogs/azuregermany/).