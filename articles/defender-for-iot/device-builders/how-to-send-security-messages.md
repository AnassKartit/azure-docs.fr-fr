---
title: Envoyer des messages de sécurité des appareils Defender pour IoT
description: Découvrez comment envoyer vos messages de sécurité à l’aide de Defender pour IoT.
ms.topic: how-to
ms.date: 2/8/2021
ms.custom: devx-track-js
ms.openlocfilehash: 791e49c4e8f0e503c67f24e440fc229998b7b9da
ms.sourcegitcommit: f6e2ea5571e35b9ed3a79a22485eba4d20ae36cc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/24/2021
ms.locfileid: "128680493"
---
# <a name="send-security-messages-sdk"></a>Envoyer des messages de sécurité à l’aide du SDK

Ce guide pratique décrit les fonctionnalités du service Defender pour IoT lorsque vous choisissez de collecter et d’envoyer les messages de sécurité des appareils sans utiliser d’agent Defender pour IoT, et explique comment les utiliser.

Dans ce guide, vous apprendrez comment :

> [!div class="checklist"]
> * Envoyer des messages de sécurité à l’aide du SDK C Azure IoT
> * Envoyer des messages de sécurité à l’aide du SDK C# Azure IoT
> * Envoyer des messages de sécurité à l’aide du SDK Python Azure IoT
> * Envoyer des messages de sécurité à l’aide du SDK Node.js Azure IoT
> * Envoyer des messages de sécurité à l’aide du SDK Java Azure IoT

## <a name="defender-for-iot-capabilities"></a>Fonctionnalités de Defender pour IoT

Defender pour IoT peut traiter et analyser tous types de messages de sécurité, à condition que les données envoyées soient conformes au schéma de Defender pour IoT et qu’il s’agisse de messages de sécurité.

## <a name="security-message"></a>Message de sécurité

Defender pour IoT définit un message de sécurité selon les critères suivants :

- Le message a été envoyé avec un SDK Azure IoT.
- Le message est conforme au schéma des messages de sécurité.
- Le message a été défini comme message de sécurité avant l’envoi.

Chaque message de sécurité comporte les métadonnées de l’expéditeur, par exemple `AgentId`, `AgentVersion` et `MessageSchemaVersion`, et une liste d’événements de sécurité.
Le schéma définit les propriétés valides et requises du message de sécurité, et notamment les types d’événements.

> [!NOTE]
> Les messages envoyés non conformes au schéma sont ignorés. Veillez à vérifier le schéma avant de lancer l’envoi de données, car les messages ignorés ne sont pas stockés.

> [!NOTE]
> Les messages envoyés sans être définis comme messages de sécurité avec le kit SDK Azure IoT ne sont pas acheminés vers le pipeline Defender pour IoT.

## <a name="valid-message-example"></a>Exemple de message valide

L’exemple ci-dessous montre un objet message de sécurité valide. Il contient les métadonnées du message et un seul événement de sécurité `ProcessCreate`.

Une fois défini comme message de sécurité et envoyé, ce message sera traité par Defender pour IoT.

```json
"AgentVersion": "0.0.1",
"AgentId": "e89dc5f5-feac-4c3e-87e2-93c16f010c25",
"MessageSchemaVersion": "1.0",
"Events": [
    {
        "EventType": "Security",
        "Category": "Triggered",
        "Name": "ProcessCreate",
        "IsEmpty": false,
        "PayloadSchemaVersion": "1.0",
        "Id": "21a2db0b-44fe-42e9-9cff-bbb2d8fdf874",
        "TimestampLocal": "2019-01-27 15:48:52Z",
        "TimestampUTC": "2019-01-27 13:48:52Z",
        "Payload":
            [
                {
                    "Executable": "/usr/bin/myApp",
                    "ProcessId": 11750,
                    "ParentProcessId": 1593,
                    "UserName": "aUser",
                    "CommandLine": "myApp -a -b"
                }
            ]
    }
]
```

## <a name="send-security-messages"></a>Envoyer des messages de sécurité

