---
title: Creare la prima app Razor Components
author: guardrex
description: Procedura dettagliata per creare un'app Razor Components e informazioni sui concetti di base di Razor Components.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: c0f7b27fdfc770f8001625ecb3bf8d50af517b99
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "57978423"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="51083-103">Creare la prima app Razor Components</span><span class="sxs-lookup"><span data-stu-id="51083-103">Build your first Razor Components app</span></span>

<span data-ttu-id="51083-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="51083-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="51083-105">Questa esercitazione illustra come creare un'app con Razor Components e offre una dimostrazione dei concetti di base di Razor Components.</span><span class="sxs-lookup"><span data-stu-id="51083-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="51083-106">È possibile seguire questa esercitazione usando sia un progetto basato su Razor Components (supportato in .NET Core 3.0 o versione successiva) che un progetto basato su Blazor (supportato in una versione futura di .NET Core).</span><span class="sxs-lookup"><span data-stu-id="51083-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="51083-107">Per un'esperienza con ASP.NET Core Razor Components (*consigliata*):</span><span class="sxs-lookup"><span data-stu-id="51083-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="51083-108">Seguire le indicazioni in <xref:razor-components/get-started> per creare un progetto basato su Razor Components.</span><span class="sxs-lookup"><span data-stu-id="51083-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="51083-109">Denominare il progetto `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="51083-109">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="51083-110">Per un'esperienza con Blazor:</span><span class="sxs-lookup"><span data-stu-id="51083-110">For an experience using Blazor:</span></span>

* <span data-ttu-id="51083-111">Seguire le indicazioni in <xref:spa/blazor/get-started> per creare un progetto basato su Blazor.</span><span class="sxs-lookup"><span data-stu-id="51083-111">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="51083-112">Denominare il progetto `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="51083-112">Name the project `Blazor`.</span></span>

<span data-ttu-id="51083-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="51083-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="51083-114">Vedere gli argomenti seguenti per i prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="51083-114">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="51083-115">Compilare i componenti</span><span class="sxs-lookup"><span data-stu-id="51083-115">Build components</span></span>

1. <span data-ttu-id="51083-116">Esplorare ognuna delle tre pagine dell'app nella cartella *Components/Pages* (*Pages* in Blazor): Home, Counter e Fetch data (Home, Contatore e Recupera dati).</span><span class="sxs-lookup"><span data-stu-id="51083-116">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="51083-117">Queste pagine vengono implementate dai file del componente Razor: *Index.Razor*, *Counter.razor* e *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="51083-117">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="51083-118">(Blazor continua a usare l'estensione file *cshtml*: *Index.cshtml*, *Counter.cshtml* e *FetchData.cshtml*.)</span><span class="sxs-lookup"><span data-stu-id="51083-118">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="51083-119">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="51083-119">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="51083-120">Per incrementare un contatore in una pagina Web è in genere richiesta la scrittura di codice JavaScript, ma Razor Components offre un approccio migliore usando C#.</span><span class="sxs-lookup"><span data-stu-id="51083-120">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="51083-121">Esaminare l'implementazione del componente Counter nel file *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="51083-121">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="51083-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="51083-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="51083-123">L'interfaccia utente del componente Counter viene definita tramite codice HTML.</span><span class="sxs-lookup"><span data-stu-id="51083-123">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="51083-124">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="51083-124">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="51083-125">Il markup HTML e la logica di rendering C# vengono convertiti un una classe del componente in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="51083-125">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="51083-126">Il nome della classe .NET generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="51083-126">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="51083-127">I membri della classe del componente sono definiti in un blocco `@functions`.</span><span class="sxs-lookup"><span data-stu-id="51083-127">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="51083-128">Nel blocco `@functions`, lo stato del componente (proprietà, campi) e i metodi vengono specificati per la gestione degli eventi o per definire la logica di altri componenti.</span><span class="sxs-lookup"><span data-stu-id="51083-128">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="51083-129">Questi membri vengono quindi usati come parte della logica di rendering del componente e per la gestione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="51083-129">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="51083-130">Quando viene selezionato il pulsante **Click me**:</span><span class="sxs-lookup"><span data-stu-id="51083-130">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="51083-131">Viene chiamato il gestore `onclick` registrato del componente Counter (metodo `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="51083-131">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="51083-132">Il componente Counter rigenera il relativo albero di rendering.</span><span class="sxs-lookup"><span data-stu-id="51083-132">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="51083-133">Il nuovo albero di rendering viene confrontato con quello precedente.</span><span class="sxs-lookup"><span data-stu-id="51083-133">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="51083-134">Vengono applicate solo le modifiche al modello DOM (Document Object Model).</span><span class="sxs-lookup"><span data-stu-id="51083-134">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="51083-135">Il conteggio visualizzato viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="51083-135">The displayed count is updated.</span></span>

