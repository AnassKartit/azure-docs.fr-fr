---
title: Adresses IP dans Azure Functions
description: Découvrez comment trouver les adresses IP entrantes et sortantes des applications de fonction, et ce qui les fait changer.
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: b332e979ad310134ce6633dbe3f23efc326ee847
ms.sourcegitcommit: e8b229b3ef22068c5e7cd294785532e144b7a45a
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/04/2021
ms.locfileid: "123479091"
---
# <a name="ip-addresses-in-azure-functions"></a>Adresses IP dans Azure Functions

Cet article aborde les concepts suivants relatifs aux adresses IP des applications de fonction :

* Localisation des adresses IP actuellement utilisées par une application de fonction.
* Conditions qui entraînent la modification des adresses IP des applications de fonction.
* Restriction des adresses IP pouvant accéder à une application de fonction.
* Définition d’adresses IP dédiées pour une application de fonction.

Les adresses IP sont associées à des applications de fonction, et non à des fonctions individuelles. Les requêtes HTTP entrantes ne peuvent pas utiliser l’adresse IP entrante pour appeler des fonctions individuelles ; elles doivent utiliser le nom de domaine par défaut (functionappname.azurewebsites.net) ou un nom de domaine personnalisé.

## <a name="function-app-inbound-ip-address"></a>Adresse IP entrante de l’application de fonction

Chaque application de fonction a une seule adresse IP entrante. Pour la trouver :

# <a name="azure-portal"></a>[Portail Azure](#tab/portal)

