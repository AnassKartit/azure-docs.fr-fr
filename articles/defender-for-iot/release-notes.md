---
title: Nouveautés d’Azure Defender pour IoT
description: Cet article vous présente les nouveautés de la dernière version de Defender pour IoT.
ms.topic: overview
ms.date: 04/19/2021
ms.openlocfilehash: da5358ccf0f69ca2ba8f5722b75889b6b7c92c07
ms.sourcegitcommit: 6686a3d8d8b7c8a582d6c40b60232a33798067be
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/20/2021
ms.locfileid: "107752603"
---
# <a name="whats-new-in-azure-defender-for-iot"></a>Nouveautés d’Azure Defender pour IoT

Cet article répertorie les nouvelles fonctionnalités et améliorations apportées à Defender pour IoT.

Ces fonctionnalités sont en PRÉVERSION. Les [Conditions d’utilisation supplémentaires des préversions Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) incluent des conditions légales supplémentaires qui s’appliquent aux fonctionnalités Azure en version bêta, en préversion ou pas encore disponibles dans la version en disponibilité générale.

## <a name="april-2021"></a>Avril 2021

### <a name="work-with-automatic-threat-intelligence-updates-public-preview"></a>Utiliser les mises à jour automatiques de Threat Intelligence (préversion publique)

De nouveaux packages Threat Intelligence peuvent désormais être automatiquement envoyés aux capteurs connectés au cloud, car ils sont publiés par Microsoft Defender pour IoT. Ceci s’ajoute au téléchargement des packages Threat Intelligence, puis à leur chargement sur les capteurs.

L’utilisation des mises à jour automatiques permet de réduire les travaux opérationnels et de garantir une plus grande sécurité. Activez la mise à jour automatique en intégrant votre capteur connecté au cloud sur le portail Defender pour IoT avec **Mises à jour automatiques de Threat Intelligence** activé.

Si vous voulez adopter une approche plus conservatrice pour mettre à jour vos données Threat Intelligence, vous pouvez envoyer manuellement les packages à partir du portail Azure Defender pour IoT vers des capteurs connectés au cloud seulement quand vous l’estimez nécessaire.
Ceci vous donne la possibilité de contrôler le moment où un package est installé, sans devoir le télécharger, puis le charger sur vos capteurs. Envoyez manuellement les mises à jour aux capteurs à partir de la page **Sites et capteurs** de Defender pour IoT.

Vous pouvez également consulter les informations suivantes sur les packages Threat Intelligence :

- Version de package installée
- Mode de mise à jour de Threat Intelligence 
- État de la mise à jour de Threat Intelligence

### <a name="view-cloud-connected-sensor-information-public-preview"></a>Visualiser des informations sur les capteurs connectés au cloud (préversion publique)

Visualisez les informations opérationnelles importantes sur les capteurs connectés au cloud sur la page **Sites et capteurs**.

- La version installée du capteur.
- L’état de connexion du capteur au cloud.
- La dernière fois que le capteur a été détecté comme connecté au cloud.

### <a name="alert-api-enhancements"></a>Améliorations de l’API d’alerte

De nouveaux champs sont disponibles pour les utilisateurs qui travaillent avec les API d’alerte.

**Console de gestion locale**

- Adresses source et de destination
- Étapes de correction
- Le nom du capteur défini par l’utilisateur
- Le nom de la zone associée au capteur 
- Le nom du site associé au capteur

**Capteur**

- Adresses source et de destination
- Étapes de correction

L’API version 2 est nécessaire quand vous utilisez les nouveaux champs.

### <a name="features-delivered-as-generally-available-ga"></a>Fonctionnalités fournies en disponibilité générale

Avant, les fonctionnalités suivantes étaient disponibles pour la préversion publique et sont maintenant en disponibilité générale (GA) :

- Capteur - Règles d’alerte personnalisées améliorées
- Console de gestion locale - Exporter les alertes
- Ajouter une deuxième interface réseau à la console de gestion locale
- Générateur d’appareils - Nouveau micro-agent

## <a name="march-2021"></a>Mars 2021

### <a name="sensor---enhanced-custom-alert-rules-public-preview"></a>Capteur - Règles d’alerte personnalisée améliorées (préversion publique)

