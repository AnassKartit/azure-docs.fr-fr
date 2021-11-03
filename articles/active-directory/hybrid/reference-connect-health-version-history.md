---
title: Historique de publication des versions d’Azure AD Connect Health
description: Ce document décrit les versions d’Azure AD Connect Health et ce qui a été inclus dans ces versions.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: daveba
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.subservice: hybrid
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 08/10/2020
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: dc0495e4ddd3e266b375a9d6a137497f457803da
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131049251"
---
# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect Health : Historique de publication des versions
L’équipe Azure Active Directory met régulièrement à jour Azure AD Connect Health avec de nouvelles fonctions et fonctionnalités. Cet article répertorie les versions et les fonctionnalités qui ont été publiées.  

> [!NOTE]
> Les agents Connect Health sont mis à jour automatiquement lorsqu’une nouvelle version est publiée. Vérifiez que les paramètres de mise à niveau automatique sont activés à partir du portail Azure.
>

Azure AD Connect Health pour la synchronisation est intégré à l’installation d’Azure AD Connect. Vous trouverez ici plus d’informations sur l’[historique des versions d’Azure AD Connect](./reference-connect-version-history.md). Pour nous donner un feedback sur les fonctionnalités, votez sur le [canal User Voice Connect Health](https://feedback.azure.com/d365community/forum/22920db1-ad25-ec11-b6e6-000d3a4f0789)

## <a name="september-2021"></a>Septembre 2021
**Mise à jour de l’agent**
- Agent Azure AD Connect Health pour AD FS (version 3.1.113.0)
  - Correctif pour extraire des informations sur l’appareil, telles que la conformité des appareils et l’état géré, le système d’exploitation de l’appareil et la version du système d’exploitation de l’appareil, à partir des audits d’AD FS dans certains scénarios d’authentification basée sur l’appareil
  - Correctif pour remplir les informations d’application OAuth en cas d’échec et classer les échecs OAuth avec des codes d’erreur plus spécifiques
  - Correctif pour les alertes sur les appels WMI interrompus sur l’ordinateur du client. À ce stade, le résultat/l’état est défini sur « non exécuté ».

## <a name="may-2021"></a>Mai 2021
**Mise à jour de l’agent**
- Agent Azure AD Connect Health pour AD FS (version 3.1.99.0)
  - Correction de la faible valeur du nombre d’utilisateurs uniques dans le rapport d’activité d’application AD FS
  - Correction des connexions avec un CorrelationId de GUID vide ou par défaut

## <a name="march-2021"></a>Mars 2021
**Mise à jour de l’agent**

- Agent Azure AD Connect Health pour AD FS (version 3.1.95.0)

  - Correctif pour résoudre le nom d’utilisateur NT4 en UPN pendant les événements de connexion.
  - Correctif pour identifier les scénarios d’identificateur d’application incorrect avec un code d’erreur dédié.
  - Modifications afin d’ajouter une nouvelle propriété pour l’identificateur de client OAuth.
  - Correctif pour afficher les valeurs correctes dans les champs **Protocole** et **Type d’authentification** dans le rapport de connexions Azure AD pour certains scénarios de connexion.
  - Correctif pour afficher les adresses IP dans le champ de chaîne IP du rapport de connexions Azure AD dans l’ordre de la demande.
  - Modification afin d’introduire un nouveau champ permettant de distinguer si une authentification secondaire a été demandée pendant une connexion.
  - Correctif afin d’afficher la propriété d’identificateur d’application AD FS dans le rapport de connexions Azure AD.

## <a name="april-2020"></a>Avril 2020
**Mise à jour de l’agent**

- Agent Azure AD Connect Health pour AD FS (version 3.1.77.0)

   - Résolution de bogue pour l’alerte « Nom de principal du service (SPN) non valide pour le service AD FS », pour laquelle l’alerte signalait une erreur incorrecte.


## <a name="july-2019"></a>Juillet 2019
**Mise à jour de l’agent**
* Agent Azure AD Connect Health pour AD FS (version 3.1.59.0) 
   1. Modification du texte dans TestWindowsTransport
   2. Modifications pour le chargement d’AD FS RP
   
* Agent Azure AD Connect Health pour AD FS (version 3.1.56.0) 
   1. Ajout du test TestWindowsTransport et suppression des vérifications de point de terminaison WsTrust dans le test CheckOffice365Endpoints
   2. Enregistrer les informations sur le système d’exploitation et .NET
   3. Augmentation de la taille de téléchargement du message de configuration RP à 1 Mo.
   4. Résolution des bogues
   
* Agent Azure AD Connect Health pour AD DS (version 3.1.56.0) 
   1. Enregistrer les informations sur le système d’exploitation et .NET 
   2. Résolution des bogues

## <a name="may-2019"></a>Mai 2019
**Mise à jour de l’agent :** 
* Agent Azure AD Connect Health pour AD FS (version 3.1.51.0) 
   1. Correction d'un bogue pour faire la distinction entre plusieurs connexions partageant le même ID de requête client.
   2. Correction d'un bogue pour analyser les erreurs de nom d’utilisateur/mot de passe sur des serveurs localisés.   

## <a name="april-2019"></a>Avril 2019
**Mise à jour de l’agent :** 
* Agent Azure AD Connect Health pour AD FS (version 3.1.46.0) 
   1. Vérification d'une correction portant sur un processus d'alerte SPN en double pour ADFS

## <a name="march-2019"></a>Mars 2019
**Mise à jour de l’agent :** 
* Agent Azure AD Connect Health pour AD DS (version 3.1.41.0)  
   1. Collection de versions .NET
   2. Amélioration de la collection de compteurs de performances en l'absence de certaines catégories
   3. Correction d'un bogue empêchant l'apparition de plusieurs instances Monitoring Agent

* Agent Azure AD Connect Health pour AD FS (version 3.1.41.0) 
   1. Intégrer et mettre à niveau des scripts de test AD FS à l’aide d'ADFSToolBox
   2. Implémenter la collection de versions de .NET
   3. Amélioration de la collection de compteurs de performances en l'absence de certaines catégories
   4. Correction d'un bogue empêchant l'apparition de plusieurs instances Monitoring Agent


## <a name="november-2018"></a>Novembre 2018
**Nouvelles fonctionnalités en disponibilité générale :** 
* Azure AD Connect Health pour la synchronisation permet de diagnostiquer et de corriger les erreurs de synchronisation d’attribut en double depuis le portail

**Mise à jour de l’agent :** 
* Agent Azure AD Connect Health pour AD DS (version 3.1.24.0) 
   1. Conformité et application du protocole TLS (Transport Layer Security) version 1.2
   2. Réduction du bruit des alertes du catalogue global
   3. Correctifs de bogues de l’inscription de l’agent d’intégrité

* Agent Azure AD Connect Health pour AD FS (version 3.1.24.0)  
   1. Conformité et application du protocole TLS (Transport Layer Security) version 1.2
   2. Prise en charge de Test-ADFSRequestToken pour un système d’exploitation localisé
   3. Résolution du problème de verrouillage EventHandler de l’agent de diagnostic
   4. Correctifs de bogues de l’inscription de l’agent d’intégrité

## <a name="august-2018"></a>Août 2018 
*  Agent Azure AD Connect Health pour la synchronisation (version 3.1.7.0) fourni avec Azure AD Connect version 1.1.880.0    
   1. Correctif logiciel pour le [problème de l’agent de surveillance concernant une utilisation intensive de l’UC avec les versions de la Base de connaissances .NET Framework](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)

## <a name="june-2018"></a>Juin 2018 
**Nouvelles fonctionnalités préliminaires :** 
* Azure AD Connect Health pour la synchronisation permet de diagnostiquer et de corriger les erreurs de synchronisation d’attribut en double depuis le portail 

**Mise à jour de l’agent :** 
* Agent Azure AD Connect Health pour AD DS (version 3.1.7.0)    
  1. Correctif logiciel pour le [problème de l’agent de surveillance concernant une utilisation intensive de l’UC avec les versions de la Base de connaissances .NET Framework](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
   
* Agent Azure AD Connect Health pour AD FS (version 3.1.7.0)  
  1. Correctif logiciel pour le [problème de l’agent de surveillance concernant une utilisation intensive de l’UC avec les versions de la Base de connaissances .NET Framework](https://support.microsoft.com/help/4346822/high-cpu-issue-in-azure-active-directory-connect-health-for-sync)
  2. Correctifs de résultats de tests sur le serveur secondaire ADFS Server 2016
   
* Agent Azure AD Connect Health pour AD FS (version 3.1.2.0)  
  1. Correctif logiciel pour la gestion de la mémoire des agents et alertes associées spécifiquement pour la version 3.0.244.0


## <a name="may-2018"></a>Mai 2018
**Mise à jour de l’agent :**
* Agent Azure AD Connect Health pour AD DS (version 3.0.244.0)
  1. Amélioration de la confidentialité de l’agent  
  2. Résolutions de bogues et améliorations générales

* Agent Azure AD Connect Health pour AD FS (version 3.0.244.0)
  1. Service de diagnostic de l’agent et améliorations associées du module PowerShell
  2. Amélioration de la confidentialité de l’agent  
  3. Résolutions de bogues et améliorations générales

* Agent Azure AD Connect Health pour la synchronisation (version 3.0.164.0) fourni avec Azure AD Connect version 1.1.819.0 
  1. Amélioration de la confidentialité de l’agent  
  2. Résolutions de bogues et améliorations générales


## <a name="march-2018"></a>Mars 2018
**Nouvelles fonctionnalités préliminaires :**
* Azure AD Connect Health pour AD FS - Rapport et alerte d’adresse IP à risque.

**Mise à jour de l’agent :**

* Agent Azure AD Connect Health pour AD DS (version 3.0.176.0)
  1. Améliorations de la disponibilité de l’agent 
  2. Résolutions de bogues et améliorations générales
* Agent Azure AD Connect Health pour AD FS (version 3.0.176.0)
  1. Améliorations de la disponibilité de l’agent 
  2. Résolutions de bogues et améliorations générales
* Agent Azure AD Connect Health pour la synchronisation (version 3.0.129.0) fourni avec Azure AD Connect version 1.1.750.0  
  1. Améliorations de la disponibilité de l’agent 
  2. Résolutions de bogues et améliorations générales

## <a name="december-2017"></a>Décembre 2017
**Mise à jour de l’agent :**

* Agent Azure AD Connect Health pour AD DS (version 3.0.145.0)
  1. Améliorations de la disponibilité de l’agent 
  2. Ajout de nouvelles commandes de résolution des problèmes de l’agent
  3. Résolutions de bogues et améliorations générales
* Agent Azure AD Connect Health pour AD FS (version 3.0.145.0)
  1. Ajout de nouvelles commandes de résolution des problèmes de l’agent
  2. Améliorations de la disponibilité de l’agent 
  3. Résolutions de bogues et améliorations générales
  
## <a name="october-2017"></a>Octobre 2017
**Mise à jour de l’agent :**

 * Agent Azure AD Connect Health pour la synchronisation (version 3.0.129.0) fourni avec Azure AD Connect version 1.1.649.0
<br></br> Résolution d’un problème de compatibilité entre Azure AD Connect et Azure AD Connect Health Agent pour la synchronisation. Ce problème affecte les clients qui effectuent une mise à niveau sur place d’Azure AD Connect vers la version 1.1.647.0, mais qui ont actuellement la version 3.0.127.0 de Health Agent. Après la mise à niveau, Health Agent ne peut plus envoyer les données d’intégrité sur le service de synchronisation Azure AD Connect à Azure AD Health Service. Avec ce correctif, Health Agent version 3.0.129.0 est installé lors de la mise à niveau sur place d’Azure AD Connect. Health Agent version 3.0.129.0 n’a pas de problème de compatibilité avec Azure AD Connect version 1.1.649.0.

## <a name="july-2017"></a>Juillet 2017
**Mise à jour de l’agent :**

* Agent Azure AD Connect Health pour AD DS (version 3.0.68.0)
  1. Résolutions de bogues et améliorations générales
  2. Prise en charge du cloud souverain
* Agent Azure AD Connect Health pour AD FS (version 3.0.68.0)
  1. Résolutions de bogues et améliorations générales
  2. Prise en charge du cloud souverain
* Agent Azure AD Connect Health pour la synchronisation (version 3.0.68.0) fourni avec Azure AD Connect version 1.1.614.0
  1. Prise en charge de Microsoft Azure Government Cloud et Microsoft Cloud Germany

## <a name="april-2017"></a>Avril 2017      
**Mise à jour de l’agent :**

* Agent Azure AD Connect Health pour AD FS (version 3.0.12.0)
  1. Résolutions de bogues et améliorations générales
* Agent Azure AD Connect Health pour AD DS (version 3.0.12.0)
  1. Améliorations du chargement des compteurs de performances
  2. Résolutions de bogues et améliorations générales

## <a name="october-2016"></a>Octobre 2016
**Mise à jour de l’agent :**

* Agent Azure AD Connect Health pour AD FS (version 2.6.408.0)
* Amélioration de la détection des adresses IP clientes dans les demandes d’authentification
* Résolution des bogues associés aux alertes
* Agent Azure AD Connect Health pour AD DS (version 2.6.408.0)
* Résolution des bogues associés aux alertes
* Agent Azure AD Connect Health pour la synchronisation (version 2.6.353.0) fourni avec Azure AD Connect version 1.1.281.0
* Ajout des données requises pour les rapports d’erreurs de synchronisation
* Résolution des bogues associés aux alertes

**Nouvelles fonctionnalités préliminaires :**

* Rapports d’erreurs de synchronisation pour Azure AD Connect

**Nouvelles fonctionnalités :**

* Azure AD Connect Health pour AD FS - le champ d’adresse IP est disponible dans le rapport Top 50 des utilisateurs ayant saisi un nom d’utilisateur/mot de passe incorrect.

## <a name="july-2016"></a>Juillet 2016
**Nouvelles fonctionnalités préliminaires :**

* [Utilisation d’Azure AD Connect Health pour AD DS](how-to-connect-health-adds.md).

## <a name="january-2016"></a>Janvier 2016
**Mise à jour de l’agent :**

* agent Azure AD Connect Health pour AD FS (version 2.6.91.1512)

**Nouvelles fonctionnalités :**

* [Outil de test de connectivité pour les agents Azure AD Connect Health](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>Novembre 2015
**Nouvelles fonctionnalités :**

* Prise en charge du [Contrôle d’accès en fonction du rôle Azure (Azure RBAC)](how-to-connect-health-operations.md#manage-access-with-azure-rbac)

**Nouvelles fonctionnalités préliminaires :**

* [Azure AD Connect Health pour la synchronisation](how-to-connect-health-sync.md).

**Problèmes résolus :**

* Correctifs pour les erreurs survenant pendant les enregistrements d’agent.

## <a name="september-2015"></a>Septembre 2015
**Nouvelles fonctionnalités :**

* Rapport incorrect de mot de passe/nom d’utilisateur pour AD FS
* Prise en charge pour configurer le serveur proxy HTTP non authentifié
* Prise en charge pour configurer l’agent sur Server core
* Améliorations apportées aux alertes pour AD FS
* Améliorations dans Azure Agent AD Connect Health pour AD FS pour la connectivité et le chargement des données.

**Problèmes résolus :**

* Correctifs de bogues dans les informations d’utilisation pour les types d’erreur AD FS.

## <a name="june-2015"></a>Juin 2015
**Version initiale d’Azure AD Connect Health pour AD FS et le proxy AD FS.**

**Nouvelles fonctionnalités :**

* Alertes de surveillance des serveurs AD FS et de proxy AD FS avec les notifications par e-mail.
* Accès facile à la topologie et aux modèles AD FS dans les compteurs de performances AD FS.
* Tendances des demandes de jeton réussies sur les serveurs AD FS regroupés par applications, méthodes d’authentification, demande d’emplacement réseau, etc.
* Tendances des demandes ayant échoué sur les serveurs AD FS regroupés par applications, types d’erreur, etc.
* Déploiement d’agent plus simple à l’aide des informations d’identification d’administrateur général Azure AD.  

## <a name="next-steps"></a>Étapes suivantes
En savoir plus sur [Surveillez votre infrastructure d’identité locale et vos services de synchronisation dans le cloud](./whatis-azure-ad-connect.md).
