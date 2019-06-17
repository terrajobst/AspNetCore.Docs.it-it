---
title: Creare la prima app Blazor
author: guardrex
description: Istruzioni dettagliate per creare un'app Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/12/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: df27dad17133f287b1c73dc308b4cc69426e0a63
ms.sourcegitcommit: 739a3d7ca4fd2908ea0984940eca589a96359482
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/13/2019
ms.locfileid: "67040718"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="a10b9-103">Creare la prima app Blazor</span><span class="sxs-lookup"><span data-stu-id="a10b9-103">Build your first Blazor app</span></span>

<span data-ttu-id="a10b9-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a10b9-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a10b9-105">Questa esercitazione mostra come compilare e modificare un'app Blazor.</span><span class="sxs-lookup"><span data-stu-id="a10b9-105">This tutorial shows you how to build and modify a Blazor app.</span></span>

<span data-ttu-id="a10b9-106">Seguire le indicazioni contenute nell'articolo <xref:blazor/get-started> per creare un progetto Blazor per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="a10b9-106">Follow the guidance in the <xref:blazor/get-started> article to create a Blazor project for this tutorial.</span></span>

## <a name="build-components"></a><span data-ttu-id="a10b9-107">Compilare i componenti</span><span class="sxs-lookup"><span data-stu-id="a10b9-107">Build components</span></span>

1. <span data-ttu-id="a10b9-108">Passare a ognuna delle tre pagine dell'app nella cartella *Pages*: Home, Counter e Fetch data (Home, Contatore e Recupera dati).</span><span class="sxs-lookup"><span data-stu-id="a10b9-108">Browse to each of the app's three pages in the *Pages* folder: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="a10b9-109">Queste pagine vengono implementate dai file dei componenti Razor *Index.razor*, *Counter.razor* e *FetchData.razor*.</span><span class="sxs-lookup"><span data-stu-id="a10b9-109">These pages are implemented by the Razor component files *Index.razor*, *Counter.razor*, and *FetchData.razor*.</span></span>

1. <span data-ttu-id="a10b9-110">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="a10b9-110">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="a10b9-111">Per incrementare un contatore in una pagina Web è in genere richiesta la scrittura di codice JavaScript, ma Blazor offre un approccio migliore usando C#.</span><span class="sxs-lookup"><span data-stu-id="a10b9-111">Incrementing a counter in a webpage normally requires writing JavaScript, but Blazor provides a better approach using C#.</span></span>

1. <span data-ttu-id="a10b9-112">Esaminare l'implementazione del componente Counter nel file *Counter.razor*.</span><span class="sxs-lookup"><span data-stu-id="a10b9-112">Examine the implementation of the Counter component in the *Counter.razor* file.</span></span>

   <span data-ttu-id="a10b9-113">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="a10b9-113">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   <span data-ttu-id="a10b9-114">L'interfaccia utente del componente Counter viene definita tramite codice HTML.</span><span class="sxs-lookup"><span data-stu-id="a10b9-114">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="a10b9-115">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="a10b9-115">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="a10b9-116">Il markup HTML e la logica di rendering C# vengono convertiti un una classe del componente in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="a10b9-116">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="a10b9-117">Il nome della classe .NET generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="a10b9-117">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="a10b9-118">I membri della classe del componente vengono definiti in un blocco `@code`.</span><span class="sxs-lookup"><span data-stu-id="a10b9-118">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="a10b9-119">Nel blocco `@code`, lo stato del componente (proprietà, campi) e i metodi vengono specificati per la gestione degli eventi o per definire la logica di altri componenti.</span><span class="sxs-lookup"><span data-stu-id="a10b9-119">In the `@code` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="a10b9-120">Questi membri vengono quindi usati come parte della logica di rendering del componente e per la gestione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="a10b9-120">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="a10b9-121">Quando viene selezionato il pulsante **Click me**:</span><span class="sxs-lookup"><span data-stu-id="a10b9-121">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="a10b9-122">Viene chiamato il gestore `onclick` registrato del componente Counter (metodo `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="a10b9-122">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="a10b9-123">Il componente Counter rigenera il relativo albero di rendering.</span><span class="sxs-lookup"><span data-stu-id="a10b9-123">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="a10b9-124">Il nuovo albero di rendering viene confrontato con quello precedente.</span><span class="sxs-lookup"><span data-stu-id="a10b9-124">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="a10b9-125">Vengono applicate solo le modifiche al modello DOM (Document Object Model).</span><span class="sxs-lookup"><span data-stu-id="a10b9-125">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="a10b9-126">Il conteggio visualizzato viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="a10b9-126">The displayed count is updated.</span></span>

