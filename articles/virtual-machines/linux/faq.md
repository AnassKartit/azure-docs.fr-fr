---
title: Forum aux questions sur les machines virtuelles Linux dans Azure
description: Fournit des réponses à certaines questions courantes sur les machines virtuelles Linux créées avec le modèle de déploiement Resource Manager.
author: cynthn
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: cynthn
ms.openlocfilehash: f55b5d9093e2e8e78d3841c332d67f26e5106d38
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98200819"
---
# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Forum aux questions sur les machines virtuelles Linux
Cet article traite certaines questions courantes concernant les machines virtuelles Linux créées dans Azure avec le modèle de déploiement Resource Manager. Pour la version Windows de cette rubrique, consultez les [Questions fréquences sur les machines virtuelles Windows](../windows/faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Qu’est-il possible d’exécuter sur une machine virtuelle Azure ?
Tous les abonnés peuvent exécuter des logiciels serveur sur une machine virtuelle Azure. Pour plus d’informations, consultez [Linux dans des distributions prises en charge par Azure](endorsed-distros.md)

## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Quelle quantité de stockage puis-je utiliser avec une machine virtuelle ?
Chaque disque de données peut avoir une capacité allant jusqu’à 32 767 Gio. Le nombre de disques de données que vous pouvez utiliser dépend de la taille de la machine virtuelle. Pour en savoir plus, voir la rubrique [Tailles de machines virtuelles](../sizes.md).

Les disques managés Azure sont les offres de stockage sur disque recommandées à utiliser avec les machines virtuelles Azure pour le stockage persistant de données. Vous pouvez utiliser plusieurs disques managés avec chaque machine virtuelle. Ces disques offrent deux types d’options de stockage durable : les disques managés Premium et les disques managés Standard. Pour obtenir des informations sur la tarification, consultez [Tarification de la fonctionnalité Disques managés](https://azure.microsoft.com/pricing/details/managed-disks).

Les comptes de stockage Azure peuvent aussi fournir le stockage pour le disque du système d’exploitation et tout disque de données. Chaque disque est un fichier .vhd stocké sous la forme d’un objet blob de pages. Pour plus d’informations sur la tarification, voir [Tarification – Stockage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="how-can-i-access-my-virtual-machine"></a>Comment puis-je accéder à ma machine virtuelle ?
Établissez une connexion à distance pour vous connecter à la machine virtuelle à l’aide de SSH (Secure Shell). Consultez les instructions sur la connexion [à partir de Windows](ssh-from-windows.md) ou [à partir de Linux et Mac](mac-create-ssh-keys.md). Par défaut, SSH autorise un maximum de 10 connexions simultanées. Vous pouvez augmenter ce nombre en modifiant le fichier de configuration.

Si vous rencontrez des problèmes, consultez [Dépanner les connexions Secure Shell (SSH)](../troubleshooting/troubleshoot-ssh-connection.md?toc=/azure/virtual-machines/linux/toc.json).

## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Est-ce que je peux utiliser le disque temporaire (/dev/sdb1) pour stocker les données ?
N’utilisez pas le disque temporaire (/dev/sdb1) pour stocker des données. Il s’agit uniquement d’un stockage temporaire. Vous risquez de perdre des données qui ne pourront pas être récupérées.

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Puis-je copier ou cloner une machine virtuelle Azure ?
Oui. Pour obtenir des instructions, consultez [Création d’une copie d’une machine virtuelle Linux dans le modèle de déploiement Resource Manager](copy-vm.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Pourquoi ne vois-je pas les régions Centre et Est du Canada dans Azure Resource Manager ?
Les deux régions Centre et Est du Canada ne sont pas enregistrées automatiquement lors de la création de machines virtuelles pour des abonnements Azure existants. Cet enregistrement s’effectue automatiquement lorsqu’une machine virtuelle est déployée par le biais du portail Azure dans n’importe quelle autre région à l’aide d’Azure Resource Manager. Une fois une machine virtuelle déployée dans toute autre région Azure, les nouvelles régions doivent être disponibles pour les machines virtuelles suivantes.

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Puis-je ajouter une carte réseau à ma machine virtuelle après sa création ?
Oui, c’est maintenant possible. La machine virtuelle doit d’abord être arrêtée et libérée. Ensuite, vous pouvez ajouter ou supprimer une carte réseau (sauf si elle est la dernière carte réseau sur la machine virtuelle). 

## <a name="are-there-any-computer-name-requirements"></a>Existe-t-il des exigences en matière de nom d’ordinateur ?
Oui. Le nom d’ordinateur peut avoir une longueur maximale de 64 caractères. Consultez l’article [Naming conventions rules and restrictions](/azure/architecture/best-practices/resource-naming) (Restrictions et règles de conventions d’affectation de noms) pour en savoir plus sur la dénomination de vos ressources.

## <a name="are-there-any-resource-group-name-requirements"></a>Existe-t-il des exigences pour le nom du groupe de ressources ?
Oui. Le nom du groupe de ressources peut avoir une longueur maximale de 90 caractères. Consultez l’article [Naming conventions rules and restrictions](/azure/architecture/best-practices/resource-naming) (Restrictions et règles de conventions d’affectation de noms) pour en savoir plus sur les groupes de ressources.

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Quelles sont les exigences en matière de nom d’utilisateur lors de la création d’une machine virtuelle ?

Les noms d’utilisateur doivent avoir une longueur de 1 à 32 caractères.

Les noms d’utilisateur suivants ne sont pas autorisés :

- `1`
- `123`
- `a`
- `actuser`
- `adm`
- `admin`
- `admin1`
- `admin2`
- `administrator`
- `aspnet`
- `backup`
- `console`
- `david`
- `guest`
- `john`
- `owner`
- `root`
- `server`
- `sql`
- `support_388945a0`
- `support`
- `sys`
- `test`
- `test1`
- `test2`
- `test3`
- `user`
- `user1`
- `user2`
- `user3`
- `user4`
- `user5`
- `video`


## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Quelles sont les exigences en matière de mot de passe lors de la création d’une machine virtuelle ?

Il existe différentes exigences en termes de longueur du mot de passe, selon l’outil que vous utilisez :
 - Portail : entre 12 et 72 caractères
 - PowerShell : entre 8 et 123 caractères
 - Interface CLI : entre 12 et 123 caractères
 - Modèles Azure Resource Manager (ARM) : 12 à 72 caractères (les caractères de contrôle ne sont pas autorisés)
 

Les mots de passe doivent également répondre à 3 des 4 exigences de complexité suivantes :

* Avoir des minuscules
* Avoir des majuscules
* Avoir un chiffre
* Avoir un caractère spécial (correspondances Regex [\W_])

Les noms mots de passe suivants ne sont pas autorisés :

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">Pa$$word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Password!</td>
        <td style="text-align:center">Password1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">iloveyou!</td>
    </tr>
</table>
