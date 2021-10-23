---
title: Ajouter et gérer des certificats TLS/SSL
description: Créez un certificat gratuit, importez un certificat App Service, importez un certificat Key Vault, ou achetez un certificat App Service dans Azure App Service.
tags: buy-ssl-certificates
ms.topic: tutorial
ms.date: 05/13/2021
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: e9344aae6dd001a660549d85ac1b1527c332f2d8
ms.sourcegitcommit: 92889674b93087ab7d573622e9587d0937233aa2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2021
ms.locfileid: "130178573"
---
# <a name="add-a-tlsssl-certificate-in-azure-app-service"></a>Ajouter un certificat TLS/SSL dans Azure App Service

[Azure App Service](overview.md) offre un service d’hébergement web hautement évolutif appliquant des mises à jour correctives automatiques. Cet article explique comment créer, charger ou importer un certificat privé ou public dans App Service. 

Une fois le certificat ajouté à votre application App Service ou à votre [application de fonction](../azure-functions/index.yml), vous pouvez [sécuriser un nom DNS personnalisé avec celui-ci](configure-ssl-bindings.md) ou l’[utiliser dans votre code d’application](configure-ssl-certificate-in-code.md).

> [!NOTE]
> Quand un certificat est chargé dans une application, celui-ci est stocké dans une unité de déploiement qui est liée à la combinaison Groupe de ressources/Région (appelée en interne *espace web*) du plan App Service. De cette façon, le certificat est accessible aux autres applications qui sont associées à la même combinaison Groupe de ressources/Région. 

Le tableau suivant répertorie les options permettant d’ajouter des certificats dans App Service :