1. <span data-ttu-id="a10b9-127">Modificare la logica C# del componente Counter per incrementare il contatore di due unità anziché una.</span><span class="sxs-lookup"><span data-stu-id="a10b9-127">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. <span data-ttu-id="a10b9-128">Ricompilare ed eseguire l'app per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="a10b9-128">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="a10b9-129">Selezionare il pulsante **Fare clic qui**.</span><span class="sxs-lookup"><span data-stu-id="a10b9-129">Select the **Click me** button.</span></span> <span data-ttu-id="a10b9-130">Il contatore viene incrementato di due.</span><span class="sxs-lookup"><span data-stu-id="a10b9-130">The counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="a10b9-131">Usare i componenti</span><span class="sxs-lookup"><span data-stu-id="a10b9-131">Use components</span></span>

<span data-ttu-id="a10b9-132">Includere un componente in un altro componente usando una sintassi HTML.</span><span class="sxs-lookup"><span data-stu-id="a10b9-132">Include a component in another component using an HTML syntax.</span></span>

1. <span data-ttu-id="a10b9-133">Aggiungere il componente Counter al componente Index dell'app aggiungendo un elemento `<Counter />` al componente Index (*Index.razor*).</span><span class="sxs-lookup"><span data-stu-id="a10b9-133">Add the Counter component to the app's Index component by adding a `<Counter />` element to the Index component (*Index.razor*).</span></span>

   <span data-ttu-id="a10b9-134">Se si usa il lato client di Blazor per questa esperienza, il componente Index include un componente Survey Prompt (elemento `<SurveyPrompt>`).</span><span class="sxs-lookup"><span data-stu-id="a10b9-134">If you're using Blazor client-side for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="a10b9-135">Sostituire l'elemento `<SurveyPrompt>` con l'elemento `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="a10b9-135">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span> <span data-ttu-id="a10b9-136">Se si usa un'app lato server Blazor per questa esperienza, aggiungere l'elemento `<Counter>` al componente Index:</span><span class="sxs-lookup"><span data-stu-id="a10b9-136">If you're using a Blazor server-side app for this experience, add the `<Counter>` element to the Index component:</span></span>

   <span data-ttu-id="a10b9-137">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="a10b9-137">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. <span data-ttu-id="a10b9-138">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a10b9-138">Rebuild and run the app.</span></span> <span data-ttu-id="a10b9-139">Il componente Index ha il proprio contatore.</span><span class="sxs-lookup"><span data-stu-id="a10b9-139">The Index component has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="a10b9-140">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="a10b9-140">Component parameters</span></span>

