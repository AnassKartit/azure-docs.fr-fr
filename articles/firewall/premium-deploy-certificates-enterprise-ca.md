---
title: Déployer et configurer des certificats d’autorité de certification d’entreprise pour le Pare-feu Azure Premium
description: Découvrez comment déployer et configurer des certificats d’autorité de certification d’entreprise pour le Pare-feu Azure Premium.
author: vhorne
ms.service: firewall
services: firewall
ms.topic: how-to
ms.date: 07/15/2021
ms.author: victorh
ms.openlocfilehash: 067daef5782697357d08edc0a6c09bea8af085f6
ms.sourcegitcommit: 7d63ce88bfe8188b1ae70c3d006a29068d066287
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/22/2021
ms.locfileid: "114441065"
---
# <a name="deploy-and-configure-enterprise-ca-certificates-for-azure-firewall"></a>Déployer et configurer des certificats d’autorité de certification d’entreprise pour le Pare-feu Azure


Le Pare-feu Azure Premium comprend une fonctionnalité d’inspection TLS qui nécessite une chaîne d’authentification par certificat. Pour les déploiements de production, vous devez utiliser une infrastructure à clé publique (PKI) d’entreprise pour générer les certificats que vous utilisez avec le Pare-feu Azure Premium. Utilisez cet article pour créer et gérer un certificat d’autorité de certification intermédiaire pour le Pare-feu Azure Premium.

Pour plus d’informations sur les certificats utilisés par le Pare-feu Azure Premium, consultez [Certificats du Pare-feu Azure Premium](premium-certificates.md).

## <a name="prerequisites"></a>Prérequis

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

Pour générer un certificat à utiliser avec le Pare-feu Azure Premium à l’aide d’une autorité de certification d’entreprise, vous devez disposer des ressources suivantes : 

- Une forêt Active Directory 
- Une autorité de certification racine des services de certification Active Directory avec l’inscription par le web activée 
- Une stratégie Pare-feu Azure Premium avec niveau Premium 
- Un coffre de clés Azure 
- Une identité managée avec autorisations de lecture sur **Certificats et secrets** définie dans la stratégie d’accès Key Vault 

## <a name="request-and-export-a-certificate"></a>Demander et exporter un certificat

1. Accédez au site d’inscription par le web sur l’autorité de certification racine, généralement `https://<servername>/certsrv`, et sélectionnez **Demander un certificat**.
1. Sélectionnez **Demande de certificat avancée**.
1. Sélectionnez **Créer et soumettre une demande de requête auprès de cette Autorité de certification**.
1. Remplissez le formulaire à l’aide du modèle Autorité de certification secondaire.
1. Envoyez la demande et installez le certificat.
1. En supposant que cette demande est effectuée à partir d’un serveur Windows à l’aide d’Internet Explorer, ouvrez **Options Internet**.
1. Accédez à l’onglet **Contenu**, puis sélectionnez **Certificats**.
1. Sélectionnez le certificat qui vient d’être émis, puis **Exporter**.
1. Sélectionnez **Suivant** pour démarrer l’Assistant. Sélectionnez **Oui, exporter la clé privée**, puis **Suivant**.
1. Le format de fichier .pfx est sélectionné par défaut. Décochez **Inclure tous les certificats dans le chemin d’accès de certification, si possible**. Si vous exportez l’intégralité de la chaîne de certificats, le processus d’importation vers le Pare-feu Azure échoue.
1. Attribuez et confirmez un mot de passe pour protéger la clé, puis sélectionnez **Suivant**.
1. Choisissez un nom de fichier et un emplacement d’exportation, puis sélectionnez **Suivant**.
1. Sélectionnez **Terminer** et déplacez le certificat exporté vers un emplacement sécurisé.

## <a name="add-the-certificate-to-a-firewall-policy"></a>Ajouter le certificat à une stratégie de pare-feu

1. Dans le portail Azure, accédez à la page Certificats de votre coffre de clés et sélectionnez **Générer/importer**.
1. Sélectionnez **Importer** comme méthode de création, nommez le certificat, sélectionnez le fichier .pfx exporté, entrez le mot de passe, puis sélectionnez **Créer**.
1. Accédez à la page **Inspection TLS** de votre stratégie de pare-feu, puis sélectionnez votre identité managée, coffre de clés et certificat. 
1. Sélectionnez **Enregistrer**.
   :::image type="content" source="media/premium-deploy-certificates-enterprise-ca/tls-inspection.png" alt-text="Inspection TLS":::

## <a name="validate-tls-inspection"></a>Valider l’inspection TLS

1. Créez une règle d’application à l’aide de l’inspection TLS sur le nom de domaine complet ou l’URL de destination de votre choix.  Par exemple : `*bing.com`.
1. À partir d’un ordinateur joint à un domaine dans la plage source de la règle, accédez à votre destination et sélectionnez le symbole de verrou à côté de la barre d’adresse de votre navigateur. Le certificat doit indiquer qu’il a été émis par votre autorité de certification d’entreprise plutôt que par une autorité de certification publique.
1. Affichez le certificat pour voir plus de détails, notamment le chemin du certificat.
   :::image type="content" source="media/premium-deploy-certificates-enterprise-ca/certificate-details.png" alt-text="détails du certificat":::
1. Dans Log Analytics, exécutez la requête KQL suivante pour retourner toutes les requêtes qui ont été soumises à l’inspection TLS :
   ```
   AzureDiagnostics 
   | where ResourceType == "AZUREFIREWALLS" 
   | where Category == "AzureFirewallApplicationRule" 
   | where msg_s contains "Url:" 
   | sort by TimeGenerated desc
   ```
   Le résultat montre l’URL complète du trafic inspecté : :::image type="content" source="media/premium-deploy-certificates-enterprise-ca/kql-query.png" alt-text="requête KQL":::

## <a name="next-steps"></a>Étapes suivantes

[Pare-feu Azure Premium dans le Portail Azure](premium-portal.md)
