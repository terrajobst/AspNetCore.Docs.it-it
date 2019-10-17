---
title: Creare la prima app Blazor
author: guardrex
description: Istruzioni dettagliate per creare un'app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: c357b324905ee3a4c9f4bd167dbbcacaf7e1bc76
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391212"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="fe636-103">Creare la prima app Blazor</span><span class="sxs-lookup"><span data-stu-id="fe636-103">Build your first Blazor app</span></span>

<span data-ttu-id="fe636-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fe636-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="fe636-105">Questa esercitazione mostra come compilare e modificare un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="fe636-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="fe636-106">Seguire le indicazioni contenute nell'articolo <xref:blazor/get-started> per creare un progetto Blazor per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="fe636-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span> <span data-ttu-id="fe636-107">Denominare il progetto *ToDoList*.</span><span class="sxs-lookup"><span data-stu-id="fe636-107">Name the project *ToDoList*.</span></span>

## <a name="build-components"></a><span data-ttu-id="fe636-108">Compilare i componenti</span><span class="sxs-lookup"><span data-stu-id="fe636-108">Build components</span></span>

1. <span data-ttu-id="fe636-109">Passare a ognuna delle tre pagine dell'app nella cartella *pages* : Home, Counter e fetch data.</span><span class="sxs-lookup"><span data-stu-id="fe636-109">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="fe636-110">Queste pagine vengono implementate dai file dei componenti Razor *Index.razor*, *Counter.razor* e *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="fe636-110">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="fe636-111">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="fe636-111">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="fe636-112">Per incrementare un contatore in una pagina Web è in genere richiesta la scrittura di codice JavaScript, ma Blazor offre un approccio migliore usando C#.</span><span class="sxs-lookup"><span data-stu-id="fe636-112">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="fe636-113">Esaminare l'implementazione del componente `Counter` nel file *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="fe636-113">Examine the implementation of the `Counter` component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="fe636-114">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="fe636-114">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="fe636-115">L'interfaccia utente del componente `Counter` viene definita tramite codice HTML.</span><span class="sxs-lookup"><span data-stu-id="fe636-115">The UI of the `Counter` component is defined using HTML.</span></span> <span data-ttu-id="fe636-116">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="fe636-116">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="fe636-117">Il markup HTML e la logica di rendering C# vengono convertiti un una classe del componente in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="fe636-117">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="fe636-118">Il nome della classe .NET generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="fe636-118">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="fe636-119">I membri della classe del componente vengono definiti in un blocco `@code`.</span><span class="sxs-lookup"><span data-stu-id="fe636-119">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="fe636-120">Nel blocco `@code`, lo stato del componente (proprietà, campi) e i metodi vengono specificati per la gestione degli eventi o per definire la logica di altri componenti.</span><span class="sxs-lookup"><span data-stu-id="fe636-120">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="fe636-121">Questi membri vengono quindi usati come parte della logica di rendering del componente e per la gestione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="fe636-121">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="fe636-122">Quando viene selezionato il pulsante **Click me**:</span><span class="sxs-lookup"><span data-stu-id="fe636-122">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="fe636-123">Viene chiamato il gestore `onclick` registrato del componente `Counter` (metodo `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="fe636-123">The `Counter` component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="fe636-124">Il componente `Counter` rigenera il relativo albero di rendering.</span><span class="sxs-lookup"><span data-stu-id="fe636-124">The `Counter` component regenerates its render tree.</span></span>
   * <span data-ttu-id="fe636-125">Il nuovo albero di rendering viene confrontato con quello precedente.</span><span class="sxs-lookup"><span data-stu-id="fe636-125">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="fe636-126">Vengono applicate solo le modifiche al modello DOM (Document Object Model).</span><span class="sxs-lookup"><span data-stu-id="fe636-126">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="fe636-127">Il conteggio visualizzato viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="fe636-127">The displayed count is updated.</span></span>

1. <span data-ttu-id="fe636-128">Modificare la logica C# del componente `Counter` per incrementare il contatore di due unità anziché una.</span><span class="sxs-lookup"><span data-stu-id="fe636-128">Modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="fe636-129">Ricompilare ed eseguire l'app per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="fe636-129">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="fe636-130">Selezionare il pulsante **Fare clic qui**.</span><span class="sxs-lookup"><span data-stu-id="fe636-130">Select the **Click me** button.</span></span> <span data-ttu-id="fe636-131">Il contatore viene incrementato di due.</span><span class="sxs-lookup"><span data-stu-id="fe636-131">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="fe636-132">Usare i componenti</span><span class="sxs-lookup"><span data-stu-id="fe636-132">Use components</span></span>

