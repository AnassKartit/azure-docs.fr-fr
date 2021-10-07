---
title: Guide pratique pour la gestion des appareils dans le portail Azure | Microsoft Docs
description: Découvrez comment utiliser le portail Azure pour gérer les appareils.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: how-to
ms.date: 03/23/2021
ms.author: joflore
author: MicrosoftGuyJFlo
manager: karenhoran
ms.reviewer: hafowler
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8ef9dca3604be281ccedf4d6f1af3e7861273e6b
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128590719"
---
# <a name="manage-device-identities-using-the-azure-portal"></a>Gérer les identités de l’appareil à l’aide du portail Microsoft Azure

Le portail Azure AD offre un emplacement central pour la gestion des identités d’appareils.

La page **Tous les appareils** vous permet d’effectuer les opérations suivantes :

- Identifier les appareils, à savoir :
   - appareils qui ont été joints ou inscrits dans Azure AD ;
   - appareils déployés à l’aide de [Windows AutoPilot](/windows/deployment/windows-autopilot/windows-autopilot) ;
   - imprimantes utilisant l’[Impression universelle](/universal-print/fundamentals/universal-print-getting-started).
- Effectuer des tâches de gestion des identités d’appareils, telles que l’activation, la désactivation, la suppression ou la gestion.
   - Les [imprimantes](/universal-print/fundamentals/) et les appareils [Windows AutoPilot](/windows/deployment/windows-autopilot/windows-autopilot) ont des options de gestion limitées dans Azure AD. Elles doivent être gérés à partir de leurs interfaces d’administration respectives.
- Configurer les paramètres d’identité de votre appareil.
- Activer ou désactiver Enterprise State Roaming.
- Examiner les journaux d’audit liés à l’appareil.
- Télécharger les appareils (préversion)

