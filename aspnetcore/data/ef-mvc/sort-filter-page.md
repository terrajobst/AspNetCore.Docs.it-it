---
title: Core di ASP.NET MVC con Entity Framework Core - ordinamento, filtro, Paging - 3 di 10
author: tdykstra
description: "In questa esercitazione si aggiungeranno ordinamento, filtro e paging funzionalità alla pagina utilizzando ASP.NET Core e componenti di base di Entity Framework."
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 6da2073b18f6fff9738808c84441e59240caefe3
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a>Ordinamento, filtro, paging e raggruppamento: EF Core con l'esercitazione di base di ASP.NET MVC (3 di 10)

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).

Nell'esercitazione precedente, l'implementazione è un set di pagine web per operazioni CRUD di base per le entità di studenti. In questa esercitazione si aggiungerà ordinamento, filtro e la funzionalità di paging alla pagina di indice di studenti. Si creeranno inoltre una pagina che esegue il raggruppamento semplice.

Nella figura seguente viene illustrato l'aspetto di pagina al termine. Le intestazioni di colonna sono link che l'utente può fare clic per ordinare in base alla colonna. Fare clic su una colonna con intestazione ripetutamente passa alternativamente da crescente e decrescente.

![Pagina di indice di studenti](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Aggiungere collegamenti di ordinamento di colonna per la pagina di indice di studenti

Per aggiungere l'ordinamento alla pagina di indice di studenti, sarà necessario impostare il `Index` metodo del controller di studenti e aggiungere codice per la visualizzazione dell'indice di studenti.

### <a name="add-sorting-functionality-to-the-index-method"></a>Aggiunta della funzionalità di ordinamento per il metodo di Index

In *StudentsController.cs*, sostituire il `Index` (metodo) con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Questo codice riceve un `sortOrder` parametro dalla stringa di query nell'URL. Il valore di stringa di query viene fornito da ASP.NET MVC di base come parametro al metodo di azione. Il parametro sarà una stringa che è "Name" o "Date", seguito facoltativamente da un carattere di sottolineatura e la stringa "desc" per specificare l'ordine decrescente. L'ordinamento predefinito è crescente.

La prima volta che viene richiesta la pagina di indice, viene individuata alcuna stringa di query. Gli studenti vengono visualizzati in ordine crescente in base al cognome, il valore predefinito è definito dall'istruzione case nel `switch` istruzione. Quando l'utente fa clic hyperlink intestazione di colonna, appropriata `sortOrder` valore viene fornito nella stringa di query.

I due `ViewData` elementi (NameSortParm e DateSortParm) vengono utilizzati dalla vista per configurare i collegamenti ipertestuali sull'intestazione di colonna con i valori di stringa di query appropriata.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Si tratta di istruzioni ternarie. Il primo specifica che se il `sortOrder` parametro è null o vuoto, NameSortParm deve essere impostato su "name_desc"; in caso contrario, deve essere impostato su una stringa vuota. Le due istruzioni seguenti consentono la visualizzazione impostare la colonna collegamenti ipertestuali di intestazione come indicato di seguito:

|  Ordinamento corrente  | Collegamento ipertestuale cognome | Collegamento ipertestuale Data |
|:--------------------:|:-------------------:|:--------------:|
| Ultimo nome (crescente)  | descending          | ascending      |
| Ultimo nome (decrescente) | ascending           | ascending      |
| Data ordine crescente       | ascending           | descending     |
| Data ordine decrescente      | ascending           | ascending      |

Il metodo Usa LINQ to Entities per specificare la colonna da ordinare. Il codice crea un `IQueryable` variabile prima dell'istruzione switch, modificarla nell'istruzione switch e chiama il `ToListAsync` metodo dopo il `switch` istruzione. Quando si crea e modifica `IQueryable` variabili, nessuna query viene inviata al database. La query non viene eseguita fino alla conversione di `IQueryable` oggetto in una raccolta chiamando un metodo, ad esempio `ToListAsync`. Pertanto, questo codice genera una singola query che non viene eseguita finché il `return View` istruzione.

Questo codice è stato possibile ottenere dettagliato con un numero elevato di colonne. [Dell'ultima esercitazione di questa serie](advanced.md#dynamic-linq) viene illustrato come scrivere codice che consente di passare il nome di `OrderBy` colonna in una variabile di stringa.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Aggiungere collegamenti ipertestuali sull'intestazione di colonna per la visualizzazione dell'indice di Student

Sostituire il codice in *Views/Students/Index.cshtml*, con il codice seguente per aggiungere collegamenti ipertestuali di intestazione di colonna. Le righe modificate vengono evidenziate.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Questo codice Usa le informazioni contenute in `ViewData` valori di stringa di proprietà per impostare i collegamenti ipertestuali con la query appropriato.

Eseguire l'app, selezionare il **studenti** scheda, quindi scegliere il **Last Name** e **data di registrazione** funziona intestazioni di colonna per verificare che l'ordinamento.

![Pagina di indice di studenti nell'ordine dei nomi](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Aggiungere una casella di ricerca alla pagina di indice di studenti

Per aggiungere filtri alla pagina di indice di studenti, aggiungerai un pulsante di invio e di una casella di testo per la visualizzazione e apportare le modifiche corrispondenti nel `Index` metodo. La casella di testo consente di immettere una stringa da cercare nel nome e cognome.

### <a name="add-filtering-functionality-to-the-index-method"></a>Aggiungere funzionalità di filtro per il metodo di Index

In *StudentsController.cs*, sostituire il `Index` (metodo) con il codice seguente (sono evidenziate le modifiche).

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

È stato aggiunto un `searchString` parametro per il `Index` (metodo). Il valore di stringa di ricerca viene ricevuto da una casella di testo che verrà aggiunto per la visualizzazione dell'indice. Inoltre aggiunti all'istruzione LINQ una clausola where clausola che seleziona solo gli studenti il cui nome o cognome contengono la stringa di ricerca. L'istruzione che aggiunge where clausola viene eseguita solo se è presente un valore per la ricerca.

> [!NOTE]
> Qui si chiama il `Where` metodo su un `IQueryable` oggetto e il filtro verrà elaborato nel server. In alcuni scenari potrebbe essere chiamata la `Where` metodo come metodo di estensione per una raccolta in memoria. (Ad esempio, si supponga che si modifica il riferimento a `_context.Students` in modo che anziché un EF `DbSet` fa riferimento a un metodo di repository che restituisce un `IEnumerable` insieme.) Il risultato sarebbe normalmente la stessa, ma in alcuni casi potrebbe essere diverso.
>
>Ad esempio, l'implementazione di .NET Framework del `Contains` metodo esegue un confronto tra maiuscole e minuscole per impostazione predefinita, ma in SQL Server tale autorizzazione è determinata dall'impostazione delle regole di confronto dell'istanza di SQL Server. Tale impostazione predefinita a tra maiuscole e minuscole. È possibile chiamare il `ToUpper` metodo per eseguire il test in modo esplicito tra maiuscole e minuscole: *in (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*. Assicurarsi che che risultati rimangono invariati se si modifica il codice in un secondo momento per usare un repository che restituisce un `IEnumerable` raccolta invece di un `IQueryable` oggetto. (Quando si chiama il `Contains` metodo su un `IEnumerable` raccolta, si ottiene l'implementazione di .NET Framework; quando si chiama su un `IQueryable` dell'oggetto, si ottiene l'implementazione del provider di database.) È tuttavia una riduzione delle prestazioni per questa soluzione. Il `ToUpper` codice dovrà inserire una funzione nella clausola WHERE dell'istruzione SELECT TSQL. Utilizzo di un indice che impedirebbe l'utilità di ottimizzazione. Dato che SQL viene installato per lo più come tra maiuscole e minuscole, è consigliabile evitare di `ToUpper` codice fino a quando non si esegue la migrazione a un archivio dati tra maiuscole e minuscole.

### <a name="add-a-search-box-to-the-student-index-view"></a>Aggiungere una casella di ricerca per la visualizzazione dell'indice per studenti

In *Views/Student/Index.cshtml*, aggiungere il codice evidenziato immediatamente prima dell'apertura tag di tabella per creare una didascalia, una casella di testo e un **ricerca** pulsante.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Questo codice Usa il `<form>` [helper di tag](xref:mvc/views/tag-helpers/intro) per aggiungere la casella di testo di ricerca e un pulsante. Per impostazione predefinita, il `<form>` helper di tag invia dati con un POST, il che significa che i parametri vengono passati nel corpo del messaggio HTTP e non nell'URL come stringhe di query. Quando si specifica di HTTP GET, i dati del form viene passati nell'URL come stringhe di query, che consente agli utenti di segnalibro l'URL. Ricevi preferibile W3C linee guida che è necessario utilizzare l'azione non comporta un aggiornamento.

Eseguire l'app, selezionare il **studenti** scheda, immettere una stringa di ricerca e fare clic su Cerca per verificare che il filtro sia funzionante.

![Pagina di indice di studenti di filtro](sort-filter-page/_static/filtering.png)

Si noti che l'URL contiene la stringa di ricerca.

```html
http://localhost:5813/Students?SearchString=an
```

Se questa pagina, quando si utilizza il segnalibro viene visualizzato l'elenco filtrato. Aggiunta di `method="get"` per il `form` tag è ciò che ha determinato la stringa di query da generare.

In questa fase, se si fa clic su un collegamento di ordinamento di intestazione di colonna si perderanno il valore del filtro immesso nel **ricerca** casella. Che potrà essere corretto nella sezione successiva.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Aggiungere funzionalità di paging alla pagina di indice di studenti

Per aggiungere il paging per la pagina di indice di studenti, si creerà un `PaginatedList` classe che utilizza `Skip` e `Take` istruzioni per filtrare i dati sul server invece di recuperare sempre tutte le righe della tabella. Quindi è possibile apportare ulteriori modifiche nel `Index` (metodo) e aggiungere i pulsanti di spostamento per la `Index` visualizzazione. Nella figura seguente mostra i pulsanti di spostamento.

![Gli studenti indice pagina con collegamenti di spostamento](sort-filter-page/_static/paging.png)

Nella cartella del progetto, creare `PaginatedList.cs`, quindi sostituire il codice del modello con il codice seguente.

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

Il `CreateAsync` metodo in questo codice ha dimensioni di pagina e il numero di pagina e applica appropriata `Skip` e `Take` istruzioni per la `IQueryable`. Quando `ToListAsync` viene chiamato sul `IQueryable`, verrà restituito un elenco contenente solo la pagina richiesta. Le proprietà `HasPreviousPage` e `HasNextPage` può essere utilizzato per abilitare o disabilitare **precedente** e **Avanti** pulsanti di spostamento.

Oggetto `CreateAsync` metodo viene utilizzato invece di un costruttore per creare il `PaginatedList<T>` dell'oggetto poiché i costruttori non possono eseguire codice asincrono.

## <a name="add-paging-functionality-to-the-index-method"></a>Aggiungere funzionalità di paging per il metodo di Index

In *StudentsController.cs*, sostituire il `Index` (metodo) con il codice seguente.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Questo codice aggiunge un parametro di numeri di pagina, un parametro di ordine di ordinamento corrente e un parametro di filtro corrente per la firma del metodo.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

La prima volta che viene visualizzata la pagina o se l'utente non è stato selezionato un paging o ordinamento di collegamento, tutti i parametri è null.  Se si seleziona un collegamento di paging, la variabile di pagina conterrà il numero di pagina da visualizzare.

Il `ViewData` elemento denominato CurrentSort fornisce la visualizzazione con ordinamento corrente, in quanto ciò deve essere inclusi i collegamenti di spostamento per mantenere la stessa durante il paging dell'ordinamento.

Il `ViewData` elemento denominato CurrentFilter fornisce la visualizzazione con la stringa di filtro corrente. Questo valore deve essere incluso nei collegamenti di spostamento per mantenere le impostazioni di filtro durante il paging, e devono essere ripristinato nella casella di testo quando la pagina viene visualizzata.

Se la stringa di ricerca viene modificata durante il paging, la pagina deve essere reimpostato su 1, in quanto può comportare dati diversi per visualizzare il nuovo filtro. La stringa di ricerca viene modificata quando viene immesso un valore nella casella di testo e viene premuto il pulsante Invia. In tal caso, il `searchString` parametro non è null.

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

Alla fine del `Index` (metodo), il `PaginatedList.CreateAsync` metodo converte la query di studenti in una singola pagina degli studenti iscritti a un tipo di insieme che supporta il paging. Pagina singola di studenti viene quindi passato alla visualizzazione.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

Il `PaginatedList.CreateAsync` metodo accetta un numero di pagina. Due punti interrogativi rappresenta l'operatore null-coalescing. L'operatore null-coalescing definisce un valore predefinito per un tipo nullable. l'espressione `(page ?? 1)` significa restituisca il valore di `page` se ha un valore oppure restituiscono 1 se `page` è null.

## <a name="add-paging-links-to-the-student-index-view"></a>Aggiungere collegamenti di spostamento per la visualizzazione dell'indice per studenti

In *Views/Students/Index.cshtml*, sostituire il codice esistente con il codice seguente. Le modifiche sono evidenziate.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

Il `@model` istruzione nella parte superiore della pagina specifica che la vista Ottiene ora un `PaginatedList<T>` oggetto anziché un `List<T>` oggetto.

I collegamenti di intestazione di colonna utilizzano la stringa di query per passare la stringa di ricerca corrente al controller in modo che l'utente può ordinare all'interno dei risultati di filtro:

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

Per gli helper di tag vengono visualizzati i pulsanti di paging:

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

Eseguire l'app e passare alla pagina di studenti.

![Gli studenti indice pagina con collegamenti di spostamento](sort-filter-page/_static/paging.png)

Fare clic sui collegamenti di spostamento in diversi tipi di ordinamento per assicurarsi che funziona di paging. Quindi immettere una stringa di ricerca e tentare di paging per verificare che il paging funziona inoltre correttamente con ordinamento e filtro.

## <a name="create-an-about-page-that-shows-student-statistics"></a>Creare una pagina di informazioni che vengono visualizzate le statistiche di Student

Per il sito Web Contoso University **su** pagina verranno visualizzati il numero di studenti sono registrati per ciascuna data di registrazione. Questa operazione richiede i calcoli di raggruppamento e semplici ai gruppi. A tale scopo, verranno eseguite le operazioni seguenti:

* Creare una classe di modello di visualizzazione per i dati che è necessario passare alla visualizzazione.

* Modificare il metodo di informazioni del controller Home.

* Modificare la visualizzazione di informazioni.

### <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Creare un *SchoolViewModels* cartella la *modelli* cartella.

Nella nuova cartella, aggiungere un file di classe *EnrollmentDateGroup.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Modificare il Controller Home

In *HomeController.cs*, aggiungere le seguenti istruzioni using all'inizio del file:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Aggiungere una variabile di classe per il contesto del database immediatamente dopo la parentesi graffa di apertura per la classe e ottenere un'istanza del contesto da ASP.NET Core DI:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Sostituire il metodo `About` con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

Raggruppa le entità di studenti per data di registrazione dell'istruzione LINQ, calcola il numero di entità in ciascun gruppo e archivia i risultati in una raccolta di `EnrollmentDateGroup` consente di visualizzare gli oggetti del modello.
> [!NOTE] 
> Nella 1.0 versione di Entity Framework Core, l'intero set di risultati viene restituito al client e il raggruppamento avviene sul client. In alcuni scenari si potrebbero creare problemi di prestazioni. Assicurarsi di testare le prestazioni con volumi di produzione di dati e se necessario, utilizzare SQL non elaborato per eseguire il raggruppamento nel server. Per informazioni sull'utilizzo di SQL non elaborati, vedere [dell'ultima esercitazione di questa serie](advanced.md).

### <a name="modify-the-about-view"></a>Modificare le informazioni sulla visualizzazione

Sostituire il codice di *Views/Home/About.cshtml* file con il codice seguente:

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Eseguire l'app e passare alla pagina di informazioni. Il numero di studenti per ogni data di registrazione viene visualizzato in una tabella.

![Informazioni sulla pagina](sort-filter-page/_static/about.png)

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato spiegato come eseguire l'ordinamento, filtro, paging e raggruppamento. Nella prossima esercitazione, si apprenderà come gestire le modifiche al modello di dati utilizzando le migrazioni.

>[!div class="step-by-step"]
[Precedente](crud.md)
[Successivo](migrations.md)  
