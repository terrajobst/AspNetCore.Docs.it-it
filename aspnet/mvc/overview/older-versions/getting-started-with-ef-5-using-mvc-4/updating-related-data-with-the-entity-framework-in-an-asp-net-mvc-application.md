---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aggiornamento dei dati correlati con Entity Framework in un'applicazione ASP.NET MVC (6 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: dde9d5022823b252e59949144e3021a53c0bdd3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371671"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Aggiornamento dei dati correlati con Entity Framework in un'applicazione ASP.NET MVC (6 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni a partire dall'inizio oppure [scarica un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si verifica un problema è possibile risolvere, [Scarica il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. Confrontando il codice per il codice completo è generalmente possibile trovare la soluzione al problema. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente si visualizzati dati correlati. in questa esercitazione si aggiornerà i dati correlati. Per la maggior parte delle relazioni, questa operazione può essere eseguita tramite l'aggiornamento di campi di chiave esterna appropriati. Per le relazioni molti-a-molti, Entity Framework non espone la tabella di join direttamente, in modo esplicito è necessario aggiungere, rimuovere le entità da e verso le proprietà di navigazione appropriate.

Le figure seguenti illustrano le pagine che verranno usate.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizzare le pagine di creazione e di modifica per i corsi

Quando viene creata, una nuova entità corso deve essere in relazione con un dipartimento esistente. Per semplificare il raggiungimento di questo obiettivo, il codice con scaffolding include i metodi del controller e le visualizzazioni di creazione e modifica includono un elenco a discesa per la selezione del dipartimento. L'elenco a discesa imposta la `Course.DepartmentID` proprietà di chiave esterna e questo è tutto Entity Framework necessita per caricare il `Department` proprietà di navigazione con l'appropriato `Department` entità. Verrà usato il codice con scaffolding, che però verrà modificato leggermente per aggiungere la gestione degli errori e l'ordinamento dell'elenco a discesa.

Nelle *CourseController.cs*, eliminare i quattro `Edit` e `Create` metodi e sostituirle con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

Il `PopulateDepartmentsDropDownList` metodo ottiene un elenco di tutti i dipartimenti ordinato in base al nome, crea un' `SelectList` raccolta per un elenco a discesa e passa tale raccolta alla visualizzazione in un `ViewBag` proprietà. Il metodo accetta il parametro facoltativo `selectedDepartment`, che consente al codice chiamante di specificare l'elemento che deve essere selezionato quando viene eseguito il rendering dell'elenco a discesa. La visualizzazione passerà il nome `DepartmentID` per [il `DropDownList` helper](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), e l'helper quindi saprà di dover per esaminare la `ViewBag` dell'oggetto per un `SelectList` denominato `DepartmentID`.

Il `HttpGet` `Create` chiamate al metodo il `PopulateDepartmentsDropDownList` metodo senza impostare l'elemento selezionato, perché per un nuovo corso al reparto non è ancora stabilito:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il `HttpGet` `Edit` metodo imposta l'elemento selezionato, in base all'ID del dipartimento già assegnato al corso in fase di modifica:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Il `HttpPost` per entrambi i metodi `Create` e `Edit` includono anche codice che imposta l'elemento selezionato quando viene visualizzata di nuovo la pagina dopo un errore:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Questo codice garantisce che quando viene visualizzata di nuovo la pagina per visualizzare il messaggio di errore, dipartimento selezionato rimane selezionato.

Nelle *Views\Course\Create.cshtml*, aggiungere il codice evidenziato di seguito per creare un nuovo campo numero di corso prima di **titolo** campo. Come spiegato in un'esercitazione precedente, i campi di chiave primaria non sottoposto a scaffolding per impostazione predefinita, ma questa chiave primaria è significativa, in modo che si desidera che l'utente sia in grado di immettere il valore della chiave.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

Nelle *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, e *Views\Course\Details.cshtml*, aggiungere un campo del numero di corso prima di **titolo**  campo. Poiché è la chiave primaria, viene visualizzato, ma non può essere modificato.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Eseguire la **Create** pagina (visualizzare la pagina di indice del corso e quindi scegliere **Crea nuovo**) e immettere i dati per un nuovo corso:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Scegliere **Crea**. Verrà visualizzata la pagina di indice corsi con il nuovo corso aggiunto all'elenco. Il nome del dipartimento nell'elenco della pagina di indice deriva dalla proprietà di navigazione, che mostra che la relazione è stata stabilita correttamente.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Eseguire la **Edit** pagina (visualizzare la pagina di indice del corso e quindi scegliere **modificare** per un corso).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Modificare i dati nella pagina e fare clic su **Save** (Salva). Verrà visualizzata la pagina di indice Course con i dati del corso aggiornati.

## <a name="adding-an-edit-page-for-instructors"></a>Aggiunta di una pagina di modifica per gli insegnanti

Quando si modifica il record di un insegnante, è necessario essere in grado di aggiornare l'assegnazione dell'ufficio. Il `Instructor` entità ha una relazione uno-a-zero-o-uno con il `OfficeAssignment` entità, che significa che è necessario gestire le situazioni seguenti:

- Se l'utente cancella l'assegnazione dell'ufficio e questa originariamente rappresentava un valore, è necessario rimuovere ed eliminare il `OfficeAssignment` entità.
- Se l'utente immette un valore di assegnazione di ufficio e questa originariamente era vuota, è necessario creare un nuovo `OfficeAssignment` entità.
- Se l'utente modifica il valore di un'assegnazione di ufficio, è necessario modificare il valore in un oggetto esistente `OfficeAssignment` entità.

Aprire *InstructorController.cs* ed esaminare le `HttpGet` `Edit` metodo:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Qui il codice con scaffolding non è auspicabile. Fase di impostazione dei dati per un elenco di riepilogo a discesa, ma è necessario basarsi su una casella di testo. Sostituire questo metodo con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Questo codice rilascia il `ViewBag` istruzione e aggiunge il caricamento eager per la proprietà associata `OfficeAssignment` entità. Non è possibile eseguire il caricamento eager con il `Find` metodo, in modo che il `Where` e `Single` metodi vengono invece usati per selezionare il tipo instructor.

Sostituire il `HttpPost` `Edit` metodo con il codice seguente. gestione degli aggiornamenti di assegnazione di ufficio:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Il codice effettua queste operazioni:

- Ottiene l'entità `Instructor` corrente dal database tramite il caricamento eager per la proprietà di navigazione `OfficeAssignment`. Questo è lo stesso come quella usata nel `HttpGet` `Edit` (metodo).
- Aggiorna l'entità `Instructor` recuperata con valori dallo strumento di associazione di modelli. Il [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) overload utilizzato consente *whitelist* le proprietà da includere. Ciò impedisce l'overposting, come illustrato nella [nella seconda esercitazione](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Se la posizione dell'ufficio è vuota, imposta la `Instructor.OfficeAssignment` proprietà su null, in modo che la riga correlata nella `OfficeAssignment` tabella verrà eliminata.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Salva le modifiche nel database.

Nelle *Views\Instructor\Edit.cshtml*, dopo il `div` elementi per il **Data assunzione** campo, aggiungere un nuovo campo per la modifica la posizione dell'ufficio:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Esecuzione della pagina (selezionare il **Instructors** scheda e quindi fare clic su **modificare** per un insegnante). Modificare **Office Location** (Posizione ufficio) e fare clic su **Save** (Salva).

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Aggiunta assegnazioni di corso all'insegnante Modifica pagina

Gli insegnanti possono tenere un numero qualsiasi di corsi. A questo punto la pagina di modifica dell'insegnante verrà migliorata aggiungendo la possibilità di modificare le assegnazioni di corso tramite un gruppo di caselle di controllo, come illustrato nello screenshot seguente:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

La relazione tra il `Course` e `Instructor` entità è molti-a-molti, ovvero non si ha accesso diretto alla tabella di join. Al contrario, si verrà aggiungere e rimuovere entità da e verso il `Instructor.Courses` proprietà di navigazione.

L'interfaccia utente che consente di modificare i corsi assegnati a un insegnante è costituita da un gruppo di caselle di controllo. È visualizzata una casella di controllo per ogni corso nel database e le caselle corrispondenti ai corsi attualmente assegnati all'insegnante sono selezionate. L'utente può selezionare e deselezionare le caselle di controllo per modificare le assegnazioni dei corsi. Se il numero di corsi fosse molto superiore, è preferibile usare un altro metodo di presentazione dei dati nella visualizzazione, ma si potrebbe usare lo stesso metodo di modifica delle proprietà di navigazione per poter creare o eliminare relazioni.

Per fornire i dati alla visualizzazione dell'elenco di caselle di controllo, si userà una classe modello di visualizzazione. Creare *AssignedCourseData.cs* nel *ViewModel* cartella e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Nelle *InstructorController.cs*, sostituire il `HttpGet` `Edit` metodo con il codice seguente. Le modifiche sono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Il codice aggiunge il caricamento eager per la proprietà di navigazione `Courses` e chiama il nuovo metodo `PopulateAssignedCourseData` per fornire informazioni per la matrice di caselle di controllo tramite la classe modello di visualizzazione `AssignedCourseData`.

Il codice nel `PopulateAssignedCourseData` metodo legge tutti `Course` classe modello le entità per caricare un elenco di corsi tramite la vista. Per ogni corso, il codice verifica se è presente nella proprietà di navigazione `Courses` dell'insegnante. Per creare un ricerca efficiente quando si verifica se un corso viene assegnato all'insegnante, i corsi assegnati all'insegnante vengono inseriti in una [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) raccolta. Il `Assigned` è impostata su `true` per i corsi l'insegnante è assegnato. La visualizzazione usa questa proprietà per determinare quali caselle di controllo devono essere visualizzate come selezionate. Infine, tale elenco viene passato alla visualizzazione in un `ViewBag` proprietà.

Aggiungere quindi il codice che viene eseguito quando l'utente fa clic su **Save** (Salva). Sostituire il `HttpPost` `Edit` metodo con il codice seguente, che chiama un nuovo metodo che aggiorni le `Courses` proprietà di navigazione del `Instructor` entità. Le modifiche sono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Poiché la vista non dispone di una raccolta di `Course` entità, il gestore di associazione del modello non può aggiornare automaticamente il `Courses` proprietà di navigazione. Invece di usare lo strumento di associazione di modelli per aggiornare le proprietà di navigazione Courses, è possibile farlo nel nuovo `UpdateInstructorCourses` (metodo). È pertanto necessario escludere la proprietà `Courses` dall'associazione di modelli. Ciò non richiede modifiche al codice che chiama [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) perché si sta usando la *nell'elenco elementi consentiti* overload e `Courses` non è presente nell'elenco di inclusione.

Se nessun controllo caselle sono state selezionate, il codice nel `UpdateInstructorCourses` Inizializza il `Courses` proprietà di navigazione con una raccolta vuota:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Il codice quindi esegue il ciclo di tutti i corsi nel database e controlla ogni corso a fronte di quelli assegnati all'insegnante rispetto a quelli selezionati nella visualizzazione. Per facilitare l'esecuzione di ricerche efficienti, le ultime due raccolte sono archiviate all'interno di oggetti `HashSet`.

Se la casella di controllo di un corso è selezionata ma il corso non è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene aggiunto alla raccolta nella proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Se la casella di controllo di un corso non è selezionata ma il corso è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene rimosso dalla proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Nella *Views\Instructor\Edit.cshtml*, aggiungere un **corsi** campo con una matrice di caselle di controllo aggiungendo il codice seguente evidenziato codice immediatamente dopo la `div` elementi per il `OfficeAssignment` campo:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Questo codice crea una tabella HTML con tre colonne. In ogni colonna è presente una casella di controllo seguita da una didascalia costituita dal numero di corso e dal titolo. Tutte le caselle di controllo hanno lo stesso nome ("selectedCourses"), che informa lo strumento di associazione del modello in cui devono essere considerati come un gruppo. Il `value` attributo di ogni casella di controllo è impostato sul valore di `CourseID.` quando la pagina viene pubblicata, lo strumento di associazione di modelli passa una matrice al controller che è costituito il `CourseID` i valori per solo le caselle di controllo siano selezionate.

Quando le caselle di controllo sono inizialmente sottoposto a rendering, quelle corrispondenti ai corsi assegnati all'insegnante hanno `checked` attributi, che rendono selezionate (visualizzate come selezionate).

Dopo aver modificato le assegnazioni dei corsi, si sia in grado di verificare le modifiche quando il sito torna al `Index` pagina. Pertanto, è necessario aggiungere una colonna nella tabella in tale pagina. In questo caso non è necessario usare il `ViewBag` dell'oggetto, le informazioni da visualizzare è già nel `Courses` proprietà di navigazione del `Instructor` entità che si sta passando alla pagina del modello.

Nelle *Views\Instructor\Index.cshtml*, aggiungere un **corsi** intestazione subito dopo la **Office** intestazione, come illustrato nell'esempio seguente:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Aggiungere quindi una nuova cella di dettaglio subito dopo la cella di dettaglio di percorso di office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Eseguire la **indice degli insegnanti** pagina per visualizzare i corsi assegnati all'insegnante ogni:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Fare clic su **modifica** per un insegnante per visualizzare la pagina di modifica.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Modificare alcune assegnazioni di corsi e fare clic su **salvare**. Le modifiche effettuate si riflettono nella pagina di indice.

 Nota: L'approccio adottato per la modifica di dati del corso insegnante funziona bene quando è presente un numero limitato di corsi. Per raccolte molto più grandi, sarebbero necessari un'interfaccia utente diversa e un altro metodo di aggiornamento.  
 

## <a name="update-the-delete-method"></a>Aggiornare il metodo Delete

Modificare il codice nel metodo HttpPost Delete in modo che il record di assegnazione di office (se presente) viene eliminato quando viene eliminato l'insegnante:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Se si prova a eliminare un istruttore viene assegnato a un reparto come amministratore, si otterrà un errore di integrità referenziale. Visualizzare [la versione corrente di questa esercitazione](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) di codice aggiuntivo che rimuoverà automaticamente l'insegnante da qualsiasi reparto in cui l'insegnante è assegnato come amministratore.

## <a name="summary"></a>Riepilogo

A questo punto è stata completata questa introduzione all'uso con i dati correlati. Finora in queste esercitazioni sono state eseguite le operazioni di intervallo della funzionalità CRUD complete, ma ancora stato affrontato i problemi di concorrenza. L'esercitazione successiva verrà introdurre l'argomento di concorrenza, illustrano le opzioni per la gestione e aggiungere al codice CRUD che già scritto un tipo di entità per la gestione della concorrenza.

Sono disponibili collegamenti ad altre risorse di Entity Framework, in fondo [ultima esercitazione della serie](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Precedente](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
