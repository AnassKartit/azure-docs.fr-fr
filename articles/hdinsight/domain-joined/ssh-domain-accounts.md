---
title: Gérer l’accès SSH pour les comptes de domaine dans Azure HDInsight
description: Étapes permettant de gérer l’accès SSH pour les comptes Azure AD dans HDInsight.
ms.service: hdinsight
ms.topic: how-to
ms.date: 02/14/2020
ms.openlocfilehash: 3fab06f5e269f311f798c096a6465c8c6ce75fc2
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98946753"
---
# <a name="manage-ssh-access-for-domain-accounts-in-azure-hdinsight"></a>Gérer l’accès SSH pour les comptes de domaine dans Azure HDInsight

Sur les clusters sécurisés, tous les utilisateurs de domaine dans [Azure AD DS](../../active-directory-domain-services/overview.md) sont, par défaut, autorisés à établir une connexion [SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) dans les nœuds principaux et les nœuds de périphérie. Ces utilisateurs ne font pas partie du groupe sudoers et n’obtiennent pas d’accès racine. L’utilisateur SSH créé lors de la création du cluster aura un accès racine.

## <a name="manage-access"></a>Gérer l’accès

Pour modifier l’accès SSH à des utilisateurs ou groupes spécifiques, mettez à jour `/etc/ssh/sshd_config` sur chacun des nœuds.

1. Utilisez la [commande ssh](../hdinsight-hadoop-linux-use-ssh-unix.md) pour vous connecter à votre cluster. Modifiez la commande ci-dessous en remplaçant CLUSTERNAME par le nom de votre cluster, puis entrez la commande :

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Ouvrez le fichier `ssh_confi`g.

    ```bash
    sudo nano /etc/ssh/sshd_config
    ```

1. Modifiez le fichier `sshd_config` comme vous le souhaitez. Si vous limitez les utilisateurs à certains groupes, les comptes locaux ne peuvent pas établir de connexion SSH à ce nœud. La syntaxe suivante n’est qu’un exemple :

    ```bash
    AllowUsers useralias1 useralias2

    AllowGroups groupname1 groupname2
    ```

    Enregistrez ensuite les changements apportés : **Ctrl + X**, **O**, **Entrée**.

1. Redémarrez SSHD.

    ```bash
    sudo systemctl restart sshd
    ```

1. Répétez les étapes ci-dessus pour chaque nœud.

## <a name="ssh-authentication-log"></a>Journal d’authentification SSH

Le journal d’authentification SSH est écrit dans `/var/log/auth.log`. Si vous constatez des échecs de connexion via SSH pour des comptes locaux ou de domaine, vous devez parcourir le journal pour déboguer les erreurs. Souvent, le problème peut être lié à des comptes d’utilisateur spécifiques, et il est généralement conseillé d’essayer d’autres comptes d’utilisateur ou SSH avec l’utilisateur SSH par défaut (compte local), puis de tenter un kinit.

## <a name="ssh-debug-log"></a>Journal de débogage SSH

Pour activer la journalisation détaillée, vous devez redémarrer `sshd` avec l’option `-d`. Par exemple, `/usr/sbin/sshd -d`. Vous pouvez également exécuter `sshd` sur un port personnalisé (comme 2222) afin de ne pas avoir à arrêter le démon SSH principal. Vous pouvez également utiliser l’option `-v` avec le client SSH pour obtenir davantage de journaux (vue côté client des défaillances).

## <a name="next-steps"></a>Étapes suivantes

* [Gérer des clusters HDInsight avec le pack Sécurité Entreprise](./apache-domain-joined-manage.md)
* [Se connecter à HDInsight (Apache Hadoop) avec SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).
