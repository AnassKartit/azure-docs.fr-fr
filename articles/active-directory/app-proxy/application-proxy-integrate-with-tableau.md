---
title: Proxy d’application Azure Active Directory et Tableau
description: Découvrez comment utiliser Proxy d’application Azure Active Directory (Azure AD) pour fournir un accès à distance à votre déploiement de Tableau.
services: active-directory
author: kenwith
manager: karenh444
ms.service: active-directory
ms.subservice: app-proxy
ms.workload: identity
ms.topic: how-to
ms.date: 04/28/2021
ms.author: kenwith
ms.reviewer: ashishj
ms.openlocfilehash: d7634858c1cc44d2a04847b7fef39eed588280ee
ms.sourcegitcommit: 611b35ce0f667913105ab82b23aab05a67e89fb7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2021
ms.locfileid: "129989244"
---
# <a name="azure-active-directory-application-proxy-and-tableau"></a>Proxy d’application Azure Active Directory et Tableau 

Proxy d’application Azure Active Directory et Tableau se sont associés afin de vous permettre d’utiliser facilement Proxy d’application pour fournir un accès à distance à votre déploiement Tableau. Cet article explique comment configurer ce scénario.  

## <a name="prerequisites"></a>Prérequis 

Le scénario décrit dans cet article suppose ce qui suit :

- [Tableau](https://onlinehelp.tableau.com/current/server/en-us/proxy.htm#azure) est configuré. 

- Vous avez installé un [connecteur Proxy d’application](../app-proxy/application-proxy-add-on-premises-application.md). 

 
## <a name="enabling-application-proxy-for-tableau"></a>Activation de Proxy d’application pour Tableau 

Le Proxy d’application prend en charge le flux OAuth 2.0 Grant, qui est nécessaire pour que Tableau fonctionne correctement. Cela signifie qu’il n’y a plus d’étapes spécifiques nécessaires pour activer cette application, autre que le fait de le configurer en suivant les étapes de publication ci-dessous.


## <a name="publish-your-applications-in-azure"></a>Publier vos applications dans Azure 

Pour publier Tableau, vous devez publier une application dans le portail Azure.

Pour :

- des instructions détaillées des étapes 1 à 8, consultez [Publication d’applications à l’aide du proxy d’application Azure AD](../app-proxy/application-proxy-add-on-premises-application.md) ; 
- des informations sur la façon de rechercher des valeurs Tableau pour des champs Proxy d’application, consultez la documentation de Tableau.  

**Pour publier votre application** : 


1. Connectez-vous au [Portail Microsoft Azure](https://portal.azure.com) en tant qu’administrateur d’application. 

2. Sélectionnez **Azure Active Directory > Applications d’entreprise**. 

3. Sélectionnez **Ajouter** en haut du panneau. 

4. Sélectionnez **On-premises application (Application locale)** . 

5. Saisissez les informations concernant votre nouvelle application dans les champs requis. Suivez les conseils ci-dessous pour les paramètres : 

    - **URL interne** : Cette application doit avoir une URL interne qui est l’URL Tableau proprement dite. Par exemple : `https://adventure-works.tableau.com`. 

    - **Méthode de pré-authentification** : Azure Active Directory (recommandée mais pas obligatoire). 

6. Sélectionnez **Ajouter** en haut du panneau. Votre application est ajoutée et le menu de démarrage rapide s’ouvre. 

7. Dans le menu de démarrage rapide, sélectionnez **Assign a user for testing (Attribuer un utilisateur à des fins de test)** , et ajoutez au moins un utilisateur à l’application. Vérifiez que ce compte de test a accès à l’application locale. 

8. Sélectionnez **Affecter** pour enregistrer l’affectation de l’utilisateur de test. 

9. (Facultatif) Dans la page de gestion de l’application, sélectionnez **Authentification unique**. Choisissez **Authentification Windows intégrée** dans le menu déroulant et renseignez les champs obligatoires en fonction de votre configuration de Tableau. Sélectionnez **Enregistrer**. 

 

## <a name="testing"></a>Test 

Votre application est maintenant prête à être testée. Accédez à l’URL externe que vous avez utilisée pour publier Tableau et connectez-vous en tant qu’utilisateur affecté aux deux applications.



## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur Proxy d’application Azure AD, consultez [Offrir un accès à distance sécurisé aux applications locales](application-proxy.md).

