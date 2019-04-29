---
title: Introduzione a Blazor in ASP.NET Core
author: guardrex
description: Esplorare ASP.NET Core Blazor, un modo per creare un'interfaccia utente Web sul lato client interattiva con .NET in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982997"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="b39e1-103">Introduzione a Blazor</span><span class="sxs-lookup"><span data-stu-id="b39e1-103">Introduction to Blazor</span></span>

<span data-ttu-id="b39e1-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b39e1-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b39e1-105">Introduzione a Blazor</span><span class="sxs-lookup"><span data-stu-id="b39e1-105">Welcome to Blazor!</span></span>

<span data-ttu-id="b39e1-106">Sviluppare un'interfaccia utente Web interattiva sul lato client con .NET:</span><span class="sxs-lookup"><span data-stu-id="b39e1-106">Build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="b39e1-107">Sviluppare interfacce utente interattive avanzate usando C# invece che JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b39e1-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="b39e1-108">Condividere la logica dell'app scritta con .NET, sul lato client e sul lato server.</span><span class="sxs-lookup"><span data-stu-id="b39e1-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="b39e1-109">Eseguire il rendering dell'interfaccia utente come HTML e CSS per un ampio supporto dei browser, inclusi i browser per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="b39e1-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="b39e1-110">Blazor supporta i principali scenari richiesti dalla maggior parte delle app:</span><span class="sxs-lookup"><span data-stu-id="b39e1-110">Blazor supports core scenarios required by most apps:</span></span>

* <span data-ttu-id="b39e1-111">Parametri</span><span class="sxs-lookup"><span data-stu-id="b39e1-111">Parameters</span></span>
* <span data-ttu-id="b39e1-112">Gestione di eventi</span><span class="sxs-lookup"><span data-stu-id="b39e1-112">Event handling</span></span>
* <span data-ttu-id="b39e1-113">Associazione dati</span><span class="sxs-lookup"><span data-stu-id="b39e1-113">Data binding</span></span>
* <span data-ttu-id="b39e1-114">Routing</span><span class="sxs-lookup"><span data-stu-id="b39e1-114">Routing</span></span>
* <span data-ttu-id="b39e1-115">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="b39e1-115">Dependency injection</span></span>
* <span data-ttu-id="b39e1-116">Layout</span><span class="sxs-lookup"><span data-stu-id="b39e1-116">Layouts</span></span>
* <span data-ttu-id="b39e1-117">Modelli</span><span class="sxs-lookup"><span data-stu-id="b39e1-117">Templates</span></span>
* <span data-ttu-id="b39e1-118">Valori a cascata</span><span class="sxs-lookup"><span data-stu-id="b39e1-118">Cascading values</span></span>

## <a name="components"></a><span data-ttu-id="b39e1-119">Componenti</span><span class="sxs-lookup"><span data-stu-id="b39e1-119">Components</span></span>

<span data-ttu-id="b39e1-120">Un *componente* in Blazor è un elemento dell'interfaccia utente, ad esempio una pagina, una finestra di dialogo o un modulo per l'immissione dei dati.</span><span class="sxs-lookup"><span data-stu-id="b39e1-120">A *component* in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="b39e1-121">I componenti gestiscono gli eventi utente e definiscono la logica di rendering dell'interfaccia utente flessibile.</span><span class="sxs-lookup"><span data-stu-id="b39e1-121">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="b39e1-122">I componenti possono essere annidati e riutilizzati.</span><span class="sxs-lookup"><span data-stu-id="b39e1-122">Components can be nested and reused.</span></span>

<span data-ttu-id="b39e1-123">I componenti sono classi .NET compilate in assembly .NET che possono essere condivisi e distribuiti come pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="b39e1-123">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="b39e1-124">La classe dei componenti viene in genere scritta sotto forma di pagina di markup di Razor con un'estensione di file *razor*.</span><span class="sxs-lookup"><span data-stu-id="b39e1-124">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span> <span data-ttu-id="b39e1-125">I componenti di Blazor vengono a volte definiti componenti Razor.</span><span class="sxs-lookup"><span data-stu-id="b39e1-125">Components in Blazor are sometimes referred to as Razor components.</span></span>

