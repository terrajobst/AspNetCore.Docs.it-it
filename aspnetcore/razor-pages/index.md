---
title: Introduzione a Razor Pages in ASP.NET Core
author: Rick-Anderson
description: Informazioni su come Razor Pages in ASP.NET Core semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine rispetto a MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 10/07/2019
uid: razor-pages/index
ms.openlocfilehash: d12cf7f4f45c98c292b0d035c99e051d9b779e9a
ms.sourcegitcommit: 383017d7060a6d58f6a79cf4d7335d5b4b6c5659
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72816131"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="2ac6b-103">Introduzione a Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2ac6b-103">Introduction to Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2ac6b-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="2ac6b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="2ac6b-105">Razor Pages possibile rendere più semplici e più produttivi gli scenari incentrati sulle pagine di codifica rispetto all'utilizzo di controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="2ac6b-106">Se si sta cercando un'esercitazione in cui si usa l'approccio Model-View-Controller, vedere [Introduzione ad ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="2ac6b-107">Questo documento offre un'introduzione a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="2ac6b-108">Non è un'esercitazione dettagliata.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="2ac6b-109">Se alcune sezioni risultano troppo avanzate, vedere [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="2ac6b-110">Per una panoramica di ASP.NET Core, vedere [Introduzione a ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ac6b-111">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="2ac6b-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2ac6b-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ac6b-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2ac6b-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ac6b-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2ac6b-114">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2ac6b-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="2ac6b-115">Creare un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2ac6b-115">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2ac6b-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ac6b-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2ac6b-117">Per istruzioni dettagliate su come creare un progetto Razor Pages, vedere [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2ac6b-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ac6b-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2ac6b-119">Eseguire `dotnet new webapp` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-119">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2ac6b-120">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2ac6b-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2ac6b-121">Eseguire `dotnet new webapp` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-121">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="2ac6b-122">Aprire il file *CSPROJ* generato da Visual Studio per Mac.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="2ac6b-123">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2ac6b-123">Razor Pages</span></span>

<span data-ttu-id="2ac6b-124">La funzionalità Razor Pages è abilitata in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-124">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12)]

<span data-ttu-id="2ac6b-125">Si consideri una pagina di base: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="2ac6b-125">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="2ac6b-126">Il codice precedente è molto simile a un [file di visualizzazione Razor](xref:tutorials/first-mvc-app/adding-view) usato in un'app ASP.NET Core con controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-126">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="2ac6b-127">Ciò che lo rende diverso è la direttiva [@page](xref:mvc/views/razor#page) .</span><span class="sxs-lookup"><span data-stu-id="2ac6b-127">What makes it different is the [@page](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="2ac6b-128">`@page` trasforma il file in un'azione MVC, ovvero gestisce le richieste direttamente, senza passare attraverso un controller.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-128">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="2ac6b-129">`@page` deve essere la prima direttiva Razor in una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-129">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="2ac6b-130">`@page` influiscono sul comportamento di altri costrutti [Razor](xref:mvc/views/razor) .</span><span class="sxs-lookup"><span data-stu-id="2ac6b-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span></span> <span data-ttu-id="2ac6b-131">I nomi dei file di Razor Pages hanno un suffisso *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="2ac6b-131">Razor Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="2ac6b-132">Nei due file seguenti viene visualizzata una pagina simile che usa una classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-132">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="2ac6b-133">Il file *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-133">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="2ac6b-134">Il modello di pagina *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-134">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="2ac6b-135">Per convenzione, il file di classe `PageModel` ha lo stesso nome del file della pagina Razor con l'aggiunta di *.cs*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-135">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="2ac6b-136">Ad esempio, la pagina Razor precedente è *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-136">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="2ac6b-137">Il file che contiene la classe `PageModel` è denominato *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-137">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="2ac6b-138">Le associazioni dei percorsi URL alle pagine sono determinate dalla posizione della pagina nel file system.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-138">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="2ac6b-139">Nella tabella seguente sono riportati alcuni percorsi di pagina Razor con gli URL corrispondenti:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-139">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="2ac6b-140">Percorso e nome file</span><span class="sxs-lookup"><span data-stu-id="2ac6b-140">File name and path</span></span>               | <span data-ttu-id="2ac6b-141">URL corrispondente</span><span class="sxs-lookup"><span data-stu-id="2ac6b-141">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="2ac6b-142">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-142">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="2ac6b-143">`/` o `/Index`</span><span class="sxs-lookup"><span data-stu-id="2ac6b-143">`/` or `/Index`</span></span> |
| <span data-ttu-id="2ac6b-144">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-144">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="2ac6b-145">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-145">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="2ac6b-146">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-146">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="2ac6b-147">`/Store` o `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="2ac6b-147">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="2ac6b-148">Note:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-148">Notes:</span></span>

* <span data-ttu-id="2ac6b-149">Il runtime cerca i file delle pagine Razor nella cartella *Pages* per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-149">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="2ac6b-150">`Index` è la pagina predefinita quando un URL non include una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-150">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="2ac6b-151">Scrivere un form di base</span><span class="sxs-lookup"><span data-stu-id="2ac6b-151">Write a basic form</span></span>

<span data-ttu-id="2ac6b-152">Razor Pages semplifica l'implementazione dei modelli normalmente usati con i Web browser durante la creazione di un'app.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-152">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="2ac6b-153">L'[associazione di modelli](xref:mvc/models/model-binding), gli [helper tag](xref:mvc/views/tag-helpers/intro) e gli helper HTML *funzionano* tutti con le proprietà definite in una classe di pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-153">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="2ac6b-154">Si consideri una pagina che implementa un form "contact us" di base per il modello `Contact`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-154">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="2ac6b-155">Per gli esempi riportati in questo documento, `DbContext` viene inizializzato nel file [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-155">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="2ac6b-156">Il modello di dati:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-156">The data model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="2ac6b-157">Il contesto del database:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-157">The db context:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="2ac6b-158">Il file di visualizzazione *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-158">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="2ac6b-159">Il modello di pagina *Pages/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-159">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="2ac6b-160">Per convenzione, la classe `PageModel` è denominata `<PageName>Model` e si trova nello stesso spazio dei nomi della pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-160">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="2ac6b-161">La classe `PageModel` consente la separazione della logica di una pagina dalla relativa presentazione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-161">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="2ac6b-162">Definisce i gestori di pagina per le richieste inviate alla pagina e i dati usati per il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-162">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="2ac6b-163">Questa separazione consente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-163">This separation allows:</span></span>

* <span data-ttu-id="2ac6b-164">Gestione delle dipendenze di pagina tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="2ac6b-165">Testing unità</span><span class="sxs-lookup"><span data-stu-id="2ac6b-165">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="2ac6b-166">La pagina contiene un oggetto `OnPostAsync`, ovvero un *metodo gestore* che viene eseguito per le richieste `POST`, quando un utente invia il form.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="2ac6b-167">È possibile aggiungere i metodi del gestore per qualsiasi verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-167">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="2ac6b-168">I gestori più comuni sono:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-168">The most common handlers are:</span></span>

* <span data-ttu-id="2ac6b-169">`OnGet` per inizializzare lo stato necessario per la pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="2ac6b-170">Nel codice precedente, il metodo `OnGet` Visualizza la pagina Razor *CreateModel. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="2ac6b-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span></span>
* <span data-ttu-id="2ac6b-171">`OnPost` per gestire gli invii di form.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="2ac6b-172">Il suffisso `Async` nel nome è facoltativo, ma viene spesso usato per convenzione per le funzioni asincrone.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="2ac6b-173">Il codice precedente è tipico di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-173">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="2ac6b-174">Se si ha familiarità con le app ASP.NET che usano i controller e le visualizzazioni:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-174">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="2ac6b-175">Il codice `OnPostAsync` nell'esempio precedente ha un aspetto simile al codice del controller tipico.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="2ac6b-176">La maggior parte delle primitive MVC come l' [associazione di modelli](xref:mvc/models/model-binding), la [convalida](xref:mvc/models/validation)e i risultati dell'azione funzionano allo stesso modo con i controller e Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span></span> 

<span data-ttu-id="2ac6b-177">Il metodo `OnPostAsync` precedente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="2ac6b-178">Il flusso di base di `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="2ac6b-179">Verificare se sono presenti errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-179">Check for validation errors.</span></span>

* <span data-ttu-id="2ac6b-180">Se non sono presenti errori, salvare i dati e reindirizzare.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-180">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="2ac6b-181">Se sono presenti errori, visualizzare di nuovo la pagina con i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="2ac6b-182">In molti casi gli errori di convalida vengono rilevati nel client e mai inviati al server.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="2ac6b-183">Il file di visualizzazione *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-183">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="2ac6b-184">Il codice HTML sottoposto a rendering da *pages/create. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-184">The rendered HTML from *Pages/Create.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="2ac6b-185">Nel codice precedente, inserendo il modulo:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-185">In the previous code, posting the form:</span></span>

* <span data-ttu-id="2ac6b-186">Con dati validi:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-186">With valid data:</span></span>

  * <span data-ttu-id="2ac6b-187">Il metodo del gestore `OnPostAsync` chiama il metodo helper <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*>.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="2ac6b-188">`RedirectToPage` restituisce un'istanza di <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-188">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="2ac6b-189">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-189">`RedirectToPage`:</span></span>

    * <span data-ttu-id="2ac6b-190">Risultato dell'azione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-190">Is an action result.</span></span>
    * <span data-ttu-id="2ac6b-191">È simile a `RedirectToAction` o `RedirectToRoute` (usato in controller e visualizzazioni).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="2ac6b-192">È personalizzato per le pagine.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-192">Is customized for pages.</span></span> <span data-ttu-id="2ac6b-193">Nell'esempio precedente viene reindirizzato alla pagina di indice radice (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="2ac6b-194">Il metodo `RedirectToPage` è descritto in dettaglio nella sezione [Generazione di URL per le pagine](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="2ac6b-195">Con gli errori di convalida passati al server:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-195">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="2ac6b-196">Il metodo del gestore `OnPostAsync` chiama il metodo helper <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*>.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="2ac6b-197">`Page` restituisce un'istanza di <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-197">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span></span> <span data-ttu-id="2ac6b-198">La restituzione di `Page` è simile al modo in cui le azioni nel controller restituiscono `View`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-198">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="2ac6b-199">`PageResult` è il tipo restituito predefinito per un metodo del gestore.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-199">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="2ac6b-200">Un metodo gestore che restituisce `void` esegue il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-200">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="2ac6b-201">Nell'esempio precedente, se si pubblica il form senza alcun valore, in [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) viene restituito false.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="2ac6b-202">In questo esempio non vengono visualizzati errori di convalida nel client.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-202">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="2ac6b-203">Il controllo degli errori di convalida viene trattato più avanti in questo documento.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-203">Validation error handing is covered later in this document.</span></span>

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="2ac6b-204">Con errori di convalida rilevati dalla convalida lato client:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-204">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="2ac6b-205">I dati **non** vengono inviati al server.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-205">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="2ac6b-206">La convalida lato client è illustrata più avanti in questo documento.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-206">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="2ac6b-207">La proprietà `Customer` utilizza [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attributo per acconsentire esplicitamente all'associazione di modelli:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="2ac6b-208">**non** utilizzare `[BindProperty]` nei modelli contenenti proprietà che non devono essere modificate dal client.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="2ac6b-209">Per ulteriori informazioni, vedere [overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span></span>

<span data-ttu-id="2ac6b-210">Per impostazione predefinita, Razor Pages associa le proprietà solo a verbi non `GET`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-210">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="2ac6b-211">L'associazione alle proprietà Elimina la necessità di scrivere codice per convertire i dati HTTP nel tipo di modello.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="2ac6b-212">Riduce il codice usando la stessa proprietà per il eseguire il rendering dei campi del form (`<input asp-for="Customer.Name">`) e accettare l'input.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-212">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="2ac6b-213">Esaminando il file di visualizzazione *pages/create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2ac6b-213">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="2ac6b-214">Nel codice precedente, l' [Helper tag di input](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` associa l'elemento HTML `<input>` all'espressione del modello `Customer.Name`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="2ac6b-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) rende disponibili gli helper tag.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="2ac6b-216">home page</span><span class="sxs-lookup"><span data-stu-id="2ac6b-216">The home page</span></span>

<span data-ttu-id="2ac6b-217">*Index. cshtml* è il Home page:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-217">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="2ac6b-218">La classe `PageModel` associata (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="2ac6b-218">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="2ac6b-219">Il file *index. cshtml* contiene il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-219">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="2ac6b-220">L' [Helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) `<a /a>` ha usato l'attributo `asp-route-{value}` per generare un collegamento alla pagina di modifica.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-220">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="2ac6b-221">Il collegamento contiene i dati della route con l'ID contatto.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="2ac6b-222">Ad esempio `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-222">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="2ac6b-223">Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-223">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>

<span data-ttu-id="2ac6b-224">Il file *index. cshtml* contiene il markup per la creazione di un pulsante Delete per ogni contatto del cliente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="2ac6b-225">HTML sottoposto a rendering:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-225">The rendered HTML:</span></span>

```HTML
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="2ac6b-226">Quando il pulsante Elimina viene sottoposto a rendering in HTML, il relativo [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) include i parametri per:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="2ac6b-227">ID contatto del cliente, specificato dall'attributo `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-227">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="2ac6b-228">`handler`specificato dall'attributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-228">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="2ac6b-229">Quando il pulsante è selezionato, viene inviata una richiesta `POST` di modulo al server.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-229">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="2ac6b-230">Per convenzione, il nome del metodo del gestore viene selezionato in base al valore del parametro `handler` in base allo schema `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-230">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="2ac6b-231">Poiché in questo esempio l'`handler` è `delete`, il metodo `OnPostDeleteAsync` viene usato per elaborare la richiesta `POST`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-231">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="2ac6b-232">Se `asp-page-handler` viene impostato su un valore diverso, ad esempio `remove`, viene selezionato un metodo gestore con il nome `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-232">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="2ac6b-233">Il metodo `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-233">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="2ac6b-234">Ottiene l'`id` dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-234">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="2ac6b-235">Interroga il database in merito al contatto del cliente con `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-235">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="2ac6b-236">Se il contatto del cliente viene trovato, viene rimosso e il database viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-236">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="2ac6b-237">Chiama <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> per reindirizzare alla pagina di indice radice (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-237">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="2ac6b-238">File Edit. cshtml</span><span class="sxs-lookup"><span data-stu-id="2ac6b-238">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="2ac6b-239">La prima riga contiene la direttiva `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-239">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="2ac6b-240">Il vincolo di routing `"{id:int}"` indica alla pagina di accettare le richieste inviate alla pagina che contengono i dati della route `int`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-240">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="2ac6b-241">Se una richiesta inviata alla pagina non contiene dati della route che possono essere convertiti in un oggetto `int`, il runtime restituisce un errore HTTP 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-241">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="2ac6b-242">Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-242">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="2ac6b-243">Il file *Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="2ac6b-243">The *Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="2ac6b-244">Convalida</span><span class="sxs-lookup"><span data-stu-id="2ac6b-244">Validation</span></span>

<span data-ttu-id="2ac6b-245">Regole di convalida:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-245">Validation rules:</span></span>

* <span data-ttu-id="2ac6b-246">Sono specificati in modo dichiarativo nella classe del modello.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-246">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="2ac6b-247">Vengono applicati ovunque nell'app.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-247">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="2ac6b-248">Lo spazio dei nomi <xref:System.ComponentModel.DataAnnotations> fornisce un set di attributi di convalida predefiniti che vengono applicati in modo dichiarativo a una classe o a una proprietà.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="2ac6b-249">DataAnnotations contiene anche attributi di formattazione come [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) che semplificano la formattazione e non forniscono alcuna convalida.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="2ac6b-250">Si consideri il modello di `Customer`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-250">Consider the `Customer` model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="2ac6b-251">Utilizzando il seguente file di visualizzazione *create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2ac6b-251">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="2ac6b-252">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-252">The preceding code:</span></span>

* <span data-ttu-id="2ac6b-253">Include gli script di convalida jQuery e jQuery.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-253">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="2ac6b-254">USA gli [Helper Tag](xref:mvc/views/tag-helpers/intro) `<div />` e `<span />` per abilitare:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="2ac6b-255">Convalida lato client.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-255">Client-side validation.</span></span>
  * <span data-ttu-id="2ac6b-256">Rendering degli errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-256">Validation error rendering.</span></span>

* <span data-ttu-id="2ac6b-257">Viene generato il codice HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-257">Generates the following HTML:</span></span>

  [!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]

<span data-ttu-id="2ac6b-258">Se si pubblica il modulo di creazione senza un valore di nome, viene visualizzato il messaggio di errore "il campo nome è obbligatorio".</span><span class="sxs-lookup"><span data-stu-id="2ac6b-258">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="2ac6b-259">nel form.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-259">on the form.</span></span> <span data-ttu-id="2ac6b-260">Se JavaScript è abilitato nel client, il browser Visualizza l'errore senza inviare al server.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="2ac6b-261">L'attributo `[StringLength(10)]` genera `data-val-length-max="10"` sul codice HTML sottoposto a rendering.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="2ac6b-262">`data-val-length-max` impedisce che i browser entrino oltre la lunghezza massima specificata.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="2ac6b-263">Se viene usato uno strumento come [Fiddler](https://www.telerik.com/fiddler) per modificare e riprodurre il post:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="2ac6b-264">Con il nome più lungo di 10.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-264">With the name longer than 10.</span></span>
* <span data-ttu-id="2ac6b-265">Il messaggio di errore "il nome del campo deve essere una stringa con una lunghezza massima di 10".</span><span class="sxs-lookup"><span data-stu-id="2ac6b-265">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="2ac6b-266">viene restituito.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-266">is returned.</span></span>

<span data-ttu-id="2ac6b-267">Si consideri il modello di `Movie` seguente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-267">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="2ac6b-268">Gli attributi di convalida specificano il comportamento da applicare alle proprietà del modello a cui sono applicati:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="2ac6b-269">Gli attributi `Required` e `MinimumLength` indicano che una proprietà deve avere un valore, ma nulla impedisce a un utente di immettere spazi vuoti per soddisfare la convalida.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="2ac6b-270">L'attributo `RegularExpression` viene usato per limitare i caratteri che possono essere inseriti.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-270">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="2ac6b-271">Nel codice precedente "Genre":</span><span class="sxs-lookup"><span data-stu-id="2ac6b-271">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="2ac6b-272">Deve includere solo lettere.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-272">Must only use letters.</span></span>
  * <span data-ttu-id="2ac6b-273">La prima lettera deve essere maiuscola.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-273">The first letter is required to be uppercase.</span></span> <span data-ttu-id="2ac6b-274">Gli spazi, i numeri e i caratteri speciali non sono consentiti.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-274">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="2ac6b-275">`RegularExpression` "Rating":</span><span class="sxs-lookup"><span data-stu-id="2ac6b-275">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="2ac6b-276">Richiede che il primo carattere sia una lettera maiuscola.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-276">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="2ac6b-277">Consente i caratteri speciali e i numeri negli spazi successivi.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-277">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="2ac6b-278">"PG-13" è valido per una classificazione, ma non per "Genre".</span><span class="sxs-lookup"><span data-stu-id="2ac6b-278">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="2ac6b-279">L'attributo `Range` vincola un valore all'interno di un intervallo specificato.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-279">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="2ac6b-280">L'attributo `StringLength` imposta la lunghezza massima di una proprietà di stringa e, facoltativamente, la lunghezza minima.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="2ac6b-281">I tipi di valore, ad esempio `decimal`, `int`, `float` e `DateTime`, sono intrinsecamente necessari e non richiedono l'attributo `[Required]`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-281">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="2ac6b-282">Nella pagina Crea per il modello di `Movie` vengono visualizzati gli errori con valori non validi:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-282">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![Il modulo di vista del film con più errori di convalida del lato client jQuery](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="2ac6b-284">Per altre informazioni, vedere:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-284">For more information, see:</span></span>

* [<span data-ttu-id="2ac6b-285">Aggiungere la convalida all'app Movie</span><span class="sxs-lookup"><span data-stu-id="2ac6b-285">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="2ac6b-286">[Convalida del modello in ASP.NET Core](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="2ac6b-287">Gestire le richieste HEAD con un fallback del gestore OnGet</span><span class="sxs-lookup"><span data-stu-id="2ac6b-287">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="2ac6b-288">`HEAD` richieste consentono di recuperare le intestazioni per una risorsa specifica.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-288">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="2ac6b-289">A differenza delle richieste `GET`, le richieste `HEAD` non restituiscono un corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-289">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="2ac6b-290">In genere, per le richieste `HEAD` viene creato e chiamato un gestore `OnHead`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-290">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="2ac6b-291">Razor Pages esegue il fallback alla chiamata al gestore `OnGet` se non è stato definito alcun gestore `OnHead`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="2ac6b-292">XSRF/CSRF e Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2ac6b-292">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="2ac6b-293">Razor Pages sono protette dalla [convalida antifalsificazione](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-293">Razor Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="2ac6b-294">[FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) inserisce i token antifalsificazione negli elementi del form HTML.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="2ac6b-295">Uso di layout, righe parzialmente eseguite, modelli e helper tag con Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2ac6b-295">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="2ac6b-296">Le pagine funzionano con tutte le funzionalità del motore di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-296">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="2ac6b-297">Layout, parti parziali, modelli, helper tag, *_ViewStart. cshtml*e *_ViewImports. cshtml* funzionano esattamente come per le visualizzazioni Razor convenzionali.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="2ac6b-298">La pagina verrà ora riorganizzata in modo da usufruire di alcune di queste funzionalità.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-298">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="2ac6b-299">Aggiungere una [pagina di layout](xref:mvc/views/layout) a *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-299">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="2ac6b-300">Il [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="2ac6b-300">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="2ac6b-301">Controlla il layout di ogni pagina, a meno che la pagina non accetti il layout.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-301">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="2ac6b-302">Importa le strutture HTML, ad esempio JavaScript e i fogli di stile.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-302">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="2ac6b-303">Viene eseguito il rendering del contenuto della pagina Razor in cui viene chiamato `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="2ac6b-304">Per ulteriori informazioni, vedere [pagina layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-304">For more information, see [layout page](xref:mvc/views/layout).</span></span>

<span data-ttu-id="2ac6b-305">La proprietà [Layout](xref:mvc/views/layout#specifying-a-layout) proprietà viene impostata in *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-305">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="2ac6b-306">Il layout è nella cartella *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-306">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="2ac6b-307">Le pagine cercano altre visualizzazioni (layout, modelli, righe parzialmente eseguite) in modo gerarchico, partendo dalla stessa cartella della pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-307">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="2ac6b-308">Un layout nella cartella *Pages/Shared* può essere usato da qualsiasi pagina Razor della cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-308">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="2ac6b-309">Il file di layout dovrebbe andare nella cartella *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-309">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="2ac6b-310">Si consiglia di **non** inserire il file di layout nella cartella *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-310">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="2ac6b-311">*Views/Shared* è un modello destinato alle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-311">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="2ac6b-312">Le pagine Razor devono basarsi sulla gerarchia di cartelle, non su convenzioni di percorso.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-312">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="2ac6b-313">La ricerca delle visualizzazioni da una pagina Razor include la cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-313">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="2ac6b-314">I layout, i modelli e i parziali usati con i controller MVC e le visualizzazioni Razor convenzionali *funzionano semplicemente*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="2ac6b-315">Aggiungere un file *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-315">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="2ac6b-316">`@namespace` viene spiegato in seguito nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-316">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="2ac6b-317">La direttiva `@addTagHelper` inserisce gli [helper tag predefiniti](xref:mvc/views/tag-helpers/builtin-th/Index) in tutte le pagine presenti nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-317">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="2ac6b-318">La direttiva `@namespace` impostata in una pagina:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-318">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="2ac6b-319">La direttiva `@namespace` imposta lo spazio dei nomi per la pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-319">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="2ac6b-320">La direttiva `@model` non deve necessariamente includere lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-320">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="2ac6b-321">Se la direttiva `@namespace` è contenuta in *_ViewImports.cshtml*, lo spazio dei nomi specificato indica il prefisso per lo spazio dei nomi generato nella pagina che importa la direttiva `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-321">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="2ac6b-322">Il resto dello spazio dei nomi generato (la parte del suffisso) è il percorso relativo separato dal punto tra la cartella contenente *_Viewimports.cshtml* e la cartella contenente la pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-322">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="2ac6b-323">Ad esempio, la classe `PageModel` *Pages/Customers/Edit.cshtml.cs* imposta in modo esplicito lo spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-323">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="2ac6b-324">Il file *Pages/_ViewImports.cshtml* file imposta il seguente spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-324">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="2ac6b-325">Lo spazio dei nomi generato per la pagina Razor *Pages/Customers/Edit.cshtml* corrisponde alla classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-325">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="2ac6b-326">`@namespace` *Funziona anche con le normali visualizzazioni Razor.*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-326">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="2ac6b-327">Si consideri il file di visualizzazione *pages/create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2ac6b-327">Consider the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

<span data-ttu-id="2ac6b-328">Il file di visualizzazione *pages/create. cshtml* aggiornato con *_ViewImports. cshtml* e il file di layout precedente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="2ac6b-329">Nel codice precedente, *_ViewImports. cshtml* ha importato lo spazio dei nomi e gli helper tag.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="2ac6b-330">Il file di layout ha importato i file JavaScript.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-330">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="2ac6b-331">Il [progetto iniziale per Razor Pages](#rpvs17) contiene il file *Pages/_ValidationScriptsPartial.cshtml*, che esegue la convalida sul lato client.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-331">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="2ac6b-332">Per altre informazioni sulle visualizzazioni parziali, vedere <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-332">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="2ac6b-333">Generazione di URL per le pagine</span><span class="sxs-lookup"><span data-stu-id="2ac6b-333">URL generation for Pages</span></span>

<span data-ttu-id="2ac6b-334">La pagina `Create`, riportata in precedenza, usa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-334">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

<span data-ttu-id="2ac6b-335">L'applicazione ha la struttura di file o cartella seguente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-335">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="2ac6b-336">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-336">*/Pages*</span></span>

  * <span data-ttu-id="2ac6b-337">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-337">*Index.cshtml*</span></span>
  * <span data-ttu-id="2ac6b-338">*Privacy. cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-338">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="2ac6b-339">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-339">*/Customers*</span></span>

    * <span data-ttu-id="2ac6b-340">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-340">*Create.cshtml*</span></span>
    * <span data-ttu-id="2ac6b-341">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-341">*Edit.cshtml*</span></span>
    * <span data-ttu-id="2ac6b-342">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-342">*Index.cshtml*</span></span>

<span data-ttu-id="2ac6b-343">Le pagine *pages/Customers/create. cshtml* e *pages/Customers/Edit. cshtml* reindirizza a *pages/Customers/index. cshtml* dopo l'esito positivo.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="2ac6b-344">La stringa `./Index` è un nome di pagina relativo usato per accedere alla pagina precedente.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-344">The string `./Index` is a relative page name used to access the preceding page.</span></span> <span data-ttu-id="2ac6b-345">Viene usato per generare gli URL nella pagina *pages/Customers/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="2ac6b-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="2ac6b-346">Esempio:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-346">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="2ac6b-347">Il nome della pagina assoluto `/Index` viene usato per generare gli URL nella pagina *pages/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="2ac6b-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="2ac6b-348">Esempio:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-348">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="2ac6b-349">Il nome della pagina è il percorso alla pagina dalla cartella radice */Pages*, inclusa una barra iniziale `/`, ad esempio `/Index`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-349">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="2ac6b-350">Gli esempi di generazione di URL precedenti offrono opzioni avanzate e funzionalità funzionali rispetto a un URL a livello di codice.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="2ac6b-351">La generazione di URL usa il [routing](xref:mvc/controllers/routing) ed è in grado di generare e codificare i parametri in base al modo in cui la route è definita nel percorso di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-351">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="2ac6b-352">La generazione di URL per le pagine supporta i nomi relativi.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-352">URL generation for pages supports relative names.</span></span> <span data-ttu-id="2ac6b-353">Nella tabella seguente viene illustrata la pagina di indice selezionata utilizzando parametri `RedirectToPage` diversi in *pages/Customers/create. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="2ac6b-354">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="2ac6b-354">RedirectToPage(x)</span></span>| <span data-ttu-id="2ac6b-355">Pagina</span><span class="sxs-lookup"><span data-stu-id="2ac6b-355">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="2ac6b-356">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="2ac6b-356">RedirectToPage("/Index")</span></span> | <span data-ttu-id="2ac6b-357">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-357">*Pages/Index*</span></span> |
| <span data-ttu-id="2ac6b-358">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="2ac6b-358">RedirectToPage("./Index");</span></span> | <span data-ttu-id="2ac6b-359">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-359">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="2ac6b-360">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="2ac6b-360">RedirectToPage("../Index")</span></span> | <span data-ttu-id="2ac6b-361">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-361">*Pages/Index*</span></span> |
| <span data-ttu-id="2ac6b-362">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="2ac6b-362">RedirectToPage("Index")</span></span>  | <span data-ttu-id="2ac6b-363">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-363">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="2ac6b-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`e `RedirectToPage("../Index")` sono *nomi relativi*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="2ac6b-365">Il parametro `RedirectToPage` è *combinato* con il percorso della pagina corrente per calcolare il nome della pagina di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-365">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="2ac6b-366">Il collegamento dei nomi relativi è utile quando si compilano siti con una struttura complessa.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-366">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="2ac6b-367">Quando vengono usati nomi relativi per il collegamento tra le pagine in una cartella:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-367">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="2ac6b-368">La ridenominazione di una cartella non interrompe i collegamenti relativi.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-368">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="2ac6b-369">I collegamenti non vengono interrotti perché non includono il nome della cartella.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-369">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="2ac6b-370">Per reindirizzare la pagina a un'[Area](xref:mvc/controllers/areas) diversa, specificare l'area:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-370">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="2ac6b-371">Per altre informazioni, vedere <xref:mvc/controllers/areas> e <xref:razor-pages/razor-pages-conventions>.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-371">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="2ac6b-372">Attributo viewData</span><span class="sxs-lookup"><span data-stu-id="2ac6b-372">ViewData attribute</span></span>

<span data-ttu-id="2ac6b-373">I dati possono essere passati a una pagina con <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="2ac6b-374">Le proprietà con l'attributo `[ViewData]` hanno i valori archiviati e caricati dall'<xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-374">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="2ac6b-375">Nell'esempio seguente, il `AboutModel` applica l'attributo `[ViewData]` alla proprietà `Title`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="2ac6b-376">Nella pagina About (Informazioni) accedere alla proprietà `Title` come proprietà del modello:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-376">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="2ac6b-377">Nel layout il titolo viene letto dal dizionario ViewData:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-377">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="2ac6b-378">TempData</span><span class="sxs-lookup"><span data-stu-id="2ac6b-378">TempData</span></span>

<span data-ttu-id="2ac6b-379">ASP.NET Core espone l'<xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="2ac6b-380">Questa proprietà archivia i dati finché non viene letta.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-380">This property stores data until it's read.</span></span> <span data-ttu-id="2ac6b-381">I metodi <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> e <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> possono essere usati per esaminare i dati senza eliminazione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-381">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="2ac6b-382">`TempData` è utile per il reindirizzamento, quando i dati sono necessari per più di una singola richiesta.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="2ac6b-383">Il codice seguente imposta il valore di `Message` usando `TempData`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-383">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="2ac6b-384">Il markup seguente nel file *Pages/Customers/Index.cshtml* visualizza il valore di `Message` usando `TempData`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-384">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="2ac6b-385">Il modello di pagina *Pages/Customers/Index.cshtml.cs* applica l'attributo `[TempData]` alla proprietà `Message`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-385">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="2ac6b-386">Per ulteriori informazioni, vedere [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="2ac6b-387">Più gestori per pagina</span><span class="sxs-lookup"><span data-stu-id="2ac6b-387">Multiple handlers per page</span></span>

<span data-ttu-id="2ac6b-388">La pagina seguente genera markup per due gestori usando l'helper tag `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-388">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="2ac6b-389">Il form nell'esempio precedente contiene due pulsanti di invio, ognuno dei quali usa `FormActionTagHelper` per l'invio a un URL diverso.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-389">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="2ac6b-390">L'attributo `asp-page-handler` è correlato a `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-390">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="2ac6b-391">`asp-page-handler` genera gli URL che indirizzano a ognuno dei metodi gestore definiti da una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-391">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="2ac6b-392">`asp-page` non è specificato poiché l'esempio è collegato alla pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-392">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="2ac6b-393">Il modello di pagina:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-393">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="2ac6b-394">Il codice precedente usa *metodi gestore denominati*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-394">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="2ac6b-395">I metodi gestore denominati vengono creati usando il testo che nel nome segue `On<HTTP Verb>` e precede `Async` (se presente).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-395">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="2ac6b-396">Nell'esempio precedente i metodi della pagina sono OnPost**JoinList**Async e OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-396">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="2ac6b-397">Rimuovendo *OnPost* e *Async*, i nomi dei gestori sono `JoinList` e `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-397">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="2ac6b-398">Usando il codice precedente, il percorso URL che indirizza a `OnPostJoinListAsync` è `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-398">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="2ac6b-399">Il percorso URL che indirizza a `OnPostJoinListUCAsync` è `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-399">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="2ac6b-400">Route personalizzate</span><span class="sxs-lookup"><span data-stu-id="2ac6b-400">Custom routes</span></span>

<span data-ttu-id="2ac6b-401">Usare la direttiva `@page` per:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-401">Use the `@page` directive to:</span></span>

* <span data-ttu-id="2ac6b-402">Specificare una route personalizzata a una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-402">Specify a custom route to a page.</span></span> <span data-ttu-id="2ac6b-403">Ad esempio, è possibile impostare la route alla pagina About (Informazioni) su `/Some/Other/Path` con `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-403">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="2ac6b-404">Aggiungere segmenti alla route predefinita di una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-404">Append segments to a page's default route.</span></span> <span data-ttu-id="2ac6b-405">Ad esempio, è possibile aggiungere un segmento "item" alla route predefinita di una pagina con `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-405">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="2ac6b-406">Aggiungere parametri alla route predefinita di una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-406">Append parameters to a page's default route.</span></span> <span data-ttu-id="2ac6b-407">Ad esempio, un parametro ID, `id`, può essere necessario per una pagina con `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-407">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="2ac6b-408">Un percorso relativo alla directory radice designato da una tilde (`~`) all'inizio del percorso è supportato.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-408">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="2ac6b-409">Ad esempio, `@page "~/Some/Other/Path"` equivale a `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-409">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="2ac6b-410">È possibile modificare la stringa di query `?handler=JoinList` nell'URL in un segmento di route `/JoinList` specificando il modello di route `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-410">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="2ac6b-411">Se si vuole omettere la stringa di query `?handler=JoinList` dall'URL, è possibile modificare la route in modo da inserire il nome del gestore nella parte di percorso dell'URL.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-411">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="2ac6b-412">È possibile personalizzare la route aggiungendo un modello di route racchiuso tra virgolette doppie dopo la direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-412">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="2ac6b-413">Usando il codice precedente, il percorso URL che indirizza a `OnPostJoinListAsync` è `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-413">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="2ac6b-414">Il percorso URL che indirizza a `OnPostJoinListUCAsync` è `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-414">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="2ac6b-415">`?` che segue `handler` indica che il parametro di route è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-415">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="2ac6b-416">Impostazioni e configurazione avanzate</span><span class="sxs-lookup"><span data-stu-id="2ac6b-416">Advanced configuration and settings</span></span>

<span data-ttu-id="2ac6b-417">La configurazione e le impostazioni nelle sezioni seguenti non sono richieste dalla maggior parte delle app.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-417">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="2ac6b-418">Per configurare le opzioni avanzate, usare il metodo di estensione <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-418">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="2ac6b-419">Usare il <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> per impostare la directory radice per le pagine o aggiungere le convenzioni del modello applicativo per le pagine.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-419">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="2ac6b-420">Per ulteriori informazioni sulle convenzioni, vedere [Razor Pages convenzioni di autorizzazione](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-420">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="2ac6b-421">Per la precompilazione delle visualizzazioni, vedere [compilazione della visualizzazione Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-421">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="2ac6b-422">Specificare che Razor Pages si trova nella radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="2ac6b-422">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="2ac6b-423">Per impostazione predefinita, la directory radice di Razor Pages è */Pages*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-423">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="2ac6b-424">Aggiungere <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> per specificare che i Razor Pages si trovano nella [radice del contenuto](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) dell'app:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="2ac6b-425">Specificare che Razor Pages è in una directory radice personalizzata</span><span class="sxs-lookup"><span data-stu-id="2ac6b-425">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="2ac6b-426">Aggiungere <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> per specificare che Razor Pages si trovano in una directory radice personalizzata nell'app (fornire un percorso relativo):</span><span class="sxs-lookup"><span data-stu-id="2ac6b-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="2ac6b-427">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2ac6b-427">Additional resources</span></span>

* <span data-ttu-id="2ac6b-428">Vedere Introduzione a [Razor Pages](xref:tutorials/razor-pages/razor-pages-start), che si basa su questa introduzione</span><span class="sxs-lookup"><span data-stu-id="2ac6b-428">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction</span></span>
* [<span data-ttu-id="2ac6b-429">Scaricare o visualizzare il codice di esempio</span><span class="sxs-lookup"><span data-stu-id="2ac6b-429">Download or view sample code</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample)
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2ac6b-430">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="2ac6b-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="2ac6b-431">Razor Pages è una nuova funzionalità di ASP.NET Core MVC che semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-431">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="2ac6b-432">Se si sta cercando un'esercitazione in cui si usa l'approccio Model-View-Controller, vedere [Introduzione ad ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-432">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="2ac6b-433">Questo documento offre un'introduzione a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-433">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="2ac6b-434">Non è un'esercitazione dettagliata.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-434">It's not a step by step tutorial.</span></span> <span data-ttu-id="2ac6b-435">Se alcune sezioni risultano troppo avanzate, vedere [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-435">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="2ac6b-436">Per una panoramica di ASP.NET Core, vedere [Introduzione a ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-436">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ac6b-437">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="2ac6b-437">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2ac6b-438">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ac6b-438">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2ac6b-439">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ac6b-439">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2ac6b-440">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2ac6b-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="2ac6b-441">Creare un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2ac6b-441">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2ac6b-442">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ac6b-442">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2ac6b-443">Per istruzioni dettagliate su come creare un progetto Razor Pages, vedere [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-443">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2ac6b-444">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2ac6b-444">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2ac6b-445">Eseguire `dotnet new webapp` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-445">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="2ac6b-446">Aprire il file *CSPROJ* generato da Visual Studio per Mac.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-446">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2ac6b-447">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2ac6b-447">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2ac6b-448">Eseguire `dotnet new webapp` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-448">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="2ac6b-449">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2ac6b-449">Razor Pages</span></span>

<span data-ttu-id="2ac6b-450">La funzionalità Razor Pages è abilitata in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-450">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="2ac6b-451">Si consideri una pagina di base: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="2ac6b-451">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="2ac6b-452">Il codice precedente è molto simile a un [file di visualizzazione Razor](xref:tutorials/first-mvc-app/adding-view) usato in un'app ASP.NET Core con controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-452">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="2ac6b-453">Ciò che lo differenzia è la direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-453">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="2ac6b-454">`@page` trasforma il file in un'azione MVC, ovvero gestisce le richieste direttamente, senza passare attraverso un controller.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-454">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="2ac6b-455">`@page` deve essere la prima direttiva Razor in una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-455">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="2ac6b-456">`@page` influisce sul comportamento di altri costrutti Razor.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-456">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="2ac6b-457">Nei due file seguenti viene visualizzata una pagina simile che usa una classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-457">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="2ac6b-458">Il file *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-458">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="2ac6b-459">Il modello di pagina *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-459">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="2ac6b-460">Per convenzione, il file di classe `PageModel` ha lo stesso nome del file della pagina Razor con l'aggiunta di *.cs*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-460">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="2ac6b-461">Ad esempio, la pagina Razor precedente è *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-461">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="2ac6b-462">Il file che contiene la classe `PageModel` è denominato *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-462">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="2ac6b-463">Le associazioni dei percorsi URL alle pagine sono determinate dalla posizione della pagina nel file system.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-463">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="2ac6b-464">Nella tabella seguente sono riportati alcuni percorsi di pagina Razor con gli URL corrispondenti:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-464">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="2ac6b-465">Percorso e nome file</span><span class="sxs-lookup"><span data-stu-id="2ac6b-465">File name and path</span></span>               | <span data-ttu-id="2ac6b-466">URL corrispondente</span><span class="sxs-lookup"><span data-stu-id="2ac6b-466">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="2ac6b-467">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-467">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="2ac6b-468">`/` o `/Index`</span><span class="sxs-lookup"><span data-stu-id="2ac6b-468">`/` or `/Index`</span></span> |
| <span data-ttu-id="2ac6b-469">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-469">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="2ac6b-470">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-470">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="2ac6b-471">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-471">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="2ac6b-472">`/Store` o `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="2ac6b-472">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="2ac6b-473">Note:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-473">Notes:</span></span>

* <span data-ttu-id="2ac6b-474">Il runtime cerca i file delle pagine Razor nella cartella *Pages* per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-474">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="2ac6b-475">`Index` è la pagina predefinita quando un URL non include una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-475">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="2ac6b-476">Scrivere un form di base</span><span class="sxs-lookup"><span data-stu-id="2ac6b-476">Write a basic form</span></span>

<span data-ttu-id="2ac6b-477">Razor Pages semplifica l'implementazione dei modelli normalmente usati con i Web browser durante la creazione di un'app.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-477">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="2ac6b-478">L'[associazione di modelli](xref:mvc/models/model-binding), gli [helper tag](xref:mvc/views/tag-helpers/intro) e gli helper HTML *funzionano* tutti con le proprietà definite in una classe di pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-478">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="2ac6b-479">Si consideri una pagina che implementa un form "contact us" di base per il modello `Contact`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-479">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="2ac6b-480">Per gli esempi riportati in questo documento, `DbContext` viene inizializzato nel file [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-480">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="2ac6b-481">Il modello di dati:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-481">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="2ac6b-482">Il contesto del database:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-482">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="2ac6b-483">Il file di visualizzazione *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-483">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="2ac6b-484">Il modello di pagina *Pages/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-484">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="2ac6b-485">Per convenzione, la classe `PageModel` è denominata `<PageName>Model` e si trova nello stesso spazio dei nomi della pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-485">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="2ac6b-486">La classe `PageModel` consente la separazione della logica di una pagina dalla relativa presentazione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-486">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="2ac6b-487">Definisce i gestori di pagina per le richieste inviate alla pagina e i dati usati per il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-487">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="2ac6b-488">Questa separazione consente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-488">This separation allows:</span></span>

* <span data-ttu-id="2ac6b-489">Gestione delle dipendenze di pagina tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="2ac6b-490">[Testing unità](xref:test/razor-pages-tests) delle pagine.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="2ac6b-491">La pagina contiene un oggetto `OnPostAsync`, ovvero un *metodo gestore* che viene eseguito per le richieste `POST`, quando un utente invia il form.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-491">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="2ac6b-492">È possibile aggiungere metodi gestore per qualsiasi verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-492">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="2ac6b-493">I gestori più comuni sono:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-493">The most common handlers are:</span></span>

* <span data-ttu-id="2ac6b-494">`OnGet` per inizializzare lo stato necessario per la pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-494">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="2ac6b-495">Esempio di [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-495">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="2ac6b-496">`OnPost` per gestire gli invii di form.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-496">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="2ac6b-497">Il suffisso `Async` nel nome è facoltativo, ma viene spesso usato per convenzione per le funzioni asincrone.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-497">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="2ac6b-498">Il codice precedente è tipico di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-498">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="2ac6b-499">Se si ha familiarità con le app ASP.NET che usano i controller e le visualizzazioni:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-499">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="2ac6b-500">Il codice `OnPostAsync` nell'esempio precedente ha un aspetto simile al codice del controller tipico.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="2ac6b-501">La maggior parte delle primitive MVC, ad esempio l' [associazione di modelli](xref:mvc/models/model-binding), la [convalida](xref:mvc/models/validation), la [convalida](xref:mvc/models/validation)e i risultati dell'azione, sono condivisi.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="2ac6b-502">Il metodo `OnPostAsync` precedente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-502">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="2ac6b-503">Il flusso di base di `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-503">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="2ac6b-504">Verificare se sono presenti errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-504">Check for validation errors.</span></span>

* <span data-ttu-id="2ac6b-505">Se non sono presenti errori, salvare i dati e reindirizzare.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-505">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="2ac6b-506">Se sono presenti errori, visualizzare di nuovo la pagina con i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-506">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="2ac6b-507">La convalida lato client è identica alle applicazioni ASP.NET Core MVC tradizionali.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-507">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="2ac6b-508">In molti casi gli errori di convalida vengono rilevati nel client e mai inviati al server.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-508">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="2ac6b-509">Quando i dati vengono immessi correttamente, il metodo gestore `OnPostAsync` chiama il metodo helper `RedirectToPage` per restituire un'istanza di `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-509">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="2ac6b-510">`RedirectToPage` è un nuovo risultato dell'azione, simile a `RedirectToAction` o `RedirectToRoute`, ma personalizzato per le pagine.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-510">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="2ac6b-511">Nell'esempio precedente viene reindirizzato alla pagina di indice radice (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-511">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="2ac6b-512">Il metodo `RedirectToPage` è descritto in dettaglio nella sezione [Generazione di URL per le pagine](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-512">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="2ac6b-513">Quando il form inviato contiene errori di convalida (che vengono passati al server), il metodo gestore `OnPostAsync` chiama il metodo helper `Page`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-513">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="2ac6b-514">`Page` restituisce un'istanza di `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-514">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="2ac6b-515">La restituzione di `Page` è simile al modo in cui le azioni nel controller restituiscono `View`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-515">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="2ac6b-516">`PageResult` è il tipo restituito predefinito per un metodo del gestore.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-516">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="2ac6b-517">Un metodo gestore che restituisce `void` esegue il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-517">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="2ac6b-518">La proprietà `Customer` usa l'attributo `[BindProperty]` optare per consentire l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-518">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="2ac6b-519">Per impostazione predefinita, Razor Pages associa le proprietà solo a verbi non `GET`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-519">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="2ac6b-520">Il binding alle proprietà può ridurre la quantità di codice da scrivere.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-520">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="2ac6b-521">Riduce il codice usando la stessa proprietà per il eseguire il rendering dei campi del form (`<input asp-for="Customer.Name">`) e accettare l'input.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-521">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="2ac6b-522">La home page (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="2ac6b-522">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="2ac6b-523">La classe `PageModel` associata (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="2ac6b-523">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="2ac6b-524">Il file *Index.cshtml* contiene il markup seguente per creare un collegamento di modifica per ogni contatto:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-524">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="2ac6b-525">L' [Helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` ha usato l'attributo `asp-route-{value}` per generare un collegamento alla pagina di modifica.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="2ac6b-526">Il collegamento contiene i dati della route con l'ID contatto.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-526">The link contains route data with the contact ID.</span></span> <span data-ttu-id="2ac6b-527">Ad esempio `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-527">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="2ac6b-528">Gli [helper tag](xref:mvc/views/tag-helpers/intro) consentono al codice lato server di partecipare alla creazione e al rendering di elementi HTML nei file Razor.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-528">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="2ac6b-529">Gli helper tag sono abilitati dal `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="2ac6b-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="2ac6b-530">Il file *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-530">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="2ac6b-531">La prima riga contiene la direttiva `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-531">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="2ac6b-532">Il vincolo di routing `"{id:int}"` indica alla pagina di accettare le richieste inviate alla pagina che contengono i dati della route `int`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-532">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="2ac6b-533">Se una richiesta inviata alla pagina non contiene dati della route che possono essere convertiti in un oggetto `int`, il runtime restituisce un errore HTTP 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-533">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="2ac6b-534">Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-534">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="2ac6b-535">Il file *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-535">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="2ac6b-536">Il file *Index.cshtml* contiene anche il markup per creare un pulsante di eliminazione per ogni contatto cliente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-536">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="2ac6b-537">Quando viene eseguito il rendering del pulsante di eliminazione in HTML, il relativo `formaction` include i parametri per:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-537">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="2ac6b-538">L'ID di contatto cliente dall'attributo `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-538">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="2ac6b-539">L'`handler` specificato dall'attributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-539">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="2ac6b-540">Di seguito è riportato un esempio di pulsante di eliminazione di cui è stato eseguito il rendering con un ID di contatto cliente `1`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-540">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="2ac6b-541">Quando il pulsante è selezionato, viene inviata una richiesta `POST` di modulo al server.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-541">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="2ac6b-542">Per convenzione, il nome del metodo del gestore viene selezionato in base al valore del parametro `handler` in base allo schema `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-542">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="2ac6b-543">Poiché in questo esempio l'`handler` è `delete`, il metodo `OnPostDeleteAsync` viene usato per elaborare la richiesta `POST`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-543">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="2ac6b-544">Se `asp-page-handler` viene impostato su un valore diverso, ad esempio `remove`, viene selezionato un metodo gestore con il nome `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-544">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="2ac6b-545">Il codice seguente illustra il gestore di `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-545">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="2ac6b-546">Il metodo `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-546">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="2ac6b-547">Accetta l'`id` dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-547">Accepts the `id` from the query string.</span></span> <span data-ttu-id="2ac6b-548">Se la direttiva della pagina *index. cshtml* conteneva il vincolo di routing `"{id:int?}"`, `id` proverrebbe dai dati della route.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="2ac6b-549">I dati della route per `id` vengono specificati nell'URI, ad esempio `https://localhost:5001/Customers/2`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="2ac6b-550">Interroga il database in merito al contatto del cliente con `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-550">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="2ac6b-551">Se viene trovato il contatto cliente, viene rimosso dall'elenco dei contatti del cliente.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-551">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="2ac6b-552">Il database viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-552">The database is updated.</span></span>
* <span data-ttu-id="2ac6b-553">Chiama `RedirectToPage` per reindirizzare alla pagina di indice radice (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-553">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="2ac6b-554">Contrassegnare le proprietà della pagina in base alle esigenze</span><span class="sxs-lookup"><span data-stu-id="2ac6b-554">Mark page properties as required</span></span>

<span data-ttu-id="2ac6b-555">Le proprietà di `PageModel` possono essere decorate con l'attributo con [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute):</span><span class="sxs-lookup"><span data-stu-id="2ac6b-555">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="2ac6b-556">Per altre informazioni, vedere [Convalida del modello](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-556">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="2ac6b-557">Gestire le richieste HEAD con un fallback del gestore OnGet</span><span class="sxs-lookup"><span data-stu-id="2ac6b-557">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="2ac6b-558">Le richieste `HEAD` consentono di recuperare le intestazioni di una risorsa specifica.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-558">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="2ac6b-559">A differenza delle richieste `GET`, le richieste `HEAD` non restituiscono un corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-559">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="2ac6b-560">In genere, per le richieste `HEAD` viene creato e chiamato un gestore `OnHead`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-560">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="2ac6b-561">In ASP.NET Core 2.1 o versioni successive se non è definito alcun gestore `OnHead`, Razor Pages esegue un fallback per chiamare il gestore `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-561">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="2ac6b-562">Questo comportamento è abilitato tramite la chiamata a [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-562">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="2ac6b-563">I modelli predefiniti generano la chiamata `SetCompatibilityVersion` in ASP.NET Core 2.1 e 2.2.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-563">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="2ac6b-564">`SetCompatibilityVersion` imposta effettivamente l'opzione di Razor Pages `AllowMappingHeadRequestsToGetHandler` su `true`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-564">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="2ac6b-565">Anziché acconsentire esplicitamente a tutti i comportamenti con `SetCompatibilityVersion`, è possibile acconsentire a comportamenti *specifici*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-565">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="2ac6b-566">Il codice seguente acconsente esplicitamente al mapping delle richieste `HEAD` al gestore `OnGet`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-566">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="2ac6b-567">XSRF/CSRF e Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2ac6b-567">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="2ac6b-568">Non è necessario scrivere codice per la [convalida antifalsificazione](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-568">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="2ac6b-569">La generazione e la convalida del token antifalsificazione sono automaticamente incluse in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-569">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="2ac6b-570">Uso di layout, righe parzialmente eseguite, modelli e helper tag con Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2ac6b-570">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="2ac6b-571">Le pagine funzionano con tutte le funzionalità del motore di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-571">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="2ac6b-572">I layout, le righe parzialmente eseguite, i modelli, gli helper tag, *_ViewStart.cshtml* e *_ViewImports.cshtml* funzionano esattamente come nelle normali visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-572">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="2ac6b-573">La pagina verrà ora riorganizzata in modo da usufruire di alcune di queste funzionalità.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-573">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="2ac6b-574">Aggiungere una [pagina di layout](xref:mvc/views/layout) a *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-574">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="2ac6b-575">Il [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="2ac6b-575">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="2ac6b-576">Controlla il layout di ogni pagina, a meno che la pagina non accetti il layout.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-576">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="2ac6b-577">Importa le strutture HTML, ad esempio JavaScript e i fogli di stile.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-577">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="2ac6b-578">Vedere l'articolo sulla [pagina Layout](xref:mvc/views/layout) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-578">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="2ac6b-579">La proprietà [Layout](xref:mvc/views/layout#specifying-a-layout) proprietà viene impostata in *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-579">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="2ac6b-580">Il layout è nella cartella *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-580">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="2ac6b-581">Le pagine cercano altre visualizzazioni (layout, modelli, righe parzialmente eseguite) in modo gerarchico, partendo dalla stessa cartella della pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-581">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="2ac6b-582">Un layout nella cartella *Pages/Shared* può essere usato da qualsiasi pagina Razor della cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-582">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="2ac6b-583">Il file di layout dovrebbe andare nella cartella *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-583">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="2ac6b-584">Si consiglia di **non** inserire il file di layout nella cartella *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-584">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="2ac6b-585">*Views/Shared* è un modello destinato alle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-585">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="2ac6b-586">Le pagine Razor devono basarsi sulla gerarchia di cartelle, non su convenzioni di percorso.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-586">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="2ac6b-587">La ricerca delle visualizzazioni da una pagina Razor include la cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-587">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="2ac6b-588">Il layout, i modelli e le righe parzialmente eseguite in uso con i controller MVC e le visualizzazioni Razor standard *funzionano solo*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-588">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="2ac6b-589">Aggiungere un file *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-589">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="2ac6b-590">`@namespace` viene spiegato in seguito nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-590">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="2ac6b-591">La direttiva `@addTagHelper` inserisce gli [helper tag predefiniti](xref:mvc/views/tag-helpers/builtin-th/Index) in tutte le pagine presenti nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-591">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="2ac6b-592">Quando la direttiva `@namespace` viene usata in modo esplicito in una pagina:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-592">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="2ac6b-593">La direttiva imposta lo spazio dei nomi per la pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-593">The directive sets the namespace for the page.</span></span> <span data-ttu-id="2ac6b-594">La direttiva `@model` non deve necessariamente includere lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-594">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="2ac6b-595">Se la direttiva `@namespace` è contenuta in *_ViewImports.cshtml*, lo spazio dei nomi specificato indica il prefisso per lo spazio dei nomi generato nella pagina che importa la direttiva `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-595">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="2ac6b-596">Il resto dello spazio dei nomi generato (la parte del suffisso) è il percorso relativo separato dal punto tra la cartella contenente *_Viewimports.cshtml* e la cartella contenente la pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-596">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="2ac6b-597">Ad esempio, la classe `PageModel` *Pages/Customers/Edit.cshtml.cs* imposta in modo esplicito lo spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-597">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="2ac6b-598">Il file *Pages/_ViewImports.cshtml* file imposta il seguente spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-598">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="2ac6b-599">Lo spazio dei nomi generato per la pagina Razor *Pages/Customers/Edit.cshtml* corrisponde alla classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-599">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="2ac6b-600">`@namespace` *Funziona anche con le normali visualizzazioni Razor.*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-600">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="2ac6b-601">Il file di visualizzazione *Pages/Create.cshtml* originale:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-601">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="2ac6b-602">Il file di visualizzazione *Pages/Create.cshtml* aggiornato:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-602">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="2ac6b-603">Il [progetto iniziale per Razor Pages](#rpvs17) contiene il file *Pages/_ValidationScriptsPartial.cshtml*, che esegue la convalida sul lato client.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-603">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="2ac6b-604">Per altre informazioni sulle visualizzazioni parziali, vedere <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-604">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="2ac6b-605">Generazione di URL per le pagine</span><span class="sxs-lookup"><span data-stu-id="2ac6b-605">URL generation for Pages</span></span>

<span data-ttu-id="2ac6b-606">La pagina `Create`, riportata in precedenza, usa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-606">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="2ac6b-607">L'applicazione ha la struttura di file o cartella seguente:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-607">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="2ac6b-608">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-608">*/Pages*</span></span>

  * <span data-ttu-id="2ac6b-609">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-609">*Index.cshtml*</span></span>
  * <span data-ttu-id="2ac6b-610">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-610">*/Customers*</span></span>

    * <span data-ttu-id="2ac6b-611">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-611">*Create.cshtml*</span></span>
    * <span data-ttu-id="2ac6b-612">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-612">*Edit.cshtml*</span></span>
    * <span data-ttu-id="2ac6b-613">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-613">*Index.cshtml*</span></span>

<span data-ttu-id="2ac6b-614">Le pagine *Pages/Customers/Create.cshtml* e *Pages/Customers/Edit.cshtml* reindirizzano a *Pages/Index.cshtml* dopo l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-614">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="2ac6b-615">La stringa `/Index` fa parte dell'URI di accesso alla pagina precedente.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-615">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="2ac6b-616">La stringa `/Index` può essere usata per generare gli URI alla pagina *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-616">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="2ac6b-617">Esempio:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-617">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="2ac6b-618">Il nome della pagina è il percorso alla pagina dalla cartella radice */Pages*, inclusa una barra iniziale `/`, ad esempio `/Index`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-618">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="2ac6b-619">Gli esempi di generazione di URL precedenti offrono opzioni e caratteristiche funzionali avanzate rispetto agli URL hardcoded.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-619">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="2ac6b-620">La generazione di URL usa il [routing](xref:mvc/controllers/routing) ed è in grado di generare e codificare i parametri in base al modo in cui la route è definita nel percorso di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-620">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="2ac6b-621">La generazione di URL per le pagine supporta i nomi relativi.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-621">URL generation for pages supports relative names.</span></span> <span data-ttu-id="2ac6b-622">La tabella seguente indica quale pagina di indice viene selezionata con diversi parametri `RedirectToPage` da *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-622">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="2ac6b-623">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="2ac6b-623">RedirectToPage(x)</span></span>| <span data-ttu-id="2ac6b-624">Pagina</span><span class="sxs-lookup"><span data-stu-id="2ac6b-624">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="2ac6b-625">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="2ac6b-625">RedirectToPage("/Index")</span></span> | <span data-ttu-id="2ac6b-626">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-626">*Pages/Index*</span></span> |
| <span data-ttu-id="2ac6b-627">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="2ac6b-627">RedirectToPage("./Index");</span></span> | <span data-ttu-id="2ac6b-628">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-628">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="2ac6b-629">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="2ac6b-629">RedirectToPage("../Index")</span></span> | <span data-ttu-id="2ac6b-630">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-630">*Pages/Index*</span></span> |
| <span data-ttu-id="2ac6b-631">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="2ac6b-631">RedirectToPage("Index")</span></span>  | <span data-ttu-id="2ac6b-632">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="2ac6b-632">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="2ac6b-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, e `RedirectToPage("../Index")` sono *nomi relativi*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="2ac6b-634">Il parametro `RedirectToPage` è *combinato* con il percorso della pagina corrente per calcolare il nome della pagina di destinazione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-634">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="2ac6b-635">Il collegamento dei nomi relativi è utile quando si compilano siti con una struttura complessa.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-635">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="2ac6b-636">Se si usano i nomi relativi per il collegamento tra le pagine in una cartella, è possibile rinominare tale cartella.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-636">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="2ac6b-637">Tutti i collegamenti continuano a funzionare perché non includono il nome della cartella.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-637">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="2ac6b-638">Per reindirizzare la pagina a un'[Area](xref:mvc/controllers/areas) diversa, specificare l'area:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-638">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="2ac6b-639">Per ulteriori informazioni, vedere <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-639">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="2ac6b-640">Attributo viewData</span><span class="sxs-lookup"><span data-stu-id="2ac6b-640">ViewData attribute</span></span>

<span data-ttu-id="2ac6b-641">È possibile passare i dati a una pagina tramite [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-641">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="2ac6b-642">I valori delle proprietà nei controller o nei modelli Razor Page decorate con `[ViewData]` vengono archiviati e caricati da [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-642">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="2ac6b-643">Nell'esempio seguente `AboutModel` contiene una proprietà `Title` decorata con `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-643">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="2ac6b-644">La proprietà `Title` è impostata sul titolo della pagina About (Informazioni):</span><span class="sxs-lookup"><span data-stu-id="2ac6b-644">The `Title` property is set to the title of the About page:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="2ac6b-645">Nella pagina About (Informazioni) accedere alla proprietà `Title` come proprietà del modello:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-645">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="2ac6b-646">Nel layout il titolo viene letto dal dizionario ViewData:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-646">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="2ac6b-647">TempData</span><span class="sxs-lookup"><span data-stu-id="2ac6b-647">TempData</span></span>

<span data-ttu-id="2ac6b-648">ASP.NET Core espone la proprietà [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) per un [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-648">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="2ac6b-649">Questa proprietà archivia i dati finché non viene letta.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-649">This property stores data until it's read.</span></span> <span data-ttu-id="2ac6b-650">I metodi `Keep` e `Peek` possono essere usati per esaminare i dati senza eliminazione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-650">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="2ac6b-651">`TempData` è utile per il reindirizzamento quando i dati sono necessari per più di una singola richiesta.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-651">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="2ac6b-652">Il codice seguente imposta il valore di `Message` usando `TempData`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-652">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="2ac6b-653">Il markup seguente nel file *Pages/Customers/Index.cshtml* visualizza il valore di `Message` usando `TempData`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-653">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="2ac6b-654">Il modello di pagina *Pages/Customers/Index.cshtml.cs* applica l'attributo `[TempData]` alla proprietà `Message`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-654">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="2ac6b-655">Per altre informazioni, vedere [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-655">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="2ac6b-656">Più gestori per pagina</span><span class="sxs-lookup"><span data-stu-id="2ac6b-656">Multiple handlers per page</span></span>

<span data-ttu-id="2ac6b-657">La pagina seguente genera markup per due gestori usando l'helper tag `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-657">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="2ac6b-658">Il form nell'esempio precedente contiene due pulsanti di invio, ognuno dei quali usa `FormActionTagHelper` per l'invio a un URL diverso.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-658">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="2ac6b-659">L'attributo `asp-page-handler` è correlato a `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-659">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="2ac6b-660">`asp-page-handler` genera gli URL che indirizzano a ognuno dei metodi gestore definiti da una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-660">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="2ac6b-661">`asp-page` non è specificato poiché l'esempio è collegato alla pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-661">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="2ac6b-662">Il modello di pagina:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-662">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="2ac6b-663">Il codice precedente usa *metodi gestore denominati*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-663">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="2ac6b-664">I metodi gestore denominati vengono creati usando il testo che nel nome segue `On<HTTP Verb>` e precede `Async` (se presente).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-664">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="2ac6b-665">Nell'esempio precedente i metodi della pagina sono OnPost**JoinList**Async e OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-665">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="2ac6b-666">Rimuovendo *OnPost* e *Async*, i nomi dei gestori sono `JoinList` e `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-666">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="2ac6b-667">Usando il codice precedente, il percorso URL che indirizza a `OnPostJoinListAsync` è `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-667">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="2ac6b-668">Il percorso URL che indirizza a `OnPostJoinListUCAsync` è `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-668">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="2ac6b-669">Route personalizzate</span><span class="sxs-lookup"><span data-stu-id="2ac6b-669">Custom routes</span></span>

<span data-ttu-id="2ac6b-670">Usare la direttiva `@page` per:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-670">Use the `@page` directive to:</span></span>

* <span data-ttu-id="2ac6b-671">Specificare una route personalizzata a una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-671">Specify a custom route to a page.</span></span> <span data-ttu-id="2ac6b-672">Ad esempio, è possibile impostare la route alla pagina About (Informazioni) su `/Some/Other/Path` con `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-672">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="2ac6b-673">Aggiungere segmenti alla route predefinita di una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-673">Append segments to a page's default route.</span></span> <span data-ttu-id="2ac6b-674">Ad esempio, è possibile aggiungere un segmento "item" alla route predefinita di una pagina con `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-674">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="2ac6b-675">Aggiungere parametri alla route predefinita di una pagina.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-675">Append parameters to a page's default route.</span></span> <span data-ttu-id="2ac6b-676">Ad esempio, un parametro ID, `id`, può essere necessario per una pagina con `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-676">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="2ac6b-677">Un percorso relativo alla directory radice designato da una tilde (`~`) all'inizio del percorso è supportato.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-677">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="2ac6b-678">Ad esempio, `@page "~/Some/Other/Path"` equivale a `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-678">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="2ac6b-679">È possibile modificare la stringa di query `?handler=JoinList` nell'URL in un segmento di route `/JoinList` specificando il modello di route `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-679">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="2ac6b-680">Se si vuole omettere la stringa di query `?handler=JoinList` dall'URL, è possibile modificare la route in modo da inserire il nome del gestore nella parte di percorso dell'URL.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-680">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="2ac6b-681">È possibile personalizzare la route aggiungendo un modello di route racchiuso tra virgolette doppie dopo la direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-681">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="2ac6b-682">Usando il codice precedente, il percorso URL che indirizza a `OnPostJoinListAsync` è `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-682">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="2ac6b-683">Il percorso URL che indirizza a `OnPostJoinListUCAsync` è `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-683">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="2ac6b-684">`?` che segue `handler` indica che il parametro di route è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-684">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="2ac6b-685">Configurazione e impostazioni</span><span class="sxs-lookup"><span data-stu-id="2ac6b-685">Configuration and settings</span></span>

<span data-ttu-id="2ac6b-686">Per configurare le opzioni avanzate, usare il metodo di estensione `AddRazorPagesOptions` nel generatore MVC:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-686">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="2ac6b-687">Attualmente è possibile usare `RazorPagesOptions` per impostare la directory radice per le pagine o aggiungere convenzioni di modello applicativo per le pagine.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-687">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="2ac6b-688">In questo modo si potrà garantire una maggiore estendibilità in futuro.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-688">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="2ac6b-689">Per la precompilazione delle visualizzazioni, vedere l'articolo sulla [compilazione delle visualizzazioni Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-689">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="2ac6b-690">[Scaricare o visualizzare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="2ac6b-690">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="2ac6b-691">Vedere [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start), che si basa su questa introduzione.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-691">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="2ac6b-692">Specificare che Razor Pages si trova nella radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="2ac6b-692">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="2ac6b-693">Per impostazione predefinita, la directory radice di Razor Pages è */Pages*.</span><span class="sxs-lookup"><span data-stu-id="2ac6b-693">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="2ac6b-694">Aggiungere [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) per specificare che il Razor Pages si trova nella [radice del contenuto](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) dell'app:</span><span class="sxs-lookup"><span data-stu-id="2ac6b-694">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="2ac6b-695">Specificare che Razor Pages è in una directory radice personalizzata</span><span class="sxs-lookup"><span data-stu-id="2ac6b-695">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="2ac6b-696">Aggiungere [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) per specificare che Razor Pages si trova in una directory radice personalizzata nell'app (fornire un percorso relativo):</span><span class="sxs-lookup"><span data-stu-id="2ac6b-696">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="2ac6b-697">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2ac6b-697">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
