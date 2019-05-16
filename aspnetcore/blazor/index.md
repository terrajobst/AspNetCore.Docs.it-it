---
title: Introduzione a Blazor in ASP.NET Core
author: guardrex
description: Esplorare ASP.NET Core Blazor, un modo per creare un'interfaccia utente Web sul lato client interattiva con .NET in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 05/01/2019
uid: blazor/index
ms.openlocfilehash: bd7d2d3e6702844627f19dfbbbad5c52389a52e5
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085774"
---
# <a name="introduction-to-blazor"></a><span data-ttu-id="2bc12-103">Introduzione a Blazor</span><span class="sxs-lookup"><span data-stu-id="2bc12-103">Introduction to Blazor</span></span>

<span data-ttu-id="2bc12-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2bc12-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2bc12-105">*Questo articolo offre un'introduzione a Blazor.*</span><span class="sxs-lookup"><span data-stu-id="2bc12-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="2bc12-106">Blazor è un framework per la compilazione di un'interfaccia utente Web lato client interattiva con .NET:</span><span class="sxs-lookup"><span data-stu-id="2bc12-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="2bc12-107">Creare interfacce utente interattive avanzate con C# invece di JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2bc12-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="2bc12-108">Condividere la logica dell'app scritta con .NET, sul lato client e sul lato server.</span><span class="sxs-lookup"><span data-stu-id="2bc12-108">Share server-side and client-side app logic written with .NET.</span></span>
* <span data-ttu-id="2bc12-109">Eseguire il rendering dell'interfaccia utente come HTML e CSS per un ampio supporto dei browser, inclusi i browser per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="2bc12-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="2bc12-110">L'uso di .NET per lo sviluppo Web lato client offre i vantaggi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2bc12-110">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="2bc12-111">scrivere codice in C# invece che in JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2bc12-111">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="2bc12-112">Permette di sfruttare l'ecosistema .NET esistente di librerie .NET.</span><span class="sxs-lookup"><span data-stu-id="2bc12-112">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="2bc12-113">Permette di condividere la logica dell'app tra il server e il client.</span><span class="sxs-lookup"><span data-stu-id="2bc12-113">Share app logic across the server and client.</span></span>
* <span data-ttu-id="2bc12-114">Permette di ottenere le prestazioni, l'affidabilità e la sicurezza di .NET.</span><span class="sxs-lookup"><span data-stu-id="2bc12-114">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="2bc12-115">produttività con Visual Studio in Windows, Linux e macOS.</span><span class="sxs-lookup"><span data-stu-id="2bc12-115">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="2bc12-116">basato su un set comune di linguaggi, framework e strumenti che sono stabili, ricchi di funzionalità e facili da usare.</span><span class="sxs-lookup"><span data-stu-id="2bc12-116">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="2bc12-117">Componenti</span><span class="sxs-lookup"><span data-stu-id="2bc12-117">Components</span></span>

<span data-ttu-id="2bc12-118">Le app Blazor sono basate su *componenti*.</span><span class="sxs-lookup"><span data-stu-id="2bc12-118">Blazor apps are based on *components*.</span></span> <span data-ttu-id="2bc12-119">Un componente in Blazor è un elemento dell'interfaccia utente, ad esempio una pagina, una finestra di dialogo o un modulo per l'immissione di dati.</span><span class="sxs-lookup"><span data-stu-id="2bc12-119">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="2bc12-120">I componenti gestiscono gli eventi utente e definiscono la logica di rendering dell'interfaccia utente flessibile.</span><span class="sxs-lookup"><span data-stu-id="2bc12-120">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="2bc12-121">I componenti possono essere annidati e riutilizzati.</span><span class="sxs-lookup"><span data-stu-id="2bc12-121">Components can be nested and reused.</span></span>

<span data-ttu-id="2bc12-122">I componenti sono classi .NET compilate in assembly .NET che possono essere condivisi e distribuiti come [pacchetti NuGet](/nuget/what-is-nuget).</span><span class="sxs-lookup"><span data-stu-id="2bc12-122">Components are .NET classes built into .NET assemblies that can be shared and distributed as [NuGet packages](/nuget/what-is-nuget).</span></span> <span data-ttu-id="2bc12-123">La classe dei componenti viene in genere scritta sotto forma di pagina di markup di Razor con un'estensione di file *razor*.</span><span class="sxs-lookup"><span data-stu-id="2bc12-123">The component class is usually written in the form of a Razor markup page with a *.razor* file extension.</span></span>

