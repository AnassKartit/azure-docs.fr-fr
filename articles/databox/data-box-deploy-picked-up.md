---
title: Tutoriel sur le renvoi d’Azure Data Box | Microsoft Docs
description: Dans ce tutoriel, découvrez comment retourner l’appareil Azure Data Box, notamment préparer l’expédition de Data Box, vérifier le chargement de données et à effacer les données de Data Box.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 10/17/2021
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: 8b51cbf8af938a71faea05b2cadbea904b508d26
ms.sourcegitcommit: 5361d9fe40d5c00f19409649e5e8fed660ba4800
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2021
ms.locfileid: "130138652"
---
::: zone target="docs"

# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Tutoriel : Retourner l’appareil Azure Data Box et vérifier le chargement des données dans Azure

::: zone-end

::: zone target="chromeless"

## <a name="return-data-box-and-verify-data-upload-to-azure"></a>Retourner l’appareil Data Box et vérifier le chargement des données dans Azure

::: zone-end

::: zone target="docs"

Ce didacticiel vous explique comment retourner l’appareil Azure Data Box et vérifier le chargement des données dans Azure.

Ce didacticiel fournit des informations sur les sujets suivants :

> [!div class="checklist"]
>
> * Prérequis
> * Préparer l’expédition
> * Expédier la Data Box à Microsoft
> * Vérifier le chargement des données dans Azure
> * Effacer les données de la Data Box

## <a name="prerequisites"></a>Prérequis

Avant de commencer, vérifiez les points suivants :

* Vous avez suivi le [Tutoriel : Copier des données sur Azure Data Box et procéder à une vérification](data-box-deploy-copy-data.md).
* Les travaux de copie sont terminés et il n’y a pas d’erreurs dans la page **Connexion et copie**. La **préparation de l’expédition** ne peut pas s’exécuter si des travaux de copie sont en cours ou s’il y a des erreurs dans la page **Connexion et copie**.

## <a name="prepare-to-ship"></a>Préparer l’expédition

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

::: zone-end

::: zone target="chromeless"

Une fois la copie des données terminée, vous pouvez préparer et expédier l’appareil. Quand l’appareil arrive au centre de données Azure, les données sont automatiquement chargées vers Azure.

## <a name="prepare-to-ship"></a>Préparer l’expédition

Avant de préparer l’expédition, assurez-vous que les travaux de copie sont terminés.

1. Accédez à la page **Préparation de l'expédition** dans l'interface utilisateur web locale et commencez la préparation de l'expédition.
2. Mettez l’appareil hors tension à partir de l’interface utilisateur web locale. Débranchez les câbles de l’appareil.

Les étapes suivantes sont déterminées par l’endroit où vous retournez l’appareil.

::: zone-end

::: zone target="docs"

## <a name="ship-data-box-back"></a>Renvoyer la Data Box

Vérifiez que la copie des données sur l’appareil a été effectuée et que l’exécution de la **préparation de l’expédition** a réussi. En fonction de la région où vous expédiez l’appareil, la procédure est différente.

### <a name="microsoft-managed-shipping"></a>Expédition gérée par Microsoft

Suivez les instructions applicables à la région à partir de laquelle vous effectuez l’expédition si celle-ci est gérée par Microsoft.

::: zone-end

## <a name="us--canada"></a>[États-Unis et Canada](#tab/in-us-canada)

Effectuez les étapes suivantes si le retour de l’appareil s’effectue aux États-Unis ou au Canada.

