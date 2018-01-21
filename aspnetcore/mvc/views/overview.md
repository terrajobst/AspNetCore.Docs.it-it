---
title: Visualizzazioni in ASP.NET MVC di base
author: ardalis
description: Informazioni su come viste di gestiscono la presentazione dei dati dell'app e l'interazione dell'utente in ASP.NET MVC di base.
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: c0a1f475941f3389e9aa1f5bb7819bef491b2cae
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="views-in-aspnet-core-mvc"></a><span data-ttu-id="95441-103">Visualizzazioni in ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="95441-103">Views in ASP.NET Core MVC</span></span>

<span data-ttu-id="95441-104">Da [Steve Smith](https://ardalis.com/) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="95441-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="95441-105">Questo documento illustra le viste utilizzate nelle applicazioni ASP.NET MVC di base.</span><span class="sxs-lookup"><span data-stu-id="95441-105">This document explains views used in ASP.NET Core MVC applications.</span></span> <span data-ttu-id="95441-106">Per informazioni sulle pagine Razor, vedere [Introduzione alle pagine Razor](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="95441-106">For information on Razor Pages, see [Introduction to Razor Pages](xref:mvc/razor-pages/index).</span></span>

<span data-ttu-id="95441-107">Nel **M**enti -**V**ISTA -**C**modello ontroller (MVC), il *vista* gestisce l'interazione utente e presentazione dei dati dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="95441-107">In the **M**odel-**V**iew-**C**ontroller (MVC) pattern, the *view* handles the app's data presentation and user interaction.</span></span> <span data-ttu-id="95441-108">Una vista è un modello HTML con incorporato [markup Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="95441-108">A view is an HTML template with embedded [Razor markup](xref:mvc/views/razor).</span></span> <span data-ttu-id="95441-109">Markup Razor è codice che interagisce con il markup HTML per produrre una pagina Web che viene inviata al client.</span><span class="sxs-lookup"><span data-stu-id="95441-109">Razor markup is code that interacts with HTML markup to produce a webpage that's sent to the client.</span></span>

<span data-ttu-id="95441-110">In ASP.NET MVC di base, le visualizzazioni sono *. cshtml* file che utilizzano il [linguaggio di programmazione c#](/dotnet/csharp/) nel codice Razor.</span><span class="sxs-lookup"><span data-stu-id="95441-110">In ASP.NET Core MVC, views are *.cshtml* files that use the [C# programming language](/dotnet/csharp/) in Razor markup.</span></span> <span data-ttu-id="95441-111">In genere, visualizzare i file sono raggruppati in cartelle denominate per ognuna dell'app [controller](xref:mvc/controllers/actions).</span><span class="sxs-lookup"><span data-stu-id="95441-111">Usually, view files are grouped into folders named for each of the app's [controllers](xref:mvc/controllers/actions).</span></span> <span data-ttu-id="95441-112">Le cartelle vengono archiviate in un *viste* cartella principale dell'app:</span><span class="sxs-lookup"><span data-stu-id="95441-112">The folders are stored in a *Views* folder at the root of the app:</span></span>

![Esplora soluzioni di Visual Studio nella cartella viste è aperta con la cartella Home aprire per visualizzare i file cshtml About.cshtml e Contact.cshtml](overview/_static/views_solution_explorer.png)

<span data-ttu-id="95441-114">Il *Home* controller è rappresentato da un *Home* cartella all'interno di *viste* cartella.</span><span class="sxs-lookup"><span data-stu-id="95441-114">The *Home* controller is represented by a *Home* folder inside the *Views* folder.</span></span> <span data-ttu-id="95441-115">Il *Home* cartella contiene le viste per il *su*, *contatto*, e *indice* pagine Web (Home Page).</span><span class="sxs-lookup"><span data-stu-id="95441-115">The *Home* folder contains the views for the *About*, *Contact*, and *Index* (homepage) webpages.</span></span> <span data-ttu-id="95441-116">Quando un utente richiede uno di questi tre pagine Web, le azioni del controller nel *Home* controller determinare quale delle tre visualizzazioni viene utilizzato per compilare e restituire una pagina Web all'utente.</span><span class="sxs-lookup"><span data-stu-id="95441-116">When a user requests one of these three webpages, controller actions in the *Home* controller determine which of the three views is used to build and return a webpage to the user.</span></span>

<span data-ttu-id="95441-117">Utilizzare [layout](xref:mvc/views/layout) per fornire le sezioni di pagina Web coerente e ridurre ripetizioni nel codice.</span><span class="sxs-lookup"><span data-stu-id="95441-117">Use [layouts](xref:mvc/views/layout) to provide consistent webpage sections and reduce code repetition.</span></span> <span data-ttu-id="95441-118">Layout contengono spesso l'intestazione, gli elementi di navigazione e i menu e il piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="95441-118">Layouts often contain the header, navigation and menu elements, and the footer.</span></span> <span data-ttu-id="95441-119">L'intestazione e piè di pagina contengono in genere codice boilerplate per molti elementi di metadati e i collegamenti alle risorse di script e stile.</span><span class="sxs-lookup"><span data-stu-id="95441-119">The header and footer usually contain boilerplate markup for many metadata elements and links to script and style assets.</span></span> <span data-ttu-id="95441-120">Layout consentono di evitare questo markup boilerplate nelle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="95441-120">Layouts help you avoid this boilerplate markup in your views.</span></span>

<span data-ttu-id="95441-121">[Le visualizzazioni parziali](xref:mvc/views/partial) ridurre la duplicazione del codice mediante la gestione di parti riutilizzabili di viste.</span><span class="sxs-lookup"><span data-stu-id="95441-121">[Partial views](xref:mvc/views/partial) reduce code duplication by managing reusable parts of views.</span></span> <span data-ttu-id="95441-122">Ad esempio, una visualizzazione parziale è utile per una biografia autore in un sito Web del blog che viene visualizzato in diverse visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="95441-122">For example, a partial view is useful for an author biography on a blog website that appears in several views.</span></span> <span data-ttu-id="95441-123">Una biografia autore è contenuto visualizzazione comune e non richiede codice da eseguire per produrre il contenuto per la pagina Web.</span><span class="sxs-lookup"><span data-stu-id="95441-123">An author biography is ordinary view content and doesn't require code to execute in order to produce the content for the webpage.</span></span> <span data-ttu-id="95441-124">Contenuto biografia autore è disponibile per la visualizzazione per l'associazione di modelli da solo, pertanto l'utilizzo di una visualizzazione parziale per questo tipo di contenuto è ideale.</span><span class="sxs-lookup"><span data-stu-id="95441-124">Author biography content is available to the view by model binding alone, so using a partial view for this type of content is ideal.</span></span>

<span data-ttu-id="95441-125">[Visualizzare i componenti](xref:mvc/views/view-components) sono simili a parziale viste che consentono di ridurre il codice ripetitivo, ma appropriati per visualizzare il contenuto che richiede il codice per l'esecuzione nel server per eseguire il rendering della pagina Web.</span><span class="sxs-lookup"><span data-stu-id="95441-125">[View components](xref:mvc/views/view-components) are similar to partial views in that they allow you to reduce repetitive code, but they're appropriate for view content that requires code to run on the server in order to render the webpage.</span></span> <span data-ttu-id="95441-126">Visualizza i componenti sono utili quando il contenuto sottoposto a rendering richiede interazione con il database, ad esempio per un sito Web carrello degli acquisti.</span><span class="sxs-lookup"><span data-stu-id="95441-126">View components are useful when the rendered content requires database interaction, such as for a website shopping cart.</span></span> <span data-ttu-id="95441-127">Visualizza i componenti non sono limitati per l'associazione del modello per produrre l'output di pagina Web.</span><span class="sxs-lookup"><span data-stu-id="95441-127">View components aren't limited to model binding in order to produce webpage output.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="95441-128">Vantaggi dell'utilizzo di viste</span><span class="sxs-lookup"><span data-stu-id="95441-128">Benefits of using views</span></span>

<span data-ttu-id="95441-129">Le viste consentono di definire un [ **S**eparation **o**f **C**oncerns (SoC) progettazione](http://deviq.com/separation-of-concerns/) all'interno di un'applicazione MVC separando il markup dell'interfaccia utente altre parti dell'app.</span><span class="sxs-lookup"><span data-stu-id="95441-129">Views help to establish a [**S**eparation **o**f **C**oncerns (SoC) design](http://deviq.com/separation-of-concerns/) within an MVC app by separating the user interface markup from other parts of the app.</span></span> <span data-ttu-id="95441-130">Dopo la progettazione SoC rende l'app modulare, che offre diversi vantaggi:</span><span class="sxs-lookup"><span data-stu-id="95441-130">Following SoC design makes your app modular, which provides several benefits:</span></span>

* <span data-ttu-id="95441-131">L'app è più facile da gestire perché è meglio organizzato.</span><span class="sxs-lookup"><span data-stu-id="95441-131">The app is easier to maintain because it's better organized.</span></span> <span data-ttu-id="95441-132">Le visualizzazioni vengono in genere raggruppate per funzionalità delle app.</span><span class="sxs-lookup"><span data-stu-id="95441-132">Views are generally grouped by app feature.</span></span> <span data-ttu-id="95441-133">Questo rende più semplice trovare viste correlate, quando si utilizza una funzionalità.</span><span class="sxs-lookup"><span data-stu-id="95441-133">This makes it easier to find related views when working on a feature.</span></span>
* <span data-ttu-id="95441-134">Le parti dell'app sono loosely coupled.</span><span class="sxs-lookup"><span data-stu-id="95441-134">The parts of the app are loosely coupled.</span></span> <span data-ttu-id="95441-135">È possibile compilare e aggiornare le visualizzazioni dell'app separatamente dai componenti di accesso dati e logica di business.</span><span class="sxs-lookup"><span data-stu-id="95441-135">You can build and update the app's views separately from the business logic and data access components.</span></span> <span data-ttu-id="95441-136">È possibile modificare le visualizzazioni dell'app senza dover necessariamente aggiornare altre parti dell'app.</span><span class="sxs-lookup"><span data-stu-id="95441-136">You can modify the views of the app without necessarily having to update other parts of the app.</span></span>
* <span data-ttu-id="95441-137">Risulta più semplice testare le parti dell'interfaccia utente dell'app, perché le visualizzazioni sono unità distinte.</span><span class="sxs-lookup"><span data-stu-id="95441-137">It's easier to test the user interface parts of the app because the views are separate units.</span></span>
* <span data-ttu-id="95441-138">A causa di una migliore organizzazione, è meno probabile che sarà accidentalmente ripetizione sezioni dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="95441-138">Due to better organization, it's less likely that you'll accidently repeat sections of the user interface.</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="95441-139">Creazione di una vista</span><span class="sxs-lookup"><span data-stu-id="95441-139">Creating a view</span></span>

<span data-ttu-id="95441-140">Le viste che sono specifiche di un controller vengono create nel *viste / [ControllerName]* cartella.</span><span class="sxs-lookup"><span data-stu-id="95441-140">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="95441-141">Le viste che vengono condivisi tra i controller vengono inserite nel *Views/Shared* cartella.</span><span class="sxs-lookup"><span data-stu-id="95441-141">Views that are shared among controllers are placed in the *Views/Shared* folder.</span></span> <span data-ttu-id="95441-142">Per creare una vista, aggiungere un nuovo file e assegnargli lo stesso nome dell'azione del controller associata con il *. cshtml* estensione di file.</span><span class="sxs-lookup"><span data-stu-id="95441-142">To create a view, add a new file and give it the same name as its associated controller action with the *.cshtml* file extension.</span></span> <span data-ttu-id="95441-143">Per creare una visualizzazione corrispondente con il *su* azione nel *Home* controller, creare un *About.cshtml* file nel *Views/Home*cartella:</span><span class="sxs-lookup"><span data-stu-id="95441-143">To create a view that corresponds with the *About* action in the *Home* controller, create an *About.cshtml* file in the *Views/Home* folder:</span></span>

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

<span data-ttu-id="95441-144">*Razor* markup inizia con il `@` simbolo.</span><span class="sxs-lookup"><span data-stu-id="95441-144">*Razor* markup starts with the `@` symbol.</span></span> <span data-ttu-id="95441-145">Eseguire istruzioni c# inserendo in c# il codice all'interno [blocchi di codice Razor](xref:mvc/views/razor#razor-code-blocks) disattivare le parentesi graffe (`{ ... }`).</span><span class="sxs-lookup"><span data-stu-id="95441-145">Run C# statements by placing C# code within [Razor code blocks](xref:mvc/views/razor#razor-code-blocks) set off by curly braces (`{ ... }`).</span></span> <span data-ttu-id="95441-146">Ad esempio, vedere l'assegnazione di "About" per `ViewData["Title"]` illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="95441-146">For example, see the assignment of "About" to `ViewData["Title"]` shown above.</span></span> <span data-ttu-id="95441-147">È possibile visualizzare i valori in HTML facendo riferimento il valore con il `@` simbolo.</span><span class="sxs-lookup"><span data-stu-id="95441-147">You can display values within HTML by simply referencing the value with the `@` symbol.</span></span> <span data-ttu-id="95441-148">Visualizzare il contenuto del `<h2>` e `<h3>` elementi sopra riportato.</span><span class="sxs-lookup"><span data-stu-id="95441-148">See the contents of the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="95441-149">Il contenuto della visualizzazione illustrato in precedenza è solo una parte dell'intera pagina Web che viene eseguito il rendering all'utente.</span><span class="sxs-lookup"><span data-stu-id="95441-149">The view content shown above is only part of the entire webpage that's rendered to the user.</span></span> <span data-ttu-id="95441-150">Il resto del layout della pagina e altri aspetti comuni della visualizzazione sono specificati in altri file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="95441-150">The rest of the page's layout and other common aspects of the view are specified in other view files.</span></span> <span data-ttu-id="95441-151">Per ulteriori informazioni, vedere il [argomento Layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="95441-151">To learn more, see the [Layout topic](xref:mvc/views/layout).</span></span>

## <a name="how-controllers-specify-views"></a><span data-ttu-id="95441-152">Il modo in cui i controller specifica viste</span><span class="sxs-lookup"><span data-stu-id="95441-152">How controllers specify views</span></span>

<span data-ttu-id="95441-153">Le visualizzazioni vengono in genere restituite dalle azioni come un [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), che è un tipo di [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span><span class="sxs-lookup"><span data-stu-id="95441-153">Views are typically returned from actions as a [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), which is a type of [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult).</span></span> <span data-ttu-id="95441-154">Metodo di azione, è possibile creare e restituire un `ViewResult` direttamente, ma che non è in genere eseguito.</span><span class="sxs-lookup"><span data-stu-id="95441-154">Your action method can create and return a `ViewResult` directly, but that isn't commonly done.</span></span> <span data-ttu-id="95441-155">Poiché la maggior parte dei controller eredita [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), è sufficiente utilizzare il `View` metodo helper per restituire il `ViewResult`:</span><span class="sxs-lookup"><span data-stu-id="95441-155">Since most controllers inherit from [Controller](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), you simply use the `View` helper method to return the `ViewResult`:</span></span>

<span data-ttu-id="95441-156">*HomeController.cs*</span><span class="sxs-lookup"><span data-stu-id="95441-156">*HomeController.cs*</span></span>

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

<span data-ttu-id="95441-157">Quando questa azione viene restituito, il *About.cshtml* visualizzazione indicata nell'ultima sezione viene visualizzato come la seguente pagina Web:</span><span class="sxs-lookup"><span data-stu-id="95441-157">When this action returns, the *About.cshtml* view shown in the last section is rendered as the following webpage:</span></span>

![Sulla pagina sottoposta a rendering nel browser Edge](overview/_static/about-page.png)

<span data-ttu-id="95441-159">Il `View` metodo helper dispone di diversi overload.</span><span class="sxs-lookup"><span data-stu-id="95441-159">The `View` helper method has several overloads.</span></span> <span data-ttu-id="95441-160">È possibile specificare:</span><span class="sxs-lookup"><span data-stu-id="95441-160">You can optionally specify:</span></span>

* <span data-ttu-id="95441-161">Una vista esplicita da restituire:</span><span class="sxs-lookup"><span data-stu-id="95441-161">An explicit view to return:</span></span>

  ```csharp
  return View("Orders");
  ```
* <span data-ttu-id="95441-162">A [modello](xref:mvc/models/model-binding) da passare alla vista:</span><span class="sxs-lookup"><span data-stu-id="95441-162">A [model](xref:mvc/models/model-binding) to pass to the the view:</span></span>

  ```csharp
  return View(Orders);
  ```
* <span data-ttu-id="95441-163">Una vista e un modello:</span><span class="sxs-lookup"><span data-stu-id="95441-163">Both a view and a model:</span></span>

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a><span data-ttu-id="95441-164">Individuazione di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="95441-164">View discovery</span></span>

<span data-ttu-id="95441-165">Quando un'azione restituisce una visualizzazione, un processo denominato *individuazione visualizzazione* avviene.</span><span class="sxs-lookup"><span data-stu-id="95441-165">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="95441-166">Questo processo determina quale file di visualizzazione in base al nome di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="95441-166">This process determines which view file is used based on the view name.</span></span> 

<span data-ttu-id="95441-167">Il comportamento predefinito del `View` metodo (`return View();`) deve restituire una visualizzazione con lo stesso nome del metodo di azione da cui viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="95441-167">The default behavior of the `View` method (`return View();`) is to return a view with the same name as the action method from which it's called.</span></span> <span data-ttu-id="95441-168">Ad esempio, il *su* `ActionResult` nome del metodo del controller a cui viene utilizzato per cercare un file di visualizzazione denominato *About.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="95441-168">For example, the *About* `ActionResult` method name of the controller is used to search for a view file named *About.cshtml*.</span></span> <span data-ttu-id="95441-169">In primo luogo, il runtime cerca *viste / [ControllerName]* cartella per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="95441-169">First, the runtime looks in the *Views/[ControllerName]* folder for the view.</span></span> <span data-ttu-id="95441-170">Se non esiste una visualizzazione corrispondente non viene trovato, viene cercato il *Shared* cartella per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="95441-170">If it doesn't find a matching view there, it searches the *Shared* folder for the view.</span></span>

<span data-ttu-id="95441-171">Non è importante se si restituisce in modo implicito il `ViewResult` con `return View();` o passare in modo esplicito il nome della visualizzazione per il `View` metodo con `return View("<ViewName>");`.</span><span class="sxs-lookup"><span data-stu-id="95441-171">It doesn't matter if you implicitly return the `ViewResult` with `return View();` or explicitly pass the view name to the `View` method with `return View("<ViewName>");`.</span></span> <span data-ttu-id="95441-172">In entrambi i casi, visualizzare l'individuazione Cerca un file di visualizzazione corrispondente nell'ordine indicato:</span><span class="sxs-lookup"><span data-stu-id="95441-172">In both cases, view discovery searches for a matching view file in this order:</span></span>

   1. <span data-ttu-id="95441-173">*Views/\[ControllerName]\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="95441-173">*Views/\[ControllerName]\[ViewName].cshtml*</span></span>
   1. <span data-ttu-id="95441-174">*Views/Shared/\[ViewName].cshtml*</span><span class="sxs-lookup"><span data-stu-id="95441-174">*Views/Shared/\[ViewName].cshtml*</span></span>

<span data-ttu-id="95441-175">Invece di un nome di visualizzazione, è possibile specificare un percorso di file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="95441-175">A view file path can be provided instead of a view name.</span></span> <span data-ttu-id="95441-176">Se si utilizza un percorso assoluto a partire dalla radice dell'applicazione (facoltativamente inizia con "/" o "~ /"), il *. cshtml* estensione deve essere specificata:</span><span class="sxs-lookup"><span data-stu-id="95441-176">If using an absolute path starting at the app root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified:</span></span>

```csharp
return View("Views/Home/About.cshtml");
```

<span data-ttu-id="95441-177">È inoltre possibile utilizzare un percorso relativo per specificare le viste in directory diverse senza la *. cshtml* estensione.</span><span class="sxs-lookup"><span data-stu-id="95441-177">You can also use a relative path to specify views in different directories without the *.cshtml* extension.</span></span> <span data-ttu-id="95441-178">All'interno di `HomeController`, è possibile restituire il *indice* visualizzazione del *Gestisci* viste con un percorso relativo:</span><span class="sxs-lookup"><span data-stu-id="95441-178">Inside the `HomeController`, you can return the *Index* view of your *Manage* views with a relative path:</span></span>

```csharp
return View("../Manage/Index");
```

<span data-ttu-id="95441-179">Analogamente, è possibile indicare la directory di specifici controller corrente con il ". /" prefisso:</span><span class="sxs-lookup"><span data-stu-id="95441-179">Similarly, you can indicate the current controller-specific directory with the "./" prefix:</span></span>

```csharp
return View("./About");
```

<span data-ttu-id="95441-180">[Le visualizzazioni parziali](xref:mvc/views/partial) e [visualizzare componenti](xref:mvc/views/view-components) utilizzare meccanismi di individuazione simile (ma non identica).</span><span class="sxs-lookup"><span data-stu-id="95441-180">[Partial views](xref:mvc/views/partial) and [view components](xref:mvc/views/view-components) use similar (but not identical) discovery mechanisms.</span></span>

<span data-ttu-id="95441-181">È possibile personalizzare la convenzione predefinita per la modalità viste si trovano all'interno dell'app usando un oggetto personalizzato [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span><span class="sxs-lookup"><span data-stu-id="95441-181">You can customize the default convention for how views are located within the app by using a custom [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).</span></span>

<span data-ttu-id="95441-182">Individuazione di visualizzazione si basa sull'individuazione di visualizzare i file in base al nome di file.</span><span class="sxs-lookup"><span data-stu-id="95441-182">View discovery relies on finding view files by file name.</span></span> <span data-ttu-id="95441-183">Se il file system sottostante è tra maiuscole e minuscole, visualizzare i nomi sono probabilmente maiuscole/minuscole.</span><span class="sxs-lookup"><span data-stu-id="95441-183">If the underlying file system is case sensitive, view names are probably case sensitive.</span></span> <span data-ttu-id="95441-184">Per garantire la compatibilità tra i sistemi operativi, maiuscole/minuscole tra controller e i nomi di azione e le cartelle di visualizzazione associata e i nomi di file.</span><span class="sxs-lookup"><span data-stu-id="95441-184">For compatibility across operating systems, match case between controller and action names and associated view folders and file names.</span></span> <span data-ttu-id="95441-185">Se si verifica un errore che non è possibile trovare un file di visualizzazione mentre si lavora con un sistema di file tra maiuscole e minuscole, verificare che le maiuscole e minuscole corrispondano tra il file di visualizzazione richiesta e il nome del file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="95441-185">If you encounter an error that a view file can't be found while working with a case-sensitive file system, confirm that the casing matches between the requested view file and the actual view file name.</span></span>

<span data-ttu-id="95441-186">Seguire la procedura consigliata di organizzare la struttura dei file per le visualizzazioni in modo da riflettere le relazioni tra i controller, azioni e visualizzazioni per maggiore chiarezza e la manutenibilità.</span><span class="sxs-lookup"><span data-stu-id="95441-186">Follow the best practice of organizing the file structure for your views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="95441-187">Passaggio di dati a viste</span><span class="sxs-lookup"><span data-stu-id="95441-187">Passing data to views</span></span>

<span data-ttu-id="95441-188">È possibile passare dati alle viste utilizzando diversi approcci.</span><span class="sxs-lookup"><span data-stu-id="95441-188">You can pass data to views using several approaches.</span></span> <span data-ttu-id="95441-189">L'approccio più efficace consiste nello specificare un [modello](xref:mvc/models/model-binding) tipo nella visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="95441-189">The most robust approach is to specify a [model](xref:mvc/models/model-binding) type in the view.</span></span> <span data-ttu-id="95441-190">Questo modello è noto come un *viewmodel*.</span><span class="sxs-lookup"><span data-stu-id="95441-190">This model is commonly referred to as a *viewmodel*.</span></span> <span data-ttu-id="95441-191">Passare un'istanza del tipo viewmodel alla visualizzazione dall'azione.</span><span class="sxs-lookup"><span data-stu-id="95441-191">You pass an instance of the viewmodel type to the view from the action.</span></span>

<span data-ttu-id="95441-192">Utilizzando un viewmodel di passare dati a una vista consente di sfruttare *sicuro* controllo dei tipi.</span><span class="sxs-lookup"><span data-stu-id="95441-192">Using a viewmodel to pass data to a view allows the view to take advantage of *strong* type checking.</span></span> <span data-ttu-id="95441-193">*Tipizzazione forte* (o *fortemente tipizzato*) significa che ogni variabile e costante dispone di un tipo definito in modo esplicito (ad esempio, `string`, `int`, o `DateTime`).</span><span class="sxs-lookup"><span data-stu-id="95441-193">*Strong typing* (or *strongly-typed*) means that every variable and constant has an explicitly defined type (for example, `string`, `int`, or `DateTime`).</span></span> <span data-ttu-id="95441-194">In fase di compilazione, viene verificata la validità dei tipi utilizzati in una vista.</span><span class="sxs-lookup"><span data-stu-id="95441-194">The validity of types used in a view is checked at compile time.</span></span>

<span data-ttu-id="95441-195">[Visual Studio](https://www.visualstudio.com/vs/) e [codice di Visual Studio](https://code.visualstudio.com/) sono elencati i membri di classe fortemente tipizzata utilizzando la funzione [IntelliSense](/visualstudio/ide/using-intellisense).</span><span class="sxs-lookup"><span data-stu-id="95441-195">[Visual Studio](https://www.visualstudio.com/vs/) and [Visual Studio Code](https://code.visualstudio.com/) list strongly-typed class members using a feature called [IntelliSense](/visualstudio/ide/using-intellisense).</span></span> <span data-ttu-id="95441-196">Quando si desidera visualizzare le proprietà di un elemento viewmodel, digitare il nome della variabile per l'elemento viewmodel seguito da un punto (`.`).</span><span class="sxs-lookup"><span data-stu-id="95441-196">When you want to see the properties of a viewmodel, type the variable name for the viewmodel followed by a period (`.`).</span></span> <span data-ttu-id="95441-197">Ciò consente di scrivere codice più rapidamente con meno errori.</span><span class="sxs-lookup"><span data-stu-id="95441-197">This helps you write code faster with fewer errors.</span></span>

<span data-ttu-id="95441-198">Specificare un modello utilizzando il `@model` direttiva.</span><span class="sxs-lookup"><span data-stu-id="95441-198">Specify a model using the `@model` directive.</span></span> <span data-ttu-id="95441-199">Utilizzare il modello con `@Model`:</span><span class="sxs-lookup"><span data-stu-id="95441-199">Use the model with `@Model`:</span></span>

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="95441-200">Per fornire al modello per la visualizzazione, il controller passa come parametro:</span><span class="sxs-lookup"><span data-stu-id="95441-200">To provide the model to the view, the controller passes it as a parameter:</span></span>

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

<span data-ttu-id="95441-201">Non sono previste restrizioni per i tipi di modello che è possibile fornire una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="95441-201">There are no restrictions on the model types that you can provide to a view.</span></span> <span data-ttu-id="95441-202">Si consiglia di utilizzare **P**Incolla **O**ld **C**LR **O**ViewModel oggetto (POCO) con pochi o nessun comportamento (metodi) definita.</span><span class="sxs-lookup"><span data-stu-id="95441-202">We recommend using **P**lain **O**ld **C**LR **O**bject (POCO) viewmodels with little or no behavior (methods) defined.</span></span> <span data-ttu-id="95441-203">In genere, le classi viewmodel sono archiviate nel *modelli* cartella o un oggetto separato *ViewModel* cartella nella radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="95441-203">Usually, viewmodel classes are either stored in the *Models* folder or a separate *ViewModels* folder at the root of the app.</span></span> <span data-ttu-id="95441-204">Il *indirizzo* viewmodel utilizzato nell'esempio precedente è un elemento viewmodel POCO archiviati in un file denominato *Address.cs*:</span><span class="sxs-lookup"><span data-stu-id="95441-204">The *Address* viewmodel used in the example above is a POCO viewmodel stored in a file named *Address.cs*:</span></span>

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
> <span data-ttu-id="95441-205">Non impedisce di utilizzare le stesse classi per i tipi di viewmodel e i tipi di modello di business.</span><span class="sxs-lookup"><span data-stu-id="95441-205">Nothing prevents you from using the same classes for both your viewmodel types and your business model types.</span></span> <span data-ttu-id="95441-206">L'utilizzo di modelli separati consente, tuttavia, le visualizzazioni variare in modo indipendente dai dati di logica di business e parti di accesso dell'app.</span><span class="sxs-lookup"><span data-stu-id="95441-206">However, using separate models allows your views to vary independently from the business logic and data access parts of your app.</span></span> <span data-ttu-id="95441-207">La separazione dei modelli e ViewModel offre vantaggi di sicurezza se i modelli usano [associazione del modello](xref:mvc/models/model-binding) e [convalida](xref:mvc/models/validation) per i dati inviati all'app dall'utente.</span><span class="sxs-lookup"><span data-stu-id="95441-207">Separation of models and viewmodels also offers security benefits when models use [model binding](xref:mvc/models/model-binding) and [validation](xref:mvc/models/validation) for data sent to the app by the user.</span></span>


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a><span data-ttu-id="95441-208">Dati con tipizzazione debole (ViewData e ViewBag)</span><span class="sxs-lookup"><span data-stu-id="95441-208">Weakly-typed data (ViewData and ViewBag)</span></span>

<span data-ttu-id="95441-209">Nota: `ViewBag` non è disponibile nelle pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="95441-209">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="95441-210">Oltre a visualizzazioni fortemente tipizzate, le visualizzazioni di avere a disposizione un *con tipizzazione debole* (chiamato anche *fortemente tipizzato*) insieme di dati.</span><span class="sxs-lookup"><span data-stu-id="95441-210">In addition to strongly-typed views, views have access to a *weakly-typed* (also called *loosely-typed*) collection of data.</span></span> <span data-ttu-id="95441-211">A differenza dei tipi sicuri, *tipi deboli* (o *perdere tipi*) significa che non dichiarare in modo esplicito il tipo di dati in uso.</span><span class="sxs-lookup"><span data-stu-id="95441-211">Unlike strong types, *weak types* (or *loose types*) means that you don't explicitly declare the type of data you're using.</span></span> <span data-ttu-id="95441-212">È possibile utilizzare la raccolta dei dati con tipizzazione debole per il passaggio di piccole quantità di dati da e verso i controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="95441-212">You can use the collection of weakly-typed data for passing small amounts of data in and out of controllers and views.</span></span>

| <span data-ttu-id="95441-213">Passaggio di dati tra un...</span><span class="sxs-lookup"><span data-stu-id="95441-213">Passing data between a ...</span></span>                        | <span data-ttu-id="95441-214">Esempio</span><span class="sxs-lookup"><span data-stu-id="95441-214">Example</span></span>                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="95441-215">Controller e una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="95441-215">Controller and a view</span></span>                             | <span data-ttu-id="95441-216">Popolamento di un elenco a discesa con i dati.</span><span class="sxs-lookup"><span data-stu-id="95441-216">Populating a dropdown list with data.</span></span>                                          |
| <span data-ttu-id="95441-217">Visualizzazione e un [visualizzazione layout](xref:mvc/views/layout)</span><span class="sxs-lookup"><span data-stu-id="95441-217">View and a [layout view](xref:mvc/views/layout)</span></span>   | <span data-ttu-id="95441-218">L'impostazione di  **\<titolo >** contenuto dell'elemento in visualizzazione layout di finestra da un file di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="95441-218">Setting the **\<title>** element content in the layout view from a view file.</span></span>  |
| <span data-ttu-id="95441-219">[Visualizzazione parziale](xref:mvc/views/partial) e una visualizzazione</span><span class="sxs-lookup"><span data-stu-id="95441-219">[Partial view](xref:mvc/views/partial) and a view</span></span> | <span data-ttu-id="95441-220">Un widget che visualizza i dati in base a una pagina Web che l'utente ha richiesto.</span><span class="sxs-lookup"><span data-stu-id="95441-220">A widget that displays data based on the webpage that the user requested.</span></span>      |

<span data-ttu-id="95441-221">È possibile fare riferimento a questa raccolta tramite il `ViewData` o `ViewBag` proprietà sul controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="95441-221">This collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="95441-222">Il `ViewData` proprietà è un dizionario di oggetti con tipizzazione debole.</span><span class="sxs-lookup"><span data-stu-id="95441-222">The `ViewData` property is a dictionary of weakly-typed objects.</span></span> <span data-ttu-id="95441-223">Il `ViewBag` proprietà è un wrapper `ViewData` che fornisce le proprietà dinamiche per sottostante `ViewData` insieme.</span><span class="sxs-lookup"><span data-stu-id="95441-223">The `ViewBag` property is a wrapper around `ViewData` that provides dynamic properties for the underlying `ViewData` collection.</span></span>

<span data-ttu-id="95441-224">`ViewData`e `ViewBag` vengono risolti in modo dinamico in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="95441-224">`ViewData` and `ViewBag` are dynamically resolved at runtime.</span></span> <span data-ttu-id="95441-225">Poiché non offrono controllo dei tipi in fase di compilazione, entrambi sono in genere più errori rispetto all'utilizzo di un elemento viewmodel.</span><span class="sxs-lookup"><span data-stu-id="95441-225">Since they don't offer compile-time type checking, both are generally more error-prone than using a viewmodel.</span></span> <span data-ttu-id="95441-226">Per questo motivo, alcuni sviluppatori preferiscono utilizzare minima o mai `ViewData` e `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="95441-226">For that reason, some developers prefer to minimally or never use `ViewData` and `ViewBag`.</span></span>


<a name="VD"></a>

<span data-ttu-id="95441-227">**ViewData**</span><span class="sxs-lookup"><span data-stu-id="95441-227">**ViewData**</span></span>

<span data-ttu-id="95441-228">`ViewData`è un [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) si accede tramite `string` chiavi.</span><span class="sxs-lookup"><span data-stu-id="95441-228">`ViewData` is a [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) object accessed through `string` keys.</span></span> <span data-ttu-id="95441-229">Dati di tipo stringa possono essere archiviati e usati direttamente, senza la necessità di un cast, ma è necessario eseguire il cast di altri `ViewData` valori a specifici tipi di oggetto quando li estraggono.</span><span class="sxs-lookup"><span data-stu-id="95441-229">String data can be stored and used directly without the need for a cast, but you must cast other `ViewData` object values to specific types when you extract them.</span></span> <span data-ttu-id="95441-230">È possibile utilizzare `ViewData` per passare i dati dal controller alle visualizzazioni e all'interno di visualizzazioni, tra cui [visualizzazioni parziali](xref:mvc/views/partial) e [layout](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="95441-230">You can use `ViewData` to pass data from controllers to views and within views, including [partial views](xref:mvc/views/partial) and [layouts](xref:mvc/views/layout).</span></span>

<span data-ttu-id="95441-231">Di seguito è riportato un esempio che imposta valori per un messaggio di benvenuto e un indirizzo utilizzando `ViewData` un'azione:</span><span class="sxs-lookup"><span data-stu-id="95441-231">The following is an example that sets values for a greeting and an address using `ViewData` in an action:</span></span>

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

<span data-ttu-id="95441-232">Utilizzare i dati in una vista:</span><span class="sxs-lookup"><span data-stu-id="95441-232">Work with the data in a view:</span></span>

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

<span data-ttu-id="95441-233">**ViewBag**</span><span class="sxs-lookup"><span data-stu-id="95441-233">**ViewBag**</span></span>

<span data-ttu-id="95441-234">Nota: `ViewBag` non è disponibile nelle pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="95441-234">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="95441-235">`ViewBag`è un [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) oggetto che fornisce l'accesso dinamico per gli oggetti archiviati in `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="95441-235">`ViewBag` is a [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) object that provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="95441-236">`ViewBag`può essere più semplice da utilizzare in quanto non richiede il cast.</span><span class="sxs-lookup"><span data-stu-id="95441-236">`ViewBag` can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="95441-237">Nell'esempio seguente viene illustrato come utilizzare `ViewBag` con lo stesso risultato utilizzando `ViewData` sopra:</span><span class="sxs-lookup"><span data-stu-id="95441-237">The following example shows how to use `ViewBag` with the same result as using `ViewData` above:</span></span>

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

<span data-ttu-id="95441-238">**Utilizzano ViewData e ViewBag simultaneamente**</span><span class="sxs-lookup"><span data-stu-id="95441-238">**Using ViewData and ViewBag simultaneously**</span></span>

<span data-ttu-id="95441-239">Nota: `ViewBag` non è disponibile nelle pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="95441-239">Note: `ViewBag` is not available in the Razor Pages.</span></span>

<span data-ttu-id="95441-240">Poiché `ViewData` e `ViewBag` fare riferimento alla stessa sottostante `ViewData` insieme, è possibile utilizzare sia `ViewData` e `ViewBag` e combinare e associare tra loro durante la lettura e scrittura di valori.</span><span class="sxs-lookup"><span data-stu-id="95441-240">Since `ViewData` and `ViewBag` refer to the same underlying `ViewData` collection, you can use both `ViewData` and `ViewBag` and mix and match between them when reading and writing values.</span></span>

<span data-ttu-id="95441-241">Impostare il titolo tramite `ViewBag` e la descrizione usando `ViewData` nella parte superiore di un *About.cshtml* Vista:</span><span class="sxs-lookup"><span data-stu-id="95441-241">Set the title using `ViewBag` and the description using `ViewData` at the top of an *About.cshtml* view:</span></span>

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

<span data-ttu-id="95441-242">Leggere le proprietà ma inverte l'utilizzo di `ViewData` e `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="95441-242">Read the properties but reverse the use of `ViewData` and `ViewBag`.</span></span> <span data-ttu-id="95441-243">Nel *layout. cshtml* file, ottenere il titolo tramite `ViewData` e ottenere la descrizione utilizzando `ViewBag`:</span><span class="sxs-lookup"><span data-stu-id="95441-243">In the *_Layout.cshtml* file, obtain the title using `ViewData` and obtain the description using `ViewBag`:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

<span data-ttu-id="95441-244">Tenere presente che le stringhe non richiedono un cast per `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="95441-244">Remember that strings don't require a cast for `ViewData`.</span></span> <span data-ttu-id="95441-245">È possibile utilizzare `@ViewData["Title"]` senza eseguire il cast.</span><span class="sxs-lookup"><span data-stu-id="95441-245">You can use `@ViewData["Title"]` without casting.</span></span>

<span data-ttu-id="95441-246">Utilizzo di entrambe `ViewData` e `ViewBag` in lavori stesso, tempo fa, combinazione e la corrispondenza di lettura e scrittura delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="95441-246">Using both `ViewData` and `ViewBag` at the same time works, as does mixing and matching reading and writing the properties.</span></span> <span data-ttu-id="95441-247">Il markup seguente viene eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="95441-247">The following markup is rendered:</span></span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

<span data-ttu-id="95441-248">**Riepilogo delle differenze tra ViewData e ViewBag**</span><span class="sxs-lookup"><span data-stu-id="95441-248">**Summary of the differences between ViewData and ViewBag**</span></span>

 <span data-ttu-id="95441-249">`ViewBag`non è disponibile nelle pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="95441-249">`ViewBag` is not available in the Razor Pages.</span></span>

* `ViewData`
  * <span data-ttu-id="95441-250">Deriva da [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), pertanto dispone di proprietà di dizionario che può essere utile, ad esempio `ContainsKey`, `Add`, `Remove`, e `Clear`.</span><span class="sxs-lookup"><span data-stu-id="95441-250">Derives from [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), so it has dictionary properties that can be useful, such as `ContainsKey`, `Add`, `Remove`, and `Clear`.</span></span>
  * <span data-ttu-id="95441-251">Le chiavi nel dizionario sono stringhe, pertanto gli spazi vuoti sono consentito.</span><span class="sxs-lookup"><span data-stu-id="95441-251">Keys in the dictionary are strings, so whitespace is allowed.</span></span> <span data-ttu-id="95441-252">Esempio: `ViewData["Some Key With Whitespace"]`</span><span class="sxs-lookup"><span data-stu-id="95441-252">Example: `ViewData["Some Key With Whitespace"]`</span></span>
  * <span data-ttu-id="95441-253">Qualsiasi tipo diverso da un `string` deve essere impostato nella visualizzazione per utilizzare `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="95441-253">Any type other than a `string` must be cast in the view to use `ViewData`.</span></span>
* `ViewBag`
  * <span data-ttu-id="95441-254">Deriva da [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), quindi ne consente la creazione delle proprietà dinamiche utilizzando la notazione del punto (`@ViewBag.SomeKey = <value or object>`), ed non è necessario alcun cast.</span><span class="sxs-lookup"><span data-stu-id="95441-254">Derives from [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), so it allows the creation of dynamic properties using dot notation (`@ViewBag.SomeKey = <value or object>`), and no casting is required.</span></span> <span data-ttu-id="95441-255">La sintassi di `ViewBag` rende più rapido aggiungere al controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="95441-255">The syntax of `ViewBag` makes it quicker to add to controllers and views.</span></span>
  * <span data-ttu-id="95441-256">Più semplice verificare la presenza di valori null.</span><span class="sxs-lookup"><span data-stu-id="95441-256">Simpler to check for null values.</span></span> <span data-ttu-id="95441-257">Esempio: `@ViewBag.Person?.Name`</span><span class="sxs-lookup"><span data-stu-id="95441-257">Example: `@ViewBag.Person?.Name`</span></span>

<span data-ttu-id="95441-258">**Quando utilizzare ViewData o ViewBag**</span><span class="sxs-lookup"><span data-stu-id="95441-258">**When to use ViewData or ViewBag**</span></span>

<span data-ttu-id="95441-259">Entrambi `ViewData` e `ViewBag` sono ugualmente validi approcci per il passaggio di piccole quantità di dati tra i controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="95441-259">Both `ViewData` and `ViewBag` are equally valid approaches for passing small amounts of data among controllers and views.</span></span> <span data-ttu-id="95441-260">La scelta di quello da utilizzare è basata sulla preferenza.</span><span class="sxs-lookup"><span data-stu-id="95441-260">The choice of which one to use is based on preference.</span></span> <span data-ttu-id="95441-261">È possibile combinare e associare `ViewData` e `ViewBag` oggetti, tuttavia, il codice è più facile da leggere e gestire con un approccio utilizzato in modo coerente.</span><span class="sxs-lookup"><span data-stu-id="95441-261">You can mix and match `ViewData` and `ViewBag` objects, however, the code is easier to read and maintain with one approach used consistently.</span></span> <span data-ttu-id="95441-262">Entrambi gli approcci sono risolti in modo dinamico in fase di esecuzione e quindi soggetto a causa di errori di runtime.</span><span class="sxs-lookup"><span data-stu-id="95441-262">Both approaches are dynamically resolved at runtime and thus prone to causing runtime errors.</span></span> <span data-ttu-id="95441-263">Alcuni team di sviluppo per evitare il problema.</span><span class="sxs-lookup"><span data-stu-id="95441-263">Some development teams avoid them.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="95441-264">Visualizzazioni dinamiche</span><span class="sxs-lookup"><span data-stu-id="95441-264">Dynamic views</span></span>

<span data-ttu-id="95441-265">Tipo di viste che non dichiarano un modello utilizzando `@model` ma che dispongono di un'istanza del modello passata a essi (ad esempio, `return View(Address);`) possono fare riferimento le proprietà dell'istanza in modo dinamico:</span><span class="sxs-lookup"><span data-stu-id="95441-265">Views that don't declare a model type using `@model` but that have a model instance passed to them (for example, `return View(Address);`) can reference the instance's properties dynamically:</span></span>

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

<span data-ttu-id="95441-266">Questa funzionalità offre flessibilità ma non offre protezione compilazione o IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="95441-266">This feature offers flexibility but doesn't offer compilation protection or IntelliSense.</span></span> <span data-ttu-id="95441-267">Se la proprietà non esiste, la generazione di pagine Web non riesce in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="95441-267">If the property doesn't exist, webpage generation fails at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="95441-268">Altre funzionalità di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="95441-268">More view features</span></span>

<span data-ttu-id="95441-269">[Gli helper di tag](xref:mvc/views/tag-helpers/intro) rendono più facile aggiungere un comportamento lato server per i tag HTML esistenti.</span><span class="sxs-lookup"><span data-stu-id="95441-269">[Tag Helpers](xref:mvc/views/tag-helpers/intro) make it easy to add server-side behavior to existing HTML tags.</span></span> <span data-ttu-id="95441-270">Gli helper di Tag consente di evitare la necessità di scrivere codice personalizzato o helper all'interno delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="95441-270">Using Tag Helpers avoids the need to write custom code or helpers within your views.</span></span> <span data-ttu-id="95441-271">Gli helper di tag vengono applicati come attributi agli elementi HTML e vengono ignorati per gli editor che non è possibile elaborarli.</span><span class="sxs-lookup"><span data-stu-id="95441-271">Tag helpers are applied as attributes to HTML elements and are ignored by editors that can't process them.</span></span> <span data-ttu-id="95441-272">Ciò consente di modificare ed eseguire il rendering di markup di visualizzazione in un'ampia gamma di strumenti.</span><span class="sxs-lookup"><span data-stu-id="95441-272">This allows you to edit and render view markup in a variety of tools.</span></span>

<span data-ttu-id="95441-273">Generazione di codice HTML personalizzato può essere ottenuta con molti helper HTML incorporato.</span><span class="sxs-lookup"><span data-stu-id="95441-273">Generating custom HTML markup can be achieved with many built-in HTML Helpers.</span></span> <span data-ttu-id="95441-274">Una logica più complessa dell'interfaccia utente può essere gestita da [Visualizza i componenti](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="95441-274">More complex user interface logic can be handled by [View Components](xref:mvc/views/view-components).</span></span> <span data-ttu-id="95441-275">Visualizza i componenti forniscono la stessa SoC tale controller e visualizzazioni offrono.</span><span class="sxs-lookup"><span data-stu-id="95441-275">View components provide the same SoC that controllers and views offer.</span></span> <span data-ttu-id="95441-276">È possibile eliminare la necessità di azioni e visualizzazioni che gestiscono i dati utilizzati da elementi dell'interfaccia utente comune.</span><span class="sxs-lookup"><span data-stu-id="95441-276">They can eliminate the need for actions and views that deal with data used by common user interface elements.</span></span>

<span data-ttu-id="95441-277">Come molti altri aspetti di ASP.NET Core, viste supportano [inserimento di dipendenze](xref:fundamentals/dependency-injection), consentendo di servizi da [inserite nelle viste](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="95441-277">Like many other aspects of ASP.NET Core, views support [dependency injection](xref:fundamentals/dependency-injection), allowing services to be [injected into views](xref:mvc/views/dependency-injection).</span></span>