|Option|Description|
|-|-|
| Créer un certificat managé App Service gratuit | Certificat privé gratuit et facile à utiliser si vous devez simplement sécuriser votre [domaine personnalisé](app-service-web-tutorial-custom-domain.md) dans App Service. |
| Acheter un certificat App Service | Certificat privé managé par Azure. Il combine la simplicité d’une gestion automatisée et la flexibilité des options de renouvellement et d’exportation. |
| Importer un certificat à partir de Key Vault | Utile si vous utilisez [Azure Key Vault](../key-vault/index.yml) pour gérer vos [certificats PKCS12](https://wikipedia.org/wiki/PKCS_12). Consultez [Exigences concernant les certificats privés](#private-certificate-requirements). |
| Téléchargement d’un certificat privé | Si vous disposez déjà d’un certificat privé provenant d’un fournisseur tiers, vous pouvez le charger. Consultez [Exigences concernant les certificats privés](#private-certificate-requirements). |
| Téléchargement d’un certificat public | Les certificats publics ne sont pas utilisés pour sécuriser les domaines personnalisés, mais vous pouvez les charger dans votre code si vous en avez besoin pour accéder aux ressources distantes. |

## <a name="prerequisites"></a>Prérequis

- [Créez une application App Service](./index.yml).
- Pour un certificat privé, assurez-vous qu’il répond [à toutes les exigences d’App Service](#private-certificate-requirements).
- **Certificat gratuit uniquement** :
    - Mappez le domaine pour lequel vous souhaitez un certificat à App Service. Pour plus d’informations, consultez [Tutoriel : Mapper un nom DNS personnalisé existant à Azure App Service](app-service-web-tutorial-custom-domain.md).
    - Pour un domaine racine (par exemple, contoso.com), assurez-vous que votre application n’a aucune [restriction d’adresse IP](app-service-ip-restrictions.md) configurée. La création du certificat et son renouvellement périodique pour un domaine racine dépendent de l’accessibilité de votre application depuis l’Internet.

## <a name="private-certificate-requirements"></a>Exigences concernant les certificats privés

Le [certificat managé App Service gratuit](#create-a-free-managed-certificate) et le [certificat App Service](#import-an-app-service-certificate) répondent déjà aux exigences d’App Service. Si vous choisissez de charger ou d’importer un certificat privé dans App Service, votre certificat doit :

* Être exporté en tant que [fichier PFX protégé par mot de passe](https://en.wikipedia.org/w/index.php?title=X.509&section=4#Certificate_filename_extensions), chiffré par Triple-DES.
* Contenir une clé privée d’au moins 2048 bits de long
* Contient tous les certificats intermédiaires et le certificat racine dans la chaîne de certificats.

Pour sécuriser un domaine personnalisé dans une liaison TLS/SSL, votre certificat doit répondre à des exigences supplémentaires :

* Contenir une [utilisation améliorée de la clé ](https://en.wikipedia.org/w/index.php?title=X.509&section=4#Extensions_informing_a_specific_usage_of_a_certificate) pour l’authentification du serveur (OID = 1.3.6.1.5.5.7.3.1)
* Être signé par une autorité de certification approuvée

> [!NOTE]
> Les **certificats de chiffrement à courbe elliptique (ECC)** sont compatibles avec App Service, mais ce sujet sort du cadre de cet article. Consultez votre autorité de certification sur les étapes à suivre pour créer des certificats ECC.

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

## <a name="create-a-free-managed-certificate"></a>Créer un certificat managé gratuit

> [!NOTE]
> Avant de créer un certificat managé gratuit, assurez-vous que vous avez [rempli les conditions préalables](#prerequisites) pour votre application.

Le certificat managé App Service gratuit est une solution clé en main pour la sécurisation de votre nom DNS personnalisé dans App Service. Il s’agit d’un certificat de serveur TLS/SSL entièrement géré par App Service et renouvelé en continu et automatiquement par incréments de six mois, 45 jours avant l’expiration. Vous créez le certificat, vous le liez à un domaine personnalisé et vous laissez App Service faire le reste.

Le certificat gratuit comprend les limitations suivantes :

- Ne prend pas en charge les certificats avec caractères génériques.
- Ne prend pas en charge l’utilisation en tant que certificat client par empreinte numérique de certificat (la suppression de l’empreinte numérique de certificat est planifiée).
- Il n’est pas exportable.
- N’est pas pris en charge sur App Service et pas accessible à tous.
- N’est pas pris en charge sur App Service Environment (ASE).
- N’est pas pris en charge avec les domaines racine intégrés à Traffic Manager.
- Si un certificat est destiné à un domaine avec mappage CNAM, le CNAM doit être mappé directement à `<app-name>.azurewebsites.net`.

> [!NOTE]
> Le certificat gratuit est émis par DigiCert. Pour certains domaines, vous devez autoriser explicitement DigiCert comme émetteur de certificat en créant un [enregistrement de domaine CAA](https://wikipedia.org/wiki/DNS_Certification_Authority_Authorization) avec la valeur : `0 issue digicert.com`.
> 

Sur le <a href="https://portal.azure.com" target="_blank">portail Azure</a>, dans le menu de gauche, sélectionnez **App Services** >  **\<app-name>** .

Dans le panneau de navigation de gauche de votre application, sélectionnez **Paramètres TLS/SSL** > **Certificats à clé privée (.pfx)**  > **Créer un certificat managé App Service**.

![Créer un certificat gratuit dans App Service](./media/configure-ssl-certificate/create-free-cert.png)

Sélectionnez le domaine personnalisé pour lequel créer un certificat gratuit, puis sélectionnez **Créer**. Vous ne pouvez créer qu’un seul certificat pour chaque domaine personnalisé pris en charge.

Une fois l’opération terminée, le certificat s’affiche dans la liste **Certificats à clé privée**.

![Création du certificat gratuit terminée](./media/configure-ssl-certificate/create-free-cert-finished.png)

> [!IMPORTANT] 
> Pour sécuriser un domaine personnalisé avec ce certificat, vous devez quand même créer une liaison de certificat. Suivez les étapes fournies dans [Créer une liaison](configure-ssl-bindings.md#create-binding).
>

## <a name="import-an-app-service-certificate"></a>Importer un certificat App Service

Si vous achetez un certificat App Service dans Azure, Azure se charge des tâches suivantes :

- Prise en charge du processus d’achat sur GoDaddy
- Vérification du domaine du certificat
- Gestion du certificat dans [Azure Key Vault](../key-vault/general/overview.md)
- Gestion du renouvellement des certificats (voir [Renouveler le certificat](#renew-an-app-service-certificate))
- Synchronisation automatique des certificats avec les copies importées dans les applications App Service

Pour acheter un certificat App Service, consultez [Commander un certificat](#start-certificate-order).

Si vous disposez déjà d’un certificat App Service, vous pouvez :

- [Importer le certificat dans App Service](#import-certificate-into-app-service)
- [Gérer le certificat](#manage-app-service-certificates), par exemple, le renouveler, renouveler sa clé ou l’exporter
> [!NOTE]
> Les certificats App Service ne sont pas pris en charge dans les clouds nationaux Azure à l’heure actuelle.

### <a name="start-certificate-order"></a>Commander un certificat

Commandez un certificat App Service dans la <a href="https://portal.azure.com/#create/Microsoft.SSL" target="_blank">page de création App Service Certificate</a>.

![Démarrer l’achat d’un certificat App Service](./media/configure-ssl-certificate/purchase-app-service-cert.png)

Aidez-vous du tableau suivant pour configurer le certificat. Lorsque vous avez terminé, cliquez sur **Créer**.

| Paramètre | Description |
|-|-|
| Nom | Nom convivial de votre certificat App Service. |
| Nom d’hôte de domaine nu | Spécifiez ici le domaine racine. Le certificat émis sécurise *à la fois* le domaine racine et le sous-domaine `www`. Dans le certificat émis, le champ Nom commun contient le domaine racine et le champ Autre nom de l’objet contient le domaine `www`. Pour sécuriser un sous-domaine uniquement, indiquez ici son nom de domaine complet (par exemple, `mysubdomain.contoso.com`).|
| Abonnement | Abonnement qui contient le certificat. |
| Resource group | Groupe de ressources qui doit contenir le certificat. Vous pouvez utiliser un nouveau groupe de ressources, ou sélectionner le même groupe de ressources que votre application App Service, par exemple. |
| Référence (SKU) de certificat | Détermine le type de certificat à créer (certificat standard ou [certificat générique](https://wikipedia.org/wiki/Wildcard_certificate)). |
| Termes et conditions | Cliquez pour confirmer que vous acceptez les termes et conditions. Les certificats sont obtenus auprès de GoDaddy. |

> [!NOTE]
> Les certificats App Service achetés auprès d’Azure sont émis par GoDaddy. Pour certains domaines, vous devez autoriser explicitement GoDaddy comme émetteur de certificat en créant un [enregistrement de domaine CAA](https://wikipedia.org/wiki/DNS_Certification_Authority_Authorization) avec la valeur : `0 issue godaddy.com`
> 

### <a name="store-in-azure-key-vault"></a>Stocker dans Azure Key Vault

Après l’achat du certificat, il vous reste quelques étapes à effectuer avant de commencer à utiliser ce certificat. 

Sélectionnez le certificat dans la page [App Service Certificates](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders), puis cliquez sur **Configuration du certificat** > **Étape 1 : Stocker**.

![Configurer le stockage Key Vault du certificat App Service](./media/configure-ssl-certificate/configure-key-vault.png)

[Key Vault](../key-vault/general/overview.md) est un service Azure qui permet de protéger les clés de chiffrement et les secrets utilisés par les services et les applications cloud. C’est le stockage recommandé pour les certificats App Service.

Dans la page **État de Key Vault**, cliquez sur **Référentiel Key Vault** pour créer un coffre ou choisir un coffre existant. Si vous créez un coffre, configurez le nouveau coffre en vous aidant du tableau ci-dessous, puis cliquez sur Créer. Créez le coffre de clés dans le même abonnement et groupe de ressources que votre application App Service.

| Paramètre | Description |
|-|-|
| Nom | Nom unique composé de caractères alphanumériques et de tirets. |
| Resource group | Nous vous conseillons de choisir le même groupe de ressources que votre certificat App Service. |
| Emplacement | Choisissez le même emplacement que votre application App Service. |
| Niveau tarifaire | Pour obtenir des informations sur les tarifs, consultez [Tarification d’Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/). |
| Stratégies d’accès| Définit les applications et l’accès autorisé aux ressources du coffre. Vous pouvez configurer ce paramètre ultérieurement, en suivant les étapes décrites dans [Attribuer une stratégie d’accès Key Vault](../key-vault/general/assign-access-policy-portal.md). |
| Accès au réseau virtuel | Limitez l’accès au coffre à certains réseaux virtuels Azure. Vous pouvez configurer ce paramètre ultérieurement, en suivant les étapes décrites dans [Configurer les pare-feux et réseaux virtuels d’Azure Key Vault](../key-vault/general/network-security.md) |

Une fois que vous avez sélectionné le coffre, fermez la page **Référentiel Key Vault**. L’option **Étape 1 : Stocker** doit afficher une coche verte de réussite. Gardez cette page ouverte pour l’étape suivante.

> [!NOTE]
> Actuellement, App Service Certificate prend en charge uniquement la stratégie d’accès Key Vault, mais pas le modèle RBAC.
>

### <a name="verify-domain-ownership"></a>Vérifier la propriété du domaine

Dans la même page **Configuration du certificat** utilisée à l’étape précédente, cliquez sur **Étape 2 : Vérifier**.

![Vérifier le domaine du certificat App Service](./media/configure-ssl-certificate/verify-domain.png)

Sélectionnez **Vérification App Service**. Étant donné que vous avez déjà mappé le domaine à votre application web (voir la section [Prérequis](#prerequisites)), le domaine a déjà été vérifié. Cliquez simplement sur **Vérifier** pour terminer cette étape. Cliquez sur le bouton **Actualiser** jusqu’à ce que le message **Le domaine du certificat a été vérifié** s’affiche.

> [!NOTE]
> Quatre types de méthodes de vérification du domaine sont pris en charge : 
> 
> - **App Service** : option la plus pratique quand le domaine a déjà été mappé à une application App Service dans le même abonnement. Elle profite du fait que l’application App Service a déjà vérifié la propriété du domaine.
> - **Domaine** : permet de vérifier un [domaine App Service que vous avez acheté sur Azure](manage-custom-dns-buy-domain.md). Azure ajoute automatiquement l’enregistrement TXT de vérification pour vous et effectue le processus.
> - **E-mail** : permet de vérifier le domaine en envoyant un e-mail à l’administrateur de domaine. Quand vous sélectionnez cette option, des instructions supplémentaires s’affichent.
> - **Manuelle** : permet de vérifier le domaine à l’aide d’une page HTML (certificat **Standard** uniquement) ou d’un enregistrement TXT DNS. Quand vous sélectionnez cette option, des instructions supplémentaires s’affichent. L’option de page HTML ne fonctionne pas avec les applications web pour lesquelles « HTTPS uniquement » est activé.

### <a name="import-certificate-into-app-service"></a>Importer le certificat dans App Service

Sur le <a href="https://portal.azure.com" target="_blank">portail Azure</a>, dans le menu de gauche, sélectionnez **App Services** >  **\<app-name>** .

Dans le panneau de navigation de gauche de votre application, sélectionnez **Paramètres TLS/SSL** > **Certificats à clé privée (.pfx)**  > **Importer un certificat App Service**.

![Importer un certificat App Service dans App Service](./media/configure-ssl-certificate/import-app-service-cert.png)

Sélectionnez le certificat que vous venez d’acheter, puis sélectionnez **OK**.

Une fois l’opération terminée, le certificat s’affiche dans la liste **Certificats à clé privée**.

![Importation du certificat App Service terminée](./media/configure-ssl-certificate/import-app-service-cert-finished.png)

> [!IMPORTANT] 
> Pour sécuriser un domaine personnalisé avec ce certificat, vous devez quand même créer une liaison de certificat. Suivez les étapes fournies dans [Créer une liaison](configure-ssl-bindings.md#create-binding).
>

## <a name="import-a-certificate-from-key-vault"></a>Importer un certificat à partir de Key Vault

Si vous utilisez Azure Key Vault pour gérer vos certificats, vous pouvez importer un certificat PKCS12 à partir de Key Vault dans App Service tant qu’il [répond aux exigences](#private-certificate-requirements).

### <a name="authorize-app-service-to-read-from-the-vault"></a>Autoriser App Service à lire les données du coffre
Par défaut, le fournisseur de ressources App Service n’a pas accès au coffre de clés. Si vous souhaitez utiliser un coffre de clés pour un déploiement de certificats, vous devez [autoriser le fournisseur de ressources à accéder en lecture aux données du coffre de clés](../key-vault/general/assign-access-policy-cli.md). 

`abfa0a7c-a6b6-4736-8310-5855508787cd` est le nom du principal de service du fournisseur de ressources App Service, et il est identique pour tous les abonnements Azure. Pour un environnement cloud Azure Government, utilisez `6a02c803-dafd-4136-b4c3-5a6f318b4714` plutôt que le nom du principal de service du fournisseur de ressources.

> [!NOTE]
> Actuellement, Key Vault Certificate prend uniquement en charge la stratégie d’accès Key Vault, mais pas le modèle RBAC.
> 

### <a name="import-a-certificate-from-your-vault-to-your-app"></a>Importer un certificat dans votre application à partir de votre coffre

Sur le <a href="https://portal.azure.com" target="_blank">portail Azure</a>, dans le menu de gauche, sélectionnez **App Services** >  **\<app-name>** .

Dans le panneau de navigation de gauche de votre application, sélectionnez **Paramètres TLS/SSL** > **Certificats à clé privée (.pfx)**  > **Importer un certificat Key Vault**.

![Importer un certificat Key Vault dans App Service](./media/configure-ssl-certificate/import-key-vault-cert.png)

Aidez-vous du tableau suivant pour sélectionner le certificat.

| Paramètre | Description |
|-|-|
| Abonnement | Abonnement auquel appartient le coffre de clés. |
| Key Vault | Coffre contenant le certificat que vous souhaitez importer. |
| Certificat | Dans le coffre, sélectionnez un certificat PKCS12 dans la liste. Tous les certificats PKCS12 du coffre sont listés avec leurs empreintes numériques, mais tous ne sont pas pris en charge dans App Service. |

Une fois l’opération terminée, le certificat s’affiche dans la liste **Certificats à clé privée**. Si l’importation échoue avec une erreur, c’est que le certificat ne répond pas aux [exigences App Service](#private-certificate-requirements).

![Importation du certificat Key Vault terminée](./media/configure-ssl-certificate/import-app-service-cert-finished.png)

> [!NOTE]
> Si vous remplacez votre certificat par un nouveau certificat dans le coffre de clés, App Service synchronise automatiquement votre certificat sous 24 heures.

> [!IMPORTANT] 
> Pour sécuriser un domaine personnalisé avec ce certificat, vous devez quand même créer une liaison de certificat. Suivez les étapes fournies dans [Créer une liaison](configure-ssl-bindings.md#create-binding).
>

## <a name="upload-a-private-certificate"></a>Téléchargement d’un certificat privé

Une fois que vous avez obtenu un certificat de votre fournisseur de certificats, effectuez les étapes décrites dans cette section afin de préparer le certificat pour App Service.

### <a name="merge-intermediate-certificates"></a>Fusionner les certificats intermédiaires

Si votre autorité de certification vous donne plusieurs certificats dans la chaîne, vous devez les fusionner dans l’ordre.

Pour ce faire, ouvrez chaque certificat reçu dans un éditeur de texte.

Créez un fichier pour le certificat fusionné, appelé _mergedcertificate.crt_. Dans un éditeur de texte, copiez le contenu de chaque certificat dans ce fichier. L’ordre de vos certificats devrait suivre l’ordre dans la chaîne d’approbation, commençant par votre certificat et finissant par le certificat racine. Cela ressemble à l’exemple suivant :

```
-----BEGIN CERTIFICATE-----
<your entire Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<The entire Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-to-pfx"></a>Exportation du certificat vers PFX

Exportez votre certificat TLS/SSL fusionné avec la clé privée ayant servi à générer votre demande de certificat.

Si vous avez généré votre demande de certificat à l’aide d’OpenSSL, vous avez créé un fichier de clé privée. Pour exporter votre certificat au format PFX, exécutez la commande suivante : Remplacez les espaces réservés _&lt;fichier de clé privée>_ et _&lt;fichier de certificat fusionné>_ par les chemins d’accès menant à votre clé privée et à votre fichier de certificat fusionné.

```bash
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

Lorsque vous y êtes invité, définissez un mot de passe d’exportation. Vous allez utiliser ce mot de passe lors du chargement de votre certificat TLS/SSL dans App Service.

Si vous avez utilisé IIS ou _Certreq.exe_ pour générer votre demande de certificat, installez le certificat sur votre ordinateur local, puis [exportez le certificat au format PFX](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754329(v=ws.11)).

### <a name="upload-certificate-to-app-service"></a>Charger le certificat dans App Service

Vous êtes maintenant prêt à charger le certificat dans App Service.

Sur le <a href="https://portal.azure.com" target="_blank">portail Azure</a>, dans le menu de gauche, sélectionnez **App Services** >  **\<app-name>** .

Dans le panneau de navigation de gauche de votre application, sélectionnez **Paramètres TLS/SSL** > **Certificats à clé privée (.pfx)**  > **Charger le certificat**.

![Charger un certificat privé dans App Service](./media/configure-ssl-certificate/upload-private-cert.png)

Dans **Fichier de certificat PFX**, sélectionnez votre fichier PFX. Dans **Mot de passe du certificat**, tapez le mot de passe que vous avez créé lors de l’exportation du fichier PFX. Lorsque vous avez terminé, cliquez sur **Charger**. 

Une fois l’opération terminée, le certificat s’affiche dans la liste **Certificats à clé privée**.

![Chargement du certificat terminé](./media/configure-ssl-certificate/create-free-cert-finished.png)

> [!IMPORTANT] 
> Pour sécuriser un domaine personnalisé avec ce certificat, vous devez quand même créer une liaison de certificat. Suivez les étapes fournies dans [Créer une liaison](configure-ssl-bindings.md#create-binding).

## <a name="upload-a-public-certificate"></a>Téléchargement d’un certificat public

Les certificats publics sont pris en charge au format *.cer*. 

Sur le <a href="https://portal.azure.com" target="_blank">portail Azure</a>, dans le menu de gauche, sélectionnez **App Services** >  **\<app-name>** .

Dans le panneau de navigation de gauche de votre application, cliquez sur **Paramètres TLS/SSL** > **Certificats publics (.cer)**  > **Charger un certificat à clé publique**.

Dans **Nom**, tapez un nom pour le certificat. Dans **Fichier de certificat CER**, sélectionnez votre fichier CER.

Cliquez sur **Télécharger**.

![Charger un certificat public dans App Service](./media/configure-ssl-certificate/upload-public-cert.png)

Une fois le certificat chargé, copiez l’empreinte du certificat, puis reportez-vous à la section [Rendre le certificat accessible](configure-ssl-certificate-in-code.md#make-the-certificate-accessible).

## <a name="renew-an-expiring-certificate"></a>Renouveler un certificat sur le point d’expirer

Avant l’expiration d’un certificat, vous devez ajouter le certificat renouvelé dans App Service et mettre à jour toute [liaison TLS/SSL](configure-ssl-certificate.md). Le processus dépend du type de certificat. Par exemple, un [certificat importé à partir de Key Vault](#import-a-certificate-from-key-vault), y compris un [certificat App Service](#import-an-app-service-certificate), se synchronise automatiquement avec App Service toutes les 24 heures et met à jour la liaison TLS/SSL lorsque vous renouvelez le certificat. Pour un [certificat chargé](#upload-a-private-certificate), il n’existe aucune mise à jour de liaison automatique. Consultez l’une des sections suivantes en fonction de votre scénario :

- [Renouveler un certificat chargé](#renew-an-uploaded-certificate)
- [Renouveler un certificat App Service](#renew-an-app-service-certificate)
- [Renouveler un certificat importé d’Azure Key Vault](#renew-a-certificate-imported-from-key-vault)

### <a name="renew-an-uploaded-certificate"></a>Renouveler un certificat chargé

Pour remplacer un certificat arrivant à expiration, la façon dont vous mettez à jour la liaison de certificat avec le nouveau certificat peut nuire à l’expérience utilisateur. Par exemple, votre adresse IP entrante peut être modifiée lorsque vous supprimez une liaison, même si cette liaison repose sur une adresse IP. Cela est particulièrement important lorsque vous renouvelez un certificat qui se trouve déjà dans une liaison reposant sur une adresse IP. Pour éviter une modification de l’adresse IP de votre application, et éviter les temps d’arrêt de votre application en raison d’erreurs HTTPS, suivez ces étapes dans l’ordre :

1. [Chargez le nouveau certificat](#upload-a-private-certificate).
2. [Liez le nouveau certificat au même domaine personnalisé](configure-ssl-bindings.md) sans supprimer le certificat existant (arrivant à expiration). Cette action remplace la liaison plutôt que de supprimer la liaison du certificat existant.
3. Supprimez le certificat existant.

### <a name="renew-an-app-service-certificate"></a>Renouveler un certificat App Service

> [!NOTE]
> Depuis le 23 septembre 2021, les certificats App Service nécessitent une validation de domaine tous les 395 jours. Cela est dû à une nouvelle recommandation implémentée par le CA/Browser Forum, à laquelle les autorités de certification doivent se conformer. 
> 
> Contrairement au certificat managé App Service, la revalidation de domaine pour App Service Certificate NE sera PAS automatisée. Consultez [Vérifier la propriété du domaine](#verify-domain-ownership) pour plus d’informations sur la vérification de votre certificat App Service.

> [!NOTE]
> Le processus de renouvellement exige que [le principal de service connu pour App Service dispose des autorisations requises sur votre coffre de clés](deploy-resource-manager-template.md#deploy-web-app-certificate-from-key-vault). Cette autorisation est configurée pour vous lorsque vous importez un App Service Certificate via le portail et ne doit pas être supprimée de votre coffre de clés.

Pour activer/désactiver à tout moment le paramètre de renouvellement automatique de votre certificat App Service, sélectionnez le certificat dans la page [Certificats App Service](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders), puis cliquez sur **Paramètres de renouvellement automatique** dans le volet de navigation de gauche. Par défaut, les certificats App Service sont valides pendant 1 an.

Sélectionnez **Activé** ou **Désactivé** et cliquez sur **Enregistrer**. Les certificats peuvent être automatiquement renouvelés 31 jours avant leur expiration si vous avez activé le renouvellement automatique.

![Renouveler automatiquement un certificat App Service](./media/configure-ssl-certificate/auto-renew-app-service-cert.png)

Pour renouveler manuellement le certificat, cliquez sur **Renouvellement manuel**. Vous pouvez demander le renouvellement manuellement de votre certificat 60 jours avant expiration.

Une fois l’opération de renouvellement terminée, cliquez sur **Synchronisation**. L’opération de synchronisation met à jour automatiquement les liaisons de nom d’hôte pour le certificat dans App Service sans perturber le fonctionnement de vos applications.

> [!NOTE]
> Si vous ne cliquez pas sur **Synchronisation**, App Service synchronise automatiquement votre certificat sous 24 heures.

### <a name="renew-a-certificate-imported-from-key-vault"></a>Renouveler un certificat importé d’Azure Key Vault

Pour renouveler un certificat que vous avez importé dans App Service à partir de Key Vault, consultez [Renouveler votre certificat Azure Key Vault](../key-vault/certificates/overview-renew-certificate.md).

Une fois le certificat renouvelé dans votre coffre de clés, App Service synchronise automatiquement le nouveau certificat et met à jour toute liaison TLS/SSL applicable dans les 24 heures. Pour effectuer une synchronisation manuelle :

1. Accédez à la page des **Paramètres TLS/SSL** de votre application.
1. Sélectionnez le certificat importé sous **Certificats de clé privée**.
1. Cliquez sur **Synchroniser**. 

## <a name="manage-app-service-certificates"></a>Gérer les certificats App Service

Cette section vous montre comment gérer le [certificat App Service que vous avez acheté](#import-an-app-service-certificate).

- [Recréer la clé du certificat](#rekey-certificate)
- [Exporter le certificat](#export-certificate)
- [Supprimer le certificat](#delete-certificate)

Consultez également [Renouveler un certificat App Service](#renew-an-app-service-certificate)

### <a name="rekey-certificate"></a>Recréer la clé du certificat

Si vous pensez que la clé privée de votre certificat est compromise, vous pouvez recréer la clé de votre certificat. Sélectionnez le certificat dans la page [App Service Certificates](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders), puis sélectionnez **Recréer la clé et synchroniser** dans le volet de navigation de gauche.

Cliquez sur le bouton **Recréer la clé** pour lancer le processus. Ce processus peut prendre de 1 à 10 minutes.

![Recréer la clé d’un certificat App Service](./media/configure-ssl-certificate/rekey-app-service-cert.png)

Le renouvellement de la clé de votre certificat remplace le certificat par un nouveau certificat émis par l’autorité de certification.

Une fois l’opération de recréation de clé terminée, cliquez sur **Synchronisation**. L’opération de synchronisation met à jour automatiquement les liaisons de nom d’hôte pour le certificat dans App Service sans perturber le fonctionnement de vos applications.

> [!NOTE]
> Si vous ne cliquez pas sur **Synchronisation**, App Service synchronise automatiquement votre certificat sous 24 heures.

### <a name="export-certificate"></a>Exportation du certificat

Étant donné qu’un certificat App Service est un [secret Key Vault](../key-vault/general/about-keys-secrets-certificates.md), vous pouvez exporter une copie de ce fichier PFX et l’utiliser pour d’autres services Azure ou en dehors d’Azure.

Pour exporter le certificat App Service dans un fichier PFX, exécutez les commandes suivantes dans [Cloud Shell](https://shell.azure.com). Vous pouvez également l’exécuter localement [si vous avez installé Azure CLI](/cli/azure/install-azure-cli). Remplacez les espaces réservés par les noms que vous avez utilisés [lorsque vous avez créé le certificat App Service](#start-certificate-order).

```azurecli-interactive
secretname=$(az resource show \
    --resource-group <group-name> \
    --resource-type "Microsoft.CertificateRegistration/certificateOrders" \
    --name <app-service-cert-name> \
    --query "properties.certificates.<app-service-cert-name>.keyVaultSecretName" \
    --output tsv)

az keyvault secret download \
    --file appservicecertificate.pfx \
    --vault-name <key-vault-name> \
    --name $secretname \
    --encoding base64
```

Le fichier *appservicecertificate.pfx* téléchargé est un fichier PKCS12 brut qui contient à la fois des certificats publics et des certificats privés. Dans chaque invite, utilisez une chaîne vide pour le mot de passe d’importation et la phrase secrète PEM.

### <a name="delete-certificate"></a>Suppression d'un certificat 

La suppression d’un certificat App Service est définitive et irréversible. La suppression d’une ressource App Service Certificate entraîne la révocation du certificat. Dans App Service, les liaisons vers ce certificat deviennent alors non valides. Pour éviter toute suppression accidentelle, Azure place un verrou sur le certificat. Pour supprimer un certificat App Service, vous devez d’abord supprimer son verrou de suppression.

Sélectionnez le certificat dans la page [App Service Certificates](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders), puis sélectionnez **Verrous** dans le volet de navigation de gauche.

Dans votre certificat, recherchez le verrou de type **Supprimer**. À droite de celui-ci, sélectionnez **Supprimer**.

![Verrou de suppression du certificat App Service](./media/configure-ssl-certificate/delete-lock-app-service-cert.png)

Vous pouvez maintenant supprimer le certificat App Service. Dans le volet de navigation de gauche, sélectionnez **Vue d’ensemble** > **Supprimer**. Dans la boîte de dialogue de confirmation, tapez le nom du certificat, puis sélectionnez **OK**.

## <a name="automate-with-scripts&quot;></a>Automatiser des tâches à l’aide de scripts

### <a name=&quot;azure-cli&quot;></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 &quot;Bind a custom TLS/SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom TLS/SSL certificate to a web app")]

## <a name="more-resources"></a>Plus de ressources

* [Sécuriser un nom DNS personnalisé avec une liaison TLS/SSL dans Azure App Service](configure-ssl-bindings.md)
* [Appliquer le protocole HTTPS](configure-ssl-bindings.md#enforce-https)
* [Appliquer le protocole TLS 1.1/1.2](configure-ssl-bindings.md#enforce-tls-versions)
* [Utiliser un certificat TLS/SSL dans votre code dans Azure App Service](configure-ssl-certificate-in-code.md)
* [FORUM AUX QUESTIONS : App Service Certificates](./faq-configuration-and-management.yml)