<span data-ttu-id="fe636-133">Includere un componente in un altro componente usando una sintassi HTML.</span><span class="sxs-lookup"><span data-stu-id="fe636-133">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="fe636-134">Aggiungere il componente `Counter` al componente `Index` dell'app aggiungendo un elemento `<Counter />` al componente `Index` (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="fe636-134">Add the `Counter` component to the app's `Index` component by adding a `<Counter />` element to the `Index` component (*Index.razor*).</span></span>

   <span data-ttu-id="fe636-135">Se si usa webassembly blazer per questa esperienza, il componente `Index` usa un componente `SurveyPrompt`.</span><span class="sxs-lookup"><span data-stu-id="fe636-135">If you're using Blazor WebAssembly for this experience, a `SurveyPrompt` component is used by the `Index` component.</span></span> <span data-ttu-id="fe636-136">Sostituire l'elemento `<SurveyPrompt>` con un elemento `<Counter />`.</span><span class="sxs-lookup"><span data-stu-id="fe636-136">Replace the `<SurveyPrompt>` element with a `<Counter />` element.</span></span> <span data-ttu-id="fe636-137">Se si usa un'app del server blazer per questa esperienza, aggiungere l'elemento `<Counter />` al componente `Index`:</span><span class="sxs-lookup"><span data-stu-id="fe636-137">If you're using a Blazor Server app for this experience, add the `<Counter />` element to the `Index` component:</span></span>

   <span data-ttu-id="fe636-138">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="fe636-138">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="fe636-139">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="fe636-139">Rebuild and run the app.</span></span> <span data-ttu-id="fe636-140">Il componente `Index` ha il proprio contatore.</span><span class="sxs-lookup"><span data-stu-id="fe636-140">The `Index` component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="fe636-141">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="fe636-141">Component parameters</span></span>

<span data-ttu-id="fe636-142">I componenti possono avere anche parametri,</span><span class="sxs-lookup"><span data-stu-id="fe636-142">Components can also have parameters.</span></span> <span data-ttu-id="fe636-143">I parametri del componente vengono definiti usando proprietà pubbliche nella classe Component con l'attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="fe636-143">Component parameters are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="fe636-144">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="fe636-144">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="fe636-145">Aggiornare il codice C# `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="fe636-145">Update the component's `@code` C# code:</span></span>

   * <span data-ttu-id="fe636-146">Aggiungere una proprietà Public `IncrementAmount` con l'attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="fe636-146">Add a public `IncrementAmount` property with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="fe636-147">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="fe636-147">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="fe636-148">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="fe636-148">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="fe636-149">Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente `Index` usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="fe636-149">Specify an `IncrementAmount` parameter in the `Index` component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="fe636-150">Impostare il valore per incrementare il contatore di dieci unità.</span><span class="sxs-lookup"><span data-stu-id="fe636-150">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="fe636-151">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="fe636-151">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="fe636-152">Ricaricare il componente `Index`.</span><span class="sxs-lookup"><span data-stu-id="fe636-152">Reload the `Index` component.</span></span> <span data-ttu-id="fe636-153">Il contatore viene incrementato di dieci unità ogni volta che viene selezionato il pulsante **Click me**.</span><span class="sxs-lookup"><span data-stu-id="fe636-153">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="fe636-154">Il contatore nel componente `Counter` continua a essere incrementato di uno.</span><span class="sxs-lookup"><span data-stu-id="fe636-154">The counter in the `Counter` component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="fe636-155">Indirizzare le richieste ai componenti</span><span class="sxs-lookup"><span data-stu-id="fe636-155">Route to components</span></span>

<span data-ttu-id="fe636-156">La direttiva `@page` all'inizio del file *Counter.razor* specifica che il componente `Counter` è un endpoint di routing.</span><span class="sxs-lookup"><span data-stu-id="fe636-156">The `@page` directive at the top of the *Counter.razor* file specifies that the `Counter` component is a routing endpoint.</span></span> <span data-ttu-id="fe636-157">Il componente `Counter` gestisce le richieste inviate a `/counter`.</span><span class="sxs-lookup"><span data-stu-id="fe636-157">The `Counter` component handles requests sent to `/counter`.</span></span> <span data-ttu-id="fe636-158">Senza la direttiva `@page`, un componente non gestisce le richieste instradate, ma può comunque essere usato da altri componenti.</span><span class="sxs-lookup"><span data-stu-id="fe636-158">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="fe636-159">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="fe636-159">Dependency injection</span></span>

