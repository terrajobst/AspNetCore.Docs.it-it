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
# <a name="build-your-first-blazor-app"></a>Creare la prima app Blazor

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

In questa esercitazione si eseguiranno tutti i passaggi per creare un'app Blazor e imparare velocemente a conoscere le funzionalità di base del framework Razor Components.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-blazor-app/samples/) ([procedura per il download](xref:index#how-to-download-a-sample)). Vedere l'argomento [Introduzione a Razor Components](xref:razor-components/get-started) per i prerequisiti.

Per creare il progetto in Visual Studio:

1. Selezionare **File** > **Nuovo** > **Progetto**. Selezionare **Web** > **Applicazione Web ASP.NET Core**. Denominare il progetto "BlazorApp1" nel campo **Nome**. Scegliere **OK**.

    ![Nuovo progetto ASP.NET Core](build-your-first-blazor-app/_static/new-aspnet-core-project.png)

1. Verrà visualizzata la finestra di dialogo **Nuova applicazione Web ASP.NET Core**. Assicurarsi che **.NET Core** sia selezionato nella parte superiore. Selezionare **ASP.NET Core 2.1**. Scegliere il modello **Blazor** e selezionare **OK**.

    ![Finestra di dialogo per una nuova app Blazor](build-your-first-blazor-app/_static/new-blazor-app-dialog.png)

1. Dopo aver creato il progetto, premere **CTRL+F5** per eseguire l'app *senza il debugger*. L'esecuzione con il debugger (**F5**) non è supportata in questo momento.

> [!NOTE]
> Se non si usa Visual Studio, creare l'app Blazor da un prompt dei comandi in Windows, macOS o Linux:
>
> ```console
> dotnet new --install "Microsoft.AspNetCore.Blazor.Templates"
> dotnet new blazor -o BlazorApp1
> cd BlazorApp1
> dotnet run
> ```
>
> Passare all'app usando l'indirizzo localhost e la porta specificata nell'output della finestra della console dopo l'esecuzione di `dotnet run`. Usare **CTRL+C** nella finestra della console per arrestare l'app.

L'app Blazor viene eseguita nel browser:

