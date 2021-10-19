---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 04/07/2021
ms.topic: include
ms.openlocfilehash: 0d0a5faa2941e429fab1d6250ef443257024a616
ms.sourcegitcommit: 860f6821bff59caefc71b50810949ceed1431510
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2021
ms.locfileid: "129725504"
---
Azure Defender fournit une protection contre les menaces en temps réel pour vos environnements conteneurisés, et génère des alertes en cas d'activités suspectes. Vous pouvez utiliser ces informations pour remédier rapidement aux problèmes de sécurité et améliorer la sécurité de vos conteneurs.

Azure Defender offre une protection contre les menaces à différents niveaux : 

* **Niveau de l’hôte (fourni par Azure Defender pour les serveurs)**  : à l’aide du même agent Log Analytics que celui utilisé par Security Center sur d’autres machines virtuelles, Azure Defender surveille vos nœuds Linux Kubernetes pour repérer les activités suspectes, par exemple, la détection de web shell et la connexion avec des adresses IP suspectes connues. L’agent surveille également les analyses propres au conteneur, comme la création de conteneurs privilégiés, l’accès suspect à des serveurs d’API et à des serveurs Secure Shell (SSH) s’exécutant dans un conteneur Docker.

    Si vous choisissez de ne pas installer les agents sur vos machines hôtes, vous ne recevrez qu’une partie des avantages et alertes de sécurité relatifs à la protection contre les menaces. Vous recevrez toujours les alertes liées à l’analyse réseau et aux communications avec des serveurs malveillants.

    >[!IMPORTANT]
    > Actuellement, nous ne prenons pas en charge l’installation de l’agent Log Analytics sur les clusters Azure Kubernetes Service qui s’exécutent sur des groupes de machines virtuelles identiques.

    Pour obtenir la liste des alertes au niveau de l’hôte, consultez la [table de référence des alertes](../articles/security-center/alerts-reference.md#alerts-containerhost).


* **Niveau du cluster (fourni par Azure Defender pour Kubernetes)**  : au niveau du cluster, la protection contre les menaces s’appuie sur l’analyse des journaux d’audit de Kubernetes. Pour activer cette analyse **sans agent**, activez Azure Defender. Si votre cluster est situé localement ou chez un autre fournisseur de cloud, activez [Kubernetes avec Azure Arc et l’extension Azure Defender](../articles/security-center/defender-for-kubernetes-azure-arc.md).

    Pour générer des alertes à ce niveau, Azure Defender surveille les journaux de vos clusters. Les tableaux de bord Kubernetes exposés, la création de rôles dotés de privilèges élevés et la création de montages sensibles sont des exemples d’événements à ce niveau.

    >[!NOTE]
    > Azure Defender génère des alertes de sécurité pour les actions et les déploiements qui se produisent après votre activation du plan Defender pour Kubernetes sur votre abonnement. 

    Pour obtenir la liste des alertes au niveau du cluster, consultez la [Table de référence des alertes](../articles/security-center/alerts-reference.md#alerts-k8scluster).

De plus, notre équipe mondiale d’experts en sécurité surveille en permanence l’évolution des menaces. Ils ajoutent des alertes et des vulnérabilités propres aux conteneurs au fur et à mesure de leur découverte.

> [!TIP]
> Vous pouvez simuler des alertes de conteneur en suivant les instructions données dans [ce billet de blog](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270).
