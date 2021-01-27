---
title: Documentation sur les rôles Administrateur dans les services Microsoft 365 – Azure AD | Microsoft Docs
description: Rechercher du contenu et des références d’API pour des rôles d’administrateur de services Microsoft 365 dans Azure Active Directory
services: active-directory
documentationcenter: ''
author: rolyon
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: reference
ms.date: 11/05/2020
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5579e0d2094c3dc596e8e50992ae6bc31818ff87
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98743064"
---
# <a name="roles-for-microsoft-365-services-in-azure-active-directory"></a>Rôles pour les services Microsoft 365 dans Azure Active Directory

Tous les produits de Microsoft 365 peuvent être gérés avec des rôles d’administration dans Azure Active Directory (Azure AD). Certains produits fournissent également des rôles supplémentaires qui sont spécifiques de ce produit. Pour plus d’informations sur les rôles pris en charge par chaque produit, consultez le tableau ci-dessous. Vous trouverez des discussions générales de problèmes de délégation dans [Planification de la délégation de rôle dans Azure Active Directory](concept-delegation.md).

## <a name="where-to-find-content"></a>Où trouver du contenu

Service Microsoft 365 | Contenu de rôle | Contenu d’API
---------------------- | ------------------ | -----------------
Rôles d’administrateur dans les plans commerciaux Office 365 et Microsoft 365 | [Rôles d’administrateur Microsoft 365](/office365/admin/add-users/about-admin-roles?view=o365-worldwide&preserve-view=true) | Non disponible
Azure Active Directory (Azure AD) et Azure AD Identity Protection| [Rôles d’administrateur Azure AD](permissions-reference.md) | [API Graph](/graph/api/overview?view=graph-rest-1.0&preserve-view=true&preserve-view=true)<br>[Récupérer des attributions de rôles](/graph/api/directoryrole-list?view=graph-rest-1.0&preserve-view=true)
Exchange Online| [Contrôle d’accès en fonction du rôle Exchange](/exchange/understanding-role-based-access-control-exchange-2013-help) |  [PowerShell pour Exchange](/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps&preserve-view=true)<br>[Récupérer des attributions de rôles](/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps&preserve-view=true)
SharePoint Online | [Rôles d’administrateur Azure AD](permissions-reference.md)<br>Également [À propos du rôle d’administrateur SharePoint dans Microsoft 365](/sharepoint/sharepoint-admin-role) | [API Graph](/graph/api/overview?view=graph-rest-1.0&preserve-view=true)<br>[Récupérer des attributions de rôles](/graph/api/directoryrole-list?view=graph-rest-1.0&preserve-view=true)
Teams/Skype Entreprise | [Rôles d’administrateur Azure AD](permissions-reference.md) | [API Graph](/graph/api/overview?view=graph-rest-1.0&preserve-view=true)<br>[Récupérer des attributions de rôles](/graph/api/directoryrole-list?view=graph-rest-1.0&preserve-view=true)
Centre de sécurité et de conformité (Protection avancée contre les menaces Office 365, Exchange Online Protection, Information Protection) | [Rôles d’administration Office 365](/office365/SecurityCompliance/permissions-in-the-security-and-compliance-center) | [Exchange PowerShell](/powershell/module/exchange/role-based-access-control/add-managementroleentry?view=exchange-ps&preserve-view=true)<br>[Récupérer des attributions de rôles](/powershell/module/exchange/role-based-access-control/get-rolegroup?view=exchange-ps&preserve-view=true)
Degré de sécurisation | [Rôles d’administrateur Azure AD](permissions-reference.md) | [API Graph](/graph/api/overview?view=graph-rest-1.0&preserve-view=true)<br>[Récupérer des attributions de rôles](/graph/api/directoryrole-list?view=graph-rest-1.0&preserve-view=true)
Compliance Manager | [Rôles Compliance Manager](/office365/securitycompliance/meet-data-protection-and-regulatory-reqs-using-microsoft-cloud#permissions-and-role-based-access-control) | Non disponible
Azure Information Protection | [Rôles d’administrateur Azure AD](permissions-reference.md) | [API Graph](/graph/api/overview?view=graph-rest-1.0&preserve-view=true)<br>[Récupérer des attributions de rôles](/graph/api/directoryrole-list?view=graph-rest-1.0&preserve-view=true)
Microsoft Cloud App Security | [Contrôle d’accès en fonction du rôle](/cloud-app-security/manage-admins) | [Informations de référence sur l'API](/cloud-app-security/api-tokens) 
Azure Advanced Threat Protection | [Groupes de rôles Azure ATP](/azure-advanced-threat-protection/atp-role-groups) | Non disponible
Protection avancée contre les menaces Windows Defender | [Contrôle d’accès en fonction du rôle Windows Defender ATP](/windows/security/threat-protection/windows-defender-atp/rbac-windows-defender-advanced-threat-protection) | Non disponible
Privileged Identity Management | [Rôles d’administrateur Azure AD](permissions-reference.md) | [API Graph](/graph/api/overview?view=graph-rest-1.0&preserve-view=true)<br>[Récupérer des attributions de rôles](/graph/api/directoryrole-list?view=graph-rest-1.0&preserve-view=true)
Intune | [Contrôle d’accès en fonction du rôle Intune](/intune/role-based-access-control) | [API Graph](/graph/api/resources/intune-rbac-conceptual?view=graph-rest-beta&preserve-view=true)<br>[Récupérer des attributions de rôles](/graph/api/intune-rbac-roledefinition-list?view=graph-rest-beta&preserve-view=true)
Ordinateur de bureau managé | [Rôles d’administrateur Azure AD](permissions-reference.md) | [API Graph](/graph/api/overview?view=graph-rest-1.0&preserve-view=true)<br>[Récupérer des attributions de rôles](/graph/api/directoryrole-list?view=graph-rest-1.0&preserve-view=true)

## <a name="next-steps"></a>Étapes suivantes

* [Guide pratique pour attribuer ou supprimer des rôles d’administrateur Azure AD](manage-roles-portal.md)
* [Informations de référence sur les rôles d’administrateur Azure AD](permissions-reference.md)