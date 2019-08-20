---
title: Modelli di hosting di ASP.NET Core Blazer
author: guardrex
description: Informazioni sui modelli di hosting lato client e lato server di Blazer.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/hosting-models
ms.openlocfilehash: 64393e826cb17550085f468f5916fca55973908f
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993394"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="e2fef-103">Modelli di hosting di ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="e2fef-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="e2fef-104">Di [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="e2fef-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="e2fef-105">Blazer è un framework web progettato per l'esecuzione sul lato client nel browser in un Runtime .NET basato su [webassembly](https://webassembly.org/) (*lato client Blazer*) o sul lato server in ASP.NET Core (*lato server Blaze*).</span><span class="sxs-lookup"><span data-stu-id="e2fef-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor client-side*) or server-side in ASP.NET Core (*Blazor server-side*).</span></span> <span data-ttu-id="e2fef-106">Indipendentemente dal modello di hosting, i modelli di app e componenti *sono gli stessi*.</span><span class="sxs-lookup"><span data-stu-id="e2fef-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="e2fef-107">Per creare un progetto per i modelli di hosting descritti in questo articolo, <xref:blazor/get-started>vedere.</span><span class="sxs-lookup"><span data-stu-id="e2fef-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="client-side"></a><span data-ttu-id="e2fef-108">Lato client</span><span class="sxs-lookup"><span data-stu-id="e2fef-108">Client-side</span></span>

<span data-ttu-id="e2fef-109">Il modello di hosting principale per blazer è in esecuzione sul lato client nel browser del webassembly.</span><span class="sxs-lookup"><span data-stu-id="e2fef-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="e2fef-110">L'app Blazor, le relative dipendenze e il runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="e2fef-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="e2fef-111">L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser.</span><span class="sxs-lookup"><span data-stu-id="e2fef-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="e2fef-112">Gli aggiornamenti dell'interfaccia utente e la gestione degli eventi si verificano nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="e2fef-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="e2fef-113">Le risorse dell'app vengono distribuite come file statici in un server Web o in un servizio in grado di servire contenuto statico ai client.</span><span class="sxs-lookup"><span data-stu-id="e2fef-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Lato client Blazer: L'app Blazer viene eseguita su un thread dell'interfaccia utente all'interno del browser.](hosting-models/_static/client-side.png)

