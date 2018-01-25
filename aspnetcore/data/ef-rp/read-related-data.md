---
title: Pagine Razor con Entity Framework Core - leggere i dati correlati - 6 di 8
author: rick-anderson
description: "In questa esercitazione si leggere e visualizzare i dati correlati, ovvero i dati di Entity Framework carica le proprietà di navigazione."
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 532020a8fe4c5a0312cbd89278e61f614b1825f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a>Lettura correlati dati - Core EF con pagine Razor (6 di 8)

Da [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

In questa esercitazione, i dati correlati vengano letto e visualizzati. I dati correlati sono dati EF Core carica le proprietà di navigazione.

Se si verificano problemi, è possibile risolvere, scaricare il [app completata per questa fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).

Le illustrazioni seguenti mostrano le pagine completate per questa esercitazione:

![Pagina di indice corsi](read-related-data/_static/courses-index.png)

![Pagina di indice istruttori](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Caricamento eager esplicito e lazy dei dati correlati

Esistono diversi modi EF Core può caricare i dati correlati alle proprietà di navigazione di un'entità:

* [Caricamento eager](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading). Caricamento eager è quando una query per un tipo di entità anche Carica entità correlate. Quando l'entità è di lettura, vengono recuperati i dati correlati. Ciò comporta in genere una query singola join che recupera tutti i dati necessari. Core EF emetterà più query per alcuni tipi di caricamento eager. Può essere più efficiente rispetto a quanto nel caso di alcune query in EF6 emette più query in cui era presente una singola query. Caricamento eager è specificato con il `Include` e `ThenInclude` metodi.

 ![Esempio di caricamento eager](read-related-data/_static/eager-loading.png)
 
 Caricamento eager invierà più query è incluso un nvavigation raccolta:

 * Una query per la query principale 
 * Una query per ogni raccolta di "margine" della struttura del carico.

* Separare le query con `Load`: I dati possono essere recuperati nelle query separata e Core EF "corregge" le proprietà di navigazione. "correzioni backup" significa che EF Core popola automaticamente le proprietà di navigazione. Separare le query con `Load` è più simile Explicit durante il caricamento di caricamento eager.

 ![Esempio di query distinte](read-related-data/_static/separate-queries.png)

 Nota: EF Core consente di correggere automaticamente le proprietà di navigazione per qualsiasi altra entità che sono stata caricata in precedenza l'istanza del contesto. Anche se i dati per una proprietà di navigazione sono *non* inclusi in modo esplicito, la proprietà comunque possibile popolare se alcune o tutte le entità correlate sono state caricate in precedenza.

* [Caricamento esplicito](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading). Durante la lettura di entità, non recuperare i dati correlati. Codice deve essere scritto per recuperare i dati correlati quando necessario. Risultati di caricamento esplicito con query separate in più query inviate al database. Con il caricamento esplicito, il codice specifica le proprietà di navigazione da caricare. Utilizzare il `Load` metodo per eseguire il caricamento esplicito. Ad esempio:

 ![Esempio di caricamento esplicito](read-related-data/_static/explicit-loading.png)

* [Caricamento lazy](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading). [Core EF non supporta attualmente il caricamento lazy](https://github.com/aspnet/EntityFrameworkCore/issues/3797). Durante la lettura di entità, non recuperare i dati correlati. La prima volta che si accede a una proprietà di navigazione, i dati necessari per la proprietà di navigazione viene recuperati automaticamente. Viene inviata una query al database ogni volta che una proprietà di navigazione avviene per la prima volta.

* Il `Select` operatore carica solo i dati correlati necessari.

## <a name="create-a-courses-page-that-displays-department-name"></a>Creare una pagina di corsi che consente di visualizzare il nome di reparto

L'entità Course include una proprietà di navigazione che contiene il `Department` entità. Il `Department` entità contiene il reparto che la linea viene assegnata a.

Per visualizzare il nome del reparto assegnato in un elenco dei corsi:

* Ottenere il `Name` proprietà il `Department` entità.
* Il `Department` entità provengono dal `Course.Department` proprietà di navigazione.

![ourse. Reparto](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>Lo scaffolding il modello di linea

* Uscire da Visual Studio.
* Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).
* Eseguire il comando seguente:

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

Il comando precedente delle strutture di `Course` modello. Aprire il progetto in Visual Studio.

Compilare il progetto. La compilazione genera errori simile al seguente:

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Una modifica globale `_context.Course` a `_context.Courses` (ovvero, aggiungere una "s" `Course`). 7 occorrenze sono disponibili e aggiornate.

Aprire *Pages/Courses/Index.cshtml.cs* ed esaminare il `OnGetAsync` metodo. Il motore di scaffolding specificato per il caricamento non differito il `Department` proprietà di navigazione. Il `Include` metodo specifica caricamento eager.

Eseguire l'app e selezionare il **corsi** collegamento. Visualizza la colonna reparto di `DepartmentID`, che non è utile.

Aggiornare il metodo `OnGetAsync` con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Il codice precedente aggiunge `AsNoTracking`. `AsNoTracking`migliora le prestazioni perché non vengono rilevate le entità restituite. Le entità non vengono rilevate in quanto non verranno aggiornati nel contesto corrente.

Aggiornamento *Views/Courses/Index.cshtml* con il markup evidenziato seguente:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Le seguenti modifiche sono state apportate al codice scaffolding:

* Modificare il titolo dall'indice ai corsi.
* Aggiungere un **numero** colonna che mostra il `CourseID` valore della proprietà. Per impostazione predefinita, le chiavi primarie non sono scaffolding perché in genere sono prive di significato per gli utenti finali. Tuttavia, in questo caso la chiave primaria è significativa.
* Modificare il **reparto** colonna per visualizzare il nome di reparto. Consente di visualizzare il codice il `Name` proprietà del `Department` entità che viene caricato il `Department` proprietà di navigazione:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Eseguire l'app e selezionare il **corsi** scheda per visualizzare l'elenco con i nomi di reparto.

![Pagina di indice corsi](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>Caricamento dati con l'istruzione Select di correlati

Il `OnGetAsync` metodo carica i dati correlati con il `Include` metodo:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

Il `Select` operatore carica solo i dati correlati necessari. Per i singoli elementi, ad esempio il `Department.Name` Usa un SQL INNER JOIN. Per le raccolte utilizza un altro accesso al database, ma anche il.`Include` operatore sulle raccolte.

Il codice seguente carica i dati correlati con il `Select` metodo:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

Il `CourseViewModel`:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Vedere [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) per un esempio completo.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Creare una pagina istruttori che mostra i corsi e le registrazioni

In questa sezione viene creata la pagina istruttori.

<a name="IP"></a>
![Pagina di indice istruttori](read-related-data/_static/instructors-index.png)

Questa pagina legge e visualizza i dati correlati nei modi seguenti:

* L'elenco di istruttori vengono visualizzati i dati correlati di `OfficeAssignment` entità (Office nella figura precedente). Il `Instructor` e `OfficeAssignment` entità si trovano in una relazione uno-a-zero-o-uno. Caricamento eager viene utilizzato per il `OfficeAssignment` entità. Caricamento eager è in genere più efficiente quando i dati correlati devono essere visualizzate. In questo caso, le assegnazioni di office per istruttori vengono visualizzate.
* Quando l'utente seleziona un docente (Harui nella figura precedente), correlato `Course` vengono visualizzate le entità. Il `Instructor` e `Course` entità si trovano in una relazione molti-a-molti. Eager caricamento per il `Course` entità e le relative `Department` viene utilizzata l'entità. In questo caso, query separate potrebbero essere più efficiente perché sono necessari solo i corsi per istruttore selezionato. In questo esempio viene illustrato come utilizzare il caricamento immediato per le proprietà di navigazione di entità nelle proprietà di navigazione.
* Quando l'utente seleziona un corso (chimica nella figura precedente), i dati da correlati di `Enrollments` entità viene visualizzata. Nella figura precedente, vengono visualizzati grado e nome di uno studente. Il `Course` e `Enrollment` entità si trovano in una relazione uno-a-molti.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Creare un modello di visualizzazione per la visualizzazione dell'indice Instructor

La pagina istruttori Mostra dati di tre tabelle diverse. Viene creato un modello di visualizzazione che include tre entità che rappresenta le tre tabelle.

Nel *SchoolViewModels* cartella, creare *InstructorIndexData.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Lo scaffolding modello Instructor

* Uscire da Visual Studio.
* Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).
* Eseguire il comando seguente:

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

Il comando precedente delle strutture di `Instructor` modello. Aprire il progetto in Visual Studio.

Compilare il progetto. La compilazione genera errori.

Una modifica globale `_context.Instructor` a `_context.Instructors` (ovvero, aggiungere una "s" `Instructor`). 7 occorrenze sono disponibili e aggiornate.

Eseguire l'app e passare alla pagina i docenti.

Sostituire *Pages/Instructors/Index.cshtml.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

Il `OnGetAsync` metodo accetta i dati della route facoltativo per l'ID di istruttore selezionato.

Esaminare la query nella *Pages/Instructors/Index.cshtml* pagina:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

La query dispone di due include:

* `OfficeAssignment`: Visualizzato nel [Visualizza i docenti](#IP).
* `CourseAssignments`: Porta in corsi tenuti.


### <a name="update-the-instructors-index-page"></a>Aggiornare la pagina di indice istruttori

Aggiornamento *Pages/Instructors/Index.cshtml* con il markup seguente:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Il markup precedente apporta le modifiche seguenti:

* Gli aggiornamenti di `page` direttiva da `@page` a `@page "{id:int?}"`. `"{id:int?}"`è un modello di route. Le stringhe di query di tipo integer nell'URL viene modificato il modello di route per i dati della route. Ad esempio, facendo clic su di **selezionare** collegamento per un docente quando la direttiva di pagina genera un URL simile al seguente:

    `http://localhost:1234/Instructors?id=2`

    Quando la direttiva di pagina è `@page "{id:int?}"`, l'URL precedente è:

    `http://localhost:1234/Instructors/2`

* Titolo della pagina è **i docenti**.
* Aggiungere un **Office** colonna che visualizza `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non è null. Poiché si tratta di una relazione uno-a-zero-o-uno, potrebbe essere presente un'entità OfficeAssignment correlata.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Aggiungere un **corsi** invece di colonna che visualizza i corsi per ciascun insegnante. Vedere [esplicita riga transizione con `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) per altre informazioni su questa sintassi razor.

* Aggiunta di codice che aggiunge dinamicamente `class="success"` per il `tr` elemento istruttore selezionato. Consente di impostare un colore di sfondo per la riga selezionata utilizzando una classe di Bootstrap.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Aggiungere un nuovo collegamento ipertestuale con l'etichetta **selezionare**. Questo collegamento invia l'ID dell'istruttore selezionato per il `Index` (metodo) e imposta un colore di sfondo.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Eseguire l'app e selezionare il **i docenti** scheda. Nella pagina vengono visualizzati il `Location` (office) da correlata `OfficeAssignment` entità. Se OfficeAssignment' è null, viene visualizzata una cella vuota della tabella.

![Pagina di indice istruttori che non selezionato](read-related-data/_static/instructors-index-no-selection.png)

Fare clic su di **selezionare** collegamento. Le modifiche dello stile di riga.

### <a name="add-courses-taught-by-selected-instructor"></a>Aggiungere i corsi tenuti da istruttore selezionato

Aggiornamento di `OnGetAsync` metodo *Pages/Instructors/Index.cshtml.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

Esaminare la query aggiornata:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

La query precedente aggiunge il `Department` entità.

Il codice seguente viene eseguita quando viene selezionato un docente (`id != null`). Istruttore selezionato viene recuperato dall'elenco di istruttori nel modello di visualizzazione. Il modello di visualizzazione `Courses` proprietà viene caricata con le `Course` entità da tale istruttore `CourseAssignments` proprietà di navigazione.

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

Il `Where` metodo restituisce una raccolta. Nel passaggio precedente `Where` un solo metodo `Instructor` entità viene restituita. Il `Single` metodo converte la raccolta in un unico `Instructor` entità. Il `Instructor` fornisce l'accesso alle entità di `CourseAssignments` proprietà. `CourseAssignments`fornisce l'accesso a correlata `Course` entità.

![Per corsi di istruttore m:M](complex-data-model/_static/courseassignment.png)

Il `Single` metodo viene utilizzato in una raccolta per la raccolta contiene un solo elemento. Il `Single` metodo genera un'eccezione se la raccolta è vuota o se è presente più di un elemento. In alternativa, è `SingleOrDefault`, che restituisce un valore predefinito (null in questo caso) se la raccolta è vuota. Utilizzando `SingleOrDefault` su una raccolta vuota:

* Genera un'eccezione (di tentare di trovare un `Courses` proprietà su un riferimento null).
* Il messaggio di eccezione meno chiaramente indica la causa del problema.

Il codice seguente consente di popolare il modello di visualizzazione `Enrollments` proprietà quando è selezionato un corso:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Aggiungere il markup seguente alla fine del *Pages/Courses/Index.cshtml* Razor pagina:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

Il markup precedente visualizza un elenco dei corsi relativi a un docente quando è selezionato un docente.

Testare l'app. Fare clic su un **selezionare** collegamento nella pagina i docenti.

![Istruttore pagina di indice istruttori selezionato](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Mostra i dati degli studenti

In questa sezione, l'app viene aggiornata per mostrare i dati di studenti per un corso selezionato.

Aggiornare la query nella `OnGetAsync` metodo *Pages/Instructors/Index.cshtml.cs* con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Aggiornamento *Pages/Instructors/Index.cshtml*. Aggiungere il markup seguente alla fine del file:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Il markup precedente visualizza un elenco degli studenti iscritti in corso selezionato.

Aggiornare la pagina e selezionare un docente. Selezionare un corso per visualizzare l'elenco degli studenti iscritti e i relativi voti.

![Insegnante di pagina di indice istruttori e corso selezionato](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Utilizzo singolo

Il `Single` metodo può passare il `Where` condizione anziché chiamare il `Where` metodo separatamente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

Precedente `Single` approccio non offre alcun vantaggio rispetto all'uso `Where`. Alcuni sviluppatori preferiscono la `Single` approccio stile.

## <a name="explicit-loading"></a>Caricamento esplicito

Il codice corrente specifica per il caricamento non differito `Enrollments` e `Students`:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Si supponga che gli utenti desiderano raramente vedere le registrazioni in un corso. In tal caso, un'ottimizzazione sarebbe per caricare solo i dati di registrazione, se richiesto. In questa sezione, il `OnGetAsync` viene aggiornato per utilizzare il caricamento esplicito di `Enrollments` e `Students`.

Aggiornamento di `OnGetAsync` con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Elimina il codice precedente il *ThenInclude* chiamate per i dati di registrazione e gli studenti. Se si seleziona un corso, recupera il codice evidenziato di seguito:

* Il `Enrollment` entità per il corso selezionato.
* Il `Student` entità per ogni `Enrollment`.

Si noti il precedente codice commento `.AsNoTracking()`. Le proprietà di navigazione possono solo essere caricate in modo esplicito per le entità rilevate.

Testare l'app. Dal punto di vista degli utenti, l'app si comporta in modo identico alla versione precedente.

L'esercitazione successiva viene illustrato come aggiornare i dati correlati.

>[!div class="step-by-step"]
>[Precedente](xref:data/ef-rp/complex-data-model)
>[Successivo](xref:data/ef-rp/update-related-data)
