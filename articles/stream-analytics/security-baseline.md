---
title: Base de référence de la sécurité Azure pour Stream Analytics
description: La ligne de base de sécurité Stream Analytics fournit des instructions et des ressources procédurales pour l’implémentation de recommandations de sécurité spécifiées dans le benchmark de sécurité Azure.
author: msmbaldwin
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/08/2021
ms.author: mbaldwin
ms.custom: subject-security-benchmark
ms.openlocfilehash: 88e27b839e3f8baf9a1df8a6a6d936aecd602b23
ms.sourcegitcommit: c6a2d9a44a5a2c13abddab932d16c295a7207d6a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/09/2021
ms.locfileid: "107285109"
---
# <a name="azure-security-baseline-for-stream-analytics"></a>Base de référence de la sécurité Azure pour Stream Analytics

Cette ligne de base de sécurité applique les instructions du [benchmark de sécurité Azure version 1.0](../security/benchmarks/overview-v1.md) à Stream Analytics. Le benchmark de sécurité Azure fournit des recommandations sur la façon dont vous pouvez sécuriser vos solutions cloud sur Azure. Le contenu est regroupé par les **contrôles de sécurité** définis par le benchmark de sécurité Azure et par les aides associées applicables à Stream Analytics. 

> [!NOTE]
> Les **contrôles** non applicables à Stream Analytics, ou dont la responsabilité incombe à Microsoft, ont été exclus. Pour voir comment Stream Analytics se mappe entièrement au benchmark de sécurité Azure, consultez le **[fichier de mappage complet de la ligne de base de sécurité Stream Analytics](https://github.com/MicrosoftDocs/SecurityBenchmarks/raw/master/Azure%20Offer%20Security%20Baselines/1.1/stream-analytics-security-baseline-v1.1.xlsx)** .

## <a name="network-security"></a>Sécurité réseau

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : sécurité réseau](../security/benchmarks/security-control-network-security.md).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1.1 : Protéger les ressources Azure au sein des réseaux virtuels

**Conseils** : Azure Stream Analytics ne prend pas en charge l’utilisation de groupes de sécurité réseau (NSG) et du Pare-feu Azure.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-network-interfaces"></a>1.2 : Superviser et journaliser la configuration et le trafic des réseaux virtuels, des sous-réseaux et des interfaces réseau

**Conseils** : Azure Stream Analytics ne prend pas en charge l’utilisation de réseaux et sous-réseaux virtuels.

**Responsabilité** : Customer

**Supervision d’Azure Security Center** : Aucune

### <a name="13-protect-critical-web-applications"></a>1.3 : Protéger les applications web critiques 

**Conseils** : Non applicable. Cette recommandation a trait aux applications web s’exécutant sur Azure App Service ou des ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1.4 : Refuser les communications avec des adresses IP connues comme étant malveillantes

**Conseils** : utilisez la protection contre les menaces d’Azure Security Center pour détecter et signaler les communications via des adresses IP Internet malveillantes ou inutilisées.

