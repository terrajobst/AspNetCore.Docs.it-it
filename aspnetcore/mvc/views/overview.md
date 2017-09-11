---
title: Panoramica di viste
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: 318d8832dadadd6946c7ffe58f9d89aaf68f54fc
ms.sourcegitcommit: 4693cb02d845adf2efa00e07ad432c81867bfa12
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/08/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a><span data-ttu-id="089d3-103">Il rendering di HTML con le viste in ASP.NET MVC di base</span><span class="sxs-lookup"><span data-stu-id="089d3-103">Rendering HTML with views in ASP.NET Core MVC</span></span>

<span data-ttu-id="089d3-104">Da [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="089d3-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="089d3-105">Controller MVC Core ASP.NET può restituire risultati formattati tramite *viste*.</span><span class="sxs-lookup"><span data-stu-id="089d3-105">ASP.NET Core MVC controllers can return formatted results using *views*.</span></span>

## <a name="what-are-views"></a><span data-ttu-id="089d3-106">Quali sono le visualizzazioni?</span><span class="sxs-lookup"><span data-stu-id="089d3-106">What are Views?</span></span>

<span data-ttu-id="089d3-107">Nel modello Model-View-Controller (MVC), il *vista* incapsula i dettagli di presentazione di interazione dell'utente con l'app.</span><span class="sxs-lookup"><span data-stu-id="089d3-107">In the Model-View-Controller (MVC) pattern, the *view* encapsulates the presentation details of the user's interaction with the app.</span></span> <span data-ttu-id="089d3-108">Le visualizzazioni sono modelli HTML con codice incorporato che generano contenuto da inviare al client.</span><span class="sxs-lookup"><span data-stu-id="089d3-108">Views are HTML templates with embedded code that generate content to send to the client.</span></span> <span data-ttu-id="089d3-109">Visualizzazioni utilizzano [sintassi Razor](razor.md), che consente l'interazione con codice HTML con quantità minima di codice o conferimento del codice.</span><span class="sxs-lookup"><span data-stu-id="089d3-109">Views use [Razor syntax](razor.md), which allows code to interact with HTML with minimal code or ceremony.</span></span>

<span data-ttu-id="089d3-110">Le visualizzazioni ASP.NET MVC di base sono *. cshtml* file archiviati per impostazione predefinita in un *viste* cartella all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="089d3-110">ASP.NET Core MVC views are *.cshtml* files stored by default in a *Views* folder within the application.</span></span> <span data-ttu-id="089d3-111">In genere, ogni controller disporrà cartella corrispondente, in cui sono viste per le azioni del controller specifico.</span><span class="sxs-lookup"><span data-stu-id="089d3-111">Typically, each controller will have its own folder, in which are views for specific controller actions.</span></span>

![Cartella Views in Esplora soluzioni](overview/_static/views_solution_explorer.png)

<span data-ttu-id="089d3-113">Oltre alle viste di azione specifici, [visualizzazioni parziali](partial.md), [layout e altri file di visualizzazione speciale](layout.md) può essere utilizzato per ridurre la ripetizione e consentire di riutilizzare all'interno delle visualizzazioni dell'app.</span><span class="sxs-lookup"><span data-stu-id="089d3-113">In addition to action-specific views, [partial views](partial.md), [layouts, and other special view files](layout.md) can be used to help reduce repetition and allow for reuse within the app's views.</span></span>

## <a name="benefits-of-using-views"></a><span data-ttu-id="089d3-114">Vantaggi dell'utilizzo di viste</span><span class="sxs-lookup"><span data-stu-id="089d3-114">Benefits of Using Views</span></span>

<span data-ttu-id="089d3-115">Forniscono visualizzazioni [separazione delle problematiche](http://deviq.com/separation-of-concerns/) all'interno di un'applicazione MVC, che incapsula markup livello di interfaccia utente separatamente dalla logica di business.</span><span class="sxs-lookup"><span data-stu-id="089d3-115">Views provide [separation of concerns](http://deviq.com/separation-of-concerns/) within an MVC app, encapsulating user interface level markup separately from business logic.</span></span> <span data-ttu-id="089d3-116">ASP.NET MVC visualizzazioni utilizzano [sintassi Razor](razor.md) per cambiare HTML markup e server logica sul lato semplice.</span><span class="sxs-lookup"><span data-stu-id="089d3-116">ASP.NET MVC views use [Razor syntax](razor.md) to make switching between HTML markup and server side logic painless.</span></span> <span data-ttu-id="089d3-117">Aspetti comuni, ricorrenti dell'interfaccia utente dell'applicazione possono essere riutilizzati facilmente tra viste utilizzando [layout e direttive condivise](layout.md) o [visualizzazioni parziali](partial.md).</span><span class="sxs-lookup"><span data-stu-id="089d3-117">Common, repetitive aspects of the app's user interface can easily be reused between views using [layout and shared directives](layout.md) or [partial views](partial.md).</span></span>

## <a name="creating-a-view"></a><span data-ttu-id="089d3-118">Creazione di una vista</span><span class="sxs-lookup"><span data-stu-id="089d3-118">Creating a View</span></span>

<span data-ttu-id="089d3-119">Le viste che sono specifiche di un controller vengono create nel *viste / [ControllerName]* cartella.</span><span class="sxs-lookup"><span data-stu-id="089d3-119">Views that are specific to a controller are created in the *Views/[ControllerName]* folder.</span></span> <span data-ttu-id="089d3-120">Le viste che vengono condivisi tra i controller vengono inserite nel */visualizzazioni/Shared* cartella.</span><span class="sxs-lookup"><span data-stu-id="089d3-120">Views that are shared among controllers are placed in the */Views/Shared* folder.</span></span> <span data-ttu-id="089d3-121">Nome del file di visualizzazione come l'azione del controller associato e aggiungere il *. cshtml* estensione di file.</span><span class="sxs-lookup"><span data-stu-id="089d3-121">Name the view file the same as its associated controller action, and add the *.cshtml* file extension.</span></span> <span data-ttu-id="089d3-122">Ad esempio, per creare una vista per la *su* azione sul *Home* controller, è necessario creare il *About.cshtml* file nel  * /visualizzazioni/Home*cartella.</span><span class="sxs-lookup"><span data-stu-id="089d3-122">For example, to create a view for the *About* action on the *Home* controller, you would create the *About.cshtml* file in the */Views/Home* folder.</span></span>

<span data-ttu-id="089d3-123">Un file di vista di esempio (*About.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="089d3-123">A sample view file (*About.cshtml*):</span></span>

<span data-ttu-id="089d3-124">[!code-html[Principale](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="089d3-124">[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]</span></span>

<span data-ttu-id="089d3-125">*Razor* codice è indicato il `@` simbolo.</span><span class="sxs-lookup"><span data-stu-id="089d3-125">*Razor* code is denoted by the `@` symbol.</span></span> <span data-ttu-id="089d3-126">C# istruzioni vengono eseguite all'interno di blocchi di codice disattivare le parentesi graffe Razor (`{` `}`), ad esempio l'assegnazione di "About" per il `ViewData["Title"]` elemento illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="089d3-126">C# statements are run within Razor code blocks set off by curly braces (`{` `}`), such as the assignment of "About" to the `ViewData["Title"]` element shown above.</span></span> <span data-ttu-id="089d3-127">Razor può essere utilizzato per visualizzare i valori all'interno di HTML facendo riferimento il valore con il `@` symbol, come illustrato nel `<h2>` e `<h3>` elementi sopra riportato.</span><span class="sxs-lookup"><span data-stu-id="089d3-127">Razor can be used to display values within HTML by simply referencing the value with the `@` symbol, as shown within the `<h2>` and `<h3>` elements above.</span></span>

<span data-ttu-id="089d3-128">Questa vista è incentrata sulla solo la parte dell'output di cui è responsabile.</span><span class="sxs-lookup"><span data-stu-id="089d3-128">This view focuses on just the portion of the output for which it is responsible.</span></span> <span data-ttu-id="089d3-129">Il resto del layout della pagina e altri aspetti comuni della visualizzazione, vengono specificati in un' posizione.</span><span class="sxs-lookup"><span data-stu-id="089d3-129">The rest of the page's layout, and other common aspects of the view, are specified elsewhere.</span></span> <span data-ttu-id="089d3-130">Altre informazioni, vedere [layout e la logica di visualizzazione condivisa](layout.md).</span><span class="sxs-lookup"><span data-stu-id="089d3-130">Learn more about [layout and shared view logic](layout.md).</span></span>

## <a name="how-do-controllers-specify-views"></a><span data-ttu-id="089d3-131">Come viste specificare controller?</span><span class="sxs-lookup"><span data-stu-id="089d3-131">How do Controllers Specify Views?</span></span>

<span data-ttu-id="089d3-132">Le visualizzazioni vengono in genere restituite dalle azioni come un `ViewResult`.</span><span class="sxs-lookup"><span data-stu-id="089d3-132">Views are typically returned from actions as a `ViewResult`.</span></span> <span data-ttu-id="089d3-133">Metodo di azione, è possibile creare e restituire un `ViewResult` direttamente, ma in genere se il controller eredita da `Controller`, si utilizzerà semplicemente il `View` metodo di supporto, come in questo esempio viene illustrato:</span><span class="sxs-lookup"><span data-stu-id="089d3-133">Your action method can create and return a `ViewResult` directly, but more commonly if your controller inherits from `Controller`, you'll simply use the `View` helper method, as this example demonstrates:</span></span>

<span data-ttu-id="089d3-134">*HomeController. cs*</span><span class="sxs-lookup"><span data-stu-id="089d3-134">*HomeController.cs*</span></span>

<span data-ttu-id="089d3-135">[!code-csharp[Principale](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span><span class="sxs-lookup"><span data-stu-id="089d3-135">[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]</span></span>

<span data-ttu-id="089d3-136">Il `View` metodo di supporto contiene diversi overload per rendere più semplice la restituzione di visualizzazioni per gli sviluppatori di app.</span><span class="sxs-lookup"><span data-stu-id="089d3-136">The `View` helper method has several overloads to make returning views easier for app developers.</span></span> <span data-ttu-id="089d3-137">È possibile specificare facoltativamente una visualizzazione da restituire, nonché un oggetto modello da passare alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="089d3-137">You can optionally specify a view to return, as well as a model object to pass to the view.</span></span>

<span data-ttu-id="089d3-138">Quando questa azione viene restituito, il *About.cshtml* visualizzazione indicata sopra viene eseguito il rendering:</span><span class="sxs-lookup"><span data-stu-id="089d3-138">When this action returns, the *About.cshtml* view shown above is rendered:</span></span>

![Informazioni sulla pagina](overview/_static/about-page.png)

### <a name="view-discovery"></a><span data-ttu-id="089d3-140">Individuazione di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="089d3-140">View Discovery</span></span>

<span data-ttu-id="089d3-141">Quando un'azione restituisce una visualizzazione, un processo denominato *individuazione visualizzazione* avviene.</span><span class="sxs-lookup"><span data-stu-id="089d3-141">When an action returns a view, a process called *view discovery* takes place.</span></span> <span data-ttu-id="089d3-142">Questo processo determina quali file di visualizzazione verrà utilizzato.</span><span class="sxs-lookup"><span data-stu-id="089d3-142">This process determines which view file will be used.</span></span> <span data-ttu-id="089d3-143">A meno che non viene specificato un file di visualizzazione, il runtime cercherà una visualizzazione specifica del controller, quindi cerca innanzitutto il nome di visualizzazione corrispondente nel *Shared* cartella.</span><span class="sxs-lookup"><span data-stu-id="089d3-143">Unless a specific view file is specified, the runtime looks for a controller-specific view first, then looks for matching view name in the *Shared* folder.</span></span>

<span data-ttu-id="089d3-144">Quando un'azione restituisce il `View` metodo, ad esempio `return View();`, il nome dell'azione viene utilizzato come nome della vista.</span><span class="sxs-lookup"><span data-stu-id="089d3-144">When an action returns the `View` method, like so `return View();`, the action name is used as the view name.</span></span> <span data-ttu-id="089d3-145">Ad esempio, se è stato chiamato da un metodo di azione denominato "Index", sarebbe equivalente al passaggio di un nome di vista di "Index".</span><span class="sxs-lookup"><span data-stu-id="089d3-145">For example, if this were called from an action method named "Index", it would be equivalent to passing in a view name of "Index".</span></span> <span data-ttu-id="089d3-146">Nome di una vista può essere passato al metodo in modo esplicito (`return View("SomeView");`).</span><span class="sxs-lookup"><span data-stu-id="089d3-146">A view name can be explicitly passed to the method (`return View("SomeView");`).</span></span> <span data-ttu-id="089d3-147">In entrambi i casi, visualizzare l'individuazione Cerca un file di visualizzazione corrispondente in:</span><span class="sxs-lookup"><span data-stu-id="089d3-147">In both of these cases, view discovery searches for a matching view file in:</span></span>

   1. <span data-ttu-id="089d3-148">Viste /\<ControllerName > /\<ViewName >. cshtml</span><span class="sxs-lookup"><span data-stu-id="089d3-148">Views/\<ControllerName>/\<ViewName>.cshtml</span></span>

   2. <span data-ttu-id="089d3-149">Viste/Shared/\<ViewName >. cshtml</span><span class="sxs-lookup"><span data-stu-id="089d3-149">Views/Shared/\<ViewName>.cshtml</span></span>

>[!TIP]
> <span data-ttu-id="089d3-150">Si consiglia di seguenti la convenzione di restituire semplicemente `View()` dalle azioni quando possibile, poiché comporta più flessibile e più semplice per il refactoring del codice.</span><span class="sxs-lookup"><span data-stu-id="089d3-150">We recommend following the convention of simply returning `View()` from actions when possible, as it results in more flexible, easier to refactor code.</span></span>

<span data-ttu-id="089d3-151">Può essere fornito un percorso di file di visualizzazione, invece di un nome di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="089d3-151">A view file path can be provided, instead of a view name.</span></span> <span data-ttu-id="089d3-152">Se si utilizza un percorso assoluto a partire dalla radice dell'applicazione (facoltativamente inizia con "/" o "~ /"), il *. cshtml* estensione deve essere specificata come parte del percorso del file.</span><span class="sxs-lookup"><span data-stu-id="089d3-152">If using an absolute path starting at the application root (optionally starting with "/" or "~/"), the *.cshtml* extension must be specified as part of the file path.</span></span> <span data-ttu-id="089d3-153">Ad esempio: `return View("Views/Home/About.cshtml");`.</span><span class="sxs-lookup"><span data-stu-id="089d3-153">For example: `return View("Views/Home/About.cshtml");`.</span></span> <span data-ttu-id="089d3-154">In alternativa, è possibile utilizzare un percorso relativo dalla directory del controller specifico all'interno di *viste* directory per specificare le viste in directory diverse.</span><span class="sxs-lookup"><span data-stu-id="089d3-154">Alternatively, you can use a relative path from the controller-specific directory within the *Views* directory to specify views in different directories.</span></span> <span data-ttu-id="089d3-155">Ad esempio: `return View("../Manage/Index");` all'interno di *Home* controller.</span><span class="sxs-lookup"><span data-stu-id="089d3-155">For example: `return View("../Manage/Index");` inside the *Home* controller.</span></span> <span data-ttu-id="089d3-156">Analogamente, è possibile scorrere la directory di specifici controller corrente: `return View("./About");`.</span><span class="sxs-lookup"><span data-stu-id="089d3-156">Similarly, you can traverse the current controller-specific directory: `return View("./About");`.</span></span> <span data-ttu-id="089d3-157">Si noti che i percorsi relativi non usano il *. cshtml* estensione.</span><span class="sxs-lookup"><span data-stu-id="089d3-157">Note that relative paths don't use the *.cshtml* extension.</span></span> <span data-ttu-id="089d3-158">Come accennato in precedenza, seguire la procedura consigliata di organizzare la struttura dei file per le viste in modo da riflettere le relazioni tra i controller, azioni e visualizzazioni per maggiore chiarezza e la manutenibilità.</span><span class="sxs-lookup"><span data-stu-id="089d3-158">As previously mentioned, follow the best practice of organizing the file structure for views to reflect the relationships among controllers, actions, and views for maintainability and clarity.</span></span>

> [!NOTE]
> <span data-ttu-id="089d3-159">[Le visualizzazioni parziali](partial.md) e [visualizzare componenti](view-components.md) utilizzare meccanismi di individuazione simile (ma non identica).</span><span class="sxs-lookup"><span data-stu-id="089d3-159">[Partial views](partial.md) and [view components](view-components.md) use similar (but not identical) discovery mechanisms.</span></span>

> [!NOTE]
> <span data-ttu-id="089d3-160">È possibile personalizzare la convenzione predefinita relative in cui le visualizzazioni si trovano all'interno dell'app usando un oggetto personalizzato `IViewLocationExpander`.</span><span class="sxs-lookup"><span data-stu-id="089d3-160">You can customize the default convention regarding where views are located within the app by using a custom `IViewLocationExpander`.</span></span>

>[!TIP]
> <span data-ttu-id="089d3-161">Nomi di visualizzazione possono essere tra maiuscole e minuscole in base al file system sottostante.</span><span class="sxs-lookup"><span data-stu-id="089d3-161">View names may be case sensitive depending on the underlying file system.</span></span> <span data-ttu-id="089d3-162">Per garantire la compatibilità tra i sistemi operativi, è sempre maiuscole/minuscole tra controller e i nomi di azione e le cartelle di visualizzazione associata e i nomi di file.</span><span class="sxs-lookup"><span data-stu-id="089d3-162">For compatibility across operating systems, always match case between controller and action names and associated view folders and filenames.</span></span>

## <a name="passing-data-to-views"></a><span data-ttu-id="089d3-163">Passaggio di dati a viste</span><span class="sxs-lookup"><span data-stu-id="089d3-163">Passing Data to Views</span></span>

<span data-ttu-id="089d3-164">È possibile passare dati alle viste utilizzando diversi meccanismi.</span><span class="sxs-lookup"><span data-stu-id="089d3-164">You can pass data to views using several mechanisms.</span></span> <span data-ttu-id="089d3-165">L'approccio più efficace consiste nello specificare un *modello* tipo nella visualizzazione (conosciuta come un *viewmodel*, per distinguerlo dai tipi di modello di dominio aziendale) e quindi passare un'istanza di questo tipo per la visualizzazione dall'azione.</span><span class="sxs-lookup"><span data-stu-id="089d3-165">The most robust approach is to specify a *model* type in the view (commonly referred to as a *viewmodel*, to distinguish it from business domain model types), and then pass an instance of this type to the view from the action.</span></span> <span data-ttu-id="089d3-166">È consigliabile che utilizzare un modello o modello di visualizzazione per passare dati a una visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="089d3-166">We recommend you use a model or view model to pass data to a view.</span></span> <span data-ttu-id="089d3-167">In questo modo la visualizzazione di sfruttare il controllo dei tipi sicuri.</span><span class="sxs-lookup"><span data-stu-id="089d3-167">This allows the view to take advantage of strong type checking.</span></span> <span data-ttu-id="089d3-168">È possibile specificare un modello per una visualizzazione utilizzando il `@model` direttiva:</span><span class="sxs-lookup"><span data-stu-id="089d3-168">You can specify a model for a view using the `@model` directive:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="089d3-169">Una volta che un modello è stato specificato per una vista, l'istanza inviato per la visualizzazione è possibile accedere in modo fortemente tipizzato tramite `@Model` come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="089d3-169">Once a model has been specified for a view, the instance sent to the view can be accessed in a strongly-typed manner using `@Model` as shown above.</span></span> <span data-ttu-id="089d3-170">Per fornire un'istanza del tipo di modello per la visualizzazione, il controller passa come parametro:</span><span class="sxs-lookup"><span data-stu-id="089d3-170">To provide an instance of the model type to the view, the controller passes it as a parameter:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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

<span data-ttu-id="089d3-171">Non sono previste restrizioni per i tipi che possono essere forniti per una vista come modello.</span><span class="sxs-lookup"><span data-stu-id="089d3-171">There are no restrictions on the types that can be provided to a view as a model.</span></span> <span data-ttu-id="089d3-172">È consigliabile passare oggetti poco (Plain Old CLR Object) di visualizzazione di modelli con pochi o nessun comportamento, in modo che la logica di business può essere incapsulata in un' posizione nell'app.</span><span class="sxs-lookup"><span data-stu-id="089d3-172">We recommend passing Plain Old CLR Object (POCO) view models with little or no behavior, so that business logic can be encapsulated elsewhere in the app.</span></span> <span data-ttu-id="089d3-173">Un esempio di questo approccio è la *indirizzo* viewmodel utilizzato nell'esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="089d3-173">An example of this approach is the *Address* viewmodel used in the example above:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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
> <span data-ttu-id="089d3-174">Non impedisce di utilizzare le stesse classi come tipi di modello di business e i tipi di modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="089d3-174">Nothing prevents you from using the same classes as your business model types and your display model types.</span></span> <span data-ttu-id="089d3-175">Tuttavia, mantenendoli separato consente le visualizzazioni variare in modo indipendente dal modello di dominio o persistenza e può offrire alcuni vantaggi di sicurezza nonché (per i modelli che invieranno agli utenti per l'app mediante [associazione del modello](../models/model-binding.md)).</span><span class="sxs-lookup"><span data-stu-id="089d3-175">However, keeping them separate allows your views to vary independently from your domain or persistence model, and can offer some security benefits as well (for models that users will send to the app using [model binding](../models/model-binding.md)).</span></span>

### <a name="loosely-typed-data"></a><span data-ttu-id="089d3-176">Dati non tipizzati</span><span class="sxs-lookup"><span data-stu-id="089d3-176">Loosely Typed Data</span></span>

<span data-ttu-id="089d3-177">Oltre a visualizzazioni fortemente tipizzate, tutte le visualizzazioni hanno accesso a una raccolta di dati non tipizzato.</span><span class="sxs-lookup"><span data-stu-id="089d3-177">In addition to strongly typed views, all views have access to a loosely typed collection of data.</span></span> <span data-ttu-id="089d3-178">È possibile fare riferimento a questa raccolta stessa tramite la `ViewData` o `ViewBag` proprietà sul controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="089d3-178">This same collection can be referenced through either the `ViewData` or `ViewBag` properties on controllers and views.</span></span> <span data-ttu-id="089d3-179">Il `ViewBag` proprietà è un wrapper `ViewData` che fornisce una visualizzazione dinamica in tale raccolta.</span><span class="sxs-lookup"><span data-stu-id="089d3-179">The `ViewBag` property is a wrapper around `ViewData` that provides a dynamic view over that collection.</span></span> <span data-ttu-id="089d3-180">Non è una raccolta separata.</span><span class="sxs-lookup"><span data-stu-id="089d3-180">It is not a separate collection.</span></span>

<span data-ttu-id="089d3-181">`ViewData`un oggetto dizionario avviene tramite `string` chiavi.</span><span class="sxs-lookup"><span data-stu-id="089d3-181">`ViewData` is a dictionary object accessed through `string` keys.</span></span> <span data-ttu-id="089d3-182">È possibile archiviare e recuperare gli oggetti in essa contenuti e dovrai cast a un tipo specifico quando si estrae li.</span><span class="sxs-lookup"><span data-stu-id="089d3-182">You can store and retrieve objects in it, and you'll need to cast them to a specific type when you extract them.</span></span> <span data-ttu-id="089d3-183">È possibile utilizzare `ViewData` per passare i dati da un controller per le visualizzazioni, nonché all'interno di viste (e le visualizzazioni parziali e layout).</span><span class="sxs-lookup"><span data-stu-id="089d3-183">You can use `ViewData` to pass data from a controller to views, as well as within views (and partial views and layouts).</span></span> <span data-ttu-id="089d3-184">Dati di tipo stringa possono essere archiviati e utilizzati direttamente, senza la necessità di un cast.</span><span class="sxs-lookup"><span data-stu-id="089d3-184">String data can be stored and used directly, without the need for a cast.</span></span>

<span data-ttu-id="089d3-185">Impostare alcuni valori per `ViewData` un'azione:</span><span class="sxs-lookup"><span data-stu-id="089d3-185">Set some values for `ViewData` in an action:</span></span>

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

<span data-ttu-id="089d3-186">Utilizzare i dati in una vista:</span><span class="sxs-lookup"><span data-stu-id="089d3-186">Work with the data in a view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

<span data-ttu-id="089d3-187">Il `ViewBag` oggetti fornisce l'accesso dinamico per gli oggetti archiviati in `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="089d3-187">The `ViewBag` objects provides dynamic access to the objects stored in `ViewData`.</span></span> <span data-ttu-id="089d3-188">Può essere più semplice da utilizzare in quanto non richiede il cast.</span><span class="sxs-lookup"><span data-stu-id="089d3-188">This can be more convenient to work with, since it doesn't require casting.</span></span> <span data-ttu-id="089d3-189">Nell'esempio stesso come in precedenza, mediante `ViewBag` anziché un oggetto fortemente tipizzato `address` istanza nella visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="089d3-189">The same example as above, using `ViewBag` instead of a strongly typed `address` instance in the view:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> <span data-ttu-id="089d3-190">Poiché entrambi si riferiscono allo stesso sottostante `ViewData` insieme, è possibile combinare e trovare una corrispondenza tra `ViewData` e `ViewBag` durante la lettura e scrittura di valori, se pratico.</span><span class="sxs-lookup"><span data-stu-id="089d3-190">Since both refer to the same underlying `ViewData` collection, you can mix and match between `ViewData` and `ViewBag` when reading and writing values, if convenient.</span></span>

### <a name="dynamic-views"></a><span data-ttu-id="089d3-191">Visualizzazioni dinamiche</span><span class="sxs-lookup"><span data-stu-id="089d3-191">Dynamic Views</span></span>

<span data-ttu-id="089d3-192">Viste non dichiarano un tipo di modello, ma hanno un'istanza del modello vengano passata possono fare riferimento l'istanza in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="089d3-192">Views that do not declare a model type but have a model instance passed to them can reference this instance dynamically.</span></span> <span data-ttu-id="089d3-193">Ad esempio, se un'istanza di `Address` viene passato a una vista che non si dichiara un `@model`, la visualizzazione sarà comunque in grado di fare riferimento alle proprietà dell'istanza in modo dinamico, come illustrato:</span><span class="sxs-lookup"><span data-stu-id="089d3-193">For example, if an instance of `Address` is passed to a view that doesn't declare an `@model`, the view would still be able to refer to the instance's properties dynamically as shown:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

<span data-ttu-id="089d3-194">Questa funzionalità può offrire una certa flessibilità, ma non offre alcun protezione compilazione IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="089d3-194">This feature can offer some flexibility, but does not offer any compilation protection or IntelliSense.</span></span> <span data-ttu-id="089d3-195">Se la proprietà non esiste, la pagina avrà esito negativo in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="089d3-195">If the property doesn't exist, the page will fail at runtime.</span></span>

## <a name="more-view-features"></a><span data-ttu-id="089d3-196">Altre funzionalità di visualizzazione</span><span class="sxs-lookup"><span data-stu-id="089d3-196">More View Features</span></span>

<span data-ttu-id="089d3-197">[Gli helper di tag](tag-helpers/intro.md) rendono più facile aggiungere un comportamento lato server per i tag HTML esistenti, evitando la necessità di utilizzare codice personalizzato o helper all'interno di viste.</span><span class="sxs-lookup"><span data-stu-id="089d3-197">[Tag helpers](tag-helpers/intro.md) make it easy to add server-side behavior to existing HTML tags, avoiding the need to use custom code or helpers within views.</span></span> <span data-ttu-id="089d3-198">Gli helper di tag vengono applicati come attributi agli elementi HTML, che vengono ignorati dall'editor che non ha familiarità con essi, che consente di visualizzare i tag essere modificato e il rendering in una varietà di strumenti.</span><span class="sxs-lookup"><span data-stu-id="089d3-198">Tag helpers are applied as attributes to HTML elements, which are ignored by editors that aren't familiar with them, allowing view markup to be edited and rendered in a variety of tools.</span></span> <span data-ttu-id="089d3-199">Gli helper di tag di SQL e in particolare è possibile effettuare [utilizzo dei moduli](working-with-forms.md) molto più semplice.</span><span class="sxs-lookup"><span data-stu-id="089d3-199">Tag helpers have many uses, and in particular can make [working with forms](working-with-forms.md) much easier.</span></span>

<span data-ttu-id="089d3-200">Generazione di codice HTML personalizzato può essere ottenuta con molti helper HTML incorporati, e una logica più complessa dell'interfaccia utente (potenzialmente con i propri requisiti di dati) può essere incapsulata [Visualizza i componenti](view-components.md).</span><span class="sxs-lookup"><span data-stu-id="089d3-200">Generating custom HTML markup can be achieved with many built-in HTML Helpers, and more complex UI logic (potentially with its own data requirements) can be encapsulated in [View Components](view-components.md).</span></span> <span data-ttu-id="089d3-201">Visualizza i componenti forniscono la stessa separazione delle problematiche che offrono controller e visualizzazioni e consente di eliminare la necessità di azioni e le viste per gestire i dati utilizzati dagli elementi dell'interfaccia utente comuni.</span><span class="sxs-lookup"><span data-stu-id="089d3-201">View components provide the same separation of concerns that controllers and views offer, and can eliminate the need for actions and views to deal with data used by common UI elements.</span></span>

<span data-ttu-id="089d3-202">Come molti altri aspetti di ASP.NET Core, viste supportano [inserimento di dipendenze](../../fundamentals/dependency-injection.md), consentendo di servizi da [inserite nelle viste](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="089d3-202">Like many other aspects of ASP.NET Core, views support [dependency injection](../../fundamentals/dependency-injection.md), allowing services to be [injected into views](dependency-injection.md).</span></span>