<span data-ttu-id="b39e1-126">[Razor](xref:mvc/views/razor) è una sintassi per la combinazione di markup HTML con codice C#.</span><span class="sxs-lookup"><span data-stu-id="b39e1-126">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="b39e1-127">Razor è progettato per la produttività degli sviluppatori, consentendo loro di alternare markup e C# nello stesso file con il supporto di [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="b39e1-127">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="b39e1-128">Anche le pagine Razor e le viste MVC usano Razor.</span><span class="sxs-lookup"><span data-stu-id="b39e1-128">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="b39e1-129">Diversamente dalle pagine Razor e dalle viste MVC, che sono basate su un modello di richiesta/risposta, i componenti vengono usati in modo specifico per la gestione della composizione dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="b39e1-129">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="b39e1-130">È possibile usare i componenti Razor specificamente per la composizione e la logica dell'interfaccia utente sul lato client.</span><span class="sxs-lookup"><span data-stu-id="b39e1-130">Razor components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="b39e1-131">Il markup seguente è un esempio di un componente finestra di dialogo personalizzato:</span><span class="sxs-lookup"><span data-stu-id="b39e1-131">The following markup is an example of a custom dialog component:</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="b39e1-132">Quando questo componente viene usato altrove nell'app, IntelliSense in [Visual Studio](https://visualstudio.microsoft.com/vs/) velocizza lo sviluppo con il completamento di sintassi e parametri.</span><span class="sxs-lookup"><span data-stu-id="b39e1-132">When this component is used elsewhere in the app, IntelliSense in [Visual Studio](https://visualstudio.microsoft.com/vs/) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="b39e1-133">Il rendering dei componenti viene eseguito in una rappresentazione in memoria del modello DOM del browser nota come *albero di rendering*, che può quindi essere usata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="b39e1-133">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="b39e1-134">Blazor sul lato server</span><span class="sxs-lookup"><span data-stu-id="b39e1-134">Blazor server-side</span></span>

<span data-ttu-id="b39e1-135">Blazor separa la logica di rendering dei componenti dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="b39e1-135">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="b39e1-136">Blazor sul lato server fornisce il supporto per l'hosting di componenti Razor nel server in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b39e1-136">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="b39e1-137">Tutti gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="b39e1-137">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="b39e1-138">Il runtime:</span><span class="sxs-lookup"><span data-stu-id="b39e1-138">The runtime:</span></span>

* <span data-ttu-id="b39e1-139">Gestisce l'invio degli eventi dell'interfaccia utente dal browser al server.</span><span class="sxs-lookup"><span data-stu-id="b39e1-139">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="b39e1-140">Applica gli aggiornamenti dell'interfaccia utente inviati dal server nel browser dopo l'esecuzione dei componenti.</span><span class="sxs-lookup"><span data-stu-id="b39e1-140">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="b39e1-141">La connessione usata da Blazor sul lato server per comunicare con il browser viene usata anche per gestire le chiamate di interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b39e1-141">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor sul lato server esegue codice .NET sul server e interagisce con il modello DOM (Document Object Model) nel client tramite una connessione SignalR](index/_static/blazor-server-side.png)

<span data-ttu-id="b39e1-143">Per ulteriori informazioni, vedere <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="b39e1-143">For more information, see <xref:blazor/hosting-models#server-side>.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="b39e1-144">Blazor sul lato client</span><span class="sxs-lookup"><span data-stu-id="b39e1-144">Blazor client-side</span></span>