- [Protection contre les menaces pour la couche des services Azure dans Azure Security Center](../security-center/azure-defender.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="15-record-network-packets"></a>1.5 : Enregistrer les paquets réseau

**Conseils** : Azure Stream Analytics n’utilise pas de groupes de sécurité réseau (NSG), et les journaux de flux pour Azure Key Vault ne sont pas capturés.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1.6 : Déployer des systèmes de détection et de prévention des intrusions (IDS/IPS) basés sur le réseau

**Conseils** : utilisez la protection contre les menaces d’Azure Security Center pour détecter des opérations inhabituelles ou potentiellement dangereuses dans votre environnement d’abonnement Azure.

- [Protection contre les menaces pour la couche des services Azure dans Azure Security Center](../security-center/azure-defender.md)

**Responsabilité** : Customer

**Supervision d’Azure Security Center** : Aucune

### <a name="17-manage-traffic-to-web-applications"></a>1.7 : Gérer le trafic à destination des applications web

**Conseils** : Non applicable. Cette recommandation a trait aux applications web s’exécutant sur Azure App Service ou des ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1.8 : Réduire la complexité et les frais administratifs liés aux règles de sécurité réseau

**Conseils** : Azure Stream Analytics ne prend pas en charge l’utilisation de réseaux virtuels et de règles de réseau.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1.9 : Gérer les configurations de sécurité standard pour les périphériques réseau 

**Conseils** : Azure Stream Analytics ne prend pas en charge l’utilisation de réseaux virtuels et de périphériques réseau.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="110-document-traffic-configuration-rules"></a>1.10 : Règles de configuration du trafic de documents 

**Conseils** : Azure Stream Analytics ne prend pas en charge l’utilisation de réseaux virtuels et de règles de configuration du trafic.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1.11 : Utiliser des outils automatisés pour superviser les configurations des ressources réseau et détecter les modifications 

**Conseils** : utilisez le Journal des activités Azure pour surveiller les configurations de ressources et détecter les modifications apportées à vos ressources Stream Analytics. Créez des alertes dans Azure Monitor, qui se déclenchent en cas de modification des ressources critiques.

- [Guide pratique pour consulter et récupérer les événements du journal d’activité Azure](https://docs.microsoft.com/azure/azure-monitor/essentials/activity-log#view-the-activity-log)

- [Guide pratique pour créer des alertes dans Azure Monitor](../azure-monitor/alerts/alerts-activity-log.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="logging-and-monitoring"></a>Journalisation et supervision

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : journalisation et supervision](../security/benchmarks/security-control-logging-monitoring.md).*

### <a name="22-configure-central-security-log-management"></a>2.2 : Configurer la gestion des journaux de sécurité centrale 

**Conseils** : ingérez les journaux via Azure Monitor pour agréger des données de sécurité telles que des événements et demandes d’audit. Dans Azure Monitor, utilisez des espaces de travail Log Analytics pour interroger et effectuer l’analytique, puis utilisez les comptes de stockage Azure pour le stockage à long terme/d’archivage, éventuellement avec des fonctionnalités de sécurité telles que le stockage immuable et les conservations de rétention appliquées. 

- [Guide pratique pour collecter des journaux et des métriques de plateforme avec Azure Monitor](../azure-monitor/essentials/diagnostic-settings.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="23-enable-audit-logging-for-azure-resources"></a>2.3 : Activer la journalisation d’audit pour les ressources Azure

**Conseils** : activez les paramètres de diagnostic sur votre Azure Stream Analytics pour accéder aux journaux d’administration, de sécurité et de diagnostic. Vous pouvez également activer les paramètres de diagnostic du Journal des activités Azure et envoyer les journaux vers le même espace de travail ou compte de stockage Log Analytics.

- [Azure Stream Analytics fournit des journaux de diagnostic et des données d’activité pour évaluation](stream-analytics-job-diagnostic-logs.md)

**Responsabilité** : Customer

**Supervision d’Azure Security Center** : le [Benchmark de sécurité Azure](/azure/governance/policy/samples/azure-security-benchmark) est l’initiative de stratégie par défaut pour Security Center et constitue la base des [recommandations de Security Center](/azure/security-center/security-center-recommendations). Les définitions Azure Policy associées à ce contrôle sont activées automatiquement par Security Center. Les alertes liées à ce contrôle peuvent nécessiter un plan [Azure Defender](/azure/security-center/azure-defender) pour les services associés.

**Définitions intégrées d’Azure Policy - Microsoft.StreamAnalytics** :

[!INCLUDE [Resource Policy for Microsoft.StreamAnalytics 2.3](../../includes/policy/standards/asb/rp-controls/microsoft.streamanalytics-2-3.md)]

### <a name="24-collect-security-logs-from-operating-systems"></a>2.4 : Collecter les journaux de sécurité des systèmes d’exploitation

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="25-configure-security-log-storage-retention"></a>2.5 : Configurer la conservation du stockage des journaux de sécurité 

**Conseils** : lors du stockage de journaux des événements de sécurité dans le compte de stockage Azure ou dans l’espace de travail Log Analytics, vous pouvez définir la stratégie de rétention en fonction des besoins de votre organisation. 

- [Azure Stream Analytics fournit des journaux de diagnostic et des données d’activité pour évaluation](stream-analytics-job-diagnostic-logs.md)

- [Guide pratique pour configurer la stratégie de conservation des journaux de compte de Stockage Azure](https://docs.microsoft.com/azure/storage/common/manage-storage-analytics-logs#configure-logging) 

- [Modification de la période de conservation des données dans Log Analytics](https://docs.microsoft.com/azure/azure-monitor/logs/manage-cost-storage#change-the-data-retention-period)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="26-monitor-and-review-logs"></a>2.6 : Superviser et examiner les journaux

**Conseils** : analysez et supervisez les journaux afin de détecter les comportements anormaux, et examinez régulièrement les résultats pour vos ressources Stream Analytics. Utilisez l’espace de travail Log Analytics d’Azure Monitor pour examiner les journaux et effectuer des requêtes sur leurs données. Vous pouvez également activer et intégrer les données dans Azure Sentinel ou une solution SIEM tierce. 

- [Guide pratique pour intégrer Azure Sentinel](../sentinel/quickstart-onboard.md) 

- [En savoir plus sur l’espace de travail Log Analytics](../azure-monitor/logs/log-analytics-tutorial.md)

- [Guide pratique pour effectuer des requêtes personnalisées dans Azure Monitor](../azure-monitor/logs/get-started-queries.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="27-enable-alerts-for-anomalous-activities"></a>2.7 : Activer les alertes d’activité anormale

**Conseils** : activez les paramètres de diagnostic pour Stream Analytics et envoyer les journaux à un espace de travail Log Analytics. Intégrez votre espace de travail Log Analytics à Azure Sentinel, car cela fournit une solution SOAR (Security Orchestration Automated Response). Cela permet de créer des playbooks (solutions automatisées) utilisables pour corriger des problèmes de sécurité. 

- [Guide pratique pour intégrer Azure Sentinel](../sentinel/quickstart-onboard.md) 

- [Guide pratique pour générer une alerte sur des données de journal Log Analytics](../azure-monitor/alerts/tutorial-response.md)  

- [Azure Stream Analytics fournit des journaux de diagnostic et des données d’activité pour évaluation](stream-analytics-job-diagnostic-logs.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="28-centralize-anti-malware-logging"></a>2.8 : Centraliser la journalisation anti-programme malveillant 

**Conseils** : non applicable ; Stream Analytics ne traite pas et ne produit pas de journaux liés aux logiciels anti-programmes malveillants.

**Responsabilité** : Customer

**Supervision d’Azure Security Center** : Aucune

### <a name="29-enable-dns-query-logging"></a>2.9 : Activer la journalisation des requêtes DNS 

**Conseils** : La solution Azure DNS Analytics (préversion) dans Azure Monitor rassemble des informations sur une infrastructure DNS concernant la sécurité, les performances et les opérations. Actuellement, elle ne prend pas en charge Azure Stream Analytics. Vous pouvez toutefois utiliser une solution de journalisation DNS tierce. 

- [Rassemblement d’informations sur votre infrastructure DNS avec la solution DNS Analytics (préversion)](../azure-monitor/insights/dns-analytics.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="210-enable-command-line-audit-logging"></a>2.10 : Activer l’enregistrement d’audit en ligne de commande 

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="identity-and-access-control"></a>Contrôle des accès et des identités

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : contrôle des accès et des identités](../security/benchmarks/security-control-identity-access-control.md).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3.1 : Tenir un inventaire des comptes d’administration 

**Aide** : Azure Active Directory (Azure AD) comporte des rôles intégrés qui doivent être explicitement attribués. Il est possible d’interroger les rôles pour découvrir l’appartenance. Utilisez le module Azure AD PowerShell pour effectuer des requêtes ad hoc afin de découvrir les comptes membres de groupes d’administration.

- [Guide pratique pour obtenir un rôle d’annuaire dans Azure AD avec PowerShell](/powershell/module/azuread/get-azureaddirectoryrole)

- [Guide pratique pour obtenir les membres d’un rôle d’annuaire dans Azure AD avec PowerShell](/powershell/module/azuread/get-azureaddirectoryrolemember)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="32-change-default-passwords-where-applicable"></a>3.2 : Modifier les mots de passe par défaut lorsque cela est possible 

**Aide** : Stream Analytics n’intègre pas le concept de mot de passe par défaut, car l’authentification est fournie avec Azure Active Directory (Azure AD) et est sécurisée par le contrôle d’accès en fonction du rôle Azure (Azure RBAC) pour gérer le service. En fonction des services de flux d’injection et des services de sortie, vous devez effectuer une rotation des informations d’identification configurées dans les travaux.

- [Rotation des informations d’identification pour les entrées et les sorties d’un travail Stream Analytics](stream-analytics-login-credentials-inputs-outputs.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="33-use-dedicated-administrative-accounts"></a>3.3 : Utiliser des comptes d’administration dédiés 

**Conseils** : créez un plan de gestion des identités et de sécurité des rôles en suivant les meilleures pratiques, dont le principe d’accès assorti d’un privilège minimum pour les rôles d’administrateur. Utilisez Azure Privileged Identity Management (PIM) pour fournir un accès privilégié juste-à-temps aux ressources Azure Active Directory (Azure AD) et Azure. Utilisez les alertes et l’historique d’audit Azure PIM pour surveiller l’activité des comptes d’administration. Utilisez les rapports de sécurité Azure AD pour faciliter l’identification de comptes d’administration potentiellement compromis.

- [En savoir plus](/azure/active-directory/privileged-identity-management/)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="34-use-azure-active-directory-single-sign-on-sso"></a>3.4 : Utiliser l’authentification unique (SSO) Azure Active Directory

**Aide** : dans la mesure du possible, utilisez l’authentification unique (SSO) Azure Active Directory (Azure AD) au lieu de configurer des informations d’identification autonomes par service. Implémentez les recommandations relatives aux accès et aux identités d’Azure Security Center.

- [Présentation de l’authentification SSO avec Azure AD](../active-directory/manage-apps/what-is-single-sign-on.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3.5 : Utiliser l’authentification multifacteur pour tous les accès basés sur Azure Active Directory

**Aide** : activez l’authentification multifacteur Azure Active Directory (Azure AD) et suivez les recommandations d’Azure Security Center en matière de gestion des identités et des accès pour protéger vos ressources Stream Analytics.

- [Guide pratique pour activer l’authentification multifacteur dans Azure](../active-directory/authentication/howto-mfa-getstarted.md)

- [Guide pratique pour superviser les identités et les accès dans Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilité** : Customer

**Supervision d’Azure Security Center** : Aucune

### <a name="36-use-secure-azure-managed-workstations-for-administrative-tasks"></a>3.6 : Utiliser des stations de travail sécurisées et gérées par Azure pour les tâches administratives

**Aide** : utilisez des stations de travail disposant d’un accès privilégié (PAW) avec l’authentification multifacteur configurée pour vous connecter aux ressources Stream Analytics et les configurer.

- [En savoir plus sur les stations de travail à accès privilégié](https://4sysops.com/archives/understand-the-microsoft-privileged-access-workstation-paw-security-model/)

- [Guide pratique pour activer l’authentification multifacteur dans Azure](../active-directory/authentication/howto-mfa-getstarted.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3.7 : Journaliser et générer des alertes en cas d’activités suspectes sur des comptes d’administration

**Aide** : Utilisez les rapports de sécurité Azure Active Directory (Azure AD) pour générer des journaux et des alertes quand une activité suspecte ou potentiellement dangereuse se produit dans l’environnement. Utiliser Azure Security Center pour superviser les activités liées aux identités et aux accès.

- [Guide pratique pour identifier les utilisateurs Azure AD pour lesquels une activité à risque a été signalée](../active-directory/identity-protection/overview-identity-protection.md)

- [Guide pratique pour superviser l’activité liée aux identités et aux accès des utilisateurs dans Azure Security Center](../security-center/security-center-identity-access.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3.8 : Gérer les ressources Azure à partir des emplacements approuvés uniquement

**Conseils** : Utilisez des emplacements nommés à accès conditionnel pour autoriser l’accès uniquement à partir de regroupements logiques spécifiques de plages d’adresses IP ou de pays/régions. 

- [Guide pratique pour configurer des emplacements nommés dans Azure](../active-directory/reports-monitoring/quickstart-configure-named-locations.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="39-use-azure-active-directory"></a>3.9 : Utiliser Azure Active Directory

**Aide** : Utiliser Azure Active Directory (Azure AD) comme système d’authentification et d’autorisation central. Azure AD fournit un contrôle d’accès en fonction du rôle (Azure RBAC) pour un contrôle affiné de l’accès d’un client aux ressources Stream Analytics.

- [Création et configuration d’une instance Azure AD](../active-directory/fundamentals/active-directory-access-create-new-tenant.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="310-regularly-review-and-reconcile-user-access"></a>3.10 : Examiner et rapprocher régulièrement l’accès utilisateur

**Aide** : examinez les journaux Azure Active Directory (Azure AD) pour identifier les comptes obsolètes, qui peuvent inclure ceux ayant des rôles d’administration de compte de stockage. De plus, utilisez les révisions d’accès des identités Azure pour gérer efficacement les appartenances aux groupes, les accès aux applications d’entreprise et les attributions de rôles. L’accès des utilisateurs doit être passé en revue régulièrement pour vérifier que seuls les utilisateurs appropriés bénéficient d’un accès permanent.

- [Présentation des rapports Azure AD](/azure/active-directory/reports-monitoring/)

- [Comment utiliser les révisions d’accès des identités Azure](../active-directory/governance/access-reviews-overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3.11 : Superviser les tentatives d’accès à des informations d’identification désactivées

**Aide** : activez les paramètres de diagnostic pour Azure Stream Analytics et Azure Active Directory (Azure AD) de façon à envoyer tous les journaux à un espace de travail Log Analytics. Configurez les alertes souhaitées (par exemple, les tentatives d’accès aux secrets désactivés) dans Log Analytics.

- [Intégrer les journaux Azure AD avec les journaux Azure Monitor](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="312-alert-on-account-sign-in-behavior-deviation"></a>3.12 : Alerter en cas d’écart de comportement de connexion à un compte

**Aide** : utilisez les fonctionnalités de protection de l’identité et contre les risques d’Azure Active Directory (Azure AD) pour configurer les réponses automatiques aux actions suspectes détectées relatives à vos ressources Stream Analytics. Vous devez activer des réponses automatisées via Azure Sentinel pour implémenter les réponses de sécurité de votre organisation.

- [Guide pratique pour afficher les connexions risquées Azure AD](../active-directory/identity-protection/overview-identity-protection.md)

- [Guide pratique pour configurer et activer des stratégies de risque Identity Protection](../active-directory/identity-protection/howto-identity-protection-configure-risk-policies.md)

- [Guide pratique pour intégrer Azure Sentinel](../sentinel/quickstart-onboard.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3.13 : Fournir à Microsoft un accès aux données client pertinentes pendant les scénarios de support

**Conseils** : non applicable. Customer Lockbox n’est pas pris en charge pour Azure Stream Analytics.

- [Services et scénarios pris en charge généralement disponibles](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="data-protection"></a>Protection des données

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : protection des données](../security/benchmarks/security-control-data-protection.md).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4.1 : Conserver un inventaire des informations sensibles

**Conseils** : utilisez des étiquettes pour faciliter le suivi des ressources Stream Analytics qui stockent ou traitent des informations sensibles. 

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4.2 : Isoler les systèmes qui stockent ou traitent les informations sensibles

**Conseils** : isolez les travaux Stream Analytics en plaçant les entrées, les sorties et les comptes de stockage dans le même abonnement. Vous pouvez limiter votre Stream Analytics pour contrôler le niveau d’accès à vos ressources Stream Analytics que vos applications et environnements d’entreprise exigent. Vous pouvez contrôler l’accès à Azure Stream Analytics via RBAC Azure Active Directory (Azure AD).

- [Guide pratique pour créer des abonnements Azure supplémentaires](../cost-management-billing/manage/create-subscription.md)

- [Guide pratique pour créer des groupes d’administration](../governance/management-groups/create-management-group-portal.md)

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

- [Comprendre les entrées d’Azure Stream Analytics](stream-analytics-add-inputs.md)

- [Comprendre les sorties d’Azure Stream Analytics](stream-analytics-define-outputs.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4.3. : Surveiller et bloquer le transfert non autorisé d’informations sensibles 

**Conseils** : les fonctionnalités de protection contre la perte de données ne sont pas encore disponibles pour les ressources Azure Stream Analytics. Implémentez une solution tierce si nécessaire à des fins de conformité.

Pour la plateforme sous-jacente gérée par Microsoft, Microsoft traite tout le contenu client comme sensible, et assure une protection contre la perte et l’exposition de données client. Pour garantir la sécurité des données client dans Azure, Microsoft a implémenté et tient à jour une suite de contrôles et de fonctionnalités de protection des données robustes.

- [Présentation de la protection des données client dans Azure](../security/fundamentals/protection-customer-data.md)

- [Comment sécuriser des comptes de Stockage Azure](../storage/blobs/security-recommendations.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4.5 : Utiliser un outil de découverte actif pour identifier les données sensibles

**Conseils** : Les fonctionnalités d’identification des données ne sont pas encore disponibles pour les ressources Azure Stream Analytics. Implémentez une solution tierce si nécessaire à des fins de conformité. 

- [Présentation de la protection des données client dans Azure](../security/fundamentals/protection-customer-data.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4.6 : Utiliser le contrôle d’accès en fonction du rôle pour contrôler l’accès aux ressources

**Conseils** : vous pouvez utiliser le contrôle d’accès en fonction du rôle Azure (Azure RBAC) pour contrôler la façon dont les utilisateurs interagissent avec le service.

- [Comment configurer Azure RBAC](../role-based-access-control/role-assignments-portal.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4.7 : Utiliser la protection contre la perte de données basée sur l’hôte pour appliquer le contrôle d’accès

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="48-encrypt-sensitive-information-at-rest"></a>4.8 : Chiffrer des informations sensibles au repos

**Conseils** : Stream Analytics ne stocke pas les données entrantes dans la mesure où tout le traitement s’effectue en mémoire.  Toutes les données privées, dont les requêtes et fonctions, que Stream Analytics doit impérativement conserver sont stockées dans le compte de stockage configuré.  Utilisez des clés gérées par le client (CMK) pour chiffrer vos données de sortie au repos dans vos comptes de stockage. Même sans CMK, Stream Analytics utilise automatiquement les normes de chiffrement les plus sophistiquées dans l’ensemble de son infrastructure pour chiffrer et sécuriser vos données.

- [Protection des données dans Azure Stream Analytics](data-protection.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4.9 : Consigner et alerter les modifications apportées aux ressources Azure critiques

**Conseils** : utilisez Azure Monitor avec le Journal des activités Azure pour créer des alertes lorsque des modifications sont apportées à des instances des ressources Azure Stream Analytics.

- [Guide pratique pour créer des alertes sur les événements du journal d’activité Azure](../azure-monitor/alerts/alerts-activity-log.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="vulnerability-management"></a>Gestion des vulnérabilités

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Gestion des vulnérabilités](../security/benchmarks/security-control-vulnerability-management.md).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5.1 : Exécuter les outils d’analyse des vulnérabilités automatisés

**Conseils** : suivez les recommandations d’Azure Security Center sur la sécurisation de vos ressources Azure Stream Analytics.

Microsoft assure la gestion des vulnérabilités sur les systèmes sous-jacents prenant en charge Azure Stream Analytics.

- [Comprendre les recommandations d’Azure Security Center](../security-center/recommendations-reference.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5.2 : Déployer une solution de gestion des correctifs de système d’exploitation automatisée

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5.3 : Déployer une solution de gestion automatisée des correctifs des logiciels tiers

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5.4 : Comparer les analyses de vulnérabilités dos à dos

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5.5 : Utilisez un processus de classement des risques pour classer par ordre de priorité la correction des vulnérabilités découvertes.

**Conseils** : Utilisez les évaluations de risques par défaut (degré de sécurisation) fournies par Azure Security Center. 

- [Présentation du degré de sécurisation Azure Security Center](../security-center/secure-score-security-controls.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="inventory-and-asset-management"></a>Gestion des stocks et des ressources

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Gestion des stocks et des ressources](../security/benchmarks/security-control-inventory-asset-management.md).*

### <a name="61-use-automated-asset-discovery-solution"></a>6.1 : Utiliser la solution de détection automatisée des ressources

**Aide** : Utilisez Azure Resource Graph pour interroger/découvrir toutes les ressources (telles que calcul, stockage, réseau, ports et protocoles) dans vos abonnements. Vérifiez les autorisations (lecture) appropriées dans votre locataire et répertoriez tous les abonnements Azure, ainsi que les ressources dans vos abonnements.

Bien que les ressources Azure classiques puissent être découvertes via Resource Graph, il est vivement recommandé de créer et d’utiliser des ressources Azure Resource Manager à l’avenir.

- [Guide pratique pour créer des requêtes avec Azure Resource Graph](../governance/resource-graph/first-query-portal.md)

- [Guide pratique pour afficher ses abonnements Azure](/powershell/module/az.accounts/get-azsubscription)

- [Présentation d’Azure RBAC](../role-based-access-control/overview.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="62-maintain-asset-metadata"></a>6.2 : Gérer les métadonnées de ressources

**Conseils** : Appliquez des balises aux ressources Azure en fournissant des métadonnées pour les organiser de façon logique par catégories.

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="63-delete-unauthorized-azure-resources"></a>6.3 : Supprimer des ressources Azure non autorisées

**Conseils** : le cas échéant, utilisez un étiquetage, des groupes d’administration et des abonnements distincts pour organiser et suivre les ressources Azure Stream Analytics. Rapprochez régulièrement l’inventaire et assurez-vous que les ressources non autorisées sont supprimées de l’abonnement en temps utile.

Appliquez également des restrictions quant au type de ressources pouvant être créées dans les abonnements clients, en utilisant Azure Policy avec les définitions de stratégie intégrées suivantes :

- Types de ressources non autorisés

- Types de ressources autorisés

- [Guide pratique pour créer des abonnements Azure supplémentaires](../cost-management-billing/manage/create-subscription.md)

- [Guide pratique pour créer des groupes d’administration](../governance/management-groups/create-management-group-portal.md)

- [Guide pratique pour créer et utiliser des étiquettes](../azure-resource-manager/management/tag-resources.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="64-define-and-maintain-inventory-of-approved-azure-resources"></a>6.4 : Définir et tenir un inventaire des ressources Azure approuvées

**Aide** : Non applicable. Cette recommandation concerne les ressources de calcul et Azure dans son ensemble.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="65-monitor-for-unapproved-azure-resources"></a>6.5 : Analyser les ressources Azure non approuvées

**Conseils** : Appliquez des restrictions quant au type de ressources pouvant être créées dans les abonnements clients, en utilisant Azure Policy avec les définitions intégrées suivantes :

- Types de ressources non autorisés

- Types de ressources autorisés

Utilisez également Azure Resource Graph pour interroger/découvrir des ressources dans les abonnements.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Guide pratique pour créer des requêtes avec Azure Graph](../governance/resource-graph/first-query-portal.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6.6 : Analyser les applications logicielles non approuvées dans des ressources de calcul

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6.7 : Supprimer des ressources et applications logicielles Azure non approuvées

**Aide** : Non applicable. Cette recommandation concerne les ressources de calcul et Azure dans son ensemble.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="68-use-only-approved-applications"></a>6.8 : Utiliser des applications approuvées uniquement

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="69-use-only-approved-azure-services"></a>6.9 : Utiliser des services Azure approuvés uniquement

**Conseils** : Appliquez des restrictions quant au type de ressources pouvant être créées dans les abonnements clients, en utilisant Azure Policy avec les définitions intégrées suivantes :
- Types de ressources non autorisés
- Types de ressources autorisés

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

**Conseils** : Non applicable. Cette recommandation a trait aux applications web s’exécutant sur Azure App Service ou des ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="secure-configuration"></a>Configuration sécurisée

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Configuration sécurisée](../security/benchmarks/security-control-secure-configuration.md).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7.1 : Établir des configurations sécurisées pour toutes les ressources Azure

**Conseils** : utilisez des alias Azure Policy dans l’espace de noms « Microsoft.StreamAnalytics » pour créer des stratégies personnalisées d’audit ou d’application de la configuration de vos instances Azure Stream Analytics. Vous pouvez également utiliser des définitions de stratégies intégrées relatives à votre Azure Stream Analytics, telles que :

\- Les journaux de diagnostic dans Azure Stream Analytics doivent être activés

- [Affichage des alias Azure Policy disponibles](/powershell/module/az.resources/get-azpolicyalias)

- [Définitions de stratégies intégrées d’Azure Policy](../governance/policy/samples/built-in-policies.md)

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="72-establish-secure-operating-system-configurations"></a>7.2 : Établir des configurations sécurisées du système d’exploitation

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="73-maintain-secure-azure-resource-configurations"></a>7.3 : Gérer les configurations de ressources Azure sécurisées

**Aide** : Utilisez les stratégies Azure Policy [refuser] et [déployer s’il n’existe pas] pour appliquer des paramètres sécurisés à vos ressources Azure.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

- [Présentation des effets d’Azure Policy](../governance/policy/concepts/effects.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="74-maintain-secure-operating-system-configurations"></a>7.4 : Préserver la sécurité des configurations du système d'exploitation

**Aide** : Non applicable. Ces conseils concernent les ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="75-securely-store-configuration-of-azure-resources"></a>7.5 : Stocker en toute sécurité la configuration des ressources Azure

**Aide** : utilisez Azure Repos pour stocker et gérer votre code en toute sécurité, notamment les stratégies Azure personnalisées, les modèles Azure Resource Manager, les scripts Desired State Configuration, les fonctions définies par l’utilisateur et les requêtes. Pour accéder aux ressources que vous gérez dans Azure DevOps, vous pouvez accorder ou refuser des autorisations à des utilisateurs spécifiques, à des groupes de sécurité intégrés ou à des groupes définis dans Azure Active Directory (Azure AD) s’ils sont intégrés à Azure DevOps ou à Azure AD s’il est intégré à TFS.

- [Stocker du code dans Azure DevOps](/azure/devops/repos/git/gitworkflow)

- [À propos des autorisations et des groupes dans Azure DevOps](/azure/devops/organizations/security/about-permissions)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="76-securely-store-custom-operating-system-images"></a>7.6 Stocker en toute sécurité des images de système d’exploitation personnalisées

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7.7 : Déployer des outils de gestion de la configuration pour les ressources Azure

**Conseils** : Utilisez des alias Azure Policy dans l’espace de noms « Microsoft.StreamAnalytics » pour créer des stratégies personnalisées d’alerte, d’audit ou d’application de configurations système. En outre, développez un processus et un pipeline pour la gestion des exceptions de stratégie.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7.8 : Déployer des outils de gestion de la configuration pour les systèmes d'exploitation

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7.9 : Mettre en place une supervision automatisée de la configuration pour les ressources Azure

**Conseils** : Utilisez des alias Azure Policy dans l’espace de noms « Microsoft.StreamAnalytics » pour créer des stratégies personnalisées d’alerte, d’audit ou d’application de configurations système. Utilisez une stratégie Azure Policy [auditer], [refuser] et [déployer si inexistant] afin d’appliquer automatiquement des configurations pour vos ressources Azure Stream Analytics.

- [Guide pratique pour configurer et gérer Azure Policy](../governance/policy/tutorials/create-and-manage.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7.10 : Implémenter la surveillance de la configuration automatique pour les systèmes d’exploitation

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="711-manage-azure-secrets-securely"></a>7.11 : Gérer les secrets Azure en toute sécurité

**Conseils** : Les détails de connexion des ressources d’entrée ou de sortie que votre travail Stream Analytics utilise, sont stockés dans le compte de stockage configuré. Chiffrez votre compte de stockage pour sécuriser toutes vos données.  Opérez régulièrement la rotation des informations d’identification pour les entrées et sorties d’un travail Stream Analytics.

- [Protection des données dans Azure Stream Analytics](data-protection.md)

- [Rotation des informations d’identification pour les entrées et sorties d’un travail Stream Analytics](stream-analytics-login-credentials-inputs-outputs.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="712-manage-identities-securely-and-automatically"></a>7.12 : Gérer les identités de façon sécurisée et automatique

**Conseils** : L’authentification par identité managée pour la sortie donne aux travaux Stream Analytics un accès direct à un service Power BI ou de compte de stockage au lieu d’utiliser une chaîne de connexion.

- [Authentifier Stream Analytics auprès d’Azure Data Lake Storage Gen1 à l’aide d’identités managées](stream-analytics-managed-identities-adls.md)

- [Utiliser des identités managées pour authentifier votre travail Azure Stream Analytics dans la sortie du Stockage Blob Azure](blob-output-managed-identity.md)

- [Utiliser des identités managées pour authentifier votre travail Azure Stream Analytics dans Power BI](powerbi-output-managed-identity.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="713-eliminate-unintended-credential-exposure"></a>7.13 : Éliminer l’exposition involontaire des informations d’identification

**Conseils** : Exécuter le moteur d’analyse des informations d’identification pour identifier les informations d’identification dans le code. Le moteur d’analyse des informations d’identification encourage également le déplacement des informations d’identification découvertes vers des emplacements plus sécurisés, tels qu’Azure Key Vault.

- [Configurer Credential Scanner](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="malware-defense"></a>Défense contre les programmes malveillants

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : Défense contre les programmes malveillants](../security/benchmarks/security-control-malware-defense.md).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8.1 : Utiliser un logiciel anti-programme malveillant géré de manière centralisée

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8.2 : Pré-analyser les fichiers à charger sur des ressources Azure non liées au calcul

**Conseils** : le logiciel anti-programme malveillant Microsoft est activé sur l’hôte sous-jacent qui prend en charge les services Azure (par exemple, Azure Stream Analytics), mais il ne s’exécute pas sur du contenu de client.

Pré-analysez tout contenu chargé sur des ressources Azure telles que App Service, Stream Analytics, Stockage Blob, etc. Microsoft ne peut pas accéder à vos données dans ces instances.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8.3 : Vérifier que les logiciels et signatures anti-programme malveillant sont mis à jour

**Conseils** : Non applicable. Cette recommandation a trait aux ressources de calcul.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="data-recovery"></a>Récupération des données

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : récupération de données](../security/benchmarks/security-control-data-recovery.md).*

### <a name="91-ensure-regular-automated-back-ups"></a>9.1 : Garantir des sauvegardes automatiques régulières

**Conseils** : en fonction du type de service de sortie sélectionné, vous pouvez effectuer des sauvegardes automatisées des données de sortie conformément aux recommandations relatives à votre service de sortie.  Les données internes, notamment les fonctions définies par l’utilisateur, les requêtes et les captures instantanées de données, sont stockées dans le compte de stockage configuré que vous pouvez sauvegarder régulièrement.

Les données de votre compte de stockage Microsoft Azure sont toujours répliquées automatiquement pour en garantir la durabilité et la haute disponibilité. Stockage Azure copie vos données afin qu’elles soient protégées contre les événements planifiés ou non, notamment les défaillances matérielles temporaires, les pannes de réseau ou de courant et les catastrophes naturelles massives. Vous pouvez choisir de répliquer vos données dans le même centre de données, dans des centres de données zonaux d’une même région ou entre des régions géographiques différentes. 

Vous pouvez également utiliser la fonctionnalité de gestion du cycle de vie pour sauvegarder des données dans le niveau Archive.  En outre, activez la suppression réversible pour vos sauvegardes stockées dans le compte de stockage.

- [Protection des données dans Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/data-protection#private-data-assets-that-are-stored)

- [Présentation de la redondance et des contrats de niveau de service Stockage Azure](../storage/common/storage-redundancy.md)

- [Gérer le cycle de vie du stockage Blob Azure](../storage/blobs/storage-lifecycle-management-concepts.md)

- [Suppression réversible pour les objets blob de Stockage Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete?tabs=azure-portal)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9.2 : Effectuer des sauvegardes complètes du système et sauvegarder les clés managées par le client

**Aide** : les données internes, notamment les fonctions définies par l’utilisateur, les requêtes et les captures instantanées de données, sont stockées dans le compte de stockage configuré que vous pouvez sauvegarder régulièrement.

Pour sauvegarder des données à partir de services de compte de stockage pris en charge, plusieurs méthodes sont disponibles, notamment l’utilisation d’azcopy ou d’outils tiers. Le stockage immuable pour le Stockage Blob Azure permet aux utilisateurs de stocker des objets de données critiques pour l’entreprise dans un état WORM (disque optique non réinscriptible). Cet état les rend non effaçables et immuables pour une durée spécifiée par l’utilisateur.

- [Protection des données dans Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/data-protection#private-data-assets-that-are-stored)

- [Bien démarrer avec AzCopy](../storage/common/storage-use-azcopy-v10.md)

- [Définir et gérer des stratégies d’immuabilité pour le stockage Blob](../storage/blobs/storage-blob-immutability-policies-manage.md)

Vous pouvez sauvegarder les clés gérées par le client ou fournies dans Azure Key Vault à l’aide d’Azure CLI ou de PowerShell.

- [Guide pratique pour sauvegarder des clés de coffre de clés dans Azure](/powershell/module/az.keyvault/backup-azkeyvaultkey)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9.3 : Valider toutes les sauvegardes, y compris les clés gérées par le client

**Conseils** : effectuez régulièrement une restauration de vos données de sauvegarde pour tester l’intégrité des données.

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9.4 : Garantir la protection des sauvegardes et des clés managées par le client

**Aide** : les sauvegardes Stream Analytics stockées dans votre stockage Azure prennent en charge le chiffrement par défaut et ne peuvent pas être désactivées.  Vous devez traiter vos sauvegardes comme des données sensibles et appliquer les contrôles d’accès et de protection des données appropriés dans le cadre de cette base de référence.  

- [Protection des données dans Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/data-protection#private-data-assets-that-are-stored)

- [Autorisation de l’accès aux données dans Stockage Azure](../storage/common/storage-auth.md)

**Responsabilité** : Customer

**Supervision Azure Security Center** : Aucune

## <a name="incident-response"></a>Réponse aux incidents

*Pour plus d’informations, consultez [Benchmark de sécurité Azure : réponse aux incidents](../security/benchmarks/security-control-incident-response.md).*

### <a name="101-create-an-incident-response-guide"></a>10.1 : Créer un guide de réponse aux incidents

**Conseils** : Créez un guide de réponse aux incidents pour votre organisation. Assurez-vous qu’il existe des plans de réponse aux incidents écrits qui définissent tous les rôles du personnel, ainsi que les phases de gestion des incidents, depuis la détection jusqu’à la revue une fois l’incident terminé.

- [Aide sur la création de votre propre processus de réponse aux incidents de sécurité](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

- [Anatomie d’un incident dans le centre de réponse aux incidents de sécurité Microsoft](https://msrc-blog.microsoft.com/2019/06/27/inside-the-msrc-anatomy-of-a-ssirp-incident/)

- [Le client peut également tirer parti du guide de gestion des incidents de sécurité informatique du NIST pour faciliter la création de son propre plan de réponse aux incidents](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)

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

- [Reportez-vous à la publication du NIST : Guide to Test, Training, and Exercise Programs for IT Plans and Capabilities (Guide de test, d’entraînement et d’utilisation des programmes destinés aux plans et fonctionnalités informatiques)](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

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

**Aide** : Suivez les règles d’engagement de Microsoft pour garantir que vos tests d’intrusion sont conformes aux stratégies de Microsoft : https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1 

- [Vous trouverez ici plus d’informations sur la stratégie de Microsoft, sur l’exécution de Red Teaming et sur les tests d’intrusion de site actif sur l’infrastructure, les services et les applications cloud qui sont managés par Microsoft](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Responsabilité** : Partagé

**Supervision Azure Security Center** : Aucune

## <a name="next-steps"></a>Étapes suivantes

- Consultez [Vue d’ensemble d’Azure Security Benchmark V2](/azure/security/benchmarks/overview)
- En savoir plus sur les [bases de référence de la sécurité Azure](/azure/security/benchmarks/security-baselines-overview)
