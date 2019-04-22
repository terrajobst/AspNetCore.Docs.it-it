---
title: Creare la prima app Blazor
author: guardrex
description: Istruzioni dettagliate per creare un'app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 310eb211f68076d6f52d6427940e07736d833e5b
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614710"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="f4a4a-103">Creare la prima app Blazor</span><span class="sxs-lookup"><span data-stu-id="f4a4a-103">Build your first Blazor app</span></span>

<span data-ttu-id="f4a4a-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f4a4a-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="f4a4a-105">Questa esercitazione illustra come creare un'app con Razor Components sul lato server o Blazor sul lato client.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-105">This tutorial shows you how to build an app with server-side Razor Components or client-side Blazor.</span></span>

<span data-ttu-id="f4a4a-106">Per un'esperienza con ASP.NET Core Razor Components sul lato server (*consigliata*):</span><span class="sxs-lookup"><span data-stu-id="f4a4a-106">For an experience using server-side ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="f4a4a-107">Seguire le indicazioni per l'*esperienza con Razor Components* in <xref:blazor/get-started#server-side-razor-components-experience> per creare un progetto basato su Razor Components.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-107">Follow the *Razor Components experience* guidance in <xref:blazor/get-started#server-side-razor-components-experience> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="f4a4a-108">Denominare il progetto `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-108">Name the project `RazorComponents`.</span></span>

<span data-ttu-id="f4a4a-109">Per un'esperienza con Blazor:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-109">For an experience using Blazor:</span></span>

* <span data-ttu-id="f4a4a-110">Seguire le indicazioni per l'*esperienza con Blazor* in <xref:blazor/get-started#client-side-blazor-experience> per creare un progetto basato su Blazor.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-110">Follow the *Blazor experience* guidance in <xref:blazor/get-started#client-side-blazor-experience> to create a Blazor-based project.</span></span>
* <span data-ttu-id="f4a4a-111">Denominare il progetto `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-111">Name the project `Blazor`.</span></span>

<span data-ttu-id="f4a4a-112">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f4a4a-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="f4a4a-113">Vedere gli argomenti seguenti per i prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-113">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="f4a4a-114">Compilare i componenti</span><span class="sxs-lookup"><span data-stu-id="f4a4a-114">Build components</span></span>

1. <span data-ttu-id="f4a4a-115">Esplorare ognuna delle tre pagine dell'app nella cartella *Components/Pages* (*Pages* in Blazor): Home, Counter e Fetch data (Home, Contatore e Recupera dati).</span><span class="sxs-lookup"><span data-stu-id="f4a4a-115">Browse to each of the app's three pages in the *Components/Pages* folder (*Pages* in Blazor): Home, Counter, and Fetch data.</span></span> <span data-ttu-id="f4a4a-116">Queste pagine vengono implementate dai file del componente Razor: *Index.Razor*, *Counter.razor* e *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-116">These pages are implemented by Razor Component files: *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span> <span data-ttu-id="f4a4a-117">(Blazor continua a usare l'estensione file *cshtml*: *Index.cshtml*, *Counter.cshtml* e *FetchData.cshtml*.)</span><span class="sxs-lookup"><span data-stu-id="f4a4a-117">(Blazor continues to use the *.cshtml* file extension: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.)</span></span>

1. <span data-ttu-id="f4a4a-118">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-118">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="f4a4a-119">Per incrementare un contatore in una pagina Web è in genere richiesta la scrittura di codice JavaScript, ma Blazor offre un approccio migliore usando C#.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-119">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="f4a4a-120">Esaminare l'implementazione del componente Counter nel file *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-120">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="f4a4a-121">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="f4a4a-121">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="f4a4a-122">L'interfaccia utente del componente Counter viene definita tramite codice HTML.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-122">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="f4a4a-123">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="f4a4a-123">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="f4a4a-124">Il markup HTML e la logica di rendering C# vengono convertiti un una classe del componente in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-124">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="f4a4a-125">Il nome della classe .NET generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-125">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="f4a4a-126">I membri della classe del componente sono definiti in un blocco `@functions`.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-126">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="f4a4a-127">Nel blocco `@functions`, lo stato del componente (proprietà, campi) e i metodi vengono specificati per la gestione degli eventi o per definire la logica di altri componenti.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-127">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="f4a4a-128">Questi membri vengono quindi usati come parte della logica di rendering del componente e per la gestione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-128">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="f4a4a-129">Quando viene selezionato il pulsante **Click me**:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-129">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="f4a4a-130">Viene chiamato il gestore `onclick` registrato del componente Counter (metodo `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="f4a4a-130">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="f4a4a-131">Il componente Counter rigenera il relativo albero di rendering.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-131">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="f4a4a-132">Il nuovo albero di rendering viene confrontato con quello precedente.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-132">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="f4a4a-133">Vengono applicate solo le modifiche al modello DOM (Document Object Model).</span><span class="sxs-lookup"><span data-stu-id="f4a4a-133">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="f4a4a-134">Il conteggio visualizzato viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-134">The displayed count is updated.</span></span>

