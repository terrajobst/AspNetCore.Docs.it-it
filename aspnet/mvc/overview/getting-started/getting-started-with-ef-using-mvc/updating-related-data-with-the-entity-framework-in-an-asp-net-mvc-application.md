---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: Aggiornare i dati correlati in un'app ASP.NET MVC con Entity Framework"
description: In questa esercitazione si aggiornerà i dati correlati. Per la maggior parte delle relazioni, questa operazione può essere eseguita tramite l'aggiornamento di campi di chiave esterna o le proprietà di navigazione.
author: tdykstra
ms.author: riande
ms.date: 01/17/2019
ms.topic: tutorial
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 3f95470fd1832d7d25a331a1b6a9dfede7356f38
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444311"
---
# <a name="tutorial-update-related-data-with-ef-in-an-aspnet-mvc-app"></a>Esercitazione: Aggiornare i dati correlati in un'app ASP.NET MVC con Entity Framework

Nell'esercitazione precedente si visualizzati dati correlati. In questa esercitazione si aggiornerà i dati correlati. Per la maggior parte delle relazioni, questa operazione può essere eseguita tramite l'aggiornamento di campi di chiave esterna o le proprietà di navigazione. Per le relazioni molti-a-molti, Entity Framework non espone la tabella di join direttamente, in modo da aggiungerà e rimuove le entità da e verso le proprietà di navigazione appropriate.

