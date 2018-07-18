---
title: ASP.NET Core SignalR JavaScript client
author: tdykstra
description: Panoramica di ASP.NET Core SignalR JavaScript client.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/javascript-client
ms.openlocfilehash: c13c41b0344b0c880e842f2799d6ee97bd7fff7e
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095424"
---
# <a name="aspnet-core-signalr-javascript-client"></a><span data-ttu-id="e38a4-103">ASP.NET Core SignalR JavaScript client</span><span class="sxs-lookup"><span data-stu-id="e38a4-103">ASP.NET Core SignalR JavaScript client</span></span>

<span data-ttu-id="e38a4-104">[Rachel Appel](http://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="e38a4-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="e38a4-105">La libreria client di ASP.NET Core SignalR JavaScript consente agli sviluppatori di chiamare il codice dell'hub sul lato server.</span><span class="sxs-lookup"><span data-stu-id="e38a4-105">The ASP.NET Core SignalR JavaScript client library enables developers to call server-side hub code.</span></span>

<span data-ttu-id="e38a4-106">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e38a4-106">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="install-the-signalr-client-package"></a><span data-ttu-id="e38a4-107">Installare il pacchetto client di SignalR</span><span class="sxs-lookup"><span data-stu-id="e38a4-107">Install the SignalR client package</span></span>

<span data-ttu-id="e38a4-108">La libreria client SignalR JavaScript viene fornita come un [npm](https://www.npmjs.com/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="e38a4-108">The SignalR JavaScript client library is delivered as an [npm](https://www.npmjs.com/) package.</span></span> <span data-ttu-id="e38a4-109">Se si usa Visual Studio, eseguire `npm install` dal **Console di gestione pacchetti** mentre si trovano nella cartella radice.</span><span class="sxs-lookup"><span data-stu-id="e38a4-109">If you're using Visual Studio, run `npm install` from the **Package Manager Console** while in the root folder.</span></span> <span data-ttu-id="e38a4-110">Per Visual Studio Code, eseguire il comando dal **terminale integrato**.</span><span class="sxs-lookup"><span data-stu-id="e38a4-110">For Visual Studio Code, run the command from the **Integrated Terminal**.</span></span>

  ```console
   npm init -y
   npm install @aspnet/signalr
  ```

<span data-ttu-id="e38a4-111">Monitoraggio prestazioni rete consente di installare il contenuto del pacchetto nel *node_modules\\ @aspnet\signalr\dist\browser*  cartella.</span><span class="sxs-lookup"><span data-stu-id="e38a4-111">Npm installs the package contents in the *node_modules\\@aspnet\signalr\dist\browser* folder.</span></span> <span data-ttu-id="e38a4-112">Creare una nuova cartella denominata *signalr* sotto il *wwwroot\\lib* cartella.</span><span class="sxs-lookup"><span data-stu-id="e38a4-112">Create a new folder named *signalr* under the *wwwroot\\lib* folder.</span></span> <span data-ttu-id="e38a4-113">Copia il *signalr.js* del file per il *wwwroot\lib\signalr* cartella.</span><span class="sxs-lookup"><span data-stu-id="e38a4-113">Copy the *signalr.js* file to the *wwwroot\lib\signalr* folder.</span></span>

## <a name="use-the-signalr-javascript-client"></a><span data-ttu-id="e38a4-114">Usare il client SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="e38a4-114">Use the SignalR JavaScript client</span></span>

<span data-ttu-id="e38a4-115">Fare riferimento a client SignalR JavaScript il `<script>` elemento.</span><span class="sxs-lookup"><span data-stu-id="e38a4-115">Reference the SignalR JavaScript client in the `<script>` element.</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="e38a4-116">Connettersi a un hub</span><span class="sxs-lookup"><span data-stu-id="e38a4-116">Connect to a hub</span></span>

<span data-ttu-id="e38a4-117">Il codice seguente crea e avvia una connessione.</span><span class="sxs-lookup"><span data-stu-id="e38a4-117">The following code creates and starts a connection.</span></span> <span data-ttu-id="e38a4-118">Nome dell'hub è maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="e38a4-118">The hub's name is case insensitive.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-12,28)]

### <a name="cross-origin-connections"></a><span data-ttu-id="e38a4-119">Connessioni cross-origin</span><span class="sxs-lookup"><span data-stu-id="e38a4-119">Cross-origin connections</span></span>

