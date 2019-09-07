---
title: Modelli di hosting di ASP.NET Core Blazer
author: guardrex
description: Informazioni sui modelli di hosting lato client e lato server di Blazer.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: blazor/hosting-models
ms.openlocfilehash: f7a16d64e1f874a4f6b3c8db5217810b13c7c6ff
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800434"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="ffb6c-103">Modelli di hosting di ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="ffb6c-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="ffb6c-104">Di [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ffb6c-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="ffb6c-105">Blazer è un framework web progettato per l'esecuzione sul lato client nel browser in un Runtime .NET basato su [webassembly](https://webassembly.org/) (*lato client Blazer*) o sul lato server in ASP.NET Core (*lato server Blaze*).</span><span class="sxs-lookup"><span data-stu-id="ffb6c-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="ffb6c-106">Indipendentemente dal modello di hosting, i modelli di app e componenti *sono gli stessi*.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="ffb6c-107">Per creare un progetto per i modelli di hosting descritti in questo articolo, <xref:blazor/get-started>vedere.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="ffb6c-108">Lato client</span><span class="sxs-lookup"><span data-stu-id="ffb6c-108">Client-side</span></span>

<span data-ttu-id="ffb6c-109">Il modello di hosting principale per blazer è in esecuzione sul lato client nel browser del webassembly.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="ffb6c-110">L'app Blazor, le relative dipendenze e il runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="ffb6c-111">L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="ffb6c-112">Gli aggiornamenti dell'interfaccia utente e la gestione degli eventi si verificano nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="ffb6c-113">Le risorse dell'app vengono distribuite come file statici in un server Web o in un servizio in grado di servire contenuto statico ai client.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Lato client Blazer: L'app Blazer viene eseguita su un thread dell'interfaccia utente all'interno del browser.](hosting-models/_static/client-side.png)

<span data-ttu-id="ffb6c-115">Per creare un'app Blazer usando il modello di hosting lato client, usare il modello di **app Webassembly Blazer** ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="ffb6c-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="ffb6c-116">Dopo aver selezionato il modello di **applicazione Webassembly Blazer** , è possibile configurare l'app per l'uso di un back-end ASP.NET Core selezionando la casella di controllo **ASP.NET Core Hosted** ([DotNet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="ffb6c-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="ffb6c-117">L'app ASP.NET Core serve l'app Blazer ai client.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="ffb6c-118">L'app del lato client blazer può interagire con il server tramite la rete usando chiamate API Web o [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="ffb6c-118">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="ffb6c-119">I modelli includono lo script *blazer. webassembly. js* che gestisce:</span><span class="sxs-lookup"><span data-stu-id="ffb6c-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="ffb6c-120">Download del runtime .NET, dell'app e delle dipendenze dell'app.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="ffb6c-121">Inizializzazione del runtime per l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="ffb6c-122">Il modello di hosting lato client offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="ffb6c-122">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="ffb6c-123">Non esiste alcuna dipendenza lato server .NET.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="ffb6c-124">L'app funziona completamente dopo il download nel client.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="ffb6c-125">Le risorse e le funzionalità client sono completamente sfruttate.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="ffb6c-126">Il lavoro viene scaricato dal server al client.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="ffb6c-127">Non è necessario un server Web ASP.NET Core per ospitare l'app.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="ffb6c-128">Gli scenari di distribuzione senza server sono possibili, ad esempio per servire l'app da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="ffb6c-129">Ci sono svantaggi per l'hosting lato client:</span><span class="sxs-lookup"><span data-stu-id="ffb6c-129">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="ffb6c-130">L'app è limitata alle funzionalità del browser.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="ffb6c-131">È necessario disporre di hardware e software client idonei (ad esempio, il supporto di webassembly).</span><span class="sxs-lookup"><span data-stu-id="ffb6c-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="ffb6c-132">Le dimensioni del download sono maggiori e le app importano più tempo per il caricamento.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="ffb6c-133">Il supporto di runtime e strumenti .NET è meno maturo.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="ffb6c-134">Ad esempio, esistono limitazioni in [.NET standard](/dotnet/standard/net-standard) supporto e debug.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="ffb6c-135">Lato server</span><span class="sxs-lookup"><span data-stu-id="ffb6c-135">Server-side</span></span>

<span data-ttu-id="ffb6c-136">Con il modello di hosting lato server, l'app viene eseguita nel server dall'interno di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-136">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="ffb6c-137">Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="ffb6c-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) sul server tramite una connessione SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="ffb6c-139">Per creare un'app Blazer usando il modello di hosting lato server, usare il modello di **app del server ASP.NET Core Blazer** ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="ffb6c-139">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="ffb6c-140">L'app ASP.NET Core ospita l'app sul lato server e crea l'endpoint SignalR a cui si connettono i client.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-140">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="ffb6c-141">L'app ASP.NET Core fa riferimento alla `Startup` classe dell'app per aggiungere:</span><span class="sxs-lookup"><span data-stu-id="ffb6c-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="ffb6c-142">Servizi lato server.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-142">Server-side services.</span></span>
* <span data-ttu-id="ffb6c-143">App per la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="ffb6c-144">Lo script&dagger; *blazer. Server. js* stabilisce la connessione client.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="ffb6c-145">È responsabilità dell'app salvare in modo permanente e ripristinare lo stato dell'app come richiesto, ad esempio in caso di perdita di una connessione di rete.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="ffb6c-146">Il modello di hosting lato server offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="ffb6c-146">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="ffb6c-147">Le dimensioni del download sono notevolmente inferiori rispetto a un'app sul lato client e l'app viene caricata molto più velocemente.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-147">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="ffb6c-148">L'app sfrutta appieno le funzionalità del server, incluso l'uso di tutte le API compatibili con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="ffb6c-149">.NET Core nel server viene usato per eseguire l'app, in modo che gli strumenti .NET esistenti, ad esempio il debug, funzionino come previsto.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="ffb6c-150">Sono supportati i thin client.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-150">Thin clients are supported.</span></span> <span data-ttu-id="ffb6c-151">Ad esempio, le app sul lato server funzionano con i browser che non supportano webassembly e nei dispositivi con vincoli di risorse.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-151">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="ffb6c-152">La codebase .NET/C# code dell'app, incluso il codice componente dell'app, non viene servita ai client.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="ffb6c-153">Ci sono svantaggi per l'hosting lato server:</span><span class="sxs-lookup"><span data-stu-id="ffb6c-153">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="ffb6c-154">In genere esiste una latenza più elevata.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-154">Higher latency usually exists.</span></span> <span data-ttu-id="ffb6c-155">Ogni interazione dell'utente comporta un hop di rete.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="ffb6c-156">Non è disponibile alcun supporto offline.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-156">There's no offline support.</span></span> <span data-ttu-id="ffb6c-157">Se la connessione client non riesce, l'app smette di funzionare.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="ffb6c-158">La scalabilità è complessa per le app con molti utenti.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="ffb6c-159">Il server deve gestire più connessioni client e gestire lo stato del client.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="ffb6c-160">Per gestire l'app, è necessario un server ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="ffb6c-161">Gli scenari di distribuzione senza server non sono possibili, ad esempio per servire l'app da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="ffb6c-162">&dagger;Lo script *blazer. Server. js* viene servito da una risorsa incorporata nel framework condiviso ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="ffb6c-163">Riconnessione allo stesso server</span><span class="sxs-lookup"><span data-stu-id="ffb6c-163">Reconnection to the same server</span></span>

