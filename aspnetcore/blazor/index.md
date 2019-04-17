---
title: Introduzione a Blazor in ASP.NET Core
author: guardrex
description: Esplorare ASP.NET Core Blazor, un modo per creare un'interfaccia utente Web sul lato client interattiva con .NET in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614597"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="baf80-103">Introduzione a Blazor</span><span class="sxs-lookup"><span data-stu-id="baf80-103">Introduction to Blazor</span></span>

<span data-ttu-id="baf80-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="baf80-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="razor-components"></a><span data-ttu-id="baf80-105">Razor Components</span><span class="sxs-lookup"><span data-stu-id="baf80-105">Razor Components</span></span>

<span data-ttu-id="baf80-106">Il modello di hosting sul lato server di Blazor, *Razor Components*, è un modo per sviluppare un'interfaccia utente Web sul lato client interattiva con .NET:</span><span class="sxs-lookup"><span data-stu-id="baf80-106">The server-side hosting model of Blazor, *Razor Components*, is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="baf80-107">Sviluppare interfacce utente interattive avanzate usando C# invece che JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf80-107">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="baf80-108">Condividere la logica dell'app, interamente scritta con .NET, sul lato client e sul lato server.</span><span class="sxs-lookup"><span data-stu-id="baf80-108">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="baf80-109">Eseguire il rendering dell'interfaccia utente come HTML e CSS per un ampio supporto dei browser, inclusi i browser per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="baf80-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="baf80-110">Razor Components supporta gli elementi fondamentali richiesti dalla maggior parte delle app, tra cui:</span><span class="sxs-lookup"><span data-stu-id="baf80-110">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="baf80-111">Parametri</span><span class="sxs-lookup"><span data-stu-id="baf80-111">Parameters</span></span>
* <span data-ttu-id="baf80-112">Gestione di eventi</span><span class="sxs-lookup"><span data-stu-id="baf80-112">Event handling</span></span>
* <span data-ttu-id="baf80-113">Associazione dati</span><span class="sxs-lookup"><span data-stu-id="baf80-113">Data binding</span></span>
* <span data-ttu-id="baf80-114">Routing</span><span class="sxs-lookup"><span data-stu-id="baf80-114">Routing</span></span>
* <span data-ttu-id="baf80-115">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="baf80-115">Dependency injection</span></span>
* <span data-ttu-id="baf80-116">Layout</span><span class="sxs-lookup"><span data-stu-id="baf80-116">Layouts</span></span>
* <span data-ttu-id="baf80-117">Modelli</span><span class="sxs-lookup"><span data-stu-id="baf80-117">Templating</span></span>
* <span data-ttu-id="baf80-118">Valori a cascata</span><span class="sxs-lookup"><span data-stu-id="baf80-118">Cascading values</span></span>

<span data-ttu-id="baf80-119">Razor Components separa la logica di rendering dei componenti dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="baf80-119">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="baf80-120">ASP.NET Core Razor Components in .NET Core 3.0 aggiunge il supporto per l'hosting di Razor Components nel server in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="baf80-120">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="baf80-121">Tutti gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="baf80-121">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="baf80-122">Il runtime:</span><span class="sxs-lookup"><span data-stu-id="baf80-122">The runtime:</span></span>

* <span data-ttu-id="baf80-123">Gestisce l'invio degli eventi dell'interfaccia utente dal browser al server.</span><span class="sxs-lookup"><span data-stu-id="baf80-123">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="baf80-124">Applica gli aggiornamenti dell'interfaccia utente inviati dal server nel browser dopo l'esecuzione dei componenti.</span><span class="sxs-lookup"><span data-stu-id="baf80-124">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="baf80-125">La connessione usata da Razor Components per comunicare con il browser viene usata anche per gestire le chiamate di interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf80-125">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor Components esegue codice .NET sul server e interagisce con il modello DOM (Document Object Model) nel client tramite una connessione SignalR](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="baf80-127">Per ulteriori informazioni, vedere <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="baf80-127">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="baf80-128">Componenti</span><span class="sxs-lookup"><span data-stu-id="baf80-128">Components</span></span>

