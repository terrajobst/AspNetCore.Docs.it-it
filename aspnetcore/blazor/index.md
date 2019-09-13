---
title: Introduzione a Blazor in ASP.NET Core
author: guardrex
description: Esplorare ASP.NET Core Blazor, un modo per creare un'interfaccia utente Web sul lato client interattiva con .NET in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 09/05/2019
uid: blazor/index
ms.openlocfilehash: 767ec8f106bebb92cf13a10eb63fab4905715d3d
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2019
ms.locfileid: "70964123"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="ff7a6-103">Introduzione a Blazor</span><span class="sxs-lookup"><span data-stu-id="ff7a6-103">Introduction to Blazor</span></span>

<span data-ttu-id="ff7a6-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ff7a6-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ff7a6-105">*Questo articolo offre un'introduzione a Blazor.*</span><span class="sxs-lookup"><span data-stu-id="ff7a6-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="ff7a6-106">Blazor è un framework per la compilazione di un'interfaccia utente Web lato client interattiva con .NET:</span><span class="sxs-lookup"><span data-stu-id="ff7a6-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="ff7a6-107">Creare interfacce utente interattive avanzate con C# invece di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="ff7a6-108">Condividere la logica dell'app scritta in .NET sul lato client e sul lato server.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="ff7a6-109">Eseguire il rendering dell'interfaccia utente come HTML e CSS per un ampio supporto dei browser, inclusi i browser per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="ff7a6-110">L'uso di .NET per lo sviluppo Web lato client offre i vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="ff7a6-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="ff7a6-111">scrivere codice in C# invece che in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="ff7a6-112">Permette di sfruttare l'ecosistema .NET esistente di librerie .NET.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="ff7a6-113">Permette di condividere la logica dell'app tra server e client.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-113">Share app logic across server and client.</span></span>
* <span data-ttu-id="ff7a6-114">Permette di ottenere le prestazioni, l'affidabilità e la sicurezza di .NET.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="ff7a6-115">produttività con Visual Studio in Windows, Linux e macOS.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="ff7a6-116">basato su un set comune di linguaggi, framework e strumenti che sono stabili, ricchi di funzionalità e facili da usare.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="ff7a6-117">Componenti</span><span class="sxs-lookup"><span data-stu-id="ff7a6-117">Components</span></span>

<span data-ttu-id="ff7a6-118">Le app Blazor sono basate su *componenti*.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="ff7a6-119">Un componente in Blazor è un elemento dell'interfaccia utente, ad esempio una pagina, una finestra di dialogo o un modulo per l'immissione di dati.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span>

<span data-ttu-id="ff7a6-120">I componenti sono classi .NET compilati in assembly .NET che:</span><span class="sxs-lookup"><span data-stu-id="ff7a6-120">Components are .NET classes built into .NET assemblies that:</span></span>

* <span data-ttu-id="ff7a6-121">Definiscono la logica di rendering dell'interfaccia utente flessibile.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-121">Define flexible UI rendering logic.</span></span>
* <span data-ttu-id="ff7a6-122">Gestiscono gli eventi utente.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-122">Handle user events.</span></span>
* <span data-ttu-id="ff7a6-123">Possono essere annidati e riutilizzati.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-123">Can be nested and reused.</span></span>
* <span data-ttu-id="ff7a6-124">Possono essere condivisi e distribuiti come [librerie di classi Razor](xref:razor-pages/ui-class) oppure [pacchetti NuGet](/nuget/what-is-nuget).</span><span class="sxs-lookup"><span data-stu-id="ff7a6-124">Can be shared and distributed as [Razor class libraries](xref:razor-pages/ui-class) or [NuGet packages](/nuget/what-is-nuget).</span></span>

<span data-ttu-id="ff7a6-125">La classe dei componenti viene in genere scritta sotto forma di pagina di markup di [Razor](xref:mvc/views/razor) con un'estensione di file *razor*.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-125">The component class is usually written in the form of a [Razor](xref:mvc/views/razor) markup page with a *.razor* file extension.</span></span> <span data-ttu-id="ff7a6-126">I componenti in Blazor sono formalmente noti come *componenti Razor*.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-126">Components in Blazor are formally referred to as *Razor components*.</span></span> <span data-ttu-id="ff7a6-127">Razor è una sintassi per la combinazione di markup HTML con codice C# progettata per la produttività degli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-127">Razor is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="ff7a6-128">Razor permette di passare tra markup HTML e C# nello stesso file con supporto per [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="ff7a6-128">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="ff7a6-129">Anche Razor Pages ed MVC usano Razor.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-129">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="ff7a6-130">Diversamente da Razor Pages ed MVC, che sono basati su un modello di richiesta/risposta, i componenti vengono usati in modo specifico per la logica e la composizione dell'interfaccia utente lato client.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-130">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="ff7a6-131">Il markup Razor seguente mostra un componente (*Dialog.razor*), che può essere annidato all'interno di un altro componente:</span><span class="sxs-lookup"><span data-stu-id="ff7a6-131">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    public string Title { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

