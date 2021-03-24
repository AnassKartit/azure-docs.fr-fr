---
title: Base de référence de sécurité Azure pour Azure Monitor
description: La base de référence de sécurité Azure Monitor fournit des instructions et des ressources pour la mise en œuvre des recommandations de sécurité spécifiées dans le Benchmark de sécurité Azure.
author: msmbaldwin
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/17/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 39dff9b48fb292d8a1c145ae3e592786ddb027a4
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102198827"
---
# <a name="azure-security-baseline-for-azure-monitor"></a>Base de référence de sécurité Azure pour Azure Monitor

Cette base de référence de sécurité applique les conseils du [Benchmark de sécurité Azure version 1.0](../security/benchmarks/overview-v1.md) à Azure Monitor. Le benchmark de sécurité Azure fournit des recommandations sur la façon dont vous pouvez sécuriser vos solutions cloud sur Azure.
Le contenu est regroupé par les **contrôles de sécurité** définis par le Benchmark de sécurité Azure et les conseils associés applicables à Azure Monitor. **Les contrôles** non applicables à Azure Monitor ont été exclus. 

 
Pour voir comment Azure Monitor est entièrement mappé au Benchmark de sécurité Azure, consultez le [fichier de mappage complet de la base de référence de sécurité Azure Monitor](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

## <a name="network-security"></a>Sécurité réseau

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : sécurité réseau](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1 : Protéger les ressources Azure au sein des réseaux virtuels

**Conseils** : Activez Azure Private Link pour autoriser l’accès aux services PaaS Azure (par exemple, Azure Monitor), ainsi qu’aux services de partenaires ou de clients hébergés par Azure sur un point de terminaison privé de votre réseau virtuel. Le trafic entre votre réseau virtuel et le service transite par le réseau principal de Microsoft, éliminant ainsi toute exposition à l’Internet public.

Pour permettre au trafic d’atteindre Azure Monitor, utilisez les balises de service « AzureMonitor » pour autoriser les trafic entrant et sortant via des groupes de sécurité réseau. Pour permettre au trafic de test de surveillance de la disponibilité d’atteindre Azure Monitor, utilisez la balise de service « ApplicationInsightsAvailability » pour tout le trafic entrant via des groupes de sécurité réseau. Pour autoriser les notifications d’alerte à atteindre des points de terminaison clients, utilisez la balise de service « ActionGroup » pour autoriser le trafic entrant via les groupes de sécurité réseau.

Les règles de réseau virtuel permettent à Azure Monitor de n’accepter que les communications provenant de sous-réseaux sélectionnés à l’intérieur d’un réseau virtuel.

Utilisez la passerelle Log Analytics pour envoyer des données à un espace de travail Log Analytics dans Azure Monitor pour le compte des ordinateurs qui ne peuvent pas se connecter directement à Internet, ce qui empêche les ordinateurs de se connecter à Internet. 

- [Guide pratique pour configurer Azure Private Link pour Azure Monitor](/azure/azure-monitor/platform/private-link-security)

- [Connecter des ordinateurs sans accès Internet en utilisant la passerelle Log Analytics dans Azure Monitor](/azure/azure-monitor/platform/gateway)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2 : Superviser et journaliser la configuration et le trafic des réseaux virtuels, des sous-réseaux et des interfaces réseau

**Conseil** : Azure Monitor étant un service principal, il ne prend pas en charge le déploiement direct sur un réseau virtuel. Son infrastructure sous-jacente est gérée par Microsoft. Toutefois, pour les ressources qui établissent des connexions réseau à l’offre Azure Monitor, sécurisez leur réseau à l’aide d’un groupe de sécurité réseau. Activez les journaux de flux de groupe de sécurité réseau et envoyez les journaux dans un compte de stockage pour auditer le trafic. Vous pouvez également envoyer des journaux à un espace de travail Log Analytics et utiliser Traffic Analytics pour obtenir des insights sur le flux de trafic dans votre cloud Azure. Parmi les avantages de Traffic Analytics figure la possibilité de visualiser l’activité réseau et d’identifier les zones réactives, d’identifier les menaces de sécurité, de comprendre les modèles de flux de trafic et de repérer les mauvaises configurations du réseau.

Lorsque vous utilisez Azure Monitor avec Azure Private Link, vous accédez à la journalisation réseau, par exemple, aux « Données traitées par le point de terminaison privé (entrée/sortie) ».

- [Configuration réseau requise pour les agents Azure Monitor](/azure/azure-monitor/platform/log-analytics-agent#network-requirements)

- [Connecter des ordinateurs sans accès Internet en utilisant la passerelle Log Analytics dans Azure Monitor](/azure/azure-monitor/platform/gateway)

- [Tutoriel : journaliser le trafic réseau à destination et en provenance d’une machine virtuelle à l’aide du portail Azure](../network-watcher/network-watcher-nsg-flow-logging-portal.md)

- [Traffic Analytics](../network-watcher/traffic-analytics.md)

- [Présentation de la sécurité réseau assurée par Azure Security Center](../security-center/security-center-network-recommendations.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8 : Réduire la complexité et les frais administratifs liés aux règles de sécurité réseau

**Conseil** : pour permettre au trafic d’atteindre Azure Monitor, utilisez les balises de service « AzureMonitor » pour autoriser les trafic entrant et sortant via des groupes de sécurité réseau. Pour permettre au trafic de test de surveillance de la disponibilité d’atteindre Azure Monitor, utilisez la balise de service « ApplicationInsightsAvailability » pour tout le trafic entrant via des groupes de sécurité réseau. Microsoft gère les préfixes d’adresse englobés par la balise de service et met à jour automatiquement la balise de service quand les adresses changent.

- [Présentation et utilisation des balises de service](../virtual-network/service-tags-overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="110-document-traffic-configuration-rules"></a>1.10 : Règles de configuration du trafic de documents

**Conseil** : Azure Monitor faisant partie des services principaux d’Azure, vous ne pouvez pas le déployer séparément en tant que service. Des composants Azure Monitor, dont l’agent Azure Monitor et le Kit de développement logiciel (SDK) Application Insights, peuvent être déployés avec vos ressources, et cela peut avoir un impact sur la posture de sécurité de ces ressources.

- [Configuration réseau requise pour les agents Azure Monitor](/azure/azure-monitor/platform/log-analytics-agent#network-requirements)

- [Connecter des ordinateurs sans accès Internet en utilisant la passerelle Log Analytics dans Azure Monitor](/azure/azure-monitor/platform/gateway) 

- [Prise en main d’Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview#get-started)

- [Comment configurer des tests web de disponibilité](app/monitor-web-app-availability.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11 : Utiliser des outils automatisés pour superviser les configurations des ressources réseau et détecter les modifications

**Conseil** : utilisez le Journal des activités Azure pour surveiller les configurations des ressources et détecter les changements des ressources réseau associées à Azure Monitor. Créez dans Azure Monitor des alertes qui se déclenchent lors de la modification de ces ressources réseau critiques.

- [Guide pratique pour consulter et récupérer les événements du journal d’activité Azure](/azure/azure-monitor/platform/activity-log#view-the-activity-log)

- [Guide pratique pour créer des alertes dans Azure Monitor](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="logging-and-monitoring"></a>Journalisation et supervision

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : journalisation et supervision](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2 : Configurer la gestion des journaux de sécurité centrale

**Conseil** : Azure Monitor utilise des journaux d’activité pour journaliser les modifications apportées à ses ressources. Vous pouvez exporter ces journaux vers le service Stockage Azure, Event Hub ou un espace de travail Log Analytics. Ingérez des journaux par le biais d’Azure Monitor pour agréger les données de sécurité générées par les appareils de point de terminaison, les ressources réseau et d’autres systèmes de sécurité. Dans Azure Monitor, vous pouvez interroger et analyser les données, ainsi qu’utiliser des comptes de Stockage Azure pour le stockage à long terme et d’archivage des journaux.

Vous pouvez également activer et intégrer les données dans Azure Sentinel ou une solution SIEM tierce.

- [Guide pratique pour collecter des journaux et des métriques de plateforme avec Azure Monitor](/azure/azure-monitor/platform/diagnostic-settings)

- [Guide pratique pour collecter les journaux des hôtes internes des machines virtuelles Azure avec Azure Monitor](/azure/azure-monitor/learn/quick-collect-azurevm)

- [Guide pratique pour intégrer Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Guide pratique pour bien démarrer avec Azure Monitor et l’intégration d’une solution SIEM tierce](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

**Responsabilité** : Customer

**Supervision d’Azure Security Center** : le [Benchmark de sécurité Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) est l’initiative de stratégie par défaut pour Security Center et constitue la base des [recommandations de Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Les définitions Azure Policy associées à ce contrôle sont activées automatiquement par Security Center. Un plan [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) pour les services associés peut être nécessaire pour les alertes liées à ce contrôle.

**Définitions intégrées à Azure Policy - microsoft.insights** :

[!INCLUDE [Resource Policy for microsoft.insights 2.2](../../includes/policy/standards/asb/rp-controls/microsoft.insights-2-2.md)]

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3 : Activer la journalisation d’audit pour les ressources Azure

**Conseils** : Azure Monitor utilise des journaux d’activité. Le journal d’activité automatiquement activé journalise les opérations effectuées sur ressources Azure Monitor en recueillant des informations telles que la personne qui les déclenche, le moment auquel elles ont lieu, leur état et d’autres informations d’audit utiles. 

- [Guide pratique pour collecter des journaux et des métriques de plateforme avec Azure Monitor](/azure/azure-monitor/platform/diagnostic-settings)

- [Présentation de la journalisation et des différents types de journaux dans Azure](/azure/azure-monitor/platform/platform-logs-overview)

**Responsabilité** : Customer

**Supervision d’Azure Security Center** : le [Benchmark de sécurité Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) est l’initiative de stratégie par défaut pour Security Center et constitue la base des [recommandations de Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Les définitions Azure Policy associées à ce contrôle sont activées automatiquement par Security Center. Un plan [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) pour les services associés peut être nécessaire pour les alertes liées à ce contrôle.

**Définitions intégrées à Azure Policy - microsoft.insights** :

[!INCLUDE [Resource Policy for microsoft.insights 2.3](../../includes/policy/standards/asb/rp-controls/microsoft.insights-2-3.md)]

### <a name="25-configure-security-log-storage-retention"></a>2.5 : Configurer la conservation du stockage des journaux de sécurité

**Conseils** : Dans Azure Monitor, définissez la période de rétention de votre espace de travail Log Analytics en fonction des réglementations de conformité de votre organisation. Utilisez des comptes de Stockage Azure pour le stockage à long terme et l’archivage de vos journaux.

- [Modification de la période de conservation des données dans Log Analytics](/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

- [Guide pratique pour configurer la stratégie de conservation des journaux de compte de Stockage Azure](/azure/storage/common/storage-monitor-storage-account#configure-logging)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="26-monitor-and-review-logs"></a>2.6 : Superviser et examiner les journaux

**Conseil** : analysez et supervisez les journaux pour détecter les comportements anormaux et examinez régulièrement les résultats. Utilisez Azure Monitor et un espace de travail Log Analytics pour examiner les journaux et effectuer des requêtes sur leurs données.

Vous pouvez également activer et intégrer les données dans Azure Sentinel ou une solution SIEM tierce.

- [Guide pratique pour intégrer Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Bien démarrer avec les requêtes Log Analytics](/azure/azure-monitor/log-query/log-analytics-tutorial)

- [Guide pratique pour effectuer des requêtes personnalisées dans Azure Monitor](/azure/azure-monitor/log-query/get-started-queries)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7 : Activer les alertes d’activité anormale

**Conseil** : utilisez Azure Security Center avec un espace de travail Log Analytics pour superviser les activités anormales détectées dans les journaux de sécurité et les événements et générer des alertes s’y rapportant. Vous pouvez également activer et intégrer les données dans Azure Sentinel.

- [Guide pratique pour intégrer Azure Sentinel](../sentinel/quickstart-onboard.md)

- [Guide pratique pour gérer les alertes dans Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md)

- [Guide pratique pour générer une alerte sur des données de journal Log Analytics](/azure/azure-monitor/learn/tutorial-response)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="identity-and-access-control"></a>Contrôle des accès et des identités

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : contrôle des accès et des identités](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1 : Tenir un inventaire des comptes d’administration

**Conseils** : Le contrôle d’accès en fonction du rôle Azure (Azure RBAC) vous permet de gérer l’accès aux ressources Azure par le biais d’attributions de rôles. Vous pouvez affecter ces rôles à des utilisateurs, regrouper des principaux de service et des identités managées. Les rôles intégrés prédéfinis pour certaines ressources peuvent être inventoriés ou interrogés par le biais d’outils tels que Azure CLI, Azure PowerShell ou le Portail Azure.

- [Guide pratique pour obtenir un rôle d’annuaire dans Azure Active Directory (Azure AD) avec PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrole?view=azureadps-2.0&amp;preserve-view=true)

- [Guide pratique pour obtenir les membres d’un rôle d’annuaire dans Azure AD avec PowerShell](https://docs.microsoft.com/powershell/module/azuread/get-azureaddirectoryrolemember?view=azureadps-2.0&amp;preserve-view=true)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="33-use-dedicated-administrative-accounts"></a>3.3 : Utiliser des comptes d’administration dédiés

**Conseils** : Créez des procédures standard autour de l’utilisation de comptes d’administration dédiés. Utilisez la gestion des identités et des accès dans Azure Security Center pour superviser le nombre de comptes d’administration.

Vous pouvez aussi activer un accès Juste-à-temps/Juste assez à l’aide des rôles privilégiés d’Azure Active Directory (Azure AD) Privileged Identity Management pour les services Microsoft et Azure Resource Manager.

- [Vue d’ensemble d’Azure AD Privileged Identity Management (PIM)](../active-directory/privileged-identity-management/pim-configure.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4 : Utiliser l’authentification unique (SSO) Azure Active Directory

**Conseil** : dans la mesure du possible, utilisez l’authentification unique (SSO) d’Azure Active Directory (Azure AD) au lieu de configurer des informations d’identification autonomes individuelles par service. Suivez les recommandations liées à la gestion des identités et des accès dans Azure Security Center.

- [Présentation de l’authentification SSO avec Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5 : Utiliser l’authentification multifacteur pour tous les accès basés sur Azure Active Directory

**Conseil** : activez l’authentification multifacteur Azure Active Directory (Azure AD), puis suivez les recommandations relatives à l’identité et à l’accès Azure Security Center.

- [Guide pratique pour activer l’authentification multifacteur dans Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Guide pratique pour superviser les identités et les accès dans Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3.6 : Utiliser des ordinateurs dédiés (stations de travail avec accès privilégié) pour toutes les tâches administratives

**Aide** : Utilisez une station de travail sécurisée et managée par Azure (également appelée station de travail à accès privilégié) pour les tâches administratives qui requièrent des privilèges élevés.

- [Comprendre les stations de travail sécurisées gérées par Azure](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Guide pratique pour activer l’authentification multifacteur Azure Active Directory (Azure AD)](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7 : Journaliser et générer des alertes en cas d’activités suspectes sur des comptes d’administration

**Conseils** : Utilisez les rapports de sécurité Azure Active Directory (Azure AD) et la supervision pour détecter les activités suspectes ou potentiellement dangereuses qui se produisent dans l’environnement. Utiliser Azure Security Center pour superviser les activités liées aux identités et aux accès.

- [Guide pratique pour identifier les utilisateurs Azure AD pour lesquels une activité à risque a été signalée](../active-directory/identity-protection/overview-identity-protection.md)

- [Guide pratique pour superviser l’activité liée aux identités et aux accès des utilisateurs dans Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8 : Gérer les ressources Azure à partir des emplacements approuvés uniquement

**Aide** : Utilisez des emplacements nommés à accès conditionnel pour autoriser l’accès uniquement à partir de regroupements logiques spécifiques de plages d’adresses IP ou de pays/régions.

- [Guide pratique pour configurer des emplacements nommés dans Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="39-use-azure-active-directory"></a>3.9 : Utiliser Azure Active Directory

**Aide** : Utiliser Azure Active Directory (Azure AD) comme système d’authentification et d’autorisation central. Azure AD protège les données en utilisant un chiffrement fort pour les données au repos et en transit. De plus, AAD sale, hache et stocke de manière sécurisée les informations d’identification utilisateur.

- [Création et configuration d’une instance Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10 : Examiner et rapprocher régulièrement l’accès utilisateur

**Aide** : Azure Active Directory (Azure AD) fournit des journaux pour vous aider à découvrir les comptes obsolètes. De plus, utilisez les révisions d’accès des identités Azure pour gérer efficacement les appartenances aux groupes, les accès aux applications d’entreprise et les attributions de rôles. L’accès des utilisateurs peut être passé en revue régulièrement pour vérifier que seules les utilisateurs appropriés continuent de bénéficier d’un accès.

- [Présentation des rapports Azure AD](/azure/active-directory/reports-monitoring/)

- [Comment utiliser les révisions d’accès des identités Azure](../active-directory/governance/access-reviews-overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11 : Superviser les tentatives d’accès à des informations d’identification désactivées

**Conseils** : vous avez accès aux sources des journaux Activité de connexion, Audit et Événement à risque Azure Active Directory (Azure AD), qui permettent l’intégration à n’importe quel outil SIEM/de supervision. Vous pouvez simplifier ce processus en créant des paramètres de diagnostic pour les comptes d’utilisateur Azure AD et en envoyant les journaux d’audit et les journaux de connexion à un espace de travail Log Analytics. Vous pouvez configurer les alertes souhaitées dans un espace de travail Log Analytics.

- [Guide pratique pour intégrer des journaux d’activité Azure dans Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

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

**Conseils** : utilisez autant que possible des balises pour faciliter le suivi des ressources Azure Monitor qui stockent ou traitent des informations sensibles telles que vos espaces de travail Log Analytics.

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

- [Gérer l’accès aux données de journal et aux espaces de travail dans Azure Monitor](/azure/azure-monitor/platform/manage-access)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2 : Isoler les systèmes qui stockent ou traitent les informations sensibles

**Conseil** : implémentez une isolation en utilisant des abonnements et groupes d’administration distincts selon les domaines de sécurité (par exemple, le type d’environnement et le niveau de confidentialité des données). Vous pouvez limiter le niveau d’accès à vos ressources Azure Monitor et associés que vos applications et environnements d’entreprise demandent. Vous pouvez contrôler l’accès à Azure Monitor via le contrôle d’accès en fonction du rôle d’Azure (RBAC).

- [Guide pratique pour créer des abonnements Azure supplémentaires](../cost-management-billing/manage/create-subscription.md)

- [Guide pratique pour créer des groupes d’administration](../governance/management-groups/create-management-group-portal.md)

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4.4 : Chiffrer toutes les informations sensibles en transit

**Conseil** : Azure Monitor négocie le protocole TLS 1.2 par défaut. Assurez-vous que les clients qui se connectent à vos ressources Azure peuvent négocier TLS 1.2 ou une version ultérieure. 

Application Insights et Log Analytics continuent d’autoriser l’ingestion des données via les protocoles TLS 1.1 et TLS 1.0. Vous pouvez limiter les données au protocole TLS 1.2 en configurant côté client.

- [Comment envoyer des données en toute sécurité à l’aide du protocole TLS 1.2](/azure/azure-monitor/platform/data-security#sending-data-securely-using-tls-12)

**Responsabilité** : Partagé

**Supervision Azure Security Center** : Aucune

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5 : Utiliser un outil de découverte actif pour identifier les données sensibles

**Conseil** : les fonctionnalités d’identification, de classification et de protection contre la perte des données ne sont pas encore disponibles pour Azure Monitor. Implémentez une solution tierce si nécessaire à des fins de conformité.
Pour la plateforme sous-jacente managée par Microsoft, Microsoft considère tout le contenu client comme sensible et met tout en œuvre pour empêcher la perte et l’exposition des données client. Pour garantir la sécurité des données client dans Azure, Microsoft a implémenté et tient à jour une suite de contrôles et de fonctionnalités de protection des données robustes.

- [Présentation de la protection des données client dans Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilité** : Partagé

**Supervision Azure Security Center** : Aucune

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6 : Utiliser le contrôle d’accès en fonction du rôle pour contrôler l’accès aux ressources

**Conseil** : utilisez un contrôle d’accès en fonction du rôle (RBAC) Azure pour gérer l’accès à Azure Monitor.

- [Rôles, autorisations et sécurité dans Azure Monitor](/azure/azure-monitor/platform/roles-permissions-security)

- [Comment configurer Azure RBAC](../role-based-access-control/role-assignments-portal.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8 : Chiffrer des informations sensibles au repos

**Conseil** : Azure Monitor veille à ce que toutes les données et requêtes enregistrées soient chiffrées au repos à l’aide de clés gérées par Microsoft (MMK). Azure Monitor fournit également une option de chiffrement à l’aide de votre propre clé qui est stockée dans votre Azure Key Vault et accessible à l’aide de l’authentification par identité managée attribuée par le système. Cette clé gérée par le client (CMK) peut être protégée par un logiciel ou par un HSM matériel.

- [Clés gérées par le client dans Azure Monitor](/azure/azure-monitor/platform/customer-managed-keys)

- [Sécurité des données Log Analytics](/azure/azure-monitor/platform/data-security)

- [Collecte, rétention et stockage des données dans Application Insights](app/data-retention-privacy.md)

- [Présentation du chiffrement au repos dans Azure](../security/fundamentals/encryption-atrest.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9 : Consigner et alerter les modifications apportées aux ressources Azure critiques

**Conseil** : Utilisez Azure Monitor avec le Journal des activités Azure pour créer des alertes lorsque des modifications sont apportées à Azure Monitor et à des ressources associées.

- [Guide pratique pour créer des alertes sur les événements du journal d’activité Azure](/azure/azure-monitor/platform/alerts-activity-log)

**Responsabilité** : Customer

**Supervision d’Azure Security Center** : le [Benchmark de sécurité Azure](/home/mbaldwin/docs/asb/azure-docs-pr/articles/governance/policy/samples/azure-security-benchmark.md) est l’initiative de stratégie par défaut pour Security Center et constitue la base des [recommandations de Security Center](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/security-center-recommendations.md). Les définitions Azure Policy associées à ce contrôle sont activées automatiquement par Security Center. Un plan [Azure Defender](/home/mbaldwin/docs/asb/azure-docs-pr/articles/security-center/azure-defender.md) pour les services associés peut être nécessaire pour les alertes liées à ce contrôle.

**Définitions intégrées à Azure Policy - microsoft.insights** :

[!INCLUDE [Resource Policy for microsoft.insights 4.9](../../includes/policy/standards/asb/rp-controls/microsoft.insights-4-9.md)]

## <a name="vulnerability-management"></a>Gestion des vulnérabilités

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Gestion des vulnérabilités](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5 : Utilisez un processus de classement des risques pour classer par ordre de priorité la correction des vulnérabilités découvertes.

**Aide** : Utilisez un programme de score de risque commun (par exemple, le système de notation des vulnérabilités courantes) ou les niveaux de risque par défaut fournis par votre outil d’analyse tiers.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="inventory-and-asset-management"></a>Gestion des stocks et des ressources

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Gestion des stocks et des ressources](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1 : Utiliser la solution de détection automatisée des ressources

**Conseils** : Utilisez Azure CLI pour interroger et découvrir des ressources Azure Monitor au sein de vos abonnements. Vérifiez les autorisations (lecture) appropriées dans votre locataire et répertoriez tous les abonnements Azure, ainsi que les ressources dans vos abonnements.

- [Interface de ligne de commande Azure Monitor](https://docs.microsoft.com/cli/azure/monitor)

- [Guide pratique pour afficher ses abonnements Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-4.8.0&preserve-view=true)

- [Présentation d’Azure RBAC](../role-based-access-control/overview.md)

- [Rôles, autorisations et sécurité dans Azure Monitor](/azure/azure-monitor/platform/roles-permissions-security)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="62-maintain-asset-metadata"></a>6.2 : Gérer les métadonnées de ressources

**Conseil** : Appliquez des balises aux ressources Azure Monitor en fournissant des métadonnées pour les organiser de façon logique par catégories.

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="63-delete-unauthorized-azure-resources"></a>6.3 : Supprimer des ressources Azure non autorisées

**Conseil** : utilisez des balises, des groupes d’administration, voire des abonnements distincts, pour organiser et suivre les ressources associées à Azure Monitor. Rapprochez régulièrement l’inventaire et assurez-vous que les ressources non autorisées sont supprimées de l’abonnement en temps utile.

- [Guide pratique pour créer des abonnements Azure supplémentaires](../cost-management-billing/manage/create-subscription.md)

- [Guide pratique pour créer des groupes d’administration](../governance/management-groups/create-management-group-portal.md)

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4 : Définir et tenir un inventaire des ressources Azure approuvées

**Conseils** : Créez un inventaire des ressources Azure et logiciels approuvés pour les ressources de calcul en fonction des besoins de votre organisation.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5 : Analyser les ressources Azure non approuvées

**Aide** : Utilisez Azure Policy pour appliquer des restrictions quant au type de ressources pouvant être créées dans vos abonnements.

Utilisez Azure Resource Graph pour interroger et découvrir des ressources dans leurs abonnements.  Vérifiez que toutes les ressources Azure présentes dans l’environnement sont approuvées.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Procédure pour créer des requêtes avec l’Explorateur Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7 : Supprimer des ressources et applications logicielles Azure non approuvées

**Conseil** : rapprochez régulièrement l’inventaire et assurez-vous que les ressources associées à Azure Monitor non autorisées sont supprimées de l’abonnement en temps utile.  

- [Supprimer un espace de travail Azure Log Analytics](/azure/azure-monitor/platform/delete-workspace)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="69-use-only-approved-azure-services"></a>6.9 : Utiliser des services Azure approuvés uniquement

**Conseil** : utilisez Azure Policy pour restreindre les ressources associées à Azure Monitor que vous pouvez approvisionner dans votre environnement.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Guide pratique pour refuser un type de ressource spécifique avec Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/built-in-policies#general)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6.11 : Limiter la capacité des utilisateurs à interagir avec Azure Resource Manager

**Aide** : Utilisez l’accès conditionnel Azure pour limiter la capacité des utilisateurs à interagir avec Azure Resource Manager en configurant « Bloquer l’accès » pour l’application « Gestion Microsoft Azure ».

- [Guide pratique pour configurer l’accès conditionnel de façon à bloquer l’accès à Azure Resource Manager](../role-based-access-control/conditional-access-azure-management.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="secure-configuration"></a>Configuration sécurisée

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Configuration sécurisée](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1 : Établir des configurations sécurisées pour toutes les ressources Azure

**Conseil** : utilisez des définitions d’Azure Policy personnalisées pour auditer ou appliquer la configuration de vos ressources associées à Azure Monitor. Vous pouvez aussi utiliser des définitions Azure Policy intégrées selon les disponibilités.

Par ailleurs, Azure Resource Manager a la possibilité d’exporter le modèle au format JSON (JavaScript Object Notation), qui doit être examiné pour vérifier que les configurations répondent/dépassent les exigences de sécurité de votre organisation.

Vous pouvez aussi utiliser les recommandations d’Azure Security Center comme base de référence de configuration sécurisée pour vos ressources Azure.

Si vous utilisez des fonctionnalités APM de streaming en direct, sécurisez le canal avec une clé API secrète en plus de la clé d’instrumentation.

- [Sécuriser le flux de métriques temps réel d’APM](https://docs.microsoft.com/azure/azure-monitor/app/live-stream#secure-the-control-channel)

- [Affichage des alias Azure Policy disponibles](https://docs.microsoft.com/powershell/module/az.resources/get-azpolicyalias?view=azps-4.8.0&amp;preserve-view=true)

- [Tutoriel : Créer et gérer des stratégies pour assurer la conformité](../governance/policy/tutorials/create-and-manage.md)

- [Exportation monoressource ou multiressource vers un modèle sur le portail Azure](../azure-resource-manager/templates/export-template-portal.md)

- [Recommandations de sécurité - Guide de référence](../security-center/recommendations-reference.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3 : Gérer les configurations de ressources Azure sécurisées

**Conseil** : utilisez les stratégies Azure Policy [refuser] et [déployer s’il n’existe pas] pour appliquer des paramètres sécurisés à vos ressources associées à Azure Monitor.  En outre, vous pouvez utiliser des modèles Azure Resource Manager pour maintenir la configuration de sécurité de vos ressources associées à Azure Monitor requises par votre organisation.

- [Comprendre les effets d'Azure Policy](../governance/policy/concepts/effects.md)

- [Créer et gérer des stratégies pour assurer la conformité](../governance/policy/tutorials/create-and-manage.md)

- [Présentation des modèles Azure Resource Manager](../azure-resource-manager/templates/overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5 : Stocker en toute sécurité la configuration des ressources Azure

**Aide** : Utilisez Azure DevOps pour stocker et gérer de manière sécurisée votre code, comme les stratégies Azure personnalisées et les modèles Azure Resource Manager. Pour accéder aux ressources que vous gérez dans Azure DevOps, vous pouvez accorder ou refuser des autorisations à des utilisateurs spécifiques, à des groupes de sécurité intégrés ou à des groupes définis dans Azure Active Directory (Azure AD) s’ils sont intégrés à Azure DevOps, ou à Active Directory s’il est intégré à TFS.

- [Stocker du code dans Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [À propos des autorisations et des groupes dans Azure DevOps](/azure/devops/organizations/security/about-permissions)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7 : Déployer des outils de gestion de la configuration pour les ressources Azure

**Conseils** : définissez et implémentez des configurations de sécurité standard pour les ressources associées à Azure Monitor à l’aide d’Azure Policy. Utilisez des définitions d’Azure Policy personnalisées pour auditer ou appliquer la configuration de sécurité de vos ressources associées à Azure Monitor. Vous pouvez également utiliser des définitions de stratégie intégrées en lien avec vos ressources spécifiques.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Alias Azure Policy](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9 : Mettre en place une supervision automatisée de la configuration pour les ressources Azure

**Conseil** : utilisez Azure Security Center pour effectuer des analyses de base pour vos ressources associées à Azure Monitor.  En outre, utilisez Azure Policy pour alerter et auditer les configurations des ressources Azure.

- [Corriger les recommandations dans Azure Security Center](../security-center/security-center-remediate-recommendations.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="711-manage-azure-secrets-securely"></a>7.11 : Gérer les secrets Azure en toute sécurité

**Conseil** : utilisez l’identité du service géré conjointement avec Azure Key Vault pour simplifier et sécuriser la gestion des secrets pour vos ressources associées à Azure Monitor prises en charge.

- [Intégration aux identités managées Azure](../azure-app-configuration/howto-integrate-azure-managed-service-identity.md)

- [Services prenant en charge les identités managées pour les ressources Azure](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)

- [Créer un coffre de clés](../key-vault/secrets/quick-create-portal.md)

- [Fournir une authentification Key Vault avec une identité managée](/azure/key-vault/general/assign-access=policy-portal)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="712-manage-identities-securely-and-automatically"></a>7.12 : Gérer les identités de façon sécurisée et automatique

**Conseils** : utilisez des identités managées pour fournir aux services Azure une identité gérée automatiquement dans Azure Active Directory (Azure AD). Les identités managées vous permettent de vous authentifier auprès de tout service prenant en charge l’authentification Azure AD, y compris les ressources Azure Monitor, sans informations d’identification dans votre code.

- [Guide pratique pour configurer des identités managées pour des ressources Azure](../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13 : Éliminer l’exposition involontaire des informations d’identification

**Conseils** : Exécuter le moteur d’analyse des informations d’identification pour identifier les informations d’identification dans le code. Le moteur d’analyse des informations d’identification encourage également le déplacement des informations d’identification découvertes vers des emplacements plus sécurisés, tels qu’Azure Key Vault.

- [Comment configurer Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="malware-defense"></a>Défense contre les programmes malveillants

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Défense contre les programmes malveillants](../security/benchmarks/security-control-malware-defense.md).*

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2 : Pré-analyser les fichiers à charger sur des ressources Azure non liées au calcul

**Conseil** : Microsoft Antimalware est activé sur l’hôte sous-jacent qui prend en charge les services Azure (par exemple, ressources associées à Azure Monitor), mais il ne s’exécute pas sur votre contenu. 

Pré-analysez tous les fichiers chargés vers les ressources associées à Azure Monitor applicables, telles que l’espace de travail Log Analytics.

Utilisez la détection des menaces pour les services de données dans Azure Security Center pour détecter les programmes malveillants chargés sur les comptes de stockage. 

- [Présentation de Microsoft Anti-malware pour Azure Cloud Services et les machines virtuelles](../security/fundamentals/antimalware.md)

- [Présentation de la détection des menaces pour les services de données d’Azure Security Center](../security-center/azure-defender.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="data-recovery"></a>Récupération des données

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : récupération de données](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1 : Garantir des sauvegardes automatiques régulières

**Conseil** : utilisez Azure Resource Manager pour exporter les ressources Azure Monitor et les ressources associées dans un modèle JavaScript Object Notation (JSON) utilisable comme sauvegarde pour Azure Monitor et des configurations associées.  Utilisez Azure Automation pour exécuter automatiquement les scripts de sauvegarde. 

- [Gérer un espace de travail Log Analytics à l’aide de modèles Azure Resource Manager](/azure/azure-monitor/samples/resource-manager-workspace)

- [Exportation monoressource ou multiressource vers un modèle sur le portail Azure](../azure-resource-manager/templates/export-template-portal.md)

- [À propos d’Azure Automation](../automation/automation-intro.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2 : Effectuer des sauvegardes complètes du système et sauvegarder les clés gérées par le client

**Conseils** : utilisez Azure Resource Manager pour exporter les ressources Azure Monitor et les ressources associées dans un modèle JavaScript Object Notation (JSON) utilisable comme sauvegarde pour Azure Monitor et des configurations associées. Sauvegardez les clés gérées par le client au sein d’Azure Key Vault si des ressources associées à Azure Monitor utilisent des clés gérées par le client.

- [Gérer un espace de travail Log Analytics à l’aide de modèles Azure Resource Manager](/azure/azure-monitor/platform/template-workspace-configuration)

- [Exportation monoressource ou multiressource vers un modèle sur le portail Azure](../azure-resource-manager/templates/export-template-portal.md)

- [Guide pratique pour sauvegarder des clés de coffre de clés dans Azure](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3 : Valider toutes les sauvegardes, y compris les clés gérées par le client

**Conseil** : assurez-vous que la restauration s’effectue régulièrement à l’aide des fichiers de modèle sauvegardés Azure Resource Manager. Testez la restauration des clés gérées par le client sauvegardées.

- [Gérer un espace de travail Log Analytics à l’aide de modèles Azure Resource Manager](/azure/azure-monitor/samples/resource-manager-workspace)

- [Guide pratique pour restaurer des clés de coffre de clés dans Azure](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey?view=azps-4.8.0&amp;preserve-view=true)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4 : Garantir la protection des sauvegardes et des clés managées par le client

**Aide** : Utilisez Azure DevOps pour stocker et gérer de manière sécurisée votre code, comme les stratégies Azure personnalisées et les modèles Azure Resource Manager. Pour protéger les ressources que vous gérez dans Azure DevOps, vous pouvez octroyer ou refuser des autorisations à des utilisateurs spécifiques, à des groupes de sécurité intégrés ou à des groupes définis dans Azure Active Directory (Azure AD) s’ils sont intégrés à Azure DevOps ou à Active Directory s’il est intégré à TFS.  Utilisez le contrôle d’accès en fonction du rôle Azure (Azure RBAC) pour protéger les clés gérées par le client.

En outre, activez la suppression réversible et la protection contre la purge dans Key Vault pour protéger les clés contre une suppression accidentelle ou malveillante. Si le Stockage Azure est utilisé pour stocker les sauvegardes de modèle Azure Resource Manager, la suppression réversible vous permet d’enregistrer et de récupérer vos données en cas de suppression d’objets blob ou d’instantanés d’objets blob.

- [Stocker du code dans Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops&amp;preserve-view=true)

- [À propos des autorisations et des groupes dans Azure DevOps](/azure/devops/organizations/security/about-permissions)

- [Guide pratique pour activer la suppression réversible et la protection contre la purge dans Key Vault](../storage/blobs/soft-delete-blob-overview.md)

- [Suppression réversible pour les objets blob de Stockage Azure](../storage/blobs/soft-delete-blob-overview.md)

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

**Conseils** : Security Center attribue un niveau de gravité à chaque alerte pour vous aider à hiérarchiser celles devant être examinées en premier. La gravité dépend du niveau de confiance que Security Center accorde à la recherche ou à l’analytique utilisées pour émettre l’alerte, mais aussi de l’intention malveillante estimée de l’activité à l’origine de l’alerte.

En outre, marquez clairement les abonnements (par ex. production, non-production) à l’aide d’étiquettes et créez un système de nommage pour identifier et classer clairement les ressources Azure, en particulier celles qui traitent des données sensibles.  Il vous incombe de hiérarchiser le traitement des alertes en fonction de la criticité des ressources et de l’environnement Azure où l’incident s’est produit.

- [Alertes de sécurité dans le Centre de sécurité Azure](../security-center/security-center-alerts-overview.md)

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

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
