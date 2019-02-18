---
title: Creare la prima app Razor Components
author: guardrex
description: Procedura dettagliata per creare un'app Razor Components e informazioni sui concetti di base di Razor Components.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 0c3dd2366581d73bad44e2911602e13c6c0daf9a
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159343"
---
# <a name="build-your-first-razor-components-app"></a><span data-ttu-id="78b0c-103">Creare la prima app Razor Components</span><span class="sxs-lookup"><span data-stu-id="78b0c-103">Build your first Razor Components app</span></span>

<span data-ttu-id="78b0c-104">Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="78b0c-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="78b0c-105">Questa esercitazione illustra come creare un'app con Razor Components e offre una dimostrazione dei concetti di base di Razor Components.</span><span class="sxs-lookup"><span data-stu-id="78b0c-105">This tutorial shows you how to build an app with Razor Components and demonstrates basic Razor Components concepts.</span></span> <span data-ttu-id="78b0c-106">È possibile seguire questa esercitazione usando sia un progetto basato su Razor Components (supportato in .NET Core 3.0 o versione successiva) che un progetto basato su Blazor (supportato in una versione futura di .NET Core).</span><span class="sxs-lookup"><span data-stu-id="78b0c-106">You can enjoy this tutorial using either a Razor Components-based project (supported in .NET Core 3.0 or later) or using a Blazor-based project (supported in a future release of .NET Core).</span></span>

<span data-ttu-id="78b0c-107">Per un'esperienza con ASP.NET Core Razor Components (*consigliata*):</span><span class="sxs-lookup"><span data-stu-id="78b0c-107">For an experience using ASP.NET Core Razor Components (*recommended*):</span></span>

* <span data-ttu-id="78b0c-108">Seguire le indicazioni in <xref:razor-components/get-started> per creare un progetto basato su Razor Components.</span><span class="sxs-lookup"><span data-stu-id="78b0c-108">Follow the guidance in <xref:razor-components/get-started> to create a Razor Components-based project.</span></span>
* <span data-ttu-id="78b0c-109">Denominare il progetto `RazorComponents`.</span><span class="sxs-lookup"><span data-stu-id="78b0c-109">Name the project `RazorComponents`.</span></span>
* <span data-ttu-id="78b0c-110">Dal modello di Razor Components viene creata una soluzione multiprogetto.</span><span class="sxs-lookup"><span data-stu-id="78b0c-110">A multi-project solution is created from the Razor Components template.</span></span> <span data-ttu-id="78b0c-111">Il progetto Razor Components verrà generato come *RazorComponents.App*.</span><span class="sxs-lookup"><span data-stu-id="78b0c-111">The Razor Components project is generated as *RazorComponents.App*.</span></span>

<span data-ttu-id="78b0c-112">Per un'esperienza con Blazor:</span><span class="sxs-lookup"><span data-stu-id="78b0c-112">For an experience using Blazor:</span></span>

* <span data-ttu-id="78b0c-113">Seguire le indicazioni in <xref:spa/blazor/get-started> per creare un progetto basato su Blazor.</span><span class="sxs-lookup"><span data-stu-id="78b0c-113">Follow the guidance in <xref:spa/blazor/get-started> to create a Blazor-based project.</span></span>
* <span data-ttu-id="78b0c-114">Denominare il progetto `Blazor`.</span><span class="sxs-lookup"><span data-stu-id="78b0c-114">Name the project `Blazor`.</span></span>
* <span data-ttu-id="78b0c-115">Dal modello Blazor viene creata una soluzione con progetto singolo.</span><span class="sxs-lookup"><span data-stu-id="78b0c-115">A single-project solution is created from the Blazor template.</span></span>

<span data-ttu-id="78b0c-116">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="78b0c-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="78b0c-117">Vedere gli argomenti seguenti per i prerequisiti:</span><span class="sxs-lookup"><span data-stu-id="78b0c-117">See the following topics for prerequisites:</span></span>

## <a name="build-components"></a><span data-ttu-id="78b0c-118">Compilare i componenti</span><span class="sxs-lookup"><span data-stu-id="78b0c-118">Build components</span></span>