<span data-ttu-id="e2fef-115">Per creare un'app Blazer usando il modello di hosting lato client, usare il modello di **app Webassembly Blazer** ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="e2fef-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="e2fef-116">Dopo aver selezionato il modello di **applicazione Webassembly Blazer** , è possibile configurare l'app per l'uso di un back-end ASP.NET Core selezionando la casella di controllo **ASP.NET Core Hosted** ([DotNet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="e2fef-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="e2fef-117">L'app ASP.NET Core serve l'app Blazer ai client.</span><span class="sxs-lookup"><span data-stu-id="e2fef-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="e2fef-118">L'app del lato client blazer può interagire con il server tramite la rete usando chiamate API Web o [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="e2fef-118">The Blazor client-side app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="e2fef-119">I modelli includono lo script *blazer. webassembly. js* che gestisce:</span><span class="sxs-lookup"><span data-stu-id="e2fef-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="e2fef-120">Download del runtime .NET, dell'app e delle dipendenze dell'app.</span><span class="sxs-lookup"><span data-stu-id="e2fef-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="e2fef-121">Inizializzazione del runtime per l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="e2fef-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="e2fef-122">Il modello di hosting lato client offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="e2fef-122">The client-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="e2fef-123">Non esiste alcuna dipendenza lato server .NET.</span><span class="sxs-lookup"><span data-stu-id="e2fef-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="e2fef-124">L'app funziona completamente dopo il download nel client.</span><span class="sxs-lookup"><span data-stu-id="e2fef-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="e2fef-125">Le risorse e le funzionalità client sono completamente sfruttate.</span><span class="sxs-lookup"><span data-stu-id="e2fef-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="e2fef-126">Il lavoro viene scaricato dal server al client.</span><span class="sxs-lookup"><span data-stu-id="e2fef-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="e2fef-127">Non è necessario un server Web ASP.NET Core per ospitare l'app.</span><span class="sxs-lookup"><span data-stu-id="e2fef-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="e2fef-128">Gli scenari di distribuzione senza server sono possibili, ad esempio per servire l'app da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="e2fef-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="e2fef-129">Ci sono svantaggi per l'hosting lato client:</span><span class="sxs-lookup"><span data-stu-id="e2fef-129">There are downsides to client-side hosting:</span></span>

* <span data-ttu-id="e2fef-130">L'app è limitata alle funzionalità del browser.</span><span class="sxs-lookup"><span data-stu-id="e2fef-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="e2fef-131">È necessario disporre di hardware e software client idonei (ad esempio, il supporto di webassembly).</span><span class="sxs-lookup"><span data-stu-id="e2fef-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="e2fef-132">Le dimensioni del download sono maggiori e le app importano più tempo per il caricamento.</span><span class="sxs-lookup"><span data-stu-id="e2fef-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="e2fef-133">Il supporto di runtime e strumenti .NET è meno maturo.</span><span class="sxs-lookup"><span data-stu-id="e2fef-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="e2fef-134">Ad esempio, esistono limitazioni in [.NET standard](/dotnet/standard/net-standard) supporto e debug.</span><span class="sxs-lookup"><span data-stu-id="e2fef-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="server-side"></a><span data-ttu-id="e2fef-135">Lato server</span><span class="sxs-lookup"><span data-stu-id="e2fef-135">Server-side</span></span>

<span data-ttu-id="e2fef-136">Con il modello di hosting lato server, l'app viene eseguita nel server dall'interno di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2fef-136">With the server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="e2fef-137">Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="e2fef-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) sul server tramite una connessione SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="e2fef-139">Per creare un'app Blazer usando il modello di hosting lato server, usare il modello di **app del server** ASP.NET Core blazer ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="e2fef-139">To create a Blazor app using the server-side hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="e2fef-140">L'app ASP.NET Core ospita l'app sul lato server e crea l'endpoint SignalR a cui si connettono i client.</span><span class="sxs-lookup"><span data-stu-id="e2fef-140">The ASP.NET Core app hosts the server-side app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="e2fef-141">L'app ASP.NET Core fa riferimento alla `Startup` classe dell'app per aggiungere:</span><span class="sxs-lookup"><span data-stu-id="e2fef-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="e2fef-142">Servizi lato server.</span><span class="sxs-lookup"><span data-stu-id="e2fef-142">Server-side services.</span></span>
* <span data-ttu-id="e2fef-143">App per la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="e2fef-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="e2fef-144">Lo script&dagger; *blazer. Server. js* stabilisce la connessione client.</span><span class="sxs-lookup"><span data-stu-id="e2fef-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="e2fef-145">È responsabilità dell'app salvare in modo permanente e ripristinare lo stato dell'app come richiesto, ad esempio in caso di perdita di una connessione di rete.</span><span class="sxs-lookup"><span data-stu-id="e2fef-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="e2fef-146">Il modello di hosting lato server offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="e2fef-146">The server-side hosting model offers several benefits:</span></span>

* <span data-ttu-id="e2fef-147">Le dimensioni del download sono notevolmente inferiori rispetto a un'app sul lato client e l'app viene caricata molto più velocemente.</span><span class="sxs-lookup"><span data-stu-id="e2fef-147">Download size is significantly smaller than a client-side app, and the app loads much faster.</span></span>
* <span data-ttu-id="e2fef-148">L'app sfrutta appieno le funzionalità del server, incluso l'uso di tutte le API compatibili con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2fef-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="e2fef-149">.NET Core nel server viene usato per eseguire l'app, in modo che gli strumenti .NET esistenti, ad esempio il debug, funzionino come previsto.</span><span class="sxs-lookup"><span data-stu-id="e2fef-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="e2fef-150">Sono supportati i thin client.</span><span class="sxs-lookup"><span data-stu-id="e2fef-150">Thin clients are supported.</span></span> <span data-ttu-id="e2fef-151">Ad esempio, le app sul lato server funzionano con i browser che non supportano webassembly e nei dispositivi con vincoli di risorse.</span><span class="sxs-lookup"><span data-stu-id="e2fef-151">For example, server-side apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="e2fef-152">La codebase .NET/C# code dell'app, incluso il codice componente dell'app, non viene servita ai client.</span><span class="sxs-lookup"><span data-stu-id="e2fef-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="e2fef-153">Ci sono svantaggi per l'hosting lato server:</span><span class="sxs-lookup"><span data-stu-id="e2fef-153">There are downsides to server-side hosting:</span></span>

