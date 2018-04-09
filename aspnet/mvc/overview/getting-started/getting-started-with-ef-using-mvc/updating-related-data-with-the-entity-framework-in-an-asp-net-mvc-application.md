---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aggiornamento dei dati correlati con Entity Framework in un'applicazione ASP.NET MVC | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Code First di Entity Framework 6 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: cf4a6183e068e8668eb706d9a9e311616649e863
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Aggiornamento dei dati correlati con Entity Framework in un'applicazione MVC ASP.NET
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Visual Studio 2013 e Code First di Entity Framework 6. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nell'esercitazione precedente si visualizzati dati correlati. in questa esercitazione si aggiornerà i dati correlati. Per la maggior parte delle relazioni, questa operazione può essere eseguita mediante l'aggiornamento di campi di chiave esterna o proprietà di navigazione. Per le relazioni molti-a-molti, Entity Framework non espone la tabella di join direttamente, in modo da aggiungerà e rimozione le entità da e verso le proprietà di navigazione appropriato.

Le figure seguenti illustrano alcune delle pagine che verranno usate.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Modifica istruttore corsi](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizzare le pagine di creazione e di modifica per i corsi

Quando viene creata, una nuova entità corso deve essere in relazione con un dipartimento esistente. Per semplificare il raggiungimento di questo obiettivo, il codice con scaffolding include i metodi del controller e le visualizzazioni di creazione e modifica includono un elenco a discesa per la selezione del dipartimento. Imposta l'elenco a discesa il `Course.DepartmentID` le proprietà di chiave esterna, e che tutte le esigenze di Entity Framework per caricare il `Department` proprietà di navigazione con l'appropriato `Department` entità. Verrà usato il codice con scaffolding, che però verrà modificato leggermente per aggiungere la gestione degli errori e l'ordinamento dell'elenco a discesa.

In *CourseController.cs*, eliminare le quattro `Create` e `Edit` metodi e sostituirli con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Aggiungere il seguente `using` istruzione all'inizio del file:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il `PopulateDepartmentsDropDownList` metodo ottiene un elenco di tutti i reparti ordinati per nome, crea un `SelectList` raccolta per un elenco a discesa e passa la raccolta per la visualizzazione in un `ViewBag` proprietà. Il metodo accetta il parametro facoltativo `selectedDepartment`, che consente al codice chiamante di specificare l'elemento che deve essere selezionato quando viene eseguito il rendering dell'elenco a discesa. La vista verrà passato il nome `DepartmentID` per il [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) helper e il supporto in grado di conoscere la ricerca di `ViewBag` dell'oggetto per un `SelectList` denominato `DepartmentID`.

Il `HttpGet` `Create` chiamate al metodo di `PopulateDepartmentsDropDownList` metodo senza impostare l'elemento selezionato, perché per un nuovo corso il reparto non è ancora stabilito:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Il `HttpGet` `Edit` metodo imposta l'elemento selezionato, in base all'ID del reparto che è già assegnato al corso modificato:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

Il `HttpPost` per entrambi i metodi `Create` e `Edit` inoltre includere codice che imposta l'elemento selezionato quando vengono nuovamente la pagina dopo un errore:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Questo codice garantisce che quando la pagina viene visualizzata per mostrare il messaggio di errore, è stato selezionato il reparto rimane selezionato.

Le viste del corso sono già scaffolding con elenchi a discesa per il campo di reparto, ma non si desidera la didascalia DepartmentID per questo campo, in modo da verificare evidenziato di seguito l'indicazione di *Views\Course\Create.cshtml* file modificare la didascalia.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Apportare la stessa modifica in *Views\Course\Edit.cshtml*.

In genere il scaffolder non lo scaffolding di una chiave primaria perché il valore della chiave è generato dal database e non può essere modificato e non è un valore significativo da visualizzare agli utenti. Per le entità Course il scaffolder include una casella di testo per il `CourseID` campo perché riconosce che il `DatabaseGeneratedOption.None` attributo indica che l'utente deve essere in grado di immettere il valore della chiave primario. Ma che non riconosce che si desidera visualizzare nelle altre visualizzazioni, pertanto è necessario aggiungerla manualmente poiché il numero è significativo.

In *Views\Course\Edit.cshtml*, aggiungere un campo numerico corso prima di **titolo** campo. Poiché è la chiave primaria, viene visualizzato, ma non può essere modificata.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

È già presente un campo nascosto (`Html.HiddenFor` supporto) per il numero di corso nella visualizzazione di modifica. Aggiunta di un *Html.LabelFor* helper non eliminano la necessità per il campo nascosto, poiché non causare il numero del corso da includere nei dati inviati quando l'utente fa clic **salvare** nella pagina di modifica.

In *Views\Course\Delete.cshtml* e *Views\Course\Details.cshtml*, modificare la didascalia di nome di reparto da "Name" da "Department" e aggiungere un campo numerico corso prima di **titolo**  campo.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Eseguire il **crea** pagina (visualizzare la pagina di indice corso, quindi fare clic su **Crea nuovo**) e immettere i dati per un nuovo corso:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Scegliere **Crea**. Verrà visualizzata la pagina di indice corso con la nuova linea aggiunto all'elenco. Il nome del dipartimento nell'elenco della pagina di indice deriva dalla proprietà di navigazione, che mostra che la relazione è stata stabilita correttamente.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Eseguire il **modifica** pagina (visualizzare la pagina di indice corso, quindi fare clic su **modifica** su un corso).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Modificare i dati nella pagina e fare clic su **Save** (Salva). Verrà visualizzata la pagina di indice corso con i dati di corso aggiornato.

## <a name="adding-an-edit-page-for-instructors"></a>Aggiunta di una pagina di modifica per i docenti

Quando si modifica il record di un insegnante, è necessario essere in grado di aggiornare l'assegnazione dell'ufficio. Il `Instructor` entità dispone di una relazione uno-a-zero-o-uno con il `OfficeAssignment` entità, ovvero è necessario gestire le situazioni seguenti:

- Se l'utente cancella l'assegnazione di office e che inizialmente aveva un valore, è necessario rimuovere ed eliminare il `OfficeAssignment` entità.
- Se l'utente immette un valore di assegnazione di office ed era originariamente vuoto, è necessario creare un nuovo `OfficeAssignment` entità.
- Se l'utente modifica il valore di un'assegnazione di office, è necessario modificare il valore in un oggetto esistente `OfficeAssignment` entità.

Aprire *InstructorController.cs* ed esaminare il `HttpGet` `Edit` metodo:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Il codice di scaffolding non è quello desiderato. Configurazione dei dati per un elenco a discesa, ma è necessario basarsi su una casella di testo. Sostituire questo metodo con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Questo codice elimina il `ViewBag` istruzione e aggiunge il caricamento immediato per la proprietà associata `OfficeAssignment` entità. Non è possibile eseguire il caricamento immediato con il `Find` metodo, pertanto la `Where` e `Single` vengono utilizzati invece metodi per selezionare un istruttore.

Sostituire il `HttpPost` `Edit` (metodo) con il codice seguente. gestione degli aggiornamenti di assegnazione di office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Il riferimento a `RetryLimitExceededException` richiede un `using` istruzione; per aggiungere il pulsante destro del mouse `RetryLimitExceededException`, quindi fare clic su **risolvere** - **utilizzando System.Data.Entity.Infrastructure**.

![Risolvere eccezioni tentativi](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Il codice effettua queste operazioni:

- Modifica il nome del metodo per `EditPost` perché la firma è ora come il `HttpGet` metodo (il `ActionName` attributo specifica che l'URL di /Edit/ viene ancora utilizzato).
- Ottiene l'entità `Instructor` corrente dal database tramite il caricamento eager per la proprietà di navigazione `OfficeAssignment`. Questo è lo stesso come il il `HttpGet` `Edit` metodo.
- Aggiorna l'entità `Instructor` recuperata con valori dallo strumento di associazione di modelli. Il [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) overload utilizzato consente di *whitelist* le proprietà che si desidera includere. In questo modo la registrazione, come illustrato in [l'esercitazione secondo](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Se il percorso di office è vuoto, imposta il `Instructor.OfficeAssignment` proprietà su null, in modo che la riga correlata nella `OfficeAssignment` tabella verrà eliminata.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Salva le modifiche nel database.

In *Views\Instructor\Edit.cshtml*, dopo il `div` elementi per il **Data assunzione** campo, aggiungere un nuovo campo per modificare il percorso di office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Eseguire la pagina (selezionare il **i docenti** scheda e quindi fare clic su **modifica** su un docente). Modificare **Office Location** (Posizione ufficio) e fare clic su **Save** (Salva).

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Pagina Modifica esercitazioni aggiunta all'istruttore

Gli insegnanti possono tenere un numero qualsiasi di corsi. A questo punto la pagina di modifica dell'insegnante verrà migliorata aggiungendo la possibilità di modificare le assegnazioni di corso tramite un gruppo di caselle di controllo, come illustrato nello screenshot seguente:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

La relazione tra il `Course` e `Instructor` entità è molti-a-molti, ovvero non è un accesso diretto alle proprietà di chiave esterna che si trovano in una tabella di join. Invece aggiungere e rimuovere le entità da e verso il `Instructor.Courses` proprietà di navigazione.

L'interfaccia utente che consente di modificare i corsi assegnati a un insegnante è costituita da un gruppo di caselle di controllo. È visualizzata una casella di controllo per ogni corso nel database e le caselle corrispondenti ai corsi attualmente assegnati all'insegnante sono selezionate. L'utente può selezionare e deselezionare le caselle di controllo per modificare le assegnazioni dei corsi. Se il numero di corsi è molto superiore, probabilmente si desidera utilizzare un altro metodo di presentazione dei dati nella visualizzazione, ma utilizzare lo stesso metodo di modifica per creare o eliminare relazioni tra le proprietà di navigazione.

Per fornire i dati alla visualizzazione dell'elenco di caselle di controllo, si userà una classe modello di visualizzazione. Creare *AssignedCourseData.cs* nel *ViewModel* cartella e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

In *InstructorController.cs*, sostituire il `HttpGet` `Edit` (metodo) con il codice seguente. Le modifiche sono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Il codice aggiunge il caricamento eager per la proprietà di navigazione `Courses` e chiama il nuovo metodo `PopulateAssignedCourseData` per fornire informazioni per la matrice di caselle di controllo tramite la classe modello di visualizzazione `AssignedCourseData`.

Il codice di `PopulateAssignedCourseData` metodo legge tutti `Course` classe entità per caricare un elenco di corsi utilizzando la visualizzazione del modello. Per ogni corso, il codice verifica se è presente nella proprietà di navigazione `Courses` dell'insegnante. Per creare ricerca efficiente quando si verifica se un corso è assegnato all'istruttore, corsi assegnati all'istruttore vengono inseriti in un [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) insieme. Il `Assigned` è impostata su `true` per corsi istruttore è assegnato. La visualizzazione usa questa proprietà per determinare quali caselle di controllo devono essere visualizzate come selezionate. Infine, tale elenco viene passato alla visualizzazione in un `ViewBag` proprietà.

Aggiungere quindi il codice che viene eseguito quando l'utente fa clic su **Save** (Salva). Sostituire il `EditPost` metodo con il codice seguente, che chiama un metodo nuovo che aggiorna il `Courses` proprietà di navigazione del `Instructor` entità. Le modifiche sono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

La firma del metodo è diverso da ora il `HttpGet` `Edit` metodo, pertanto, il nome del metodo di modifica da `EditPost` al `Edit`.

Poiché la vista non dispone di una raccolta di `Course` entità, il gestore di associazione del modello non può aggiornare automaticamente il `Courses` proprietà di navigazione. Anziché utilizzare lo strumento di associazione del modello per aggiornare il `Courses` proprietà di navigazione, che verranno eseguite nella nuova `UpdateInstructorCourses` metodo. È pertanto necessario escludere la proprietà `Courses` dall'associazione di modelli. Questo non richiede modifiche al codice che chiama [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) poiché si utilizza il *whitelist* overload e `Courses` non è presente nell'elenco di inclusione.

Se nessun controllo caselle sono state selezionate, il codice in `UpdateInstructorCourses` Inizializza il `Courses` proprietà di navigazione con una raccolta vuota:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Il codice quindi esegue il ciclo di tutti i corsi nel database e controlla ogni corso a fronte di quelli assegnati all'insegnante rispetto a quelli selezionati nella visualizzazione. Per facilitare l'esecuzione di ricerche efficienti, le ultime due raccolte sono archiviate all'interno di oggetti `HashSet`.

Se la casella di controllo di un corso è selezionata ma il corso non è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene aggiunto alla raccolta nella proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Se la casella di controllo di un corso non è selezionata ma il corso è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene rimosso dalla proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

In *Views\Instructor\Edit.cshtml*, aggiungere un **corsi** campo con una matrice di caselle di controllo aggiungendo il seguente codice immediatamente dopo il `div` elementi per il `OfficeAssignment` campo e prima di `div` elemento per il **salvare** pulsante:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Dopo avere incollato il codice, se le interruzioni di riga e il rientro non aspetto come avviene in questo caso, correggere manualmente tutti gli elementi in modo che risulti simile quanto visualizzato qui. Il rientro non deve necessariamente essere perfetto, ma le righe `@</tr><tr>`, `@:<td>`, `@:</td>` e `@</tr>` devono trovarsi in una sola riga, come illustrato. In caso contrario, viene visualizzato un errore di runtime.

Questo codice crea una tabella HTML con tre colonne. In ogni colonna è presente una casella di controllo seguita da una didascalia costituita dal numero di corso e dal titolo. Tutte le caselle di controllo con lo stesso nome ("selectedCourses"), che informa il gestore di associazione del modello in cui devono essere considerate come un gruppo. Il `value` attributo di ogni casella di controllo è impostato sul valore di `CourseID.` quando viene eseguito il postback della pagina, il gestore di associazione del modello passa una matrice per il controller che include il `CourseID` i valori per solo le caselle di controllo selezionate.

Quando le caselle di controllo sono inizialmente eseguito il rendering, quelli per i corsi assegnati all'istruttore hanno `checked` attributi, che seleziona (Display li selezionate).

Dopo la modifica delle esercitazioni, è opportuno essere in grado di verificare le modifiche quando viene restituito per il sito di `Index` pagina. Pertanto, è necessario aggiungere una colonna alla tabella in tale pagina. In questo caso non è necessario utilizzare il `ViewBag` dell'oggetto, in quanto le informazioni che si desidera visualizzare sono già nel `Courses` proprietà di navigazione del `Instructor` entità che si sta passando alla pagina come modello.

In *Views\Instructor\Index.cshtml*, aggiungere un **corsi** intestazione immediatamente dopo il **Office** intestazione, come illustrato nell'esempio seguente:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Aggiungere quindi una nuova cella dei dettagli subito dopo la cella di dettaglio di percorso di office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Eseguire il **istruttore indice** pagina per visualizzare i corsi assegnati a ciascun insegnante:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Fare clic su **modifica** su un docente per visualizzare la pagina di modifica.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Modificare alcune esercitazioni e fare clic su **salvare**. Le modifiche effettuate si riflettono nella pagina di indice.

 Nota: L'approccio adottato qui per modificare i dati del corso istruttore funziona anche quando è presente un numero limitato di corsi. Per raccolte molto più grandi, sarebbero necessari un'interfaccia utente diversa e un altro metodo di aggiornamento.  
 

## <a name="update-the-deleteconfirmed-method"></a>Aggiornare il metodo DeleteConfirmed

In *InstructorController.cs*, eliminare il `DeleteConfirmed` (metodo) e l'inserimento di codice seguente al suo posto.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Questo codice rende la modifica seguente:

- Se istruttore è assegnato come amministratore del reparto, rimuove l'assegnazione istruttore tale reparto. Senza questo codice, si otterrebbe un errore di integrità referenziale si è tentato di eliminare un docente assegnato come amministratore per un reparto.

Questo codice non gestisce lo scenario di un insegnante assegnato come amministratore di più reparti. Nell'ultima esercitazione si aggiungerà codice che impedisce di tale scenario accada.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Aggiungere posizione dell'ufficio e corsi alla pagina Create (Crea)

In *InstructorController.cs*, eliminare il `HttpGet` e `HttpPost` `Create` metodi e quindi aggiungere il codice seguente al loro posto:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Questo codice è simile a quello visualizzato in per i metodi di modifica con la differenza che inizialmente non sono selezionati corsi. Il `HttpGet` `Create` chiamate al metodo di `PopulateAssignedCourseData` metodo non perché potrebbero essere presenti corsi selezionati ma in ordine per fornire una raccolta vuota per il `foreach` ciclo nella vista (in caso contrario Visualizza codice genera un'eccezione di riferimento null ).

Il metodo Create HttpPost aggiunge ogni corso selezionato per la proprietà di navigazione corsi prima del codice di modello che verifica la presenza di errori di convalida e aggiunge il nuovo docente al database. Corsi vengono aggiunti anche se sono presenti errori del modello in modo che quando sono presenti errori di modello (per un esempio, l'utente con chiave in una data non valida) in modo che quando la pagina viene visualizzata con un messaggio di errore, le selezioni corso apportate vengono automaticamente ripristinate.

Si noti che, perché sia possibile aggiungere corsi alla proprietà di navigazione `Courses`, è necessario inizializzare la proprietà come raccolta vuota:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Anziché all'interno di codice di controllo, è possibile eseguire questa operazione nel modello Instructor modificando il getter della proprietà in modo da creare automaticamente la raccolta, se questa non esiste, come illustrato nell'esempio seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Se si modifica la proprietà `Courses` in questo modo, è possibile rimuovere il codice di inizializzazione esplicita della proprietà nel controller.

In *Views\Instructor\Create.cshtml*, aggiungere una casella di testo percorso office e del corso di caselle di controllo dopo l'assunzione data campo e prima di **Invia** pulsante.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Dopo avere incollato il codice, correggere le interruzioni di riga e il rientro come fatto in precedenza per la pagina di modifica.

Eseguire la pagina di creazione e aggiungere un docente.

![Istruttore creare corsi](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>Gestione delle transazioni

Come spiegato nel [esercitazione funzionalità CRUD di base](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), per impostazione predefinita di Entity Framework implementa in modo implicito le transazioni. Per scenari in cui è necessario più controllare, ad esempio, se si desidera includere operazioni eseguite all'esterno di Entity Framework in una transazione, vedere [si utilizzano le transazioni](https://msdn.microsoft.com/data/dn456843) su MSDN.

## <a name="summary"></a>Riepilogo

In questa introduzione per lavorare con i dati correlati sono stati completati. In queste esercitazioni finora è utilizzato con codice che esegue i/o sincrono. È possibile rendere l'applicazione utilizzare risorse del server web in modo più efficiente mediante l'implementazione di codice asincrono e che è il nella prossima esercitazione verrà eseguita.

Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione ed è stato possibile apportare miglioramenti. È inoltre possibile richiedere nuovi argomenti in [Mostra Me come con codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Sono disponibili collegamenti ad altre risorse di Entity Framework in [accesso ai dati ASP.NET - risorse](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
