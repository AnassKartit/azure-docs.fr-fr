---
title: Problèmes connus liés au provisionnement d’applications dans Azure Active Directory
description: Prenez connaissance des problèmes connus pouvant survenir lorsque vous utilisez le provisionnement automatique d’applications dans Azure Active Directory.
author: kenwith
ms.author: kenwith
manager: mtillman
services: active-directory
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/07/2021
ms.reviewer: arvinh
ms.openlocfilehash: 1180852bdaa88f1c717ab6a44d85d84412c5b155
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/22/2021
ms.locfileid: "114449604"
---
# <a name="known-issues-for-application-provisioning-in-azure-active-directory"></a>Problèmes connus liés au provisionnement d’applications dans Azure Active Directory
Cet article décrit les problèmes connus à prendre en compte lorsque vous utilisez l’approvisionnement d’applications. Pour formuler des commentaires sur le service d’approvisionnement d’applications sur UserVoice, voir [UserVoice pour l’approvisionnement d’applications Azure Active Directory (Azure AD)](https://aka.ms/appprovisioningfeaturerequest). Nous surveillons le service UserVoice de près pour améliorer le service.

> [!NOTE]
> Cet article ne constitue pas une liste exhaustive des problèmes connus. Si vous découvrez un problème qui ne figure pas dans la liste, partagez vos commentaires en bas de la page.

## <a name="authorization"></a>Autorisation 

#### <a name="unable-to-save"></a>Impossible d’enregistrer

L’URL du locataire, le jeton secret et l’e-mail de notification doivent être renseignés pour être enregistrés. Vous ne pouvez pas uniquement fournir une seule de ces données. 

#### <a name="unable-to-change-provisioning-mode-back-to-manual"></a>Impossible de rétablir le mode d’approvisionnement manuel

Une fois que vous avez configuré l’approvisionnement pour la première fois, vous remarquerez que le mode de configuration est passé de manuel à automatique. Vous ne pouvez pas le repasser sur manuel. Toutefois, vous pouvez désactiver l’approvisionnement par le biais de l’interface utilisateur. La désactivation de l’approvisionnement dans l’interface utilisateur a le même effet la définition de la liste déroulante sur le mode manuel.

## <a name="attribute-mappings"></a>Mappages d’attributs 

#### <a name="attribute-samaccountname-or-usertype-not-available-as-a-source-attribute"></a>L’attribut SamAccountName ou userType n’est pas disponible en tant qu’attribut source

Les attributs **SamAccountName** et **userType** ne sont pas disponibles par défaut en tant qu’attributs sources. Développez votre schéma pour ajouter les attributs. Vous pouvez ajouter les attributs à la liste des attributs sources disponibles en développant votre schéma. Pour plus d’informations, consultez [Attribut source manquant](user-provisioning-sync-attributes-for-mapping.md). 

#### <a name="source-attribute-dropdown-missing-for-schema-extension"></a>Liste déroulante d’attributs sources manquante pour l’extension de schéma

Les extensions de votre schéma peuvent parfois être absentes dans la liste déroulante d’attributs sources de l’interface utilisateur. Accédez aux paramètres avancés de vos mappages d’attributs et ajoutez manuellement les attributs. Pour plus d’informations, consultez [Personnalisation des mappages d’attributs](customize-application-attributes.md).

#### <a name="null-attribute-cant-be-provisioned"></a>L’attribut null ne peut pas être approvisionné

Azure AD ne peut actuellement pas approvisionner des attributs null. Si un attribut est null sur l’objet utilisateur, il sera ignoré. 

#### <a name="maximum-characters-for-attribute-mapping-expressions"></a>Nombre maximal de caractères pour les expressions de mappage d’attributs

Les expressions de mappage d’attributs peuvent avoir un maximum de 10 000 caractères. 

#### <a name="unsupported-scoping-filters"></a>Filtres d’étendue non pris en charge

Les extensions de répertoire et les attributs **appRoleAssignments**, **userType** et **accountExpires** ne sont pas prises en charge en tant que filtres d’étendue.

#### <a name="multivalue-directory-extensions"></a>Extensions de répertoire à valeurs multiples

Les extensions de répertoire à valeurs multiples ne peuvent pas être utilisées dans les mappages d’attributs ni les filtres d’étendue. 

## <a name="service-issues"></a>Problèmes liés aux services 

#### <a name="unsupported-scenarios"></a>Scénarios non pris en charge

- L’approvisionnement de mots de passe n’est pas pris en charge. 
- L’approvisionnement de groupes imbriqués n’est pas pris en charge. 
- L’approvisionnement de locataires B2C n’est pas pris en charge en raison de la taille des locataires.
- Toutes les applications d’approvisionnement ne sont pas disponibles dans tous les clouds. Par exemple, Atlassian n’est pas encore disponible dans le cloud Government. Nous travaillons en collaboration avec les développeurs d’applications pour intégrer leurs applications à tous les clouds.

#### <a name="automatic-provisioning-isnt-available-on-my-oidc-based-application"></a>Le provisionnement automatique n’est pas disponible sur mon application OIDC

Si vous créez une inscription d’application, le principal de service correspondant dans les applications d’entreprise n’est pas activé pour le provisionnement automatique d’utilisateurs. Vous devez soit demander l’ajout de l’application à la Galerie, si elle est destinée à être utilisée par plusieurs organisations, soit créer une deuxième application hors Galerie pour le provisionnement.

#### <a name="the-provisioning-interval-is-fixed"></a>L’intervalle de provisionnement est fixe

Le [temps](./application-provisioning-when-will-provisioning-finish-specific-user.md#how-long-will-it-take-to-provision-users) entre les cycles de provisionnement n’est actuellement pas configurable. 

#### <a name="changes-not-moving-from-target-app-to-azure-ad"></a>Les modifications ne se déplacent pas de l’application cible vers Azure AD

Le service d’approvisionnement d’application n’a pas connaissance des modifications apportées aux applications externes. Par conséquent, aucune action n’est effectuée pour la restauration. Le service d’approvisionnement d’application s’appuie sur les modifications apportées à Azure AD. 

#### <a name="switching-from-sync-all-to-sync-assigned-not-working"></a>Le basculement de la synchronisation de tous les utilisateurs à la synchronisation des utilisateurs affectés ne fonctionne pas

Après avoir remplacé l’étendue **Synchroniser tous les utilisateurs** par **Synchroniser les utilisateurs affectés**, veillez à effectuer un redémarrage pour que le changement prenne effet. Vous pouvez effectuer le redémarrage à partir de l’interface utilisateur.

#### <a name="provisioning-cycle-continues-until-completion"></a>Le cycle d’approvisionnement continue jusqu’à la fin

Lorsque vous définissez l’approvisionnement sur `enabled = off` ou sélectionnez **Arrêter**, le cycle d’approvisionnement actuel continue à s’exécuter jusqu’à la fin. Le service arrête l’exécution des cycles futurs jusqu’à ce que vous réactiviez l’approvisionnement.

#### <a name="member-of-group-not-provisioned"></a>Membre du groupe non approvisionné

Lorsqu’un groupe est dans l’étendue et qu’un membre est hors de cette étendue, le groupe est approvisionné. L’utilisateur hors de l’étendue n’est pas approvisionné. Si le membre revient dans l’étendue, le service ne détecte pas immédiatement la modification. Le redémarrage de l’approvisionnement permet de résoudre le problème. Redémarrez régulièrement le service pour vous assurer que tous les utilisateurs sont correctement approvisionnés.

#### <a name="manager-isnt-provisioned"></a>Le gestionnaire n’est pas approvisionné

Si un utilisateur et son gestionnaire se trouvent tous deux dans l’étendue d’approvisionnement, le service approvisionne l’utilisateur, puis met à jour le gestionnaire. Si, au premier jour, l’utilisateur se trouve dans l’étendue mais pas le gestionnaire, nous approvisionnons l’utilisateur sans référence au gestionnaire. Une fois le gestionnaire dans l’étendue, la référence au gestionnaire n’est pas mise à jour tant que vous ne redémarrez pas l’approvisionnement pour permettre au service de réévaluer tous les utilisateurs. 

## <a name="on-premises-application-provisioning"></a>Approvisionnement d’application local
Les informations suivantes constituent la liste actuelle des limitations connues avec l’hôte de connecteur Azure AD ECMA et l’approvisionnement d’application local.

### <a name="application-and-directories"></a>Applications et annuaires
Les applications et annuaires suivants ne sont pas encore pris en charge.

#### <a name="active-directory-domain-services-user-or-group-writeback-from-azure-ad-by-using-the-on-premises-provisioning-preview"></a>Active Directory Domain Services (écriture différée d’utilisateur ou de groupe à partir d’Azure AD à l’aide de la version préliminaire de l’approvisionnement local)
   - Quand un utilisateur est managé par Azure AD Connect, la source d’autorité est l’Azure AD local. Il n’est donc pas possible de modifier les attributs de l’utilisateur dans Azure AD. Cette préversion ne modifie pas la source d’autorité pour les utilisateurs managés par Azure AD Connect.
   - Toute tentative d’utilisation d’Azure AD Connect et de l’approvisionnement local pour approvisionner des groupes ou des utilisateurs dans Active Directory Domain Services peut entraîner la création d’une boucle, où Azure AD Connect peut remplacer une modification apportée par le service d’approvisionnement dans le cloud. Microsoft travaille sur une fonctionnalité dédiée à l’écriture différée de groupe ou d’utilisateur. Votez pour les commentaires UserVoice sur [ce site web](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/16887037-enable-user-writeback-to-on-premise-ad-from-azure) afin de suivre l’état de la préversion. Vous pouvez également utiliser [Microsoft Identity Manager](/microsoft-identity-manager/microsoft-identity-manager-2016) pour l’écriture différée d’utilisateur ou de groupe d’Azure AD à Active Directory.

#### <a name="connectors-other-than-sql"></a>Connecteurs autres que SQL

   L’hôte de connecteur Azure AD ECMA est officiellement pris en charge pour le connecteur SQL générique. Bien qu’il soit possible d’utiliser d’autres connecteurs, tels que le connecteur de services web ou des connecteurs ECMA personnalisés, cela n’est *pas encore pris en charge*.

#### <a name="azure-ad"></a>Azure AD

   En utilisant l’approvisionnement local, vous pouvez approvisionner un utilisateur se trouvant déjà dans Azure AD dans une application tierce. *Vous ne pouvez pas introduire un utilisateur dans l’annuaire à partir d’une application tierce.* Les clients doivent s’appuyer sur nos intégrations RH natives, Azure AD Connect, Microsoft Identity Manager ou Microsoft Graph pour introduire des utilisateurs dans l’annuaire.

### <a name="attributes-and-objects"></a>Attributs et objets 
Les attributs et objets suivants ne sont pas pris en charge :
   - Attributs à valeurs multiples.
   - Attributs de référence (par exemple, manager).
   - Groupes.
   - Ancres complexes (par exemple, ObjectTypeName+UserName).
   - Certaines applications locales ne sont pas fédérées avec Azure AD et exigent des mots de passe locaux. La préversion de l’approvisionnement local *ne prend pas en charge l’approvisionnement de mots de passe à usage unique ou la synchronisation de mots de passe* entre Azure AD et des applications tierces.
   - L’attribut virtuel **export_password**, les opérations **SetPassword** et **ChangePassword** ne sont pas pris en charge.

#### <a name="ssl-certificates"></a>Certificats SSL
   L’hôte de connecteur Azure AD ECMA requiert actuellement l’utilisation d’un certificat SSL approuvé par Azure ou par l’agent d’approvisionnement. L’objet du certificat doit correspondre au nom d’hôte sur lequel l’hôte de connecteur Azure AD ECMA est installé.

#### <a name="anchor-attributes"></a>Attributs d’ancre
   L’hôte de connecteur Azure AD ECMA ne prend actuellement pas en charge les modifications d’attribut d’ancre (renommages) ou les systèmes cibles qui requièrent plusieurs attributs pour former une ancre. 

#### <a name="attribute-discovery-and-mapping"></a>Détection et mappage des attributs
   Les attributs pris en charge par l’application cible sont découverts et exposés sur le portail Azure dans **Mappages d’attributs**. La découverte des attributs nouvellement ajoutés continuera. Si un type d’attribut a changé (par exemple, de chaîne en booléen) et qu’il fait partie des mappages, le type ne change pas automatiquement dans le portail Azure. Les clients doivent alors accéder aux paramètres avancés des mappages pour mettre à jour le type d’attribut manuellement.

## <a name="next-steps"></a>Étapes suivantes
[Comment fonctionne le provisionnement](how-provisioning-works.md)
