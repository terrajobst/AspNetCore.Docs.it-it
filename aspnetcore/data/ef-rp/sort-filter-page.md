---
title: Pagine Razor con Entity Framework Core - ordinamento, filtro, Paging - 3 di 8
author: rick-anderson
description: "In questa esercitazione si aggiungeranno ordinamento, filtro e paging funzionalità alla pagina utilizzando ASP.NET Core e componenti di base di Entity Framework."
ms.author: riande
ms.date: 10/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 08f00e183dd8a8daa883d0b9ff15698b3a39f625
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-razor-pages-3-of-8"></a>Ordinamento, filtro, paging e raggruppamento: EF Core con pagine Razor (3 di 8)

Da [Tom Dykstra](https://github.com/tdykstra), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

In questa esercitazione, ordinamento, filtro, raggruppamento e il paging, viene aggiunta la funzionalità.

Nella figura seguente mostra una pagina completata. Le intestazioni di colonna sono collegamenti selezionabili per ordinare la colonna. Fare clic su una colonna con intestazione ripetutamente passa tra crescente e decrescente.

![Pagina di indice di studenti](sort-filter-page/_static/paging.png)

Se si verificano problemi, è possibile risolvere, scaricare il [app completata per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

## <a name="add-sorting-to-the-index-page"></a>Aggiungere l'ordinamento alla pagina di indice

Aggiungere stringhe di *Students/Index.cshtml.cs* `PageModel` per contenere i parametri di ordinamento:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]


Aggiornamento di *Students/Index.cshtml.cs* `OnGetAsync` con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

Il codice precedente riceve un `sortOrder` parametro dalla stringa di query nell'URL. L'URL (inclusa la stringa di query) viene generato dal [Helper di Tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

Il `sortOrder` parametro è "Name" o "Data". Il `sortOrder` parametro è facoltativamente seguito da "_desc" per specificare l'ordine decrescente. L'ordinamento predefinito è crescente.

Quando la pagina di indice viene richiesto dal **studenti** collegare, nessuna stringa di query. Gli studenti vengono visualizzati in ordine crescente in base al cognome. Ordine crescente in base al cognome è il valore predefinito (istruzione case) nei `switch` istruzione. Quando l'utente fa clic su un collegamento di intestazione di colonna, appropriato `sortOrder` valore viene fornito il valore di stringa di query.

`NameSort`e `DateSort` vengono utilizzati dalla pagina Razor per configurare i collegamenti ipertestuali sull'intestazione di colonna con i valori di stringa di query appropriata:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

Contiene il seguente codice c# [?: operatore](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/conditional-operator):

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

La prima riga specifica che quando `sortOrder` è null o vuoto, `NameSort` è impostata su "name_desc". Se `sortOrder` è **non** null o vuoto, `NameSort` è impostata su una stringa vuota.

Il `?: operator` è noto anche come l'operatore ternario disponibile.

Le due istruzioni seguenti consentono la visualizzazione impostare la colonna collegamenti ipertestuali di intestazione come indicato di seguito:

| Ordinamento corrente | Collegamento ipertestuale cognome | Collegamento ipertestuale Data |
|:--------------------:|:-------------------:|:--------------:|
| Ultimo nome (crescente) | descending        | ascending      |
| Ultimo nome (decrescente) | ascending           | ascending      |
| Data ordine crescente       | ascending           | descending     |
| Data ordine decrescente      | ascending           | ascending      |

Il metodo Usa LINQ to Entities per specificare la colonna da ordinare. Il codice Inizializza un `IQueryable<Student> ` prima dell'istruzione switch e lo modifica nell'istruzione switch:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-)]

 Quando un`IQueryable` viene creata o modificata, non viene inviata alcuna query al database. La query non viene eseguita finché il `IQueryable` oggetto viene convertito in una raccolta. `IQueryable`vengono convertiti in una raccolta chiamando un metodo, ad esempio `ToListAsync`. Pertanto, il `IQueryable` codice risultati in una singola query che non viene eseguito fino a quando l'istruzione seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync`è stato possibile ottenere dettagliato con un numero elevato di colonne.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Aggiungere collegamenti ipertestuali sull'intestazione di colonna per la visualizzazione dell'indice di Student

Sostituire il codice in *Students/Index.cshtml*, con il seguente codice evidenziato:

