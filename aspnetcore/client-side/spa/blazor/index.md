---
title: Introduzione a Blazor
author: guardrex
description: 'Esplorare ASP.NET Core Blazor, un nuovo modo per creare app sul lato client interattive con .NET eseguite nel browser con WebAssembly.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: spa/blazor/index
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="28606-103">Introduzione a Blazor</span><span class="sxs-lookup"><span data-stu-id="28606-103">Introduction to Blazor</span></span>

<span data-ttu-id="28606-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="28606-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="28606-105">Blazor è un framework per app a pagina singola per la creazione di app Web sul lato client interattive con .NET.</span><span class="sxs-lookup"><span data-stu-id="28606-105">Blazor is a single-page app framework for building interactive client-side Web apps with .NET.</span></span> <span data-ttu-id="28606-106">Blazor usa gli standard Web aperti senza plug-in o transpiling del codice.</span><span class="sxs-lookup"><span data-stu-id="28606-106">Blazor uses open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="28606-107">Blazor funziona in tutti i Web browser moderni, inclusi i browser per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="28606-107">Blazor works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="28606-108">L'uso di .NET nel browser per lo sviluppo Web sul lato client offre molti vantaggi:</span><span class="sxs-lookup"><span data-stu-id="28606-108">Using .NET in the browser for client-side web development offers many advantages:</span></span>

* <span data-ttu-id="28606-109">**Linguaggio C#**: scrivere codice in C# invece che in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="28606-109">**C# language**: Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="28606-110">**Ecosistema .NET**: sfruttare l'ecosistema esistente di librerie .NET.</span><span class="sxs-lookup"><span data-stu-id="28606-110">**.NET Ecosystem**: Leverage the existing ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="28606-111">**Sviluppo per lo stack completo**: condividere la logica sul lato client e server.</span><span class="sxs-lookup"><span data-stu-id="28606-111">**Full-stack development**: Share server and client-side logic.</span></span>
* <span data-ttu-id="28606-112">**Velocità e scalabilità**: .NET è stato progettato per prestazioni, affidabilità e sicurezza.</span><span class="sxs-lookup"><span data-stu-id="28606-112">**Speed and scalability**: .NET was built for performance, reliability, and security.</span></span>
* <span data-ttu-id="28606-113">**Strumenti leader nel settore**: produttività con Visual Studio in Windows, Linux e macOS.</span><span class="sxs-lookup"><span data-stu-id="28606-113">**Industry-leading tools**: Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="28606-114">**Stabilità e coerenza**:  basato su un set comune di linguaggi, framework e strumenti che sono stabili, ricchi di funzionalità e facili da usare.</span><span class="sxs-lookup"><span data-stu-id="28606-114">**Stability and consistency**:  Build on a commonset of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

