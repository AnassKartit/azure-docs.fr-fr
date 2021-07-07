---
title: Résoudre les problèmes liés à l'inscription d'un nouveau compte sur le portail Azure
description: Résolution d'un problème rencontré lors de la tentative de création d'un compte sur le portail Microsoft Azure.
services: cost-management-billing
author: v-miegge
manager: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: troubleshooting
ms.date: 05/13/2021
ms.author: v-miegge
ms.openlocfilehash: 6e9c3d09c5dbf9ddae779f585425f8317b41ab46
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/26/2021
ms.locfileid: "110467512"
---
# <a name="troubleshoot-issues-when-you-sign-up-for-a-new-account-in-azure-portal"></a>Résoudre les problèmes liés à l'inscription d'un nouveau compte sur le portail Azure

Un problème peut survenir lorsque vous tentez d'inscrire un nouveau compte sur le portail Microsoft Azure. Ce guide succinct vous guide tout au long du processus d’inscription et aborde certains problèmes courants à chaque étape.

> [!NOTE]
> Si vous disposez déjà d’un compte existant et que vous recherchez des conseils pour résoudre les problèmes de connexion, consultez [Résoudre les problèmes de connexion à un abonnement Azure](./troubleshoot-sign-in-issue.md).

## <a name="before-you-begin"></a>Avant de commencer

Avant de commencer l’inscription, vérifiez les points suivants :

- Les informations de votre profil Azure (comme votre adresse e-mail, votre adresse postale et votre numéro de téléphone) sont correctes.
- Les informations de votre carte de crédit sont correctes.
- Vous n’avez pas déjà un compte Microsoft avec les mêmes informations.

## <a name="guided-walkthrough-of-azure-sign-up"></a>Procédure pas à pas pour l’inscription à Azure

L’expérience d’inscription à Azure se compose de quatre sections :

- À propos de vous
- Vérification d’identité par téléphone
- Vérification d'identité par carte
- Contrat

Cette procédure pas à pas fournit des exemples d’informations correctes à fournir pour créer un compte Azure. Chaque section contient également des problèmes courants et explique comment les résoudre.

## <a name="about-you"></a>À propos de vous

Lors de votre inscription sur Azure, vous devez fournir des informations sur vous, notamment :

- Votre pays ou région
- Prénom
- Nom
- Adresse de messagerie
- Numéro de téléphone
- Informations de carte de crédit
 
### <a name="common-issues-and-solutions"></a>Problèmes courants et solutions

#### <a name="you-see-the-message-we-cannot-proceed-with-sign-up-due-to-an-issue-with-your-account-please-contact-billing-support"></a>Vous voyez le message : « En raison d’un problème lié à votre compte, nous ne pouvons pas procéder à l’inscription. Veuillez contacter le support de facturation. »

Pour résoudre cette erreur, effectuez les étapes suivantes :