[!code-html[](intro/samples/cu/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

Il codice precedente:

* Aggiunti collegamenti ipertestuali per il `LastName` e `EnrollmentDate` le intestazioni di colonna.
* Utilizza le informazioni contenute in `NameSort` e `DateSort` per impostare i collegamenti ipertestuali con i valori di ordinamento corrente.

Per verificare il funzionamento dell'ordinamento:

* Eseguire l'app e selezionare il **studenti** scheda.
* Fare clic su **cognome**.
* Fare clic su **data di registrazione**.

Per ottenere una migliore comprensione del codice:

* In *Student/Index.cshtml.cs*, impostare un punto di interruzione `switch (sortOrder)`.
* Aggiungere un'espressione di controllo per `NameSort` e `DateSort`.
* In *Student/Index.cshtml*, impostare un punto di interruzione `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Eseguire il debugger.

## <a name="add-a-search-box-to-the-students-index-page"></a>Aggiungere una casella di ricerca alla pagina di indice di studenti

Per aggiungere filtri alla pagina di indice di studenti:

* Una casella di testo e un pulsante di invio viene aggiunto alla pagina Razor. La casella di testo fornisce una stringa di ricerca sul nome del primo o ultimo.
* Il file code-behind viene aggiornato per utilizzare il valore della casella di testo.

### <a name="add-filtering-functionality-to-the-index-method"></a>Aggiungere funzionalità di filtro per il metodo di Index

Aggiornamento di *Students/Index.cshtml.cs* `OnGetAsync` con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Il codice precedente:

* Aggiunge il `searchString` parametro per il `OnGetAsync` metodo. Il valore di stringa di ricerca viene ricevuto da una casella di testo che viene aggiunto nella sezione successiva.
* Aggiungere l'istruzione LINQ un `Where` clausola. Il `Where` clausola seleziona solo gli studenti il cui nome o cognome contengono la stringa di ricerca. L'istruzione LINQ viene eseguita solo se è presente un valore per la ricerca.

Nota: Il precedente codice chiama il `Where` metodo su un `IQueryable` oggetto e il filtro viene elaborato nel server. In alcuni scenari, potrebbe essere chiamata di app il `Where` metodo come metodo di estensione per una raccolta in memoria. Si supponga ad esempio `_context.Students` cambia da Entity Framework Core `DbSet` a un metodo di repository che restituisce un `IEnumerable` insieme. Il risultato sarebbe normalmente la stessa, ma in alcuni casi potrebbe essere diverso.

Ad esempio, l'implementazione di .NET Framework di `Contains` esegue un confronto tra maiuscole e minuscole per impostazione predefinita. In SQL Server, `Contains` distinzione maiuscole/minuscole è determinato dall'impostazione delle regole di confronto dell'istanza di SQL Server. SQL Server per impostazione predefinita a tra maiuscole e minuscole. `ToUpper`può essere chiamata per eseguire il test in modo esplicito tra maiuscole e minuscole:

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

Il codice precedente di assicurarsi che i risultati sono distinzione tra maiuscole e se viene modificato il codice per l'utilizzo di `IEnumerable`. Quando `Contains` viene chiamato su un `IEnumerable` insieme, viene utilizzata l'implementazione di .NET Core. Quando `Contains` viene chiamato su un `IQueryable` dell'oggetto, viene utilizzata l'implementazione di database. Restituzione di un `IEnumerable` da un repository può avere un penality significativo delle prestazioni:

1. Tutte le righe vengono restituite dal server di database.
1. Il filtro viene applicato a tutte le righe restituite nell'applicazione.

Si verifica una riduzione delle prestazioni per chiamare `ToUpper`. Il `ToUpper` codice aggiunge una funzione nella clausola WHERE dell'istruzione SELECT TSQL. La funzione aggiunta impedisce l'utilizzo di un indice query optimizer. Dato che SQL viene installato come tra maiuscole e minuscole, è consigliabile evitare di `ToUpper` chiamare quando non è necessaria.

### <a name="add-a-search-box-to-the-student-index-view"></a>Aggiungere una casella di ricerca per la visualizzazione dell'indice per studenti

In *Views/Student/Index.cshtml*, aggiungere il codice evidenziato di seguito per creare un **ricerca** pulsante e chrome diversi.

[!code-html[](intro/samples/cu/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

Il codice precedente utilizza il `<form>` [helper di tag](xref:mvc/views/tag-helpers/intro) per aggiungere la casella di testo di ricerca e un pulsante. Per impostazione predefinita, il `<form>` helper di tag invia dati con un POST del form. Con POST, i parametri vengono passati nel corpo del messaggio HTTP e non nell'URL. Quando viene utilizzato HTTP GET, i dati del form viene passati nell'URL come stringhe di query. Il passaggio di dati con le stringhe di query consente agli utenti di segnalibro l'URL. Il [linee guida W3C](https://www.w3.org/2001/tag/doc/whenToUseGet.html) consiglia che GET deve essere utilizzato quando l'azione non comporta un aggiornamento.

Eseguire il test dell'app:

* Selezionare il **studenti** scheda e immettere una stringa di ricerca.
* Selezionare **ricerca**.

Si noti che l'URL contiene la stringa di ricerca.

```html
http://localhost:5000/Students?SearchString=an
```

Se la pagina è contrassegnata, il segnalibro contiene l'URL alla pagina e `SearchString` stringa di query. Il `method="get"` nel `form` tag è ciò che ha determinato la stringa di query da generare.

Attualmente, quando si seleziona un collegamento di ordinamento di intestazione di colonna, il filtro valore il **ricerca** casella viene persa. Il valore di filtro persi è stato risolto nella sezione successiva.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Aggiungere funzionalità di paging alla pagina di indice di studenti

In questa sezione, un `PaginatedList` classe viene creata per supportare il paging. Il `PaginatedList` utilizzato dalla classe `Skip` e `Take` istruzioni per filtrare i dati sul server invece di recuperare tutte le righe della tabella. Nella figura seguente mostra i pulsanti di spostamento.

![Gli studenti indice pagina con collegamenti di spostamento](sort-filter-page/_static/paging.png)

Nella cartella del progetto, creare `PaginatedList.cs` con il codice seguente:

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

Il `CreateAsync` metodo nel codice precedente ha dimensioni di pagina e il numero di pagina e applica appropriata `Skip` e `Take` istruzioni per la `IQueryable`. Quando `ToListAsync` viene chiamato sul `IQueryable`, viene restituito un elenco contenente solo la pagina richiesta. Le proprietà `HasPreviousPage` e `HasNextPage` vengono utilizzati per abilitare o disabilitare **precedente** e **Avanti** pulsanti di spostamento.

Il `CreateAsync` metodo viene utilizzato per creare il `PaginatedList<T>`. Non è possibile creare un costruttore di `PaginatedList<T>` dell'oggetto, i costruttori non possono eseguire codice asincrono.

## <a name="add-paging-functionality-to-the-index-method"></a>Aggiungere funzionalità di paging per il metodo di Index

In *Students/Index.cshtml.cs*, aggiornare il tipo di `Student` da `IList<Student>` a `PaginatedList<Student>`:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Aggiornamento di *Students/Index.cshtml.cs* `OnGetAsync` con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-)]

Il codice precedente aggiunge l'indice della pagina, corrente `sortOrder`e `currentFilter` alla firma del metodo.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Tutti i parametri sono null quando:

* La pagina viene chiamata dal **studenti** collegamento.
* L'utente non è stato selezionato un paging o ordinamento di collegamento.

Quando viene fatto clic su un collegamento di paging, la variabile di indice di pagina contiene il numero di pagina da visualizzare.

`CurrentSort`fornisce la pagina Razor con il criterio di ordinamento corrente. L'ordinamento corrente deve essere inclusi i collegamenti di spostamento per mantenere l'ordinamento durante il paging.

`CurrentFilter`fornisce la pagina Razor con la stringa di filtro corrente. Il `CurrentFilter` valore:

* Devono essere inclusi i collegamenti di spostamento per mantenere le impostazioni di filtro durante il paging.
* Devono essere ripristinati nella casella di testo quando viene nuovamente visualizzata la pagina.

Se la stringa di ricerca viene modificata durante il paging, la pagina viene reimpostata su 1. La pagina deve essere reimpostato su 1, in quanto può comportare dati diversi per visualizzare il nuovo filtro. Quando viene immesso un valore di ricerca e **Invia** è selezionata:

* La stringa di ricerca viene modificata.
* Il `searchString` parametro non è null.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

Il `PaginatedList.CreateAsync` metodo converte la query di studenti in una singola pagina degli studenti iscritti a un tipo di insieme che supporta il paging. Pagina singola di studenti viene passato alla pagina Razor.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

I due punti interrogativi in `PaginatedList.CreateAsync` rappresentano il [operatore null-coalescing](https://docs.microsoft.com/ dotnet/csharp/language-reference/operators/null-conditional-operator). L'operatore null-coalescing definisce un valore predefinito per un tipo nullable. L'espressione `(pageIndex ?? 1)` significa restituisca il valore di `pageIndex` se ha un valore. Se `pageIndex` non hanno un valore, restituiscono 1.

## <a name="add-paging-links-to-the-student-razor-page"></a>Aggiungere collegamenti di spostamento allo studente pagina Razor

Aggiornare il markup in *Students/Index.cshtml*. Le modifiche vengono evidenziate:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-)]

I collegamenti di intestazione di colonna utilizzano la stringa di query per passare la stringa di ricerca corrente per il `OnGetAsync` metodo in modo che l'utente può ordinare all'interno dei risultati di filtro:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=28-31)]

Per gli helper di tag vengono visualizzati i pulsanti di paging:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=72-)]

Eseguire l'app e passare alla pagina di studenti.

* Per rendere che funziona di paging, fare clic sui collegamenti di spostamento in diversi tipi di ordinamento.
* Per verificare che il paging funziona correttamente con ordinamento e filtro, immettere una stringa di ricerca e provare il paging.

![Gli studenti indice pagina con collegamenti di spostamento](sort-filter-page/_static/paging.png)

Per ottenere una migliore comprensione del codice:

* In *Student/Index.cshtml.cs*, impostare un punto di interruzione `switch (sortOrder)`.
* Aggiungere un'espressione di controllo per `NameSort`, `DateSort`, `CurrentSort`, e `Model.Student.PageIndex`.
* In *Student/Index.cshtml*, impostare un punto di interruzione `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Eseguire il debugger.

## <a name="update-the-about-page-to-show-student-statistics"></a>Aggiornare la pagina di informazioni su per visualizzare le statistiche di student

In questo passaggio, *Pages/About.cshtml* viene aggiornato per visualizzare il numero di studenti sono registrati per ciascuna data di registrazione. L'aggiornamento viene utilizzato il raggruppamento e include i passaggi seguenti:

* Creare una classe di modello di visualizzazione per i dati utilizzati dal **su** pagina.
* Modificare il file code-behind e sulla pagina Razor.

### <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Creare un *SchoolViewModels* cartella la *modelli* cartella.

Nel *SchoolViewModels* cartella, aggiungere un *EnrollmentDateGroup.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-code-behind-page"></a>Aggiornare la pagina code-behind di informazioni su

Aggiornamento di *Pages/About.cshtml.cs* file con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/About.cshtml.cs)]

