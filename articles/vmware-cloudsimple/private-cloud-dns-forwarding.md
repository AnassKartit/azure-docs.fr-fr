---
title: Azure VMware Solutions – Transfert de DNS de cloud privé vers un emplacement local
description: Décrit comment permettre à votre serveur DNS de cloud privé CloudSimple de transférer une recherche de ressources locales
author: suzizuber
ms.author: v-szuber
ms.date: 02/29/2020
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8d6107df467d3be35612ff3d2ac5c9a602022f26
ms.sourcegitcommit: 1d56a3ff255f1f72c6315a0588422842dbcbe502
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/06/2021
ms.locfileid: "129616243"
---
# <a name="enable-cloudsimple-private-cloud-dns-servers-to-forward-dns-lookup-of-on-premises-resources-to-your-dns-servers"></a>Permettre à des serveurs DNS de cloud privé CloudSimple de transférer une recherche DNS de ressources locales à vos serveurs DNS

Des serveurs DNS de cloud privé peuvent transférer une recherche DNS pour toute ressource locale à vos serveurs DNS.  L’activation de la recherche permet aux composants vSphere du cloud privé de rechercher tout service s’exécutant dans votre environnement local et de communiquer avec celui-ci à l’aide de noms de domaine complets (FQDN).

## <a name="scenarios"></a>Scénarios 

Le transfert de recherche DNS pour votre serveur DNS local vous permet d’utiliser votre cloud privé pour les scénarios suivants :

* Utiliser un cloud privé en tant que dispositif de récupération d’urgence pour votre solution VMware locale
* Utiliser votre Active Directory local en tant que source d’identité pour la vSphere votre cloud privé
* Utiliser HCX pour la migration de machines virtuelles d’un emplacement local vers un cloud privé

## <a name="before-you-begin"></a>Avant de commencer

Une connexion réseau doit être établie de votre réseau de cloud privé à votre réseau local pour que le transfert DNS fonctionne.  Vous pouvez configurer la connexion réseau comme suit :

* [Connecter un réseau local à CloudSimple à l’aide d’ExpressRoute](on-premises-connection.md)
* [Configurer une passerelle VPN de site à site](./vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)

Les ports de pare-feu doivent être ouverts sur cette connexion pour que le transfert DNS fonctionne.  Les ports utilisés sont le port TCP 53 ou le port UDP 53.

> [!NOTE]
> Si vous utilisez un VPN site à site, votre sous-réseau de serveur DNS local doit être ajouté en même temps que les préfixes locaux.

## <a name="request-dns-forwarding-from-private-cloud-to-on-premises"></a>Demander un transfert DNS de cloud privé vers un emplacement local

Pour activer le transfert DNS de cloud privé vers un emplacement local, envoyez une [demande de support](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) en fournissant les informations suivantes.

* Type de problème : **Technique**
* Abonnement : **Abonnement dans lequel le service CloudSimple est déployé**
* Service : **Solution VMware par CloudSimple**
* Type de problème : **avis ou comment faire...**
* Sous-type de problème : **besoin d’aide avec NW**
* Indiquez le nom de domaine de votre domaine local dans le volet d’informations.
* Fournissez la liste de vos serveurs DNS locaux vers lesquels la recherche sera transférée à partir de votre cloud privé dans le volet d’informations.

## <a name="next-steps"></a>Étapes suivantes

* [Apprenez-en davantage sur la configuration du pare-feu local](on-premises-firewall-configuration.md)
* [Configuration du serveur DNS local](on-premises-dns-setup.md)