Vous pouvez maintenant créer des règles d’alerte personnalisées en fonction du jour, du groupe de jours et de l’heure à laquelle l’activité réseau a été détectée.  L’utilisation de conditions de règle de jour et d’heure est utile, par exemple dans les cas où la gravité de l’alerte est dérivée du moment où l’événement d’alerte a lieu. Créez par exemple une règle personnalisée qui déclenche une alerte de gravité élevée lorsque l’activité réseau est détectée le week-end ou le soir.

Cette fonctionnalité est disponible sur le capteur avec la version 10.2.

### <a name="on-premises-management-console---export-alerts-public-preview"></a>Console de gestion locale - Exporter les alertes (préversion publique)

Les informations d’alerte peuvent maintenant être exportées vers un fichier. csv à partir de la console de gestion locale. Vous pouvez exporter les informations de toutes les alertes détectées ou exporter des informations en fonction de la vue filtrée.

Cette fonctionnalité est disponible sur la console de gestion locale avec la version 10.2.

### <a name="add-second-network-interface-to-on-premises-management-console-public-preview"></a>Ajouter une deuxième interface réseau à la console de gestion locale (préversion publique)

Vous pouvez maintenant améliorer la sécurité de votre déploiement en ajoutant une deuxième interface réseau à votre console de gestion locale. Cette fonctionnalité permet à votre gestion sur site d’avoir ses capteurs connectés sur un réseau sécurisé, tout en permettant à vos utilisateurs d’accéder à la console de gestion sur site via une deuxième interface réseau distincte.

Cette fonctionnalité est disponible sur la console de gestion locale avec la version 10.2.

### <a name="add-second-network-interface-to-on-premises-management-console-public-preview"></a>Ajouter une deuxième interface réseau à la console de gestion locale (préversion publique)

Vous pouvez maintenant améliorer la sécurité de votre déploiement en ajoutant une deuxième interface réseau à votre console de gestion locale. Cette fonctionnalité permet à votre gestion sur site d’avoir ses capteurs connectés sur un réseau sécurisé, tout en permettant à vos utilisateurs d’accéder à la console de gestion sur site via une deuxième interface réseau distincte.

Cette fonctionnalité est disponible sur la console de gestion locale avec la version 10.2.
### <a name="device-builder---new-micro-agent-public-preview"></a>Générateur d’appareils - Nouveau micro-agent (préversion publique)

Un nouveau module Générateur d’appareils est disponible. Le module, désigné sous le terme de micro-agent, offre les éléments suivants :

- **Intégration à Azure IoT Hub et Azure Defender pour IoT** : renforcez la sécurité des points de terminaison directement sur vos appareils IoT en l’intégrant à l’option d’analyse fournie par Azure IoT Hub et Azure Defender pour IoT.
- **Options de déploiement flexibles avec prise en charge des systèmes d’exploitation IoT standard** : possibilité de déploiement en tant que package binaire ou en tant que code source modifiable, avec prise en charge des systèmes d’exploitation IoT standard, tels que Linux et Azure RTOS.
- **Ressources minimales requises sans dépendances du noyau du système d’exploitation** : faible encombrement, faible consommation du processeur et aucune dépendance du noyau du système d’exploitation.
- **Gestion de la posture de sécurité** : surveillez de manière proactive la posture de sécurité de vos appareils IoT.
- **Détection continue des menaces IoT/OT en temps réel** : détectez les menaces, telles que les botnets, les tentatives d’attaque par force brute, les cryptomineurs et l’activité réseau suspecte

La documentation du micro-agent Defender-IoT déconseillé sera déplacée dans le dossier *Solution basée sur les agents pour les générateurs d’appareils > Classique*.

Cet ensemble de fonctionnalités est disponible avec la version cloud de la préversion publique actuelle.

## <a name="january-2021"></a>Janvier 2021