1. <span data-ttu-id="51083-136">Modificare la logica C# del componente Counter per incrementare il contatore di due unità anziché una.</span><span class="sxs-lookup"><span data-stu-id="51083-136">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="51083-137">Ricompilare ed eseguire l'app per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="51083-137">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="51083-138">Selezionare il pulsante **Click me** e il contatore viene incrementato di due unità.</span><span class="sxs-lookup"><span data-stu-id="51083-138">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="51083-139">Usare i componenti</span><span class="sxs-lookup"><span data-stu-id="51083-139">Use components</span></span>

<span data-ttu-id="51083-140">Includere un componente in un altro componente usando una sintassi simile a HTML.</span><span class="sxs-lookup"><span data-stu-id="51083-140">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="51083-141">Aggiungere il componente Counter al componente Index (home page) dell'app aggiungendo un elemento `<Counter />` al componente Index.</span><span class="sxs-lookup"><span data-stu-id="51083-141">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="51083-142">Se si usa Blazor per questa esperienza, il componente Index include un componente Survey Prompt (elemento `<SurveyPrompt>`).</span><span class="sxs-lookup"><span data-stu-id="51083-142">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="51083-143">Sostituire l'elemento `<SurveyPrompt>` con l'elemento `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="51083-143">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="51083-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="51083-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="51083-145">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="51083-145">Rebuild and run the app.</span></span> <span data-ttu-id="51083-146">La home page ha un contatore proprio.</span><span class="sxs-lookup"><span data-stu-id="51083-146">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="51083-147">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="51083-147">Component parameters</span></span>

<span data-ttu-id="51083-148">I componenti possono avere anche parametri,</span><span class="sxs-lookup"><span data-stu-id="51083-148">Components can also have parameters.</span></span> <span data-ttu-id="51083-149">che vengono definiti usando proprietà non pubbliche nella classe del componente decorata con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="51083-149">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="51083-150">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="51083-150">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="51083-151">Aggiornare il codice C# `@functions` del componente:</span><span class="sxs-lookup"><span data-stu-id="51083-151">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="51083-152">Aggiungere una proprietà `IncrementAmount` decorata con l'attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="51083-152">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="51083-153">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="51083-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="51083-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="51083-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="51083-155">Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Home usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="51083-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="51083-156">Impostare il valore per incrementare il contatore di dieci unità.</span><span class="sxs-lookup"><span data-stu-id="51083-156">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="51083-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="51083-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="51083-158">Ricaricare la pagina.</span><span class="sxs-lookup"><span data-stu-id="51083-158">Reload the page.</span></span> <span data-ttu-id="51083-159">Il contatore della home page viene incrementato di dieci unità ogni volta che viene selezionato il pulsante **Click me**.</span><span class="sxs-lookup"><span data-stu-id="51083-159">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="51083-160">Il contatore nella pagina *Counter* viene incrementato di un'unità.</span><span class="sxs-lookup"><span data-stu-id="51083-160">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="51083-161">Indirizzare le richieste ai componenti</span><span class="sxs-lookup"><span data-stu-id="51083-161">Route to components</span></span>

