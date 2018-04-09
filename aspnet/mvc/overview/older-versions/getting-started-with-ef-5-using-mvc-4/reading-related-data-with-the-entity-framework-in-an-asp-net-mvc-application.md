---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Lettura dei dati correlati con Entity Framework in un'applicazione ASP.NET MVC (5 di 10) | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 831f5e0a8b57907ea012c10c1d1f8ff166f5e88b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Lettura correlate a dati con Entity Framework in un'applicazione ASP.NET MVC (5 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 utilizzando Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto di avvio per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si esegue un problema, è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. In genere, è possibile trovare la soluzione al problema di mediante un confronto tra il codice per il codice completato. Per alcuni errori comuni e su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente è stato completato il modello di dati dell'istituto di istruzione. In questa esercitazione verrà leggere e visualizzare i dati correlati, ovvero i dati di Entity Framework carica le proprietà di navigazione.

Le figure seguenti illustrano le pagine che verranno usate.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Lazywriter, Eager ed esplicita durante il caricamento di dati correlati

Esistono diversi modi di Entity Framework può caricare i dati correlati alle proprietà di navigazione di un'entità:

- *Caricamento lazy*. Quando un'entità viene letta per la prima volta, i dati correlati non vengono recuperati. La prima volta che si tenta di accedere a una proprietà di navigazione, tuttavia, i dati necessari per quest'ultima vengono recuperati automaticamente. Di conseguenza, più query inviate al database, ovvero uno per l'entità stessa e uno ogni volta che i dati per l'entità correlati deve essere recuperato. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Caricamento eager*. Quando l'entità viene letta, i dati correlati corrispondenti vengono recuperati insieme ad essa. Ciò in genere ha come risultato una query join singola che recupera tutti i dati necessari. Per specificare il caricamento non differito, utilizzare il `Include` metodo.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Caricamento esplicito* È simile al caricamento lazy, ad eccezione del fatto che vengano recuperate in modo esplicito i dati correlati nel codice. non avviene automaticamente quando si accede a una proprietà di navigazione. Si caricare manualmente i dati correlati ottenendo la voce di gestione dello stato di oggetto per un'entità e la chiamata di `Collection.Load` metodo per le raccolte o `Reference.Load` metodo per la proprietà che contengono una singola entità. (Nell'esempio seguente, se si desidera caricare la proprietà di navigazione di amministratore, sostituirà `Collection(x => x.Courses)` con `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Perché non immediatamente recuperano i valori delle proprietà, il caricamento differito e il caricamento esplicito sono entrambi noto anche come *il caricamento posticipato*.

In generale, se si conosce sono necessari i dati correlati per ogni entità caricamento eager, recuperato offre le migliori prestazioni, perché una singola query inviate al database è in genere più efficiente delle query separate per ogni entità recuperati. Ad esempio, negli esempi precedenti, si supponga che ogni reparto di dieci corsi correlati. Nell'esempio di caricamento eager comporta solo una query singola (join) e un singolo round trip al database. Il caricamento lazy e il caricamento esplicito esempi sia comporta undici query e undici round trip al database. I round trip aggiuntivi al database influiscono in modo particolarmente negativo sulle prestazioni quando la latenza è elevata.

D'altra parte, in alcuni scenari è più efficiente il caricamento lazy. Caricamento eager potrebbe causare un join molto complesso da generare, quale SQL Server in grado di elaborare in modo efficiente. O se si desidera accedere alle proprietà di navigazione di un'entità solo per un subset di un set di entità, si sta elaborando, il caricamento lazy potrebbe offrire prestazioni migliori perché il caricamento non differito dovrà recuperare più dati è necessario. Se le prestazioni rappresentano un aspetto essenziale, per avere la certezza di scegliere il metodo più efficiente è consigliabile testare le prestazioni di entrambi i tipi di caricamento.

In genere si utilizza il caricamento esplicito solo dopo aver attivato disattivazione del caricamento lazy. Uno scenario in cui è necessario attivare disattivazione del caricamento lazy è durante la serializzazione. Serializzazione e il caricamento lazy non combinare anche, e se non è possibile che il esecuzione di query notevolmente più dati da quello desiderato quando lazy attenzione durante il caricamento è abilitato. Serializzazione funziona in genere l'accesso a ogni proprietà in un'istanza di un tipo. Accesso alle proprietà attiva il caricamento lazy e le entità di caricamento lazy vengono serializzate. Il processo di serializzazione accede quindi a ogni proprietà delle entità caricamento lazy, causando potenzialmente maggiore serializzazione e il caricamento lazy. Per evitare questa reazione a catena runaway, attivare la disattivazione del caricamento prima che la serializzazione di un'entità lazy.

Classe del contesto di database esegue il caricamento lazy per impostazione predefinita. Esistono due modi per disabilitare il caricamento lazy:

- Per le proprietà di navigazione specifici, omettere il `virtual` (parola chiave) quando si dichiara la proprietà.
- Per tutte le proprietà di navigazione, impostare `LazyLoadingEnabled` a `false`. Ad esempio, è possibile inserire il codice seguente nel costruttore della classe di contesto: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Caricamento lazy in grado di mascherare il codice che causa problemi di prestazioni. Codice che non consente di specificare il caricamento eager o esplicito ma elabora un'elevata quantità di entità e utilizzate varie proprietà di navigazione in ogni iterazione, ad esempio, potrebbe essere molto inefficiente (a causa dei numerosi round trip al database). Un'applicazione che esegue anche in fase di sviluppo utilizzando un server SQL locale può verificarsi dei problemi di prestazioni quando è stato spostato in Database SQL di Azure a causa di un aumento della latenza e il caricamento lazy. Profilatura delle query di database con un carico di test realistici consentirà di determinare se il caricamento lazy è appropriato. Per ulteriori informazioni vedere [Demistificazione delle strategie di Entity Framework: il caricamento di dati correlati](https://msdn.microsoft.com/magazine/hh205756.aspx) e [tramite Entity Framework per ridurre la latenza di rete a SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Creare il nome del reparto consente di visualizzare una pagina di indice corsi

Il `Course` entità include proprietà di navigazione che contiene il `Department` entità del corso assegnato al reparto. Per visualizzare il nome del reparto assegnato in un elenco dei corsi, è necessario ottenere il `Name` proprietà il `Department` entità che la `Course.Department` proprietà di navigazione.

Creare un controller denominato `CourseController` per il `Course` tipo di entità, con le stesse opzioni che è stato fatto in precedenza per il `Student` controller, come illustrato nella figura seguente (tranne che a differenza dell'immagine, la classe di contesto nello spazio dei nomi DAL, non i modelli dello spazio dei nomi):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Aprire *Controllers\CourseController.cs* ed esaminare il `Index` metodo:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Lo scaffolding automatico ha specificato il caricamento eager per la proprietà di navigazione `Department` tramite il metodo `Include`.

Aprire *Views\Course\Index.cshtml* e sostituire il codice esistente con il codice seguente. Le modifiche sono evidenziate:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Al codice con scaffolding sono state apportate le modifiche seguenti:

- Modificare il titolo da **indice** a **corsi**.
- Spostare i collegamenti di riga a sinistra.
- Aggiunta di una colonna sotto l'intestazione **numero** che mostra il `CourseID` valore della proprietà. (Per impostazione predefinita, le chiavi primarie non sono scaffolding perché in genere sono utilizzati per gli utenti finali. Tuttavia, in questo caso la chiave primaria è significativa e si desidera visualizzarlo.)
- All'ultima intestazione di colonna da modificare **DepartmentID** (il nome della chiave esterna per il `Department` entità) per **reparto**.

Si noti che per l'ultima colonna, il codice di scaffolding Visualizza il `Name` proprietà del `Department` entità che viene caricato il `Department` proprietà di navigazione:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Eseguire la pagina (selezionare il **corsi** scheda nella home page University di Contoso) per visualizzare l'elenco con i nomi di reparto.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Creare una pagina di indice istruttori che mostra i corsi e le registrazioni

In questa sezione viene creata un controller e visualizzare per il `Instructor` entità per visualizzare la pagina di indice istruttori:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Questa pagina legge e visualizza dati correlati nei modi seguenti:

- L'elenco di istruttori vengono visualizzati i dati correlati di `OfficeAssignment` entità. Tra le entità `Instructor` e `OfficeAssignment` c'è una relazione uno-a-zero-o-uno. Si userà il caricamento immediato per la `OfficeAssignment` entità. Come spiegato in precedenza, il caricamento eager è in genere più efficiente quando sono necessari i dati correlati per tutte le righe recuperate della tabella primaria. In questo caso, si vogliono visualizzare le assegnazioni di ufficio per tutti gli insegnanti visualizzati.
- Quando l'utente seleziona un docente, correlato `Course` vengono visualizzate le entità. Tra le entità `Instructor` e `Course` esiste una relazione molti-a-molti. Si userà il caricamento immediato per la `Course` entità e le relative `Department` entità. In questo caso, il caricamento lazy potrebbe essere più efficiente perché è necessario corsi solo per istruttore selezionato. Questo esempio, tuttavia, illustra come usare il caricamento eager per le proprietà di navigazione con entità esse stesse all'interno di proprietà di navigazione.
- Quando l'utente seleziona un corso, i dati da relativi il `Enrollments` viene visualizzato il set di entità. Tra le entità `Course` e `Enrollment` esiste una relazione uno-a-molti. Si aggiungerà il caricamento esplicito per `Enrollment` entità e le relative `Student` entità. (Caricamento esplicito non è necessario perché il caricamento lazy è abilitato, ma viene illustrato come eseguire il caricamento esplicito.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Creare un modello di visualizzazione per la visualizzazione dell'indice Instructor

La pagina di indice istruttore mostra tre tabelle diverse. Verrà quindi creato un modello di visualizzazione che includa tre proprietà, ognuna contenente i dati di una delle tabelle.

Nel *ViewModel* cartella, creare *InstructorIndexData.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Aggiunta di uno stile per le righe selezionate

Per contrassegnare le righe selezionate, è necessario un colore di sfondo diversi. Per fornire uno stile per questa interfaccia, aggiungere il codice evidenziato di seguito nella sezione `/* info and errors */` in *Content\Site.css*, come illustrato di seguito:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Creazione del Controller Instructor e viste

Creare un `InstructorController` controller come illustrato nella figura seguente:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Aprire *Controllers\InstructorController.cs* e aggiungere un `using` istruzione per il `ViewModels` dello spazio dei nomi:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Il codice scaffolding di `Index` metodo specifica caricamento eager solo per il `OfficeAssignment` proprietà di navigazione:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Sostituire il `Index` (metodo) con il codice seguente per caricare altri dati correlati e inserirlo nel modello di visualizzazione:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Il metodo accetta i dati della route facoltativo (`id`) e un parametro di stringa di query (`courseID`) che forniscono i valori di ID del corso selezionato e istruttore selezionato e tutti i dati richiesti passa alla visualizzazione. I parametri sono forniti dai collegamenti ipertestuali **Select** (Seleziona) nella pagina.

> [!TIP]
> 
> **Dati sull'itinerario**
> 
> Dati di route sono presenti il gestore di associazione del modello in un segmento di URL specificato nella tabella di routing. Ad esempio, la route predefinita specifica `controller`, `action`, e `id` segmenti:
> 
> routes.MapRoute(  
>  nome: "Default",  
>  url: "{controller}/{action}/{id}",  
>  impostazioni predefinite: new {controller = "Home" action = "Index", id = UrlParameter.Optional}  
> );
> 
> L'URL seguente, la route predefinita viene eseguito il mapping `Instructor` come il `controller`, `Index` come il `action` e 1 come il `id`; si tratta di valori di dati di route.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID = 2021" è un valore di stringa di query. Lo strumento di associazione del modello funziona anche se si passa il `id` come valore di stringa di query:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Vengono creati gli URL `ActionLink` istruzioni nella visualizzazione Razor. Nel codice seguente, il `id` parametro corrisponde la route predefinita, pertanto `id` viene aggiunto ai dati di route.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> Nel codice seguente, `courseID` non corrisponde a un parametro di route predefinita, pertanto viene aggiunta come una stringa di query.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


Il codice inizia creando un'istanza del modello di visualizzazione e inserendola nell'elenco degli insegnanti. Il codice specifica per il caricamento immediato di `Instructor.OfficeAssignment` e `Instructor.Courses` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

Il secondo `Include` metodo carica corsi e ogni corso caricato caricamento eager quanto avviene per il `Course.Department` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Come accennato in precedenza, il caricamento non differito non è obbligatorio ma è lo scopo di migliorare le prestazioni. Poiché la vista è sempre necessario il `OfficeAssignment` entità, risulta più efficiente recuperare che nella stessa query. `Course` le entità sono necessarie quando un docente è selezionato nella pagina web è migliore il caricamento lazy solo se la pagina viene visualizzata più spesso con un corso selezionato rispetto senza caricamento eager.

Se è stato selezionato un ID istruttore istruttore selezionato viene recuperato dall'elenco di istruttori nel modello di visualizzazione. Il modello di visualizzazione `Courses` proprietà viene quindi caricata con le `Course` entità da tale istruttore `Courses` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Il `Where` metodo restituisce una raccolta, ma in questo caso i criteri di passati al risultato in un solo metodo `Instructor` la restituzione di entità. Il `Single` metodo converte la raccolta in un unico `Instructor` entità, che consente di accedere a tale entità `Courses` proprietà.

Utilizzare il [singolo](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) metodo in una raccolta quando si conosce la raccolta ha un solo elemento. Il `Single` metodo genera un'eccezione se la raccolta passata a esso è vuota o se è presente più di un elemento. In alternativa, è [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), che restituisce un valore predefinito (`null` in questo caso) se la raccolta è vuota. Tuttavia, in questo caso che ancora comporta un'eccezione (di tentare di trovare un `Courses` proprietà in un `null` riferimento), e il messaggio di eccezione meno chiaramente indica la causa del problema. Quando si chiama il `Single` (metodo), è inoltre possibile passare il `Where` condizione anziché chiamare il `Where` metodo separatamente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Invece di:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Se è stato selezionato un corso, questo viene quindi recuperato dall'elenco dei corsi nel modello di visualizzazione. Del quindi il modello di visualizzazione `Enrollments` proprietà viene caricata con le `Enrollment` entità da quel corso `Enrollments` proprietà di navigazione.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Modifica la visualizzazione dell'indice Instructor

In *Views\Instructor\Index.cshtml*, sostituire il codice esistente con il codice seguente. Le modifiche sono evidenziate:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Al codice esistente sono state apportate le modifiche seguenti:

- La classe del modello è stata modificata in `InstructorIndexData`.
- Il titolo pagina è stato modificato da **Index** (Indice) a **Instructors** (Insegnanti).
- Spostare le colonne di collegamento di riga a sinistra.
- Rimuovere il **FullName** colonna.
- Aggiungere un **Office** colonna che visualizza `item.OfficeAssignment.Location` solo se `item.OfficeAssignment` non è null. (Poiché si tratta di una relazione uno-a-zero-o-uno, potrebbe non essere disponibile un processo di `OfficeAssignment` entità.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Il codice aggiunto che verrà aggiunto in modo dinamico `class="selectedrow"` per il `tr` elemento istruttore selezionato. Consente di impostare un colore di sfondo per la riga selezionata utilizzando la classe CSS che creato in precedenza. (Il `valign` attributo sarà utile per l'esercitazione seguente quando si aggiunge una colonna con più righe alla tabella.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Aggiunta di un nuovo `ActionLink` etichetta **selezionare** immediatamente prima di altri collegamenti in ogni riga, che comporta l'ID istruttore selezionato da inviare al `Index` metodo.

Eseguire l'applicazione e selezionare il **i docenti** scheda. Nella pagina vengono visualizzati il `Location` proprietà correlate `OfficeAssignment` entità e una tabella vuota cella quando è no correlato `OfficeAssignment` entità.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Nel *Views\Instructor\Index.cshtml* file, dopo la chiusura `table` elemento (alla fine del file), aggiungere il codice evidenziato di seguito. Consente di visualizzare un elenco di corsi relativi a un docente quando è selezionato un docente.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Questo codice legge la proprietà `Courses` del modello di visualizzazione per visualizzare l'elenco dei corsi. Fornisce inoltre un `Select` collegamento ipertestuale che invia l'ID del corso selezionato per il `Index` metodo di azione.

> [!NOTE]
> Il *CSS* file è memorizzato nella cache dal browser. Se le modifiche non visualizzata quando si esegue l'applicazione, si aggiorna un disco rigido (tenere premuto il tasto CTRL mentre si fa clic il **aggiornamento** , oppure premere CTRL + F5).


Eseguire la pagina e selezionare un docente. È ora possibile vedere una griglia con i corsi assegnati all'insegnante selezionato. Per ogni corso è possibile vedere il nome del dipartimento assegnato.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Dopo il blocco di codice appena aggiunto, aggiungere il codice seguente. Quando è selezionato un corso, questo codice visualizza l'elenco degli studenti iscritti al corso selezionato.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Questo codice legge il `Enrollments` proprietà del modello di visualizzazione per visualizzare un elenco di studenti iscritti in corso.

Eseguire la pagina e selezionare un docente. Selezionare quindi un corso per visualizzare l'elenco degli studenti iscritti e i voti corrispondenti.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Aggiunta di caricamento esplicito

Aprire *InstructorController.cs* e osservare come la `Index` metodo ottiene l'elenco delle registrazioni per un corso selezionato:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Quando è stato recuperato l'elenco di istruttori, specificato per il caricamento immediato di `Courses` proprietà di navigazione e per il `Department` proprietà di ogni corso. Inserire il `Courses` insieme nel modello di visualizzazione e l'ora si accede di `Enrollments` da un'entità nella raccolta di proprietà di navigazione. Poiché non è stato specificato per il caricamento non differito il `Course.Enrollments` proprietà di navigazione, i dati da tale proprietà viene visualizzato nella pagina in seguito al caricamento lazy.

Se è stato disabilitato il caricamento lazy senza modificare il codice in altro modo, il `Enrollments` proprietà sarà null indipendentemente dal numero le registrazioni il corso ha effettivamente. In questo caso, per caricare il `Enrollments` proprietà, è necessario specificare il caricamento eager o caricamento esplicito. Già visto come eseguire l'operazione di caricamento eager. Per visualizzare un esempio di caricamento esplicito, sostituire il `Index` metodo con il codice seguente, che carica in modo esplicito il `Enrollments` proprietà. Il codice modificato vengono evidenziati.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Dopo aver ottenuto il `Course` entità, il nuovo codice carica in modo esplicito tale corso `Enrollments` proprietà di navigazione:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Successivamente viene caricata in modo esplicito ogni `Enrollment` collegati entità `Student` entità:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Si noti che si utilizzano il `Collection` per caricare una proprietà di raccolta, ma per una proprietà che contiene una sola entità, utilizzare il `Reference` metodo. È possibile eseguire ora la pagina di indice Instructor e non noterai alcuna differenza di ciò che viene visualizzato nella pagina, anche se hai modificato la modalità di recupero dei dati.

## <a name="summary"></a>Riepilogo

Ora utilizzati tutti i tre modi (Lazywriter, eager ed espliciti) per caricare i dati correlati nelle proprietà di navigazione. Nella prossima esercitazione si apprenderà come aggiornare i dati correlati.

Collegamenti ad altre risorse di Entity Framework, vedere il [mappa del contenuto ASP.NET dati accesso](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Successivo](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