<span data-ttu-id="baf80-129">Un *componente Razor* è una parte dell'interfaccia utente, ad esempio una pagina, una finestra di dialogo o un modulo per l'immissione dei dati.</span><span class="sxs-lookup"><span data-stu-id="baf80-129">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="baf80-130">I componenti gestiscono gli eventi utente e definiscono la logica di rendering dell'interfaccia utente flessibile.</span><span class="sxs-lookup"><span data-stu-id="baf80-130">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="baf80-131">I componenti possono essere annidati e riutilizzati.</span><span class="sxs-lookup"><span data-stu-id="baf80-131">Components can be nested and reused.</span></span>

<span data-ttu-id="baf80-132">I componenti sono classi .NET compilate in assembly .NET che possono essere condivisi e distribuiti come pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="baf80-132">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="baf80-133">La classe viene in genere scritta in forma di pagina di markup Razor con estensione di file *razor* (Razor Components) o di pagina di markup Razor con estensione *cshtml* (Blazor).</span><span class="sxs-lookup"><span data-stu-id="baf80-133">The class is normally written in the form of a Razor markup page with a *.razor* file extension (Razor Components) or Razor markup page with a *.cshtml* file extension (Blazor).</span></span>

<span data-ttu-id="baf80-134">[Razor](xref:mvc/views/razor) è una sintassi per la combinazione di markup HTML con codice C#.</span><span class="sxs-lookup"><span data-stu-id="baf80-134">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="baf80-135">Razor è progettato per la produttività degli sviluppatori, consentendo loro di alternare markup e C# nello stesso file con il supporto di [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="baf80-135">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="baf80-136">Anche le pagine Razor e le viste MVC usano Razor.</span><span class="sxs-lookup"><span data-stu-id="baf80-136">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="baf80-137">Diversamente dalle pagine Razor e dalle viste MVC, che sono basate su un modello di richiesta/risposta, i componenti vengono usati in modo specifico per la gestione della composizione dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="baf80-137">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="baf80-138">È possibile usare Razor Components in modo specifico per la composizione e la logica dell'interfaccia utente sul lato client.</span><span class="sxs-lookup"><span data-stu-id="baf80-138">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="baf80-139">Il markup seguente è un esempio di un componente finestra di dialogo personalizzato:</span><span class="sxs-lookup"><span data-stu-id="baf80-139">The following markup is an example of a custom dialog component:</span></span>

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

<span data-ttu-id="baf80-140">Quando questo componente viene usato in altre posizioni nell'app, IntelliSense accelera lo sviluppo con il completamento di sintassi e parametri.</span><span class="sxs-lookup"><span data-stu-id="baf80-140">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="baf80-141">Il rendering dei componenti viene eseguito in una rappresentazione in memoria del modello DOM del browser nota come *albero di rendering*, che può quindi essere usata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="baf80-141">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="baf80-142">Interoperabilità JavaScript</span><span class="sxs-lookup"><span data-stu-id="baf80-142">JavaScript interop</span></span>

<span data-ttu-id="baf80-143">Per le app che richiedono librerie JavaScript e API browser di terze parti, i componenti supportano l'interoperabilità con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf80-143">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="baf80-144">I componenti sono in grado di usare qualsiasi libreria o API supportata da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf80-144">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="baf80-145">Il codice C# può effettuare chiamate nel codice JavaScript e vice versa.</span><span class="sxs-lookup"><span data-stu-id="baf80-145">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="baf80-146">Per altre informazioni, vedere [Interoperabilità JavaScript](xref:blazor/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="baf80-146">For more information, see [JavaScript interop](xref:blazor/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="baf80-147">Condivisione del codice e .NET Standard</span><span class="sxs-lookup"><span data-stu-id="baf80-147">Code sharing and .NET Standard</span></span>

<span data-ttu-id="baf80-148">Le app possono fare riferimento a librerie [.NET Standard](/dotnet/standard/net-standard) esistenti e usarle.</span><span class="sxs-lookup"><span data-stu-id="baf80-148">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="baf80-149">.NET Standard è una specifica formale delle API .NET comuni tra le implementazioni di .NET.</span><span class="sxs-lookup"><span data-stu-id="baf80-149">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="baf80-150">Blazor implementa .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="baf80-150">Blazor implements .NET Standard 2.0.</span></span> <span data-ttu-id="baf80-151">Le API non applicabili all'interno di un Web browser (ad esempio, per l'accesso al file system, l'apertura di un socket, la gestione dei thread e altre funzionalità) generano <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="baf80-151">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="baf80-152">Le librerie di classi .NET Standard possono essere condivise tra diverse piattaforme .NET, come Blazor, .NET Framework, .NET Core, Xamarin, Mono e Unity.</span><span class="sxs-lookup"><span data-stu-id="baf80-152">.NET Standard class libraries can be shared across different .NET platforms, like Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

