---
title: Configurer des notifications par e-mail pour les alertes du Centre de sécurité Azure
description: Découvrez comment ajuster les types d’e-mails émis par le Centre de sécurité Azure pour les alertes de sécurité.
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: quickstart
ms.date: 04/07/2021
ms.author: memildin
ms.openlocfilehash: 4bdfdcc83a920b5b2e7317c4706c470e302cc49f
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "121731093"
---
# <a name="configure-email-notifications-for-security-alerts"></a>Configurer des notifications par e-mail pour les alertes de sécurité 

Les alertes de sécurité doivent toucher les bonnes personnes au sein de votre organisation. Par défaut, le Centre de sécurité envoie des e-mails aux propriétaires d’abonnements par courrier électronique chaque fois qu’une alerte de gravité élevée est déclenchée pour leur abonnement. Cette page explique comment personnaliser ces notifications.

Pour définir vos propres préférences en lien avec les e-mails de notification, la page des paramètres des **notifications par e-mail** du Centre de sécurité Azure vous permet de choisir :

- ***qui* doit être notifié** – vous pouvez envoyer des e-mails à des personnes sélectionnées ou à toute personne disposant d’un rôle Azure spécifié pour un abonnement. 
- ***ce qui* doit être notifié** – vous modifier les niveaux de gravité pour lesquels le Centre de sécurité doit envoyer des notifications.

Pour éviter la fatigue liée aux alertes, Security Center limite le volume des e-mails sortants. Pour chaque abonnement, Security Center envoie :

- Un maximum d’un e-mail toutes les **six heures** (quatre e-mails par jour) pour les alertes de **gravité élevée**.
- Un maximum d’un e-mail toutes les **12 heures** (deux e-mails par jour) pour les alertes de **gravité moyenne**.
- Un maximum d’un e-mail toutes les **24 heures** pour les alertes de **faible gravité**.

:::image type="content" source="./media/security-center-provide-security-contacts/email-notification-settings.png" alt-text="Configuration des détails du contact qui recevra les e-mails sur les alertes de sécurité." :::
 
## <a name="availability"></a>Disponibilité

|Aspect|Détails|
|----|:----|
|État de sortie :|Disponibilité générale|
|Prix :|Gratuit|
|Rôles et autorisations obligatoires :|**Administrateur de la sécurité**<br>**Propriétaire de l’abonnement** |
|Clouds :|:::image type="icon" source="./media/icons/yes-icon.png"::: Clouds commerciaux<br>:::image type="icon" source="./media/icons/yes-icon.png"::: National/Souverain (Azure Government, Azure China 21Vianet)|
|||


## <a name="customize-the-security-alerts-email-notifications-via-the-portal"></a>Personnaliser les notifications par e-mail des alertes de sécurité via le portail<a name="email"></a>
Vous pouvez envoyer des notifications par e-mail à des individus ou à tous les utilisateurs ayant des rôles Azure spécifiques.

1. Dans la zone **Tarification et paramètres** du Centre de sécurité, sélectionnez l’abonnement approprié, puis **Notifications par e-mail**.

1. Définissez les destinataires de vos notifications avec l’une des options suivantes ou les deux :

    - Dans la liste déroulante, sélectionnez parmi les rôles disponibles.
    - Entrez les adresses e-mail spécifiques séparées par des virgules. Vous pouvez entrer autant d’adresses e-mail que vous le voulez.

1. Pour appliquer les coordonnées de sécurité à votre abonnement, sélectionnez **Enregistrer**.

## <a name="customize-the-alerts-email-notifications-through-the-api"></a>Personnaliser les notifications par e-mail des alertes via l’API
Vous pouvez également gérer vos notifications par e-mail via l’API REST fournie. Pour plus d’informations, consultez la [documentation de l’API SecurityContacts](/rest/api/securitycenter/securitycontacts).

Voici un exemple de corps de requête pour la requête PUT lors de la création d’une configuration de contact de sécurité :

URI : https://management.azure.com/subscriptions/<SubscriptionId> /providers/Microsoft.Security/securityContacts/default?api-version=2020-01-01-preview

```json
{
    "properties": {
        "emails": admin@contoso.com;admin2@contoso.com,
        "notificationsByRole": {
            "state": "On",
            "roles": ["AccountAdmin", "Owner"]
        },
        "alertNotifications": {
            "state": "On",
            "minimalSeverity": "Medium"
        },
        "phone": ""
    }
}
```


## <a name="see-also"></a>Voir aussi
Pour plus d’informations sur les alertes de sécurité, consultez les pages suivantes :

- [Alertes de sécurité – Guide de référence](alerts-reference.md) : apprenez-en davantage sur les alertes de sécurité possibles du module Protection contre les menaces d’Azure Security Center.
- [Gérer et résoudre les alertes de sécurité dans Azure Security Center](security-center-managing-and-responding-alerts.md) : découvrez comment gérer et résoudre les alertes de sécurité.
- [Automatisation de workflow](workflow-automation.md) : automatisez les réponses aux alertes avec une logique de notification personnalisée.