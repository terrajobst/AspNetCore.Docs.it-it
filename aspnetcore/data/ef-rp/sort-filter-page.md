---
title: Razor Pages con EF Core in ASP.NET Core - Ordinamento, filtro, suddivisione in pagine - 3 di 8
author: rick-anderson
description: In questa esercitazione viene spiegato come aggiungere alla pagina le funzionalità di ordinamento, filtro e suddivisione in pagine tramite ASP.NET Core e Entity Framework.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 162586059e3115bc15efaa63a9a0652e09872f1b
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209965"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---sort-filter-paging---3-of-8"></a>Razor Pages con EF Core in ASP.NET Core - Ordinamento, filtro, suddivisione in pagine - 3 di 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Di [Tom Dykstra](https://github.com/tdykstra), [Rick Anderson](https://twitter.com/RickAndMSFT) e [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

In questa esercitazione viene aggiunta la funzionalità di ordinamento, filtro, suddivisione in pagine e raggruppamento.

La figura seguente illustra una pagina completata. Le intestazioni di colonna sono collegamenti selezionabili per ordinare la colonna. Facendo clic più volte su un'intestazione di colonna è possibile passare dall'ordinamento crescente a quello decrescente e viceversa.

![Pagina Student Index (Indice degli studenti)](sort-filter-page/_static/paging.png)

Se si verificano problemi che non si è in grado di risolvere, scaricare l'[app completa](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

## <a name="add-sorting-to-the-index-page"></a>Aggiungere l'ordinamento alla pagina Index (Indice)

Aggiungere stringhe al `PageModel` *Students/Index.cshtml.cs* per contenere i parametri di ordinamento:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]

Aggiornare *Students/Index.cshtml.cs* `OnGetAsync` con il codice seguente:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

Il codice precedente riceve un parametro `sortOrder` dalla stringa di query nell'URL. L'URL (inclusa la stringa di query) viene generato dall'[helper tag di ancoraggio](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

Il parametro `sortOrder` è "Name" o "Data". Il parametro `sortOrder` è facoltativamente seguito da "_desc" per specificare l'ordine decrescente. L'ordinamento predefinito è crescente.

Quando la pagina Index (Indice) viene richiesta dal collegamento **Students** (Studenti), non è presente alcuna stringa di query. Gli studenti vengono visualizzati in ordine crescente in base al cognome. L'ordine crescente in base al cognome è l'impostazione predefinita (caso di fallthrough) nell'istruzione `switch`. Quando l'utente fa clic sul collegamento di un'intestazione di colonna, nel valore della stringa di query viene specificato il valore `sortOrder` appropriato.

Gli elementi `NameSort` e `DateSort` vengono usati dalla pagina Razor per configurare i collegamenti ipertestuali sull'intestazione di colonna con i valori della stringa di query appropriata:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

Il codice seguente contiene l'[operatore condizionale ?:](/dotnet/csharp/language-reference/operators/conditional-operator) di C#:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

La prima riga specifica che quando `sortOrder` è Null o vuoto, `NameSort` è impostato su "name_desc". Se `sortOrder` **non** è Null o vuoto, `NameSort` è impostato su una stringa vuota.

`?: operator` è noto anche come operatore ternario.

Queste due istruzioni consentono alla pagina di impostare i collegamenti ipertestuali delle intestazioni di colonna come indicato di seguito:

| Ordinamento corrente | Collegamento ipertestuale cognome | Collegamento ipertestuale data |
|:--------------------:|:-------------------:|:--------------:|
| Cognome in ordine crescente | descending        | ascending      |
| Cognome in ordine decrescente | ascending           | ascending      |
| Data in ordine crescente       | ascending           | descending     |
| Data in ordine decrescente      | ascending           | ascending      |

