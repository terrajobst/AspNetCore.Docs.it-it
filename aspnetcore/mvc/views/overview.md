---
title: Visualizzazioni in ASP.NET Core MVC
author: ardalis
description: Informazioni su come le visualizzazioni gestiscono la presentazione dei dati dell'app e l'interazione dell'utente in ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 12/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/overview
ms.openlocfilehash: b9af2068aec4326585eb2a8994399a16461db3be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="c801f-103">Visualizzazioni in ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="c801f-103">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="c801f-104">Di [Steve Smith](https://ardalis.com/) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c801f-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c801f-105">Questo documento illustra le visualizzazioni usate nelle applicazioni ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c801f-105">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="c801f-106">Per informazioni relative a Razor Pages, vedere [Introduzione a Razor Pages](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="c801f-106">For information on Razor Pages, see [Introduction to Razor Pages](xref:mvc/razor-pages/index).</span></span>

<span data-ttu-id="c801f-107">Nello schema **M**odel-**V**iew-**C**ontroller (MVC) la *visualizzazione* gestisce la presentazione dei dati dell'app e l'interazione dell'utente.</span><span class="sxs-lookup"><span data-stu-id="c801f-107">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="c801f-108">Una visualizzazione è un modello HTML con [markup Razor](xref:mvc/views/razor) incorporato.</span><span class="sxs-lookup"><span data-stu-id="c801f-108">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="c801f-109">Il markup Razor è il codice che interagisce con il markup HTML per produrre una pagina Web che viene inviata al client.</span><span class="sxs-lookup"><span data-stu-id="c801f-109">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="c801f-110">In ASP.NET Core MVC le visualizzazioni sono file con estensione *cshtml* che usano il [linguaggio di programmazione C#](/dotnet/csharp/) nel markup Razor.</span><span class="sxs-lookup"><span data-stu-id="c801f-110">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="c801f-111">I file di visualizzazione sono in genere raggruppati in cartelle denominate per ognuno dei [controller](xref:mvc/controllers/actions) dell'app.</span><span class="sxs-lookup"><span data-stu-id="c801f-111">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="c801f-112">Le cartelle vengono archiviate in una cartella *Views* alla radice dell'app:</span><span class="sxs-lookup"><span data-stu-id="c801f-112">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Cartella delle visualizzazioni aperta in Esplora soluzioni di Visual Studio con la cartella Home aperta per mostrare i file About.cshtml, Contact.cshtml e Index.cshtml](overview/_static/views_solution_explorer.png)

<span data-ttu-id="c801f-114">Il controller *Home* è rappresentato da una cartella *Home* all'interno della cartella *Views*.</span><span class="sxs-lookup"><span data-stu-id="c801f-114">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="c801f-115">La cartella *Home* contiene le visualizzazioni per le pagine Web *About*, *Contact* e *Index* (home page).</span><span class="sxs-lookup"><span data-stu-id="c801f-115">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="c801f-116">Quando un utente richiede una di queste tre pagine Web, le azioni del controller nel controller *Home* determinano quale delle tre visualizzazioni verrà usata per compilare e restituire una pagina Web all'utente.</span><span class="sxs-lookup"><span data-stu-id="c801f-116">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="c801f-117">Usare i [layout](xref:mvc/views/layout) per offrire sezioni di pagine Web coerenti e ridurre la ripetizione del codice.</span><span class="sxs-lookup"><span data-stu-id="c801f-117">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="c801f-118">I layout spesso includono l'intestazione, elementi di navigazione e menu, nonché il piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="c801f-118">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="c801f-119">L'intestazione e il piè di pagina in genere contengono il markup boilerplate per molti elementi di metadati e i collegamenti alle risorse di script e stile.</span><span class="sxs-lookup"><span data-stu-id="c801f-119">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="c801f-120">I layout consentono di evitare questo markup boilerplate nelle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c801f-120">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="c801f-121">Le [visualizzazioni parziali](xref:mvc/views/partial) riducono la duplicazione del codice grazie alla gestione delle parti riutilizzabili delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c801f-121">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="c801f-122">Una visualizzazione parziale è ad esempio utile per la biografia di un autore nel sito Web di un blog che viene mostrata in diverse visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c801f-122">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="c801f-123">Nel caso della biografia di un autore, il contenuto di visualizzazione è ordinario e non richiede l'esecuzione di codice per produrre il contenuto per la pagina Web.</span><span class="sxs-lookup"><span data-stu-id="c801f-123">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="c801f-124">Il contenuto della biografia di un autore è disponibile per la visualizzazione tramite la sola associazione di modelli, quindi l'uso di una visualizzazione parziale per questo tipo di contenuto è ideale.</span><span class="sxs-lookup"><span data-stu-id="c801f-124">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="c801f-125">I [componenti di visualizzazione](xref:mvc/views/view-components) sono simili alle visualizzazioni parziali nel senso che consentono di ridurre il codice ripetitivo, ma sono adatti per visualizzare contenuto che richiede l'esecuzione di codice nel server al fine di eseguire il rendering della pagina Web.</span><span class="sxs-lookup"><span data-stu-id="c801f-125">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="c801f-126">I componenti di visualizzazione sono utili quando il contenuto sottoposto a rendering richiede l'interazione con il database, ad esempio nel caso del carrello acquisti di un sito Web.</span><span class="sxs-lookup"><span data-stu-id="c801f-126">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="c801f-127">I componenti di visualizzazione non si limitano all'associazione di modelli per produrre l'output della pagina Web.</span><span class="sxs-lookup"><span data-stu-id="c801f-127">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="c801f-128">Vantaggi dell'uso delle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="c801f-128">Benefits of using views</span></span>

<span data-ttu-id="c801f-129">Le visualizzazioni consentono di stabilire una [progettazione **S**eparation **o**f **C**oncerns (SoC)](http://deviq.com/separation-of-concerns/) all'interno di un'app MVC separando il markup dell'interfaccia utente da altre parti dell'app.</span><span class="sxs-lookup"><span data-stu-id="c801f-129">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="c801f-130">La progettazione SoC rende l'app modulare offrendo diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="c801f-130">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="c801f-131">L'app risulta più facile da gestire perché è organizzata meglio.</span><span class="sxs-lookup"><span data-stu-id="c801f-131">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="c801f-132">Le visualizzazioni sono in genere raggruppate per funzionalità dell'app.</span><span class="sxs-lookup"><span data-stu-id="c801f-132">Views are generally grouped by app feature.</span></span> <span data-ttu-id="c801f-133">Individuare le visualizzazioni correlate quando si lavora su una funzionalità risulterà quindi più semplice.</span><span class="sxs-lookup"><span data-stu-id="c801f-133">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="c801f-134">Le parti dell'app sono a regime di controllo libero.</span><span class="sxs-lookup"><span data-stu-id="c801f-134">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="c801f-135">È possibile compilare e aggiornare le visualizzazioni dell'app separatamente dai componenti di logica di business e accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="c801f-135">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="c801f-136">È possibile modificare le visualizzazioni dell'app senza dover necessariamente aggiornare altre parti dell'app.</span><span class="sxs-lookup"><span data-stu-id="c801f-136">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="c801f-137">Poiché le visualizzazioni sono unità distinte, testare le parti dell'interfaccia utente dell'app risulta più semplice.</span><span class="sxs-lookup"><span data-stu-id="c801f-137">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="c801f-138">Grazie alla migliore organizzazione, le probabilità che le sezioni dell'interfaccia utente vengano ripetute accidentalmente sono minori.</span><span class="sxs-lookup"><span data-stu-id="c801f-138">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="c801f-139">Creazione di una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="c801f-139">Creating a view</span></span>

<span data-ttu-id="c801f-140">Le visualizzazioni specifiche di un controller vengono create nella cartella *Views/[ControllerName]*.</span><span class="sxs-lookup"><span data-stu-id="c801f-140">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="c801f-141">Le visualizzazioni condivise tra i controller vengono inserite nella cartella *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="c801f-141">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="c801f-142">Per creare una visualizzazione aggiungere un nuovo file e assegnargli lo stesso nome dell'azione del controller a essa associata con l'estensione *cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c801f-142">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="c801f-143">Per creare una visualizzazione che corrisponda all'azione *About* nel controller *Home*, creare un file *About.cshtml* nella cartella *Views/Home*:</span><span class="sxs-lookup"><span data-stu-id="c801f-143">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="c801f-144">Il markup *Razor* inizia con il simbolo `@`.</span><span class="sxs-lookup"><span data-stu-id="c801f-144">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="c801f-145">Eseguire le istruzioni C# inserendo il codice C# entro i [blocchi di codice Razor](xref:mvc/views/razor#razor-code-blocks) racchiusi tra le parentesi graffe (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="c801f-145">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="c801f-146">Vedere ad esempio l'assegnazione di "About" a `ViewData["Title"]` illustrato sopra.</span><span class="sxs-lookup"><span data-stu-id="c801f-146">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="c801f-147">È possibile visualizzare i valori in HTML facendo semplicemente riferimento al valore con il simbolo `@`.</span><span class="sxs-lookup"><span data-stu-id="c801f-147">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="c801f-148">Vedere il contenuto degli elementi `<h2>` e `<h3>` riportati sopra.</span><span class="sxs-lookup"><span data-stu-id="c801f-148">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="c801f-149">Il contenuto della visualizzazione illustrato sopra è solo una parte dell'intera pagina Web di cui viene eseguito il rendering per l'utente.</span><span class="sxs-lookup"><span data-stu-id="c801f-149">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="c801f-150">Il resto del layout della pagina e altri aspetti comuni della visualizzazione sono specificati in altri file.</span><span class="sxs-lookup"><span data-stu-id="c801f-150">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="c801f-151">Per altre informazioni, vedere l'[argomento Layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="c801f-151">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="c801f-152">Modalità di scelta delle visualizzazioni da parte dei controller</span><span class="sxs-lookup"><span data-stu-id="c801f-152">How controllers specify views</span></span>

<span data-ttu-id="c801f-153">Le visualizzazioni vengono in genere restituite da azioni come [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), ovvero un tipo di [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="c801f-153">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="c801f-154">Il metodo dell'azione può creare e restituire direttamente un elemento `ViewResult`, ma questa procedura non è molto comune.</span><span class="sxs-lookup"><span data-stu-id="c801f-154">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="c801f-155">Poiché la maggior parte dei controller ereditano da [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), si usa semplicemente il metodo helper `View` per restituire l'elemento `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="c801f-155">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="c801f-156">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="c801f-156">*HomeController.cs*</span></span>

[!code-csharp[](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="c801f-157">Quando viene restituita questa azione, il rendering della visualizzazione *About.cshtml* nell'ultima sezione corrisponderà alla pagina Web seguente:</span><span class="sxs-lookup"><span data-stu-id="c801f-157">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![Rendering della pagina About nel browser Microsoft Edge](overview/_static/about-page.png)

<span data-ttu-id="c801f-159">Il metodo helper `View` ha diversi overload.</span><span class="sxs-lookup"><span data-stu-id="c801f-159">The `View` helper method has several overloads.</span></span> <span data-ttu-id="c801f-160">È possibile specificare:</span><span class="sxs-lookup"><span data-stu-id="c801f-160">You can optionally specify:</span></span>

* <span data-ttu-id="c801f-161">Una visualizzazione esplicita da restituire:</span><span class="sxs-lookup"><span data-stu-id="c801f-161">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="c801f-162">Un [modello](xref:mvc/models/model-binding) da passare alla visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="c801f-162">A [model](xref:mvc/models/model-binding) to pass to the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="c801f-163">Una visualizzazione e un modello:</span><span class="sxs-lookup"><span data-stu-id="c801f-163">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="c801f-164">Individuazione delle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="c801f-164">View discovery</span></span>

<span data-ttu-id="c801f-165">Quando un'azione restituisce una visualizzazione viene eseguito un processo denominato *individuazione delle visualizzazioni*.</span><span class="sxs-lookup"><span data-stu-id="c801f-165">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="c801f-166">Questo processo determina il file di visualizzazione che verrà usato in base al nome della visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c801f-166">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="c801f-167">In base al comportamento predefinito, il metodo `View` (`return View();`) restituisce una visualizzazione con lo stesso nome del metodo dell'azione dal quale viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="c801f-167">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="c801f-168">Nel caso di *About*, ad esempio, il nome del metodo `ActionResult` del controller viene usato per cercare un file di visualizzazione denominato *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c801f-168">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="c801f-169">Il runtime cerca prima la visualizzazione nella cartella *Views/[ControllerName]*.</span><span class="sxs-lookup"><span data-stu-id="c801f-169">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="c801f-170">Se la cartella non contiene una visualizzazione corrispondente, la ricerca passerà alla cartella *Shared*.</span><span class="sxs-lookup"><span data-stu-id="c801f-170">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="c801f-171">La restituzione implicita di `ViewResult` con `return View();` o il passaggio esplicito del nome della visualizzazione al metodo `View` con `return View("<ViewName>");` non sono rilevanti.</span><span class="sxs-lookup"><span data-stu-id="c801f-171">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="c801f-172">In entrambi i casi, l'individuazione delle visualizzazioni cercherà un file di visualizzazione corrispondente in quest'ordine:</span><span class="sxs-lookup"><span data-stu-id="c801f-172">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="c801f-173">*Viste /\[ControllerName] /\[ViewName]. cshtml*</span><span class="sxs-lookup"><span data-stu-id="c801f-173">*Views/\[ControllerName]/\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="c801f-174">*Views/Shared/\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="c801f-174">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="c801f-175">È possibile fornire il percorso di un file di visualizzazione anziché il nome di una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c801f-175">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="c801f-176">Se si usa un percorso assoluto che inizia alla radice dell'app (che inizia facoltativamente con "/" o "~/") è necessario specificare l'estensione *cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c801f-176">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="c801f-177">È anche possibile usare un percorso relativo per specificare le visualizzazioni presenti in directory diverse senza l'estensione *cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c801f-177">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="c801f-178">All'interno di `HomeController` è possibile restituire la visualizzazione *Index* delle visualizzazioni *Manage* con il percorso relativo:</span><span class="sxs-lookup"><span data-stu-id="c801f-178">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="c801f-179">In modo analogo, è possibile indicare la directory corrente specifica dei controller con il prefisso "./":</span><span class="sxs-lookup"><span data-stu-id="c801f-179">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="c801f-180">Le [visualizzazioni parziali](xref:mvc/views/partial) e i [componenti di visualizzazione](xref:mvc/views/view-components) usano meccanismi di individuazione simili, ma non identici.</span><span class="sxs-lookup"><span data-stu-id="c801f-180">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="c801f-181">È possibile personalizzare la convenzione predefinita per la modalità di ricerca delle visualizzazioni all'interno dell'app usando un'interfaccia [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander) personalizzata.</span><span class="sxs-lookup"><span data-stu-id="c801f-181">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="c801f-182">L'individuazione delle visualizzazioni si basa sull'individuazione dei file di visualizzazione in base al nome del file.</span><span class="sxs-lookup"><span data-stu-id="c801f-182">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="c801f-183">Se il file system sottostante prevede la distinzione tra maiuscole e minuscole, per i nomi delle visualizzazioni verrà probabilmente applicata questa distinzione.</span><span class="sxs-lookup"><span data-stu-id="c801f-183">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="c801f-184">Per la compatibilità tra sistemi operativi, rispettare la distinzione maiuscole/minuscole tra i nomi di controller e azioni e i nomi di cartelle e file di visualizzazione associati.</span><span class="sxs-lookup"><span data-stu-id="c801f-184">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="c801f-185">In caso di errore relativo a un file di visualizzazione non trovato durante l'uso di un file system con distinzione tra maiuscole e minuscole, verificare che tra il nome del file di visualizzazione richiesto e il nome effettivo del file di visualizzazione l'uso delle maiuscole e minuscole corrisponda.</span><span class="sxs-lookup"><span data-stu-id="c801f-185">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="c801f-186">Per manutenibilità e chiarezza, seguire le procedure consigliate relative all'organizzazione della struttura di file per le visualizzazioni in modo da riflettere le relazioni tra controller, azioni e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c801f-186">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="c801f-187">Passaggio dei dati alle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="c801f-187">Passing data to views</span></span>

<span data-ttu-id="c801f-188">Per passare i dati alle visualizzazioni è possibile usare diversi approcci.</span><span class="sxs-lookup"><span data-stu-id="c801f-188">You can pass data to views using several approaches.</span></span> <span data-ttu-id="c801f-189">L'approccio più efficace consiste nello specificare un tipo di [modello](xref:mvc/models/model-binding) nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c801f-189">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="c801f-190">Questo modello è comunemente noto come *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="c801f-190">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="c801f-191">Un'istanza del tipo viewmodel viene passato alla visualizzazione dall'azione.</span><span class="sxs-lookup"><span data-stu-id="c801f-191">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="c801f-192">L'uso di un viewmodel per passare i dati a una visualizzazione consente a quest'ultima di trarre vantaggio dal controllo con tipizzazione *forte*.</span><span class="sxs-lookup"><span data-stu-id="c801f-192">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="c801f-193">L'espressione *tipizzazione forte* o *fortemente tipizzato* indica che ogni variabile e costante ha un tipo definito in modo esplicito, ad esempio, `string`, `int` o `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="c801f-193">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="c801f-194">La validità dei tipi usati in una visualizzazione viene verificata in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="c801f-194">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="c801f-195">In [Visual Studio](https://www.visualstudio.com/vs/) e [Visual Studio Code](https://code.visualstudio.com/) i membri delle classi fortemente tipizzati vengono elencati usando una funzionalità denominata [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="c801f-195">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="c801f-196">Per visualizzare le proprietà di un elemento viewmodel, digitare il nome della variabile per l'elemento viewmodel seguito da un punto (`.`).</span><span class="sxs-lookup"><span data-stu-id="c801f-196">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="c801f-197">Ciò consente di scrivere il codice più velocemente con un minor numero di errori.</span><span class="sxs-lookup"><span data-stu-id="c801f-197">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="c801f-198">Specificare un modello usando la direttiva `@model`.</span><span class="sxs-lookup"><span data-stu-id="c801f-198">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="c801f-199">Usare il modello con `@Model`:</span><span class="sxs-lookup"><span data-stu-id="c801f-199">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="c801f-200">Per fornire il modello alla visualizzazione, il controller lo passa come parametro:</span><span class="sxs-lookup"><span data-stu-id="c801f-200">To provide the model to the view, the controller passes it as a parameter:</span></span>

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

<span data-ttu-id="c801f-201">Non ci sono limitazioni ai tipi di modello che è possibile fornire a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c801f-201">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="c801f-202">È consigliabile usare viewmodel **P**lain **O**ld **C**LR **O**bject (POCO) per i quali non siano stati definiti comportamenti (metodi) o ne siano stati definiti solo alcuni.</span><span class="sxs-lookup"><span data-stu-id="c801f-202">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="c801f-203">Le classi viewmodel sono in genere archiviate nella cartella *Models* o in una cartella *ViewModels* separata alla radice dell'app.</span><span class="sxs-lookup"><span data-stu-id="c801f-203">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="c801f-204">Il viewmodel *Address* usato nell'esempio precedente è un viewmodel POCO archiviato in un file denominato *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="c801f-204">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> <span data-ttu-id="c801f-205">Nulla impedisce di usare le stesse classi per i tipi viewmodel e i tipi del modello aziendale.</span><span class="sxs-lookup"><span data-stu-id="c801f-205">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="c801f-206">L'uso di modelli separati consente tuttavia la variazione delle visualizzazioni indipendentemente dalle parti relative alla logica di business e all'accesso ai dati dell'app.</span><span class="sxs-lookup"><span data-stu-id="c801f-206">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="c801f-207">La separazione di modelli e viewmodel offre anche vantaggi in termini di sicurezza quando i modelli usano l'[associazione di modelli](xref:mvc/models/model-binding) e la [convalida](xref:mvc/models/validation) per i dati inviati all'app dall'utente.</span><span class="sxs-lookup"><span data-stu-id="c801f-207">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="c801f-208">Dati con tipizzazione debole (ViewData e ViewBag)</span><span class="sxs-lookup"><span data-stu-id="c801f-208">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="c801f-209">Nota: `ViewBag` non è disponibile in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c801f-209">Note: `ViewBag` isn't available in the Razor Pages.</span></span>

<span data-ttu-id="c801f-210">Oltre alle visualizzazioni fortemente tipizzate, le visualizzazioni hanno accesso a una raccolta di dati *con tipizzazione debole* o *debolmente tipizzata*.</span><span class="sxs-lookup"><span data-stu-id="c801f-210">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="c801f-211">A differenza della tipizzazione forte, la *tipizzazione debole*, o l'espressione *debolmente tipizzato*, indica che il tipo di dati in uso non viene dichiarato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="c801f-211">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="c801f-212">È possibile usare la raccolta di dati con tipizzazione debole per passare piccole quantità di dati da e verso i controller e le visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c801f-212">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="c801f-213">Passaggio dei dati tra...</span><span class="sxs-lookup"><span data-stu-id="c801f-213">Passing data between a ...</span></span>                        | <span data-ttu-id="c801f-214">Esempio</span><span class="sxs-lookup"><span data-stu-id="c801f-214">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="c801f-215">Un controller e una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="c801f-215">Controller and a view</span></span>                             | <span data-ttu-id="c801f-216">Popolamento di dati in un elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="c801f-216">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="c801f-217">Una visualizzazione e una [visualizzazione Layout](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="c801f-217">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="c801f-218">Impostazione del contenuto dell'elemento **\<title>** nella visualizzazione Layout da un file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c801f-218">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="c801f-219">Una [visualizzazione parziale](xref:mvc/views/partial) e una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="c801f-219">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="c801f-220">Widget che visualizza i dati in base alla pagina Web richiesta dall'utente.</span><span class="sxs-lookup"><span data-stu-id="c801f-220">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="c801f-221">È possibile fare riferimento a questa raccolta tramite le proprietà `ViewData` o `ViewBag` nei controller e nelle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c801f-221">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="c801f-222">La proprietà `ViewData` è un dizionario di oggetti con tipizzazione debole.</span><span class="sxs-lookup"><span data-stu-id="c801f-222">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="c801f-223">La proprietà `ViewBag` è un wrapper di `ViewData` che offre proprietà dinamiche per la raccolta `ViewData` sottostante.</span><span class="sxs-lookup"><span data-stu-id="c801f-223">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="c801f-224">`ViewData` e `ViewBag` vengono risolte in modo dinamico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="c801f-224">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="c801f-225">Poiché non offrono il controllo del tipo in fase di compilazione, entrambe sono in genere più soggette a errori rispetto all'uso di un elemento viewmodel.</span><span class="sxs-lookup"><span data-stu-id="c801f-225">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="c801f-226">Per questo motivo, alcuni sviluppatori preferiscono non usare mai `ViewData` e `ViewBag` o usarle il meno possibile.</span><span class="sxs-lookup"><span data-stu-id="c801f-226">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="c801f-227">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="c801f-227">**ViewData**</span></span>

<span data-ttu-id="c801f-228">`ViewData` è un oggetto [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) a cui si accede tramite le chiavi `string`.</span><span class="sxs-lookup"><span data-stu-id="c801f-228">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="c801f-229">I dati di tipo stringa possono essere archiviati e usati direttamente, senza la necessità di un cast, ma è necessario eseguire il cast di altri valori dell'oggetto `ViewData` in tipi specifici quando vengono estratti.</span><span class="sxs-lookup"><span data-stu-id="c801f-229">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="c801f-230">È possibile usare `ViewData` per passare i dati dai controller alle visualizzazioni e al loro interno, inclusi [visualizzazioni parziali](xref:mvc/views/partial) e [layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="c801f-230">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="c801f-231">Nell'esempio seguente vengono impostati i valori per una formula di saluto e un indirizzo usando `ViewData` in un'azione:</span><span class="sxs-lookup"><span data-stu-id="c801f-231">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

<span data-ttu-id="c801f-232">Lavorare con i dati in una visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="c801f-232">Work with the data in a view:</span></span>

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

<span data-ttu-id="c801f-233">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="c801f-233">**ViewBag**</span></span>

<span data-ttu-id="c801f-234">Nota: `ViewBag` non è disponibile in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c801f-234">Note: `ViewBag` isn't available in the Razor Pages.</span></span>

<span data-ttu-id="c801f-235">`ViewBag` è un oggetto [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) che consente l'accesso dinamico agli oggetti archiviati in `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="c801f-235">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="c801f-236">`ViewBag` può risultare più comodo da usare poiché non richiede l'esecuzione del cast.</span><span class="sxs-lookup"><span data-stu-id="c801f-236">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="c801f-237">Nell'esempio seguente viene illustrato come usare `ViewBag` con lo stesso risultato che si ottiene con l'uso di `ViewData` descritto in precedenza:</span><span class="sxs-lookup"><span data-stu-id="c801f-237">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

<span data-ttu-id="c801f-238">**Uso simultaneo di ViewData e ViewBag**</span><span class="sxs-lookup"><span data-stu-id="c801f-238">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="c801f-239">Nota: `ViewBag` non è disponibile in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c801f-239">Note: `ViewBag` isn't available in the Razor Pages.</span></span>

<span data-ttu-id="c801f-240">Dal momento che `ViewData` e `ViewBag` fanno riferimento alla stessa raccolta `ViewData` sottostante, è possibile usare `ViewData` e `ViewBag` e combinarle durante la lettura e la scrittura dei valori.</span><span class="sxs-lookup"><span data-stu-id="c801f-240">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="c801f-241">Impostare il titolo usando `ViewBag` e la descrizione usando `ViewData` all'inizio di una visualizzazione *About.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c801f-241">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="c801f-242">Leggere le proprietà ma invertire l'uso di `ViewData` e `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="c801f-242">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="c801f-243">Nel file *_Layout.cshtml* ottenere il titolo usando `ViewData` e ottenere la descrizione usando `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="c801f-243">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="c801f-244">Tenere presente che per `ViewData` non è necessario eseguire il cast delle stringhe.</span><span class="sxs-lookup"><span data-stu-id="c801f-244">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="c801f-245">È possibile usare `@ViewData["Title"]` senza eseguire il cast.</span><span class="sxs-lookup"><span data-stu-id="c801f-245">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="c801f-246">L'uso simultaneo di `ViewData` e `ViewBag` funziona, così come funziona la combinazione di entrambe durante la lettura e la scrittura delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="c801f-246">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="c801f-247">Viene eseguito il rendering del markup seguente:</span><span class="sxs-lookup"><span data-stu-id="c801f-247">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="c801f-248">**Riepilogo delle differenze tra ViewData e ViewBag**</span><span class="sxs-lookup"><span data-stu-id="c801f-248">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="c801f-249">`ViewBag` non è disponibile in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c801f-249">`ViewBag` isn't available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="c801f-250">Deriva da [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) e include quindi proprietà di dizionario che possono risultare utili, ad esempio `ContainsKey`, `Add`, `Remove` e `Clear`.</span><span class="sxs-lookup"><span data-stu-id="c801f-250">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="c801f-251">Le chiavi nel dizionario sono stringhe, pertanto lo spazio vuoto è consentito.</span><span class="sxs-lookup"><span data-stu-id="c801f-251">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="c801f-252">Esempio: `ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="c801f-252">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="c801f-253">Per usare `ViewData` è necessario eseguire il cast di tutti i tipi diversi da `string` nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c801f-253">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="c801f-254">Deriva da [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) e consente quindi la creazione di proprietà dinamiche usando la notazione del punto (`@ViewBag.SomeKey = <value or object>`). L'esecuzione del cast non è necessaria.</span><span class="sxs-lookup"><span data-stu-id="c801f-254">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="c801f-255">La sintassi di `ViewBag` velocizza l'aggiunta in controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c801f-255">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="c801f-256">Verificare la presenza di valori Null è più facile.</span><span class="sxs-lookup"><span data-stu-id="c801f-256">Simpler to check for null values.</span></span> <span data-ttu-id="c801f-257">Esempio: `@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="c801f-257">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="c801f-258">**Quando usare ViewData o ViewBag**</span><span class="sxs-lookup"><span data-stu-id="c801f-258">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="c801f-259">`ViewData` e `ViewBag` sono entrambi approcci ugualmente validi per passare piccole quantità di dati tra controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c801f-259">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="c801f-260">La scelta di quello da usare è basata sulla preferenza.</span><span class="sxs-lookup"><span data-stu-id="c801f-260">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="c801f-261">È possibile combinare oggetti `ViewData` e `ViewBag`. La lettura e la gestione del codice risulteranno tuttavia più semplici con un unico approccio usato in modo coerente.</span><span class="sxs-lookup"><span data-stu-id="c801f-261">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="c801f-262">Entrambi gli approcci vengono risolti in modo dinamico in fase di esecuzione e possono quindi determinare errori di runtime.</span><span class="sxs-lookup"><span data-stu-id="c801f-262">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="c801f-263">Alcuni team di sviluppo li evitano.</span><span class="sxs-lookup"><span data-stu-id="c801f-263">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="c801f-264">Visualizzazioni dinamiche</span><span class="sxs-lookup"><span data-stu-id="c801f-264">Dynamic views</span></span>

<span data-ttu-id="c801f-265">Le visualizzazioni che non dichiarano un tipo di modello usando `@model` ma a cui è stata passata l'istanza di un modello, ad esempio `return View(Address);`, possono fare riferimento alle proprietà dell'istanza in modo dinamico:</span><span class="sxs-lookup"><span data-stu-id="c801f-265">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="c801f-266">Questa funzionalità offre flessibilità ma non offre la protezione della compilazione o IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="c801f-266">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="c801f-267">Se la proprietà non esiste, in fase di esecuzione la generazione della pagina Web non riesce.</span><span class="sxs-lookup"><span data-stu-id="c801f-267">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="c801f-268">Altre funzionalità delle visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="c801f-268">More view features</span></span>

<span data-ttu-id="c801f-269">Gli [helper tag](xref:mvc/views/tag-helpers/intro) semplificano l'aggiunta del comportamento sul lato server ai tag HTML esistenti.</span><span class="sxs-lookup"><span data-stu-id="c801f-269">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="c801f-270">Grazie all'uso degli helper tag, è possibile evitare di scrivere codice personalizzato o helper all'interno delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c801f-270">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="c801f-271">Gli helper tag vengono applicati come attributi agli elementi HTML e vengono ignorati dagli editor che non sono in grado di elaborarli.</span><span class="sxs-lookup"><span data-stu-id="c801f-271">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="c801f-272">Ciò consente di modificare ed eseguire il rendering del markup delle visualizzazioni in un'ampia gamma di strumenti.</span><span class="sxs-lookup"><span data-stu-id="c801f-272">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="c801f-273">La generazione di markup HTML personalizzato può essere ottenuta con molti helper HTML predefiniti.</span><span class="sxs-lookup"><span data-stu-id="c801f-273">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="c801f-274">Una logica dell'interfaccia utente più complessa può essere gestita dai [componenti di visualizzazione](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="c801f-274">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="c801f-275">I componenti di visualizzazione offrono lo stesso tipo di progettazione SoC offerta dai controller e dalle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="c801f-275">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="c801f-276">Possono eliminare la necessità di azioni e visualizzazioni che gestiscono i dati usati da elementi comuni dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="c801f-276">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="c801f-277">Come molti altri aspetti di ASP.NET Core, le visualizzazioni supportano l'[inserimento di dipendenze](xref:fundamentals/dependency-injection), consentendo che i servizi siano [inseriti nelle visualizzazioni](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c801f-277">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
