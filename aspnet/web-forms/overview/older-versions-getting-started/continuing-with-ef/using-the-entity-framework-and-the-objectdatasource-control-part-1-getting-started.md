---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Utilizzo di Entity Framework 4.0 e il controllo ObjectDataSource, parte 1: introduzione | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni compila nell'applicazione web di Contoso University creato da introduttiva la serie di esercitazioni di Entity Framework. Se,...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 83fe815af9030aee10a5204718b00c79925e9126
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Utilizzo di Entity Framework 4.0 e il controllo ObjectDataSource, parte 1: introduzione
====================
Da [Tom Dykstra](https://github.com/tdykstra)

> Questa serie di esercitazioni si basa sull'applicazione web di Contoso University creando il [introduzione di Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie di esercitazioni. Se non ha completato le esercitazioni precedenti, come punto di partenza per questa esercitazione è possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) che consente di creare. È anche possibile [scaricare l'applicazione](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creati tramite la serie di esercitazioni completo.
> 
> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET utilizzando il Entity Framework 4.0 e Visual Studio 2010. L'applicazione di esempio è un sito Web per un'università Contoso fittizio. Include funzionalità, ad esempio l'ammissione di studenti, la creazione di corsi e assegnazioni istruttore.
> 
> L'esercitazione è riportati esempi in c#. Il [esempio scaricabile](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) contiene il codice in c# e Visual Basic.
> 
> ## <a name="database-first"></a>Prima di database
> 
> È possibile utilizzare i dati in Entity Framework in tre modi: *Database First*, *Model First*, e *Code First*. In questa esercitazione è per il primo Database. Per informazioni sulle differenze tra questi flussi di lavoro e indicazioni su come scegliere quello più adatto per lo scenario, vedere [flussi di lavoro di Entity Framework sviluppo](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Form
> 
> Come serie di attività iniziali, questa serie di esercitazioni viene utilizzato il modello Web Form ASP.NET e presuppone che si conosca come utilizzare i Web Form ASP.NET in Visual Studio. Se non, viene visualizzato [Introduzione a Web Form ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Se si preferisce utilizzare con il framework ASP.NET MVC, vedere [Introduzione a Entity Framework usando ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> | **Nell'esercitazione** | **Funziona anche con** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express per Web. L'esercitazione non è stata testata con le versioni successive di Visual Studio. Esistono numerose differenze in selezioni di menu, finestre di dialogo e i modelli. |
> | .NET 4 | .NET 4.5 è compatibile con .NET 4, ma l'esercitazione non è stata testata con .NET 4.5. |
> | Entity Framework 4 | L'esercitazione non è stata testata con le versioni successive di Entity Framework. A partire da Entity Framework 5, Entity Framework Usa per impostazione predefinita il `DbContext API` che è stata introdotta con Entity Framework 4.1. Il controllo EntityDataSource è stato progettato per utilizzare il `ObjectContext` API. Per informazioni su come usare EntityDataSource controllare con il `DbContext` API, vedere [questo post di blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Domande
> 
> Nel caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework e LINQ to forum entità](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).


Il `EntityDataSource` controllo consente di creare un'applicazione molto rapidamente, ma di mantenere una quantità significativa di logica di business e logica di accesso ai dati in richiede in genere il *aspx* pagine. Se si prevede che l'applicazione per l'aumento della complessità e richiedono una manutenzione costante, di investire anticipo più tempo di sviluppo per creare un *livelli* o *a più livelli* la struttura dell'applicazione è quindi più gestibile. Per implementare questa architettura, si separa il livello di presentazione dal livello di logica di business (BLL) e il livello di accesso ai dati (DAL). Per implementare questa struttura è possibile utilizzare il `ObjectDataSource` controllo anziché il `EntityDataSource` controllo. Quando si utilizza il `ObjectDataSource` controllo, si implementa il proprio codice di accesso ai dati e quindi richiamarlo in *aspx* pagine utilizzando un controllo che ha molte delle stesse funzionalità degli altri controlli origine dati. Ciò consente di combinare i vantaggi di un approccio a più livelli con i vantaggi dell'utilizzo di un controllo Web Form per l'accesso ai dati.

Il `ObjectDataSource` controllo offre una maggiore flessibilità in anche altri modi. Perché si scrive codice di accesso ai dati, è facile oltre la semplice leggere, inserire, aggiornare o eliminare un tipo di entità specifico, quali sono le attività che la `EntityDataSource` controllo progettato per l'esecuzione. Ad esempio, è possibile eseguire la registrazione ogni volta che viene aggiornata un'entità, archiviare i dati ogni volta che viene eliminata un'entità, o automaticamente controllo e aggiornamento dati correlati in base alle necessità durante l'inserimento di una riga con un valore di chiave esterna.

## <a name="business-logic-and-repository-classes"></a>Logica di business e classi del Repository

Un `ObjectDataSource` controllo tramite la chiamata di una classe che viene creata. La classe include metodi che recuperano e aggiornano i dati e fornire i nomi di tali metodi per il `ObjectDataSource` controllo nel markup. Durante il rendering o elaborazione postback, il `ObjectDataSource` chiama i metodi che è stato specificato.

Oltre alle operazioni CRUD di base, la classe creata per l'utilizzo con il `ObjectDataSource` controllo potrebbe essere necessario eseguire la logica di business quando il `ObjectDataSource` legge o aggiorna i dati. Quando si aggiorna un reparto, ad esempio, potrebbe essere necessario convalidare che altri reparti non hanno lo stesso di amministratore poiché una persona non può essere l'amministratore di più di un reparto.

In alcuni `ObjectDataSource` documentazione, ad esempio il [Cenni preliminari sulla classe ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), il controllo chiama una classe definita come un *oggetto business* che include sia la logica di business e logica di accesso ai dati . In questa esercitazione si creerà classi separate per la logica di business e per la logica di accesso ai dati. La classe che incapsula la logica di accesso ai dati viene chiamata un *repository*. La classe di logica di business include i metodi di logica di business e i metodi di accesso ai dati, ma il repository per eseguire attività di accesso ai dati di chiamare i metodi di accesso ai dati.

Si creerà anche un livello di astrazione tra BLL DAL che facilita la unit test automatizzati e test di BLL. Questo livello di astrazione è implementato mediante la creazione di un'interfaccia e tramite l'interfaccia quando si crea un'istanza del repository nella classe della logica di business. In questo modo è possibile fornire la logica di business di classe con un riferimento a qualsiasi oggetto che implementa l'interfaccia del repository. Per il normale funzionamento, è fornire un oggetto del repository che funziona con Entity Framework. Per i test, è fornire un oggetto del repository che funziona con i dati archiviati in modo che è possibile modificare facilmente, ad esempio le variabili di classe definite come raccolte.

Nella figura seguente mostra la differenza tra una classe di logica di business che include la logica di accesso ai dati senza un repository e che usa un repository.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Si inizierà creando pagine web in cui il `ObjectDataSource` è associato direttamente a un repository perché consente di eseguire solo operazioni di base di accesso ai dati. Nella prossima esercitazione verrà creato una classe della logica di business con la logica di convalida e associare il `ObjectDataSource` controllo a tale classe anziché alla classe del repository. Si creerà anche unit test per la logica di convalida. Nella terza esercitazione di questa serie verranno aggiunti di ordinamento e filtro funzionalità all'applicazione.

Utilizzano le pagine create in questa esercitazione il `Departments` set di entità del modello di dati creati nel [Getting Started tutorial serie](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>L'aggiornamento del Database e il modello di dati

Iniziare l'esercitazione da apportare due modifiche al database, che richiedono le modifiche corrispondenti al modello di dati creati nel [Introduzione a Entity Framework e Web Form](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) esercitazioni. In uno di tali esercitazioni, sono apportate modifiche nella finestra di progettazione manualmente per sincronizzare il modello di dati con il database dopo una modifica al database. In questa esercitazione si utilizzerà la finestra di progettazione **Aggiorna modello da Database** strumento per aggiornare automaticamente il modello di dati.

### <a name="adding-a-relationship-to-the-database"></a>Aggiunta di una relazione per il Database

In Visual Studio, aprire l'applicazione web di Contoso University creato nel [Introduzione a Entity Framework e Web Form](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie di esercitazioni e quindi aprire il `SchoolDiagram` diagramma di database.

Se si esamina il `Department` tabella nel diagramma di database, si noterà che contiene un `Administrator` colonna. Questa colonna è una chiave esterna per il `Person` tabella, ma nessuna relazione di chiave esterna è definito nel database. È necessario creare la relazione e aggiornare il modello di dati in modo da Entity Framework può gestire automaticamente questa relazione.

Nel diagramma di database, fare doppio clic su di `Department` tabella e selezionare **relazioni**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

Nel **relazioni di chiave esterna** fare clic su **Aggiungi**, quindi fare clic sui puntini di sospensione per **specifica tabelle e colonne**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

Nel **tabelle e colonne** finestra di dialogo Imposta la tabella di chiave primaria e campo `Person` e `PersonID`e impostare la tabella chiave esterna e campo `Department` e `Administrator`. (In questo caso, il nome della relazione passerà da ' `FK_Department_Department` a `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Fare clic su **OK** nel **tabelle e colonne** fare clic su **Chiudi** nel **relazioni di chiave esterna** e salvare le modifiche. Se viene chiesto se si desidera salvare il `Person` e `Department` tabelle, fare clic su **Sì**.

> [!NOTE]
> Se è stato eliminato `Person` le righe che corrispondono ai dati che sono già presente nel `Administrator` colonna, non sarà in grado di salvare questa modifica. In tal caso, utilizzare l'editor di tabella in **Esplora Server** per assicurarsi che il `Administrator` valore in ogni `Department` riga contiene l'ID di un record che sia effettivamente presente nel `Person` tabella.
> 
> Dopo aver salvato la modifica, non sarà in grado di eliminare una riga di `Person` tabella se tale utente è un amministratore del reparto. In un'applicazione di produzione, è necessario fornire un messaggio di errore specifico quando un vincolo database impedisce l'eliminazione oppure è possibile specificare un'operazione di eliminazione a catena. Per un esempio di come specificare un'operazione di eliminazione a catena, vedere [Entity Framework e ASP.NET: Guida introduttiva parte 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Aggiunta di una vista nel database

Nel nuovo *Departments.aspx* pagina che si creano, si desidera fornire un elenco di riepilogo a discesa di istruttori, con i nomi nel formato "cognome, nome" in modo che gli utenti possono selezionare gli amministratori di reparto. Per rendere più semplice a tale scopo, si creerà una vista nel database. La vista includerà solo i dati necessari per l'elenco a discesa: il nome completo (corretto) e la chiave del record.

In **Esplora Server**, espandere *School. mdf*, fare doppio clic su di **viste** cartella e selezionare **Aggiungi nuova vista**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Fare clic su **Chiudi** quando il **Aggiungi tabella** viene visualizzata la finestra di dialogo e incollare l'istruzione SQL seguente nel riquadro SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Salvare la vista come `vInstructorName`.

### <a name="updating-the-data-model"></a>L'aggiornamento del modello di dati

Nel *DAL* cartella, aprire il *SchoolModel* file, fare doppio clic nell'area di progettazione e selezionare **il modello di aggiornamento dal Database**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

Nel **Seleziona oggetti di Database** la finestra di dialogo, seleziona il **Aggiungi** e selezionare la vista appena creata.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Scegliere **Fine**.

Nella finestra di progettazione, vedrai che viene creato un `vInstructorName` entità e una nuova associazione tra il `Department` e `Person` entità.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> Nel **Output** e **elenco errori** di tasti di windows è possibile vedere un messaggio di avviso che informa che lo strumento viene creato automaticamente un database primario per il nuovo `vInstructorName` visualizzazione. Si tratta di un comportamento previsto.


Quando si fa riferimento al nuovo `vInstructorName` entità nel codice, non si desidera utilizzare la convenzione di database del prefisso "v" minuscola a esso. Pertanto, si rinominerà l'entità e set nel modello di entità.

Aprire il **Browser modello**. Vedrai `vInstructorName` elencato come un tipo di entità e una visualizzazione.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

In **SchoolModel** (non **SchoolModel.Store**), fare doppio clic su **vInstructorName** e selezionare **proprietà**. Nel **proprietà** finestra, modifica il **nome** proprietà su "InstructorName" e modificare il **nome del Set di entità** proprietà su "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Salvare e chiudere il modello di dati e quindi ricompilare il progetto.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Utilizzo di una classe di Repository e un controllo ObjectDataSource

Creare un nuovo file di classe nel *DAL* cartella, il nome *SchoolRepository.cs*e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Questo codice fornisce un singolo `GetDepartments` metodo che restituisce tutte le entità nel `Departments` set di entità. Poiché è noto che si accede di `Person` proprietà di navigazione per ogni riga restituita, si specifica eager caricamento per tale proprietà utilizzando il `Include` metodo. La classe implementa anche il `IDisposable` interfaccia per essere certi che la connessione al database viene rilasciata quando l'oggetto viene eliminato.

> [!NOTE]
> Una pratica comune consiste nel creare una classe di repository per ogni tipo di entità. In questa esercitazione viene utilizzata una classe di repository per più tipi di entità. Per ulteriori informazioni sul modello di repository, vedere i post in [blog del team di Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) e [blog di Julie Lerman](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


Il `GetDepartments` metodo restituisce un `IEnumerable` oggetto anziché un oggetto `IQueryable` oggetto per garantire che la raccolta restituita è utilizzabile anche dopo l'eliminazione dell'oggetto del repository stesso. Un `IQueryable` oggetto può provocare l'accesso al database ogni volta che è possibile accedervi, ma l'oggetto repository può essere eliminato nel momento in cui un controllo con associazione a dati tenta di eseguire il rendering dei dati. È possibile restituire un altro tipo di raccolta, ad esempio un `IList` oggetto anziché un `IEnumerable` oggetto. Tuttavia, restituendo un `IEnumerable` oggetto assicura che è possibile eseguire le operazioni di elaborazione tipico elenco di sola lettura, ad esempio `foreach` cicli e le query LINQ, ma è Impossibile aggiungere o rimuovere elementi nella raccolta, che potrebbe implicare che tali modifiche sarebbero resi persistenti nel database.

Creare un *Departments.aspx* pagina che utilizza il *Site. master* pagina master e aggiungere il markup seguente nel `Content` controllo denominato `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Questo codice crea un `ObjectDataSource` controllo che utilizza la classe di repository appena creato, e un `GridView` controllo per visualizzare i dati. Il `GridView` controllo specifica **modifica** e **eliminare** comandi, ma è ancora stato aggiunto codice a ancora supportati.

Utilizzano colonne diverse `DynamicField` controlli in modo che è possibile sfruttare la funzionalità di formattazione e la convalida automatica dei dati. Per questi elementi di lavoro, è necessario chiamare il `EnableDynamicData` metodo il `Page_Init` gestore dell'evento. (`DynamicControl` controlli non vengono utilizzati nel `Administrator` campo perché non funzionano con le proprietà di navigazione.)

Il `Vertical-Align="Top"` attributi diverranno più importanti in un secondo momento quando si aggiunge una colonna che nidificato `GridView` controllo alla griglia.

Aprire il *Departments.aspx.cs* file e aggiungere le seguenti `using` istruzione:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Quindi aggiungere il seguente gestore per la pagina `Init` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

Nel *DAL* cartella, creare un nuovo file di classe denominato *Department.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Questo codice aggiunge i metadati per il modello di dati. Specifica che il `Budget` proprietà del `Department` entità rappresenta effettivamente valuta anche se il tipo di dati è `Decimal`, e specifica che il valore deve essere compreso tra 0 e $1,000,000.00. Specifica inoltre che il `StartDate` proprietà deve essere formattata come una data nel formato mm/gg/aaaa.

Eseguire il *Departments.aspx* pagina.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Si noti che anche se non è stato specificato in una stringa di formato di *Departments.aspx* markup della pagina per il **Budget** o **data di inizio** predefinito di colonne, valuta e date è stata applicata la formattazione a essi di `DynamicField` controlli, utilizzando i metadati che è fornito nel *Department.cs* file.

## <a name="adding-insert-and-delete-functionality"></a>Inserimento di aggiunta e la funzionalità di eliminazione

Aprire *SchoolRepository.cs*, aggiungere il codice seguente per creare un `Insert` (metodo) e un `Delete` metodo. Il codice include anche un metodo denominato `GenerateDepartmentID` che calcola il valore chiave successivo di record disponibili per l'utilizzo dal `Insert` metodo. Questa operazione è necessaria perché il database non è configurato per questa operazione automaticamente per calcolare il `Department` tabella.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Il metodo di collegamento

Il `DeleteDepartment` chiamate al metodo di `Attach` metodo per ristabilire il collegamento che viene mantenuto in Gestione stato oggetto di contesto dell'oggetto tra l'entità in memoria e il database di riga rappresenta. Questo deve verificarsi prima il metodo chiama il `SaveChanges` metodo.

Il termine *contesto dell'oggetto* fa riferimento alla classe che deriva da Entity Framework di `ObjectContext` classe che consente di accedere ai set di entità e le entità. Nel codice per questo progetto, la classe è denominata `SchoolEntities`, e un'istanza è sempre denominata `context`. Contesto dell'oggetto *gestione dello stato degli oggetti* è una classe che deriva dalla `ObjectStateManager` classe. Contattare l'oggetto utilizza il gestore degli stati oggetto per archiviare gli oggetti entità e di tenere traccia se ciascuna di esse è sincronizzata con la riga corrispondente della tabella o le righe nel database.

Durante la lettura di un'entità, il contesto dell'oggetto viene memorizzato nella gestione dello stato di oggetto e consente di determinare se tale rappresentazione dell'oggetto è sincronizzato con il database. Ad esempio, se si modifica un valore della proprietà, viene impostato un flag per indicare che la proprietà che è stata modificata non è più sincronizzata con il database. Quando si chiama il `SaveChanges` (metodo), il contesto dell'oggetto sa operazioni da eseguire nel database perché l'oggetto stato in grado di indicare esattamente ciò che è diverso tra lo stato corrente dell'entità e lo stato del database.

Tuttavia, questo processo in genere non funziona in un'applicazione web, perché l'istanza contesto dell'oggetto che legge un'entità, insieme a tutti gli elementi di gestione dello stato relativo oggetto viene eliminato dopo il rendering di una pagina. Istanza dell'oggetto di contesto che è necessario applicare le modifiche è una nuova istanza che viene creata un'istanza per l'elaborazione di postback. In caso del `DeleteDepartment` (metodo), il `ObjectDataSource` controllo crea nuovamente la versione originale dell'entità di dai valori nello stato di visualizzazione, ma questo ricreato `Department` entità non esiste nella gestione stato oggetto. Se è stato chiamato il `DeleteObject` metodo su questa entità creata di nuovo, la chiamata avrà esito negativo perché il contesto dell'oggetto non sa se l'entità è sincronizzata con il database. Tuttavia, la chiamata di `Attach` metodo ristabilisce la registrazione tra l'entità creata di nuovo e i valori nel database che è stata originariamente eseguito automaticamente quando l'entità è stato letto in un'istanza precedente del contesto dell'oggetto.

Vi sono casi quando non si desidera il contesto dell'oggetto per rilevare le entità nella gestione dello stato di oggetto, ed è possibile impostare flag per impedirne l'esecuzione di operazioni con. Esempi di questo tipo vengono visualizzati nelle esercitazioni successive di questa serie.

### <a name="the-savechanges-method"></a>Il metodo SaveChanges

Questa classe repository semplice illustra i principi di base su come eseguire operazioni CRUD. In questo esempio, il `SaveChanges` metodo viene chiamato immediatamente dopo ogni aggiornamento. In un'applicazione di produzione è possibile chiamare il `SaveChanges` metodo da un metodo separato per fornire maggiore controllo sulla quando il database viene aggiornato. (Al termine dell'esercitazione successiva si troverà un collegamento a un documento che descrive l'unità di lavoro modello che è uno degli approcci disponibili per il coordinamento degli aggiornamenti correlati.) Si noti inoltre che nell'esempio di `DeleteDepartment` metodo non sia incluso codice per gestire i conflitti di concorrenza; verrà aggiunto codice a tale scopo in un'esercitazione successiva di questa serie.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Il recupero dei nomi istruttore selezionare durante l'inserimento

Gli utenti devono essere in grado di selezionare un amministratore da un elenco di istruttori in un elenco a discesa durante la creazione di nuovi reparti. Pertanto, aggiungere il seguente codice al *SchoolRepository.cs* per creare un metodo per recuperare l'elenco di istruttori utilizzando la vista creata in precedenza:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Creazione di una pagina per l'inserimento dei reparti

Creare un *DepartmentsAdd.aspx* pagina che utilizza il *Site. master* pagina e aggiungere il markup seguente nel `Content` controllo denominato `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Questo codice crea due `ObjectDataSource` controlli, uno per l'inserimento di nuovi `Department` entità e una per il recupero dei nomi istruttore per il `DropDownList` controllo che viene utilizzata per selezionare gli amministratori di reparto. Il codice crea un `DetailsView` di controllo per l'immissione di nuovi reparti e specifica un gestore per il controllo `ItemInserting` evento in modo che sia possibile impostare il `Administrator` valore di chiave esterna. Alla fine è un `ValidationSummary` controllo per visualizzare i messaggi di errore.

Aprire *DepartmentsAdd.aspx.cs* e aggiungere le seguenti `using` istruzione:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Aggiungere i metodi e una variabile di classe seguente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

Il `Page_Init` metodo abilita la funzionalità di Dynamic Data. Il gestore per il `DropDownList` del controllo `Init` evento Salva un riferimento al controllo e il gestore per il `DetailsView` del controllo `Inserting` eventi utilizza tale riferimento per ottenere il `PersonID` valore istruttore selezionato ed eseguirne l'aggiornamento il `Administrator` di proprietà di chiave esterna di `Department` entità.

Eseguire la pagina, aggiungere le informazioni per una nuova categoria e quindi fare clic su di **inserire** collegamento.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Immettere i valori per un altro reparto di nuovo. Immettere un numero maggiore di 1,000,000.00 nel **Budget** campo e tab per il campo successivo. Viene visualizzato un asterisco nel campo e se si posiziona il puntatore del mouse su di esso, è possibile visualizzare il messaggio di errore immessi nei metadati per il campo.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Fare clic su **inserire**, e viene visualizzato il messaggio di errore visualizzato dal `ValidationSummary` controllo nella parte inferiore della pagina.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Quindi, chiudere il browser e aprire il *Departments.aspx* pagina. Aggiungere funzionalità di eliminazione per il *Departments.aspx* pagina aggiungendo un `DeleteMethod` attributo la `ObjectDataSource` (controllo) e un `DataKeyNames` attributo la `GridView` controllo. I tag di apertura per questi controlli saranno ora simile all'esempio seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Eseguire la pagina.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Eliminare il reparto in cui è stato aggiunto al momento dell'esecuzione di *DepartmentsAdd.aspx* pagina.

## <a name="adding-update-functionality"></a>Aggiunta di funzionalità di aggiornamento

Aprire *SchoolRepository.cs* e aggiungere le seguenti `Update` metodo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Quando fa clic su **aggiornamento** nel *Departments.aspx* pagina il `ObjectDataSource` controllo crea due `Department` entità da passare al `UpdateDepartment` metodo. Uno contiene i valori originali sono stati archiviati in stato di visualizzazione e l'altra contiene i nuovi valori che sono sono immessi i `GridView` controllo. Il codice di `UpdateDepartment` metodo passa il `Department` entità contenente i valori originali per il `Attach` metodo per stabilire la spaziatura tra l'entità e il database. Quindi il codice passa il `Department` entità contenente i nuovi valori di `ApplyCurrentValues` metodo. Contesto dell'oggetto confronta i valori vecchi e nuovi. Se un nuovo valore è diverso dal valore precedente, il valore della proprietà di contesto dell'oggetto. Il `SaveChanges` metodo quindi aggiorna solo le colonne modificate nel database. (Tuttavia, se la funzione di aggiornamento per questa entità sono stata associata a una stored procedure, l'intera riga sarebbe aggiornata indipendentemente da quali colonne sono state modificate.)

Aprire il *Departments.aspx* file e aggiungere i seguenti attributi per il `DepartmentsObjectDataSource` controllo:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Questo causa precedente per i valori archiviati in stato di visualizzazione in modo da poter essere confrontate con i nuovi valori nel `Update` metodo.
- `OldValuesParameterFormatString="orig{0}"`   
 Ciò indica al controllo che è il nome del parametro dei valori originali `origDepartment` .

Il markup per il tag di apertura del `ObjectDataSource` controllo ora simile all'esempio seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Aggiungere un `OnRowUpdating="DepartmentsGridView_RowUpdating"` attributo la `GridView` controllo. Usato per impostare il `Administrator` valore della proprietà in base alla riga in cui l'utente seleziona un elenco a discesa. Il `GridView` tag di apertura ora simile all'esempio seguente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Aggiungere un `EditItemTemplate` controllare per il `Administrator` colonna per il `GridView` controllare, subito dopo il `ItemTemplate` controllo per la colonna:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Questo `EditItemTemplate` è simile al controllo il `InsertItemTemplate` controllo il *DepartmentsAdd.aspx* pagina. La differenza è che il valore iniziale del controllo è impostato tramite il `SelectedValue` attributo.

Prima del `GridView` di controllo, aggiungere un `ValidationSummary` controllare seguendo il *DepartmentsAdd.aspx* pagina.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Aprire *Departments.aspx.cs* e immediatamente dopo la dichiarazione di classe parziale, aggiungere il codice seguente per creare un campo privato per fare riferimento il `DropDownList` controllo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Quindi aggiungere i gestori di `DropDownList` del controllo `Init` evento e `GridView` del controllo `RowUpdating` evento:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Il gestore per il `Init` evento Salva un riferimento al `DropDownList` controllo nel campo della classe. Il gestore per il `RowUpdating` evento utilizza il riferimento per ottenere il valore specificato dall'utente e applicarlo al `Administrator` proprietà del `Department` entità.

Utilizzare il *DepartmentsAdd.aspx* pagina per aggiungere una nuova categoria, quindi eseguire il *Departments.aspx* pagina e fare clic su **modifica** sulla riga che è stato aggiunto.

> [!NOTE]
> Non sarà in grado di modificare le righe che non è stata aggiunta (ovvero, che sono stati già nel database), a causa di dati non validi nel database. gli amministratori per le righe che sono stati creati con il database siano studenti. Se si tenta di modificare uno di essi, si otterrà una pagina di errore che segnala un errore di`'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Se si immette un valido **Budget** importo e quindi fare clic su **aggiornamento**, visualizzare l'asterisco stesso e un messaggio di errore che si è visto nella *Departments.aspx* pagina.

Modificare un valore del campo o selezionare un diverso per l'amministratore e fare clic su **aggiornamento**. Viene visualizzata la modifica.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Introduzione all'uso è stata completata la `ObjectDataSource` controllo per CRUD di base (creare, leggere, aggiornare ed eliminare) operazioni con Entity Framework. Una volta compilato una semplice applicazione a più livelli, ma il livello di logica di business è ancora strettamente a livello di accesso ai dati, che rendono più complessa di unit test automatici. Nell'esercitazione seguente si noterà come implementare il modello di repository per facilitare gli unit test.

>[!div class="step-by-step"]
[avanti](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
