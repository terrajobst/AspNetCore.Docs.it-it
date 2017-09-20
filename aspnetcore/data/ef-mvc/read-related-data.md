---
title: Il Core di ASP.NET MVC con Entity Framework Core - leggere i dati correlati - 6 di 10
author: tdykstra
description: "In questa esercitazione verrà leggere e visualizzare i dati correlati, ovvero i dati di Entity Framework carica le proprietà di navigazione."
keywords: ASP.NET Core, Entity Framework Core, i dati correlati, join
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 71fec30f-8ea7-4ca8-96e3-d2e26c5be44e
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: e818411f2cc568afdfd0612a6367dc3e257d0dd7
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2017
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a>Lettura correlati dati - EF Core con l'esercitazione di base di ASP.NET MVC (6 di 10)

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).

Nell'esercitazione precedente, è stato completato il modello di dati dell'istituto di istruzione. In questa esercitazione verrà leggere e visualizzare i dati correlati, ovvero i dati di Entity Framework carica le proprietà di navigazione.

Le illustrazioni seguenti mostrano le pagine che verranno utilizzate.

![Pagina di indice corsi](read-related-data/_static/courses-index.png)

![Pagina di indice istruttori](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Caricamento eager esplicito e lazy dei dati correlati

Esistono diversi modi software Mapping relazionale a oggetti (ORM), ad esempio Entity Framework è possibile caricare i dati correlati alle proprietà di navigazione di un'entità:

* Caricamento eager. Durante la lettura di entità, i dati correlati vengono recuperati con essa. Ciò comporta in genere una query singola join che recupera tutti i dati necessari. Specificare il caricamento non differito in Entity Framework Core utilizzando il `Include` e `ThenInclude` metodi.

  ![Esempio di caricamento eager](read-related-data/_static/eager-loading.png)

  È possibile recuperare alcuni dei dati nella query separate ed EF "correzioni" le proprietà di navigazione.  Vale a dire EF vengono aggiunti automaticamente le entità recuperate separatamente in cui appartengono le proprietà di navigazione di entità recuperate in precedenza. Per la query che recupera i dati correlati, è possibile utilizzare il `Load` metodo anziché un metodo che restituisce un elenco o un oggetto, ad esempio `ToList` o `Single`.

  ![Esempio di query distinte](read-related-data/_static/separate-queries.png)

* Caricamento esplicito. Durante la lettura di entità, non recuperare i dati correlati. Si scrive codice che recupera i dati correlati se necessario. Come nel caso di il caricamento eager con query separate, esplicita, il caricamento dei risultati in più query inviate al database. La differenza è che, con il caricamento esplicito, il codice specifica le proprietà di navigazione da caricare. In Entity Framework Core 1.1 è possibile utilizzare il `Load` metodo per eseguire il caricamento esplicito. Ad esempio:

  ![Esempio di caricamento esplicito](read-related-data/_static/explicit-loading.png)

* Caricamento lazy. Durante la lettura di entità, non recuperare i dati correlati. La prima volta che si tenta di accedere a una proprietà di navigazione, viene tuttavia recuperati automaticamente i dati necessari per la proprietà di navigazione. Invio di una query al database ogni volta che si tenta di ottenere dati da una proprietà di navigazione per la prima volta. Entity Framework Core 1.0 non supporta il caricamento lazy.

### <a name="performance-considerations"></a>Considerazioni sulle prestazioni

Se si conosce che i dati correlati è necessario per tutte le entità recuperate, il caricamento eager spesso offre prestazioni ottimali, perché è in genere più efficiente delle query separate per ogni entità recuperata una singola query inviate al database. Ad esempio, si supponga che ogni reparto di dieci corsi correlati. Caricamento eager di tutti i dati correlati comporta solo una query singola (join) e un singolo round trip al database. Una query separata per i corsi per ogni reparto comporta undici round trip al database. Il round trip aggiuntivi al database sono particolarmente effetti negativi sulle prestazioni quando la latenza è elevata.

D'altra parte, in alcuni scenari è più efficiente query separate. Caricamento eager di tutti i dati correlati in una query può causare un join molto complesso da generare, quale SQL Server in grado di elaborare in modo efficiente. O se si desidera accedere alle proprietà di navigazione di un'entità solo per un subset di un set di entità a cui che si sta elaborando, query separate potrebbero risultare migliori perché il caricamento immediato di tutti gli elementi di sarebbe recuperare più dati superflui. Se le prestazioni sono essenziali, è consigliabile testare le prestazioni di entrambi i modi per adottare la scelta migliore.

## <a name="create-a-courses-page-that-displays-department-name"></a>Creare una pagina di corsi che consente di visualizzare il nome di reparto

L'entità Course include una proprietà di navigazione che contiene l'entità reparto del corso assegnato al reparto. Per visualizzare il nome del reparto assegnato in un elenco dei corsi, è necessario ottenere la proprietà del nome dell'entità di reparto nel `Course.Department` proprietà di navigazione.

Creare un controller denominato CoursesController per il tipo di entità Course, con le stesse opzioni per il **Controller MVC con visualizzazioni, mediante Entity Framework** scaffolder che hai usato in precedenza per il controller di studenti, come illustrato di Nella figura seguente:

![Aggiungi controller corsi](read-related-data/_static/add-courses-controller.png)

Aprire *CoursesController.cs* ed esaminare il `Index` metodo. Lo scaffolding automatica è specificato per il caricamento non differito il `Department` proprietà di navigazione tramite il `Include` metodo.

Sostituire il `Index` (metodo) con il codice seguente che utilizza un nome più appropriato per il `IQueryable` che restituisce le entità Course (`courses` anziché `schoolContext`):

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

Aprire *Views/Courses/Index.cshtml* e sostituire il codice del modello con il codice seguente. Le modifiche vengono evidenziate:

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Le seguenti modifiche apportate al codice scaffolding:

* Modificare il titolo dall'indice ai corsi.

* Aggiungere un **numero** colonna che mostra il `CourseID` valore della proprietà. Per impostazione predefinita, le chiavi primarie non sono scaffolding perché in genere sono utilizzati per gli utenti finali. Tuttavia, in questo caso la chiave primaria è significativa e si desidera visualizzarlo.

* Modificare il **reparto** colonna per visualizzare il nome di reparto. Il codice visualizza il `Name` proprietà dell'entità reparto che viene caricato il `Department` proprietà di navigazione:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Eseguire l'app e selezionare il **corsi** scheda per visualizzare l'elenco con i nomi di reparto.

![Pagina di indice corsi](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Creare una pagina istruttori che mostra i corsi e le registrazioni

In questa sezione si creerà un controller e una visualizzazione per l'entità Instructor per visualizzare la pagina istruttori:

![Pagina di indice istruttori](read-related-data/_static/instructors-index.png)

Questa pagina legge e visualizza i dati correlati nei modi seguenti:

* L'elenco di istruttori Mostra dati correlati nell'entità OfficeAssignment. Le entità Instructor e OfficeAssignment sono in una relazione uno-a-zero-o-uno. Caricamento eager userai per le entità OfficeAssignment. Come spiegato in precedenza, il caricamento non differito è in genere più efficiente quando i dati correlati è necessario per tutte le righe recuperate della tabella primaria. In questo caso, si desidera visualizzare le assegnazioni di office per tutti i docenti visualizzati.

* Quando l'utente seleziona un docente, vengono visualizzate le entità Course correlate. Le entità Instructor e corso hanno una relazione molti-a-molti. Caricamento eager da usare per le entità Course e le relative entità correlate di reparto. In questo caso, query separate potrebbero essere più efficiente perché è necessario corsi solo per istruttore selezionato. Tuttavia, in questo esempio viene illustrato come utilizzare il caricamento immediato per le proprietà di navigazione all'interno delle entità che sono a loro volta nelle proprietà di navigazione.

* Quando l'utente seleziona un corso, viene visualizzati i dati correlati dal set di entità le registrazioni. Le entità Course e registrazione sono in una relazione uno-a-molti. Utilizzare query separate per le entità di registrazione e le relative entità Student correlati.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Creare un modello di visualizzazione per la visualizzazione dell'indice Instructor

La pagina istruttori Mostra dati di tre tabelle diverse. Pertanto, si creerà un modello di visualizzazione che include tre proprietà, ciascuna contenente i dati per una delle tabelle.

Nel *SchoolViewModels* cartella, creare *InstructorIndexData.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Creare le visualizzazioni e controller Instructor

Creare un controller istruttori con azioni di lettura/scrittura EF come illustrato nella figura seguente:

![Aggiungere i docenti controller](read-related-data/_static/add-instructors-controller.png)

Aprire *InstructorsController.cs* e aggiungere un tramite l'istruzione per lo spazio dei nomi ViewModel:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

Sostituire il metodo di indice con il codice seguente per eseguire il caricamento immediato di dati correlati e inserirlo nel modello di visualizzazione.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

Il metodo accetta i dati della route facoltativo (`id`) e un parametro di stringa di query (`courseID`) che forniscono i valori di ID del corso selezionato e istruttore selezionato. I parametri sono forniti dal **selezionare** della pagina.

Il codice inizia creando un'istanza del modello di visualizzazione e inserire in essa contenuti l'elenco di istruttori. Il codice specifica per il caricamento immediato di `Instructor.OfficeAssignment` e `Instructor.CourseAssignments` le proprietà di navigazione. All'interno di `CourseAssignments` proprietà, il `Course` è caricato all'interno della quale, il `Enrollments` e `Department` proprietà vengono caricati e all'interno di ogni `Enrollment` entità il `Student` proprietà è caricata.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

Poiché la vista richiede sempre l'entità OfficeAssignment, è più efficiente recuperare che nella stessa query. Le entità Course sono necessari quando viene selezionato un docente nella pagina web, una singola query è migliore di più query solo se la pagina viene visualizzata più spesso con un corso selezionato rispetto senza.

Il codice ripete `CourseAssignments` e `Course` perché è necessario due proprietà da `Course`. La prima stringa di `ThenInclude` chiama Ottiene `CourseAssignment.Course`, `Course.Enrollments`, e `Enrollment.Student`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

A questo punto nel codice, un altro `ThenInclude` per proprietà di navigazione di `Student`, che non è necessario. Ma la chiamata `Include` ricomincia con `Instructor` proprietà, pertanto è necessario passare attraverso la catena di nuovo, questa volta specificare `Course.Department` anziché `Course.Enrollments`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

Il codice seguente viene eseguito quando è stato selezionato un docente. Istruttore selezionato viene recuperato dall'elenco di istruttori nel modello di visualizzazione. Il modello di visualizzazione `Courses` proprietà viene quindi caricata con le entità Course da tale istruttore `CourseAssignments` proprietà di navigazione.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

Il `Where` metodo restituisce una raccolta, ma in questo caso i criteri di passati a tale metodo vengono restituiti solo una singola entità Instructor restituita. Il `Single` metodo converte la raccolta in una singola entità Instructor, che consente di accedere a tale entità `CourseAssignments` proprietà. Il `CourseAssignments` contiene proprietà `CourseAssignment` entità, da cui si desidera solo correlata `Course` entità.

Utilizzare il `Single` metodo in una raccolta quando si conosce la raccolta ha un solo elemento. L'unico metodo genera un'eccezione se la raccolta passata a esso è vuota o se è presente più di un elemento. In alternativa, è `SingleOrDefault`, che restituisce un valore predefinito (null in questo caso) se la raccolta è vuota. Tuttavia, in questo caso che ancora comporta un'eccezione (di tentare di trovare un `Courses` proprietà su un riferimento null), e il messaggio di eccezione meno chiaramente indica la causa del problema. Quando si chiama il `Single` (metodo), è anche possibile passare in Where condizione anziché chiamare il `Where` metodo separatamente:

```csharp
.Single(i => i.ID == id.Value)
```

Invece di:

```csharp
.Where(I => i.ID == id.Value).Single()
```

Successivamente, se è stato selezionato un corso, viene recuperato il corso selezionato dall'elenco dei corsi nel modello di visualizzazione. Del quindi il modello di visualizzazione `Enrollments` proprietà viene caricata con le entità di registrazione da quel corso `Enrollments` proprietà di navigazione.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a>Modificare la visualizzazione dell'indice Instructor

In *Views/Instructors/Index.cshtml*, sostituire il codice del modello con il codice seguente. Le modifiche sono evidenziate.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

Le seguenti modifiche apportate al codice esistente:

* Modificare la classe di modello per `InstructorIndexData`.

* Modificare il titolo della pagina da **indice** a **i docenti**.

* Aggiungere un **Office** colonna che visualizza `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non è null. (Poiché si tratta di una relazione uno-a-zero-o-uno, potrebbe essere presente un'entità correlata di OfficeAssignment.)

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Aggiungere un **corsi** invece di colonna che visualizza i corsi per ciascun insegnante. Vedere [esplicita riga transizione con `@:` ](xref:mvc/views/razor#explicit-line-transition-with-label) per altre informazioni su questa sintassi razor.

* Aggiunta di codice che aggiunge dinamicamente `class="success"` per il `tr` elemento istruttore selezionato. Consente di impostare un colore di sfondo per la riga selezionata utilizzando una classe di Bootstrap.

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Aggiungere un nuovo collegamento ipertestuale con l'etichetta **selezionare** immediatamente prima di altri collegamenti in ogni riga, che comporta l'ID dell'istruttore selezionato da inviare al `Index` metodo.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Eseguire l'app e selezionare il **i docenti** scheda. La pagina Visualizza la proprietà Location di entità OfficeAssignment correlate e una cella vuota della tabella quando è presente alcuna entità OfficeAssignment correlati.

![Pagina di indice istruttori che non selezionato](read-related-data/_static/instructors-index-no-selection.png)

Nel *Views/Instructors/Index.cshtml* file, dopo la chiusura tabella elemento (alla fine del file), aggiungere il codice seguente. Questo codice visualizza un elenco dei corsi relativi a un docente quando è selezionato un docente.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

Questo codice legge il `Courses` proprietà del modello di visualizzazione per visualizzare un elenco dei corsi. Fornisce inoltre un **selezionare** collegamento ipertestuale che invia l'ID del corso selezionato per il `Index` metodo di azione.

Aggiornare la pagina e selezionare un docente. È ora possibile visualizzare una griglia che visualizza i corsi assegnati a istruttore selezionato e per ogni corso vedrai il nome del reparto assegnato.

![Istruttore pagina di indice istruttori selezionato](read-related-data/_static/instructors-index-instructor-selected.png)

Dopo il blocco di codice che appena aggiunto, aggiungere il codice seguente. Consente di visualizzare un elenco degli studenti iscritti un corso quando quel corso è selezionata.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

Questo codice legge la proprietà le registrazioni del modello di visualizzazione per visualizzare un elenco di studenti iscritti in corso.

Aggiornare di nuovo la pagina e selezionare un docente. Selezionare quindi un corso per visualizzare l'elenco degli studenti iscritti e i relativi voti.

![Insegnante di pagina di indice istruttori e corso selezionato](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a>Caricamento esplicito

Quando è stato recuperato l'elenco di istruttori in *InstructorsController.cs*, specificato per il caricamento non differito il `CourseAssignments` proprietà di navigazione.

Si supponga che dovrebbero agli utenti di solo raramente si desidera visualizzare le registrazioni in corso e istruttore selezionato. In tal caso, si potrebbe voler caricare i dati di registrazione solo se richiesto. Per visualizzare un esempio di come eseguire il caricamento esplicito, sostituire il `Index` (metodo) con il codice seguente, che rimuove il caricamento immediato per le registrazioni e carica in modo esplicito tale proprietà. Le modifiche al codice sono evidenziati.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

Elimina il nuovo codice di *ThenInclude* chiamate per i dati di registrazione dal codice che consente di recuperare entità instructor. Se vengono selezionati un docente e corso, il codice evidenziato Recupera entità di registrazione per il corso selezionato e le entità di studenti per ogni registrazione.

Eseguire che l'app, andare alla pagina di indice istruttori ora non si vedrà alcuna differenza di ciò che viene visualizzato nella pagina anche se hai modificato la modalità di recupero dei dati.

## <a name="summary"></a>Riepilogo

Ora utilizzati caricamento eager con una query e con più query per leggere i dati correlati in proprietà di navigazione. Nella prossima esercitazione verrà illustrato come aggiornare i dati correlati.

>[!div class="step-by-step"]
>[Precedente](complex-data-model.md)
>[Successivo](update-related-data.md)  
