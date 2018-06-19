---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Entity Framework 6 scenari avanzati di per un'applicazione Web 5 MVC (12 12) | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Code First di Entity Framework 6 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 50987b30a49173605e7aeb8eb65ff1fe5d5e5753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877450"
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>Avanzate di Entity Framework 6 scenari per un'applicazione Web 5 MVC (12 12)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Visual Studio 2013 e Code First di Entity Framework 6. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nell'esercitazione precedente è implementato ereditarietà tabella per gerarchia. In questa esercitazione include un'introduzione agli argomenti più utili da tenere presenti quando superano le nozioni di base dello sviluppo di applicazioni web ASP.NET che utilizzano Code First di Entity Framework. Istruzioni dettagliate consentono di eseguire il codice e l'utilizzo di Visual Studio per gli argomenti seguenti:

- [L'esecuzione di query SQL non elaborata](#rawsql)
- [L'esecuzione di query di rilevamento no](#notracking)
- [Analisi SQL inviati al database](#sql)

L'esercitazione vengono descritti i diversi argomenti con una breve introduzione seguita da collegamenti alle risorse per altre informazioni:

- [Repository e un'unità di lavoro modelli](#repo)
- [Classi proxy](#proxies)
- [Rilevamento delle modifiche automatico](#changedetection)
- [Convalida automatica](#validation)
- [Strumenti di Entity Framework per Visual Studio](#tools)
- [Codice sorgente di Entity Framework](#source)

L'esercitazione include le sezioni seguenti:

- [Riepilogo](#summary)
- [Riconoscimenti](#acknowledgments)
- [Una nota relativa VB](#vb)
- [Gli errori comuni e le soluzioni per tali](#errors)

Per la maggior parte degli argomenti seguenti, si utilizzeranno le pagine che già stato creato. Per utilizzare SQL non elaborato per effettuare aggiornamenti bulk si creerà una nuova pagina che aggiorna il numero di crediti di tutti i corsi nel database:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Eseguendo query SQL non elaborato

L'API di Entity Framework codice prima include metodi che consentono di passare i comandi SQL direttamente al database. Sono disponibili le seguenti opzioni:

- Utilizzare il [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) metodo per le query che restituiscono tipi di entità. Gli oggetti restituiti devono essere del tipo previsto per il `DbSet` oggetto e vengono rilevate automaticamente dal contesto di database a meno che non si disattiva rilevamento. (Vedere la sezione seguente [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) metodo.)
- Utilizzare il [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) metodo per le query che restituiscono tipi non entità. I dati restituiti non vengono registrati dal contesto di database, anche se il metodo viene usato per recuperare i tipi di entità.
- Utilizzare il [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) per i comandi non query.

Uno dei vantaggi dell'utilizzo di Entity Framework è la mancanza di un collegamento troppo stretto del codice a un particolare metodo di archiviazione dei dati. Le query e i comandi SQL vengono generati automaticamente e non è necessario scriverli. Esistono scenari eccezionali, quando è necessario eseguire query SQL specifiche che è stato creato manualmente, ma questi metodi consentono di gestire tali eccezioni.

Come avviene quando si eseguono comandi SQL in un'applicazione Web, è necessario adottare delle precauzioni per proteggere il sito dagli attacchi SQL injection. A questo scopo è possibile usare query parametrizzate per assicurarsi che le stringhe inviate da una pagina Web non possano essere interpretate come comandi SQL. In questa esercitazione verranno usate query parametrizzate quando l'input dell'utente viene integrato in una query.

### <a name="calling-a-query-that-returns-entities"></a>La chiamata di una Query che restituisce le entità

Il [DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) classe fornisce un metodo che è possibile utilizzare per eseguire una query che restituisce un'entità di tipo `TEntity`. Per visualizzarne il funzionamento si modificherà il codice nel `Details` metodo il `Department` controller.

In *DepartmentController.cs*nella `Details` (metodo), sostituire il `db.Departments.FindAsync` chiamata al metodo con un `db.Departments.SqlQuery` chiamata al metodo, come illustrato nel codice evidenziato seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Per verificare il corretto funzionamento del nuovo codice, selezionare la scheda **Departments** e quindi **Dettagli** per uno dei reparti.

![Dettagli del reparto](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>La chiamata di una Query che restituisce gli altri tipi di oggetti

In precedenza è stata creata una griglia delle statistiche degli studenti per la pagina About che visualizza il numero di studenti per ogni data di registrazione. Il codice che esegue questa operazione in *HomeController.cs* utilizza LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Si supponga di che voler scrivere codice che recupera i dati direttamente in SQL anziché l'utilizzo di LINQ. Scopo che è necessario eseguire una query che restituisce un valore diverso da oggetti entità, vale a dire che è necessario utilizzare il [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) metodo.

In *HomeController.cs*, sostituire l'istruzione LINQ nel `About` (metodo) con un'istruzione SQL, come illustrato nel codice evidenziato seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Eseguire la pagina di informazioni. Vengono visualizzati gli stessi dati visualizzati in precedenza.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>La chiamata di una Query di aggiornamento

Si supponga che gli amministratori University Contoso desiderano essere in grado di eseguire modifiche di massa nel database, ad esempio la modifica del numero di crediti per ogni corso. Se il numero di corsi dell'università è elevato, potrebbe non essere utile recuperarli tutti come entità e modificarli singolarmente. In questa sezione viene implementato una pagina web che consente all'utente di specificare un fattore in base a cui si desidera modificare il numero di crediti per tutti i corsi e ti la modifica tramite l'esecuzione di un database SQL `UPDATE` istruzione. La pagina Web apparirà come segue:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

In *CourseContoller.cs*, aggiungere `UpdateCourseCredits` metodi per `HttpGet` e `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Quando il controller elabora un `HttpGet` richiesta, non viene restituito il `ViewBag.RowsAffected` variabile e la visualizzazione consente di visualizzare una casella di testo e un pulsante di invio, come illustrato nella figura precedente.

Quando il **aggiornamento** si fa clic sul pulsante, il `HttpPost` metodo viene chiamato, e `multiplier` con il valore immesso nella casella di testo. Il codice quindi viene eseguita la query SQL che aggiorna corsi e restituisce il numero di righe interessate alla vista di `ViewBag.RowsAffected` variabile. Quando la vista Ottiene un valore che variabile, viene visualizzato il numero di righe aggiornate invece della casella di testo e inviare pulsante, come illustrato nella figura seguente:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

In *CourseController.cs*, fare doppio clic su uno del `UpdateCourseCredits` metodi e quindi fare clic su **Aggiungi visualizzazione**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

In *Views\Course\UpdateCourseCredits.cshtml*, sostituire il codice del modello con il codice seguente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Eseguire il metodo `UpdateCourseCredits` selezionando la scheda **Courses**, quindi aggiungendo "/UpdateCourseCredits" alla fine dell'URL nella barra degli indirizzi del browser (ad esempio: `http://localhost:50205/Course/UpdateCourseCredits`). Immettere un numero nella casella di testo:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Fare clic su **Aggiorna**. Viene visualizzato il numero di righe interessate:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Fare clic su **Torna all'elenco** per visualizzare l'elenco dei corsi con il numero di crediti modificato.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Per ulteriori informazioni sulle query SQL non elaborate, vedere [query SQL non elaborati](https://msdn.microsoft.com/data/jj592907) su MSDN.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>Query di rilevamento di No

Quando un contesto di database recupera righe di tabella e crea oggetti entità che le rappresentano, per impostazione predefinita rileva se le entità in memoria sono sincronizzate con ciò che è presente nel database. I dati in memoria svolgono la funzione di una cache e vengono usati per l'aggiornamento di un'entità. Questa memorizzazione nella cache spesso non è necessaria in un'applicazione Web poiché le istanze del contesto hanno spesso una durata breve (viene creata ed eliminata una nuova istanza per ogni richiesta) e il contesto che legge un'entità viene in genere eliminato prima che l'entità venga riutilizzata.

È possibile disabilitare il rilevamento degli oggetti entità in memoria tramite il [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) metodo. Gli scenari tipici in cui viene disabilitata la registrazione includono i seguenti:

- Una query recupera un grosso volume di dati che potrebbero migliorare notevolmente le prestazioni di disattivazione del rilevamento.
- Si desidera collegare un'entità per l'aggiornamento, ma è recuperato in precedenza la stessa entità per uno scopo diverso. Poiché l'entità viene già registrata dal contesto di database, non è possibile collegare l'entità che si vuole modificare. Per gestire questa situazione è possibile utilizzare il `AsNoTracking` opzione con la query precedente.

Per un esempio che illustra come usare il [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) metodo, vedere [la versione precedente di questa esercitazione](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Questa versione dell'esercitazione non imposta il flag modificato su un'entità di modello dello strumento di associazione creato nel metodo di modifica, in modo che non sia `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Analisi SQL inviati al database

A volte può essere utile visualizzare le query SQL inviate al database. In un'esercitazione precedente è stato illustrato come eseguire questa operazione nel codice dell'intercettore. a questo punto si noterà che alcuni modi per eseguire questa operazione senza scrivere codice dell'intercettore. Per provare la natura, verrà esaminata una semplice query e quindi osservare cosa accade a esso quando si aggiungono le opzioni di questo tipo eager durante il caricamento, filtro e ordinamento.

In *controller/CourseController*, sostituire il `Index` (metodo) con il codice seguente, per interrompere temporaneamente il caricamento non differito:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Ora impostare un punto di interruzione di `return` istruzione (F9 con il cursore sulla riga). Premere F5 per eseguire il progetto in modalità di debug, quindi selezionare la pagina di indice corso. Quando il codice raggiunge il punto di interruzione, esaminare il `sql` variabile. Viene visualizzata la query che viene inviata a SQL Server. È un semplice `Select` istruzione.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Fare clic sulla classe per visualizzare la query lente di **Visualizzatore testo**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Ora si aggiungerà un elenco a discesa alla pagina di indice corsi in modo che gli utenti possono filtrare per un particolare reparto. Si ordinerà i corsi per titolo, e si specificano il caricamento immediato per la `Department` proprietà di navigazione.

In *CourseController.cs*, sostituire il `Index` (metodo) con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Ripristina il punto di interruzione di `return` istruzione.

Il metodo riceve il valore selezionato dell'elenco di riepilogo a discesa il `SelectedDepartment` parametro. Se non è selezionato, questo parametro sarà null.

Oggetto `SelectList` raccolta contenente tutti i reparti viene passato alla visualizzazione per l'elenco a discesa. I parametri passati al `SelectList` costruttore specificare il nome del campo valore, il nome del campo di testo e l'elemento selezionato.

Per il `Get` metodo il `Course` repository, il codice specifica un'espressione di filtro, ordinamento e il caricamento per eager il `Department` proprietà di navigazione. Restituisce sempre l'espressione di filtro `true` se nell'elenco a discesa viene selezionato nulla (vale a dire `SelectedDepartment` è null).

In *Views\Course\Index.cshtml*, immediatamente prima dell'apertura `table` tag, aggiungere il codice seguente per creare l'elenco a discesa e un pulsante di invio:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Con il punto di interruzione impostato, eseguire la pagina di indice corso. Continuare il processo le volte che il codice raggiunge un punto di interruzione, in modo che la pagina viene visualizzata nel browser. Selezionare un reparto dall'elenco a discesa e fare clic su **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Questa volta sarà il primo punto di interruzione per la query reparti per l'elenco a discesa. Ignorare che e visualizzare il `query` variabile la volta successiva che il codice raggiunge il punto di interruzione per vedere quali il `Course` query ora è simile. Verrà visualizzato qualcosa di simile al seguente:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

È possibile vedere che la query è ora un `JOIN` query che carica `Department` insieme ai dati di `Course` dati e che includa un `WHERE` clausola.

Rimuovere il `var sql = courses.ToString()` riga.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Modelli di repository e unità di lavoro

Molti sviluppatori scrivono codice per implementare i modelli di repository e unità di lavoro come wrapper per il codice usato con Entity Framework. Questi modelli sono progettati per la creazione di un livello di astrazione tra il livello di accesso ai dati e il livello della logica di business di un'applicazione. L'implementazione di questi modelli può essere utile per isolare l'applicazione dalle modifiche nell'archivio dati e può semplificare il testing unità automatizzato o lo sviluppo basato su test (TDD). Tuttavia, la scrittura di codice aggiuntivo per l'implementazione di questi modelli non è sempre la scelta ideale per applicazioni che usano Entity Framework, per diversi motivi:

- La classe del contesto EF isola il codice dal codice specifico dell'archivio dati.
- La classe del contesto di EF può svolgere la funzione di classe di unità di lavoro per gli aggiornamenti di database eseguiti usando EF.
- Funzionalità introdotte in Entity Framework 6 rendono più facile da implementare TDD senza scrivere codice di repository.

Per ulteriori informazioni su come implementare il repository e l'unità di lavoro modelli, vedere [la versione di Entity Framework 5 di questa serie di esercitazioni](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Per informazioni sui modi per implementare TDD in Entity Framework 6, vedere le risorse seguenti:

- [Come EF6 consenta Mocking DbSets più facilmente](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Eseguire i test con un framework di simulazione](https://msdn.microsoft.com/data/dn314429)
- [Eseguire i test con la propria copie di test](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Classi proxy

Quando Entity Framework crea istanze di entità (ad esempio, quando si esegue una query), vengono spesso creati come istanze di un tipo derivato in modo dinamico generato che funge da proxy per l'entità. Ad esempio, vedere le seguenti due immagini del debugger. Nella prima immagine, vedrai che il `student` variabile è previsto `Student` digitare subito dopo avere creato un'istanza dell'entità. Nella seconda figura dopo Entity Framework è stato utilizzato per leggere un'entità studente dal database, vengono visualizzati la classe proxy.

![Prima di classe proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Dopo la classe proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Questa classe proxy esegue l'override di alcune proprietà dell'entità da inserire hook per eseguire automaticamente azioni quando si accede alla proprietà virtuali. Questo meccanismo è utilizzare per una funzione è il caricamento lazy.

La maggior parte dei casi non è necessario essere a conoscenza di questo utilizzo di proxy, ma esistono alcune eccezioni:

- In alcuni scenari è consigliabile impedire la creazione di istanze di proxy di Entity Framework. Ad esempio, quando si sta serializzando l'entità desiderata in genere le classi POCO, non le classi proxy. È di un modo per evitare problemi di serializzazione per serializzare oggetti di trasferimento di dati DTO anziché oggetti entità, come illustrato nel [tramite Web API with Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) esercitazione. È inoltre possibile [disabilita la creazione di proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Quando si crea un'istanza una classe di entità mediante il `new` (operatore), non si ottiene un'istanza del proxy. Ciò significa che non si ottengono funzionalità, ad esempio rilevamento delle modifiche automatico e il caricamento lazy. Si tratta in genere OK; in genere non è necessario il caricamento lazy, perché si sta creando una nuova entità che non è nel database, e in genere non occorre rilevamento se si sta contrassegnare in modo esplicito l'entità come `Added`. Tuttavia, se è necessario il caricamento lazy ed è necessario il rilevamento delle modifiche, è possibile creare nuove istanze di entità con proxy utilizzando il [crea](https://msdn.microsoft.com/library/gg679504.aspx) metodo la `DbSet` classe.
- È possibile ottenere un tipo di entità effettivo da un tipo proxy. È possibile utilizzare il [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) metodo la `ObjectContext` classe per ottenere il tipo di entità effettivo di un'istanza del tipo proxy.

Per ulteriori informazioni, vedere [utilizzo di proxy](https://msdn.microsoft.com/data/JJ592886.aspx) su MSDN.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Rilevamento automatico delle modifiche

Entity Framework determina come è stata modificata un'entità (e di conseguenza gli aggiornamenti da inviare al database) confrontando i valori correnti di un'entità con i valori originali. I valori originali vengono memorizzati quando viene eseguita una query sull'entità o quando viene collegata. I metodi che causano il rilevamento automatico delle modifiche includono:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Se si desidera tenere traccia di un numero elevato di entità e viene chiamato uno di questi metodi più volte in un ciclo, è possibile ottenere miglioramenti significativi delle prestazioni disattivando temporaneamente il rilevamento delle modifiche automatico utilizzando il [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) proprietà. Per ulteriori informazioni, vedere [automaticamente il rilevamento modifiche](https://msdn.microsoft.com/data/jj556205) su MSDN.

<a id="validation"></a>
## <a name="automatic-validation"></a>Convalida automatica

Quando si chiama il `SaveChanges` (metodo), per impostazione predefinita di Entity Framework convalida i dati in tutte le proprietà di tutte le entità modificate prima di aggiornare il database. Se è stato aggiornato un numero elevato di entità già stato convalidato i dati, questa operazione è necessaria e far sì che il processo di salvataggio le modifiche avranno meno tempo per disattivare temporaneamente la convalida. È possibile eseguire tale utilizzando il [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) proprietà. Per ulteriori informazioni, vedere [convalida](https://msdn.microsoft.com/data/gg193959) su MSDN.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Strumenti di Entity Framework Power

[Strumenti di Entity Framework Power](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) è un componente aggiuntivo di Visual Studio che è stato utilizzato per creare i diagrammi di modello di dati visualizzato in queste esercitazioni. Gli strumenti è anche possono eseguire altra funzione, ad esempio generare classi di entità in base alle tabelle in un database esistente in modo che è possibile utilizzare il database con Code First. Dopo aver installato gli strumenti, alcune opzioni aggiuntive vengono visualizzati nel menu di scelta rapida. Ad esempio, quando si fa clic su della classe contesto **Esplora**, viene visualizzata un'opzione per generare un diagramma. Quando si utilizza Code First non è possibile modificare il modello di dati nel diagramma, ma è possibile spostare elementi per renderne più semplice da comprendere.

![EF nel menu di scelta rapida](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![Diagramma di Entity Framework](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Codice sorgente di Entity Framework

Il codice sorgente per Entity Framework 6 è disponibile all'indirizzo [GitHub](https://github.com/aspnet/EntityFramework6). È possibile segnalare i bug ed è possibile fornire il propria miglioramenti per il codice sorgente di Entity Framework.

Anche se il codice sorgente è aperto, Entity Framework è completamente supportata come un prodotto Microsoft. Il team di Microsoft Entity Framework controlla i contributi accettati ed esegue il test di tutte le modifiche al codice per garantire la qualità di ogni rilascio.

<a id="summary"></a>
## <a name="summary"></a>Riepilogo

Questa serie di esercitazioni sull'uso di Entity Framework in un'applicazione MVC ASP.NET è stata completata. Per ulteriori informazioni sull'utilizzo dei dati mediante Entity Framework, vedere il [pagina della documentazione di Entity Framework in MSDN](https://msdn.microsoft.com/data/ee712907) e [accesso ai dati ASP.NET - risorse](../../../../whitepapers/aspnet-data-access-content-map.md).

Per ulteriori informazioni su come distribuire l'applicazione web dopo aver creato, vedere [distribuzione Web di ASP.NET - risorse](../../../../whitepapers/aspnet-web-deployment-content-map.md) in MSDN Library.

Per informazioni su altri argomenti relativi a MVC, quali l'autenticazione e autorizzazione, vedere il [ASP.NET MVC - risorse](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Ringraziamenti

- Tom Dykstra ha scritto la versione originale di questa esercitazione, co-autore l'aggiornamento di Entity Framework 5 e scritto l'aggiornamento 6 di Entity Framework. Tom è un writer di programmazione senior nel Team di contenuto di strumenti e piattaforma Web Microsoft.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) ha la maggior parte delle operazioni di aggiornamento dell'esercitazione per Entity Framework 5 e MVC 4 e co-autore dell'aggiornamento 6 di Entity Framework. Rick è un writer di programmazione senior per porre l'attenzione su Azure e MVC Microsoft.
- [Miller rowan](http://www.romiller.com) e altri membri del team di Entity Framework consente di eseguire il debug molti problemi con le migrazioni che si verifica mentre si stava aggiornando l'esercitazione per Entity Framework 5 e 6 di Entity Framework e assistiti da con le revisioni del codice.

<a id="vb"></a>
## <a name="vb"></a>VB

Quando l'esercitazione è stato originariamente creata per Entity Framework 4.1, abbiamo fornito versioni c# e VB del progetto completato il download. A causa di limitazioni di tempo e altre priorità che non abbiamo realizzato che per questa versione. Se si compila un progetto VB utilizzando queste esercitazioni e sarebbe disposti per condividerli con altri utenti, Saremmo lieti di sapere.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Gli errori comuni e le soluzioni per tali

### <a name="cannot-createshadow-copy"></a>Impossibile creare/shadow copia

Messaggio di errore:

> Impossibile creare/shadow copia '&lt;filename&gt;' quando il file esiste già.


Soluzione

Attendere alcuni secondi e aggiornare la pagina.

### <a name="update-database-not-recognized"></a>Update-Database non riconosciuto

Messaggio di errore (dal `Update-Database` comando PMC):

> Il termine 'Update-Database' non è riconosciuto come nome di un cmdlet, funzione, file di script o programma eseguibile. Controllare l'ortografia del nome, o se è stato incluso un percorso, verificare che il percorso sia corretto e riprovare.


Soluzione

Uscire da Visual Studio. Riaprire progetto e riprovare.

### <a name="validation-failed"></a>Convalida non riuscita

Messaggio di errore (dal `Update-Database` comando PMC):

> Convalida non riuscita per uno o più entità. La proprietà 'EntityValidationErrors' per ulteriori informazioni, vedere.


Soluzione

Una delle cause di questo problema è errori di convalida quando la `Seed` esecuzione del metodo. Vedere [Seeding e database di debug di Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) per suggerimenti sul debug di `Seed` metodo.

### <a name="http-50019-error"></a>HTTP 500.19 errore

Messaggio di errore:

> Errore HTTP 500.19 - errore interno del Server  
> Non è possibile accedere alla pagina richiesta perché i dati di configurazione per la pagina non sono validi.


Soluzione

È possibile ottenere questo errore è da più copie della soluzione, ognuno di essi utilizzando lo stesso numero di porta. In genere, è possibile risolvere questo problema, uscire da tutte le istanze di Visual Studio, quindi riavviare il progetto che si sta lavorando. Se il problema persiste, provare a modificare il numero di porta. Fare clic con il pulsante destro sul file di progetto e quindi fare clic su proprietà. Selezionare il **Web** scheda e quindi modificare il numero di porta di **Url progetto** casella di testo.

### <a name="error-locating-sql-server-instance"></a>Errore di individuazione dell'istanza di SQL Server

Messaggio di errore:

> Si è verificato un errore di rete o specifico dell'istanza mentre si cercava di stabilire una connessione con SQL Server. Il server non è stato trovato o non è accessibile. Verificare che il nome dell'istanza sia corretto e che SQL Server sia configurato in modo da consentire connessioni remote. (provider: interfacce di rete SQL, errore: 26 - Errore nell'individuazione del server/dell'istanza specificata)


Soluzione

Controllare la stringa di connessione. Se è stato eliminato manualmente il database, modificare il nome del database nella stringa di costruzione.

> [!div class="step-by-step"]
> [Precedente](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