<span data-ttu-id="e38a4-120">In genere, i browser il carico delle connessioni appartenenti allo stesso dominio della pagina richiesta.</span><span class="sxs-lookup"><span data-stu-id="e38a4-120">Typically, browsers load connections from the same domain as the requested page.</span></span> <span data-ttu-id="e38a4-121">Tuttavia, esistono alcuni casi quando è necessaria una connessione a un altro dominio.</span><span class="sxs-lookup"><span data-stu-id="e38a4-121">However, there are occasions when a connection to another domain is required.</span></span>

<span data-ttu-id="e38a4-122">Per impedire la lettura dei dati sensibili da un altro sito, di un sito dannoso [le connessioni cross-origin](xref:security/cors) sono disabilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e38a4-122">To prevent a malicious site from reading sensitive data from another site, [cross-origin connections](xref:security/cors) are disabled by default.</span></span> <span data-ttu-id="e38a4-123">Per consentire a una richiesta multiorigine, abilitarla nel `Startup` classe.</span><span class="sxs-lookup"><span data-stu-id="e38a4-123">To allow a cross-origin request, enable it in the `Startup` class.</span></span>

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="e38a4-124">Chiamare i metodi dell'hub da client</span><span class="sxs-lookup"><span data-stu-id="e38a4-124">Call hub methods from client</span></span>

<span data-ttu-id="e38a4-125">Client JavaScript chiamano metodi pubblici in hub da usando `connection.invoke`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-125">JavaScript clients call public methods on hubs by using `connection.invoke`.</span></span> <span data-ttu-id="e38a4-126">Il `invoke` metodo accetta due argomenti:</span><span class="sxs-lookup"><span data-stu-id="e38a4-126">The `invoke` method accepts two arguments:</span></span>