Envoyez des messages de sécurité *sans* utiliser l’agent Defender pour IoT à l’aide du [SDK de l’appareil C Azure IoT](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview), du [SDK de l’appareil C# Azure IoT](https://github.com/Azure/azure-iot-sdk-csharp/tree/preview), du [SDK Node.js Azure IoT](https://github.com/Azure/azure-iot-sdk-node), du [SDK Python Azure IoT](https://github.com/Azure/azure-iot-sdk-python) ou du [SDK Java Azure IoT](https://github.com/Azure/azure-iot-sdk-java).

Pour envoyer les données à partir de vos appareils en vue de leur traitement par Defender pour IoT, utilisez une des API suivantes pour marquer les messages avec le bon routage vers le pipeline de traitement Defender pour IoT.

Toutes les données envoyées, même si elles sont marquées avec l’en-tête approprié, doivent également respecter le schéma de message de Defender pour IoT.

### <a name="send-security-message-api"></a>API d’envoi de message de sécurité

L’API d’**envoi de messages de sécurité** est actuellement disponible en C, C#, Python, Node.js et Java.

#### <a name="c-api"></a>API C

```c
bool SendMessageAsync(IoTHubAdapter* iotHubAdapter, const void* data, size_t dataSize) {

    bool success = true;
    IOTHUB_MESSAGE_HANDLE messageHandle = NULL;

    messageHandle = IoTHubMessage_CreateFromByteArray(data, dataSize);

    if (messageHandle == NULL) {
        success = false;
        goto cleanup;
    }

    if (IoTHubMessage_SetAsSecurityMessage(messageHandle) != IOTHUB_MESSAGE_OK) {
        success = false;
        goto cleanup;
    }

    if (IoTHubModuleClient_SendEventAsync(iotHubAdapter->moduleHandle, messageHandle, SendConfirmCallback, iotHubAdapter) != IOTHUB_CLIENT_OK) {
        success = false;
        goto cleanup;
    }

cleanup:
    if (messageHandle != NULL) {
        IoTHubMessage_Destroy(messageHandle);
    }

    return success;
}

static void SendConfirmCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback) {
    if (userContextCallback == NULL) {
        //error handling
        return;
    }

    if (result != IOTHUB_CLIENT_CONFIRMATION_OK){
        //error handling
    }
}
```

#### <a name="c-api"></a>C# API

```cs

private static async Task SendSecurityMessageAsync(string messageContent)
{
    ModuleClient client = ModuleClient.CreateFromConnectionString("<connection_string>");
    Message  securityMessage = new Message(Encoding.UTF8.GetBytes(messageContent));
    securityMessage.SetAsSecurityMessage();
    await client.SendEventAsync(securityMessage);
}
```

#### <a name="nodejs-api"></a>API Node.js

```typescript
var Protocol = require('azure-iot-device-mqtt').Mqtt

function SendSecurityMessage(messageContent)
{
  var client = Client.fromConnectionString(connectionString, Protocol);

  var connectCallback = function (err) {
    if (err) {
      console.error('Could not connect: ' + err.message);
    } else {
      var message = new Message(messageContent);
      message.setAsSecurityMessage();
      client.sendEvent(message);
  
      client.on('error', function (err) {
        console.error(err.message);
      });
  
      client.on('disconnect', function () {
        clearInterval(sendInterval);
        client.removeAllListeners();
        client.open(connectCallback);
      });
    }
  };

  client.open(connectCallback);
}
```

#### <a name="python-api"></a>API Python

Pour utiliser l’API Python, vous devez installer le package [azure-iot-device](https://pypi.org/project/azure-iot-device/).

Lorsque vous utilisez l’API Python, vous pouvez envoyer le message de sécurité via le module ou via l’appareil à l’aide de la chaîne de connexion unique de l’appareil ou du module. Lorsque vous utilisez l’exemple de script Python suivant, pour un appareil, utilisez **IoTHubDeviceClient** et pour un module, utilisez **IoTHubModuleClient**.

```python
from azure.iot.device.aio import IoTHubDeviceClient, IoTHubModuleClient
from azure.iot.device import Message

async def send_security_message_async(message_content):
    conn_str = os.getenv("<connection_string>")
    device_client = IoTHubDeviceClient.create_from_connection_string(conn_str)
    await device_client.connect()
    security_message = Message(message_content)
    security_message.set_as_security_message()
    await device_client.send_message(security_message)
    await device_client.disconnect()
```

#### <a name="java-api"></a>API Java

```java
public void SendSecurityMessage(string message)
{
    ModuleClient client = new ModuleClient("<connection_string>", IotHubClientProtocol.MQTT);
    Message msg = new Message(message);
    msg.setAsSecurityMessage();
    EventCallback callback = new EventCallback();
    string context = "<user_context>";
    client.sendEventAsync(msg, callback, context);
}
```

## <a name="next-steps"></a>Étapes suivantes

- Lire la [vue d’ensemble](overview.md) du service Defender pour IoT
- Pour en savoir plus sur Defender pour IoT, consultez [Présentation de la solution basée sur un agent pour les fabricants d’appareils](architecture-agent-based.md)
- Activer le [service](quickstart-onboard-iot-hub.md)
- Consultez le [Forum aux questions sur Azure Defender pour IoT](resources-agent-frequently-asked-questions.md)
- Découvrir comment accéder aux [données de sécurité brutes](how-to-security-data-access.md)
- Comprendre les [recommandations](concept-recommendations.md)
- Comprendre les [alertes](concept-security-alerts.md)
