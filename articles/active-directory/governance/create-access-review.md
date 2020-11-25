---
title: Créer une révision d’accès des groupes et applications - Azure AD
description: Découvrez comment créer une révision d’accès pour les membres de groupes ou pour l’accès aux applications dans les révisions d’accès Azure Active Directory.
services: active-directory
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 09/15/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 18f0627b809f56b813052cc763e6ff961f31aa02
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94697133"
---
# <a name="create-an-access-review-of-groups-and-applications-in-azure-ad-access-reviews"></a>Créer une révision d’accès des groupes et applications dans les révisions d’accès Azure AD

L’accès aux groupes et aux applications pour les employés et les invités change au fil du temps. Afin de réduire les risques associés aux affectations d’accès obsolètes, les administrateurs peuvent utiliser Azure Active Directory (Azure AD) pour créer des révision des accès pour les membres de groupes ou pour l’accès aux applications. Si vous devez régulièrement passer en revue les accès, vous pouvez aussi créer des révisions d’accès périodiques. Pour plus d’informations sur ces scénarios, consultez [Gérer l’accès des utilisateurs](manage-user-access-with-access-reviews.md) et [Gérer l’accès des invités](manage-guest-access-with-access-reviews.md).

Vous pouvez regarder une courte vidéo sur l’activation des révisions d’accès :

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

Cet article explique comment créer une ou plusieurs révisions d’accès pour les membres de groupes ou pour l’accès aux applications.

## <a name="prerequisites"></a>Prérequis

- Azure AD Premium P2
- Administrateur général ou Administrateur d’utilisateurs
- (Préversion) Les propriétaires de ressources de groupes Microsoft 365 peuvent créer des révisions sur les groupes Microsoft 365 qu’ils possèdent.
- (Préversion) Les propriétaires de ressources de groupes de sécurité Azure AD peuvent créer des révisions sur les groupes de sécurité Azure AD qu’ils possèdent.