1. <span data-ttu-id="78b0c-119">Passare a ognuna delle tre pagine dell'app: Home, Counter e Fetch data (Home, Contatore e Recupera dati).</span><span class="sxs-lookup"><span data-stu-id="78b0c-119">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span> <span data-ttu-id="78b0c-120">Queste pagine vengono implementate dai file Razor nella cartella *Pages*: *Index.cshtml*, *Counter.cshtml* e *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="78b0c-120">These pages are implemented by Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span>

1. <span data-ttu-id="78b0c-121">Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="78b0c-121">On the Counter page, select the **Click me** button to increment the counter without a page refresh.</span></span> <span data-ttu-id="78b0c-122">Per incrementare un contatore in una pagina Web è in genere richiesta la scrittura di codice JavaScript, ma Razor Components offre un approccio migliore usando C#.</span><span class="sxs-lookup"><span data-stu-id="78b0c-122">Incrementing a counter in a webpage normally requires writing JavaScript, but Razor Components provides a better approach using C#.</span></span>

1. <span data-ttu-id="78b0c-123">Esaminare l'implementazione del componente Counter nel file *Counter.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="78b0c-123">Examine the implementation of the Counter component in the *Counter.cshtml* file.</span></span>

   <span data-ttu-id="78b0c-124">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="78b0c-124">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.cshtml)]

   <span data-ttu-id="78b0c-125">L'interfaccia utente del componente Counter viene definita tramite codice HTML.</span><span class="sxs-lookup"><span data-stu-id="78b0c-125">The UI of the Counter component is defined using HTML.</span></span> <span data-ttu-id="78b0c-126">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="78b0c-126">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="78b0c-127">Il markup HTML e la logica di rendering C# vengono convertiti un una classe del componente in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="78b0c-127">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="78b0c-128">Il nome della classe .NET generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="78b0c-128">The name of the generated .NET class matches the file name.</span></span>

   <span data-ttu-id="78b0c-129">I membri della classe del componente sono definiti in un blocco `@functions`.</span><span class="sxs-lookup"><span data-stu-id="78b0c-129">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="78b0c-130">Nel blocco `@functions`, lo stato del componente (proprietà, campi) e i metodi vengono specificati per la gestione degli eventi o per definire la logica di altri componenti.</span><span class="sxs-lookup"><span data-stu-id="78b0c-130">In the `@functions` block, component state (properties, fields) and methods are specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="78b0c-131">Questi membri vengono quindi usati come parte della logica di rendering del componente e per la gestione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="78b0c-131">These members are then used as part of the component's rendering logic and for handling events.</span></span>

   <span data-ttu-id="78b0c-132">Quando viene selezionato il pulsante **Click me**:</span><span class="sxs-lookup"><span data-stu-id="78b0c-132">When the **Click me** button is selected:</span></span>

   * <span data-ttu-id="78b0c-133">Viene chiamato il gestore `onclick` registrato del componente Counter (metodo `IncrementCount`).</span><span class="sxs-lookup"><span data-stu-id="78b0c-133">The Counter component's registered `onclick` handler is called (the `IncrementCount` method).</span></span>
   * <span data-ttu-id="78b0c-134">Il componente Counter rigenera il relativo albero di rendering.</span><span class="sxs-lookup"><span data-stu-id="78b0c-134">The Counter component regenerates its render tree.</span></span>
   * <span data-ttu-id="78b0c-135">Il nuovo albero di rendering viene confrontato con quello precedente.</span><span class="sxs-lookup"><span data-stu-id="78b0c-135">The new render tree is compared to the previous one.</span></span>
   * <span data-ttu-id="78b0c-136">Vengono applicate solo le modifiche al modello DOM (Document Object Model).</span><span class="sxs-lookup"><span data-stu-id="78b0c-136">Only modifications to the Document Object Model (DOM) are applied.</span></span> <span data-ttu-id="78b0c-137">Il conteggio visualizzato viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="78b0c-137">The displayed count is updated.</span></span>

1. <span data-ttu-id="78b0c-138">Modificare la logica C# del componente Counter per incrementare il contatore di due unità anziché una.</span><span class="sxs-lookup"><span data-stu-id="78b0c-138">Modify the C# logic of the Counter component to make the count increment by two instead of one.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.cshtml?highlight=14)]

