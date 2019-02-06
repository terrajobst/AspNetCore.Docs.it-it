---
title: Creare la prima app Blazor
author: guardrex
description: Tutti i passaggi per creare un'app Blazor e informazioni per apprendere rapidamente le funzionalità di base del framework Razor Components.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: 99396e200eb121524abccc20a7d461062c94ecc7
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668033"
---
# <a name="build-your-first-blazor-app"></a><span data-ttu-id="dae93-103">Creare la prima app Blazor</span><span class="sxs-lookup"><span data-stu-id="dae93-103">Build your first Blazor app</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="dae93-104">In questa esercitazione si eseguiranno tutti i passaggi per creare un'app Blazor e imparare velocemente a conoscere le funzionalità di base del framework Razor Components.</span><span class="sxs-lookup"><span data-stu-id="dae93-104">In this tutorial, you build a Blazor app step-by-step and quickly learn the basic features of the Razor Components framework.</span></span>

<span data-ttu-id="dae93-105">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="dae93-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="dae93-106">Vedere l'argomento [Introduzione a Razor Components](xref:razor-components/get-started) per i prerequisiti.</span><span class="sxs-lookup"><span data-stu-id="dae93-106">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

<span data-ttu-id="dae93-107">Per creare il progetto in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="dae93-107">To create the project in Visual Studio:</span></span>

