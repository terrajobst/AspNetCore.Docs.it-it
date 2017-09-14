---
title: Introduzione a Razor Pages in ASP.NET Core
author: Rick-Anderson
description: Panoramica di Razor Pages in ASP.NET Core
keywords: ASP.NET Core, Razor Pages
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: 543399d99af127f943f7e9119fb5d84c8c5bc499
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="028d2-104">Introduzione a Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="028d2-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="028d2-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="028d2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="028d2-106">Razor Pages è una nuova funzionalità di ASP.NET Core MVC che semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine.</span><span class="sxs-lookup"><span data-stu-id="028d2-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="028d2-107">Se si sta cercando un'esercitazione in cui si usa l'approccio Model-View-Controller, vedere l'[introduzione ad ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="028d2-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="028d2-108">Prerequisiti di ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="028d2-108">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="028d2-109">Installare [.NET Core](https://www.microsoft.com/net/core) 2.0.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="028d2-109">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="028d2-110">Se si usa Visual Studio, installare [Visual Studio](https://www.visualstudio.com/vs/) 15.3 o versione successiva con i carichi di lavoro seguenti:</span><span class="sxs-lookup"><span data-stu-id="028d2-110">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="028d2-111">**Sviluppo ASP.NET e Web**</span><span class="sxs-lookup"><span data-stu-id="028d2-111">**ASP.NET and web development**</span></span>
* <span data-ttu-id="028d2-112">**Sviluppo multipiattaforma .NET Core**</span><span class="sxs-lookup"><span data-stu-id="028d2-112">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="028d2-113">Creazione di un progetto Razor Pages</span><span class="sxs-lookup"><span data-stu-id="028d2-113">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="028d2-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="028d2-114">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="028d2-115">Per istruzioni dettagliate su come creare un progetto Razor Pages con Visual Studio, vedere la [guida introduttiva a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="028d2-115">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="028d2-116">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="028d2-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="028d2-117">Eseguire `dotnet new razor` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="028d2-117">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="028d2-118">Aprire il file *CSPROJ* generato da Visual Studio per Mac.</span><span class="sxs-lookup"><span data-stu-id="028d2-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="028d2-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="028d2-119">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="028d2-120">Eseguire `dotnet new razor` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="028d2-120">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="028d2-121">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="028d2-121">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="028d2-122">Eseguire `dotnet new razor` dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="028d2-122">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="028d2-123">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="028d2-123">Razor Pages</span></span>

<span data-ttu-id="028d2-124">La funzionalità Razor Pages è abilitata in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="028d2-124">Razor Pages is enabled in *Startup.cs*:</span></span>

<span data-ttu-id="028d2-125">[!code-cs[principale](index/sample/RazorPagesIntro/Startup.cs?name=Startup)]</span><span class="sxs-lookup"><span data-stu-id="028d2-125">[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=Startup)]</span></span>

<span data-ttu-id="028d2-126">Si consideri una pagina di base: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="028d2-126">Consider a basic page: <a name="OnGet"></a></span></span>