Il metodo usa LINQ to Entities per specificare la colonna in base alla quale eseguire l'ordinamento. Il codice inizializza un elemento `IQueryable<Student>` prima dell'istruzione switch e lo modifica nell'istruzione switch:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-999)]

 Quando viene creato o modificato un elemento `IQueryable`, non viene inviata alcuna query al database. La query non viene eseguita finché l'oggetto `IQueryable` non viene convertito in una raccolta. Gli elementi `IQueryable` vengono convertiti in una raccolta chiamando un metodo come ad esempio `ToListAsync`. Il codice `IQueryable` genera pertanto una singola query che non viene eseguita fino all'istruzione seguente:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync` può essere reso dettagliato con un numero elevato di colonne ordinabili.

### <a name="add-column-heading-hyperlinks-to-the-student-index-page"></a>Aggiungere collegamenti ipertestuali delle intestazioni di colonna alla pagina Student Index (Indice degli studenti)

Sostituire il codice in *Students/Index.cshtml*, con il codice evidenziato seguente:

[!code-html[](intro/samples/cu21/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

Il codice precedente:

* Aggiunge collegamenti ipertestuali alle intestazioni di colonna `LastName` e `EnrollmentDate`.
* Usa le informazioni contenute in `NameSort` e `DateSort` per impostare i collegamenti ipertestuali con i valori di ordinamento corrente.

Per verificare il corretto funzionamento dell'ordinamento:

* Eseguire l'app e selezionare la scheda **Students** (Studenti).
* Fare clic su **Last Name** (Cognome).
* Fare clic su **Enrollment Date** (Data di iscrizione).

Per comprendere meglio il codice:

* In *Students/Index.cshtml.cs* impostare un punto di interruzione su `switch (sortOrder)`.
* Aggiungere un'espressione di controllo per `NameSort` e `DateSort`.
* In *Students/Index.cshtml* impostare un punto di interruzione su `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Eseguire il debugger.

## <a name="add-a-search-box-to-the-students-index-page"></a>Aggiungere una casella di ricerca alla pagina Students Index (Indice degli studenti)

Per aggiungere un filtro alla pagina Students Index (Indice degli studenti):

* Alla pagina Razor vengono aggiunti una casella di testo e un pulsante di invio. La casella di testo specifica una stringa di ricerca sul nome o cognome.
* Il modello di pagina viene aggiornato in modo da usare il valore della casella di testo.

### <a name="add-filtering-functionality-to-the-index-method"></a>Aggiungere la funzionalità di filtro al metodo Index

Aggiornare *Students/Index.cshtml.cs* `OnGetAsync` con il codice seguente:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Il codice precedente:

* Aggiunge il parametro `searchString` al metodo `OnGetAsync`. Il valore della stringa di ricerca viene ricevuto da una casella di testo che verrà aggiunta nella sezione seguente.
* Aggiunge una clausola `Where` all'istruzione LINQ. La clausola `Where` seleziona solo gli studenti il cui nome o cognome contiene la stringa di ricerca. L'istruzione LINQ viene eseguita solo se è presente un valore per la ricerca.

Nota: il codice precedente chiama il metodo `Where` su un oggetto `IQueryable` e il filtro viene elaborato nel server. In alcuni scenari l'app potrebbe chiamare il metodo `Where` come metodo di estensione per una raccolta in memoria. Si supponga ad esempio che `_context.Students` cambi da `DbSet`Entity Framework Core a un metodo di repository che restituisce una raccolta `IEnumerable`. Il risultato sarebbe in genere lo stesso, ma in alcuni casi potrebbe essere diverso.

Ad esempio, l'implementazione di .NET Framework di `Contains` esegue un confronto tra maiuscole e minuscole per impostazione predefinita. In SQL Server, la distinzione tra maiuscole e minuscole di `Contains` è determinata dall'impostazione delle regole di confronto dell'istanza di SQL Server. SQL Server usa come valore predefinito la non applicazione della distinzione tra maiuscole e minuscole. È possibile chiamare `ToUpper` per fare in modo che il test non applichi in modo esplicito la distinzione tra maiuscole e minuscole:

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

Il codice precedente assicura che i risultati applichino la distinzione tra maiuscole e minuscole se il codice viene modificato per usare `IEnumerable`. Quando viene chiamato `Contains` su una raccolta `IEnumerable`, viene usata l'implementazione di .NET Core. Quando viene chiamato `Contains` su un oggetto `IQueryable`, viene usata l'implementazione di database. La restituzione di un elemento `IEnumerable` da un repository può compromettere in modo significativo le prestazioni:

1. Tutte le righe vengono restituite dal server di database.
1. Il filtro viene applicato a tutte le righe restituite nell'applicazione.

Si verifica una riduzione delle prestazioni per la chiamata di `ToUpper`. Il codice `ToUpper` aggiunge una funzione nella clausola WHERE dell'istruzione TSQL SELECT. La funzione aggiunta impedisce all'ottimizzazione di usare un indice. Dato che SQL viene installato con l'impostazione che non fa distinzione tra maiuscole e minuscole, è consigliabile evitare di chiamare `ToUpper` quando non è necessario.

### <a name="add-a-search-box-to-the-student-index-page"></a>Aggiungere una casella di ricerca alla pagina Student Index (Indice degli studenti)

In *Pages/Student/Index.cshtml* aggiungere il codice evidenziato seguente per creare un pulsante **Search** (Ricerca) e riquadri diversi.

