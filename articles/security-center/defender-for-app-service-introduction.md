---
title: Azure Defender pour App Service - avantages et fonctionnalités
description: Découvrez les fonctionnalités d’Azure Defender pour App Service et comment activer ce service dans votre abonnement
author: memildin
ms.author: memildin
ms.date: 01/25/2021
ms.topic: overview
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: 788cce5f8f380c2bcf2fa7c66b06f22bcda167f2
ms.sourcegitcommit: 0046757af1da267fc2f0e88617c633524883795f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2021
ms.locfileid: "121732581"
---
# <a name="protect-your-web-apps-and-apis"></a>Protéger vos applications web et vos API

## <a name="prerequisites"></a>Prérequis

Security Center étant intégré en mode natif avec App Service, aucun déploiement ou intégration ne sont nécessaires. L’intégration est transparente.

Pour protéger votre plan Azure App Service avec Azure Defender pour App Service, vous aurez besoin des éléments suivants :

- Un plan App Service pris en charge associé aux ordinateurs dédiés. Les plans pris en charge sont répertoriés dans la section [Disponibilité](#availability).

- Azure Defender activé sur votre abonnement, comme décrit dans [Démarrage rapide : Activer Azure Defender](enable-azure-defender.md).

    > [!TIP]
    > Si vous le souhaitez, vous pouvez activer plusieurs plans dans Azure Defender (comme Azure Defender pour App Service).

## <a name="availability"></a>Disponibilité

| Aspect                       | Détails                                                                                                                                                                                        |
|------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| État de sortie :               | Disponibilité générale                                                                                                                                                                      |
| Prix :                     | [Azure Defender pour App Service](azure-defender.md) est facturé conformément à la [tarification de Security Center](https://azure.microsoft.com/pricing/details/security-center/)<br>La facturation se fait en fonction du nombre total d’instances de calcul dans tous les plans       |
| Plans App Service pris en charge : | [Tous les plans App service](https://azure.microsoft.com/pricing/details/app-service/plans/) sont pris en charge, à l’exception d’[Azure Functions sur le plan de consommation](../azure-functions/functions-scale.md). |
| Clouds :                      | :::image type="icon" source="./media/icons/yes-icon.png"::: Clouds commerciaux<br>:::image type="icon" source="./media/icons/no-icon.png"::: National/Souverain (Azure Government, Azure China 21Vianet)                                                     |
|                              |                                                                                                                                                                                                |

## <a name="what-are-the-benefits-of-azure-defender-for-app-service"></a>Quels sont les avantages d’Azure Defender pour App Service ?

Azure App Service est une plateforme complètement managée qui permet de créer et d’héberger vos applications web et vos API. Étant donné que la plateforme est complètement managée, vous n’avez pas à vous soucier de l’infrastructure. Elle fournit des informations sur la gestion, la surveillance et l’exploitation afin de répondre à des exigences en matière de performances, de sécurité et de conformité de classe entreprise. Pour en savoir plus, consultez [Azure App Service](https://azure.microsoft.com/services/app-service/).

**Azure Defender pour App Service** utilise l’échelle du cloud pour identifier les attaques ciblant les applications exécutées sur App Service. Les attaquants sondent les applications web pour trouver et exploiter les faiblesses. Avant d’être routées vers des environnements spécifiques, les demandes adressées aux applications s’exécutant dans Azure passent par plusieurs passerelles, où elles sont inspectées et journalisées. Ces données sont ensuite utilisées pour identifier les codes malveillants exploitant une faille de sécurité ainsi que les attaquants, et pour découvrir de nouveaux modèles qui seront utilisés ultérieurement.

Quand vous activez Azure Defender pour App Service, vous bénéficiez immédiatement des services suivants proposés par ce plan Azure Defender :

- **Sécurisation** : Security Center évalue les ressources couvertes par votre plan App Service et génère des suggestions de sécurité en fonction de ses résultats. Utilisez les instructions détaillées de ces recommandations pour durcir la sécurité de vos ressources App Service.

- **Détection** : Azure Defender détecte une multitude de menaces pour vos ressources App Service en supervisant les éléments suivants :
    - L’instance de machine virtuelle dans laquelle App Service s’exécute et son interface de gestion
    - Les requêtes et les réponses échangées avec vos applications App Service
    - Les bacs à sable et les machines virtuelles sous-jacents
    - Journaux internes App Service : disponibles grâce à la visibilité dont bénéficie Azure en tant que fournisseur de cloud

En tant que solution native Cloud, Azure Defender peut identifier les méthodologies d’attaque qui s’appliquent à plusieurs cibles. Par exemple, il serait difficile, à partir d’un seul hôte, d’identifier une attaque distribuée en se basant sur un petit sous-ensemble d’adresses IP et en analysant des points de terminaison similaires sur plusieurs hôtes.

Ensemble, les données des journaux et l’infrastructure peuvent expliquer ce qui s’est passé, depuis la circulation d’une nouvelle attaque à la compromission des machines clients. Par conséquent, même si Security Center est déployé après qu’une application web a été attaquée, il est capable de détecter les attaques en cours.


## <a name="what-threats-can-azure-defender-for-app-service-detect"></a>Quelles sont les menaces qui peuvent être détectées par Azure Defender pour App Service ?

### <a name="threats-by-mitre-attck-tactics"></a>Menaces émanant des tactiques MITRE ATT&CK

Azure Defender recherche de nombreuses menaces sur vos ressources App Service. Les alertes couvrent la quasi totalité des tactiques MITRE ATT&CK, de la pré-attaque jusqu’à l’attaque « commande et contrôle ». Azure Defender peut détecter les menaces suivantes :

- **Menaces de pré-attaque** : Defender peut détecter plusieurs types d’analyseurs de vulnérabilité que les attaquants utilisent fréquemment pour sonder les failles des applications.

- **Menaces d’accès initial** - [Microsoft Threat Intelligence](https://go.microsoft.com/fwlink/?linkid=2128684) génère ces alertes, notamment lorsqu’une adresse IP malveillante connue se connecte à votre interface FTP Azure App Service.

- **Menaces d’exécution** : Defender peut détecter les tentatives d’exécution de commandes à privilèges élevés et de commandes Linux sur App Service Windows, mais également les comportements d’attaque sans fichier, les outils d’exploration de données monétaires numériques et de nombreuses autres activités d’exécution de code suspect et malveillant.

### <a name="dangling-dns-detection"></a>Détection des entrées DNS non résolues

Azure Defender pour App Service identifie également les entrées DNS restantes dans votre bureau d’enregistrement DNS lors de la mise hors service d’un site web App Service. Ces entrées DNS sont appelées « entrées non résolues ». Lorsque vous supprimez un site web sans supprimer son domaine personnalisé de votre bureau d’enregistrement DNS, l’entrée DNS pointe vers une ressource inexistante, ce qui rend votre sous-domaine vulnérable aux prises de contrôle. Azure Defender ne recherche pas les entrées DNS non résolues *existantes* dans votre bureau d’enregistrement DNS. Il vous avertit quand un site web App Service a été désactivé, mais que son domaine personnalisé (entrée DNS) n’a pas été supprimé.

La prise de contrôle des sous-domaines constitue une menace grave et courante pour les organisations. Lorsqu’un acteur de menace détecte une entrée DNS non résolue, il crée son propre site à l’adresse de destination. Le trafic destiné au domaine de l’organisation est ensuite dirigé vers le site de l’acteur de menace, qui peut utiliser ce trafic pour un large éventail d’activités malveillantes.

La protection des entrées DNS non résolues est disponible pour les domaines gérés par Azure DNS et pour ceux gérés par un bureau d’enregistrement de domaines externes. En outre, elle s’applique aussi bien à App Service sur Windows qu’à App Service sur Linux.

:::image type="content" source="media/defender-for-app-service-introduction/dangling-dns-alert.png" alt-text="Exemple d’alerte Azure Defender concernant la détection d’une entrée DNS non résolue. Activez Azure Defender pour App Service afin de recevoir ce message et d’autres alertes concernant votre environnement." lightbox="media/defender-for-app-service-introduction/dangling-dns-alert.png":::

Pour plus d’informations sur la prise de sous-domaine et les entrées DNS non résolues, consultez [Empêcher les entrées DNS non résolues et la prise de contrôle des sous-domaines](../security/fundamentals/subdomain-takeover.md).

Pour obtenir la liste complète des alertes Azure App Service, consultez la [table de référence des alertes](alerts-reference.md#alerts-azureappserv).

> [!NOTE]
> Defender peut ne pas déclencher d’alertes concernant des entrées DNS non résolues si votre domaine personnalisé ne pointe pas directement vers une ressource App Service, ou si Defender n’a pas supervisé le trafic vers votre site web depuis l’activation de la protection des entrées DNS non résolues (car il n’y a pas de journaux pour faciliter l’identification du domaine personnalisé).

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous avez découvert Azure Defender pour App Service. 

> [!div class="nextstepaction"]
> [Activer Azure Defender](enable-azure-defender.md)

Pour des informations connexes, consultez les articles suivants : 

- Pour exporter vos alertes vers Azure Sentinel (ou un système SIEM tiers) ou tout autre outil externe, suivez les instructions fournies dans [Diffuser des alertes vers un système SIEM, SOAR ou une solution de gestion des services informatiques](export-to-siem.md).
- Pour obtenir la liste des alertes Azure Defender pour App Service, consultez la [table de référence des alertes](alerts-reference.md#alerts-azureappserv).
- Pour plus d’informations sur les plans App Service, consultez [Plans App Service](https://azure.microsoft.com/pricing/details/app-service/plans/).