## <a name="blazor"></a><span data-ttu-id="baf80-153">Blazor</span><span class="sxs-lookup"><span data-stu-id="baf80-153">Blazor</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="baf80-154">Blazor è un framework per app a pagina singola per la creazione di app Web sul lato client interattive con .NET.</span><span class="sxs-lookup"><span data-stu-id="baf80-154">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="baf80-155">Blazor usa gli standard Web aperti senza plug-in o transpiling del codice.</span><span class="sxs-lookup"><span data-stu-id="baf80-155">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="baf80-156">Blazor funziona in tutti i Web browser moderni, inclusi i browser per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="baf80-156">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="baf80-157">L'uso di .NET nel browser per lo sviluppo Web sul lato client offre molti vantaggi:</span><span class="sxs-lookup"><span data-stu-id="baf80-157">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="baf80-158">**Linguaggio C#**: scrivere codice in C# invece che in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf80-158">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="baf80-159">**Ecosistema .NET**: sfruttare l'ecosistema esistente di librerie .NET.</span><span class="sxs-lookup"><span data-stu-id="baf80-159">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="baf80-160">**Sviluppo per lo stack completo**: condividere la logica sul lato client e server.</span><span class="sxs-lookup"><span data-stu-id="baf80-160">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="baf80-161">**Velocità e scalabilità**: .NET è stato progettato per prestazioni, affidabilità e sicurezza.</span><span class="sxs-lookup"><span data-stu-id="baf80-161">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="baf80-162">**Strumenti leader nel settore**: produttività con Visual Studio in Windows, Linux e macOS.</span><span class="sxs-lookup"><span data-stu-id="baf80-162">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="baf80-163">**Stabilità e coerenza**:  basato su un set comune di linguaggi, framework e strumenti che sono stabili, ricchi di funzionalità e facili da usare.</span><span class="sxs-lookup"><span data-stu-id="baf80-163">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="baf80-164">L'esecuzione di codice .NET all'interno di Web browser è resa possibile da [WebAssembly](http://webassembly.org) (tecnologia nota anche con l'abbreviazione *wasm*).</span><span class="sxs-lookup"><span data-stu-id="baf80-164">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="baf80-165">WebAssembly è un standard Web aperto ed è supportato nei Web browser senza plug-in.</span><span class="sxs-lookup"><span data-stu-id="baf80-165">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="baf80-166">WebAssembly è un formato bytecode compatto ottimizzato per il download veloce e la velocità massima di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="baf80-166">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="baf80-167">Il codice WebAssembly può accedere a tutte le funzionalità del browser tramite interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf80-167">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="baf80-168">Allo stesso tempo, il codice .NET eseguito tramite WebAssembly viene eseguito nello stesso ambiente sandbox attendibile di JavaScript per evitare azioni dannose nel computer client.</span><span class="sxs-lookup"><span data-stu-id="baf80-168">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor esegue codice .NET nel browser con WebAssembly.](index/_static/blazor.png)

