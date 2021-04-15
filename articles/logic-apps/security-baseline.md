---
title: Ligne de base de sécurité Azure pour Logic Apps
description: La base de référence de sécurité Logic Apps fournit des instructions et des ressources pour la mise en œuvre des recommandations de sécurité spécifiées dans le benchmark de sécurité Azure.
author: msmbaldwin
ms.service: logic-apps
ms.topic: conceptual
ms.date: 03/29/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 0b74962a192de6a10c09b9855780ed0cd6244515
ms.sourcegitcommit: f5448fe5b24c67e24aea769e1ab438a465dfe037
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105967289"
---
# <a name="azure-security-baseline-for-logic-apps"></a>Ligne de base de sécurité Azure pour Logic Apps

Cette base de référence de sécurité applique les conseils du [benchmark de sécurité Azure version 1.0](../security/benchmarks/overview-v1.md) à Logic Apps. Le benchmark de sécurité Azure fournit des recommandations sur la façon dont vous pouvez sécuriser vos solutions cloud sur Azure. Le contenu est regroupé en fonction des **contrôles de sécurité** définis par le benchmark de sécurité Azure et les conseils associés applicables à Logic Apps. Les **contrôles** non applicables à Logic Apps, ou dont la responsabilité incombe à Microsoft, ont été exclus.