![Home page dell'app Blazor](https://user-images.githubusercontent.com/1874516/39509497-5515c3ea-4d9b-11e8-887f-019ea4fdb3ee.png)

## <a name="build-components"></a>Compilare i componenti

1. Passare a ognuna delle tre pagine dell'app: Home, Counter e Fetch data (Home, Contatore e Recupera dati).

    Queste tre pagine vengono implementate dai tre file Razor nella cartella *Pages*: *Index.cshtml*, *Counter.cshtml* e *FetchData.cshtml*. Ognuno di questi file implementa un componente che viene compilato ed eseguito sul lato client nel browser.

1. Selezionare il pulsante nella pagina Counter.

    ![Home page dell'app Blazor](https://user-images.githubusercontent.com/1874516/39509525-6e367c66-4d9b-11e8-9978-e52a9750c34b.png)

    Ogni volta che viene selezionato il pulsante, il contatore viene incrementato senza un aggiornamento della pagina. In genere, questo tipo di comportamento sul lato client viene gestito in JavaScript, ma in questo caso è implementato in C# e .NET dal componente `Counter`.

1. Questa è l'implementazione del componente `Counter` nel file *Counter.cshtml*:

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

    L'interfaccia utente per il componente `Counter` viene definita mediante HTML normale. La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](https://docs.microsoft.com/aspnet/core/mvc/views/razor). Il markup HTML e la logica di rendering C# vengono convertiti un una classe del componente in fase di compilazione. Il nome della classe .NET generata corrisponde al nome del file.

    I membri della classe del componente sono definiti in un blocco `@functions`. Nel blocco `@functions`, lo stato del componente (proprietà, campi) e i metodi possono essere specificati per la gestione degli eventi o per definire la logica di altri componenti. Questi membri possono quindi essere usati come parte della logica di rendering del componente e per la gestione degli eventi.

    Quando si seleziona il pulsante, viene chiamato il gestore `onclick` registrato del componente `Counter` (metodo `IncrementCount` ) e il componente `Counter` rigenera l'albero di rendering. Blazor confronta il nuovo albero di rendering con quello precedente e applica eventuali modifiche al modello DOM (Document Object Model) del browser. Il conteggio visualizzato viene aggiornato.

1. Aggiornare il markup per il componente `Counter` per rendere più *interessante* l'intestazione di primo livello.

    ```cshtml
    @page "/counter"
    <h1><em>Counter!!</em></h1>
    ```

1. Modificare anche la logica C# del componente `Counter` per incrementare il contatore di due unità anziché una.

    ```cshtml
    @functions {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount += 2;
        }
    }
    ```

1. Aggiornare la pagina del contatore nel browser per visualizzare le modifiche.

    ![Contatore interessante](https://user-images.githubusercontent.com/1874516/39509668-e8949a92-4d9b-11e8-91e9-b6a494695d92.png)

## <a name="use-components"></a>Usare i componenti

Dopo aver definito un componente, il componente può essere usato per implementare altri componenti. Il markup per l'uso di un componente è simile a un tag HTML, in cui il nome del tag è il tipo di componente.

1. Aggiungere un componente `Counter` alla home page dell'app (*index.cshtml*).

    ```cshtml
    @page "/"

    <h1>Hello, world!</h1>

    Welcome to your new app.

    <SurveyPrompt Title="How is Blazor working for you?" />

    <Counter />
    ```

1. Aggiornare la home page nel browser. Si noti l'istanza separata del componente `Counter` nella home page.

    ![Home page di Blazor con il contatore](https://user-images.githubusercontent.com/1874516/39509718-224483f6-4d9c-11e8-9030-b4c7228d669d.png)

## <a name="component-parameters"></a>Parametri del componente

I componenti possono avere anche parametri, che vengono definiti usando proprietà private nella classe del componente decorata con `[Parameter]`. Usare gli attributi per specificare gli argomenti per un componente nel markup. 

1. Aggiornare il componente `Counter` per ottenere un parametro `IncrementAmount` con impostazione predefinita 1.

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
    > Da Visual Studio è possibile aggiungere rapidamente un parametro del componente tramite il frammento di codice `para`. Digitare `para` e quindi premere `Tab` due volte.

1. Nella home page (*index.cshtml*), modificare la quantità di incremento per `Counter` a 10 impostando un attributo che corrisponde al nome della proprietà del componente per `IncrementCount`.

    ```cshtml
    <Counter IncrementAmount="10" />
    ```

1. Ricaricare la pagina.

    Il contatore nella home page viene ora incrementato di 10 unità, mentre il contatore nella pagina Counter viene ancora incrementato di 1.

    ![Conteggio Blazor con incrementi per dieci](https://user-images.githubusercontent.com/1874516/39509798-618f0720-4d9c-11e8-9125-3d4c634dff46.png)

## <a name="route-to-components"></a>Indirizzare le richieste ai componenti

La direttiva `@page` all'inizio del file *Counter.cshtml* specifica che questo componente è una pagina a cui possono essere indirizzate le richieste. In particolare, il componente `Counter` gestisce le richieste inviate a `/Counter`. Senza la direttiva `@page`, il componente non gestirebbe le richieste indirizzate, ma potrebbe ancora essere usato da altri componenti.

## <a name="dependency-injection"></a>Inserimento di dipendenze

I servizi registrati per il provider di servizi dell'app sono disponibili per i componenti tramite [inserimento delle dipendenze](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). I servizi possono essere inseriti in un componente usando la direttiva `@inject`.

Di seguito è riportata l'implementazione del componente `FetchData` nel file *FetchData.cshtml*. La direttiva `@inject` viene usata per inserire un'istanza [HttpClient](https://docs.microsoft.com/dotnet/api/system.net.http.httpclient) nel componente.

```cshtml
@page "/fetchdata"
@inject HttpClient Http
```

Il componente `FetchData` usa l'istanza `HttpClient` inserita per recuperare i dati JSON dal server quando il componente viene inizializzato. Dietro le quinte, l'istanza `HttpClient` fornita dal runtime di Blazor viene implementata tramite l'interoperabilità JavaScript per chiamare l'API Fetch del browser sottostante per l'invio della richiesta (da C# è possibile chiamare qualsiasi libreria JavaScript o API browser). I dati recuperati vengono deserializzati nella variabile C# `forecasts` come matrice di oggetti `WeatherForecast`ì.

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

Viene usato un ciclo `@foreach` per eseguire il rendering di ogni istanza di previsione come una riga nella tabella meteo.

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

## <a name="build-a-todo-list"></a>Compilare un elenco attività

Aggiungere una nuova pagina all'app che implementa un semplice elenco attività.

1. Aggiungere un file di testo vuoto alla cartella *Pages* denominata *Todo.cshtml*.

1. Specificare il markup iniziale per la pagina.

    ```cshtml
    @page "/todo"

    <h1>Todo</h1>
    ```

1. Aggiungere la pagina Todo alla barra di spostamento aggiornando *Shared/NavMenu.cshtml*. Aggiungere un `NavLink` per la pagina Todo aggiungendo il markup seguente per l'elemento di elenco sotto gli elementi di elenco esistenti.

    ```cshtml
    <li class="nav-item px-3">
        <NavLink class="nav-link" href="todo">
            <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
        </NavLink>
    </li>
    ```

1. Aggiornare l'app nel browser. Vedere la nuova pagina Todo.

    ![Pagina Todo Blazor iniziale](https://user-images.githubusercontent.com/1874516/39509907-bb27e77a-4d9c-11e8-91e7-ea1e7c01729e.png)

1. Aggiungere un file *TodoItem.cs* nella radice del progetto per contenere una classe per rappresentare gli elementi attività.

1. Usare il codice C# seguente per la classe `TodoItem`.

    ```csharp
    public class TodoItem
    {
        public string Title { get; set; }
        public bool IsDone { get; set; }
    }
    ```

1. Tornare al componente `Todo` in *Todo.cshtml* e aggiungere un campo per le attività in un blocco `@functions`. Il componente `Todo` usa questo campo per mantenere lo stato dell'elenco attività.

    ```cshtml
    @functions {
        IList<TodoItem> todos = new List<TodoItem>();
    }
    ```

1. Aggiungere il markup per l'elenco non ordinato e un ciclo `foreach` per eseguire il rendering di ogni elemento attività come elemento di elenco.

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

1. L'app richiede elementi dell'interfaccia utente per l'aggiunta di attività all'elenco. Aggiungere un input testo e un pulsante sotto l'elenco.

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

1. Aggiornare il browser.

    ![Pulsante Add todo](https://user-images.githubusercontent.com/1874516/39512402-bc88ab46-4da5-11e8-9e3f-87b875b56383.png)

    Non accade nulla quando viene selezionato il pulsante **Add todo** perché al pulsante non è associato alcun gestore eventi.

1. Aggiungere un metodo `AddTodo` al componente `Todo` e registrarlo per i clic sul pulsante con l'attributo `onclick`.

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

    Il metodo C# `AddTodo` viene chiamato ogni volta che viene selezionato il pulsante.

1. Per ottenere il titolo del nuovo elemento attività, aggiungere un campo stringa `newTodo` e associarlo al valore dell'input di testo tramite l'attributo `bind`.

    ```csharp
    IList<TodoItem> todos = new List<TodoItem>();
    string newTodo;
    ```

    ```cshtml
    <input placeholder="Something todo" bind="@newTodo" />
    ```

1. Aggiornare il metodo `AddTodo` per aggiungere l'elemento `TodoItem` con il titolo specificato all'elenco. Non dimenticare di cancellare il valore dell'input testo impostando `newTodo` su una stringa vuota.

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

1. Aggiornare il browser. Aggiungere alcune attività all'elenco attività.

    ![Aggiungere attività](https://user-images.githubusercontent.com/1874516/39512531-2d2ff62e-4da6-11e8-8b83-291b0efc821b.png)

1. Infine, verranno aggiunte le caselle di controllo imprescindibili per un elenco attività. Anche il testo del titolo per ogni elemento attività può essere reso modificabile. Aggiungere un input casella di controllo e un input testo per ogni elemento attività e associare i valori rispettivamente alle proprietà `Title` e `IsDone`.

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

1. Per verificare che questi valori siano associati, aggiornare l'intestazione `h1` per visualizzare un conteggio del numero di elementi attività non ancora completati (`IsDone` è `false`).

    ```cshtml
    <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
    ```

1. Il componente `Todo` completato dovrebbe essere simile al seguente:

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

Aggiornare l'app nel browser. Provare ad aggiungere alcuni elementi attività.

![Elenco attività Blazor completato](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

## <a name="publish-and-deploy"></a>Pubblicare e distribuire

Quando si usa Visual Studio, seguire questa procedura per pubblicare l'app Todo Blazor in Azure:

1. Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** e scegliere **Pubblica**.

1. Nella finestra di dialogo **Selezionare una destinazione di pubblicazione** selezionare **Servizio app** e **Crea nuovo**. Selezionare **Pubblica**.

    ![Selezionare una destinazione di pubblicazione](build-your-first-blazor-app/_static/blazor-publish-pick-target.png)

1. Nella finestra di dialogo **Crea servizio app** scegliere un nome per l'app e selezionare la sottoscrizione, il gruppo di risorse e il piano di hosting. Selezionare **Crea** per creare il servizio app e pubblicare l'app.

    ![Crea servizio app](build-your-first-blazor-app/_static/blazor-publish-create-appservice2.png)

Attendere più o meno un minuto per la distribuzione dell'app.

L'app dovrebbe ora essere in esecuzione in Azure. Contrassegna l'oggetto per creare la tua prima app Blazor come *fatto*. completata.

![Blazor in Azure](https://user-images.githubusercontent.com/1874516/39512621-83406508-4da6-11e8-8244-5bae2ac6b22d.png)

> [!NOTE]
> Se non si usa Visual Studio, pubblicare l'app Blazor da un prompt dei comandi in Windows, macOS o Linux:
>
> ```console
> dotnet publish -c Release
> ```
>
> La distribuzione viene creata nella cartella */bin/Release/\<target-framework>/publish*. Spostare il contenuto della cartella *publish* nel server o nel servizio di hosting.
>
> Per altre informazioni, vedere l'argomento [Ospitare e distribuire Razor Components](xref:host-and-deploy/razor-components/index#publish-the-app).

## <a name="additional-resources"></a>Risorse aggiuntive

Per un'app di esempio di Blazor più articolata, vedere l'[esempio Flight Finder](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor) su GitHub.

![Blazor Flight Finder](https://msdnshared.blob.core.windows.net/media/2018/03/blazor-flight-finder.png)
