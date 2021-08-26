---
title: Résoudre les problèmes de démarrage des rôles | Microsoft Docs
description: Vous trouverez ici quelques raisons courantes des problèmes de démarrage d’un rôle de service cloud. Nous vous présentons également des solutions à ces problèmes.
ms.topic: article
ms.service: cloud-services
ms.date: 10/14/2020
author: hirenshah1
ms.author: hirshah
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 509c44f83940ad1a79f5c30c0bb37084dae699b4
ms.sourcegitcommit: d11ff5114d1ff43cc3e763b8f8e189eb0bb411f1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2021
ms.locfileid: "122823866"
---
# <a name="troubleshoot-azure-cloud-service-classic-roles-that-fail-to-start"></a>Résoudre les problèmes de démarrage des rôles Azure Cloud Services (classique)

[!INCLUDE [Cloud Services (classic) deprecation announcement](includes/deprecation-announcement.md)]

Voici des solutions à quelques problèmes courants liés à l’échec du démarrage des rôles Azure Cloud Services.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>DLL ou dépendances manquantes
Les rôles qui ne répondent pas ou qui basculent sans cesse entre les états **Initialisation**, **Occupé** et **Arrêt** peuvent être affectés par l’absence de bibliothèques DLL ou d’assemblys.

Symptômes liés à l’absence de bibliothèques DLL ou d’assemblys :

* Votre instance de rôle alterne entre les états **Initialisation**, **Occupé** et **Arrêt**.
* Votre instance de rôle est passée à l’état **Prêt** , mais la page ne s’affiche pas lorsque vous accédez à votre application web.

Plusieurs méthodes sont recommandées pour rechercher une solution à ces problèmes.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Diagnostiquer les problèmes de DLL manquante dans un rôle web
Lorsque vous accédez à un site web déployé dans un rôle web et que le navigateur affiche une erreur de serveur semblable à ce qui suit, cela peut indiquer qu’une bibliothèque DLL est manquante.