* <span data-ttu-id="e2fef-154">In genere esiste una latenza più elevata.</span><span class="sxs-lookup"><span data-stu-id="e2fef-154">Higher latency usually exists.</span></span> <span data-ttu-id="e2fef-155">Ogni interazione dell'utente comporta un hop di rete.</span><span class="sxs-lookup"><span data-stu-id="e2fef-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="e2fef-156">Non è disponibile alcun supporto offline.</span><span class="sxs-lookup"><span data-stu-id="e2fef-156">There's no offline support.</span></span> <span data-ttu-id="e2fef-157">Se la connessione client non riesce, l'app smette di funzionare.</span><span class="sxs-lookup"><span data-stu-id="e2fef-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="e2fef-158">La scalabilità è complessa per le app con molti utenti.</span><span class="sxs-lookup"><span data-stu-id="e2fef-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="e2fef-159">Il server deve gestire più connessioni client e gestire lo stato del client.</span><span class="sxs-lookup"><span data-stu-id="e2fef-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="e2fef-160">Per gestire l'app, è necessario un server ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2fef-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="e2fef-161">Gli scenari di distribuzione senza server non sono possibili, ad esempio per servire l'app da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="e2fef-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="e2fef-162">&dagger;Lo script *blazer. Server. js* viene servito da una risorsa incorporata nel framework condiviso ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e2fef-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="e2fef-163">Riconnessione allo stesso server</span><span class="sxs-lookup"><span data-stu-id="e2fef-163">Reconnection to the same server</span></span>

<span data-ttu-id="e2fef-164">Le app sul lato server di Blazer richiedono una connessione SignalR attiva al server.</span><span class="sxs-lookup"><span data-stu-id="e2fef-164">Blazor server-side apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="e2fef-165">Se la connessione viene persa, l'app tenta di riconnettersi al server.</span><span class="sxs-lookup"><span data-stu-id="e2fef-165">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="e2fef-166">Fino a quando lo stato del client è ancora in memoria, la sessione client riprende senza perdere lo stato.</span><span class="sxs-lookup"><span data-stu-id="e2fef-166">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>
 
<span data-ttu-id="e2fef-167">Quando il client rileva che la connessione è stata persa, viene visualizzata un'interfaccia utente predefinita quando il client tenta di riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="e2fef-167">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="e2fef-168">Se la riconnessione non riesce, all'utente viene offerta l'opzione per riprovare.</span><span class="sxs-lookup"><span data-stu-id="e2fef-168">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="e2fef-169">Per personalizzare l'interfaccia utente, definire un elemento `components-reconnect-modal` con `id` come nella pagina Razor *_Host. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="e2fef-169">To customize the UI, define an element with `components-reconnect-modal` as its `id` in the *_Host.cshtml* Razor page.</span></span> <span data-ttu-id="e2fef-170">Il client aggiorna questo elemento con una delle seguenti classi CSS in base allo stato della connessione:</span><span class="sxs-lookup"><span data-stu-id="e2fef-170">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>
 
* <span data-ttu-id="e2fef-171">`components-reconnect-show`&ndash; Mostra l'interfaccia utente per indicare che la connessione è stata persa e il client sta tentando di riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="e2fef-171">`components-reconnect-show` &ndash; Show the UI to indicate the connection was lost and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="e2fef-172">`components-reconnect-hide`&ndash; Il client ha una connessione attiva e nasconde l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="e2fef-172">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="e2fef-173">`components-reconnect-failed`&ndash; Riconnessione non riuscita.</span><span class="sxs-lookup"><span data-stu-id="e2fef-173">`components-reconnect-failed` &ndash; Reconnection failed.</span></span> <span data-ttu-id="e2fef-174">Per ritentare la riconnessione `window.Blazor.reconnect()`, chiamare.</span><span class="sxs-lookup"><span data-stu-id="e2fef-174">To attempt reconnection again, call `window.Blazor.reconnect()`.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="e2fef-175">Riconnessione con stato dopo il rendering preliminare</span><span class="sxs-lookup"><span data-stu-id="e2fef-175">Stateful reconnection after prerendering</span></span>
 