<span data-ttu-id="028d2-127">[!code-cshtml[principale](index/sample/RazorPagesIntro/Pages/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="028d2-127">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]</span></span>

<span data-ttu-id="028d2-128">Il codice precedente è molto simile a un file di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="028d2-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="028d2-129">Ciò che lo differenzia è la direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="028d2-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="028d2-130">`@page` trasforma il file in un'azione MVC, ovvero gestisce le richieste direttamente, senza passare attraverso un controller.</span><span class="sxs-lookup"><span data-stu-id="028d2-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="028d2-131">`@page` deve essere la prima direttiva Razor in una pagina.</span><span class="sxs-lookup"><span data-stu-id="028d2-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="028d2-132">`@page` influisce sul comportamento di altri costrutti Razor.</span><span class="sxs-lookup"><span data-stu-id="028d2-132">`@page` affects the behavior of other Razor constructs.</span></span> <span data-ttu-id="028d2-133">La direttiva [@functions](xref:mvc/views/razor#functions) abilita il contenuto a livello di funzione.</span><span class="sxs-lookup"><span data-stu-id="028d2-133">The [@functions](xref:mvc/views/razor#functions) directive enables function-level content.</span></span>

<span data-ttu-id="028d2-134">Una pagina simile, con `PageModel` in un file separato, è illustrata nei seguenti due file.</span><span class="sxs-lookup"><span data-stu-id="028d2-134">A similar page, with the `PageModel` in a separate file, is shown in the following two files.</span></span> <span data-ttu-id="028d2-135">Il file *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="028d2-135">The *Pages/Index2.cshtml* file:</span></span>

<span data-ttu-id="028d2-136">[!code-cshtml[principale](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="028d2-136">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]</span></span>

<span data-ttu-id="028d2-137">Il file *Pages/Index2.cshtml.cs* 'code-behind':</span><span class="sxs-lookup"><span data-stu-id="028d2-137">The *Pages/Index2.cshtml.cs* 'code-behind' file:</span></span>

<span data-ttu-id="028d2-138">[!code-cs[principale](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="028d2-138">[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]</span></span>

<span data-ttu-id="028d2-139">Per convenzione, il file di classe `PageModel` ha lo stesso nome del file della pagina Razor con l'aggiunta di *.cs*.</span><span class="sxs-lookup"><span data-stu-id="028d2-139">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="028d2-140">Ad esempio, la pagina Razor precedente è *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="028d2-140">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="028d2-141">Il file che contiene la classe `PageModel` è denominato *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="028d2-141">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="028d2-142">Per le pagine semplici, si può usare una combinazione della classe `PageModel` con il markup Razor.</span><span class="sxs-lookup"><span data-stu-id="028d2-142">For simple pages, mixing the `PageModel` class with the Razor markup is fine.</span></span> <span data-ttu-id="028d2-143">Se il codice è più complesso, è consigliabile separare il codice del modello di pagina.</span><span class="sxs-lookup"><span data-stu-id="028d2-143">For more complex code, it's a best practice to keep the page model code separate.</span></span>

<span data-ttu-id="028d2-144">Le associazioni dei percorsi URL alle pagine sono determinate dalla posizione della pagina nel file system.</span><span class="sxs-lookup"><span data-stu-id="028d2-144">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="028d2-145">Nella tabella seguente sono riportati alcuni percorsi di pagina Razor con gli URL corrispondenti:</span><span class="sxs-lookup"><span data-stu-id="028d2-145">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="028d2-146">Percorso e nome file</span><span class="sxs-lookup"><span data-stu-id="028d2-146">File name and path</span></span>               | <span data-ttu-id="028d2-147">URL corrispondente</span><span class="sxs-lookup"><span data-stu-id="028d2-147">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="028d2-148">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="028d2-148">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="028d2-149">`/` o `/Index`</span><span class="sxs-lookup"><span data-stu-id="028d2-149">`/` or `/Index`</span></span> |
| <span data-ttu-id="028d2-150">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="028d2-150">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="028d2-151">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="028d2-151">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="028d2-152">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="028d2-152">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="028d2-153">`/Store` o `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="028d2-153">`/Store` or `/Store/Index`</span></span>  |

<span data-ttu-id="028d2-154">Note:</span><span class="sxs-lookup"><span data-stu-id="028d2-154">Notes:</span></span>

* <span data-ttu-id="028d2-155">Il runtime cerca i file delle pagine Razor nella cartella *Pages* per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="028d2-155">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="028d2-156">`Index` è la pagina predefinita quando un URL non include una pagina.</span><span class="sxs-lookup"><span data-stu-id="028d2-156">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="028d2-157">Scrittura di un form di base</span><span class="sxs-lookup"><span data-stu-id="028d2-157">Writing a basic form</span></span>

<span data-ttu-id="028d2-158">Le funzionalità Razor Pages sono progettate in modo da semplificare i modelli normalmente usati con i Web browser.</span><span class="sxs-lookup"><span data-stu-id="028d2-158">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="028d2-159">L'[associazione di modelli](xref:mvc/models/model-binding), gli [helper tag](xref:mvc/views/tag-helpers/intro) e gli helper HTML *funzionano* tutti con le proprietà definite in una classe di pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="028d2-159">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="028d2-160">Si consideri una pagina che implementa un form "contact us" di base per il modello `Contact`:</span><span class="sxs-lookup"><span data-stu-id="028d2-160">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="028d2-161">Per gli esempi riportati in questo documento, `DbContext` viene inizializzato nel file [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="028d2-161">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

<span data-ttu-id="028d2-162">[!code-cs[principale](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="028d2-162">[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]</span></span>

<span data-ttu-id="028d2-163">Il modello di dati:</span><span class="sxs-lookup"><span data-stu-id="028d2-163">The data model:</span></span>

<span data-ttu-id="028d2-164">[!code-cs[principale](index/sample/RazorPagesContacts/Data/Customer.cs)]</span><span class="sxs-lookup"><span data-stu-id="028d2-164">[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]</span></span>

<span data-ttu-id="028d2-165">Il file di visualizzazione *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="028d2-165">The *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="028d2-166">[!code-cshtml[principale](index/sample/RazorPagesContacts/Pages/Create.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="028d2-166">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]</span></span>

<span data-ttu-id="028d2-167">Il file code-behind *Pages/Create.cshtml.cs* per la visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="028d2-167">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

<span data-ttu-id="028d2-168">[!code-cs[principale](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=ALL)]</span><span class="sxs-lookup"><span data-stu-id="028d2-168">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=ALL)]</span></span>

<span data-ttu-id="028d2-169">Per convenzione, la classe `PageModel` è denominata `<PageName>Model` e si trova nello stesso spazio dei nomi della pagina.</span><span class="sxs-lookup"><span data-stu-id="028d2-169">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span> <span data-ttu-id="028d2-170">Non sono necessarie molte modifiche per eseguire la conversione da una pagina che usa `@functions` per definire i gestori e una pagina che usa una classe `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="028d2-170">Not much change is needed to convert from a page using `@functions` to define handlers and a page using a `PageModel` class.</span></span>

<span data-ttu-id="028d2-171">L'uso di un file `PageModel` code-behind supporta il testing unità, ma richiede la scrittura di un costruttore e una classe espliciti.</span><span class="sxs-lookup"><span data-stu-id="028d2-171">Using a `PageModel` code-behind file supports unit testing, but requires you to write an explicit constructor and class.</span></span> <span data-ttu-id="028d2-172">Le pagine senza file `PageModel` code-behind supportano la compilazione di runtime e questo può essere un vantaggio in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="028d2-172">Pages without `PageModel` code-behind files support runtime compilation, which can be an advantage in development.</span></span>  <!-- review: advantage because you can make changes and refresh the browser without explicitly compiling the app -->

<span data-ttu-id="028d2-173">La pagina contiene un oggetto `OnPostAsync`, ovvero un *metodo gestore* che viene eseguito per le richieste `POST`, quando un utente invia il form.</span><span class="sxs-lookup"><span data-stu-id="028d2-173">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="028d2-174">È possibile aggiungere metodi gestore per qualsiasi verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="028d2-174">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="028d2-175">I gestori più comuni sono:</span><span class="sxs-lookup"><span data-stu-id="028d2-175">The most common handlers are:</span></span>

* <span data-ttu-id="028d2-176">`OnGet` per inizializzare lo stato necessario per la pagina.</span><span class="sxs-lookup"><span data-stu-id="028d2-176">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="028d2-177">Esempio di [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="028d2-177">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="028d2-178">`OnPost` per gestire gli invii di form.</span><span class="sxs-lookup"><span data-stu-id="028d2-178">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="028d2-179">Il suffisso `Async` nel nome è facoltativo, ma viene spesso usato per convenzione per le funzioni asincrone.</span><span class="sxs-lookup"><span data-stu-id="028d2-179">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="028d2-180">Il codice `OnPostAsync` nell'esempio precedente è simile a ciò che in genere si scrive in un controller.</span><span class="sxs-lookup"><span data-stu-id="028d2-180">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="028d2-181">Il codice precedente è tipico di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="028d2-181">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="028d2-182">La maggior parte delle primitive di MVC, ad esempio l'[associazione di modelli](xref:mvc/models/model-binding), la [convalida](xref:mvc/models/validation) e i risultati dell'azione vengono condivise.</span><span class="sxs-lookup"><span data-stu-id="028d2-182">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="028d2-183">Il metodo `OnPostAsync` precedente:</span><span class="sxs-lookup"><span data-stu-id="028d2-183">The previous `OnPostAsync` method:</span></span>

<span data-ttu-id="028d2-184">[!code-cs[principale](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync)]</span><span class="sxs-lookup"><span data-stu-id="028d2-184">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync)]</span></span>

<span data-ttu-id="028d2-185">Il flusso di base di `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="028d2-185">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="028d2-186">Verificare se sono presenti errori di convalida.</span><span class="sxs-lookup"><span data-stu-id="028d2-186">Check for validation errors.</span></span>

*  <span data-ttu-id="028d2-187">Se non sono presenti errori, salvare i dati e reindirizzare.</span><span class="sxs-lookup"><span data-stu-id="028d2-187">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="028d2-188">Se sono presenti errori, visualizzare di nuovo la pagina con i messaggi di convalida.</span><span class="sxs-lookup"><span data-stu-id="028d2-188">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="028d2-189">La convalida lato client è identica alle applicazioni ASP.NET Core MVC tradizionali.</span><span class="sxs-lookup"><span data-stu-id="028d2-189">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="028d2-190">In molti casi gli errori di convalida vengono rilevati nel client e mai inviati al server.</span><span class="sxs-lookup"><span data-stu-id="028d2-190">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="028d2-191">Quando i dati vengono immessi correttamente, il metodo gestore `OnPostAsync` chiama il metodo helper `RedirectToPage` per restituire un'istanza di `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="028d2-191">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="028d2-192">`RedirectToPage` è un nuovo risultato dell'azione, simile a `RedirectToAction` o `RedirectToRoute`, ma personalizzato per le pagine.</span><span class="sxs-lookup"><span data-stu-id="028d2-192">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="028d2-193">Nell'esempio precedente viene reindirizzato alla pagina di indice radice (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="028d2-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="028d2-194">Il metodo `RedirectToPage` è descritto in dettaglio nella sezione [Generazione di URL per le pagine](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="028d2-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="028d2-195">Quando il form inviato contiene errori di convalida (che vengono passati al server), il metodo gestore `OnPostAsync` chiama il metodo helper `Page`.</span><span class="sxs-lookup"><span data-stu-id="028d2-195">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="028d2-196">`Page` restituisce un'istanza di `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="028d2-196">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="028d2-197">La restituzione di `Page` è simile al modo in cui le azioni nel controller restituiscono `View`.</span><span class="sxs-lookup"><span data-stu-id="028d2-197">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="028d2-198">`PageResult` è il tipo restituito <!-- Review  --> predefinito per un metodo gestore.</span><span class="sxs-lookup"><span data-stu-id="028d2-198">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="028d2-199">Un metodo gestore che restituisce `void` esegue il rendering della pagina.</span><span class="sxs-lookup"><span data-stu-id="028d2-199">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="028d2-200">La proprietà `Customer` usa l'attributo `[BindProperty]` optare per consentire l'associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="028d2-200">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

<span data-ttu-id="028d2-201">[!code-cs[principale](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=PageModel&highlight=10-11)]</span><span class="sxs-lookup"><span data-stu-id="028d2-201">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=PageModel&highlight=10-11)]</span></span>

<span data-ttu-id="028d2-202">Per impostazione predefinita Razor Pages associa le proprietà solo ai verbi non GET.</span><span class="sxs-lookup"><span data-stu-id="028d2-202">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="028d2-203">Il binding alle proprietà può ridurre la quantità di codice da scrivere.</span><span class="sxs-lookup"><span data-stu-id="028d2-203">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="028d2-204">Riduce il codice usando la stessa proprietà per il eseguire il rendering dei campi del form (`<input asp-for="Customer.Name" />`) e accettare l'input.</span><span class="sxs-lookup"><span data-stu-id="028d2-204">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="028d2-205">Il codice seguente rappresenta la versione combinata della pagina di creazione:</span><span class="sxs-lookup"><span data-stu-id="028d2-205">The following code shows the combined version of the create page:</span></span>

<span data-ttu-id="028d2-206">[!code-cshtml[principale](index/sample/RazorPagesContacts/Pages/CreateCombined.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="028d2-206">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/CreateCombined.cshtml)]</span></span>

<span data-ttu-id="028d2-207">Anziché usare `@model`, si sfrutta una nuova funzionalità di Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="028d2-207">Rather than using `@model`, we're taking advantage of a new feature for Pages.</span></span> <span data-ttu-id="028d2-208">Per impostazione predefinita, la classe derivata da `Page` generata *è* il modello.</span><span class="sxs-lookup"><span data-stu-id="028d2-208">By default, the generated `Page`-derived class *is* the model.</span></span> <span data-ttu-id="028d2-209">Usare un *modello di visualizzazione* con le visualizzazioni Razor è una procedura consigliata.</span><span class="sxs-lookup"><span data-stu-id="028d2-209">Using a *view model* with Razor views is a best practice.</span></span> <span data-ttu-id="028d2-210">Con Razor Pages si ottiene *automaticamente* un modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="028d2-210">With Pages, you get a view model *automatically*.</span></span>

<span data-ttu-id="028d2-211">La modifica principale è la sostituzione dell'inserimento del costruttore con le proprietà inserite (`@inject`).</span><span class="sxs-lookup"><span data-stu-id="028d2-211">The main change is replacing constructor injection with injected (`@inject`) properties.</span></span> <span data-ttu-id="028d2-212">Questa pagina usa [@inject](xref:mvc/views/razor#inject) per l'[inserimento delle dipendenze del costruttore](xref:mvc/controllers/dependency-injection#constructor-injection).</span><span class="sxs-lookup"><span data-stu-id="028d2-212">This page uses [@inject](xref:mvc/views/razor#inject) for [constructor dependency injection](xref:mvc/controllers/dependency-injection#constructor-injection).</span></span> <span data-ttu-id="028d2-213">L'istruzione `@inject` genera e inizializza la proprietà `Db` usata in `OnPostAsync`.</span><span class="sxs-lookup"><span data-stu-id="028d2-213">The `@inject` statement generates and initializes the `Db` property that is used in `OnPostAsync`.</span></span> <span data-ttu-id="028d2-214">Le proprietà inserite (`@inject`) vengono impostate prima di eseguire i metodi gestore.</span><span class="sxs-lookup"><span data-stu-id="028d2-214">Injected (`@inject`) properties are set before handler methods run.</span></span>


<span data-ttu-id="028d2-215">La home page (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="028d2-215">The home page (*Index.cshtml*):</span></span>

<span data-ttu-id="028d2-216">[!code-cshtml[principale](index/sample/RazorPagesContacts/Pages/Index.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="028d2-216">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]</span></span>

<span data-ttu-id="028d2-217">Il file code-behind *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="028d2-217">The code behind *Index.cshtml.cs* file:</span></span>

<span data-ttu-id="028d2-218">[!code-cs[principale](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="028d2-218">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]</span></span>

<span data-ttu-id="028d2-219">Il file *Index.cshtml* contiene il markup seguente per creare un collegamento di modifica per ogni contatto:</span><span class="sxs-lookup"><span data-stu-id="028d2-219">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

```html
<a asp-page="./Edit" asp-route-id="@contact.Id">edit</a>
```

<span data-ttu-id="028d2-220">[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) ha usato l'attributo [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) per generare un collegamento alla pagina di modifica.</span><span class="sxs-lookup"><span data-stu-id="028d2-220">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/AnchorTagHelper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="028d2-221">Il collegamento contiene i dati della route con l'ID contatto.</span><span class="sxs-lookup"><span data-stu-id="028d2-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="028d2-222">Ad esempio `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="028d2-222">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="028d2-223">Il file *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="028d2-223">The *Pages/Edit.cshtml* file:</span></span>

<span data-ttu-id="028d2-224">[!code-cshtml[principale](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="028d2-224">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]</span></span>

<span data-ttu-id="028d2-225">La prima riga contiene la direttiva `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="028d2-225">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="028d2-226">Il vincolo di routing `"{id:int}"` indica alla pagina di accettare le richieste inviate alla pagina che contengono i dati della route `int`.</span><span class="sxs-lookup"><span data-stu-id="028d2-226">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="028d2-227">Se una richiesta inviata alla pagina non contiene dati della route che possono essere convertiti in un oggetto `int`, il runtime restituisce un errore HTTP 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="028d2-227">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="028d2-228">Il file *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="028d2-228">The *Pages/Edit.cshtml.cs* file:</span></span>

<span data-ttu-id="028d2-229">[!code-cs[principale](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]</span><span class="sxs-lookup"><span data-stu-id="028d2-229">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="028d2-230">XSRF/CSRF e Razor Pages</span><span class="sxs-lookup"><span data-stu-id="028d2-230">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="028d2-231">Non è necessario scrivere codice per la [convalida antifalsificazione](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="028d2-231">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="028d2-232">La generazione e la convalida del token antifalsificazione sono automaticamente incluse in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="028d2-232">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="028d2-233">Uso di layout, righe parzialmente eseguite, modelli e helper tag con Razor Pages</span><span class="sxs-lookup"><span data-stu-id="028d2-233">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="028d2-234">Le pagine funzionano con tutte le funzionalità del motore di visualizzazione Razor.</span><span class="sxs-lookup"><span data-stu-id="028d2-234">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="028d2-235">I layout, le righe parzialmente eseguite, i modelli, gli helper tag, *_ViewStart.cshtml* e *_ViewImports.cshtml* funzionano esattamente come nelle normali visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="028d2-235">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="028d2-236">La pagina verrà ora riorganizzata in modo da usufruire di alcune di queste funzionalità.</span><span class="sxs-lookup"><span data-stu-id="028d2-236">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="028d2-237">Aggiungere una [pagina di layout](xref:mvc/views/layout) a *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="028d2-237">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

<span data-ttu-id="028d2-238">[!code-cshtml[principale](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="028d2-238">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]</span></span>

<span data-ttu-id="028d2-239">Il [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="028d2-239">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="028d2-240">Controlla il layout di ogni pagina, a meno che la pagina non accetti il layout.</span><span class="sxs-lookup"><span data-stu-id="028d2-240">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="028d2-241">Importa le strutture HTML, ad esempio JavaScript e i fogli di stile.</span><span class="sxs-lookup"><span data-stu-id="028d2-241">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="028d2-242">Vedere l'articolo sulla [pagina Layout](xref:mvc/views/layout) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="028d2-242">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="028d2-243">La proprietà [Layout](xref:mvc/views/layout#specifying-a-layout) proprietà viene impostata in *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="028d2-243">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

<span data-ttu-id="028d2-244">[!code-cshtml[principale](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="028d2-244">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]</span></span>

<span data-ttu-id="028d2-245">Nota: il layout è nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="028d2-245">Note: The layout is in the *Pages* folder.</span></span> <span data-ttu-id="028d2-246">Le pagine cercano altre visualizzazioni (layout, modelli, righe parzialmente eseguite) in modo gerarchico, partendo dalla stessa cartella della pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="028d2-246">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="028d2-247">Un layout nella cartella *Pages* può essere usato da qualsiasi pagina Razor della cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="028d2-247">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="028d2-248">Si consiglia di **non** inserire il file di layout nella cartella *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="028d2-248">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="028d2-249">*Views/Shared* è un modello destinato alle visualizzazioni MVC.</span><span class="sxs-lookup"><span data-stu-id="028d2-249">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="028d2-250">Le pagine Razor devono basarsi sulla gerarchia di cartelle, non su convenzioni di percorso.</span><span class="sxs-lookup"><span data-stu-id="028d2-250">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="028d2-251">La ricerca delle visualizzazioni da una pagina Razor include la cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="028d2-251">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="028d2-252">Il layout, i modelli e le righe parzialmente eseguite in uso con i controller MVC e le visualizzazioni Razor standard *funzionano solo*.</span><span class="sxs-lookup"><span data-stu-id="028d2-252">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="028d2-253">Aggiungere un file *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="028d2-253">Add a *Pages/_ViewImports.cshtml* file:</span></span>

<span data-ttu-id="028d2-254">[!code-cshtml[principale](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="028d2-254">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="028d2-255">`@namespace` viene spiegato in seguito nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="028d2-255">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="028d2-256">La direttiva `@addTagHelper` inserisce gli [helper tag predefiniti](xref:mvc/views/tag-helpers/builtin-th/Index) in tutte le pagine presenti nella cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="028d2-256">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="028d2-257">Quando la direttiva `@namespace` viene usata in modo esplicito in una pagina:</span><span class="sxs-lookup"><span data-stu-id="028d2-257">When the `@namespace` directive is used explicitly on a page:</span></span>

<span data-ttu-id="028d2-258">[!code-cshtml[principale](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="028d2-258">[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]</span></span>

<span data-ttu-id="028d2-259">La direttiva imposta lo spazio dei nomi per la pagina.</span><span class="sxs-lookup"><span data-stu-id="028d2-259">The directive sets the namespace for the page.</span></span> <span data-ttu-id="028d2-260">La direttiva `@model` non deve necessariamente includere lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="028d2-260">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="028d2-261">Se la direttiva `@namespace` è contenuta in *_ViewImports.cshtml*, lo spazio dei nomi specificato indica il prefisso per lo spazio dei nomi generato nella pagina che importa la direttiva `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="028d2-261">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="028d2-262">Il resto dello spazio dei nomi generato (la parte del suffisso) è il percorso relativo separato dal punto tra la cartella contenente *_Viewimports.cshtml* e la cartella contenente la pagina.</span><span class="sxs-lookup"><span data-stu-id="028d2-262">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="028d2-263">Ad esempio, il file code-behind *Pages/Customers/Edit.cshtml.cs* imposta in modo esplicito lo spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="028d2-263">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

<span data-ttu-id="028d2-264">[!code-cs[principale](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=namespace)]</span><span class="sxs-lookup"><span data-stu-id="028d2-264">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=namespace)]</span></span>

<span data-ttu-id="028d2-265">Il file *Pages/_ViewImports.cshtml* file imposta il seguente spazio dei nomi:</span><span class="sxs-lookup"><span data-stu-id="028d2-265">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

<span data-ttu-id="028d2-266">[!code-cshtml[principale](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="028d2-266">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]</span></span>

<span data-ttu-id="028d2-267">Lo spazio dei nomi generato per la pagina Razor *Pages/Customers/Edit.cshtml* corrisponde al file code-behind.</span><span class="sxs-lookup"><span data-stu-id="028d2-267">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="028d2-268">La direttiva `@namespace` è stata progettata in modo che le classi C# aggiunte a un progetto e il codice generato dalle pagine *funzionino* senza dover aggiungere una direttiva `@using` per il file code-behind.</span><span class="sxs-lookup"><span data-stu-id="028d2-268">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="028d2-269">Nota: `@namespace` funziona anche con le normali visualizzazioni Razor.</span><span class="sxs-lookup"><span data-stu-id="028d2-269">Note: `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="028d2-270">Il file di visualizzazione *Pages/Create.cshtml* originale:</span><span class="sxs-lookup"><span data-stu-id="028d2-270">The original *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="028d2-271">[!code-cshtml[principale](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="028d2-271">[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]</span></span>

<span data-ttu-id="028d2-272">La pagina aggiornata:</span><span class="sxs-lookup"><span data-stu-id="028d2-272">The updated page:</span></span>

<span data-ttu-id="028d2-273">Il file di visualizzazione *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="028d2-273">The *Pages/Create.cshtml* view file:</span></span>

<span data-ttu-id="028d2-274">[!code-cshtml[principale](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="028d2-274">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]</span></span>

<span data-ttu-id="028d2-275">Il [progetto iniziale per Razor Pages](#rpvs17) contiene il file *Pages/_ValidationScriptsPartial.cshtml*, che esegue la convalida sul lato client.</span><span class="sxs-lookup"><span data-stu-id="028d2-275">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="028d2-276">Generazione di URL per le pagine</span><span class="sxs-lookup"><span data-stu-id="028d2-276">URL generation for Pages</span></span>

<span data-ttu-id="028d2-277">La pagina `Create`, riportata in precedenza, usa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="028d2-277">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

<span data-ttu-id="028d2-278">[!code-cs[principale](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="028d2-278">[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=OnPostAsync&highlight=10)]</span></span>

<span data-ttu-id="028d2-279">L'applicazione ha la seguente struttura di file o cartella</span><span class="sxs-lookup"><span data-stu-id="028d2-279">The app has the following file/folder structure</span></span>

* <span data-ttu-id="028d2-280">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="028d2-280">*/Pages*</span></span>

  * <span data-ttu-id="028d2-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="028d2-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="028d2-282">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="028d2-282">*/Customer*</span></span>

    * <span data-ttu-id="028d2-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="028d2-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="028d2-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="028d2-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="028d2-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="028d2-285">*Index.cshtml*</span></span>

<span data-ttu-id="028d2-286">Le pagine *Pages/Customers/Create.cshtml* e *Pages/Customers/Edit.cshtml* reindirizzano a *Pages/Index.cshtml* dopo l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="028d2-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="028d2-287">La stringa `/Index` fa parte dell'URI di accesso alla pagina precedente.</span><span class="sxs-lookup"><span data-stu-id="028d2-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="028d2-288">La stringa `/Index` può essere usata per generare gli URI alla pagina *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="028d2-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="028d2-289">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="028d2-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="028d2-290">Il nome della pagina è il percorso alla pagina dalla cartella radice */Pages*, inclusa una barra iniziale (`/`), ad esempio `/Index`.</span><span class="sxs-lookup"><span data-stu-id="028d2-290">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="028d2-291">Gli esempi di generazione di URL precedenti sono molto più ricco di funzionalità rispetto a livello di codice solo un URL.</span><span class="sxs-lookup"><span data-stu-id="028d2-291">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="028d2-292">La generazione di URL usa il [routing](xref:mvc/controllers/routing) ed è in grado di generare e codificare i parametri in base al modo in cui la route è definita nel percorso di destinazione.</span><span class="sxs-lookup"><span data-stu-id="028d2-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="028d2-293">La generazione di URL per le pagine supporta i nomi relativi.</span><span class="sxs-lookup"><span data-stu-id="028d2-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="028d2-294">La tabella seguente indica quale pagina di indice viene selezionata con diversi parametri `RedirectToPage` da *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="028d2-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="028d2-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="028d2-295">RedirectToPage(x)</span></span>| <span data-ttu-id="028d2-296">Pagina</span><span class="sxs-lookup"><span data-stu-id="028d2-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="028d2-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="028d2-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="028d2-298">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="028d2-298">*Pages/Index*</span></span> |
| <span data-ttu-id="028d2-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="028d2-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="028d2-300">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="028d2-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="028d2-301">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="028d2-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="028d2-302">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="028d2-302">*Pages/Index*</span></span> |
| <span data-ttu-id="028d2-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="028d2-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="028d2-304">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="028d2-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="028d2-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, e `RedirectToPage("../Index")` sono *nomi relativi*.</span><span class="sxs-lookup"><span data-stu-id="028d2-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="028d2-306">Il parametro `RedirectToPage` è *combinato* con il percorso della pagina corrente per calcolare il nome della pagina di destinazione.</span><span class="sxs-lookup"><span data-stu-id="028d2-306">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="028d2-307">Il collegamento dei nomi relativi è utile quando si compilano siti con una struttura complessa.</span><span class="sxs-lookup"><span data-stu-id="028d2-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="028d2-308">Se si usano i nomi relativi per il collegamento tra le pagine in una cartella, è possibile rinominare tale cartella.</span><span class="sxs-lookup"><span data-stu-id="028d2-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="028d2-309">Tutti i collegamenti continuano a funzionare perché non includono il nome della cartella.</span><span class="sxs-lookup"><span data-stu-id="028d2-309">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="028d2-310">TempData</span><span class="sxs-lookup"><span data-stu-id="028d2-310">TempData</span></span>

<span data-ttu-id="028d2-311">ASP.NET Core espone la proprietà [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) per un [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="028d2-311">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="028d2-312">Questa proprietà archivia i dati finché non viene letta.</span><span class="sxs-lookup"><span data-stu-id="028d2-312">This property stores data until it is read.</span></span> <span data-ttu-id="028d2-313">I metodi `Keep` e `Peek` possono essere usati per esaminare i dati senza eliminazione.</span><span class="sxs-lookup"><span data-stu-id="028d2-313">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="028d2-314">`TempData` è utile per il reindirizzamento quando i dati sono necessari per più di una singola richiesta.</span><span class="sxs-lookup"><span data-stu-id="028d2-314">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="028d2-315">L'attributo `[TempData]` è nuovo in ASP.NET Core 2.0 ed è supportato per controller e pagine.</span><span class="sxs-lookup"><span data-stu-id="028d2-315">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="028d2-316">Nel codice seguente il valore di `Message` viene impostato usando `TempData`.</span><span class="sxs-lookup"><span data-stu-id="028d2-316">The following code sets the value of `Message` using `TempData`.</span></span>
<span data-ttu-id="028d2-317">[!code-cs[principale](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,27-28&name=snippetTemp)]</span><span class="sxs-lookup"><span data-stu-id="028d2-317">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,27-28&name=snippetTemp)]</span></span>

<span data-ttu-id="028d2-318">Il markup seguente nel file *Pages/Customers/Index.cshtml* visualizza il valore di `Message` usando `TempData`.</span><span class="sxs-lookup"><span data-stu-id="028d2-318">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="028d2-319">Il file code-behind *Pages/Customers/Index.cshtml.cs* applica l'attributo `[TempData]` alla proprietà `Message`.</span><span class="sxs-lookup"><span data-stu-id="028d2-319">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="028d2-320">Per altre informazioni, vedere [TempData](xref:fundamentals/app-state#temp).</span><span class="sxs-lookup"><span data-stu-id="028d2-320">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="028d2-321">Più gestori per pagina</span><span class="sxs-lookup"><span data-stu-id="028d2-321">Multiple handlers per page</span></span>

<span data-ttu-id="028d2-322">La pagina seguente genera markup per due gestori di pagina usando l'helper tag `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="028d2-322">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

<span data-ttu-id="028d2-323">[!code-cshtml[principale](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="028d2-323">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span></span>

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="028d2-324">Il form nell'esempio precedente contiene due pulsanti di invio, ognuno dei quali usa `FormActionTagHelper` per l'invio a un URL diverso.</span><span class="sxs-lookup"><span data-stu-id="028d2-324">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="028d2-325">L'attributo `asp-page-handler` è correlato a `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="028d2-325">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="028d2-326">`asp-page-handler` genera gli URL che indirizzano a ognuno dei metodi gestore definiti da una pagina.</span><span class="sxs-lookup"><span data-stu-id="028d2-326">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="028d2-327">`asp-page` non è specificato poiché l'esempio è collegato alla pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="028d2-327">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="028d2-328">Il file code-behind:</span><span class="sxs-lookup"><span data-stu-id="028d2-328">The code-behind file:</span></span>

<span data-ttu-id="028d2-329">[!code-cs[principale](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]</span><span class="sxs-lookup"><span data-stu-id="028d2-329">[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]</span></span>

<span data-ttu-id="028d2-330">Il codice precedente usa *metodi gestore denominati*.</span><span class="sxs-lookup"><span data-stu-id="028d2-330">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="028d2-331">I metodi gestore denominati vengono creati usando il testo che nel nome segue `On<HTTP Verb>` e precede `Async` (se presente).</span><span class="sxs-lookup"><span data-stu-id="028d2-331">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="028d2-332">Nell'esempio precedente i metodi della pagina sono OnPost**JoinList**Async e OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="028d2-332">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="028d2-333">Rimuovendo *OnPost* e *Async*, i nomi dei gestori sono `JoinList` e `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="028d2-333">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

<span data-ttu-id="028d2-334">[!code-cshtml[principale](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span><span class="sxs-lookup"><span data-stu-id="028d2-334">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]</span></span>

<span data-ttu-id="028d2-335">Usando il codice precedente, il percorso URL che indirizza a `OnPostJoinListAsync` è `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="028d2-335">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="028d2-336">Il percorso URL che indirizza a `OnPostJoinListUCAsync` è `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="028d2-336">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="028d2-337">Personalizzazione del routing</span><span class="sxs-lookup"><span data-stu-id="028d2-337">Customizing Routing</span></span>

<span data-ttu-id="028d2-338">Se si vuole omettere la stringa di query `?handler=JoinList` dall'URL, è possibile modificare la route in modo da inserire il nome del gestore nella parte di percorso dell'URL.</span><span class="sxs-lookup"><span data-stu-id="028d2-338">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="028d2-339">È possibile personalizzare la route aggiungendo un modello di route racchiuso tra virgolette doppie dopo la direttiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="028d2-339">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

<span data-ttu-id="028d2-340">[!code-cshtml[principale](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="028d2-340">[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]</span></span>


<span data-ttu-id="028d2-341">La route precedente inserisce il nome del gestore nel percorso URL anziché nella stringa di query.</span><span class="sxs-lookup"><span data-stu-id="028d2-341">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="028d2-342">`?` che segue `handler` indica che il parametro di route è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="028d2-342">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="028d2-343">È possibile usare `@page` per aggiungere altri segmenti e parametri alla route di una pagina.</span><span class="sxs-lookup"><span data-stu-id="028d2-343">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="028d2-344">Tutti gli elementi presenti vengono **aggiunti** alla route predefinita della pagina.</span><span class="sxs-lookup"><span data-stu-id="028d2-344">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="028d2-345">L'uso di un percorso assoluto o virtuale per modificare la route della pagina, ad esempio `"~/Some/Other/Path"`, non è supportato.</span><span class="sxs-lookup"><span data-stu-id="028d2-345">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="028d2-346">Configurazione e impostazioni</span><span class="sxs-lookup"><span data-stu-id="028d2-346">Configuration and settings</span></span>

<span data-ttu-id="028d2-347">Per configurare le opzioni avanzate, usare il metodo di estensione `AddRazorPagesOptions` nel generatore MVC:</span><span class="sxs-lookup"><span data-stu-id="028d2-347">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

<span data-ttu-id="028d2-348">[!code-cs[principale](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="028d2-348">[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet1)]</span></span>

<span data-ttu-id="028d2-349">Attualmente è possibile usare `RazorPagesOptions` per impostare la directory radice per le pagine o aggiungere convenzioni di modello applicativo per le pagine.</span><span class="sxs-lookup"><span data-stu-id="028d2-349">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="028d2-350">Si spera che in questo modo si potrà usufruire di una maggiore estendibilità in futuro.</span><span class="sxs-lookup"><span data-stu-id="028d2-350">We hope to enable more extensibility this way in the future.</span></span>

<span data-ttu-id="028d2-351">Per la precompilazione delle visualizzazioni, vedere l'articolo sulla [compilazione delle visualizzazioni Razor](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="028d2-351">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="028d2-352">[Scaricare o visualizzare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="028d2-352">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="028d2-353">Vedere l'articolo [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start) (Guida introduttiva a Razor Pages in ASP.NET Core), che si basa su questa introduzione.</span><span class="sxs-lookup"><span data-stu-id="028d2-353">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>
