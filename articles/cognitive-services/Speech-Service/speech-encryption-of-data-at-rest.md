---
title: Chiffrement du service Speech pour les données au repos
titleSuffix: Azure Cognitive Services
description: Microsoft propose des clés de chiffrement gérées par Microsoft et vous permet également de gérer vos abonnements Cognitive Services à l’aide de vos propres clés, appelées clés gérées par le client (CMK). Cet article traite du chiffrement des données au repos pour le service vocal.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/14/2021
ms.author: egeaney
ms.openlocfilehash: cdf8276904fda5098b3192779e0372b4a1bcc9d2
ms.sourcegitcommit: 9339c4d47a4c7eb3621b5a31384bb0f504951712
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/14/2021
ms.locfileid: "113766611"
---
# <a name="speech-service-encryption-of-data-at-rest"></a>Chiffrement du service Speech pour les données au repos

Le service Speech chiffre automatiquement vos données lors de leur conservation dans le cloud. Le chiffrement du service Speech protège vos données et vous aide à répondre aux engagements de votre entreprise en matière de sécurité et de conformité.

## <a name="about-cognitive-services-encryption"></a>À propos du chiffrement de Cognitive Services

Les données sont chiffrées et déchiffrées à l'aide du chiffrement [AES 256 bits](https://en.wikipedia.org/wiki/FIPS_140-2) certifié [FIPS 140-2](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard). Le chiffrement et le déchiffrement sont transparents, ce qui signifie que le chiffrement et l’accès sont gérés automatiquement. Vos données étant sécurisées par défaut, vous n’avez pas besoin de modifier votre code ou vos applications pour tirer parti du chiffrement.

## <a name="about-encryption-key-management"></a>À propos de la gestion des clés de chiffrement

Lorsque vous utilisez Custom Speech et Custom Voice, le service Speech peut stocker les données suivantes dans le cloud :  

* Données de trace Speech : uniquement si vous activez la trace pour votre point de terminaison personnalisé
* Données de formation et de test chargées

Par défaut, vos données sont stockées dans un compte de stockage Microsoft et votre abonnement utilise des clés de chiffrement gérées par Microsoft. Vous pouvez également préparer votre propre compte de stockage. L’accès au magasin est géré par la fonctionnalité Identité managée, et le service Speech ne peut pas accéder directement à vos propres données, notamment les données de trace Speech, les données de formation de personnalisation et les modèles personnalisés.

Pour plus d’informations sur Identité managée, consultez [Que sont les identités managées ?](../../active-directory/managed-identities-azure-resources/overview.md).

En attendant, quand vous utilisez une commande personnalisée, vous pouvez gérer votre abonnement avec vos propres clés de chiffrement. Les clés gérées par le client (CMK), également appelées BYOK (Bring Your Own Key), offrent plus de flexibilité pour créer, permuter, désactiver et révoquer des contrôles d’accès. Vous pouvez également effectuer un audit sur les clés de chiffrement utilisées pour protéger vos données. Pour plus d’informations sur les commandes personnalisées et les CMK, consultez [Chiffrement Commandes personnalisées des données au repos](custom-commands-encryption-of-data-at-rest.md).

## <a name="bring-your-own-storage-byos-for-customization-and-logging"></a>Mode Bring your own storage (BYOS) pour la personnalisation et la journalisation

Pour demander l’accès à la fonctionnalité Mode Bring your own storage (BYOS), remplissez puis envoyez le  [formulaire de demande Service Speech - bring your own storage (BYOS)](https://aka.ms/cogsvc-cmk). Une fois votre demande approuvée, vous devrez créer votre propre compte de stockage pour y stocker les données requises pour la personnalisation et la journalisation. Lorsque vous ajoutez un compte de stockage, la ressource du service Speech active une identité managée attribuée par le système.

> [!IMPORTANT]
> Le compte d’utilisateur que vous utilisez pour créer une ressource Speech avec la fonctionnalité BYOS doit se voir attribuer le [rôle de propriétaire dans l’étendue de l’abonnement Azure](../../cost-management-billing/manage/add-change-subscription-administrator.md#to-assign-a-user-as-an-administrator). Dans le cas contraire, vous obtiendrez une erreur d’autorisation lors de l’approvisionnement des ressources.

Une fois que l’identité managée affectée par le système est activée, cette ressource est inscrite auprès d’Azure Active Directory (AAD). Une fois inscrite, l’identité managée aura accès au compte de stockage. Pour plus d’informations sur les identités managées, consultez [Que sont les identités managées ?](../../active-directory/managed-identities-azure-resources/overview.md).

> [!IMPORTANT]
> Si vous désactivez les identités managées attribuées par le système, l’accès au compte de stockage sera supprimé. Les composants du service Speech qui requièrent l’accès au compte de stockage cesseront de fonctionner.  

Actuellement, le service Speech ne prend pas en charge Customer Lockbox. Cependant, les données client peuvent être stockées à l'aide de BYOS, ce qui vous permet d'effectuer des contrôles de données semblables à ceux de [Customer Lockbox](../../security/fundamentals/customer-lockbox-overview.md). N'oubliez pas que les données du service Speech restent et sont traitées dans la région où la ressource Speech a été créée. Cela s'applique à toutes les données au repos ainsi qu'aux données en transit. Lorsque vous utilisez des fonctionnalités de personnalisation, telles que Custom Speech et Custom Voice, toutes les données client sont transférées, stockées et traitées dans la région où résident votre BYOS (si utilisé) et la ressource du service Speech.

> [!IMPORTANT]
> Microsoft **n'utilise pas** les données client pour améliorer ses modèles vocaux. En outre, si la journalisation des points de terminaison est désactivée et qu'aucune personnalisation n'est utilisée, aucune donnée client n'est stockée. 

## <a name="next-steps"></a>Étapes suivantes

* [Formulaire de demande Service Speech - bring your own storage (BYOS)](https://aka.ms/cogsvc-cmk)
* [Que sont les identités managées ?](../../active-directory/managed-identities-azure-resources/overview.md)
