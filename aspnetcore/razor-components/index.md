---
title: Introduzione a Razor Components
author: guardrex
description: Esplorare ASP.NET Core Razor Components, un modo per creare un'interfaccia utente Web sul lato client interattiva con .NET in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/27/2019
uid: razor-components/index
ms.openlocfilehash: 8b2e87fe856598a5ac231e3bc1d413957829b448
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2019
ms.locfileid: "58751010"
---
# <a name="introduction-to-razor-components"></a><span data-ttu-id="09ce9-103">Introduzione a Razor Components</span><span class="sxs-lookup"><span data-stu-id="09ce9-103">Introduction to Razor Components</span></span>

<span data-ttu-id="09ce9-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="09ce9-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="09ce9-105">*Razor Components* è un modo per sviluppare un'interfaccia utente Web sul lato client interattiva con .NET:</span><span class="sxs-lookup"><span data-stu-id="09ce9-105">*Razor Components* is a way to build interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="09ce9-106">Sviluppare interfacce utente interattive avanzate usando C# invece che JavaScript.</span><span class="sxs-lookup"><span data-stu-id="09ce9-106">Build rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="09ce9-107">Condividere la logica dell'app, interamente scritta con .NET, sul lato client e sul lato server.</span><span class="sxs-lookup"><span data-stu-id="09ce9-107">Share server-side and client-side app logic all written with .NET.</span></span>
* <span data-ttu-id="09ce9-108">Eseguire il rendering dell'interfaccia utente come HTML e CSS per un ampio supporto dei browser, inclusi i browser per dispositivi mobili.</span><span class="sxs-lookup"><span data-stu-id="09ce9-108">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>

<span data-ttu-id="09ce9-109">Razor Components supporta gli elementi fondamentali richiesti dalla maggior parte delle app, tra cui:</span><span class="sxs-lookup"><span data-stu-id="09ce9-109">Razor Components supports core facilities required by most apps, including:</span></span>

* <span data-ttu-id="09ce9-110">Parametri</span><span class="sxs-lookup"><span data-stu-id="09ce9-110">Parameters</span></span>
* <span data-ttu-id="09ce9-111">Gestione di eventi</span><span class="sxs-lookup"><span data-stu-id="09ce9-111">Event handling</span></span>
* <span data-ttu-id="09ce9-112">Associazione dati</span><span class="sxs-lookup"><span data-stu-id="09ce9-112">Data binding</span></span>
* <span data-ttu-id="09ce9-113">Routing</span><span class="sxs-lookup"><span data-stu-id="09ce9-113">Routing</span></span>
* <span data-ttu-id="09ce9-114">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="09ce9-114">Dependency injection</span></span>
* <span data-ttu-id="09ce9-115">Layout</span><span class="sxs-lookup"><span data-stu-id="09ce9-115">Layouts</span></span>
* <span data-ttu-id="09ce9-116">Modelli</span><span class="sxs-lookup"><span data-stu-id="09ce9-116">Templating</span></span>
* <span data-ttu-id="09ce9-117">Valori a cascata</span><span class="sxs-lookup"><span data-stu-id="09ce9-117">Cascading values</span></span>

<span data-ttu-id="09ce9-118">Razor Components separa la logica di rendering dei componenti dal modo in cui vengono applicati gli aggiornamenti dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="09ce9-118">Razor Components decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="09ce9-119">ASP.NET Core Razor Components in .NET Core 3.0 aggiunge il supporto per l'hosting di Razor Components nel server in un'app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09ce9-119">ASP.NET Core Razor Components in .NET Core 3.0 adds support for hosting Razor Components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="09ce9-120">Tutti gli aggiornamenti dell'interfaccia utente vengono gestiti tramite una connessione SignalR.</span><span class="sxs-lookup"><span data-stu-id="09ce9-120">UI updates are handled over a SignalR connection.</span></span>