1. Vérifiez que l’appareil est hors tension et que les câbles sont débranchés.
2. Enroulez le cordon d’alimentation fourni avec la Data Box et fixez-le à l’arrière de l’appareil.
3. Assurez-vous que l’étiquette d’expédition est affichée sur l’écran E-ink et planifiez l’enlèvement avec votre transporteur. Si l’étiquette est endommagée ou perdue, ou si elle ne s’affiche pas sur l’écran E-ink, contactez le support Microsoft. Si le support technique le suggère, vous pouvez accéder à **Vue d’ensemble > Télécharger une étiquette d’expédition** dans le portail Azure. Téléchargez l’étiquette d’expédition et collez-la sur l’appareil. 
4. Planifiez une date d’enlèvement avec UPS en cas de renvoi de l’appareil. Pour planifier l’enlèvement :

    * Appelez l’UPS local (numéro d’appel gratuit propre à chaque pays/région).
    * Lors de l’appel, indiquez le numéro de suivi du retour, qui est indiqué dans l’écran E-ink ou sur l’étiquette de retour imprimée. Si vous n’indiquez pas le numéro de suivi, UPS vous facturera des frais supplémentaires à l’enlèvement.
    * Si des problèmes surviennent pendant la planification d’un enlèvement, ou si vous êtes invité à payer des frais supplémentaires, contactez le service des opérations Azure Data Box. Envoyez un e-mail à [adbops@microsoft.com](mailto:adbops@microsoft.com).

    Au lieu de planifier l’enlèvement, vous pouvez déposer la Data Box dans le point de dépôt le plus proche.
4. Une fois que la Data Box a été scannée et récupérée par le transporteur, l’état de la commande dans le portail affiche **Récupérée**. Un ID de suivi s’affiche également.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Vérifier le chargement des données dans Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Effacer les données de la Data Box

