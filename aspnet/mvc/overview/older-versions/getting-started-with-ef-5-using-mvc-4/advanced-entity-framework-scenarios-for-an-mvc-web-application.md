---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Entity Framework scenari avanzati di per un'applicazione Web MVC (10 di 10) | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d58a745896b29317c1d1049e3bf1a5ec2e628820
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Scenari di avanzate di Entity Framework per un'applicazione Web MVC (10 di 10)
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 utilizzando Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto di avvio per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si esegue un problema, è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. In genere, è possibile trovare la soluzione al problema di mediante un confronto tra il codice per il codice completato. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente è implementato il repository e l'unità di lavoro modelli. In questa esercitazione vengono trattati gli argomenti seguenti:

- Esecuzione di query SQL non elaborata.
- Esecuzione di query di rilevamento di no.
- Esaminare le query inviate al database.
- Utilizzo delle classi proxy.
- Disabilitare il rilevamento automatico delle modifiche.
- Sulla disabilitazione della convalida durante il salvataggio di modifiche.
- [Errori e soluzioni alternative](#errors)

Per la maggior parte degli argomenti seguenti, si utilizzeranno le pagine che già stato creato. Per utilizzare SQL non elaborato per effettuare aggiornamenti bulk si creerà una nuova pagina che aggiorna il numero di crediti di tutti i corsi nel database:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

E per l'utilizzo di una query di rilevamento di no si aggiungerà la nuova logica di convalida per la pagina Modifica reparto:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Eseguendo query SQL non elaborato

L'API di Entity Framework codice prima include metodi che consentono di passare i comandi SQL direttamente al database. Sono disponibili le seguenti opzioni:

- Utilizzare il `DbSet.SqlQuery` metodo per le query che restituiscono tipi di entità. Gli oggetti restituiti devono essere del tipo previsto per il `DbSet` oggetto e vengono rilevate automaticamente dal contesto di database a meno che non si disattiva rilevamento. (Vedere la sezione seguente `AsNoTracking` metodo.)
- Utilizzare il `Database.SqlQuery` metodo per le query che restituiscono tipi non entità. I dati restituiti non sono rilevati dal contesto del database, anche se si utilizza questo metodo per recuperare i tipi di entità.
- Utilizzare il [Database.ExecuteSqlCommand](https://msdn.microsoft.com/en-us/library/gg679456(v=vs.103).aspx) per i comandi non query.

Uno dei vantaggi dell'utilizzo di Entity Framework è che evita abbinata codice posizione troppo ravvicinata a un metodo particolare per l'archiviazione dei dati. Ciò avviene mediante la generazione di comandi e query SQL, che consente inoltre di dover scrivere manualmente. Esistono scenari eccezionali, quando è necessario eseguire query SQL specifiche che è stato creato manualmente, ma questi metodi consentono di gestire tali eccezioni.

Come è sempre true per l'esecuzione di comandi SQL in un'applicazione web, è necessario adottare le precauzioni necessarie per proteggere il sito da attacchi SQL injection. Un modo per eseguire questa operazione consiste nell'utilizzare le query con parametri per assicurarsi che le stringhe inviate da una pagina web non possono essere interpretate come comandi SQL. In questa esercitazione si userà le query con parametri per l'integrazione di input dell'utente in una query.

### <a name="calling-a-query-that-returns-entities"></a>La chiamata di una Query che restituisce le entità

Si supponga di `GenericRepository` classe per fornire ulteriori filtro e ordinamento flessibilità senza creare una classe derivata con metodi aggiuntivi. Un modo per ottenere questo risultato, è possibile aggiungere un metodo che accetta una query SQL. È quindi possibile specificare qualsiasi tipo di filtro o ordinamento si desidera nel controller, ad esempio un `Where` clausola che dipende da un join o una sottoquery. In questa sezione si noterà come implementare questo metodo.

Creare il `GetWithRawSql` aggiungendo il seguente codice al metodo di *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

In *CourseController.cs*, il nuovo metodo da chiamare il `Details` (metodo), come illustrato nell'esempio seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

In questo caso è possibile utilizzare il `GetByID` (metodo), ma si sta utilizzando il `GetWithRawSql` metodo per verificare che il `GetWithRawSQL` funzionamento del metodo.

Eseguire la pagina dei dettagli verificare che la query select mediante works (selezionare il **corso** scheda e quindi **dettagli** per un corso).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>La chiamata di una Query che restituisce gli altri tipi di oggetti

È stato creato in precedenza una griglia delle statistiche di studenti per la pagina di informazioni che è stato illustrato il numero di studenti per ogni data di registrazione. Il codice che esegue questa operazione in *HomeController.cs* utilizza LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Si supponga di che voler scrivere codice che recupera i dati direttamente in SQL anziché l'utilizzo di LINQ. Scopo che è necessario eseguire una query che restituisce un valore diverso da oggetti entità, vale a dire che è necessario utilizzare il `Database.SqlQuery` metodo.

In *HomeController.cs*, sostituire l'istruzione LINQ nel `About` (metodo) con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Eseguire la pagina di informazioni. Visualizza gli stessi dati che non è stato modificato.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>La chiamata di una Query di aggiornamento

Si supponga che gli amministratori University Contoso desiderano essere in grado di eseguire modifiche di massa nel database, ad esempio la modifica del numero di crediti per ogni corso. Università dispone di un numero elevato di corsi, sarebbe opportuno recuperarli tutti come entità e modificarli singolarmente. In questa sezione viene implementato una pagina web che consente all'utente di specificare un fattore in base a cui si desidera modificare il numero di crediti per tutti i corsi e ti la modifica tramite l'esecuzione di un database SQL `UPDATE` istruzione. La pagina web sarà simile al seguente:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Nell'esercitazione precedente il repository generico è utilizzato per leggere e aggiornare `Course` le entità nel `Course` controller. Per questa operazione di aggiornamento in blocco, è necessario creare un nuovo metodo di repository che non è nel repository generico. A tale scopo, si creerà una dedicata `CourseRepository` classe che deriva dalla `GenericRepository` classe.

Nel *DAL* cartella, creare *CourseRepository.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

In *UnitOfWork.cs*, modificare il `Course` il tipo di repository da `GenericRepository<Course>` a`CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

In *CourseContoller.cs*, aggiungere un `UpdateCourseCredits` metodo:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Questo metodo viene utilizzato sia per `HttpGet` e `HttpPost`. Quando il `HttpGet` `UpdateCourseCredits` viene eseguito il metodo di `multiplier` variabile sarà null e la visualizzazione verrà visualizzata una casella di testo e un pulsante di invio, come illustrato nella figura precedente.

Quando il **aggiornamento** si fa clic sul pulsante e `HttpPost` viene eseguito il metodo `multiplier` avrà il valore immesso nella casella di testo. Il codice chiama quindi il repository `UpdateCourseCredits` metodo, che restituisce il numero di righe interessate e tale valore viene archiviato nel `ViewBag` oggetto. Quando la vista riceve il numero di righe interessate nella `ViewBag` dell'oggetto, viene visualizzato il numero anziché la casella di testo e inviare pulsante, come illustrato nella figura seguente:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Creare una visualizzazione di *Views\Course* cartella per la pagina crediti corso di aggiornamento:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

In *Views\Course\UpdateCourseCredits.cshtml*, sostituire il codice del modello con il codice seguente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Eseguire il `UpdateCourseCredits` metodo selezionando il **corsi** scheda, quindi aggiungendo "/ UpdateCourseCredits" alla fine dell'URL nella barra degli indirizzi del browser (ad esempio: `http://localhost:50205/Course/UpdateCourseCredits`). Immettere un numero nella casella di testo:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Fare clic su **Aggiorna**. Visualizzare il numero di righe interessate:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Fare clic su **elenco** per visualizzare l'elenco dei corsi con il numero di crediti modificato.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Per ulteriori informazioni sulle query SQL non elaborate, vedere [query SQL non elaborati](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) nel blog del team di Entity Framework.

## <a name="no-tracking-queries"></a>Query di rilevamento di No

Quando un contesto di database recupera le righe di database e crea gli oggetti di entità che li rappresentano, per impostazione predefinita consente di determinare se le entità in memoria sono sincronizzate con i dati del database. I dati in memoria funge da cache e viene utilizzati quando si aggiorna un'entità. La memorizzazione nella cache non è spesso necessario in un'applicazione web perché contesto istanze sono in genere breve durata (un nuovo uno viene creato o eliminato per ogni richiesta) e il contesto che legge un'entità viene in genere eliminata prima di tale entità viene usato di nuovo.

È possibile specificare se il contesto tiene traccia degli oggetti di entità per una query utilizzando il `AsNoTracking` metodo. Scenari tipici in cui si desideri farlo che includono quanto segue:

- La query recupera un grosso volume di dati che potrebbero migliorare notevolmente le prestazioni di disattivazione del rilevamento.
- Si desidera collegare un'entità per l'aggiornamento, ma è recuperato in precedenza la stessa entità per uno scopo diverso. Poiché l'entità è già rilevato dal contesto di database, è possibile collegare l'entità che si desidera modificare. Un modo per evitare questa situazione consiste nell'utilizzare il `AsNoTracking` opzione con la query precedente.

In questa sezione viene implementato logica di business che illustra la seconda di questi scenari. In particolare, si sarà applicare una regola di business che segnala che un docente non può essere l'amministratore di più di un reparto.

In *DepartmentController.cs*, aggiungere un nuovo metodo che è possibile chiamare dal `Edit` e `Create` metodi per garantire che nessun due reparti abbiano lo stesso amministratore:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Aggiungere il codice nel `try` block del `HttpPost` `Edit` metodo da chiamare questo metodo di nuovo se non sono presenti errori di convalida. Il `try` blocco avrà ora un aspetto analogo al seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Eseguire la pagina Modifica reparto e tenta di modificare l'amministratore di un reparto in un docente che è già l'amministratore di un reparto diverso. Viene visualizzato il messaggio di errore previsto:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

A questo punto, eseguire nuovamente la pagina Modifica di reparto e la modifica ora il **Budget** quantità. Quando fa clic su **salvare**, viene visualizzata una pagina di errore:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Il messaggio di errore di eccezione è "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" Ciò dovuto la sequenza di eventi seguente:

- Il `Edit` chiamate al metodo di `ValidateOneAdministratorAssignmentPerInstructor` metodo che recupera tutti i reparti che dispongono di Kim Abercrombie come amministratore. Che fa sì che il reparto inglese da leggere. Poiché questo è il reparto in fase di modifica, viene segnalato alcun errore. In seguito a questa operazione di lettura, tuttavia, entità reparto inglese che è stato letto dal database è ora rilevato dal contesto del database.
- Il `Edit` metodo tenta di impostare il `Modified` flag inglese entità reparto creato per il gestore di associazione del modello MVC, ma che non riesce perché il contesto sta già rilevando un'entità per il reparto in lingua inglese.

Per risolvere questo problema consiste nel mantenere il contesto da entità reparto in memoria recuperate dalla query la convalida di rilevamento. Non vi è alcun svantaggio di questa operazione, poiché è non aggiorna l'entità o si legge nuovamente in modo che può costituire un vantaggio da quest'ultimo viene memorizzato nella cache in memoria.

In *DepartmentController.cs*nel `ValidateOneAdministratorAssignmentPerInstructor` (metodo), non specificare alcun rilevamento, come illustrato nell'esempio seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Ripetere il tentativo di modificare il **Budget** quantità di un reparto. Questo periodo l'operazione ha esito positivo e il sito restituisce come previsto per la pagina di indice di reparti, visualizzazione del valore di budget rivisto.

## <a name="examining-queries-sent-to-the-database"></a>Esaminare le query inviate al Database

A volte risulta utile essere in grado di visualizzare le query SQL effettive vengono inviate al database. A tale scopo, è possibile esaminare una variabile di query nel debugger o la query denominata `ToString` metodo. Per provare la natura, verrà esaminata una semplice query e quindi osservare cosa accade a esso quando si aggiungono le opzioni di questo tipo eager durante il caricamento, filtro e ordinamento.

In *controller/CourseController*, sostituire il `Index` (metodo) con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Ora impostare un punto di interruzione nella *GenericRepository.cs* sul `return query.ToList();` e `return orderBy(query).ToList();` istruzioni del `Get` metodo. Eseguire il progetto in modalità di debug e selezionare la pagina di indice corso. Quando il codice raggiunge il punto di interruzione, esaminare il `query` variabile. Viene visualizzata la query che viene inviata a SQL Server. È un semplice `Select` istruzione:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Le query possono essere troppo lunghe da visualizzare nelle finestre di debug in Visual Studio. Per visualizzare l'intera query, è possibile copiare il valore della variabile e incollarlo in un editor di testo:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Ora si aggiungerà un elenco a discesa alla pagina di indice corso in modo che gli utenti possono filtrare per un particolare reparto. Si ordinerà i corsi per titolo, e si specificano il caricamento immediato per la `Department` proprietà di navigazione. In *CourseController.cs*, sostituire il `Index` (metodo) con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

Il metodo riceve il valore selezionato dell'elenco di riepilogo a discesa il `SelectedDepartment` parametro. Se non è selezionato, questo parametro sarà null.

Oggetto `SelectList` raccolta contenente tutti i reparti viene passato alla visualizzazione per l'elenco a discesa. I parametri passati al `SelectList` costruttore specificare il nome del campo valore, il nome del campo di testo e l'elemento selezionato.

Per il `Get` metodo il `Course` repository, il codice specifica un'espressione di filtro, ordinamento e il caricamento per eager il `Department` proprietà di navigazione. Restituisce sempre l'espressione di filtro `true` se nell'elenco a discesa viene selezionato nulla (vale a dire `SelectedDepartment` è null).

In *Views\Course\Index.cshtml*, immediatamente prima dell'apertura `table` tag, aggiungere il codice seguente per creare l'elenco a discesa e un pulsante di invio:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Con i punti di interruzione ancora `GenericRepository` (classe), eseguire la pagina di indice corso. Continuare con i primi due volte che il codice raggiunge un punto di interruzione, in modo che la pagina viene visualizzata nel browser. Selezionare un reparto dall'elenco a discesa e fare clic su **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Questa volta sarà il primo punto di interruzione per la query reparti per l'elenco a discesa. Ignorare che e visualizzare il `query` variabile la volta successiva che il codice raggiunge il punto di interruzione per vedere quali il `Course` query ora è simile. Verrà visualizzato qualcosa di simile al seguente:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

È possibile vedere che la query è ora un `JOIN` query che carica `Department` insieme ai dati di `Course` dati e che includa un `WHERE` clausola.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Utilizzo delle classi Proxy

Quando Entity Framework crea istanze di entità (ad esempio, quando si esegue una query), vengono spesso creati come istanze di un tipo derivato in modo dinamico generato che funge da proxy per l'entità. Questo proxy esegue l'override di alcune proprietà dell'entità da inserire hook per eseguire automaticamente azioni quando si accede alla proprietà virtuali. Ad esempio, questo meccanismo viene usato per supportare il caricamento differito di relazioni.

La maggior parte dei casi non è necessario essere a conoscenza di questo utilizzo di proxy, ma esistono alcune eccezioni:

- In alcuni scenari è consigliabile impedire la creazione di istanze di proxy di Entity Framework. Ad esempio, la serializzazione di istanze di proxy non potrebbe essere più efficiente la serializzazione di istanze di proxy.
- Quando si crea un'istanza una classe di entità mediante il `new` (operatore), non si ottiene un'istanza del proxy. Ciò significa che non si ottengono funzionalità, ad esempio rilevamento delle modifiche automatico e il caricamento lazy. Si tratta in genere OK; in genere non è necessario il caricamento lazy, perché si sta creando una nuova entità che non è nel database, e in genere non occorre rilevamento se si sta contrassegnare in modo esplicito l'entità come `Added`. Tuttavia, se è necessario il caricamento lazy ed è necessario il rilevamento delle modifiche, è possibile creare nuove istanze di entità con proxy utilizzando il `Create` metodo la `DbSet` classe.
- È possibile ottenere un tipo di entità effettivo da un tipo proxy. È possibile utilizzare il `GetObjectType` metodo la `ObjectContext` classe per ottenere il tipo di entità effettivo di un'istanza del tipo proxy.

Per ulteriori informazioni, vedere [utilizzo di proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) nel blog del team di Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Disabilitare il rilevamento automatico delle modifiche

Entity Framework determina come è stata modificata un'entità (e pertanto gli aggiornamenti devono essere inviati al database), confrontando i valori correnti di un'entità con i valori originali. Quando l'entità è stata eseguita la query o collegato, vengono archiviati i valori originali. Alcuni dei metodi che causano il rilevamento delle modifiche automatico sono i seguenti:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Se si desidera tenere traccia di un numero elevato di entità e viene chiamato uno di questi metodi più volte in un ciclo, è possibile ottenere miglioramenti significativi delle prestazioni disattivando temporaneamente il rilevamento delle modifiche automatico utilizzando il [AutoDetectChangesEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) proprietà. Per ulteriori informazioni, vedere [automaticamente il rilevamento modifiche](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Sulla disabilitazione della convalida durante il salvataggio di modifiche

Quando si chiama il `SaveChanges` (metodo), per impostazione predefinita di Entity Framework convalida i dati in tutte le proprietà di tutte le entità modificate prima di aggiornare il database. Se è stato aggiornato un numero elevato di entità già stato convalidato i dati, questa operazione è necessaria e far sì che il processo di salvataggio le modifiche avranno meno tempo per disattivare temporaneamente la convalida. È possibile eseguire tale utilizzando il [ValidateOnSaveEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) proprietà. Per ulteriori informazioni, vedere [convalida](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Riepilogo

Questa serie di esercitazioni sull'uso di Entity Framework in un'applicazione MVC ASP.NET è stata completata. Collegamenti ad altre risorse di Entity Framework, vedere il [mappa del contenuto ASP.NET dati accesso](../../../../whitepapers/aspnet-data-access-content-map.md).

Per ulteriori informazioni su come distribuire l'applicazione web dopo aver creato, vedere [mappa del contenuto di distribuzione di ASP.NET](https://msdn.microsoft.com/en-us/library/bb386521.aspx) in MSDN Library.

Per informazioni su altri argomenti relativi a MVC, quali l'autenticazione e autorizzazione, vedere il [risorse consigliato MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Riconoscimenti

- Tom Dykstra ha scritto la versione originale di questa esercitazione ed è un writer di programmazione senior nel Team di contenuto di strumenti e piattaforma Web Microsoft.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) co-autore di questa esercitazione e ha la maggior parte delle operazioni di aggiornamento per Entity Framework 5 e MVC 4. Rick è un writer di programmazione senior per porre l'attenzione su Azure e MVC Microsoft.
- [Miller rowan](http://www.romiller.com) e altri membri del team di Entity Framework assistiti da con le revisioni del codice che ha contribuito debug molti problemi con le migrazioni che si è verificato mentre si stava aggiornando l'esercitazione per Entity Framework 5.

## <a name="vb"></a>VB

Quando è stato originariamente creata nell'esercitazione, è fornito versioni c# e VB del progetto completato il download. Con questo aggiornamento viene fornito un progetto scaricabile c# per ogni capitolo per renderne più semplice iniziare in qualsiasi punto della serie, ma a causa di limitazioni di tempo e altre priorità senza che per VB. Se si compila un progetto VB utilizzando queste esercitazioni e sarebbe disposti per condividerli con altri utenti, Saremmo lieti di sapere.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Errori e soluzioni alternative

### <a name="cannot-createshadow-copy"></a>Impossibile creare/shadow copia

Messaggio di errore:

*Impossibile creare/shadow copia 'DotNetOpenAuth.OpenId' se il file esiste già.*

Soluzione:

Attendere alcuni secondi e aggiornare la pagina.

### <a name="update-database-not-recognized"></a>Update-Database non riconosciuto

Messaggio di errore:

*Il termine 'Update-Database' non è riconosciuto come nome di un cmdlet, funzione, file di script o programma eseguibile. Controllare l'ortografia del nome, o se è stato incluso un percorso, verificare che il percorso sia corretto e riprovare.* (Dal  *`Update-Database`*  comando PMC.)

Soluzione:

Uscire da Visual Studio. Riaprire progetto e riprovare.

### <a name="validation-failed"></a>Convalida non riuscita

Messaggio di errore:

*Convalida non riuscita per uno o più entità. La proprietà 'EntityValidationErrors' per ulteriori informazioni, vedere.* (Dal  *`Update-Database`*  comando PMC.)

Soluzione:

Una delle cause di questo problema è errori di convalida quando la `Seed` esecuzione del metodo. Vedere [Seeding e database di debug di Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) per suggerimenti sul debug di `Seed` metodo.

### <a name="http-50019-error"></a>HTTP 500.19 errore

Messaggio di errore:

*Errore HTTP 500.19 - errore interno del Server alla pagina richiesta non accessibile perché i dati di configurazione per la pagina non sono validi.*

Soluzione:

È possibile ottenere questo errore è da più copie della soluzione, ognuno di essi utilizzando lo stesso numero di porta. In genere, è possibile risolvere questo problema, uscire da tutte le istanze di Visual Studio, quindi riavviare il progetto di lavoro in. Se il problema persiste, provare a modificare il numero di porta. Fare clic con il pulsante destro sul file di progetto e quindi fare clic su proprietà. Selezionare il **Web** scheda e quindi modificare il numero di porta di **Url progetto** casella di testo.

### <a name="error-locating-sql-server-instance"></a>Istanza di SQL Server individuazione errore

Messaggio di errore:

*Si è verificato un errore di rete o specifico dell'istanza durante la connessione a SQL Server. Il server non è stato trovato o non è accessibile. Verificare che il nome dell'istanza sia corretto e che SQL Server è configurato per consentire le connessioni remote. (provider: interfacce di rete SQL, errore: 26 - errore nell'individuazione Server / dell'istanza specificati)*

Soluzione:

Verificare la stringa di connessione. Se è stato eliminato manualmente il database, modificare il nome del database nella stringa di costruzione.

>[!div class="step-by-step"]
[Precedente](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
[Successivo](building-the-ef5-mvc4-chapter-downloads.md)
