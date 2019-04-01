---
title: Creare la prima app Razor Components
author: guardrex
description: Procedura dettagliata per creare un'app Razor Components e informazioni sui concetti di base di Razor Components.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/24/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 2a987b3f2e687cd9d4dffa2c573c938e68ea3cc8
ms.sourcegitcommit: 7d6019f762fc5b8cbedcd69801e8310f51a17c18
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419365"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="52a4b-103">Creare la prima app Razor Components</span><span class="sxs-lookup"><span data-stu-id="52a4b-103">Build your first Razor Components app</span></span>

<span data-ttu-id="52a4b-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="52a4b-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="52a4b-105">Questa esercitazione illustra come creare un'app con Razor Components e offre una dimostrazione dei concetti di base di Razor Components.</span><span class="sxs-lookup"><span data-stu-id="52a4b-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="52a4b-106">È possibile seguire questa esercitazione usando sia un progetto basato su Razor Components (supportato in .NET Core 3.0 o versione successiva) che un progetto basato su Blazor (supportato in una versione futura di .NET Core).</span><span class="sxs-lookup"><span data-stu-id="52a4b-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="52a4b-107">Per un'esperienza con ASP.NET Core Razor Components (*consigliata*):</span><span class="sxs-lookup"><span data-stu-id="52a4b-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="52a4b-108">Seguire le indicazioni in <xref:razor-components/get-started> per creare un progetto basato su Razor Components.</span><span class="sxs-lookup"><span data-stu-id="52a4b-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="52a4b-109">Denominare il progetto `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="52a4b-109">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="52a4b-110">Per un'esperienza con Blazor:</span><span class="sxs-lookup"><span data-stu-id="52a4b-110">For an experience using Blazor:</span></span>

* <span data-ttu-id="52a4b-111">Seguire le indicazioni in <xref:spa/blazor/get-started> per creare un progetto basato su Blazor.</span><span class="sxs-lookup"><span data-stu-id="52a4b-111">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="52a4b-112">Denominare il progetto `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="52a4b-112">Name the project `Blazor`.</span></span>

<span data-ttu-id="52a4b-113">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="52a4b-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="52a4b-114">Vedere gli argomenti seguenti per i prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="52a4b-114">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="52a4b-115">Compilare i componenti</span><span class="sxs-lookup"><span data-stu-id="52a4b-115">Build components</span></span>

1. <span data-ttu-id="52a4b-116">Esplorare ognuna delle tre pagine dell'app nella cartella *Components/Pages* (*Pages* in Blazor): Home, Counter e Fetch data (Home, Contatore e Recupera dati).</span><span class="sxs-lookup"><span data-stu-id="52a4b-116">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="52a4b-117">Queste pagine vengono implementate dai file del componente Razor: *Index.Razor*, *Counter.razor* e *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="52a4b-117">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="52a4b-118">(Blazor continua a usare l'estensione file *cshtml*: *Index.cshtml*, *Counter.cshtml* e *FetchData.cshtml*.)</span><span class="sxs-lookup"><span data-stu-id="52a4b-118">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="52a4b-119">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="52a4b-119">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="52a4b-120">Per incrementare un contatore in una pagina Web è in genere richiesta la scrittura di codice JavaScript, ma Razor Components offre un approccio migliore usando C#.</span><span class="sxs-lookup"><span data-stu-id="52a4b-120">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="52a4b-121">Esaminare l'implementazione del componente Counter nel file *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="52a4b-121">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="52a4b-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="52a4b-122">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="52a4b-123">L'interfaccia utente del componente Counter viene definita tramite codice HTML.</span><span class="sxs-lookup"><span data-stu-id="52a4b-123">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="52a4b-124">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="52a4b-124">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="52a4b-125">Il markup HTML e la logica di rendering C# vengono convertiti un una classe del componente in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="52a4b-125">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="52a4b-126">Il nome della classe .NET generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="52a4b-126">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="52a4b-127">I membri della classe del componente sono definiti in un blocco `@functions`.</span><span class="sxs-lookup"><span data-stu-id="52a4b-127">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="52a4b-128">Nel blocco `@functions`, lo stato del componente (proprietà, campi) e i metodi vengono specificati per la gestione degli eventi o per definire la logica di altri componenti.</span><span class="sxs-lookup"><span data-stu-id="52a4b-128">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="52a4b-129">Questi membri vengono quindi usati come parte della logica di rendering del componente e per la gestione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="52a4b-129">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="52a4b-130">Quando viene selezionato il pulsante **Click me**:</span><span class="sxs-lookup"><span data-stu-id="52a4b-130">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="52a4b-131">Viene chiamato il gestore `onclick` registrato del componente Counter (metodo `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="52a4b-131">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="52a4b-132">Il componente Counter rigenera il relativo albero di rendering.</span><span class="sxs-lookup"><span data-stu-id="52a4b-132">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="52a4b-133">Il nuovo albero di rendering viene confrontato con quello precedente.</span><span class="sxs-lookup"><span data-stu-id="52a4b-133">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="52a4b-134">Vengono applicate solo le modifiche al modello DOM (Document Object Model).</span><span class="sxs-lookup"><span data-stu-id="52a4b-134">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="52a4b-135">Il conteggio visualizzato viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="52a4b-135">The displayed count is updated.</span></span>

