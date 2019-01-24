---
title: ASP.NET Core SignalR JavaScript client
author: bradygaster
description: Panoramica di ASP.NET Core SignalR JavaScript client.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: dd40a64d3c8405e92337ac640ad5cbe913cd849f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54835570"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="47b31-103">ASP.NET Core SignalR JavaScript client</span><span class="sxs-lookup"><span data-stu-id="47b31-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="47b31-104">[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="47b31-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="47b31-105">La libreria client di ASP.NET Core SignalR JavaScript consente agli sviluppatori di chiamare il codice dell'hub sul lato server.</span><span class="sxs-lookup"><span data-stu-id="47b31-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="47b31-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="47b31-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="47b31-107">Installare il pacchetto client di SignalR</span><span class="sxs-lookup"><span data-stu-id="47b31-107">Install the SignalR client package</span></span>

<span data-ttu-id="47b31-108">La libreria client SignalR JavaScript viene fornita come un [npm](https://www.npmjs.com/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="47b31-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="47b31-109">Se si usa Visual Studio, eseguire `npm install` dal **Console di gestione pacchetti** mentre si trovano nella cartella radice.</span><span class="sxs-lookup"><span data-stu-id="47b31-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="47b31-110">Per Visual Studio Code, eseguire il comando dal **terminale integrato**.</span><span class="sxs-lookup"><span data-stu-id="47b31-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="47b31-111">Monitoraggio prestazioni rete consente di installare il contenuto del pacchetto nel *node_modules\\@aspnet\signalr\dist\browser* cartella.</span><span class="sxs-lookup"><span data-stu-id="47b31-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="47b31-112">Creare una nuova cartella denominata *signalr* sotto il *wwwroot\\lib* cartella.</span><span class="sxs-lookup"><span data-stu-id="47b31-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="47b31-113">Copia il *signalr.js* del file per il *wwwroot\lib\signalr* cartella.</span><span class="sxs-lookup"><span data-stu-id="47b31-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="47b31-114">Usare il client SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="47b31-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="47b31-115">Fare riferimento a client SignalR JavaScript il `<script>` elemento.</span><span class="sxs-lookup"><span data-stu-id="47b31-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="47b31-116">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="47b31-116">Connect to a hub</span></span>

<span data-ttu-id="47b31-117">Il codice seguente crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="47b31-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="47b31-118">Nome dell'hub è maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="47b31-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

### <a name="cross-origin-connections"></a><span data-ttu-id="47b31-119">Connessioni cross-origin</span><span class="sxs-lookup"><span data-stu-id="47b31-119">Cross-origin connections</span></span>

<span data-ttu-id="47b31-120">In genere, i browser il carico delle connessioni appartenenti allo stesso dominio della pagina richiesta.</span><span class="sxs-lookup"><span data-stu-id="47b31-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="47b31-121">Tuttavia, esistono alcuni casi quando è necessaria una connessione a un altro dominio.</span><span class="sxs-lookup"><span data-stu-id="47b31-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="47b31-122">Per impedire la lettura dei dati sensibili da un altro sito, di un sito dannoso [le connessioni cross-origin](xref:security/cors) sono disabilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="47b31-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="47b31-123">Per consentire a una richiesta multiorigine, abilitarla nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="47b31-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="47b31-124">Chiamare i metodi dell'hub da client</span><span class="sxs-lookup"><span data-stu-id="47b31-124">Call hub methods from client</span></span>

<span data-ttu-id="47b31-125">Client JavaScript chiamano metodi pubblici in hub tramite il [richiamare](/javascript/api/%40aspnet/signalr/hubconnection#invoke) metodo per il [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="47b31-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="47b31-126">Il `invoke` metodo accetta due argomenti:</span><span class="sxs-lookup"><span data-stu-id="47b31-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="47b31-127">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="47b31-127">The name of the hub method.</span></span> <span data-ttu-id="47b31-128">Nell'esempio seguente, è il nome del metodo dell'hub `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="47b31-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="47b31-129">Eventuali argomenti definiti nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="47b31-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="47b31-130">Nell'esempio seguente, è il nome dell'argomento `message`.</span><span class="sxs-lookup"><span data-stu-id="47b31-130">In the following example, the argument name is `message`.</span></span> <span data-ttu-id="47b31-131">Il codice di esempio Usa sintassi della funzione freccia è supportata nelle versioni correnti di tutti i principali browser, ad eccezione di Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="47b31-131">The example code uses arrow function syntax that is supported in current versions of all major browsers except Internet Explorer.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="47b31-132">Chiamare i metodi client hub</span><span class="sxs-lookup"><span data-stu-id="47b31-132">Call client methods from hub</span></span>

<span data-ttu-id="47b31-133">Per ricevere messaggi dall'hub, definire un metodo usando il [sul](/javascript/api/%40aspnet/signalr/hubconnection#on) metodo del `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="47b31-133">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="47b31-134">Il nome del metodo client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="47b31-134">The name of the JavaScript client method.</span></span> <span data-ttu-id="47b31-135">Nell'esempio seguente, è il nome del metodo `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="47b31-135">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="47b31-136">Argomenti che dell'hub viene passata al metodo.</span><span class="sxs-lookup"><span data-stu-id="47b31-136">Arguments the hub passes to the method.</span></span> <span data-ttu-id="47b31-137">Nell'esempio seguente, il valore dell'argomento è `message`.</span><span class="sxs-lookup"><span data-stu-id="47b31-137">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="47b31-138">Il codice precedente in `connection.on` viene eseguito quando viene chiamato codice lato server usando il [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) (metodo).</span><span class="sxs-lookup"><span data-stu-id="47b31-138">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="47b31-139">SignalR determina quale metodo di client per chiamare creando una corrispondenza tra il nome del metodo e gli argomenti definiti nella `SendAsync` e `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="47b31-139">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="47b31-140">Come procedura consigliata, chiamare il [avviare](/javascript/api/%40aspnet/signalr/hubconnection#start) metodo sulle `HubConnection` dopo `on`.</span><span class="sxs-lookup"><span data-stu-id="47b31-140">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="47b31-141">In tal modo che i gestori registrati prima di tutti i messaggi vengono ricevuti.</span><span class="sxs-lookup"><span data-stu-id="47b31-141">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="47b31-142">La registrazione e la gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="47b31-142">Error handling and logging</span></span>

<span data-ttu-id="47b31-143">Catena di una `catch` alla fine del metodo di `start` metodo per gestire gli errori lato client.</span><span class="sxs-lookup"><span data-stu-id="47b31-143">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="47b31-144">Usare `console.error` agli errori di output alla console del browser.</span><span class="sxs-lookup"><span data-stu-id="47b31-144">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=43-45)]

<span data-ttu-id="47b31-145">Analisi di log lato client installazione passando un logger e il tipo di evento per registrare quando viene stabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="47b31-145">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="47b31-146">I messaggi vengono registrati con il livello di log specificato e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="47b31-146">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="47b31-147">Livelli di log disponibili sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="47b31-147">Available log levels are as follows:</span></span>

* <span data-ttu-id="47b31-148">`signalR.LogLevel.Error` &ndash; Messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="47b31-148">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="47b31-149">I log `Error` solo i messaggi.</span><span class="sxs-lookup"><span data-stu-id="47b31-149">Logs `Error` messages only.</span></span>
* <span data-ttu-id="47b31-150">`signalR.LogLevel.Warning` &ndash; Messaggi di avviso sui potenziali errori.</span><span class="sxs-lookup"><span data-stu-id="47b31-150">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="47b31-151">I log `Warning`, e `Error` messaggi.</span><span class="sxs-lookup"><span data-stu-id="47b31-151">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="47b31-152">`signalR.LogLevel.Information` &ndash; Messaggi di stato senza errori.</span><span class="sxs-lookup"><span data-stu-id="47b31-152">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="47b31-153">I log `Information`, `Warning`, e `Error` messaggi.</span><span class="sxs-lookup"><span data-stu-id="47b31-153">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="47b31-154">`signalR.LogLevel.Trace` &ndash; I messaggi di traccia.</span><span class="sxs-lookup"><span data-stu-id="47b31-154">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="47b31-155">Registra tutti gli elementi, inclusi i dati trasportati tra l'hub e client.</span><span class="sxs-lookup"><span data-stu-id="47b31-155">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="47b31-156">Usare la [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metodo sul [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) per configurare il livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="47b31-156">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="47b31-157">I messaggi vengono registrati nella console del browser.</span><span class="sxs-lookup"><span data-stu-id="47b31-157">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a><span data-ttu-id="47b31-158">Riconnettere i client</span><span class="sxs-lookup"><span data-stu-id="47b31-158">Reconnect clients</span></span>

<span data-ttu-id="47b31-159">Non riconnessione automaticamente il client JavaScript per SignalR.</span><span class="sxs-lookup"><span data-stu-id="47b31-159">The JavaScript client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="47b31-160">È necessario scrivere codice che si riconnetterà manualmente il client.</span><span class="sxs-lookup"><span data-stu-id="47b31-160">You must write code that will reconnect your client manually.</span></span> <span data-ttu-id="47b31-161">Il codice seguente illustra un approccio tipico di riconnessione:</span><span class="sxs-lookup"><span data-stu-id="47b31-161">The following code demonstrates a typical reconnection approach:</span></span>

1. <span data-ttu-id="47b31-162">Una funzione (in questo caso, il `start` (funzione)) viene creato per avviare la connessione.</span><span class="sxs-lookup"><span data-stu-id="47b31-162">A function (in this case, the `start` function) is created to start the connection.</span></span>
1. <span data-ttu-id="47b31-163">Chiamare il `start` funzione della connessione `onclose` gestore dell'evento.</span><span class="sxs-lookup"><span data-stu-id="47b31-163">Call the `start` function in the connection's `onclose` event handler.</span></span>

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

<span data-ttu-id="47b31-164">Un'implementazione reale potrebbe usare un backoff esponenziale o ripetere un numero specificato di volte prima di rinunciare.</span><span class="sxs-lookup"><span data-stu-id="47b31-164">A real-world implementation would use an exponential back-off or retry a specified number of times before giving up.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="47b31-165">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="47b31-165">Additional resources</span></span>

* [<span data-ttu-id="47b31-166">Informazioni di riferimento sulle API JavaScript</span><span class="sxs-lookup"><span data-stu-id="47b31-166">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="47b31-167">Esercitazione di JavaScript</span><span class="sxs-lookup"><span data-stu-id="47b31-167">JavaScript tutorial</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="47b31-168">Esercitazione su WebPack e TypeScript</span><span class="sxs-lookup"><span data-stu-id="47b31-168">WebPack and TypeScript tutorial</span></span>](xref:tutorials/signalr-typescript-webpack)
* [<span data-ttu-id="47b31-169">Hub</span><span class="sxs-lookup"><span data-stu-id="47b31-169">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="47b31-170">Client .NET</span><span class="sxs-lookup"><span data-stu-id="47b31-170">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="47b31-171">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="47b31-171">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="47b31-172">Richieste Multiorigine (CORS)</span><span class="sxs-lookup"><span data-stu-id="47b31-172">Cross-Origin Requests (CORS)</span></span>](xref:security/cors)