<span data-ttu-id="2bc12-124">I componenti in Blazor vengono talvolta denominati *componenti Razor*.</span><span class="sxs-lookup"><span data-stu-id="2bc12-124">Components in Blazor are sometimes referred to as *Razor components*.</span></span> <span data-ttu-id="2bc12-125">[Razor](xref:mvc/views/razor) è una sintassi per la combinazione di markup HTML con codice C# progettata per la produttività degli sviluppatori.</span><span class="sxs-lookup"><span data-stu-id="2bc12-125">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="2bc12-126">Razor permette di passare tra markup HTML e C# nello stesso file con supporto per [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="2bc12-126">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="2bc12-127">Anche Razor Pages ed MVC usano Razor.</span><span class="sxs-lookup"><span data-stu-id="2bc12-127">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="2bc12-128">Diversamente da Razor Pages ed MVC, che sono basati su un modello di richiesta/risposta, i componenti vengono usati in modo specifico per la logica e la composizione dell'interfaccia utente lato client.</span><span class="sxs-lookup"><span data-stu-id="2bc12-128">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="2bc12-129">Il markup Razor seguente mostra un componente (*Dialog.razor*), che può essere annidato all'interno di un altro componente:</span><span class="sxs-lookup"><span data-stu-id="2bc12-129">The following Razor markup demonstrates a component (*Dialog.razor*), which can be nested within another component:</span></span>

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button onclick="@OnYes">Yes!</button>
</div>