<span data-ttu-id="28606-115">L'esecuzione di codice .NET all'interno di Web browser è resa possibile da [WebAssembly](http://webassembly.org) (tecnologia nota anche con l'abbreviazione *wasm*).</span><span class="sxs-lookup"><span data-stu-id="28606-115">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="28606-116">WebAssembly è un standard Web aperto ed è supportato nei Web browser senza plug-in.</span><span class="sxs-lookup"><span data-stu-id="28606-116">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="28606-117">WebAssembly è un formato bytecode compatto ottimizzato per il download veloce e la velocità massima di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="28606-117">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="28606-118">Il codice WebAssembly può accedere a tutte le funzionalità del browser tramite interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="28606-118">WebAssembly code can access the full functionality of the browser via JavaScript interop.</span></span> <span data-ttu-id="28606-119">Allo stesso tempo, il codice .NET eseguito tramite WebAssembly viene eseguito nello stesso ambiente sandbox attendibile di JavaScript per evitare azioni dannose nel computer client.</span><span class="sxs-lookup"><span data-stu-id="28606-119">At the same time, .NET code executed via WebAssembly runs in the same trusted sandbox as JavaScript to prevent malicious actions on the client machine.</span></span>

![Blazor esegue codice .NET nel browser con WebAssembly.](index/_static/blazor.png)

<span data-ttu-id="28606-121">Quando un'app Blazor viene compilata ed eseguita in un browser:</span><span class="sxs-lookup"><span data-stu-id="28606-121">When a Blazor app is built and run in a browser:</span></span>

* <span data-ttu-id="28606-122">I file di codice C# e i file Razor vengono compilati in assembly .NET.</span><span class="sxs-lookup"><span data-stu-id="28606-122">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="28606-123">Gli assembly e il runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="28606-123">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="28606-124">Blazor esegue il bootstrap del runtime .NET e configura il runtime per caricare gli assembly per l'app.</span><span class="sxs-lookup"><span data-stu-id="28606-124">Blazor bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="28606-125">La modifica del modello DOM (Document Object Model) e le chiamate API al browser vengono gestite dal runtime Blazor tramite interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="28606-125">Document Object Model (DOM) manipulation and browser API calls are handled by the Blazor runtime via JavaScript interop.</span></span>

<span data-ttu-id="28606-126">Blazor supporta gli elementi fondamentali richiesti dalla maggior parte delle app, tra cui:</span><span class="sxs-lookup"><span data-stu-id="28606-126">Blazor supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="28606-127">Parametri</span><span class="sxs-lookup"><span data-stu-id="28606-127">Parameters</span></span>
* <span data-ttu-id="28606-128">Gestione di eventi</span><span class="sxs-lookup"><span data-stu-id="28606-128">Event handling</span></span>
* <span data-ttu-id="28606-129">Associazione dati</span><span class="sxs-lookup"><span data-stu-id="28606-129">Data binding</span></span>
* <span data-ttu-id="28606-130">Routing</span><span class="sxs-lookup"><span data-stu-id="28606-130">Routing</span></span>
* <span data-ttu-id="28606-131">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="28606-131">Dependency injection</span></span>
* <span data-ttu-id="28606-132">Layout</span><span class="sxs-lookup"><span data-stu-id="28606-132">Layouts</span></span>
* <span data-ttu-id="28606-133">Modelli</span><span class="sxs-lookup"><span data-stu-id="28606-133">Templating</span></span>
* <span data-ttu-id="28606-134">Valori a cascata</span><span class="sxs-lookup"><span data-stu-id="28606-134">Cascading values</span></span>

<span data-ttu-id="28606-135">Per ridurre le dimensioni dell'app scaricata, il codice inutilizzato viene rimosso dall'app quando viene pubblicata dal [linker Intermediate Language (IL)](xref:host-and-deploy/razor-components/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="28606-135">To reduce the size of the downloaded app unused code stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/razor-components/configure-linker).</span></span>

<span data-ttu-id="28606-136">Blazor è il modello di hosting sul lato client per Razor Components.</span><span class="sxs-lookup"><span data-stu-id="28606-136">Blazor is the client-side hosting model for Razor Components.</span></span> <span data-ttu-id="28606-137">Dato che Razor Components separa la logica di rendering di un componente dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente, c'è flessibilità nelle modalità di hosting di Razor Components.</span><span class="sxs-lookup"><span data-stu-id="28606-137">Because Razor Components decouple a component's rendering logic from how UI updates are applied, there's flexibility in how Razor Components can be hosted.</span></span> <span data-ttu-id="28606-138">Usare ASP.NET Core Razor Components per ospitare Razor Components nel server in un'app ASP.NET Core in cui gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="28606-138">Use ASP.NET Core Razor Components to host Razor Components on the server in an ASP.NET Core app where UI updates are handled over a SignalR connection.</span></span> <span data-ttu-id="28606-139">Per ulteriori informazioni, vedere <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="28606-139">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span> 

## <a name="components"></a><span data-ttu-id="28606-140">Componenti</span><span class="sxs-lookup"><span data-stu-id="28606-140">Components</span></span>

<span data-ttu-id="28606-141">Un *componente Razor* è una parte dell'interfaccia utente, ad esempio una pagina, una finestra di dialogo o un modulo per l'immissione dei dati.</span><span class="sxs-lookup"><span data-stu-id="28606-141">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="28606-142">I componenti gestiscono gli eventi utente e definiscono la logica di rendering dell'interfaccia utente flessibile.</span><span class="sxs-lookup"><span data-stu-id="28606-142">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="28606-143">I componenti possono essere annidati e riutilizzati.</span><span class="sxs-lookup"><span data-stu-id="28606-143">Components can be nested and reused.</span></span>

<span data-ttu-id="28606-144">I componenti sono classi .NET compilate in assembly .NET che possono essere condivisi e distribuiti come pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="28606-144">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="28606-145">La classe può essere scritta sotto forma di una pagina di markup Razor (*.cshtml*) o come una classe C# (*.cs*).</span><span class="sxs-lookup"><span data-stu-id="28606-145">The class can either be written in the form of a Razor markup page (*.cshtml*) or as a C# class (*.cs*).</span></span>

<span data-ttu-id="28606-146">[Razor](xref:mvc/views/razor) è una sintassi per la combinazione di markup HTML con codice C#.</span><span class="sxs-lookup"><span data-stu-id="28606-146">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="28606-147">Razor è progettato per la produttività degli sviluppatori, consentendo loro di alternare markup e C# nello stesso file con il supporto di [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="28606-147">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="28606-148">Anche le pagine Razor e le viste MVC usano Razor.</span><span class="sxs-lookup"><span data-stu-id="28606-148">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="28606-149">Diversamente dalle pagine Razor e dalle viste MVC, che sono basate su un modello di richiesta/risposta, i componenti vengono usati in modo specifico per la gestione della composizione dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="28606-149">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="28606-150">È possibile usare Razor Components in modo specifico per la composizione e la logica dell'interfaccia utente sul lato client.</span><span class="sxs-lookup"><span data-stu-id="28606-150">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="28606-151">Il markup seguente è un esempio di un componente finestra di dialogo personalizzato in un file Razor (*DialogComponent.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="28606-151">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.cshtml*):</span></span>

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

<span data-ttu-id="28606-152">Quando questo componente viene usato in altre posizioni nell'app, IntelliSense accelera lo sviluppo con il completamento di sintassi e parametri.</span><span class="sxs-lookup"><span data-stu-id="28606-152">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="28606-153">Il rendering dei componenti viene eseguito in una rappresentazione in memoria del modello DOM del browser nota come *albero di rendering*, che può quindi essere usata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="28606-153">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="28606-154">Interoperabilità JavaScript</span><span class="sxs-lookup"><span data-stu-id="28606-154">JavaScript interop</span></span>

<span data-ttu-id="28606-155">Per le app che richiedono librerie JavaScript e API browser di terze parti, Blazor supporta l'interoperabilità con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="28606-155">For apps that require third-party JavaScript libraries and browser APIs, Blazor interoperates with JavaScript.</span></span> <span data-ttu-id="28606-156">I componenti sono in grado di usare qualsiasi libreria o API supportata da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="28606-156">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="28606-157">Il codice C# può effettuare chiamate nel codice JavaScript e vice versa.</span><span class="sxs-lookup"><span data-stu-id="28606-157">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="28606-158">Per altre informazioni, vedere [Interoperabilità JavaScript](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="28606-158">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="28606-159">Condivisione del codice e .NET Standard</span><span class="sxs-lookup"><span data-stu-id="28606-159">Code sharing and .NET Standard</span></span>

<span data-ttu-id="28606-160">Le app possono fare riferimento a librerie [.NET Standard](/dotnet/standard/net-standard) esistenti e usarle.</span><span class="sxs-lookup"><span data-stu-id="28606-160">Apps can reference and use existing [.NET Standard](/dotnet/standard/net-standard) libraries.</span></span> <span data-ttu-id="28606-161">.NET Standard è una specifica formale delle API .NET comuni tra le implementazioni di .NET.</span><span class="sxs-lookup"><span data-stu-id="28606-161">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="28606-162">Sono supportati .NET standard 2.0 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="28606-162">.NET Standard 2.0 or higher is supported.</span></span> <span data-ttu-id="28606-163">Le API non applicabili all'interno di un Web browser (ad esempio, per l'accesso al file system, l'apertura di un socket, la gestione dei thread e altre funzionalità) generano <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="28606-163">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, threading, and other features) throw <xref:System.PlatformNotSupportedException>.</span></span> <span data-ttu-id="28606-164">Le librerie di classi .NET Standard possono essere condivise tra il codice sul lato server e nelle app basate su browser.</span><span class="sxs-lookup"><span data-stu-id="28606-164">.NET Standard class libraries can be shared across server code and in browser-based apps.</span></span>

## <a name="optimization"></a><span data-ttu-id="28606-165">Ottimizzazione</span><span class="sxs-lookup"><span data-stu-id="28606-165">Optimization</span></span>

<span data-ttu-id="28606-166">Le dimensioni del payload sono un fattore cruciale per le prestazioni per l'usabilità dell'app.</span><span class="sxs-lookup"><span data-stu-id="28606-166">Payload size is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="28606-167">Blazor consente di ottimizzare le dimensioni del payload per ridurre i tempi di download:</span><span class="sxs-lookup"><span data-stu-id="28606-167">Blazor optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="28606-168">Le parti inutilizzate degli assembly .NET vengono rimosse durante il processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="28606-168">Unused parts of .NET assemblies are removed during the build process.</span></span>
* <span data-ttu-id="28606-169">Le risposte HTTP vengono compresse.</span><span class="sxs-lookup"><span data-stu-id="28606-169">HTTP responses are compressed.</span></span>
* <span data-ttu-id="28606-170">Il runtime e gli assembly .NET vengono memorizzati nella cache nel browser.</span><span class="sxs-lookup"><span data-stu-id="28606-170">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="28606-171">[Razor Components sul lato server](xref:razor-components/index) offre dimensioni di payload anche inferiori rispetto a Blazor grazie alla gestione degli assembly .NET, dell'assembly dell'app e del runtime sul lato server.</span><span class="sxs-lookup"><span data-stu-id="28606-171">[Server-side Razor Components](xref:razor-components/index) provides an even smaller payload size than Blazor by maintaining .NET assemblies, the app's assembly, and the runtime server-side.</span></span> <span data-ttu-id="28606-172">Le app realizzate con Razor Components forniscono solo file di markup e asset statici ai client.</span><span class="sxs-lookup"><span data-stu-id="28606-172">Razor Components apps only serve markup files and static assets to clients.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28606-173">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="28606-173">Additional resources</span></span>

* <xref:razor-components/index>
* [<span data-ttu-id="28606-174">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="28606-174">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="28606-175">Guida a C#</span><span class="sxs-lookup"><span data-stu-id="28606-175">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="28606-176">HTML</span><span class="sxs-lookup"><span data-stu-id="28606-176">HTML</span></span>](https://www.w3.org/html/)
