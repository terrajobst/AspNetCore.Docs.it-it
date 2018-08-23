---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Advanced scenari di Entity Framework per un'applicazione Web MVC (10 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5fa6ff439790f70cd60d0985a791df26d69c7107
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837394"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Scenari avanzati Entity Framework per un'applicazione Web MVC (10 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni a partire dall'inizio oppure [scarica un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si verifica un problema è possibile risolvere, [Scarica il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. Confrontando il codice per il codice completo è generalmente possibile trovare la soluzione al problema. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente è implementato il repository di modelli e unità di lavoro. Questa esercitazione illustra gli argomenti seguenti:

- Esecuzione di query SQL non elaborate.
- Esecuzione di query senza registrazione.
- Esaminare le query inviate al database.
- Uso delle classi proxy.
- Disabilitazione del rilevamento automatico delle modifiche.
- La disabilitazione della convalida durante il salvataggio di modifiche.
- [Gli errori e alternative di lavoro](#errors)

Per la maggior parte degli argomenti seguenti, si utilizzeranno le pagine in cui è già stato creato. Per usare SQL non elaborate per effettuare aggiornamenti bulk si creerà una nuova pagina che aggiorna il numero di crediti di tutti i corsi nel database:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

E usare una query senza registrazione che si aggiungerà la nuova logica di convalida alla pagina Department Edit:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Query SQL non elaborate

L'API di Entity Framework Code First include metodi che consentono di passare i comandi SQL direttamente al database. Sono disponibili le seguenti opzioni:

- Usare il metodo `DbSet.SqlQuery` per le query che restituiscono tipi di entità. Gli oggetti restituiti devono essere del tipo previsto per il `DbSet` oggetto che vengono automaticamente registrate dal contesto del database a meno che non è disattivata la registrazione. (Vedere la sezione seguente `AsNoTracking` (metodo).)
- Usare il `Database.SqlQuery` metodo per le query che restituiscono tipi non entità. I dati restituiti non vengono registrati dal contesto di database, anche se il metodo viene usato per recuperare i tipi di entità.
- Usare la [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) per i comandi non query.

Uno dei vantaggi dell'utilizzo di Entity Framework è la mancanza di un collegamento troppo stretto del codice a un particolare metodo di archiviazione dei dati. Le query e i comandi SQL vengono generati automaticamente e non è necessario scriverli. Ma esistono alcuni scenari particolari in cui è necessario eseguire query SQL specifiche create manualmente e questi metodi consentono di gestire tali eccezioni.

Come avviene quando si eseguono comandi SQL in un'applicazione Web, è necessario adottare delle precauzioni per proteggere il sito dagli attacchi SQL injection. A questo scopo è possibile usare query parametrizzate per assicurarsi che le stringhe inviate da una pagina Web non possano essere interpretate come comandi SQL. In questa esercitazione verranno usate query parametrizzate quando l'input dell'utente viene integrato in una query.

### <a name="calling-a-query-that-returns-entities"></a>Chiamata di una Query che restituisce le entità

Si supponga che il `GenericRepository` classe per fornire operazioni aggiuntive di filtro e ordinamento di flessibilità senza richiedere che crei una classe derivata con metodi aggiuntivi. Un modo per ottenere questo risultato, è possibile aggiungere un metodo che accetta una query SQL. È quindi possibile specificare qualsiasi tipo di filtro o ordinamento è desiderata nel controller, ad esempio un `Where` clausola che dipende da un join o una sottoquery. In questa sezione verrà illustrato come implementare questo metodo.

Creare il `GetWithRawSql` aggiungendo il codice seguente al metodo *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

Nelle *CourseController.cs*, chiamare il nuovo metodo dal `Details` metodo, come illustrato nell'esempio seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

In questo caso è possibile utilizzare il `GetByID` metodo, ma si usa il `GetWithRawSql` metodo per verificare che il `GetWithRawSQL` funzionamento del metodo.

Eseguire la pagina dei dettagli per verificare che l'istruzione select delle query (selezionare il **Course** scheda e quindi **dettagli** per un corso).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Chiamata di una Query che restituisce altri tipi di oggetti

In precedenza è stata creata una griglia delle statistiche degli studenti per la pagina About che visualizza il numero di studenti per ogni data di registrazione. Il codice che esegue questa operazione nel *HomeController.cs* Usa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Si supponga di che voler scrivere il codice che recupera i dati direttamente in SQL, anziché usare LINQ. Per farlo è necessario eseguire una query che restituisce un valore diverso da oggetti entità, ovvero è necessario usare il `Database.SqlQuery` (metodo).

Nelle *HomeController.cs*, sostituire l'istruzione LINQ nel `About` metodo con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Eseguire la pagina di informazioni. Vengono visualizzati gli stessi dati visualizzati in precedenza.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>La chiamata a una Query di aggiornamento

Si supponga che gli amministratori di Contoso University vogliono essere in grado di eseguire modifiche di massa nel database, ad esempio modificare il numero di crediti per ogni corso. Se il numero di corsi dell'università è elevato, potrebbe non essere utile recuperarli tutti come entità e modificarli singolarmente. In questa sezione è possibile implementare una pagina web che consente all'utente di specificare un fattore in base a cui si desidera modificare il numero di crediti per tutti i corsi e viene apportata la modifica tramite l'esecuzione di un database SQL `UPDATE` istruzione. La pagina Web apparirà come segue:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Nell'esercitazione precedente è usato il repository generico per leggere e aggiornare `Course` entities nel `Course` controller. Per questa operazione di aggiornamento bulk, è necessario creare un nuovo metodo di repository che non si trova nel repository generico. A tale scopo, si creerà un oggetto dedicato `CourseRepository` classe che deriva dal `GenericRepository` classe.

Nel *DAL* cartella, creare *CourseRepository.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

Nelle *UnitOfWork.cs*, modificare il `Course` tipo di repository da `GenericRepository<Course>` a `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

Nelle *CourseContoller.cs*, aggiungere un `UpdateCourseCredits` metodo:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Questo metodo verrà utilizzato per entrambi `HttpGet` e `HttpPost`. Quando la `HttpGet` `UpdateCourseCredits` viene eseguito il metodo di `multiplier` variabile sarà null e la visualizzazione verrà visualizzata una casella di testo vuoto e un pulsante di invio, come illustrato nella figura precedente.

Quando la **Update** pulsante e il `HttpPost` viene eseguito il metodo `multiplier` avrà il valore immesso nella casella di testo. Il codice chiama quindi il repository `UpdateCourseCredits` metodo, che restituisce il numero di righe interessate, e tale valore viene archiviato nel `ViewBag` oggetto. Quando la visualizzazione riceve il numero di righe interessate nella `ViewBag` dell'oggetto, viene visualizzato tale numero anziché la casella di testo e inviare pulsante, come illustrato nella figura seguente:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Creare una vista nel *Views\Course* cartella per la pagina Update Course Credits:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

Nelle *Views\Course\UpdateCourseCredits.cshtml*, sostituire il codice del modello con il codice seguente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Eseguire il metodo `UpdateCourseCredits` selezionando la scheda **Courses**, quindi aggiungendo "/UpdateCourseCredits" alla fine dell'URL nella barra degli indirizzi del browser (ad esempio: `http://localhost:50205/Course/UpdateCourseCredits`). Immettere un numero nella casella di testo:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Fare clic su **Aggiorna**. Viene visualizzato il numero di righe interessate:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Fare clic su **Torna all'elenco** per visualizzare l'elenco dei corsi con il numero di crediti modificato.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Per altre informazioni sulle query SQL non elaborate, vedere [query SQL non elaborate](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) nel blog del team di Entity Framework.

## <a name="no-tracking-queries"></a>senza rilevamento delle modifiche

Quando un contesto di database recupera righe di database e crea oggetti entità che li rappresentano, per impostazione predefinita consente di determinare se le entità in memoria sono sincronizzate con ciò che si trova nel database. I dati in memoria svolgono la funzione di una cache e vengono usati per l'aggiornamento di un'entità. Questa memorizzazione nella cache spesso non è necessaria in un'applicazione Web poiché le istanze del contesto hanno spesso una durata breve (viene creata ed eliminata una nuova istanza per ogni richiesta) e il contesto che legge un'entità viene in genere eliminato prima che l'entità venga riutilizzata.

È possibile specificare se il contesto tiene traccia degli oggetti entità per una query usando il `AsNoTracking` (metodo). Gli scenari tipici in cui viene disabilitata la registrazione includono i seguenti:

- La query recupera tali una grande quantità di dati che potrebbero migliorare notevolmente le prestazioni disattivazione del rilevamento.
- Si vuole collegare un'entità per aggiornarla, ma è recuperato in precedenza alla stessa entità per uno scopo diverso. Poiché l'entità viene già registrata dal contesto di database, non è possibile collegare l'entità che si vuole modificare. Un modo per evitare questa situazione consiste nell'utilizzare il `AsNoTracking` opzione con la query precedente.

In questa sezione è possibile implementare logica di business che illustra il secondo di questi scenari. In particolare, si sarà applicare una regola business che afferma che un insegnante non può essere l'amministratore di più di un reparto.

Nelle *DepartmentController.cs*, aggiungere un nuovo metodo che è possibile chiamare dalle `Edit` e `Create` metodi per verificare che nessuna due reparti abbiano lo stesso amministratore:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Aggiungere il codice nel `try` blocco la `HttpPost` `Edit` metodo da chiamare questo nuovo metodo se non sono presenti errori di convalida. Il `try` blocco avrà ora un aspetto simile al seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Eseguire la pagina Department Edit e provare a modificare l'amministratore del reparto per un insegnante che ha già l'amministratore di un reparto diverso. Viene visualizzato il messaggio di errore previsto:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Ora eseguire nuovamente la pagina Department Edit e questa modifica ora il **Budget** quantità. Quando fa clic su **salvare**, viene visualizzata una pagina di errore:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Il messaggio di errore di eccezione è "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" Ciò dovuto la sequenza di eventi seguente:

- Il `Edit` chiamate al metodo il `ValidateOneAdministratorAssignmentPerInstructor` metodo, che recupera tutti i reparti che dispongono di Kim Abercrombie come amministratore. Che fa sì che il reparto English da leggere. Poiché questo è il reparto in fase di modifica, viene segnalato alcun errore. In seguito a questa operazione di lettura, tuttavia, l'entità department in lingua inglese che è stato letto dal database è ora viene registrata dal contesto del database.
- Il `Edit` metodo prova a impostare il `Modified` flag su inglese entità department creato per lo strumento di associazione di modelli MVC, ma che non riesce perché il contesto sta già rilevando un'entità per il reparto English.

Per risolvere questo problema è mantenere il contesto da entità department in memoria recuperate dalla query di convalida con rilevamento. Non è disponibile alcun svantaggio di questa operazione, perché è non verrà aggiornata l'entità o leggerlo nuovamente in modo che risulta vantaggiosa per la memorizzazione nella cache in memoria.

Nelle *DepartmentController.cs*, nella `ValidateOneAdministratorAssignmentPerInstructor` (metodo), non specificare alcun rilevamento, come illustrato nell'esempio seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Ripetere il tentativo di modificare la **Budget** quantità di un reparto. Questa volta l'operazione ha esito positivo e il sito restituisce come previsto per la pagina Departments Index con il valore di budget rivisto.

## <a name="examining-queries-sent-to-the-database"></a>Esaminare le query inviate al Database

A volte può essere utile visualizzare le query SQL inviate al database. A tale scopo, è possibile esaminare una variabile di query nel debugger o la query denominata `ToString` (metodo). Per provare questo timeout, verrà esaminare una semplice query e quindi esaminare che cosa succede quando si aggiungono le opzioni di questo tipo eager il caricamento, filtro e ordinamento.

Nelle *controller/CourseController*, sostituire il `Index` metodo con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Ora impostare un punto di interruzione nella *GenericRepository.cs* nel `return query.ToList();` e il `return orderBy(query).ToList();` rapporti del `Get` (metodo). Eseguire il progetto in modalità di debug e selezionare la pagina di indice del corso. Quando il codice raggiunge il punto di interruzione, esaminare il `query` variabile. Viene visualizzata la query che viene inviata a SQL Server. È una semplice `Select` istruzione:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Le query possono essere troppo lunghe da visualizzare nelle finestre di debug in Visual Studio. Per visualizzare l'intera query, è possibile copiare il valore della variabile e incollarlo in un editor di testo:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Ora si aggiungerà un elenco di riepilogo a discesa alla pagina di indice di corsi in modo che gli utenti possono filtrare per un reparto specifico. Si ordinerà i corsi per titolo, e si specificano il caricamento eager per la `Department` proprietà di navigazione. Nelle *CourseController.cs*, sostituire il `Index` metodo con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

Il metodo riceve il valore dell'elenco di riepilogo a discesa selezionato il `SelectedDepartment` parametro. Se è stata eseguita alcuna selezione, questo parametro sarà null.

Oggetto `SelectList` insieme contenente tutti i reparti è passato alla visualizzazione per l'elenco a discesa. I parametri passati al `SelectList` costruttore specificare il nome del campo valore, il nome del campo di testo e l'elemento selezionato.

Per il `Get` metodo per il `Course` repository, il codice specifica un'espressione di filtro, ordinamento e il caricamento eager per la `Department` proprietà di navigazione. L'espressione restituisce sempre `true` se è stata eseguita alcuna selezione nell'elenco a discesa scegliere (vale a dire `SelectedDepartment` è null).

Nelle *Views\Course\Index.cshtml*, immediatamente prima dell'apertura `table` tag, aggiungere il codice seguente per creare l'elenco a discesa e un pulsante di invio:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Con i punti di interruzione ancora impostate `GenericRepository` (classe), eseguire la pagina di indice del corso. Continuare con i primi due volte che il codice raggiunge un punto di interruzione, in modo che la pagina viene visualizzata nel browser. Selezionare un reparto dall'elenco a discesa e fare clic su **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Questa volta sarà il primo punto di interruzione per la query di reparti per l'elenco a discesa. Ignora che e visualizzare il `query` variabile la volta successiva che il codice raggiunge il punto di interruzione per vedere quali il `Course` query dovrebbe ora apparire così. Si otterrà un risultato simile al seguente:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Si noterà che la query è ora un `JOIN` query che consente di caricare `Department` insieme ai dati di `Course` dati e che includa un `WHERE` clausola.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Uso delle classi Proxy

Quando Entity Framework crea istanze di entità (ad esempio, quando si esegue una query), vengono spesso creati come istanze di un tipo derivato generata dinamicamente che funge da proxy per l'entità. Questo proxy esegue l'override di alcune proprietà dell'entità da inserire hook per eseguire automaticamente azioni quando si accede alla proprietà virtuale. Ad esempio, questo meccanismo viene usato per supportare il caricamento lazy di relazioni.

La maggior parte dei casi non è necessario essere a conoscenza di questo utilizzo di proxy, ma sono presenti eccezioni:

- In alcuni scenari si potrebbe voler impedire la creazione di istanze del proxy di Entity Framework. La serializzazione di istanze non proxy, ad esempio, potrebbe essere più efficiente rispetto alla serializzazione di istanze del proxy.
- Quando si crea un'istanza di una classe di entità mediante il `new` operatore, non si ottiene un'istanza del proxy. Ciò significa che non si ottengono funzionalità, ad esempio il caricamento lazy e automatico il rilevamento delle modifiche. Si tratta in genere bene; in genere non è necessario il caricamento lazy, perché si sta creando una nuova entità che non è presente nel database, e in genere non occorre rilevamento delle modifiche se si sta contrassegnare in modo esplicito l'entità come `Added`. Tuttavia, se è necessario il caricamento lazy ed è necessario il rilevamento delle modifiche, è possibile creare nuove istanze dell'entità con proxy usando le `Create` metodo del `DbSet` classe.
- È possibile ottenere un tipo di entità effettivo da un tipo di proxy. È possibile usare la `GetObjectType` metodo di `ObjectContext` classe per ottenere il tipo di entità effettivo di un'istanza del tipo proxy.

Per altre informazioni, vedere [uso di proxy di](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) nel blog del team di Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Disabilitare il rilevamento automatico delle modifiche

Entity Framework determina come è stata modificata un'entità (e di conseguenza gli aggiornamenti da inviare al database) confrontando i valori correnti di un'entità con i valori originali. Quando l'entità è stata eseguita una query o collegato, vengono archiviati i valori originali. I metodi che causano il rilevamento automatico delle modifiche includono:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Se si desidera monitorare un numero elevato di entità e si chiama uno di questi metodi molte volte in un ciclo, è possibile ottenere miglioramenti significativi delle prestazioni disattivando temporaneamente il rilevamento automatico delle modifiche tramite il [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) proprietà. Per altre informazioni, vedere [rilevare automaticamente le modifiche](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>La disabilitazione della convalida durante il salvataggio di modifiche

Quando si chiama il `SaveChanges` (metodo), per impostazione predefinita Entity Framework convalida i dati in tutte le proprietà di tutte le entità modificate prima di aggiornare il database. Se è stato aggiornato un numero elevato di entità e aver già convalidato i dati, questa operazione è necessaria e far sì che il processo di salvataggio le modifiche diventano meno tempo disattivando temporaneamente la convalida. È possibile farlo tramite il [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) proprietà. Per altre informazioni, vedere [convalida](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Riepilogo

Questa serie di esercitazioni sull'uso di Entity Framework in un'applicazione ASP.NET MVC è stata completata. Sono disponibili collegamenti ad altre risorse di Entity Framework nel [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

Per altre informazioni su come distribuire l'applicazione web dopo aver creato, vedere [mappa del contenuto ASP.NET distribuzione](https://msdn.microsoft.com/library/bb386521.aspx) in MSDN Library.

Per informazioni su altri argomenti correlati a MVC, ad esempio l'autenticazione e autorizzazione, vedere la [risorse consigliate per MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Ringraziamenti

- Tom Dykstra ha scritto la versione originale di questa esercitazione ed è una scrittrice senior di programmazione in Microsoft Web Platform and Tools Team del contenuto.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) co-autore di questa esercitazione e ha la maggior parte del lavoro aggiornarla per Entity Framework 5 e MVC 4. Rick è una scrittrice senior di programmazione per Microsoft che si occupa in Azure e MVC.
- [Rowan Miller](http://www.romiller.com) e altri membri del team di Entity Framework assistito con revisioni del codice e il debug dei molti problemi con le migrazioni che si è verificato mentre si stava aggiornando l'esercitazione per Entity Framework 5.

## <a name="vb"></a>VB

Quando è stato originariamente creata nell'esercitazione, abbiamo fornito le versioni di c# e VB del progetto di download completato. Con questo aggiornamento forniamo un progetto scaricabile c# per ciascun capitolo per renderne più semplice iniziare a usare un punto qualsiasi della serie, ma a causa di limitazioni di tempo e altre priorità non ciò che abbiamo fatto per VB. Se si compila un progetto di Visual Basic usando queste esercitazioni e sono disposti a condividere con altri utenti, è possibile comunicarlo.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Gli errori e soluzioni alternative

### <a name="cannot-createshadow-copy"></a>Non è possibile creare/shadow copia

Messaggio di errore:

*Non è possibile creare/shadow copia 'DotNetOpenAuth.OpenId' se il file esiste già.*

Soluzione:

Attendere alcuni secondi e aggiornare la pagina.

### <a name="update-database-not-recognized"></a>Update-Database non riconosciuto

Messaggio di errore:

*Il termine "Update-Database" non è riconosciuto come il nome di un cmdlet, funzione, file di script o programma eseguibile. Controllare l'ortografia del nome, o se è stato incluso un percorso, verificare che il percorso sia corretto e riprovare.* (Dal *`Update-Database`* comando nella console di gestione pacchetti.)

Soluzione:

Uscire da Visual Studio. Riaprire progetto e riprovare.

### <a name="validation-failed"></a>Convalida non riuscita

Messaggio di errore:

*Convalida non riuscita per uno o più entità. Vedere la proprietà 'EntityValidationErrors' per altri dettagli.* (Dal *`Update-Database`* comando nella console di gestione pacchetti.)

Soluzione:

Una delle cause di questo problema è errori di convalida quando il `Seed` esecuzione del metodo. Visualizzare [Seeding e al debug di Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) per suggerimenti sul debug di `Seed` (metodo).

### <a name="http-50019-error"></a>HTTP 500.19 errore

Messaggio di errore:

*Errore HTTP 500.19 - errore interno del Server alla pagina richiesta non è accessibile perché i dati di configurazione per la pagina non sono validi.*

Soluzione:

È possibile ottenere questo errore è da più copie della soluzione, ognuna di esse usando lo stesso numero di porta. È in genere possibile risolvere questo problema, uscire da tutte le istanze di Visual Studio, quindi riavviare il progetto di lavorare su. Se il problema persiste, provare a modificare il numero di porta. Fare clic con il pulsante destro sul file di progetto e quindi fare clic su proprietà. Selezionare il **Web** scheda e quindi cambiare il numero di porta la **Url progetto** casella di testo.

### <a name="error-locating-sql-server-instance"></a>Errore di individuazione dell'istanza di SQL Server

Messaggio di errore:

*Si è verificato un errore relativo alla rete o specifico dell'istanza mentre veniva stabilita una connessione a SQL Server. Il server non è stato trovato o non è accessibile. Verificare che il nome dell'istanza sia corretto e che SQL Server sia configurato per consentire le connessioni remote. (provider: interfacce di rete SQL, errore: 26 - errore nell'individuazione Server / dell'istanza specificati)*

Soluzione:

Controllare la stringa di connessione. Se è stato eliminato manualmente il database, modificare il nome del database nella stringa di costruzione.

> [!div class="step-by-step"]
> [Precedente](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [Successivo](building-the-ef5-mvc4-chapter-downloads.md)
