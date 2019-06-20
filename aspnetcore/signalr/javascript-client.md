---
title: ASP.NET Core SignalR JavaScript client
author: bradygaster
description: Panoramica di ASP.NET Core SignalR JavaScript client.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/javascript-client
ms.openlocfilehash: 1565aa38a69113781d7c272a1710298cccc1f045
ms.sourcegitcommit: 3eedd6180fbbdcb81a8e1ebdbeb035bf4f2feb92
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284509"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="4b924-103">ASP.NET Core SignalR JavaScript client</span><span class="sxs-lookup"><span data-stu-id="4b924-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="4b924-104">[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="4b924-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="4b924-105">La libreria client di ASP.NET Core SignalR JavaScript consente agli sviluppatori di chiamare il codice dell'hub sul lato server.</span><span class="sxs-lookup"><span data-stu-id="4b924-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="4b924-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4b924-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="4b924-107">Installare il pacchetto client di SignalR</span><span class="sxs-lookup"><span data-stu-id="4b924-107">Install the SignalR client package</span></span>

<span data-ttu-id="4b924-108">La libreria client SignalR JavaScript viene fornita come un [npm](https://www.npmjs.com/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="4b924-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="4b924-109">Se si usa Visual Studio, eseguire `npm install` dal **Console di gestione pacchetti** mentre si trovano nella cartella radice.</span><span class="sxs-lookup"><span data-stu-id="4b924-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="4b924-110">Per Visual Studio Code, eseguire il comando dal **terminale integrato**.</span><span class="sxs-lookup"><span data-stu-id="4b924-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="4b924-111">Monitoraggio prestazioni rete consente di installare il contenuto del pacchetto nel *node_modules\\@aspnet\signalr\dist\browser* cartella.</span><span class="sxs-lookup"><span data-stu-id="4b924-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="4b924-112">Creare una nuova cartella denominata *signalr* sotto il *wwwroot\\lib* cartella.</span><span class="sxs-lookup"><span data-stu-id="4b924-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="4b924-113">Copia il *signalr.js* del file per il *wwwroot\lib\signalr* cartella.</span><span class="sxs-lookup"><span data-stu-id="4b924-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="4b924-114">Usare il client SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="4b924-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="4b924-115">Fare riferimento a client SignalR JavaScript il `<script>` elemento.</span><span class="sxs-lookup"><span data-stu-id="4b924-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="4b924-116">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="4b924-116">Connect to a hub</span></span>

<span data-ttu-id="4b924-117">Il codice seguente crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="4b924-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="4b924-118">Nome dell'hub è maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="4b924-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="4b924-119">Connessioni cross-origin</span><span class="sxs-lookup"><span data-stu-id="4b924-119">Cross-origin connections</span></span>

<span data-ttu-id="4b924-120">In genere, i browser il carico delle connessioni appartenenti allo stesso dominio della pagina richiesta.</span><span class="sxs-lookup"><span data-stu-id="4b924-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="4b924-121">Tuttavia, esistono alcuni casi quando è necessaria una connessione a un altro dominio.</span><span class="sxs-lookup"><span data-stu-id="4b924-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="4b924-122">Per impedire la lettura dei dati sensibili da un altro sito, di un sito dannoso [le connessioni cross-origin](xref:security/cors) sono disabilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4b924-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="4b924-123">Per consentire a una richiesta multiorigine, abilitarla nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="4b924-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="4b924-124">Chiamare i metodi dell'hub da client</span><span class="sxs-lookup"><span data-stu-id="4b924-124">Call hub methods from client</span></span>

<span data-ttu-id="4b924-125">Client JavaScript chiamano metodi pubblici in hub tramite il [richiamare](/javascript/api/%40aspnet/signalr/hubconnection#invoke) metodo per il [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="4b924-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="4b924-126">Il `invoke` metodo accetta due argomenti:</span><span class="sxs-lookup"><span data-stu-id="4b924-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="4b924-127">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="4b924-127">The name of the hub method.</span></span> <span data-ttu-id="4b924-128">Nell'esempio seguente, è il nome del metodo dell'hub `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="4b924-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="4b924-129">Eventuali argomenti definiti nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="4b924-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="4b924-130">Nell'esempio seguente, è il nome dell'argomento `message`.</span><span class="sxs-lookup"><span data-stu-id="4b924-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="4b924-131">Il codice di esempio Usa sintassi della funzione freccia è supportata nelle versioni correnti di tutti i principali browser, ad eccezione di Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="4b924-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="4b924-132">Se si usa il servizio Azure SignalR in *modalità senza server*, non è possibile chiamare i metodi dell'hub da un client.</span><span class="sxs-lookup"><span data-stu-id="4b924-132">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="4b924-133">Per altre informazioni, vedere la [documentazione di SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="4b924-133">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="4b924-134">Il `invoke` metodo viene restituito un JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span><span class="sxs-lookup"><span data-stu-id="4b924-134">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="4b924-135">Il `Promise` viene risolto con il valore restituito (se presente) quando restituito dal metodo nel server.</span><span class="sxs-lookup"><span data-stu-id="4b924-135">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="4b924-136">Se il metodo nel server genera un errore, il `Promise` viene rifiutato con il messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="4b924-136">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="4b924-137">Usare la `then` e `catch` metodi sulle `Promise` per gestire questi casi (o `await` sintassi).</span><span class="sxs-lookup"><span data-stu-id="4b924-137">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="4b924-138">Il `send` metodo viene restituito un JavaScript `Promise`.</span><span class="sxs-lookup"><span data-stu-id="4b924-138">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="4b924-139">Il `Promise` è risolto quando il messaggio è stato inviato al server.</span><span class="sxs-lookup"><span data-stu-id="4b924-139">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="4b924-140">Se si verifica un errore di invio del messaggio, il `Promise` viene rifiutato con il messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="4b924-140">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="4b924-141">Usare la `then` e `catch` metodi sulle `Promise` per gestire questi casi (o `await` sintassi).</span><span class="sxs-lookup"><span data-stu-id="4b924-141">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="4b924-142">Usando `send` senza aspettare il server ha ricevuto il messaggio.</span><span class="sxs-lookup"><span data-stu-id="4b924-142">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="4b924-143">Non è pertanto possibile restituire errori o i dati dal server.</span><span class="sxs-lookup"><span data-stu-id="4b924-143">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="4b924-144">Chiamare i metodi client hub</span><span class="sxs-lookup"><span data-stu-id="4b924-144">Call client methods from hub</span></span>

<span data-ttu-id="4b924-145">Per ricevere messaggi dall'hub, definire un metodo usando il [sul](/javascript/api/%40aspnet/signalr/hubconnection#on) metodo del `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="4b924-145">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="4b924-146">Il nome del metodo client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4b924-146">The name of the JavaScript client method.</span></span> <span data-ttu-id="4b924-147">Nell'esempio seguente, è il nome del metodo `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="4b924-147">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="4b924-148">Argomenti che dell'hub viene passata al metodo.</span><span class="sxs-lookup"><span data-stu-id="4b924-148">Arguments the hub passes to the method.</span></span> <span data-ttu-id="4b924-149">Nell'esempio seguente, il valore dell'argomento è `message`.</span><span class="sxs-lookup"><span data-stu-id="4b924-149">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="4b924-150">Il codice precedente in `connection.on` viene eseguito quando viene chiamato codice lato server usando il [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) (metodo).</span><span class="sxs-lookup"><span data-stu-id="4b924-150">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="4b924-151">SignalR determina quale metodo di client per chiamare creando una corrispondenza tra il nome del metodo e gli argomenti definiti nella `SendAsync` e `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="4b924-151">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="4b924-152">Come procedura consigliata, chiamare il [avviare](/javascript/api/%40aspnet/signalr/hubconnection#start) metodo sulle `HubConnection` dopo `on`.</span><span class="sxs-lookup"><span data-stu-id="4b924-152">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="4b924-153">In tal modo che i gestori registrati prima di tutti i messaggi vengono ricevuti.</span><span class="sxs-lookup"><span data-stu-id="4b924-153">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="4b924-154">La registrazione e la gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="4b924-154">Error handling and logging</span></span>

<span data-ttu-id="4b924-155">Catena di una `catch` alla fine del metodo di `start` metodo per gestire gli errori lato client.</span><span class="sxs-lookup"><span data-stu-id="4b924-155">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="4b924-156">Usare `console.error` agli errori di output alla console del browser.</span><span class="sxs-lookup"><span data-stu-id="4b924-156">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="4b924-157">Analisi di log lato client installazione passando un logger e il tipo di evento per registrare quando viene stabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="4b924-157">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="4b924-158">I messaggi vengono registrati con il livello di log specificato e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="4b924-158">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="4b924-159">Livelli di log disponibili sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="4b924-159">Available log levels are as follows:</span></span>

* <span data-ttu-id="4b924-160">`signalR.LogLevel.Error` &ndash; Messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="4b924-160">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="4b924-161">I log `Error` solo i messaggi.</span><span class="sxs-lookup"><span data-stu-id="4b924-161">Logs `Error` messages only.</span></span>
* <span data-ttu-id="4b924-162">`signalR.LogLevel.Warning` &ndash; Messaggi di avviso sui potenziali errori.</span><span class="sxs-lookup"><span data-stu-id="4b924-162">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="4b924-163">I log `Warning`, e `Error` messaggi.</span><span class="sxs-lookup"><span data-stu-id="4b924-163">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="4b924-164">`signalR.LogLevel.Information` &ndash; Messaggi di stato senza errori.</span><span class="sxs-lookup"><span data-stu-id="4b924-164">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="4b924-165">I log `Information`, `Warning`, e `Error` messaggi.</span><span class="sxs-lookup"><span data-stu-id="4b924-165">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="4b924-166">`signalR.LogLevel.Trace` &ndash; I messaggi di traccia.</span><span class="sxs-lookup"><span data-stu-id="4b924-166">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="4b924-167">Registra tutti gli elementi, inclusi i dati trasportati tra l'hub e client.</span><span class="sxs-lookup"><span data-stu-id="4b924-167">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="4b924-168">Usare la [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metodo sul [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) per configurare il livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="4b924-168">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="4b924-169">I messaggi vengono registrati nella console del browser.</span><span class="sxs-lookup"><span data-stu-id="4b924-169">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="4b924-170">Riconnettere i client</span><span class="sxs-lookup"><span data-stu-id="4b924-170">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="4b924-171">La riconnessione automatica</span><span class="sxs-lookup"><span data-stu-id="4b924-171">Automatically reconnect</span></span>

<span data-ttu-id="4b924-172">Il client JavaScript per SignalR può essere configurato per la riconnessione automatica usando la `withAutomaticReconnect` metodo sul [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="4b924-172">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="4b924-173">Non riconnettersi automaticamente per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4b924-173">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="4b924-174">Senza parametri, `withAutomaticReconnect()` consente di configurare il client per l'attesa di 0, 2, 10 e 30 secondi rispettivamente prima di provare ogni tentativo di riconnessione, fermandosi quando quattro tentativi non riusciti.</span><span class="sxs-lookup"><span data-stu-id="4b924-174">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="4b924-175">Prima di avviare qualsiasi tentativo di riconnessione, il `HubConnection` verrà transizione per il `HubConnectionState.Reconnecting` sullo stato e la generazione di relativo `onreconnecting` callback anziché in fase di transizione per il `Disconnected` dello stato e l'attivazione relativo `onclose` i callback, ad esempio un `HubConnection`senza la riconnessione automatica configurata.</span><span class="sxs-lookup"><span data-stu-id="4b924-175">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="4b924-176">Ciò offre un'opportunità per avvisare gli utenti che la connessione è stata persa e disattivare gli elementi dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="4b924-176">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="4b924-177">Se il client si riconnette correttamente entro i primi quattro diversi, il `HubConnection` passerà al `Connected` lo stato e la generazione di relativo `onreconnected` i callback.</span><span class="sxs-lookup"><span data-stu-id="4b924-177">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="4b924-178">Ciò offre un'opportunità per informare gli utenti che è stata ristabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="4b924-178">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="4b924-179">Poiché la connessione sia completamente nuovo al server, un nuovo `connectionId` riceveranno la `onreconnected` callback.</span><span class="sxs-lookup"><span data-stu-id="4b924-179">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="4b924-180">Il `onreconnected` del callback `connectionId` parametro sarà indefinito se il `HubConnection` è stato configurato per [ignorare la negoziazione](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="4b924-180">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="4b924-181">`withAutomaticReconnect()` non configurare il `HubConnection` tentativo iniziale di avvio non riuscite, in modo che gli errori di avvio devono essere gestite manualmente:</span><span class="sxs-lookup"><span data-stu-id="4b924-181">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="4b924-182">Se il client non è stata riconnettersi entro i primi quattro diversi, il `HubConnection` verranno transizione per il `Disconnected` lo stato e la generazione di relativo [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) i callback.</span><span class="sxs-lookup"><span data-stu-id="4b924-182">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="4b924-183">Ciò offre un'opportunità per informare gli utenti la connessione viene definitivamente perduto e consiglia di aggiornare la pagina:</span><span class="sxs-lookup"><span data-stu-id="4b924-183">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="4b924-184">Per configurare un numero di tentativi di riconnessione prima della disconnessione personalizzato o modificare l'intervallo di tempo di riconnessione, `withAutomaticReconnect` accetta una matrice di numeri che rappresenta il ritardo in millisecondi di attesa prima di avviare ogni tentativo di riconnessione.</span><span class="sxs-lookup"><span data-stu-id="4b924-184">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="4b924-185">Nell'esempio precedente viene configurata la `HubConnection` per avviare il tentativo di riconnessioni immediatamente dopo la connessione viene persa.</span><span class="sxs-lookup"><span data-stu-id="4b924-185">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="4b924-186">Questo vale anche per la configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4b924-186">This is also true for the default configuration.</span></span>

<span data-ttu-id="4b924-187">Se non riesce al primo tentativo di riconnessione, il secondo tentativo di riconnessione anche inizierà immediatamente anziché attendere 2 secondi, come verrebbe mostrata nella configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4b924-187">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="4b924-188">Se ha esito negativo al secondo tentativo di riconnessione, il terzo tentativo di riconnessione verrà avviato entro 10 secondi, ovvero anche in questo caso, ad esempio la configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4b924-188">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="4b924-189">Il comportamento personalizzato quindi differisce anche in questo caso il comportamento predefinito arrestando dopo la riconnessione terzo tentativo errore anziché tentare di uno più riconnettersi tentativo in un altro, come verrebbe mostrata nella configurazione predefinita di 30 secondi.</span><span class="sxs-lookup"><span data-stu-id="4b924-189">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="4b924-190">Se si desidera maggiore controllo sulla temporizzazione e numero di automatico tentativi, di riconnessione `withAutomaticReconnect` accetta un oggetto che implementa le `IReconnectPolicy` interfaccia che dispone di un singolo metodo denominato `nextRetryDelayInMilliseconds`.</span><span class="sxs-lookup"><span data-stu-id="4b924-190">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IReconnectPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="4b924-191">`nextRetryDelayInMilliseconds` accetta due argomenti, `previousRetryCount` e `elapsedMilliseconds`, che sono entrambi i numeri.</span><span class="sxs-lookup"><span data-stu-id="4b924-191">`nextRetryDelayInMilliseconds` takes two arguments, `previousRetryCount` and `elapsedMilliseconds`, which are both numbers.</span></span> <span data-ttu-id="4b924-192">Prima il primo tentativo di riconnessione, entrambe `previousRetryCount` e `elapsedMilliseconds` saranno pari a zero.</span><span class="sxs-lookup"><span data-stu-id="4b924-192">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero.</span></span> <span data-ttu-id="4b924-193">Dopo ogni tentativo di ripetizione dei tentativi non riusciti `previousRetryCount` viene incrementato di uno e `elapsedMilliseconds` verrà aggiornato per riflettere la quantità di tempo impiegato per la riconnessione finora in millisecondi.</span><span class="sxs-lookup"><span data-stu-id="4b924-193">After each failed retry attempt, `previousRetryCount` will be incremented by one and `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds.</span></span>

<span data-ttu-id="4b924-194">`nextRetryDelayInMilliseconds` deve restituire un numero che rappresenta il numero di millisecondi di attesa prima del successivo tentativo di riconnessione o `null` se il `HubConnection` deve interrompere la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="4b924-194">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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
        }
    })
    .build();
```

<span data-ttu-id="4b924-195">In alternativa, è possibile scrivere codice che si riconnetterà il client manualmente, come illustrato in [riconnettere manualmente](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="4b924-195">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="4b924-196">Riconnettere manualmente</span><span class="sxs-lookup"><span data-stu-id="4b924-196">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="4b924-197">Prima 3.0, non riconnettersi automaticamente il client JavaScript per SignalR.</span><span class="sxs-lookup"><span data-stu-id="4b924-197">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="4b924-198">È necessario scrivere codice che si riconnetterà manualmente il client.</span><span class="sxs-lookup"><span data-stu-id="4b924-198">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="4b924-199">Il codice seguente illustra un approccio tipico riconnessione manuale:</span><span class="sxs-lookup"><span data-stu-id="4b924-199">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="4b924-200">Una funzione (in questo caso, il `start` (funzione)) viene creato per avviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="4b924-200">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="4b924-201">Chiamare il `start` funzione della connessione `onclose` gestore dell'evento.</span><span class="sxs-lookup"><span data-stu-id="4b924-201">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="4b924-202">Un'implementazione reale potrebbe usare un backoff esponenziale o ripetere un numero specificato di volte prima di rinunciare.</span><span class="sxs-lookup"><span data-stu-id="4b924-202">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4b924-203">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4b924-203">Additional resources</span></span>

* [<span data-ttu-id="4b924-204">Informazioni di riferimento sulle API JavaScript</span><span class="sxs-lookup"><span data-stu-id="4b924-204">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="4b924-205">Esercitazione di JavaScript</span><span class="sxs-lookup"><span data-stu-id="4b924-205">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="4b924-206">Esercitazione su WebPack e TypeScript</span><span class="sxs-lookup"><span data-stu-id="4b924-206">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="4b924-207">Hub</span><span class="sxs-lookup"><span data-stu-id="4b924-207">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4b924-208">Client .NET</span><span class="sxs-lookup"><span data-stu-id="4b924-208">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="4b924-209">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="4b924-209">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="4b924-210">Richieste Multiorigine (CORS)</span><span class="sxs-lookup"><span data-stu-id="4b924-210">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* [<span data-ttu-id="4b924-211">Documentazione senza server di Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="4b924-211">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
