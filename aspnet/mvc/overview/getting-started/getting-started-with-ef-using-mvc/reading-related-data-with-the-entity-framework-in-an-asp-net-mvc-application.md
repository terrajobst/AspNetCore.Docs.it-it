---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lettura dei dati correlati con Entity Framework in un'applicazione MVC ASP.NET | Documenti Microsoft
author: tdykstra
description: /ajax/tutorials/using-ajax-control-toolkit-controls-and-control-extenders-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 06784d8b610856e71eae78b0db2d0253faedb955
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Lettura correlate a dati con Entity Framework in un'applicazione MVC ASP.NET
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Visual Studio 2013 e Code First di Entity Framework 6. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nell'esercitazione precedente è stato completato il modello di dati dell'istituto di istruzione. In questa esercitazione verrà leggere e visualizzare i dati correlati, ovvero i dati di Entity Framework carica le proprietà di navigazione.

Le figure seguenti illustrano le pagine che verranno usate.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Lazywriter, Eager ed esplicita durante il caricamento di dati correlati

Esistono diversi modi di Entity Framework può caricare i dati correlati alle proprietà di navigazione di un'entità:

- *Caricamento lazy*. Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati. La prima volta che si tenta di accedere a una proprietà di navigazione, tuttavia, i dati necessari per quest'ultima vengono recuperati automaticamente. Di conseguenza, più query inviate al database, ovvero uno per l'entità stessa e uno ogni volta che i dati per l'entità correlati deve essere recuperato. La `DbContext` classe consente il caricamento lazy per impostazione predefinita. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Caricamento eager*. Quando l'entità viene letta, i dati correlati corrispondenti vengono recuperati insieme ad essa. Ciò in genere ha come risultato una query join singola che recupera tutti i dati necessari. Per specificare il caricamento non differito, utilizzare il `Include` metodo.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Caricamento esplicito* È simile al caricamento lazy, ad eccezione del fatto che vengano recuperate in modo esplicito i dati correlati nel codice. non avviene automaticamente quando si accede a una proprietà di navigazione. Si caricare manualmente i dati correlati ottenendo la voce di gestione dello stato di oggetto per un'entità e la chiamata di [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) metodo per le raccolte o [Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) metodo per la proprietà che contengono un singola entità. (Nell'esempio seguente, se si desidera caricare la proprietà di navigazione di amministratore, sostituirà `Collection(x => x.Courses)` con `Reference(x => x.Administrator)`.) In genere si utilizza il caricamento esplicito solo dopo aver attivato disattivazione del caricamento lazy.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Perché non immediatamente recuperano i valori delle proprietà, il caricamento differito e il caricamento esplicito sono entrambi noto anche come *il caricamento posticipato*.

### <a name="performance-considerations"></a>Considerazioni sulle prestazioni

Se si sa di aver bisogno di dati correlati per tutte le entità recuperate, il caricamento eager spesso garantisce le prestazioni migliori, perché l'invio di un'unica query al database è in genere più efficiente dell'invio di query separate per ogni entità recuperata. Ad esempio, negli esempi precedenti, si supponga che ogni reparto di dieci corsi correlati. Nell'esempio di caricamento eager comporta solo una query singola (join) e un singolo round trip al database. Il caricamento lazy e il caricamento esplicito esempi sia comporta undici query e undici round trip al database. I round trip aggiuntivi al database influiscono in modo particolarmente negativo sulle prestazioni quando la latenza è elevata.

D'altra parte, in alcuni scenari è più efficiente il caricamento lazy. Caricamento eager potrebbe causare un join molto complesso da generare, quale SQL Server in grado di elaborare in modo efficiente. O se si desidera accedere alle proprietà di navigazione di un'entità solo per un subset di un set di entità, si sta elaborando, il caricamento lazy potrebbe offrire prestazioni migliori perché il caricamento non differito dovrà recuperare più dati è necessario. Se le prestazioni rappresentano un aspetto essenziale, per avere la certezza di scegliere il metodo più efficiente è consigliabile testare le prestazioni di entrambi i tipi di caricamento.