1. Connectez-vous au [Centre des comptes Microsoft](https://account.microsoft.com/).
1. En haut de la page, sélectionnez **Vos informations**.
1. Vérifiez que vos données de facturation et d'expédition sont complètes et valides.
1.  Quand vous souscrivez l’abonnement Azure, vérifiez que l’adresse de facturation entrée pendant l’enregistrement de la carte de crédit correspond à celle que possède votre banque.

Si vous recevez toujours le même message, réessayez de vous inscrire à l’aide d’un autre navigateur.

Qu’en est-il de la navigation InPrivate ?

#### <a name="free-trial-is-not-available"></a>L’essai gratuit n’est pas disponible

Avez-vous utilisé un abonnement Azure auparavant ? Les conditions d’utilisation d’Azure n’autorisent l’activation que d’une seule évaluation gratuite pour tout utilisateur n’ayant jamais utilisé Azure. Si vous avez déjà souscrit un autre type d’abonnement Azure, vous ne pouvez pas activer une version d'évaluation gratuite. Inscrivez-vous à un [abonnement de type paiement à l’utilisation](https://azure.microsoft.com/offers/ms-azr-0003p/).

#### <a name="you-see-the-message-you-are-not-eligible-for-an-azure-subscription"></a>Vous voyez le message « Vous n’êtes pas éligible à un abonnement Azure »

Pour résoudre ce problème, vérifiez que les points suivants sont vrais :

- Les informations que vous avez fournies pour votre profil de compte Azure (notamment l’adresse e-mail du contact, l’adresse postale et le numéro de téléphone) sont correctes.
- Les informations de la carte de crédit sont correctes.
- Vous n’avez pas déjà un compte Microsoft utilisant les mêmes informations.

#### <a name="you-see-the-message-your-current-account-type-is-not-supported"></a>Vous voyez le message « Le type de votre compte actuel n’est pas pris en charge »

Ce problème peut se produire si le compte est inscrit dans un [répertoire Azure AD non managé](../../active-directory/enterprise-users/directory-self-service-signup.md) et ne se trouve pas dans le répertoire Azure AD de votre organisation.
Pour résoudre ce problème, inscrivez le compte Azure en utilisant un autre compte, ou prenez en charge le répertoire AD non managé. Pour plus d’informations, consultez [Prendre le contrôle d’un annuaire non géré en tant qu’administrateur dans Azure Active Directory](../../active-directory/enterprise-users/domains-admin-takeover.md).

## <a name="identity-verification-by-phone"></a>Vérification d’identité par téléphone

![Vérification d’identité par téléphone](./media/troubleshoot-azure-sign-up/2.png)
 
Lorsque vous obtenez le message texte ou recevez l’appel, entrez le code que vous avez reçu dans la zone de texte.

### <a name="common-issues-and-solutions"></a>Problèmes courants et solutions

#### <a name="no-verification-text-message-or-phone-call"></a>Aucun SMS ou appel téléphonique de vérification

Bien que le processus de vérification de l’inscription soit généralement rapide, l’envoi du code de vérification peut prendre jusqu’à quatre minutes.

Voici quelques conseils supplémentaires :

- Vous pouvez utiliser n’importe quel numéro de téléphone pour la vérification, à condition qu’il réponde aux exigences. Le numéro de téléphone que vous entrez pour la vérification n’est pas stocké sous forme de numéro de contact pour le compte.
  - Vous ne pouvez pas utiliser un numéro de téléphone VoIP pour le processus de vérification par téléphone.
  - Vérifiez que votre téléphone peut recevoir des appels ou des SMS provenant d’un numéro de téléphone basé aux États-Unis.
- Vérifiez par deux fois le numéro de téléphone saisi, y compris l’indicatif du pays que vous sélectionnez dans le menu déroulant.
- Si votre téléphone ne reçoit pas les messages texte (SMS), essayez l’option **M’appeler**.

## <a name="identity-verification-by-card"></a>Vérification d'identité par carte

![Vérification d'identité par carte](./media/troubleshoot-azure-sign-up/3.png)
 
### <a name="common-issues-and-solutions"></a>Problèmes courants et solutions

#### <a name="credit-card-declined-or-not-accepted"></a>Carte de crédit refusée

Les cartes de crédit ou de débit virtuelles ou prépayées ne sont pas acceptées comme mode de paiement pour les abonnements Azure. Pour voir les autres raisons qui peuvent conduire au refus de votre carte de crédit, consultez [Résoudre les problèmes de refus de carte lors de l’inscription à Azure](./troubleshoot-declined-card.md).

#### <a name="credit-card-form-doesnt-support-my-billing-address"></a>Le formulaire relatif à la carte de crédit ne prend pas en charge mon adresse de facturation

Votre adresse de facturation doit se trouver dans le pays que vous avez sélectionné dans la section **About you** (À propos de vous). Vérifiez que vous avez sélectionné le pays approprié.

#### <a name="progress-bar-hangs-in-identity-verification-by-card-section"></a>La barre de progression se bloque dans la section Vérification d’identité par carte.

Pour terminer la vérification d’identité par carte, les cookies tiers doivent être autorisés pour votre navigateur.

Effectuez la procédure suivante pour mettre à jour les paramètres de cookies de votre navigateur.

1. Mettez à jour les paramètres de cookie.
   - Si vous utilisez **Chrome** :
     - Sélectionnez **Paramètres** > **Afficher les paramètres avancés** > **Confidentialité** > **Paramètres de contenu**. Décochez **Bloquer les cookies et les données de site tiers**.

   - Si vous utilisez **Microsoft Edge** :
     - Sélectionnez **Paramètres** > **Afficher les paramètres avancés** > **Cookies** > **Ne pas bloquer les cookies**.

1. Actualisez la page d’inscription à Azure, puis vérifiez si le problème est résolu.
1. Si l’actualisation n’a pas résolu le problème, quittez le navigateur, redémarrez-le, puis réessayez.

### <a name="i-saw-a-charge-on-my-free-trial-account"></a>J’ai vu des frais sur mon compte d’essai gratuit

Vous pouvez voir une petite vérification temporaire retenue sur votre compte de carte de crédit après votre inscription. La retenue est supprimée dans un délai de trois à cinq jours. Si vous êtes soucieux quant à la gestion des coûts, lisez l’article dédié à l’[analyse des frais inattendus](../understand/analyze-unexpected-charges.md).

## <a name="agreement"></a>Contrat

Terminez le contrat.

## <a name="other-issues"></a>Autres problèmes

### <a name="cant-activate-azure-benefit-plan-like-visual-studio-bizspark-bizsparkplus-or-mpn"></a>Impossible d’activer un plan d’avantages Azure de type Visual Studio, BizSpark, BizSparkPlus ou MPN

Vérifiez que vous utilisez les bonnes informations de connexion. Consultez ensuite le programme d’avantages pour vérifier que vous êtes éligible.
- Visual Studio
  - Vérifiez votre état d’éligibilité dans la [page de votre compte Visual Studio](https://my.visualstudio.com/Benefits).
  - Si vous ne pouvez pas vérifier votre état, contactez le [support des abonnements Visual Studio](https://visualstudio.microsoft.com/subscriptions/support/).
- Microsoft for Startups
  - Connectez-vous au [Portail Microsoft for Startups](https://startups.microsoft.com/#start-two) pour vérifier l’état de votre éligibilité à Microsoft for Startups.
  - Si vous ne pouvez pas vérifier votre état, vous pouvez obtenir de l’aide sur les [forums Microsoft for Startups](https://www.microsoftpartnercommunity.com/t5/Microsoft-for-Startups/ct-p/Microsoft_Startups).
- MPN
  - Connectez-vous au [portail MPN](https://mspartner.microsoft.com/Pages/Locale.aspx) pour vérifier l’état de votre éligibilité. Vous pouvez bénéficier d’avantages supplémentaires si vous disposez des [compétences de plateforme cloud](https://mspartner.microsoft.com/pages/membership/cloud-platform-competency.aspx) appropriées.
  - Si vous ne pouvez pas vérifier votre état, contactez le [support MPN](https://mspartner.microsoft.com/Pages/Support/Premium/contact-support.aspx).

### <a name="cant-activate-new-azure-in-open-subscription"></a>Je n’arrive pas à activer un nouvel abonnement Azure dans Open

Pour pouvoir créer un nouvel abonnement Azure dans Open, vous devez disposer d’une clé d’activation de service en ligne (OSA) valide associée à au moins un jeton Azure dans Open. Si vous ne disposez d’aucune clé OSA, contactez l’un des partenaires Microsoft répertoriés dans [Microsoft Pinpoint](https://pinpoint.microsoft.com/).

## <a name="additional-help-resources"></a>Ressources d'aide supplémentaires

Autres articles sur la résolution des problèmes liés à la facturation et aux abonnements Azure

- [Carte refusée](./troubleshoot-declined-card.md)
- [Problèmes liés à la souscription d'un abonnement](./troubleshoot-sign-in-issue.md)
- [Aucun abonnement trouvé](./no-subscriptions-found.md)
- [Désactivation de l’affichage des coûts d’entreprise](./enterprise-mgmt-grp-troubleshoot-cost-view.md)

## <a name="contact-us-for-help"></a>Contactez-nous pour obtenir de l'aide

Si vous avez des questions ou besoin d’aide, [créez une demande de support](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="find-out-more-about-azure-cost-management"></a>En savoir plus sur Azure Cost Management

- [Documentation sur la facturation et Azure Cost Management](../index.yml)