Pour voir la façon dont Logic Apps est entièrement mappé au benchmark de sécurité Azure, consultez le [fichier de mappage complet de la base de référence de sécurité Logic Apps](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Sécurité réseau

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : sécurité réseau](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1 : Protéger les ressources Azure au sein des réseaux virtuels

**Aide** : Les connecteurs qui s’exécutent dans le service « global » à plusieurs locataires de Logic Apps sont déployés et gérés par Microsoft. Ces connecteurs fournissent des déclencheurs et actions qui permettent d’accéder à des services cloud et/ou à des systèmes locaux, tels qu’Office 365, Stockage Blob Azure, SQL Server, Dynamics, Salesforce, SharePoint, etc. Vous pouvez utiliser la balise de service AzureConnectors lors de la spécification de règles sur votre groupe de sécurité réseau ou votre pare-feu Azure pour autoriser l’accès aux ressources associées.

Pour les applications logiques qui ont besoin d’un accès direct aux ressources d’un réseau virtuel Azure, vous pouvez créer un environnement d’intégration de service (ISE) dans lequel vous pouvez générer, déployer et exécuter vos applications logiques sur des ressources dédiées. Certains réseaux virtuels Azure utilisent des points de terminaison privés (Azure Private Link) pour fournir un accès aux services PaaS Azure, tels que Stockage Azure, Azure Cosmos DB ou Azure SQL Database, aux services partenaires ou aux services clients hébergés sur Azure. Si vos applications logiques ont besoin d’accéder à des réseaux virtuels qui utilisent des points de terminaison privés, vous devez créer, déployer et exécuter ces applications logiques à l’intérieur d’un environnement ISE.

Lorsque vous créez votre ISE, vous pouvez choisir d’utiliser des points de terminaison d’accès internes ou externes. Votre sélection détermine si les déclencheurs de demande ou de webhook sur les applications logiques dans votre ISE peuvent recevoir des appels en dehors de votre réseau virtuel. Les points de terminaison d’accès interne et externe déterminent également si vous pouvez afficher l’historique d’exécution de votre application logique, y compris les entrées et sorties d’une exécution, à l’intérieur ou à l’extérieur de votre réseau virtuel.

Assurez-vous qu'un groupe de sécurité réseau lié à votre ISE a été appliqué à tous les déploiements de sous-réseaux du réseau virtuel avec des contrôles d'accès réseau spécifiques aux ports et sources approuvés de votre application. Quand vous déployez vos applications logiques dans un environnement ISE, utilisez une liaison privée. Azure Private Link vous permet d’accéder aux services Azure PaaS ainsi qu’aux services de partenaires ou de clients hébergés par Azure sur un point de terminaison privé dans votre réseau virtuel. Pour les cas d’usage spécifiques, vous pouvez également satisfaire aux exigences en implémentant le Pare-feu Azure. Pour vous faciliter la tâche lors de la configuration de règles de sécurité,utilisez des balises de service qui représentent les groupes de préfixes d’adresse IP d’un service Azure spécifique.

- [Comprendre les connecteurs pour Logic Apps](../connectors/apis-list.md)

- [Comprendre les balises de service dans Azure](../virtual-network/service-tags-overview.md)

- [Comprendre comment accéder aux ressources Réseau virtuel Microsoft Azure à partir d’Azure Logic Apps à l’aide d’environnements de service d’intégration (ISE)](connect-virtual-network-vnet-isolated-environment-overview.md)

- [Présentation des points de terminaison de service de réseau virtuel](../virtual-network/virtual-network-service-endpoints-overview.md)

- [Présentation d’Azure Private Link](../private-link/private-link-overview.md)

- [Comprendre comment accéder au point de terminaison de l’environnement ISE](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment-overview#ise-endpoint-access)

- [Guide pratique pour créer un réseau virtuel](../virtual-network/quick-create-portal.md)

- [Guide pratique pour créer un groupe NSG avec une configuration de sécurité](../virtual-network/tutorial-filter-network-traffic.md)

- [Guide pratique pour déployer et configurer le Pare-feu Azure](../firewall/tutorial-firewall-deploy-portal.md)

- [Guide pratique pour activer l’accès pour l’environnement ISE](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment#enable-access-for-ise)

**Responsabilité** : Partagé

**Supervision Azure Security Center** : Aucune

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2 : Superviser et journaliser la configuration et le trafic des réseaux virtuels, des sous-réseaux et des interfaces réseau

**Aide** : Si vous exécutez des applications logiques dans un environnement de service d’intégration (ISE) qui utilise un point d’accès externe, vous pouvez utiliser un groupe de sécurité réseau (NSG) pour réduire le risque d’exfiltration de données. Activez les journaux de flux NSG et transférez-les vers un compte de stockage Azure pour l'audit du trafic. Vous pouvez aussi envoyer ces journaux dans un espace de travail Log Analytics et utiliser Traffic Analytics pour obtenir des insights sur le flux de trafic dans votre cloud Azure. Parmi les avantages de Traffic Analytics figure la possibilité de visualiser l’activité réseau et d’identifier les zones réactives, d’identifier les menaces de sécurité, de comprendre les modèles de flux de trafic et de repérer les mauvaises configurations du réseau.

- [Comprendre comment accéder au point de terminaison de l’environnement ISE](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment-overview#ise-endpoint-access)

- [Guide pratique pour activer les journaux de flux NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Guide pratique pour activer et utiliser Traffic Analytics](../network-watcher/traffic-analytics.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4 : Refuser les communications avec des adresses IP connues comme étant malveillantes

**Aide** : Si votre application logique utilise un déclencheur basé sur une requête, qui reçoit des requêtes ou appels entrants, comme le déclencheur Requête ou Webhook, vous pouvez restreindre l’accès de sorte que seuls les clients autorisés puissent démarrer votre application logique.

Si vous exécutez des applications logiques dans un environnement de service d’intégration (ISE), activez la protection DDoS standard sur le réseau virtuel associé à votre ISE pour vous protéger contre les attaques DDoS. Utilisez la fonctionnalité de renseignement sur les menaces intégrée à Azure Security Center pour refuser les communications avec des adresses IP Internet connues comme étant malveillantes ou inutilisées.

Déployez le Pare-feu Azure à chaque extrémité du réseau de l'organisation en activant la fonctionnalité de renseignement sur les menaces et en la configurant sur « Alerter et refuser » pour vous protéger de tout trafic réseau malveillant.

Utilisez la fonctionnalité d'accès réseau juste-à-temps d'Azure Security Center pour configurer les groupes de sécurité réseau (NSG) et limiter l'exposition des points de terminaison aux adresses IP approuvées pendant une période limitée.

Utilisez la fonctionnalité de renforcement du réseau adaptatif d'Azure Security Center pour recommander des configurations NSG qui limitent les ports et les adresses IP sources en fonction du trafic réel et du renseignement sur les menaces.

- [Guide pratique pour sécuriser les appels entrants à Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#secure-inbound-requests)

- [Guide pratique pour limiter les adresses IP entrantes](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#restrict-inbound-ip-addresses)

- [Guide pratique pour configurer la protection DDoS](../ddos-protection/manage-ddos-protection.md)

- [Guide pratique pour déployer le Pare-feu Azure](../firewall/tutorial-firewall-deploy-portal.md)

- [Présentation de la fonctionnalité Threat Intelligence intégrée à Azure Security Center](../security-center/azure-defender.md)

- [Présentation de la fonctionnalité de renforcement du réseau adaptatif d’Azure Security Center](../security-center/security-center-adaptive-network-hardening.md)

- [Présentation de la fonctionnalité de contrôle d’accès réseau juste-à-temps d’Azure Security Center](../security-center/security-center-just-in-time.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="15-record-network-packets"></a>1.5 : Enregistrer les paquets réseau

**Aide** : Si vous exécutez des applications logiques dans un environnement de service d’intégration (ISE) qui utilise un point d’accès externe, vous pouvez utiliser un groupe de sécurité réseau (NSG) pour réduire le risque d’exfiltration de données. Activez les journaux de flux NSG et transférez-les vers un compte de stockage Azure pour l'audit du trafic. Vous pouvez aussi envoyer ces journaux dans un espace de travail Log Analytics et utiliser Traffic Analytics pour obtenir des insights sur le flux de trafic dans votre cloud Azure. Parmi les avantages de Traffic Analytics figure la possibilité de visualiser l’activité réseau et d’identifier les zones réactives, d’identifier les menaces de sécurité, de comprendre les modèles de flux de trafic et de repérer les mauvaises configurations du réseau.

Pour fournir une protection et des informations supplémentaires sur le trafic réseau, vous pouvez consulter les journaux d’accès, qui sont générés uniquement si vous les avez activés sur chaque instance de Application Gateway. Vous pouvez utiliser ce journal pour voir les modèles d’accès Application Gateway et analyser les informations importantes. Ceci comprend l’adresse IP de l’appelant, l’URL demandée, la latence de réponse, le code de retour et les octets d’entrée et de sortie.

Dans le cas contraire, vous pouvez tirer parti d’une solution tierce de la place de marché pour répondre à ces exigences.

- [Comprendre comment accéder au point de terminaison de l’environnement ISE](https://docs.microsoft.com/azure/logic-apps/connect-virtual-network-vnet-isolated-environment-overview#ise-endpoint-access)

- [Guide pratique pour activer les journaux de flux NSG](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Guide pratique pour activer et utiliser Traffic Analytics](../network-watcher/traffic-analytics.md)

- [Intégrer le service Gestion des API dans un réseau virtuel interne avec Application Gateway](../api-management/api-management-howto-integrate-internal-vnet-appgateway.md)

- [Guide pratique pour comprendre les journaux d’accès WAF](https://docs.microsoft.com/azure/web-application-firewall/ag/web-application-firewall-logs#access-log)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6 : Déployer des systèmes de détection et de prévention des intrusions basés sur le réseau (IDS/IPS)

**Aide** : Sélectionnez une offre de la place de marché Azure qui prend en charge les fonctionnalités IDS/IPS avec des fonctionnalités d’inspection de charge utile.  Si la détection et/ou la prévention des intrusions basées sur l’inspection de la charge utile ne font pas partie des exigences, vous pouvez utiliser le Pare-feu Azure avec Threat Intelligence. Le filtrage basé sur le renseignement sur les menaces du Pare-feu Azure peut générer des alertes et refuser le trafic depuis ou vers des adresses IP et des domaines malveillants connus. Ces adresses IP et domaines proviennent du flux Microsoft Threat Intelligence.

Déployez la solution de pare-feu de votre choix dans les limites réseau de votre organisation pour détecter et/ou refuser le trafic malveillant.

- [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/?term=Firewall)

- [Guide pratique pour déployer le Pare-feu Azure](../firewall/tutorial-firewall-deploy-portal.md)

- [Guide pratique pour configurer des alertes avec le Pare-feu Azure](../firewall/threat-intel.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : aucune

### <a name="17-manage-traffic-to-web-applications"></a>1.7 : Gérer le trafic à destination des applications web

**Aide** : Si vous exécutez des applications logiques dans un environnement d’intégration de service (ISE), déployez Application Gateway d’Azure.

- [Intégrer le service Gestion des API dans un réseau virtuel interne avec Application Gateway](../api-management/api-management-howto-integrate-internal-vnet-appgateway.md)

- [Guide pratique pour configurer Application Gateway de façon à utiliser le protocole HTTPS](../application-gateway/create-ssl-portal.md) 

- [Présentation de l’équilibrage de charge de niveau 7 avec les passerelles d’applications web Azure](../application-gateway/overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8 : Réduire la complexité et les frais administratifs liés aux règles de sécurité réseau

**Aide** : Pour les ressources qui doivent accéder à vos instances Azure Logic Apps, utilisez des étiquettes de service de réseau virtuel afin de définir des contrôles d’accès réseau sur des groupes de sécurité réseau ou le pare-feu Azure. Vous pouvez utiliser des balises de service à la place des adresses IP spécifiques lors de la création de règles de sécurité. En spécifiant le nom de la balise de service (par exemple, LogicApps, LogicAppsManagement) dans le champ Source ou Destination approprié d'une règle, vous pouvez autoriser ou refuser le trafic pour le service correspondant. Microsoft gère les préfixes d’adresse englobés par la balise de service et met à jour automatiquement la balise de service quand les adresses changent.

- [Pour plus d’informations sur l’utilisation des étiquettes de service](../virtual-network/service-tags-overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9 : Gérer les configurations de sécurité standard pour les périphériques réseau

**Conseils** : Définissez et implémentez des configurations de sécurité standard pour les ressources réseau associées à vos instances Azure Logic Apps avec Azure Policy. Utilisez des alias Azure Policy dans les espaces de noms « Microsoft.Logic » et « Microsoft. Network » afin de créer des stratégies personnalisées pour auditer ou appliquer la configuration réseau de vos instances Azure Logic Apps. Vous pouvez également utiliser des définitions de stratégie intégrée, par exemple :

- Les journaux de diagnostic dans Logic Apps doivent être activés.

- DDoS Protection Standard doit être activé

Vous pouvez également utiliser Azure Blueprints pour simplifier les déploiements Azure à grande échelle en regroupant les artefacts d’environnement clés, tels que les modèles Resource Manager, le contrôle d’accès en fonction du rôle Azure (Azure RBAC) et les stratégies au sein d’une seule définition de blueprint. Appliquez facilement le blueprint aux nouveaux abonnements et environnements, et ajustez le contrôle et la gestion par le biais du versioning.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Guide pratique pour créer un blueprint Azure](../governance/blueprints/create-blueprint-portal.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="110-document-traffic-configuration-rules"></a>1.10 : Règles de configuration du trafic de documents

**Aide** : Concernant les règles NSG individuelles, utilisez le champ « Description » afin de spécifier le besoin métier et/ou la durée (etc.) pour toutes les règles qui autorisent le trafic vers/depuis un réseau.

Utilisez l’une des définitions de stratégie Azure intégrée en lien avec l’étiquetage comme « Exiger une étiquette et sa valeur » pour vous assurer que toutes les ressources créées sont étiquetées et être informé de l’existence de ressources non étiquetées.

Vous pouvez utiliser Azure PowerShell ou Azure CLI pour rechercher des ressources ou effectuer des actions sur des ressources en fonction de leurs étiquettes.

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

- [Guide pratique pour créer un réseau virtuel](../virtual-network/quick-create-portal.md)

- [Guide pratique pour créer un groupe NSG avec une configuration de sécurité](../virtual-network/tutorial-filter-network-traffic.md)

- [Liste des définitions de Azure Policy pour Logic Apps](policy-reference.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11 : Utiliser des outils automatisés pour superviser les configurations des ressources réseau et détecter les modifications

**Conseils** : Utilisez le journal d’activité Azure pour surveiller les configurations des ressources réseau et détecter les modifications de celles-ci associées à vos instances Azure Logic Apps. Créez des alertes dans Azure Monitor, qui se déclenchent lors de la modification de ressources réseau critiques.

- [Guide pratique pour consulter et récupérer les événements du journal d’activité Azure](/azure/azure-monitor/platform/activity-log#view-the-activity-log)

- [Guide pratique pour créer des alertes dans Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="logging-and-monitoring"></a>Journalisation et supervision

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : journalisation et supervision](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2 : Configurer la gestion des journaux de sécurité centrale

**Aide** : Pour obtenir des informations de débogage plus détaillées sur vos applications logiques lors de l'exécution, vous pouvez configurer et utiliser les journaux Azure Monitor afin d'enregistrer et de stocker les informations relatives aux données et événements d'exécution, comme les événements de déclenchement, les événements d'exécution et les événements d'action, dans un espace de travail Log Analytics. Azure Monitor vous permet de superviser vos environnements cloud et locaux afin que vous puissiez assurer plus facilement leur disponibilité et leurs performances. Les journaux Azure Monitor vous permettent de créer des requêtes de journal afin de collecter et de visualiser ces informations. Vous pouvez également utiliser ces données de diagnostic avec d'autres services Azure, tels que Stockage Azure et Azure Event Hubs.

Vous pouvez également activer et intégrer les données dans Azure Sentinel ou une solution SIEM tierce. 

- [Guide pratique pour activer les paramètres de diagnostic du journal d’activité Azure](/azure/azure-monitor/platform/activity-log)

- [Guide pratique pour configurer les journaux d’activité Azure Monitor et collecter des données de diagnostic pour Azure Logic Apps](monitor-logic-apps-log-analytics.md)

- [Guide pratique pour intégrer Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3 : Activer la journalisation d’audit pour les ressources Azure

**Aide** : Pour obtenir des informations de débogage plus détaillées sur vos applications logiques lors de l'exécution, vous pouvez configurer et utiliser les journaux Azure Monitor afin d'enregistrer et de stocker les informations relatives aux données et événements d'exécution, comme les événements de déclenchement, les événements d'exécution et les événements d'action, dans un espace de travail Log Analytics. Azure Monitor vous permet de superviser vos environnements cloud et locaux afin que vous puissiez assurer plus facilement leur disponibilité et leurs performances. Les journaux Azure Monitor vous permettent de créer des requêtes de journal afin de collecter et de visualiser ces informations. Vous pouvez également utiliser ces données de diagnostic avec d'autres services Azure, tels que Stockage Azure et Azure Event Hubs.

Vous pouvez également activer et intégrer les données dans Azure Sentinel ou une solution SIEM tierce. 

- [Guide pratique pour activer les paramètres de diagnostic du journal d’activité Azure](/azure/azure-monitor/platform/activity-log)

- [Guide pratique pour configurer les journaux d’activité Azure Monitor et collecter des données de diagnostic pour Azure Logic Apps](monitor-logic-apps-log-analytics.md)

- [Guide pratique pour intégrer Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilité** : Customer

**Supervision d’Azure Security Center** : le [Benchmark de sécurité Azure](/azure/governance/policy/samples/azure-security-benchmark) est l’initiative de stratégie par défaut pour Security Center et constitue la base des [recommandations de Security Center](/azure/security-center/security-center-recommendations). Les définitions Azure Policy associées à ce contrôle sont activées automatiquement par Security Center. Un plan [Azure Defender](/azure/security-center/azure-defender) pour les services associés peut être nécessaire pour les alertes liées à ce contrôle.

**Définitions intégrées à Azure Policy – Microsoft.Logic** :

[!INCLUDE [Resource Policy for Microsoft.Logic 2.3](../../includes/policy/standards/asb/rp-controls/microsoft.logic-2-3.md)]

### <a name="25-configure-security-log-storage-retention"></a>2.5 : Configurer la conservation du stockage des journaux de sécurité

**Aide** : Après avoir créé et exécuté une application logique, vous pouvez vérifier son état d’exécution, son historique des exécutions, son historique des déclencheurs et ses performances. Pour une surveillance des événements en temps réel et un débogage enrichi, configurez une journalisation des diagnostics pour votre application logique à l’aide de journaux Azure Monitor. Ce service Azure vous aide à superviser vos environnements cloud et locaux afin pouvoir maintenir plus facilement leur disponibilité et leurs performances. Vous pouvez ensuite rechercher et consulter des événements, tels que des événements de déclencheur, des événements d’exécution et des événements d’action. En stockant ces informations dans des journaux Azure Monitor, vous pouvez créer des requêtes de journal qui vous aident à rechercher et à analyser ces informations. Vous pouvez également utiliser ces données de diagnostic avec d’autres services Azure, tels que Stockage Azure et Azure Event Hubs.

Dans Azure Monitor, définissez la période de rétention des journaux associés à vos instances Azure Logic Apps conformément aux réglementations de conformité de votre organisation.

- [Guide pratique pour superviser l’état d’exécution, examiner l’historique des déclencheurs et configurer des alertes pour Azure Logic Apps](monitor-logic-apps.md)

- [Guide pratique pour définir les paramètres de conservation des journaux](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="26-monitor-and-review-logs"></a>2.6 : Superviser et examiner les journaux

**Aide** : Pour configurer la journalisation de votre application logique, vous pouvez activer Log Analytics au moment de la création de l'application. Pour les applications logiques existantes, vous pouvez aussi installer la solution Logic Apps Management dans votre espace de travail Log Analytics. Cette solution fournit des informations agrégées sur les exécutions de votre application logique et inclut des détails spécifiques comme l'état, la durée d'exécution, l'état de la nouvelle soumission et les ID de corrélation. Ensuite, pour activer la journalisation et créer des requêtes propres à ces informations, configurez les journaux Azure Monitor.

Vous pouvez également activer les paramètres de diagnostic des journaux d’activité Azure et envoyez les journaux à un espace de travail Log Analytics. Exécutez des requêtes dans Log Analytics pour rechercher des termes, identifier des tendances, analyser des modèles et fournir de nombreuses autres informations basées sur les données du journal d’activité qui ont pu être collectées pour Azure Logic Apps.

Vous pouvez également activer et intégrer les données dans Azure Sentinel ou une solution SIEM tierce. 

- [Guide pratique pour configurer les journaux d’activité Azure Monitor et collecter des données de diagnostic pour Azure Logic Apps](monitor-logic-apps-log-analytics.md)

- [Guide pratique pour activer les paramètres de diagnostic du journal d’activité Azure](/azure/azure-monitor/platform/activity-log)

- [Guide pratique pour collecter et analyser des journaux d’activité Azure dans Log Analytics dans Azure Monitor](/azure/azure-monitor/platform/activity-log-collect)

- [Guide pratique pour intégrer Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7 : Activer les alertes d’activité anormale

**Aide** : Utilisez Azure Security Center avec Log Analytics pour superviser les activités anormales détectées dans les journaux de sécurité et les événements et générer des alertes s’y rapportant.

Vous pouvez également activer et intégrer les données dans Azure Sentinel.

- [Guide pratique pour intégrer Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Guide pratique pour gérer les alertes dans Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

- [Guide pratique pour générer une alerte sur des données de journal Log Analytics](/azure/azure-monitor/learn/tutorial-response)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="identity-and-access-control"></a>Contrôle des accès et des identités

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : contrôle des accès et des identités](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1 : Tenir un inventaire des comptes d’administration

**Aide** : Azure Active Directory (Azure AD) comporte des rôles intégrés qui doivent être explicitement attribués et qui peuvent être interrogés. Utilisez le module Azure AD PowerShell pour effectuer des requêtes ad hoc afin de découvrir les comptes membres de groupes d’administration.

Pour accéder facilement à d’autres ressources protégées par Azure AD et authentifier votre identité sans vous connecter, votre application logique peut utiliser une identité managée (anciennement appelée MSI ou Managed Service Identity) à la place d’informations d’identification ou de secrets. Azure gère cette identité pour vous et vous aide à sécuriser vos informations d’identification, car vous n’êtes pas obligé de fournir ni de faire pivoter des secrets.

Chaque point de terminaison de requête pour une application logique dispose d’une partie Signature d’accès partagé (SAP) dans l’URL du point de terminaison. Si vous décidez de partager l’URL d’un point de terminaison pour un déclencheur basé sur des requêtes, vous pouvez générer des URL de rappel qui utilisent des clés et pour lesquelles des dates d’expiration ont été configurées. De cette façon, vous pouvez facilement restaurer les clés ou restreindre l’accès au déclenchement de votre application logique selon une période donnée.

- [Guide pratique pour obtenir un rôle d’annuaire dans Azure AD avec PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [Authentifier l’accès aux ressources Azure avec des identités managées dans Azure Logic Apps](create-managed-service-identity.md)

- [Guide pratique pour obtenir les membres d’un rôle d’annuaire dans Azure AD avec PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

- [Guide pratique pour sécuriser l’accès et les données dans Azure Logic Apps avec SAS](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#sas)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="32-change-default-passwords-where-applicable"></a>3.2 : Modifier les mots de passe par défaut lorsque cela est possible

**Conseils** : Azure Active Directory (Azure AD) et Azure Logic Apps n’ont pas le concept de mots de passe par défaut.

Si vous utilisez l’authentification de base, vous devez spécifier un nom d’utilisateur et un mot de passe. Lors de la création de ces informations d’identification, veillez à configurer un mot de passe fort pour l’authentification.

Si vous utilisez l’infrastructure en tant que code, évitez de stocker les mots de passe dans le code et utilisez plutôt Azure Key Vault pour stocker et récupérer les informations d’identification.

- [Guide pratique pour sécuriser et accéder aux données dans Logic Apps](logic-apps-securing-a-logic-app.md)

- [Guide pratique pour définir et récupérer un secret à partir d’Azure Key Vault](../key-vault/general/quick-create-portal.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="33-use-dedicated-administrative-accounts"></a>3.3 : Utiliser des comptes d’administration dédiés

**Conseils** : Créez des procédures standard autour de l’utilisation de comptes d’administration dédiés. Utilisez la gestion des identités et des accès dans Azure Security Center pour superviser le nombre de comptes d’administration.

En outre, pour vous aider à suivre les comptes d’administration dédiés, vous pouvez utiliser des recommandations d’Azure Security Center ou des stratégies Azure intégrées, telles que les suivantes :

- Plusieurs propriétaires doivent être affectés à votre abonnement
- Les comptes dépréciés disposant d’autorisations de propriétaire doivent être supprimés de votre abonnement
- Les comptes externes disposant d’autorisations de propriétaire doivent être supprimés de votre abonnement

Pour plus d’informations, consultez les références suivantes :

- [Utilisation d’Azure Security Center pour superviser l’identité et l’accès (préversion)](../security-center/security-center-identity-access.md)

- [Utilisation d’Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4 : Utiliser l’authentification unique (SSO) Azure Active Directory

**Aide** : Utilisez une inscription d’application Azure (principal du service) afin de récupérer un jeton qui peut être utilisé pour interagir avec vos coffres Recovery Services via des appels d’API.

Dans le cas de nombreux connecteurs, vous devez également commencer par créer une connexion au service ou système cible et fournir des informations d’identification d’authentification ou d’autres détails de configuration pour être en mesure d’utiliser un déclencheur ou une action dans votre application logique. Par exemple, vous devez autoriser une connexion à un compte Twitter pour accéder aux données ou publier des messages en votre nom.

Dans le cas des connecteurs qui utilisent l’authentification OAuth Azure AD (Azure Active Directory), créer une connexion signifie se connecter au service (tel qu’Office 365, Salesforce ou GitHub), où votre jeton d’accès est chiffré et stocké de manière sécurisée dans un magasin de secrets Azure. D’autres connecteurs, comme FTP et SQL, nécessitent une connexion comprenant des détails de configuration, tels que l’adresse du serveur, le nom d’utilisateur et le mot de passe. Ces informations de configuration de connexion sont également chiffrées et stockées de manière sécurisée.

- [Appels d’API REST Azure](/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

- [Comment inscrire votre application cliente auprès d’Azure AD](/rest/api/azure/#register-your-client-application-with-azure-ad)

- [Informations sur l’API des déclencheurs de flux de travail](/rest/api/logic/workflowtriggers)

- [Comprendre la configuration du connecteur](../connectors/apis-list.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5 : Utiliser l’authentification multifacteur pour tous les accès basés sur Azure Active Directory

**Conseils** : activez l’authentification multifacteur Azure Active Directory (Azure AD), puis suivez les recommandations relatives à la gestion des identités et des accès d’Azure Security Center.

- [Guide pratique pour activer l’authentification multifacteur dans Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Guide pratique pour superviser les identités et les accès dans Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilité** : Customer

**Supervision d’Azure Security Center** : Aucune

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6 : Utiliser des stations de travail sécurisées et gérées par Azure pour les tâches administratives

**Aide** : Utilisez des stations de travail à accès privilégié (PAW) avec l’authentification multifacteur configurée pour vous connecter à des ressources Azure et les configurer.

- [En savoir plus sur les stations de travail à accès privilégié](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Guide pratique pour activer l’authentification multifacteur dans Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7 : Journaliser et générer des alertes en cas d’activités suspectes sur des comptes d’administration

**Aide** : Utilisez Azure Active Directory (Azure AD) Privileged Identity Management (PIM) pour générer des journaux et des alertes quand des activités suspectes ou potentiellement dangereuses se produisent dans l’environnement.

Utilisez également les détections de risque Azure AD pour visualiser les alertes et des rapports sur les comportements à risque des utilisateurs.

- [Déploiement de Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-deployment-plan.md)

- [Présentation des détections de risques Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8 : Gérer les ressources Azure à partir des emplacements approuvés uniquement

**Conseils** : Utilisez des emplacements nommés avec accès conditionnel pour autoriser l’accès au Portail Azure uniquement à partir de regroupements logiques spécifiques de plages d’adresses IP ou de pays/régions.

De plus, chaque point de terminaison de requête pour une application logique dispose d’une partie Signature d’accès partagé (SAP) dans l’URL du point de terminaison. Vous pouvez restreindre votre application logique pour qu’elle accepte uniquement les demandes émanant de certaines adresses IP.

- [Guide pratique pour configurer des emplacements nommés dans Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

- [Comprendre comment limiter les adresses IP entrantes dans Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#restrict-inbound-ip-addresses)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="39-use-azure-active-directory"></a>3.9 : Utiliser Azure Active Directory

**Conseils** : Utilisez Azure Active Directory (Azure AD) comme système central d’authentification et d’autorisation pour vos instances Azure Logic Apps. Azure AD protège les données en utilisant un chiffrement fort pour les données au repos et en transit. De plus, AAD sale, hache et stocke de manière sécurisée les informations d’identification utilisateur.

En cas de prise en charge dans Logic Apps, utilisez une identité managée pour accéder facilement à d’autres ressources protégées par Azure AD et authentifiez-vous avec votre identité sans vous connecter, plutôt que d’utiliser des informations d’identification ou des secrets. Azure gère cette identité pour vous et vous aide à sécuriser vos informations d’identification, car vous n’êtes pas obligé de fournir ni de faire pivoter des secrets.

Azure Logic Apps prend en charge aussi bien les identités managées affectées par le système que les identités managées affectées par l’utilisateur. Votre application logique peut utiliser soit l’identité affectée par le système, soit une seule identité affectée par l’utilisateur, que vous pouvez partager dans un groupe d’applications logiques, mais pas les deux. Actuellement, seuls des actions et déclencheurs intégrés spécifiques prennent en charge les identités managées, et non les connexions ou connecteurs managés, tels que :

- HTTP
- Azure Functions
- Gestion des API Azure
- Azure App Services

Pour plus d’informations, consultez les références suivantes :

- [Création et configuration d’une instance Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

- [Authentifier l’accès aux ressources Azure avec des identités managées dans Azure Logic Apps](create-managed-service-identity.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10 : Examiner et rapprocher régulièrement l’accès utilisateur

**Aide** : Azure Active Directory (Azure AD) fournit des journaux pour vous aider à découvrir les comptes obsolètes. De plus, utilisez les révisions d’accès des identités Azure pour gérer efficacement les appartenances aux groupes, les accès aux applications d’entreprise et les attributions de rôles. L’accès des utilisateurs peut être passé en revue régulièrement pour vérifier que seules les utilisateurs appropriés continuent de bénéficier d’un accès.

- [Présentation des rapports Azure AD](/azure/active-directory/reports-monitoring/)

- [Comment utiliser les révisions d’accès des identités Azure](../active-directory/governance/access-reviews-overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11 : Superviser les tentatives d’accès à des informations d’identification désactivées

**Conseils** : Utilisez Azure Active Directory (Azure AD) comme système central d’authentification et d’autorisation pour vos instances Azure Logic Apps. Azure AD protège les données en utilisant un chiffrement fort pour les données au repos et en transit. De plus, AAD sale, hache et stocke de manière sécurisée les informations d’identification utilisateur.

Vous avez accès aux activités de connexion Azure AD et aux sources des journaux des événements à risque et des audits, ce qui vous permet de les intégrer à Azure Sentinel ou à un outil SIEM tiers.

Vous pouvez simplifier ce processus en créant des paramètres de diagnostic pour les comptes d’utilisateur Azure AD et en envoyant les journaux d’audit et les journaux de connexion à un espace de travail Log Analytics. Vous pouvez configurer les alertes de journal souhaitées dans un espace de travail Log Analytics.

- [Guide pratique pour intégrer des journaux d’activité Azure dans Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

- [Procédure d’intégration d’Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12 : Alerter en cas d’écart de comportement de connexion à un compte

**Aide** : Utilisez les détections de risque et la fonctionnalité Risk and Identity Protection d’Azure Active Directory (Azure AD) pour configurer des réponses automatiques aux actions suspectes détectées liées aux identités d’utilisateur. Vous pouvez également ingérer des données dans Azure Sentinel pour approfondir votre examen.

- [Guide pratique pour afficher les connexions risquées Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Guide pratique pour configurer et activer des stratégies de risque Identity Protection](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Guide pratique pour intégrer Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="data-protection"></a>Protection des données

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : protection des données](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1 : Conserver un inventaire des informations sensibles

**Conseils** : Utilisez des étiquettes pour faciliter le suivi des ressources Azure qui stockent ou traitent des informations sensibles.

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2 : Isoler les systèmes qui stockent ou traitent les informations sensibles

**Aide** : Les connecteurs qui s’exécutent dans le service « global » à plusieurs locataires de Logic Apps sont déployés et gérés par Microsoft. Ces connecteurs fournissent des déclencheurs et actions qui permettent d’accéder à des services cloud et/ou à des systèmes locaux, tels qu’Office 365, Stockage Blob Azure, SQL Server, Dynamics, Salesforce, SharePoint, etc.

Pour les applications logiques qui ont besoin d’un accès direct aux ressources d’un réseau virtuel Azure, vous pouvez créer un environnement d’intégration de service (ISE) dans lequel vous pouvez générer, déployer et exécuter vos applications logiques sur des ressources dédiées. Certains réseaux virtuels Azure utilisent des points de terminaison privés (Azure Private Link) pour fournir un accès aux services PaaS Azure, tels que Stockage Azure, Azure Cosmos DB ou Azure SQL Database, aux services partenaires ou aux services clients hébergés sur Azure. Si vos applications logiques ont besoin d’accéder à des réseaux virtuels qui utilisent des points de terminaison privés, vous devez créer, déployer et exécuter ces applications logiques à l’intérieur d’un environnement ISE.

Lorsque vous créez votre ISE, vous pouvez choisir d’utiliser des points de terminaison d’accès internes ou externes. Votre sélection détermine si les déclencheurs de demande ou de webhook sur les applications logiques dans votre ISE peuvent recevoir des appels en dehors de votre réseau virtuel.

De plus, vous pouvez implémenter une isolation en utilisant des abonnements et groupes d’administration distincts selon les domaines de sécurité (par exemple, le type d’environnement et le niveau de confidentialité des données). Vous pouvez limiter le niveau d’accès à vos ressources Azure demandées par vos applications et environnements d’entreprise. Il est possible de contrôler l’accès aux ressources Azure au moyen du contrôle d’accès en fonction du rôle d’Azure (Azure RBAC).

- [Comprendre les connecteurs pour Logic Apps](../connectors/apis-list.md)

- [Accéder aux ressources Réseau virtuel Microsoft Azure à partir d’Azure Logic Apps à l’aide d’environnements de service d’intégration (ISE)](connect-virtual-network-vnet-isolated-environment-overview.md)

- [Guide pratique pour créer des abonnements Azure supplémentaires](../cost-management-billing/manage/create-subscription.md)

- [Guide pratique pour créer des groupes d’administration](/azure/governance/management-groups/create)

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3. : Surveiller et bloquer le transfert non autorisé d’informations sensibles

**Aide** : Actuellement non disponible ; les fonctionnalités d’identification des données, de classification des données et de protection contre la perte de données ne sont pas encore disponibles pour Azure Logic Apps.

Exploitez une solution tierce issue de la Place de marché Azure sur les périmètres du réseau, qui surveille et bloque les transferts non autorisés d’informations sensibles tout en alertant les professionnels de la sécurité de l’information. 

Microsoft gère l’infrastructure sous-jacente d’Azure Logic Apps, et a implémenté des contrôles stricts pour empêcher la perte ou la divulgation des données client.

- [Présentation de la protection des données client dans Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilité** : Partagé

**Supervision Azure Security Center** : Aucune

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4 : Chiffrer toutes les informations sensibles en transit

**Instructions** : Chiffrer toutes les informations sensibles en transit. Dans Azure Logic Apps, toutes les données liées à l’exécution d’une application logique sont chiffrées pendant le transit à l’aide du protocole TLS (Transport Layer Security), mais aussi au repos. Lorsque vous affichez l’historique des exécutions de votre application logique, Logic Apps authentifie votre accès, et fournit des liens vers les entrées et les sorties des requêtes et des réponses pour chaque exécution. Toutefois, pour les actions qui gèrent des mots de passe, des secrets, des clés ou d’autres informations sensibles, vous devez empêcher les autres utilisateurs d’accéder à ces données. Par exemple, si votre application logique obtient un secret dans Azure Key Vault en vue de l’utiliser lors de l’authentification d’une action HTTP, vous devez empêcher l’affichage de ce secret.

Le déclencheur de demande prend en charge uniquement le protocole TLS (Transport Layer Security) 1.2 pour les requêtes entrantes. Assurez-vous que les clients qui se connectent à vos ressources Azure peuvent négocier TLS 1.2 ou une version ultérieure. Les appels sortants à l’aide du connecteur HTTP prennent en charge le protocole TLS (Transport Layer Security) 1.0, 1.1 et 1.2. 

Suivez les recommandations d’Azure Security Center relatives au chiffrement au repos et au chiffrement en transit, le cas échéant.

- [Accès et données sécurisés dans Azure Logic Apps – Appels entrants à des déclencheurs basés sur des requêtes](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#secure-inbound-requests)

- [Accès et données sécurisés dans Azure Logic Apps – Appels sortants à d’autres services et systèmes](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#secure-outbound-requests)

- [Présentation du chiffrement en transit avec Azure](https://docs.microsoft.com/azure/security/fundamentals/encryption-overview#encryption-of-data-in-transit)

- [Comprendre le chiffrement des données au repos avec Azure](../security/fundamentals/encryption-atrest.md)

- [Configurer des clés gérées par le client afin de chiffrer les données au repos pour les environnements de service d’intégration (ISE) dans Azure Logic Apps](customer-managed-keys-integration-service-environment.md)

**Responsabilité** : Partagé

**Supervision Azure Security Center** : Aucune

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5 : Utiliser un outil de découverte actif pour identifier les données sensibles

**Aide** : Dans Azure Logic Apps, de nombreux déclencheurs et actions ont des paramètres que vous pouvez activer pour sécuriser les entrées, les sorties, ou les deux, en masquant ces données à partir de l’historique des exécutions d’une application logique.

Microsoft gère l’infrastructure sous-jacente d’Azure Logic Apps, et a implémenté des contrôles stricts pour empêcher la perte ou la divulgation des données client.

- [Sécuriser l’accès à l’historique des exécutions](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-run-history-data)

- [Présentation de la protection des données client dans Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilité** : Partagé

**Supervision Azure Security Center** : aucune

### <a name="46-use-azure-rbac-to-control-access-to-resources"></a>4.6 : Utiliser Azure RBAC pour contrôler l’accès aux ressources 

**Conseils** : Vous pouvez autoriser seulement certains utilisateurs ou groupes à exécuter des tâches, telles que la gestion, la modification ou l’affichage des applications logiques. Pour contrôler leurs autorisations, utilisez le Contrôle d’accès en fonction du rôle (Azure RBAC) afin d’attribuer des rôles personnalisés ou intégrés aux membres de votre abonnement Azure :

- Contributeur d’application logique : Permet de gérer des applications logiques, mais pas d’en modifier l’accès.
- Opérateur d’application logique : Permet de lire, d’activer et de désactiver des applications logiques, mais pas de les modifier ni de les mettre à jour.

Pour empêcher la modification ou la suppression de votre application logique, vous pouvez utiliser le Verrouillage de la ressource Azure. Grâce à cette fonctionnalité, vous pouvez empêcher d’autres utilisateurs de modifier ou de supprimer des ressources de production.

- [Sécuriser l’accès aux opérations Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-logic-app-operations)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8 : Chiffrer des informations sensibles au repos

**Aide** : Azure Logic Apps s’appuie sur Stockage Azure pour stocker et chiffrer automatiquement les données au repos. Ce chiffrement protège vos données et vous aide à répondre aux engagements de votre entreprise en matière de sécurité et de conformité. Par défaut, Stockage Azure utilise des clés managées par Microsoft pour chiffrer vos données.

Quand vous créez un environnement de service d’intégration (ISE) pour héberger vos applications logiques et que vous souhaitez davantage de contrôle sur les clés de chiffrement utilisées par Stockage Azure, vous pouvez configurer, utiliser et gérer votre propre clé à l’aide d’Azure Key Vault. Cette fonctionnalité est également appelée « Bring Your Own Key » (BYOK) et votre clé est appelée « clé gérée par le client ».

- [ Chiffrer les données au repos et les environnements de service d’intégration dans Azure Logic Apps](customer-managed-keys-integration-service-environment.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9 : Consigner et alerter les modifications apportées aux ressources Azure critiques

**Aide** : Utilisez Azure Monitor avec le journal d’activité Azure pour créer des alertes quand des changements sont apportés à Logic Apps ainsi qu’à d’autres ressources critiques ou connexes.

- [Guide pratique pour créer des alertes sur les événements du journal d’activité Azure](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="inventory-and-asset-management"></a>Gestion des stocks et des ressources

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Gestion des stocks et des ressources](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1 : Utiliser la solution de détection automatisée des ressources

**Aide** : Utilisez Azure Resource Graph pour interroger/découvrir toutes les ressources (telles que calcul, stockage, réseau, ports et protocoles) dans vos abonnements. Vérifiez les autorisations (lecture) appropriées dans votre locataire et répertoriez tous les abonnements Azure, ainsi que les ressources dans vos abonnements.

Bien que les ressources Azure classiques puissent être découvertes via Resource Graph, il est vivement recommandé de créer et d’utiliser des ressources Azure Resource Manager à l’avenir.

- [Guide pratique pour créer des requêtes avec Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Guide pratique pour afficher ses abonnements Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&amp;preserve-view=true)

- [Présentation d’Azure RBAC](../role-based-access-control/overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="62-maintain-asset-metadata"></a>6.2 : Gérer les métadonnées de ressources

**Conseils** : Appliquez des balises aux ressources Azure en fournissant des métadonnées pour les organiser de façon logique par catégories.

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="63-delete-unauthorized-azure-resources"></a>6.3 : Supprimer des ressources Azure non autorisées

**Aide** : Utilisez des étiquettes, des groupes d’administration et des abonnements distincts, le cas échéant, pour organiser et suivre les ressources Azure. Rapprochez régulièrement l’inventaire et assurez-vous que les ressources non autorisées sont supprimées de l’abonnement en temps utile.

Appliquez également des restrictions quant au type de ressources pouvant être créées dans les abonnements clients, en utilisant Azure Policy avec les définitions de stratégie intégrées suivantes :

- Types de ressources non autorisés
- Types de ressources autorisés

Pour plus d’informations, consultez les références suivantes :

- [Guide pratique pour créer des abonnements Azure supplémentaires](../cost-management-billing/manage/create-subscription.md)

- [Guide pratique pour créer des groupes d’administration](/azure/governance/management-groups/create)

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4 : Définir et tenir un inventaire des ressources Azure approuvées

**Conseils** : Créez un inventaire des ressources Azure (comme les connecteurs) et des logiciels approuvés pour les ressources de calcul en fonction des besoins de votre organisation.

Remarque : En raison des politiques sur les données et la confidentialité de Google, vous pouvez utiliser le connecteur Gmail uniquement avec les services approuvés par Google. Cette situation est susceptible d’évoluer et d’affecter d’autres connecteurs Google à l’avenir.

- [Liste de tous les connecteurs Azure Logic Apps](/connectors/connector-reference/connector-reference-logicapps-connectors)

- [Comprendre les problèmes et les limitations des connecteurs Gmail](/connectors/gmail/#known-issues-and-limitations)

- [Plus d’informations sur la politique de confidentialité Google](../connectors/connectors-google-data-security-privacy-policy.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5 : Analyser les ressources Azure non approuvées

**Aide** : Utilisez Azure Policy pour appliquer des restrictions quant au type de ressources pouvant être créées dans vos abonnements. 

Utilisez Azure Resource Graph pour interroger/découvrir des ressources dans leurs abonnements.  Vérifiez que toutes les ressources Azure présentes dans l’environnement sont approuvées.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Guide pratique pour créer des requêtes avec Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="69-use-only-approved-azure-services"></a>6.9 : Utiliser des services Azure approuvés uniquement

**Conseils** : Appliquez des restrictions quant au type de ressources pouvant être créées dans les abonnements clients, en utilisant Azure Policy avec les définitions intégrées suivantes :
- Types de ressources non autorisés
- Types de ressources autorisés

Pour plus d’informations, consultez les références suivantes :

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Guide pratique pour refuser un type de ressource spécifique avec Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6.10 : Tenir un inventaire des titres de logiciels approuvés

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11 : Limiter la capacité des utilisateurs à interagir avec Azure Resource Manager

**Aide** : Configurez l’accès conditionnel Azure pour limiter la capacité des utilisateurs à interagir avec Azure Resource Manager en configurant « Bloquer l’accès » pour l’application « Gestion Microsoft Azure ».

- [Configuration de l’accès conditionnel pour bloquer l’accès à Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6.12 : Limiter la capacité des utilisateurs à exécuter des scripts dans des ressources de calcul

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6.13 : Séparer physiquement ou logiquement des applications à risque élevé

**Aide** : Les ressources liées à Logic Apps nécessaires à des opérations métier, mais qui peuvent poser un risque élevé pour l’organisation, doivent être isolés sur leur propre machine virtuelle et/ou réseau virtuel et être suffisamment sécurisés à l’aide d’un Pare-feu Azure ou d’un groupe de sécurité réseau.

Les ressources Logic Apps nécessaires aux opérations d’entreprise, mais qui peuvent entraîner des risques plus élevés pour l’organisation, doivent être isolées dès que possible via des groupes de ressources distincts avec des autorisations spécifiques et des limites Azure RBAC.

- [Guide pratique pour créer un réseau virtuel](../virtual-network/quick-create-portal.md) 

- [Guide pratique pour créer un groupe NSG avec une configuration de sécurité](../virtual-network/tutorial-filter-network-traffic.md)

- [Guide pratique pour créer des groupes d’administration](/azure/governance/management-groups/create) 

- [Guide pratique pour sécuriser l’accès à Logic Apps via Azure RBAC](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-logic-app-operations)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="secure-configuration"></a>Configuration sécurisée

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Configuration sécurisée](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1 : Établir des configurations sécurisées pour toutes les ressources Azure

**Aide** : Définissez et implémentez des configurations de sécurité standard pour vos instances Azure Logic Apps avec Azure Policy. Utilisez des alias Azure Policy dans l’espace de noms « Microsoft.Logic » pour créer des stratégies personnalisées d’audit ou d’application de la configuration de vos instances Logic Apps. Par exemple, vous pouvez empêcher d’autres utilisateurs de créer ou d’utiliser des connexions aux ressources pour lesquelles vous souhaitez restreindre l’accès.

Par ailleurs, Azure Resource Manager a la possibilité d’exporter le modèle au format JSON (JavaScript Object Notation), qui doit être examiné pour vérifier que les configurations répondent/dépassent les exigences de sécurité de votre organisation.

Utilisez également des paramètres sécurisés pour protéger les données sensibles et les secrets.

- [Affichage des alias Azure Policy disponibles](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Bloquer les connexions créées par les connecteurs dans Azure Logic Apps](block-connections-connectors.md)

- [Exportation monoressource ou multiressource vers un modèle sur le portail Azure](../azure-resource-manager/templates/export-template-portal.md)

- [Guide pratique pour déployer des modèles Azure Resource Manager pour Azure Logic Apps](logic-apps-deploy-azure-resource-manager-templates.md)

- [Comprendre comment sécuriser les paramètres d’action](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#secure-action-parameters)

- [Suggestions en matière de sécurité pour les paramètres](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-best-practices#security-recommendations-for-parameters)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="72-establish-secure-operating-system-configurations"></a>7.2 : Établir des configurations sécurisées du système d’exploitation

**Aide** : Non applicable. Ces conseils concernent les ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3 : Gérer les configurations de ressources Azure sécurisées

**Conseils** : Utilisez les commandes Azure Policy [refuser] et [déployer s’il n’existe pas] pour appliquer des paramètres sécurisés à vos ressources Azure.

Définissez et implémentez des configurations de sécurité standard pour vos instances Azure Logic Apps avec Azure Policy. Utilisez des alias Azure Policy dans l’espace de noms « Microsoft.Logic » pour créer des stratégies personnalisées d’audit ou d’application de la configuration de vos instances Logic Apps. Par exemple, vous pouvez empêcher d’autres utilisateurs de créer ou d’utiliser des connexions aux ressources pour lesquelles vous souhaitez restreindre l’accès.

Par ailleurs, Azure Resource Manager a la possibilité d’exporter le modèle au format JSON (JavaScript Object Notation), qui doit être examiné pour vérifier que les configurations répondent/dépassent les exigences de sécurité de votre organisation.

Assurez-vous également de sécuriser les données dans l’historique des exécutions à l’aide d’une obfuscation.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Présentation des effets d’Azure Policy](../governance/policy/concepts/effects.md)

- [Bloquer les connexions créées par les connecteurs dans Azure Logic Apps](block-connections-connectors.md)

- [Exportation monoressource ou multiressource vers un modèle sur le portail Azure](../azure-resource-manager/templates/export-template-portal.md)

- [Guide pratique pour déployer des modèles Azure Resource Manager pour Azure Logic Apps](logic-apps-deploy-azure-resource-manager-templates.md)

- [Sécuriser l’accès aux entrées et aux sorties de l’historique des exécutions](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#obfuscate)

- [Sécuriser l’accès aux entrées de paramètres](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#secure-action-parameters)

- [Suggestions en matière de sécurité pour les paramètres](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-best-practices#security-recommendations-for-parameters)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4 : Préserver la sécurité des configurations du système d'exploitation

**Aide** : Non applicable. Ces conseils concernent les ressources de calcul.

**Responsabilité** : Partagé

**Supervision Azure Security Center** : Aucune

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5 : Stocker en toute sécurité la configuration des ressources Azure

**Conseils** : Si vous utilisez des définitions Azure Policy personnalisées, utilisez Azure DevOps ou Azure Repos pour stocker et gérer votre code en toute sécurité.

Par ailleurs, Azure Resource Manager a la possibilité d’exporter le modèle au format JSON (JavaScript Object Notation), qui doit être examiné pour vérifier que les configurations répondent/dépassent les exigences de sécurité de votre organisation.

- [Stocker du code dans Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [Documentation Azure Repos](https://docs.microsoft.com/azure/devops/repos/?view=azure-devops&amp;preserve-view=true)

- [Exportation monoressource ou multiressource vers un modèle sur le portail Azure](../azure-resource-manager/templates/export-template-portal.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="76-securely-store-custom-operating-system-images"></a>7.6 Stocker en toute sécurité des images de système d’exploitation personnalisées

**Aide** : Non applicable. Ces conseils concernent les ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7 : Déployer des outils de gestion de la configuration pour les ressources Azure

**Aide** : Utilisez les définitions Azure Policy intégrées ainsi que les alias Azure Policy dans l’espace de noms « Microsoft.Logic » pour créer des stratégies personnalisées d’alerte, d’audit ou d’application de configurations système. Utilisez des alias Azure Policy pour créer des stratégies personnalisées afin d’auditer ou d’appliquer la configuration réseau de vos ressources Azure. En outre, développez un processus et un pipeline pour la gestion des exceptions de stratégie.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8 : Déployer des outils de gestion de la configuration pour les systèmes d'exploitation

**Aide** : Non applicable. Ces conseils concernent les ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9 : Mettre en place une supervision automatisée de la configuration pour les ressources Azure

**Aide** : Utilisez les définitions Azure Policy intégrées ainsi que les alias Azure Policy dans l’espace de noms « Microsoft.Logic » pour créer des stratégies personnalisées d’alerte, d’audit ou d’application de configurations système. Utilisez une stratégie Azure Policy [auditer], [refuser] et [déployer s’il n’existe pas] afin d’appliquer automatiquement des configurations pour vos ressources Azure.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10 : Implémenter la surveillance de la configuration automatique pour les systèmes d’exploitation

**Aide** : Non applicable. Ces conseils concernent les ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="711-manage-azure-secrets-securely"></a>7.11 : Gérer les secrets Azure en toute sécurité

**Aide** : Sécurisez les entrées et les sorties de l’historique des exécutions d’une application logique à l’aide de l’obfuscation. Si vous effectuez des déploiements dans différents environnements, vous pouvez paramétrer les valeurs de votre définition de workflow dans l’application logique qui varient selon l’environnement. De cette façon, vous pouvez éviter les données codées en dur en utilisant un modèle Azure Resource Manager pour déployer votre application logique, protéger les données sensibles en définissant des paramètres sécurisés et transmettre ces données sous forme d’entrées distinctes par le biais des paramètres du modèle à l’aide d’un fichier de paramètres. Vous pouvez utiliser Key Vault pour stocker des données sensibles et utiliser des paramètres de modèle sécurisés qui récupèrent ces valeurs à partir de Key Vault au moment du déploiement. Vous pouvez ensuite référencer le coffre de clés et les secrets dans votre fichier de paramètres. 

Quand vous créez un environnement de service d’intégration (ISE) pour héberger vos applications logiques et que vous souhaitez davantage de contrôle sur les clés de chiffrement utilisées par Stockage Azure, vous pouvez configurer, utiliser et gérer votre propre clé à l’aide d’Azure Key Vault. Cette fonctionnalité est également appelée « Bring Your Own Key » (BYOK) et votre clé est appelée « clé gérée par le client ».

- [Sécuriser les entrées et les sorties dans l’historique des exécutions dans Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#obfuscate)

- [Suggestions en matière de sécurité pour les paramètres](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-best-practices#security-recommendations-for-parameters)

- [Sécuriser l’accès aux entrées de paramètres dans Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-parameter-inputs)

- [Pour transmettre une valeur de paramètre sécurisée pendant le déploiement avec Azure Key Vault](../azure-resource-manager/templates/key-vault-parameter.md)

- [Configurer des clés gérées par le client afin de chiffrer les données au repos pour les environnements de service d’intégration (ISE) dans Azure Logic Apps](customer-managed-keys-integration-service-environment.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="712-manage-identities-securely-and-automatically"></a>7.12 : Gérer les identités de façon sécurisée et automatique

**Aide** : Pour accéder facilement à d’autres ressources protégées par Azure Active Directory (Azure AD) et authentifier votre identité sans vous connecter, votre application logique peut utiliser une identité managée (anciennement appelée MSI ou Managed Service Identity) à la place d’informations d’identification ou de secrets. Azure gère cette identité pour vous et vous aide à sécuriser vos informations d’identification, car vous n’êtes pas obligé de fournir ni de faire pivoter des secrets.

Actuellement, seuls des actions et déclencheurs intégrés spécifiques prennent en charge les identités managées, et non les connexions ou connecteurs managés, tels que :

- HTTP
- Azure Functions
- Gestion des API Azure
- Azure App Services

Pour plus d’informations, consultez les références suivantes :

- [Guide pratique pour authentifier l’accès aux ressources Azure avec des identités managées dans Azure Logic Apps](create-managed-service-identity.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13 : Éliminer l’exposition involontaire des informations d’identification

**Aide** : Sécurisez les entrées et les sorties de l’historique des exécutions d’une application logique à l’aide de l’obfuscation. Si vous effectuez des déploiements dans différents environnements, vous pouvez paramétrer les valeurs de votre définition de workflow dans l’application logique qui varient selon l’environnement. De cette façon, vous pouvez éviter les données codées en dur en utilisant un modèle Azure Resource Manager pour déployer votre application logique, protéger les données sensibles en définissant des paramètres sécurisés et transmettre ces données sous forme d’entrées distinctes par le biais des paramètres du modèle à l’aide d’un fichier de paramètres. Vous pouvez utiliser Key Vault pour stocker des données sensibles et utiliser des paramètres de modèle sécurisés qui récupèrent ces valeurs à partir de Key Vault au moment du déploiement. Vous pouvez ensuite référencer le coffre de clés et les secrets dans votre fichier de paramètres. 

Vous pouvez aussi exécuter le moteur d’analyse des informations d’identification pour identifier les informations d’identification dans le code. Le moteur d’analyse des informations d’identification encourage également le déplacement des informations d’identification découvertes vers des emplacements plus sécurisés, tels qu’Azure Key Vault. 

- [Sécuriser les entrées et les sorties dans l’historique des exécutions dans Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#obfuscate)

- [Suggestions en matière de sécurité pour les paramètres](https://docs.microsoft.com/azure/azure-resource-manager/templates/template-best-practices#security-recommendations-for-parameters)

- [Sécuriser l’accès aux entrées de paramètres dans Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#access-to-parameter-inputs)

- [Pour transmettre une valeur de paramètre sécurisée pendant le déploiement avec Azure Key Vault](../azure-resource-manager/templates/key-vault-parameter.md)

- [Configurer Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="malware-defense"></a>Défense contre les programmes malveillants

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Défense contre les programmes malveillants](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1 : Utiliser un logiciel anti-programme malveillant géré de manière centralisée

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul. Microsoft Antimalware est activé sur l’hôte sous-jacent qui prend en charge les services Azure (par exemple Azure Logic Apps), mais il ne s’exécute pas sur du contenu client.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2 : Pré-analyser les fichiers à charger sur des ressources Azure non liées au calcul

**Aide** : Microsoft Anti-malware est activé sur l’hôte sous-jacent qui prend en charge les services Azure (par exemple, Sauvegarde Azure), mais il ne s’exécute pas sur votre contenu. 

Pré-analysez les fichiers chargés sur des ressources Azure non liées au calcul, comme App Service, des Data Lake Storage, Stockage Blob, etc. 

Utilisez la détection des menaces pour les services de données dans Azure Security Center pour détecter les programmes malveillants chargés sur les comptes de stockage. 

- [Présentation de Microsoft Anti-malware pour Azure Cloud Services et les machines virtuelles](../security/fundamentals/antimalware.md)

- [Présentation de la détection des menaces pour les services de données d’Azure Security Center](/azure/security-center/threat-protection)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3 : Vérifier que les logiciels et signatures anti-programme malveillant sont mis à jour

**Aide** : Non applicable. Ces conseils concernent les ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="data-recovery"></a>Récupération des données

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : récupération de données](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1 : Garantir des sauvegardes automatiques régulières

**Aide** : Implémentez une solution de reprise d’activité (DR) afin de pouvoir protéger les données, restaurer rapidement les ressources qui prennent en charge les fonctions métier critiques et maintenir l’exécution des opérations afin de préserver la continuité d’activité (BC).

Cette stratégie de récupération d’urgence est axée sur la configuration de votre application logique principale afin qu’elle bascule vers une application logique de secours ou de sauvegarde dans un autre emplacement où Azure Logic Apps est également disponible. Ainsi, si l’application logique principale subit des pertes, des interruptions ou des défaillances, l’application logique secondaire peut prendre le relais. Cette stratégie exige que votre application logique secondaire et les ressources dépendantes soient déjà déployées et prêtes dans l’emplacement secondaire.

En outre, vous devez étendre la définition du workflow sous-jacent de votre application logique dans un modèle de Azure Resource Manager. Ce modèle définit l’infrastructure, les ressources, les paramètres et d’autres informations pour l’approvisionnement et le déploiement de votre application logique.

- [En savoir plus sur la continuité d’activité et reprise d’activité pour Azure Logic Apps](business-continuity-disaster-recovery-guidance.md)

- [Guide pratique pour automatiser le déploiement pour le service Azure Logic Apps à l’aide de modèles Resource Manager](logic-apps-azure-resource-manager-templates-overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2 : Effectuer des sauvegardes complètes du système et sauvegarder les clés gérées par le client

**Aide** : Implémentez une solution de reprise d’activité (DR) afin de pouvoir protéger les données, restaurer rapidement les ressources qui prennent en charge les fonctions métier critiques et maintenir l’exécution des opérations afin de préserver la continuité d’activité (BC).

Cette stratégie de récupération d’urgence est axée sur la configuration de votre application logique principale afin qu’elle bascule vers une application logique de secours ou de sauvegarde dans un autre emplacement où Azure Logic Apps est également disponible. Ainsi, si l’application logique principale subit des pertes, des interruptions ou des défaillances, l’application logique secondaire peut prendre le relais. Cette stratégie exige que votre application logique secondaire et les ressources dépendantes soient déjà déployées et prêtes dans l’emplacement secondaire.

En outre, vous devez étendre la définition du workflow sous-jacent de votre application logique dans un modèle de Azure Resource Manager. Ce modèle définit l’infrastructure, les ressources, les paramètres et d’autres informations pour l’approvisionnement et le déploiement de votre application logique.

Chaque point de terminaison de requête pour une application logique dispose d’une partie Signature d’accès partagé (SAP) dans l’URL du point de terminaison. Si vous utilisez Azure Key Vault pour stocker vos secrets, assurez-vous de sauvegarder régulièrement vos clés et vos URL.

- [En savoir plus sur la continuité d’activité et reprise d’activité pour Azure Logic Apps](business-continuity-disaster-recovery-guidance.md)

- [Automatiser le déploiement pour le service Azure Logic Apps à l’aide de modèles Resource Manager](logic-apps-azure-resource-manager-templates-overview.md)

- [Sécuriser l’accès et les données dans Azure Logic Apps avec SAS](https://docs.microsoft.com/azure/logic-apps/logic-apps-securing-a-logic-app#sas)

- [Comment sauvegarder des clés Key Vault](/powershell/module/azurerm.keyvault/backup-azurekeyvaultkey)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3 : Valider toutes les sauvegardes, y compris les clés gérées par le client

**Conseils** : Votre stratégie de récupération d’urgence doit être axée sur la configuration de votre application logique principale afin qu’elle bascule vers une application logique de secours ou de sauvegarde dans un autre emplacement où Azure Logic Apps est également disponible. Ainsi, si l’application logique principale subit des pertes, des interruptions ou des défaillances, l’application logique secondaire peut prendre le relais. Cette stratégie exige que votre application logique secondaire et les ressources dépendantes soient déjà déployées et prêtes dans l’emplacement secondaire.

Testez la restauration des clés gérées par le client sauvegardées. Notez que cela s’applique uniquement aux Logic Apps s’exécutant dans les environnements d’intégration de service (ISE).

- [En savoir plus sur la continuité d’activité et reprise d’activité pour Azure Logic Apps](business-continuity-disaster-recovery-guidance.md)

- [Configurer des clés gérées par le client afin de chiffrer les données au repos pour les environnements de service d’intégration (ISE) dans Azure Logic Apps](customer-managed-keys-integration-service-environment.md)

- [Guide pratique pour restaurer des clés de coffre de clés dans Azure](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4 : Garantir la protection des sauvegardes et des clés gérées par le client

**Conseils** : Votre stratégie de récupération d’urgence doit être axée sur la configuration de votre application logique principale afin qu’elle bascule vers une application logique de secours ou de sauvegarde dans un autre emplacement où Azure Logic Apps est également disponible. Ainsi, si l’application logique principale subit des pertes, des interruptions ou des défaillances, l’application logique secondaire peut prendre le relais. Cette stratégie exige que votre application logique secondaire et les ressources dépendantes soient déjà déployées et prêtes dans l’emplacement secondaire. 

Protégez les clés sauvegardées gérées par le client. Notez que cela s’applique uniquement aux Logic Apps s’exécutant dans les environnements d’intégration de service (ISE).

Activez la suppression réversible et la protection contre la purge dans Key Vault pour protéger les clés contre une suppression accidentelle ou malveillante.

- [En savoir plus sur la continuité d’activité et reprise d’activité pour Azure Logic Apps](business-continuity-disaster-recovery-guidance.md)

- [Configurer des clés gérées par le client afin de chiffrer les données au repos pour les environnements de service d’intégration (ISE) dans Azure Logic Apps](customer-managed-keys-integration-service-environment.md)

- [Guide pratique pour activer la suppression réversible et la protection contre la purge dans Key Vault](https://docs.microsoft.com/azure/storage/blobs/soft-delete-overview?tabs=azure-portal)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="incident-response"></a>Réponse aux incidents

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : réponse aux incidents](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1 : Créer un guide de réponse aux incidents

**Conseils** : Créez un guide de réponse aux incidents pour votre organisation. Assurez-vous qu’il existe des plans de réponse aux incidents écrits qui définissent tous les rôles du personnel, ainsi que les phases de gestion des incidents, depuis la détection jusqu’à la revue une fois l’incident terminé. 

- [Aide sur la création de votre propre processus de réponse aux incidents de sécurité](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomie d’un incident dans le centre de réponse aux incidents de sécurité Microsoft](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Tirer parti du guide de gestion des incidents de sécurité informatique du NIST pour faciliter la création de votre propre plan de réponse aux incidents](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10.2 : Créer une procédure de notation et de classement des incidents

**Conseils** : Security Center attribue un niveau de gravité à chaque alerte pour vous aider à hiérarchiser celles devant être examinées en premier. La gravité dépend de la confiance que Security Center accorde à la recherche ou à la métrique utilisées pour émettre l’alerte, ainsi qu’à la conviction quand à l’existence d’une intention malveillante derrière l’activité à l’origine de l’alerte. 

En outre, marquez clairement les abonnements (par ex. production, non-production) à l’aide d’étiquettes et créez un système de nommage pour identifier et classer clairement les ressources Azure, en particulier celles qui traitent des données sensibles.  Il vous incombe de hiérarchiser le traitement des alertes en fonction de la criticité des ressources et de l’environnement Azure où l’incident s’est produit.

- [Alertes de sécurité dans le Centre de sécurité Azure](../security-center/security-center-alerts-overview.md)

- [Organisation des ressources Azure à l’aide de catégories](../azure-resource-manager/management/tag-resources.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="103-test-security-response-procedures"></a>10.3 : Tester les procédures de réponse de sécurité

**Conseils** : Effectuez des exercices pour tester les capacités de réponse aux incidents de vos systèmes à intervalles réguliers, afin de protéger vos ressources Azure. Identifiez les points faibles et les lacunes, et révisez le plan en fonction des besoins.

- [Publication du NIST : Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities](https://csrc.nist.gov/publications/detail/sp/800-84/final)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10.4 : Fournir des informations de contact pour les incidents de sécurité et configurer des notifications d’alerte pour les incidents de sécurité

**Instructions** : Microsoft utilisera les informations de contact pour le signalement d’incidents de sécurité pour vous contacter si le Microsoft Security Response Center (MSRC) découvre que vos données ont été consultées de manière illégale ou par un tiers non autorisé. Examinez les incidents après les faits pour vous assurer que les problèmes sont résolus.

- [Comment définir le contact de sécurité d’Azure Security Center](../security-center/security-center-provide-security-contact-details.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10.5 : Intégrer des alertes de sécurité à votre système de réponse aux incidents

**Aide** : Exportez vos alertes et recommandations Azure Security Center en utilisant la fonctionnalité d’exportation continue pour identifier les risques pesant sur les ressources Azure. L’exportation continue vous permet d’exporter les alertes et les recommandations manuellement, ou automatiquement de manière continue. Vous pouvez utiliser le connecteur de données Azure Security Center pour diffuser en continu les alertes vers Azure Sentinel.

- [Comment configurer l’exportation continue](../security-center/continuous-export.md)

- [Comment envoyer des alertes à Azure Sentinel](../sentinel/connect-azure-security-center.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="106-automate-the-response-to-security-alerts"></a>10.6 : Automatiser la réponse aux alertes de sécurité

**Aide** : Utilisez la fonctionnalité d’automatisation de workflow d’Azure Security Center pour déclencher automatiquement des réponses via « Logic Apps » aux alertes et aux recommandations de sécurité afin de protéger vos ressources Azure.

- [Comment configurer l’automatisation des workflows et Logic Apps](../security-center/workflow-automation.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="penetration-tests-and-red-team-exercises"></a>Tests d’intrusion et exercices Red Team

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : tests d’intrusion et exercices Red Team](../security/benchmarks/security-control-penetration-tests-red-team-exercises.md).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11.1 : Procéder régulièrement à des tests d’intrusion des ressources Azure et veiller à corriger tous les problèmes de sécurité critiques détectés

**Aide** : Suivez les règles d’engagement de Microsoft pour que vos tests d’intrusion soient conformes aux stratégies de Microsoft. Utilisez la stratégie et l’exécution de Red Teaming de Microsoft ainsi que les tests d’intrusion de site actif sur l’infrastructure cloud, les services et les applications gérés par Microsoft.

- [Règles d’engagement des tests d’intrusion](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

- [Microsoft Cloud Red Teaming](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Responsabilité** : Partagé

**Supervision Azure Security Center** : Aucune

## <a name="next-steps"></a>Étapes suivantes

- Consultez [Vue d’ensemble d’Azure Security Benchmark V2](/azure/security/benchmarks/overview)
- En savoir plus sur les [bases de référence de la sécurité Azure](/azure/security/benchmarks/security-baselines-overview)
