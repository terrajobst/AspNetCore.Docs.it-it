---
title: ASP.NET Core Blazor modelli di hosting
author: guardrex
description: Informazioni sui modelli di hosting Blazor webassembly e Blazor server.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/23/2019
no-loc:
- Blazor
- SignalR
uid: blazor/hosting-models
ms.openlocfilehash: 38db9804c9cdd1aa31ca48af2dd9ec2e85175156
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681045"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a><span data-ttu-id="589cc-103">ASP.NET Core Blazor modelli di hosting</span><span class="sxs-lookup"><span data-stu-id="589cc-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="589cc-104">Di [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="589cc-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="589cc-105"> è un framework web progettato per l'esecuzione sul lato client nel browser in un Runtime .NET basato su [webassembly](https://webassembly.org/)( *Blazor webassembly*) o sul lato server in ASP.NET Core ( *Blazor server*).</span><span class="sxs-lookup"><span data-stu-id="589cc-105"> is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="589cc-106">Indipendentemente dal modello di hosting, i modelli di app e componenti *sono gli stessi*.</span><span class="sxs-lookup"><span data-stu-id="589cc-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="589cc-107">Per creare un progetto per i modelli di hosting descritti in questo articolo, vedere <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="589cc-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="589cc-108"> webassembly</span><span class="sxs-lookup"><span data-stu-id="589cc-108"> WebAssembly</span></span>

<span data-ttu-id="589cc-109">Il modello di hosting principale per Blazor è in esecuzione sul lato client nel browser del webassembly.</span><span class="sxs-lookup"><span data-stu-id="589cc-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="589cc-110">L'app Blazor, le relative dipendenze e il Runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="589cc-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="589cc-111">L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser.</span><span class="sxs-lookup"><span data-stu-id="589cc-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="589cc-112">Gli aggiornamenti dell'interfaccia utente e la gestione degli eventi si verificano nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="589cc-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="589cc-113">Le risorse dell'app vengono distribuite come file statici in un server Web o in un servizio in grado di servire contenuto statico ai client.</span><span class="sxs-lookup"><span data-stu-id="589cc-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![[! OP. NO-LOC (Blazer)]<span data-ttu-id="589cc-114"> webassembly: [! OP. L'app NO-LOC (Blazer)] viene eseguita su un thread dell'interfaccia utente all'interno del browser.</span><span class="sxs-lookup"><span data-stu-id="589cc-114"> WebAssembly: The Blazor app runs on a UI thread inside the browser.</span></span>](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="589cc-115">Per creare un'app Blazor usando il modello di hosting lato client, usare il modello di **app webassemblyBlazor** ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="589cc-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="589cc-116">Dopo aver selezionato il modello di **app webassemblyBlazor** , è possibile configurare l'app per l'uso di un back-end ASP.NET Core selezionando la casella di controllo **ASP.NET Core Hosted** ([DotNet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="589cc-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="589cc-117">L'app ASP.NET Core serve l'app Blazor ai client.</span><span class="sxs-lookup"><span data-stu-id="589cc-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="589cc-118">L'app webassembly Blazor può interagire con il server tramite la rete usando chiamate API Web o [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="589cc-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="589cc-119">I modelli includono lo script *blazer. webassembly. js* che gestisce:</span><span class="sxs-lookup"><span data-stu-id="589cc-119">The templates include the *blazor.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="589cc-120">Download del runtime .NET, dell'app e delle dipendenze dell'app.</span><span class="sxs-lookup"><span data-stu-id="589cc-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="589cc-121">Inizializzazione del runtime per l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="589cc-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="589cc-122">Il modello di hosting webassembly Blazor offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="589cc-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="589cc-123">Non esiste alcuna dipendenza lato server .NET.</span><span class="sxs-lookup"><span data-stu-id="589cc-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="589cc-124">L'app funziona completamente dopo il download nel client.</span><span class="sxs-lookup"><span data-stu-id="589cc-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="589cc-125">Le risorse e le funzionalità client sono completamente sfruttate.</span><span class="sxs-lookup"><span data-stu-id="589cc-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="589cc-126">Il lavoro viene scaricato dal server al client.</span><span class="sxs-lookup"><span data-stu-id="589cc-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="589cc-127">Non è necessario un server Web ASP.NET Core per ospitare l'app.</span><span class="sxs-lookup"><span data-stu-id="589cc-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="589cc-128">Gli scenari di distribuzione senza server sono possibili, ad esempio per servire l'app da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="589cc-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="589cc-129">Ci sono svantaggi per Blazor l'hosting di webassembly:</span><span class="sxs-lookup"><span data-stu-id="589cc-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="589cc-130">L'app è limitata alle funzionalità del browser.</span><span class="sxs-lookup"><span data-stu-id="589cc-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="589cc-131">È necessario disporre di hardware e software client idonei (ad esempio, il supporto di webassembly).</span><span class="sxs-lookup"><span data-stu-id="589cc-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="589cc-132">Le dimensioni del download sono maggiori e le app importano più tempo per il caricamento.</span><span class="sxs-lookup"><span data-stu-id="589cc-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="589cc-133">Il supporto di runtime e strumenti .NET è meno maturo.</span><span class="sxs-lookup"><span data-stu-id="589cc-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="589cc-134">Ad esempio, esistono limitazioni in [.NET standard](/dotnet/standard/net-standard) supporto e debug.</span><span class="sxs-lookup"><span data-stu-id="589cc-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="opno-locblazor-server"></a><span data-ttu-id="589cc-135">Server Blazor</span><span class="sxs-lookup"><span data-stu-id="589cc-135">Blazor Server</span></span>

<span data-ttu-id="589cc-136">Con il modello di hosting del server di Blazor, l'app viene eseguita nel server dall'interno di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="589cc-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="589cc-137">Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestiti tramite una connessione [SignalR](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="589cc-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) sul server tramite un [! OP. Connessione NO-LOC (SignalR)].](hosting-models/_static/blazor-server.png)

