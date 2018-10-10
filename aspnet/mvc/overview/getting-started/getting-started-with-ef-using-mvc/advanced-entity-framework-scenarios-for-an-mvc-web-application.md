---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Advanced scenari di Entity Framework 6 per un'applicazione Web 5 MVC (12 12) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 5 con Entity Framework 6 Code First e Visual Studio...
ms.author: riande
ms.date: 12/08/2014
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 0aa440e700c9bfb02aa5d55ebf481850a730febe
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912683"
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>Scenari avanzati Entity Framework 6 per un'applicazione Web 5 MVC (12 12)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 5 con Entity Framework 6 Code First e Visual Studio. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Nell'esercitazione precedente è implementata l'ereditarietà tabella per gerarchia. Sono incluse in questa esercitazione presenta diversi argomenti che è utili tenere presente dopo aver appreso le nozioni di base dello sviluppo di applicazioni web ASP.NET che usano Entity Framework Code First. Istruzioni dettagliate consentono di eseguire il codice e l'utilizzo di Visual Studio per gli argomenti seguenti:

- [Esecuzione di query SQL non elaborate](#rawsql)
- [Esecuzione di query senza registrazione](#notracking)
- [Analisi SQL inviato al database](#sql)

L'esercitazione presenta diversi argomenti con brevi introduzioni seguite da collegamenti alle risorse per altre informazioni:

- [Repository di modelli e unità di lavoro](#repo)
- [Classi proxy](#proxies)
- [Rilevamento delle modifiche automatico](#changedetection)
- [Convalida automatica](#validation)
- [Strumenti di Entity Framework per Visual Studio](#tools)
- [Codice sorgente di Entity Framework](#source)

Questa esercitazione include inoltre le sezioni seguenti:

- [Riepilogo](#summary)
- [Riconoscimenti](#acknowledgments)
- [Nota su Visual Basic](#vb)
- [Gli errori comuni e soluzioni o soluzioni alternative per tali](#errors)

Per la maggior parte degli argomenti seguenti, si utilizzeranno le pagine in cui è già stato creato. Per usare SQL non elaborate per effettuare aggiornamenti bulk si creerà una nuova pagina che aggiorna il numero di crediti di tutti i corsi nel database:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Query SQL non elaborate

L'API di Entity Framework Code First include metodi che consentono di passare i comandi SQL direttamente al database. Sono disponibili le seguenti opzioni:

- Usare la [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) metodo per le query che restituiscono tipi di entità. Gli oggetti restituiti devono essere del tipo previsto per il `DbSet` oggetto che vengono automaticamente registrate dal contesto del database a meno che non è disattivata la registrazione. (Vedere la sezione seguente [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) (metodo).)
- Usare la [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) metodo per le query che restituiscono tipi non entità. I dati restituiti non vengono registrati dal contesto di database, anche se il metodo viene usato per recuperare i tipi di entità.
- Usare la [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) per i comandi non query.

Uno dei vantaggi dell'utilizzo di Entity Framework è la mancanza di un collegamento troppo stretto del codice a un particolare metodo di archiviazione dei dati. Le query e i comandi SQL vengono generati automaticamente e non è necessario scriverli. Ma esistono alcuni scenari particolari in cui è necessario eseguire query SQL specifiche create manualmente e questi metodi consentono di gestire tali eccezioni.

Come avviene quando si eseguono comandi SQL in un'applicazione Web, è necessario adottare delle precauzioni per proteggere il sito dagli attacchi SQL injection. A questo scopo è possibile usare query parametrizzate per assicurarsi che le stringhe inviate da una pagina Web non possano essere interpretate come comandi SQL. In questa esercitazione verranno usate query parametrizzate quando l'input dell'utente viene integrato in una query.

### <a name="calling-a-query-that-returns-entities"></a>Chiamata di una Query che restituisce le entità

Il [DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) classe fornisce un metodo che è possibile usare per eseguire una query che restituisce un'entità di tipo `TEntity`. Per visualizzarne il funzionamento è necessario modificare il codice nel `Details` metodo di `Department` controller.

Nella *DepartmentController.cs*, nella `Details` metodo, sostituire il `db.Departments.FindAsync` chiamata al metodo con un `db.Departments.SqlQuery` chiamata al metodo, come illustrato nel codice evidenziato seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Per verificare il corretto funzionamento del nuovo codice, selezionare la scheda **Departments** e quindi **Dettagli** per uno dei reparti.

![Dettagli del reparto](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Chiamata di una Query che restituisce altri tipi di oggetti

In precedenza è stata creata una griglia delle statistiche degli studenti per la pagina About che visualizza il numero di studenti per ogni data di registrazione. Il codice che esegue questa operazione nel *HomeController.cs* Usa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Si supponga di che voler scrivere il codice che recupera i dati direttamente in SQL, anziché usare LINQ. Per farlo è necessario eseguire una query che restituisce un valore diverso da oggetti entità, ovvero è necessario usare il [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) (metodo).

Nelle *HomeController.cs*, sostituire l'istruzione LINQ nel `About` metodo con un'istruzione SQL, come illustrato nel codice evidenziato seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Eseguire la pagina di informazioni. Vengono visualizzati gli stessi dati visualizzati in precedenza.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>La chiamata a una Query di aggiornamento

Si supponga che gli amministratori di Contoso University vogliono essere in grado di eseguire modifiche di massa nel database, ad esempio modificare il numero di crediti per ogni corso. Se il numero di corsi dell'università è elevato, potrebbe non essere utile recuperarli tutti come entità e modificarli singolarmente. In questa sezione è possibile implementare una pagina web che consente all'utente di specificare un fattore in base a cui si desidera modificare il numero di crediti per tutti i corsi e viene apportata la modifica tramite l'esecuzione di un database SQL `UPDATE` istruzione. La pagina Web apparirà come segue:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

Nelle *CourseContoller.cs*, aggiungere `UpdateCourseCredits` per i metodi `HttpGet` e `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Quando il controller elabora un' `HttpGet` richiesta, non viene restituito il `ViewBag.RowsAffected` variabile, mentre la vista viene visualizzata una casella di testo vuoto e un pulsante di invio, come illustrato nella figura precedente.

Quando la **Update** si fa clic sul pulsante, il `HttpPost` viene chiamato il metodo e `multiplier` ha il valore immesso nella casella di testo. Il codice esegue quindi il codice SQL che aggiorna i corsi e restituisce il numero di righe interessate nella visualizzazione nel `ViewBag.RowsAffected` variabile. Quando la vista Ottiene un valore in tale variabile, Visualizza il numero di righe aggiornate invece la casella di testo e pulsante di invio, come illustrato nella figura seguente:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

Nelle *CourseController.cs*, fare doppio clic su uno delle `UpdateCourseCredits` metodi e quindi fare clic su **Aggiungi visualizzazione**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Nelle *Views\Course\UpdateCourseCredits.cshtml*, sostituire il codice del modello con il codice seguente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Eseguire il metodo `UpdateCourseCredits` selezionando la scheda **Courses**, quindi aggiungendo "/UpdateCourseCredits" alla fine dell'URL nella barra degli indirizzi del browser (ad esempio: `http://localhost:50205/Course/UpdateCourseCredits`). Immettere un numero nella casella di testo:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Fare clic su **Aggiorna**. Viene visualizzato il numero di righe interessate:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Fare clic su **Torna all'elenco** per visualizzare l'elenco dei corsi con il numero di crediti modificato.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Per altre informazioni sulle query SQL non elaborate, vedere [query SQL non elaborate](https://msdn.microsoft.com/data/jj592907) su MSDN.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>senza rilevamento delle modifiche

Quando un contesto di database recupera righe di tabella e crea oggetti entità che le rappresentano, per impostazione predefinita rileva se le entità in memoria sono sincronizzate con ciò che è presente nel database. I dati in memoria svolgono la funzione di una cache e vengono usati per l'aggiornamento di un'entità. Questa memorizzazione nella cache spesso non è necessaria in un'applicazione Web poiché le istanze del contesto hanno spesso una durata breve (viene creata ed eliminata una nuova istanza per ogni richiesta) e il contesto che legge un'entità viene in genere eliminato prima che l'entità venga riutilizzata.

È possibile disabilitare il rilevamento di oggetti entità in memoria usando il [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) (metodo). Gli scenari tipici in cui viene disabilitata la registrazione includono i seguenti:

- Una query recupera tali una grande quantità di dati che potrebbero migliorare notevolmente le prestazioni disattivazione del rilevamento.
- Si vuole collegare un'entità per aggiornarla, ma è recuperato in precedenza alla stessa entità per uno scopo diverso. Poiché l'entità viene già registrata dal contesto di database, non è possibile collegare l'entità che si vuole modificare. Un modo per gestire questa situazione consiste nell'utilizzare il `AsNoTracking` opzione con la query precedente.

Per un esempio che illustra come usare il [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) metodo, vedere [la versione precedente di questa esercitazione](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Questa versione dell'esercitazione non imposta il flag modificato su un'entità modello dello strumento di associazione creato nel metodo di modifica, è necessario `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Analisi SQL inviato al database

A volte può essere utile visualizzare le query SQL inviate al database. In un'esercitazione precedente è stato illustrato come eseguire questa operazione nel codice dell'intercettore. a questo punto si noterà che alcuni modi per farlo senza scrivere codice dell'intercettore. Per provare questo timeout, verrà esaminare una semplice query e quindi esaminare che cosa succede quando si aggiungono le opzioni di questo tipo eager il caricamento, filtro e ordinamento.

Nelle *controller/CourseController*, sostituire il `Index` metodo con il codice seguente, per arrestare temporaneamente il caricamento eager:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Ora impostare un punto di interruzione il `return` istruzione (F9 con il cursore sulla riga). Premere **F5** per eseguire il progetto in modalità di debug e selezionare la pagina di indice del corso. Quando il codice raggiunge il punto di interruzione, esaminare il `sql` variabile. Viene visualizzata la query che viene inviata a SQL Server. È una semplice `Select` istruzione.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Fare clic sulla lente di ingrandimento per visualizzare la query nella **Visualizzatore testo**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Ora si aggiungerà un elenco di riepilogo a discesa per la pagina di indice dei corsi in modo che gli utenti possono filtrare per un reparto specifico. Si ordinerà i corsi per titolo, e si specificano il caricamento eager per la `Department` proprietà di navigazione.

Nelle *CourseController.cs*, sostituire il `Index` metodo con il codice seguente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Ripristinare il punto di interruzione sul `return` istruzione.

Il metodo riceve il valore dell'elenco di riepilogo a discesa selezionato il `SelectedDepartment` parametro. Se è stata eseguita alcuna selezione, questo parametro sarà null.

Oggetto `SelectList` insieme contenente tutti i reparti è passato alla visualizzazione per l'elenco a discesa. I parametri passati al `SelectList` costruttore specificare il nome del campo valore, il nome del campo di testo e l'elemento selezionato.

Per il `Get` metodo per il `Course` repository, il codice specifica un'espressione di filtro, ordinamento e il caricamento eager per la `Department` proprietà di navigazione. L'espressione restituisce sempre `true` se è stata eseguita alcuna selezione nell'elenco a discesa scegliere (vale a dire `SelectedDepartment` è null).

Nelle *Views\Course\Index.cshtml*, immediatamente prima dell'apertura `table` tag, aggiungere il codice seguente per creare l'elenco a discesa e un pulsante di invio:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Con il punto di interruzione impostato, eseguire la pagina di indice del corso. Continuare con le volte che il codice raggiunge un punto di interruzione, in modo che la pagina viene visualizzata nel browser. Selezionare un reparto dall'elenco a discesa e fare clic su **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Questa volta sarà il primo punto di interruzione per la query di reparti per l'elenco a discesa. Ignora che e visualizzare il `query` variabile la volta successiva che il codice raggiunge il punto di interruzione per vedere quali il `Course` query dovrebbe ora apparire così. Si otterrà un risultato simile al seguente:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Si noterà che la query è ora un `JOIN` query che consente di caricare `Department` insieme ai dati di `Course` dati e che includa un `WHERE` clausola.

Rimuovere il `var sql = courses.ToString()` riga.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Modelli di repository e unità di lavoro

Molti sviluppatori scrivono codice per implementare i modelli di repository e unità di lavoro come wrapper per il codice usato con Entity Framework. Questi modelli sono progettati per la creazione di un livello di astrazione tra il livello di accesso ai dati e il livello della logica di business di un'applicazione. L'implementazione di questi modelli può essere utile per isolare l'applicazione dalle modifiche nell'archivio dati e può semplificare il testing unità automatizzato o lo sviluppo basato su test (TDD). Tuttavia, la scrittura di codice aggiuntivo per l'implementazione di questi modelli non è sempre la scelta migliore per le applicazioni che usano Entity Framework, per diversi motivi:

- La classe del contesto EF isola il codice dal codice specifico dell'archivio dati.
- La classe del contesto di EF può svolgere la funzione di classe di unità di lavoro per gli aggiornamenti di database eseguiti usando EF.
- Nuove funzionalità di Entity Framework 6 rendono più semplice da implementare TDD senza scrivere codice di repository.

Per altre informazioni su come implementare il repository di modelli e unità di lavoro, vedere [la versione di Entity Framework 5 di questa serie di esercitazioni](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Per informazioni sui metodi di implementazione TDD in Entity Framework 6, vedere le risorse seguenti:

- [EF6 modo DbSet Mocking più facilmente](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Test con un framework di simulazione](https://msdn.microsoft.com/data/dn314429)
- [Test con il proprio copie di test](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Classi proxy

Quando Entity Framework crea istanze di entità (ad esempio, quando si esegue una query), vengono spesso creati come istanze di un tipo derivato generata dinamicamente che funge da proxy per l'entità. Ad esempio, vedere le seguenti due immagini del debugger. Si può vedere nell'immagine prima, la `student` variabile è quella prevista `Student` digitare subito dopo si crea un'istanza dell'entità. Nell'immagine secondo dopo EF è stato usato per leggere un'entità student dal database, noterete che la classe proxy.

![Prima della classe proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Dopo la classe proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Questa classe proxy esegue l'override di alcune proprietà dell'entità da inserire hook per eseguire automaticamente azioni quando si accede alla proprietà virtuale. Questo meccanismo viene usato per una funzione è il caricamento lazy.

La maggior parte dei casi non è necessario essere a conoscenza di questo utilizzo di proxy, ma sono presenti eccezioni:

- In alcuni scenari si potrebbe voler impedire la creazione di istanze del proxy di Entity Framework. Ad esempio, quando si esegue la serializzazione delle entità in generale le classi POCO, non le classi proxy. È un modo per evitare problemi di serializzazione per serializzare gli oggetti di trasferimento di dati (DTO) invece di oggetti entità, come illustrato nella [uso di API Web con Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) esercitazione. È anche possibile [disabilita la creazione di proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Quando si crea un'istanza di una classe di entità mediante il `new` operatore, non si ottiene un'istanza del proxy. Ciò significa che non si ottengono funzionalità, ad esempio il caricamento lazy e automatico il rilevamento delle modifiche. Si tratta in genere bene; in genere non è necessario il caricamento lazy, perché si sta creando una nuova entità che non è presente nel database, e in genere non occorre rilevamento delle modifiche se si sta contrassegnare in modo esplicito l'entità come `Added`. Tuttavia, se è necessario il caricamento lazy ed è necessario il rilevamento delle modifiche, è possibile creare nuove istanze dell'entità con proxy usando il [Create](https://msdn.microsoft.com/library/gg679504.aspx) metodo del `DbSet` classe.
- È possibile ottenere un tipo di entità effettivo da un tipo di proxy. È possibile usare la [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) metodo il `ObjectContext` classe per ottenere il tipo di entità effettivo di un'istanza del tipo proxy.

Per altre informazioni, vedere [utilizzo di proxy](https://msdn.microsoft.com/data/JJ592886.aspx) su MSDN.

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

Se si desidera monitorare un numero elevato di entità e si chiama uno di questi metodi molte volte in un ciclo, è possibile ottenere miglioramenti significativi delle prestazioni disattivando temporaneamente il rilevamento automatico delle modifiche tramite il [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) proprietà. Per altre informazioni, vedere [rilevare automaticamente le modifiche](https://msdn.microsoft.com/data/jj556205) su MSDN.

<a id="validation"></a>
## <a name="automatic-validation"></a>Convalida automatica

Quando si chiama il `SaveChanges` (metodo), per impostazione predefinita Entity Framework convalida i dati in tutte le proprietà di tutte le entità modificate prima di aggiornare il database. Se è stato aggiornato un numero elevato di entità e aver già convalidato i dati, questa operazione è necessaria e far sì che il processo di salvataggio le modifiche diventano meno tempo disattivando temporaneamente la convalida. È possibile farlo tramite il [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) proprietà. Per altre informazioni, vedere [convalida](https://msdn.microsoft.com/data/gg193959) su MSDN.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Entity Framework Power Tools

[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) è un componente aggiuntivo di Visual Studio che è stato utilizzato per creare i diagrammi di modello di dati visualizzato in queste esercitazioni. Gli strumenti è anche possono eseguire altre funzioni, ad esempio generare classi di entità basate sulle tabelle in un database esistente in modo che è possibile usare il database con Code First. Dopo aver installato gli strumenti, alcune opzioni aggiuntive vengono visualizzati nel menu di scelta rapida. Ad esempio, quando mouse sulla classe di contesto nella **Esplora soluzioni**, si ottiene un'opzione per generare un diagramma. Quando si utilizza Code First è possibile modificare il modello di dati nel diagramma, ma è possibile spostarsi cose per renderlo più facile da comprendere.

![Entity Framework nel menu di scelta rapida](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![Diagramma di Entity Framework](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Codice sorgente di Entity Framework

Il codice sorgente per Entity Framework 6 è disponibile all'indirizzo [GitHub](https://github.com/aspnet/EntityFramework6). È possibile archiviare il bug e contribuire con i proprio miglioramenti al codice sorgente di Entity Framework.

Anche se il codice sorgente è aperto, Entity Framework è completamente supportato come prodotto di Microsoft. Il team di Microsoft Entity Framework controlla i contributi accettati ed esegue il test di tutte le modifiche al codice per garantire la qualità di ogni rilascio.

<a id="summary"></a>
## <a name="summary"></a>Riepilogo

Questa serie di esercitazioni sull'uso di Entity Framework in un'applicazione ASP.NET MVC è stata completata. Per altre informazioni su come usare i dati con Entity Framework, vedere la [pagina della documentazione di Entity Framework in MSDN](https://msdn.microsoft.com/data/ee712907) e [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

Per altre informazioni su come distribuire l'applicazione web dopo aver creato, vedere [distribuzione Web ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-web-deployment-content-map.md) in MSDN Library.

Per informazioni su altri argomenti correlati a MVC, ad esempio l'autenticazione e autorizzazione, vedere la [MVC ASP.NET - risorse consigliate](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Ringraziamenti

- Tom Dykstra ha scritto la versione originale di questa esercitazione, coautore l'aggiornamento di Entity Framework 5 ed è stata scritta l'aggiornamento di Entity Framework 6. Tom è una scrittrice senior di programmazione in Microsoft Web Platform and Tools Team del contenuto.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) non ha la maggior parte delle operazioni di aggiornamento nell'esercitazione per Entity Framework 5 e MVC 4 ed è coautore l'aggiornamento di Entity Framework 6. Rick è una scrittrice senior di programmazione per Microsoft che si occupa in Azure e MVC.
- [Rowan Miller](http://www.romiller.com) e altri membri del team di Entity Framework assistito con revisioni del codice e il debug dei molti problemi con le migrazioni che si è verificato mentre si stava aggiornando l'esercitazione per Entity Framework 5 e 6 di Entity Framework.

<a id="vb"></a>
## <a name="vb"></a>VB

Quando l'esercitazione è stato originariamente creata per Entity Framework 4.1, abbiamo fornito le versioni di c# e VB del progetto di download completato. A causa di limitazioni di tempo e altre priorità abbiamo non abbiamo fatto che per questa versione. Se si compila un progetto di Visual Basic usando queste esercitazioni e sono disposti a condividere con altri utenti, è possibile comunicarlo.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Gli errori comuni e soluzioni o soluzioni alternative per tali

### <a name="cannot-createshadow-copy"></a>Non è possibile creare/shadow copia

Messaggio di errore:

> Non è possibile creare/shadow copia '&lt;filename&gt;' quando il file esiste già.

Soluzione

Attendere alcuni secondi e aggiornare la pagina.

### <a name="update-database-not-recognized"></a>Update-Database non riconosciuto

Messaggio di errore (dal `Update-Database` comando nella console di gestione pacchetti):

> Il termine "Update-Database" non è riconosciuto come il nome di un cmdlet, funzione, file di script o programma eseguibile. Controllare l'ortografia del nome, o se è stato incluso un percorso, verificare che il percorso sia corretto e riprovare.

Soluzione

Uscire da Visual Studio. Riaprire progetto e riprovare.

### <a name="validation-failed"></a>Convalida non riuscita

Messaggio di errore (dal `Update-Database` comando nella console di gestione pacchetti):

> Convalida non riuscita per uno o più entità. Vedere la proprietà 'EntityValidationErrors' per altri dettagli.

Soluzione

Una delle cause di questo problema è errori di convalida quando il `Seed` esecuzione del metodo. Visualizzare [Seeding e al debug di Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) per suggerimenti sul debug di `Seed` (metodo).

### <a name="http-50019-error"></a>HTTP 500.19 errore

Messaggio di errore:

> Errore HTTP 500.19 - errore interno del Server alla pagina richiesta non è accessibile perché i dati di configurazione per la pagina non sono validi.

Soluzione

È possibile ottenere questo errore è da più copie della soluzione, ognuna di esse usando lo stesso numero di porta. È in genere possibile risolvere questo problema, uscire da tutte le istanze di Visual Studio, quindi riavviare il progetto che si sta lavorando. Se il problema persiste, provare a modificare il numero di porta. Fare clic con il pulsante destro sul file di progetto e quindi fare clic su proprietà. Selezionare il **Web** scheda e quindi cambiare il numero di porta la **Url progetto** casella di testo.

### <a name="error-locating-sql-server-instance"></a>Errore di individuazione dell'istanza di SQL Server

Messaggio di errore:

> Si è verificato un errore di rete o specifico dell'istanza mentre si cercava di stabilire una connessione con SQL Server. Il server non è stato trovato o non è accessibile. Verificare che il nome dell'istanza sia corretto e che SQL Server sia configurato in modo da consentire connessioni remote. (provider: interfacce di rete SQL, errore: 26 - Errore nell'individuazione del server/dell'istanza specificata)

Soluzione

Controllare la stringa di connessione. Se è stato eliminato manualmente il database, modificare il nome del database nella stringa di costruzione.

> [!div class="step-by-step"]
> [Precedente](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
