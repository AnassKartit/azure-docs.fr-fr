---
title: Migration de MySQL local vers Azure Database pour MySQL – Cas d’usage représentatif
description: Le cas d’usage suivant est basé sur un scénario client réel d’une entreprise qui a migré sa charge de travail MySQL vers Azure Database pour MySQL.
ms.service: mysql
ms.subservice: migration-guide
ms.topic: how-to
author: arunkumarthiags
ms.author: arthiaga
ms.reviewer: maghan
ms.custom: ''
ms.date: 06/21/2021
ms.openlocfilehash: 508e27006c96003bd4c2825c9987761f83358f0d
ms.sourcegitcommit: 8b38eff08c8743a095635a1765c9c44358340aa8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/30/2021
ms.locfileid: "113085071"
---
# <a name="migrate-mysql-on-premises-to-azure-database-for-mysql-representative-use-case"></a>Migration de MySQL local vers Azure Database pour MySQL – Cas d’usage représentatif

[!INCLUDE[applies-to-mysql-single-flexible-server](../../includes/applies-to-mysql-single-flexible-server.md)]

## <a name="prerequisites"></a>Prérequis

[Introduction](01-mysql-migration-guide-intro.md)
## <a name="overview"></a>Vue d’ensemble

Le cas d’usage suivant est basé sur un scénario client réel d’une entreprise qui a migré sa charge de travail MySQL vers Azure Database pour MySQL.

La société WWI (World-Wide Importers) est un fabricant et distributeur en gros de gadgets basé à San Francisco en Californie. Elle a démarré en 2002 et a développé un modèle B2B (Business-to-Business) efficace, en vendant les articles qu’elle produit directement à des clients revendeurs dans tous les États-Unis. Ses clients comprennent des magasins spécialisés, des supermarchés, des magasins d’informatique, des boutiques d’attractions touristiques et quelques particuliers. Ce modèle B2B constitue un système de distribution rationalisé de ses produits, qui lui permet de réduire les coûts et d’offrir un tarif plus compétitif sur ses articles manufacturés. Elle vend également à d’autres grossistes via un réseau d’agents qui promeuvent ses produits pour le compte de WWI.

Avant de se lancer dans de nouveaux secteurs, WWI veut vérifier que son infrastructure informatique peut gérer la croissance attendue. WWI héberge actuellement toute son infrastructure informatique locale au siège de l’entreprise et estime que le déplacement de ces ressources vers le cloud permettra une croissance future. En conséquence, le directeur informatique a été chargé de superviser la migration de leur portail client et des charges de travail de données associées dans le cloud.

WWI souhaite continuer à tirer parti des nombreuses fonctionnalités avancées disponibles dans le cloud, et voudrait migrer ses bases de données et leurs charges de travail associées dans Azure. Elle veut le faire rapidement et sans avoir à apporter de modifications à ses applications ou à ses bases de données. À la base, elle prévoit de migrer vers le cloud son application web du portail client basée sur Java ainsi que les bases de données et charges de travail MySQL associées.

### <a name="migration-goals"></a>Objectifs de la migration

Les principaux objectifs de la migration vers le cloud de ses bases de données et des charges de travail SQL associées sont les suivants :

  - Améliorer la posture globale de sécurité pour les données au repos et en transit.

  - Améliorer les capacités an matière de haute disponibilité et de reprise d’activité.

  - Positionner l’organisation pour qu’elle utilise des fonctionnalités et des technologies cloud natives, comme la restauration à un point dans le temps.

  - Tirer parti des fonctionnalités d’administration et d’optimisation des performances d’Azure Database pour MySQL.

  - Créer une plateforme scalable qu’elle peut utiliser pour développer son activité dans de nouvelles régions géographiques.

  - Permettre une meilleure conformité avec les différentes législations en matière de stockage des informations personnelles.

WWI a utilisé le [Cloud Adoption Framework](/azure/cloud-adoption-framework/) pour former son équipe aux bonnes pratiques en matière de migration cloud. Ensuite, en utilisant le Cloud Adoption Framework comme guide de migration général, WWI a personnalisé sa migration en trois phases principales. Enfin, elle a défini les activités qui devaient être traitées dans chaque phase pour garantir la réussite de la migration cloud lift-and-shift.

Ces étapes sont les suivantes :

| Étape | Nom | Activités |
|-------|------|------------|
| 1 | Prémigration | Évaluation, Planification, Évaluation des méthodes de migration, Implications des applications, Plans de test, Bases de référence des performances |
| 2 | Migration     | Exécuter la migration, Exécuter les plans de test                                                                          |
| 3 | Postmigration| Continuité des activités, Reprise d’activité, Gestion, Sécurité, Optimisation des performances, Modernisation de la plateforme |

WWI a plusieurs instances de MySQL s’exécutant avec des différentes versions allant de 5.5 à 5.7. Ils veulent faire passer leurs instances sur la dernière version dès que possible, mais veulent être sûrs que leurs applications peuvent dans ce cas continuer à fonctionner. Ils ne voient aucun problème à faire passer la même version dans le cloud et à effectuer la mise à niveau après, mais ils préfèreraient effectuer les deux tâches à la fois.

Ils voudraient aussi vérifier que leurs charges de travail de données sont sûres et disponibles dans plusieurs régions géographiques en cas de panne et examiner les options de configuration disponibles.

WWI veut commencer par une application simple pour la première migration, puis passer à des applications plus critiques dans une phase ultérieure. Ceci va donner à l’équipe les connaissances et l’expérience nécessaire pour préparer et planifier ces migrations futures.  

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Évaluation](./03-assessment.md)
