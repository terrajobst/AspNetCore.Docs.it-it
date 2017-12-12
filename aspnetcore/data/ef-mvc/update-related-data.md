---
title: Dati - 7 di 10 correlati alla base di ASP.NET MVC con Entity Framework Core - aggiornamento
author: tdykstra
description: "In questa esercitazione si aggiornerà i dati correlati mediante l'aggiornamento di campi di chiave esterni e le proprietà di navigazione."
keywords: ASP.NET Core, Entity Framework Core, i dati correlati, join
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 67bd162b-bfb7-4750-9e7f-705228b5288c
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: b59782bccce00f3940da4ec8bcff768aff8fa4ef
ms.sourcegitcommit: ccf08615ad59bc6f654560de33b93396113a2eb0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/11/2017
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a>L'aggiornamento dei dati correlati - EF Core con l'esercitazione di base di ASP.NET MVC (7 di 10)

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).

Nell'esercitazione precedente si visualizzati dati correlati. in questa esercitazione si aggiornerà i dati correlati mediante l'aggiornamento di campi di chiave esterni e le proprietà di navigazione.

Le illustrazioni seguenti mostrano alcune delle pagine che verranno utilizzate.

![Pagina Modifica corso](update-related-data/_static/course-edit.png)

![Pagina Modifica Instructor](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizzare la creazione e le pagine di modifica per i corsi

Quando viene creata una nuova entità course, deve avere una relazione a una categoria esistente. A tale scopo, il codice di scaffolding include i metodi del controller e creare e modificare le viste che includono un elenco a discesa per la selezione del reparto. Imposta l'elenco a discesa il `Course.DepartmentID` le proprietà di chiave esterna, e che tutte le esigenze di Entity Framework per caricare il `Department` proprietà di navigazione con l'entità Department appropriato. Verranno utilizzare il codice di supporto temporaneo, ma modificare leggermente per aggiungere la gestione degli errori e ordinare l'elenco a discesa.

In *CoursesController.cs*, eliminare i quattro metodi di creazione e modifica e sostituirli con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

Dopo il `Edit` metodo HttpPost, creare un nuovo metodo che consente di caricare le informazioni di reparto per l'elenco a discesa.

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

Il `PopulateDepartmentsDropDownList` metodo ottiene un elenco di tutti i reparti ordinati per nome, crea un `SelectList` raccolta per un elenco a discesa e passa l'insieme alla vista `ViewBag`. Il metodo accetta il parametro facoltativo `selectedDepartment` parametro che consente al codice chiamante specificare l'elemento selezionato quando l'elenco a discesa viene eseguito il rendering. La vista verrà passare il nome "DepartmentID" al `<select>` helper di tag e il supporto in grado di conoscere la ricerca di `ViewBag` dell'oggetto per un `SelectList` denominato "DepartmentID".

Il HttpGet `Create` chiamate al metodo di `PopulateDepartmentsDropDownList` metodo senza impostare l'elemento selezionato, perché per un nuovo corso il reparto non è ancora stabilito:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

Il HttpGet `Edit` metodo imposta l'elemento selezionato, in base all'ID del reparto che è già assegnato al corso modificato:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

Per entrambi i metodi di HttpPost `Create` e `Edit` inoltre includere codice che imposta l'elemento selezionato quando vengono nuovamente la pagina dopo un errore. Ciò garantisce che quando la pagina viene visualizzata per mostrare il messaggio di errore, è stato selezionato il reparto rimane selezionato.

### <a name="add-asnotracking-to-details-and-delete-methods"></a>Aggiungere. AsNoTracking dettagli ed eliminare i metodi

Per ottimizzare le prestazioni del corso dettagli e pagine di eliminazione, aggiungere `AsNoTracking` chiama il `Details` e HttpGet `Delete` metodi.

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>Modificare le viste del corso

In *Views/Courses/Create.cshtml*, aggiungere un'opzione "Selezionare reparto" per il **reparto** elenco a discesa elenco, modificare la didascalia da **DepartmentID** a  **Reparto**e aggiungere un messaggio di convalida.

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

In *Views/Courses/Edit.cshtml*, apportare la stessa modifica per il campo reparto consente di verificare in *Create.cshtml*.

Anche in *Views/Courses/Edit.cshtml*, aggiungere un campo numerico corso prima di **titolo** campo. Poiché il numero di corso è la chiave primaria, viene visualizzato, ma non può essere modificata.

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

È già presente un campo nascosto (`<input type="hidden">`) per il numero di corso nella visualizzazione di modifica. Aggiunta di un `<label>` helper di tag non di eliminare la necessità per il campo nascosto poiché non causare il numero del corso da includere nei dati inviati quando l'utente fa clic **salvare** sul **modifica** pagina.

In *Views/Courses/Delete.cshtml*, aggiungere un campo del numero di corso nella parte superiore e modificare l'ID del reparto al nome del reparto.

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

In *Views/Courses/Details.cshtml*, apportare la stessa modifica effettuati per *Delete.cshtml*.

### <a name="test-the-course-pages"></a>Le pagine del corso di test

Eseguire l'app, selezionare il **corsi** scheda, fare clic su **Crea nuovo**e immettere i dati per un nuovo corso:

![Pagina Crea corso](update-related-data/_static/course-create.png)

Scegliere **Crea**. Verrà visualizzata la pagina di indice corsi con la nuova linea aggiunto all'elenco. Il nome di reparto nell'elenco di pagina di indice deriva dalla proprietà di navigazione, che mostra che la relazione è stata stabilita correttamente.

Fare clic su **modifica** su un corso nella pagina di indice corsi.

![Pagina Modifica corso](update-related-data/_static/course-edit.png)

Modificare i dati nella pagina e fare clic su **salvare**. Verrà visualizzata la pagina di indice corsi con i dati di corso aggiornato.

## <a name="add-an-edit-page-for-instructors"></a>Aggiungere una pagina di modifica per i docenti

Quando si modifica un record istruttore, si desidera essere in grado di aggiornare l'assegnazione dell'ufficio dell'insegnante. Entità Instructor ha una relazione uno-a-zero-o-uno con l'entità OfficeAssignment, ovvero che il codice deve gestire le situazioni seguenti:

* Se l'utente cancella l'assegnazione di office e che inizialmente aveva un valore, eliminare l'entità OfficeAssignment.

* Se l'utente immette un valore di assegnazione di office ed era originariamente vuoto, creare una nuova entità OfficeAssignment.

* Se l'utente modifica il valore di un'assegnazione di office, modificare il valore in un'entità OfficeAssignment esistente.

### <a name="update-the-instructors-controller"></a>Aggiornare il controller istruttori

In *InstructorsController.cs*, modificare il codice di HttpGet `Edit` metodo in modo che carica l'entità Instructor `OfficeAssignment` proprietà di navigazione e chiamate `AsNoTracking`:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

Sostituire il HttpPost `Edit` metodo con il codice seguente per gestire gli aggiornamenti di assegnazione di office:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

Il codice esegue le operazioni seguenti:

-  Modifica il nome del metodo per `EditPost` perché la firma è ora come il HttpGet `Edit` metodo (il `ActionName` attributo specifica che il `/Edit/` URL viene ancora utilizzato).

-  Ottiene l'entità Instructor corrente dal database utilizzando eager caricamento per il `OfficeAssignment` proprietà di navigazione. Ciò equivale il il HttpGet `Edit` metodo.

-  Aggiorna l'entità Instructor recuperata con valori dallo strumento di associazione del modello. Il `TryUpdateModel` overload consente all'elenco elementi consentiti le proprietà che si desidera includere. In questo modo la registrazione, come illustrato nel [esercitazione secondo](crud.md).

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   Se il percorso di office è vuoto, imposta la proprietà Instructor.OfficeAssignment su null in modo che la riga correlata nella tabella OfficeAssignment verrà eliminata.

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- Salva le modifiche al database.

### <a name="update-the-instructor-edit-view"></a>Aggiornare la visualizzazione di modifica Instructor

In *Views/Instructors/Edit.cshtml*, aggiungere un nuovo campo per modificare il percorso di office, al termine prima di **salvare** pulsante:

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

Eseguire l'app, selezionare il **i docenti** scheda e quindi fare clic su **modifica** su un docente. Modifica il **ufficio** e fare clic su **salvare**.

![Pagina Modifica Instructor](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Aggiungere esercitazioni per la pagina Modifica Instructor

I docenti potrebbero indicare un numero qualsiasi di corsi. A questo punto sarà possibile migliorare la pagina Modifica istruttore aggiungendo la possibilità di modificare le assegnazioni di corso usando un gruppo di caselle di controllo, come illustrato nella schermata seguente:

![Pagina Modifica istruttore corsi](update-related-data/_static/instructor-edit-courses.png)

La relazione tra le entità Course e istruttore è molti-a-molti. Per aggiungere e rimuovere le relazioni, aggiungere e rimuovere le entità da e verso il set di entità CourseAssignments join.

L'interfaccia utente che consente di modificare quali corsi un docente è assegnato a è un gruppo di caselle di controllo. Viene visualizzata una casella di controllo per ogni corso nel database e quelle che è attualmente assegnata istruttore selezionati. L'utente può selezionare o deselezionare le caselle di controllo per modificare le assegnazioni dei corsi. Se il numero di corsi è molto superiore, probabilmente si desidera utilizzare un altro metodo di presentazione dei dati nella visualizzazione, ma utilizzare lo stesso metodo di manipolazione di un'entità di join per creare o eliminare le relazioni.

### <a name="update-the-instructors-controller"></a>Aggiornare il controller istruttori

Per fornire dati per la visualizzazione per un elenco di caselle di controllo, si utilizzerà una classe di modello di visualizzazione.

Creare *AssignedCourseData.cs* nel *SchoolViewModels* cartella e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

In *InstructorsController.cs*, sostituire il HttpGet `Edit` metodo con il codice seguente. Le modifiche sono evidenziate.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

Il codice aggiunge il caricamento immediato per la `Courses` proprietà di navigazione e chiama il nuovo `PopulateAssignedCourseData` metodo per fornire informazioni per la matrice di casella di controllo utilizzando il `AssignedCourseData` visualizzare una classe di modello.

Il codice di `PopulateAssignedCourseData` metodo legge tutte le entità Course per caricare un elenco di corsi utilizzando la classe di modello di visualizzazione. Per ogni corso, il codice verifica se il corso è presente nell'istruttore `Courses` proprietà di navigazione. Per creare ricerca efficiente quando si verifica se un corso è assegnato all'istruttore, corsi assegnati all'istruttore vengono inseriti in un `HashSet` insieme. Il `Assigned` è impostata su true per i corsi istruttore viene assegnato a. La vista utilizzerà questa proprietà per determinare quale controllo caselle devono essere visualizzate come selezionato. Infine, tale elenco viene passato alla visualizzazione in `ViewData`.

Successivamente, aggiungere il codice che viene eseguito quando l'utente fa clic **salvare**. Sostituire il `EditPost` (metodo) con il seguente codice, quindi aggiungere un nuovo metodo che aggiorna il `Courses` proprietà di navigazione di entità Instructor.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

La firma del metodo è diversa dal HttpGet `Edit` metodo, pertanto, il nome del metodo di modifica da `EditPost` al `Edit`.

Poiché la vista non dispone di una raccolta di entità corso, lo strumento di associazione del modello non può aggiornare automaticamente il `CourseAssignments` proprietà di navigazione. Anziché utilizzare lo strumento di associazione del modello per aggiornare il `CourseAssignments` proprietà di navigazione è eseguire questa operazione nella nuova `UpdateInstructorCourses` metodo. È pertanto necessario escludere il `CourseAssignments` proprietà dall'associazione del modello. Questo non richiede modifiche al codice che chiama `TryUpdateModel` poiché si utilizza l'overload whitelist e `CourseAssignments` non è presente nell'elenco di inclusione.

Se nessun controllo caselle sono state selezionate, il codice in `UpdateInstructorCourses` Inizializza il `CourseAssignments` proprietà di navigazione e restituisce una raccolta vuota:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

Il codice quindi esegue il ciclo di tutti i corsi nel database e controlla ogni corso rispetto a quelli attualmente assegnati all'istruttore rispetto a quelli che sono stati selezionati nella visualizzazione. Per facilitare le ricerche efficienti, quest'ultime due raccolte vengono archiviate in `HashSet` oggetti.

Se è stata selezionata la casella di controllo per un corso ma non è corso la `Instructor.CourseAssignments` proprietà di navigazione, la linea viene aggiunto alla raccolta nella proprietà di navigazione.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

Se non è stata selezionata la casella di controllo per un corso, ma il corso è il `Instructor.CourseAssignments` proprietà di navigazione, la linea viene rimossa dalla proprietà di navigazione.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>Aggiornare le visualizzazioni Instructor

In *Views/Instructors/Edit.cshtml*, aggiungere un **corsi** campo con una matrice di caselle di controllo aggiungendo il seguente codice immediatamente dopo il `div` elementi per il **Office**  campo e prima di `div` elemento per il **salvare** pulsante.

<a id="notepad"></a>
> [!NOTE] 
> Quando si incolla il codice in Visual Studio, le interruzioni di riga verranno modificate in modo che interrompe il codice.  Premere Ctrl + Z di una volta per annullare la formattazione automatica.  Ciò consente di risolvere le interruzioni di riga in modo che vengano visualizzate come gli elementi visualizzati. Il rientro non deve necessariamente essere perfetto, ma la `@</tr><tr>`, `@:<td>`, `@:</td>`, e `@:</tr>` righe devono essere su una singola riga come illustrato o si otterrà un errore di runtime. Con il blocco di nuovo codice selezionato, premere Tab tre volte per allineare il nuovo codice con il codice esistente. È possibile controllare lo stato del problema [qui](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

Questo codice crea una tabella HTML che ha tre colonne. In ogni colonna è seguita da una didascalia che include il numero di corso e il titolo di una casella di controllo. Tutte le caselle di controllo con lo stesso nome ("selectedCourses"), che informa il gestore di associazione del modello in cui devono essere considerate come un gruppo. L'attributo value di ogni casella di controllo è impostata sul valore di `CourseID`. Quando viene eseguito il postback della pagina, il gestore di associazione del modello passa una matrice per il controller che include il `CourseID` i valori per solo le caselle di controllo selezionate.

Quando le caselle di controllo sono inizialmente eseguito il rendering, quelli per i corsi assegnati all'istruttore sono controllati gli attributi, che seleziona (Display li selezionate).

Eseguire l'app, selezionare il **i docenti** scheda e fare clic su **modifica** su un docente per visualizzare il **modifica** pagina.

![Pagina Modifica istruttore corsi](update-related-data/_static/instructor-edit-courses.png)

Modificare alcune esercitazioni e fare clic su Salva. Le modifiche apportate vengono riflesse nella pagina di indice.

> [!NOTE] 
> L'approccio adottato qui per modificare i dati del corso istruttore funziona anche quando è presente un numero limitato di corsi. Per le raccolte che sono più grandi, un'interfaccia utente differente e un altro metodo di aggiornamento sono necessarie.

## <a name="update-the-delete-page"></a>Aggiornare la pagina di eliminazione

In *InstructorsController.cs*, eliminare il `DeleteConfirmed` (metodo) e l'inserimento di codice seguente al suo posto.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

Questo codice rende le modifiche seguenti:

* Eager fa caricamento per il `CourseAssignments` proprietà di navigazione.  È necessario includere questa o EF non rileveranno correlati `CourseAssignment` entità e non di eliminarli.  Per evitare la necessità di leggerli qui è possibile configurare eliminazione a catena nel database.

* Se istruttore da eliminare viene assegnato come responsabile per i reparti, rimuove l'assegnazione istruttore i reparti.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Aggiungere la pagina Crea ufficio e corsi

In *InstructorsController.cs*, eliminare il HttpGet e HttpPost `Create` metodi e quindi aggiungere il codice seguente al loro posto:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

Questo codice è simile a quello visualizzato per il `Edit` metodi, ad eccezione che inizialmente non corsi selezionati. Il HttpGet `Create` chiamate al metodo di `PopulateAssignedCourseData` metodo non perché potrebbero essere presenti corsi selezionati ma in ordine per fornire una raccolta vuota per il `foreach` ciclo nella vista (in caso contrario Visualizza codice genera un'eccezione di riferimento null).

HttpPost `Create` metodo aggiunge ogni corso selezionato per il `CourseAssignments` proprietà di navigazione prima che verifica la presenza di errori di convalida e aggiunge il nuovo docente al database. Corsi vengono aggiunti anche se sono presenti errori del modello in modo che quando sono presenti errori del modello (per un esempio, l'utente con chiave in una data non valida) e la pagina viene visualizzata con un messaggio di errore, le selezioni corso apportate vengono automaticamente ripristinate.

Si noti che per poter essere in grado di aggiungere i corsi per il `CourseAssignments` proprietà di navigazione, è necessario inizializzare la proprietà su una raccolta vuota:

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

Come alternativa a questa operazione nel codice del controller, è possibile usare il modello istruttore modificando il metodo Get della proprietà per creare automaticamente la raccolta se non esiste, come illustrato nell'esempio seguente:

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

Se si modifica il `CourseAssignments` proprietà in questo modo, è possibile rimuovere il codice di inizializzazione di proprietà espliciti nel controller.

In *Views/Instructor/Create.cshtml*, aggiungere una casella di testo percorso office caselle di controllo e per i corsi prima pulsante Invia. Come nel caso di modifica pagina [correggere la formattazione se Visual Studio riformatta il codice quando lo si incolla](#notepad).

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

Verificare l'esecuzione dell'app e la creazione di un docente. 

## <a name="handling-transactions"></a>Gestione delle transazioni

Come spiegato nel [esercitazione CRUD](crud.md), Entity Framework implementa in modo implicito le transazioni. Per scenari in cui è necessario più controllare, ad esempio, se si desidera includere operazioni eseguite all'esterno di Entity Framework in una transazione, vedere [transazioni](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="summary"></a>Riepilogo

Introduzione all'utilizzo di dati correlati sono stati completati. Nella prossima esercitazione verrà visualizzato come gestire i conflitti di concorrenza.

>[!div class="step-by-step"]
[Precedente](read-related-data.md)
[Successivo](concurrency.md)  