<span data-ttu-id="a10b9-141">I componenti possono avere anche parametri,</span><span class="sxs-lookup"><span data-stu-id="a10b9-141">Components can also have parameters.</span></span> <span data-ttu-id="a10b9-142">che vengono definiti usando proprietà non pubbliche nella classe del componente decorata con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="a10b9-142">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="a10b9-143">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="a10b9-143">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="a10b9-144">Aggiornare il codice C# `@code` del componente:</span><span class="sxs-lookup"><span data-stu-id="a10b9-144">Update the component's `@code` C# code:</span></span>

   * <span data-ttu-id="a10b9-145">Aggiungere una proprietà `IncrementAmount` decorata con l'attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="a10b9-145">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="a10b9-146">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="a10b9-146">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="a10b9-147">*Pages/Counter.razor*:</span><span class="sxs-lookup"><span data-stu-id="a10b9-147">*Pages/Counter.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="a10b9-148">Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Index usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="a10b9-148">Specify an `IncrementAmount` parameter in the Index component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="a10b9-149">Impostare il valore per incrementare il contatore di dieci unità.</span><span class="sxs-lookup"><span data-stu-id="a10b9-149">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="a10b9-150">*Pages/Index.razor*:</span><span class="sxs-lookup"><span data-stu-id="a10b9-150">*Pages/Index.razor*:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. <span data-ttu-id="a10b9-151">Ricaricare il componente Index.</span><span class="sxs-lookup"><span data-stu-id="a10b9-151">Reload the Index component.</span></span> <span data-ttu-id="a10b9-152">Il contatore viene incrementato di dieci unità ogni volta che viene selezionato il pulsante **Click me**.</span><span class="sxs-lookup"><span data-stu-id="a10b9-152">The counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="a10b9-153">Il contatore nel componente Counter continua a essere incrementato di uno.</span><span class="sxs-lookup"><span data-stu-id="a10b9-153">The counter in the Counter component continues to increment by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="a10b9-154">Indirizzare le richieste ai componenti</span><span class="sxs-lookup"><span data-stu-id="a10b9-154">Route to components</span></span>

<span data-ttu-id="a10b9-155">La direttiva `@page` all'inizio del file *Counter.razor* specifica che il componente Counter è un endpoint di routing.</span><span class="sxs-lookup"><span data-stu-id="a10b9-155">The `@page` directive at the top of the *Counter.razor* file specifies that the Counter component is a routing endpoint.</span></span> <span data-ttu-id="a10b9-156">Il componente Counter gestisce le richieste inviate a `/counter`.</span><span class="sxs-lookup"><span data-stu-id="a10b9-156">The Counter component handles requests sent to `/counter`.</span></span> <span data-ttu-id="a10b9-157">Senza la direttiva `@page`, un componente non gestisce le richieste instradate, ma può comunque essere usato da altri componenti.</span><span class="sxs-lookup"><span data-stu-id="a10b9-157">Without the `@page` directive, a component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="a10b9-158">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="a10b9-158">Dependency injection</span></span>

<span data-ttu-id="a10b9-159">I servizi registrati nel contenitore del servizio dell'app sono disponibili per i componenti tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a10b9-159">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a10b9-160">Inserire i servizi in un componente usando la direttiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="a10b9-160">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="a10b9-161">Esaminare le direttive del componente FetchData.</span><span class="sxs-lookup"><span data-stu-id="a10b9-161">Examine the directives of the FetchData component.</span></span>

<span data-ttu-id="a10b9-162">Se si usa un'app lato server Blazor, il servizio `WeatherForecastService` viene registrato come [singleton](xref:fundamentals/dependency-injection#service-lifetimes), in modo che un'istanza del servizio sia disponibile nell'intera app.</span><span class="sxs-lookup"><span data-stu-id="a10b9-162">If working with a Blazor server-side app, the `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span> <span data-ttu-id="a10b9-163">La direttiva `@inject` viene usata per inserire l'istanza del servizio `WeatherForecastService` nel componente.</span><span class="sxs-lookup"><span data-stu-id="a10b9-163">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component.</span></span>

<span data-ttu-id="a10b9-164">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="a10b9-164">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