Raggruppa le entità di studenti per data di registrazione dell'istruzione LINQ, calcola il numero di entità in ciascun gruppo e archivia i risultati in una raccolta di `EnrollmentDateGroup` consente di visualizzare gli oggetti del modello.

Nota: LINQ `group` comando attualmente non è supportato da Entity Framework Core. Nel codice precedente, vengono restituiti tutti i record di studenti da SQL Server. Il `group` comando viene applicato nell'app pagine Razor, non in SQL Server. Core EF 2.1 supporterà questa LINQ `group` operatore e il raggruppamento si verifica in SQL Server. Vedere [relazionale: supporta la conversione di GroupBy per SQL](https://github.com/aspnet/EntityFrameworkCore/issues/2341). [Core EF 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/roadmap) verrà rilasciata con .NET Core 2.1. Per ulteriori informazioni, vedere il [Guida di orientamento per componenti di base .NET](https://github.com/dotnet/core/blob/master/roadmap.md).

### <a name="modify-the-about-razor-page"></a>Modificare le informazioni sulla pagina Razor

Sostituire il codice di *Views/Home/About.cshtml* file con il codice seguente:

[!code-html[](intro/samples/cu/Pages/About.cshtml)]

Eseguire l'app e passare alla pagina di informazioni. Il numero di studenti per ogni data di registrazione viene visualizzato in una tabella.

Se si verificano problemi, è possibile risolvere, scaricare il [app completata per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![Informazioni sulla pagina](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Debug dell'origine ASP.NET Core 2.x](https://github.com/aspnet/Docs/issues/4155)

Nella prossima esercitazione, l'app Usa le migrazioni per aggiornare il modello di dati.

>[!div class="step-by-step"]
[Precedente](xref:data/ef-rp/crud)
[Successivo](xref:data/ef-rp/migrations)