<span data-ttu-id="b39e1-145">Blazor sul lato client è un framework per app a pagina singola per la creazione di app Web sul lato client interattive con .NET.</span><span class="sxs-lookup"><span data-stu-id="b39e1-145">Blazor client-side is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="b39e1-146">Blazor sul lato client usa standard Web aperti senza plug-in o transpiling del codice.</span><span class="sxs-lookup"><span data-stu-id="b39e1-146">Blazor client-side uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="b39e1-147">Blazor sul lato client funziona in tutti i Web browser moderni, inclusi quelli per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="b39e1-147">Blazor client-side works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="b39e1-148">L'uso di .NET nel browser per lo sviluppo Web sul lato client offre molti vantaggi:</span><span class="sxs-lookup"><span data-stu-id="b39e1-148">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="b39e1-149">**Linguaggio C#**: scrivere codice in C# invece che in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b39e1-149">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="b39e1-150">**Ecosistema .NET**: sfruttare l'ecosistema esistente di librerie .NET.</span><span class="sxs-lookup"><span data-stu-id="b39e1-150">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="b39e1-151">**Sviluppo per lo stack completo**: condividere la logica sul lato client e server.</span><span class="sxs-lookup"><span data-stu-id="b39e1-151">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="b39e1-152">**Velocità e scalabilità**: .NET è stato progettato per prestazioni, affidabilità e sicurezza.</span><span class="sxs-lookup"><span data-stu-id="b39e1-152">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="b39e1-153">**Strumenti leader nel settore**: produttività con Visual Studio in Windows, Linux e macOS.</span><span class="sxs-lookup"><span data-stu-id="b39e1-153">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="b39e1-154">**Stabilità e coerenza**:  basato su un set comune di linguaggi, framework e strumenti che sono stabili, ricchi di funzionalità e facili da usare.</span><span class="sxs-lookup"><span data-stu-id="b39e1-154">**Stability and consistency**:  Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="b39e1-155">L'esecuzione di codice .NET all'interno di Web browser è resa possibile da [WebAssembly](http://webassembly.org) (tecnologia nota anche con l'abbreviazione *wasm*).</span><span class="sxs-lookup"><span data-stu-id="b39e1-155">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="b39e1-156">WebAssembly è un standard Web aperto ed è supportato nei Web browser senza plug-in.</span><span class="sxs-lookup"><span data-stu-id="b39e1-156">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="b39e1-157">WebAssembly è un formato bytecode compatto ottimizzato per il download veloce e la velocità massima di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b39e1-157">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="b39e1-158">Il codice WebAssembly può accedere a tutte le funzionalità del browser tramite interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b39e1-158">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="b39e1-159">Allo stesso tempo, il codice .NET eseguito tramite WebAssembly viene eseguito nello stesso ambiente sandbox attendibile di JavaScript per evitare azioni dannose nel computer client.</span><span class="sxs-lookup"><span data-stu-id="b39e1-159">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor sul lato client esegue codice .NET nel browser con WebAssembly.](index/_static/blazor-client-side.png)

<span data-ttu-id="b39e1-161">Quando un'app Blazor sul lato client viene compilata ed eseguita in un browser:</span><span class="sxs-lookup"><span data-stu-id="b39e1-161">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="b39e1-162">I file di codice C# e i file Razor vengono compilati in assembly .NET.</span><span class="sxs-lookup"><span data-stu-id="b39e1-162">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="b39e1-163">Gli assembly e il runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="b39e1-163">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="b39e1-164">Blazor sul lato client esegue il bootstrap del runtime .NET e configura il runtime per caricare gli assembly per l'app.</span><span class="sxs-lookup"><span data-stu-id="b39e1-164">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="b39e1-165">La modifica del modello DOM (Document Object Model) e le chiamate API al browser vengono gestite dal runtime di Blazor sul lato client tramite interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b39e1-165">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor client-side runtime via JavaScript interop.</span></span>

<span data-ttu-id="b39e1-166">Per ridurre le dimensioni dell'app scaricata, il codice inutilizzato viene rimosso dall'app quando viene pubblicata dal [linker Intermediate Language (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="b39e1-166">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="b39e1-167">Blazor sul lato client è un modello di hosting lato client.</span><span class="sxs-lookup"><span data-stu-id="b39e1-167">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="b39e1-168">Dato che Blazor separa la logica di rendering di un componente dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente, c'è flessibilità nelle modalità di hosting di Blazor.</span><span class="sxs-lookup"><span data-stu-id="b39e1-168">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="b39e1-169">Usare [Blazor sul lato server](#blazor-server-side) per ospitare Blazor nel server in un'app ASP.NET Core in cui gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="b39e1-169">Use [Blazor server-side](#blazor-server-side) to host Blazor on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="b39e1-170">Per ulteriori informazioni, vedere <xref:blazor/hosting-models#server-side>.</span><span class="sxs-lookup"><span data-stu-id="b39e1-170">For more information, see <xref:blazor/hosting-models#server-side>.</span></span> 

<span data-ttu-id="b39e1-171">Le dimensioni del payload sono un fattore cruciale per le prestazioni per l'usabilità dell'app.</span><span class="sxs-lookup"><span data-stu-id="b39e1-171">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="b39e1-172">Blazor sul lato client ottimizza le dimensioni del payload per ridurre i tempi di download:</span><span class="sxs-lookup"><span data-stu-id="b39e1-172">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="b39e1-173">Le parti inutilizzate degli assembly .NET vengono rimosse durante il processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="b39e1-173">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="b39e1-174">Le risposte HTTP vengono compresse.</span><span class="sxs-lookup"><span data-stu-id="b39e1-174">HTTP responses are compressed.</span></span>
* <span data-ttu-id="b39e1-175">Il runtime e gli assembly .NET vengono memorizzati nella cache nel browser.</span><span class="sxs-lookup"><span data-stu-id="b39e1-175">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="b39e1-176">[Blazor sul lato server](#blazor-server-side) offre dimensioni di payload inferiori rispetto a Blazor sul lato client mantenendo assembly .NET, assembly dell'app e runtime sul lato server.</span><span class="sxs-lookup"><span data-stu-id="b39e1-176">[Blazor server-side](#blazor-server-side) provides a smaller payload size than Blazor client-side by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="b39e1-177">Le app Blazor sul lato server forniscono solo file di markup e asset statici ai client.</span><span class="sxs-lookup"><span data-stu-id="b39e1-177">Blazor server-side apps only serve markup files and static assets to clients.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="b39e1-178">Interoperabilità JavaScript</span><span class="sxs-lookup"><span data-stu-id="b39e1-178">JavaScript interop</span></span>

<span data-ttu-id="b39e1-179">Per le app che richiedono librerie JavaScript e API browser di terze parti, i componenti supportano l'interoperabilità con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b39e1-179">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="b39e1-180">I componenti sono in grado di usare qualsiasi libreria o API supportata da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b39e1-180">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="b39e1-181">Il codice C# può effettuare chiamate nel codice JavaScript e vice versa.</span><span class="sxs-lookup"><span data-stu-id="b39e1-181">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="b39e1-182">Per altre informazioni, vedere [Interoperabilità JavaScript](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="b39e1-182">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="b39e1-183">Condivisione del codice e .NET Standard</span><span class="sxs-lookup"><span data-stu-id="b39e1-183">Code sharing and .NET Standard</span></span>

<span data-ttu-id="b39e1-184">Le app possono fare riferimento a librerie [.NET Standard](/dotnet/standard/net-standard) esistenti e usarle.</span><span class="sxs-lookup"><span data-stu-id="b39e1-184">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="b39e1-185">.NET Standard è una specifica formale delle API .NET comuni tra le implementazioni di .NET.</span><span class="sxs-lookup"><span data-stu-id="b39e1-185">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="b39e1-186">Blazor implementa .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="b39e1-186">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="b39e1-187">Le API non applicabili all'interno di un Web browser (ad esempio, per l'accesso al file system, l'apertura di un socket, la gestione dei thread e altre funzionalità) generano <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="b39e1-187">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="b39e1-188">Le librerie di classi .NET Standard possono essere condivise tra diverse piattaforme .NET, come Blazor, .NET Framework, .NET Core, Xamarin, Mono e Unity.</span><span class="sxs-lookup"><span data-stu-id="b39e1-188">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b39e1-189">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b39e1-189">Additional resources</span></span>

* [<span data-ttu-id="b39e1-190">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="b39e1-190">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="b39e1-191">Guida a C#</span><span class="sxs-lookup"><span data-stu-id="b39e1-191">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="b39e1-192">HTML</span><span class="sxs-lookup"><span data-stu-id="b39e1-192">HTML</span></span>](https://www.w3.org/html/)
