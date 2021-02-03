---
title: Exception de stockage après la réinitialisation de la connexion dans Azure HDInsight
description: Exception de stockage après la réinitialisation de la connexion dans Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/08/2019
ms.openlocfilehash: 82cad7fc68d650e5f525a8722d3e2f3e9865f456
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98936752"
---
# <a name="scenario-storage-exception-after-connection-reset-in-azure-hdinsight"></a>Scénario : Exception de stockage après la réinitialisation de la connexion dans Azure HDInsight

Cet article décrit les éventuelles solutions à appliquer pour résoudre les problèmes rencontrés lors d’interactions avec des clusters Azure HDInsight.

## <a name="issue"></a>Problème

Impossible de créer une nouvelle table Apache HBase.

## <a name="cause"></a>Cause

Au cours d’un processus de troncation de table, un problème de connexion de stockage s’est produit. L’entrée de table a été supprimée dans la table de métadonnées HBase. Tous les fichiers blob ont été supprimés, sauf un.

Même s’il n’y avait aucun dossier d’objets blob nommé `/hbase/data/default/ThatTable` dans le stockage. Le pilote WASB a découvert l’existence du fichier blob ci-dessus et a refusé la création de tout objet blob nommé `/hbase/data/default/ThatTable` car il a supposé que les dossiers parents existaient. La création de table échoue donc.

## <a name="resolution"></a>Résolution

1. À partir de l’interface utilisateur Apache Ambari, redémarrez le HMaster actif. Cela permettra à l’un des deux HMaster de secours de devenir actif et le nouveau HMaster actif recharge les informations de la table de métadonnées. Par conséquent, vous ne verrez pas la table `already-deleted` dans l’interface utilisateur HMaster.

1. Vous pouvez rechercher le fichier d’objet blob orphelin à partir des outils d’interface utilisateur tels que Cloud Explorer ou en exécutant une commande telle que `hdfs dfs -ls /xxxxxx/yyyyy`. Exécutez la commande `hdfs dfs -rmr /xxxxx/yyyy` pour supprimer cet objet blob. Par exemple : `hdfs dfs -rmr /hbase/data/default/ThatTable/ThatFile`.

Vous pouvez maintenant créer une nouvelle table portant le même nom dans HBase.

## <a name="next-steps"></a>Étapes suivantes

Si votre problème ne figure pas dans cet article ou si vous ne parvenez pas à le résoudre, utilisez un des canaux suivants pour obtenir de l’aide :

* Obtenez des réponses de la part d’experts Azure en faisant appel au [Support de la communauté Azure](https://azure.microsoft.com/support/community/).

* Connectez-vous à [@AzureSupport](https://twitter.com/azuresupport), le compte Microsoft Azure officiel pour améliorer l’expérience client. Connexion de la communauté Azure aux ressources appropriées : réponses, support technique et experts.

* Si vous avez besoin d’une aide supplémentaire, vous pouvez envoyer une requête de support à partir du [Portail Microsoft Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Sélectionnez **Support** dans la barre de menus, ou ouvrez le hub **Aide + Support**. Pour plus d’informations, consultez [Création d’une demande de support Azure](../../azure-portal/supportability/how-to-create-azure-support-request.md). L’accès au support relatif à la gestion et à la facturation des abonnements est inclus avec votre abonnement Microsoft Azure. En outre, le support technique est fourni avec l’un des [plans de support Azure](https://azure.microsoft.com/support/plans/).