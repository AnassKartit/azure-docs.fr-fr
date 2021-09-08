---
title: Définir la configuration technique d’une offre Dynamics 365 pour Customer Engagement et Power Apps sur Microsoft AppSource - Place de marché Azure
description: Définissez la configuration technique d’une offre Dynamics 365 pour Customer Engagement et Power Apps sur Microsoft AppSource (Place de marché Azure).
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
ms.custom: references_regions
author: vamahtan
ms.author: vamahtan
ms.date: 06/29/2021
ms.openlocfilehash: 247c36391826ef427ffe1ed8bcc8f7a8991a44d8
ms.sourcegitcommit: 7854045df93e28949e79765a638ec86f83d28ebc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2021
ms.locfileid: "122864424"
---
# <a name="set-up-dynamics-365-for-customer-engagement--power-apps-offer-technical-configuration"></a>Définir la configuration technique d’une offre Dynamics 365 pour Customer Engagement et Power Apps

Cette page définit les détails techniques utilisés pour se connecter à votre offre. Cette connexion nous permet d’approvisionner votre offre aux clients qui souhaitent l’acquérir.

## <a name="offer-information"></a>Informations sur l’offre

Le **modèle de licence de base** détermine la façon dont votre application est affectée aux clients dans le centre d’administration CRM. Effectuez l’une des opérations suivantes :

- Sélectionner **Ressource** pour une licence basée sur une instance
- Sélectionnez **Utilisateur** si des licences sont attribuées par locataire ou si vous souhaitez gérer vos licences d’application avec Microsoft.

La case à cocher **Requiert l’accès sortant S2S et accès au magasin sécurisé CRM** permet la configuration de l’accès à un magasin sécurisé CRM ou de l’accès sortant S2S (serveur à serveur). Cette fonctionnalité exige une considération spécialisée de la part de l’équipe Dynamics 365 pendant la phase de certification. Microsoft vous contactera pour effectuer des étapes supplémentaires afin de prendre en charge cette fonctionnalité.

Laissez le champ **URL de configuration d’application** vide ; il sera utilisé ultérieurement.

## <a name="crm-package"></a>Package CRM

Dans la case **URL de l’emplacement de votre package**, entrez l’URL d’un compte de Stockage Blob Azure qui contient le fichier .zip du package CRM chargé. Incluez une clé SAP en lecture seule dans l’URL pour que Microsoft puisse sélectionner votre package à des fins de vérification.

> [!IMPORTANT]
> Pour éviter un bloc de publication, assurez-vous que la date d’expiration de l’URL de votre stockage d’objets BLOB n’a pas expiré. Vous pouvez modifier la date en accédant à votre stratégie. Nous vous recommandons d’avoir une **Heure d’expiration** d’au moins un mois dans le futur.

Cochez la case **Il y a plusieurs packages CRM dans mon fichier de package**, le cas échéant. Dans ce cas, veillez à inclure tous les packages dans votre fichier .zip.

Pour plus d’informations sur la façon de générer votre package et de mettre à jour sa structure, consultez l’[Étape 3 : Créer un package AppSource pour votre application](/powerapps/developer/common-data-service/create-package-app-appsource).

## <a name="crm-package-availability"></a>Disponibilité du package CRM

Sélectionnez **+ Ajouter une région** pour spécifier les régions géographiques dans lesquelles votre package CRM sera disponible pour les clients. Ne sélectionnez aucune des régions souveraines suivantes : US Gov Cloud Sovereign, China Cloud Sovereign, Germany Sovereign, US Gov High Cloud Sovereign, Test in production Sovereign et US DoD Cloud Sovereign.

Par défaut, **l’URL de configuration d’application** que vous avez saisie ci-dessus sera utilisée pour chaque région. Laissez le champ d’URL de configuration de l’application vide.

Sélectionnez **Enregistrer le brouillon** avant de passer à l’onglet suivant du menu de navigation de gauche, **Co-vendre avec Microsoft**. Pour plus d’informations sur la configuration de la co-vente avec Microsoft (facultatif), consultez la [Vue d’ensemble de la co-vente avec les équipes de ventes et les partenaires Microsoft](./co-sell-overview.md). Si vous ne configurez pas la co-vente ou si vous avez terminé, passez à **Étapes suivantes** ci-dessous.

## <a name="next-steps"></a>Étapes suivantes

- [Configurer du contenu supplémentaire](dynamics-365-customer-engage-supplemental-content.md)