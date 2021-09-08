---
title: Comment démarrer une connexion WebSocket au service Azure Web PubSub
description: Instruction sur la façon de démarrer une connexion WebSocket au service Azure Web PubSub dans différentes langues
author: vicancy
ms.author: lianwei
ms.service: azure-web-pubsub
ms.topic: how-to
ms.date: 08/26/2021
ms.openlocfilehash: 91d326197737ab6dde07bbb72584648cfe025b05
ms.sourcegitcommit: dcf1defb393104f8afc6b707fc748e0ff4c81830
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2021
ms.locfileid: "123116026"
---
#  <a name="how-to-start-websocket-connection-to-the-azure-web-pubsub-service"></a>Comment démarrer une connexion WebSocket au service Azure Web PubSub

Les clients se connectent au service Azure Web PubSub à l’aide du protocole [WebSocket](https://tools.ietf.org/html/rfc6455) standard. Les langages prenant en charge le client WebSocket peuvent donc être utilisés pour écrire un client pour le service. Dans les sections ci-dessous, nous montrons plusieurs exemples de clients WebSocket dans différents langages.

## <a name="auth"></a>Auth
Le service Web PubSub utilise un [jeton JWT](https://tools.ietf.org/html/rfc7519.html) pour valider et authentifier les clients. Les clients peuvent placer le jeton dans le paramètre de requête `access_token` ou dans l’en-tête `Authorization` lors de la connexion au service.

Dans un workflow classique, le client communique d’abord avec son serveur d’applications pour obtenir l’URL du service et le jeton. Le client ouvre ensuite la connexion WebSocket au service à l’aide de l’URL et du jeton qu’il reçoit.

Le portail fournit également une *URL de client* générée de manière dynamique pour que les clients démarrent un test rapide :

:::image type="content" source="./media/howto-websocket-connect/generate-client-url.png" alt-text="Capture d’écran montrant où trouver le générateur d’URL de client.":::

> [!NOTE]
> Veillez à n’inclure que les rôles nécessaires lors de la génération du jeton.
>

Pour simplifier l’exemple de workflow, dans les sections ci-dessous, nous utilisons cette URL générée temporairement à partir du portail pour que le client se connecte, à l’aide de `<Client_URL_From_Portal>` pour représenter la valeur. Le jeton généré expire par défaut dans un délai de 50 minutes. N’oubliez donc pas d’en régénérer un lorsque le jeton expire.

Le service prend en charge deux types de clients WebSocket : l’un est le client WebSocket simple et l’autre est le client WebSocket PubSub. Nous montrons ici comment ces deux types de clients se connectent au service. Pour plus d’informations sur ces deux types de clients, consultez [Protocoles de client WebSocket pour Azure Web PubSub](./concept-client-protocols.md).

## <a name="dependency"></a>Dépendance

# <a name="in-browser"></a>[Dans le navigateur](#tab/browser)
Dans la plupart des navigateurs modernes, l’API `WebSocket` est prise en charge de manière native. 

# <a name="nodejs"></a>[Node.JS](#tab/javascript)

* [Node.js 12.x ou version ultérieure](https://nodejs.org)
* `npm install ws`

# <a name="python"></a>[Python](#tab/python)
* [Python](https://www.python.org/)
* `pip install websockets`

# <a name="c"></a>[C#](#tab/csharp)

* [.NET Core 2.1 ou version ultérieure](https://dotnet.microsoft.com/download)
* `dotnet add package Websocket.Client`
    * [WebSocket.Client](https://github.com/Marfusios/websocket-client) est un client WebSocket tiers avec une reconnexion et une gestion des erreurs intégrées

# <a name="java"></a>[Java](#tab/java)
- [Java Development Kit (JDK)](/java/azure/jdk/) version 8 ou ultérieure.
- [Apache Maven](https://maven.apache.org/download.cgi).

---

## <a name="simple-websocket-client"></a>Client WebSocket simple

# <a name="in-browser"></a>[Dans le navigateur](#tab/browser)

Dans le bloc `script` de la page HTML :
```html
<script>
    // Don't forget to replace this <Client_URL_From_Portal> with the value fetched from the portal
    let ws = new WebSocket("<Client_URL_From_Portal>");
    ws.onopen = () => {
        // Do things when the WebSocket connection is established
    };

    ws.onmessage = event => {
        // Do things when messages are received.
    };
</script>
```

# <a name="nodejs"></a>[Node.JS](#tab/javascript)

```js
const WebSocket = require('ws');
// Don't forget to replace this <Client_URL_From_Portal> with the value fetched from the portal
const client = new WebSocket("<Client_URL_From_Portal>");
client.on('open', () => {
     // Do things when the WebSocket connection is established
});
client.on('message', msg => {
     // Do things when messages are received.
});
```

# <a name="python"></a>[Python](#tab/python)

```python
import asyncio
import websockets

async def hello():
    # Don't forget to replace this <Client_URL_From_Portal> with the value fetched from the portal
    uri = '<Client_URL_From_Portal>'
    async with websockets.connect(uri) as ws:
        while True:
            await ws.send('hello')
            greeting = await ws.recv()
            print(greeting)

asyncio.get_event_loop().run_until_complete(hello())
```

# <a name="c"></a>[C#](#tab/csharp)

```csharp
using System;
using System.IO;
using System.Text;
using System.Threading.Tasks;
using Websocket.Client;

namespace subscriber
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // Don't forget to replace this <Client_URL_From_Portal> with the value fetched from the portal
            using (var client = new WebsocketClient(new Uri("<Client_URL_From_Portal>")))
            {
                // Disable the auto disconnect and reconnect because the sample would like the client to stay online even no data comes in
                client.ReconnectTimeout = null;
                client.MessageReceived.Subscribe(msg => Console.WriteLine($"Message received: {msg}"));
                await client.Start();
                Console.WriteLine("Connected.");
                Console.Read();
            }
        }
    }
}
```

# <a name="java"></a>[Java](#tab/java)

```java
package client;

import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.WebSocket;
import java.util.concurrent.CompletionStage;

/**
 * A simple WebSocket Client.
 *
 */
public final class SimpleClient {
    private SimpleClient() {
    }

    /**
     * Starts a simple WebSocket connection.
     * @param args The arguments of the program.
     */
    public static void main(String[] args) throws Exception {
        // Don't forget to replace this <Client_URL_From_Portal> with the value fetched from the portal
        WebSocket ws = HttpClient.newHttpClient().newWebSocketBuilder()
                .buildAsync(URI.create("<Client_URL_From_Portal>"), new WebSocketClient()).join();
        System.in.read();
    }

    private static final class WebSocketClient implements WebSocket.Listener {
        private WebSocketClient() {
        }

        @Override
        public void onOpen(WebSocket webSocket) {
            System.out.println("onOpen using subprotocol " + webSocket.getSubprotocol());
            WebSocket.Listener.super.onOpen(webSocket);
        }

        @Override
        public CompletionStage<?> onText(WebSocket webSocket, CharSequence data, boolean last) {
            System.out.println("onText received " + data);
            return WebSocket.Listener.super.onText(webSocket, data, last);
        }

        @Override
        public void onError(WebSocket webSocket, Throwable error) {
            System.out.println("Bad day! " + webSocket.toString());
            WebSocket.Listener.super.onError(webSocket, error);
        }
    }
}

```


---

## <a name="pubsub-websocket-client"></a>Client WebSocket PubSub

# <a name="in-browser"></a>[Dans le navigateur](#tab/browser)

Dans le bloc `script` de la page HTML :
```html
<script>
    // Don't forget to replace this <Client_URL_From_Portal> with the value fetched from the portal
    let ws = new WebSocket("<Client_URL_From_Portal>", 'json.webpubsub.azure.v1');
    ws.onopen = () => {
        // Do things when the WebSocket connection is established
    };

    ws.onmessage = event => {
        // Do things when messages are received.
    };
</script>
```

# <a name="nodejs"></a>[Node.JS](#tab/javascript)

```js
const WebSocket = require('ws');
// Don't forget to replace this <Client_URL_From_Portal> with the value fetched from the portal
const client = new WebSocket("<Client_URL_From_Portal>", "json.webpubsub.azure.v1");
client.on('open', () => {
     // Do things when the WebSocket connection is established
});
client.on('message', msg => {
     // Do things when messages are received.
});
```

# <a name="python"></a>[Python](#tab/python)

```python
import asyncio
import websockets

async def join_group():
    # Don't forget to replace this <Client_URL_From_Portal> with the value fetched from the portal
    uri = '<Client_URL_From_Portal>'
    async with websockets.connect(uri, subprotocols=['json.webpubsub.azure.v1']) as ws:
        await ws.send('{"type":"joinGroup","ackId":1,"group":"group1"}')
        return await ws.recv()

print(asyncio.get_event_loop().run_until_complete(join_group()))
```

# <a name="c"></a>[C#](#tab/csharp)

```csharp
using System;
using System.IO;
using System.Net.WebSockets;
using System.Text;
using System.Threading.Tasks;
namespace subscriber
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // Don't forget to replace this <Client_URL_From_Portal> with the value fetched from the portal
            using (var client = new WebsocketClient(new Uri("<Client_URL_From_Portal>"), () =>
            {
                var inner = new ClientWebSocket();
                inner.Options.AddSubProtocol("json.webpubsub.azure.v1");
                return inner;
            }))
            {
                // Disable the auto disconnect and reconnect because the sample would like the client to stay online even no data comes in
                client.ReconnectTimeout = null;
                client.MessageReceived.Subscribe(msg => Console.WriteLine($"Message received: {msg}"));
                await client.Start();
                Console.WriteLine("Connected.");
                Console.Read();
            }
        }
    }
}
```

# <a name="java"></a>[Java](#tab/java)

```java
package client;

import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.WebSocket;
import java.util.concurrent.CompletionStage;

/**
 * A PubSub WebSocket Client.
 *
 */
public final class SubprotocolClient {
    private SubprotocolClient() {
    }

    /**
     * Starts a PubSub WebSocket connection.
     * @param args The arguments of the program.
     */
    public static void main(String[] args) throws Exception {
        // Don't forget to replace this <Client_URL_From_Portal> with the value fetched from the portal
        WebSocket ws = HttpClient.newHttpClient().newWebSocketBuilder().subprotocols("json.webpubsub.azure.v1")
                .buildAsync(URI.create("<Client_URL_From_Portal>"), new WebSocketClient()).join();

        ws.sendText("{\"type\":\"joinGroup\",\"ackId\":1,\"group\":\"group1\"}", true);
        System.in.read();
    }

    private static final class WebSocketClient implements WebSocket.Listener {
        private WebSocketClient() {
        }

        @Override
        public void onOpen(WebSocket webSocket) {
            System.out.println("onOpen using subprotocol " + webSocket.getSubprotocol());
            WebSocket.Listener.super.onOpen(webSocket);
        }

        @Override
        public CompletionStage<?> onText(WebSocket webSocket, CharSequence data, boolean last) {
            System.out.println("onText received " + data);
            return WebSocket.Listener.super.onText(webSocket, data, last);
        }

        @Override
        public void onError(WebSocket webSocket, Throwable error) {
            System.out.println("Bad day! " + webSocket.toString());
            WebSocket.Listener.super.onError(webSocket, error);
        }
    }
}
```

---

## <a name="next-step"></a>Étape suivante

Dans cet article, nous montrons comment se connecter au service à l’aide de l’URL générée à partir du portail.  Consultez les tutoriels ci-dessous pour voir comment les clients communiquent avec le serveur d’applications pour obtenir l’URL dans des applications réelles.

> [!div class="nextstepaction"]
> [Tutoriel : créer un salon de conversation avec Azure Web PubSub](./tutorial-build-chat.md)

> [!div class="nextstepaction"]
> [Tutoriel : diffusion en continu du client à l’aide du sous-protocole pris en charge par le service](./tutorial-subprotocol.md)

> [!div class="nextstepaction"]
> [Explorer d’autres exemples Azure Web PubSub](https://aka.ms/awps/samples)