<span data-ttu-id="589cc-139">Per creare un'app Blazor usando il modello di hosting del server Blazor, usare il modello di **app ASP.NET CoreBlazor server** ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="589cc-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="589cc-140">L'app ASP.NET Core ospita l'app Blazor server e crea l'endpoint SignalR a cui si connettono i client.</span><span class="sxs-lookup"><span data-stu-id="589cc-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="589cc-141">L'app ASP.NET Core fa riferimento alla classe `Startup` dell'app per aggiungere:</span><span class="sxs-lookup"><span data-stu-id="589cc-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="589cc-142">Servizi lato server.</span><span class="sxs-lookup"><span data-stu-id="589cc-142">Server-side services.</span></span>
* <span data-ttu-id="589cc-143">App per la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="589cc-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="589cc-144">Lo script *blazer. Server. js*&dagger; stabilisce la connessione client.</span><span class="sxs-lookup"><span data-stu-id="589cc-144">The *blazor.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="589cc-145">È responsabilità dell'app salvare in modo permanente e ripristinare lo stato dell'app come richiesto, ad esempio in caso di perdita di una connessione di rete.</span><span class="sxs-lookup"><span data-stu-id="589cc-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="589cc-146">Il modello di hosting del server Blazor offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="589cc-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="589cc-147">Le dimensioni del download sono notevolmente inferiori rispetto a un'app webassembly Blazor e l'app viene caricata molto più velocemente.</span><span class="sxs-lookup"><span data-stu-id="589cc-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="589cc-148">L'app sfrutta appieno le funzionalità del server, incluso l'uso di tutte le API compatibili con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="589cc-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="589cc-149">.NET Core nel server viene usato per eseguire l'app, in modo che gli strumenti .NET esistenti, ad esempio il debug, funzionino come previsto.</span><span class="sxs-lookup"><span data-stu-id="589cc-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="589cc-150">Sono supportati i thin client.</span><span class="sxs-lookup"><span data-stu-id="589cc-150">Thin clients are supported.</span></span> <span data-ttu-id="589cc-151">Ad esempio, le app Server Blazor funzionano con i browser che non supportano webassembly e nei dispositivi con vincoli di risorse.</span><span class="sxs-lookup"><span data-stu-id="589cc-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="589cc-152">La codebase .NET/C# code dell'app, incluso il codice componente dell'app, non viene servita ai client.</span><span class="sxs-lookup"><span data-stu-id="589cc-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="589cc-153">Ci sono svantaggi per l'hosting di Blazor server:</span><span class="sxs-lookup"><span data-stu-id="589cc-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="589cc-154">In genere esiste una latenza più elevata.</span><span class="sxs-lookup"><span data-stu-id="589cc-154">Higher latency usually exists.</span></span> <span data-ttu-id="589cc-155">Ogni interazione dell'utente comporta un hop di rete.</span><span class="sxs-lookup"><span data-stu-id="589cc-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="589cc-156">Non è disponibile alcun supporto offline.</span><span class="sxs-lookup"><span data-stu-id="589cc-156">There's no offline support.</span></span> <span data-ttu-id="589cc-157">Se la connessione client non riesce, l'app smette di funzionare.</span><span class="sxs-lookup"><span data-stu-id="589cc-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="589cc-158">La scalabilità è complessa per le app con molti utenti.</span><span class="sxs-lookup"><span data-stu-id="589cc-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="589cc-159">Il server deve gestire più connessioni client e gestire lo stato del client.</span><span class="sxs-lookup"><span data-stu-id="589cc-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="589cc-160">Per gestire l'app, è necessario un server ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="589cc-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="589cc-161">Gli scenari di distribuzione senza server non sono possibili, ad esempio per servire l'app da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="589cc-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="589cc-162">&dagger;lo script *blazer. Server. js* viene servito da una risorsa incorporata nel framework condiviso ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="589cc-162">&dagger;The *blazor.server.js* script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="589cc-163">Confronto con interfaccia utente sottoposta a rendering server</span><span class="sxs-lookup"><span data-stu-id="589cc-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="589cc-164">Un modo per comprendere Blazor app Server consiste nel comprendere come si differenzia dai modelli tradizionali per il rendering dell'interfaccia utente nelle app ASP.NET Core con visualizzazioni Razor o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="589cc-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="589cc-165">Entrambi i modelli utilizzano il linguaggio Razor per descrivere il contenuto HTML, ma presentano differenze significative per il rendering del markup.</span><span class="sxs-lookup"><span data-stu-id="589cc-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="589cc-166">Quando viene eseguito il rendering di una pagina o di una visualizzazione Razor, ogni riga di codice Razor emette HTML in formato testo.</span><span class="sxs-lookup"><span data-stu-id="589cc-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="589cc-167">Dopo il rendering, il server Elimina l'istanza della pagina o della vista, incluso qualsiasi stato prodotto.</span><span class="sxs-lookup"><span data-stu-id="589cc-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="589cc-168">Quando si verifica un'altra richiesta della pagina, ad esempio quando la convalida del server non riesce e viene visualizzato il riepilogo di convalida:</span><span class="sxs-lookup"><span data-stu-id="589cc-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="589cc-169">Viene nuovamente eseguito il rendering dell'intera pagina in testo HTML.</span><span class="sxs-lookup"><span data-stu-id="589cc-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="589cc-170">La pagina viene inviata al client.</span><span class="sxs-lookup"><span data-stu-id="589cc-170">The page is sent to the client.</span></span>