<span data-ttu-id="51083-162">La direttiva `@page` all'inizio del file *Counter.razor* specifica che questo componente è un endpoint di routing.</span><span class="sxs-lookup"><span data-stu-id="51083-162">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="51083-163">Il componente Counter gestisce le richieste inviate a `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="51083-163">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="51083-164">Senza la direttiva `@page`, il componente non gestisce le richieste indirizzate, ma può ancora essere usato da altri componenti.</span><span class="sxs-lookup"><span data-stu-id="51083-164">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="51083-165">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="51083-165">Dependency injection</span></span>

<span data-ttu-id="51083-166">I servizi registrati nel contenitore del servizio dell'app sono disponibili per i componenti tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="51083-166">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="51083-167">Inserire i servizi in un componente usando la direttiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="51083-167">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="51083-168">Esaminare le direttive del componente FetchData.</span><span class="sxs-lookup"><span data-stu-id="51083-168">Examine the directives of the FetchData component.</span></span> <span data-ttu-id="51083-169">La direttiva `@inject` viene usata per inserire l'istanza del servizio `WeatherForecastService` nel componente:</span><span class="sxs-lookup"><span data-stu-id="51083-169">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

<span data-ttu-id="51083-170">*Components/Pages/FetchData.razor* (*Pages/FetchData.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="51083-170">*Components/Pages/FetchData.razor* (*Pages/FetchData.cshtml* in Blazor):</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="51083-171">Il servizio `WeatherForecastService` viene registrato come [singleton](xref:fundamentals/dependency-injection#service-lifetimes), in modo che un'istanza del servizio sia disponibile per tutta l'app.</span><span class="sxs-lookup"><span data-stu-id="51083-171">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="51083-172">Il componente FetchData usa il servizio inserito, come `ForecastService`, per recuperare una matrice di oggetti `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="51083-172">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="51083-173">Viene usato un ciclo [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) per eseguire il rendering di ogni istanza di previsione come una riga nella tabella dei dati meteo:</span><span class="sxs-lookup"><span data-stu-id="51083-173">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="51083-174">Compilare un elenco attività</span><span class="sxs-lookup"><span data-stu-id="51083-174">Build a todo list</span></span>

<span data-ttu-id="51083-175">Aggiungere una nuova pagina all'app che implementa un semplice elenco attività.</span><span class="sxs-lookup"><span data-stu-id="51083-175">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="51083-176">Aggiungere un file vuoto nella cartella *Components/Pages* (cartella *Pages* in Blazor) denominato *Todo.razor*.</span><span class="sxs-lookup"><span data-stu-id="51083-176">Add an empty file to the *Components/Pages* folder (*Pages* folder in Blazor) named *Todo.razor*.</span></span>

1. <span data-ttu-id="51083-177">Specificare il markup iniziale per la pagina:</span><span class="sxs-lookup"><span data-stu-id="51083-177">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="51083-178">Aggiungere la pagina Todo alla barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="51083-178">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="51083-179">Il componente NavMenu (*Components/Shared/NavMenu.razor* oppure *Shared/NavMenu.cshtml* in Blazor) viene usato nel layout dell'app.</span><span class="sxs-lookup"><span data-stu-id="51083-179">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="51083-180">I layout sono componenti che consentono di evitare la duplicazione del contenuto nell'app.</span><span class="sxs-lookup"><span data-stu-id="51083-180">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="51083-181">Per ulteriori informazioni, vedere <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="51083-181">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="51083-182">Aggiungere un `<NavLink>` per la pagina Todo aggiungendo il markup seguente per l'elemento di elenco sotto gli elementi di elenco esistenti nel file *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="51083-182">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="51083-183">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="51083-183">Rebuild and run the app.</span></span> <span data-ttu-id="51083-184">Visitare la nuova pagina Todo per confermare che il collegamento alla pagina Todo funziona.</span><span class="sxs-lookup"><span data-stu-id="51083-184">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="51083-185">Aggiungere un file *TodoItem.cs* nella radice del progetto per contenere una classe per rappresentare un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="51083-185">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="51083-186">Usare il codice C# seguente per la classe `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="51083-186">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="51083-187">Tornare al componente Todo (*Components/Pages/Todo.razor* oppure *Pages/Todo.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="51083-187">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="51083-188">Aggiungere un campo per le attività in un blocco `@functions`.</span><span class="sxs-lookup"><span data-stu-id="51083-188">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="51083-189">Il componente Todo usa questo campo per mantenere lo stato dell'elenco attività.</span><span class="sxs-lookup"><span data-stu-id="51083-189">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="51083-190">Aggiungere il markup per l'elenco non ordinato e un ciclo `foreach` per eseguire il rendering di ogni elemento attività come elemento di elenco.</span><span class="sxs-lookup"><span data-stu-id="51083-190">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="51083-191">L'app richiede elementi dell'interfaccia utente per l'aggiunta di attività all'elenco.</span><span class="sxs-lookup"><span data-stu-id="51083-191">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="51083-192">Aggiungere un input di testo e un pulsante sotto l'elenco:</span><span class="sxs-lookup"><span data-stu-id="51083-192">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="51083-193">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="51083-193">Rebuild and run the app.</span></span> <span data-ttu-id="51083-194">Quando viene selezionato il pulsante **Add todo** non accade nulla perché al pulsante non è associato un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="51083-194">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="51083-195">Aggiungere un metodo `AddTodo` al componente Todo e registrarlo per i clic sul pulsante con l'attributo `onclick`:</span><span class="sxs-lookup"><span data-stu-id="51083-195">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="51083-196">Il metodo C# `AddTodo` viene chiamato quando viene selezionato il pulsante.</span><span class="sxs-lookup"><span data-stu-id="51083-196">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="51083-197">Per ottenere il titolo del nuovo elemento attività, aggiungere un campo stringa `newTodo` e associarlo al valore dell'input di testo tramite l'attributo `bind`:</span><span class="sxs-lookup"><span data-stu-id="51083-197">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="51083-198">Aggiornare il metodo `AddTodo` per aggiungere l'elemento `TodoItem` con il titolo specificato all'elenco.</span><span class="sxs-lookup"><span data-stu-id="51083-198">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="51083-199">Cancellare il valore dell'input di testo impostando `newTodo` su una stringa vuota:</span><span class="sxs-lookup"><span data-stu-id="51083-199">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="51083-200">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="51083-200">Rebuild and run the app.</span></span> <span data-ttu-id="51083-201">Aggiungere alcune attività all'elenco attività per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="51083-201">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="51083-202">Il testo del titolo per ogni elemento attività può essere reso modificabile e una casella di controllo può consentire all'utente di tenere traccia degli elementi completati.</span><span class="sxs-lookup"><span data-stu-id="51083-202">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="51083-203">Aggiungere un input casella di controllo per ogni elemento attività e associarne il valore alla proprietà `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="51083-203">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="51083-204">Modificare `@todo.Title` in un elemento `<input>` associato a `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="51083-204">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="51083-205">Per verificare che questi valori siano associati, aggiornare l'intestazione `<h1>` per visualizzare un conteggio del numero di elementi attività non completati (`IsDone` è `false`).</span><span class="sxs-lookup"><span data-stu-id="51083-205">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="51083-206">Il componente Todo completato (*Components/Pages/Todo.razor* oppure *Pages/Todo.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="51083-206">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="51083-207">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="51083-207">Rebuild and run the app.</span></span> <span data-ttu-id="51083-208">Aggiungere elementi attività per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="51083-208">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="51083-209">Pubblicare e distribuire l'app</span><span class="sxs-lookup"><span data-stu-id="51083-209">Publish and deploy the app</span></span>

<span data-ttu-id="51083-210">Per pubblicare l'app, vedere <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="51083-210">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