Le figure seguenti illustrano alcune delle pagine che verranno usate.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Modifica insegnante con corsi](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Personalizzare le pagine di corsi
> * Aggiungere office alla pagina instructors (insegnanti)
> * Aggiungere corsi alla pagina instructors (insegnanti)
> * Aggiornare DeleteConfirmed
> * Aggiungere posizione dell'ufficio e corsi alla pagina Create (Crea)

## <a name="prerequisites"></a>Prerequisiti

* [Lettura di dati correlati](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-courses-pages"></a>Personalizzare le pagine di corsi

Quando viene creata, una nuova entità corso deve essere in relazione con un dipartimento esistente. Per semplificare il raggiungimento di questo obiettivo, il codice con scaffolding include i metodi del controller e le visualizzazioni di creazione e modifica includono un elenco a discesa per la selezione del dipartimento. L'elenco a discesa imposta la `Course.DepartmentID` proprietà di chiave esterna e questo è tutto Entity Framework necessita per caricare il `Department` proprietà di navigazione con l'appropriato `Department` entità. Verrà usato il codice con scaffolding, che però verrà modificato leggermente per aggiungere la gestione degli errori e l'ordinamento dell'elenco a discesa.

Nelle *CourseController.cs*, eliminare i quattro `Create` e `Edit` metodi e sostituirle con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Aggiungere il codice seguente `using` istruzione all'inizio del file:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Il `PopulateDepartmentsDropDownList` metodo ottiene un elenco di tutti i dipartimenti ordinato in base al nome, crea un' `SelectList` raccolta per un elenco a discesa e passa tale raccolta alla visualizzazione in un `ViewBag` proprietà. Il metodo accetta il parametro facoltativo `selectedDepartment`, che consente al codice chiamante di specificare l'elemento che deve essere selezionato quando viene eseguito il rendering dell'elenco a discesa. La visualizzazione passerà il nome `DepartmentID` per il [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) helper e l'helper quindi saprà di dover per cercare `ViewBag` dell'oggetto per un `SelectList` denominato `DepartmentID`.

Il `HttpGet` `Create` chiamate al metodo il `PopulateDepartmentsDropDownList` metodo senza impostare l'elemento selezionato, perché per un nuovo corso al reparto non è ancora stabilito:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

Il `HttpGet` `Edit` metodo imposta l'elemento selezionato, in base all'ID del dipartimento già assegnato al corso in fase di modifica:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

Il `HttpPost` per entrambi i metodi `Create` e `Edit` includono anche codice che imposta l'elemento selezionato quando viene visualizzata di nuovo la pagina dopo un errore:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Questo codice garantisce che quando viene visualizzata di nuovo la pagina per visualizzare il messaggio di errore, dipartimento selezionato rimane selezionato.

Le visualizzazioni dei corsi sono già sottoposto a scaffolding con elenchi a discesa per il campo department, ma non si desidera la didascalia DepartmentID per questo campo, in modo da modificare marca evidenziato di seguito per il *Views\Course\Create.cshtml* file modificare la didascalia.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Apportare la stessa modifica in *Views\Course\Edit.cshtml*.

In genere l'utilità di scaffolding non eseguire lo scaffolding di una chiave primaria perché il valore della chiave viene generato dal database e non può essere modificato e non è un valore significativo da visualizzare agli utenti. Per le entità Course l'utilità di scaffolding include una casella di testo per il `CourseID` campo perché riconosce che il `DatabaseGeneratedOption.None` attributo indica che l'utente deve essere in grado di immettere il valore della chiave primaria. Ma che non riconosce che si desidera visualizzarlo nelle altre visualizzazioni, pertanto è necessario aggiungerla manualmente poiché il numero è significativo.

Nelle *Views\Course\Edit.cshtml*, aggiungere un campo del numero di corso prima di **titolo** campo. Poiché è la chiave primaria, viene visualizzato, ma non può essere modificato.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

È già presente un campo nascosto (`Html.HiddenFor` helper) per il numero di corso nella visualizzazione di modifica. Aggiunta di un *Html.LabelFor* helper non elimina la necessità del campo nascosto, poiché senza di questo numero del corso da includere nei dati inviati quando l'utente sceglie **salvare** nella pagina di modifica.

Nelle *Views\Course\Delete.cshtml* e *Views\Course\Details.cshtml*, modificare la didascalia di nome di reparto da "Name" a "Department" e aggiungere un campo del numero di corso prima di **titolo**  campo.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Eseguire la **Create** pagina (visualizzare la pagina di indice del corso e quindi scegliere **Crea nuovo**) e immettere i dati per un nuovo corso:

| Value | Impostazione |
| ----- | ------- |
| Number | Immettere *1000*. |
| Titolo | Immettere *Algebra*. |
| Crediti | Immettere *4*. |
|Department | Selezionare **matematica**. |

Scegliere **Crea**. Verrà visualizzata la pagina di indice corsi con il nuovo corso aggiunto all'elenco. Il nome del dipartimento nell'elenco della pagina di indice deriva dalla proprietà di navigazione, che mostra che la relazione è stata stabilita correttamente.

Eseguire la **Edit** pagina (visualizzare la pagina di indice del corso e quindi scegliere **modificare** per un corso).

Modificare i dati nella pagina e fare clic su **Save** (Salva). Verrà visualizzata la pagina di indice Course con i dati del corso aggiornati.

## <a name="add-office-to-instructors-page"></a>Aggiungere office alla pagina instructors (insegnanti)

Quando si modifica il record di un insegnante, è necessario essere in grado di aggiornare l'assegnazione dell'ufficio. Il `Instructor` entità ha una relazione uno-a-zero-o-uno con il `OfficeAssignment` entità, che significa che è necessario gestire le situazioni seguenti:

- Se l'utente cancella l'assegnazione dell'ufficio e questa originariamente rappresentava un valore, è necessario rimuovere ed eliminare il `OfficeAssignment` entità.
- Se l'utente immette un valore di assegnazione di ufficio e questa originariamente era vuota, è necessario creare un nuovo `OfficeAssignment` entità.
- Se l'utente modifica il valore di un'assegnazione di ufficio, è necessario modificare il valore in un oggetto esistente `OfficeAssignment` entità.

Aprire *InstructorController.cs* ed esaminare le `HttpGet` `Edit` metodo:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Qui il codice con scaffolding non è auspicabile. Fase di impostazione dei dati per un elenco di riepilogo a discesa, ma è necessario basarsi su una casella di testo. Sostituire questo metodo con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Questo codice rilascia il `ViewBag` istruzione e aggiunge il caricamento eager per la proprietà associata `OfficeAssignment` entità. Non è possibile eseguire il caricamento eager con il `Find` metodo, in modo che il `Where` e `Single` metodi vengono invece usati per selezionare il tipo instructor.

Sostituire il `HttpPost` `Edit` metodo con il codice seguente. gestione degli aggiornamenti di assegnazione di ufficio:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Il riferimento al `RetryLimitExceededException` richiede un `using` istruzione. Per aggiungerla, passare il mouse su `RetryLimitExceededException`. Viene visualizzata una spiegazione del problema. Selezionare **Mostra possibili correzioni** e quindi fare clic su **usando System.Data.Entity.Infrastructure;**.

![Risolvere eccezioni tentativi](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Il codice effettua queste operazioni:

- Modifica il nome di metodo in `EditPost` perché la firma corrisponde ora al `HttpGet` metodo (il `ActionName` attributo specifica che l'URL /Edit/ viene ancora usato).
- Ottiene l'entità `Instructor` corrente dal database tramite il caricamento eager per la proprietà di navigazione `OfficeAssignment`. Questo è lo stesso come quella usata nel `HttpGet` `Edit` (metodo).
- Aggiorna l'entità `Instructor` recuperata con valori dallo strumento di associazione di modelli. Il [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) overload utilizzato consente *whitelist* le proprietà da includere. Ciò impedisce l'overposting, come illustrato nella [nella seconda esercitazione](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Se la posizione dell'ufficio è vuota, imposta la `Instructor.OfficeAssignment` proprietà su null, in modo che la riga correlata nella `OfficeAssignment` tabella verrà eliminata.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Salva le modifiche nel database.

Nelle *Views\Instructor\Edit.cshtml*, dopo il `div` elementi per il **Data assunzione** campo, aggiungere un nuovo campo per la modifica la posizione dell'ufficio:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Esecuzione della pagina (selezionare il **Instructors** scheda e quindi fare clic su **modificare** per un insegnante). Modificare **Office Location** (Posizione ufficio) e fare clic su **Save** (Salva).

## <a name="add-courses-to-instructors-page"></a>Aggiungere corsi alla pagina instructors (insegnanti)

Gli insegnanti possono tenere un numero qualsiasi di corsi. A questo punto sarà possibile migliorare la pagina di modifica dell'insegnante aggiungendo la possibilità di modificare le assegnazioni di corso tramite un gruppo di caselle di controllo.

La relazione tra il `Course` e `Instructor` entità è molti-a-molti, ovvero non si ha accesso diretto alle proprietà di chiave esterna che si trovano in una tabella di join. Al contrario, si aggiungere e rimuovere le entità da e verso il `Instructor.Courses` proprietà di navigazione.

L'interfaccia utente che consente di modificare i corsi assegnati a un insegnante è costituita da un gruppo di caselle di controllo. È visualizzata una casella di controllo per ogni corso nel database e le caselle corrispondenti ai corsi attualmente assegnati all'insegnante sono selezionate. L'utente può selezionare e deselezionare le caselle di controllo per modificare le assegnazioni dei corsi. Se il numero di corsi fosse molto superiore, è preferibile usare un altro metodo di presentazione dei dati nella visualizzazione, ma si potrebbe usare lo stesso metodo di modifica delle proprietà di navigazione per poter creare o eliminare relazioni.

Per fornire i dati alla visualizzazione dell'elenco di caselle di controllo, si userà una classe modello di visualizzazione. Creare *AssignedCourseData.cs* nel *ViewModel* cartella e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Nelle *InstructorController.cs*, sostituire il `HttpGet` `Edit` metodo con il codice seguente. Le modifiche vengono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Il codice aggiunge il caricamento eager per la proprietà di navigazione `Courses` e chiama il nuovo metodo `PopulateAssignedCourseData` per fornire informazioni per la matrice di caselle di controllo tramite la classe modello di visualizzazione `AssignedCourseData`.

Il codice nel `PopulateAssignedCourseData` metodo legge tutti `Course` classe modello le entità per caricare un elenco di corsi tramite la vista. Per ogni corso, il codice verifica se è presente nella proprietà di navigazione `Courses` dell'insegnante. Per creare un ricerca efficiente quando si verifica se un corso viene assegnato all'insegnante, i corsi assegnati all'insegnante vengono inseriti in una [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) raccolta. Il `Assigned` è impostata su `true` per i corsi l'insegnante è assegnato. La visualizzazione usa questa proprietà per determinare quali caselle di controllo devono essere visualizzate come selezionate. Infine, tale elenco viene passato alla visualizzazione in un `ViewBag` proprietà.

Aggiungere quindi il codice che viene eseguito quando l'utente fa clic su **Save** (Salva). Sostituire il `EditPost` metodo con il codice seguente, che chiama un nuovo metodo che aggiorni le `Courses` proprietà di navigazione del `Instructor` entità. Le modifiche vengono evidenziate.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

La firma del metodo ora è diversa dal `HttpGet` `Edit` metodo, in modo che il nome del metodo cambia da `EditPost` al `Edit`.

Poiché la vista non dispone di una raccolta di `Course` entità, il gestore di associazione del modello non può aggiornare automaticamente il `Courses` proprietà di navigazione. Invece di usare lo strumento di associazione di modelli per aggiornare il `Courses` proprietà di navigazione, è possibile farlo nel nuovo `UpdateInstructorCourses` (metodo). È pertanto necessario escludere la proprietà `Courses` dall'associazione di modelli. Ciò non richiede modifiche al codice che chiama [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) perché si sta usando la *nell'elenco elementi consentiti* overload e `Courses` non è presente nell'elenco di inclusione.

Se nessun controllo caselle sono state selezionate, il codice nel `UpdateInstructorCourses` Inizializza il `Courses` proprietà di navigazione con una raccolta vuota:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Il codice quindi esegue il ciclo di tutti i corsi nel database e controlla ogni corso a fronte di quelli assegnati all'insegnante rispetto a quelli selezionati nella visualizzazione. Per facilitare l'esecuzione di ricerche efficienti, le ultime due raccolte sono archiviate all'interno di oggetti `HashSet`.

Se la casella di controllo di un corso è selezionata ma il corso non è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene aggiunto alla raccolta nella proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Se la casella di controllo di un corso non è selezionata ma il corso è presente nella proprietà di navigazione `Instructor.Courses`, il corso viene rimosso dalla proprietà di navigazione.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Nelle *Views\Instructor\Edit.cshtml*, aggiungere un **corsi** campo con una matrice di caselle di controllo aggiungendo il codice seguente codice immediatamente dopo la `div` elementi per il `OfficeAssignment` campo e prima di `div` (elemento) per il **salvare** pulsante:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Dopo aver incollato il codice, se le interruzioni di riga e il rientro non aspetto come avviene in questo caso, correggere manualmente tutti gli elementi in modo che somigli a quello che vedete qui. Il rientro non deve necessariamente essere perfetto, ma le righe `@</tr><tr>`, `@:<td>`, `@:</td>` e `@</tr>` devono trovarsi in una sola riga, come illustrato. In caso contrario, viene visualizzato un errore di runtime.

Questo codice crea una tabella HTML con tre colonne. In ogni colonna è presente una casella di controllo seguita da una didascalia costituita dal numero di corso e dal titolo. Tutte le caselle di controllo hanno lo stesso nome ("selectedCourses"), che informa lo strumento di associazione del modello in cui devono essere considerati come un gruppo. Il `value` attributo di ogni casella di controllo è impostato sul valore di `CourseID.` quando la pagina viene pubblicata, lo strumento di associazione di modelli passa una matrice al controller che è costituito il `CourseID` i valori per solo le caselle di controllo siano selezionate.

Quando le caselle di controllo sono inizialmente sottoposto a rendering, quelle corrispondenti ai corsi assegnati all'insegnante hanno `checked` attributi, che rendono selezionate (visualizzate come selezionate).

Dopo aver modificato le assegnazioni dei corsi, si sia in grado di verificare le modifiche quando il sito torna al `Index` pagina. Pertanto, è necessario aggiungere una colonna nella tabella in tale pagina. In questo caso non è necessario usare il `ViewBag` dell'oggetto, le informazioni da visualizzare è già nel `Courses` proprietà di navigazione del `Instructor` entità che si sta passando alla pagina del modello.

Nelle *Views\Instructor\Index.cshtml*, aggiungere un **corsi** intestazione subito dopo la **Office** intestazione, come illustrato nell'esempio seguente:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Aggiungere quindi una nuova cella di dettaglio subito dopo la cella di dettaglio di percorso di office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Eseguire la **indice degli insegnanti** pagina per visualizzare i corsi assegnati a ogni insegnante.

Fare clic su **modifica** per un insegnante per visualizzare la pagina di modifica.

Modificare alcune assegnazioni di corsi e fare clic su **salvare**. Le modifiche effettuate si riflettono nella pagina di indice.

 Nota: L'approccio qui adottato per la modifica di dati course insegnante funziona bene quando è presente un numero limitato di corsi. Per raccolte molto più grandi, sarebbero necessari un'interfaccia utente diversa e un altro metodo di aggiornamento.

## <a name="update-deleteconfirmed"></a>Aggiornare DeleteConfirmed

Nelle *InstructorController.cs*, eliminare il `DeleteConfirmed` (metodo) e inserire il codice seguente al suo posto.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Questo codice rende la modifica seguente:

- Se l'insegnante è assegnato come responsabile di qualsiasi reparto, tale assegnazione viene rimossa da tale reparto. Senza questo codice, si otterrebbe un errore di integrità referenziale se si è provato a eliminare un insegnante che è stato assegnato come amministratore per un reparto.

Questo codice non gestisce lo scenario di un insegnante assegnato come amministratore di più reparti. Nell'ultima esercitazione si aggiungerà codice che impedisce che si verifichi tale scenario.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Aggiungere posizione dell'ufficio e corsi alla pagina Create (Crea)

Nelle *InstructorController.cs*, eliminare il `HttpGet` e `HttpPost` `Create` metodi e quindi aggiungere il codice seguente al loro posto:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Questo codice è simile a quello per i metodi di modifica con la differenza che inizialmente non è selezionato alcun corso. Il `HttpGet` `Create` chiamate al metodo il `PopulateAssignedCourseData` metodo non perché possono essere presenti corsi selezionati, ma per poter fornire una raccolta vuota per il `foreach` ciclo nella visualizzazione (in caso contrario, il codice di visualizzazione genera un'eccezione con riferimento null ).

Il metodo di creazione HttpPost aggiunge i corsi selezionati alla proprietà di navigazione corsi prima il codice del modello che verifica la presenza di errori di convalida e aggiunta il nuovo insegnante al database. I corsi vengono aggiunti anche se sono presenti errori del modello in modo che quando sono presenti errori di modello (ad esempio, l'utente digita una data non valida), in modo che quando la pagina viene visualizzata di nuovo con un messaggio di errore, qualsiasi selezione di corsi effettuate vengono ripristinati automaticamente.

Si noti che, perché sia possibile aggiungere corsi alla proprietà di navigazione `Courses`, è necessario inizializzare la proprietà come raccolta vuota:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Anziché all'interno di codice di controllo, è possibile eseguire questa operazione nel modello Instructor modificando il getter della proprietà in modo da creare automaticamente la raccolta, se questa non esiste, come illustrato nell'esempio seguente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Se si modifica la proprietà `Courses` in questo modo, è possibile rimuovere il codice di inizializzazione esplicita della proprietà nel controller.

Nelle *Views\Instructor\Create.cshtml*, aggiungere una casella di testo percorso office e le caselle di controllo di corsi dopo l'assunzione data campo e prima di **Submit** pulsante.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Dopo aver incollato il codice, correggere le interruzioni di riga e rientri, come in precedenza per la pagina di modifica.

Eseguire la pagina di creazione e aggiungere un insegnante.

<a id="transactions"></a>

## <a name="handling-transactions"></a>Gestione delle transazioni

Come spiegato nel [esercitazione di base funzionalità CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), per impostazione predefinita Entity Framework implementa in modo implicito le transazioni. Per gli scenari in cui è necessario un maggior controllo, ad esempio, se si desidera includere le operazioni eseguite all'esterno di Entity Framework in una transazione, vedere [utilizzo di transazioni](https://msdn.microsoft.com/data/dn456843) su MSDN.

## <a name="get-the-code"></a>Ottenere il codice

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>Risorse aggiuntive

Collegamenti ad altre risorse di Entity Framework sono disponibili nel [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-step"></a>Passaggio successivo

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Pagine dei corsi personalizzati
> * Office aggiunto alla pagina instructors (insegnanti)
> * Corsi aggiunti alla pagina instructors (insegnanti)
> * DeleteConfirmed aggiornato
> * Posizione dell'ufficio aggiunto e corsi alla pagina Create

Passare all'articolo successivo per informazioni su come implementare un modello di programmazione asincrona.
> [!div class="nextstepaction"]
> [Modello di programmazione asincrona](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