Caricamento lazy in grado di mascherare il codice che causa problemi di prestazioni. Codice che non consente di specificare il caricamento eager o esplicito ma elabora un'elevata quantità di entità e utilizzate varie proprietà di navigazione in ogni iterazione, ad esempio, potrebbe essere molto inefficiente (a causa dei numerosi round trip al database). Un'applicazione che esegue anche in fase di sviluppo utilizzando un server SQL locale può verificarsi dei problemi di prestazioni quando è stato spostato in Database SQL di Azure a causa di un aumento della latenza e il caricamento lazy. Profilatura delle query di database con un carico di test realistici consentirà di determinare se il caricamento lazy è appropriato. Per ulteriori informazioni vedere [Demistificazione delle strategie di Entity Framework: il caricamento di dati correlati](https://msdn.microsoft.com/magazine/hh205756.aspx) e [tramite Entity Framework per ridurre la latenza di rete a SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Disabilitare il caricamento lazy prima della serializzazione

Se si lascia caricamento lazy abilitato durante la serializzazione, possono finire una query di dati significativamente più elevata da quello desiderato. Serializzazione funziona in genere l'accesso a ogni proprietà in un'istanza di un tipo. Accesso alle proprietà attiva il caricamento lazy e le entità di caricamento lazy vengono serializzate. Il processo di serializzazione accede quindi a ogni proprietà delle entità caricamento lazy, causando potenzialmente maggiore serializzazione e il caricamento lazy. Per evitare questa reazione a catena runaway, attivare la disattivazione del caricamento prima che la serializzazione di un'entità lazy.

Serializzazione può anche essere complicata dalle classi proxy che usa Entity Framework, come illustrato nel [esercitazione scenari avanzati](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

È di un modo per evitare problemi di serializzazione per serializzare oggetti di trasferimento di dati DTO anziché oggetti entità, come illustrato nel [tramite Web API with Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) esercitazione.

Se non si utilizza DTO, è possibile disabilitare il caricamento lazy e per evitare problemi di proxy da [la disabilitazione di creazione del proxy](https://msdn.microsoft.com/data/jj592886.aspx).

Ecco un altro [modi per disabilitare il caricamento lazy](https://msdn.microsoft.com/data/jj574232):

- Per le proprietà di navigazione specifici, omettere il `virtual` (parola chiave) quando si dichiara la proprietà.
- Per tutte le proprietà di navigazione, impostare `LazyLoadingEnabled` a `false`, inserire il codice seguente nel costruttore della classe di contesto: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>Creare il nome del reparto consente di visualizzare una pagina di corsi

Il `Course` entità include proprietà di navigazione che contiene il `Department` entità del corso assegnato al reparto. Per visualizzare il nome del reparto assegnato in un elenco dei corsi, è necessario ottenere il `Name` proprietà il `Department` entità che la `Course.Department` proprietà di navigazione.

Creare un controller denominato `CourseController` (non CoursesController) per il `Course` tipo di entità, con le stesse opzioni per il **Controller MVC 5 con visualizzazioni, mediante Entity Framework** scaffolder che hai usato in precedenza per il `Student` controller, come illustrato nella figura seguente:

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Aprire *Controllers\CourseController.cs* ed esaminare il `Index` metodo:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Lo scaffolding automatico ha specificato il caricamento eager per la proprietà di navigazione `Department` tramite il metodo `Include`.

Aprire *Views\Course\Index.cshtml* e sostituire il codice del modello con il codice seguente. Le modifiche sono evidenziate:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Al codice con scaffolding sono state apportate le modifiche seguenti:

- Modificare il titolo da **indice** a **corsi**.
- È stata aggiunta la colonna **Number** (Numero) con il valore della proprietà `CourseID`. Per impostazione predefinita, le chiavi primarie non sono scaffolding perché in genere sono utilizzati per gli utenti finali. In questo caso, tuttavia, la chiave primaria è significativa ed è necessario visualizzarla.
- Spostare il **reparto** colonna a destra e modificare l'intestazione. Il scaffolder correttamente scelto di visualizzare il `Name` proprietà il `Department` entità, ma in questo caso nella pagina corso sull'intestazione di colonna deve essere **reparto** anziché **nome**.

Si noti che per la colonna di reparto, il codice di scaffolding Visualizza il `Name` proprietà del `Department` entità che viene caricato il `Department` proprietà di navigazione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Eseguire la pagina (selezionare il **corsi** scheda nella home page University di Contoso) per visualizzare l'elenco con i nomi di reparto.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Creare una pagina istruttori che mostra i corsi e le registrazioni

In questa sezione viene creata un controller e visualizzare per il `Instructor` entità per visualizzare la pagina istruttori:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Questa pagina legge e visualizza dati correlati nei modi seguenti:

- L'elenco di istruttori vengono visualizzati i dati correlati di `OfficeAssignment` entità. Tra le entità `Instructor` e `OfficeAssignment` c'è una relazione uno-a-zero-o-uno. Si userà il caricamento immediato per la `OfficeAssignment` entità. Come spiegato in precedenza, il caricamento eager è in genere più efficiente quando sono necessari i dati correlati per tutte le righe recuperate della tabella primaria. In questo caso, si vogliono visualizzare le assegnazioni di ufficio per tutti gli insegnanti visualizzati.
- Quando l'utente seleziona un docente, correlato `Course` vengono visualizzate le entità. Tra le entità `Instructor` e `Course` esiste una relazione molti-a-molti. Si userà il caricamento immediato per la `Course` entità e le relative `Department` entità. In questo caso, il caricamento lazy potrebbe essere più efficiente perché è necessario corsi solo per istruttore selezionato. Questo esempio, tuttavia, illustra come usare il caricamento eager per le proprietà di navigazione con entità esse stesse all'interno di proprietà di navigazione.
- Quando l'utente seleziona un corso, i dati da relativi il `Enrollments` viene visualizzato il set di entità. Tra le entità `Course` e `Enrollment` esiste una relazione uno-a-molti. Si aggiungerà il caricamento esplicito per `Enrollment` entità e le relative `Student` entità. (Caricamento esplicito non è necessario perché il caricamento lazy è abilitato, ma viene illustrato come eseguire il caricamento esplicito.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Creare un modello di visualizzazione per la visualizzazione dell'indice Instructor

La pagina istruttori mostra tre tabelle diverse. Verrà quindi creato un modello di visualizzazione che includa tre proprietà, ognuna contenente i dati di una delle tabelle.

Nel *ViewModel* cartella, creare *InstructorIndexData.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Creare le visualizzazioni e Controller Instructor

Creare un `InstructorController` (non InstructorsController) controller con azioni di lettura/scrittura EF come illustrato nella figura seguente:

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Aprire *Controllers\InstructorController.cs* e aggiungere un `using` istruzione per il `ViewModels` dello spazio dei nomi:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Il codice scaffolding di `Index` metodo specifica caricamento eager solo per il `OfficeAssignment` proprietà di navigazione:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Sostituire il `Index` (metodo) con il codice seguente per caricare altri dati correlati e inserirlo nel modello di visualizzazione:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Il metodo accetta i dati della route facoltativo (`id`) e un parametro di stringa di query (`courseID`) che forniscono i valori di ID del corso selezionato e istruttore selezionato e tutti i dati richiesti passa alla visualizzazione. I parametri sono forniti dai collegamenti ipertestuali **Select** (Seleziona) nella pagina.

Il codice inizia creando un'istanza del modello di visualizzazione e inserendola nell'elenco degli insegnanti. Il codice specifica per il caricamento immediato di `Instructor.OfficeAssignment` e `Instructor.Courses` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

Il secondo `Include` metodo carica corsi e ogni corso caricato caricamento eager quanto avviene per il `Course.Department` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Come accennato in precedenza, il caricamento non differito non è obbligatorio ma è lo scopo di migliorare le prestazioni. Poiché la vista è sempre necessario il `OfficeAssignment` entità, risulta più efficiente recuperare che nella stessa query. `Course` le entità sono necessarie quando un docente è selezionato nella pagina web è migliore il caricamento lazy solo se la pagina viene visualizzata più spesso con un corso selezionato rispetto senza caricamento eager.

Se è stato selezionato un ID istruttore istruttore selezionato viene recuperato dall'elenco di istruttori nel modello di visualizzazione. Il modello di visualizzazione `Courses` proprietà viene quindi caricata con le `Course` entità da tale istruttore `Courses` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Il `Where` metodo restituisce una raccolta, ma in questo caso i criteri di passati al risultato in un solo metodo `Instructor` la restituzione di entità. Il `Single` metodo converte la raccolta in un unico `Instructor` entità, che consente di accedere a tale entità `Courses` proprietà.

Utilizzare il [singolo](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) metodo in una raccolta quando si conosce la raccolta ha un solo elemento. Il `Single` metodo genera un'eccezione se la raccolta passata a esso è vuota o se è presente più di un elemento. In alternativa, è [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), che restituisce un valore predefinito (`null` in questo caso) se la raccolta è vuota. Tuttavia, in questo caso che ancora comporta un'eccezione (di tentare di trovare un `Courses` proprietà in un `null` riferimento), e il messaggio di eccezione meno chiaramente indica la causa del problema. Quando si chiama il `Single` (metodo), è inoltre possibile passare il `Where` condizione anziché chiamare il `Where` metodo separatamente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Invece di:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Se è stato selezionato un corso, questo viene quindi recuperato dall'elenco dei corsi nel modello di visualizzazione. Del quindi il modello di visualizzazione `Enrollments` proprietà viene caricata con le `Enrollment` entità da quel corso `Enrollments` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Modificare la visualizzazione dell'indice Instructor

In *Views\Instructor\Index.cshtml*, sostituire il codice del modello con il codice seguente. Le modifiche sono evidenziate:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Al codice esistente sono state apportate le modifiche seguenti:

- La classe del modello è stata modificata in `InstructorIndexData`.
- Il titolo pagina è stato modificato da **Index** (Indice) a **Instructors** (Insegnanti).
- Aggiungere un **Office** colonna che visualizza `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non è null. (Poiché si tratta di una relazione uno-a-zero-o-uno, potrebbe non essere disponibile un processo di `OfficeAssignment` entità.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Il codice aggiunto che verrà aggiunto in modo dinamico `class="success"` per il `tr` elemento istruttore selezionato. Consente di impostare un colore di sfondo per la riga selezionata utilizzando un [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) classe. 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Aggiunta di un nuovo `ActionLink` etichetta **selezionare** immediatamente prima di altri collegamenti in ogni riga, che comporta l'ID istruttore selezionato da inviare al `Index` metodo.

Eseguire l'applicazione e selezionare il **i docenti** scheda. Nella pagina vengono visualizzati il `Location` proprietà correlate `OfficeAssignment` entità e una tabella vuota cella quando è no correlato `OfficeAssignment` entità.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Nel *Views\Instructor\Index.cshtml* file, dopo la chiusura `table` elemento (alla fine del file), aggiungere il codice seguente. Quando è selezionato un insegnante, Questo codice visualizza un elenco dei corsi correlati all'insegnante stesso.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Questo codice legge la proprietà `Courses` del modello di visualizzazione per visualizzare l'elenco dei corsi. Fornisce inoltre un `Select` collegamento ipertestuale che invia l'ID del corso selezionato per il `Index` metodo di azione.

Eseguire la pagina e selezionare un docente. È ora possibile vedere una griglia con i corsi assegnati all'insegnante selezionato. Per ogni corso è possibile vedere il nome del dipartimento assegnato.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Dopo il blocco di codice appena aggiunto, aggiungere il codice seguente. Quando è selezionato un corso, questo codice visualizza l'elenco degli studenti iscritti al corso selezionato.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Questo codice legge il `Enrollments` proprietà del modello di visualizzazione per visualizzare un elenco di studenti iscritti in corso.

Eseguire la pagina e selezionare un docente. Selezionare quindi un corso per visualizzare l'elenco degli studenti iscritti e i voti corrispondenti.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>Aggiunta di caricamento esplicito

Aprire *InstructorController.cs* e osservare come la `Index` metodo ottiene l'elenco delle registrazioni per un corso selezionato:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Quando è stato recuperato l'elenco di istruttori, specificato per il caricamento immediato di `Courses` proprietà di navigazione e per il `Department` proprietà di ogni corso. Inserire il `Courses` insieme nel modello di visualizzazione e l'ora si accede di `Enrollments` da un'entità nella raccolta di proprietà di navigazione. Poiché non è stato specificato per il caricamento non differito il `Course.Enrollments` proprietà di navigazione, i dati da tale proprietà viene visualizzato nella pagina in seguito al caricamento lazy.

Se è stato disabilitato il caricamento lazy senza modificare il codice in altro modo, il `Enrollments` proprietà sarà null indipendentemente dal numero le registrazioni il corso ha effettivamente. In questo caso, per caricare il `Enrollments` proprietà, è necessario specificare il caricamento eager o caricamento esplicito. Già visto come eseguire l'operazione di caricamento eager. Per visualizzare un esempio di caricamento esplicito, sostituire il `Index` metodo con il codice seguente, che carica in modo esplicito il `Enrollments` proprietà. Il codice modificato vengono evidenziati.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Dopo aver ottenuto il `Course` entità, il nuovo codice carica in modo esplicito tale corso `Enrollments` proprietà di navigazione:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Successivamente viene caricata in modo esplicito ogni `Enrollment` collegati entità `Student` entità:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Si noti che si utilizzano il `Collection` per caricare una proprietà di raccolta, ma per una proprietà che contiene una sola entità, utilizzare il `Reference` metodo.

Eseguire la pagina di indice istruttore a questo punto, non si noterà alcuna differenza di ciò che viene visualizzato nella pagina anche se hai modificato la modalità di recupero dei dati.

## <a name="summary"></a>Riepilogo

Ora utilizzati tutti i tre modi (Lazywriter, eager ed espliciti) per caricare i dati correlati nelle proprietà di navigazione. Nella prossima esercitazione si apprenderà come aggiornare i dati correlati.

Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione ed è stato possibile apportare miglioramenti. È inoltre possibile richiedere nuovi argomenti in [Mostra Me come con codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Collegamenti ad altre risorse di Entity Framework, vedere il [accesso ai dati ASP.NET - risorse](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Successivo](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
