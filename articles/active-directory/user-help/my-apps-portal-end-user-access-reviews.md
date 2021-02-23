---
title: Gérer l’accès à votre organisation aux applications et groupes - Azure AD
description: Découvrez comment exécuter une révision d’accès pour gérer l’accès à la sécurité des applications et groupes de votre organisation à partir du portail Mes applications.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 01/19/2021
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 34885e2a364778a2f81f4920aa26aa3bb5f40320
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100095016"
---
# <a name="perform-an-access-review-from-the-my-apps-portal"></a>Effectuer une révision d’accès à partir du portail Mes applications

Vous pouvez utiliser votre compte professionnel ou scolaire avec le portail web **Mes applications** pour effectuer des révisions d’accès pour vos applications et vos groupes. Les révisions d’accès vous aident à gérer les accès obsolètes ou les changements d’exigences en matière d’accès et à vérifier qu’ils sont révisés et mis à jour.

Si vous n’avez pas accès au portail **Mes applications**, contactez le support technique pour obtenir l’autorisation.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-my-apps-portal.md)]

>[!Important]
>Ce contenu est destiné aux utilisateurs de **Mes applications**. Si vous êtes administrateur, vous trouverez des informations sur la configuration et la gestion de vos applications cloud dans la [documentation sur la gestion des applications](../manage-apps/index.yml).
>
> Si vous voyez une erreur lors de la connexion avec un compte Microsoft personnel, vous pouvez toujours vous connecter en utilisant le nom de domaine de votre organisation (par exemple, contoso.com) ou l’**ID de locataire** de votre organisation obtenu auprès de votre administrateur dans l’une des URL suivantes :
>
>   - https://myapplications.microsoft.com?tenantId=*votre_nom_de_domaine*
>   - https://myapplications.microsoft.com?tenant=*votre_ID_de_locataire*

## <a name="manage-access-reviews"></a>Gérer les révisions d’accès

Si votre administrateur vous a autorisé à effectuer vos propres révisions d’accès, vous pouvez gérer l’accès à vos groupes ou applications à partir de la vignette **Révisions d’accès** sur la page du portail **Mes applications**.

>[!Note]
>Si vous ne voyez pas la vignette **Révisions d’accès**, cela signifie que vous n’êtes pas autorisé à effectuer des révisions d’accès ou qu’aucune révision n’est en attente de votre approbation. Si vous pensez que vous devez avoir accès à la vignette, contactez votre support technique pour obtenir de l’aide.

## <a name="to-perform-your-access-reviews"></a>Pour effectuer vos révisions d’accès

1. Connectez-vous à votre compte professionnel ou scolaire.

1. Ouvrez votre navigateur web et accédez à https://myapps.microsoft.com, ou utilisez le lien fourni par votre organisation. Par exemple, vous pouvez être dirigé vers une page personnalisée de votre organisation, comme https://myapps.microsoft.com/contoso.com.

    La page **Applications** s'affiche, avec toutes les applications cloud de votre organisation que vous pouvez utiliser.

    ![Page Applications du portail Mes applications](media/my-apps-portal/my-apps-home.png)

1. Sélectionnez la vignette **Révisions d’accès** pour afficher la liste des révisions d’accès en attente de votre approbation.

    ![Page Révisions d’accès avec des révisions d’accès en attente pour l’organisation](media/my-apps-portal/my-apps-portal-access-reviews-page.png)

1. Sélectionnez **Commencer la révision** pour démarrer votre révision d’accès.

5. Révisez votre accès et déterminez s’il est toujours nécessaire.

    ![Page Révision d’accès présentant les détails de la révision](media/my-apps-portal/my-apps-portal-perform-access-reviews-page.png)

    >[!Note]
    >Si vous êtes un administrateur autorisé à réviser l’accès de votre organisation à des groupes et applications, vous verrez une page différente. Pour plus d’informations sur la révision de groupes ou d’applications pour votre organisation, voir [Réviser l’accès à des groupes ou applications dans les Révisions d’accès Azure AD](../governance/perform-access-review.md).

6. Sélectionnez **Oui** conserver votre accès ou **Non** pour supprimer votre accès.

    Si vous sélectionnez **Oui**, vous devrez peut-être spécifier une justification dans la zone **Raison**.

    ![Page Révision d’accès affichant la zone Raison avec exemple de texte](media/my-apps-portal/my-apps-portal-perform-access-reviews-reason-box.png)

7. Sélectionnez **Envoyer**.

    Votre révision d’accès est terminée et vous revenez au portail **Mes applications**.

    >[!Note]
    >Vous pouvez modifier votre accès à tout moment jusqu’à la fin de votre période de révision d’accès. Si vous supprimez votre accès à une application ou à un groupe, cet accès n’est pas supprimé immédiatement. La suppression se produit à la fin de la période de révision d’accès ou quand un administrateur ferme la révision.

## <a name="next-steps"></a>Étapes suivantes

- [Accéder aux applications et les utiliser sur le portail Mes applications](my-apps-portal-end-user-access.md)
- [Modifier vos informations de profil](./my-account-portal-settings.md)
- [Consulter et mettre à jour les informations relatives aux groupes](my-apps-portal-end-user-groups.md)