<span data-ttu-id="589cc-171">Un'app Blazor è costituita da elementi riutilizzabili di un'interfaccia utente denominata *Components*.</span><span class="sxs-lookup"><span data-stu-id="589cc-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="589cc-172">Un componente contiene C# codice, markup e altri componenti.</span><span class="sxs-lookup"><span data-stu-id="589cc-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="589cc-173">Quando viene eseguito il rendering di un componente, Blazor produce un grafico dei componenti inclusi in modo analogo a un HTML o XML Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="589cc-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="589cc-174">Questo grafico include lo stato del componente contenuto in proprietà e campi.</span><span class="sxs-lookup"><span data-stu-id="589cc-174">This graph includes component state held in properties and fields.</span></span> Blazor<span data-ttu-id="589cc-175"> valuta il grafico dei componenti per produrre una rappresentazione binaria del markup.</span><span class="sxs-lookup"><span data-stu-id="589cc-175"> evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="589cc-176">Il formato binario può essere:</span><span class="sxs-lookup"><span data-stu-id="589cc-176">The binary format can be:</span></span>

* <span data-ttu-id="589cc-177">Trasformato in testo HTML (durante il prerendering).</span><span class="sxs-lookup"><span data-stu-id="589cc-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="589cc-178">Utilizzato per aggiornare in modo efficiente il markup durante il normale rendering.</span><span class="sxs-lookup"><span data-stu-id="589cc-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="589cc-179">Un aggiornamento dell'interfaccia utente in Blazor viene attivato da:</span><span class="sxs-lookup"><span data-stu-id="589cc-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="589cc-180">Interazione dell'utente, ad esempio la selezione di un pulsante.</span><span class="sxs-lookup"><span data-stu-id="589cc-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="589cc-181">Trigger dell'app, ad esempio un timer.</span><span class="sxs-lookup"><span data-stu-id="589cc-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="589cc-182">Viene eseguito il rendering del grafico e viene calcolata *una differenza dell'interfaccia utente* (differenza).</span><span class="sxs-lookup"><span data-stu-id="589cc-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="589cc-183">Questa differenza è il set più piccolo di modifiche DOM necessarie per aggiornare l'interfaccia utente nel client.</span><span class="sxs-lookup"><span data-stu-id="589cc-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="589cc-184">Il diff viene inviato al client in un formato binario e applicato dal browser.</span><span class="sxs-lookup"><span data-stu-id="589cc-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="589cc-185">Un componente viene eliminato dopo che l'utente si è spostato dal client.</span><span class="sxs-lookup"><span data-stu-id="589cc-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="589cc-186">Mentre un utente interagisce con un componente, lo stato del componente (servizi, risorse) deve essere mantenuto nella memoria del server.</span><span class="sxs-lookup"><span data-stu-id="589cc-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="589cc-187">Poiché lo stato di molti componenti può essere gestito simultaneamente dal server, l'esaurimento della memoria è un problema che deve essere risolto.</span><span class="sxs-lookup"><span data-stu-id="589cc-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="589cc-188">Per istruzioni su come creare un'app Server Blazor per garantire l'utilizzo ottimale della memoria del server, vedere <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="589cc-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="circuits"></a><span data-ttu-id="589cc-189">Circuiti</span><span class="sxs-lookup"><span data-stu-id="589cc-189">Circuits</span></span>

