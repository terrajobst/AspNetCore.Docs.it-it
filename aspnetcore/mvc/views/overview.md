---
title: Visualizzazioni in ASP.NET MVC di base
author: ardalis
description: Informazioni su come viste di gestiscono la presentazione dei dati dell'app e l'interazione dell'utente in ASP.NET MVC di base.
keywords: ASP.NET Core, visualizzare, MVC, razor, viewmodel, viewdata, viewbag
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 2562d4e5fb85159e6ccb47990f54448ddc188077
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2017
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="30d87-104">Visualizzazioni in ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="30d87-104">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="30d87-105">Da [Steve Smith](https://ardalis.com/) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="30d87-105">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="30d87-106">Questo documento illustra le viste utilizzate nelle applicazioni ASP.NET MVC di base.</span><span class="sxs-lookup"><span data-stu-id="30d87-106">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="30d87-107">Per informazioni sulle pagine Razor, vedere [Introduzione alle pagine Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="30d87-107">For information on Razor Pages, see [Introduction to Razor Pages](xref:mvc/razor-pages/index).</span></span>

<span data-ttu-id="30d87-108">Nel **M**enti -**V**ISTA -**C**modello ontroller (MVC), il *vista* gestisce l'interazione utente e presentazione dei dati dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="30d87-108">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="30d87-109">Una vista è un modello HTML con incorporato [markup Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="30d87-109">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="30d87-110">Markup Razor è codice che interagisce con il markup HTML per produrre una pagina Web che viene inviata al client.</span><span class="sxs-lookup"><span data-stu-id="30d87-110">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="30d87-111">In ASP.NET MVC di base, le visualizzazioni sono *. cshtml* file che utilizzano il [linguaggio di programmazione c#](/dotnet/csharp/) nel codice Razor.</span><span class="sxs-lookup"><span data-stu-id="30d87-111">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="30d87-112">In genere, visualizzare i file sono raggruppati in cartelle denominate per ognuna dell'app [controller](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="30d87-112">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="30d87-113">Le cartelle vengono archiviate in un *viste* cartella principale dell'app:</span><span class="sxs-lookup"><span data-stu-id="30d87-113">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Esplora soluzioni di Visual Studio nella cartella viste è aperta con la cartella Home aprire per visualizzare i file cshtml About.cshtml e Contact.cshtml](overview/_static/views_solution_explorer.png)

<span data-ttu-id="30d87-115">Il *Home* controller è rappresentato da un *Home* cartella all'interno di *viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="30d87-115">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="30d87-116">Il *Home* cartella contiene le viste per il *su*, *contatto*, e *indice* pagine Web (Home Page).</span><span class="sxs-lookup"><span data-stu-id="30d87-116">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="30d87-117">Quando un utente richiede uno di questi tre pagine Web, le azioni del controller nel *Home* controller determinare quale delle tre visualizzazioni viene utilizzato per compilare e restituire una pagina Web all'utente.</span><span class="sxs-lookup"><span data-stu-id="30d87-117">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="30d87-118">Utilizzare [layout](xref:mvc/views/layout) per fornire le sezioni di pagina Web coerente e ridurre ripetizioni nel codice.</span><span class="sxs-lookup"><span data-stu-id="30d87-118">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="30d87-119">Layout contengono spesso l'intestazione, gli elementi di navigazione e i menu e il piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="30d87-119">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="30d87-120">L'intestazione e piè di pagina contengono in genere codice boilerplate per molti elementi di metadati e i collegamenti alle risorse di script e stile.</span><span class="sxs-lookup"><span data-stu-id="30d87-120">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="30d87-121">Layout consentono di evitare questo markup boilerplate nelle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="30d87-121">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="30d87-122">[Le visualizzazioni parziali](xref:mvc/views/partial) ridurre la duplicazione del codice mediante la gestione di parti riutilizzabili di viste.</span><span class="sxs-lookup"><span data-stu-id="30d87-122">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="30d87-123">Ad esempio, una visualizzazione parziale è utile per una biografia autore in un sito Web del blog che viene visualizzato in diverse visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="30d87-123">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="30d87-124">Una biografia autore è contenuto visualizzazione comune e non richiede codice da eseguire per produrre il contenuto per la pagina Web.</span><span class="sxs-lookup"><span data-stu-id="30d87-124">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="30d87-125">Contenuto biografia autore è disponibile per la visualizzazione per l'associazione di modelli da solo, pertanto l'utilizzo di una visualizzazione parziale per questo tipo di contenuto è ideale.</span><span class="sxs-lookup"><span data-stu-id="30d87-125">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="30d87-126">[Visualizzare i componenti](xref:mvc/views/view-components) sono simili a parziale viste che consentono di ridurre il codice ripetitivo, ma appropriati per visualizzare il contenuto che richiede il codice per l'esecuzione nel server per eseguire il rendering della pagina Web.</span><span class="sxs-lookup"><span data-stu-id="30d87-126">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="30d87-127">Visualizza i componenti sono utili quando il contenuto sottoposto a rendering richiede interazione con il database, ad esempio per un sito Web carrello degli acquisti.</span><span class="sxs-lookup"><span data-stu-id="30d87-127">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="30d87-128">Visualizza i componenti non sono limitati per l'associazione del modello per produrre l'output di pagina Web.</span><span class="sxs-lookup"><span data-stu-id="30d87-128">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="30d87-129">Vantaggi dell'utilizzo di viste</span><span class="sxs-lookup"><span data-stu-id="30d87-129">Benefits of using views</span></span>

<span data-ttu-id="30d87-130">Le viste consentono di definire un [ **S**eparation **o**f **C**oncerns (SoC) progettazione](http://deviq.com/separation-of-concerns/) all'interno di un'applicazione MVC separando il markup dell'interfaccia utente altre parti dell'app.</span><span class="sxs-lookup"><span data-stu-id="30d87-130">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="30d87-131">Dopo la progettazione SoC rende l'app modulare, che offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="30d87-131">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="30d87-132">L'app è più facile da gestire perché è meglio organizzato.</span><span class="sxs-lookup"><span data-stu-id="30d87-132">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="30d87-133">Le visualizzazioni vengono in genere raggruppate per funzionalità delle app.</span><span class="sxs-lookup"><span data-stu-id="30d87-133">Views are generally grouped by app feature.</span></span> <span data-ttu-id="30d87-134">Questo rende più semplice trovare viste correlate, quando si utilizza una funzionalità.</span><span class="sxs-lookup"><span data-stu-id="30d87-134">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="30d87-135">Le parti dell'app sono loosely coupled.</span><span class="sxs-lookup"><span data-stu-id="30d87-135">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="30d87-136">È possibile compilare e aggiornare le visualizzazioni dell'app separatamente dai componenti di accesso dati e logica di business.</span><span class="sxs-lookup"><span data-stu-id="30d87-136">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="30d87-137">È possibile modificare le visualizzazioni dell'app senza dover necessariamente aggiornare altre parti dell'app.</span><span class="sxs-lookup"><span data-stu-id="30d87-137">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="30d87-138">Risulta più semplice testare le parti dell'interfaccia utente dell'app, perché le visualizzazioni sono unità distinte.</span><span class="sxs-lookup"><span data-stu-id="30d87-138">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="30d87-139">A causa di una migliore organizzazione, è meno probabile che sarà accidentalmente ripetizione sezioni dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="30d87-139">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="30d87-140">Creazione di una vista</span><span class="sxs-lookup"><span data-stu-id="30d87-140">Creating a view</span></span>

<span data-ttu-id="30d87-141">Le viste che sono specifiche di un controller vengono create nel *viste / [ControllerName]* cartella.</span><span class="sxs-lookup"><span data-stu-id="30d87-141">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="30d87-142">Le viste che vengono condivisi tra i controller vengono inserite nel *Views/Shared* cartella.</span><span class="sxs-lookup"><span data-stu-id="30d87-142">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="30d87-143">Per creare una vista, aggiungere un nuovo file e assegnargli lo stesso nome dell'azione del controller associata con il *. cshtml* estensione di file.</span><span class="sxs-lookup"><span data-stu-id="30d87-143">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="30d87-144">Per creare una visualizzazione corrispondente con il *su* azione nel *Home* controller, creare un *About.cshtml* file nel *Views/Home*cartella:</span><span class="sxs-lookup"><span data-stu-id="30d87-144">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="30d87-145">*Razor* markup inizia con il `@` simbolo.</span><span class="sxs-lookup"><span data-stu-id="30d87-145">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="30d87-146">Eseguire istruzioni c# inserendo in c# il codice all'interno [blocchi di codice Razor](xref:mvc/views/razor#razor-code-blocks) disattivare le parentesi graffe (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="30d87-146">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="30d87-147">Ad esempio, vedere l'assegnazione di "About" per `ViewData["Title"]` illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="30d87-147">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="30d87-148">È possibile visualizzare i valori in HTML facendo riferimento il valore con il `@` simbolo.</span><span class="sxs-lookup"><span data-stu-id="30d87-148">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="30d87-149">Visualizzare il contenuto del `<h2>` e `<h3>` elementi sopra riportato.</span><span class="sxs-lookup"><span data-stu-id="30d87-149">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="30d87-150">Il contenuto della visualizzazione illustrato in precedenza è solo una parte dell'intera pagina Web che viene eseguito il rendering all'utente.</span><span class="sxs-lookup"><span data-stu-id="30d87-150">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="30d87-151">Il resto del layout della pagina e altri aspetti comuni della visualizzazione sono specificati in altri file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="30d87-151">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="30d87-152">Per ulteriori informazioni, vedere il [argomento Layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="30d87-152">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="30d87-153">Il modo in cui i controller specifica viste</span><span class="sxs-lookup"><span data-stu-id="30d87-153">How controllers specify views</span></span>

<span data-ttu-id="30d87-154">Le visualizzazioni vengono in genere restituite dalle azioni come un [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), che è un tipo di [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="30d87-154">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="30d87-155">Metodo di azione, è possibile creare e restituire un `ViewResult` direttamente, ma che non è in genere eseguito.</span><span class="sxs-lookup"><span data-stu-id="30d87-155">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="30d87-156">Poiché la maggior parte dei controller eredita [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), è sufficiente utilizzare il `View` metodo helper per restituire il `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="30d87-156">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="30d87-157">*HomeController. cs*</span><span class="sxs-lookup"><span data-stu-id="30d87-157">*HomeController.cs*</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="30d87-158">Quando questa azione viene restituito, il *About.cshtml* visualizzazione indicata nell'ultima sezione viene visualizzato come la seguente pagina Web:</span><span class="sxs-lookup"><span data-stu-id="30d87-158">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![Sulla pagina sottoposta a rendering nel browser Edge](overview/_static/about-page.png)

<span data-ttu-id="30d87-160">Il `View` metodo helper dispone di diversi overload.</span><span class="sxs-lookup"><span data-stu-id="30d87-160">The `View` helper method has several overloads.</span></span> <span data-ttu-id="30d87-161">È possibile specificare:</span><span class="sxs-lookup"><span data-stu-id="30d87-161">You can optionally specify:</span></span>

* <span data-ttu-id="30d87-162">Una vista esplicita da restituire:</span><span class="sxs-lookup"><span data-stu-id="30d87-162">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="30d87-163">A [modello](xref:mvc/models/model-binding) da passare alla vista:</span><span class="sxs-lookup"><span data-stu-id="30d87-163">A [model](xref:mvc/models/model-binding) to pass to the the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="30d87-164">Una vista e un modello:</span><span class="sxs-lookup"><span data-stu-id="30d87-164">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="30d87-165">Individuazione di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="30d87-165">View discovery</span></span>

<span data-ttu-id="30d87-166">Quando un'azione restituisce una visualizzazione, un processo denominato *individuazione visualizzazione* avviene.</span><span class="sxs-lookup"><span data-stu-id="30d87-166">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="30d87-167">Questo processo determina quale file di visualizzazione in base al nome di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="30d87-167">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="30d87-168">Il comportamento predefinito del `View` metodo (`return View();`) deve restituire una visualizzazione con lo stesso nome del metodo di azione da cui viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="30d87-168">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="30d87-169">Ad esempio, il *su* `ActionResult` nome del metodo del controller a cui viene utilizzato per cercare un file di visualizzazione denominato *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="30d87-169">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="30d87-170">In primo luogo, il runtime cerca *viste / [ControllerName]* cartella per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="30d87-170">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="30d87-171">Se non esiste una visualizzazione corrispondente non viene trovato, viene cercato il *Shared* cartella per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="30d87-171">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="30d87-172">Non è importante se si restituisce in modo implicito il `ViewResult` con `return View();` o passare in modo esplicito il nome della visualizzazione per il `View` metodo con `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="30d87-172">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="30d87-173">In entrambi i casi, visualizzare l'individuazione Cerca un file di visualizzazione corrispondente nell'ordine indicato:</span><span class="sxs-lookup"><span data-stu-id="30d87-173">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="30d87-174">*Viste /\[ControllerName]\[ViewName]. cshtml*</span><span class="sxs-lookup"><span data-stu-id="30d87-174">*Views/\[ControllerName]\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="30d87-175">*Viste/Shared/\[ViewName]. cshtml*</span><span class="sxs-lookup"><span data-stu-id="30d87-175">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="30d87-176">Invece di un nome di visualizzazione, è possibile specificare un percorso di file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="30d87-176">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="30d87-177">Se si utilizza un percorso assoluto a partire dalla radice dell'applicazione (facoltativamente inizia con "/" o "~ /"), il *. cshtml* estensione deve essere specificata:</span><span class="sxs-lookup"><span data-stu-id="30d87-177">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="30d87-178">È inoltre possibile utilizzare un percorso relativo per specificare le viste in directory diverse senza la *. cshtml* estensione.</span><span class="sxs-lookup"><span data-stu-id="30d87-178">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="30d87-179">All'interno di `HomeController`, è possibile restituire il *indice* visualizzazione del *Gestisci* viste con un percorso relativo:</span><span class="sxs-lookup"><span data-stu-id="30d87-179">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="30d87-180">Analogamente, è possibile indicare la directory di specifici controller corrente con il ". /" prefisso:</span><span class="sxs-lookup"><span data-stu-id="30d87-180">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="30d87-181">[Le visualizzazioni parziali](xref:mvc/views/partial) e [visualizzare componenti](xref:mvc/views/view-components) utilizzare meccanismi di individuazione simile (ma non identica).</span><span class="sxs-lookup"><span data-stu-id="30d87-181">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="30d87-182">È possibile personalizzare la convenzione predefinita per la modalità viste si trovano all'interno dell'app usando un oggetto personalizzato [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="30d87-182">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="30d87-183">Individuazione di visualizzazione si basa sull'individuazione di visualizzare i file in base al nome di file.</span><span class="sxs-lookup"><span data-stu-id="30d87-183">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="30d87-184">Se il file system sottostante è tra maiuscole e minuscole, visualizzare i nomi sono probabilmente maiuscole/minuscole.</span><span class="sxs-lookup"><span data-stu-id="30d87-184">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="30d87-185">Per garantire la compatibilità tra i sistemi operativi, maiuscole/minuscole tra controller e i nomi di azione e le cartelle di visualizzazione associata e i nomi di file.</span><span class="sxs-lookup"><span data-stu-id="30d87-185">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="30d87-186">Se si verifica un errore che non è possibile trovare un file di visualizzazione mentre si lavora con un sistema di file tra maiuscole e minuscole, verificare che le maiuscole e minuscole corrispondano tra il file di visualizzazione richiesta e il nome del file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="30d87-186">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="30d87-187">Seguire la procedura consigliata di organizzare la struttura dei file per le visualizzazioni in modo da riflettere le relazioni tra i controller, azioni e visualizzazioni per maggiore chiarezza e la manutenibilità.</span><span class="sxs-lookup"><span data-stu-id="30d87-187">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="30d87-188">Passaggio di dati a viste</span><span class="sxs-lookup"><span data-stu-id="30d87-188">Passing data to views</span></span>

<span data-ttu-id="30d87-189">È possibile passare dati alle viste utilizzando diversi approcci.</span><span class="sxs-lookup"><span data-stu-id="30d87-189">You can pass data to views using several approaches.</span></span> <span data-ttu-id="30d87-190">L'approccio più efficace consiste nello specificare un [modello](xref:mvc/models/model-binding) tipo nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="30d87-190">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="30d87-191">Questo modello è noto come un *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="30d87-191">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="30d87-192">Passare un'istanza del tipo viewmodel alla visualizzazione dall'azione.</span><span class="sxs-lookup"><span data-stu-id="30d87-192">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="30d87-193">Utilizzando un viewmodel di passare dati a una vista consente di sfruttare *sicuro* controllo dei tipi.</span><span class="sxs-lookup"><span data-stu-id="30d87-193">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="30d87-194">*Tipizzazione forte* (o *fortemente tipizzato*) significa che ogni variabile e costante dispone di un tipo definito in modo esplicito (ad esempio, `string`, `int`, o `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="30d87-194">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="30d87-195">In fase di compilazione, viene verificata la validità dei tipi utilizzati in una vista.</span><span class="sxs-lookup"><span data-stu-id="30d87-195">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="30d87-196">[Visual Studio](https://www.visualstudio.com/vs/) e [codice di Visual Studio](https://code.visualstudio.com/) sono elencati i membri di classe fortemente tipizzata utilizzando la funzione [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="30d87-196">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="30d87-197">Quando si desidera visualizzare le proprietà di un elemento viewmodel, digitare il nome della variabile per l'elemento viewmodel seguito da un punto (`.`).</span><span class="sxs-lookup"><span data-stu-id="30d87-197">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="30d87-198">Ciò consente di scrivere codice più rapidamente con meno errori.</span><span class="sxs-lookup"><span data-stu-id="30d87-198">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="30d87-199">Specificare un modello utilizzando il `@model` direttiva.</span><span class="sxs-lookup"><span data-stu-id="30d87-199">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="30d87-200">Utilizzare il modello con `@Model`:</span><span class="sxs-lookup"><span data-stu-id="30d87-200">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="30d87-201">Per fornire al modello per la visualizzazione, il controller passa come parametro:</span><span class="sxs-lookup"><span data-stu-id="30d87-201">To provide the model to the view, the controller passes it as a parameter:</span></span>

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

<span data-ttu-id="30d87-202">Non sono previste restrizioni per i tipi di modello che è possibile fornire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="30d87-202">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="30d87-203">Si consiglia di utilizzare **P**Incolla **O**ld **C**LR **O**ViewModel oggetto (POCO) con pochi o nessun comportamento (metodi) definita.</span><span class="sxs-lookup"><span data-stu-id="30d87-203">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="30d87-204">In genere, le classi viewmodel sono archiviate nel *modelli* cartella o un oggetto separato *ViewModel* cartella nella radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="30d87-204">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="30d87-205">Il *indirizzo* viewmodel utilizzato nell'esempio precedente è un elemento viewmodel POCO archiviati in un file denominato *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="30d87-205">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

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
> <span data-ttu-id="30d87-206">Non impedisce di utilizzare le stesse classi per i tipi di viewmodel e i tipi di modello di business.</span><span class="sxs-lookup"><span data-stu-id="30d87-206">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="30d87-207">L'utilizzo di modelli separati consente, tuttavia, le visualizzazioni variare in modo indipendente dai dati di logica di business e parti di accesso dell'app.</span><span class="sxs-lookup"><span data-stu-id="30d87-207">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="30d87-208">La separazione dei modelli e ViewModel offre vantaggi di sicurezza se i modelli usano [associazione del modello](xref:mvc/models/model-binding) e [convalida](xref:mvc/models/validation) per i dati inviati all'app dall'utente.</span><span class="sxs-lookup"><span data-stu-id="30d87-208">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="30d87-209">Dati con tipizzazione debole (ViewData e ViewBag)</span><span class="sxs-lookup"><span data-stu-id="30d87-209">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="30d87-210">Nota: `ViewBag` non è disponibile nelle pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="30d87-210">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="30d87-211">Oltre a visualizzazioni fortemente tipizzate, le visualizzazioni di avere a disposizione un *con tipizzazione debole* (chiamato anche *fortemente tipizzato*) insieme di dati.</span><span class="sxs-lookup"><span data-stu-id="30d87-211">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="30d87-212">A differenza dei tipi sicuri, *tipi deboli* (o *perdere tipi*) significa che non dichiarare in modo esplicito il tipo di dati in uso.</span><span class="sxs-lookup"><span data-stu-id="30d87-212">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="30d87-213">È possibile utilizzare la raccolta dei dati con tipizzazione debole per il passaggio di piccole quantità di dati da e verso i controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="30d87-213">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="30d87-214">Passaggio di dati tra un...</span><span class="sxs-lookup"><span data-stu-id="30d87-214">Passing data between a ...</span></span>                        | <span data-ttu-id="30d87-215">Esempio</span><span class="sxs-lookup"><span data-stu-id="30d87-215">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="30d87-216">Controller e una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="30d87-216">Controller and a view</span></span>                             | <span data-ttu-id="30d87-217">Popolamento di un elenco a discesa con i dati.</span><span class="sxs-lookup"><span data-stu-id="30d87-217">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="30d87-218">Visualizzazione e un [visualizzazione layout](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="30d87-218">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="30d87-219">L'impostazione di  **\<titolo >** contenuto dell'elemento in visualizzazione layout di finestra da un file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="30d87-219">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="30d87-220">[Visualizzazione parziale](xref:mvc/views/partial) e una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="30d87-220">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="30d87-221">Un widget che visualizza i dati in base a una pagina Web che l'utente ha richiesto.</span><span class="sxs-lookup"><span data-stu-id="30d87-221">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="30d87-222">È possibile fare riferimento a questa raccolta tramite il `ViewData` o `ViewBag` proprietà sul controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="30d87-222">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="30d87-223">Il `ViewData` proprietà è un dizionario di oggetti con tipizzazione debole.</span><span class="sxs-lookup"><span data-stu-id="30d87-223">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="30d87-224">Il `ViewBag` proprietà è un wrapper `ViewData` che fornisce le proprietà dinamiche per sottostante `ViewData` insieme.</span><span class="sxs-lookup"><span data-stu-id="30d87-224">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="30d87-225">`ViewData`e `ViewBag` vengono risolti in modo dinamico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="30d87-225">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="30d87-226">Poiché non offrono controllo dei tipi in fase di compilazione, entrambi sono in genere più errori rispetto all'utilizzo di un elemento viewmodel.</span><span class="sxs-lookup"><span data-stu-id="30d87-226">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="30d87-227">Per questo motivo, alcuni sviluppatori preferiscono utilizzare minima o mai `ViewData` e `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="30d87-227">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="30d87-228">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="30d87-228">**ViewData**</span></span>

<span data-ttu-id="30d87-229">`ViewData`è un [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) si accede tramite `string` chiavi.</span><span class="sxs-lookup"><span data-stu-id="30d87-229">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="30d87-230">Dati di tipo stringa possono essere archiviati e usati direttamente, senza la necessità di un cast, ma è necessario eseguire il cast di altri `ViewData` valori a specifici tipi di oggetto quando li estraggono.</span><span class="sxs-lookup"><span data-stu-id="30d87-230">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="30d87-231">È possibile utilizzare `ViewData` per passare i dati dal controller alle visualizzazioni e all'interno di visualizzazioni, tra cui [visualizzazioni parziali](xref:mvc/views/partial) e [layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="30d87-231">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="30d87-232">Di seguito è riportato un esempio che imposta valori per un messaggio di benvenuto e un indirizzo utilizzando `ViewData` un'azione:</span><span class="sxs-lookup"><span data-stu-id="30d87-232">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

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

<span data-ttu-id="30d87-233">Utilizzare i dati in una vista:</span><span class="sxs-lookup"><span data-stu-id="30d87-233">Work with the data in a view:</span></span>

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

<span data-ttu-id="30d87-234">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="30d87-234">**ViewBag**</span></span>

<span data-ttu-id="30d87-235">Nota: `ViewBag` non è disponibile nelle pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="30d87-235">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="30d87-236">`ViewBag`è un [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) oggetto che fornisce l'accesso dinamico per gli oggetti archiviati in `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="30d87-236">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="30d87-237">`ViewBag`può essere più semplice da utilizzare in quanto non richiede il cast.</span><span class="sxs-lookup"><span data-stu-id="30d87-237">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="30d87-238">Nell'esempio seguente viene illustrato come utilizzare `ViewBag` con lo stesso risultato utilizzando `ViewData` sopra:</span><span class="sxs-lookup"><span data-stu-id="30d87-238">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

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

<span data-ttu-id="30d87-239">**Utilizzano ViewData e ViewBag simultaneamente**</span><span class="sxs-lookup"><span data-stu-id="30d87-239">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="30d87-240">Nota: `ViewBag` non è disponibile nelle pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="30d87-240">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="30d87-241">Poiché `ViewData` e `ViewBag` fare riferimento alla stessa sottostante `ViewData` insieme, è possibile utilizzare sia `ViewData` e `ViewBag` e combinare e associare tra loro durante la lettura e scrittura di valori.</span><span class="sxs-lookup"><span data-stu-id="30d87-241">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="30d87-242">Impostare il titolo tramite `ViewBag` e la descrizione usando `ViewData` nella parte superiore di un *About.cshtml* Vista:</span><span class="sxs-lookup"><span data-stu-id="30d87-242">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="30d87-243">Leggere le proprietà ma inverte l'utilizzo di `ViewData` e `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="30d87-243">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="30d87-244">Nel *layout. cshtml* file, ottenere il titolo tramite `ViewData` e ottenere la descrizione utilizzando `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="30d87-244">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="30d87-245">Tenere presente che le stringhe non richiedono un cast per `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="30d87-245">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="30d87-246">È possibile utilizzare `@ViewData["Title"]` senza eseguire il cast.</span><span class="sxs-lookup"><span data-stu-id="30d87-246">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="30d87-247">Utilizzo di entrambe `ViewData` e `ViewBag` in lavori stesso, tempo fa, combinazione e la corrispondenza di lettura e scrittura delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="30d87-247">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="30d87-248">Il markup seguente viene eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="30d87-248">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="30d87-249">**Riepilogo delle differenze tra ViewData e ViewBag**</span><span class="sxs-lookup"><span data-stu-id="30d87-249">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="30d87-250">`ViewBag`non è disponibile nelle pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="30d87-250">`ViewBag` is not available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="30d87-251">Deriva da [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), pertanto dispone di proprietà di dizionario che può essere utile, ad esempio `ContainsKey`, `Add`, `Remove`, e `Clear`.</span><span class="sxs-lookup"><span data-stu-id="30d87-251">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="30d87-252">Le chiavi nel dizionario sono stringhe, pertanto gli spazi vuoti sono consentito.</span><span class="sxs-lookup"><span data-stu-id="30d87-252">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="30d87-253">Esempio: `ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="30d87-253">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="30d87-254">Qualsiasi tipo diverso da un `string` deve essere impostato nella visualizzazione per utilizzare `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="30d87-254">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="30d87-255">Deriva da [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), quindi ne consente la creazione delle proprietà dinamiche utilizzando la notazione del punto (`@ViewBag.SomeKey = <value or object>`), ed non è necessario alcun cast.</span><span class="sxs-lookup"><span data-stu-id="30d87-255">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="30d87-256">La sintassi di `ViewBag` rende più rapido aggiungere al controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="30d87-256">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="30d87-257">Più semplice verificare la presenza di valori null.</span><span class="sxs-lookup"><span data-stu-id="30d87-257">Simpler to check for null values.</span></span> <span data-ttu-id="30d87-258">Esempio: `@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="30d87-258">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="30d87-259">**Quando utilizzare ViewData o ViewBag**</span><span class="sxs-lookup"><span data-stu-id="30d87-259">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="30d87-260">Entrambi `ViewData` e `ViewBag` sono ugualmente validi approcci per il passaggio di piccole quantità di dati tra i controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="30d87-260">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="30d87-261">La scelta di quello da utilizzare è basata sulla preferenza.</span><span class="sxs-lookup"><span data-stu-id="30d87-261">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="30d87-262">È possibile combinare e associare `ViewData` e `ViewBag` oggetti, tuttavia, il codice è più facile da leggere e gestire con un approccio utilizzato in modo coerente.</span><span class="sxs-lookup"><span data-stu-id="30d87-262">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="30d87-263">Entrambi gli approcci sono risolti in modo dinamico in fase di esecuzione e quindi soggetto a causa di errori di runtime.</span><span class="sxs-lookup"><span data-stu-id="30d87-263">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="30d87-264">Alcuni team di sviluppo per evitare il problema.</span><span class="sxs-lookup"><span data-stu-id="30d87-264">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="30d87-265">Visualizzazioni dinamiche</span><span class="sxs-lookup"><span data-stu-id="30d87-265">Dynamic views</span></span>

<span data-ttu-id="30d87-266">Tipo di viste che non dichiarano un modello utilizzando `@model` ma che dispongono di un'istanza del modello passata a essi (ad esempio, `return View(Address);`) possono fare riferimento le proprietà dell'istanza in modo dinamico:</span><span class="sxs-lookup"><span data-stu-id="30d87-266">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="30d87-267">Questa funzionalità offre flessibilità ma non offre protezione compilazione o IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="30d87-267">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="30d87-268">Se la proprietà non esiste, la generazione di pagine Web non riesce in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="30d87-268">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="30d87-269">Altre funzionalità di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="30d87-269">More view features</span></span>

<span data-ttu-id="30d87-270">[Gli helper di tag](xref:mvc/views/tag-helpers/intro) rendono più facile aggiungere un comportamento lato server per i tag HTML esistenti.</span><span class="sxs-lookup"><span data-stu-id="30d87-270">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="30d87-271">Gli helper di Tag consente di evitare la necessità di scrivere codice personalizzato o helper all'interno delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="30d87-271">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="30d87-272">Gli helper di tag vengono applicati come attributi agli elementi HTML e vengono ignorati per gli editor che non è possibile elaborarli.</span><span class="sxs-lookup"><span data-stu-id="30d87-272">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="30d87-273">Ciò consente di modificare ed eseguire il rendering di markup di visualizzazione in un'ampia gamma di strumenti.</span><span class="sxs-lookup"><span data-stu-id="30d87-273">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="30d87-274">Generazione di codice HTML personalizzato può essere ottenuta con molti helper HTML incorporato.</span><span class="sxs-lookup"><span data-stu-id="30d87-274">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="30d87-275">Una logica più complessa dell'interfaccia utente può essere gestita da [Visualizza i componenti](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="30d87-275">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="30d87-276">Visualizza i componenti forniscono la stessa SoC tale controller e visualizzazioni offrono.</span><span class="sxs-lookup"><span data-stu-id="30d87-276">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="30d87-277">È possibile eliminare la necessità di azioni e visualizzazioni che gestiscono i dati utilizzati da elementi dell'interfaccia utente comune.</span><span class="sxs-lookup"><span data-stu-id="30d87-277">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="30d87-278">Come molti altri aspetti di ASP.NET Core, viste supportano [inserimento di dipendenze](xref:fundamentals/dependency-injection), consentendo di servizi da [inserite nelle viste](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="30d87-278">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