1. <span data-ttu-id="78b0c-139">Ricompilare ed eseguire l'app per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="78b0c-139">Rebuild and run the app to see the changes.</span></span> <span data-ttu-id="78b0c-140">Selezionare il pulsante **Click me** e il contatore viene incrementato di due unità.</span><span class="sxs-lookup"><span data-stu-id="78b0c-140">Select the **Click me** button, and the counter increments by two.</span></span>

## <a name="use-components"></a><span data-ttu-id="78b0c-141">Usare i componenti</span><span class="sxs-lookup"><span data-stu-id="78b0c-141">Use components</span></span>

<span data-ttu-id="78b0c-142">Includere un componente in un altro componente usando una sintassi simile a HTML.</span><span class="sxs-lookup"><span data-stu-id="78b0c-142">Include a component into another component using an HTML-like syntax.</span></span>

1. <span data-ttu-id="78b0c-143">Aggiungere il componente Counter al componente Index (home page) dell'app aggiungendo un elemento `<Counter />` al componente Index.</span><span class="sxs-lookup"><span data-stu-id="78b0c-143">Add the Counter component to the app's Index (home page) component by adding a `<Counter />` element to the Index component.</span></span>

   <span data-ttu-id="78b0c-144">Se si usa Blazor per questa esperienza, il componente Index include un componente Survey Prompt (elemento `<SurveyPrompt>`).</span><span class="sxs-lookup"><span data-stu-id="78b0c-144">If you're using Blazor for this experience, a Survey Prompt component (`<SurveyPrompt>` element) is in the Index component.</span></span> <span data-ttu-id="78b0c-145">Sostituire l'elemento `<SurveyPrompt>` con l'elemento `<Counter>`.</span><span class="sxs-lookup"><span data-stu-id="78b0c-145">Replace the `<SurveyPrompt>` element with the `<Counter>` element.</span></span>

   <span data-ttu-id="78b0c-146">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="78b0c-146">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.cshtml?highlight=7)]

1. <span data-ttu-id="78b0c-147">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="78b0c-147">Rebuild and run the app.</span></span> <span data-ttu-id="78b0c-148">La home page ha un contatore proprio.</span><span class="sxs-lookup"><span data-stu-id="78b0c-148">The home page has its own counter.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="78b0c-149">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="78b0c-149">Component parameters</span></span>

<span data-ttu-id="78b0c-150">I componenti possono avere anche parametri,</span><span class="sxs-lookup"><span data-stu-id="78b0c-150">Components can also have parameters.</span></span> <span data-ttu-id="78b0c-151">che vengono definiti usando proprietà non pubbliche nella classe del componente decorata con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="78b0c-151">Component parameters are defined using non-public properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="78b0c-152">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="78b0c-152">Use attributes to specify arguments for a component in markup.</span></span>

1. <span data-ttu-id="78b0c-153">Aggiornare il codice C# `@functions` del componente:</span><span class="sxs-lookup"><span data-stu-id="78b0c-153">Update the component's `@functions` C# code:</span></span>

   * <span data-ttu-id="78b0c-154">Aggiungere una proprietà `IncrementAmount` decorata con l'attributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="78b0c-154">Add a `IncrementAmount` property decorated with the `[Parameter]` attribute.</span></span>
   * <span data-ttu-id="78b0c-155">Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.</span><span class="sxs-lookup"><span data-stu-id="78b0c-155">Change the `IncrementCount` method to use the `IncrementAmount` when increasing the value of `currentCount`.</span></span>

   <span data-ttu-id="78b0c-156">*Pages/Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="78b0c-156">*Pages/Counter.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Counter.cshtml?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. <span data-ttu-id="78b0c-157">Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Home usando un attributo.</span><span class="sxs-lookup"><span data-stu-id="78b0c-157">Specify an `IncrementAmount` parameter in the Home component's `<Counter>` element using an attribute.</span></span> <span data-ttu-id="78b0c-158">Impostare il valore per incrementare il contatore di dieci unità.</span><span class="sxs-lookup"><span data-stu-id="78b0c-158">Set the value to increment the counter by ten.</span></span>

   <span data-ttu-id="78b0c-159">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="78b0c-159">*Pages/Index.cshtml*:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Index.cshtml?highlight=7)]