[![Vue Tous les appareils dans le portail Azure](./media/device-management-azure-portal/all-devices-azure-portal.png)](./media/device-management-azure-portal/all-devices-azure-portal.png#lightbox)

Vous pouvez accéder au portail des appareils en procédant comme suit :

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Accédez à **Azure Active Directory** > **Appareils**.

## <a name="manage-devices"></a>Gérer des appareils

Il existe deux emplacements pour gérer les appareils dans Azure AD :

- **Portail Azure** > **Azure Active Directory** > **Appareils**
- **Portail Azure** > **Azure Active Directory** > **Utilisateurs** > Sélectionner un utilisateur > **Appareils**

Les deux options permettent aux administrateurs d’effectuer les opérations suivantes :

- Rechercher des appareils.
- Consultez les détails d’appareil, notamment :
    - Nom du périphérique
    - ID de périphérique
    - Système d’exploitation et version
    - Type de jointure
    - Propriétaire
    - Gestion et conformité d’appareil mobile
    - Clé de récupération BitLocker
- Effectuer des tâches de gestion d’identité d’appareil telles que activer, désactiver, supprimer ou gérer.
   - Les [imprimantes](/universal-print/fundamentals/) et les appareils [Windows AutoPilot](/windows/deployment/windows-autopilot/windows-autopilot) ont des options de gestion limitées dans Azure AD. Elles doivent être gérés à partir de leurs interfaces d’administration respectives.

> [!TIP]
> - Les appareils Windows 10 hybrides joints à Azure AD n’ont pas de propriétaire. Si vous cherchiez un appareil à l’aide de son propriétaire et ne l’avez pas trouvé, recherchez-le à l’aide de son ID.
>
> - Si vous voyez un appareil « joint à une version hybride d’Azure AD » avec l’état « En attente » dans la colonne REGISTERED (Inscrit), cela signifie que l’appareil a été synchronisé à partir d’Azure AD Connect et qu’il attend de terminer l’inscription à partir du client. Pour en savoir plus, consultez [Planifier votre implémentation de la jointure d’Azure AD Hybride](hybrid-azuread-join-plan.md). Pour plus d’informations, consultez le [Forum aux questions sur les appareils](faq.yml).
>
> - Pour certains appareils iOS, les noms d’appareils qui contiennent des apostrophes peuvent utiliser d’autres caractères qui ressemblent à des apostrophes. La recherche de ces appareils peut donc être difficile. Si les résultats que vous attendez ne s’affichent pas, vérifiez que la chaîne de recherche contient les caractères d’apostrophe attendus.

### <a name="manage-an-intune-device"></a>Gérer un appareil Intune

Si vous êtes administrateur de service Intune, vous pouvez gérer les appareils où la GPM est marquée **Microsoft Intune**. Si l’appareil n’est pas inscrit auprès de Microsoft Intune, l’option « Gérer » est grisée.

### <a name="enable-or-disable-an-azure-ad-device"></a>Activer ou désactiver un appareil Azure AD

Pour activer ou désactiver des appareils, vous avez le choix entre deux options :

- La barre d’outils sur la page **Tous les appareils** après avoir sélectionné un ou plusieurs appareils.
- La barre d’outils après l’exploration d’un appareil spécifique.

> [!IMPORTANT]
> - Vous devez être administrateur général, administrateur Intune ou administrateur d’appareil cloud dans Azure AD pour activer ou désactiver un appareil. 
> - La désactivation d’un périphérique empêche celui-ci de s’authentifier à Azure AD et, par conséquent, d’accéder à vos ressources Azure AD qui sont protégées par un accès conditionnel basé sur les appareils ou l’utilisation des informations d’identification Windows Hello Entreprise.
> - La désactivation d’un appareil entraîne la révocation du jeton d’actualisation principal (PRT) et des éventuels jetons d’actualisation (RT) sur l’appareil.
> - Les imprimantes ne peuvent pas être activées ou désactivées dans Azure AD.

### <a name="delete-an-azure-ad-device"></a>Supprimer un appareil Azure AD

Pour supprimer un appareil, vous avez deux options :

- La barre d’outils sur la page **Tous les appareils** après avoir sélectionné un ou plusieurs appareils.
- La barre d’outils après l’exploration d’un appareil spécifique.

> [!IMPORTANT]
> - Pour supprimer un appareil, vous devez avoir le rôle administrateur d’appareils cloud, administrateur de service Intune ou administrateur général dans Azure AD.
> - Les imprimantes et les appareils Windows AutoPilot ne peuvent pas être supprimés dans Azure AD
> - La suppression d’un appareil :
>    - Empêche celui-ci d’accéder à vos ressources Azure AD.
>    - Supprime toutes les informations associées à l’appareil, par exemple, les clés BitLocker des appareils Windows.  
>    - Est une action irréversible et donc non recommandée, sauf si elle est absolument nécessaire.

Si un appareil est géré par une autre autorité de gestion (par exemple, Microsoft Intune), vérifiez que l’appareil a été réinitialisé ou mis hors service avant de le supprimer d’Azure AD. Lisez comment [gérer des appareils obsolètes](manage-stale-devices.md) avant de supprimer des appareils.

### <a name="view-or-copy-device-id"></a>Afficher ou copier l’ID de l’appareil

Vous pouvez utiliser un ID d’appareil pour vérifier les informations d’ID de l’appareil ou utiliser PowerShell durant le dépannage. Pour accéder à l’option de copie, cliquez sur l’appareil.

![Afficher un ID d’appareil](./media/device-management-azure-portal/35.png)
  
### <a name="view-or-copy-bitlocker-keys"></a>Afficher ou copier des clés BitLocker

Vous pouvez afficher et copier les clés BitLocker pour permettre aux utilisateurs de récupérer leurs lecteurs chiffrés. Ces clés sont uniquement disponibles pour les appareils Windows chiffrés dont les clés sont stockées dans Azure AD. Vous pouvez trouver ces clés lors de l’accès aux détails d’un appareil en sélectionnant **Afficher la clé de récupération**. La sélection d’**Afficher la clé de récupération** génère un journal d’audit que vous pouvez trouver dans la catégorie `KeyManagement`.

![Afficher les clés BitLocker](./media/device-management-azure-portal/device-details-show-bitlocker-key.png)

Pour afficher ou copier les clés BitLocker, vous devez être le propriétaire de l’appareil ou un utilisateur possédant au moins l’un des rôles suivants attribués :

- Administrateur d’appareil cloud
- Administrateur général
- Administrateur du support technique
- Administrateur de services Intune
- Security Administrator
- Lecteur de sécurité

### <a name="device-list-filtering-preview"></a>Filtrage de la liste des appareils (préversion)

Auparavant, vous pouviez uniquement filtrer la liste des appareils par activité et état activé. Cette préversion vous permet désormais de filtrer la liste des appareils en fonction des attributs suivants sur un appareil :

- État activé
- État conforme
- Type de jonction (jonction Azure AD, jonction Azure AD Hybride, inscrit sur Azure AD)
- Timestamp d’activité
- Système d’exploitation
- Type d’appareil (imprimantes, machines virtuelles sécurisées, appareils partagés, appareils inscrits)

Pour activer la fonctionnalité de filtrage en préversion dans la vue **Tous les appareils** :

![Activer la fonctionnalité de filtrage en préversion](./media/device-management-azure-portal/device-filter-preview-enable.png)

1. Connectez-vous au [portail Azure](https://portal.azure.com).
1. Accédez à **Azure Active Directory** > **Appareils**.
1. Sélectionnez la bannière qui indique : **Essayez les améliorations apportées au filtrage des appareils. Cliquez pour activer la préversion.**

Vous avez maintenant la possibilité d’**Ajouter des filtres** à votre vue **Tous les appareils**.

### <a name="download-devices-preview"></a>Télécharger les appareils (préversion)

Les administrateurs d’appareils cloud, les administrateurs Intune et les administrateurs généraux peuvent utiliser l’option **Télécharger les appareils (préversion)**  pour exporter un fichier CSV d’appareils en fonction des filtres appliqués. Si aucun filtre n’est appliqué à la liste, tous les appareils sont exportés. Une exportation peut s’exécuter pendant une période allant jusqu’à une heure 

La liste exportée comprend les attributs d’identité d’appareil suivants :

`accountEnabled, approximateLastLogonTimeStamp, deviceOSType, deviceOSVersion, deviceTrustType, dirSyncEnabled, displayName, isCompliant, isManaged, lastDirSyncTime, objectId, profileType, registeredOwners, systemLabels, registrationTime, mdmDisplayName`

## <a name="configure-device-settings"></a>Configurer les paramètres de l’appareil

Pour que vous puissiez gérer les identités d’appareils avec le portail Azure AD, ces appareils doivent être [inscrits ou joints](overview.md) à Azure AD. En tant qu’administrateur, vous pouvez contrôler le processus d’inscription et de jonction d’appareils en configurant les paramètres d’appareil suivants.

Pour afficher ou gérer les paramètres d’un appareil dans le portail Azure, vous devez être titulaire de l’un des rôles suivants :

- Administrateur général
- Administrateur d’appareil cloud
- Lecteur général
- Lecteur d’annuaire

![Paramètres d’appareil associés à Azure AD](./media/device-management-azure-portal/device-settings-azure-portal.png)

- **Les utilisateurs peuvent joindre des appareils à Azure AD** : ce paramètre vous permet de sélectionner les utilisateurs qui peuvent inscrire leurs appareils en tant qu’appareils joints à Azure AD. La valeur par défaut est **Tous**.

> [!NOTE]
> Le paramètre **Les utilisateurs peuvent joindre des appareils à Azure AD** s’applique uniquement à Azure AD Join sous Windows 10. Il ne s’applique ni aux appareils à jointure hybride Azure AD, ni aux [machines virtuelles à jointure Azure AD dans Azure](./howto-vm-sign-in-azure-ad-windows.md#enabling-azure-ad-login-in-for-windows-vm-in-azure), ni aux appareils à jointure Azure AD suivant le [mode de déploiement automatique Windows Autopilot](/mem/autopilot/self-deploying), car ces méthodes fonctionnent dans un contexte sans utilisateur.

- **Administrateurs locaux supplémentaires sur les appareils joints à Azure AD** : vous pouvez sélectionner les utilisateurs qui peuvent disposer de droits d’administrateur local sur un appareil. Ces utilisateurs sont ajoutés au rôle *Administrateurs d’appareils* dans Azure AD. Les administrateurs généraux Azure AD et les propriétaires d’appareils bénéficient de droits d’administrateur local par défaut. Cette option est une fonctionnalité de l’édition Premium disponible dans les produits comme Azure AD Premium ou EMS (Enterprise Mobility Suite).
- **Les utilisateurs peuvent inscrire leurs appareils sur Azure AD** : vous devez configurer ce paramètre pour permettre l’inscription des appareils Windows 10 Personnel, iOS, Android et macOS dans Azure AD. Si vous sélectionnez **Aucun**, les appareils ne peuvent pas être inscrits dans Azure AD. L’inscription auprès de Microsoft Intune ou de la Gestion des appareils mobiles (MDM) pour Microsoft 365 nécessite l’enregistrement de l’appareil. Si vous avez configuré l’un de ces services, l’option **TOUS** est sélectionnée et l’option **AUCUN** est désactivée.
- **Exiger une authentification multifacteur pour inscrire ou joindre des appareils avec Azure AD** : vous pouvez déterminer si les utilisateurs doivent indiquer un facteur d’authentification supplémentaire pour joindre ou inscrire leur appareil à Azure AD. La valeur par défaut est **No**. Il est recommandé d’exiger l’authentification multifacteur au moment de l’inscription ou de la jointure d’un appareil. Avant d’activer l’authentification multifacteur pour ce service, vous devez vérifier que l’authentification multifacteur est configurée pour les utilisateurs qui inscrivent leurs appareils. Pour plus d’informations sur les différents services Azure AD Multi-Factor Authentication, consultez [Bien démarrer avec Azure AD Multi-Factor Authentication](../authentication/concept-mfa-howitworks.md). 

> [!NOTE]
> Le paramètre **Exiger une authentification multifacteur pour inscrire ou joindre des appareils avec Azure AD** s’applique aux appareils joints à Azure AD (sauf exception) et aux appareils inscrits auprès d’Azure AD. Il ne s’applique ni aux appareils à jointure hybride Azure AD, ni aux [machines virtuelles à jointure Azure AD dans Azure](./howto-vm-sign-in-azure-ad-windows.md#enabling-azure-ad-login-in-for-windows-vm-in-azure), ni aux appareils à jointure Azure AD suivant le [mode de déploiement automatique Windows Autopilot](/mem/autopilot/self-deploying).

> [!IMPORTANT]
> - Nous vous recommandons d’utiliser l’action de l’utilisateur [« Inscrire ou joindre des appareils »](../conditional-access/concept-conditional-access-cloud-apps.md#user-actions) dans l’accès conditionnel pour appliquer l’authentification multifacteur pour la jonction ou l’inscription d’un appareil. 
> - Vous devez définir ce paramètre sur **Non** si vous utilisez la stratégie d’accès conditionnel pour exiger l’authentification multifacteur. 

- **Nombre maximal d’appareils par utilisateur** : ce paramètre vous permet de sélectionner le nombre maximal d’appareils joints à Azure AD ou inscrits à Azure AD et qu’un utilisateur peut avoir dans Azure AD. Si un utilisateur atteint ce quota, il ne pourra pas ajouter d’autres appareils tant qu’un ou plusieurs appareils existants n’auront pas été supprimés. La valeur par défaut est de **50**. Vous pouvez augmenter la valeur jusqu’à 100 et, si vous entrez une valeur supérieure à 100, Azure AD la définit sur 100. Vous pouvez également utiliser une valeur illimitée pour appliquer une limite autre que les limites de quotas existantes.

> [!NOTE]
> Le paramètre **Nombre maximal d’appareils par utilisateur** s’applique aux appareils joints à Azure AD ou inscrits à Azure AD. Ce paramètre ne s’applique pas aux appareils hybrides joints à Azure AD.

- [Enterprise State Roaming](enterprise-state-roaming-overview.md)

## <a name="audit-logs"></a>Journaux d’audit

Des activités de l’appareil sont disponibles dans les journaux d’activité. Ces journaux comprennent les activités déclenchées par le service d’inscription des appareils et par les utilisateurs :

- Création d’un appareil et ajout de propriétaires/d’utilisateurs sur l’appareil
- Modification des paramètres de l’appareil
- Opérations concernant les appareils, telles que la suppression ou la mise à jour d’un appareil

Les **Journaux d’audit** dans la section **Activité** de la page **Appareils** constituent le point d’entrée des données d’audit.

Un journal d’audit inclut une vue Liste par défaut, qui indique les informations suivantes :

- La date et l’heure de l’occurrence
- Les cibles
- L’initiateur/intervenant d’une activité (qui)
- L’activité (quoi)

:::image type="content" source="./media/device-management-azure-portal/63.png" alt-text="Capture d’écran d’un tableau dans la section Activité de la page Appareils qui répertorie la date, la cible, l’acteur et l’activité pour quatre journaux d’audit." border="false":::

Vous pouvez personnaliser le mode Liste en cliquant sur **Colonnes** dans la barre d’outils.

:::image type="content" source="./media/device-management-azure-portal/64.png" alt-text="Capture d’écran montrant la barre d’outils de la page Appareils. L’élément Colonnes est mis en évidence." border="false":::

Pour limiter les données transmises à un niveau qui vous convient, vous pouvez filtrer les données d’audit à l’aide des champs suivants :

- Category
- Type de ressource d’activité
- Activité
- Plage de dates
- Cible
- Initié par (intervenant)

Vous pouvez filtrer les entrées, mais également rechercher des entrées spécifiques.

:::image type="content" source="./media/device-management-azure-portal/65.png" alt-text="Capture d’écran des contrôles de filtre de données d’audit, avec les champs Catégorie, Type de ressource d’activité, Activité, Plage de dates, Cible et Acteur et un champ de recherche." border="false":::

## <a name="next-steps"></a>Étapes suivantes

[Guide pratique pour gérer les appareils obsolètes dans Azure AD](manage-stale-devices.md)

[Enterprise State Roaming](enterprise-state-roaming-overview.md)