<span data-ttu-id="ffb6c-164">Le app sul lato server di Blazer richiedono una connessione SignalR attiva al server.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-164">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="ffb6c-165">Se la connessione viene persa, l'app tenta di riconnettersi al server.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-165">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="ffb6c-166">Fino a quando lo stato del client è ancora in memoria, la sessione client riprende senza perdere lo stato.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-166">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="ffb6c-167">Quando il client rileva che la connessione è stata persa, viene visualizzata un'interfaccia utente predefinita quando il client tenta di riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-167">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="ffb6c-168">Se la riconnessione non riesce, all'utente viene offerta l'opzione per riprovare.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-168">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="ffb6c-169">Per personalizzare l'interfaccia utente, definire un elemento `components-reconnect-modal` con `id` come nella pagina Razor *_Host. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ffb6c-169">To customize the UI, define an element with `components-reconnect-modal` as its `id` in the *_Host.cshtml* Razor page.</span></span> <span data-ttu-id="ffb6c-170">Il client aggiorna questo elemento con una delle seguenti classi CSS in base allo stato della connessione:</span><span class="sxs-lookup"><span data-stu-id="ffb6c-170">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="ffb6c-171">`components-reconnect-show`&ndash; Mostra l'interfaccia utente per indicare che la connessione è stata persa e il client sta tentando di riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-171">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="ffb6c-172">`components-reconnect-hide`&ndash; Il client ha una connessione attiva e nasconde l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-172">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="ffb6c-173">`components-reconnect-failed`&ndash; Riconnessione non riuscita.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-173">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="ffb6c-174">Per ritentare la riconnessione `window.Blazor.reconnect()`, chiamare.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-174">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="ffb6c-175">Riconnessione con stato dopo il rendering preliminare</span><span class="sxs-lookup"><span data-stu-id="ffb6c-175">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="ffb6c-176">Per impostazione predefinita, le app lato server di Blaze sono impostate per eseguire il prerendering dell'interfaccia utente nel server prima che venga stabilita la connessione client al server.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-176">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="ffb6c-177">Questa impostazione è configurata nella pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="ffb6c-177">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="ffb6c-178">`RenderMode`Configura se il componente:</span><span class="sxs-lookup"><span data-stu-id="ffb6c-178">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="ffb6c-179">Viene preeseguito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-179">Is prerendered into the page.</span></span>
* <span data-ttu-id="ffb6c-180">Viene visualizzato come HTML statico nella pagina o se include le informazioni necessarie per eseguire il bootstrap di un'app Blazer dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-180">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="ffb6c-181">Descrizione</span><span class="sxs-lookup"><span data-stu-id="ffb6c-181">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="ffb6c-182">Esegue il rendering del componente in HTML statico e include un marcatore per un'app Blazer sul lato server.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-182">Renders the component into static HTML and includes a marker for a Blazor server-side app.</span></span> <span data-ttu-id="ffb6c-183">Quando l'agente utente viene avviato, questo marcatore viene usato per il bootstrap di un'app blazer.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-183">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="ffb6c-184">I parametri non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-184">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="ffb6c-185">Esegue il rendering di un marcatore per un'app del lato server Blaze.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-185">Renders a marker for a Blazor server-side app.</span></span> <span data-ttu-id="ffb6c-186">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-186">Output from the component isn't included.</span></span> <span data-ttu-id="ffb6c-187">Quando l'agente utente viene avviato, questo marcatore viene usato per il bootstrap di un'app blazer.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-187">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="ffb6c-188">I parametri non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-188">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="ffb6c-189">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-189">Renders the component into static HTML.</span></span> <span data-ttu-id="ffb6c-190">I parametri sono supportati.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-190">Parameters are supported.</span></span> |