Une fois que le chargement des données vers Azure est terminé, la Data Box efface les données de ses disques, conformément aux [instructions du standard NIST SP 800-88 Revision 1](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

## <a name="europe"></a>[Europe](#tab/in-europe)

Effectuez les étapes suivantes si le retour de l’appareil s’effectue en Europe.

1. Vérifiez que l’appareil est hors tension et que les câbles sont débranchés.
2. Enroulez le cordon d’alimentation fourni avec la Data Box et fixez-le à l’arrière de l’appareil.
3. Assurez-vous que l’étiquette d’expédition est affichée sur l’écran E-ink et planifiez l’enlèvement avec votre transporteur. Si l’étiquette est endommagée ou perdue, ou si elle ne s’affiche pas sur l’écran E-ink, contactez le support Microsoft. Si le support technique le suggère, vous pouvez accéder à **Vue d’ensemble > Télécharger une étiquette d’expédition** dans le portail Azure. Téléchargez l’étiquette d’expédition et collez-la sur l’appareil.
1. **Si vous effectuez l’expédition en Allemagne ou en Suisse,** le centre de données Azure exige un avis préalable concernant tous les retours d’appareil :
    1. Envoyez un e-mail à Azure Data Box Operations, à l’aide du modèle suivant, pour recevoir un ID entrant. Envoyez un e-mail à [adbops@microsoft.com](mailto:adbops@microsoft.com).

       ```
       To: adbops@microsoft.com
       Subject: Request for Azure Data Box Inbound ID: <orderName> 
       Body: 
        
       I am ready to return an Azure Data Box and would like to request an Inbound ID for the following order:
       
       Order Name: <orderName>
       Return Tracking Number: <returnTracking#>
       ```

    1. Notez le numéro d’ID entrant fourni par Azure Data Box Operations, puis collez-le sur l’unité, de manière clairement visible, près de l’étiquette de retour.
1. Planifiez une date d’enlèvement avec UPS en cas de renvoi de l’appareil. Pour planifier l’enlèvement :

    * Appelez l’UPS local (numéro d’appel gratuit propre à chaque pays/région).
    * Lors de l’appel, indiquez le numéro de suivi du retour, qui est indiqué dans l’écran E-ink ou sur l’étiquette de retour imprimée. Si vous n’indiquez pas le numéro de suivi, UPS vous facturera des frais supplémentaires à l’enlèvement.
    * Si des problèmes surviennent pendant la planification d’un enlèvement, ou si vous êtes invité à payer des frais supplémentaires, contactez le service des opérations Azure Data Box. Envoyez un e-mail à [adbops@microsoft.com](mailto:adbops@microsoft.com).

    Au lieu de planifier l’enlèvement, vous pouvez déposer la Data Box dans le point de dépôt le plus proche.

    **Si vous effectuez l’expédition en Allemagne ou en Suisse,** vous pouvez également [utiliser l’expédition autogérée](data-box-deploy-picked-up.md#self-managed-shipping).

4. Une fois que la Data Box a été scannée et récupérée par le transporteur, l’état de la commande dans le portail affiche **Récupérée**. Un ID de suivi s’affiche également.


::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Vérifier le chargement des données dans Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Effacer les données de la Data Box

Une fois que le chargement des données vers Azure est terminé, la Data Box efface les données de ses disques, conformément aux [instructions du standard NIST SP 800-88 Revision 1](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

## <a name="australia"></a>[Australie](#tab/in-australia)

Les centres de données Azure de l’Australie ont une notification de sécurité supplémentaire. Toutes les expéditions entrantes doivent avoir une notification préalable. Effectuez les étapes suivantes pour expédier en Australie.

1. Conservez la boîte d’origine utilisée pour expédier l’appareil, en cas de retour.
2. Vérifiez que la copie des données sur l’appareil a été effectuée et que la **préparation de l’expédition** a réussi.
3. Mettez l’appareil hors tension et débranchez les câbles.
4. Enroulez le cordon d’alimentation fourni avec l’appareil, et fixez-le à l’arrière de ce dernier.
5. Réservez un enlèvement en ligne via le [Lien DHL](https://mydhl.express.dhl/au/en/schedule-pickup.html#/schedule-pickup#label-reference).

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Vérifier le chargement des données dans Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Effacer les données de la Data Box

Une fois que le chargement des données vers Azure est terminé, la Data Box efface les données de ses disques, conformément aux [instructions du standard NIST SP 800-88 Revision 1](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

## <a name="japan"></a>[Japon](#tab/in-japan)

1. Conservez la boîte d’origine utilisée pour expédier l’appareil, en cas de retour.
2. Mettez l’appareil hors tension et débranchez les câbles.
3. Enroulez le cordon d’alimentation fourni avec l’appareil, et fixez-le à l’arrière de ce dernier.
4. Écrivez le nom et l’adresse de votre entreprise dans la partie du bordereau de réexpédition réservée à l’expéditeur.
5. Envoyez un e-mail à Quantium Solutions en utilisant le modèle suivant.

    * Si le bordereau de réexpédition de Japon Post Chakubarai était manquant, consignez ce fait que dans cet e-mail. Quantium Solutions demandera à Japan Post d’apporter le bordereau de réexpédition à l’enlèvement.
    * Si vous avez plusieurs commandes, envoyez un e-mail pour garantir un enlèvement individuel.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Azure Data Box｜Job name： 
    Body:
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
    a. 08：00-13：00 
    b. 13：00-15：00 
    c. 15：00-17：00 
    d. 17：00-19：00 
    ```

6. Recevoir un e-mail de confirmation de Quantium Solutions après avoir réservé un enlèvement. L’e-mail de confirmation inclut également des informations sur le bordereau de réexpédition de Chakubarai.

Si nécessaire, vous pouvez contacter le support de Quantium Solutions (en langue japonaise) dont les coordonnées sont les suivantes :

* E-mail ：Customerservice.JP@quantiumsolutions.com 
* Téléphone：+81 3 57 55 01 50 

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Vérifier le chargement des données dans Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Effacer les données de la Data Box
 
Une fois que le chargement des données vers Azure est terminé, la Data Box efface les données de ses disques, conformément aux [instructions du standard NIST SP 800-88 Revision 1](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

## <a name="singapore"></a>[Singapour](#tab/in-singapore)

1. Conservez la boîte d’origine utilisée pour expédier l’appareil, en cas de retour.
2. Notez le numéro de suivi (indiqué comme numéro de référence dans la page **Préparation de l’expédition** de l’interface utilisateur web locale de la Data Box). Le numéro de suivi est disponible une fois l’étape **Préparation de l’expédition** terminée. Téléchargez l’étiquette d’expédition à partir de cette page, puis collez-la sur le carton d’emballage.
3. Mettez l’appareil hors tension et débranchez les câbles.
4. Enroulez le cordon d’alimentation fourni avec l’appareil, et fixez-le à l’arrière de ce dernier. 
5. Envoyez un e-mail au service client SingPost en utilisant le modèle d’e-mail suivant avec le numéro de suivi.

    ```
    To: kadcustcare@singpost.com
    Subject: Microsoft Azure Pickup - OrderName 
    Body: 
        1. Requestor name  
        2. Requestor contact number
        3. Requestor collection address
        4. Preferred collection date
    ```

   > [!NOTE]
   > Pour les demandes de réservation reçues un jour ouvré :
   > * Avant 15 h, la collecte sera effectuée le jour ouvré suivant entre 9 h et 13 h.
   > * Après 15 h, la collecte sera effectuée le jour ouvré suivant entre 14 h et 18 h.  

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Vérifier le chargement des données dans Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Effacer les données de la Data Box

Une fois que le chargement des données vers Azure est terminé, la Data Box efface les données de ses disques, conformément aux [instructions du standard NIST SP 800-88 Revision 1](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

## <a name="south-africa"></a>[Afrique du Sud](#tab/in-sa)

1. Emballez l’appareil à des fins de réexpédition dans sa boîte d’origine.
2. Enroulez le cordon d’alimentation fourni avec l’appareil, et fixez-le à l’arrière de ce dernier.
3. Notez le numéro de suivi (indiqué comme numéro de référence dans la page **Préparation de l’expédition** de l’interface utilisateur web locale de la Data Box). Ce numéro de suivi est disponible une fois l’étape « Préparation de l’expédition » terminée. Téléchargez l’étiquette d’expédition à partir de cette page, puis collez-la sur le carton d’emballage.
4. Demandez un code de retour à partir des opérations Azure Data Box. Un code de retour est nécessaire pour renvoyer le package au centre de données. Envoyez un e-mail à [adbops@microsoft.com](mailto:adbops@microsoft.com). Notez ce code sur l’étiquette d’expédition, à côté de l’adresse de retour, où il est bien visible.
5. Prenez rendez-vous avec DHL pour un enlèvement en utilisant l’une des méthodes suivantes :
 
   * Prenez rendez-vous en ligne pour un enlèvement en accédant à [DHL Express South Africa, **Schedule a Pickup**](https://mydhl.express.dhl/za/en/schedule-pickup.html#/schedule-pickup#label-reference).
   * Envoyer un e-mail à l’adresse [Priority.Support@dhl.com](mailto:Priority.Support@dhl.com) en utilisant le modèle suivant :

     ```output
     To: Priority.Support@dhl.com
     Subject: Pickup request for Microsoft Azure
     Body: Need pick up for the below shipment
       *  DHL tracking number: (reference number/waybill number)
       *  Requested pickup date: yyyy/mm/dd;time:HH MM
       *  Shipper contact: (company name)
       *  Contact person: 
       *  Phone number: 
       *  Full physical address: 
       *  Item to be collected: Azure Dt
     ```

    * Vous pouvez également déposer le colis au point de service DHL le plus proche.

6. Si vous rencontrez des problèmes, envoyez un e-mail à l’adresse [Priority.Support@dhl.com](mailto:Priority.Support@dhl.com), en détaillant le ou les problèmes et en indiquant le numéro de lettre de transport dans la ligne Objet. Vous pouvez également appeler le +27(0)119213902.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Vérifier le chargement des données dans Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Effacer les données de la Data Box

Une fois que le chargement des données vers Azure est terminé, la Data Box efface les données de ses disques, conformément aux [instructions du standard NIST SP 800-88 Revision 1](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

## <a name="hong-kong"></a>[Hong Kong (R.A.S.)](#tab/in-hk)

1. Emballez l’appareil à des fins de réexpédition dans sa boîte d’origine.
2. Enroulez le cordon d’alimentation fourni avec l’appareil, et fixez-le à l’arrière de ce dernier.
3. Appelez l’assistance téléphonique de **Quantium Solutions** au **+852 2318 1213 (Hong Kong, R.A.S.)** pendant les heures de bureau (de 9h00 à 18h00, du lundi au vendredi).  
4. Indiquez Enlèvement Microsoft Azure, le numéro de référence et le numéro de suivi (code-barres en haut) sur l’étiquette de réexpédition pour organiser l’enlèvement.
5. Vous recevrez une confirmation verbale de la planification de l’enlèvement. Si le transporteur ne procède pas à l’enlèvement, appelez l’assistance téléphonique de Quantium Solutions pour trouver une autre solution.
6. Lors de la réservation d’un enlèvement auprès de Quantium Solutions, partagez la confirmation avec [Microsoft Data Box Operations Asia](mailto:adbo@microsoft.com) à l’aide du modèle suivant :

    ```output
    To: adbo@microsoft.com
    Subject: Microsoft Data Box Job: [order name] has completed copy
    Body:
    We have confirmed the pickup details with Quantium Solutions.

       * Requestor name:
       * Requestor contact number:
       * Pickup Date:  
       * Pickup time:
    ```

Si vous rencontrez des problèmes, envoyez un e-mail à Data Box Operations Asia à cette adresse [adbo@microsoft.com](mailto:adbo@microsoft.com), en détaillant le ou les problèmes et en indiquant le nom du travail dans la ligne Objet.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Vérifier le chargement des données dans Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Effacer les données de la Data Box
 

::: zone-end

## <a name="uae"></a>[Émirats Arabes Unis](#tab/in-uae)

1. Conservez la boîte d’origine utilisée pour expédier l’appareil, en cas de retour.
2. Vérifiez que la copie des données sur l’appareil est terminée et que l’étape **Préparation de l’expédition** a été correctement effectuée.
3. Notez le numéro de référence indiqué dans la page **Préparation de l’expédition** de l’interface utilisateur web locale .
4. Mettez l’appareil hors tension, puis débranchez les câbles. Enroulez le cordon d’alimentation fourni avec l’appareil, et fixez-le à l’arrière de ce dernier.
6. Emballez l’appareil à des fins de réexpédition dans sa boîte d’origine.
7. Envoyez un e-mail à l’[équipe des opérations Azure Data Box](mailto:adbops@microsoft.com) pour recevoir un ID qui sera utilisé pour identifier le package lors de son retour au centre de données.
8. Inscrivez cet ID sur l’étiquette d’expédition imprimée, à côté de l’adresse de retour afin qu’il soit clairement visible.  
9. Réservez un enlèvement en ligne en accédant au site [DHL Express UAE](https://mydhl.express.dhl/ae/en/home.html#/schedulePickupTab) (DHL Express - Émirats arabes unis) > **Schedule a Pickup** (Planifier un enlèvement).
   - Entrez le numéro de référence indiqué dans la page **Préparation de l’expédition** de l’interface utilisateur web locale de l’appareil dans le champ du numéro de bordereau.
   - Les réservations sont acceptées de 9h00 à 14h00, 6 jours par semaine (sauf le vendredi et les jours fériés).
   - Les demandes d’enlèvement doivent être effectuées au moins 90 minutes avant l’heure de fermeture de bureau du client.
10. Si vous rencontrez des problèmes avec l’outil de réservation de DHL, vous pouvez contacter DHL en utilisant l’une des méthodes suivantes :
    - Appelez le 04-2924545.
    - Envoyez un e-mail à l’adresse [ecom.ae@dhl.com](mailto:ecom.ae@dhl.com) contenant des détails sur le ou les problèmes rencontrés, et indiquez le numéro de numéro de bordereau dans la ligne Objet.
    - Appelez le support du service client au 600 567567.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Vérifier le chargement des données dans Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Effacer les données de la Data Box
 
Une fois que le chargement des données vers Azure est terminé, la Data Box efface les données de ses disques, conformément aux [instructions du standard NIST SP 800-88 Revision 1](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

<!--## [In Korea](#tab/in-korea) 

1. Retain the original box used to ship the device for return shipment.
2. Note down the tracking number (shown as reference number on the **Prepare to Ship** page of the Data Box local web UI). The tracking number is available after the **Prepare to ship** step successfully completes. Download the shipping label from this page and paste on the packing box. 
3. Power off the device and remove the cables.
4. Spool and securely place the power cord that was provided with the device in the back of the device. 

Request pickup  
If consignment note is present:  

1. Call Quantium Solutions International hotline at 070-8231-1418 during office hours (10 AM to 5 PM, Monday to Friday). Quote Microsoft Azure pickup and the service request number to arrange for a collection.
2. If the hotline is busy, email microsoft@rocketparcel.com, with the email subject Microsoft Azure Pickup and the service request number as reference.  
3. If the courier does not arrive for collection, call Quantium Solutions International hotline for alternate arrangements.  
4. You will receive an email confirmation for the pickup schedule.  

Exception process
If the consignment note is not present:
1. Call Quantium Solutions International hotline at 070-8231-1418 during office hours (10 AM to 5 PM, Monday to Friday). Quote Microsoft Azure pickup and the service request number. Specify that you need a new consignment note to arrange for a collection. Provide sender (customer), receiver information (Azure datacenter), and reference number (service request number).
2. If the hotline is busy, email microsoft@rocketparcel.com, with the email subject Microsoft Azure Pickup and the service request number as reference.
3. If the courier does not arrive for collection, call Quantium Solutions International hotline for alternate arrangements.
4. You get a verbal confirmation if request is made via telephone.  
::: zone target="chromeless"

## Verify data upload to Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## Erasure of data from Data Box
 
Once the upload to Azure is complete, the Data Box erases the data on its disks as per the [NIST SP 800-88 Revision 1 guidelines](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload.md)]

::: zone-end
-->

---

### <a name="self-managed-shipping"></a>Expédition autogérée

Suivez les instructions applicables à la région à partir de laquelle vous effectuez l’expédition si celle-ci est gérée par Microsoft.

Si vous utilisez la Data Box dans la région US Government, au Japon, à Singapour, en Corée, en Inde, en Afrique du Sud, au Royaume-Uni, en Allemagne, en Suisse, en Europe de l’Ouest, en Australie ou au Brésil dans le cadre d’une offre US Government et que vous avez sélectionné l’expédition autogérée au moment de créer votre commande, suivez ces instructions.

1. Indiquez le code d’autorisation mentionné dans la page **Préparation de l’expédition** de l’interface utilisateur web locale de la Data Box une fois l’étape terminée.
2. Mettez l’appareil hors tension et débranchez les câbles. Enroulez le cordon d’alimentation fourni avec l’appareil, et fixez-le à l’arrière de ce dernier.
3. Lorsque vous êtes prêt à renvoyer l’appareil, envoyez un e-mail à l’équipe des opérations Azure Data Box en utilisant le modèle ci-dessous.

    ```
    To: adbops@microsoft.com
    Subject: Request for Azure Data Box drop-off for order: 'orderName'
    Body:
        1. Order name  
        2. Authorization code available after Prepare to Ship has completed [Yes/No]  
        3. Contact name of the person dropping off. You will need to display a government-approved ID during the drop off.
    ```

   > [!NOTE]
   > - Les informations requises pour le renvoi de l’appareil peuvent varier selon la région. 
   > - Si vous renvoyez un Data Box au Brésil, consultez la rubrique [Utilisation de l’expédition autogérée pour Azure Data Box](data-box-portal-customer-managed-shipping.md) pour obtenir des instructions détaillées. 

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Vérifier le chargement des données dans Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Effacer les données de la Data Box
 
Une fois que le chargement des données vers Azure est terminé, la Data Box efface les données de ses disques, conformément aux [instructions du standard NIST SP 800-88 Revision 1](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

---

::: zone target="docs"

## <a name="verify-data-upload-to-azure"></a>Vérifier le chargement des données dans Azure

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end