<span data-ttu-id="589cc-190">Un'app Server Blazor è basata sull' [SignalRASP.NET Core ](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="589cc-190">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="589cc-191">Ogni client comunica al server tramite una o più connessioni di SignalR chiamate *circuito*.</span><span class="sxs-lookup"><span data-stu-id="589cc-191">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="589cc-192">Un circuito viene Blazorastrazione su SignalR connessioni che possono tollerare interruzioni di rete temporanee.</span><span class="sxs-lookup"><span data-stu-id="589cc-192">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="589cc-193">Quando un client Blazor rileva che la connessione SignalR è disconnessa, tenta di riconnettersi al server utilizzando una nuova connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="589cc-193">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="589cc-194">Ogni schermata del browser (scheda del browser o iframe) connessa a un'app del server Blazor usa una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="589cc-194">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="589cc-195">Si tratta ancora di un'altra distinzione importante rispetto alle app tipiche sottoposte a rendering server.</span><span class="sxs-lookup"><span data-stu-id="589cc-195">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="589cc-196">In un'app sottoposta a rendering server, l'apertura della stessa app in più schermate del browser in genere non viene convertita in richieste di risorse aggiuntive sul server.</span><span class="sxs-lookup"><span data-stu-id="589cc-196">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="589cc-197">In un'app Server Blazor, ogni schermata del browser richiede un circuito separato e istanze separate dello stato dei componenti che devono essere gestite dal server.</span><span class="sxs-lookup"><span data-stu-id="589cc-197">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

Blazor<span data-ttu-id="589cc-198"> considera la chiusura di una scheda del browser o l'esplorazione di un URL esterno a una chiusura *normale* .</span><span class="sxs-lookup"><span data-stu-id="589cc-198"> considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="589cc-199">In caso di chiusura normale, il circuito e le risorse associate vengono immediatamente rilasciati.</span><span class="sxs-lookup"><span data-stu-id="589cc-199">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="589cc-200">Un client può anche disconnettersi in modo non corretto, ad esempio a causa di un'interruzione della rete.</span><span class="sxs-lookup"><span data-stu-id="589cc-200">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> Blazor<span data-ttu-id="589cc-201"> Server archivia circuiti disconnessi per un intervallo configurabile in modo da consentire la riconnessione del client.</span><span class="sxs-lookup"><span data-stu-id="589cc-201"> Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="589cc-202">Per ulteriori informazioni, vedere la sezione [riconnessione allo stesso server](#reconnection-to-the-same-server) .</span><span class="sxs-lookup"><span data-stu-id="589cc-202">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="589cc-203">Latenza interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="589cc-203">UI Latency</span></span>

<span data-ttu-id="589cc-204">La latenza dell'interfaccia utente è il tempo necessario da un'azione iniziata al momento dell'aggiornamento dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="589cc-204">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="589cc-205">I valori più bassi per la latenza dell'interfaccia utente sono imperativi perché un'app risponda a un utente.</span><span class="sxs-lookup"><span data-stu-id="589cc-205">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="589cc-206">In un'app Server Blazor, ogni azione viene inviata al server, elaborata e viene restituita una diff dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="589cc-206">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="589cc-207">Di conseguenza, la latenza dell'interfaccia utente è la somma della latenza di rete e la latenza del server nell'elaborazione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="589cc-207">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="589cc-208">Per un'app line-of-business limitata a una rete aziendale privata, l'effetto sulle percezioni utente della latenza a causa della latenza di rete è in genere impercettibile.</span><span class="sxs-lookup"><span data-stu-id="589cc-208">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="589cc-209">Per un'app distribuita su Internet, la latenza può diventare evidente per gli utenti, in particolare se gli utenti sono ampiamente distribuiti geograficamente.</span><span class="sxs-lookup"><span data-stu-id="589cc-209">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="589cc-210">L'utilizzo della memoria può anche contribuire alla latenza dell'app.</span><span class="sxs-lookup"><span data-stu-id="589cc-210">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="589cc-211">Un aumento dell'utilizzo della memoria comporta un frequente Garbage Collection o il paging della memoria su disco, che comportano una riduzione delle prestazioni dell'app e quindi aumentano la latenza dell'interfaccia</span><span class="sxs-lookup"><span data-stu-id="589cc-211">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="589cc-212">Per ulteriori informazioni, vedere <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="589cc-212">For more information, see <xref:security/blazor/server>.</span></span>

Blazor<span data-ttu-id="589cc-213"> le app Server devono essere ottimizzate per ridurre al minimo la latenza dell'interfaccia utente riducendo la latenza di rete e l'utilizzo</span><span class="sxs-lookup"><span data-stu-id="589cc-213"> Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="589cc-214">Per un approccio alla misurazione della latenza di rete, vedere <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="589cc-214">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="589cc-215">Per ulteriori informazioni su SignalR e Blazor, vedere:</span><span class="sxs-lookup"><span data-stu-id="589cc-215">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="589cc-216">Connessione al server</span><span class="sxs-lookup"><span data-stu-id="589cc-216">Connection to the server</span></span>

Blazor<span data-ttu-id="589cc-217"> app Server richiedono una connessione SignalR attiva al server.</span><span class="sxs-lookup"><span data-stu-id="589cc-217"> Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="589cc-218">Se la connessione viene persa, l'app tenta di riconnettersi al server.</span><span class="sxs-lookup"><span data-stu-id="589cc-218">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="589cc-219">Fino a quando lo stato del client è ancora in memoria, la sessione client riprende senza perdere lo stato.</span><span class="sxs-lookup"><span data-stu-id="589cc-219">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="589cc-220">Riconnessione allo stesso server</span><span class="sxs-lookup"><span data-stu-id="589cc-220">Reconnection to the same server</span></span>

<span data-ttu-id="589cc-221">Viene eseguito il prerendering di un'app Blazor server in risposta alla prima richiesta client, che imposta lo stato dell'interfaccia utente sul server.</span><span class="sxs-lookup"><span data-stu-id="589cc-221">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="589cc-222">Quando il client tenta di creare una connessione SignalR, il client deve riconnettersi allo stesso server.</span><span class="sxs-lookup"><span data-stu-id="589cc-222">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> Blazor<span data-ttu-id="589cc-223"> app server che usano più di un server back-end devono implementare *sessioni permanenti* per le connessioni SignalR.</span><span class="sxs-lookup"><span data-stu-id="589cc-223"> Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="589cc-224">È consigliabile usare il [servizio SignalR di Azure](/azure/azure-signalr) per le app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="589cc-224">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="589cc-225">Il servizio consente di aumentare le prestazioni di un'app Server Blazor per un numero elevato di connessioni SignalR simultanee.</span><span class="sxs-lookup"><span data-stu-id="589cc-225">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="589cc-226">Le sessioni permanenti sono abilitate per il servizio Azure SignalR impostando l'opzione di `ServerStickyMode` del servizio o il valore di configurazione su `Required`.</span><span class="sxs-lookup"><span data-stu-id="589cc-226">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="589cc-227">Per ulteriori informazioni, vedere <xref:host-and-deploy/blazor/server#signalr-configuration>.</span><span class="sxs-lookup"><span data-stu-id="589cc-227">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="589cc-228">Quando si usa IIS, le sessioni permanenti sono abilitate con il routing delle richieste di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="589cc-228">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="589cc-229">Per altre informazioni, vedere [bilanciamento del carico http usando il routing delle richieste di applicazioni](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="589cc-229">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="589cc-230">Riflette lo stato di connessione nell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="589cc-230">Reflect the connection state in the UI</span></span>

<span data-ttu-id="589cc-231">Quando il client rileva che la connessione è stata persa, viene visualizzata un'interfaccia utente predefinita quando il client tenta di riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="589cc-231">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="589cc-232">Se la riconnessione non riesce, all'utente viene offerta l'opzione per riprovare.</span><span class="sxs-lookup"><span data-stu-id="589cc-232">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="589cc-233">Per personalizzare l'interfaccia utente, definire un elemento con un `id` di `components-reconnect-modal` nel `<body>` della pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="589cc-233">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```html
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="589cc-234">Nella tabella seguente vengono descritte le classi CSS applicate all'elemento `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="589cc-234">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="589cc-235">Classe CSS</span><span class="sxs-lookup"><span data-stu-id="589cc-235">CSS class</span></span>                       | <span data-ttu-id="589cc-236">Indica&hellip;</span><span class="sxs-lookup"><span data-stu-id="589cc-236">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="589cc-237">Connessione persa.</span><span class="sxs-lookup"><span data-stu-id="589cc-237">A lost connection.</span></span> <span data-ttu-id="589cc-238">È in corso un tentativo di riconnessione del client.</span><span class="sxs-lookup"><span data-stu-id="589cc-238">The client is attempting to reconnect.</span></span> <span data-ttu-id="589cc-239">Mostra il modale.</span><span class="sxs-lookup"><span data-stu-id="589cc-239">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="589cc-240">Viene ristabilita una connessione attiva al server.</span><span class="sxs-lookup"><span data-stu-id="589cc-240">An active connection is re-established to the server.</span></span> <span data-ttu-id="589cc-241">Nascondere il modale.</span><span class="sxs-lookup"><span data-stu-id="589cc-241">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="589cc-242">Riconnessione non riuscita. probabilmente a causa di un errore di rete.</span><span class="sxs-lookup"><span data-stu-id="589cc-242">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="589cc-243">Per tentare la riconnessione, chiamare `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="589cc-243">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="589cc-244">Riconnessione rifiutata.</span><span class="sxs-lookup"><span data-stu-id="589cc-244">Reconnection rejected.</span></span> <span data-ttu-id="589cc-245">Il server è stato raggiunto ma ha rifiutato la connessione e lo stato dell'utente nel server è andato perso.</span><span class="sxs-lookup"><span data-stu-id="589cc-245">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="589cc-246">Per ricaricare l'app, chiamare `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="589cc-246">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="589cc-247">Questo stato di connessione può verificarsi nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="589cc-247">This connection state may result when:</span></span><ul><li><span data-ttu-id="589cc-248">Si verifica un arresto anomalo del circuito sul lato server.</span><span class="sxs-lookup"><span data-stu-id="589cc-248">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="589cc-249">Il client viene disconnesso abbastanza a lungo da consentire al server di eliminare lo stato dell'utente.</span><span class="sxs-lookup"><span data-stu-id="589cc-249">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="589cc-250">Le istanze dei componenti con cui l'utente interagisce sono state eliminate.</span><span class="sxs-lookup"><span data-stu-id="589cc-250">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="589cc-251">Il server viene riavviato oppure il processo di lavoro dell'app viene riciclato.</span><span class="sxs-lookup"><span data-stu-id="589cc-251">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="589cc-252">Riconnessione con stato dopo il rendering preliminare</span><span class="sxs-lookup"><span data-stu-id="589cc-252">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="589cc-253">per impostazione predefinita, le app di Blazor server sono impostate per eseguire il prerendering dell'interfaccia utente nel server prima che venga stabilita la connessione client al server.</span><span class="sxs-lookup"><span data-stu-id="589cc-253">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="589cc-254">Questa impostazione è configurata nella pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="589cc-254">This is set up in the *_Host.cshtml* Razor page:</span></span>

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

<span data-ttu-id="589cc-255">`RenderMode` configura se il componente:</span><span class="sxs-lookup"><span data-stu-id="589cc-255">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="589cc-256">Viene preeseguito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="589cc-256">Is prerendered into the page.</span></span>
* <span data-ttu-id="589cc-257">Viene sottoposto a rendering come HTML statico nella pagina o se include le informazioni necessarie per avviare un'app Blazor dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="589cc-257">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

::: moniker range=">= aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="589cc-258">Descrizione</span><span class="sxs-lookup"><span data-stu-id="589cc-258">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="589cc-259">Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="589cc-259">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="589cc-260">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="589cc-260">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="589cc-261">Esegue il rendering di un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="589cc-261">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="589cc-262">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="589cc-262">Output from the component isn't included.</span></span> <span data-ttu-id="589cc-263">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="589cc-263">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="589cc-264">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="589cc-264">Renders the component into static HTML.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.1"

| `RenderMode`        | <span data-ttu-id="589cc-265">Descrizione</span><span class="sxs-lookup"><span data-stu-id="589cc-265">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="589cc-266">Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="589cc-266">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="589cc-267">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="589cc-267">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="589cc-268">I parametri non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="589cc-268">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="589cc-269">Esegue il rendering di un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="589cc-269">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="589cc-270">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="589cc-270">Output from the component isn't included.</span></span> <span data-ttu-id="589cc-271">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="589cc-271">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="589cc-272">I parametri non sono supportati.</span><span class="sxs-lookup"><span data-stu-id="589cc-272">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="589cc-273">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="589cc-273">Renders the component into static HTML.</span></span> <span data-ttu-id="589cc-274">I parametri sono supportati.</span><span class="sxs-lookup"><span data-stu-id="589cc-274">Parameters are supported.</span></span> |

::: moniker-end

<span data-ttu-id="589cc-275">Il rendering dei componenti server da una pagina HTML statica non è supportato.</span><span class="sxs-lookup"><span data-stu-id="589cc-275">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="589cc-276">Quando `RenderMode` viene `ServerPrerendered`, il componente viene inizialmente sottoposto a rendering statico come parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="589cc-276">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="589cc-277">Quando il browser stabilisce una connessione al server, viene *nuovamente*eseguito il rendering del componente e il componente è ora interattivo.</span><span class="sxs-lookup"><span data-stu-id="589cc-277">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="589cc-278">Se è presente il metodo [{Async} Lifecycle OnInitialized](xref:blazor/lifecycle#component-initialization-methods) per l'inizializzazione del componente, il metodo viene eseguito *due volte*:</span><span class="sxs-lookup"><span data-stu-id="589cc-278">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="589cc-279">Quando il componente viene preeseguito in modo statico.</span><span class="sxs-lookup"><span data-stu-id="589cc-279">When the component is prerendered statically.</span></span>
* <span data-ttu-id="589cc-280">Una volta stabilita la connessione al server.</span><span class="sxs-lookup"><span data-stu-id="589cc-280">After the server connection has been established.</span></span>

<span data-ttu-id="589cc-281">Ciò può comportare una modifica evidente nei dati visualizzati nell'interfaccia utente quando il componente viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="589cc-281">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="589cc-282">Per evitare lo scenario di doppio rendering in un'app Server Blazor:</span><span class="sxs-lookup"><span data-stu-id="589cc-282">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="589cc-283">Passare un identificatore che può essere usato per memorizzare nella cache lo stato durante il prerendering e recuperare lo stato dopo il riavvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="589cc-283">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="589cc-284">Utilizzare l'identificatore durante il prerendering per salvare lo stato del componente.</span><span class="sxs-lookup"><span data-stu-id="589cc-284">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="589cc-285">Utilizzare l'identificatore dopo il prerendering per recuperare lo stato memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="589cc-285">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="589cc-286">Il codice seguente illustra un `WeatherForecastService` aggiornato in un'app Server Blazor basata su modelli che evita il doppio rendering:</span><span class="sxs-lookup"><span data-stu-id="589cc-286">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

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

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="589cc-287">Eseguire il rendering di componenti interattivi con stato da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="589cc-287">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="589cc-288">I componenti interattivi con stato possono essere aggiunti a una pagina o a una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="589cc-288">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="589cc-289">Quando viene eseguito il rendering della pagina o della visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="589cc-289">When the page or view renders:</span></span>

* <span data-ttu-id="589cc-290">Il componente viene preeseguito con la pagina o la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="589cc-290">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="589cc-291">Lo stato iniziale del componente utilizzato per il prerendering viene perso.</span><span class="sxs-lookup"><span data-stu-id="589cc-291">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="589cc-292">Quando viene stabilita la connessione SignalR viene creato il nuovo stato del componente.</span><span class="sxs-lookup"><span data-stu-id="589cc-292">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="589cc-293">La pagina Razor seguente esegue il rendering di un componente `Counter`:</span><span class="sxs-lookup"><span data-stu-id="589cc-293">The following Razor page renders a `Counter` component:</span></span>

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

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="589cc-294">Eseguire il rendering di componenti non interattivi da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="589cc-294">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="589cc-295">Nella pagina Razor seguente il componente `MyComponent` viene sottoposto a rendering statico con un valore iniziale specificato utilizzando un form:</span><span class="sxs-lookup"><span data-stu-id="589cc-295">In the following Razor page, the `MyComponent` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="589cc-296">Poiché `MyComponent` viene sottoposto a rendering statico, il componente non può essere interattivo.</span><span class="sxs-lookup"><span data-stu-id="589cc-296">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="589cc-297">Rilevare il momento in cui viene eseguito il prerendering dell'app</span><span class="sxs-lookup"><span data-stu-id="589cc-297">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="589cc-298">Configurare il client di SignalR per le app Blazor server</span><span class="sxs-lookup"><span data-stu-id="589cc-298">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="589cc-299">In alcuni casi, è necessario configurare il client SignalR usato dalle app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="589cc-299">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="589cc-300">È ad esempio possibile configurare la registrazione nel client di SignalR per diagnosticare un problema di connessione.</span><span class="sxs-lookup"><span data-stu-id="589cc-300">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="589cc-301">Per configurare il client di SignalR nel file *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="589cc-301">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="589cc-302">Aggiungere un attributo `autostart="false"` al tag `<script>` per lo script *blazer. Server. js* .</span><span class="sxs-lookup"><span data-stu-id="589cc-302">Add an `autostart="false"` attribute to the `<script>` tag for the *blazor.server.js* script.</span></span>
* <span data-ttu-id="589cc-303">Chiamare `Blazor.start` e passare un oggetto di configurazione che specifichi il generatore di SignalR.</span><span class="sxs-lookup"><span data-stu-id="589cc-303">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="589cc-304">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="589cc-304">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