1. <span data-ttu-id="52a4b-136">Modificare la logica C# del componente Counter per incrementare il contatore di due unità anziché una.</span><span class="sxs-lookup"><span data-stu-id="52a4b-136">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="52a4b-137">Ricompilare ed eseguire l'app per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="52a4b-137">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="52a4b-138">Selezionare il pulsante **Click me** e il contatore viene incrementato di due unità.</span><span class="sxs-lookup"><span data-stu-id="52a4b-138">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="52a4b-139">Usare i componenti</span><span class="sxs-lookup"><span data-stu-id="52a4b-139">Use components</span></span>

<span data-ttu-id="52a4b-140">Includere un componente in un altro componente usando una sintassi simile a HTML.</span><span class="sxs-lookup"><span data-stu-id="52a4b-140">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="52a4b-141">Aggiungere il componente Counter al componente Index (Home) dell'app aggiungendo un elemento `<Counter />` al componente Index.</span><span class="sxs-lookup"><span data-stu-id="52a4b-141">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="52a4b-142">Se si usa Blazor per questa esperienza, il componente Index include un componente Survey Prompt (elemento `<SurveyPrompt>`).</span><span class="sxs-lookup"><span data-stu-id="52a4b-142">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="52a4b-143">Sostituire l'elemento `<SurveyPrompt>` con l'elemento `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="52a4b-143">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="52a4b-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="52a4b-144">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="52a4b-145">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="52a4b-145">Rebuild and run the app.</span></span> <span data-ttu-id="52a4b-146">La home page ha un contatore proprio.</span><span class="sxs-lookup"><span data-stu-id="52a4b-146">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="52a4b-147">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="52a4b-147">Component parameters</span></span>

<span data-ttu-id="52a4b-148">I componenti possono avere anche parametri,</span><span class="sxs-lookup"><span data-stu-id="52a4b-148">Components can also have parameters.</span></span> <span data-ttu-id="52a4b-149">che vengono definiti usando proprietà non pubbliche nella classe del componente decorata con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="52a4b-149">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="52a4b-150">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="52a4b-150">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="52a4b-151">Aggiornare il codice C# `@functions` del componente:</span><span class="sxs-lookup"><span data-stu-id="52a4b-151">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="52a4b-152">Aggiungere una proprietà `IncrementAmount` decorata con l'attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="52a4b-152">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="52a4b-153">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="52a4b-153">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="52a4b-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="52a4b-154">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="52a4b-155">Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Home usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="52a4b-155">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="52a4b-156">Impostare il valore per incrementare il contatore di dieci unità.</span><span class="sxs-lookup"><span data-stu-id="52a4b-156">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="52a4b-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="52a4b-157">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="52a4b-158">Ricaricare la home page.</span><span class="sxs-lookup"><span data-stu-id="52a4b-158">Reload the Home page.</span></span> <span data-ttu-id="52a4b-159">Il contatore viene incrementato di dieci unità ogni volta che viene selezionato il pulsante **Click me**.</span><span class="sxs-lookup"><span data-stu-id="52a4b-159">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="52a4b-160">Il contatore nella pagina Counter viene incrementato di un'unità.</span><span class="sxs-lookup"><span data-stu-id="52a4b-160">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="52a4b-161">Indirizzare le richieste ai componenti</span><span class="sxs-lookup"><span data-stu-id="52a4b-161">Route to components</span></span>