<span data-ttu-id="ffb6c-191">Il rendering dei componenti server da una pagina HTML statica non è supportato.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-191">Rendering server components from a static HTML page isn't supported.</span></span>
 
<span data-ttu-id="ffb6c-192">Il client si riconnette al server con lo stesso stato usato per eseguire il prerendering dell'app.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-192">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="ffb6c-193">Se lo stato dell'app è ancora in memoria, lo stato del componente non viene sottoposto a rendering dopo che è stata stabilita la connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-193">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="ffb6c-194">Eseguire il rendering di componenti interattivi con stato da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="ffb6c-194">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="ffb6c-195">I componenti interattivi con stato possono essere aggiunti a una pagina o a una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-195">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="ffb6c-196">Quando viene eseguito il rendering della pagina o della visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="ffb6c-196">When the page or view renders:</span></span>

* <span data-ttu-id="ffb6c-197">Il componente viene preeseguito con la pagina o la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-197">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="ffb6c-198">Lo stato iniziale del componente utilizzato per il prerendering viene perso.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-198">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="ffb6c-199">Quando viene stabilita la connessione SignalR, viene creato il nuovo stato del componente.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-199">New component state is created when the SignalR connection is established.</span></span>
 
<span data-ttu-id="ffb6c-200">La pagina Razor seguente esegue il rendering `Counter` di un componente:</span><span class="sxs-lookup"><span data-stu-id="ffb6c-200">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>
 
@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="ffb6c-201">Eseguire il rendering di componenti non interattivi da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="ffb6c-201">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="ffb6c-202">Nella pagina Razor seguente il `MyComponent` componente viene sottoposto a rendering statico con un valore iniziale specificato usando un form:</span><span class="sxs-lookup"><span data-stu-id="ffb6c-202">In the following Razor page, the `MyComponent` component is statically rendered with an initial value that's specified using a form:</span></span>
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>
 
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.Static, 
    new { InitialValue = InitialValue }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

<span data-ttu-id="ffb6c-203">Poiché `MyComponent` viene sottoposto a rendering statico, il componente non può essere interattivo.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-203">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="ffb6c-204">Rilevare il momento in cui viene eseguito il prerendering dell'app</span><span class="sxs-lookup"><span data-stu-id="ffb6c-204">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="ffb6c-205">Configurare il client SignalR per le app del lato server di Blaze</span><span class="sxs-lookup"><span data-stu-id="ffb6c-205">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="ffb6c-206">In alcuni casi, è necessario configurare il client SignalR usato dalle app del lato server di Blazer.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-206">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="ffb6c-207">Ad esempio, potrebbe essere necessario configurare la registrazione nel client SignalR per diagnosticare un problema di connessione.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-207">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="ffb6c-208">Per configurare il client SignalR nel file *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="ffb6c-208">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="ffb6c-209">Aggiungere un `autostart="false"` attributo `<script>` al tag per lo script *blazer. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="ffb6c-209">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="ffb6c-210">Chiamare `Blazor.start` e passare un oggetto di configurazione che specifichi il generatore SignalR.</span><span class="sxs-lookup"><span data-stu-id="ffb6c-210">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging(2); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a><span data-ttu-id="ffb6c-211">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ffb6c-211">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
