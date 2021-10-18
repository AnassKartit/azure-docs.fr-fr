---
title: Comment configurer Azure Defender pour SQL
description: Découvrez comment activer l’outil facultatif Azure Defender d’Azure Security Center pour un plan SQL
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: ba46c460-6ba7-48b2-a6a7-ec802dd4eec2
ms.service: security-center
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/11/2021
ms.author: memildin
ms.openlocfilehash: bbfd426c67d09ca8632c3ab665b1242d27c6ad58
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2021
ms.locfileid: "129711690"
---
# <a name="enable-azure-defender-for-sql-servers-on-machines"></a>Activer Azure Defender pour serveurs SQL Server sur des machines 

Ce plan Azure Defender détecte les activités anormales indiquant des tentatives inhabituelles et potentiellement dangereuses pour accéder à des bases de données ou les exploiter.

Vous voyez s’afficher des alertes en cas d’activités de base de données suspectes, de vulnérabilités potentielles ou d’attaques par injection de code SQL ainsi qu’en cas de détection de modèles de requêtes et d’accès anormaux à la base de données.

## <a name="availability"></a>Disponibilité

|Aspect|Détails|
|----|:----|
|État de sortie :|Disponibilité générale|
|Prix :|**Azure Defender pour serveurs SQL Server sur des machines** est facturé conformément à la [Tarification de Security Center](https://azure.microsoft.com/pricing/details/security-center/)|
|Versions de SQL protégées :|Azure SQL Server (toutes les versions couvertes par le support Microsoft)|
|Clouds :|:::image type="icon" source="./media/icons/yes-icon.png"::: Clouds commerciaux<br>:::image type="icon" source="./media/icons/yes-icon.png":::Azure Government<br>:::image type="icon" source="./media/icons/no-icon.png"::: Azure China 21Vianet|
|||

## <a name="set-up-azure-defender-for-sql-servers-on-machines"></a>Configurer Azure Defender pour les serveurs SQL sur des machines

Pour activer ce plan :

[Étape 1 : Installer l’extension de l’agent](#step-1-install-the-agent-extension)

[Étape 2 : Approvisionner l’agent Log Analytics sur l’hôte de votre serveur SQL Server :](#step-2-provision-the-log-analytics-agent-on-your-sql-servers-host)

[Étape 3 : Activer le plan facultatif dans la page de tarification et de paramètres de Security Center :](#step-3-enable-the-optional-plan-in-security-centers-pricing-and-settings-page)


### <a name="step-1-install-the-agent-extension"></a>Étape 1. Installer l’extension de l’agent

- **SQL Server sur une machine virtuelle Azure** : inscrivez votre machine virtuelle SQL Server auprès de l’extension SQL IaaS Agent, comme expliqué dans [Inscrire une machine virtuelle SQL Server auprès de l’extension SQL IaaS Agent](../azure-sql/virtual-machines/windows/sql-agent-extension-manually-register-single-vm.md).

- **SQL Server sur Azure Arc** : installez l’agent Azure Arc en suivant les méthodes d’installation décrites dans la [documentation relative à Azure Arc](../azure-arc/servers/manage-vm-extensions.md).

### <a name="step-2-provision-the-log-analytics-agent-on-your-sql-servers-host"></a>Étape 2. Provisionnez l’agent Log Analytics sur l’hôte du serveur SQL :

- **SQL Server sur une machine virtuelle Azure** : si votre ordinateur SQL est hébergé sur une machine virtuelle Azure, vous pouvez [provisionner automatiquement l'agent de Log Analytics <a name="auto-provision-mma"></a>](security-center-enable-data-collection.md#auto-provision-mma). Vous pouvez également suivre la procédure manuelle pour [intégrer vos machines virtuelles Azure Stack Hub](quickstart-onboard-machines.md?pivots=azure-portal#onboard-your-azure-stack-hub-vms).
- **SQL Server sur Azure Arc** : si votre serveur SQL Server est géré par des serveurs avec [Azure Arc](../azure-arc/index.yml), vous pouvez déployer l’agent Log Analytics en suivant la recommandation Security Center « L’agent Log Analytics doit être installé sur vos machines Azure Arc Windows (préversion) ».

- **SQL Server local** : si votre serveur SQL Server est hébergé sur une machine Windows locale sans Azure Arc, vous disposez de deux options pour la connecter à Azure :
    
    - **Déployer Azure Arc** : vous pouvez connecter n’importe quelle machine Windows à Security Center. Toutefois, Azure Arc permet une intégration plus poussée dans *tout* votre environnement Azure. Si vous configurez Azure Arc, vous verrez la page **SQL Server – Azure Arc** dans le portail et vos alertes de sécurité s’afficheront dans un onglet **Sécurité** dédié sur cette page. Donc, la première option recommandée consiste à [configurer Azure Arc sur l’hôte](../azure-arc/servers/onboard-portal.md#install-and-validate-the-agent-on-windows) et à suivre les instructions pour **SQL Server sur Azure Arc**, ci-dessus.
        
    - **Connecter la machine Windows sans Azure Arc** : si vous choisissez de connecter un serveur SQL Server exécuté sur une machine Windows sans utiliser Azure Arc, suivez les instructions dans [Connecter des machines Windows à Azure Monitor](../azure-monitor/agents/agent-windows.md).


### <a name="step-3-enable-the-optional-plan-in-security-centers-pricing-and-settings-page"></a>Étape 3. Activez le plan facultatif dans la page de tarification et des paramètres de Security Center :

1. Dans le menu de Security Center, ouvrez la page **Tarification et paramètres**.

    - Si vous utilisez **l’espace de travail par défaut d’Azure Security Center** (nommé « defaultworkspace-[ID de votre abonnement]-[région] »), sélectionnez **l’abonnement** approprié.

    - Si vous utilisez un **espace de travail autre que celui par défaut**, sélectionnez **l’espace de travail** approprié (entrez le nom de l’espace de travail dans le filtre, si nécessaire) :

        ![Recherche de votre espace de travail non défini par défaut par titre.](./media/security-center-advanced-iaas-data/pricing-and-settings-workspaces.png)

1. Définissez l’option pour le plan **Azure Defender pour les serveurs SQL Server sur les machines** à  **sur**. 

    :::image type="content" source="./media/security-center-advanced-iaas-data/sql-servers-on-vms-in-pricing-small.png" alt-text="Page de tarification Security Center avec des plans facultatifs.":::

    Le plan est activé sur tous les serveurs SQL connectés à l’espace de travail sélectionné. La protection sera complètement active après le premier redémarrage de l’instance SQL Server.

    >[!TIP] 
    > Pour créer un espace de travail, suivez les instructions fournies dans [Créer un espace de travail Log Analytics](../azure-monitor/logs/quick-create-workspace.md).


1. Vous pouvez également configurer la notification par e-mail pour les alertes de sécurité. 
    Vous pouvez définir une liste de destinataires devant recevoir une notification par e-mail quand des alertes Security Center sont générées. L’e-mail contient un lien direct vers l’alerte dans Azure Security Center avec tous les détails pertinents. Pour plus d'informations, consultez [Configurer des notifications par e-mail pour les alertes de sécurité](security-center-provide-security-contact-details.md).


## <a name="azure-defender-for-sql-alerts"></a>Azure Defender pour les alertes SQL
Les alertes sont générées en cas de détection de tentatives d’accès ou d’exploitation inhabituelles et potentiellement dangereuses des machines SQL. Ces événements peuvent déclencher des alertes indiquées sur la [page de référence des alertes](alerts-reference.md#alerts-sql-db-and-warehouse).

## <a name="explore-and-investigate-security-alerts"></a>Explorer et examiner les alertes de sécurité

Les alertes Azure Defender pour SQL sont disponibles sur la page des alertes de Security Center, sur la page Sécurité de la machine, dans le [tableau de bord Azure Defender](azure-defender-dashboard.md) ou via le lien direct des messages d'alerte.

1. Pour afficher les alertes, sélectionnez **Alertes de sécurité** dans le menu de Security Center et sélectionnez une alerte.

1. Les alertes sont conçues pour être autonomes, avec des étapes de correction détaillées et des informations d’investigation dans chacune d’elles. Vous pouvez approfondir vos investigations en utilisant d’autres fonctionnalités d’Azure Security Center et d’Azure Sentinel pour avoir une vue plus large :

    * Activez la fonctionnalité d’audit de SQL Server pour faire d’autres investigations. Si vous utilisez Azure Sentinel, chargez les journaux d’audit SQL des événements du journal de sécurité Windows pour profiter d’une expérience d’investigation enrichie. [En savoir plus sur l’audit SQL Server](/sql/relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification?preserve-view=true&view=sql-server-ver15).
    * Pour améliorer votre posture de sécurité, appliquez les recommandations concernant la machine hôte que Security Center indique dans chaque alerte. Vous réduirez ainsi les risques d’attaques futures. 

    [En savoir plus sur la gestion des alertes et la réponse à celles-ci](security-center-managing-and-responding-alerts.md).


## <a name="faq---azure-defender-for-sql-servers-on-machines"></a>FAQ – Azure Defender pour serveurs SQL sur des machines

### <a name="if-i-enable-this-azure-defender-plan-on-my-subscription-are-all-sql-servers-on-the-subscription-protected"></a>Si j’active ce plan Azure Defender sur mon abonnement, tous les serveurs SQL de l’abonnement sont-ils protégés ? 

Non. Pour défendre un déploiement de SQL Server sur une machine virtuelle Azure ou un serveur SQL s’exécutant sur une machine avec Azure Arc, Azure Defender nécessite les deux éléments suivants :

- un agent Log Analytics sur la machine ; 
- l’espace de travail Log Analytics concerné pour lequel la solution Azure Defender pour SQL est activée. 

L’*état* de l’abonnement, indiqué sur la page des serveurs SQL dans le portail Azure, reflète l’état par défaut de l’espace de travail et s’applique à toutes les machines connectées. Seuls les serveurs SQL sur les hôtes dotés d’un agent Log Analytics faisant leur rapport à cet espace de travail sont protégés par Azure Defender. 




## <a name="next-steps"></a>Étapes suivantes

Pour des informations connexes, consultez l’article suivant :

- [Alertes de sécurité pour SQL Database et Azure Synapse Analytics](alerts-reference.md#alerts-sql-db-and-warehouse)
- [Configurer des notifications par e-mail pour les alertes de sécurité](security-center-provide-security-contact-details.md)
- [En savoir plus sur Azure Sentinel](../sentinel/index.yml)