<span data-ttu-id="52a4b-162">La direttiva `@page` all'inizio del file *Counter.razor* specifica che questo componente è un endpoint di routing.</span><span class="sxs-lookup"><span data-stu-id="52a4b-162">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="52a4b-163">Il componente Counter gestisce le richieste inviate a `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="52a4b-163">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="52a4b-164">Senza la direttiva `@page`, il componente non gestisce le richieste indirizzate, ma può ancora essere usato da altri componenti.</span><span class="sxs-lookup"><span data-stu-id="52a4b-164">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="52a4b-165">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="52a4b-165">Dependency injection</span></span>

<span data-ttu-id="52a4b-166">I servizi registrati nel contenitore del servizio dell'app sono disponibili per i componenti tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="52a4b-166">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="52a4b-167">Inserire i servizi in un componente usando la direttiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="52a4b-167">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="52a4b-168">Esaminare le direttive del componente FetchData nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="52a4b-168">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="52a4b-169">Nell'app di esempio Razor Components il servizio `WeatherForecastService` viene registrato come [singleton](xref:fundamentals/dependency-injection#service-lifetimes), in modo che un'istanza del servizio sia disponibile nell'intera app.</span><span class="sxs-lookup"><span data-stu-id="52a4b-169">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="52a4b-170">La direttiva `@inject` viene usata per inserire l'istanza del servizio `WeatherForecastService` nel componente.</span><span class="sxs-lookup"><span data-stu-id="52a4b-170">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="52a4b-171">*Components/Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="52a4b-171">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="52a4b-172">Il componente FetchData usa il servizio inserito, come `ForecastService`, per recuperare una matrice di oggetti `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="52a4b-172">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="52a4b-173">Nella versione Blazor dell'app di esempio `HttpClient` viene inserito per ottenere i dati delle previsioni meteo dal file *weather.json* nella cartella *wwwroot/sample-data*:</span><span class="sxs-lookup"><span data-stu-id="52a4b-173">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="52a4b-174">*Pages/FetchData.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="52a4b-174">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="52a4b-175">In entrambe le app di esempio viene usato un ciclo [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) per eseguire il rendering di ogni istanza di previsione come una riga della tabella dei dati meteo:</span><span class="sxs-lookup"><span data-stu-id="52a4b-175">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="52a4b-176">Compilare un elenco attività</span><span class="sxs-lookup"><span data-stu-id="52a4b-176">Build a todo list</span></span>

<span data-ttu-id="52a4b-177">Aggiungere all'app un nuovo componente che implementa un semplice elenco attività.</span><span class="sxs-lookup"><span data-stu-id="52a4b-177">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="52a4b-178">Aggiungere un file vuoto all'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="52a4b-178">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="52a4b-179">Per l'esperienza Razor Components aggiungere un file *Todo.razor* alla cartella *Components/Pages*.</span><span class="sxs-lookup"><span data-stu-id="52a4b-179">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="52a4b-180">Per l'esperienza Blazor aggiungere un file *Todo.cshtml* alla cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="52a4b-180">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="52a4b-181">Specificare il markup iniziale per il componente:</span><span class="sxs-lookup"><span data-stu-id="52a4b-181">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="52a4b-182">Aggiungere il componente Todo alla barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="52a4b-182">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="52a4b-183">Il componente NavMenu (*Components/Shared/NavMenu.razor* oppure *Shared/NavMenu.cshtml* in Blazor) viene usato nel layout dell'app.</span><span class="sxs-lookup"><span data-stu-id="52a4b-183">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="52a4b-184">I layout sono componenti che consentono di evitare la duplicazione del contenuto nell'app.</span><span class="sxs-lookup"><span data-stu-id="52a4b-184">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="52a4b-185">Per ulteriori informazioni, vedere <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="52a4b-185">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="52a4b-186">Aggiungere un elemento `<NavLink>` per il componente Todo aggiungendo il markup seguente per l'elemento di elenco sotto gli elementi di elenco esistenti nel file *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="52a4b-186">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="52a4b-187">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="52a4b-187">Rebuild and run the app.</span></span> <span data-ttu-id="52a4b-188">Visitare la nuova pagina Todo per confermare che il collegamento al componente Todo funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="52a4b-188">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="52a4b-189">Aggiungere un file *TodoItem.cs* nella radice del progetto per contenere una classe per rappresentare un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="52a4b-189">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="52a4b-190">Usare il codice C# seguente per la classe `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="52a4b-190">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="52a4b-191">Tornare al componente Todo (*Components/Pages/Todo.razor* oppure *Pages/Todo.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="52a4b-191">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="52a4b-192">Aggiungere un campo per le attività in un blocco `@functions`.</span><span class="sxs-lookup"><span data-stu-id="52a4b-192">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="52a4b-193">Il componente Todo usa questo campo per mantenere lo stato dell'elenco attività.</span><span class="sxs-lookup"><span data-stu-id="52a4b-193">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="52a4b-194">Aggiungere il markup per l'elenco non ordinato e un ciclo `foreach` per eseguire il rendering di ogni elemento attività come elemento di elenco.</span><span class="sxs-lookup"><span data-stu-id="52a4b-194">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="52a4b-195">L'app richiede elementi dell'interfaccia utente per l'aggiunta di attività all'elenco.</span><span class="sxs-lookup"><span data-stu-id="52a4b-195">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="52a4b-196">Aggiungere un input di testo e un pulsante sotto l'elenco:</span><span class="sxs-lookup"><span data-stu-id="52a4b-196">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="52a4b-197">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="52a4b-197">Rebuild and run the app.</span></span> <span data-ttu-id="52a4b-198">Quando viene selezionato il pulsante **Add todo** non accade nulla perché al pulsante non è associato un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="52a4b-198">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="52a4b-199">Aggiungere un metodo `AddTodo` al componente Todo e registrarlo per i clic sul pulsante con l'attributo `onclick`:</span><span class="sxs-lookup"><span data-stu-id="52a4b-199">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="52a4b-200">Il metodo C# `AddTodo` viene chiamato quando viene selezionato il pulsante.</span><span class="sxs-lookup"><span data-stu-id="52a4b-200">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="52a4b-201">Per ottenere il titolo del nuovo elemento attività, aggiungere un campo stringa `newTodo` e associarlo al valore dell'input di testo tramite l'attributo `bind`:</span><span class="sxs-lookup"><span data-stu-id="52a4b-201">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="52a4b-202">Aggiornare il metodo `AddTodo` per aggiungere l'elemento `TodoItem` con il titolo specificato all'elenco.</span><span class="sxs-lookup"><span data-stu-id="52a4b-202">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="52a4b-203">Cancellare il valore dell'input di testo impostando `newTodo` su una stringa vuota:</span><span class="sxs-lookup"><span data-stu-id="52a4b-203">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="52a4b-204">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="52a4b-204">Rebuild and run the app.</span></span> <span data-ttu-id="52a4b-205">Aggiungere alcune attività all'elenco attività per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="52a4b-205">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="52a4b-206">Il testo del titolo per ogni elemento attività può essere reso modificabile e una casella di controllo può consentire all'utente di tenere traccia degli elementi completati.</span><span class="sxs-lookup"><span data-stu-id="52a4b-206">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="52a4b-207">Aggiungere un input casella di controllo per ogni elemento attività e associarne il valore alla proprietà `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="52a4b-207">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="52a4b-208">Modificare `@todo.Title` in un elemento `<input>` associato a `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="52a4b-208">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="52a4b-209">Per verificare che questi valori siano associati, aggiornare l'intestazione `<h1>` per visualizzare un conteggio del numero di elementi attività non completati (`IsDone` è `false`).</span><span class="sxs-lookup"><span data-stu-id="52a4b-209">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="52a4b-210">Il componente Todo completato (*Components/Pages/Todo.razor* oppure *Pages/Todo.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="52a4b-210">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="52a4b-211">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="52a4b-211">Rebuild and run the app.</span></span> <span data-ttu-id="52a4b-212">Aggiungere elementi attività per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="52a4b-212">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="52a4b-213">Pubblicare e distribuire l'app</span><span class="sxs-lookup"><span data-stu-id="52a4b-213">Publish and deploy the app</span></span>

<span data-ttu-id="52a4b-214">Per pubblicare l'app, vedere <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="52a4b-214">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