[!code-html[](intro/samples/cu21/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

Il codice precedente usa l'[helper tag](xref:mvc/views/tag-helpers/intro) `<form>` per aggiungere la casella di testo e il pulsante di ricerca. Per impostazione predefinita, l'helper tag `<form>` invia i dati del modulo con POST. Con POST, i parametri vengono passati nel corpo del messaggio HTTP e non nell'URL. Quando viene usato HTTP GET, i dati del modulo vengono passati nell'URL come stringhe di query. Il passaggio di dati con le stringhe di query consente agli utenti di inserire l'URL nei segnalibri. Le [linee guida W3C](https://www.w3.org/2001/tag/doc/whenToUseGet.html) consigliano di usare l'istruzione GET quando l'azione non risulta in un aggiornamento.

Eseguire il test dell'app:

* Selezionare la scheda **Students** (Studenti) e immettere una stringa di ricerca.
* Selezionare **Search** (Ricerca).

Si noti che l'URL contiene la stringa di ricerca.

```html
http://localhost:5000/Students?SearchString=an
```

Se la pagina è inserita nei segnalibri, il segnalibro contiene l'URL alla pagina e la stringa di query `SearchString`. L'elemento `method="get"` nel tag `form` è ciò che ha determinato la generazione della stringa di query.

Attualmente, quando si seleziona un collegamento di ordinamento di un'intestazione di colonna, il valore del filtro dalla casella **Search** (Ricerca) viene perso. Il valore del filtro perso viene risolto nella sezione successiva.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Aggiungere la funzionalità di suddivisione in pagine alla pagina Students Index (Indice degli studenti)

In questa sezione viene creata una classe `PaginatedList` per supportare la suddivisione in pagine. La classe `PaginatedList` usa le istruzioni `Skip` e `Take` per filtrare i dati sul server invece di recuperare tutte le righe della tabella. Nella figura seguente vengono illustrati i pulsanti di suddivisione in pagine.

![Pagina Student Index (Indice degli studenti) con collegamenti di suddivisione in pagine](sort-filter-page/_static/paging.png)

Nella cartella del progetto, creare `PaginatedList.cs` con il codice seguente:

[!code-csharp[](intro/samples/cu21/PaginatedList.cs)]

Il metodo `CreateAsync` nel codice precedente accetta le dimensioni di pagina e il numero delle pagine e applica le istruzioni `Skip` e `Take` appropriate a `IQueryable`. Quando `ToListAsync` viene chiamato su `IQueryable`, restituisce un elenco contenente solo la pagina richiesta. Le proprietà `HasPreviousPage` e `HasNextPage` vengono usate per abilitare o disabilitare i pulsanti di suddivisione in pagine **Previous** (Indietro) e **Next** (Avanti).

Il metodo `CreateAsync` viene usato per creare l'elemento `PaginatedList<T>`. Un costruttore non può creare l'oggetto `PaginatedList<T>`, poiché i costruttori non possono eseguire codice asincrono.

## <a name="add-paging-functionality-to-the-index-method"></a>Aggiungere la funzionalità di suddivisione in pagine al metodo Index

In *Students/Index.cshtml.cs*, aggiornare il tipo di `Student` da `IList<Student>` a `PaginatedList<Student>`:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Aggiornare *Students/Index.cshtml.cs* `OnGetAsync` con il codice seguente:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-999)]

Il codice precedente aggiunge l'indice della pagina, l'elemento `sortOrder` corrente e l'elemento `currentFilter` alla firma del metodo.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Tutti i parametri sono Null quando:

* La pagina viene chiamata dal collegamento **Students** (Studenti).
* L'utente non ha selezionato un collegamento di suddivisione in pagine o di ordinamento.

Se si seleziona un collegamento di suddivisione in pagine, la variabile dell'indice di pagina contiene il numero di pagina da visualizzare.

`CurrentSort` indica la pagina Razor con il criterio di ordinamento corrente. L'ordinamento corrente deve essere incluso nei collegamenti di suddivisione in pagine per mantenere l'ordinamento nella suddivisione in pagine.

`CurrentFilter` indica la pagina Razor con la stringa di filtro corrente. Il valore `CurrentFilter`:

* Deve essere incluso nei collegamenti di suddivisione in pagine per mantenere le impostazioni di filtro nella suddivisione in pagine.
* Deve essere ripristinato nella casella di testo quando viene nuovamente visualizzata la pagina.

Se la stringa di ricerca viene modificata durante la suddivisione in pagine, la pagina viene reimpostata su 1. La pagina deve essere reimpostata su 1, poiché il nuovo filtro può comportare la visualizzazione di dati diversi. Quando viene immesso un valore di ricerca e si seleziona **Submit** (Invia):

