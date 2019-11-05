---
title: Modelli di hosting di ASP.NET Core Blazer
author: guardrex
description: Informazioni sui modelli di hosting di webassembly e blazer server blazer.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/03/2019
uid: blazor/hosting-models
ms.openlocfilehash: d1b9e6ab7ba93c00a569be309f2334df9e3f4140
ms.sourcegitcommit: e5d4768aaf85703effb4557a520d681af8284e26
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2019
ms.locfileid: "73616595"
---
# <a name="aspnet-core-blazor-hosting-models"></a><span data-ttu-id="59a0b-103">Modelli di hosting di ASP.NET Core Blazer</span><span class="sxs-lookup"><span data-stu-id="59a0b-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="59a0b-104">Di [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="59a0b-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="59a0b-105">Blazer è un framework web progettato per l'esecuzione sul lato client nel browser in un Runtime .NET basato su [webassembly](https://webassembly.org/)(*Blazer webassembly*) o sul lato server in ASP.NET Core (*Server Blazer*).</span><span class="sxs-lookup"><span data-stu-id="59a0b-105">Blazor is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="59a0b-106">Indipendentemente dal modello di hosting, i modelli di app e componenti *sono gli stessi*.</span><span class="sxs-lookup"><span data-stu-id="59a0b-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="59a0b-107">Per creare un progetto per i modelli di hosting descritti in questo articolo, vedere <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="59a0b-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="59a0b-108">WebAssembly Blazor</span><span class="sxs-lookup"><span data-stu-id="59a0b-108">Blazor WebAssembly</span></span>

