---
title: Introduzione a Razor Pages in ASP.NET Core
author: Rick-Anderson
description: Informazioni su come Razor Pages in ASP.NET Core semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine rispetto a MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: 5e2b53a4771a97b0a4091f593720b9c0e4e345bf
ms.sourcegitcommit: c4a31aaf902f2e84aaf4a9d882ca980fdf6488c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="6b51d-103">Introduzione a Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b51d-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="6b51d-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="6b51d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="6b51d-105">Razor Pages è una nuova funzionalità di ASP.NET Core MVC che semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine.</span><span class="sxs-lookup"><span data-stu-id="6b51d-105">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="6b51d-106">Se si sta cercando un'esercitazione in cui si usa l'approccio Model-View-Controller, vedere [Introduzione ad ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="6b51d-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="6b51d-107">Questo documento offre un'introduzione a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b51d-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="6b51d-108">Non è un'esercitazione dettagliata.</span><span class="sxs-lookup"><span data-stu-id="6b51d-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="6b51d-109">Se alcune sezioni risultano troppo avanzate, vedere [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="6b51d-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="6b51d-110">Per una panoramica di ASP.NET Core, vedere [Introduzione a ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="6b51d-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b51d-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="6b51d-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="6b51d-112">Creazione di un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b51d-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6b51d-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b51d-113">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="6b51d-114">Per istruzioni dettagliate su come creare un progetto Razor Pages con Visual Studio, vedere [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="6b51d-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6b51d-115">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="6b51d-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6b51d-116">Eseguire `dotnet new razor` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="6b51d-116">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="6b51d-117">Aprire il file *CSPROJ* generato da Visual Studio per Mac.</span><span class="sxs-lookup"><span data-stu-id="6b51d-117">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6b51d-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6b51d-118">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="6b51d-119">Eseguire `dotnet new razor` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="6b51d-119">Run `dotnet new razor` from the command line.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6b51d-120">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="6b51d-120">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="6b51d-121">Eseguire `dotnet new razor` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="6b51d-121">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="6b51d-122">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b51d-122">Razor Pages</span></span>

<span data-ttu-id="6b51d-123">La funzionalità Razor Pages è abilitata in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-123">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="6b51d-124">Si consideri una pagina di base: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="6b51d-124">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="6b51d-125">Il codice precedente è molto simile a un file di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="6b51d-125">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="6b51d-126">Ciò che lo differenzia è la direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-126">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="6b51d-127">`@page` trasforma il file in un'azione MVC, ovvero gestisce le richieste direttamente, senza passare attraverso un controller.</span><span class="sxs-lookup"><span data-stu-id="6b51d-127">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="6b51d-128">`@page` deve essere la prima direttiva Razor in una pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-128">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="6b51d-129">`@page` influisce sul comportamento di altri costrutti Razor.</span><span class="sxs-lookup"><span data-stu-id="6b51d-129">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="6b51d-130">Nei due file seguenti viene visualizzata una pagina simile che usa una classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-130">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="6b51d-131">Il file *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-131">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="6b51d-132">Il modello di pagina *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-132">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="6b51d-133">Per convenzione, il file di classe `PageModel` ha lo stesso nome del file della pagina Razor con l'aggiunta di *.cs*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-133">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="6b51d-134">Ad esempio, la pagina Razor precedente è *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-134">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="6b51d-135">Il file che contiene la classe `PageModel` è denominato *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-135">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="6b51d-136">Le associazioni dei percorsi URL alle pagine sono determinate dalla posizione della pagina nel file system.</span><span class="sxs-lookup"><span data-stu-id="6b51d-136">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="6b51d-137">Nella tabella seguente sono riportati alcuni percorsi di pagina Razor con gli URL corrispondenti:</span><span class="sxs-lookup"><span data-stu-id="6b51d-137">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="6b51d-138">Percorso e nome file</span><span class="sxs-lookup"><span data-stu-id="6b51d-138">File name and path</span></span>               | <span data-ttu-id="6b51d-139">URL corrispondente</span><span class="sxs-lookup"><span data-stu-id="6b51d-139">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="6b51d-140">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b51d-140">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="6b51d-141">`/` o `/Index`</span><span class="sxs-lookup"><span data-stu-id="6b51d-141">`/` or `/Index`</span></span> |
| <span data-ttu-id="6b51d-142">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b51d-142">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="6b51d-143">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b51d-143">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="6b51d-144">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b51d-144">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="6b51d-145">`/Store` o `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="6b51d-145">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="6b51d-146">Note:</span><span class="sxs-lookup"><span data-stu-id="6b51d-146">Notes:</span></span>

* <span data-ttu-id="6b51d-147">Il runtime cerca i file delle pagine Razor nella cartella *Pages* per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6b51d-147">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="6b51d-148">`Index` è la pagina predefinita quando un URL non include una pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-148">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="6b51d-149">Scrittura di un form di base</span><span class="sxs-lookup"><span data-stu-id="6b51d-149">Writing a basic form</span></span>

<span data-ttu-id="6b51d-150">Le funzionalità Razor Pages sono progettate in modo da semplificare i modelli normalmente usati con i Web browser.</span><span class="sxs-lookup"><span data-stu-id="6b51d-150">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="6b51d-151">L'[associazione di modelli](xref:mvc/models/model-binding), gli [helper tag](xref:mvc/views/tag-helpers/intro) e gli helper HTML *funzionano* tutti con le proprietà definite in una classe di pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="6b51d-151">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="6b51d-152">Si consideri una pagina che implementa un form "contact us" di base per il modello `Contact`:</span><span class="sxs-lookup"><span data-stu-id="6b51d-152">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="6b51d-153">Per gli esempi riportati in questo documento, `DbContext` viene inizializzato nel file [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="6b51d-153">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="6b51d-154">Il modello di dati:</span><span class="sxs-lookup"><span data-stu-id="6b51d-154">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="6b51d-155">Il contesto del database:</span><span class="sxs-lookup"><span data-stu-id="6b51d-155">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="6b51d-156">Il file di visualizzazione *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-156">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="6b51d-157">Il modello di pagina *Pages/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-157">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="6b51d-158">Per convenzione, la classe `PageModel` è denominata `<PageName>Model` e si trova nello stesso spazio dei nomi della pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-158">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="6b51d-159">La classe `PageModel` consente la separazione della logica di una pagina dalla relativa presentazione.</span><span class="sxs-lookup"><span data-stu-id="6b51d-159">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="6b51d-160">Definisce i gestori di pagina per le richieste inviate alla pagina e i dati usati per il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-160">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="6b51d-161">Questa separazione consente di gestire le dipendenze della pagina tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection) e di eseguire [unit test](xref:testing/razor-pages-testing) delle pagine.</span><span class="sxs-lookup"><span data-stu-id="6b51d-161">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="6b51d-162">La pagina contiene un oggetto `OnPostAsync`, ovvero un *metodo gestore* che viene eseguito per le richieste `POST`, quando un utente invia il form.</span><span class="sxs-lookup"><span data-stu-id="6b51d-162">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="6b51d-163">È possibile aggiungere metodi gestore per qualsiasi verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b51d-163">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="6b51d-164">I gestori più comuni sono:</span><span class="sxs-lookup"><span data-stu-id="6b51d-164">The most common handlers are:</span></span>

* <span data-ttu-id="6b51d-165">`OnGet` per inizializzare lo stato necessario per la pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-165">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="6b51d-166">Esempio di [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="6b51d-166">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="6b51d-167">`OnPost` per gestire gli invii di form.</span><span class="sxs-lookup"><span data-stu-id="6b51d-167">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="6b51d-168">Il suffisso `Async` nel nome è facoltativo, ma viene spesso usato per convenzione per le funzioni asincrone.</span><span class="sxs-lookup"><span data-stu-id="6b51d-168">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="6b51d-169">Il codice `OnPostAsync` nell'esempio precedente è simile a ciò che in genere si scrive in un controller.</span><span class="sxs-lookup"><span data-stu-id="6b51d-169">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="6b51d-170">Il codice precedente è tipico di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b51d-170">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="6b51d-171">La maggior parte delle primitive di MVC, ad esempio l'[associazione di modelli](xref:mvc/models/model-binding), la [convalida](xref:mvc/models/validation) e i risultati dell'azione vengono condivise.</span><span class="sxs-lookup"><span data-stu-id="6b51d-171">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="6b51d-172">Il metodo `OnPostAsync` precedente:</span><span class="sxs-lookup"><span data-stu-id="6b51d-172">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="6b51d-173">Il flusso di base di `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="6b51d-173">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="6b51d-174">Verificare se sono presenti errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="6b51d-174">Check for validation errors.</span></span>

*  <span data-ttu-id="6b51d-175">Se non sono presenti errori, salvare i dati e reindirizzare.</span><span class="sxs-lookup"><span data-stu-id="6b51d-175">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="6b51d-176">Se sono presenti errori, visualizzare di nuovo la pagina con i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="6b51d-176">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="6b51d-177">La convalida lato client è identica alle applicazioni ASP.NET Core MVC tradizionali.</span><span class="sxs-lookup"><span data-stu-id="6b51d-177">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="6b51d-178">In molti casi gli errori di convalida vengono rilevati nel client e mai inviati al server.</span><span class="sxs-lookup"><span data-stu-id="6b51d-178">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="6b51d-179">Quando i dati vengono immessi correttamente, il metodo gestore `OnPostAsync` chiama il metodo helper `RedirectToPage` per restituire un'istanza di `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-179">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="6b51d-180">`RedirectToPage` è un nuovo risultato dell'azione, simile a `RedirectToAction` o `RedirectToRoute`, ma personalizzato per le pagine.</span><span class="sxs-lookup"><span data-stu-id="6b51d-180">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="6b51d-181">Nell'esempio precedente viene reindirizzato alla pagina di indice radice (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="6b51d-181">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="6b51d-182">Il metodo `RedirectToPage` è descritto in dettaglio nella sezione [Generazione di URL per le pagine](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="6b51d-182">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="6b51d-183">Quando il form inviato contiene errori di convalida (che vengono passati al server), il metodo gestore `OnPostAsync` chiama il metodo helper `Page`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-183">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="6b51d-184">`Page` restituisce un'istanza di `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-184">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="6b51d-185">La restituzione di `Page` è simile al modo in cui le azioni nel controller restituiscono `View`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-185">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="6b51d-186">`PageResult` è il tipo restituito <!-- Review  --> predefinito per un metodo gestore.</span><span class="sxs-lookup"><span data-stu-id="6b51d-186">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="6b51d-187">Un metodo gestore che restituisce `void` esegue il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-187">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="6b51d-188">La proprietà `Customer` usa l'attributo `[BindProperty]` optare per consentire l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="6b51d-188">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="6b51d-189">Per impostazione predefinita Razor Pages associa le proprietà solo ai verbi non GET.</span><span class="sxs-lookup"><span data-stu-id="6b51d-189">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="6b51d-190">Il binding alle proprietà può ridurre la quantità di codice da scrivere.</span><span class="sxs-lookup"><span data-stu-id="6b51d-190">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="6b51d-191">Riduce il codice usando la stessa proprietà per il eseguire il rendering dei campi del form (`<input asp-for="Customer.Name" />`) e accettare l'input.</span><span class="sxs-lookup"><span data-stu-id="6b51d-191">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="6b51d-192">Per motivi di sicurezza, è necessario acconsentire esplicitamente all'associazione dei dati della richiesta GET alle proprietà del modello di pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-192">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="6b51d-193">Verificare l'input dell'utente prima di eseguirne il mapping alle proprietà.</span><span class="sxs-lookup"><span data-stu-id="6b51d-193">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="6b51d-194">Acconsentire esplicitamente a questo comportamento è utile quando si compilano funzionalità che si basano su valori di route o stringa di query.</span><span class="sxs-lookup"><span data-stu-id="6b51d-194">Opting in to this behavior is useful when building features which rely on query string or route values.</span></span>
>
> <span data-ttu-id="6b51d-195">Per associare una proprietà nelle richieste GET, impostare la proprietà `SupportsGet` dell'attributo `[BindProperty]` su `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="6b51d-195">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="6b51d-196">La home page (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="6b51d-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="6b51d-197">Il file code-behind *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-197">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="6b51d-198">Il file *Index.cshtml* contiene il markup seguente per creare un collegamento di modifica per ogni contatto:</span><span class="sxs-lookup"><span data-stu-id="6b51d-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="6b51d-199">[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) ha usato l'attributo `asp-route-{value}` per generare un collegamento alla pagina di modifica.</span><span class="sxs-lookup"><span data-stu-id="6b51d-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="6b51d-200">Il collegamento contiene i dati della route con l'ID contatto.</span><span class="sxs-lookup"><span data-stu-id="6b51d-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="6b51d-201">Ad esempio `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-201">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="6b51d-202">Il file *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-202">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="6b51d-203">La prima riga contiene la direttiva `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-203">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="6b51d-204">Il vincolo di routing `"{id:int}"` indica alla pagina di accettare le richieste inviate alla pagina che contengono i dati della route `int`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-204">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="6b51d-205">Se una richiesta inviata alla pagina non contiene dati della route che possono essere convertiti in un oggetto `int`, il runtime restituisce un errore HTTP 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="6b51d-205">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="6b51d-206">Per rendere l'ID facoltativo, aggiungere `?` al vincolo di route:</span><span class="sxs-lookup"><span data-stu-id="6b51d-206">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="6b51d-207">Il file *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="6b51d-208">Il file *Index.cshtml* contiene anche il markup per creare un pulsante di eliminazione per ogni contatto cliente:</span><span class="sxs-lookup"><span data-stu-id="6b51d-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="6b51d-209">Quando viene eseguito il rendering del pulsante di eliminazione in HTML, il relativo `formaction` include i parametri per:</span><span class="sxs-lookup"><span data-stu-id="6b51d-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="6b51d-210">L'ID di contatto cliente dall'attributo `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="6b51d-211">L'`handler` specificato dall'attributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="6b51d-212">Di seguito è riportato un esempio di pulsante di eliminazione di cui è stato eseguito il rendering con un ID di contatto cliente `1`:</span><span class="sxs-lookup"><span data-stu-id="6b51d-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="6b51d-213">Quando il pulsante è selezionato, viene inviata una richiesta `POST` di modulo al server.</span><span class="sxs-lookup"><span data-stu-id="6b51d-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="6b51d-214">Per convenzione, il nome del metodo del gestore viene selezionato in base al valore del parametro `handler` in base allo schema `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="6b51d-215">Poiché in questo esempio l'`handler` è `delete`, il metodo `OnPostDeleteAsync` viene usato per elaborare la richiesta `POST`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="6b51d-216">Se `asp-page-handler` viene impostato su un valore diverso, come `remove`, viene selezionato un metodo gestore della pagina con il nome `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="6b51d-217">Il metodo `OnPostDeleteAsync`:</span><span class="sxs-lookup"><span data-stu-id="6b51d-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="6b51d-218">Accetta l'`id` dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="6b51d-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="6b51d-219">Interroga il database in merito al contatto del cliente con `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="6b51d-220">Se viene trovato il contatto cliente, viene rimosso dall'elenco dei contatti del cliente.</span><span class="sxs-lookup"><span data-stu-id="6b51d-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="6b51d-221">Il database viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="6b51d-221">The database is updated.</span></span>
* <span data-ttu-id="6b51d-222">Chiama `RedirectToPage` per reindirizzare alla pagina di indice radice (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="6b51d-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="6b51d-223">XSRF/CSRF e Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b51d-223">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="6b51d-224">Non è necessario scrivere codice per la [convalida antifalsificazione](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="6b51d-224">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="6b51d-225">La generazione e la convalida del token antifalsificazione sono automaticamente incluse in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6b51d-225">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="6b51d-226">Uso di layout, righe parzialmente eseguite, modelli e helper tag con Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b51d-226">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="6b51d-227">Le pagine funzionano con tutte le funzionalità del motore di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="6b51d-227">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="6b51d-228">I layout, le righe parzialmente eseguite, i modelli, gli helper tag, *_ViewStart.cshtml* e *_ViewImports.cshtml* funzionano esattamente come nelle normali visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="6b51d-228">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="6b51d-229">La pagina verrà ora riorganizzata in modo da usufruire di alcune di queste funzionalità.</span><span class="sxs-lookup"><span data-stu-id="6b51d-229">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="6b51d-230">Aggiungere una [pagina di layout](xref:mvc/views/layout) a *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-230">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="6b51d-231">Il [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="6b51d-231">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="6b51d-232">Controlla il layout di ogni pagina, a meno che la pagina non accetti il layout.</span><span class="sxs-lookup"><span data-stu-id="6b51d-232">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="6b51d-233">Importa le strutture HTML, ad esempio JavaScript e i fogli di stile.</span><span class="sxs-lookup"><span data-stu-id="6b51d-233">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="6b51d-234">Vedere l'articolo sulla [pagina Layout](xref:mvc/views/layout) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="6b51d-234">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="6b51d-235">La proprietà [Layout](xref:mvc/views/layout#specifying-a-layout) proprietà viene impostata in *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-235">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="6b51d-236">**Nota**: il layout è nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-236">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="6b51d-237">Le pagine cercano altre visualizzazioni (layout, modelli, righe parzialmente eseguite) in modo gerarchico, partendo dalla stessa cartella della pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="6b51d-237">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="6b51d-238">Un layout nella cartella *Pages* può essere usato da qualsiasi pagina Razor della cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-238">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="6b51d-239">Si consiglia di **non** inserire il file di layout nella cartella *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-239">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="6b51d-240">*Views/Shared* è un modello destinato alle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="6b51d-240">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="6b51d-241">Le pagine Razor devono basarsi sulla gerarchia di cartelle, non su convenzioni di percorso.</span><span class="sxs-lookup"><span data-stu-id="6b51d-241">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="6b51d-242">La ricerca delle visualizzazioni da una pagina Razor include la cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-242">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="6b51d-243">Il layout, i modelli e le righe parzialmente eseguite in uso con i controller MVC e le visualizzazioni Razor standard *funzionano solo*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-243">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="6b51d-244">Aggiungere un file *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-244">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="6b51d-245">`@namespace` viene spiegato in seguito nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="6b51d-245">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="6b51d-246">La direttiva `@addTagHelper` inserisce gli [helper tag predefiniti](xref:mvc/views/tag-helpers/builtin-th/Index) in tutte le pagine presenti nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-246">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="6b51d-247">Quando la direttiva `@namespace` viene usata in modo esplicito in una pagina:</span><span class="sxs-lookup"><span data-stu-id="6b51d-247">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="6b51d-248">La direttiva imposta lo spazio dei nomi per la pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-248">The directive sets the namespace for the page.</span></span> <span data-ttu-id="6b51d-249">La direttiva `@model` non deve necessariamente includere lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="6b51d-249">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="6b51d-250">Se la direttiva `@namespace` è contenuta in *_ViewImports.cshtml*, lo spazio dei nomi specificato indica il prefisso per lo spazio dei nomi generato nella pagina che importa la direttiva `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-250">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="6b51d-251">Il resto dello spazio dei nomi generato (la parte del suffisso) è il percorso relativo separato dal punto tra la cartella contenente *_Viewimports.cshtml* e la cartella contenente la pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-251">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="6b51d-252">Ad esempio, il file code-behind *Pages/Customers/Edit.cshtml.cs* imposta in modo esplicito lo spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="6b51d-252">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="6b51d-253">Il file *Pages/_ViewImports.cshtml* file imposta il seguente spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="6b51d-253">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="6b51d-254">Lo spazio dei nomi generato per la pagina Razor *Pages/Customers/Edit.cshtml* corrisponde al file code-behind.</span><span class="sxs-lookup"><span data-stu-id="6b51d-254">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="6b51d-255">La direttiva `@namespace` è stata progettata in modo che le classi C# aggiunte a un progetto e il codice generato dalle pagine *funzionino* senza dover aggiungere una direttiva `@using` per il file code-behind.</span><span class="sxs-lookup"><span data-stu-id="6b51d-255">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="6b51d-256">**Nota:** `@namespace` funziona anche con le normali visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="6b51d-256">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="6b51d-257">Il file di visualizzazione *Pages/Create.cshtml* originale:</span><span class="sxs-lookup"><span data-stu-id="6b51d-257">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="6b51d-258">Il file di visualizzazione *Pages/Create.cshtml* aggiornato:</span><span class="sxs-lookup"><span data-stu-id="6b51d-258">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="6b51d-259">Il [progetto iniziale per Razor Pages](#rpvs17) contiene il file *Pages/_ValidationScriptsPartial.cshtml*, che esegue la convalida sul lato client.</span><span class="sxs-lookup"><span data-stu-id="6b51d-259">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="6b51d-260">Generazione di URL per le pagine</span><span class="sxs-lookup"><span data-stu-id="6b51d-260">URL generation for Pages</span></span>

<span data-ttu-id="6b51d-261">La pagina `Create`, riportata in precedenza, usa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="6b51d-261">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="6b51d-262">L'applicazione ha la struttura di file o cartella seguente:</span><span class="sxs-lookup"><span data-stu-id="6b51d-262">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="6b51d-263">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="6b51d-263">*/Pages*</span></span>

  * <span data-ttu-id="6b51d-264">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b51d-264">*Index.cshtml*</span></span>
  * <span data-ttu-id="6b51d-265">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="6b51d-265">*/Customers*</span></span>

    * <span data-ttu-id="6b51d-266">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b51d-266">*Create.cshtml*</span></span>
    * <span data-ttu-id="6b51d-267">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b51d-267">*Edit.cshtml*</span></span>
    * <span data-ttu-id="6b51d-268">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="6b51d-268">*Index.cshtml*</span></span>

<span data-ttu-id="6b51d-269">Le pagine *Pages/Customers/Create.cshtml* e *Pages/Customers/Edit.cshtml* reindirizzano a *Pages/Index.cshtml* dopo l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6b51d-269">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="6b51d-270">La stringa `/Index` fa parte dell'URI di accesso alla pagina precedente.</span><span class="sxs-lookup"><span data-stu-id="6b51d-270">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="6b51d-271">La stringa `/Index` può essere usata per generare gli URI alla pagina *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-271">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="6b51d-272">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6b51d-272">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="6b51d-273">Il nome della pagina è il percorso alla pagina dalla cartella radice */Pages*, inclusa una barra iniziale (`/`), ad esempio `/Index`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-273">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="6b51d-274">Gli esempi di generazione di URL precedenti sono molto più ricco di funzionalità rispetto a livello di codice solo un URL.</span><span class="sxs-lookup"><span data-stu-id="6b51d-274">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="6b51d-275">La generazione di URL usa il [routing](xref:mvc/controllers/routing) ed è in grado di generare e codificare i parametri in base al modo in cui la route è definita nel percorso di destinazione.</span><span class="sxs-lookup"><span data-stu-id="6b51d-275">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="6b51d-276">La generazione di URL per le pagine supporta i nomi relativi.</span><span class="sxs-lookup"><span data-stu-id="6b51d-276">URL generation for pages supports relative names.</span></span> <span data-ttu-id="6b51d-277">La tabella seguente indica quale pagina di indice viene selezionata con diversi parametri `RedirectToPage` da *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6b51d-277">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="6b51d-278">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="6b51d-278">RedirectToPage(x)</span></span>| <span data-ttu-id="6b51d-279">Pagina</span><span class="sxs-lookup"><span data-stu-id="6b51d-279">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="6b51d-280">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="6b51d-280">RedirectToPage("/Index")</span></span> | <span data-ttu-id="6b51d-281">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="6b51d-281">*Pages/Index*</span></span> |
| <span data-ttu-id="6b51d-282">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="6b51d-282">RedirectToPage("./Index");</span></span> | <span data-ttu-id="6b51d-283">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="6b51d-283">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="6b51d-284">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="6b51d-284">RedirectToPage("../Index")</span></span> | <span data-ttu-id="6b51d-285">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="6b51d-285">*Pages/Index*</span></span> |
| <span data-ttu-id="6b51d-286">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="6b51d-286">RedirectToPage("Index")</span></span>  | <span data-ttu-id="6b51d-287">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="6b51d-287">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="6b51d-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, e `RedirectToPage("../Index")` sono <em>nomi relativi</em>.</span><span class="sxs-lookup"><span data-stu-id="6b51d-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="6b51d-289">Il parametro `RedirectToPage` è <em>combinato</em> con il percorso della pagina corrente per calcolare il nome della pagina di destinazione.</span><span class="sxs-lookup"><span data-stu-id="6b51d-289">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="6b51d-290">Il collegamento dei nomi relativi è utile quando si compilano siti con una struttura complessa.</span><span class="sxs-lookup"><span data-stu-id="6b51d-290">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="6b51d-291">Se si usano i nomi relativi per il collegamento tra le pagine in una cartella, è possibile rinominare tale cartella.</span><span class="sxs-lookup"><span data-stu-id="6b51d-291">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="6b51d-292">Tutti i collegamenti continuano a funzionare perché non includono il nome della cartella.</span><span class="sxs-lookup"><span data-stu-id="6b51d-292">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="6b51d-293">TempData</span><span class="sxs-lookup"><span data-stu-id="6b51d-293">TempData</span></span>

<span data-ttu-id="6b51d-294">ASP.NET Core espone la proprietà [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) per un [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="6b51d-294">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="6b51d-295">Questa proprietà archivia i dati finché non viene letta.</span><span class="sxs-lookup"><span data-stu-id="6b51d-295">This property stores data until it's read.</span></span> <span data-ttu-id="6b51d-296">I metodi `Keep` e `Peek` possono essere usati per esaminare i dati senza eliminazione.</span><span class="sxs-lookup"><span data-stu-id="6b51d-296">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="6b51d-297">`TempData` è utile per il reindirizzamento quando i dati sono necessari per più di una singola richiesta.</span><span class="sxs-lookup"><span data-stu-id="6b51d-297">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="6b51d-298">L'attributo `[TempData]` è nuovo in ASP.NET Core 2.0 ed è supportato per controller e pagine.</span><span class="sxs-lookup"><span data-stu-id="6b51d-298">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="6b51d-299">Il codice seguente imposta il valore di `Message` usando `TempData`:</span><span class="sxs-lookup"><span data-stu-id="6b51d-299">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="6b51d-300">Il markup seguente nel file *Pages/Customers/Index.cshtml* visualizza il valore di `Message` usando `TempData`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-300">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="6b51d-301">Il modello di pagina *Pages/Customers/Index.cshtml.cs* applica l'attributo `[TempData]` alla proprietà `Message`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-301">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="6b51d-302">Per altre informazioni, vedere [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="6b51d-302">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="6b51d-303">Più gestori per pagina</span><span class="sxs-lookup"><span data-stu-id="6b51d-303">Multiple handlers per page</span></span>

<span data-ttu-id="6b51d-304">La pagina seguente genera markup per due gestori di pagina usando l'helper tag `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="6b51d-304">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="6b51d-305">Il form nell'esempio precedente contiene due pulsanti di invio, ognuno dei quali usa `FormActionTagHelper` per l'invio a un URL diverso.</span><span class="sxs-lookup"><span data-stu-id="6b51d-305">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="6b51d-306">L'attributo `asp-page-handler` è correlato a `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-306">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="6b51d-307">`asp-page-handler` genera gli URL che indirizzano a ognuno dei metodi gestore definiti da una pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-307">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="6b51d-308">`asp-page` non è specificato poiché l'esempio è collegato alla pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="6b51d-308">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="6b51d-309">Il modello di pagina:</span><span class="sxs-lookup"><span data-stu-id="6b51d-309">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="6b51d-310">Il codice precedente usa *metodi gestore denominati*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-310">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="6b51d-311">I metodi gestore denominati vengono creati usando il testo che nel nome segue `On<HTTP Verb>` e precede `Async` (se presente).</span><span class="sxs-lookup"><span data-stu-id="6b51d-311">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="6b51d-312">Nell'esempio precedente i metodi della pagina sono OnPost**JoinList**Async e OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="6b51d-312">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="6b51d-313">Rimuovendo *OnPost* e *Async*, i nomi dei gestori sono `JoinList` e `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-313">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="6b51d-314">Usando il codice precedente, il percorso URL che indirizza a `OnPostJoinListAsync` è `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-314">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="6b51d-315">Il percorso URL che indirizza a `OnPostJoinListUCAsync` è `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-315">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="6b51d-316">Personalizzazione del routing</span><span class="sxs-lookup"><span data-stu-id="6b51d-316">Customizing Routing</span></span>

<span data-ttu-id="6b51d-317">Se si vuole omettere la stringa di query `?handler=JoinList` dall'URL, è possibile modificare la route in modo da inserire il nome del gestore nella parte di percorso dell'URL.</span><span class="sxs-lookup"><span data-stu-id="6b51d-317">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="6b51d-318">È possibile personalizzare la route aggiungendo un modello di route racchiuso tra virgolette doppie dopo la direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="6b51d-318">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="6b51d-319">La route precedente inserisce il nome del gestore nel percorso URL anziché nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="6b51d-319">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="6b51d-320">`?` che segue `handler` indica che il parametro di route è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="6b51d-320">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="6b51d-321">È possibile usare `@page` per aggiungere altri segmenti e parametri alla route di una pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-321">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="6b51d-322">Tutti gli elementi presenti vengono **aggiunti** alla route predefinita della pagina.</span><span class="sxs-lookup"><span data-stu-id="6b51d-322">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="6b51d-323">L'uso di un percorso assoluto o virtuale per modificare la route della pagina, ad esempio `"~/Some/Other/Path"`, non è supportato.</span><span class="sxs-lookup"><span data-stu-id="6b51d-323">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="6b51d-324">Configurazione e impostazioni</span><span class="sxs-lookup"><span data-stu-id="6b51d-324">Configuration and settings</span></span>

<span data-ttu-id="6b51d-325">Per configurare le opzioni avanzate, usare il metodo di estensione `AddRazorPagesOptions` nel generatore MVC:</span><span class="sxs-lookup"><span data-stu-id="6b51d-325">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="6b51d-326">Attualmente è possibile usare `RazorPagesOptions` per impostare la directory radice per le pagine o aggiungere convenzioni di modello applicativo per le pagine.</span><span class="sxs-lookup"><span data-stu-id="6b51d-326">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="6b51d-327">In questo modo si potrà garantire una maggiore estendibilità in futuro.</span><span class="sxs-lookup"><span data-stu-id="6b51d-327">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="6b51d-328">Per la precompilazione delle visualizzazioni, vedere l'articolo sulla [compilazione delle visualizzazioni Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="6b51d-328">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="6b51d-329">[Scaricare o visualizzare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="6b51d-329">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="6b51d-330">Vedere [Introduzione a Razor Pages](xref:tutorials/razor-pages/razor-pages-start), che si basa su questa introduzione.</span><span class="sxs-lookup"><span data-stu-id="6b51d-330">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="6b51d-331">Specificare che Razor Pages si trova nella radice del contenuto</span><span class="sxs-lookup"><span data-stu-id="6b51d-331">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="6b51d-332">Per impostazione predefinita, la directory radice di Razor Pages è */Pages*.</span><span class="sxs-lookup"><span data-stu-id="6b51d-332">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="6b51d-333">Aggiungere [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) per specificare che Razor Pages è nella radice del contenuto ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) dell'app:</span><span class="sxs-lookup"><span data-stu-id="6b51d-333">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="6b51d-334">Specificare che Razor Pages è in una directory radice personalizzata</span><span class="sxs-lookup"><span data-stu-id="6b51d-334">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="6b51d-335">Aggiungere [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) per specificare che Razor Pages si trova in una directory radice personalizzata nell'app (fornire un percorso relativo):</span><span class="sxs-lookup"><span data-stu-id="6b51d-335">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="6b51d-336">Vedere anche</span><span class="sxs-lookup"><span data-stu-id="6b51d-336">See also</span></span>

* [<span data-ttu-id="6b51d-337">Introduzione a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6b51d-337">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="6b51d-338">Sintassi Razor</span><span class="sxs-lookup"><span data-stu-id="6b51d-338">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="6b51d-339">Introduzione a Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b51d-339">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="6b51d-340">Convenzioni di autorizzazione di Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b51d-340">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="6b51d-341">Route personalizzata di Razor Pages e provider di modelli di pagina</span><span class="sxs-lookup"><span data-stu-id="6b51d-341">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="6b51d-342">Unit test e test di integrazione per Razor Pages</span><span class="sxs-lookup"><span data-stu-id="6b51d-342">Razor Pages unit and integration tests</span></span>](xref:testing/razor-pages-testing)
