---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lettura dei dati correlati con Entity Framework in un'applicazione ASP.NET MVC (5 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4767b015db0bad09942802827ce54162687fcabc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826906"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>La lettura di dati correlati con Entity Framework in un'applicazione ASP.NET MVC (5 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni a partire dall'inizio oppure [scarica un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si verifica un problema è possibile risolvere, [Scarica il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. Confrontando il codice per il codice completo è generalmente possibile trovare la soluzione al problema. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente è stato completato il modello di dati dell'istituto di istruzione. In questa esercitazione verrà letto e visualizzare dati correlati, ovvero i dati che Entity Framework carica all'interno di proprietà di navigazione.

Le figure seguenti illustrano le pagine che verranno usate.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Caricamento esplicito, Eager e lazy dei dati correlati

Esistono diversi modi che Entity Framework può caricare i dati correlati nelle proprietà di navigazione di un'entità:

- *Caricamento lazy*. Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati. La prima volta che si tenta di accedere a una proprietà di navigazione, tuttavia, i dati necessari per quest'ultima vengono recuperati automaticamente. Ciò comporta più query inviate al database, ovvero uno per l'entità stessa e uno ogni volta che i dati per l'entità correlati deve essere recuperato. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Caricamento eager*. Quando l'entità viene letta, i dati correlati corrispondenti vengono recuperati insieme ad essa. Ciò in genere ha come risultato una query join singola che recupera tutti i dati necessari. Specificare il caricamento eager con la `Include` (metodo).

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Caricamento esplicito* Ciò è simile a il caricamento lazy, ad eccezione del fatto che recuperare in modo esplicito i dati correlati nel codice. non avviene automaticamente quando si accede a una proprietà di navigazione. Caricare manualmente i dati correlati ottenendo la voce di gestione dello stato di oggetti per un'entità e chiamare il metodo la `Collection.Load` metodo per le raccolte o `Reference.Load` metodo per la proprietà che contengono una singola entità. (Nell'esempio seguente, se si desidera caricare la proprietà di navigazione di amministratore, sostituirà `Collection(x => x.Courses)` con `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Perché non immediatamente recuperano i valori delle proprietà, il caricamento lazy e il caricamento esplicito sono entrambi noto anche come *caricamento posticipato*.

In generale, se si conosce occorre dati correlati per ogni entità durante il caricamento eager, recuperato offre le migliori prestazioni, perché è in genere più efficiente delle query separate per ogni entità recuperata una singola query inviata al database. Ad esempio, negli esempi precedenti, si supponga che ogni dipartimento abbia dieci corsi correlati. Nell'esempio il caricamento eager comporta solo una query singola (join) e un singolo round trip al database. Il caricamento lazy ed esempi di caricamento esplicito entrambi comporta undici query e di disporre di undici round trip al database. I round trip aggiuntivi al database influiscono in modo particolarmente negativo sulle prestazioni quando la latenza è elevata.

D'altra parte, in alcuni scenari il caricamento lazy è più efficiente. Il caricamento eager potrebbe causare un join molto complesso da generare, quale SQL Server non è possibile elaborare in modo efficiente. Oppure, se si desidera accedere alle proprietà di navigazione di un'entità solo per un subset di un set di entità l'elaborazione, il caricamento lazy potrebbe offrire prestazioni migliori perché il caricamento eager recupererebbe più dati superflui. Se le prestazioni rappresentano un aspetto essenziale, per avere la certezza di scegliere il metodo più efficiente è consigliabile testare le prestazioni di entrambi i tipi di caricamento.

In genere si usa il caricamento esplicito solo quando hai attivato disattivare caricamento lazy. Uno scenario in cui è necessario attivare disattivare caricamento lazy è durante la serializzazione. Serializzazione e il caricamento lazy non combinare correttamente e se non si presta attenzione che può accadere che l'esecuzione di query notevolmente più dati da quello desiderato quando lazy è abilitato il caricamento. Serializzazione in genere funziona l'accesso a ogni proprietà in un'istanza di un tipo. Accesso a proprietà attiva il caricamento lazy e le entità caricate lazy vengono serializzati. Il processo di serializzazione accede quindi a ogni proprietà delle entità a caricamento lazy, causando potenzialmente anche altre modalità di caricamento lazy e la serializzazione. Per evitare questa reazione a catena runaway, disattivare caricamento disattivato prima di serializzazione di un'entità lazy.

La classe del contesto del database esegue il caricamento lazy per impostazione predefinita. Esistono due modi per disabilitare il caricamento lazy:

- Per le proprietà di navigazione specifici, omettere il `virtual` parola chiave quando si dichiara la proprietà.
- Per tutte le proprietà di navigazione, impostare `LazyLoadingEnabled` a `false`. Ad esempio, è possibile inserire il codice seguente nel costruttore della classe di contesto: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Il caricamento lazy può nascondere il codice che causa problemi di prestazioni. Codice che non è specificato il caricamento eager o esplicito, ma elabora un'elevata quantità di entità e Usa diverse proprietà di navigazione in ogni iterazione, ad esempio, potrebbe essere molto inefficiente (a causa di molti round trip al database). Un'applicazione che esegue anche in fase di sviluppo tramite un server SQL locale potrebbe avere problemi di prestazioni quando si è spostato in Database SQL di Azure a causa di un aumento della latenza e il caricamento lazy. La profilatura delle query di database con un carico realistico test consente di determinare se il caricamento lazy è appropriato. Per altre informazioni, vedere [Demistificazione delle strategie di Entity Framework: il caricamento di dati correlati](https://msdn.microsoft.com/magazine/hh205756.aspx) e [usando Entity Framework per ridurre la latenza di rete verso SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Creare il nome del reparto consente di visualizzare una pagina di indice corsi

Il `Course` entità include una proprietà di navigazione che contiene il `Department` entità del dipartimento assegnato al corso. Per visualizzare il nome del dipartimento assegnato in un elenco dei corsi, è necessario ottenere il `Name` proprietà dal `Department` entità a cui è nel `Course.Department` proprietà di navigazione.

Creare un controller denominato `CourseController` per il `Course` tipo di entità, utilizzando le stesse opzioni che è stato fatto in precedenza per il `Student` controller, come illustrato nella figura seguente (tranne che a differenza dell'immagine, la classe context nello spazio dei nomi DAL, non modelli spazio dei nomi):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Aprire *Controllers\CourseController.cs* ed esaminare il `Index` metodo:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Lo scaffolding automatico ha specificato il caricamento eager per la proprietà di navigazione `Department` tramite il metodo `Include`.

Aprire *Views\Course\Index.cshtml* e sostituire il codice esistente con il codice seguente. Le modifiche sono evidenziate:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Al codice con scaffolding sono state apportate le modifiche seguenti:

- Modificare il titolo da **indice** al **corsi**.
- Spostare i collegamenti di riga a sinistra.
- Aggiunta di una colonna sotto l'intestazione **numero** che mostra il `CourseID` valore della proprietà. (Per impostazione predefinita, le chiavi primarie non sottoposto a scaffolding perché in genere sono significative per gli utenti finali. Tuttavia, in questo caso la chiave primaria è significativa e si vuole visualizzarlo.)
- Modificare l'ultima intestazione di colonna da **DepartmentID** (il nome della chiave esterna per il `Department` entità) per **reparto**.

Si noti che per l'ultima colonna, il codice con scaffolding Visualizza il `Name` proprietà del `Department` entità a cui viene caricato nel `Department` proprietà di navigazione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Esecuzione della pagina (selezionare il **corsi** scheda nella home page di Contoso University) per visualizzare l'elenco con i nomi dei reparti.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Creare una pagina di indice degli insegnanti che mostri i corsi e le registrazioni

In questa sezione viene creata un controller e visualizzare per il `Instructor` entità per visualizzare la pagina di indice degli insegnanti:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Questa pagina legge e visualizza dati correlati nei modi seguenti:

- L'elenco di insegnanti Visualizza dati correlati tratti dal `OfficeAssignment` entità. Tra le entità `Instructor` e `OfficeAssignment` c'è una relazione uno-a-zero-o-uno. Si userà il caricamento eager per la `OfficeAssignment` entità. Come spiegato in precedenza, il caricamento eager è in genere più efficiente quando sono necessari i dati correlati per tutte le righe recuperate della tabella primaria. In questo caso, si vogliono visualizzare le assegnazioni di ufficio per tutti gli insegnanti visualizzati.
- Quando l'utente seleziona un insegnante, correlato `Course` vengono visualizzate le entità. Tra le entità `Instructor` e `Course` esiste una relazione molti-a-molti. Si userà il caricamento eager per la `Course` le entità e le relative `Department` entità. In questo caso, il caricamento lazy può essere più efficiente perché i corsi sono necessari solo per l'insegnante selezionato. Questo esempio, tuttavia, illustra come usare il caricamento eager per le proprietà di navigazione con entità esse stesse all'interno di proprietà di navigazione.
- Quando l'utente seleziona un corso, i dati correlati di `Enrollments` set di entità viene visualizzato. Tra le entità `Course` e `Enrollment` esiste una relazione uno-a-molti. Si aggiungerà il caricamento esplicito per `Enrollment` le entità e le relative `Student` entità. (Caricamento esplicito non è necessario perché il caricamento lazy è abilitato, ma viene illustrato come eseguire il caricamento esplicito.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Creare un modello di visualizzazione per la visualizzazione dell'indice Instructor

La pagina di indice degli insegnanti Visualizza tre tabelle diverse. Verrà quindi creato un modello di visualizzazione che includa tre proprietà, ognuna contenente i dati di una delle tabelle.

Nel *ViewModel* cartella, creare *InstructorIndexData.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Aggiunta di uno stile per le righe selezionate

Per contrassegnare le righe selezionate è necessario un colore di sfondo differente. Per fornire uno stile per questa interfaccia, aggiungere il codice evidenziato seguente alla sezione `/* info and errors */` nelle *Content\Site.css*, come illustrato di seguito:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Creare le visualizzazioni e Controller Instructor

Creare un `InstructorController` controller come illustrato nella figura seguente:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Aprire *Controllers\InstructorController.cs* e aggiungere un `using` istruzione per il `ViewModels` dello spazio dei nomi:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Il codice con scaffolding nel `Index` metodo viene specificato solo per il caricamento eager di `OfficeAssignment` proprietà di navigazione:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Sostituire il `Index` metodo con il codice seguente per caricare altri dati correlati e inserirlo nel modello di visualizzazione:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Il metodo accetta i dati di route facoltativi (`id`) e un parametro di stringa di query (`courseID`) che forniscono i valori di ID dell'insegnante e corsi selezionati e tutti i dati necessari passa alla visualizzazione. I parametri sono forniti dai collegamenti ipertestuali **Select** (Seleziona) nella pagina.

> [!TIP]
> 
> **Dati della route**
> 
> I dati di route sono che il gestore di associazione di modelli disponibili in un segmento di URL specificato nella tabella di routing. Ad esempio, la route predefinita specifica `controller`, `action`, e `id` segmenti:
> 
> route. Metodo MapRoute (  
>  nome: "Default",  
>  URL: "{controller} / {action} / {id}",  
>  le impostazioni predefinite: new {controller = "Home" action = "Index", id = UrlParameter.Optional}  
> );
> 
> L'URL seguente, la route predefinita mappa `Instructor` come il `controller`, `Index` come il `action` e 1 come il `id`; questi sono i valori dei dati di route.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID = 2021" è un valore di stringa di query. Lo strumento individuerebbe funzionerà anche se si passa il `id` come un valore di stringa di query:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Gli URL creati da `ActionLink` istruzioni nella visualizzazione Razor. Nel codice seguente, il `id` parametro corrisponde alla route predefinita, pertanto `id` viene aggiunto ai dati della route.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> Nel codice seguente, `courseID` non corrisponde a un parametro della route predefinita, viene aggiunto come una stringa di query.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Il codice inizia creando un'istanza del modello di visualizzazione e inserendola nell'elenco degli insegnanti. Nel codice viene specificato il caricamento eager per la `Instructor.OfficeAssignment` e il `Instructor.Courses` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

La seconda `Include` metodo carica corsi e per ogni corso caricato avviene il caricamento eager per la `Course.Department` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Come accennato in precedenza, il caricamento eager non è obbligatorio ma è lo scopo di migliorare le prestazioni. Poiché la visualizzazione richiede sempre il `OfficeAssignment` entità, è più efficiente recuperare quest nella stessa query. `Course` le entità sono necessari quando viene selezionato un insegnante nella pagina web, in modo che il caricamento eager è migliore il caricamento lazy solo se la pagina viene visualizzata più spesso con un corso selezionato.

Se è stato selezionato un ID istruttore, insegnante selezionato viene recuperato dall'elenco di insegnanti nel modello di visualizzazione. Il modello di visualizzazione `Courses` proprietà viene quindi caricata con il `Course` entità da tale insegnante `Courses` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Il `Where` metodo restituisce una raccolta, ma in questo caso i criteri passati a tale metodo vengono restituiti solo una singola `Instructor` la restituzione di entità. Il `Single` metodo converte la raccolta in un'unica `Instructor` entità, che consente di accedere a tale entità `Courses` proprietà.

Si utilizza il [singolo](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) metodo in una raccolta quando si sa che la raccolta non avrà un solo elemento. Il `Single` metodo genera un'eccezione se la raccolta passata è vuota o se è presente più di un elemento. Un'alternativa consiste [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), che restituisce un valore predefinito (`null` in questo caso) se la raccolta è vuota. Tuttavia, in questo caso che avrebbe comunque come risultato un'eccezione (dal tentativo di trovare una `Courses` proprietà su un `null` riferimento), e il messaggio di eccezione indicherebbe meno chiaramente la causa del problema. Quando si chiama il `Single` metodo, è anche possibile passare nel `Where` condizione invece di chiamare il `Where` metodo separatamente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Invece di:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Se è stato selezionato un corso, questo viene quindi recuperato dall'elenco dei corsi nel modello di visualizzazione. Quindi del modello di visualizzazione `Enrollments` proprietà viene caricata con le `Enrollment` entità di tale corso `Enrollments` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Modifica la visualizzazione dell'indice Instructor

Nelle *Views\Instructor\Index.cshtml*, sostituire il codice esistente con il codice seguente. Le modifiche sono evidenziate:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Al codice esistente sono state apportate le modifiche seguenti:

- La classe del modello è stata modificata in `InstructorIndexData`.
- Il titolo pagina è stato modificato da **Index** (Indice) a **Instructors** (Insegnanti).
- Spostare le colonne di collegamento di riga a sinistra.
- Rimossa il **FullName** colonna.
- Aggiungere un **Office** colonna che consente di visualizzare `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non null. (Poiché si tratta di una relazione uno-a-zero-o-uno, potrebbe non esserci una correlati `OfficeAssignment` entity.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Aggiunto il codice che verrà aggiunto in modo dinamico `class="selectedrow"` per il `tr` elemento dell'insegnante selezionato. Consente di impostare un colore di sfondo per la riga selezionata usando la classe CSS che creato in precedenza. (Il `valign` attributo saranno utile nell'esercitazione seguente quando si aggiunge una colonna con più righe alla tabella.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Aggiunto un nuovo `ActionLink` etichettato **selezionate** immediatamente prima degli altri collegamenti in ogni riga, in modo che l'ID dell'insegnante selezionato da inviare al `Index` (metodo).

Eseguire l'applicazione e selezionare il **instructors (insegnanti)** scheda. Nella pagina viene visualizzato il `Location` proprietà correlate `OfficeAssignment` entità e una tabella vuota cella quando è presente no correlato `OfficeAssignment` entità.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Nel *Views\Instructor\Index.cshtml* file, dopo la chiusura `table` elemento (alla fine del file), aggiungere il codice evidenziato seguente. Ciò consente di visualizzare un elenco dei corsi correlati all'insegnante quando viene selezionato un insegnante.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Questo codice legge la proprietà `Courses` del modello di visualizzazione per visualizzare l'elenco dei corsi. Fornisce inoltre una `Select` collegamento ipertestuale che invia l'ID del corso selezionato per il `Index` metodo di azione.

> [!NOTE]
> Il *CSS* file viene memorizzato nella cache dal browser. Se quando si esegue l'applicazione non vengono visualizzate le modifiche, eseguire un aggiornamento di disco rigido (tenere premuto il tasto CTRL mentre si fa clic il **Aggiorna** , oppure premere CTRL + F5).


Eseguire la pagina e selezionare un insegnante. È ora possibile vedere una griglia con i corsi assegnati all'insegnante selezionato. Per ogni corso è possibile vedere il nome del dipartimento assegnato.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Dopo il blocco di codice appena aggiunto, aggiungere il codice seguente. Quando è selezionato un corso, questo codice visualizza l'elenco degli studenti iscritti al corso selezionato.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Questo codice legge la `Enrollments` proprietà del modello di visualizzazione per visualizzare un elenco di studenti iscritti al corso.

Eseguire la pagina e selezionare un insegnante. Selezionare quindi un corso per visualizzare l'elenco degli studenti iscritti e i voti corrispondenti.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Aggiunta di caricamento esplicito

Aprire *InstructorController.cs* e osservare come il `Index` metodo ottiene l'elenco delle registrazioni per un corso selezionato:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Durante il recupero, elenco di insegnanti è specificato il caricamento eager per la `Courses` proprietà di navigazione e per il `Department` proprietà di ogni corso. Quindi si inseriscono le `Courses` insieme nel modello di visualizzazione e l'ora sta eseguendo l'accesso il `Enrollments` proprietà di navigazione da un'entità in tale raccolta. Poiché non è stato specificato il caricamento eager per la `Course.Enrollments` proprietà di navigazione, i dati da tale proprietà viene visualizzato nella pagina come risultato il caricamento lazy.

Se è disabilitato per il caricamento lazy senza modificare il codice in qualsiasi altro modo, il `Enrollments` proprietà sarà null indipendentemente dal numero registrazioni il corso, abbiamo dovuto. In tal caso, per caricare il `Enrollments` proprietà, è necessario specificare il caricamento eager o il caricamento esplicito. Già visto come eseguire il caricamento eager. Per visualizzare un esempio di caricamento esplicito, sostituire il `Index` metodo con il codice seguente, che carica in modo esplicito il `Enrollments` proprietà. Sono evidenziati il codice modificato.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Dopo aver ottenuto l'oggetto selezionato `Course` entità, il nuovo codice carica in modo esplicito tale corso `Enrollments` proprietà di navigazione:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Successivamente viene caricata in modo esplicito ognuno `Enrollment` entità relativa alla `Student` entità:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Si noti che si utilizzano il `Collection` per caricare una proprietà di raccolta, ma per una proprietà che contiene una sola entità, si utilizza il `Reference` (metodo). È possibile eseguire ora la pagina di indice degli insegnanti e non si vedrà alcuna differenza in ciò che viene visualizzata nella pagina, anche se è stata modificata la modalità di recupero dei dati.

## <a name="summary"></a>Riepilogo

È stato usato tutti i tre modi (lazy, eager ed espliciti) per caricare i dati correlati nelle proprietà di navigazione. Nella prossima esercitazione si apprenderà come aggiornare i dati correlati.

Sono disponibili collegamenti ad altre risorse di Entity Framework nel [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Successivo](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