<span data-ttu-id="ff7a6-132">Il contenuto del corpo della finestra di dialogo (`ChildContent`) e il titolo (`Title`) vengono forniti dal componente che usa questo componente nella propria interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-132">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="ff7a6-133">`OnYes` è un metodo C# attivato dall'evento `onclick` del pulsante.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-133">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="ff7a6-134">Blazor usa tag HTML naturali per la composizione dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-134">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="ff7a6-135">Gli elementi HTML specificano i componenti e gli attributi di un tag passano valori alle proprietà del componente.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-135">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span>

<span data-ttu-id="ff7a6-136">Nell'esempio seguente il componente `Index` usa il componente `Dialog`.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-136">In the following example, the `Index` component uses the `Dialog` component.</span></span> <span data-ttu-id="ff7a6-137">`ChildContent` e `Title` vengono impostati dagli attributi e dal contenuto dell'elemento `<Dialog>`.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-137">`ChildContent` and `Title` are set by the attributes and content of the `<Dialog>` element.</span></span>

<span data-ttu-id="ff7a6-138">*Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="ff7a6-138">*Index.razor*:</span></span>

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="ff7a6-139">Il rendering della finestra di dialogo viene eseguito quando viene effettuato l'accesso all'elemento padre (*Index.razor*) in un browser:</span><span class="sxs-lookup"><span data-stu-id="ff7a6-139">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![Rendering del componente Dialog nel browser](index/_static/dialog.png)

<span data-ttu-id="ff7a6-141">Quando questo componente viene usato nell'app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) e [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) accelera lo sviluppo con il completamento di sintassi e parametri.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-141">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="ff7a6-142">Il rendering dei componenti viene eseguito in una rappresentazione in memoria del modello DOM (Document Object Model) del browser denominata *albero di rendering*, usato per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-142">Components render into an in-memory representation of the browser's Document Object Model (DOM) called a *render tree*, which is used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-webassembly"></a><span data-ttu-id="ff7a6-143">Webassembly Blazer</span><span class="sxs-lookup"><span data-stu-id="ff7a6-143">Blazor WebAssembly</span></span>