<span data-ttu-id="e2fef-176">Per impostazione predefinita, le app lato server di Blaze sono impostate per eseguire il prerendering dell'interfaccia utente nel server prima che venga stabilita la connessione client al server.</span><span class="sxs-lookup"><span data-stu-id="e2fef-176">Blazor server-side apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="e2fef-177">Questa impostazione è configurata nella pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e2fef-177">This is set up in the *_Host.cshtml* Razor page:</span></span>
 
```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>())</app>
 
    <script src="_framework/blazor.server.js"></script>
</body>
```
 
<span data-ttu-id="e2fef-178">Il client si riconnette al server con lo stesso stato usato per eseguire il prerendering dell'app.</span><span class="sxs-lookup"><span data-stu-id="e2fef-178">The client reconnects to the server with the same state that was used to prerender the app.</span></span> <span data-ttu-id="e2fef-179">Se lo stato dell'app è ancora in memoria, lo stato del componente non viene sottoposto a rendering dopo che è stata stabilita la connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="e2fef-179">If the app's state is still in memory, the component state isn't rerendered after the SignalR connection is established.</span></span>

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="e2fef-180">Eseguire il rendering di componenti interattivi con stato da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="e2fef-180">Render stateful interactive components from Razor pages and views</span></span>
 
<span data-ttu-id="e2fef-181">I componenti interattivi con stato possono essere aggiunti a una pagina o a una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="e2fef-181">Stateful interactive components can be added to a Razor page or view.</span></span> <span data-ttu-id="e2fef-182">Quando viene eseguito il rendering della pagina o della visualizzazione, il componente viene preeseguito.</span><span class="sxs-lookup"><span data-stu-id="e2fef-182">When the page or view renders, the component is prerendered with it.</span></span> <span data-ttu-id="e2fef-183">L'app si riconnette quindi allo stato del componente dopo che la connessione client viene stabilita finché lo stato è ancora in memoria.</span><span class="sxs-lookup"><span data-stu-id="e2fef-183">The app then reconnects to the component state once the client connection is established as long as the state is still in memory.</span></span>
 
<span data-ttu-id="e2fef-184">La pagina Razor seguente, ad esempio, esegue il `Counter` rendering di un componente con un conteggio iniziale specificato utilizzando un form:</span><span class="sxs-lookup"><span data-stu-id="e2fef-184">For example, the following Razor page renders a `Counter` component with an initial count that's specified using a form:</span></span>
 
```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialCount" />
    <button type="submit">Set initial count</button>
</form>
 
@(await Html.RenderComponentAsync<Counter>(new { InitialCount = InitialCount }))
 
@code {
    [BindProperty(SupportsGet=true)]
    public int InitialCount { get; set; }
}
```

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="e2fef-185">Rilevare il momento in cui viene eseguito il prerendering dell'app</span><span class="sxs-lookup"><span data-stu-id="e2fef-185">Detect when the app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-side-apps"></a><span data-ttu-id="e2fef-186">Configurare il client SignalR per le app del lato server di Blaze</span><span class="sxs-lookup"><span data-stu-id="e2fef-186">Configure the SignalR client for Blazor server-side apps</span></span>
 
<span data-ttu-id="e2fef-187">In alcuni casi, è necessario configurare il client SignalR usato dalle app del lato server di Blazer.</span><span class="sxs-lookup"><span data-stu-id="e2fef-187">Sometimes, you need to configure the SignalR client used by Blazor server-side apps.</span></span> <span data-ttu-id="e2fef-188">Ad esempio, potrebbe essere necessario configurare la registrazione nel client SignalR per diagnosticare un problema di connessione.</span><span class="sxs-lookup"><span data-stu-id="e2fef-188">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>
 
<span data-ttu-id="e2fef-189">Per configurare il client SignalR nel file *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e2fef-189">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="e2fef-190">Aggiungere un `autostart="false"` attributo `<script>` al tag per lo script *blazer. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="e2fef-190">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="e2fef-191">Chiamare `Blazor.start` e passare un oggetto di configurazione che specifichi il generatore SignalR.</span><span class="sxs-lookup"><span data-stu-id="e2fef-191">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>
 
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

## <a name="additional-resources"></a><span data-ttu-id="e2fef-192">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e2fef-192">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
