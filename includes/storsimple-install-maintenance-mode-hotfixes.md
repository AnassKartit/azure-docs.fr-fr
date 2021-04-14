---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 2d0fd904dd704c704662192e1e92fe403f0971c5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "67177093"
---
#### <a name="to-install-maintenance-mode-hotfixes-via-windows-powershell-for-storsimple"></a>Pour installer les correctifs logiciels en mode Maintenance via Windows PowerShell pour StorSimple
> [!IMPORTANT]
> En mode Maintenance, vous devez appliquer le correctif logiciel d’abord à un contrôleur, puis à l’autre.
> 
> 

1. Mettez l’appareil en mode Maintenance. Consultez l’[Étape 2 : passage en mode Maintenance](../articles/storsimple/storsimple-update-device.md#step2) pour obtenir des instructions sur le passage en mode Maintenance.
2. Pour appliquer le correctif logiciel, tapez :
   
     `Start-HcsHotfix` 
3. Lorsque vous y êtes invité, indiquez le chemin d’accès au dossier partagé sur le réseau qui contient les fichiers du correctif logiciel.
4. Vous êtes invité à confirmer l’opération. Tapez **Y** pour installer le correctif.
5. Après avoir appliqué le correctif logiciel à un contrôleur, connectez-vous à l’autre contrôleur. Appliquez le correctif logiciel comme vous l’avez fait pour le contrôleur précédent.
6. Une fois les correctifs logiciels appliqués, quittez le mode Maintenance. Pour obtenir des instructions, consultez l’[Étape 4 : quitter le mode Maintenance](../articles/storsimple/storsimple-update-device.md#step4).

