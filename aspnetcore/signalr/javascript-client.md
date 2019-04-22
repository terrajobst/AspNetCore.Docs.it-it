---
title: ASP.NET Core SignalR JavaScript client
author: bradygaster
description: Panoramica di ASP.NET Core SignalR JavaScript client.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: f1f072e63928502fa1bad62e808ff035e57f2fd3
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983013"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript client

[Rachel Appel](http://twitter.com/rachelappel)

La libreria client di ASP.NET Core SignalR JavaScript consente agli sviluppatori di chiamare il codice dell'hub sul lato server.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-client-package"></a>Installare il pacchetto client di SignalR

La libreria client SignalR JavaScript viene fornita come un [npm](https://www.npmjs.com/) pacchetto. Se si usa Visual Studio, eseguire `npm install` dal **Console di gestione pacchetti** mentre si trovano nella cartella radice. Per Visual Studio Code, eseguire il comando dal **terminale integrato**.

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

Monitoraggio prestazioni rete consente di installare il contenuto del pacchetto nel *node_modules\\@aspnet\signalr\dist\browser* cartella. Creare una nuova cartella denominata *signalr* sotto il *wwwroot\\lib* cartella. Copia il *signalr.js* del file per il *wwwroot\lib\signalr* cartella.

## <a name="use-the-signalr-javascript-client"></a>Usare il client SignalR JavaScript

Fare riferimento a client SignalR JavaScript il `<script>` elemento.

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>Connettersi a un hub

Il codice seguente crea e avvia una connessione. Nome dell'hub è maiuscole e minuscole.

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>Connessioni cross-origin

In genere, i browser il carico delle connessioni appartenenti allo stesso dominio della pagina richiesta. Tuttavia, esistono alcuni casi quando è necessaria una connessione a un altro dominio.

Per impedire la lettura dei dati sensibili da un altro sito, di un sito dannoso [le connessioni cross-origin](xref:security/cors) sono disabilitati per impostazione predefinita. Per consentire a una richiesta multiorigine, abilitarla nel `Startup` classe.

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>Chiamare i metodi dell'hub da client

Client JavaScript chiamano metodi pubblici in hub tramite il [richiamare](/javascript/api/%40aspnet/signalr/hubconnection#invoke) metodo per il [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection). Il `invoke` metodo accetta due argomenti:

* Il nome del metodo dell'hub. Nell'esempio seguente, è il nome del metodo dell'hub `SendMessage`.
* Eventuali argomenti definiti nel metodo dell'hub. Nell'esempio seguente, è il nome dell'argomento `message`. Il codice di esempio Usa sintassi della funzione freccia è supportata nelle versioni correnti di tutti i principali browser, ad eccezione di Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Se si usa il servizio Azure SignalR in *modalità senza server*, non è possibile chiamare i metodi dell'hub da un client. Per altre informazioni, vedere la [documentazione di SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).

Il `invoke` metodo viene restituito un JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise). Il `Promise` viene risolto con il valore restituito (se presente) quando restituito dal metodo nel server. Se il metodo nel server genera un errore, il `Promise` viene rifiutato con il messaggio di errore. Usare la `then` e `catch` metodi sulle `Promise` per gestire questi casi (o `await` sintassi).

Il `send` metodo viene restituito un JavaScript `Promise`. Il `Promise` è risolto quando il messaggio è stato inviato al server. Se si verifica un errore di invio del messaggio, il `Promise` viene rifiutato con il messaggio di errore. Usare la `then` e `catch` metodi sulle `Promise` per gestire questi casi (o `await` sintassi).

> [!NOTE]
> Usando `send` senza aspettare il server ha ricevuto il messaggio. Non è pertanto possibile restituire errori o i dati dal server.

## <a name="call-client-methods-from-hub"></a>Chiamare i metodi client hub

Per ricevere messaggi dall'hub, definire un metodo usando il [sul](/javascript/api/%40aspnet/signalr/hubconnection#on) metodo del `HubConnection`.

* Il nome del metodo client JavaScript. Nell'esempio seguente, è il nome del metodo `ReceiveMessage`.
* Argomenti che dell'hub viene passata al metodo. Nell'esempio seguente, il valore dell'argomento è `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Il codice precedente in `connection.on` viene eseguito quando viene chiamato codice lato server usando il [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) (metodo).

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR determina quale metodo di client per chiamare creando una corrispondenza tra il nome del metodo e gli argomenti definiti nella `SendAsync` e `connection.on`.

> [!NOTE]
> Come procedura consigliata, chiamare il [avviare](/javascript/api/%40aspnet/signalr/hubconnection#start) metodo sulle `HubConnection` dopo `on`. In tal modo che i gestori registrati prima di tutti i messaggi vengono ricevuti.

## <a name="error-handling-and-logging"></a>La registrazione e la gestione degli errori

Catena di una `catch` alla fine del metodo di `start` metodo per gestire gli errori lato client. Usare `console.error` agli errori di output alla console del browser.

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

Analisi di log lato client installazione passando un logger e il tipo di evento per registrare quando viene stabilita la connessione. I messaggi vengono registrati con il livello di log specificato e versioni successive. Livelli di log disponibili sono i seguenti:

* `signalR.LogLevel.Error` &ndash; Messaggi di errore. I log `Error` solo i messaggi.
* `signalR.LogLevel.Warning` &ndash; Messaggi di avviso sui potenziali errori. I log `Warning`, e `Error` messaggi.
* `signalR.LogLevel.Information` &ndash; Messaggi di stato senza errori. I log `Information`, `Warning`, e `Error` messaggi.
* `signalR.LogLevel.Trace` &ndash; I messaggi di traccia. Registra tutti gli elementi, inclusi i dati trasportati tra l'hub e client.

Usare la [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metodo sul [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) per configurare il livello di registrazione. I messaggi vengono registrati nella console del browser.

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>Riconnettere i client

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>La riconnessione automatica

Il client JavaScript per SignalR può essere configurato per la riconnessione automatica usando la `withAutomaticReconnect` metodo sul [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). Non riconnettersi automaticamente per impostazione predefinita.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Senza parametri, `withAutomaticReconnect()` consente di configurare il client per l'attesa di 0, 2, 10 e 30 secondi rispettivamente prima di provare ogni tentativo di riconnessione, fermandosi quando quattro tentativi non riusciti.

Prima di avviare qualsiasi tentativo di riconnessione, il `HubConnection` verrà transizione per il `HubConnectionState.Reconnecting` sullo stato e la generazione di relativo `onreconnecting` callback anziché in fase di transizione per il `Disconnected` dello stato e l'attivazione relativo `onclose` i callback, ad esempio un `HubConnection`senza la riconnessione automatica configurata. Ciò offre un'opportunità per avvisare gli utenti che la connessione è stata persa e disattivare gli elementi dell'interfaccia utente.

```javascript
connection.onreconnecting((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
  document.getElementById("messagesList").appendChild(li);
});
```

Se il client si riconnette correttamente entro i primi quattro diversi, il `HubConnection` passerà al `Connected` lo stato e la generazione di relativo `onreconnected` i callback. Ciò offre un'opportunità per informare gli utenti che è stata ristabilita la connessione.

Poiché la connessione sia completamente nuovo al server, un nuovo `connectionId` riceveranno la `onreconnected` callback.

> [!WARNING]
> Il `onreconnected` del callback `connectionId` parametro sarà indefinito se il `HubConnection` è stato configurato per [ignorare la negoziazione](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
  console.assert(connection.state === signalR.HubConnectionState.Connected);

  document.getElementById("messageInput").disabled = false;

  const li = document.createElement("li");
  li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
  document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` non configurare il `HubConnection` tentativo iniziale di avvio non riuscite, in modo che gli errori di avvio devono essere gestite manualmente:

```javascript
async function start() {
    try {
        await connection.start();
        console.assert(connection.state === signalR.HubConnectionState.Connected);
        console.log("connected");
    } catch (err) {
        console.assert(connection.state === signalR.HubConnectionState.Disconnected);
        console.log(err);
        setTimeout(() => start(), 5000);
    }
};
```

Se il client non è stata riconnettersi entro i primi quattro diversi, il `HubConnection` verranno transizione per il `Disconnected` lo stato e la generazione di relativo [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) i callback. Ciò offre un'opportunità per informare gli utenti la connessione viene definitivamente perduto e consiglia di aggiornare la pagina:

```javascript
connection.onclose((error) => {
  console.assert(connection.state === signalR.HubConnectionState.Disconnected);

  document.getElementById("messageInput").disabled = true;

  const li = document.createElement("li");
  li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
  document.getElementById("messagesList").appendChild(li);
})
```

Per configurare un numero di tentativi di riconnessione prima della disconnessione personalizzato o modificare l'intervallo di tempo di riconnessione, `withAutomaticReconnect` accetta una matrice di numeri che rappresenta il ritardo in millisecondi di attesa prima di avviare ogni tentativo di riconnessione.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

Nell'esempio precedente viene configurata la `HubConnection` per avviare il tentativo di riconnessioni immediatamente dopo la connessione viene persa. Questo vale anche per la configurazione predefinita.

Se non riesce al primo tentativo di riconnessione, il secondo tentativo di riconnessione anche inizierà immediatamente anziché attendere 2 secondi, come verrebbe mostrata nella configurazione predefinita.

Se ha esito negativo al secondo tentativo di riconnessione, il terzo tentativo di riconnessione verrà avviato entro 10 secondi, ovvero anche in questo caso, ad esempio la configurazione predefinita.

Il comportamento personalizzato quindi differisce anche in questo caso il comportamento predefinito arrestando dopo la riconnessione terzo tentativo errore anziché tentare di uno più riconnettersi tentativo in un altro, come verrebbe mostrata nella configurazione predefinita di 30 secondi.

Se si desidera maggiore controllo sulla temporizzazione e numero di automatico tentativi, di riconnessione `withAutomaticReconnect` accetta un oggetto che implementa le `IReconnectPolicy` interfaccia che dispone di un singolo metodo denominato `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds` accetta due argomenti, `previousRetryCount` e `elapsedMilliseconds`, che sono entrambi i numeri. Prima il primo tentativo di riconnessione, entrambe `previousRetryCount` e `elapsedMilliseconds` saranno pari a zero. Dopo ogni tentativo di ripetizione dei tentativi non riusciti `previousRetryCount` viene incrementato di uno e `elapsedMilliseconds` verrà aggiornato per riflettere la quantità di tempo impiegato per la riconnessione finora in millisecondi.

`nextRetryDelayInMilliseconds` deve restituire un numero che rappresenta il numero di millisecondi di attesa prima del successivo tentativo di riconnessione o `null` se il `HubConnection` deve interrompere la riconnessione.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: (previousRetryCount, elapsedMilliseconds) => {
          if (elapsedMilliseconds < 60000) {
            // If we've been reconnecting for less than 60 seconds so far,
            // wait between 0 and 10 seconds before the next reconnect attempt.
            return Math.random() * 10000;
          } else {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
          }
        })
    .build();
```

In alternativa, è possibile scrivere codice che si riconnetterà il client manualmente, come illustrato in [riconnettere manualmente](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Riconnettere manualmente

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Prima 3.0, non riconnettersi automaticamente il client JavaScript per SignalR. È necessario scrivere codice che si riconnetterà manualmente il client.

::: moniker-end

Il codice seguente illustra un approccio tipico riconnessione manuale:

1. Una funzione (in questo caso, il `start` (funzione)) viene creato per avviare la connessione.
1. Chiamare il `start` funzione della connessione `onclose` gestore dell'evento.

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

Un'implementazione reale potrebbe usare un backoff esponenziale o ripetere un numero specificato di volte prima di rinunciare. 

## <a name="additional-resources"></a>Risorse aggiuntive

* [Informazioni di riferimento sulle API JavaScript](/javascript/api/?view=signalr-js-latest)
* [Esercitazione di JavaScript](xref:tutorials/signalr)
* [Esercitazione su WebPack e TypeScript](xref:tutorials/signalr-typescript-webpack)
* [Hub](xref:signalr/hubs)
* [Client .NET](xref:signalr/dotnet-client)
* [Pubblicare in Azure](xref:signalr/publish-to-azure-web-app)
* [Richieste Multiorigine (CORS)](xref:security/cors)
* [Documentazione senza server di Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