![Erreur de serveur dans l’application « / ».](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Diagnostiquer les problèmes en désactivant les erreurs personnalisées
Vous pouvez afficher des informations plus complètes sur les erreurs en configurant le fichier web.config du rôle web pour qu’il désactive le mode d’erreur personnalisée et en redéployant le service.

Pour afficher des erreurs plus détaillées sans utiliser le Bureau à distance :

1. Ouvrez la solution dans Microsoft Visual Studio.
2. Dans l’ **Explorateur de solutions**, recherchez et ouvrez le fichier web.config.
3. Dans le fichier web.config, recherchez la section system.web et ajoutez la ligne suivante :

    ```xml
    <customErrors mode="Off" />
    ```
4. Enregistrez le fichier .
5. Recréez le package et redéployez le service.

Une fois le service redéployé, un message d’erreur s’affiche avec le nom de la DLL ou de l’assembly manquant.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Diagnostiquer les problèmes en affichant l’erreur à distance
Vous pouvez utiliser le Bureau à distance pour accéder au rôle et afficher des informations plus complètes sur l’erreur à distance. Pour afficher les erreurs à l’aide du Bureau à distance, procédez comme suit :

1. Assurez-vous que le Kit de développement logiciel (SDK) Azure 1.3 ou ultérieur est installé.
2. Lors du déploiement de la solution avec Visual Studio, activez le Bureau à distance. Pour plus d’informations, consultez la page [Activer la Connexion Bureau à distance pour un rôle dans Azure Cloud Services avec Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md).
3. Dans le portail Microsoft Azure, une fois que l’instance affiche l’état **Prêt**, accédez à distance à l’instance. Pour plus d’informations sur l’utilisation du bureau à distance avec les Services cloud, consultez [Accéder à distance aux instances de rôles](cloud-services-role-enable-remote-desktop-new-portal.md#remote-into-role-instances).
5. Connectez-vous à la machine virtuelle à l’aide des informations d’identification spécifiées lors de la configuration du Bureau à distance.
6. Ouvrez une fenêtre de commandes.
7. Tapez `IPconfig`.
8. Notez la valeur de l’adresse IPV4.
9. Ouvrez Internet Explorer.
10. Entrez l’adresse et le nom de l’application Web. Par exemple : `http://<IPV4 Address>/default.aspx`.

L’accès au site web renvoie maintenant des messages d’erreur plus explicites :

* Erreur de serveur dans l’application « / ».
* Description : une exception non gérée s’est produite lors de l’exécution de la requête Web en cours. Veuillez consulter l’arborescence des appels de procédure pour plus d’informations sur l’erreur et sa source dans le code.
* Détails de l’exception : System.IO.FIleNotFoundException : impossible de charger le fichier ou l’assembly « Microsoft.WindowsAzure.StorageClient, Version=1.1.0.0, Culture=neutral, PublicKeyToken=31bf856ad364e35 » ou l’une de ses dépendances. Le système ne peut pas trouver le fichier spécifié.

Par exemple :

![Erreur de serveur explicite dans l’application « / »](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Diagnostiquer les problèmes à l’aide de l’émulateur de calcul
Vous pouvez utiliser l’émulateur de calcul Microsoft Azure pour diagnostiquer et résoudre les problèmes de dépendances manquantes et les erreurs du fichier web.config.

Pour obtenir de meilleurs résultats à l’aide de cette méthode de diagnostic, vous devez utiliser un ordinateur ou une machine virtuelle disposant d’une nouvelle installation de Windows. Pour mieux simuler l’environnement Azure, utilisez Windows Server 2008 R2 x64.

1. Installez la version autonome du [SDK Azure](https://azure.microsoft.com/downloads/).
2. Sur l’ordinateur de développement, générez le projet de service cloud.
3. Dans l’Explorateur Windows, accédez au dossier bin\debug du projet de service cloud.
4. Copiez le dossier .csx et le fichier .cscfg sur l’ordinateur que vous utilisez pour déboguer les problèmes.
5. Sur le nouvel ordinateur, ouvrez une fenêtre d’invite de commandes du Kit de développement logiciel (SDK) Azure et tapez `csrun.exe /devstore:start`.
6. À l’invite de commandes, tapez `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.
7. Au démarrage du rôle, le détail de l’erreur s’affiche dans Internet Explorer. Vous pouvez également utiliser les outils de dépannage Windows standard pour diagnostiquer le problème.

## <a name="diagnose-issues-by-using-intellitrace"></a>Diagnostiquer les problèmes à l’aide d’IntelliTrace
Pour les rôles de travail et les rôles web qui utilisent .NET Framework 4, vous pouvez utiliser l’utilitaire [IntelliTrace](/visualstudio/debugger/intellitrace), qui est disponible dans Microsoft Visual Studio Enterprise.

Pour déployer le service avec la fonction IntelliTrace activée, procédez comme suit :

1. Vérifiez que le Kit de développement logiciel (SDK) Azure 1.3 ou ultérieur est installé.
2. Déployez la solution à l’aide de Visual Studio. Au cours du déploiement, cochez la case **Activer IntelliTrace pour les rôles .NET 4** .
3. Une fois l’instance démarrée, ouvrez l’ **Explorateur de serveurs**.
4. Développez le nœud **Azure\\Cloud Services** et recherchez le déploiement.
5. Développez le déploiement jusqu’à ce que les instances de rôle s’affichent. Cliquez avec le bouton droit sur l’une des instances.
6. Sélectionnez **Afficher les fichiers journaux d’activité IntelliTrace**. Le **Résumé IntelliTrace** s’ouvre.
7. Recherchez la section du résumé relative aux exceptions. S’il existe des exceptions, la section s’intitule **Données d’exception**.
8. Développez les **Données d’exception** et recherchez les erreurs **System.IO.FileNotFoundException** semblables à ce qui suit :

![Données d’exception, fichier ou assembly manquant](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Résoudre les problèmes de DLL et d’assemblys manquants
Pour résoudre les erreurs liées à des DLL et des assemblies manquants, procédez comme suit :

1. Ouvrez la solution dans Visual Studio.
2. Dans l’**Explorateur de solutions**, ouvrez le dossier **Références**.
3. Cliquez sur l’assembly identifié dans l’erreur.
4. Dans le volet **Propriétés**, recherchez la propriété **Copy Local** et définissez sa valeur sur **True**.
5. Redéployez le service cloud.

Après avoir vérifié que toutes les erreurs ont été corrigées, vous pouvez déployer le service sans cocher la case **Activer IntelliTrace pour les rôles .NET 4** .

## <a name="next-steps"></a>Étapes suivantes
Affichez plus d’ [articles de résolution des problèmes](../index.yml?product=cloud-services&tag=top-support-issue) liés aux services cloud.

Pour découvrir comment résoudre les problèmes liés aux rôles de service cloud à l’aide des données de diagnostic de calcul PaaS Azure, consultez la [série de blogs de Kevin Williamson](/archive/blogs/kwill/windows-azure-paas-compute-diagnostics-data).
