---
title: Creare la prima app Razor Components
author: guardrex
description: Procedura dettagliata per creare un'app Razor Components e informazioni sui concetti di base di Razor Components.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: tutorials/first-razor-components-app
ms.openlocfilehash: 697c4659bcc9952ffe9868fe9b3c0d28019bc369
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468776"
---
# <a name="build-your-first-razor-components-app"></a>Creare la prima app Razor Components

Di [Daniel Roth](https://github.com/danroth27) e [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Questa esercitazione illustra come creare un'app con Razor Components e offre una dimostrazione dei concetti di base di Razor Components. È possibile seguire questa esercitazione usando sia un progetto basato su Razor Components (supportato in .NET Core 3.0 o versione successiva) che un progetto basato su Blazor (supportato in una versione futura di .NET Core).

Per un'esperienza con ASP.NET Core Razor Components (*consigliata*):

* Seguire le indicazioni in <xref:razor-components/get-started> per creare un progetto basato su Razor Components.
* Denominare il progetto `RazorComponents`.

Per un'esperienza con Blazor:

* Seguire le indicazioni in <xref:spa/blazor/get-started> per creare un progetto basato su Blazor.
* Denominare il progetto `Blazor`.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/build-your-first-razor-components-app/samples/) ([procedura per il download](xref:index#how-to-download-a-sample)). Vedere gli argomenti seguenti per i prerequisiti:

## <a name="build-components"></a>Compilare i componenti

1. Esplorare ognuna delle tre pagine dell'app nella cartella *Components/Pages* (*Pages* in Blazor): Home, Counter e Fetch data (Home, Contatore e Recupera dati). Queste pagine vengono implementate dai file del componente Razor: *Index.Razor*, *Counter.razor* e *FetchData.razor*. (Blazor continua a usare l'estensione file *cshtml*: *Index.cshtml*, *Counter.cshtml* e *FetchData.cshtml*.)

1. Nella pagina Counter selezionare il pulsante **Click me** per incrementare il contatore senza un aggiornamento della pagina. Per incrementare un contatore in una pagina Web è in genere richiesta la scrittura di codice JavaScript, ma Razor Components offre un approccio migliore usando C#.

1. Esaminare l'implementazione del componente Counter nel file *Counter.razor*.

   *Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter1.razor)]

   L'interfaccia utente del componente Counter viene definita tramite codice HTML. La logica di rendering dinamico (ad esempio, cicli, condizioni, espressioni) viene aggiunta usando una sintassi C# incorporata chiamata [Razor](xref:mvc/views/razor). Il markup HTML e la logica di rendering C# vengono convertiti un una classe del componente in fase di compilazione. Il nome della classe .NET generata corrisponde al nome del file.

   I membri della classe del componente sono definiti in un blocco `@functions`. Nel blocco `@functions`, lo stato del componente (proprietà, campi) e i metodi vengono specificati per la gestione degli eventi o per definire la logica di altri componenti. Questi membri vengono quindi usati come parte della logica di rendering del componente e per la gestione degli eventi.

   Quando viene selezionato il pulsante **Click me**:

   * Viene chiamato il gestore `onclick` registrato del componente Counter (metodo `IncrementCount`).
   * Il componente Counter rigenera il relativo albero di rendering.
   * Il nuovo albero di rendering viene confrontato con quello precedente.
   * Vengono applicate solo le modifiche al modello DOM (Document Object Model). Il conteggio visualizzato viene aggiornato.

1. Modificare la logica C# del componente Counter per incrementare il contatore di due unità anziché una.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Ricompilare ed eseguire l'app per visualizzare le modifiche. Selezionare il pulsante **Click me** e il contatore viene incrementato di due unità.

## <a name="use-components"></a>Usare i componenti

Includere un componente in un altro componente usando una sintassi simile a HTML.

1. Aggiungere il componente Counter al componente Index (Home) dell'app aggiungendo un elemento `<Counter />` al componente Index.

   Se si usa Blazor per questa esperienza, il componente Index include un componente Survey Prompt (elemento `<SurveyPrompt>`). Sostituire l'elemento `<SurveyPrompt>` con l'elemento `<Counter>`.

   *Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/Index.razor?highlight=7)]

1. Ricompilare ed eseguire l'app. La home page ha un contatore proprio.

## <a name="component-parameters"></a>Parametri del componente

I componenti possono avere anche parametri, che vengono definiti usando proprietà non pubbliche nella classe del componente decorata con `[Parameter]`. Usare gli attributi per specificare gli argomenti per un componente nel markup.

1. Aggiornare il codice C# `@functions` del componente:

   * Aggiungere una proprietà `IncrementAmount` decorata con l'attributo `[Parameter]`.
   * Modificare il metodo `IncrementCount` per usare `IncrementAmount` quando si aumenta il valore di `currentCount`.

   *Components/Pages/Counter.razor* (*Pages/Counter.cshtml* in Blazor):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Counter.razor?highlight=12,16)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Specificare un parametro `IncrementAmount` nell'elemento `<Counter>` del componente Home usando un attributo. Impostare il valore per incrementare il contatore di dieci unità.

   *Components/Pages/Index.razor* (*Pages/Index.cshtml* in Blazor):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Index.razor?highlight=7)]

1. Ricaricare la home page. Il contatore viene incrementato di dieci unità ogni volta che viene selezionato il pulsante **Click me**. Il contatore nella pagina Counter viene incrementato di un'unità.

## <a name="route-to-components"></a>Indirizzare le richieste ai componenti