1. <span data-ttu-id="f4a4a-135">Modificare la logica C# del componente Counter per incrementare il contatore di due unità anziché una.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-135">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="f4a4a-136">Ricompilare ed eseguire l'app per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-136">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="f4a4a-137">Selezionare il pulsante **Click me** e il contatore viene incrementato di due unità.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-137">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="f4a4a-138">Usare i componenti</span><span class="sxs-lookup"><span data-stu-id="f4a4a-138">Use components</span></span>

<span data-ttu-id="f4a4a-139">Includere un componente in un altro componente usando una sintassi simile a HTML.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-139">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="f4a4a-140">Aggiungere il componente Counter al componente Index (Home) dell'app aggiungendo un elemento `<Counter />` al componente Index.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-140">Add the Counter component to the app's Index (Home) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="f4a4a-141">Se si usa Blazor per questa esperienza, il componente Index include un componente Survey Prompt (elemento `<SurveyPrompt>`).</span><span class="sxs-lookup"><span data-stu-id="f4a4a-141">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="f4a4a-142">Sostituire l'elemento `<SurveyPrompt>` con l'elemento `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-142">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="f4a4a-143">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="f4a4a-143">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. <span data-ttu-id="f4a4a-144">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-144">Rebuild and run the app.</span></span> <span data-ttu-id="f4a4a-145">La home page ha un contatore proprio.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-145">The Home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="f4a4a-146">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="f4a4a-146">Component parameters</span></span>

<span data-ttu-id="f4a4a-147">I componenti possono avere anche parametri,</span><span class="sxs-lookup"><span data-stu-id="f4a4a-147">Components can also have parameters.</span></span> <span data-ttu-id="f4a4a-148">che vengono definiti usando proprietà non pubbliche nella classe del componente decorata con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-148">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="f4a4a-149">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-149">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="f4a4a-150">Aggiornare il codice C# `@functions` del componente:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-150">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="f4a4a-151">Aggiungere una proprietà `IncrementAmount` decorata con l'attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-151">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="f4a4a-152">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-152">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="f4a4a-153">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="f4a4a-153">*Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="f4a4a-154">Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Home usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-154">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="f4a4a-155">Impostare il valore per incrementare il contatore di dieci unità.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-155">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="f4a4a-156">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="f4a4a-156">*Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. <span data-ttu-id="f4a4a-157">Ricaricare la home page.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-157">Reload the Home page.</span></span> <span data-ttu-id="f4a4a-158">Il contatore viene incrementato di dieci unità ogni volta che viene selezionato il pulsante **Click me**.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-158">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="f4a4a-159">Il contatore nella pagina Counter viene incrementato di un'unità.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-159">The counter on the Counter page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="f4a4a-160">Indirizzare le richieste ai componenti</span><span class="sxs-lookup"><span data-stu-id="f4a4a-160">Route to components</span></span>

