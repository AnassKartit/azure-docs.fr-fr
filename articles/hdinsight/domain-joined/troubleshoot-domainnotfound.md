---
title: Échec de création du cluster avec l’erreur DomainNotFound dans Azure HDInsight
description: Étapes de détection de problèmes et résolutions possibles pour les problèmes rencontrés lors d’interactions avec des clusters Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: 953feca9542d73776aee749eff416ba3f2e04921
ms.sourcegitcommit: 106f5c9fa5c6d3498dd1cfe63181a7ed4125ae6d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/02/2021
ms.locfileid: "131079963"
---
# <a name="scenario-cluster-creation-fails-with-domainnotfound-error-in-azure-hdinsight"></a>Scénario : La création du cluster échoue avec l’erreur DomainNotFound dans Azure HDInsight

Cet article décrit les éventuelles solutions à appliquer pour résoudre les problèmes rencontrés lors d’interactions avec des clusters Azure HDInsight.

## <a name="issue"></a>Problème

La création de cluster HDI Secure (Pack Sécurité Entreprise) échoue avec le message d’erreur `DomainNotFound`.

## <a name="cause"></a>Cause

Paramètres DNS incorrects.

## <a name="resolution"></a>Résolution

Lors du déploiement des clusters joints à un domaine, HDI crée un nom d’utilisateur et un mot de passe internes dans AAD DS (pour chaque cluster) et joint tous les nœuds de cluster à ce domaine. La jonction de domaine s’effectue à l’aide d’outils Samba. Vérifiez que les prérequis suivants sont remplis :

* Le nom de domaine doit être résolu via DNS.
* L’adresse IP des contrôleurs de domaine doit être définie dans les paramètres DNS du réseau virtuel sur lequel le cluster est déployé.
* Si le réseau virtuel est pairé avec le réseau virtuel d’AAD DS, vous devrez le faire manuellement.
* Si vous utilisez des redirecteurs DNS, le nom de domaine doit être résolu correctement dans le réseau virtuel.
* Les stratégies de sécurité (NSG) ne doivent pas bloquer la jonction de domaine.

### <a name="additional-debugging-steps"></a>Étapes de débogage supplémentaires

* Déployer une machine virtuelle Windows dans le même sous-réseau, joindre le domaine à l’ordinateur à l’aide d’un nom d’utilisateur et d’un mot de passe (cette opération peut être effectuée via l’interface utilisateur du Panneau de configuration), ou

* Déployer une machine virtuelle Ubuntu dans le même sous-réseau et joindre le domaine à l’ordinateur
  * Se connecter avec SSH à l’ordinateur
  * sudo su
  * Exécuter le script avec le nom d’utilisateur et le mot de passe
  * Le script effectue un test ping et crée les fichiers config requis, puis le domaine. En cas de succès, vos paramètres DNS sont corrects.

## <a name="next-steps"></a>Étapes suivantes

[!INCLUDE [troubleshooting next steps](../includes/hdinsight-troubleshooting-next-steps.md)]