La direttiva `@page` all'inizio del file *Counter.razor* specifica che questo componente è un endpoint di routing. Il componente Counter gestisce le richieste inviate a `/Counter`. Senza la direttiva `@page`, il componente non gestisce le richieste indirizzate, ma può ancora essere usato da altri componenti.

## <a name="dependency-injection"></a>Inserimento di dipendenze

I servizi registrati nel contenitore del servizio dell'app sono disponibili per i componenti tramite [inserimento delle dipendenze](xref:fundamentals/dependency-injection). Inserire i servizi in un componente usando la direttiva `@inject`.

Esaminare le direttive del componente FetchData nell'app di esempio.

Nell'app di esempio Razor Components il servizio `WeatherForecastService` viene registrato come [singleton](xref:fundamentals/dependency-injection#service-lifetimes), in modo che un'istanza del servizio sia disponibile nell'intera app. La direttiva `@inject` viene usata per inserire l'istanza del servizio `WeatherForecastService` nel componente.

*Components/Pages/FetchData.razor*:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

Il componente FetchData usa il servizio inserito, come `ForecastService`, per recuperare una matrice di oggetti `WeatherForecast`:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

Nella versione Blazor dell'app di esempio `HttpClient` viene inserito per ottenere i dati delle previsioni meteo dal file *weather.json* nella cartella *wwwroot/sample-data*:

*Pages/FetchData.cshtml*:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData1.cshtml?highlight=7)]

In entrambe le app di esempio viene usato un ciclo [@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) per eseguire il rendering di ogni istanza di previsione come una riga della tabella dei dati meteo:

[!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]

## <a name="build-a-todo-list"></a>Compilare un elenco attività

Aggiungere all'app un nuovo componente che implementa un semplice elenco attività.

1. Aggiungere un file vuoto all'app di esempio:

   * Per l'esperienza Razor Components aggiungere un file *Todo.razor* alla cartella *Components/Pages*.
   * Per l'esperienza Blazor aggiungere un file *Todo.cshtml* alla cartella *Pages*.

1. Specificare il markup iniziale per il componente:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Aggiungere il componente Todo alla barra di spostamento.

   Il componente NavMenu (*Components/Shared/NavMenu.razor* oppure *Shared/NavMenu.cshtml* in Blazor) viene usato nel layout dell'app. I layout sono componenti che consentono di evitare la duplicazione del contenuto nell'app. Per ulteriori informazioni, vedere <xref:razor-components/layouts>.

   Aggiungere un elemento `<NavLink>` per il componente Todo aggiungendo il markup seguente per l'elemento di elenco sotto gli elementi di elenco esistenti nel file *Components/Shared/NavMenu.razor* (*Shared/NavMenu.cshtml* in Blazor):

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Ricompilare ed eseguire l'app. Visitare la nuova pagina Todo per confermare che il collegamento al componente Todo funzioni correttamente.

1. Aggiungere un file *TodoItem.cs* nella radice del progetto per contenere una classe per rappresentare un elemento attività. Usare il codice C# seguente per la classe `TodoItem`:

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/TodoItem.cs)]

1. Tornare al componente Todo (*Components/Pages/Todo.razor* oppure *Pages/Todo.cshtml* in Blazor):

   * Aggiungere un campo per le attività in un blocco `@functions`. Il componente Todo usa questo campo per mantenere lo stato dell'elenco attività.
   * Aggiungere il markup per l'elenco non ordinato e un ciclo `foreach` per eseguire il rendering di ogni elemento attività come elemento di elenco.

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. L'app richiede elementi dell'interfaccia utente per l'aggiunta di attività all'elenco. Aggiungere un input di testo e un pulsante sotto l'elenco:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Ricompilare ed eseguire l'app. Quando viene selezionato il pulsante **Add todo** non accade nulla perché al pulsante non è associato un gestore eventi.

1. Aggiungere un metodo `AddTodo` al componente Todo e registrarlo per i clic sul pulsante con l'attributo `onclick`:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

   Il metodo C# `AddTodo` viene chiamato quando viene selezionato il pulsante.

1. Per ottenere il titolo del nuovo elemento attività, aggiungere un campo stringa `newTodo` e associarlo al valore dell'input di testo tramite l'attributo `bind`:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" bind="@newTodo">
   ```

1. Aggiornare il metodo `AddTodo` per aggiungere l'elemento `TodoItem` con il titolo specificato all'elenco. Cancellare il valore dell'input di testo impostando `newTodo` su una stringa vuota:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Ricompilare ed eseguire l'app. Aggiungere alcune attività all'elenco attività per testare il nuovo codice.

1. Il testo del titolo per ogni elemento attività può essere reso modificabile e una casella di controllo può consentire all'utente di tenere traccia degli elementi completati. Aggiungere un input casella di controllo per ogni elemento attività e associarne il valore alla proprietà `IsDone`. Modificare `@todo.Title` in un elemento `<input>` associato a `@todo.Title`:

   [!code-cshtml[](build-your-first-razor-components-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Per verificare che questi valori siano associati, aggiornare l'intestazione `<h1>` per visualizzare un conteggio del numero di elementi attività non completati (`IsDone` è `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Il componente Todo completato (*Components/Pages/Todo.razor* oppure *Pages/Todo.cshtml* in Blazor):

   [!code-cshtml[](build-your-first-razor-components-app/samples/3.x/RazorComponents/Components/Pages/Todo.razor)]

1. Ricompilare ed eseguire l'app. Aggiungere elementi attività per testare il nuovo codice.

## <a name="publish-and-deploy-the-app"></a>Pubblicare e distribuire l'app

Per pubblicare l'app, vedere <xref:host-and-deploy/razor-components-blazor/index>.