1. Connectez-vous au [portail Azure](https://portal.azure.com).
2. Accédez à l’application de fonction.
3. Sous **Paramètres**, sélectionnez **Propriétés**. L’adresse IP entrante apparaît sous **Adresse IP virtuelle**.

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

Utilisez l’utilitaire `nslookup` de votre ordinateur client local :

```command
nslookup <APP_NAME>.azurewebsites.net
```

---

## <a name="function-app-outbound-ip-addresses"></a><a name="find-outbound-ip-addresses"></a>Adresses IP sortantes de l’application de fonction

Chaque application de fonction a différentes adresses IP sortantes disponibles. Toute connexion sortante d’une fonction, par exemple, à une base de données principale, utilise l’une des adresses IP sortantes comme adresse IP d’origine. Il n’est pas possible de savoir à l’avance laquelle sera utilisée. C’est pourquoi le service principal doit ouvrir son pare-feu à toutes les adresses IP sortantes de l’application de fonction.

Pour trouver les adresses IP sortantes disponibles pour une application de fonction :

# <a name="azure-portal"></a>[Azure portal](#tab/portal)

1. Connectez-vous à [Azure Resource Explorer](https://resources.azure.com).
2. Sélectionnez **Abonnements > {votre abonnement} > Fournisseurs > Microsoft.Web > Sites**.
3. Dans le volet JSON, recherchez le site dont la propriété `id` se termine par le nom de votre application de fonction.
4. Localisez `outboundIpAddresses` et `possibleOutboundIpAddresses`. 

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

```azurecli-interactive
az functionapp show --resource-group <GROUP_NAME> --name <APP_NAME> --query outboundIpAddresses --output tsv
az functionapp show --resource-group <GROUP_NAME> --name <APP_NAME> --query possibleOutboundIpAddresses --output tsv
```
---

L’ensemble de `outboundIpAddresses` est actuellement accessible à l’application de fonction. L’ensemble de `possibleOutboundIpAddresses` comporte les adresses IP qui ne seront disponibles que si l’application de fonction [passe à d’autres niveaux tarifaires](#outbound-ip-address-changes).

> [!NOTE]
> Lorsqu’une application de fonction qui s’exécute sur le [plan de consommation](consumption-plan.md) ou le [plan Premium](functions-premium-plan.md) est mise à l’échelle, une nouvelle plage d’adresses IP sortantes peut être attribuée. En cas d’exécution sur l’un de ces plans, vous ne pouvez pas compter sur les adresses IP sortantes signalées pour créer une liste d'autorisation définitive. Pour pouvoir inclure toutes les adresses sortantes potentielles utilisées pendant la mise à l’échelle dynamique, vous devez ajouter l’ensemble du centre de données à votre liste d'autorisation.

## <a name="data-center-outbound-ip-addresses"></a>Adresses IP sortantes du centre de données

Si vous devez ajouter les adresses IP sortantes utilisées par vos applications de fonction à une liste d’autorisation, vous pouvez également ajouter le centre de données des applications de fonction (région Azure) à une liste d’autorisation. Vous pouvez [télécharger un fichier JSON qui liste les adresses IP de tous les centres de données Azure](https://www.microsoft.com/en-us/download/details.aspx?id=56519). Recherchez ensuite le fragment JSON qui s’applique à la région dans laquelle s’exécute votre application de fonction.

Par exemple, le fragment JSON suivant est ce à quoi pourrait ressembler la liste d’autorisation pour la région Europe Ouest :

```
{
  "name": "AzureCloud.westeurope",
  "id": "AzureCloud.westeurope",
  "properties": {
    "changeNumber": 9,
    "region": "westeurope",
    "platform": "Azure",
    "systemService": "",
    "addressPrefixes": [
      "13.69.0.0/17",
      "13.73.128.0/18",
      ... Some IP addresses not shown here
     "213.199.180.192/27",
     "213.199.183.0/24"
    ]
  }
}
```

 Pour savoir quand ce fichier est mis à jour et quand les adresses IP changent, développez la section **Détails** de la [page du Centre de téléchargement](https://www.microsoft.com/en-us/download/details.aspx?id=56519).

## <a name="inbound-ip-address-changes"></a><a name="inbound-ip-address-changes"></a>Changement d’adresse IP entrante

L’adresse IP entrante **peut** changer dans les cas suivants :

- vous supprimez une application de fonction, puis la recréez dans un autre groupe de ressources ;
- vous supprimez la dernière application de fonction dans une combinaison de groupe de ressources et de région, puis la recréez ;
- vous supprimez une liaison TLS, par exemple, pendant le [renouvellement des certificats](../app-service/configure-ssl-certificate.md#renew-an-expiring-certificate).

Lorsque votre application de fonction s’exécute dans un [plan de consommation](consumption-plan.md) ou un [plan Premium](functions-premium-plan.md), l’adresse IP entrante peut également changer, même quand vous n’avez effectué aucune des actions [répertoriées ci-dessus](#inbound-ip-address-changes).

## <a name="outbound-ip-address-changes"></a>Changement d’adresse IP sortante

La stabilité relative de l'adresse IP sortante dépend du plan d'hébergement.  

### <a name="consumption-and-premium-plans"></a>Plans de consommation et Premium

En raison des comportements de mise à l'échelle automatique, l'adresse IP sortante peut changer à tout moment en cas d'exécution sur un [plan de consommation](consumption-plan.md) ou un [plan Premium](functions-premium-plan.md). 

Si vous devez contrôler l'adresse IP sortante de votre application de fonction, par exemple pour l'ajouter à une liste d'autorisation, envisagez d'implémenter une [passerelle NAT de réseau virtuel](#virtual-network-nat-gateway-for-outbound-static-ip) lors de son exécution dans votre plan d'hébergement Premium. Vous pouvez également effectuer cette opération en l’exécutant dans un plan dédié (App Service).

### <a name="dedicated-plans"></a>Plans dédiés

En cas d'exécution sur des plans dédiés (App Service), l'ensemble des adresses IP sortantes disponibles pour une application de fonction peut changer lorsque :

* vous effectuez une action susceptible de modifier l’adresse IP entrante ;
* vous modifiez le niveau tarifaire de votre plan dédié (App Service). La liste de toutes les adresses IP sortantes utilisables par votre application, pour tous les niveaux tarifaires, est donnée dans la propriété `possibleOutboundIPAddresses`. Consultez [Trouver des adresses IP sortantes](#find-outbound-ip-addresses).

#### <a name="forcing-an-outbound-ip-address-change"></a>Imposer une modification d'adresse IP sortante

Utilisez la procédure suivante pour imposer délibérément une modification d'adresse IP sortante dans un plan dédié (App Service) :

1. Faites évoluer votre plan App Service entre les niveaux tarifaires Standard et Premium v2.

2. Attendez 10 minutes.

3. Revenez au niveau tarifaire de départ.

## <a name="ip-address-restrictions"></a>Restriction des adresses IP

Vous pouvez configurer la liste des adresses IP auxquelles vous souhaitez autoriser ou refuser l’accès à une application de fonction. Pour plus d’informations, consultez [Restrictions d’adresse IP statique avec Azure App Service](../app-service/app-service-ip-restrictions.md).

## <a name="dedicated-ip-addresses"></a>Adresses IP dédiées

Il existe plusieurs stratégies à explorer lorsque votre application de fonction requiert des adresses IP statiques et dédiées. 

### <a name="virtual-network-nat-gateway-for-outbound-static-ip"></a>Passerelle NAT de réseau virtuel pour les adresses IP statiques sortantes

Vous pouvez contrôler l’adresse IP du trafic sortant à partir de vos fonctions en utilisant une passerelle NAT de réseau virtuel pour diriger le trafic vers une IP publique statique. Vous pouvez utiliser cette topologie lors de l’exécution dans un [plan Premium](functions-premium-plan.md) ou un [plan dédié (App Service)](dedicated-plan.md). Poiur en savoir plus, consultez [Tutoriel : Contrôler l’adresse IP sortante Azure Functions avec une passerelle NAT de réseau virtuel Azure](functions-how-to-use-nat-gateway.md).

### <a name="app-service-environments"></a>Environnements App Service

Pour un contrôle total sur les adresses IP, entrantes et sortantes, nous recommandons les [environnements App Service Environment](../app-service/environment/intro.md) ([niveau Isolé](https://azure.microsoft.com/pricing/details/app-service/) des plans App service). Pour plus d’informations, voir [Adresses IP de l’environnement App Service](../app-service/environment/network-info.md#ase-ip-addresses) et [Guide pratique pour contrôler le trafic entrant dans un environnement App Service](../app-service/environment/app-service-app-service-environment-control-inbound-traffic.md).

Pour savoir si votre application de fonction s’exécute dans un environnement App Service :

# <a name="azure-porta"></a>[Portail Azure](#tab/portal)

1. Connectez-vous au [portail Azure](https://portal.azure.com).
2. Accédez à l’application de fonction.
3. Sélectionnez l’onglet **Vue d’ensemble**.
4. Le niveau du plan App Service apparaît sous **Niveau tarifaire/plan App Service**. Le niveau tarifaire de l’environnement App Service est **Isolé**.

# <a name="azure-cli"></a>[Azure CLI](#tab/azurecli)

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query sku --output tsv
```

---

L’environnement App Service `sku` est `Isolated`.

## <a name="next-steps"></a>Étapes suivantes

L’une des causes courantes des changements d’adresses IP est le changement d’échelle de l’application de fonction. [En savoir plus sur la mise à l’échelle des applications de fonction](functions-scale.md).