1. <span data-ttu-id="78b0c-160">Ricaricare la pagina.</span><span class="sxs-lookup"><span data-stu-id="78b0c-160">Reload the page.</span></span> <span data-ttu-id="78b0c-161">Il contatore della home page viene incrementato di dieci unità ogni volta che viene selezionato il pulsante **Click me**.</span><span class="sxs-lookup"><span data-stu-id="78b0c-161">The home page counter increments by ten each time the **Click me** button is selected.</span></span> <span data-ttu-id="78b0c-162">Il contatore nella pagina *Counter* viene incrementato di un'unità.</span><span class="sxs-lookup"><span data-stu-id="78b0c-162">The counter on the *Counter* page increments by one.</span></span>

## <a name="route-to-components"></a><span data-ttu-id="78b0c-163">Indirizzare le richieste ai componenti</span><span class="sxs-lookup"><span data-stu-id="78b0c-163">Route to components</span></span>

<span data-ttu-id="78b0c-164">La direttiva `@page` all'inizio del file *Counter.cshtml* specifica che questo componente è un endpoint di routing.</span><span class="sxs-lookup"><span data-stu-id="78b0c-164">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a routing endpoint.</span></span> <span data-ttu-id="78b0c-165">Il componente Counter gestisce le richieste inviate a `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="78b0c-165">The Counter component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="78b0c-166">Senza la direttiva `@page`, il componente non gestisce le richieste indirizzate, ma può ancora essere usato da altri componenti.</span><span class="sxs-lookup"><span data-stu-id="78b0c-166">Without the `@page` directive, the component doesn't handle routed requests, but the component can still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="78b0c-167">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="78b0c-167">Dependency injection</span></span>

<span data-ttu-id="78b0c-168">I servizi registrati nel contenitore del servizio dell'app sono disponibili per i componenti tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="78b0c-168">Services registered in the app's service container are available to components via [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="78b0c-169">Inserire i servizi in un componente usando la direttiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="78b0c-169">Inject services into a component using the `@inject` directive.</span></span>

<span data-ttu-id="78b0c-170">Esaminare le direttive del componente FetchData (*Pages/FetchData.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="78b0c-170">Examine the directives of the FetchData component (*Pages/FetchData.cshtml*).</span></span> <span data-ttu-id="78b0c-171">La direttiva `@inject` viene usata per inserire l'istanza del servizio `WeatherForecastService` nel componente:</span><span class="sxs-lookup"><span data-stu-id="78b0c-171">The `@inject` directive is used to inject the instance of the `WeatherForecastService` service into the component:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=3)]

<span data-ttu-id="78b0c-172">Il servizio `WeatherForecastService` viene registrato come [singleton](xref:fundamentals/dependency-injection#service-lifetimes), in modo che un'istanza del servizio sia disponibile per tutta l'app.</span><span class="sxs-lookup"><span data-stu-id="78b0c-172">The `WeatherForecastService` service is registered as a [singleton](xref:fundamentals/dependency-injection#service-lifetimes), so one instance of the service is available throughout the app.</span></span>

<span data-ttu-id="78b0c-173">Il componente FetchData usa il servizio inserito, come `ForecastService`, per recuperare una matrice di oggetti `WeatherForecast`:</span><span class="sxs-lookup"><span data-stu-id="78b0c-173">The FetchData component uses the injected service, as `ForecastService`, to retrieve an array of `WeatherForecast` objects:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.cshtml?highlight=6)]

<span data-ttu-id="78b0c-174">Viene usato un ciclo [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) per eseguire il rendering di ogni istanza di previsione come una riga nella tabella dei dati meteo:</span><span class="sxs-lookup"><span data-stu-id="78b0c-174">A [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) loop is used to render each forecast instance as a row in the table of weather data:</span></span>

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.cshtml?highlight=11-19)]

## <a name="build-a-todo-list"></a><span data-ttu-id="78b0c-175">Compilare un elenco attività</span><span class="sxs-lookup"><span data-stu-id="78b0c-175">Build a todo list</span></span>

