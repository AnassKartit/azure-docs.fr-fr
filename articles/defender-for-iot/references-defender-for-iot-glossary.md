---
title: Glossaire de Defender pour IoT
description: Ce glossaire fournit une brève description des termes et concepts importants de la plateforme Defender pour IoT.
ms.date: 12/09/2020
ms.topic: article
ms.openlocfilehash: 829469a523ffd259b0108fcfc189715ba99cce74
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2021
ms.locfileid: "108203682"
---
# <a name="defender-for-iot-glossary"></a>Glossaire de Defender pour IoT

Ce glossaire fournit une brève description des termes et concepts importants pour la plateforme Azure Defender pour IoT. Sélectionnez les liens **En savoir plus** pour accéder aux termes associés dans le glossaire. Cela vous permettra d’apprendre et d’utiliser plus rapidement les outils de produit.

> [!Note]
> Tout terme avec un `(DB)` listé dans son nom est un terme du générateur d’appareils basé sur un agent. 

<a name="glossary-a"></a>

## <a name="a"></a>Un

| Terme | Description | En savoir plus |
|--|--|--|
| **Groupe d’accès** | Répondez aux exigences d’accès utilisateur pour les grandes organisations en créant des règles de groupe d’accès.<br /><br />Les règles vous permettent de contrôler l’accès à l’affichage et à la configuration de la console de gestion locale Defender pour IoT pour des rôles d’utilisateur spécifiques aux unités commerciales, régions, zones et sites pertinents.<br /><br />Par exemple, autorisez les analystes de la sécurité d’un groupe Active Directory à accéder aux données automobiles d’Europe de l’Ouest tout en empêchant l’accès aux données d’Afrique. | **[Console de gestion locale](#o)** <br /><br />**[Unité commerciale](#b)** |
| **Jetons d’accès** | Générez des jetons d’accès pour accéder à l’API REST Defender pour IoT. | **[API](#glossary-a)** |
| **Accuser réception d’un événement d’alerte** | Demandez à Defender pour IoT de masquer l’alerte une seule fois pour l’événement détecté. L’alerte sera à nouveau déclenchée si l’événement est de nouveau détecté. | **[Alerte](#glossary-a)<br /><br />[Découvrir un événement d’alerte](#l)<br /><br />[Désactiver les événements d’alerte](#m)** |
| **Alert** | Message qu’un moteur Defender pour IoT déclenche lors des écarts par rapport au comportement réseau autorisé, des anomalies réseau ou d’activités réseau et de trafic suspects. | **[Règle de transfert](#f)<br /><br />[Règle d’exclusion](#e)<br /><br />[Notifications système](#s)** |
| **Commentaire d’alerte** | Commentaires que les analystes et administrateurs de la sécurité indiquent dans les messages d’alerte. Par exemple, un commentaire d’alerte peut fournir des instructions sur les mesures d’atténuation à prendre, ou les noms des personnes à contacter concernant l’événement.<br /><br />Les utilisateurs qui consultent les alertes peuvent choisir le ou les commentaires qui reflètent le mieux l’état de l’événement ou les étapes à suivre pour examiner l’alerte. | **[Alerte](#glossary-a)** |
| **Moteur d’anomalies** | Un moteur Defender pour IoT qui détecte les communications et comportements inhabituels entre machines. Par exemple, le moteur peut détecter des tentatives de connexion SMB excessives. Les alertes d’anomalie sont déclenchées lorsque ces événements sont détectés. | **[Moteurs Defender pour IoT](#d)** |
| **API** | Permet aux systèmes externes d’accéder aux données découvertes par Defender pour IoT et d’effectuer des actions à l’aide de l’API REST externe sur des connexions SSL. | **[Jetons d’accès](#glossary-a)** |
| **Rapport de vecteur d’attaque** | Représentation graphique en temps réel des chaînes de vulnérabilité des points de terminaison exploitables.<br /><br />Les rapports vous permettent d’évaluer l’effet des activités d’atténuation dans la séquence d’attaque à déterminer. Par exemple, vous pouvez évaluer si une mise à niveau du système perturbe le chemin d’accès de l’attaquant en interrompant la chaîne d’attaque ou si un autre chemin d’accès d’attaque demeure. Cela hiérarchise les activités de correction et d’atténuation. | **[Rapport d’évaluation des risques](#r)** |

## <a name="b"></a>B

| Terme | Description | En savoir plus |
|--|--|--|
| **Unité commerciale** | Organisation logique de votre entreprise selon des normes spécifiques au secteur.<br /><br />Par exemple, une société mondiale qui possède des usines de verre et de plastique peut être gérée comme deux unités commerciales différentes. Vous pouvez contrôler l’accès des utilisateurs de Defender pour IoT à des unités commerciales spécifiques. | **[Console de gestion locale](#o)<br /><br />[Groupe d’accès](#glossary-a)<br /><br />[Site](#s)<br /><br />[Zone](#z)** |
| **Performances de base** | Trafic réseau, protocoles, commandes et appareils approuvés. Defender pour IoT identifie les écarts par rapport à la ligne de base du réseau. Affichez le trafic de base approuvé en générant des rapports d’exploration de données. | **[Exploration de données](#d)<br /><br />[Mode apprentissage](#l)** |

## <a name="c"></a>C

| Terme | Description | En savoir plus |
|--|--|--|
| **Commandes CLI** | Options de l’interface de ligne de commande (CLI) pour les utilisateurs administrateurs de Defender pour IoT. Les commandes CLI sont disponibles pour les fonctionnalités qui ne sont pas accessibles à partir des consoles Defender pour IoT. | - |


## <a name="d"></a>D

| Terme | Description | En savoir plus |
|--|--|--|
| **Exploration de données** | Générez des rapports complets et granulaires sur vos périphériques réseau :<br /><br />- **réponse aux incidents SOC** : Rapports en temps réel pour vous aider à répondre immédiatement aux incidents. Par exemple, un rapport peut répertorier les appareils qui peuvent nécessiter une mise à jour corrective.<br /><br />- **Forensique** : Rapports basés sur les données d’historique des rapports d’investigation.<br /><br />- **intégrité du réseau informatique** : Rapports qui contribuent à améliorer la sécurité globale du réseau. Par exemple, un rapport peut répertorier des appareils avec des informations d’identification faibles.<br /><br />- **visibilité** : Rapports qui couvrent tous les éléments de requête pour afficher tous les paramètres de ligne de base de votre réseau.<br /><br />Enregistrez les rapports d’exploration de données pour que les utilisateurs en lecture seule puissent les afficher. | **[Ligne de base](#b)<br /><br />[Rapports](#r)** |
| **plateforme Defender pour IoT** | La solution Defender pour IoT installée sur Defender pour les capteurs IoT et la console de gestion locale. | **[Capteur](#s)<br /><br />[Console de gestion locale](#o)** |
| **Carte des appareils** | Représentation graphique des périphériques réseau détectés par Defender pour IoT. Elle montre les connexions entre les appareils et les informations sur chaque appareil. Utilisez la carte pour :<br /><br />- Récupérer et contrôler les informations critiques de l’appareil.<br /><br />- Analyser des sections du réseau.<br /><br />- Exporter les détails et les résumés des appareils. | **[Groupe de couches Purdue](#p)** |
| **Inventaire des appareils – capteur** | L’inventaire des appareils affiche un large éventail d’attributs d’appareil détectés par Defender pour IoT. Des options sont disponibles pour :<br /><br />- Filtrer les informations affichées.<br /><br />- Exporter ces informations dans un fichier CSV.<br /><br />- Importer les détails du Registre Windows. | **[Groupe](#g)** <br /><br />**[Inventaire des appareils – console de gestion locale](#d)** |
| **Inventaire des appareils – console de gestion locale** | Les informations d’appareil obtenues à partir des capteurs connectés peuvent être consultées à partir de la console de gestion locale dans l’inventaire des appareils. Les utilisateurs de la console de gestion locale disposent ainsi d’une vue complète de toutes les informations réseau. | **[Inventaire des appareils – Capteur](#d)<br /><br />[Inventaire des appareils – intégrateur de données](#d)** |
| **Inventaire des appareils – intégrateur de données** | Les fonctionnalités d’intégration de données de la console de gestion locale vous permettent d’améliorer les données de l’inventaire des appareils avec des informations provenant d’autres ressources de l’entreprise. Des exemples de ces ressources sont les CMDB, DNS, pare-feu et API web. | **[Inventaire des appareils – console de gestion locale](#d)** |
| **Jumeaux d’appareil** `(DB)` | Les jumeaux d’appareil sont des documents JSON qui stockent des informations sur l’état des appareils (métadonnées, configurations et conditions). | [Jumeau de module](#m) <br /> <br />[Jumeau du micro-agent Defender-IoT](#s) |

## <a name="e"></a>E

| Terme | Description | En savoir plus |
|--|--|--|
| **Engines** (Moteurs) | Les moteurs d’analyse d’apprentissage automatique de Defender pour IoT éliminent la nécessité de mettre à jour les signatures ou de définir des règles. Les moteurs utilisent l’analyse comportementale et la science des données propres à ICS pour analyser en permanence le trafic réseau à la recherche d’anomalies, de logiciels malveillants, de problèmes opérationnels, de violations de protocole et d’écarts par rapport à l’activité réseau de ligne de base.<br /><br />Lorsqu’un moteur détecte un écart, une alerte est déclenchée. Les alertes peuvent être affichées et gérées à partir de l’écran **Alertes** ou d’un SIEM. | **[Alerte](#glossary-a)** |
| **Vue entreprise** | Carte globale qui présente les unités commerciales, sites et zones où les capteurs Defender pour IoT sont installés. Affichez les emplacements géographiques des alertes malveillantes, des alertes opérationnelles et plus encore. | **[Unité commerciale](#b)<br /><br />[Site](#s)<br /><br />[Zone](#z)** |
| **Chronologie des événements** | Chronologie des activités détectées sur votre réseau, notamment :<br /><br />- Alertes déclenchées.<br /><br />- Événements réseau (informations).<br /><br />- Opérations de l’utilisateur, comme la connexion, la suppression de l’utilisateur et la création d’un utilisateur, ainsi que les opérations de gestion des alertes, comme la désactivation, la découverte et l’envoi d’un accusé de réception. Disponible dans les consoles des capteurs. | - |
| **Règle d’exclusion** | Demandez à Defender pour IoT d’ignorer les déclencheurs d’alerte en fonction de la période, de l’adresse de l’appareil et du nom de l’alerte, ou par capteur spécifique.<br /><br />Par exemple, si vous savez que tous les appareils OT surveillés par un capteur spécifique passent par une procédure de maintenance comprise entre 6 h 30 et 10 h 15 le matin, vous pouvez définir une règle d’exclusion qui indique que ce capteur ne doit pas envoyer d’alertes dans la période prédéfinie. | **[Alerte](#glossary-a)<br /><br />[Désactiver les événements d’alerte](#m)** |

## <a name="f"></a>F

| Terme | Description | En savoir plus |
|--|--|--|
| **Règle de transfert** | Les règles de transfert indiquent à Defender pour IoT d’envoyer des informations d’alerte aux fournisseurs ou aux systèmes partenaires.<br /><br />Par exemple, envoyez des informations d’alerte à un serveur Splunk ou à un serveur syslog. | **[Alerte](#glossary-a)** |

## <a name="g"></a>G

| Terme | Description | En savoir plus |
|--|--|--|
| **Groupe** | Groupes prédéfinis ou personnalisés d’appareils qui contiennent des attributs spécifiques, tels que des appareils qui ont effectué une activité de programmation ou des appareils situés sur un sous-réseau spécifique. Utilisez des groupes pour vous aider à afficher et analyser des appareils dans Defender pour IoT.<br /><br />Les groupes peuvent être affichés dans et créés à partir de la carte des appareils et dans l’inventaire des appareils. | **[Carte des appareils](#d)<br /><br />[Inventaire des appareils](#d)** |

## <a name="h"></a>H

| Terme | Description | En savoir plus |
|--|--|--|
| **Environnement de développement ouvert Horizon** | Sécurisez les appareils IoT et ICS exécutant des protocoles propriétaires et personnalisés, ou des protocoles qui s’écartent de n’importe quelle norme. Utilisez le kit de développement logiciel (SDK) horizon Open Development Environment (ODE) pour créer des plug-ins de dissecteurs qui décodent le trafic réseau en fonction des protocoles définis. Les services Defender pour IoT analysent le trafic pour fournir une surveillance, des alertes et des rapports complets.<br /><br />Utilisez horizon pour :<br /><br />- **Développez** la visibilité et le contrôle sans avoir besoin de mettre à niveau les versions de la plateforme Defender pour IoT.<br /><br />- **Sécurisez** les informations propriétaires en développant sur site en tant que plug-in externe.<br /><br />- **Localisez** le texte pour les alertes, les événements et les paramètres de protocole.<br /><br />Pour plus d’informations, contactez votre représentant Réussite du client. | **[Prise en charge de protocole](#p)<br /><br />[Localisation](#l)** |
| **alerte personnalisée Horizon** | Améliorez la gestion des alertes au sein de votre entreprise en déclenchant des alertes personnalisées pour n’importe quel protocole (sur la base des dissecteurs de trafic du framework Horizon).<br /><br />Ces alertes peuvent être utilisées pour communiquer des informations :<br /><br />- Sur les détections de trafic basées sur les protocoles et les protocoles sous-jacents dans un plug-in Horizon propriétaire.<br /><br />- Sur une combinaison de champs de protocole à partir de toutes les couches de protocole. | **[Prise en charge de protocole](#p)** |

## <a name="i"></a>I

| Terme | Description | En savoir plus |
|--|--|--|
| **IoT Hub** `(DB)` | Service managé, hébergé dans le cloud, qui agit en tant que concentrateur de messages central pour la communication bidirectionnelle entre votre application IoT et les appareils qu’il gère.  |   |
| **Intégrations** | Développez les fonctionnalités de Defender pour IoT en partageant les informations des appareils avec des systèmes partenaires. Les organisations peuvent relier des solutions de gestion des appareils, de sécurité, de gestion des incidents et de sécurité précédemment en isolées afin d’accélérer les réponses au niveau du système et d’atténuer les risques plus rapidement. | **[Règle de transfert](#f)** |
| **Sous-réseau interne** | Configurations de sous-réseau définies par Defender pour IoT. Dans certains cas, par exemple pour les environnements qui utilisent des plages publiques comme plages internes, vous pouvez demander à Defender pour IoT de résoudre tous les sous-réseaux en tant que sous-réseaux internes. Les sous-réseaux s’affichent dans la carte et dans différents rapports de Defender pour IoT. | **[Sous-réseaux](#s)** |

## <a name="l"></a>L

| Terme | Description | En savoir plus |
|--|--|--|
| **Découvrir un événement d’alerte** | Demandez à Defender pour IoT d’autoriser le trafic détecté dans un événement d’alerte. | **[Alerte](#glossary-a)<br /><br />[Accuser réception d’un événement d’alerte](#glossary-a)<br /><br />[Désactiver les événements d’alerte](#m)** |
| **Mode apprentissage** | Mode utilisé lorsque Defender pour IoT découvre votre activité réseau. Cette activité devient la ligne de base de votre réseau. Defender pour IoT reste dans ce mode pendant une période prédéfinie après l’installation. Une activité qui dévie de l’activité apprise après cette période déclenchera des alertes Defender pour IoT. | **[Apprentissage informatique intelligent](#s)<br /><br />[Ligne de base](#b)** |
| **Localisation** | Localisez le texte pour les alertes, les événements et les paramètres de protocole pour les plug-ins de dissecteur développés par Horizon. | **[Environnement de développement ouvert Horizon](#h)** |

## <a name="m"></a>M


| Terme | Description | En savoir plus |
|--|--|--|
| **Micro-agent** `(DB)` | Fournit des fonctionnalités de sécurité approfondies pour les appareils IoT, y compris l’état de la sécurité et la détection des menaces. | |
| **Jumeau de module** `(DB)` | Les jumeaux de module sont des documents JSON qui stockent des informations sur l’état des modules, telles que les métadonnées, configurations et conditions. | [Jumeau d’appareil](#d) <br /> <br />[Jumeau du micro-agent Defender-IoT](#s) |
| **Désactiver les événements d’alerte** | Demandez à Defender pour IoT d’ignorer continuellement l’activité avec des appareils identiques et un trafic comparable. | **[Alerte](#glossary-a)<br /><br />[Règle d’exclusion](#e)<br /><br />[Accuser réception d’un événement d’alerte](#glossary-a)<br /><br />[Découvrir un événement d’alerte](#l)** |

## <a name="n"></a>N

| Terme | Description | En savoir plus |
|--|--|--|
| **Notifications** | Informations sur les modifications du réseau ou les propriétés d’appareil non résolues. Des options sont disponibles pour mettre à jour les informations d’appareil et de réseau avec les nouvelles données détectées. La réponse aux notifications enrichit l’inventaire des appareils, la carte et divers rapports. Disponible sur les consoles de capteur. | **[Alerte](#glossary-a)<br /><br />[Notifications système](#s)** |

## <a name="o"></a>O

| Terme | Description | En savoir plus |
|--|--|--|
| **Console de gestion locale** | La console de gestion locale offre une vue et une gestion centralisées des appareils et des menaces détectés par les déploiements de capteurs Defender pour IoT dans votre organisation. | **[Plateforme Defender pour IoT](#d)<br /><br />[Capteur](#s)** |
| **Alerte opérationnelle** | Alertes qui concernent des problèmes réseau opérationnels, par exemple un appareil soupçonné de s’être déconnecté du réseau. | **[Alerte](#glossary-a)<br /><br />[Alerte de sécurité](#s)** |

## <a name="p"></a>P

| Terme | Description | En savoir plus |
|--|--|--|
| **Couche Purdue** | Affiche les interconnexions et les interdépendances des principaux composants d’un ICS standard sur la carte. |  |
| **Prise en charge du protocole** | En plus de la prise en charge des protocoles incorporés, vous pouvez sécuriser les appareils IoT et ICS exécutant des protocoles propriétaires et personnalisés, ou des protocoles qui s’écartent de n’importe quel standard, en utilisant le Kit de développement logiciel (SDK) de l’environnement de développement ouvert Horizon. | **[Environnement de développement ouvert Horizon](#h)** |

## <a name="r"></a>R

| Terme | Description | En savoir plus |
|--|--|--|
| **Région** | Division logique d’une organisation globale en régions géographiques. Exemples : Amérique du Nord, Europe de l’Ouest et Europe de l’Est.<br /><br />L’Amérique du Nord peut avoir des usines appartenant à différentes unités commerciales. | **[Groupe d’accès](#glossary-a)<br /><br />[Unité commerciale](#b)<br /><br />[Console de gestion locale](#o)<br /><br />[Site](#s)<br /><br />[Zone](#z)** |
| **Rapports** | Les rapports reflètent les informations générées par les résultats d’une requête d’exploration de données. Cela comprend les résultats d’exploration de données par défaut, qui sont disponibles dans la vue **Rapports**. Les administrateurs et les analystes de la sécurité peuvent également générer des requêtes d’exploration de données personnalisées et les enregistrer sous forme de rapports. Ces rapports sont également disponibles pour les utilisateurs en lecture seule. | **[Exploration de données](#d)** |
| **Rapport d’évaluation des risques** | Les rapports d’évaluation des risques vous permettent de générer un score de sécurité pour chaque appareil réseau, ainsi qu’un score de sécurité réseau global. Le score global représente le pourcentage de sécurité (sur une échelle de 100 %). Le rapport fournit des recommandations d’atténuation qui vous aideront à améliorer votre score de sécurité actuel. | - |

## <a name="s"></a>S

| Terme | Description | En savoir plus |
|--|--|--|
| **Alerte de sécurité** | Alertes qui concernent des problèmes de sécurité, comme des tentatives de connexion SMB excessives ou des détections de programmes malveillants. | **[Alerte](#glossary-a)<br /><br />[Alerte opérationnelle](#o)** |
| **Jumeau du micro-agent Defender-IoT** `(DB)` | Le jumeau du micro-agent Defender IoT contient toutes les informations relatives à la sécurité des appareils pour chaque appareil spécifique de votre solution. | [Jumeau d’appareil](#d) <br /> <br />[Jumeau de module](#m)  |
| **Détection sélective** | Defender pour IoT inspecte passivement le trafic informatique et OT et détecte les informations pertinentes sur les appareils, leurs attributs, leur comportement et bien plus encore. Dans certains cas, il est possible que certaines informations ne soient pas visibles dans les analyses réseau passives.<br /><br />Dans ce cas, vous pouvez utiliser les outils de détection granulaires sûrs dans Defender pour IoT pour découvrir des informations importantes sur les appareils précédemment inaccessibles. | - |
| **Capteur** | Machine physique ou virtuel sur laquelle la plateforme Defender pour IoT est installée. | **[Console de gestion locale](#o)** |
| **Site** | Emplacement d’une usine ou d’une autre entité. Le site doit contenir une ou plusieurs zones dans lesquelles un capteur est installé. | **[Zone](#z)** |
| **Gestion de site** | L’option de la console de gestion locale qui vous permet de gérer les capteurs d’entreprise. | - |
| **Apprentissage informatique intelligent** | Une fois la période d’apprentissage terminée et le mode d’apprentissage désactivé, Defender pour IoT peut détecter un niveau anormalement élevé de modifications de ligne de base qui sont le résultat d’une activité informatique normale, comme des requêtes DNS et HTTP. Ce trafic peut déclencher des alertes de violation de stratégie et des notifications système inutiles. Pour réduire ces alertes et notifications, vous pouvez activer l’apprentissage informatique intelligent. | **[Mode apprentissage](#l)<br /><br />[Ligne de base](#b)** |
| **Sous-réseaux** | Pour permettre la focalisation sur les appareils OT, les appareils informatiques sont automatiquement regroupés par sous-réseau dans la carte des appareils. Chaque sous-réseau est présenté sous la forme d’une entité unique sur la carte, avec une capacité de réduction ou d’extension interactive pour se concentrer sur un sous-réseau informatique. | **[Carte des appareils](#d)** |
| **Notifications système** | Notifications de la console de gestion locale au sujet des éléments suivants :<br /><br />- État de la connexion du capteur.<br /><br />- Échecs de sauvegarde à distance. | **[Notifications](#n)<br /><br />[Alerte](#glossary-a)** |

## <a name="z"></a>Z

| Terme | Description | En savoir plus |
|--|--|--|
| **Zone** | Zone au sein d’un site dans laquelle un ou plusieurs capteurs sont installés. | **[Site](#s)<br /><br />[Unité commerciale](#b)<br /><br />[Région](#r)** |