1. <span data-ttu-id="dae93-108">Selezionare **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="dae93-108">Select **File** > **New** > **Project**.</span></span> <span data-ttu-id="dae93-109">Selezionare **Web** > **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="dae93-109">Select **Web** > **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="dae93-110">Denominare il progetto "BlazorApp1" nel campo **Nome**.</span><span class="sxs-lookup"><span data-stu-id="dae93-110">Name the project "BlazorApp1" in the **Name** field.</span></span> <span data-ttu-id="dae93-111">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="dae93-111">Select **OK**.</span></span>

    ![Nuovo progetto ASP.NET Core](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. <span data-ttu-id="dae93-113">Verrà visualizzata la finestra di dialogo **Nuova applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="dae93-113">The **New ASP.NET Core Web Application** dialog appears.</span></span> <span data-ttu-id="dae93-114">Assicurarsi che **.NET Core** sia selezionato nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="dae93-114">Make sure **.NET Core** is selected at the top.</span></span> <span data-ttu-id="dae93-115">Selezionare **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="dae93-115">Select **ASP.NET Core 2.1**.</span></span> <span data-ttu-id="dae93-116">Scegliere il modello **Blazor** e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="dae93-116">Choose the **Blazor** template and select **OK**.</span></span>

    ![Finestra di dialogo per una nuova app Blazor](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. <span data-ttu-id="dae93-118">Dopo aver creato il progetto, premere **CTRL+F5** per eseguire l'app *senza il debugger*.</span><span class="sxs-lookup"><span data-stu-id="dae93-118">Once the project is created, press **Ctrl-F5** to run the app *without the debugger*.</span></span> <span data-ttu-id="dae93-119">L'esecuzione con il debugger (**F5**) non è supportata in questo momento.</span><span class="sxs-lookup"><span data-stu-id="dae93-119">Running with the debugger (**F5**) isn't supported at this time.</span></span>

> [!NOTE]
> <span data-ttu-id="dae93-120">Se non si usa Visual Studio, creare l'app Blazor da un prompt dei comandi in Windows, macOS o Linux:</span><span class="sxs-lookup"><span data-stu-id="dae93-120">If not using Visual Studio, create the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> <span data-ttu-id="dae93-121">Passare all'app usando l'indirizzo localhost e la porta specificata nell'output della finestra della console dopo l'esecuzione di `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="dae93-121">Navigate to the app using the localhost address and port provided in the console window output after `dotnet run` is executed.</span></span> <span data-ttu-id="dae93-122">Usare **CTRL+C** nella finestra della console per arrestare l'app.</span><span class="sxs-lookup"><span data-stu-id="dae93-122">Use **Ctrl-C** in the console window to shutdown the app.</span></span>

<span data-ttu-id="dae93-123">L'app Blazor viene eseguita nel browser:</span><span class="sxs-lookup"><span data-stu-id="dae93-123">The Blazor app runs in the browser:</span></span>

![Home page dell'app Blazor](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a><span data-ttu-id="dae93-125">Compilare i componenti</span><span class="sxs-lookup"><span data-stu-id="dae93-125">Build components</span></span>

1. <span data-ttu-id="dae93-126">Passare a ognuna delle tre pagine dell'app: Home, Counter e Fetch data (Home, Contatore e Recupera dati).</span><span class="sxs-lookup"><span data-stu-id="dae93-126">Browse to each of the app's three pages: Home, Counter, and Fetch data.</span></span>

    <span data-ttu-id="dae93-127">Queste tre pagine vengono implementate dai tre file Razor nella cartella *Pages*: *Index.cshtml*, *Counter.cshtml* e *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dae93-127">These three pages are implemented by the three Razor files in the *Pages* folder: *Index.cshtml*, *Counter.cshtml*, and *FetchData.cshtml*.</span></span> <span data-ttu-id="dae93-128">Ognuno di questi file implementa un componente che viene compilato ed eseguito sul lato client nel browser.</span><span class="sxs-lookup"><span data-stu-id="dae93-128">Each of these files implements a component that's compiled and executed client-side in the browser.</span></span>

1. <span data-ttu-id="dae93-129">Selezionare il pulsante nella pagina Counter.</span><span class="sxs-lookup"><span data-stu-id="dae93-129">Select the button on the Counter page.</span></span>

    ![Home page dell'app Blazor](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    <span data-ttu-id="dae93-131">Ogni volta che viene selezionato il pulsante, il contatore viene incrementato senza un aggiornamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="dae93-131">Each time the button is selected, the counter is incremented without a page refresh.</span></span> <span data-ttu-id="dae93-132">In genere, questo tipo di comportamento sul lato client viene gestito in JavaScript, ma in questo caso è implementato in C# e .NET dal componente `Counter`.</span><span class="sxs-lookup"><span data-stu-id="dae93-132">Normally, this kind of client-side behavior is handled in JavaScript; but here, it's implemented in C# and .NET by the `Counter` component.</span></span>

1. <span data-ttu-id="dae93-133">Questa è l'implementazione del componente `Counter` nel file *Counter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="dae93-133">Take a look at the implementation of the `Counter` component in the *Counter.cshtml* file:</span></span>

    ```cshtml
    @page "/counter"

    <h1>Counter</h1>

    <p>Current count: @currentCount</p>

    <button class="btn btn-primary" onclick="@IncrementCount">Click me</button>

    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
    ```

    <span data-ttu-id="dae93-134">L'interfaccia utente per il componente `Counter` viene definita mediante HTML normale.</span><span class="sxs-lookup"><span data-stu-id="dae93-134">The UI for the `Counter` component is defined using normal HTML.</span></span> <span data-ttu-id="dae93-135">La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="dae93-135">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor).</span></span> <span data-ttu-id="dae93-136">Il markup HTML e la logica di rendering C# vengono convertiti un una classe del componente in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="dae93-136">The HTML markup and C# rendering logic are converted into a component class at build time.</span></span> <span data-ttu-id="dae93-137">Il nome della classe .NET generata corrisponde al nome del file.</span><span class="sxs-lookup"><span data-stu-id="dae93-137">The name of the generated .NET class matches the name of the file.</span></span>

    <span data-ttu-id="dae93-138">I membri della classe del componente sono definiti in un blocco `@functions`.</span><span class="sxs-lookup"><span data-stu-id="dae93-138">Members of the component class are defined in a `@functions` block.</span></span> <span data-ttu-id="dae93-139">Nel blocco `@functions`, lo stato del componente (proprietà, campi) e i metodi possono essere specificati per la gestione degli eventi o per definire la logica di altri componenti.</span><span class="sxs-lookup"><span data-stu-id="dae93-139">In the `@functions` block, component state (properties, fields) and methods can be specified for event handling or for defining other component logic.</span></span> <span data-ttu-id="dae93-140">Questi membri possono quindi essere usati come parte della logica di rendering del componente e per la gestione degli eventi.</span><span class="sxs-lookup"><span data-stu-id="dae93-140">These members can then be used as part of the component's rendering logic and for handling events.</span></span>

    <span data-ttu-id="dae93-141">Quando si seleziona il pulsante, viene chiamato il gestore `onclick` registrato del componente `Counter` (metodo `IncrementCount` ) e il componente `Counter` rigenera l'albero di rendering.</span><span class="sxs-lookup"><span data-stu-id="dae93-141">When the button is selected, the `Counter` component's registered `onclick` handler is called (the `IncrementCount` method) and the `Counter` component regenerates its render tree.</span></span> <span data-ttu-id="dae93-142">Blazor confronta il nuovo albero di rendering con quello precedente e applica eventuali modifiche al modello DOM (Document Object Model) del browser.</span><span class="sxs-lookup"><span data-stu-id="dae93-142">Blazor compares the new render tree against the previous one and applies any modifications to the browser Document Object Model (DOM).</span></span> <span data-ttu-id="dae93-143">Il conteggio visualizzato viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="dae93-143">The displayed count is updated.</span></span>

1. <span data-ttu-id="dae93-144">Aggiornare il markup per il componente `Counter` per rendere più *interessante* l'intestazione di primo livello.</span><span class="sxs-lookup"><span data-stu-id="dae93-144">Update the markup for the `Counter` component to make the top-level header more *exciting*.</span></span>

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. <span data-ttu-id="dae93-145">Modificare anche la logica C# del componente `Counter` per incrementare il contatore di due unità anziché una.</span><span class="sxs-lookup"><span data-stu-id="dae93-145">Also modify the C# logic of the `Counter` component to make the count increment by two instead of one.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. <span data-ttu-id="dae93-146">Aggiornare la pagina del contatore nel browser per visualizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="dae93-146">Refresh the counter page in the browser to see the changes.</span></span>

    ![Contatore interessante](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a><span data-ttu-id="dae93-148">Usare i componenti</span><span class="sxs-lookup"><span data-stu-id="dae93-148">Use components</span></span>

<span data-ttu-id="dae93-149">Dopo aver definito un componente, il componente può essere usato per implementare altri componenti.</span><span class="sxs-lookup"><span data-stu-id="dae93-149">After a component is defined, the component can be used to implement other components.</span></span> <span data-ttu-id="dae93-150">Il markup per l'uso di un componente è simile a un tag HTML, in cui il nome del tag è il tipo di componente.</span><span class="sxs-lookup"><span data-stu-id="dae93-150">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

1. <span data-ttu-id="dae93-151">Aggiungere un componente `Counter` alla home page dell'app (*index.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="dae93-151">Add a `Counter` component to the Home page of the app (*Index.cshtml*).</span></span>

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. <span data-ttu-id="dae93-152">Aggiornare la home page nel browser.</span><span class="sxs-lookup"><span data-stu-id="dae93-152">Refresh the home page in the browser.</span></span> <span data-ttu-id="dae93-153">Si noti l'istanza separata del componente `Counter` nella home page.</span><span class="sxs-lookup"><span data-stu-id="dae93-153">Note the separate instance of the `Counter` component on the Home page.</span></span>

    ![Home page di Blazor con il contatore](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a><span data-ttu-id="dae93-155">Parametri del componente</span><span class="sxs-lookup"><span data-stu-id="dae93-155">Component parameters</span></span>

<span data-ttu-id="dae93-156">I componenti possono avere anche parametri, che vengono definiti usando proprietà private nella classe del componente decorata con `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="dae93-156">Components can also have parameters, which are defined using private properties on the component class decorated with `[Parameter]`.</span></span> <span data-ttu-id="dae93-157">Usare gli attributi per specificare gli argomenti per un componente nel markup.</span><span class="sxs-lookup"><span data-stu-id="dae93-157">Use attributes to specify arguments for a component in markup.</span></span> 

1. <span data-ttu-id="dae93-158">Aggiornare il componente `Counter` per ottenere un parametro `IncrementAmount` con impostazione predefinita 1.</span><span class="sxs-lookup"><span data-stu-id="dae93-158">Update the `Counter` component to have an `IncrementAmount` parameter that defaults to 1.</span></span>

    ```cshtml
    @functions {
        int currentCount = 0;

        [Parameter]
        private int IncrementAmount { get; set; } = 1;

        void IncrementCount()
        {
            currentCount += IncrementAmount;
        }
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="dae93-159">Da Visual Studio è possibile aggiungere rapidamente un parametro del componente tramite il frammento di codice `para`.</span><span class="sxs-lookup"><span data-stu-id="dae93-159">From Visual Studio, you can quickly add a component parameter by using the `para` snippet.</span></span> <span data-ttu-id="dae93-160">Digitare `para` e quindi premere `Tab` due volte.</span><span class="sxs-lookup"><span data-stu-id="dae93-160">Type `para` and then press the `Tab` key twice.</span></span>

1. <span data-ttu-id="dae93-161">Nella home page (*index.cshtml*), modificare la quantità di incremento per `Counter` a 10 impostando un attributo che corrisponde al nome della proprietà del componente per `IncrementCount`.</span><span class="sxs-lookup"><span data-stu-id="dae93-161">On the Home page (*Index.cshtml*), change the increment amount for the `Counter` to 10 by setting an attribute that matches the name of the component's property for `IncrementCount`.</span></span>

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. <span data-ttu-id="dae93-162">Ricaricare la pagina.</span><span class="sxs-lookup"><span data-stu-id="dae93-162">Reload the page.</span></span>

    <span data-ttu-id="dae93-163">Il contatore nella home page viene ora incrementato di 10 unità, mentre il contatore nella pagina Counter viene ancora incrementato di 1.</span><span class="sxs-lookup"><span data-stu-id="dae93-163">The counter on the Home page now increments by 10, while the counter on the Counter page still increments by 1.</span></span>

    ![Conteggio Blazor con incrementi per dieci](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a><span data-ttu-id="dae93-165">Indirizzare le richieste ai componenti</span><span class="sxs-lookup"><span data-stu-id="dae93-165">Route to components</span></span>

<span data-ttu-id="dae93-166">La direttiva `@page` all'inizio del file *Counter.cshtml* specifica che questo componente è una pagina a cui possono essere indirizzate le richieste.</span><span class="sxs-lookup"><span data-stu-id="dae93-166">The `@page` directive at the top of the *Counter.cshtml* file specifies that this component is a page to which requests can be routed.</span></span> <span data-ttu-id="dae93-167">In particolare, il componente `Counter` gestisce le richieste inviate a `/Counter`.</span><span class="sxs-lookup"><span data-stu-id="dae93-167">Specifically, the `Counter` component handles requests sent to `/Counter`.</span></span> <span data-ttu-id="dae93-168">Senza la direttiva `@page`, il componente non gestirebbe le richieste indirizzate, ma potrebbe ancora essere usato da altri componenti.</span><span class="sxs-lookup"><span data-stu-id="dae93-168">Without the `@page` directive, the component wouldn't handle routed requests, but the component could still be used by other components.</span></span>

## <a name="dependency-injection"></a><span data-ttu-id="dae93-169">Inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="dae93-169">Dependency injection</span></span>

<span data-ttu-id="dae93-170">I servizi registrati per il provider di servizi dell'app sono disponibili per i componenti tramite [inserimento delle dipendenze](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="dae93-170">Services registered with the app's service provider are available to components via [dependency injection (DI)](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection).</span></span> <span data-ttu-id="dae93-171">I servizi possono essere inseriti in un componente usando la direttiva `@inject`.</span><span class="sxs-lookup"><span data-stu-id="dae93-171">Services can be injected into a component using the `@inject` directive.</span></span>

<span data-ttu-id="dae93-172">Di seguito è riportata l'implementazione del componente `FetchData` nel file *FetchData.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dae93-172">Take a look at the implementation of the `FetchData` component in *FetchData.cshtml*.</span></span> <span data-ttu-id="dae93-173">La direttiva `@inject` viene usata per inserire un'istanza [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) nel componente.</span><span class="sxs-lookup"><span data-stu-id="dae93-173">The `@inject` directive is used to inject an [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) instance into the component.</span></span>

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

<span data-ttu-id="dae93-174">Il componente `FetchData` usa l'istanza `HttpClient` inserita per recuperare i dati JSON dal server quando il componente viene inizializzato.</span><span class="sxs-lookup"><span data-stu-id="dae93-174">The `FetchData` component uses the injected `HttpClient` to retrieve JSON data from the server when the component is initialized.</span></span> <span data-ttu-id="dae93-175">Dietro le quinte, l'istanza `HttpClient` fornita dal runtime di Blazor viene implementata tramite l'interoperabilità JavaScript per chiamare l'API Fetch del browser sottostante per l'invio della richiesta (da C# è possibile chiamare qualsiasi libreria JavaScript o API browser).</span><span class="sxs-lookup"><span data-stu-id="dae93-175">Under the covers, the `HttpClient` provided by the Blazor runtime is implemented using JavaScript interop to call the underlying browser's Fetch API to send the request (from C#, it's possible to call any JavaScript library or browser API).</span></span> <span data-ttu-id="dae93-176">I dati recuperati vengono deserializzati nella variabile C# `forecasts` come matrice di oggetti `WeatherForecast`ì.</span><span class="sxs-lookup"><span data-stu-id="dae93-176">The retrieved data is deserialized into the `forecasts` C# variable as an array of `WeatherForecast` objects.</span></span>

```cshtml
@functions {
    WeatherForecast[] forecasts;

    protected override async Task OnInitAsync()
    {
        forecasts = await Http.GetJsonAsync<WeatherForecast[]>("/sample-data/weather.json");
    }

    class WeatherForecast
    {
        public DateTime Date { get; set; }
        public int TemperatureC { get; set; }
        public int TemperatureF { get; set; }
        public string Summary { get; set; }
    }
}
```

<span data-ttu-id="dae93-177">Viene usato un ciclo `@foreach` per eseguire il rendering di ogni istanza di previsione come una riga nella tabella meteo.</span><span class="sxs-lookup"><span data-stu-id="dae93-177">A `@foreach` loop is used to render each forecast instance as a row in the weather table.</span></span>

```cshtml
<table class="table">
    <thead>
        <tr>
            <th>Date</th>
            <th>Temp. (C)</th>
            <th>Temp. (F)</th>
            <th>Summary</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var forecast in forecasts)
        {
            <tr>
                <td>@forecast.Date.ToShortDateString()</td>
                <td>@forecast.TemperatureC</td>
                <td>@forecast.TemperatureF</td>
                <td>@forecast.Summary</td>
            </tr>
        }
    </tbody>
</table>
```

## <a name="build-a-todo-list"></a><span data-ttu-id="dae93-178">Compilare un elenco attività</span><span class="sxs-lookup"><span data-stu-id="dae93-178">Build a todo list</span></span>

<span data-ttu-id="dae93-179">Aggiungere una nuova pagina all'app che implementa un semplice elenco attività.</span><span class="sxs-lookup"><span data-stu-id="dae93-179">Add a new page to the app that implements a simple todo list.</span></span>

1. <span data-ttu-id="dae93-180">Aggiungere un file di testo vuoto alla cartella *Pages* denominata *Todo.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dae93-180">Add an empty text file to the *Pages* folder named *Todo.cshtml*.</span></span>

1. <span data-ttu-id="dae93-181">Specificare il markup iniziale per la pagina.</span><span class="sxs-lookup"><span data-stu-id="dae93-181">Provide the initial markup for the page.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. <span data-ttu-id="dae93-182">Aggiungere la pagina Todo alla barra di spostamento aggiornando *Shared/NavMenu.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="dae93-182">Add the Todo page to the navigation bar by updating *Shared/NavMenu.cshtml*.</span></span> <span data-ttu-id="dae93-183">Aggiungere un `NavLink` per la pagina Todo aggiungendo il markup seguente per l'elemento di elenco sotto gli elementi di elenco esistenti.</span><span class="sxs-lookup"><span data-stu-id="dae93-183">Add a `NavLink` for the Todo page by adding the following list item markup below the existing list items.</span></span>

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. <span data-ttu-id="dae93-184">Aggiornare l'app nel browser.</span><span class="sxs-lookup"><span data-stu-id="dae93-184">Refresh the app in the browser.</span></span> <span data-ttu-id="dae93-185">Vedere la nuova pagina Todo.</span><span class="sxs-lookup"><span data-stu-id="dae93-185">See the new Todo page.</span></span>

    ![Pagina Todo Blazor iniziale](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. <span data-ttu-id="dae93-187">Aggiungere un file *TodoItem.cs* nella radice del progetto per contenere una classe per rappresentare gli elementi attività.</span><span class="sxs-lookup"><span data-stu-id="dae93-187">Add a *TodoItem.cs* file to the root of the project to hold a class to represent the todo items.</span></span>

1. <span data-ttu-id="dae93-188">Usare il codice C# seguente per la classe `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="dae93-188">Use the following C# code for the `TodoItem` class.</span></span>

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. <span data-ttu-id="dae93-189">Tornare al componente `Todo` in *Todo.cshtml* e aggiungere un campo per le attività in un blocco `@functions`.</span><span class="sxs-lookup"><span data-stu-id="dae93-189">Go back to the `Todo` component in *Todo.cshtml* and add a field for the todos in a `@functions` block.</span></span> <span data-ttu-id="dae93-190">Il componente `Todo` usa questo campo per mantenere lo stato dell'elenco attività.</span><span class="sxs-lookup"><span data-stu-id="dae93-190">The `Todo` component uses this field to maintain the state of the todo list.</span></span>

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="dae93-191">Aggiungere il markup per l'elenco non ordinato e un ciclo `foreach` per eseguire il rendering di ogni elemento attività come elemento di elenco.</span><span class="sxs-lookup"><span data-stu-id="dae93-191">Add unordered list markup and a `foreach` loop to render each todo item as a list item.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>
    ```

1. <span data-ttu-id="dae93-192">L'app richiede elementi dell'interfaccia utente per l'aggiunta di attività all'elenco.</span><span class="sxs-lookup"><span data-stu-id="dae93-192">The app requires UI elements for adding todos to the list.</span></span> <span data-ttu-id="dae93-193">Aggiungere un input testo e un pulsante sotto l'elenco.</span><span class="sxs-lookup"><span data-stu-id="dae93-193">Add a text input and a button below the list.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" />
    <button>Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. <span data-ttu-id="dae93-194">Aggiornare il browser.</span><span class="sxs-lookup"><span data-stu-id="dae93-194">Refresh the browser.</span></span>

    ![Pulsante Add todo](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    <span data-ttu-id="dae93-196">Non accade nulla quando viene selezionato il pulsante **Add todo** perché al pulsante non è associato alcun gestore eventi.</span><span class="sxs-lookup"><span data-stu-id="dae93-196">Nothing happens when the **Add todo** button is selected because no event handler is wired up to the button.</span></span>

1. <span data-ttu-id="dae93-197">Aggiungere un metodo `AddTodo` al componente `Todo` e registrarlo per i clic sul pulsante con l'attributo `onclick`.</span><span class="sxs-lookup"><span data-stu-id="dae93-197">Add an `AddTodo` method to the `Todo` component and register it for button clicks using the `onclick` attribute.</span></span>

    ```cshtml
    <input placeholder="Something todo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();

        void AddTodo()
        {
            // Todo: Add the todo
        }
    }
    ```

    <span data-ttu-id="dae93-198">Il metodo C# `AddTodo` viene chiamato ogni volta che viene selezionato il pulsante.</span><span class="sxs-lookup"><span data-stu-id="dae93-198">The `AddTodo` C# method is called every time the button is selected.</span></span>

1. <span data-ttu-id="dae93-199">Per ottenere il titolo del nuovo elemento attività, aggiungere un campo stringa `newTodo` e associarlo al valore dell'input di testo tramite l'attributo `bind`.</span><span class="sxs-lookup"><span data-stu-id="dae93-199">To get the title of the new todo item, add a `newTodo` string field and bind it to the value of the text input using the `bind` attribute.</span></span>

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. <span data-ttu-id="dae93-200">Aggiornare il metodo `AddTodo` per aggiungere l'elemento `TodoItem` con il titolo specificato all'elenco.</span><span class="sxs-lookup"><span data-stu-id="dae93-200">Update the `AddTodo` method to add the `TodoItem` with the specified title to the list.</span></span> <span data-ttu-id="dae93-201">Non dimenticare di cancellare il valore dell'input testo impostando `newTodo` su una stringa vuota.</span><span class="sxs-lookup"><span data-stu-id="dae93-201">Don't forget to clear the value of the text input by setting `newTodo` to an empty string.</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>@todo.Title</li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

1. <span data-ttu-id="dae93-202">Aggiornare il browser.</span><span class="sxs-lookup"><span data-stu-id="dae93-202">Refresh the browser.</span></span> <span data-ttu-id="dae93-203">Aggiungere alcune attività all'elenco attività.</span><span class="sxs-lookup"><span data-stu-id="dae93-203">Add some todos to the todo list.</span></span>

    ![Aggiungere attività](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. <span data-ttu-id="dae93-205">Infine, verranno aggiunte le caselle di controllo imprescindibili per un elenco attività.</span><span class="sxs-lookup"><span data-stu-id="dae93-205">Lastly, what's a todo list without check boxes?</span></span> <span data-ttu-id="dae93-206">Anche il testo del titolo per ogni elemento attività può essere reso modificabile.</span><span class="sxs-lookup"><span data-stu-id="dae93-206">The title text for each todo item can be made editable as well.</span></span> <span data-ttu-id="dae93-207">Aggiungere un input casella di controllo e un input testo per ogni elemento attività e associare i valori rispettivamente alle proprietà `Title` e `IsDone`.</span><span class="sxs-lookup"><span data-stu-id="dae93-207">Add a check box input and text input for each todo item and bind their values to the `Title` and `IsDone` properties, respectively.</span></span>

    ```cshtml
    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>
    ```

1. <span data-ttu-id="dae93-208">Per verificare che questi valori siano associati, aggiornare l'intestazione `h1` per visualizzare un conteggio del numero di elementi attività non ancora completati (`IsDone` è `false`).</span><span class="sxs-lookup"><span data-stu-id="dae93-208">To verify that these values are bound, update the `h1` header to show a count of the number of todo items that are not yet done (`IsDone` is `false`).</span></span>

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. <span data-ttu-id="dae93-209">Il componente `Todo` completato dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="dae93-209">The completed `Todo` component should look like this:</span></span>

    ```cshtml
    @page "/todo"

    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>

    <ul>
        @foreach (var todo in todos)
        {
            <li>
                <input type="checkbox" bind="@todo.IsDone" />
                <input bind="@todo.Title" />
            </li>
        }
    </ul>

    <input placeholder="Something todo" bind="@newTodo" />
    <button onclick="@AddTodo">Add todo</button>

    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
        string newTodo;

        void AddTodo()
        {
            if (!string.IsNullOrWhiteSpace(newTodo))
            {
                todos.Add(new TodoItem { Title = newTodo });
                newTodo = string.Empty;
            }
        }
    }
    ```

<span data-ttu-id="dae93-210">Aggiornare l'app nel browser.</span><span class="sxs-lookup"><span data-stu-id="dae93-210">Refresh the app in the browser.</span></span> <span data-ttu-id="dae93-211">Provare ad aggiungere alcuni elementi attività.</span><span class="sxs-lookup"><span data-stu-id="dae93-211">Try adding some todo items.</span></span>

![Elenco attività Blazor completato](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a><span data-ttu-id="dae93-213">Pubblicare e distribuire</span><span class="sxs-lookup"><span data-stu-id="dae93-213">Publish and deploy</span></span>

<span data-ttu-id="dae93-214">Quando si usa Visual Studio, seguire questa procedura per pubblicare l'app Todo Blazor in Azure:</span><span class="sxs-lookup"><span data-stu-id="dae93-214">When using Visual Studio, perform the following steps to publish the Todo Blazor app to Azure:</span></span>

1. <span data-ttu-id="dae93-215">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="dae93-215">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>

1. <span data-ttu-id="dae93-216">Nella finestra di dialogo **Selezionare una destinazione di pubblicazione** selezionare **Servizio app** e **Crea nuovo**.</span><span class="sxs-lookup"><span data-stu-id="dae93-216">In the **Pick a publish target** dialog, select **App Service** and **Create New**.</span></span> <span data-ttu-id="dae93-217">Selezionare **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="dae93-217">Select **Publish**.</span></span>

    ![Selezionare una destinazione di pubblicazione](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. <span data-ttu-id="dae93-219">Nella finestra di dialogo **Crea servizio app** scegliere un nome per l'app e selezionare la sottoscrizione, il gruppo di risorse e il piano di hosting.</span><span class="sxs-lookup"><span data-stu-id="dae93-219">In the **Create App Service** dialog, choose a name for the app and select the subscription, resource group, and hosting plan.</span></span> <span data-ttu-id="dae93-220">Selezionare **Crea** per creare il servizio app e pubblicare l'app.</span><span class="sxs-lookup"><span data-stu-id="dae93-220">Select **Create** to create the app service and publish the app.</span></span>

    ![Crea servizio app](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

<span data-ttu-id="dae93-222">Attendere più o meno un minuto per la distribuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="dae93-222">Wait a minute or so for the app to be deployed.</span></span>

<span data-ttu-id="dae93-223">L'app dovrebbe ora essere in esecuzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="dae93-223">The app should now be running in Azure.</span></span> <span data-ttu-id="dae93-224">Contrassegna l'oggetto per creare la tua prima app Blazor come *fatto*.</span><span class="sxs-lookup"><span data-stu-id="dae93-224">Mark the todo item to build your first Blazor app as *done*.</span></span> <span data-ttu-id="dae93-225">completata.</span><span class="sxs-lookup"><span data-stu-id="dae93-225">Nice job!</span></span>

![Blazor in Azure](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> <span data-ttu-id="dae93-227">Se non si usa Visual Studio, pubblicare l'app Blazor da un prompt dei comandi in Windows, macOS o Linux:</span><span class="sxs-lookup"><span data-stu-id="dae93-227">If not using Visual Studio, publish the Blazor app at a command prompt on Windows, macOS, or Linux:</span></span>
>
> ```console
> dotnet publish -c Release
> ```
>
> <span data-ttu-id="dae93-228">La distribuzione viene creata nella cartella */bin/Release/\<target-framework>/publish*.</span><span class="sxs-lookup"><span data-stu-id="dae93-228">The deployment is created in the */bin/Release/\<target-framework>/publish* folder.</span></span> <span data-ttu-id="dae93-229">Spostare il contenuto della cartella *publish* nel server o nel servizio di hosting.</span><span class="sxs-lookup"><span data-stu-id="dae93-229">Move the contents of the *publish* folder to the server or hosting service.</span></span>
>
> <span data-ttu-id="dae93-230">Per altre informazioni, vedere l'argomento [Ospitare e distribuire Razor Components](xref:host-and-deploy/razor-components/index#publish-the-app).</span><span class="sxs-lookup"><span data-stu-id="dae93-230">For more information, see the [Host and deploy](xref:host-and-deploy/razor-components/index#publish-the-app) topic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dae93-231">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="dae93-231">Additional resources</span></span>

<span data-ttu-id="dae93-232">Per un'app di esempio di Blazor più articolata, vedere l'[esempio Flight Finder](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) su GitHub.</span><span class="sxs-lookup"><span data-stu-id="dae93-232">For a more involved Blazor sample app, check out the [Flight Finder sample](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) on GitHub.</span></span>

![Blazor Flight Finder](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
