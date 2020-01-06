---
title: ASP.NET Core SignalR client JavaScript
author: bradygaster
description: Panoramica di ASP.NET Core SignalR client JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/javascript-client
ms.openlocfilehash: eaf737642cdbd7ab2b1b5c16538b47a70cddd332
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354691"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a>ASP.NET Core SignalR client JavaScript

Di [Rachel Appel](https://twitter.com/rachelappel)

La ASP.NET Core libreria client JavaScript SignalR consente agli sviluppatori di chiamare il codice dell'hub sul lato server.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-client-package"></a>Installare il pacchetto client di SignalR

La libreria client JavaScript SignalR viene distribuita come pacchetto [NPM](https://www.npmjs.com/) . Se si usa Visual Studio, eseguire `npm install` dal **Console di gestione pacchetti** mentre si trovano nella cartella radice. Per Visual Studio Code, eseguire il comando dal **terminale integrato**.

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

Monitoraggio prestazioni rete consente di installare il contenuto del pacchetto nel *node_modules\\@microsoft\signalr\dist\browser* cartella. Creare una nuova cartella denominata *signalr* sotto il *wwwroot\\lib* cartella. Copia il *signalr.js* del file per il *wwwroot\lib\signalr* cartella.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

Monitoraggio prestazioni rete consente di installare il contenuto del pacchetto nel *node_modules\\@aspnet\signalr\dist\browser* cartella. Creare una nuova cartella denominata *signalr* sotto il *wwwroot\\lib* cartella. Copia il *signalr.js* del file per il *wwwroot\lib\signalr* cartella.

::: moniker-end

## <a name="use-the-opno-locsignalr-javascript-client"></a>Usare il client JavaScript SignalR

Fare riferimento all'SignalR client JavaScript nell'elemento `<script>`.

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
* Eventuali argomenti definiti nel metodo dell'hub. Nell'esempio seguente, è il nome dell'argomento `message`. Il codice di esempio usa la sintassi della funzione freccia supportata nelle versioni correnti di tutti i browser principali, ad eccezione di Internet Explorer.

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Se si usa il servizio SignalR di Azure in *modalità senza server*, non è possibile chiamare i metodi dell'hub da un client. Per ulteriori informazioni, vedere la [documentazione del servizioSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

Il metodo `invoke` restituisce un [Suggerimento](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)JavaScript. Il `Promise` viene risolto con il valore restituito (se presente) quando il metodo nel server restituisce. Se il metodo sul server genera un errore, il `Promise` viene rifiutato con il messaggio di errore. Usare i metodi `then` e `catch` sulla `Promise` stessa per gestire questi casi (o la sintassi `await`).

Il metodo `send` restituisce una `Promise`JavaScript. Il `Promise` viene risolto quando il messaggio è stato inviato al server. Se si verifica un errore durante l'invio del messaggio, il `Promise` viene rifiutato con il messaggio di errore. Usare i metodi `then` e `catch` sulla `Promise` stessa per gestire questi casi (o la sintassi `await`).

> [!NOTE]
> L'utilizzo di `send` non attende fino a quando il server non ha ricevuto il messaggio. Di conseguenza, non è possibile restituire dati o errori dal server.

## <a name="call-client-methods-from-hub"></a>Chiamare i metodi client hub

Per ricevere messaggi dall'hub, definire un metodo usando il [sul](/javascript/api/%40aspnet/signalr/hubconnection#on) metodo del `HubConnection`.

* Il nome del metodo client JavaScript. Nell'esempio seguente, è il nome del metodo `ReceiveMessage`.
* Argomenti che dell'hub viene passata al metodo. Nell'esempio seguente, il valore dell'argomento è `message`.

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

Il codice precedente in `connection.on` viene eseguito quando viene chiamato codice lato server usando il [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) (metodo).

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR determina quale metodo client chiamare associando il nome del metodo e gli argomenti definiti in `SendAsync` e `connection.on`.

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

### <a name="automatically-reconnect"></a>Riconnetti automaticamente

Il client JavaScript per SignalR può essere configurato per la riconnessione automatica usando il metodo di `withAutomaticReconnect` in [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder). Per impostazione predefinita, non verrà riconnessa automaticamente.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

Senza parametri, `withAutomaticReconnect()` configura il client in modo che attenda rispettivamente 0, 2, 10 e 30 secondi prima di tentare ogni tentativo di riconnessione, arrestandosi dopo quattro tentativi non riusciti.

Prima di iniziare i tentativi di riconnessione, il `HubConnection` eseguirà la transizione allo stato di `HubConnectionState.Reconnecting` e genererà i callback `onreconnecting` invece di passare allo stato `Disconnected` e attivare i callback `onclose` come un `HubConnection` senza la riconnessione automatica configurata. Questo consente di avvisare gli utenti che la connessione è stata persa e di disabilitare gli elementi dell'interfaccia utente.

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Se il client si riconnette correttamente entro i primi quattro tentativi, il `HubConnection` eseguirà la transizione allo stato di `Connected` e attiverà i callback di `onreconnected`. Questo consente di informare gli utenti che è stata ristabilita la connessione.

Poiché la connessione ha un aspetto completamente nuovo per il server, al callback `onreconnected` verrà fornito un nuovo `connectionId`.

> [!WARNING]
> Il `connectionId` parametro del callback `onreconnected` non sarà definito se il `HubConnection` è stato configurato per [ignorare la negoziazione](xref:signalr/configuration#configure-client-options).

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

`withAutomaticReconnect()` non configurerà l'`HubConnection` per ritentare gli errori iniziali di avvio, quindi gli errori di avvio devono essere gestiti manualmente:

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

Se il client non si riconnette correttamente entro i primi quattro tentativi, il `HubConnection` eseguirà la transizione allo stato `Disconnected` e attiverà i callback [OnClose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) . Questo consente di informare gli utenti che la connessione è stata persa definitivamente e si consiglia di aggiornare la pagina:

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

Per configurare un numero personalizzato di tentativi di riconnessione prima di disconnettere o modificare la temporizzazione di riconnessione, `withAutomaticReconnect` accetta una matrice di numeri che rappresenta il ritardo in millisecondi di attesa prima di avviare ogni tentativo di riconnessione.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

Nell'esempio precedente la `HubConnection` viene configurata per avviare il tentativo di riconnessione immediatamente dopo la perdita della connessione. Questo vale anche per la configurazione predefinita.

Se il primo tentativo di riconnessione ha esito negativo, anche il secondo tentativo di riconnessione verrà avviato immediatamente anziché attendere 2 secondi come nella configurazione predefinita.

Se il secondo tentativo di riconnessione ha esito negativo, il terzo tentativo di riconnessione si avvierà entro 10 secondi, che è simile alla configurazione predefinita.

Il comportamento personalizzato, quindi, differisce di nuovo dal comportamento predefinito arrestando il terzo tentativo di riconnessione anziché tentare un ulteriore tentativo di riconnessione in altri 30 secondi, come nella configurazione predefinita.

Se si desidera un maggiore controllo sull'intervallo e sul numero di tentativi di riconnessione automatici, `withAutomaticReconnect` accetta un oggetto che implementa l'interfaccia `IRetryPolicy`, che dispone di un solo metodo denominato `nextRetryDelayInMilliseconds`.

`nextRetryDelayInMilliseconds` accetta un solo argomento con il tipo `RetryContext`. Il `RetryContext` dispone di tre proprietà: `previousRetryCount`, `elapsedMilliseconds` e `retryReason`, ovvero un `number`, un `number` e un `Error` rispettivamente. Prima del primo tentativo di riconnessione, sia `previousRetryCount` che `elapsedMilliseconds` saranno zero e il `retryReason` sarà l'errore che ha causato la perdita della connessione. Dopo ogni nuovo tentativo non riuscito, `previousRetryCount` verrà incrementato di uno, `elapsedMilliseconds` verrà aggiornato per riflettere la quantità di tempo impiegato per la riconnessione fino a questo punto, in millisecondi e il `retryReason` sarà l'errore che ha causato l'ultimo tentativo di riconnessione.

`nextRetryDelayInMilliseconds` deve restituire un numero che rappresenta il numero di millisecondi di attesa prima del successivo tentativo di riconnessione o `null` se il `HubConnection` deve arrestare la riconnessione.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect({
        nextRetryDelayInMilliseconds: retryContext => {
            if (retryContext.elapsedMilliseconds < 60000) {
                // If we've been reconnecting for less than 60 seconds so far,
                // wait between 0 and 10 seconds before the next reconnect attempt.
                return Math.random() * 10000;
            } else {
                // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
                return null;
            }
        }
    })
    .build();
```

In alternativa, è possibile scrivere codice per riconnettere manualmente il client, come illustrato in [riconnessione manuale](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Riconnessione manuale

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> Prima del 3,0, il client JavaScript per SignalR non si riconnette automaticamente. È necessario scrivere codice che si riconnetterà manualmente il client.

::: moniker-end

Il codice seguente illustra un tipico approccio di riconnessione manuale:

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
* [Documentazione senza server del servizio SignalR di Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)