<span data-ttu-id="09ce9-121">Il runtime:</span><span class="sxs-lookup"><span data-stu-id="09ce9-121">The runtime:</span></span>

* <span data-ttu-id="09ce9-122">Gestisce l'invio degli eventi dell'interfaccia utente dal browser al server.</span><span class="sxs-lookup"><span data-stu-id="09ce9-122">Handles sending UI events from the browser to the server.</span></span>
* <span data-ttu-id="09ce9-123">Applica gli aggiornamenti dell'interfaccia utente inviati dal server nel browser dopo l'esecuzione dei componenti.</span><span class="sxs-lookup"><span data-stu-id="09ce9-123">Applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="09ce9-124">La connessione usata da Razor Components per comunicare con il browser viene usata anche per gestire le chiamate di interoperabilità JavaScript.</span><span class="sxs-lookup"><span data-stu-id="09ce9-124">The connection used by Razor Components to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![Razor Components esegue codice .NET sul server e interagisce con il modello DOM (Document Object Model) nel client tramite una connessione SignalR](index/_static/aspnet-core-razor-components.png)

<span data-ttu-id="09ce9-126">Per ulteriori informazioni, vedere <xref:razor-components/hosting-models#server-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="09ce9-126">For more information, see <xref:razor-components/hosting-models#server-side-hosting-model>.</span></span>

<span data-ttu-id="09ce9-127">*Blazor* è il modello di hosting sul lato client sperimentale per Razor Components.</span><span class="sxs-lookup"><span data-stu-id="09ce9-127">*Blazor* is the experimental client-side hosting model of Razor Components.</span></span> <span data-ttu-id="09ce9-128">Blazor viene eseguito in .NET nel browser tramite gli standard Web aperti senza plug-in o transpiling del codice.</span><span class="sxs-lookup"><span data-stu-id="09ce9-128">Blazor runs on .NET in the browser using open web standards without plugins or code transpilation.</span></span> <span data-ttu-id="09ce9-129">Per altre informazioni, vedere <xref:spa/blazor/index> e <xref:razor-components/hosting-models#client-side-hosting-model>.</span><span class="sxs-lookup"><span data-stu-id="09ce9-129">For more information, see <xref:spa/blazor/index> and <xref:razor-components/hosting-models#client-side-hosting-model>.</span></span>

## <a name="components"></a><span data-ttu-id="09ce9-130">Componenti</span><span class="sxs-lookup"><span data-stu-id="09ce9-130">Components</span></span>

<span data-ttu-id="09ce9-131">Un *componente Razor* è una parte dell'interfaccia utente, ad esempio una pagina, una finestra di dialogo o un modulo per l'immissione dei dati.</span><span class="sxs-lookup"><span data-stu-id="09ce9-131">A *Razor Component* is a piece of UI, such as a page, dialog, or data entry form.</span></span> <span data-ttu-id="09ce9-132">I componenti gestiscono gli eventi utente e definiscono la logica di rendering dell'interfaccia utente flessibile.</span><span class="sxs-lookup"><span data-stu-id="09ce9-132">Components handle user events and define flexible UI rendering logic.</span></span> <span data-ttu-id="09ce9-133">I componenti possono essere annidati e riutilizzati.</span><span class="sxs-lookup"><span data-stu-id="09ce9-133">Components can be nested and reused.</span></span>

<span data-ttu-id="09ce9-134">I componenti sono classi .NET compilate in assembly .NET che possono essere condivisi e distribuiti come pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="09ce9-134">Components are .NET classes built into .NET assemblies that can be shared and distributed as NuGet packages.</span></span> <span data-ttu-id="09ce9-135">La classe viene in genere scritta sotto forma di pagina di markup Razor in un file con estensione *.razor*.</span><span class="sxs-lookup"><span data-stu-id="09ce9-135">The class is normally written in the form of a Razor markup page with a *.razor* file extension.</span></span>

