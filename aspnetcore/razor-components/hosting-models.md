---
title: Componenti di Razor modelli di hosting
author: guardrex
description: Comprendere Blazor lato client e lato server ASP.NET Core Razor componenti modelli di hosting.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: razor-components/hosting-models
ms.openlocfilehash: 8ed70117c94bf1a3e4c208f70310bbf0473bae44
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515527"
---
# <a name="razor-components-hosting-models"></a><span data-ttu-id="12cd7-103">Componenti di Razor modelli di hosting</span><span class="sxs-lookup"><span data-stu-id="12cd7-103">Razor Components hosting models</span></span>

<span data-ttu-id="12cd7-104">Da [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="12cd7-104">By [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="12cd7-105">Componenti Razor è un framework web progettato per l'esecuzione del client nel browser in un [WebAssembly](http://webassembly.org/)-basati su runtime di .NET (*Blazor*) o sul lato server in ASP.NET Core (*Razor di ASP.NET Core Componenti*).</span><span class="sxs-lookup"><span data-stu-id="12cd7-105">Razor Components is a web framework designed to run client-side in the browser on a [WebAssembly](http://webassembly.org/)-based .NET runtime (*Blazor*) or server-side in ASP.NET Core (*ASP.NET Core Razor Components*).</span></span> <span data-ttu-id="12cd7-106">Indipendentemente dal fatto i modelli di modello, l'app e il componente di hosting *rimangono invariate*.</span><span class="sxs-lookup"><span data-stu-id="12cd7-106">Regardless of the hosting model, the app and component models *remain the same*.</span></span> <span data-ttu-id="12cd7-107">Questo articolo illustra i modelli di hosting disponibili:</span><span class="sxs-lookup"><span data-stu-id="12cd7-107">This article discusses the available hosting models:</span></span>

* [<span data-ttu-id="12cd7-108">Blazor sul lato client</span><span class="sxs-lookup"><span data-stu-id="12cd7-108">Client-side Blazor</span></span>](#client-side-hosting-model)
* [<span data-ttu-id="12cd7-109">Razor Components ASP.NET Core sul lato server</span><span class="sxs-lookup"><span data-stu-id="12cd7-109">Server-side ASP.NET Core Razor Components</span></span>](#server-side-hosting-model)

## <a name="client-side-hosting-model"></a><span data-ttu-id="12cd7-110">Modello di hosting sul lato client</span><span class="sxs-lookup"><span data-stu-id="12cd7-110">Client-side hosting model</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="12cd7-111">Modello di hosting dell'entità per Blazor è sul lato client in esecuzione nel browser sul WebAssembly.</span><span class="sxs-lookup"><span data-stu-id="12cd7-111">The principal hosting model for Blazor is running client-side in the browser on WebAssembly.</span></span> <span data-ttu-id="12cd7-112">L'app Blazor, le relative dipendenze e il runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="12cd7-112">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser.</span></span> <span data-ttu-id="12cd7-113">L'app viene eseguita direttamente nel thread dell'interfaccia utente del browser.</span><span class="sxs-lookup"><span data-stu-id="12cd7-113">The app is executed directly on the browser UI thread.</span></span> <span data-ttu-id="12cd7-114">Aggiornamenti dell'interfaccia utente e la gestione degli eventi si verificano all'interno dello stesso processo.</span><span class="sxs-lookup"><span data-stu-id="12cd7-114">UI updates and event handling occur within the same process.</span></span> <span data-ttu-id="12cd7-115">Gli asset dell'app vengono distribuiti come file statici da un server web o servizio in grado di servire contenuti statici per i client.</span><span class="sxs-lookup"><span data-stu-id="12cd7-115">The app's assets are deployed as static files to a web server or service capable of serving static content to clients.</span></span>

![Blazor lato client: L'app Blazor viene eseguita su un thread dell'interfaccia utente all'interno del browser.](hosting-models/_static/client-side.png)

<span data-ttu-id="12cd7-117">Per creare un'app Blazor usando il modello di hosting lato client, utilizzare uno dei modelli seguenti:</span><span class="sxs-lookup"><span data-stu-id="12cd7-117">To create a Blazor app using the client-side hosting model, use either of the following templates:</span></span>

* <span data-ttu-id="12cd7-118">**Blazor** ([blazor nuovo dotnet](/dotnet/core/tools/dotnet-new)) &ndash; distribuito come un set di file statici.</span><span class="sxs-lookup"><span data-stu-id="12cd7-118">**Blazor** ([dotnet new blazor](/dotnet/core/tools/dotnet-new)) &ndash; Deployed as a set of static files.</span></span>
* <span data-ttu-id="12cd7-119">**(ASP.NET Core ospitate) Blazor** ([blazorhosted nuovo dotnet](/dotnet/core/tools/dotnet-new)) &ndash; ospitate da un server di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12cd7-119">**Blazor (ASP.NET Core Hosted)** ([dotnet new blazorhosted](/dotnet/core/tools/dotnet-new)) &ndash; Hosted by an ASP.NET Core server.</span></span> <span data-ttu-id="12cd7-120">L'app ASP.NET Core viene usato l'app Blazor ai client.</span><span class="sxs-lookup"><span data-stu-id="12cd7-120">The ASP.NET Core app serves the Blazor app to clients.</span></span> <span data-ttu-id="12cd7-121">L'app Blazor lato client può interagire con il server in rete con chiamate all'API web o [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="12cd7-121">The client-side Blazor app can interact with the server over the network using web API calls or [SignalR](xref:signalr/introduction).</span></span>

<span data-ttu-id="12cd7-122">I modelli includono le *components.webassembly.js* script che gestisce:</span><span class="sxs-lookup"><span data-stu-id="12cd7-122">The templates include the *components.webassembly.js* script that handles:</span></span>

* <span data-ttu-id="12cd7-123">Scaricare il runtime di .NET, l'app e le dipendenze dell'app.</span><span class="sxs-lookup"><span data-stu-id="12cd7-123">Downloading the .NET runtime, the app, and the app's dependencies.</span></span>
* <span data-ttu-id="12cd7-124">Inizializzazione del runtime per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="12cd7-124">Initialization of the runtime to run the app.</span></span>

<span data-ttu-id="12cd7-125">Il modello di hosting lato client offre diversi vantaggi.</span><span class="sxs-lookup"><span data-stu-id="12cd7-125">The client-side hosting model offers several benefits.</span></span> <span data-ttu-id="12cd7-126">Blazor lato client:</span><span class="sxs-lookup"><span data-stu-id="12cd7-126">Client-side Blazor:</span></span>

* <span data-ttu-id="12cd7-127">Non dispone di alcuna dipendenza lato server .NET.</span><span class="sxs-lookup"><span data-stu-id="12cd7-127">Has no .NET server-side dependency.</span></span>
* <span data-ttu-id="12cd7-128">Utilizza le funzionalità e le risorse del client.</span><span class="sxs-lookup"><span data-stu-id="12cd7-128">Fully leverages client resources and capabilities.</span></span>
* <span data-ttu-id="12cd7-129">Gli Offload di lavoro dal server al client.</span><span class="sxs-lookup"><span data-stu-id="12cd7-129">Offloads work from the server to the client.</span></span>
* <span data-ttu-id="12cd7-130">Supporta scenari non in linea.</span><span class="sxs-lookup"><span data-stu-id="12cd7-130">Supports offline scenarios.</span></span>

<span data-ttu-id="12cd7-131">Degli svantaggi all'hosting lato client.</span><span class="sxs-lookup"><span data-stu-id="12cd7-131">There are downsides to client-side hosting.</span></span> <span data-ttu-id="12cd7-132">Blazor lato client:</span><span class="sxs-lookup"><span data-stu-id="12cd7-132">Client-side Blazor:</span></span>

* <span data-ttu-id="12cd7-133">Limita l'app per le funzionalità del browser.</span><span class="sxs-lookup"><span data-stu-id="12cd7-133">Restricts the app to the capabilities of the browser.</span></span>
* <span data-ttu-id="12cd7-134">Richiede un client in grado di supportare hardware e software (ad esempio, WebAssembly supporto).</span><span class="sxs-lookup"><span data-stu-id="12cd7-134">Requires capable client hardware and software (for example, WebAssembly support).</span></span>
* <span data-ttu-id="12cd7-135">Ha una maggiore dimensione del download e l'app più il tempo di caricamento.</span><span class="sxs-lookup"><span data-stu-id="12cd7-135">Has a larger download size and longer app load time.</span></span>
* <span data-ttu-id="12cd7-136">È inferiore a maturare runtime .NET e supporto degli strumenti (ad esempio, le limitazioni nel [.NET Standard](/dotnet/standard/net-standard) supporto e il debug).</span><span class="sxs-lookup"><span data-stu-id="12cd7-136">Has less mature .NET runtime and tooling support (for example, limitations in [.NET Standard](/dotnet/standard/net-standard) support and debugging).</span></span>

## <a name="server-side-hosting-model"></a><span data-ttu-id="12cd7-137">Modello di hosting sul lato server</span><span class="sxs-lookup"><span data-stu-id="12cd7-137">Server-side hosting model</span></span>

<span data-ttu-id="12cd7-138">Con il modello di hosting ASP.NET Core Razor componenti lato server, l'app viene eseguita nel server da all'interno di un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12cd7-138">With the ASP.NET Core Razor Components server-side hosting model, the app is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="12cd7-139">Aggiornamenti dell'interfaccia utente, la gestione degli eventi e le chiamate JavaScript vengono gestite tramite un [SignalR](xref:signalr/introduction) connessione.</span><span class="sxs-lookup"><span data-stu-id="12cd7-139">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

![ASP.NET Core Razor componenti lato server: Il browser interagisce con l'app (ospitata all'interno di un'app ASP.NET Core) sul server tramite una connessione SignalR.](hosting-models/_static/server-side.png)

<span data-ttu-id="12cd7-141">Per creare un'app Razor componenti usando il modello di hosting sul lato server, usare ASP.NET Core **componenti Razor** modello ([dotnet nuovo razorcomponents](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="12cd7-141">To create a Razor Components app using the server-side hosting model, use the ASP.NET Core **Razor Components** template ([dotnet new razorcomponents](/dotnet/core/tools/dotnet-new)).</span></span> <span data-ttu-id="12cd7-142">L'app ASP.NET Core ospita l'app sul lato server per i componenti di Razor e configura l'endpoint di SignalR in cui i client si connettono.</span><span class="sxs-lookup"><span data-stu-id="12cd7-142">The ASP.NET Core app hosts the Razor Components server-side app and sets up the SignalR endpoint where clients connect.</span></span> <span data-ttu-id="12cd7-143">L'app ASP.NET Core fa riferimento l'app `Startup` classe da aggiungere:</span><span class="sxs-lookup"><span data-stu-id="12cd7-143">The ASP.NET Core app references the app's `Startup` class to add:</span></span>

* <span data-ttu-id="12cd7-144">Servizi di Razor componenti lato server.</span><span class="sxs-lookup"><span data-stu-id="12cd7-144">Server-side Razor Components services.</span></span>
* <span data-ttu-id="12cd7-145">L'app per la gestione della pipeline delle richieste.</span><span class="sxs-lookup"><span data-stu-id="12cd7-145">The app to the request handling pipeline.</span></span>

[!code-csharp[](hosting-models/samples_snapshot/Startup.cs?highlight=5,27)]

<span data-ttu-id="12cd7-146">Il *components.server.js* script&dagger; stabilisce la connessione client.</span><span class="sxs-lookup"><span data-stu-id="12cd7-146">The *components.server.js* script&dagger; establishes the client connection.</span></span> <span data-ttu-id="12cd7-147">È responsabilità dell'app per mantenere e ripristinare lo stato di app in base alle esigenze (ad esempio, in caso di una connessione di rete persa).</span><span class="sxs-lookup"><span data-stu-id="12cd7-147">It's the app's responsibility to persist and restore app state as required (for example, in the event of a lost network connection).</span></span>

<span data-ttu-id="12cd7-148">Il modello di hosting di server-side offre diversi vantaggi.</span><span class="sxs-lookup"><span data-stu-id="12cd7-148">The server-side hosting model offers several benefits.</span></span> <span data-ttu-id="12cd7-149">Componenti lato server Razor:</span><span class="sxs-lookup"><span data-stu-id="12cd7-149">Server-side Razor Components:</span></span>

* <span data-ttu-id="12cd7-150">Hanno dimensioni app notevolmente inferiori rispetto a un'app Blazor lato client e caricare più velocemente.</span><span class="sxs-lookup"><span data-stu-id="12cd7-150">Have a significantly smaller app size than a client-side Blazor app and load much faster.</span></span>
* <span data-ttu-id="12cd7-151">Possono sfruttare le funzionalità server, incluso l'uso di qualsiasi API compatibili con .NET Core.</span><span class="sxs-lookup"><span data-stu-id="12cd7-151">Can take full advantage of server capabilities, including using any .NET Core compatible APIs.</span></span>
* <span data-ttu-id="12cd7-152">Eseguire in .NET Core nel server, quindi .NET esistenti degli strumenti, ad esempio il debug, funziona come previsto.</span><span class="sxs-lookup"><span data-stu-id="12cd7-152">Run on .NET Core on the server, so existing .NET tooling, such as debugging, works as expected.</span></span>
* <span data-ttu-id="12cd7-153">Funziona con i thin client (ad esempio, i browser che non supportano WebAssembly e risorse vincolate dispositivi).</span><span class="sxs-lookup"><span data-stu-id="12cd7-153">Works with thin clients (for example, browsers that don't support WebAssembly and resource constrained devices).</span></span>
* <span data-ttu-id="12cd7-154">.NET /C# base di codice, incluso il codice di componente dell'app, non viene servita al client.</span><span class="sxs-lookup"><span data-stu-id="12cd7-154">.NET/C# code base, including the app's component code, isn't served to clients.</span></span>

<span data-ttu-id="12cd7-155">Esistono svantaggi lato server di hosting.</span><span class="sxs-lookup"><span data-stu-id="12cd7-155">There are downsides to server-side hosting.</span></span> <span data-ttu-id="12cd7-156">Componenti lato server Razor:</span><span class="sxs-lookup"><span data-stu-id="12cd7-156">Server-side Razor Components:</span></span>

* <span data-ttu-id="12cd7-157">Hanno una latenza maggiore: Ogni interazione dell'utente prevede un hop di rete.</span><span class="sxs-lookup"><span data-stu-id="12cd7-157">Have higher latency: Every user interaction involves a network hop.</span></span>
* <span data-ttu-id="12cd7-158">Non offrono alcun supporto offline: Se la connessione del client non riesce, l'app smette di funzionare.</span><span class="sxs-lookup"><span data-stu-id="12cd7-158">Offer no offline support: If the client connection fails, the app stops working.</span></span>
* <span data-ttu-id="12cd7-159">La scalabilità ridotta: Il server deve gestire più connessioni di client e la gestione dello stato del client.</span><span class="sxs-lookup"><span data-stu-id="12cd7-159">Have reduced scalability: The server must manage multiple client connections and handle client state.</span></span>
* <span data-ttu-id="12cd7-160">Richiede un server di ASP.NET Core per rendere disponibile l'app.</span><span class="sxs-lookup"><span data-stu-id="12cd7-160">Require an ASP.NET Core server to serve the app.</span></span> <span data-ttu-id="12cd7-161">Distribuzione senza un server (ad esempio, da una rete CDN) non è possibile.</span><span class="sxs-lookup"><span data-stu-id="12cd7-161">Deployment without a server (for example, from a CDN) isn't possible.</span></span>

<span data-ttu-id="12cd7-162">&dagger;Il *components.server.js* lo script viene pubblicato nel seguente percorso: *bin / {Debug | Versione} / {FRAMEWORK di destinazione} /publish/ {nome applicazione}. Dist/App/Framework*.</span><span class="sxs-lookup"><span data-stu-id="12cd7-162">&dagger;The *components.server.js* script is published to the following path: *bin/{Debug|Release}/{TARGET FRAMEWORK}/publish/{APPLICATION NAME}.App/dist/_framework*.</span></span>
