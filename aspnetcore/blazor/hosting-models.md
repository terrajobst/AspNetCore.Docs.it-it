---
title: ASP.NET Core Blazor modelli di hosting
author: guardrex
description: Informazioni sui modelli di hosting Blazor webassembly e Blazor server.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
- blazor.webassembly.js
uid: blazor/hosting-models
ms.openlocfilehash: 2c66bede9c1e31b22fd1612fead556176d6f192b
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726857"
---
# <a name="aspnet-core-opno-locblazor-hosting-models"></a><span data-ttu-id="ed7bb-103">ASP.NET Core Blazor modelli di hosting</span><span class="sxs-lookup"><span data-stu-id="ed7bb-103">ASP.NET Core Blazor hosting models</span></span>

<span data-ttu-id="ed7bb-104">Di [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="ed7bb-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor<span data-ttu-id="ed7bb-105"> è un framework web progettato per l'esecuzione sul lato client nel browser in un Runtime .NET basato su [webassembly](https://webassembly.org/)( *Blazor webassembly*) o sul lato server in ASP.NET Core ( *Blazor server*).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-105"> is a web framework designed to run client-side in the browser on a [WebAssembly](https://webassembly.org/)-based .NET runtime (*Blazor WebAssembly*) or server-side in ASP.NET Core (*Blazor Server*).</span></span> <span data-ttu-id="ed7bb-106">Indipendentemente dal modello di hosting, i modelli di app e componenti *sono gli stessi*.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-106">Regardless of the hosting model, the app and component models *are the same*.</span></span>

<span data-ttu-id="ed7bb-107">Per creare un progetto per i modelli di hosting descritti in questo articolo, vedere <xref:blazor/get-started>.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-107">To create a project for the hosting models described in this article, see <xref:blazor/get-started>.</span></span>

## <a name="opno-locblazor-webassembly"></a>Blazor<span data-ttu-id="ed7bb-108"> webassembly</span><span class="sxs-lookup"><span data-stu-id="ed7bb-108"> WebAssembly</span></span>

<span data-ttu-id="ed7bb-109">Il modello di hosting principale per Blazor è in esecuzione sul lato client nel browser del webassembly.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-109">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="ed7bb-110">L'app Blazor, le relative dipendenze e il Runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-110">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="ed7bb-111">L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-111">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="ed7bb-112">Gli aggiornamenti dell'interfaccia utente e la gestione degli eventi si verificano nello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-112">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="ed7bb-113">Le risorse dell'app vengono distribuite come file statici in un server Web o in un servizio in grado di servire contenuto statico ai client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-113">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![[! OP. NO-LOC (Blazer)]<span data-ttu-id="ed7bb-114"> webassembly: [! OP. L'app NO-LOC (Blazer)] viene eseguita su un thread dell'interfaccia utente all'interno del browser.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-114"> WebAssembly: The Blazor app runs on a UI thread inside the browser.</span></span>](hosting-models/_static/blazor-webassembly.png)

<span data-ttu-id="ed7bb-115">Per creare un'app Blazor usando il modello di hosting lato client, usare il modello di **app webassemblyBlazor** ([DotNet New blazorwasm](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-115">To create a Blazor app using the client-side hosting model, use the **Blazor WebAssembly App** template ([dotnet new blazorwasm](/dotnet/core/tools/dotnet-new)).</span></span>

<span data-ttu-id="ed7bb-116">Dopo aver selezionato il modello di **app webassemblyBlazor** , è possibile configurare l'app per l'uso di un back-end ASP.NET Core selezionando la casella di controllo **ASP.NET Core Hosted** ([DotNet New blazorwasm--Hosted](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-116">After selecting the **Blazor WebAssembly App** template, you have the option of configuring the app to use an ASP.NET Core backend by selecting the **ASP.NET Core hosted** check box ([dotnet new blazorwasm --hosted](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="ed7bb-117">L'app ASP.NET Core serve l'app Blazor ai client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-117">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="ed7bb-118">L'app webassembly Blazor può interagire con il server tramite la rete usando chiamate API Web o [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-118">The Blazor WebAssembly app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="ed7bb-119">I modelli includono lo script `blazor.webassembly.js` che gestisce:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-119">The templates include the `blazor.webassembly.js` script that handles:</span></span>

* <span data-ttu-id="ed7bb-120">Download del runtime .NET, dell'app e delle dipendenze dell'app.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-120">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="ed7bb-121">Inizializzazione del runtime per l'esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-121">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="ed7bb-122">Il modello di hosting webassembly Blazor offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-122">The Blazor WebAssembly hosting model offers several benefits:</span></span>

* <span data-ttu-id="ed7bb-123">Non esiste alcuna dipendenza lato server .NET.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-123">There's no .NET server-side dependency.</span></span> <span data-ttu-id="ed7bb-124">L'app funziona completamente dopo il download nel client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-124">The app is fully functioning after downloaded to the client.</span></span>
* <span data-ttu-id="ed7bb-125">Le risorse e le funzionalità client sono completamente sfruttate.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-125">Client resources and capabilities are fully leveraged.</span></span>
* <span data-ttu-id="ed7bb-126">Il lavoro viene scaricato dal server al client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-126">Work is offloaded from the server to the client.</span></span>
* <span data-ttu-id="ed7bb-127">Non è necessario un server Web ASP.NET Core per ospitare l'app.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-127">An ASP.NET Core web server isn't required to host the app.</span></span> <span data-ttu-id="ed7bb-128">Gli scenari di distribuzione senza server sono possibili, ad esempio per servire l'app da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-128">Serverless deployment scenarios are possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="ed7bb-129">Ci sono svantaggi per Blazor l'hosting di webassembly:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-129">There are downsides to Blazor WebAssembly hosting:</span></span>

* <span data-ttu-id="ed7bb-130">L'app è limitata alle funzionalità del browser.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-130">The app is restricted to the capabilities of the browser.</span></span>
* <span data-ttu-id="ed7bb-131">È necessario disporre di hardware e software client idonei (ad esempio, il supporto di webassembly).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-131">Capable client hardware and software (for example, WebAssembly support) is required.</span></span>
* <span data-ttu-id="ed7bb-132">Le dimensioni del download sono maggiori e le app importano più tempo per il caricamento.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-132">Download size is larger, and apps take longer to load.</span></span>
* <span data-ttu-id="ed7bb-133">Il supporto di runtime e strumenti .NET è meno maturo.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-133">.NET runtime and tooling support is less mature.</span></span> <span data-ttu-id="ed7bb-134">Ad esempio, esistono limitazioni in [.NET standard](/dotnet/standard/net-standard) supporto e debug.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-134">For example, limitations exist in [.NET Standard](/dotnet/standard/net-standard) support and debugging.</span></span>

## <a name="opno-locblazor-server"></a><span data-ttu-id="ed7bb-135">Server Blazor</span><span class="sxs-lookup"><span data-stu-id="ed7bb-135">Blazor Server</span></span>

<span data-ttu-id="ed7bb-136">Con il modello di hosting del server di Blazor, l'app viene eseguita nel server dall'interno di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-136">With the Blazor Server hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="ed7bb-137">Gli aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestiti tramite una connessione [SignalR](xref:signalr/introduction) .</span><span class="sxs-lookup"><span data-stu-id="ed7bb-137">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![Il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) sul server tramite un [! OP. Connessione NO-LOC (SignalR)].](hosting-models/_static/blazor-server.png)

<span data-ttu-id="ed7bb-139">Per creare un'app Blazor usando il modello di hosting del server Blazor, usare il modello di **app ASP.NET CoreBlazor server** ([DotNet New blazorserver](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-139">To create a Blazor app using the Blazor Server hosting model, use the ASP.NET Core **Blazor Server App** template ([dotnet new blazorserver](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="ed7bb-140">L'app ASP.NET Core ospita l'app Blazor server e crea l'endpoint SignalR a cui si connettono i client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-140">The ASP.NET Core app hosts the Blazor Server app and creates the SignalR endpoint where clients connect.</span></span>

<span data-ttu-id="ed7bb-141">L'app ASP.NET Core fa riferimento alla classe `Startup` dell'app per aggiungere:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-141">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="ed7bb-142">Servizi lato server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-142">Server-side services.</span></span>
* <span data-ttu-id="ed7bb-143">App per la pipeline di gestione delle richieste.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-143">The app to the request handling pipeline.</span></span>

<span data-ttu-id="ed7bb-144">Lo script `blazor.server.js`&dagger; stabilisce la connessione client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-144">The `blazor.server.js` script&dagger; establishes the client connection.</span></span> <span data-ttu-id="ed7bb-145">È responsabilità dell'app salvare in modo permanente e ripristinare lo stato dell'app come richiesto, ad esempio in caso di perdita di una connessione di rete.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-145">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="ed7bb-146">Il modello di hosting del server Blazor offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-146">The Blazor Server hosting model offers several benefits:</span></span>

* <span data-ttu-id="ed7bb-147">Le dimensioni del download sono notevolmente inferiori rispetto a un'app webassembly Blazor e l'app viene caricata molto più velocemente.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-147">Download size is significantly smaller than a Blazor WebAssembly app, and the app loads much faster.</span></span>
* <span data-ttu-id="ed7bb-148">L'app sfrutta appieno le funzionalità del server, incluso l'uso di tutte le API compatibili con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-148">The app takes full advantage of server capabilities, including use of any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="ed7bb-149">.NET Core nel server viene usato per eseguire l'app, in modo che gli strumenti .NET esistenti, ad esempio il debug, funzionino come previsto.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-149">.NET Core on the server is used to run the app, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="ed7bb-150">Sono supportati i thin client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-150">Thin clients are supported.</span></span> <span data-ttu-id="ed7bb-151">Ad esempio, le app Server Blazor funzionano con i browser che non supportano webassembly e nei dispositivi con vincoli di risorse.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-151">For example, Blazor Server apps work with browsers that don't support WebAssembly and on resource-constrained devices.</span></span>
* <span data-ttu-id="ed7bb-152">La codebase .NET/C# code dell'app, incluso il codice componente dell'app, non viene servita ai client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-152">The app's .NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="ed7bb-153">Ci sono svantaggi per l'hosting di Blazor server:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-153">There are downsides to Blazor Server hosting:</span></span>

* <span data-ttu-id="ed7bb-154">In genere esiste una latenza più elevata.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-154">Higher latency usually exists.</span></span> <span data-ttu-id="ed7bb-155">Ogni interazione dell'utente comporta un hop di rete.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-155">Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="ed7bb-156">Non è disponibile alcun supporto offline.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-156">There's no offline support.</span></span> <span data-ttu-id="ed7bb-157">Se la connessione client non riesce, l'app smette di funzionare.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-157">If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="ed7bb-158">La scalabilità è complessa per le app con molti utenti.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-158">Scalability is challenging for apps with many users.</span></span> <span data-ttu-id="ed7bb-159">Il server deve gestire più connessioni client e gestire lo stato del client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-159">The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="ed7bb-160">Per gestire l'app, è necessario un server ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-160">An ASP.NET Core server is required to serve the app.</span></span> <span data-ttu-id="ed7bb-161">Gli scenari di distribuzione senza server non sono possibili, ad esempio per servire l'app da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-161">Serverless deployment scenarios aren't possible (for example, serving the app from a CDN).</span></span>

<span data-ttu-id="ed7bb-162">&dagger;lo script di `blazor.server.js` viene servito da una risorsa incorporata nel framework ASP.NET Core Shared.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-162">&dagger;The `blazor.server.js` script is served from an embedded resource in the ASP.NET Core shared framework.</span></span>

### <a name="comparison-to-server-rendered-ui"></a><span data-ttu-id="ed7bb-163">Confronto con interfaccia utente sottoposta a rendering server</span><span class="sxs-lookup"><span data-stu-id="ed7bb-163">Comparison to server-rendered UI</span></span>

<span data-ttu-id="ed7bb-164">Un modo per comprendere Blazor app Server consiste nel comprendere come si differenzia dai modelli tradizionali per il rendering dell'interfaccia utente nelle app ASP.NET Core con visualizzazioni Razor o Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-164">One way to understand Blazor Server apps is to understand how it differs from traditional models for rendering UI in ASP.NET Core apps using Razor views or Razor Pages.</span></span> <span data-ttu-id="ed7bb-165">Entrambi i modelli utilizzano il linguaggio Razor per descrivere il contenuto HTML, ma presentano differenze significative per il rendering del markup.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-165">Both models use the Razor language to describe HTML content, but they significantly differ in how markup is rendered.</span></span>

<span data-ttu-id="ed7bb-166">Quando viene eseguito il rendering di una pagina o di una visualizzazione Razor, ogni riga di codice Razor emette HTML in formato testo.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-166">When a Razor Page or view is rendered, every line of Razor code emits HTML in text form.</span></span> <span data-ttu-id="ed7bb-167">Dopo il rendering, il server Elimina l'istanza della pagina o della vista, incluso qualsiasi stato prodotto.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-167">After rendering, the server disposes of the page or view instance, including any state that was produced.</span></span> <span data-ttu-id="ed7bb-168">Quando si verifica un'altra richiesta della pagina, ad esempio quando la convalida del server non riesce e viene visualizzato il riepilogo di convalida:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-168">When another request for the page occurs, for instance when server validation fails and the validation summary is displayed:</span></span>

* <span data-ttu-id="ed7bb-169">Viene nuovamente eseguito il rendering dell'intera pagina in testo HTML.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-169">The entire page is rerendered to HTML text again.</span></span>
* <span data-ttu-id="ed7bb-170">La pagina viene inviata al client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-170">The page is sent to the client.</span></span>

<span data-ttu-id="ed7bb-171">Un'app Blazor è costituita da elementi riutilizzabili di un'interfaccia utente denominata *Components*.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-171">A Blazor app is composed of reusable elements of UI called *components*.</span></span> <span data-ttu-id="ed7bb-172">Un componente contiene C# codice, markup e altri componenti.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-172">A component contains C# code, markup, and other components.</span></span> <span data-ttu-id="ed7bb-173">Quando viene eseguito il rendering di un componente, Blazor produce un grafico dei componenti inclusi in modo analogo a un HTML o XML Document Object Model (DOM).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-173">When a component is rendered, Blazor produces a graph of the included components similar to an HTML or XML Document Object Model (DOM).</span></span> <span data-ttu-id="ed7bb-174">Questo grafico include lo stato del componente contenuto in proprietà e campi.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-174">This graph includes component state held in properties and fields.</span></span> Blazor<span data-ttu-id="ed7bb-175"> valuta il grafico dei componenti per produrre una rappresentazione binaria del markup.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-175"> evaluates the component graph to produce a binary representation of the markup.</span></span> <span data-ttu-id="ed7bb-176">Il formato binario può essere:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-176">The binary format can be:</span></span>

* <span data-ttu-id="ed7bb-177">Trasformato in testo HTML (durante il prerendering).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-177">Turned into HTML text (during prerendering).</span></span>
* <span data-ttu-id="ed7bb-178">Utilizzato per aggiornare in modo efficiente il markup durante il normale rendering.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-178">Used to efficiently update the markup during regular rendering.</span></span>

<span data-ttu-id="ed7bb-179">Un aggiornamento dell'interfaccia utente in Blazor viene attivato da:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-179">A UI update in Blazor is triggered by:</span></span>

* <span data-ttu-id="ed7bb-180">Interazione dell'utente, ad esempio la selezione di un pulsante.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-180">User interaction, such as selecting a button.</span></span>
* <span data-ttu-id="ed7bb-181">Trigger dell'app, ad esempio un timer.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-181">App triggers, such as a timer.</span></span>

<span data-ttu-id="ed7bb-182">Viene eseguito il rendering del grafico e viene calcolata *una differenza dell'interfaccia utente* (differenza).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-182">The graph is rerendered, and a UI *diff* (difference) is calculated.</span></span> <span data-ttu-id="ed7bb-183">Questa differenza è il set più piccolo di modifiche DOM necessarie per aggiornare l'interfaccia utente nel client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-183">This diff is the smallest set of DOM edits required to update the UI on the client.</span></span> <span data-ttu-id="ed7bb-184">Il diff viene inviato al client in un formato binario e applicato dal browser.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-184">The diff is sent to the client in a binary format and applied by the browser.</span></span>

<span data-ttu-id="ed7bb-185">Un componente viene eliminato dopo che l'utente si è spostato dal client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-185">A component is disposed after the user navigates away from it on the client.</span></span> <span data-ttu-id="ed7bb-186">Mentre un utente interagisce con un componente, lo stato del componente (servizi, risorse) deve essere mantenuto nella memoria del server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-186">While a user is interacting with a component, the component's state (services, resources) must be held in the server's memory.</span></span> <span data-ttu-id="ed7bb-187">Poiché lo stato di molti componenti può essere gestito simultaneamente dal server, l'esaurimento della memoria è un problema che deve essere risolto.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-187">Because the state of many components might be maintained by the server concurrently, memory exhaustion is a concern that must be addressed.</span></span> <span data-ttu-id="ed7bb-188">Per istruzioni su come creare un'app Server Blazor per garantire l'utilizzo ottimale della memoria del server, vedere <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-188">For guidance on how to author a Blazor Server app to ensure the best use of server memory, see <xref:security/blazor/server>.</span></span>

### <a name="integrate-razor-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="ed7bb-189">Integrare i componenti Razor in app Razor Pages e MVC</span><span class="sxs-lookup"><span data-stu-id="ed7bb-189">Integrate Razor components into Razor Pages and MVC apps</span></span>

#### <a name="use-components-in-pages-and-views"></a><span data-ttu-id="ed7bb-190">Usare i componenti in pagine e visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="ed7bb-190">Use components in pages and views</span></span>

<span data-ttu-id="ed7bb-191">Un'app Razor Pages o MVC esistente può integrare i componenti Razor in pagine e visualizzazioni:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-191">An existing Razor Pages or MVC app can integrate Razor components into pages and views:</span></span>

1. <span data-ttu-id="ed7bb-192">Nel file di layout dell'app ( *_Layout. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="ed7bb-192">In the app's layout file (*_Layout.cshtml*):</span></span>

   * <span data-ttu-id="ed7bb-193">Aggiungere il tag di `<base>` seguente all'elemento `<head>`:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-193">Add the following `<base>` tag to the `<head>` element:</span></span>

     ```html
     <base href="~/" />
     ```

     <span data-ttu-id="ed7bb-194">Il valore `href` (il *percorso di base dell'app*) nell'esempio precedente presuppone che l'app si trovi nel percorso URL radice (`/`).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-194">The `href` value (the *app base path*) in the preceding example assumes that the app resides at the root URL path (`/`).</span></span> <span data-ttu-id="ed7bb-195">Se l'app è un'applicazione secondaria, seguire le istruzioni nella sezione *percorso di base dell'app* dell'articolo <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-195">If the app is a sub-application, follow the guidance in the *App base path* section of the <xref:host-and-deploy/blazor/index#app-base-path> article.</span></span>

     <span data-ttu-id="ed7bb-196">Il file *_Layout. cshtml* si trova nella cartella *pages/Shared* in un'app Razor Pages o in una cartella *Views/Shared* in un'app MVC.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-196">The *_Layout.cshtml* file is located in the *Pages/Shared* folder in a Razor Pages app or *Views/Shared* folder in an MVC app.</span></span>

   * <span data-ttu-id="ed7bb-197">Aggiungere un tag `<script>` per lo script *blazer. Server. js* all'interno del tag di chiusura `</body>`:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-197">Add a `<script>` tag for the *blazor.server.js* script inside of the closing `</body>` tag:</span></span>

     ```html
     <script src="_framework/blazor.server.js"></script>
     ```

     <span data-ttu-id="ed7bb-198">Il Framework aggiunge lo script *blazer. Server. js* all'app.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-198">The framework adds the *blazor.server.js* script to the app.</span></span> <span data-ttu-id="ed7bb-199">Non è necessario aggiungere manualmente lo script all'app.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-199">There's no need to manually add the script to the app.</span></span>

1. <span data-ttu-id="ed7bb-200">Aggiungere un file *_Imports. Razor* alla cartella radice del progetto con il contenuto seguente (modificare l'ultimo spazio dei nomi, `MyAppNamespace`, nello spazio dei nomi dell'app):</span><span class="sxs-lookup"><span data-stu-id="ed7bb-200">Add an *_Imports.razor* file to the root folder of the project with the following content (change the last namespace, `MyAppNamespace`, to the namespace of the app):</span></span>

   ```csharp
   @using System.Net.Http
   @using Microsoft.AspNetCore.Authorization
   @using Microsoft.AspNetCore.Components.Authorization
   @using Microsoft.AspNetCore.Components.Forms
   @using Microsoft.AspNetCore.Components.Routing
   @using Microsoft.AspNetCore.Components.Web
   @using Microsoft.JSInterop
   @using MyAppNamespace
   ```

1. <span data-ttu-id="ed7bb-201">In `Startup.ConfigureServices`aggiungere il servizio Blazor server:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-201">In `Startup.ConfigureServices`, add the Blazor Server service:</span></span>

   ```csharp
   services.AddServerSideBlazor();
   ```

1. <span data-ttu-id="ed7bb-202">In `Startup.Configure`aggiungere l'endpoint dell'hub Blazor a `app.UseEndpoints`:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-202">In `Startup.Configure`, add the Blazor Hub endpoint to `app.UseEndpoints`:</span></span>

   ```csharp
   endpoints.MapBlazorHub();
   ```

1. <span data-ttu-id="ed7bb-203">Integrare i componenti in qualsiasi pagina o visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-203">Integrate components into any page or view.</span></span> <span data-ttu-id="ed7bb-204">Per ulteriori informazioni, vedere la sezione *integrare componenti in Razor Pages e app MVC* dell'articolo <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-204">For more information, see the *Integrate components into Razor Pages and MVC apps* section of the <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps> article.</span></span>

#### <a name="use-routable-components-in-a-razor-pages-app"></a><span data-ttu-id="ed7bb-205">Usare componenti instradabili in un'app Razor Pages</span><span class="sxs-lookup"><span data-stu-id="ed7bb-205">Use routable components in a Razor Pages app</span></span>

<span data-ttu-id="ed7bb-206">Per supportare componenti Razor instradabili nelle app Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-206">To support routable Razor components in Razor Pages apps:</span></span>

1. <span data-ttu-id="ed7bb-207">Seguire le istruzioni riportate nella sezione [usare i componenti in pagine e viste](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="ed7bb-207">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="ed7bb-208">Aggiungere un file *app. Razor* alla radice del progetto con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-208">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="ed7bb-209">Aggiungere un file *_Host. cshtml* alla cartella *pages* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-209">Add a *_Host.cshtml* file to the *Pages* folder with the following content:</span></span>

   ```cshtml
   @page "/blazor"
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="ed7bb-210">I componenti usano il file Shared *_Layout. cshtml* per il layout.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-210">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="ed7bb-211">Aggiungere una route con priorità bassa per la pagina *_Host. cshtml* alla configurazione dell'endpoint in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-211">Add a low-priority route for the *_Host.cshtml* page to endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToPage("/_Host");
   });
   ```

1. <span data-ttu-id="ed7bb-212">Aggiungere componenti instradabili all'app.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-212">Add routable components to the app.</span></span> <span data-ttu-id="ed7bb-213">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-213">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="ed7bb-214">Quando si usa una cartella personalizzata per archiviare i componenti dell'app, aggiungere lo spazio dei nomi che rappresenta la cartella al file *pages/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ed7bb-214">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Pages/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="ed7bb-215">Per ulteriori informazioni, vedere <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-215">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

#### <a name="use-routable-components-in-an-mvc-app"></a><span data-ttu-id="ed7bb-216">Usare componenti instradabili in un'app MVC</span><span class="sxs-lookup"><span data-stu-id="ed7bb-216">Use routable components in an MVC app</span></span>

<span data-ttu-id="ed7bb-217">Per supportare i componenti Razor instradabili nelle app MVC:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-217">To support routable Razor components in MVC apps:</span></span>

1. <span data-ttu-id="ed7bb-218">Seguire le istruzioni riportate nella sezione [usare i componenti in pagine e viste](#use-components-in-pages-and-views) .</span><span class="sxs-lookup"><span data-stu-id="ed7bb-218">Follow the guidance in the [Use components in pages and views](#use-components-in-pages-and-views) section.</span></span>

1. <span data-ttu-id="ed7bb-219">Aggiungere un file *app. Razor* alla radice del progetto con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-219">Add an *App.razor* file to the root of the project with the following content:</span></span>

   ```razor
   @using Microsoft.AspNetCore.Components.Routing

   <Router AppAssembly="typeof(Program).Assembly">
       <Found Context="routeData">
           <RouteView RouteData="routeData" />
       </Found>
       <NotFound>
           <h1>Page not found</h1>
           <p>Sorry, but there's nothing here!</p>
       </NotFound>
   </Router>
   ```

1. <span data-ttu-id="ed7bb-220">Aggiungere un file *_Host. cshtml* alla cartella *views/Home* con il contenuto seguente:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-220">Add a *_Host.cshtml* file to the *Views/Home* folder with the following content:</span></span>

   ```cshtml
   @{
       Layout = "_Layout";
   }

   <app>
       <component type="typeof(App)" render-mode="ServerPrerendered" />
   </app>
   ```

   <span data-ttu-id="ed7bb-221">I componenti usano il file Shared *_Layout. cshtml* per il layout.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-221">Components use the shared *_Layout.cshtml* file for their layout.</span></span>

1. <span data-ttu-id="ed7bb-222">Aggiungere un'azione al controller Home:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-222">Add an action to the Home controller:</span></span>

   ```csharp
   public IActionResult Blazor()
   {
      return View("_Host");
   }
   ```

1. <span data-ttu-id="ed7bb-223">Aggiungere una route con priorità bassa per l'azione del controller che restituisce la vista *_Host. cshtml* alla configurazione dell'endpoint in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-223">Add a low-priority route for the controller action that returns the *_Host.cshtml* view to the endpoint configuration in `Startup.Configure`:</span></span>

   ```csharp
   app.UseEndpoints(endpoints =>
   {
       ...

       endpoints.MapFallbackToController("Blazor", "Home");
   });
   ```

1. <span data-ttu-id="ed7bb-224">Creare una cartella *pages* e aggiungere componenti instradabili all'app.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-224">Create a *Pages* folder and add routable components to the app.</span></span> <span data-ttu-id="ed7bb-225">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-225">For example:</span></span>

   ```razor
   @page "/counter"

   <h1>Counter</h1>

   ...
   ```

   <span data-ttu-id="ed7bb-226">Quando si usa una cartella personalizzata per archiviare i componenti dell'app, aggiungere lo spazio dei nomi che rappresenta la cartella al file *views/_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ed7bb-226">When using a custom folder to hold the app's components, add the namespace representing the folder to the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="ed7bb-227">Per ulteriori informazioni, vedere <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-227">For more information, see <xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps>.</span></span>

### <a name="circuits"></a><span data-ttu-id="ed7bb-228">Circuiti</span><span class="sxs-lookup"><span data-stu-id="ed7bb-228">Circuits</span></span>

<span data-ttu-id="ed7bb-229">Un'app Server Blazor è basata sull' [SignalRASP.NET Core ](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-229">A Blazor Server app is built on top of [ASP.NET Core SignalR](xref:signalr/introduction).</span></span> <span data-ttu-id="ed7bb-230">Ogni client comunica al server tramite una o più connessioni di SignalR chiamate *circuito*.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-230">Each client communicates to the server over one or more SignalR connections called a *circuit*.</span></span> <span data-ttu-id="ed7bb-231">Un circuito viene Blazorastrazione su SignalR connessioni che possono tollerare interruzioni di rete temporanee.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-231">A circuit is Blazor's abstraction over SignalR connections that can tolerate temporary network interruptions.</span></span> <span data-ttu-id="ed7bb-232">Quando un client Blazor rileva che la connessione SignalR è disconnessa, tenta di riconnettersi al server utilizzando una nuova connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-232">When a Blazor client sees that the SignalR connection is disconnected, it attempts to reconnect to the server using a new SignalR connection.</span></span>

<span data-ttu-id="ed7bb-233">Ogni schermata del browser (scheda del browser o iframe) connessa a un'app del server Blazor usa una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-233">Each browser screen (browser tab or iframe) that is connected to a Blazor Server app uses a SignalR connection.</span></span> <span data-ttu-id="ed7bb-234">Si tratta ancora di un'altra distinzione importante rispetto alle app tipiche sottoposte a rendering server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-234">This is yet another important distinction compared to typical server-rendered apps.</span></span> <span data-ttu-id="ed7bb-235">In un'app sottoposta a rendering server, l'apertura della stessa app in più schermate del browser in genere non viene convertita in richieste di risorse aggiuntive sul server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-235">In a server-rendered app, opening the same app in multiple browser screens typically doesn't translate into additional resource demands on the server.</span></span> <span data-ttu-id="ed7bb-236">In un'app Server Blazor, ogni schermata del browser richiede un circuito separato e istanze separate dello stato dei componenti che devono essere gestite dal server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-236">In a Blazor Server app, each browser screen requires a separate circuit and separate instances of component state to be managed by the server.</span></span>

Blazor<span data-ttu-id="ed7bb-237"> considera la chiusura di una scheda del browser o l'esplorazione di un URL esterno a una chiusura *normale* .</span><span class="sxs-lookup"><span data-stu-id="ed7bb-237"> considers closing a browser tab or navigating to an external URL a *graceful* termination.</span></span> <span data-ttu-id="ed7bb-238">In caso di chiusura normale, il circuito e le risorse associate vengono immediatamente rilasciati.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-238">In the event of a graceful termination, the circuit and associated resources are immediately released.</span></span> <span data-ttu-id="ed7bb-239">Un client può anche disconnettersi in modo non corretto, ad esempio a causa di un'interruzione della rete.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-239">A client may also disconnect non-gracefully, for instance due to a network interruption.</span></span> Blazor<span data-ttu-id="ed7bb-240"> Server archivia circuiti disconnessi per un intervallo configurabile in modo da consentire la riconnessione del client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-240"> Server stores disconnected circuits for a configurable interval to allow the client to reconnect.</span></span> <span data-ttu-id="ed7bb-241">Per ulteriori informazioni, vedere la sezione [riconnessione allo stesso server](#reconnection-to-the-same-server) .</span><span class="sxs-lookup"><span data-stu-id="ed7bb-241">For more information, see the [Reconnection to the same server](#reconnection-to-the-same-server) section.</span></span>

### <a name="ui-latency"></a><span data-ttu-id="ed7bb-242">Latenza interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="ed7bb-242">UI Latency</span></span>

<span data-ttu-id="ed7bb-243">La latenza dell'interfaccia utente è il tempo necessario da un'azione iniziata al momento dell'aggiornamento dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-243">UI latency is the time it takes from an initiated action to the time the UI is updated.</span></span> <span data-ttu-id="ed7bb-244">I valori più bassi per la latenza dell'interfaccia utente sono imperativi perché un'app risponda a un utente.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-244">Smaller values for UI latency are imperative for an app to feel responsive to a user.</span></span> <span data-ttu-id="ed7bb-245">In un'app Server Blazor, ogni azione viene inviata al server, elaborata e viene restituita una diff dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-245">In a Blazor Server app, each action is sent to the server, processed, and a UI diff is sent back.</span></span> <span data-ttu-id="ed7bb-246">Di conseguenza, la latenza dell'interfaccia utente è la somma della latenza di rete e la latenza del server nell'elaborazione dell'azione.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-246">Consequently, UI latency is the sum of network latency and the server latency in processing the action.</span></span>

<span data-ttu-id="ed7bb-247">Per un'app line-of-business limitata a una rete aziendale privata, l'effetto sulle percezioni utente della latenza a causa della latenza di rete è in genere impercettibile.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-247">For a line of business app that's limited to a private corporate network, the effect on user perceptions of latency due to network latency are usually imperceptible.</span></span> <span data-ttu-id="ed7bb-248">Per un'app distribuita su Internet, la latenza può diventare evidente per gli utenti, in particolare se gli utenti sono ampiamente distribuiti geograficamente.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-248">For an app deployed over the Internet, latency may become noticeable to users, particularly if users are widely distributed geographically.</span></span>

<span data-ttu-id="ed7bb-249">L'utilizzo della memoria può anche contribuire alla latenza dell'app.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-249">Memory usage can also contribute to app latency.</span></span> <span data-ttu-id="ed7bb-250">Un aumento dell'utilizzo della memoria comporta un frequente Garbage Collection o il paging della memoria su disco, che comportano una riduzione delle prestazioni dell'app e quindi aumentano la latenza dell'interfaccia</span><span class="sxs-lookup"><span data-stu-id="ed7bb-250">Increased memory usage results in frequent garbage collection or paging memory to disk, both of which degrade app performance and consequently increase UI latency.</span></span> <span data-ttu-id="ed7bb-251">Per ulteriori informazioni, vedere <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-251">For more information, see <xref:security/blazor/server>.</span></span>

Blazor<span data-ttu-id="ed7bb-252"> le app Server devono essere ottimizzate per ridurre al minimo la latenza dell'interfaccia utente riducendo la latenza di rete e l'utilizzo</span><span class="sxs-lookup"><span data-stu-id="ed7bb-252"> Server apps should be optimized to minimize UI latency by reducing network latency and memory usage.</span></span> <span data-ttu-id="ed7bb-253">Per un approccio alla misurazione della latenza di rete, vedere <xref:host-and-deploy/blazor/server#measure-network-latency>.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-253">For an approach to measuring network latency, see <xref:host-and-deploy/blazor/server#measure-network-latency>.</span></span> <span data-ttu-id="ed7bb-254">Per ulteriori informazioni su SignalR e Blazor, vedere:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-254">For more information on SignalR and Blazor, see:</span></span>

* <xref:host-and-deploy/blazor/server>
* <xref:security/blazor/server>

### <a name="connection-to-the-server"></a><span data-ttu-id="ed7bb-255">Connessione al server</span><span class="sxs-lookup"><span data-stu-id="ed7bb-255">Connection to the server</span></span>

Blazor<span data-ttu-id="ed7bb-256"> app Server richiedono una connessione SignalR attiva al server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-256"> Server apps require an active SignalR connection to the server.</span></span> <span data-ttu-id="ed7bb-257">Se la connessione viene persa, l'app tenta di riconnettersi al server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-257">If the connection is lost, the app attempts to reconnect to the server.</span></span> <span data-ttu-id="ed7bb-258">Fino a quando lo stato del client è ancora in memoria, la sessione client riprende senza perdere lo stato.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-258">As long as the client's state is still in memory, the client session resumes without losing state.</span></span>

#### <a name="reconnection-to-the-same-server"></a><span data-ttu-id="ed7bb-259">Riconnessione allo stesso server</span><span class="sxs-lookup"><span data-stu-id="ed7bb-259">Reconnection to the same server</span></span>

<span data-ttu-id="ed7bb-260">Viene eseguito il prerendering di un'app Blazor server in risposta alla prima richiesta client, che imposta lo stato dell'interfaccia utente sul server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-260">A Blazor Server app prerenders in response to the first client request, which sets up the UI state on the server.</span></span> <span data-ttu-id="ed7bb-261">Quando il client tenta di creare una connessione SignalR, il client deve riconnettersi allo stesso server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-261">When the client attempts to create a SignalR connection, the client must reconnect to the same server.</span></span> Blazor<span data-ttu-id="ed7bb-262"> app server che usano più di un server back-end devono implementare *sessioni permanenti* per le connessioni SignalR.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-262"> Server apps that use more than one backend server should implement *sticky sessions* for SignalR connections.</span></span>

<span data-ttu-id="ed7bb-263">È consigliabile usare il [servizio SignalR di Azure](/azure/azure-signalr) per le app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-263">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="ed7bb-264">Il servizio consente di aumentare le prestazioni di un'app Server Blazor per un numero elevato di connessioni SignalR simultanee.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-264">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="ed7bb-265">Le sessioni permanenti sono abilitate per il servizio Azure SignalR impostando l'opzione di `ServerStickyMode` del servizio o il valore di configurazione su `Required`.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-265">Sticky sessions are enabled for the Azure SignalR Service by setting the service's `ServerStickyMode` option or configuration value to `Required`.</span></span> <span data-ttu-id="ed7bb-266">Per ulteriori informazioni, vedere <xref:host-and-deploy/blazor/server#signalr-configuration>.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-266">For more information, see <xref:host-and-deploy/blazor/server#signalr-configuration>.</span></span>

<span data-ttu-id="ed7bb-267">Quando si usa IIS, le sessioni permanenti sono abilitate con il routing delle richieste di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-267">When using IIS, sticky sessions are enabled with Application Request Routing.</span></span> <span data-ttu-id="ed7bb-268">Per altre informazioni, vedere [bilanciamento del carico http usando il routing delle richieste di applicazioni](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span><span class="sxs-lookup"><span data-stu-id="ed7bb-268">For more information, see [HTTP Load Balancing using Application Request Routing](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing).</span></span>

#### <a name="reflect-the-connection-state-in-the-ui"></a><span data-ttu-id="ed7bb-269">Riflette lo stato di connessione nell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="ed7bb-269">Reflect the connection state in the UI</span></span>

<span data-ttu-id="ed7bb-270">Quando il client rileva che la connessione è stata persa, viene visualizzata un'interfaccia utente predefinita quando il client tenta di riconnettersi.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-270">When the client detects that the connection has been lost, a default UI is displayed to the user while the client attempts to reconnect.</span></span> <span data-ttu-id="ed7bb-271">Se la riconnessione non riesce, all'utente viene offerta l'opzione per riprovare.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-271">If reconnection fails, the user is provided the option to retry.</span></span>

<span data-ttu-id="ed7bb-272">Per personalizzare l'interfaccia utente, definire un elemento con un `id` di `components-reconnect-modal` nel `<body>` della pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="ed7bb-272">To customize the UI, define an element with an `id` of `components-reconnect-modal` in the `<body>` of the *_Host.cshtml* Razor page:</span></span>

```cshtml
<div id="components-reconnect-modal">
    ...
</div>
```

<span data-ttu-id="ed7bb-273">Nella tabella seguente vengono descritte le classi CSS applicate all'elemento `components-reconnect-modal`.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-273">The following table describes the CSS classes applied to the `components-reconnect-modal` element.</span></span>

| <span data-ttu-id="ed7bb-274">Classe CSS</span><span class="sxs-lookup"><span data-stu-id="ed7bb-274">CSS class</span></span>                       | <span data-ttu-id="ed7bb-275">Indica&hellip;</span><span class="sxs-lookup"><span data-stu-id="ed7bb-275">Indicates&hellip;</span></span> |
| ------------------------------- | ------------------------- |
| `components-reconnect-show`     | <span data-ttu-id="ed7bb-276">Connessione persa.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-276">A lost connection.</span></span> <span data-ttu-id="ed7bb-277">È in corso un tentativo di riconnessione del client.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-277">The client is attempting to reconnect.</span></span> <span data-ttu-id="ed7bb-278">Mostra il modale.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-278">Show the modal.</span></span> |
| `components-reconnect-hide`     | <span data-ttu-id="ed7bb-279">Viene ristabilita una connessione attiva al server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-279">An active connection is re-established to the server.</span></span> <span data-ttu-id="ed7bb-280">Nascondere il modale.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-280">Hide the modal.</span></span> |
| `components-reconnect-failed`   | <span data-ttu-id="ed7bb-281">Riconnessione non riuscita. probabilmente a causa di un errore di rete.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-281">Reconnection failed, probably due to a network failure.</span></span> <span data-ttu-id="ed7bb-282">Per tentare la riconnessione, chiamare `window.Blazor.reconnect()`.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-282">To attempt reconnection, call `window.Blazor.reconnect()`.</span></span> |
| `components-reconnect-rejected` | <span data-ttu-id="ed7bb-283">Riconnessione rifiutata.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-283">Reconnection rejected.</span></span> <span data-ttu-id="ed7bb-284">Il server è stato raggiunto ma ha rifiutato la connessione e lo stato dell'utente nel server è andato perso.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-284">The server was reached but refused the connection, and the user's state on the server is lost.</span></span> <span data-ttu-id="ed7bb-285">Per ricaricare l'app, chiamare `location.reload()`.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-285">To reload the app, call `location.reload()`.</span></span> <span data-ttu-id="ed7bb-286">Questo stato di connessione può verificarsi nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-286">This connection state may result when:</span></span><ul><li><span data-ttu-id="ed7bb-287">Si verifica un arresto anomalo del circuito sul lato server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-287">A crash in the server-side circuit occurs.</span></span></li><li><span data-ttu-id="ed7bb-288">Il client viene disconnesso abbastanza a lungo da consentire al server di eliminare lo stato dell'utente.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-288">The client is disconnected long enough for the server to drop the user's state.</span></span> <span data-ttu-id="ed7bb-289">Le istanze dei componenti con cui l'utente interagisce sono state eliminate.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-289">Instances of the components that the user is interacting with are disposed.</span></span></li><li><span data-ttu-id="ed7bb-290">Il server viene riavviato oppure il processo di lavoro dell'app viene riciclato.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-290">The server is restarted, or the app's worker process is recycled.</span></span></li></ul> |

### <a name="stateful-reconnection-after-prerendering"></a><span data-ttu-id="ed7bb-291">Riconnessione con stato dopo il rendering preliminare</span><span class="sxs-lookup"><span data-stu-id="ed7bb-291">Stateful reconnection after prerendering</span></span>

<span data-ttu-id="ed7bb-292">per impostazione predefinita, le app di Blazor server sono impostate per eseguire il prerendering dell'interfaccia utente nel server prima che venga stabilita la connessione client al server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-292">Blazor Server apps are set up by default to prerender the UI on the server before the client connection to the server is established.</span></span> <span data-ttu-id="ed7bb-293">Questa impostazione è configurata nella pagina Razor *_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="ed7bb-293">This is set up in the *_Host.cshtml* Razor page:</span></span>

```cshtml
<body>
    <app>
      <component type="typeof(App)" render-mode="ServerPrerendered" />
    </app>

    <script src="_framework/blazor.server.js"></script>
</body>
```

<span data-ttu-id="ed7bb-294">`RenderMode` configura se il componente:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-294">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="ed7bb-295">Viene preeseguito nella pagina.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-295">Is prerendered into the page.</span></span>
* <span data-ttu-id="ed7bb-296">Viene sottoposto a rendering come HTML statico nella pagina o se include le informazioni necessarie per avviare un'app Blazor dall'agente utente.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-296">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="ed7bb-297">Descrizione</span><span class="sxs-lookup"><span data-stu-id="ed7bb-297">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="ed7bb-298">Esegue il rendering del componente in HTML statico e include un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-298">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="ed7bb-299">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-299">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="ed7bb-300">Esegue il rendering di un marcatore per un'app Server Blazor.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-300">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="ed7bb-301">L'output del componente non è incluso.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-301">Output from the component isn't included.</span></span> <span data-ttu-id="ed7bb-302">Quando l'agente utente viene avviato, questo marcatore viene usato per eseguire il bootstrap di un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-302">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="ed7bb-303">Esegue il rendering del componente in HTML statico.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-303">Renders the component into static HTML.</span></span> |

<span data-ttu-id="ed7bb-304">Il rendering dei componenti server da una pagina HTML statica non è supportato.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-304">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="ed7bb-305">Quando `RenderMode` viene `ServerPrerendered`, il componente viene inizialmente sottoposto a rendering statico come parte della pagina.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-305">When `RenderMode` is `ServerPrerendered`, the component is initially rendered statically as part of the page.</span></span> <span data-ttu-id="ed7bb-306">Quando il browser stabilisce una connessione al server, viene *nuovamente*eseguito il rendering del componente e il componente è ora interattivo.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-306">Once the browser establishes a connection back to the server, the component is rendered *again*, and the component is now interactive.</span></span> <span data-ttu-id="ed7bb-307">Se è presente il metodo [{Async} Lifecycle OnInitialized](xref:blazor/lifecycle#component-initialization-methods) per l'inizializzazione del componente, il metodo viene eseguito *due volte*:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-307">If the [OnInitialized{Async}](xref:blazor/lifecycle#component-initialization-methods) lifecycle method for initializing the component is present, the method is executed *twice*:</span></span>

* <span data-ttu-id="ed7bb-308">Quando il componente viene preeseguito in modo statico.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-308">When the component is prerendered statically.</span></span>
* <span data-ttu-id="ed7bb-309">Una volta stabilita la connessione al server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-309">After the server connection has been established.</span></span>

<span data-ttu-id="ed7bb-310">Ciò può comportare una modifica evidente nei dati visualizzati nell'interfaccia utente quando il componente viene sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-310">This can result in a noticeable change in the data displayed in the UI when the component is finally rendered.</span></span>

<span data-ttu-id="ed7bb-311">Per evitare lo scenario di doppio rendering in un'app Server Blazor:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-311">To avoid the double-rendering scenario in a Blazor Server app:</span></span>

* <span data-ttu-id="ed7bb-312">Passare un identificatore che può essere usato per memorizzare nella cache lo stato durante il prerendering e recuperare lo stato dopo il riavvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-312">Pass in an identifier that can be used to cache the state during prerendering and to retrieve the state after the app restarts.</span></span>
* <span data-ttu-id="ed7bb-313">Utilizzare l'identificatore durante il prerendering per salvare lo stato del componente.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-313">Use the identifier during prerendering to save component state.</span></span>
* <span data-ttu-id="ed7bb-314">Utilizzare l'identificatore dopo il prerendering per recuperare lo stato memorizzato nella cache.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-314">Use the identifier after prerendering to retrieve the cached state.</span></span>

<span data-ttu-id="ed7bb-315">Il codice seguente illustra un `WeatherForecastService` aggiornato in un'app Server Blazor basata su modelli che evita il doppio rendering:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-315">The following code demonstrates an updated `WeatherForecastService` in a template-based Blazor Server app that avoids the double rendering:</span></span>

```csharp
public class WeatherForecastService
{
    private static readonly string[] _summaries = new[]
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
                Summary = _summaries[rng.Next(_summaries.Length)]
            }).ToArray();
        });
    }
}
```

### <a name="render-stateful-interactive-components-from-razor-pages-and-views"></a><span data-ttu-id="ed7bb-316">Eseguire il rendering di componenti interattivi con stato da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="ed7bb-316">Render stateful interactive components from Razor pages and views</span></span>

<span data-ttu-id="ed7bb-317">I componenti interattivi con stato possono essere aggiunti a una pagina o a una visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-317">Stateful interactive components can be added to a Razor page or view.</span></span>

<span data-ttu-id="ed7bb-318">Quando viene eseguito il rendering della pagina o della visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-318">When the page or view renders:</span></span>

* <span data-ttu-id="ed7bb-319">Il componente viene preeseguito con la pagina o la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-319">The component is prerendered with the page or view.</span></span>
* <span data-ttu-id="ed7bb-320">Lo stato iniziale del componente utilizzato per il prerendering viene perso.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-320">The initial component state used for prerendering is lost.</span></span>
* <span data-ttu-id="ed7bb-321">Quando viene stabilita la connessione SignalR viene creato il nuovo stato del componente.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-321">New component state is created when the SignalR connection is established.</span></span>

<span data-ttu-id="ed7bb-322">La pagina Razor seguente esegue il rendering di un componente `Counter`:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-322">The following Razor page renders a `Counter` component:</span></span>

```cshtml
<h1>My Razor Page</h1>

<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-InitialValue="InitialValue" />

@code {
    [BindProperty(SupportsGet=true)]
    public int InitialValue { get; set; }
}
```

### <a name="render-noninteractive-components-from-razor-pages-and-views"></a><span data-ttu-id="ed7bb-323">Eseguire il rendering di componenti non interattivi da pagine e visualizzazioni Razor</span><span class="sxs-lookup"><span data-stu-id="ed7bb-323">Render noninteractive components from Razor pages and views</span></span>

<span data-ttu-id="ed7bb-324">Nella pagina Razor seguente il componente `Counter` viene sottoposto a rendering statico con un valore iniziale specificato utilizzando un form:</span><span class="sxs-lookup"><span data-stu-id="ed7bb-324">In the following Razor page, the `Counter` component is statically rendered with an initial value that's specified using a form:</span></span>

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

<span data-ttu-id="ed7bb-325">Poiché `MyComponent` viene sottoposto a rendering statico, il componente non può essere interattivo.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-325">Since `MyComponent` is statically rendered, the component can't be interactive.</span></span>

### <a name="detect-when-the-app-is-prerendering"></a><span data-ttu-id="ed7bb-326">Rilevare il momento in cui viene eseguito il prerendering dell'app</span><span class="sxs-lookup"><span data-stu-id="ed7bb-326">Detect when the app is prerendering</span></span>

[!INCLUDE[](~/includes/blazor-prerendering.md)]

### <a name="configure-the-opno-locsignalr-client-for-opno-locblazor-server-apps"></a><span data-ttu-id="ed7bb-327">Configurare il client di SignalR per le app Blazor server</span><span class="sxs-lookup"><span data-stu-id="ed7bb-327">Configure the SignalR client for Blazor Server apps</span></span>

<span data-ttu-id="ed7bb-328">In alcuni casi, è necessario configurare il client SignalR usato dalle app Blazor server.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-328">Sometimes, you need to configure the SignalR client used by Blazor Server apps.</span></span> <span data-ttu-id="ed7bb-329">È ad esempio possibile configurare la registrazione nel client di SignalR per diagnosticare un problema di connessione.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-329">For example, you might want to configure logging on the SignalR client to diagnose a connection issue.</span></span>

<span data-ttu-id="ed7bb-330">Per configurare il client di SignalR nel file *pages/_Host. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="ed7bb-330">To configure the SignalR client in the *Pages/_Host.cshtml* file:</span></span>

* <span data-ttu-id="ed7bb-331">Aggiungere un attributo `autostart="false"` al tag `<script>` per lo script `blazor.server.js`.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-331">Add an `autostart="false"` attribute to the `<script>` tag for the `blazor.server.js` script.</span></span>
* <span data-ttu-id="ed7bb-332">Chiamare `Blazor.start` e passare un oggetto di configurazione che specifichi il generatore di SignalR.</span><span class="sxs-lookup"><span data-stu-id="ed7bb-332">Call `Blazor.start` and pass in a configuration object that specifies the SignalR builder.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="ed7bb-333">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ed7bb-333">Additional resources</span></span>

* <xref:blazor/get-started>
* <xref:signalr/introduction>