<span data-ttu-id="a10b9-165">Il componente FetchData usa il servizio inserito, come `ForecastService`, per recuperare una matrice di oggetti `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="a10b9-165">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

<span data-ttu-id="a10b9-166">Se si usa un'app lato client Blazor, viene inserito `HttpClient` per ottenere i dati delle previsioni meteo dal file *weather.json* nella cartella *wwwroot/sample-data*:</span><span class="sxs-lookup"><span data-stu-id="a10b9-166">If working with a Blazor client-side app, `HttpClient` is injected to obtain weather forecast data from the *weather.json* file in the *wwwroot/sample-data* folder:</span></span>

<span data-ttu-id="a10b9-167">*Pages/FetchData.razor*:</span><span class="sxs-lookup"><span data-stu-id="a10b9-167">*Pages/FetchData.razor*:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

<span data-ttu-id="a10b9-168">Viene usato un ciclo [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) per eseguire il rendering di ogni istanza di previsione come riga nella tabella dei dati meteo:</span><span class="sxs-lookup"><span data-stu-id="a10b9-168">A [\@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a><span data-ttu-id="a10b9-169">Compilare un elenco attività</span><span class="sxs-lookup"><span data-stu-id="a10b9-169">Build a todo list</span></span>

<span data-ttu-id="a10b9-170">Aggiungere all'app un nuovo componente che implementa un semplice elenco attività.</span><span class="sxs-lookup"><span data-stu-id="a10b9-170">Add a new component to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="a10b9-171">Aggiungere un file vuoto denominato *Todo.razor* all'app nella cartella *Pages*:</span><span class="sxs-lookup"><span data-stu-id="a10b9-171">Add an empty file named *Todo.razor* to the app in the *Pages* folder:</span></span>

1. <span data-ttu-id="a10b9-172">Specificare il markup iniziale per il componente:</span><span class="sxs-lookup"><span data-stu-id="a10b9-172">Provide the initial markup for the component:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="a10b9-173">Aggiungere il componente Todo alla barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="a10b9-173">Add the Todo component to the navigation bar.</span></span>

   <span data-ttu-id="a10b9-174">Il componente NavMenu (*Shared/NavMenu.razor*) viene usato nel layout dell'app.</span><span class="sxs-lookup"><span data-stu-id="a10b9-174">The NavMenu component (*Shared/NavMenu.razor*) is used in the app's layout.</span></span> <span data-ttu-id="a10b9-175">I layout sono componenti che consentono di evitare la duplicazione del contenuto nell'app.</span><span class="sxs-lookup"><span data-stu-id="a10b9-175">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="a10b9-176">Per ulteriori informazioni, vedere <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="a10b9-176">For more information, see <xref:blazor/layouts>.</span></span>

   <span data-ttu-id="a10b9-177">Aggiungere un elemento `<NavLink>` per il componente Todo aggiungendo il markup seguente per la voce di elenco sotto le voci di elenco esistenti nel file *Shared/NavMenu.razor*:</span><span class="sxs-lookup"><span data-stu-id="a10b9-177">Add a `<NavLink>` for the Todo component by adding the following list item markup below the existing list items in the *Shared/NavMenu.razor* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="a10b9-178">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a10b9-178">Rebuild and run the app.</span></span> <span data-ttu-id="a10b9-179">Visitare la nuova pagina Todo per confermare che il collegamento al componente Todo funzioni correttamente.</span><span class="sxs-lookup"><span data-stu-id="a10b9-179">Visit the new Todo page to confirm that the link to the Todo component works.</span></span>

1. <span data-ttu-id="a10b9-180">Aggiungere un file *TodoItem.cs* nella radice del progetto per contenere una classe per rappresentare un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="a10b9-180">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="a10b9-181">Usare il codice C# seguente per la classe `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="a10b9-181">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. <span data-ttu-id="a10b9-182">Tornare al componente Todo (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="a10b9-182">Return to the Todo component (*Pages/Todo.razor*):</span></span>

   * <span data-ttu-id="a10b9-183">Aggiungere un campo per le voci todo in un blocco `@code`.</span><span class="sxs-lookup"><span data-stu-id="a10b9-183">Add a field for the todo items in an `@code` block.</span></span> <span data-ttu-id="a10b9-184">Il componente Todo usa questo campo per mantenere lo stato dell'elenco attività.</span><span class="sxs-lookup"><span data-stu-id="a10b9-184">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="a10b9-185">Aggiungere il markup per l'elenco non ordinato e un ciclo `foreach` per eseguire il rendering di ogni elemento attività come elemento di elenco.</span><span class="sxs-lookup"><span data-stu-id="a10b9-185">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. <span data-ttu-id="a10b9-186">L'app richiede elementi dell'interfaccia utente per l'aggiunta di voci todo all'elenco.</span><span class="sxs-lookup"><span data-stu-id="a10b9-186">The app requires UI elements for adding todo items to the list.</span></span> <span data-ttu-id="a10b9-187">Aggiungere un input di testo e un pulsante sotto l'elenco:</span><span class="sxs-lookup"><span data-stu-id="a10b9-187">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. <span data-ttu-id="a10b9-188">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a10b9-188">Rebuild and run the app.</span></span> <span data-ttu-id="a10b9-189">Quando viene selezionato il pulsante **Add todo** non accade nulla perché al pulsante non è associato un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="a10b9-189">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="a10b9-190">Aggiungere un metodo `AddTodo` al componente Todo e registrarlo per i clic sul pulsante con l'attributo `@onclick`:</span><span class="sxs-lookup"><span data-stu-id="a10b9-190">Add an `AddTodo` method to the Todo component and register it for button clicks using the `@onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   <span data-ttu-id="a10b9-191">Il metodo C# `AddTodo` viene chiamato quando viene selezionato il pulsante.</span><span class="sxs-lookup"><span data-stu-id="a10b9-191">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="a10b9-192">Per ottenere il titolo del nuovo elemento attività, aggiungere un campo stringa `newTodo` e associarlo al valore dell'input di testo tramite l'attributo `bind`:</span><span class="sxs-lookup"><span data-stu-id="a10b9-192">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="@newTodo" />
   ```

1. <span data-ttu-id="a10b9-193">Aggiornare il metodo `AddTodo` per aggiungere l'elemento `TodoItem` con il titolo specificato all'elenco.</span><span class="sxs-lookup"><span data-stu-id="a10b9-193">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="a10b9-194">Cancellare il valore dell'input di testo impostando `newTodo` su una stringa vuota:</span><span class="sxs-lookup"><span data-stu-id="a10b9-194">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. <span data-ttu-id="a10b9-195">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a10b9-195">Rebuild and run the app.</span></span> <span data-ttu-id="a10b9-196">Aggiungere alcune voci todo all'elenco todo per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="a10b9-196">Add some todo items to the todo list to test the new code.</span></span>

1. <span data-ttu-id="a10b9-197">Il testo del titolo per ogni elemento attività può essere reso modificabile e una casella di controllo può consentire all'utente di tenere traccia degli elementi completati.</span><span class="sxs-lookup"><span data-stu-id="a10b9-197">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="a10b9-198">Aggiungere un input casella di controllo per ogni elemento attività e associarne il valore alla proprietà `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="a10b9-198">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="a10b9-199">Modificare `@todo.Title` in un elemento `<input>` associato a `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="a10b9-199">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. <span data-ttu-id="a10b9-200">Per verificare che questi valori siano associati, aggiornare l'intestazione `<h1>` per visualizzare un conteggio del numero di elementi attività non completati (`IsDone` è `false`).</span><span class="sxs-lookup"><span data-stu-id="a10b9-200">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="a10b9-201">Componente Todo completato (*Pages/Todo.razor*):</span><span class="sxs-lookup"><span data-stu-id="a10b9-201">The completed Todo component (*Pages/Todo.razor*):</span></span>

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. <span data-ttu-id="a10b9-202">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="a10b9-202">Rebuild and run the app.</span></span> <span data-ttu-id="a10b9-203">Aggiungere elementi attività per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="a10b9-203">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="a10b9-204">Pubblicare e distribuire l'app</span><span class="sxs-lookup"><span data-stu-id="a10b9-204">Publish and deploy the app</span></span>

<span data-ttu-id="a10b9-205">Per pubblicare l'app, vedere <xref:host-and-deploy/blazor/index>.</span><span class="sxs-lookup"><span data-stu-id="a10b9-205">To publish the app, see <xref:host-and-deploy/blazor/index>.</span></span>