* <span data-ttu-id="e38a4-127">Il nome del metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="e38a4-127">The name of the hub method.</span></span> <span data-ttu-id="e38a4-128">Nell'esempio seguente, è il nome dell'hub `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-128">In the following example, the hub name is `SendMessage`.</span></span>
* <span data-ttu-id="e38a4-129">Eventuali argomenti definiti nel metodo dell'hub.</span><span class="sxs-lookup"><span data-stu-id="e38a4-129">Any arguments defined in the hub method.</span></span> <span data-ttu-id="e38a4-130">Nell'esempio seguente, è il nome dell'argomento `message`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-130">In the following example, the argument name is `message`.</span></span>

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="e38a4-131">Chiamare i metodi client hub</span><span class="sxs-lookup"><span data-stu-id="e38a4-131">Call client methods from hub</span></span>

<span data-ttu-id="e38a4-132">Per ricevere messaggi dall'hub, definire un metodo tramite la `connection.on` (metodo).</span><span class="sxs-lookup"><span data-stu-id="e38a4-132">To receive messages from the hub, define a method using the `connection.on` method.</span></span>

* <span data-ttu-id="e38a4-133">Il nome del metodo client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e38a4-133">The name of the JavaScript client method.</span></span> <span data-ttu-id="e38a4-134">Nell'esempio seguente, è il nome del metodo `ReceiveMessage`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-134">In the following example, the method name is `ReceiveMessage`.</span></span>
* <span data-ttu-id="e38a4-135">Argomenti che dell'hub viene passata al metodo.</span><span class="sxs-lookup"><span data-stu-id="e38a4-135">Arguments the hub passes to the method.</span></span> <span data-ttu-id="e38a4-136">Nell'esempio seguente, il valore dell'argomento è `message`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-136">In the following example, the argument value is `message`.</span></span>

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

<span data-ttu-id="e38a4-137">Il codice precedente in `connection.on` viene eseguito quando viene chiamato codice lato server usando il `SendAsync` (metodo).</span><span class="sxs-lookup"><span data-stu-id="e38a4-137">The preceding code in `connection.on` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

<span data-ttu-id="e38a4-138">SignalR determina quale metodo di client per chiamare creando una corrispondenza tra il nome del metodo e gli argomenti definiti nella `SendAsync` e `connection.on`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-138">SignalR determines which client method to call by matching the method name and arguments defined in `SendAsync` and `connection.on`.</span></span>

> [!NOTE]
> <span data-ttu-id="e38a4-139">Come procedura consigliata, chiamare `connection.start` dopo `connection.on` in modo che i gestori registrati prima di tutti i messaggi vengono ricevuti.</span><span class="sxs-lookup"><span data-stu-id="e38a4-139">As a best practice, call `connection.start` after `connection.on` so your handlers are registered before any messages are received.</span></span>

## <a name="error-handling-and-logging"></a><span data-ttu-id="e38a4-140">La registrazione e la gestione degli errori</span><span class="sxs-lookup"><span data-stu-id="e38a4-140">Error handling and logging</span></span>

<span data-ttu-id="e38a4-141">Catena di una `catch` alla fine del metodo di `connection.start` metodo per gestire gli errori lato client.</span><span class="sxs-lookup"><span data-stu-id="e38a4-141">Chain a `catch` method to the end of the `connection.start` method to handle client-side errors.</span></span> <span data-ttu-id="e38a4-142">Usare `console.error` agli errori di output alla console del browser.</span><span class="sxs-lookup"><span data-stu-id="e38a4-142">Use `console.error` to output errors to the browser's console.</span></span>

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=28)]

<span data-ttu-id="e38a4-143">Analisi di log lato client installazione passando un logger e il tipo di evento per registrare quando viene stabilita la connessione.</span><span class="sxs-lookup"><span data-stu-id="e38a4-143">Setup client-side log tracing by passing a logger and type of event to log when the connection is made.</span></span> <span data-ttu-id="e38a4-144">I messaggi vengono registrati con il livello di log specificato e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="e38a4-144">Messages are logged with the specified log level and higher.</span></span> <span data-ttu-id="e38a4-145">Livelli di log disponibili sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="e38a4-145">Available log levels are as follows:</span></span>

* <span data-ttu-id="e38a4-146">`signalR.LogLevel.Error` : Messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="e38a4-146">`signalR.LogLevel.Error` : Error messages.</span></span> <span data-ttu-id="e38a4-147">I log `Error` solo i messaggi.</span><span class="sxs-lookup"><span data-stu-id="e38a4-147">Logs `Error` messages only.</span></span>
* <span data-ttu-id="e38a4-148">`signalR.LogLevel.Warning` : Messaggi di avviso sui potenziali errori.</span><span class="sxs-lookup"><span data-stu-id="e38a4-148">`signalR.LogLevel.Warning` : Warning messages about potential errors.</span></span> <span data-ttu-id="e38a4-149">I log `Warning`, e `Error` messaggi.</span><span class="sxs-lookup"><span data-stu-id="e38a4-149">Logs `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="e38a4-150">`signalR.LogLevel.Information` : Messaggi di stato senza errori.</span><span class="sxs-lookup"><span data-stu-id="e38a4-150">`signalR.LogLevel.Information` : Status messages without errors.</span></span> <span data-ttu-id="e38a4-151">I log `Information`, `Warning`, e `Error` messaggi.</span><span class="sxs-lookup"><span data-stu-id="e38a4-151">Logs `Information`, `Warning`, and `Error` messages.</span></span>
* <span data-ttu-id="e38a4-152">`signalR.LogLevel.Trace` : I messaggi di traccia.</span><span class="sxs-lookup"><span data-stu-id="e38a4-152">`signalR.LogLevel.Trace` : Trace messages.</span></span> <span data-ttu-id="e38a4-153">Registra tutti gli elementi, inclusi i dati trasportati tra l'hub e client.</span><span class="sxs-lookup"><span data-stu-id="e38a4-153">Logs everything, including data transported between hub and client.</span></span>

<span data-ttu-id="e38a4-154">Usare la `configureLogging` metodo `HubConnectionBuilder` per configurare il livello di registrazione.</span><span class="sxs-lookup"><span data-stu-id="e38a4-154">Use the `configureLogging` method on `HubConnectionBuilder` to configure the log level.</span></span> <span data-ttu-id="e38a4-155">I messaggi vengono registrati nella console del browser.</span><span class="sxs-lookup"><span data-stu-id="e38a4-155">Messages are logged to the browser console.</span></span>

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="related-resources"></a><span data-ttu-id="e38a4-156">Risorse correlate</span><span class="sxs-lookup"><span data-stu-id="e38a4-156">Related resources</span></span>

* [<span data-ttu-id="e38a4-157">Hub</span><span class="sxs-lookup"><span data-stu-id="e38a4-157">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="e38a4-158">Client .NET</span><span class="sxs-lookup"><span data-stu-id="e38a4-158">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="e38a4-159">Pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="e38a4-159">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="e38a4-160">Abilitare le richieste Multiorigine (CORS) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e38a4-160">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>](xref:security/cors)