* La stringa di ricerca viene modificata.
* Il parametro `searchString` non è Null.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

Il metodo `PaginatedList.CreateAsync` converte la query degli studenti in una pagina singola di studenti in un tipo di raccolta che supporta la suddivisione in pagine. La pagina singola di studenti viene quindi passata alla pagina Razor.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

I due punti interrogativi in `PaginatedList.CreateAsync` rappresentano l'[operatore null-coalescing](/dotnet/csharp/language-reference/operators/null-conditional-operator). L'operatore null-coalescing definisce un valore predefinito per un tipo nullable. L'espressione `(pageIndex ?? 1)` restituisce il valore di `pageIndex` se ha un valore. Se `pageIndex` non ha un valore, restituisce 1.

## <a name="add-paging-links-to-the-student-razor-page"></a>Aggiungere collegamenti di suddivisione in pagine alla pagina Razor degli studenti

Aggiornare il markup in *Students/Index.cshtml*. Le modifiche sono evidenziate:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-999)]

I collegamenti delle intestazioni di colonna usano la stringa di query per passare la stringa di ricerca corrente al metodo `OnGetAsync` in modo che l'utente possa procedere all'ordinamento all'interno dei risultati di filtro:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=28-31)]

I pulsanti di suddivisione in pagine vengono visualizzati dagli helper tag:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=72-)]

Eseguire l'app e passare alla pagina degli studenti.

* Per verificare che la suddivisione in pagine funzioni correttamente, fare clic sui collegamenti di suddivisione in pagine in ordinamenti diversi.
* Per verificare che la suddivisione in pagine funzioni correttamente con l'ordinamento e il filtro, immettere una stringa di ricerca e provare nuovamente la suddivisione in pagine.

![pagina Student Index (Indice degli studenti) con collegamenti di suddivisione in pagine](sort-filter-page/_static/paging.png)

Per comprendere meglio il codice:

* In *Students/Index.cshtml.cs* impostare un punto di interruzione su `switch (sortOrder)`.
* Aggiungere un'espressione di controllo per `NameSort`, `DateSort`, `CurrentSort` e `Model.Student.PageIndex`.
* In *Students/Index.cshtml* impostare un punto di interruzione su `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Eseguire il debugger.

## <a name="update-the-about-page-to-show-student-statistics"></a>Aggiornare la pagina About (Informazioni) per visualizzare le statistiche degli studenti

In questo passaggio, *Pages/About.cshtml* viene aggiornato per visualizzare il numero di studenti iscritti per ogni data di registrazione. L'aggiornamento usa il raggruppamento e include i passaggi seguenti:

* Creare un modello di visualizzazione per i dati usati dalla pagina **About** (Informazioni).
* Aggiornare la pagina About in modo che usi il modello di visualizzazione.

### <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Creare una cartella *SchoolViewModels* nella cartella *Models*.

Nella cartella *SchoolViewModels* aggiungere un elemento *EnrollmentDateGroup.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu21/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>Aggiornare il modello di pagina About (Informazioni)

I modelli Web in ASP.NET Core 2.2 non includono la pagina About (Informazioni). Se si usa ASP.NET Core 2.2, creare la pagina Razor About (Informazioni).

Aggiornare il file *Pages/About.cshtml.cs* file con il codice seguente:

[!code-csharp[](intro/samples/cu21/Pages/About.cshtml.cs)]

L'istruzione LINQ raggruppa le entità di studenti per data di registrazione, calcola il numero di entità in ogni gruppo e archivia i risultati in una raccolta di oggetti di modello della visualizzazione `EnrollmentDateGroup`.

### <a name="modify-the-about-razor-page"></a>Modificare la pagina Razor About (Informazioni)

Sostituire il codice nel file *Pages/About.cshtml* con il codice seguente:

[!code-html[](intro/samples/cu21/Pages/About.cshtml)]

Eseguire l'app e passare alla pagina About (Informazioni). Il numero di studenti per ogni data di registrazione viene visualizzato in una tabella.

Se si verificano problemi che non è possibile risolvere, scaricare l'[app completa per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![Pagina About (Informazioni)](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Debug dell'origine ASP.NET Core 2.x](https://github.com/aspnet/Docs/issues/4155)
* [Versione YouTube dell'esercitazione](https://www.youtube.com/watch?v=MDs7PFpoMqI)

Nella prossima esercitazione, l'app usa le migrazioni per aggiornare il modello di dati.

::: moniker-end

> [!div class="step-by-step"]
> [Precedente](xref:data/ef-rp/crud)
> [Successivo](xref:data/ef-rp/migrations)
