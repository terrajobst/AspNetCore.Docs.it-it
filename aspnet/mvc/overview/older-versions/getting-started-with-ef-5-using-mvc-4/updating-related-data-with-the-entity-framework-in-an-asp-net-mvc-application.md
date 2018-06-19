---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aggiornamento dei dati correlati con Entity Framework in un'applicazione ASP.NET MVC (6 di 10) | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 227a7fed0ced884db591f0375454d6d0a62518f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875084"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Aggiornamento dei dati correlati con Entity Framework in un'applicazione ASP.NET MVC (6 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 utilizzando Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto di avvio per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si esegue un problema, è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. In genere, è possibile trovare la soluzione al problema di mediante un confronto tra il codice per il codice completato. Per alcuni errori comuni e su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente si visualizzati dati correlati. in questa esercitazione si aggiornerà i dati correlati. Per la maggior parte delle relazioni, questa operazione può essere eseguita aggiornando i campi di chiave esterni appropriati. Per le relazioni molti-a-molti, Entity Framework non espone la tabella di join direttamente, in modo da aggiungere e rimuovere le entità da e verso le proprietà di navigazione appropriato in modo esplicito.

Le figure seguenti illustrano le pagine che verranno usate.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizzare le pagine di creazione e di modifica per i corsi

Quando viene creata, una nuova entità corso deve essere in relazione con un dipartimento esistente. Per semplificare il raggiungimento di questo obiettivo, il codice con scaffolding include i metodi del controller e le visualizzazioni di creazione e modifica includono un elenco a discesa per la selezione del dipartimento. Imposta l'elenco a discesa il `Course.DepartmentID` le proprietà di chiave esterna, e che tutte le esigenze di Entity Framework per caricare il `Department` proprietà di navigazione con l'appropriato `Department` entità. Verrà usato il codice con scaffolding, che però verrà modificato leggermente per aggiungere la gestione degli errori e l'ordinamento dell'elenco a discesa.

In *CourseController.cs*, eliminare le quattro `Edit` e `Create` metodi e sostituirli con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

Il `PopulateDepartmentsDropDownList` metodo ottiene un elenco di tutti i reparti ordinati per nome, crea un `SelectList` raccolta per un elenco a discesa e passa la raccolta per la visualizzazione in un `ViewBag` proprietà. Il metodo accetta il parametro facoltativo `selectedDepartment`, che consente al codice chiamante di specificare l'elemento che deve essere selezionato quando viene eseguito il rendering dell'elenco a discesa. La vista verrà passato il nome `DepartmentID` per [il `DropDownList` helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), e il supporto in grado di conoscere la ricerca di `ViewBag` dell'oggetto per un `SelectList` denominato `DepartmentID`.

Il `HttpGet` `Create` chiamate al metodo di `PopulateDepartmentsDropDownList` metodo senza impostare l'elemento selezionato, perché per un nuovo corso il reparto non è ancora stabilito:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il `HttpGet` `Edit` metodo imposta l'elemento selezionato, in base all'ID del reparto che è già assegnato al corso modificato:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Il `HttpPost` per entrambi i metodi `Create` e `Edit` inoltre includere codice che imposta l'elemento selezionato quando vengono nuovamente la pagina dopo un errore:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Questo codice garantisce che quando la pagina viene visualizzata per mostrare il messaggio di errore, è stato selezionato il reparto rimane selezionato.

In *Views\Course\Create.cshtml*, aggiungere il codice evidenziato di seguito per creare un nuovo campo Numero corso prima di **titolo** campo. Come illustrato in un'esercitazione precedente, i campi chiave primaria non sono scaffolding per impostazione predefinita, ma la chiave primaria è significativa, pertanto si desidera che l'utente la possibilità di immettere il valore della chiave.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

In *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, e *Views\Course\Details.cshtml*, aggiungere un campo numerico corso prima di **titolo**  campo. Poiché è la chiave primaria, viene visualizzato, ma non può essere modificata.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Eseguire il **crea** pagina (visualizzare la pagina di indice corso, quindi fare clic su **Crea nuovo**) e immettere i dati per un nuovo corso:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Scegliere **Crea**. Verrà visualizzata la pagina di indice corso con la nuova linea aggiunto all'elenco. Il nome del dipartimento nell'elenco della pagina di indice deriva dalla proprietà di navigazione, che mostra che la relazione è stata stabilita correttamente.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Eseguire il **modifica** pagina (visualizzare la pagina di indice corso, quindi fare clic su **modifica** su un corso).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Modificare i dati nella pagina e fare clic su **Save** (Salva). Verrà visualizzata la pagina di indice corso con i dati di corso aggiornato.

## <a name="adding-an-edit-page-for-instructors"></a>Aggiunta di una pagina di modifica per i docenti

Quando si modifica il record di un insegnante, è necessario essere in grado di aggiornare l'assegnazione dell'ufficio. Il `Instructor` entità dispone di una relazione uno-a-zero-o-uno con il `OfficeAssignment` entità, ovvero è necessario gestire le situazioni seguenti:

- Se l'utente cancella l'assegnazione di office e che inizialmente aveva un valore, è necessario rimuovere ed eliminare il `OfficeAssignment` entità.
- Se l'utente immette un valore di assegnazione di office ed era originariamente vuoto, è necessario creare un nuovo `OfficeAssignment` entità.
- Se l'utente modifica il valore di un'assegnazione di office, è necessario modificare il valore in un oggetto esistente `OfficeAssignment` entità.

Aprire *InstructorController.cs* ed esaminare il `HttpGet` `Edit` metodo:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Il codice di scaffolding non è quello desiderato. Configurazione dei dati per un elenco a discesa, ma è necessario basarsi su una casella di testo. Sostituire questo metodo con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Questo codice elimina il `ViewBag` istruzione e aggiunge il caricamento immediato per la proprietà associata `OfficeAssignment` entità. Non è possibile eseguire il caricamento immediato con il `Find` metodo, pertanto la `Where` e `Single` vengono utilizzati invece metodi per selezionare un istruttore.

Sostituire il `HttpPost` `Edit` (metodo) con il codice seguente. gestione degli aggiornamenti di assegnazione di office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Il codice effettua queste operazioni:

- Ottiene l'entità `Instructor` corrente dal database tramite il caricamento eager per la proprietà di navigazione `OfficeAssignment`. Questo è lo stesso come il il `HttpGet` `Edit` metodo.
- Aggiorna l'entità `Instructor` recuperata con valori dallo strumento di associazione di modelli. Il [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) overload utilizzato consente di *whitelist* le proprietà che si desidera includere. In questo modo la registrazione, come illustrato in [l'esercitazione secondo](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Se il percorso di office è vuoto, imposta il `Instructor.OfficeAssignment` proprietà su null, in modo che la riga correlata nella `OfficeAssignment` tabella verrà eliminata.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Salva le modifiche nel database.

In *Views\Instructor\Edit.cshtml*, dopo il `div` elementi per il **Data assunzione** campo, aggiungere un nuovo campo per modificare il percorso di office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Eseguire la pagina (selezionare il **i docenti** scheda e quindi fare clic su **modifica** su un docente). Modificare **Office Location** (Posizione ufficio) e fare clic su **Save** (Salva).

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Pagina Modifica esercitazioni aggiunta all'istruttore

Gli insegnanti possono tenere un numero qualsiasi di corsi. A questo punto la pagina di modifica dell'insegnante verrà migliorata aggiungendo la possibilità di modificare le assegnazioni di corso tramite un gruppo di caselle di controllo, come illustrato nello screenshot seguente:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

La relazione tra il `Course` e `Instructor` entità è molti-a-molti, ovvero non è un accesso diretto alla tabella di join. Al contrario, si aggiungerà e rimuoverà entità da e verso il `Instructor.Courses` proprietà di navigazione.

L'interfaccia utente che consente di modificare i corsi assegnati a un insegnante è costituita da un gruppo di caselle di controllo. È visualizzata una casella di controllo per ogni corso nel database e le caselle corrispondenti ai corsi attualmente assegnati all'insegnante sono selezionate. L'utente può selezionare e deselezionare le caselle di controllo per modificare le assegnazioni dei corsi. Se il numero di corsi è molto superiore, probabilmente si desidera utilizzare un altro metodo di presentazione dei dati nella visualizzazione, ma utilizzare lo stesso metodo di modifica per creare o eliminare relazioni tra le proprietà di navigazione.

Per fornire i dati alla visualizzazione dell'elenco di caselle di controllo, si userà una classe modello di visualizzazione. Creare *AssignedCourseData.cs* nel *ViewModel* cartella e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

In *InstructorController.cs*, sostituire il `HttpGet` `Edit` (metodo) con il codice seguente. Le modifiche sono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Il codice aggiunge il caricamento eager per la proprietà di navigazione `Courses` e chiama il nuovo metodo `PopulateAssignedCourseData` per fornire informazioni per la matrice di caselle di controllo tramite la classe modello di visualizzazione `AssignedCourseData`.

Il codice di `PopulateAssignedCourseData` metodo legge tutti `Course` classe entità per caricare un elenco di corsi utilizzando la visualizzazione del modello. Per ogni corso, il codice verifica se è presente nella proprietà di navigazione `Courses` dell'insegnante. Per creare ricerca efficiente quando si verifica se un corso è assegnato all'istruttore, corsi assegnati all'istruttore vengono inseriti in un [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) insieme. Il `Assigned` è impostata su `true` per corsi istruttore è assegnato. La visualizzazione usa questa proprietà per determinare quali caselle di controllo devono essere visualizzate come selezionate. Infine, tale elenco viene passato alla visualizzazione in un `ViewBag` proprietà.

Aggiungere quindi il codice che viene eseguito quando l'utente fa clic su **Save** (Salva). Sostituire il `HttpPost` `Edit` metodo con il codice seguente, che chiama un metodo nuovo che aggiorna il `Courses` proprietà di navigazione del `Instructor` entità. Le modifiche sono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Poiché la vista non dispone di una raccolta di `Course` entità, il gestore di associazione del modello non può aggiornare automaticamente il `Courses` proprietà di navigazione. Anziché utilizzare lo strumento di associazione del modello per aggiornare la proprietà di navigazione corsi, l'utente eseguirà che nel nuovo `UpdateInstructorCourses` metodo. È pertanto necessario escludere la proprietà `Courses` dall'associazione di modelli. Questo non richiede modifiche al codice che chiama [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) poiché si utilizza il *whitelist* overload e `Courses` non è presente nell'elenco di inclusione.

Se nessun controllo caselle sono state selezionate, il codice in `UpdateInstructorCourses` Inizializza il `Courses` proprietà di navigazione con una raccolta vuota:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Il codice quindi esegue il ciclo di tutti i corsi nel database e controlla ogni corso a fronte di quelli assegnati all'insegnante rispetto a quelli selezionati nella visualizzazione. Per facilitare l'esecuzione di ricerche efficienti, le ultime due raccolte sono archiviate all'interno di oggetti `HashSet`.

Se la casella di controllo di un corso è selezionata ma il corso non è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene aggiunto alla raccolta nella proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Se la casella di controllo di un corso non è selezionata ma il corso è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene rimosso dalla proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

In *Views\Instructor\Edit.cshtml*, aggiungere un **corsi** campo con una matrice di caselle di controllo aggiungendo quanto segue evidenziato codice immediatamente dopo il `div` elementi per il `OfficeAssignment` campo:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Questo codice crea una tabella HTML con tre colonne. In ogni colonna è presente una casella di controllo seguita da una didascalia costituita dal numero di corso e dal titolo. Tutte le caselle di controllo con lo stesso nome ("selectedCourses"), che informa il gestore di associazione del modello in cui devono essere considerate come un gruppo. Il `value` attributo di ogni casella di controllo è impostato sul valore di `CourseID.` quando viene eseguito il postback della pagina, il gestore di associazione del modello passa una matrice per il controller che include il `CourseID` i valori per solo le caselle di controllo selezionate.

Quando le caselle di controllo sono inizialmente eseguito il rendering, quelli per i corsi assegnati all'istruttore hanno `checked` attributi, che seleziona (Display li selezionate).

Dopo la modifica delle esercitazioni, è opportuno essere in grado di verificare le modifiche quando viene restituito per il sito di `Index` pagina. Pertanto, è necessario aggiungere una colonna alla tabella in tale pagina. In questo caso non è necessario utilizzare il `ViewBag` dell'oggetto, in quanto le informazioni che si desidera visualizzare sono già nel `Courses` proprietà di navigazione del `Instructor` entità che si sta passando alla pagina come modello.

In *Views\Instructor\Index.cshtml*, aggiungere un **corsi** intestazione immediatamente dopo il **Office** intestazione, come illustrato nell'esempio seguente:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Aggiungere quindi una nuova cella dei dettagli subito dopo la cella di dettaglio di percorso di office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Eseguire il **istruttore indice** pagina per visualizzare i corsi assegnati a ciascun insegnante:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Fare clic su **modifica** su un docente per visualizzare la pagina di modifica.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Modificare alcune esercitazioni e fare clic su **salvare**. Le modifiche effettuate si riflettono nella pagina di indice.

 Nota: L'approccio adottato per modificare i dati del corso istruttore funziona anche quando è presente un numero limitato di corsi. Per raccolte molto più grandi, sarebbero necessari un'interfaccia utente diversa e un altro metodo di aggiornamento.  
 

## <a name="update-the-delete-method"></a>Aggiornare il metodo Delete

Modificare il codice nel metodo HttpPost Elimina in modo il record di assegnazione di office (se presente) viene eliminato quando viene eliminato l'istruttore:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Se si tenta di eliminare un docente assegnato a un reparto come amministratore, si otterrà un errore di integrità referenziale. Vedere [la versione corrente di questa esercitazione](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) per codice aggiuntivo che verrà automaticamente rimosse istruttore qualsiasi reparto in cui istruttore viene assegnato come amministratore.

## <a name="summary"></a>Riepilogo

In questa introduzione per lavorare con i dati correlati sono stati completati. Finora in queste esercitazioni aver eseguito le operazioni di intervallo completo di CRUD, ma non sono stati gestiti i problemi di concorrenza. Esercitazione successiva verrà aggiunto l'argomento di concorrenza, illustrano le opzioni per la gestione e aggiungere Gestione al codice CRUD che già creato per un tipo di entità della concorrenza.

Sono disponibili collegamenti ad altre risorse di Entity Framework, alla fine di [dell'ultima esercitazione di questa serie](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Precedente](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