Pour plus d’informations, consultez [Exigences des licences](access-reviews-overview.md#license-requirements).

## <a name="create-one-or-more-access-reviews"></a>Créer une ou plusieurs révisions d’accès

1. Connectez-vous au portail Azure et ouvrez la page [Identity Governance](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. Dans le menu de gauche, cliquez sur **Révisions d’accès**.

1. Cliquez sur **Nouvelle révision d’accès** pour créer une révision d’accès.

    ![Volet des révisions d’accès dans Gouvernance des identités](./media/create-access-review/access-reviews.png)

1. Nommez la révision d’accès. Si vous le souhaitez, vous pouvez fournir une description de cette révision. Les réviseurs ont accès au nom et à la description de la révision.

    ![Créer une révision d’accès - Nom et description de la révision](./media/create-access-review/name-description.png)

1. Définissez la **Date de début**. Par défaut, une révision d’accès se produit une seule fois, démarre le jour même de sa création et s’arrête un mois plus tard. Vous pouvez modifier les dates de début et de fin pour que la révision d’accès démarre ultérieurement et dure le nombre de jours souhaité.

    ![Créer une révision d’accès - Dates de début et de fin](./media/create-access-review/start-end-dates.png)

1. Pour que la révision d’accès soit périodique, modifiez le paramètre **Fréquence** en remplaçant la valeur **Une fois** par **Hebdomadaire**, **Mensuelle**, **Trimestrielle**, **Semestrielle** ou **Annuelle**. Utilisez la zone de texte ou le curseur **Durée** pour définir le nombre de jours pendant lequel chaque révision de la série périodique pourra être modifiée par les réviseurs. Par exemple, la durée maximale d’une révision mensuelle est de 27 jours, ce qui permet d’éviter le chevauchement des révisions.

1. Utilisez le paramètre **Fin** pour spécifier comment mettre fin à la série de révisions d’accès récurrentes. La série peut se terminer de trois façons : 
    1. Elle s’exécute en continu pour lancer des révisions indéfiniment.
    1. Elle s’exécute jusqu’à une date spécifique.
    1. Elle s’exécute jusqu’à la fin d’un nombre défini d’occurrences. 
  
    Un autre administrateur d’utilisateur, ou un autre administrateur d’entreprise ou vous-même pouvez arrêter la série après sa création en modifiant la date définie dans la zone **Paramètres**, afin qu’elle s’arrête à cette date.

1. Dans la section **Utilisateurs**, spécifiez les utilisateurs auxquels s’applique la révision d’accès. Les révisions d’accès peuvent porter sur les membres d’un groupe ou sur les utilisateurs qui ont été assignés à une application. Vous pouvez affiner davantage la révision d’accès pour passer en revue seulement les utilisateurs invités qui sont des membres (ou affectés à l’application) au lieu d’examiner tous les utilisateurs qui sont des membres ou qui ont accès à l’application.

    ![Créer une révision d’accès - Utilisateurs](./media/create-access-review/users.png)

1. Dans la section **Groupe**, sélectionnez un ou plusieurs groupes dont vous souhaitez vérifier l’appartenance.

    > [!NOTE]
    > La sélection de plusieurs groupes crée plusieurs révisions d’accès. Par exemple, en sélectionnant cinq groupes, vous créez cinq révisions d’accès distinctes.
    
    ![Créer une révision d’accès - Sélectionner un groupe](./media/create-access-review/select-group.png)

1. Dans la section **Applications** (si vous avez sélectionné **Affecté(e) à une application** à l’étape 8), sélectionnez les applications dont vous souhaitez vérifier les paramètres d’accès.

    > [!NOTE]
    > La sélection de plusieurs applications crée plusieurs révisions d’accès. Par exemple, en sélectionnant cinq applications, vous créez cinq révisions d’accès distinctes.
    
    ![Créer une révision d’accès - Sélectionner une application](./media/create-access-review/select-application.png)

1. Dans la section **Réviseurs**, sélectionnez une ou plusieurs personnes pour passer en revue tous les utilisateurs concernés. Sinon, vous pouvez choisir de faire en sorte que les membres vérifient leur propre accès. Si la ressource est un groupe, vous pouvez demander aux propriétaires de groupe d’effectuer la révision. Vous pouvez également demander à ce que les réviseurs fournissent un motif lorsqu’ils approuvent l’accès.

    ![Créer une révision d’accès - Réviseurs](./media/create-access-review/reviewers.png)

1. Dans la section **Programmes**, sélectionnez le programme que vous voulez utiliser. L’option **Programme par défaut** est toujours présente.

    ![Créer une révision d’accès - Programmes](./media/create-access-review/programs.png)

    Vous pouvez simplifier la collecte et le suivi des révisions d’accès en les organisant en programmes. Chaque révision d’accès peut être liée à un programme. Ainsi, lorsque vous préparez des rapports pour un auditeur, vous pouvez vous concentrer sur les révisions d’accès qui sont associées à une certaine initiative. Les programmes et les résultats des révisions d’accès sont visibles pour les utilisateurs disposant du rôle Administrateur général, Administrateur d’utilisateurs, Administrateur de la sécurité ou Lecteur Sécurité.

    Pour consulter une liste des programmes, accédez à la page des révisions d’accès et sélectionnez **Programmes**. Si vous disposez d’un rôle Administrateur général ou Administrateur d’utilisateurs, vous pouvez créer des programmes supplémentaires. Par exemple, vous pouvez choisir de disposer d’un programme pour chaque initiative de conformité ou objectif de l’entreprise. Lorsque vous n’avez plus besoin d’un programme et qu’il n’est lié à aucun contrôle, vous pouvez le supprimer.

### <a name="upon-completion-settings"></a>Paramètres de saisie semi-automatique

1. Pour spécifier ce qui se passe après la fin d’une révision, développez la section **Paramètres de saisie semi-automatique**.

    ![Créer une révision d’accès : paramètres de saisie semi-automatique](./media/create-access-review/upon-completion-settings-new.png)

2. Si vous voulez supprimer automatiquement l’accès pour les utilisateurs qui ont été refusés, définissez **Appliquer automatiquement les résultats à la ressource** sur **Activer**. Si vous voulez appliquer manuellement les résultats quand la révision est terminée, cliquez sur **Désactiver**.

3. Utilisez la liste **Si les réviseurs ne répondent pas** pour spécifier ce qui se passe pour les utilisateurs qui ne sont pas vérifiés par le réviseur au cours de la révision. Ce paramètre n’affecte pas les utilisateurs qui ont été vérifiés manuellement par les réviseurs. Si la décision finale du réviseur est Refuser, l’accès de l’utilisateur est supprimé.

    - **Aucune modification** : laisser l’accès de l’utilisateur inchangé
    - **Supprimer l’accès** : supprimer l’accès de l’utilisateur
    - **Approuver l’accès** : approuver l’accès de l’utilisateur
    - **Accepter les recommandations** : accepter la recommandation du système sur le refus ou l’approbation de la prolongation de l’accès de l’utilisateur

    ![Créer une révision d’accès : paramètres avancés](./media/create-access-review/advanced-settings-preview-new.png)

4. (Préversion) Utilisez l’Action à appliquer aux utilisateurs refusés pour spécifier ce qui arrive aux utilisateurs invités s’ils sont refusés.
    - **L’option 1** supprimera l’accès à l’utilisateur refusé au groupe ou à l’application en cours de révision ; il sera toujours en mesure de se connecter au locataire. 
    - **L’option 2** empêchera les utilisateurs non autorisés de se connecter au locataire, même s’ils ont accès à d’autres ressources. En cas d’erreur ou si un administrateur décide de réactiver l’accès, cela est possible dans les 30 jours suivant la désactivation de l’utilisateur. Si aucune action n’est effectuée sur les utilisateurs désactivés, ils seront supprimés du locataire.

Pour en savoir plus sur les meilleures pratiques pour la suppression des utilisateurs invités qui n’ont plus accès aux ressources de votre organisation, lisez l’article intitulé [Utiliser Azure AD Identity Governance pour examiner et supprimer les utilisateurs externes qui n’ont plus accès aux ressources.](access-reviews-external-users.md).

>[!NOTE]
> L’action à appliquer aux utilisateurs refusés ne fonctionne que si vous avez précédemment étendu une révision aux utilisateurs invités uniquement (voir la section **Créer une ou plusieurs révisions d’accès**, étape 8)

### <a name="advanced-settings"></a>Paramètres avancés

1. Pour spécifier d’autres paramètres, développez la section **Paramètres avancés**.

1. Définissez **Afficher les recommandations** sur **Activer** pour montrer aux réviseurs les recommandations du système basées sur les informations des accès de l’utilisateur.

1. Définissez **Exiger la raison lors de l’approbation** sur **Activer** afin d’exiger que le réviseur indique un motif pour l’approbation.

1. Définissez **Notifications par e-mail** sur **Activer** pour qu’Azure AD envoie des notifications par e-mail aux réviseurs quand une révision d’accès commence et aux administrateurs quand une révision se termine.

1. Définissez **Rappels** sur **Activer** pour qu’Azure AD envoie des rappels concernant les révisions d’accès en cours aux réviseurs qui n’ont pas terminé leur révision. 

    >[!NOTE]
    > Par défaut, Azure AD envoie automatiquement un rappel à mi-parcours de l'échéance fixée pour les réviseurs qui n'ont pas encore répondu.

1. (Préversion) Le contenu de l’e-mail envoyé aux réviseurs est généré automatiquement en fonction des détails de la révision, comme le nom de la révision, le nom de la ressource, la date d’échéance, etc. Si vous souhaitez communiquer des informations supplémentaires telles que des instructions ou des coordonnées, vous pouvez les ajouter dans l’**e-mail Contenu supplémentaire pour le réviseur** qui sera inclus dans les e-mails d’invitation et de rappel envoyés aux réviseurs désignés. Ces informations seront affichées dans la section en surbrillance ci-dessous.

    ![Réviser l'accès d'un utilisateur à un groupe](./media/create-access-review/review-users-access-group.png)

## <a name="start-the-access-review"></a>Démarrer la révision d’accès

Une fois que vous avez spécifié les paramètres pour une révision d’accès, cliquez sur **Démarrer**. La révision d’accès s’affiche dans votre liste, avec un indicateur de son état.

![Liste des révisions d’accès et de leur état](./media/create-access-review/access-reviews-list.png)

Par défaut, Azure AD envoie un e-mail aux réviseurs peu de temps après le démarrage de la révision. Si vous ne souhaitez pas qu’Azure AD envoie cet e-mail, veillez à informer les réviseurs qu’une révision d’accès leur a été assignée. Vous pouvez leur montrer les instructions relatives à la [révision d’accès aux groupes ou aux applications](perform-access-review.md). Si votre révision s’adresse à des invités qui doivent réviser leur propre accès, donnez-leur des instructions sur la méthode à suivre pour [réviser leur accès à des groupes ou à des applications](review-your-access.md).

Si vous avez attribué le rôle de réviseur à des clients invités, mais que ceux-ci ont refusé l’invitation, ils ne recevront pas d’e-mails des révisions d’accès, car il est nécessaire d’avoir accepté l’invitation pour pouvoir commencer la révision.

## <a name="access-review-status-table"></a>Tableau de l’état de la révision d’accès

| Statut | Définition |
|--------|------------|
|NotStarted | La révision a été créée, la détection des utilisateurs est en attente de démarrage. |
|Initialisation   | La détection des utilisateurs est en cours pour identifier tous les utilisateurs faisant partie de la révision. |
|Démarrage en cours | La révision démarre. Si les notifications par e-mail sont activées, des e-mails sont envoyés aux réviseurs. |
|InProgress | La révision a démarré. Si les notifications par e-mail sont activées, des e-mails ont été envoyés aux réviseurs. Les réviseurs peuvent soumettre des décisions jusqu’à la date d’échéance. |
|Completing | La révision est terminée et les e-mails sont envoyés au propriétaire de la révision. |
|Révision automatique | La révision se fait dans le cadre d’une phase de vérification du système. Le système enregistre des décisions pour les utilisateurs qui n’ont pas été révisés en fonction de recommandations ou de décisions préconfigurées. |
|Révisé automatiquement | Les décisions ont été enregistrées par le système pour tous les utilisateurs qui n’ont pas été révisés. La révision est prête à passer à l’étape **Applying** si l’application automatique est activée. |
|Applying | L’accès n’est pas modifié pour les utilisateurs qui ont été approuvés. |
|Applied | Les utilisateurs refusés, le cas échéant, ont été supprimés de la ressource ou du répertoire. |
|Échec | La révision n'a pas pu se poursuivre. Cette erreur peut être liée à la suppression du locataire, à une modification des licences ou à d'autres changements internes au niveau du locataire. |

## <a name="create-reviews-via-apis"></a>Créer des révisions via des API

Vous pouvez également créer des révisions d’accès avec des API. Ce que vous faites pour gérer les révisions d’accès des groupes et des utilisateurs d’applications dans le portail Azure peut également être effectué avec les API Microsoft Graph. Pour en savoir plus, voir [Informations de référence sur les API des révisions d’accès d’Azure AD](/graph/api/resources/accessreviews-root?view=graph-rest-beta). Pour obtenir un exemple de code, voir [Exemple de récupération de révisions d’accès Azure AD via Microsoft Graph](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/m-p/236096).

## <a name="next-steps"></a>Étapes suivantes

- [Réviser l’accès à des groupes ou à des applications](perform-access-review.md)
- [Réviser son accès à des groupes ou à des applications](review-your-access.md)
- [Effectuer une révision d’accès de groupes ou révisions d’accès des applications dans Azure AD](complete-access-review.md)