### <a name="blazor-server-experience"></a><span data-ttu-id="fe636-160">Esperienza del server Blazer</span><span class="sxs-lookup"><span data-stu-id="fe636-160">Blazor Server experience</span></span>

<span data-ttu-id="fe636-161">Se si utilizza un'app del server blazer, il servizio `WeatherForecastService` viene registrato come [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fe636-161">If working with a Blazor Server app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fe636-162">Un'istanza del servizio è disponibile in tutte le app tramite l' [inserimento di dipendenze](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="fe636-162">An instance of the service is available throughout the app via [dependency injection (DI)](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](build-your-first-blazor-app/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="fe636-163">La direttiva `@inject` viene utilizzata per inserire l'istanza del servizio `WeatherForecastService` nel componente `FetchData`.</span><span class="sxs-lookup"><span data-stu-id="fe636-163">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the `FetchData` component.</span></span>

<span data-ttu-id="fe636-164">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="fe636-164">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="fe636-165">Il componente `FetchData` usa il servizio inserito, come `ForecastService`, per recuperare una matrice di oggetti `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="fe636-165">The `FetchData` component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

### <a name="blazor-webassembly-experience"></a><span data-ttu-id="fe636-166">Esperienza webassembly Blazer</span><span class="sxs-lookup"><span data-stu-id="fe636-166">Blazor WebAssembly experience</span></span>

<span data-ttu-id="fe636-167">Se si usa un'app webassembly blazer, viene inserito `HttpClient` per ottenere i dati delle previsioni meteo dal file *Weather. JSON* nella cartella *wwwroot/Sample-Data* .</span><span class="sxs-lookup"><span data-stu-id="fe636-167">If working with a Blazor WebAssembly app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder.</span></span>

<span data-ttu-id="fe636-168">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="fe636-168">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="fe636-169">Un ciclo [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) viene usato per eseguire il rendering di ogni istanza di previsione come riga nella tabella dei dati meteorologici:</span><span class="sxs-lookup"><span data-stu-id="fe636-169">An [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="fe636-170">Compilare un elenco attività</span><span class="sxs-lookup"><span data-stu-id="fe636-170">Build a todo list</span></span>

<span data-ttu-id="fe636-171">Aggiungere all'app un nuovo componente che implementa un semplice elenco attività.</span><span class="sxs-lookup"><span data-stu-id="fe636-171">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="fe636-172">Aggiungere un file vuoto denominato *Todo.razor* all'app nella cartella *Pages*:</span><span class="sxs-lookup"><span data-stu-id="fe636-172">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="fe636-173">Specificare il markup iniziale per il componente:</span><span class="sxs-lookup"><span data-stu-id="fe636-173">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="fe636-174">Aggiungere il componente `Todo` alla barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="fe636-174">Add the `Todo` component to the navigation bar.</span></span>

   <span data-ttu-id="fe636-175">Il componente `NavMenu` (*Shared/NavMenu.razor*) viene usato nel layout dell'app.</span><span class="sxs-lookup"><span data-stu-id="fe636-175">The `NavMenu` component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="fe636-176">I layout sono componenti che consentono di evitare la duplicazione del contenuto nell'app.</span><span class="sxs-lookup"><span data-stu-id="fe636-176">Layouts are components that allow you to avoid duplication of content in the app.</span></span>

   <span data-ttu-id="fe636-177">Aggiungere un elemento `<NavLink>` per il componente `Todo` aggiungendo il markup seguente per la voce di elenco sotto le voci di elenco esistenti nel file *Shared/NavMenu.razor*:</span><span class="sxs-lookup"><span data-stu-id="fe636-177">Add a `<NavLink>` element for the `Todo` component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="fe636-178">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="fe636-178">Rebuild and run the app.</span></span> <span data-ttu-id="fe636-179">Visitare la nuova pagina Todo per confermare che il collegamento al componente `Todo` funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="fe636-179">Visit the new Todo page to confirm that the link to the `Todo` component works.</span></span>

1. <span data-ttu-id="fe636-180">Aggiungere un file *TodoItem.cs* nella radice del progetto per contenere una classe per rappresentare un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="fe636-180">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="fe636-181">Usare il codice C# seguente per la classe `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="fe636-181">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="fe636-182">Tornare al componente `Todo` (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="fe636-182">Return to the `Todo` component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="fe636-183">Aggiungere un campo per le voci todo in un blocco `@code`.</span><span class="sxs-lookup"><span data-stu-id="fe636-183">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="fe636-184">Il componente `Todo` usa questo campo per mantenere lo stato dell'elenco attività.</span><span class="sxs-lookup"><span data-stu-id="fe636-184">The `Todo` component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="fe636-185">Aggiungere il markup per l'elenco non ordinato e un ciclo `foreach` per eseguire il rendering di ogni elemento Todo come elemento di elenco (`<li>`).</span><span class="sxs-lookup"><span data-stu-id="fe636-185">Add unordered list markup and a `foreach` loop to render each todo item as a list item (`<li>`).</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="fe636-186">L'app richiede elementi dell'interfaccia utente per l'aggiunta di voci todo all'elenco.</span><span class="sxs-lookup"><span data-stu-id="fe636-186">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="fe636-187">Aggiungere un input di testo (`<input>`) e un pulsante (`<button>`) sotto l'elenco non ordinato (`<ul>...</ul>`):</span><span class="sxs-lookup"><span data-stu-id="fe636-187">Add a text input (`<input>`) and a button (`<button>`) below the unordered list (`<ul>...</ul>`):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="fe636-188">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="fe636-188">Rebuild and run the app.</span></span> <span data-ttu-id="fe636-189">Quando viene selezionato il pulsante **Add todo** non accade nulla perché al pulsante non è associato un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="fe636-189">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="fe636-190">Aggiungere un metodo `AddTodo` al componente `Todo` e registrarlo per le selezioni con pulsante con l'attributo `@onclick`.</span><span class="sxs-lookup"><span data-stu-id="fe636-190">Add an `AddTodo` method to the `Todo` component and register it for button selections using the `@onclick` attribute.</span></span> <span data-ttu-id="fe636-191">Il metodo C# `AddTodo` viene chiamato quando viene selezionato il pulsante:</span><span class="sxs-lookup"><span data-stu-id="fe636-191">The `AddTodo` C# method is called when the button is selected:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. <span data-ttu-id="fe636-192">Per ottenere il titolo del nuovo elemento Todo, aggiungere un campo stringa `newTodo` all'inizio del blocco `@code` e associarlo al valore dell'input di testo tramite l'attributo `bind` dell'elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="fe636-192">To get the title of the new todo item, add a `newTodo` string field at the top of the `@code` block and bind it to the value of the text input using the `bind` attribute in the `<input>` element:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. <span data-ttu-id="fe636-193">Aggiornare il metodo `AddTodo` per aggiungere l'elemento `TodoItem` con il titolo specificato all'elenco.</span><span class="sxs-lookup"><span data-stu-id="fe636-193">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="fe636-194">Cancellare il valore dell'input di testo impostando `newTodo` su una stringa vuota:</span><span class="sxs-lookup"><span data-stu-id="fe636-194">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="fe636-195">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="fe636-195">Rebuild and run the app.</span></span> <span data-ttu-id="fe636-196">Aggiungere alcune voci todo all'elenco todo per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="fe636-196">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="fe636-197">Il testo del titolo per ogni elemento Todo può essere reso modificabile e una casella di controllo può consentire all'utente di tenere traccia degli elementi completati.</span><span class="sxs-lookup"><span data-stu-id="fe636-197">The title text for each todo item can be made editable, and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="fe636-198">Aggiungere un input casella di controllo per ogni elemento attività e associarne il valore alla proprietà `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="fe636-198">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="fe636-199">Modificare `@todo.Title` in un elemento `<input>` associato a `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="fe636-199">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="fe636-200">Per verificare che questi valori siano associati, aggiornare l'intestazione `<h1>` per visualizzare un conteggio del numero di elementi attività non completati (`IsDone` è `false`).</span><span class="sxs-lookup"><span data-stu-id="fe636-200">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="fe636-201">Componente `Todo` completato (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="fe636-201">The completed `Todo` component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="fe636-202">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="fe636-202">Rebuild and run the app.</span></span> <span data-ttu-id="fe636-203">Aggiungere elementi attività per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="fe636-203">Add todo items to test the new code.</span></span>

> [!div class="nextstepaction"]
> <xref:blazor/components>
