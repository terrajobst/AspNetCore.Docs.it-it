---
title: ASP.NET Core SignalR JavaScript client
author: tdykstra
description: Panoramica di ASP.NET Core SignalR JavaScript client.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/14/2018
uid: signalr/javascript-client
ms.openlocfilehash: 02844c35d1933d36576c25ff335a572fb65eff5c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50208018"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="0cb36-103">ASP.NET Core SignalR JavaScript client</span><span class="sxs-lookup"><span data-stu-id="0cb36-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="0cb36-104">[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="0cb36-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="0cb36-105">La libreria client di ASP.NET Core SignalR JavaScript consente agli sviluppatori di chiamare il codice dell'hub sul lato server.</span><span class="sxs-lookup"><span data-stu-id="0cb36-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="0cb36-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0cb36-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="0cb36-107">Installare il pacchetto client di SignalR</span><span class="sxs-lookup"><span data-stu-id="0cb36-107">Install the SignalR client package</span></span>

<span data-ttu-id="0cb36-108">La libreria client SignalR JavaScript viene fornita come un [npm](https://www.npmjs.com/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="0cb36-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="0cb36-109">Se si usa Visual Studio, eseguire `npm install` dal **Console di gestione pacchetti** mentre si trovano nella cartella radice.</span><span class="sxs-lookup"><span data-stu-id="0cb36-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="0cb36-110">Per Visual Studio Code, eseguire il comando dal **terminale integrato**.</span><span class="sxs-lookup"><span data-stu-id="0cb36-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

<span data-ttu-id="0cb36-111">Monitoraggio prestazioni rete consente di installare il contenuto del pacchetto nel *node_modules\\@aspnet\signalr\dist\browser* cartella.</span><span class="sxs-lookup"><span data-stu-id="0cb36-111">npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="0cb36-112">Creare una nuova cartella denominata *signalr* sotto il *wwwroot\\lib* cartella.</span><span class="sxs-lookup"><span data-stu-id="0cb36-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="0cb36-113">Copia il *signalr.js* del file per il *wwwroot\lib\signalr* cartella.</span><span class="sxs-lookup"><span data-stu-id="0cb36-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="0cb36-114">Usare il client SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="0cb36-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="0cb36-115">Fare riferimento a client SignalR JavaScript il `<script>` elemento.</span><span class="sxs-lookup"><span data-stu-id="0cb36-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="0cb36-116">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="0cb36-116">Connect to a hub</span></span>

<span data-ttu-id="0cb36-117">Il codice seguente crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="0cb36-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="0cb36-118">Nome dell'hub è maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="0cb36-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="0cb36-119">Connessioni cross-origin</span><span class="sxs-lookup"><span data-stu-id="0cb36-119">Cross-origin connections</span></span>

<span data-ttu-id="0cb36-120">In genere, i browser il carico delle connessioni appartenenti allo stesso dominio della pagina richiesta.</span><span class="sxs-lookup"><span data-stu-id="0cb36-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="0cb36-121">Tuttavia, esistono alcuni casi quando è necessaria una connessione a un altro dominio.</span><span class="sxs-lookup"><span data-stu-id="0cb36-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="0cb36-122">Per impedire la lettura dei dati sensibili da un altro sito, di un sito dannoso [le connessioni cross-origin](xref:security/cors) sono disabilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="0cb36-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="0cb36-123">Per consentire a una richiesta multiorigine, abilitarla nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="0cb36-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="0cb36-124">Chiamare i metodi dell'hub da client</span><span class="sxs-lookup"><span data-stu-id="0cb36-124">Call hub methods from client</span></span>

<span data-ttu-id="0cb36-125">Client JavaScript chiamano metodi pubblici in hub tramite il [richiamare](/javascript/api/%40aspnet/signalr/hubconnection#invoke) metodo per il [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span><span class="sxs-lookup"><span data-stu-id="0cb36-125">JavaScript clients call public methods on hubs via the [invoke](/javascript/api/%40aspnet/signalr/hubconnection#invoke) method of the [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection).</span></span> <span data-ttu-id="0cb36-126">Il `invoke` metodo accetta due argomenti:</span><span class="sxs-lookup"><span data-stu-id="0cb36-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="0cb36-127">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="0cb36-127">The name of the hub method.</span></span> <span data-ttu-id="0cb36-128">Nell'esempio seguente, è il nome del metodo dell'hub `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="0cb36-128">In the following example, the method name on the hub is `SendMessage`.</span></span>
* <span data-ttu-id="0cb36-129">Eventuali argomenti definiti nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="0cb36-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="0cb36-130">Nell'esempio seguente, è il nome dell'argomento `message`.</span><span class="sxs-lookup"><span data-stu-id="0cb36-130">In the following example, the argument name is `message`.</span></span>

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="0cb36-131">Chiamare i metodi client hub</span><span class="sxs-lookup"><span data-stu-id="0cb36-131">Call client methods from hub</span></span>

<span data-ttu-id="0cb36-132">Per ricevere messaggi dall'hub, definire un metodo usando il [sul](/javascript/api/%40aspnet/signalr/hubconnection#on) metodo del `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="0cb36-132">To receive messages from the hub, define a method using the [on](/javascript/api/%40aspnet/signalr/hubconnection#on) method of the `HubConnection`.</span></span>

* <span data-ttu-id="0cb36-133">Il nome del metodo client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0cb36-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="0cb36-134">Nell'esempio seguente, è il nome del metodo `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="0cb36-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="0cb36-135">Argomenti che dell'hub viene passata al metodo.</span><span class="sxs-lookup"><span data-stu-id="0cb36-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="0cb36-136">Nell'esempio seguente, il valore dell'argomento è `message`.</span><span class="sxs-lookup"><span data-stu-id="0cb36-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="0cb36-137">Il codice precedente in `connection.on` viene eseguito quando viene chiamato codice lato server usando il [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) (metodo).</span><span class="sxs-lookup"><span data-stu-id="0cb36-137">The preceding code in `connection.on` runs when server-side code calls it using the [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="0cb36-138">SignalR determina quale metodo di client per chiamare creando una corrispondenza tra il nome del metodo e gli argomenti definiti nella `SendAsync` e `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="0cb36-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="0cb36-139">Come procedura consigliata, chiamare il [avviare](/javascript/api/%40aspnet/signalr/hubconnection#start) metodo sulle `HubConnection` dopo `on`.</span><span class="sxs-lookup"><span data-stu-id="0cb36-139">As a best practice, call the [start](/javascript/api/%40aspnet/signalr/hubconnection#start) method on the `HubConnection` after `on`.</span></span> <span data-ttu-id="0cb36-140">In tal modo che i gestori registrati prima di tutti i messaggi vengono ricevuti.</span><span class="sxs-lookup"><span data-stu-id="0cb36-140">Doing so ensures your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="0cb36-141">La registrazione e la gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="0cb36-141">Error handling and logging</span></span>

<span data-ttu-id="0cb36-142">Catena di una `catch` alla fine del metodo di `start` metodo per gestire gli errori lato client.</span><span class="sxs-lookup"><span data-stu-id="0cb36-142">Chain a `catch` method to the end of the `start` method to handle client-side errors.</span></span> <span data-ttu-id="0cb36-143">Usare `console.error` agli errori di output alla console del browser.</span><span class="sxs-lookup"><span data-stu-id="0cb36-143">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="0cb36-144">Analisi di log lato client installazione passando un logger e il tipo di evento per registrare quando viene stabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="0cb36-144">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="0cb36-145">I messaggi vengono registrati con il livello di log specificato e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="0cb36-145">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="0cb36-146">Livelli di log disponibili sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="0cb36-146">Available log levels are as follows:</span></span>

* <span data-ttu-id="0cb36-147">`signalR.LogLevel.Error` &ndash; Messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="0cb36-147">`signalR.LogLevel.Error` &ndash; Error messages.</span></span> <span data-ttu-id="0cb36-148">I log `Error` solo i messaggi.</span><span class="sxs-lookup"><span data-stu-id="0cb36-148">Logs `Error` messages only.</span></span>
* <span data-ttu-id="0cb36-149">`signalR.LogLevel.Warning` &ndash; Messaggi di avviso sui potenziali errori.</span><span class="sxs-lookup"><span data-stu-id="0cb36-149">`signalR.LogLevel.Warning` &ndash; Warning messages about potential errors.</span></span> <span data-ttu-id="0cb36-150">I log `Warning`, e `Error` messaggi.</span><span class="sxs-lookup"><span data-stu-id="0cb36-150">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="0cb36-151">`signalR.LogLevel.Information` &ndash; Messaggi di stato senza errori.</span><span class="sxs-lookup"><span data-stu-id="0cb36-151">`signalR.LogLevel.Information` &ndash; Status messages without errors.</span></span> <span data-ttu-id="0cb36-152">I log `Information`, `Warning`, e `Error` messaggi.</span><span class="sxs-lookup"><span data-stu-id="0cb36-152">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="0cb36-153">`signalR.LogLevel.Trace` &ndash; I messaggi di traccia.</span><span class="sxs-lookup"><span data-stu-id="0cb36-153">`signalR.LogLevel.Trace` &ndash; Trace messages.</span></span> <span data-ttu-id="0cb36-154">Registra tutti gli elementi, inclusi i dati trasportati tra l'hub e client.</span><span class="sxs-lookup"><span data-stu-id="0cb36-154">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="0cb36-155">Usare la [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) metodo sul [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) per configurare il livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="0cb36-155">Use the [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging) method on [HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder) to configure the log level.</span></span> <span data-ttu-id="0cb36-156">I messaggi vengono registrati nella console del browser.</span><span class="sxs-lookup"><span data-stu-id="0cb36-156">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="additional-resources"></a><span data-ttu-id="0cb36-157">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0cb36-157">Additional resources</span></span>

* [<span data-ttu-id="0cb36-158">Informazioni di riferimento sulle API JavaScript</span><span class="sxs-lookup"><span data-stu-id="0cb36-158">JavaScript API reference</span></span>](/javascript/api/?view=signalr-js-latest)
* [<span data-ttu-id="0cb36-159">Hub</span><span class="sxs-lookup"><span data-stu-id="0cb36-159">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="0cb36-160">Client .NET</span><span class="sxs-lookup"><span data-stu-id="0cb36-160">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="0cb36-161">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="0cb36-161">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="0cb36-162">Abilitare le richieste Multiorigine (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0cb36-162">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