<span data-ttu-id="59a0b-109">Il modello di hosting principale per blazer è in esecuzione sul lato client nel browser del webassembly.</span><span class="sxs-lookup"><span data-stu-id="59a0b-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="59a0b-110">L'app Blazor, le relative dipendenze e il runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="59a0b-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="59a0b-111">L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser.</span><span class="sxs-lookup"><span data-stu-id="59a0b-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="59a0b-112">Gli aggiornamenti dell'interfaccia utente e la gestione degli eventi si verificano nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="59a0b-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="59a0b-113">Le risorse dell'app vengono distribuite come file statici in un server Web o in un servizio in grado di servire contenuto statico ai client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Webassembly Blazer: l'app Blazer viene eseguita su un thread dell'interfaccia utente all'interno del browser.](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="59a0b-115">Per creare un'app Blazer usando il modello di hosting lato client, usare il modello di **app Webassembly Blazer** ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="59a0b-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="59a0b-116">Dopo aver selezionato il modello di **applicazione Webassembly Blazer** , è possibile configurare l'app per l'uso di un back-end ASP.NET Core selezionando la casella di controllo **ASP.NET Core Hosted** ([DotNet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="59a0b-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="59a0b-117">L'app ASP.NET Core serve l'app Blazer ai client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="59a0b-118">L'app webassembly blazer può interagire con il server tramite la rete usando chiamate API Web o [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="59a0b-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="59a0b-119">I modelli includono lo script *blazer. webassembly. js* che gestisce:</span><span class="sxs-lookup"><span data-stu-id="59a0b-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="59a0b-120">Download del runtime .NET, dell'app e delle dipendenze dell'app.</span><span class="sxs-lookup"><span data-stu-id="59a0b-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="59a0b-121">Inizializzazione del runtime per l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="59a0b-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="59a0b-122">Il modello di hosting webassembly Blazer offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="59a0b-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="59a0b-123">Non esiste alcuna dipendenza lato server .NET.</span><span class="sxs-lookup"><span data-stu-id="59a0b-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="59a0b-124">L'app funziona completamente dopo il download nel client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="59a0b-125">Le risorse e le funzionalità client sono completamente sfruttate.</span><span class="sxs-lookup"><span data-stu-id="59a0b-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="59a0b-126">Il lavoro viene scaricato dal server al client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="59a0b-127">Non è necessario un server Web ASP.NET Core per ospitare l'app.</span><span class="sxs-lookup"><span data-stu-id="59a0b-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="59a0b-128">Gli scenari di distribuzione senza server sono possibili, ad esempio per servire l'app da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="59a0b-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="59a0b-129">Ci sono svantaggi per l'hosting di Blazer webassembly:</span><span class="sxs-lookup"><span data-stu-id="59a0b-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="59a0b-130">L'app è limitata alle funzionalità del browser.</span><span class="sxs-lookup"><span data-stu-id="59a0b-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="59a0b-131">È necessario disporre di hardware e software client idonei (ad esempio, il supporto di webassembly).</span><span class="sxs-lookup"><span data-stu-id="59a0b-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="59a0b-132">Le dimensioni del download sono maggiori e le app importano più tempo per il caricamento.</span><span class="sxs-lookup"><span data-stu-id="59a0b-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="59a0b-133">Il supporto di runtime e strumenti .NET è meno maturo.</span><span class="sxs-lookup"><span data-stu-id="59a0b-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="59a0b-134">Ad esempio, esistono limitazioni in [.NET standard](/dotnet/standard/net-standard) supporto e debug.</span><span class="sxs-lookup"><span data-stu-id="59a0b-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="59a0b-135">Server Blazor</span><span class="sxs-lookup"><span data-stu-id="59a0b-135">Blazor Server</span></span>

<span data-ttu-id="59a0b-136">Con il modello di hosting del server blazer, l'app viene eseguita sul server dall'interno di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59a0b-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="59a0b-137">Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite una connessione [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="59a0b-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) sul server tramite una connessione SignalR.](hosting-models/_static/blazor-server.png)

<span data-ttu-id="59a0b-139">Per creare un'app Blazer usando il modello di hosting del server blazer, usare il modello di **app del server ASP.NET Core Blazer** ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="59a0b-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="59a0b-140">L'app ASP.NET Core ospita l'app Server blazer e crea l'endpoint SignalR a cui si connettono i client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="59a0b-141">L'app ASP.NET Core fa riferimento alla classe `Startup` dell'app per aggiungere:</span><span class="sxs-lookup"><span data-stu-id="59a0b-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="59a0b-142">Servizi lato server.</span><span class="sxs-lookup"><span data-stu-id="59a0b-142">Server-side services.</span></span>
* <span data-ttu-id="59a0b-143">App per la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="59a0b-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="59a0b-144">Lo script *blazer. Server. js*&dagger; stabilisce la connessione client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="59a0b-145">È responsabilità dell'app salvare in modo permanente e ripristinare lo stato dell'app come richiesto, ad esempio in caso di perdita di una connessione di rete.</span><span class="sxs-lookup"><span data-stu-id="59a0b-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="59a0b-146">Il modello di hosting del server Blazer offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="59a0b-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="59a0b-147">Le dimensioni del download sono notevolmente inferiori rispetto a un'app webassembly blazer e l'app viene caricata molto più velocemente.</span><span class="sxs-lookup"><span data-stu-id="59a0b-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="59a0b-148">L'app sfrutta appieno le funzionalità del server, incluso l'uso di tutte le API compatibili con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="59a0b-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="59a0b-149">.NET Core nel server viene usato per eseguire l'app, in modo che gli strumenti .NET esistenti, ad esempio il debug, funzionino come previsto.</span><span class="sxs-lookup"><span data-stu-id="59a0b-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="59a0b-150">Sono supportati i thin client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-150">Thin clients are supported.</span></span> <span data-ttu-id="59a0b-151">Ad esempio, le app del server Blazer funzionano con i browser che non supportano webassembly e nei dispositivi con vincoli di risorse.</span><span class="sxs-lookup"><span data-stu-id="59a0b-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="59a0b-152">La codebase .NET/C# code dell'app, incluso il codice componente dell'app, non viene servita ai client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="59a0b-153">Ci sono svantaggi per l'hosting del server Blazer:</span><span class="sxs-lookup"><span data-stu-id="59a0b-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="59a0b-154">In genere esiste una latenza più elevata.</span><span class="sxs-lookup"><span data-stu-id="59a0b-154">Higher latency usually exists.</span></span> <span data-ttu-id="59a0b-155">Ogni interazione dell'utente comporta un hop di rete.</span><span class="sxs-lookup"><span data-stu-id="59a0b-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="59a0b-156">Non è disponibile alcun supporto offline.</span><span class="sxs-lookup"><span data-stu-id="59a0b-156">There's no offline support.</span></span> <span data-ttu-id="59a0b-157">Se la connessione client non riesce, l'app smette di funzionare.</span><span class="sxs-lookup"><span data-stu-id="59a0b-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="59a0b-158">La scalabilità è complessa per le app con molti utenti.</span><span class="sxs-lookup"><span data-stu-id="59a0b-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="59a0b-159">Il server deve gestire più connessioni client e gestire lo stato del client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="59a0b-160">Per gestire l'app, è necessario un server ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59a0b-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="59a0b-161">Gli scenari di distribuzione senza server non sono possibili, ad esempio per servire l'app da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="59a0b-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="59a0b-162">lo script &dagger;The *blazer. Server. js* viene fornito da una risorsa incorporata nel framework condiviso ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="59a0b-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="59a0b-163">Confronto con interfaccia utente sottoposta a rendering server</span><span class="sxs-lookup"><span data-stu-id="59a0b-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="59a0b-164">Un modo per comprendere le app del server blazer è comprendere come si differenzia dai modelli tradizionali per il rendering dell'interfaccia utente nelle app ASP.NET Core usando le visualizzazioni Razor o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="59a0b-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="59a0b-165">Entrambi i modelli utilizzano il linguaggio Razor per descrivere il contenuto HTML, ma presentano differenze significative per il rendering del markup.</span><span class="sxs-lookup"><span data-stu-id="59a0b-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="59a0b-166">Quando viene eseguito il rendering di una pagina o di una visualizzazione Razor, ogni riga di codice Razor emette HTML in formato testo.</span><span class="sxs-lookup"><span data-stu-id="59a0b-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="59a0b-167">Dopo il rendering, il server Elimina l'istanza della pagina o della vista, incluso qualsiasi stato prodotto.</span><span class="sxs-lookup"><span data-stu-id="59a0b-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="59a0b-168">Quando si verifica un'altra richiesta della pagina, ad esempio quando la convalida del server non riesce e viene visualizzato il riepilogo di convalida:</span><span class="sxs-lookup"><span data-stu-id="59a0b-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="59a0b-169">Viene nuovamente eseguito il rendering dell'intera pagina in testo HTML.</span><span class="sxs-lookup"><span data-stu-id="59a0b-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="59a0b-170">La pagina viene inviata al client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-170">The page is sent to the client.</span></span>

<span data-ttu-id="59a0b-171">Un'app blazer è costituita da elementi riutilizzabili di un'interfaccia utente denominata *Components*.</span><span class="sxs-lookup"><span data-stu-id="59a0b-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="59a0b-172">Un componente contiene C# codice, markup e altri componenti.</span><span class="sxs-lookup"><span data-stu-id="59a0b-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="59a0b-173">Quando viene eseguito il rendering di un componente, blazer produce un grafico dei componenti inclusi in modo simile a un codice HTML o XML Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="59a0b-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="59a0b-174">Questo grafico include lo stato del componente contenuto in proprietà e campi.</span><span class="sxs-lookup"><span data-stu-id="59a0b-174">This graph includes component state held in properties and fields.</span></span> <span data-ttu-id="59a0b-175">Blazer valuta il grafico dei componenti per produrre una rappresentazione binaria del markup.</span><span class="sxs-lookup"><span data-stu-id="59a0b-175">Blazor evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="59a0b-176">Il formato binario può essere:</span><span class="sxs-lookup"><span data-stu-id="59a0b-176">The binary format can be:</span></span>

* <span data-ttu-id="59a0b-177">Trasformato in testo HTML (durante il prerendering).</span><span class="sxs-lookup"><span data-stu-id="59a0b-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="59a0b-178">Utilizzato per aggiornare in modo efficiente il markup durante il normale rendering.</span><span class="sxs-lookup"><span data-stu-id="59a0b-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="59a0b-179">Un aggiornamento dell'interfaccia utente in blazer viene attivato da:</span><span class="sxs-lookup"><span data-stu-id="59a0b-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="59a0b-180">Interazione dell'utente, ad esempio la selezione di un pulsante.</span><span class="sxs-lookup"><span data-stu-id="59a0b-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="59a0b-181">Trigger dell'app, ad esempio un timer.</span><span class="sxs-lookup"><span data-stu-id="59a0b-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="59a0b-182">Viene eseguito il rendering del grafico e viene calcolata *una differenza dell'interfaccia utente* (differenza).</span><span class="sxs-lookup"><span data-stu-id="59a0b-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="59a0b-183">Questa differenza è il set più piccolo di modifiche DOM necessarie per aggiornare l'interfaccia utente nel client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="59a0b-184">Il diff viene inviato al client in un formato binario e applicato dal browser.</span><span class="sxs-lookup"><span data-stu-id="59a0b-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="59a0b-185">Un componente viene eliminato dopo che l'utente si è spostato dal client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="59a0b-186">Mentre un utente interagisce con un componente, lo stato del componente (servizi, risorse) deve essere mantenuto nella memoria del server.</span><span class="sxs-lookup"><span data-stu-id="59a0b-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="59a0b-187">Poiché lo stato di molti componenti può essere gestito simultaneamente dal server, l'esaurimento della memoria è un problema che deve essere risolto.</span><span class="sxs-lookup"><span data-stu-id="59a0b-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="59a0b-188">Per istruzioni su come creare un'app del server blazer per garantire l'utilizzo ottimale della memoria del server, vedere <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="59a0b-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="circuits"></a><span data-ttu-id="59a0b-189">Circuiti</span><span class="sxs-lookup"><span data-stu-id="59a0b-189">Circuits</span></span>

<span data-ttu-id="59a0b-190">Un'app Server Blazer si basa su [ASP.NET Core SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="59a0b-190">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="59a0b-191">Ogni client comunica al server tramite una o più connessioni SignalR dette *circuito*.</span><span class="sxs-lookup"><span data-stu-id="59a0b-191">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="59a0b-192">Un circuito è l'astrazione di Blazer sulle connessioni SignalR che possono tollerare interruzioni di rete temporanee.</span><span class="sxs-lookup"><span data-stu-id="59a0b-192">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="59a0b-193">Quando un client Blaze rileva che la connessione SignalR è disconnessa, tenta di riconnettersi al server usando una nuova connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="59a0b-193">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="59a0b-194">Ogni schermata del browser (scheda del browser o iframe) connessa a un'app del server Blazer usa una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="59a0b-194">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="59a0b-195">Si tratta ancora di un'altra distinzione importante rispetto alle app tipiche sottoposte a rendering server.</span><span class="sxs-lookup"><span data-stu-id="59a0b-195">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="59a0b-196">In un'app sottoposta a rendering server, l'apertura della stessa app in più schermate del browser in genere non viene convertita in richieste di risorse aggiuntive sul server.</span><span class="sxs-lookup"><span data-stu-id="59a0b-196">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="59a0b-197">In un'app Server blazer, ogni schermata del browser richiede un circuito separato e istanze separate dello stato dei componenti che devono essere gestite dal server.</span><span class="sxs-lookup"><span data-stu-id="59a0b-197">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

<span data-ttu-id="59a0b-198">Blazer considera la chiusura di una scheda del browser o l'esplorazione di un URL esterno a una chiusura *normale* .</span><span class="sxs-lookup"><span data-stu-id="59a0b-198">Blazor considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="59a0b-199">In caso di chiusura normale, il circuito e le risorse associate vengono immediatamente rilasciati.</span><span class="sxs-lookup"><span data-stu-id="59a0b-199">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="59a0b-200">Un client può anche disconnettersi in modo non corretto, ad esempio a causa di un'interruzione della rete.</span><span class="sxs-lookup"><span data-stu-id="59a0b-200">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> <span data-ttu-id="59a0b-201">Il server Blazer archivia circuiti disconnessi per un intervallo configurabile in modo da consentire la riconnessione del client.</span><span class="sxs-lookup"><span data-stu-id="59a0b-201">Blazor Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="59a0b-202">Per ulteriori informazioni, vedere la sezione [riconnessione allo stesso server](#reconnection-to-the-same-server) .</span><span class="sxs-lookup"><span data-stu-id="59a0b-202">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="59a0b-203">Latenza interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="59a0b-203">UI Latency</span></span>

<span data-ttu-id="59a0b-204">La latenza dell'interfaccia utente è il tempo necessario da un'azione iniziata al momento dell'aggiornamento dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="59a0b-204">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="59a0b-205">I valori più bassi per la latenza dell'interfaccia utente sono imperativi perché un'app risponda a un utente.</span><span class="sxs-lookup"><span data-stu-id="59a0b-205">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="59a0b-206">In un'app Server Blazer ogni azione viene inviata al server, elaborata e viene restituita una diff dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="59a0b-206">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="59a0b-207">Di conseguenza, la latenza dell'interfaccia utente è la somma della latenza di rete e la latenza del server nell'elaborazione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="59a0b-207">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="59a0b-208">Per un'app line-of-business limitata a una rete aziendale privata, l'effetto sulle percezioni utente della latenza a causa della latenza di rete è in genere impercettibile.</span><span class="sxs-lookup"><span data-stu-id="59a0b-208">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="59a0b-209">Per un'app distribuita su Internet, la latenza può diventare evidente per gli utenti, in particolare se gli utenti sono ampiamente distribuiti geograficamente.</span><span class="sxs-lookup"><span data-stu-id="59a0b-209">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="59a0b-210">L'utilizzo della memoria può anche contribuire alla latenza dell'app.</span><span class="sxs-lookup"><span data-stu-id="59a0b-210">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="59a0b-211">Un aumento dell'utilizzo della memoria comporta un frequente Garbage Collection o il paging della memoria su disco, che comportano una riduzione delle prestazioni dell'app e quindi aumentano la latenza dell'interfaccia</span><span class="sxs-lookup"><span data-stu-id="59a0b-211">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="59a0b-212">Per ulteriori informazioni, vedere <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="59a0b-212">For more information, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="59a0b-213">Le app del server Blazer devono essere ottimizzate per ridurre la latenza dell'interfaccia utente riducendo la latenza di rete e l'utilizzo della memoria</span><span class="sxs-lookup"><span data-stu-id="59a0b-213">Blazor Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="59a0b-214">Per un approccio alla misurazione della latenza di rete, vedere <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="59a0b-214">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="59a0b-215">Per ulteriori informazioni su SignalR e blazer, vedere:</span><span class="sxs-lookup"><span data-stu-id="59a0b-215">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="59a0b-216">Riconnessione allo stesso server</span><span class="sxs-lookup"><span data-stu-id="59a0b-216">Reconnection to the same server</span></span>

<span data-ttu-id="59a0b-217">Le app del server Blazer richiedono una connessione SignalR attiva al server.</span><span class="sxs-lookup"><span data-stu-id="59a0b-217">Blazor Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="59a0b-218">Se la connessione viene persa, l'app tenta di riconnettersi al server.</span><span class="sxs-lookup"><span data-stu-id="59a0b-218">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="59a0b-219">Fino a quando lo stato del client è ancora in memoria, la sessione client riprende senza perdere lo stato.</span><span class="sxs-lookup"><span data-stu-id="59a0b-219">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

<span data-ttu-id="59a0b-220">Quando il client rileva che la connessione è stata persa, viene visualizzata un'interfaccia utente predefinita quando il client tenta di riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="59a0b-220">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="59a0b-221">Se la riconnessione non riesce, all'utente viene offerta l'opzione per riprovare.</span><span class="sxs-lookup"><span data-stu-id="59a0b-221">If reconnection fails, the user is provided the option to retry.</span></span> <span data-ttu-id="59a0b-222">Per personalizzare l'interfaccia utente, definire un elemento con `components-reconnect-modal` come `id` nella pagina Razor *_Host. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="59a0b-222">To customize the UI, define an element with `components-reconnect-modal` as its `id` in the *_Host.cshtml* Razor page.</span></span> <span data-ttu-id="59a0b-223">Il client aggiorna questo elemento con una delle seguenti classi CSS in base allo stato della connessione:</span><span class="sxs-lookup"><span data-stu-id="59a0b-223">The client updates this element with one of the following CSS classes based on the state of the connection:</span></span>

* <span data-ttu-id="59a0b-224">`components-reconnect-show` &ndash; Mostra l'interfaccia utente per indicare una connessione persa e il client sta tentando di riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="59a0b-224">`components-reconnect-show` &ndash; Show the UI to indicate a lost connection and the client is attempting to reconnect.</span></span>
* <span data-ttu-id="59a0b-225">`components-reconnect-hide` &ndash; il client dispone di una connessione attiva, nascondere l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="59a0b-225">`components-reconnect-hide` &ndash; The client has an active connection, hide the UI.</span></span>
* <span data-ttu-id="59a0b-226">riconnessione `components-reconnect-failed` &ndash; non riuscita, probabilmente a causa di un errore di rete.</span><span class="sxs-lookup"><span data-stu-id="59a0b-226">`components-reconnect-failed` &ndash; Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="59a0b-227">Per tentare la riconnessione, chiamare `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="59a0b-227">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span>
* <span data-ttu-id="59a0b-228">riconnessione `components-reconnect-rejected` &ndash; rifiutata.</span><span class="sxs-lookup"><span data-stu-id="59a0b-228">`components-reconnect-rejected` &ndash; Reconnection rejected.</span></span> <span data-ttu-id="59a0b-229">Il server è stato raggiunto ma ha rifiutato la connessione e lo stato dell'utente sul server è andato perso.</span><span class="sxs-lookup"><span data-stu-id="59a0b-229">The server was reached but refused the connection, and the user's state on the server is gone.</span></span> <span data-ttu-id="59a0b-230">Per ricaricare l'app, chiamare `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="59a0b-230">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="59a0b-231">Questo stato di connessione può verificarsi nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="59a0b-231">This connection state may result when:</span></span>
  * <span data-ttu-id="59a0b-232">Si verifica un arresto anomalo del circuito (codice sul lato server).</span><span class="sxs-lookup"><span data-stu-id="59a0b-232">A crash in the circuit (server-side code) occurs.</span></span>
  * <span data-ttu-id="59a0b-233">Il client viene disconnesso abbastanza a lungo da consentire al server di eliminare lo stato dell'utente.</span><span class="sxs-lookup"><span data-stu-id="59a0b-233">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="59a0b-234">Vengono eliminate le istanze dei componenti con cui l'utente interagisce.</span><span class="sxs-lookup"><span data-stu-id="59a0b-234">Instances of components that the user was interacting with are disposed.</span></span>

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="59a0b-235">Riconnessione con stato dopo il rendering preliminare</span><span class="sxs-lookup"><span data-stu-id="59a0b-235">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="59a0b-236">Per impostazione predefinita, le app del server Blaze sono configurate per eseguire il prerendering dell'interfaccia utente nel server prima che venga stabilita la connessione client al server.</span><span class="sxs-lookup"><span data-stu-id="59a0b-236">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="59a0b-237">Questa impostazione è configurata nella pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="59a0b-237">This is set up in the *_Host.cshtml* Razor page:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<body>
    <app>@(await Html.RenderComponentAsync<App>(RenderMode.ServerPrerendered))</app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

::: moniker-end

<span data-ttu-id="59a0b-238">`RenderMode` configura se il componente:</span><span class="sxs-lookup"><span data-stu-id="59a0b-238">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="59a0b-239">Viene preeseguito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="59a0b-239">Is prerendered into the page.</span></span>
* <span data-ttu-id="59a0b-240">Viene visualizzato come HTML statico nella pagina o se include le informazioni necessarie per eseguire il bootstrap di un'app Blazer dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="59a0b-240">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

::: moniker range=">= aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="59a0b-241">Descrizione</span><span class="sxs-lookup"><span data-stu-id="59a0b-241">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="59a0b-242">Esegue il rendering del componente in HTML statico e include un marcatore per un'app del server blazer.</span><span class="sxs-lookup"><span data-stu-id="59a0b-242">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="59a0b-243">Quando l'agente utente viene avviato, questo marcatore viene usato per il bootstrap di un'app blazer.</span><span class="sxs-lookup"><span data-stu-id="59a0b-243">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="59a0b-244">Esegue il rendering di un marcatore per un'app del server blazer.</span><span class="sxs-lookup"><span data-stu-id="59a0b-244">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="59a0b-245">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="59a0b-245">Output from the component isn't included.</span></span> <span data-ttu-id="59a0b-246">Quando l'agente utente viene avviato, questo marcatore viene usato per il bootstrap di un'app blazer.</span><span class="sxs-lookup"><span data-stu-id="59a0b-246">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="59a0b-247">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="59a0b-247">Renders the component into static HTML.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="59a0b-248">Descrizione</span><span class="sxs-lookup"><span data-stu-id="59a0b-248">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="59a0b-249">Esegue il rendering del componente in HTML statico e include un marcatore per un'app del server blazer.</span><span class="sxs-lookup"><span data-stu-id="59a0b-249">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="59a0b-250">Quando l'agente utente viene avviato, questo marcatore viene usato per il bootstrap di un'app blazer.</span><span class="sxs-lookup"><span data-stu-id="59a0b-250">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="59a0b-251">I parametri non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="59a0b-251">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="59a0b-252">Esegue il rendering di un marcatore per un'app del server blazer.</span><span class="sxs-lookup"><span data-stu-id="59a0b-252">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="59a0b-253">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="59a0b-253">Output from the component isn't included.</span></span> <span data-ttu-id="59a0b-254">Quando l'agente utente viene avviato, questo marcatore viene usato per il bootstrap di un'app blazer.</span><span class="sxs-lookup"><span data-stu-id="59a0b-254">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="59a0b-255">I parametri non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="59a0b-255">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="59a0b-256">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="59a0b-256">Renders the component into static HTML.</span></span> <span data-ttu-id="59a0b-257">I parametri sono supportati.</span><span class="sxs-lookup"><span data-stu-id="59a0b-257">Parameters are supported.</span></span> |

::: moniker-end

<span data-ttu-id="59a0b-258">Il rendering dei componenti server da una pagina HTML statica non è supportato.</span><span class="sxs-lookup"><span data-stu-id="59a0b-258">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="59a0b-259">Quando `RenderMode` viene `ServerPrerendered`, il componente viene inizialmente sottoposto a rendering statico come parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="59a0b-259">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="59a0b-260">Quando il browser stabilisce una connessione al server, viene *nuovamente*eseguito il rendering del componente e il componente è ora interattivo.</span><span class="sxs-lookup"><span data-stu-id="59a0b-260">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="59a0b-261">Se è presente un [metodo del ciclo](xref:blazor/components#lifecycle-methods) di vita per inizializzare il componente (`OnInitialized{Async}`), il metodo viene eseguito *due volte*:</span><span class="sxs-lookup"><span data-stu-id="59a0b-261">If a [lifecycle method](xref:blazor/components#lifecycle-methods) for initializing the component (`OnInitialized{Async}`) is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="59a0b-262">Quando il componente viene preeseguito in modo statico.</span><span class="sxs-lookup"><span data-stu-id="59a0b-262">When the component is prerendered statically.</span></span>
* <span data-ttu-id="59a0b-263">Una volta stabilita la connessione al server.</span><span class="sxs-lookup"><span data-stu-id="59a0b-263">After the server connection has been established.</span></span>

<span data-ttu-id="59a0b-264">Ciò può comportare una modifica evidente nei dati visualizzati nell'interfaccia utente quando il componente viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="59a0b-264">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="59a0b-265">Per evitare lo scenario di doppio rendering in un'app Server Blazer:</span><span class="sxs-lookup"><span data-stu-id="59a0b-265">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="59a0b-266">Passare un identificatore che può essere usato per memorizzare nella cache lo stato durante il prerendering e recuperare lo stato dopo il riavvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="59a0b-266">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="59a0b-267">Utilizzare l'identificatore durante il prerendering per salvare lo stato del componente.</span><span class="sxs-lookup"><span data-stu-id="59a0b-267">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="59a0b-268">Utilizzare l'identificatore dopo il prerendering per recuperare lo stato memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="59a0b-268">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="59a0b-269">Il codice seguente illustra un `WeatherForecastService` aggiornato in un'app del server Blazer basata su modello che evita il doppio rendering:</span><span class="sxs-lookup"><span data-stu-id="59a0b-269">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] Summaries = new[]
    {
        "Freezing", "Bracing", "Chilly", "Cool", "Mild",
        "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
    };
    
    public WeatherForecastService(IMemoryCache memoryCache)
    {
        MemoryCache = memoryCache;
    }
    
    public IMemoryCache MemoryCache { get; }

    public Task<WeatherForecast[]> GetForecastAsync(DateTime startDate)
    {
        return MemoryCache.GetOrCreateAsync(startDate, async e =>
        {
            e.SetOptions(new MemoryCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = 
                    TimeSpan.FromSeconds(30)
            });

            var rng = new Random();

            await Task.Delay(TimeSpan.FromSeconds(10));

            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = startDate.AddDays(index),
                TemperatureC = rng.Next(-20, 55),
                Summary = Summaries[rng.Next(Summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="59a0b-270">Eseguire il rendering di componenti interattivi con stato da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="59a0b-270">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="59a0b-271">I componenti interattivi con stato possono essere aggiunti a una pagina o a una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="59a0b-271">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="59a0b-272">Quando viene eseguito il rendering della pagina o della visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="59a0b-272">When the page or view renders:</span></span>

* <span data-ttu-id="59a0b-273">Il componente viene preeseguito con la pagina o la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="59a0b-273">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="59a0b-274">Lo stato iniziale del componente utilizzato per il prerendering viene perso.</span><span class="sxs-lookup"><span data-stu-id="59a0b-274">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="59a0b-275">Quando viene stabilita la connessione SignalR, viene creato il nuovo stato del componente.</span><span class="sxs-lookup"><span data-stu-id="59a0b-275">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="59a0b-276">La pagina Razor seguente esegue il rendering di un componente `Counter`:</span><span class="sxs-lookup"><span data-stu-id="59a0b-276">The following Razor page renders a `Counter` component:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

@(await Html.RenderComponentAsync<Counter>(RenderMode.ServerPrerendered))

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="59a0b-277">Eseguire il rendering di componenti non interattivi da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="59a0b-277">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="59a0b-278">Nella pagina Razor seguente il componente `MyComponent` viene sottoposto a rendering statico con un valore iniziale specificato utilizzando un form:</span><span class="sxs-lookup"><span data-stu-id="59a0b-278">In the following Razor page, the `MyComponent` component is statically rendered with an initial value that's specified using a form:</span></span>

::: moniker range=">= aspnetcore-3.1"

```cshtml
<h1>My Razor Page</h1>

<form>
    <input type="number" asp-for="InitialValue" />
    <button type="submit">Set initial value</button>
</form>

<component type="typeof(Counter)" render-mode="Static" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

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

::: moniker-end

<span data-ttu-id="59a0b-279">Poiché `MyComponent` viene sottoposto a rendering statico, il componente non può essere interattivo.</span><span class="sxs-lookup"><span data-stu-id="59a0b-279">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="59a0b-280">Rilevare il momento in cui viene eseguito il prerendering dell'app</span><span class="sxs-lookup"><span data-stu-id="59a0b-280">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-signalr-client-for-blazor-server-apps"></a><span data-ttu-id="59a0b-281">Configurare il client SignalR per le app del server Blazer</span><span class="sxs-lookup"><span data-stu-id="59a0b-281">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="59a0b-282">In alcuni casi, è necessario configurare il client SignalR usato dalle app del server blazer.</span><span class="sxs-lookup"><span data-stu-id="59a0b-282">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="59a0b-283">Ad esempio, potrebbe essere necessario configurare la registrazione nel client SignalR per diagnosticare un problema di connessione.</span><span class="sxs-lookup"><span data-stu-id="59a0b-283">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="59a0b-284">Per configurare il client SignalR nel file *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="59a0b-284">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="59a0b-285">Aggiungere un attributo `autostart="false"` al tag `<script>` per lo script *blazer. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="59a0b-285">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="59a0b-286">Chiamare `Blazor.start` e passare un oggetto di configurazione che specifica il generatore SignalR.</span><span class="sxs-lookup"><span data-stu-id="59a0b-286">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

```html
<script src="_framework/blazor.server.js" autostart="false"></script>
<script>
  Blazor.start({
    configureSignalR: function (builder) {
      builder.configureLogging("information"); // LogLevel.Information
    }
  });
</script>
```

## <a name="additional-resources"></a><span data-ttu-id="59a0b-287">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="59a0b-287">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