<span data-ttu-id="f4a4a-161">La direttiva `@page` all'inizio del file *Counter.razor* specifica che questo componente è un endpoint di routing.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-161">The `@page` directive at the top of the *Counter.razor* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="f4a4a-162">Il componente Counter gestisce le richieste inviate a `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-162">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="f4a4a-163">Senza la direttiva `@page`, il componente non gestisce le richieste indirizzate, ma può ancora essere usato da altri componenti.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-163">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="f4a4a-164">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="f4a4a-164">Dependency injection</span></span>

<span data-ttu-id="f4a4a-165">I servizi registrati nel contenitore del servizio dell'app sono disponibili per i componenti tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f4a4a-165">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="f4a4a-166">Inserire i servizi in un componente usando la direttiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-166">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="f4a4a-167">Esaminare le direttive del componente FetchData nell'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-167">Examine the directives of the FetchData component in the sample app.</span></span>

<span data-ttu-id="f4a4a-168">Nell'app di esempio Razor Components il servizio `WeatherForecastService` viene registrato come [singleton](xref:fundamentals/dependency-injection#service-lifetimes), in modo che un'istanza del servizio sia disponibile nell'intera app.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-168">In the Razor Components sample app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="f4a4a-169">La direttiva `@inject` viene usata per inserire l'istanza del servizio `WeatherForecastService` nel componente.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-169">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="f4a4a-170">*Components/Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-170">*Components/Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="f4a4a-171">Il componente FetchData usa il servizio inserito, come `ForecastService`, per recuperare una matrice di oggetti `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-171">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="f4a4a-172">Nella versione Blazor dell'app di esempio `HttpClient` viene inserito per ottenere i dati delle previsioni meteo dal file *weather.json* nella cartella *wwwroot/sample-data*:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-172">In the Blazor version of the sample app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="f4a4a-173">*Pages/FetchData.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-173">*Pages/FetchData.cshtml*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

<span data-ttu-id="f4a4a-174">In entrambe le app di esempio viene usato un ciclo [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) per eseguire il rendering di ogni istanza di previsione come una riga della tabella dei dati meteo:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-174">In both sample apps, a [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="f4a4a-175">Compilare un elenco attività</span><span class="sxs-lookup"><span data-stu-id="f4a4a-175">Build a todo list</span></span>

<span data-ttu-id="f4a4a-176">Aggiungere all'app un nuovo componente che implementa un semplice elenco attività.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-176">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="f4a4a-177">Aggiungere un file vuoto all'app di esempio:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-177">Add an empty file to the sample app:</span></span>

   * <span data-ttu-id="f4a4a-178">Per l'esperienza Razor Components aggiungere un file *Todo.razor* alla cartella *Components/Pages*.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-178">For the Razor Components experience, add a *Todo.razor* file to the *Components/Pages* folder.</span></span>
   * <span data-ttu-id="f4a4a-179">Per l'esperienza Blazor aggiungere un file *Todo.cshtml* alla cartella *Pages*.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-179">For the Blazor experience, add a *Todo.cshtml* file to the *Pages* folder.</span></span>

1. <span data-ttu-id="f4a4a-180">Specificare il markup iniziale per il componente:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-180">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="f4a4a-181">Aggiungere il componente Todo alla barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-181">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="f4a4a-182">Il componente NavMenu (*Components/Shared/NavMenu.razor* oppure *Shared/NavMenu.cshtml* in Blazor) viene usato nel layout dell'app.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-182">The NavMenu component (*Components/Shared/NavMenu.razor* or *Shared/NavMenu.cshtml* in Blazor) is used in the app's layout.</span></span> <span data-ttu-id="f4a4a-183">I layout sono componenti che consentono di evitare la duplicazione del contenuto nell'app.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-183">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="f4a4a-184">Per ulteriori informazioni, vedere <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-184">For more information, see <xref:blazor/layouts>.</span></span>

   <span data-ttu-id="f4a4a-185">Aggiungere un elemento `<NavLink>` per il componente Todo aggiungendo il markup seguente per l'elemento di elenco sotto gli elementi di elenco esistenti nel file *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="f4a4a-185">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor) file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="f4a4a-186">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-186">Rebuild and run the app.</span></span> <span data-ttu-id="f4a4a-187">Visitare la nuova pagina Todo per confermare che il collegamento al componente Todo funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-187">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="f4a4a-188">Aggiungere un file *TodoItem.cs* nella radice del progetto per contenere una classe per rappresentare un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-188">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="f4a4a-189">Usare il codice C# seguente per la classe `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-189">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. <span data-ttu-id="f4a4a-190">Tornare al componente Todo (*Components/Pages/Todo.razor* oppure *Pages/Todo.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="f4a4a-190">Return to the Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   * <span data-ttu-id="f4a4a-191">Aggiungere un campo per le attività in un blocco `@functions`.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-191">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="f4a4a-192">Il componente Todo usa questo campo per mantenere lo stato dell'elenco attività.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-192">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="f4a4a-193">Aggiungere il markup per l'elenco non ordinato e un ciclo `foreach` per eseguire il rendering di ogni elemento attività come elemento di elenco.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-193">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="f4a4a-194">L'app richiede elementi dell'interfaccia utente per l'aggiunta di attività all'elenco.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-194">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="f4a4a-195">Aggiungere un input di testo e un pulsante sotto l'elenco:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-195">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="f4a4a-196">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-196">Rebuild and run the app.</span></span> <span data-ttu-id="f4a4a-197">Quando viene selezionato il pulsante **Add todo** non accade nulla perché al pulsante non è associato un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-197">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="f4a4a-198">Aggiungere un metodo `AddTodo` al componente Todo e registrarlo per i clic sul pulsante con l'attributo `onclick`:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-198">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="f4a4a-199">Il metodo C# `AddTodo` viene chiamato quando viene selezionato il pulsante.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-199">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="f4a4a-200">Per ottenere il titolo del nuovo elemento attività, aggiungere un campo stringa `newTodo` e associarlo al valore dell'input di testo tramite l'attributo `bind`:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-200">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. <span data-ttu-id="f4a4a-201">Aggiornare il metodo `AddTodo` per aggiungere l'elemento `TodoItem` con il titolo specificato all'elenco.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-201">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="f4a4a-202">Cancellare il valore dell'input di testo impostando `newTodo` su una stringa vuota:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-202">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="f4a4a-203">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-203">Rebuild and run the app.</span></span> <span data-ttu-id="f4a4a-204">Aggiungere alcune attività all'elenco attività per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-204">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="f4a4a-205">Il testo del titolo per ogni elemento attività può essere reso modificabile e una casella di controllo può consentire all'utente di tenere traccia degli elementi completati.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-205">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="f4a4a-206">Aggiungere un input casella di controllo per ogni elemento attività e associarne il valore alla proprietà `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-206">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="f4a4a-207">Modificare `@todo.Title` in un elemento `<input>` associato a `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="f4a4a-207">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="f4a4a-208">Per verificare che questi valori siano associati, aggiornare l'intestazione `<h1>` per visualizzare un conteggio del numero di elementi attività non completati (`IsDone` è `false`).</span><span class="sxs-lookup"><span data-stu-id="f4a4a-208">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="f4a4a-209">Il componente Todo completato (*Components/Pages/Todo.razor* oppure *Pages/Todo.cshtml* in Blazor):</span><span class="sxs-lookup"><span data-stu-id="f4a4a-209">The completed Todo component (*Components/Pages/Todo.razor* or *Pages/Todo.cshtml* in Blazor):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. <span data-ttu-id="f4a4a-210">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-210">Rebuild and run the app.</span></span> <span data-ttu-id="f4a4a-211">Aggiungere elementi attività per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-211">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="f4a4a-212">Pubblicare e distribuire l'app</span><span class="sxs-lookup"><span data-stu-id="f4a4a-212">Publish and deploy the app</span></span>

<span data-ttu-id="f4a4a-213">Per pubblicare l'app, vedere <xref:host-and-deploy/blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="f4a4a-213">To publish the app, see <xref:host-and-deploy/blazor/index>.</span></span>
