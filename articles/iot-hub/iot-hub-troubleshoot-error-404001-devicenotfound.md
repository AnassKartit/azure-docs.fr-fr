---
title: Résolution de l’erreur Azure IoT Hub 404001 DeviceNotFound
description: Comprendre comment corriger l’erreur 404001 DeviceNotFound
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: 15aa21c2ec2c11bb251f7208fd22c92ceb859d6d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/19/2021
ms.locfileid: "76960302"
---
# <a name="404001-devicenotfound"></a>404001 DeviceNotFound

Cet article décrit les causes et solutions des erreurs **404001 DeviceNotFound**.

## <a name="symptoms"></a>Symptômes

Au cours d’une communication cloud-à-appareil (C2D) telle qu’un message C2D, une mise à jour de jumeau ou une méthode directe, l’opération échoue en générant l’erreur **404001 DeviceNotFound**.

## <a name="cause"></a>Cause

L’opération a échoué parce que IoT Hub ne peut pas trouver l’appareil. L’appareil n’est pas inscrit ou est désactivé.

## <a name="solution"></a>Solution

Inscrivez l’ID d’appareil que vous avez utilisé, puis réessayez.