<span data-ttu-id="ff7a6-144">Blazer webassembly è un Framework di app a singola pagina per la creazione di app Web interattive sul lato client con .NET.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-144">Blazor WebAssembly is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="ff7a6-145">Blazer webassembly USA standard Web aperti senza plug-in o transpilazione di codice e funziona in tutti i Web browser moderni, inclusi i browser per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-145">Blazor WebAssembly uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="ff7a6-146">L'esecuzione di codice .NET all'interno di Web browser è resa possibile da [WebAssembly](https://webassembly.org) (tecnologia nota anche con l'abbreviazione *wasm*).</span><span class="sxs-lookup"><span data-stu-id="ff7a6-146">Running .NET code inside web browsers is made possible by [WebAssembly](https://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="ff7a6-147">WebAssembly è un formato bytecode compatto ottimizzato per il download veloce e la velocità massima di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-147">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span> <span data-ttu-id="ff7a6-148">WebAssembly è un standard Web aperto ed è supportato nei Web browser senza plug-in.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-148">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span>

<span data-ttu-id="ff7a6-149">Il codice WebAssembly può accedere a tutte le funzionalità del browser tramite l'*interoperabilità JavaScript* (*JavaScript interop*).</span><span class="sxs-lookup"><span data-stu-id="ff7a6-149">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="ff7a6-150">Il codice .NET eseguito tramite WebAssembly nel browser viene eseguito nella sandbox JavaScript del browser con le misure di sicurezza offerte dalla sandbox per la protezione da azioni dannose nel computer client.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-150">.NET code executed via WebAssembly in the browser runs in the browser's JavaScript sandbox with the protections that the sandbox provides against malicious actions on the client machine.</span></span>

![Webassembly Blazer esegue il codice .NET nel browser con webassembly.](index/_static/blazor-webassembly.png)

<span data-ttu-id="ff7a6-152">Quando viene compilata ed eseguita un'app webassembly Blazer in un browser:</span><span class="sxs-lookup"><span data-stu-id="ff7a6-152">When a Blazor WebAssembly app is built and run in a browser:</span></span>

* <span data-ttu-id="ff7a6-153">I file di codice C# e i file Razor vengono compilati in assembly .NET.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-153">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="ff7a6-154">Gli assembly e il runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-154">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="ff7a6-155">Blazer webassembly avvia il Runtime .NET e configura il runtime per caricare gli assembly per l'app.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-155">Blazor WebAssembly bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="ff7a6-156">Il runtime di webassembly di Blazer usa l'interoperabilità JavaScript per gestire la manipolazione DOM e le chiamate API del browser.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-156">The Blazor WebAssembly runtime uses JavaScript interop to handle DOM manipulation and browser API calls.</span></span>

<span data-ttu-id="ff7a6-157">La dimensione dell'app pubblicata, ovvero la *dimensione del payload*, è un fattore cruciale per le prestazioni ai fini dell'usabilità dell'app.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-157">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="ff7a6-158">Un'app di grandi dimensioni impiega relativamente molto tempo a essere scaricata in un browser, influendo negativamente sull'esperienza utente.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-158">A large app takes a relatively long time to download to a browser, which diminishes the user experience.</span></span> <span data-ttu-id="ff7a6-159">Blazer webassembly ottimizza le dimensioni del payload per ridurre i tempi di download:</span><span class="sxs-lookup"><span data-stu-id="ff7a6-159">Blazor WebAssembly optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="ff7a6-160">Il codice non usato viene rimosso dall'app quando questa viene pubblicata dal [linker del linguaggio intermedio](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="ff7a6-160">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="ff7a6-161">Le risposte HTTP vengono compresse.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-161">HTTP responses are compressed.</span></span>
* <span data-ttu-id="ff7a6-162">Il runtime e gli assembly .NET vengono memorizzati nella cache nel browser.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-162">The .NET runtime and assemblies are cached in the browser.</span></span>

## <a name="blazor-server"></a><span data-ttu-id="ff7a6-163">Server Blazer</span><span class="sxs-lookup"><span data-stu-id="ff7a6-163">Blazor Server</span></span>

<span data-ttu-id="ff7a6-164">Blazor separa la logica di rendering dei componenti dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-164">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="ff7a6-165">Il server Blazer fornisce il supporto per l'hosting di componenti Razor sul server in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-165">Blazor Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="ff7a6-166">Gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="ff7a6-166">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="ff7a6-167">Il runtime gestisce l'invio di eventi dell'interfaccia utente dal browser al server e quindi applica gli aggiornamenti dell'interfaccia utente inviati dal server di nuovo al browser dopo l'esecuzione dei componenti.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-167">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="ff7a6-168">Per gestire le chiamate di interoperabilità JavaScript, viene usata anche la connessione utilizzata dal server Blaze per comunicare con il browser.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-168">The connection used by Blazor Server to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Il server Blaze esegue il codice .NET sul server e interagisce con il Document Object Model sul client tramite una connessione SignalR](index/_static/blazor-server.png)

## <a name="javascript-interop"></a><span data-ttu-id="ff7a6-170">Interoperabilità JavaScript</span><span class="sxs-lookup"><span data-stu-id="ff7a6-170">JavaScript interop</span></span>

<span data-ttu-id="ff7a6-171">Per le app che richiedono librerie JavaScript di terze parti e l'accesso alle API del browser, i componenti supportano l'interoperabilità con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-171">For apps that require third-party JavaScript libraries and access to browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="ff7a6-172">I componenti sono in grado di usare qualsiasi libreria o API supportata da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-172">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="ff7a6-173">Il codice C# può effettuare chiamate nel codice JavaScript e vice versa.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-173">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="ff7a6-174">Per altre informazioni, vedere <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-174">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="ff7a6-175">Condivisione del codice e .NET Standard</span><span class="sxs-lookup"><span data-stu-id="ff7a6-175">Code sharing and .NET Standard</span></span>

<span data-ttu-id="ff7a6-176">Blazor implementa [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="ff7a6-176">Blazor implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="ff7a6-177">.NET Standard è una specifica formale delle API .NET comuni tra le implementazioni di .NET.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-177">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="ff7a6-178">Le librerie di classi .NET Standard possono essere condivise tra diverse piattaforme .NET, come Blazor, .NET Framework, .NET Core, Xamarin, Mono e Unity.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-178">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="ff7a6-179">Le API non valide all'interno di un Web browser (ad esempio per l'accesso al file system, l'apertura di un socket e la gestione dei thread) generano un'eccezione <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="ff7a6-179">APIs that aren't applicable inside of a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff7a6-180">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ff7a6-180">Additional resources</span></span>

* [<span data-ttu-id="ff7a6-181">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="ff7a6-181">WebAssembly</span></span>](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [<span data-ttu-id="ff7a6-182">Guida a C#</span><span class="sxs-lookup"><span data-stu-id="ff7a6-182">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="ff7a6-183">HTML</span><span class="sxs-lookup"><span data-stu-id="ff7a6-183">HTML</span></span>](https://www.w3.org/html/)
