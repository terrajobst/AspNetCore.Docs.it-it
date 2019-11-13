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
ms.openlocfilehash: 926160a41c82853d83890f0d52b14d7d5561a990
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963777"
---
# <a name="aspnet-core-opno-locsignalr-javascript-client"></a><span data-ttu-id="c1834-103">ASP.NET Core SignalR client JavaScript</span><span class="sxs-lookup"><span data-stu-id="c1834-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="c1834-104">[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="c1834-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="c1834-105">La ASP.NET Core libreria client JavaScript SignalR consente agli sviluppatori di chiamare il codice dell'hub sul lato server.</span><span class="sxs-lookup"><span data-stu-id="c1834-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="c1834-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c1834-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-opno-locsignalr-client-package"></a><span data-ttu-id="c1834-107">Installare il pacchetto client di SignalR</span><span class="sxs-lookup"><span data-stu-id="c1834-107">Install the SignalR client package</span></span>

<span data-ttu-id="c1834-108">La libreria client JavaScript SignalR viene distribuita come pacchetto [NPM](https://www.npmjs.com/) .</span><span class="sxs-lookup"><span data-stu-id="c1834-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="c1834-109">Se si usa Visual Studio, eseguire `npm install` dalla console di **Gestione pacchetti** mentre si trova nella cartella radice.</span><span class="sxs-lookup"><span data-stu-id="c1834-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="c1834-110">Per Visual Studio Code, eseguire il comando dal **terminale integrato**.</span><span class="sxs-lookup"><span data-stu-id="c1834-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

::: moniker range=">= aspnetcore-3.0"

  ```console
  npm init -y
  npm install @microsoft/signalr
  ```

<span data-ttu-id="c1834-111">NPM installa il contenuto del pacchetto nella cartella *node_modules\\@microsoft\signalr\dist\browser* .</span><span class="sxs-lookup"><span data-stu-id="c1834-111">npm installs the package contents in the *node_modules\\@microsoft\signalr\dist\browser* folder.</span></span> <span data-ttu-id="c1834-112">Creare una nuova cartella denominata *SignalR* nella cartella *wwwroot\\lib* .</span><span class="sxs-lookup"><span data-stu-id="c1834-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="c1834-113">Copiare il file *SignalR. js* nella cartella *wwwroot\lib\signalr* .</span><span class="sxs-lookup"><span data-stu-id="c1834-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="c1834-114">NPM installa il contenuto del pacchetto nella cartella *node_modules\\@aspnet\signalr\dist\browser* .</span><span class="sxs-lookup"><span data-stu-id="c1834-114">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="c1834-115">Creare una nuova cartella denominata *SignalR* nella cartella *wwwroot\\lib* .</span><span class="sxs-lookup"><span data-stu-id="c1834-115">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="c1834-116">Copiare il file *SignalR. js* nella cartella *wwwroot\lib\signalr* .</span><span class="sxs-lookup"><span data-stu-id="c1834-116">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

::: moniker-end

## <a name="use-the-opno-locsignalr-javascript-client"></a><span data-ttu-id="c1834-117">Usare il client JavaScript SignalR</span><span class="sxs-lookup"><span data-stu-id="c1834-117">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="c1834-118">Fare riferimento all'SignalR client JavaScript nell'elemento `<script>`.</span><span class="sxs-lookup"><span data-stu-id="c1834-118">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="c1834-119">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="c1834-119">Connect to a hub</span></span>

<span data-ttu-id="c1834-120">Il codice seguente crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="c1834-120">The following code creates and starts a connection.</span></span> <span data-ttu-id="c1834-121">Il nome dell'hub non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="c1834-121">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a><span data-ttu-id="c1834-122">Connessioni tra le origini</span><span class="sxs-lookup"><span data-stu-id="c1834-122">Cross-origin connections</span></span>

<span data-ttu-id="c1834-123">In genere, i browser caricano le connessioni dallo stesso dominio della pagina richiesta.</span><span class="sxs-lookup"><span data-stu-id="c1834-123">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="c1834-124">In alcuni casi, tuttavia, è necessaria una connessione a un altro dominio.</span><span class="sxs-lookup"><span data-stu-id="c1834-124">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="c1834-125">Per impedire a un sito dannoso di leggere dati sensibili da un altro sito, le [connessioni tra le origini](xref:security/cors) sono disabilitate per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c1834-125">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="c1834-126">Per consentire una richiesta tra origini, abilitarla nella classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="c1834-126">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="c1834-127">Chiamare i metodi dell'hub dal client</span><span class="sxs-lookup"><span data-stu-id="c1834-127">Call hub methods from client</span></span>

<span data-ttu-id="c1834-128">I client JavaScript chiamano metodi pubblici sugli hub tramite il metodo [Invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) di [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="c1834-128">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="c1834-129">Il metodo `invoke` accetta due argomenti:</span><span class="sxs-lookup"><span data-stu-id="c1834-129">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="c1834-130">Nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="c1834-130">The name of the hub method.</span></span> <span data-ttu-id="c1834-131">Nell'esempio seguente il nome del metodo nell'hub è `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="c1834-131">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="c1834-132">Qualsiasi argomento definito nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="c1834-132">Any arguments defined in the hub method.</span></span> <span data-ttu-id="c1834-133">Nell'esempio seguente il nome dell'argomento è `message`.</span><span class="sxs-lookup"><span data-stu-id="c1834-133">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="c1834-134">Il codice di esempio usa la sintassi della funzione freccia supportata nelle versioni correnti di tutti i browser principali, ad eccezione di Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="c1834-134">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> <span data-ttu-id="c1834-135">Se si usa il servizio SignalR di Azure in *modalità senza server*, non è possibile chiamare i metodi dell'hub da un client.</span><span class="sxs-lookup"><span data-stu-id="c1834-135">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="c1834-136">Per ulteriori informazioni, vedere la [documentazione del servizioSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="c1834-136">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

<span data-ttu-id="c1834-137">Il metodo `invoke` restituisce un [Suggerimento](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise)JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c1834-137">The `invoke` method returns a JavaScript [Promise](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise).</span></span> <span data-ttu-id="c1834-138">Il `Promise` viene risolto con il valore restituito (se presente) quando il metodo nel server restituisce.</span><span class="sxs-lookup"><span data-stu-id="c1834-138">The `Promise` is resolved with the return value (if any) when the method on the server returns.</span></span> <span data-ttu-id="c1834-139">Se il metodo sul server genera un errore, il `Promise` viene rifiutato con il messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="c1834-139">If the method on the server throws an error, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="c1834-140">Usare i metodi `then` e `catch` sulla `Promise` stessa per gestire questi casi (o la sintassi `await`).</span><span class="sxs-lookup"><span data-stu-id="c1834-140">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

<span data-ttu-id="c1834-141">Il metodo `send` restituisce una `Promise`JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c1834-141">The `send` method returns a JavaScript `Promise`.</span></span> <span data-ttu-id="c1834-142">Il `Promise` viene risolto quando il messaggio è stato inviato al server.</span><span class="sxs-lookup"><span data-stu-id="c1834-142">The `Promise` is resolved when the message has been sent to the server.</span></span> <span data-ttu-id="c1834-143">Se si verifica un errore durante l'invio del messaggio, il `Promise` viene rifiutato con il messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="c1834-143">If there is an error sending the message, the `Promise` is rejected with the error message.</span></span> <span data-ttu-id="c1834-144">Usare i metodi `then` e `catch` sulla `Promise` stessa per gestire questi casi (o la sintassi `await`).</span><span class="sxs-lookup"><span data-stu-id="c1834-144">Use the `then` and `catch` methods on the `Promise` itself to handle these cases (or `await` syntax).</span></span>

> [!NOTE]
> <span data-ttu-id="c1834-145">L'utilizzo di `send` non attende fino a quando il server non ha ricevuto il messaggio.</span><span class="sxs-lookup"><span data-stu-id="c1834-145">Using `send` doesn't wait until the server has received the message.</span></span> <span data-ttu-id="c1834-146">Di conseguenza, non è possibile restituire dati o errori dal server.</span><span class="sxs-lookup"><span data-stu-id="c1834-146">Consequently, it's not possible to return data or errors from the server.</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="c1834-147">Chiamare i metodi client dall'hub</span><span class="sxs-lookup"><span data-stu-id="c1834-147">Call client methods from hub</span></span>

<span data-ttu-id="c1834-148">Per ricevere messaggi dall'hub, definire un metodo usando il metodo [on](/javascript/api/%40aspnet/signalr/hubconnection#on) del `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="c1834-148">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="c1834-149">Nome del metodo client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="c1834-149">The name of the JavaScript client method.</span></span> <span data-ttu-id="c1834-150">Nell'esempio seguente il nome del metodo è `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="c1834-150">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="c1834-151">Argomenti passati dall'hub al metodo.</span><span class="sxs-lookup"><span data-stu-id="c1834-151">Arguments the hub passes to the method.</span></span> <span data-ttu-id="c1834-152">Nell'esempio seguente il valore dell'argomento è `message`.</span><span class="sxs-lookup"><span data-stu-id="c1834-152">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="c1834-153">Il codice precedente in `connection.on` viene eseguito quando il codice sul lato server lo chiama usando il metodo [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) .</span><span class="sxs-lookup"><span data-stu-id="c1834-153">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR<span data-ttu-id="c1834-154"> determina quale metodo client chiamare associando il nome del metodo e gli argomenti definiti in `SendAsync` e `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="c1834-154"> determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="c1834-155">Come procedura consigliata, chiamare il metodo [Start](/javascript/api/%40aspnet/signalr/hubconnection#start) sul `HubConnection` dopo la `on`.</span><span class="sxs-lookup"><span data-stu-id="c1834-155">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="c1834-156">In questo modo si garantisce che i gestori vengano registrati prima della ricezione dei messaggi.</span><span class="sxs-lookup"><span data-stu-id="c1834-156">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="c1834-157">Gestione e registrazione degli errori</span><span class="sxs-lookup"><span data-stu-id="c1834-157">Error handling and logging</span></span>

<span data-ttu-id="c1834-158">Concatenare un metodo di `catch` alla fine del metodo `start` per gestire gli errori sul lato client.</span><span class="sxs-lookup"><span data-stu-id="c1834-158">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="c1834-159">Usare `console.error` per restituire gli errori alla console del browser.</span><span class="sxs-lookup"><span data-stu-id="c1834-159">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

<span data-ttu-id="c1834-160">Configurare la traccia del log sul lato client passando un logger e un tipo di evento da registrare quando viene stabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="c1834-160">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="c1834-161">I messaggi vengono registrati con il livello di registrazione specificato e un valore superiore.</span><span class="sxs-lookup"><span data-stu-id="c1834-161">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="c1834-162">I livelli di log disponibili sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1834-162">Available log levels are as follows:</span></span>

* <span data-ttu-id="c1834-163">`signalR.LogLevel.Error` &ndash; messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="c1834-163">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="c1834-164">Registra `Error` solo i messaggi.</span><span class="sxs-lookup"><span data-stu-id="c1834-164">Logs `Error` messages only.</span></span>
* <span data-ttu-id="c1834-165">`signalR.LogLevel.Warning` &ndash; messaggi di avviso relativi a potenziali errori.</span><span class="sxs-lookup"><span data-stu-id="c1834-165">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="c1834-166">Registra `Warning`e `Error` messaggi.</span><span class="sxs-lookup"><span data-stu-id="c1834-166">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="c1834-167">`signalR.LogLevel.Information` &ndash; messaggi di stato senza errori.</span><span class="sxs-lookup"><span data-stu-id="c1834-167">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="c1834-168">Registra `Information`, `Warning`e `Error` messaggi.</span><span class="sxs-lookup"><span data-stu-id="c1834-168">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="c1834-169">`signalR.LogLevel.Trace` &ndash; i messaggi di traccia.</span><span class="sxs-lookup"><span data-stu-id="c1834-169">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="c1834-170">Registra tutti gli elementi, inclusi i dati trasportati tra l'hub e il client.</span><span class="sxs-lookup"><span data-stu-id="c1834-170">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="c1834-171">Usare il metodo [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) in [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) per configurare il livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="c1834-171">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="c1834-172">I messaggi vengono registrati nella console del browser.</span><span class="sxs-lookup"><span data-stu-id="c1834-172">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="c1834-173">Riconnettere i client</span><span class="sxs-lookup"><span data-stu-id="c1834-173">Reconnect clients</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="c1834-174">Riconnetti automaticamente</span><span class="sxs-lookup"><span data-stu-id="c1834-174">Automatically reconnect</span></span>

<span data-ttu-id="c1834-175">Il client JavaScript per SignalR può essere configurato per la riconnessione automatica usando il metodo di `withAutomaticReconnect` in [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="c1834-175">The JavaScript client for SignalR can be configured to automatically reconnect using the `withAutomaticReconnect` method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder).</span></span> <span data-ttu-id="c1834-176">Per impostazione predefinita, non verrà riconnessa automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c1834-176">It won't automatically reconnect by default.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect()
    .build();
```

<span data-ttu-id="c1834-177">Senza parametri, `withAutomaticReconnect()` configura il client in modo che attenda rispettivamente 0, 2, 10 e 30 secondi prima di tentare ogni tentativo di riconnessione, arrestandosi dopo quattro tentativi non riusciti.</span><span class="sxs-lookup"><span data-stu-id="c1834-177">Without any parameters, `withAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="c1834-178">Prima di iniziare i tentativi di riconnessione, il `HubConnection` eseguirà la transizione allo stato di `HubConnectionState.Reconnecting` e genererà i callback `onreconnecting` invece di passare allo stato `Disconnected` e attivare i callback `onclose` come un `HubConnection` senza la riconnessione automatica configurata.</span><span class="sxs-lookup"><span data-stu-id="c1834-178">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire its `onreconnecting` callbacks instead of transitioning to the `Disconnected` state and triggering its `onclose` callbacks like a `HubConnection` without automatic reconnect configured.</span></span> <span data-ttu-id="c1834-179">Questo consente di avvisare gli utenti che la connessione è stata persa e di disabilitare gli elementi dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="c1834-179">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span>

```javascript
connection.onreconnecting((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Reconnecting);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection lost due to error "${error}". Reconnecting.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="c1834-180">Se il client si riconnette correttamente entro i primi quattro tentativi, il `HubConnection` eseguirà la transizione allo stato di `Connected` e attiverà i callback di `onreconnected`.</span><span class="sxs-lookup"><span data-stu-id="c1834-180">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire its `onreconnected` callbacks.</span></span> <span data-ttu-id="c1834-181">Questo consente di informare gli utenti che è stata ristabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="c1834-181">This provides an opportunity to inform users the connection has been reestablished.</span></span>

<span data-ttu-id="c1834-182">Poiché la connessione ha un aspetto completamente nuovo per il server, al callback `onreconnected` verrà fornito un nuovo `connectionId`.</span><span class="sxs-lookup"><span data-stu-id="c1834-182">Since the connection looks entirely new to the server, a new `connectionId` will be provided to the `onreconnected` callback.</span></span>

> [!WARNING]
> <span data-ttu-id="c1834-183">Il `connectionId` parametro del callback `onreconnected` non sarà definito se il `HubConnection` è stato configurato per [ignorare la negoziazione](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="c1834-183">The `onreconnected` callback's `connectionId` parameter will be undefined if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```javascript
connection.onreconnected((connectionId) => {
    console.assert(connection.state === signalR.HubConnectionState.Connected);

    document.getElementById("messageInput").disabled = false;

    const li = document.createElement("li");
    li.textContent = `Connection reestablished. Connected with connectionId "${connectionId}".`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="c1834-184">`withAutomaticReconnect()` non configurerà l'`HubConnection` per ritentare gli errori iniziali di avvio, quindi gli errori di avvio devono essere gestiti manualmente:</span><span class="sxs-lookup"><span data-stu-id="c1834-184">`withAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="c1834-185">Se il client non si riconnette correttamente entro i primi quattro tentativi, il `HubConnection` eseguirà la transizione allo stato `Disconnected` e attiverà i callback [OnClose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) .</span><span class="sxs-lookup"><span data-stu-id="c1834-185">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire its [onclose](/javascript/api/%40aspnet/signalr/hubconnection#onclose) callbacks.</span></span> <span data-ttu-id="c1834-186">Questo consente di informare gli utenti che la connessione è stata persa definitivamente e si consiglia di aggiornare la pagina:</span><span class="sxs-lookup"><span data-stu-id="c1834-186">This provides an opportunity to inform users the connection has been permanently lost and recommend refreshing the page:</span></span>

```javascript
connection.onclose((error) => {
    console.assert(connection.state === signalR.HubConnectionState.Disconnected);

    document.getElementById("messageInput").disabled = true;

    const li = document.createElement("li");
    li.textContent = `Connection closed due to error "${error}". Try refreshing this page to restart the connection.`;
    document.getElementById("messagesList").appendChild(li);
});
```

<span data-ttu-id="c1834-187">Per configurare un numero personalizzato di tentativi di riconnessione prima di disconnettere o modificare la temporizzazione di riconnessione, `withAutomaticReconnect` accetta una matrice di numeri che rappresenta il ritardo in millisecondi di attesa prima di avviare ogni tentativo di riconnessione.</span><span class="sxs-lookup"><span data-stu-id="c1834-187">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `withAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chatHub")
    .withAutomaticReconnect([0, 0, 10000])
    .build();

    // .withAutomaticReconnect([0, 2000, 10000, 30000]) yields the default behavior
```

<span data-ttu-id="c1834-188">Nell'esempio precedente la `HubConnection` viene configurata per avviare il tentativo di riconnessione immediatamente dopo la perdita della connessione.</span><span class="sxs-lookup"><span data-stu-id="c1834-188">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="c1834-189">Questo vale anche per la configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c1834-189">This is also true for the default configuration.</span></span>

<span data-ttu-id="c1834-190">Se il primo tentativo di riconnessione ha esito negativo, anche il secondo tentativo di riconnessione verrà avviato immediatamente anziché attendere 2 secondi come nella configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c1834-190">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="c1834-191">Se il secondo tentativo di riconnessione ha esito negativo, il terzo tentativo di riconnessione si avvierà entro 10 secondi, che è simile alla configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c1834-191">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="c1834-192">Il comportamento personalizzato, quindi, differisce di nuovo dal comportamento predefinito arrestando il terzo tentativo di riconnessione anziché tentare un ulteriore tentativo di riconnessione in altri 30 secondi, come nella configurazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c1834-192">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure instead of trying one more reconnect attempt in another 30 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="c1834-193">Se si desidera un maggiore controllo sull'intervallo e sul numero di tentativi di riconnessione automatici, `withAutomaticReconnect` accetta un oggetto che implementa l'interfaccia `IRetryPolicy`, che dispone di un solo metodo denominato `nextRetryDelayInMilliseconds`.</span><span class="sxs-lookup"><span data-stu-id="c1834-193">If you want even more control over the timing and number of automatic reconnect attempts, `withAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `nextRetryDelayInMilliseconds`.</span></span>

<span data-ttu-id="c1834-194">`nextRetryDelayInMilliseconds` accetta un solo argomento con il tipo `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="c1834-194">`nextRetryDelayInMilliseconds` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="c1834-195">Il `RetryContext` dispone di tre proprietà: `previousRetryCount`, `elapsedMilliseconds` e `retryReason`, ovvero un `number`, un `number` e un `Error` rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="c1834-195">The `RetryContext` has three properties: `previousRetryCount`, `elapsedMilliseconds` and `retryReason` which are a `number`, a `number` and an `Error` respectively.</span></span> <span data-ttu-id="c1834-196">Prima del primo tentativo di riconnessione, sia `previousRetryCount` che `elapsedMilliseconds` saranno zero e il `retryReason` sarà l'errore che ha causato la perdita della connessione.</span><span class="sxs-lookup"><span data-stu-id="c1834-196">Before the first reconnect attempt, both `previousRetryCount` and `elapsedMilliseconds` will be zero, and the `retryReason` will be the Error that caused the connection to be lost.</span></span> <span data-ttu-id="c1834-197">Dopo ogni nuovo tentativo non riuscito, `previousRetryCount` verrà incrementato di uno, `elapsedMilliseconds` verrà aggiornato per riflettere la quantità di tempo impiegato per la riconnessione fino a questo punto, in millisecondi e il `retryReason` sarà l'errore che ha causato l'ultimo tentativo di riconnessione.</span><span class="sxs-lookup"><span data-stu-id="c1834-197">After each failed retry attempt, `previousRetryCount` will be incremented by one, `elapsedMilliseconds` will be updated to reflect the amount of time spent reconnecting so far in milliseconds, and the `retryReason` will be the Error that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="c1834-198">`nextRetryDelayInMilliseconds` deve restituire un numero che rappresenta il numero di millisecondi di attesa prima del successivo tentativo di riconnessione o `null` se il `HubConnection` deve arrestare la riconnessione.</span><span class="sxs-lookup"><span data-stu-id="c1834-198">`nextRetryDelayInMilliseconds` must return either a number representing the number of milliseconds to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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
        })
    .build();
```

<span data-ttu-id="c1834-199">In alternativa, è possibile scrivere codice per riconnettere manualmente il client, come illustrato in [riconnessione manuale](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="c1834-199">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="c1834-200">Riconnessione manuale</span><span class="sxs-lookup"><span data-stu-id="c1834-200">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="c1834-201">Prima del 3,0, il client JavaScript per SignalR non si riconnette automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c1834-201">Prior to 3.0, the JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="c1834-202">È necessario scrivere codice che riconnetta il client manualmente.</span><span class="sxs-lookup"><span data-stu-id="c1834-202">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="c1834-203">Il codice seguente illustra un tipico approccio di riconnessione manuale:</span><span class="sxs-lookup"><span data-stu-id="c1834-203">The following code demonstrates a typical manual reconnection approach:</span></span>

1. <span data-ttu-id="c1834-204">Viene creata una funzione (in questo caso, la funzione `start`) per avviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="c1834-204">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="c1834-205">Chiamare la funzione `start` nel gestore dell'evento `onclose` della connessione.</span><span class="sxs-lookup"><span data-stu-id="c1834-205">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="c1834-206">Un'implementazione reale utilizzerebbe un backup esponenziale o ritenterà un numero di volte specificato prima di rinunciare.</span><span class="sxs-lookup"><span data-stu-id="c1834-206">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c1834-207">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c1834-207">Additional resources</span></span>

* [<span data-ttu-id="c1834-208">Informazioni di riferimento sulle API JavaScript</span><span class="sxs-lookup"><span data-stu-id="c1834-208">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="c1834-209">Esercitazione su JavaScript</span><span class="sxs-lookup"><span data-stu-id="c1834-209">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="c1834-210">Esercitazione su Webpack e TypeScript</span><span class="sxs-lookup"><span data-stu-id="c1834-210">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="c1834-211">Hub</span><span class="sxs-lookup"><span data-stu-id="c1834-211">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c1834-212">Client .NET</span><span class="sxs-lookup"><span data-stu-id="c1834-212">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c1834-213">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="c1834-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="c1834-214">Richieste tra le origini (CORS)</span><span class="sxs-lookup"><span data-stu-id="c1834-214">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
* <span data-ttu-id="c1834-215">[Documentazione senza server del servizio SignalR di Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="c1834-215">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