@functions {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#!");
    }
}
```

<span data-ttu-id="2bc12-130">Il contenuto del corpo della finestra di dialogo (`ChildContent`) e il titolo (`Title`) vengono forniti dal componente che usa questo componente nella propria interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="2bc12-130">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="2bc12-131">`OnYes` è un metodo C# attivato dall'evento `onclick` del pulsante.</span><span class="sxs-lookup"><span data-stu-id="2bc12-131">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="2bc12-132">Blazor usa tag HTML naturali per la composizione dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="2bc12-132">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="2bc12-133">Gli elementi HTML specificano i componenti e gli attributi di un tag passano valori alle proprietà del componente.</span><span class="sxs-lookup"><span data-stu-id="2bc12-133">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span> <span data-ttu-id="2bc12-134">`ChildContent` e `Title` vengono impostati dal componente che usa il componente Dialog (*Index.razor*):</span><span class="sxs-lookup"><span data-stu-id="2bc12-134">`ChildContent` and `Title` are set by the component that uses the Dialog component (*Index.razor*):</span></span>

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="2bc12-135">Il rendering della finestra di dialogo viene eseguito quando viene effettuato l'accesso all'elemento padre (*Index.razor*) in un browser:</span><span class="sxs-lookup"><span data-stu-id="2bc12-135">The dialog is rendered when the parent (*Index.razor*) is accessed in a browser:</span></span>

![Rendering del componente Dialog nel browser](index/_static/dialog.png)

<span data-ttu-id="2bc12-137">Quando questo componente viene usato nell'app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) e [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) accelera lo sviluppo con il completamento di sintassi e parametri.</span><span class="sxs-lookup"><span data-stu-id="2bc12-137">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="2bc12-138">Il rendering dei componenti viene eseguito in una rappresentazione in memoria del modello DOM del browser denominata *albero di rendering*, usato per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="2bc12-138">Components render into an in-memory representation of the browser DOM called a *render tree* that's used to update the UI in a flexible and efficient way.</span></span>

## <a name="blazor-client-side"></a><span data-ttu-id="2bc12-139">Blazor sul lato client</span><span class="sxs-lookup"><span data-stu-id="2bc12-139">Blazor client-side</span></span>

<span data-ttu-id="2bc12-140">Il lato client di Blazor è un framework per app a pagina singola per la compilazione di app Web lato client interattive con .NET.</span><span class="sxs-lookup"><span data-stu-id="2bc12-140">Blazor client-side is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="2bc12-141">Il lato client di Blazor usa standard Web aperti senza plug-in o transpile del codice e funziona in tutti i Web browser moderni, inclusi quelli per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="2bc12-141">Blazor client-side uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="2bc12-142">L'esecuzione di codice .NET all'interno di Web browser è resa possibile da [WebAssembly](http://webassembly.org) (tecnologia nota anche con l'abbreviazione *wasm*).</span><span class="sxs-lookup"><span data-stu-id="2bc12-142">Running .NET code inside web browsers is made possible by [WebAssembly](http://webassembly.org) (abbreviated *wasm*).</span></span> <span data-ttu-id="2bc12-143">WebAssembly è un standard Web aperto ed è supportato nei Web browser senza plug-in.</span><span class="sxs-lookup"><span data-stu-id="2bc12-143">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span> <span data-ttu-id="2bc12-144">WebAssembly è un formato bytecode compatto ottimizzato per il download veloce e la velocità massima di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2bc12-144">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span>

<span data-ttu-id="2bc12-145">Il codice WebAssembly può accedere a tutte le funzionalità del browser tramite l'\*interoperabilità JavaScript\*\*\*.</span><span class="sxs-lookup"><span data-stu-id="2bc12-145">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="2bc12-146">Il codice .NET eseguito tramite WebAssembly nel browser viene eseguito nella stessa sandbox attendibile di JavaScript e questo comportamento elimina praticamente la possibilità per un'app di eseguire azioni dannose nel computer client.</span><span class="sxs-lookup"><span data-stu-id="2bc12-146">.NET code executed via WebAssembly in the browser runs in the same trusted sandbox as JavaScript, which virtually eliminates the opportunity for an app to perform malicious actions on the client machine.</span></span>

![Blazor sul lato client esegue codice .NET nel browser con WebAssembly.](index/_static/blazor-client-side.png)

<span data-ttu-id="2bc12-148">Quando un'app Blazor sul lato client viene compilata ed eseguita in un browser:</span><span class="sxs-lookup"><span data-stu-id="2bc12-148">When a Blazor client-side app is built and run in a browser:</span></span>

* <span data-ttu-id="2bc12-149">I file di codice C# e i file Razor vengono compilati in assembly .NET.</span><span class="sxs-lookup"><span data-stu-id="2bc12-149">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="2bc12-150">Gli assembly e il runtime .NET vengono scaricati nel browser.</span><span class="sxs-lookup"><span data-stu-id="2bc12-150">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="2bc12-151">Blazor sul lato client esegue il bootstrap del runtime .NET e configura il runtime per caricare gli assembly per l'app.</span><span class="sxs-lookup"><span data-stu-id="2bc12-151">Blazor client-side bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="2bc12-152">Il runtime lato client di Blazor usa l'interoperabilità JavaScript per gestire la modifica di modelli DOM (Document Object Model) e chiamate API del browser.</span><span class="sxs-lookup"><span data-stu-id="2bc12-152">The Blazor client-side runtime uses JavaScript interop to handle Document Object Model (DOM) manipulation and browser API calls.</span></span>

<span data-ttu-id="2bc12-153">La dimensione dell'app pubblicata, ovvero la *dimensione del payload*, è un fattore cruciale per le prestazioni ai fini dell'usabilità dell'app.</span><span class="sxs-lookup"><span data-stu-id="2bc12-153">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="2bc12-154">Un'app di grandi dimensioni impiega relativamente molto tempo a essere scaricata in un browser, influendo negativamente sull'esperienza utente.</span><span class="sxs-lookup"><span data-stu-id="2bc12-154">A large app takes a relatively long time to download to a browser, which hurts the user experience.</span></span> <span data-ttu-id="2bc12-155">Blazor sul lato client ottimizza le dimensioni del payload per ridurre i tempi di download:</span><span class="sxs-lookup"><span data-stu-id="2bc12-155">Blazor client-side optimizes payload size to reduce download times:</span></span>

* <span data-ttu-id="2bc12-156">Il codice non usato viene rimosso dall'app quando questa viene pubblicata dal [linker del linguaggio intermedio](xref:host-and-deploy/blazor/configure-linker).</span><span class="sxs-lookup"><span data-stu-id="2bc12-156">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:host-and-deploy/blazor/configure-linker).</span></span>
* <span data-ttu-id="2bc12-157">Le risposte HTTP vengono compresse.</span><span class="sxs-lookup"><span data-stu-id="2bc12-157">HTTP responses are compressed.</span></span>
* <span data-ttu-id="2bc12-158">Il runtime e gli assembly .NET vengono memorizzati nella cache nel browser.</span><span class="sxs-lookup"><span data-stu-id="2bc12-158">The .NET runtime and assemblies are cached in the browser.</span></span>

<span data-ttu-id="2bc12-159">Per altre informazioni e indicazioni sulla scelta di un modello di hosting, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2bc12-159">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="blazor-server-side"></a><span data-ttu-id="2bc12-160">Blazor sul lato server</span><span class="sxs-lookup"><span data-stu-id="2bc12-160">Blazor server-side</span></span>

<span data-ttu-id="2bc12-161">Blazor separa la logica di rendering dei componenti dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="2bc12-161">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="2bc12-162">Blazor sul lato server fornisce il supporto per l'hosting di componenti Razor nel server in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2bc12-162">Blazor server-side provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="2bc12-163">Gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="2bc12-163">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="2bc12-164">Il runtime gestisce l'invio di eventi dell'interfaccia utente dal browser al server e quindi applica gli aggiornamenti dell'interfaccia utente inviati dal server di nuovo al browser dopo l'esecuzione dei componenti.</span><span class="sxs-lookup"><span data-stu-id="2bc12-164">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="2bc12-165">La connessione usata da Blazor sul lato server per comunicare con il browser viene usata anche per gestire le chiamate di interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2bc12-165">The connection used by Blazor server-side to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Blazor sul lato server esegue codice .NET sul server e interagisce con il modello DOM (Document Object Model) nel client tramite una connessione SignalR](index/_static/blazor-server-side.png)

<span data-ttu-id="2bc12-167">Per altre informazioni e indicazioni sulla scelta di un modello di hosting, vedere <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="2bc12-167">For more information and guidance on choosing a hosting model, see <xref:blazor/hosting-models>.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="2bc12-168">Interoperabilità JavaScript</span><span class="sxs-lookup"><span data-stu-id="2bc12-168">JavaScript interop</span></span>

<span data-ttu-id="2bc12-169">Per le app che richiedono librerie JavaScript e API browser di terze parti, i componenti supportano l'interoperabilità con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2bc12-169">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="2bc12-170">I componenti sono in grado di usare qualsiasi libreria o API supportata da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2bc12-170">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="2bc12-171">Il codice C# può effettuare chiamate nel codice JavaScript e vice versa.</span><span class="sxs-lookup"><span data-stu-id="2bc12-171">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="2bc12-172">Per ulteriori informazioni, vedere <xref:blazor/javascript-interop>.</span><span class="sxs-lookup"><span data-stu-id="2bc12-172">For more information, see <xref:blazor/javascript-interop>.</span></span>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="2bc12-173">Condivisione del codice e .NET Standard</span><span class="sxs-lookup"><span data-stu-id="2bc12-173">Code sharing and .NET Standard</span></span>

<span data-ttu-id="2bc12-174">Blazor implementa [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="2bc12-174">Blazor implements [.NET Standard 2.0](/dotnet/standard/net-standard).</span></span> <span data-ttu-id="2bc12-175">.NET Standard è una specifica formale delle API .NET comuni tra le implementazioni di .NET.</span><span class="sxs-lookup"><span data-stu-id="2bc12-175">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="2bc12-176">Le librerie di classi .NET Standard possono essere condivise tra diverse piattaforme .NET, come Blazor, .NET Framework, .NET Core, Xamarin, Mono e Unity.</span><span class="sxs-lookup"><span data-stu-id="2bc12-176">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="2bc12-177">Le API non valide all'interno di un Web browser (ad esempio per l'accesso al file system, l'apertura di un socket e la gestione dei thread) generano un'eccezione <xref:System.PlatformNotSupportedException>.</span><span class="sxs-lookup"><span data-stu-id="2bc12-177">APIs that aren't applicable inside a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2bc12-178">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2bc12-178">Additional resources</span></span>

* [<span data-ttu-id="2bc12-179">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="2bc12-179">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="2bc12-180">Guida a C#</span><span class="sxs-lookup"><span data-stu-id="2bc12-180">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="2bc12-181">HTML</span><span class="sxs-lookup"><span data-stu-id="2bc12-181">HTML</span></span>](https://www.w3.org/html/)