<span data-ttu-id="09ce9-136">[Razor](xref:mvc/views/razor) è una sintassi per la combinazione di markup HTML con codice C#.</span><span class="sxs-lookup"><span data-stu-id="09ce9-136">[Razor](xref:mvc/views/razor) is a syntax for combining HTML markup with C# code.</span></span> <span data-ttu-id="09ce9-137">Razor è progettato per la produttività degli sviluppatori, consentendo loro di alternare markup e C# nello stesso file con il supporto di [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="09ce9-137">Razor is designed for developer productivity, allowing the developer to switch between markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="09ce9-138">Anche le pagine Razor e le viste MVC usano Razor.</span><span class="sxs-lookup"><span data-stu-id="09ce9-138">Razor Pages and MVC views also use Razor.</span></span> <span data-ttu-id="09ce9-139">Diversamente dalle pagine Razor e dalle viste MVC, che sono basate su un modello di richiesta/risposta, i componenti vengono usati in modo specifico per la gestione della composizione dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="09ce9-139">Unlike Razor Pages and MVC views, which are built around a request/response model, components are used specifically for handling UI composition.</span></span> <span data-ttu-id="09ce9-140">È possibile usare Razor Components in modo specifico per la composizione e la logica dell'interfaccia utente sul lato client.</span><span class="sxs-lookup"><span data-stu-id="09ce9-140">Razor Components can be used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="09ce9-141">Il markup seguente è un esempio di un componente finestra di dialogo personalizzato in un file Razor (*DialogComponent.razor*):</span><span class="sxs-lookup"><span data-stu-id="09ce9-141">The following markup is an example of a custom dialog component in a Razor file (*DialogComponent.razor*):</span></span>

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

<span data-ttu-id="09ce9-142">Quando questo componente viene usato in altre posizioni nell'app, IntelliSense accelera lo sviluppo con il completamento di sintassi e parametri.</span><span class="sxs-lookup"><span data-stu-id="09ce9-142">When this component is used elsewhere in the app, IntelliSense speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="09ce9-143">Il rendering dei componenti viene eseguito in una rappresentazione in memoria del modello DOM del browser nota come *albero di rendering*, che può quindi essere usata per aggiornare l'interfaccia utente in modo flessibile ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="09ce9-143">Components render into an in-memory representation of the browser DOM called a *render tree* that can then be used to update the UI in a flexible and efficient way.</span></span>

## <a name="javascript-interop"></a><span data-ttu-id="09ce9-144">Interoperabilità JavaScript</span><span class="sxs-lookup"><span data-stu-id="09ce9-144">JavaScript interop</span></span>

<span data-ttu-id="09ce9-145">Per le app che richiedono librerie JavaScript e API browser di terze parti, i componenti supportano l'interoperabilità con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="09ce9-145">For apps that require third-party JavaScript libraries and browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="09ce9-146">I componenti sono in grado di usare qualsiasi libreria o API supportata da JavaScript.</span><span class="sxs-lookup"><span data-stu-id="09ce9-146">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="09ce9-147">Il codice C# può effettuare chiamate nel codice JavaScript e vice versa.</span><span class="sxs-lookup"><span data-stu-id="09ce9-147">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="09ce9-148">Per altre informazioni, vedere [Interoperabilità JavaScript](xref:razor-components/javascript-interop).</span><span class="sxs-lookup"><span data-stu-id="09ce9-148">For more information, see [JavaScript interop](xref:razor-components/javascript-interop).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09ce9-149">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="09ce9-149">Additional resources</span></span>

* <xref:spa/blazor/index>
* [<span data-ttu-id="09ce9-150">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="09ce9-150">WebAssembly</span></span>](http://webassembly.org/)
* [<span data-ttu-id="09ce9-151">Guida a C#</span><span class="sxs-lookup"><span data-stu-id="09ce9-151">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="09ce9-152">HTML</span><span class="sxs-lookup"><span data-stu-id="09ce9-152">HTML</span></span>](https://www.w3.org/html/)