<span data-ttu-id="baf80-170">Quando un'app Blazor viene compilata ed eseguita in un browser:</span><span class="sxs-lookup"><span data-stu-id="baf80-170">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="baf80-171">I file di codice C# e i file Razor vengono compilati in assembly .NET.</span><span class="sxs-lookup"><span data-stu-id="baf80-171">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="baf80-172">Gli assembly e il runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="baf80-172">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="baf80-173">Blazor esegue il bootstrap del runtime .NET e configura il runtime per caricare gli assembly per l'app.</span><span class="sxs-lookup"><span data-stu-id="baf80-173">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="baf80-174">La modifica del modello DOM (Document Object Model) e le chiamate API al browser vengono gestite dal runtime Blazor tramite interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="baf80-174">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="baf80-175">Blazor supporta gli elementi fondamentali richiesti dalla maggior parte delle app, tra cui:</span><span class="sxs-lookup"><span data-stu-id="baf80-175">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="baf80-176">Parametri</span><span class="sxs-lookup"><span data-stu-id="baf80-176">Parameters</span></span>
* <span data-ttu-id="baf80-177">Gestione di eventi</span><span class="sxs-lookup"><span data-stu-id="baf80-177">Event handling</span></span>
* <span data-ttu-id="baf80-178">Associazione dati</span><span class="sxs-lookup"><span data-stu-id="baf80-178">Data binding</span></span>
* <span data-ttu-id="baf80-179">Routing</span><span class="sxs-lookup"><span data-stu-id="baf80-179">Routing</span></span>
* <span data-ttu-id="baf80-180">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="baf80-180">Dependency injection</span></span>
* <span data-ttu-id="baf80-181">Layout</span><span class="sxs-lookup"><span data-stu-id="baf80-181">Layouts</span></span>
* <span data-ttu-id="baf80-182">Modelli</span><span class="sxs-lookup"><span data-stu-id="baf80-182">Templating</span></span>
* <span data-ttu-id="baf80-183">Valori a cascata</span><span class="sxs-lookup"><span data-stu-id="baf80-183">Cascading values</span></span>

<span data-ttu-id="baf80-184">Per ridurre le dimensioni dell'app scaricata, il codice inutilizzato viene rimosso dall'app quando viene pubblicata dal [linker Intermediate Language (IL)](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="baf80-184">To reduce the size of the downloaded app, unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>

<span data-ttu-id="baf80-185">Blazor sul lato client è un modello di hosting lato client.</span><span class="sxs-lookup"><span data-stu-id="baf80-185">Blazor client-side is a client-side hosting model.</span></span> <span data-ttu-id="baf80-186">Dato che Blazor separa la logica di rendering di un componente dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente, c'è flessibilità nelle modalità di hosting di Blazor.</span><span class="sxs-lookup"><span data-stu-id="baf80-186">Because Blazor decouples a component's rendering logic from how UI updates are applied, there's flexibility in how Blazor can be hosted.</span></span> <span data-ttu-id="baf80-187">Usare ASP.NET Core [Razor Components](#razor-components) per ospitare Razor Components nel server in un'app ASP.NET Core in cui gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="baf80-187">Use ASP.NET Core [Razor Components](#razor-components) to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="baf80-188">Per ulteriori informazioni, vedere <xref:blazor/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="baf80-188">For more information, see <xref:blazor/hosting-models#server-side-hosting-model>.</span></span> 

<span data-ttu-id="baf80-189">Le dimensioni del payload sono un fattore cruciale per le prestazioni per l'usabilità dell'app.</span><span class="sxs-lookup"><span data-stu-id="baf80-189">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="baf80-190">Blazor consente di ottimizzare le dimensioni del payload per ridurre i tempi di download:</span><span class="sxs-lookup"><span data-stu-id="baf80-190">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="baf80-191">Le parti inutilizzate degli assembly .NET vengono rimosse durante il processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="baf80-191">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="baf80-192">Le risposte HTTP vengono compresse.</span><span class="sxs-lookup"><span data-stu-id="baf80-192">HTTP responses are compressed.</span></span>
* <span data-ttu-id="baf80-193">Il runtime e gli assembly .NET vengono memorizzati nella cache nel browser.</span><span class="sxs-lookup"><span data-stu-id="baf80-193">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="baf80-194">[Razor Components](#razor-components) offre dimensioni di payload anche inferiori rispetto a Blazor grazie alla gestione degli assembly .NET, dell'assembly dell'app e del runtime sul lato server.</span><span class="sxs-lookup"><span data-stu-id="baf80-194">[Razor Components](#razor-components) provides a smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="baf80-195">Le app realizzate con Razor Components forniscono solo file di markup e asset statici ai client.</span><span class="sxs-lookup"><span data-stu-id="baf80-195">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="baf80-196">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="baf80-196">Additional resources</span></span>

* [<span data-ttu-id="baf80-197">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="baf80-197">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="baf80-198">Guida a C#</span><span class="sxs-lookup"><span data-stu-id="baf80-198">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="baf80-199">HTML</span><span class="sxs-lookup"><span data-stu-id="baf80-199">HTML</span></span>](https://www.w3.org/html/)