<span data-ttu-id="78b0c-176">Aggiungere una nuova pagina all'app che implementa un semplice elenco attività.</span><span class="sxs-lookup"><span data-stu-id="78b0c-176">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="78b0c-177">Aggiungere un file vuoto alla cartella *Pages* denominata *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="78b0c-177">Add an empty file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="78b0c-178">Specificare il markup iniziale per la pagina:</span><span class="sxs-lookup"><span data-stu-id="78b0c-178">Provide the initial markup for the page:</span></span>

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. <span data-ttu-id="78b0c-179">Aggiungere la pagina Todo alla barra di spostamento.</span><span class="sxs-lookup"><span data-stu-id="78b0c-179">Add the Todo page to the navigation bar.</span></span>

   <span data-ttu-id="78b0c-180">Il componente NavMenu (*Shared/NavMenu.csthml*) viene usato nel layout dell'app.</span><span class="sxs-lookup"><span data-stu-id="78b0c-180">The NavMenu component (*Shared/NavMenu.csthml*) is used in the app's layout.</span></span> <span data-ttu-id="78b0c-181">I layout sono componenti che consentono di evitare la duplicazione del contenuto nell'app.</span><span class="sxs-lookup"><span data-stu-id="78b0c-181">Layouts are components that allow you to avoid duplication of content in the app.</span></span> <span data-ttu-id="78b0c-182">Per ulteriori informazioni, vedere <xref:razor-components/layouts>.</span><span class="sxs-lookup"><span data-stu-id="78b0c-182">For more information, see <xref:razor-components/layouts>.</span></span>

   <span data-ttu-id="78b0c-183">Aggiungere un `<NavLink>` per la pagina Todo aggiungendo il markup seguente per l'elemento di elenco sotto gli elementi di elenco esistenti nel file *Shared/NavMenu.csthml*:</span><span class="sxs-lookup"><span data-stu-id="78b0c-183">Add a `<NavLink>` for the Todo page by adding the following list item markup below the existing list items in the *Shared/NavMenu.csthml* file:</span></span>

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. <span data-ttu-id="78b0c-184">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="78b0c-184">Rebuild and run the app.</span></span> <span data-ttu-id="78b0c-185">Visitare la nuova pagina Todo per confermare che il collegamento alla pagina Todo funziona.</span><span class="sxs-lookup"><span data-stu-id="78b0c-185">Visit the new Todo page to confirm that the link to the Todo page works.</span></span>

1. <span data-ttu-id="78b0c-186">Aggiungere un file *TodoItem.cs* nella radice del progetto per contenere una classe per rappresentare un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="78b0c-186">Add a *TodoItem.cs* file to the root of the project to hold a class that represents a todo item.</span></span> <span data-ttu-id="78b0c-187">Usare il codice C# seguente per la classe `TodoItem`:</span><span class="sxs-lookup"><span data-stu-id="78b0c-187">Use the following C# code for the `TodoItem` class:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/TodoItem.cs)]