- [Sécurité](#security)
- [Intégration](#onboarding)
- [Usage](#usability)
- [Autres mises à jour](#other-updates)
### <a name="security"></a>Sécurité

Des améliorations ont été apportées à la récupération des certificats et des mots de passe pour cette version.

#### <a name="certificates"></a>Certificats
  
Cette version vous permet de :

- Charger les certificats SSL directement dans les capteurs et les consoles de gestion locales.
- Effectuer la validation entre la console de gestion locale et les capteurs connectés, et entre une console de gestion et une console de gestion haute disponibilité. La validation repose sur les dates d’expiration, l’authenticité de l’autorité de certification racine et les listes de révocation de certificats.  Si la validation échoue, la session est interrompue.

Pour les mises à niveau :

- Aucune modification n’est apportée au certificat SSL ou à la fonctionnalité de validation lors de la mise à niveau.
- Après la mise à niveau, les utilisateurs administratifs de la console de gestion locale et du capteur peuvent remplacer les certificats SSL ou activer la validation de certificat SSL à partir de la fenêtre Paramètres système > Certificat SSL.  

Pour les nouvelles installations :

- À la première connexion, les utilisateurs doivent utiliser un certificat SSL (recommandé) ou un certificat auto-signé généré localement (non recommandé)
- La validation des certificats est activée par défaut pour les nouvelles installations.

#### <a name="password-recovery"></a>Récupération de mot de passe
  
Les utilisateurs administratifs de la console de gestion locale et du capteur peuvent désormais récupérer les mots de passe à partir du portail Azure Defender pour IoT. La récupération du mot de passe nécessitait auparavant l’intervention de l’équipe du support technique.

### <a name="onboarding"></a>Mise en route

#### <a name="on-premises-management-console---committed-devices"></a>Console de gestion locale - appareils validés

Après la connexion initiale à la console de gestion locale, les utilisateurs doivent désormais charger un fichier d’activation. Le fichier contient le nombre agrégé d’appareils à surveiller sur le réseau de l’organisation. Ce nombre est désigné sous le terme de nombre d’appareils validés.
Les appareils validés sont définis au cours du processus d’intégration sur le portail Azure Defender pour IoT, où le fichier d’activation est généré.
Les nouveaux utilisateurs et ceux qui effectuent une mise à niveau doivent charger le fichier d’activation.
Après l’activation initiale, le nombre d’appareils détectés sur le réseau peut dépasser le nombre d’appareils validés. Cet événement peut se produire, par exemple, si vous connectez d’autres capteurs à la console de gestion. S’il y a une différence entre le nombre d’appareils détectés et le nombre d’appareils validés, un avertissement s’affiche dans la console de gestion. Si cet événement se produit, vous devez charger un nouveau fichier d’activation.

#### <a name="pricing-page-options"></a>Options de la page de tarification

La page de tarification vous permet d’intégrer de nouveaux abonnements à Azure Defender pour IoT et de définir les appareils validés dans votre réseau.  
En outre, elle vous permet désormais de gérer les abonnements existants associés à un capteur et de mettre à jour la validation de l’appareil.

#### <a name="view-and-manage-onboarded-sensors"></a>Afficher et gérer les capteurs intégrés

Une nouvelle page du portail dédiée au site et aux capteurs vous permet de :

- Ajouter des informations détaillées relatives au capteur. Par exemple, une zone associée au capteur ou des étiquettes de texte libre.
- Afficher et filtrer les informations sur le capteur. Par exemple, affichez des détails sur les capteurs connectés au cloud ou gérés localement, ou des informations sur les capteurs dans une zone spécifique.  

### <a name="usability"></a>Facilité d'utilisation

#### <a name="azure-sentinel-new-connector-page"></a>Page du nouveau connecteur Azure Sentinel

La page du connecteur de données Azure Defender pour IoT dans Azure Sentinel a été repensée. Le connecteur de données est désormais basé sur des abonnements plutôt que sur IoT Hubs, ce qui permet aux clients de mieux gérer leur connexion de configuration à Azure Sentinel.

#### <a name="azure-portal-permission-updates"></a>Mises à jour des autorisations du portail Azure  

La prise en charge du Lecteur de sécurité et de l’Administrateur de la sécurité a été ajoutée.

### <a name="other-updates"></a>Autres mises à jour

#### <a name="access-group---zone-permissions"></a>Groupe d’accès - autorisations de zone
  
Les règles de groupe d’accès à la console de gestion locale n’incluent pas l’option permettant d’accorder l’accès à une zone spécifique. La définition des règles qui utilisent des sites, des régions et des unités commerciales reste inchangée.   Après la mise à niveau, les groupes d’accès qui contenaient des règles autorisant l’accès à des zones spécifiques seront modifiés pour autoriser l’accès au site parent, y compris toutes ses zones.

#### <a name="terminology-changes"></a>Changements terminologiques

Le terme « ressource » a été renommé en « appareil » dans le capteur et la console de gestion locale, les rapports et d’autres interfaces de la solution.
Dans les alertes de la console de gestion locale et du capteur, le terme Gérer cet événement a été nommé Étapes de correction.

## <a name="next-steps"></a>Étapes suivantes

[Bien démarrer avec Defender pour IoT](getting-started.md)
