---
title: Maintenance planifiée - Azure Database pour MySQL - Serveur flexible
description: Cet article décrit la fonction de maintenance planifiée dans Azure Database pour MySQL - Serveur flexible.
author: niklarin
ms.author: nlarin
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: 477d3f5c83c8fa50a4a7d895681dc0c7b2b401a7
ms.sourcegitcommit: 702df701fff4ec6cc39134aa607d023c766adec3
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/03/2021
ms.locfileid: "131442309"
---
# <a name="scheduled-maintenance-in-azure-database-for-mysql--flexible-server"></a>Maintenance planifiée dans Azure Database pour MySQL – Serveur flexible

[[!INCLUDE[applies-to-mysql-flexible-server](../includes/applies-to-mysql-flexible-server.md)]

Azure Database pour MySQL - Serveur flexible effectue une maintenance périodique pour garantir la sécurité, la stabilité et la mise à jour de votre base de données managée. Au cours de la maintenance, le serveur récupère des fonctionnalités, des mises à jour et des correctifs.

## <a name="select-a-maintenance-window"></a>Sélectionner une fenêtre de maintenance

Vous pouvez planifier la maintenance pour un jour spécifique de la semaine et d’une fenêtre de temps dans ce jour. Vous pouvez aussi laisser le système choisir un jour et une heure pour vous automatiquement. Dans les deux cas, le système vous alerte cinq jours avant d’exécuter une opération de maintenance. Le système indique également quand la maintenance est démarrée et quand elle est terminée.

Les notifications relatives à la maintenance planifiée à venir peuvent être :

* Envoyées par e-mail à une adresse spécifique.
* Envoyées par e-mail à un rôle Azure Resource Manager.
* Envoyées dans un message texte (SMS) à des appareils mobiles.
* Envoyées en tant que notification à une application Azure.
* Remises sous forme de message vocal.

Quand vous spécifiez des préférences de planification de la maintenance, vous pouvez choisir un jour de la semaine et une fenêtre de temps. Si vous n’en spécifiez pas, le système choisit des heures entre 23 h 00 et 7 h dans le fuseau horaire de la région de votre serveur. Vous pouvez définir différentes planifications pour chaque serveur flexible dans votre abonnement Azure.

> [!IMPORTANT]
> Normalement, il faut compter au moins 30 jours entre les événements de maintenance planifiés réussis pour un serveur.
>
> Toutefois, dans le cas d’une mise à jour critique urgente telle qu’une vulnérabilité grave, la fenêtre de notification peut être inférieure à cinq jours. La mise à jour critique peut être appliquée à votre serveur même si une maintenance planifiée réussie a été effectuée au cours des 30 derniers jours.

Vous pouvez mettre à jour les paramètres de planification à tout moment. Si une maintenance est planifiée pour votre serveur flexible et que vous mettez à jour les préférences de planification, le déploiement actuel va être effectué comme prévu et la modification des paramètres de planification prendra effet dès la fin de l’opération pour la maintenance planifiée suivante.

Vous pouvez définir une planification gérée par le système ou une planification personnalisée pour chaque serveur flexible dans votre abonnement Azure.
* Avec une planification personnalisée, vous pouvez spécifier votre fenêtre de maintenance pour le serveur en sélectionnant le jour de la semaine et une fenêtre d’une heure.
* Avec la planification gérée par le système, le système choisit une fenêtre d’une heure comprise entre 23h et 7h dans le fuseau horaire de la région de votre serveur.

Dans le cadre du déploiement des modifications, nous appliquons d’abord les mises à jour aux serveurs configurés avec la planification gérée par le système, puis les serveurs avec une planification personnalisée après un intervalle minimal de 7 jours dans une région donnée. Si vous prévoyez de recevoir des mises à jour anticipées sur la flotte des serveurs d’environnement de développement et de test, nous vous recommandons de configurer une planification gérée par le système pour ces serveurs utilisés dans un environnement de développement et de test. Ceci va vous permettre de recevoir la dernière mise à jour d’abord dans votre environnement de développement/test à des fins de test et d’évaluation pour validation. Si vous rencontrez des changements cassants ou de comportement, vous aurez le temps de les traiter avant que la même mise à jour ne soit déployée sur les serveurs de production avec une planification gérée personnalisée. La mise à jour commence à se déployer sur des serveurs flexibles avec planification personnalisée au bout de 7 jours et est appliquée à votre serveur dans la fenêtre de maintenance définie. Pour l’instant, il n’y a pas d’option permettant de différer la mise à jour après l’envoi de la notification. La planification personnalisée est recommandée seulement pour les environnements de production.

Dans de rares cas, l’événement de maintenance peut être annulé par le système ou ne peut pas se terminer correctement. Si la mise à jour échoue, elle est annulée et la version précédente des fichiers binaires est restaurée. Dans ces scénarios d’échec de la mise à jour, il est néanmoins possible que le serveur redémarre pendant la fenêtre de maintenance. Si la mise à jour a été annulée ou a échoué, le système crée une notification concernant l’événement de maintenance annulée ou en échec qui vous en avertit. La tentative suivante d’effectuer l’opération de maintenance sera planifiée en fonction de vos paramètres de planification actuels et vous recevrez une notification cinq jours à l’avance.

## <a name="next-steps"></a>Étapes suivantes

* Découvrez comment [modifier la planification de la maintenance](how-to-maintenance-portal.md).
* Découvrez comment [recevoir des notifications sur la maintenance à venir](../../service-health/service-notifications.md) à l’aide d’Azure Service Health.
* Découvrez comment [configurer des alertes concernant les événements de maintenance planifiée à venir](../../service-health/resource-health-alert-monitor-guide.md).