1. <span data-ttu-id="78b0c-188">Tornare al componente Todo (*Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="78b0c-188">Return to the Todo component (*Todo.cshtml*):</span></span>

   * <span data-ttu-id="78b0c-189">Aggiungere un campo per le attività in un blocco `@functions`.</span><span class="sxs-lookup"><span data-stu-id="78b0c-189">Add a field for the todos in an `@functions` block.</span></span> <span data-ttu-id="78b0c-190">Il componente Todo usa questo campo per mantenere lo stato dell'elenco attività.</span><span class="sxs-lookup"><span data-stu-id="78b0c-190">The Todo component uses this field to maintain the state of the todo list.</span></span>
   * <span data-ttu-id="78b0c-191">Aggiungere il markup per l'elenco non ordinato e un ciclo `foreach` per eseguire il rendering di ogni elemento attività come elemento di elenco.</span><span class="sxs-lookup"><span data-stu-id="78b0c-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData4.cshtml?highlight=5-10,12-14)]

1. <span data-ttu-id="78b0c-192">L'app richiede elementi dell'interfaccia utente per l'aggiunta di attività all'elenco.</span><span class="sxs-lookup"><span data-stu-id="78b0c-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="78b0c-193">Aggiungere un input di testo e un pulsante sotto l'elenco:</span><span class="sxs-lookup"><span data-stu-id="78b0c-193">Add a text input and a button below the list:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData5.cshtml?highlight=12-13)]

1. <span data-ttu-id="78b0c-194">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="78b0c-194">Rebuild and run the app.</span></span> <span data-ttu-id="78b0c-195">Quando viene selezionato il pulsante **Add todo** non accade nulla perché al pulsante non è associato un gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="78b0c-195">When the **Add todo** button is selected, nothing happens because an event handler isn't wired up to the button.</span></span>

1. <span data-ttu-id="78b0c-196">Aggiungere un metodo `AddTodo` al componente Todo e registrarlo per i clic sul pulsante con l'attributo `onclick`:</span><span class="sxs-lookup"><span data-stu-id="78b0c-196">Add an `AddTodo` method to the Todo component and register it for button clicks using the `onclick` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData6.cshtml?highlight=2,7-10)]

   <span data-ttu-id="78b0c-197">Il metodo C# `AddTodo` viene chiamato quando viene selezionato il pulsante.</span><span class="sxs-lookup"><span data-stu-id="78b0c-197">The `AddTodo` C# method is called when the button is selected.</span></span>

1. <span data-ttu-id="78b0c-198">Per ottenere il titolo del nuovo elemento attività, aggiungere un campo stringa `newTodo` e associarlo al valore dell'input di testo tramite l'attributo `bind`:</span><span class="sxs-lookup"><span data-stu-id="78b0c-198">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData7.cshtml?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo" />
   ```

1. <span data-ttu-id="78b0c-199">Aggiornare il metodo `AddTodo` per aggiungere l'elemento `TodoItem` con il titolo specificato all'elenco.</span><span class="sxs-lookup"><span data-stu-id="78b0c-199">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="78b0c-200">Cancellare il valore dell'input di testo impostando `newTodo` su una stringa vuota:</span><span class="sxs-lookup"><span data-stu-id="78b0c-200">Clear the value of the text input by setting `newTodo` to an empty string:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData8.cshtml?highlight=19-26)]

1. <span data-ttu-id="78b0c-201">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="78b0c-201">Rebuild and run the app.</span></span> <span data-ttu-id="78b0c-202">Aggiungere alcune attività all'elenco attività per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="78b0c-202">Add some todos to the todo list to test the new code.</span></span>

1. <span data-ttu-id="78b0c-203">Il testo del titolo per ogni elemento attività può essere reso modificabile e una casella di controllo può consentire all'utente di tenere traccia degli elementi completati.</span><span class="sxs-lookup"><span data-stu-id="78b0c-203">The title text for each todo item can be made editable and a check box can help the user keep track of completed items.</span></span> <span data-ttu-id="78b0c-204">Aggiungere un input casella di controllo per ogni elemento attività e associarne il valore alla proprietà `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="78b0c-204">Add a check box input for each todo item and bind its value to the `IsDone` property.</span></span> <span data-ttu-id="78b0c-205">Modificare `@todo.Title` in un elemento `<input>` associato a `@todo.Title`:</span><span class="sxs-lookup"><span data-stu-id="78b0c-205">Change `@todo.Title` to an `<input>` element bound to `@todo.Title`:</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData9.cshtml?highlight=5-6)]

1. <span data-ttu-id="78b0c-206">Per verificare che questi valori siano associati, aggiornare l'intestazione `<h1>` per visualizzare un conteggio del numero di elementi attività non completati (`IsDone` è `false`).</span><span class="sxs-lookup"><span data-stu-id="78b0c-206">To verify that these values are bound, update the `<h1>` header to show a count of the number of todo items that aren't complete (`IsDone` is `false`).</span></span>

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. <span data-ttu-id="78b0c-207">Il componente Todo completato (*Todo.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="78b0c-207">The completed Todo component (*Todo.cshtml*):</span></span>

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/RazorComponents.App/Pages/Todo.cshtml)]

1. <span data-ttu-id="78b0c-208">Ricompilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="78b0c-208">Rebuild and run the app.</span></span> <span data-ttu-id="78b0c-209">Aggiungere elementi attività per testare il nuovo codice.</span><span class="sxs-lookup"><span data-stu-id="78b0c-209">Add todo items to test the new code.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="78b0c-210">Pubblicare e distribuire l'app</span><span class="sxs-lookup"><span data-stu-id="78b0c-210">Publish and deploy the app</span></span>

<span data-ttu-id="78b0c-211">Per pubblicare l'app, vedere <xref:host-and-deploy/razor-components/index#publish-the-app>.</span><span class="sxs-lookup"><span data-stu-id="78b0c-211">To publish the app, see <xref:host-and-deploy/razor-components/index#publish-the-app>.</span></span>
