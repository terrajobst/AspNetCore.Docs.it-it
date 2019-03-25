---
title: Razor Pages con EF Core in ASP.NET Core - Leggere dati correlati - 6 di 8
author: rick-anderson
description: In questa esercitazione verranno letti e visualizzati dati correlati, ovvero dati che Entity Framework carica all'interno delle proprietà di navigazione.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 4b564e9e407dcb6b7fd71d0a6c41596269ed5e09
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320121"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a>Razor Pages con EF Core in ASP.NET Core - Leggere dati correlati - 6 di 8

Di [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog) e [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

In questa esercitazione vengono letti e visualizzati dati correlati. I dati correlati sono dati che EF Core carica all'interno delle proprietà di navigazione.

Se si verificano problemi che non si è in grado di risolvere, [scaricare o visualizzare l'app completa](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). [Istruzioni per il download](xref:index#how-to-download-a-sample).

Le figure seguenti mostrano le pagine completate per questa esercitazione:

![Pagina di indice dei corsi](read-related-data/_static/courses-index.png)

![Pagina di indice degli insegnanti](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Caricamento eager, esplicito e lazy dei dati correlati

Esistono diversi modi con cui EF Core può caricare i dati correlati nelle proprietà di navigazione di un'entità:

* [Caricamento eager](/ef/core/querying/related-data#eager-loading). Il caricamento eager si verifica quando una query per un solo tipo di entità carica anche entità correlate. Quando l'entità viene letta, vengono recuperati i dati correlati corrispondenti. Ciò in genere ha come risultato una query join singola che recupera tutti i dati necessari. Per alcuni tipi di caricamento eager, EF Core genera più query. La generazione di più query può essere più efficiente rispetto al caso di alcune query in EF6, in cui era presente una singola query. Il caricamento eager viene specificato con i metodi `Include` e `ThenInclude`.

  ![Esempio di caricamento eager](read-related-data/_static/eager-loading.png)
 
  Il caricamento eager invia più query quando è inclusa una navigazione di raccolte:

  * Una query per la query principale 
  * Una query per ogni raccolta "perimetrale" nell'albero del caricamento.

* Query separate con `Load`: i dati possono essere recuperati in query separate ed EF Core "corregge" le proprietà di navigazione. "Corregge" significa che EF Core popola automaticamente le proprietà di navigazione. Le query separate con `Load` sono più simili a un caricamento esplicito che a un caricamento eager.

  ![Esempio di query separate](read-related-data/_static/separate-queries.png)

  Nota: EF Core corregge automaticamente le proprietà di navigazione per qualsiasi altra entità caricata in precedenza nell'istanza di contesto. Anche se i dati per una proprietà di navigazione *non* sono inclusi in modo esplicito, la proprietà può comunque essere popolata se alcune o tutte le entità correlate sono state caricate in precedenza.

* [Caricamento esplicito](/ef/core/querying/related-data#explicit-loading) Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati. Per recuperare i dati correlati quando necessario, è necessario scrivere codice. Il caricamento esplicito con query separate ha come risultato l'invio di più query al database. Con il caricamento esplicito, il codice specifica le proprietà di navigazione da caricare. Per eseguire il caricamento esplicito, usare il metodo `Load`. Ad esempio:

  ![Esempio di caricamento esplicito](read-related-data/_static/explicit-loading.png)

* [Caricamento lazy](/ef/core/querying/related-data#lazy-loading). [Il caricamento lazy è stato aggiunto a EF Core nella versione 2.1](/ef/core/querying/related-data#lazy-loading). Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati. La prima volta che si accede a una proprietà di navigazione, i dati necessari per quest'ultima vengono recuperati automaticamente. Ogni volta che si accede a una proprietà di navigazione per la prima volta, viene inviata una query al database.

* L'operatore `Select` carica solo i dati correlati necessari.

## <a name="create-a-course-page-that-displays-department-name"></a>Creare una pagina Course che visualizza il nome dei dipartimenti

L'entità Course include una proprietà di navigazione che contiene l'entità `Department`. L'entità `Department` contiene il dipartimento a cui il corso è assegnato.

Per visualizzare il nome del dipartimento assegnato in un elenco dei corsi:

* Ottenere la proprietà `Name` dall'entità `Department`.
* L'entità `Department` proviene dalla proprietà di navigazione `Course.Department`.

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a>Scaffolding del modello Course

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Seguire le istruzioni in [Eseguire lo scaffolding del modello Student (Studente)](xref:data/ef-rp/intro#scaffold-the-student-model) e usare `Course` per la classe modello.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

 Eseguire il comando seguente:

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

Il comando precedente esegue lo scaffolding del modello `Course`. Aprire il progetto in Visual Studio.

Aprire *Pages/Courses/Index.cshtml.cs* ed esaminare il metodo `OnGetAsync`. Il motore di scaffolding ha specificato il caricamento eager per la proprietà di navigazione `Department`. Il metodo `Include` specifica il caricamento eager.

Eseguire l'app e selezionare il collegamento **Courses** (Corsi). La colonna dei dipartimenti visualizza il `DepartmentID`, che non è utile.

Aggiornare il metodo `OnGetAsync` con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Il codice precedente aggiunge `AsNoTracking`. `AsNoTracking` migliora le prestazioni, perché le entità restituite non vengono registrate, dato che non vengono aggiornate nel contesto corrente.

Aggiornare *Pages/Courses/Index.cshtml* con il markup evidenziato seguente:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Al codice con scaffolding sono state apportate le modifiche seguenti:

* Il titolo è stato modificato da Index (Indice) a Courses (Corsi).
* È stata aggiunta la colonna **Number** (Numero) con il valore della proprietà `CourseID`. Per impostazione predefinita, non viene eseguito lo scaffolding delle chiavi primarie, perché in genere non sono significative per gli utenti finali. In questo caso, tuttavia, la chiave primaria è significativa.
* Modificare la colonna **Department** (Dipartimento) per visualizzare il nome del dipartimento. Il codice visualizza la proprietà `Name` dell'entità `Department` che viene caricata nella proprietà di navigazione `Department`:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Eseguire l'app e selezionare la scheda **Courses** (Corsi) per visualizzare l'elenco con i nomi dei dipartimenti.

![Pagina di indice dei corsi](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a>Caricamento di dati correlati con Select

Il metodo `OnGetAsync` carica i dati correlati con il metodo `Include`:

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

L'operatore `Select` carica solo i dati correlati necessari. Per singoli elementi, ad esempio per `Department.Name` usa un INNER JOIN SQL. Per le raccolte usa un altro accesso al database, ma anche l'operatore `Include` fa lo stesso sulle raccolte.

Il codice seguente carica dati correlati con il metodo `Select`:

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

`CourseViewModel`:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Per un esempio completo, vedere [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) e [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs).

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Creare una pagina Instructors (Insegnanti) che mostri i corsi e le iscrizioni

In questa sezione viene creata la pagina Instructors (Insegnanti).

<a name="IP"></a>
![Pagina di indice degli insegnanti](read-related-data/_static/instructors-index.png)

Questa pagina legge e visualizza dati correlati nei modi seguenti:

* L'elenco di insegnanti visualizza dati correlati provenienti dall'entità `OfficeAssignment`, Office (Ufficio) nella figura precedente. Tra le entità `Instructor` e `OfficeAssignment` c'è una relazione uno-a-zero-o-uno. Per le entità `OfficeAssignment` viene usato il caricamento eager. Il caricamento eager è in genere più efficiente quando i dati correlati devono essere visualizzati. In questo caso, devono essere visualizzate le assegnazioni di ufficio degli insegnanti.
* Quando l'utente seleziona un insegnante (Harui nella figura precedente), vengono visualizzate le entità `Course` correlate. Tra le entità `Instructor` e `Course` esiste una relazione molti-a-molti. Il caricamento eager viene usato per le entità `Course` e le entità `Department` correlate. In questo caso, query separate potrebbero essere più efficienti, perché sono necessari solo i corsi per l'insegnante selezionato. Questo esempio illustra come usare il caricamento eager per le proprietà di navigazione di entità all'interno di proprietà di navigazione.
* Quando l'utente seleziona un corso (Chemistry (Chimica) nella figura precedente), vengono visualizzati i dati correlati dell'entità `Enrollments`. Nella figura precedente, vengono visualizzati il voto e il nome degli studenti. Tra le entità `Course` e `Enrollment` esiste una relazione uno-a-molti.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Creare un modello per la visualizzazione dell'indice degli insegnanti

La pagina Instructors (Insegnanti) mostra i dati di tre tabelle diverse. Viene creato un modello di visualizzazione che include le tre entità che rappresentano le tre tabelle.

Nella cartella *SchoolViewModels* creare *InstructorIndexData.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Scaffolding del modello Instructor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Seguire le istruzioni in [Eseguire lo scaffolding del modello Student (Studente)](xref:data/ef-rp/intro#scaffold-the-student-model) e usare `Instructor` per la classe modello.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

 Eseguire il comando seguente:

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

Il comando precedente esegue lo scaffolding del modello `Instructor`. Eseguire l'app e passare alla pagina Instructors (Insegnanti).

Sostituire *Pages/Instructors/Index.cshtml.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

Il metodo `OnGetAsync` accetta i dati di route facoltativi per l'ID dell'insegnante selezionato.

Esaminare la query nel file *Pages/Instructors/Index.cshtml.cs*:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

La query ha due istruzioni Include, che riguardano:

* `OfficeAssignment`: visualizzato nella [visualizzazione degli insegnanti](#IP).
* `CourseAssignments`: visualizza i corsi tenuti.

### <a name="update-the-instructors-index-page"></a>Aggiornare la pagina di indice degli insegnanti

Aggiornare *Pages/Instructors/Index.cshtml* con il markup seguente:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

Il markup precedente apporta le modifiche seguenti:

* Aggiorna la direttiva `page` da `@page` a `@page "{id:int?}"`. `"{id:int?}"` è un modello di route. Il modello di route cambia le stringhe di query di tipo integer nell'URL in dati di route. Se ad esempio si fa clic su sul collegamento **Select** (Seleziona) per un insegnante con la sola direttiva `@page`, viene generato un URL come il seguente:

  `http://localhost:1234/Instructors?id=2`

  Quando la direttiva della pagina è `@page "{id:int?}"`, l'URL precedente è:

  `http://localhost:1234/Instructors/2`

* Il titolo pagina è **Instructors** (Insegnanti).
* È stata aggiunta la colonna **Office** (Ufficio) che visualizza `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non è Null. Poiché questa è una relazione uno-a-zero-o-uno, potrebbe non esserci un'entità OfficeAssignment correlata.

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* È stata aggiunta la colonna **Courses** (Corsi) che visualizza i corsi tenuti da ogni insegnante. Per altre informazioni su questa sintassi Razor, vedere [Transizione riga esplicita con `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) .

* È stato aggiunto codice che aggiunge `class="success"` in modo dinamico all'elemento `tr` dell'insegnante selezionato. In questo modo viene impostato un colore di sfondo per la riga selezionata tramite una classe Bootstrap.

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* È stato aggiunto un nuovo collegamento ipertestuale con etichetta **Select** (Seleziona). Questo collegamento invia l'ID dell'insegnante selezionato al metodo `Index` e imposta un colore di sfondo.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Eseguire l'app e selezionare la scheda **Instructors** (Insegnanti). La pagina visualizza `Location` (Ufficio) dall'entità `OfficeAssignment` correlata. Se OfficeAssignment è Null, nella tabella viene visualizzata una cella vuota.

![Pagina di indice degli insegnanti con nessuna selezione](read-related-data/_static/instructors-index-no-selection.png)

Fare clic sul collegamento **Select** (Seleziona). Lo stile delle righe cambia.

### <a name="add-courses-taught-by-selected-instructor"></a>Aggiungere corsi tenuti dall'insegnante selezionato

Aggiornare il metodo `OnGetAsync` in *Pages/Instructors/Index.cshtml.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

Aggiungere `public int CourseID { get; set; }`

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

Esaminare la query aggiornata:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

La query precedente aggiunge l'entità `Department`.

Il codice seguente viene eseguito quando viene selezionato un insegnante (`id != null`). L'insegnante selezionato viene recuperato dall'elenco di insegnanti nel modello di visualizzazione. La proprietà `Courses` del modello di visualizzazione viene caricata con le entità `Course` dalla proprietà di navigazione `CourseAssignments` di tale insegnante.

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

Il metodo `Where` restituisce una raccolta. Nel metodo `Where` precedente viene restituita una sola entità `Instructor`. Il metodo `Single` converte la raccolta in un'unica entità `Instructor`. L'entità `Instructor` consente l'accesso alla proprietà `CourseAssignments`. `CourseAssignments` consente l'accesso alle entità `Course` correlate.

![Relazione m:M tra insegnante e corsi](complex-data-model/_static/courseassignment.png)

Il metodo `Single` viene usato in una raccolta quando quest'ultima ha un solo elemento. Il metodo `Single` genera un'eccezione se la raccolta è vuota o se contiene più elementi. In alternativa, è possibile usare `SingleOrDefault`, che restituisce un valore predefinito (Null in questo caso) se la raccolta è vuota. L'uso di `SingleOrDefault` per una raccolta vuota:

* Genera un'eccezione, a causa del tentativo di cercare la proprietà `Courses` in un riferimento Null.
* Il messaggio di eccezione indica meno chiaramente la causa del problema.

Il codice seguente popola la proprietà `Enrollments` del modello di visualizzazione quando è selezionato un corso:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Aggiungere il markup seguente alla fine della pagina Razor *Pages/Instructors/Index.cshtml*:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

Quando è selezionato un insegnante, il markup precedente visualizza un elenco dei corsi correlati all'insegnante stesso.

Eseguire il test dell'app. Fare clic su un collegamento **Select** (Seleziona) nella pagina Instructors (Insegnanti).

![Pagina di indice degli insegnanti con un insegnante selezionato](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Visualizzare i dati degli studenti

In questa sezione, l'app viene aggiornata in modo da visualizzare i dati degli studenti per il corso selezionato.

Aggiornare la query nel metodo `OnGetAsync` in *Pages/Instructors/Index.cshtml.cs* con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Aggiornare *Pages/Instructors/Index.cshtml*. Aggiungere il markup seguente alla fine del file:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

Il markup precedente visualizza l'elenco degli studenti iscritti al corso selezionato.

Aggiornare la pagina e selezionare un insegnante. Selezionare un corso per visualizzare l'elenco degli studenti iscritti e i voti corrispondenti.

![Pagina di indice degli insegnanti con un corso selezionato](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Usare Single

Il metodo `Single` può passare la condizione `Where` anziché chiamare il metodo `Where` separatamente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

L'approccio `Single` precedente non offre alcun vantaggio rispetto all'uso di `Where`. Alcuni sviluppatori preferiscono lo stile dell'approccio `Single`.

## <a name="explicit-loading"></a>Caricamento esplicito

Il codice corrente specifica il caricamento eager per `Enrollments` e `Students`:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Si supponga che gli utenti richiedano raramente la visualizzazione delle iscrizioni a un corso. In questo caso, per ottimizzare il funzionamento dell'app è utile caricare i dati delle iscrizioni solo se vengono richiesti. In questa sezione, viene eseguito l'aggiornamento di `OnGetAsync` in modo da usare il caricamento esplicito di `Enrollments` e `Students`.

Aggiornare `OnGetAsync` con il codice seguente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

Il codice precedente rilascia le chiamate del metodo *ThenInclude* per i dati delle iscrizioni e degli studenti. Se è selezionato un corso, il codice evidenziato recupera:

* Le entità `Enrollment` per il corso selezionato.
* Le entità `Student` per ogni entità `Enrollment`.

Si noti che nel codice precedente `.AsNoTracking()` è commentato. Le proprietà di navigazione possono solo essere caricate in modo esplicito per le entità registrate.

Eseguire il test dell'app. Dal punto di vista degli utenti, l'app si comporta in modo identico alla versione precedente.

La prossima esercitazione illustra come aggiornare i dati correlati.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Versione YouTube dell'esercitazione (parte 1)](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [Versione YouTube dell'esercitazione (parte 2)](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
>[Precedente](xref:data/ef-rp/complex-data-model)
>[Successivo](xref:data/ef-rp/update-related-data)
